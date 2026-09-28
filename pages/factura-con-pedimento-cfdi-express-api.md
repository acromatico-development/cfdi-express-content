---
title: "Factura con pedimento en CFDI 4.0: cuándo va y cómo emitirla por API"
description: "Número de pedimento en CFDI 4.0: cuándo va la información aduanera en una venta de primera mano y cómo mandarla por API o por MCP."
image: "https://videos.acromatico.dev/api/images/assets/9b42be87-d026-4d28-a6fe-6f0f2c878dec.png"
author: "Rafael González"
date: "2026-09-27"
keywords: "factura con pedimento, cómo facturar mercancía de importación, número de pedimento en CFDI 4.0, información aduanera CFDI, InformacionAduanera, NumeroPedimento, pedimento SAT, venta de primera mano, CFDI Express API, MCP pedimento, agente IA CFDI"
---
# Factura con pedimento en CFDI 4.0: cuándo va y cómo emitirla por API

![Factura con pedimento](https://videos.acromatico.dev/api/images/assets/9b42be87-d026-4d28-a6fe-6f0f2c878dec.png?w=1600)

Si vendes mercancía importada y el cliente te pide **factura con pedimento**, el número no va en la cabecera del CFDI. Va en el concepto, nodo `InformacionAduanera`, atributo `NumeroPedimento`.

Este post es para quien timbra con la [API de CFDI Express](https://cfdi.express/api): cuándo aplica, cómo se escriben los 21 caracteres que pide el SAT y el campo `items[].pedimentos` de `POST /v1/invoices` y `POST /v1/credit_notes`. El mismo campo está en el [servidor MCP](https://api.cfdi.express/mcp), en `create_invoice` y `create_credit_note`. Las reglas del comprobante salen del [Anexo 20 de la RMF 2022](http://omawww.sat.gob.mx/tramitesyservicios/Paginas/documentos/Anexo20_2022.pdf) (estándar CFDI 4.0). El contrato del campo sale del OpenAPI en vivo, `https://api.cfdi.express/openapi.json`, y de las herramientas del MCP.

Pide **datos fiscales** del receptor (RFC, nombre o razón social, régimen fiscal y código postal). Qué cruza el SAT contra el padrón está en [CFDI facturas 4.0](/blog/cfdi-facturas-4-0).

## ¿Qué es el número de pedimento y cuándo va en la factura?

**Respuesta corta:** es el pedimento que amparó la importación del bien. En el CFDI 4.0 se captura por concepto, cuando vendes esa mercancía de primera mano.

En el estándar del Anexo 20, `InformacionAduanera` es un nodo **opcional** del `Concepto` (de cero a ilimitado). La descripción del nodo dice que aplica «cuando se trate de ventas de primera mano de mercancías importadas o se trate de operaciones de comercio exterior con bienes o servicios». Si el nodo existe, `NumeroPedimento` es **requerido**: es el número del pedimento que ampara la importación de ese bien.

Las validaciones adicionales del mismo anexo, para el `NumeroPedimento` del concepto, precisan el caso de la factura nacional:

- Se registra cuando el CFDI **no** lleva el complemento de comercio exterior. El anexo lo describe como venta de primera mano nacional.
- No se registra cuando el CFDI **sí** lleva el complemento de comercio exterior.

En la práctica, para una factura de ingreso sin ese complemento: si tú importaste el bien y esta es la primera venta en México, el pedimento va en ese concepto. En una reventa —el producto ya se vendió antes en el país— el nodo no corresponde. El mismo anexo también permite `InformacionAduanera` dentro de `Parte`. El campo de esta API es el del concepto, no el de cada parte.

El OpenAPI de CFDI Express no publica el complemento de comercio exterior. Lo que sí publica, para la factura de ingreso y la nota de crédito, es `items[].pedimentos`.

## Los 15 dígitos, y los 21 caracteres que timbra el SAT

**Respuesta corta:** el pedimento son 15 dígitos. En el XML van en una cadena de longitud 21, con dos espacios entre cada grupo.

El Anexo 20 fija `NumeroPedimento` así: tipo string, **longitud 21**, patrón de cuatro grupos separados por dos espacios. El formato que describe el anexo:

1. **Año de validación (2).** Los últimos dos dígitos del año de validación.
2. **Aduana de despacho (2).** Dos espacios, luego la clave de la aduana.
3. **Patente (4).** Dos espacios, luego el número de patente.
4. **Consecutivo (7).** Dos espacios, luego siete dígitos. El primero es el último dígito del año en curso, **salvo** que sea un pedimento consolidado iniciado en el año inmediato anterior o el pedimento original de una rectificación. Los otros seis son la numeración progresiva por aduana.

Quince dígitos, seis espacios, veintiún caracteres:

```text
[0-9]{2}  [0-9]{2}  [0-9]{4}  [0-9]{7}
```

El ejemplo que publica el OpenAPI, ya en esa forma de 21 caracteres, es:

```text
25  47  3807  5001234
```

Léelo por grupos: año `25`, aduana `47`, patente `3807`, consecutivo `5001234` (el `5` y, después, `001234`). Es el ejemplo de la spec, para ver la forma. En un timbre real la aduana y la patente tienen que existir en los catálogos del SAT (más abajo).

Esas validaciones adicionales hablan de **posiciones del string de 21 caracteres**, contando los espacios:

| Posiciones | Qué revisa el SAT, según el Anexo 20 |
| --- | --- |
| 1 y 2 | Menor o igual que los últimos dos dígitos del año de la fecha actual |
| 5 y 6 | Clave vigente de `catCFDI:c_Aduana` |
| 9 a 12 | Número de patente de `catCFDI:c_PatenteAduanal` |
| Últimos 6 dígitos | Entre 1 y el máximo de `catCFDI:c_NumPedimentoAduana` para esa aduana en ese año |

Si partes el número ya sin espacios, la aduana son los dígitos 3 y 4, no las “posiciones 5 y 6”. Esas posiciones solo cuadran cuando los dos espacios ya están puestos.

## Errores comunes al capturar el pedimento

**Respuesta corta:** un solo espacio, un guion o un dígito de menos cambian la longitud. Y el pedimento vive en el concepto, no en la factura.

| Lo que suele llegar | Qué pasa |
| --- | --- |
| Un solo espacio: `25 47 3807 5001234` (18 caracteres) | En el XML del SAT la longitud es 21. La API **sí acepta** un espacio, varios o ninguno, y emite los dos espacios. |
| Guiones o barras: `25-47-3807-5001234` | No cumple el patrón. `POST` responde **400** (validation error). |
| Le faltan dígitos, o le sobran | El patrón pide 2 + 2 + 4 + 7. Si no cierra, **400**. |
| El pedimento en la raíz del JSON | No hay un `pedimentos` de factura. Va en cada elemento de `items`. |
| Pedimento en una reventa | El anexo lo ata a la venta de primera mano. La API no infiere si tu venta lo es: si el string cumple el patrón, lo emite. |
| El mismo número repetido en el arreglo | El OpenAPI dice que los duplicados se descartan: un nodo por pedimento. |
| `pedimentos: []` | Si mandas el arreglo, `minItems` es 1. Para omitirlo, no envíes la propiedad. |

El arreglo, cuando va, admite de **1 a 100** pedimentos en ese concepto.

## Cómo emitirlo con la API

**Respuesta corta:** en cada concepto, `pedimentos` es un arreglo de strings. Puedes mandar los 15 dígitos juntos o con espacios. El XML sale con la forma de 21 caracteres.

El campo está en el `items[]` de `POST /v1/invoices` y de `POST /v1/credit_notes`. Es opcional. Cada string tiene que cumplir:

```text
^\s*(\d{2})\s*(\d{2})\s*(\d{4})\s*(\d{7})\s*$
```

Eso es lo que describe la spec: 15 dígitos —año (2), aduana (2), patente (4), consecutivo (7)— con o sin espacios. La API los emite en la forma de 21 caracteres del SAT, con dos espacios entre grupos, un nodo `InformacionAduanera` por pedimento.

`POST /v1/invoices` espera el header `Idempotency-Key` (1 a 255 caracteres). Igual que el resto de los timbres: espera hasta **20 s** el UUID (**201**) y, si la cola está saturada, responde **202** para que consultes `GET /v1/invoices/{id}`. Un replay de la misma llave trae `Idempotency-Replayed: true`. Si esa llave sigue en vuelo, **409**.

Los datos del receptor en el ejemplo son los de prueba que ya usamos en [el post de lanzamiento](/blog/lanzamiento-cfdi-express-api) (`EKU9003173C9`). `01010101` y `E48` también: sirven para ver la forma en sandbox. En producción la clave de producto y la unidad salen de `GET /v1/catalogs/products` y `GET /v1/catalogs/units`, y el uso de CFDI tiene que ser compatible con el régimen (catálogo `GET /v1/catalogs/usos-cfdi`).

```bash
curl https://api.cfdi.express/v1/invoices \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: pedimento-orden-1042" \
  -H "Content-Type: application/json" \
  -d '{
    "merchantId": "mer_...",
    "metodoPago": "PUE",
    "formaPago": "03",
    "usoCfdi": "G03",
    "receiver": {
      "rfc": "EKU9003173C9",
      "name": "ESCUELA KEMPER URGATE",
      "zip": "76000",
      "regimenFiscal": "601"
    },
    "items": [{
      "productCode": "01010101",
      "unitCode": "E48",
      "description": "Mercancía importada de primera mano",
      "quantity": 1,
      "unitPrice": 1500,
      "pedimentos": ["25  47  3807  5001234"]
    }]
  }'
```

Estas tres cadenas son el mismo ejemplo, y las tres caben en el patrón: `254738075001234`, `25 47 3807 5001234` y `25  47  3807  5001234`. Varios pedimentos distintos van en el mismo arreglo, uno por nodo:

```json
"pedimentos": [
  "254738075001234",
  "25  47  3807  5001235"
]
```

El segundo valor solo muestra otro consecutivo. No es un pedimento publicado por el SAT.

Si llega **201**, guarda el `uuid`. Si llega **202**, sigue con `GET /v1/invoices/{id}`. El recurso de factura que publica el OpenAPI trae estatus y UUID; el pedimento viaja en el XML del concepto, no como un campo suelto del JSON de respuesta.

### Notas de crédito

`POST /v1/credit_notes` repite el mismo `items[].pedimentos`. El timbrado es el de siempre (201 o 202, `Idempotency-Key` obligatoria) y la nota se consulta como factura con `kind=credit_note`.

`tipoRelacion` acepta `01` (default: nota de crédito de los documentos relacionados) o `03` (devolución de mercancía). Con `relatedInvoiceId` el receptor sale de la factura relacionada; con `relatedUuid` (un ingreso timbrado fuera de esta API) el receptor es obligatorio. `usoCfdi` default `G02`.

```bash
curl https://api.cfdi.express/v1/credit_notes \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: pedimento-nc-1042" \
  -H "Content-Type: application/json" \
  -d '{
    "merchantId": "mer_...",
    "relatedInvoiceId": "inv_...",
    "tipoRelacion": "03",
    "formaPago": "03",
    "items": [{
      "productCode": "01010101",
      "unitCode": "E48",
      "description": "Devolución de mercancía importada",
      "quantity": 1,
      "unitPrice": 1500,
      "pedimentos": ["254738075001234"]
    }]
  }'
```

La API no documenta que copie sola los pedimentos de la factura original. Si el concepto de la nota debe llevarlos, van en su `items[]`.

## También desde un agente de IA

El mismo `items[].pedimentos` va por REST y por el [servidor MCP de CFDI Express](https://api.cfdi.express/mcp). `create_invoice` (incluye facturas globales) y `create_credit_note` lo aceptan con el contrato de la API: arreglo opcional, de 1 a 100 strings, 15 dígitos con o sin espacios, emitidos en 21 caracteres, un nodo por pedimento y duplicados descartados. La herramienta lo describe para la venta de primera mano de mercancía importada.

Le hablas en español. El número de abajo es el ejemplo de la spec, y `<cliente>` es un hueco: el agente necesita los datos fiscales reales del receptor (RFC, nombre o razón social, régimen y código postal).

```text
Factura 2 laptops importadas a <cliente> con el pedimento 25 47 3807 5001234
```

- **claude.ai / ChatGPT:** OAuth en `https://api.cfdi.express/mcp`.
- **Claude Code / Cursor:** la misma URL con tu API key (`sk_test_` o `sk_live_`).
- **Sandbox:** `https://api.cfdi.express/mcp/test` timbra contra el SAT de pruebas, gratis.

La [landing de la API](https://cfdi.express/api) y [cfdi.express/agente](https://cfdi.express/agente) tienen el setup.

## Qué valida la API y qué sigue en el SAT

**Respuesta corta:** la API y el MCP normalizan la forma con el mismo contrato. Que la aduana, la patente y el consecutivo existan, y que la venta sea de primera mano, lo cruza el SAT.

| | API y MCP (`items[].pedimentos`) | SAT (Anexo 20) |
| --- | --- | --- |
| Forma | Patrón de 15 dígitos, con o sin espacios. Emite 21 caracteres y dos espacios entre grupos. | Longitud 21 y el patrón con dos espacios. |
| Cuándo | Opcional. Si lo mandas, de 1 a 100 strings por concepto. | Nodo opcional. Si existe, `NumeroPedimento` es requerido. |
| Repetidos | Los duplicados se descartan. | Un nodo de información aduanera por pedimento. |
| Catálogos | El contrato publicado no documenta consulta a `c_Aduana`, `c_PatenteAduanal` ni `c_NumPedimentoAduana`. | Posiciones 5–6, 9–12 y los últimos 6 dígitos contra esos catálogos. |
| Fondo de la operación | No decide si tu venta es de primera mano ni si el CFDI lleva complemento de comercio exterior. | El nodo va en la venta de primera mano nacional y no se registra si el CFDI trae ese complemento. |

Un string que no cumple el patrón se queda en **400**. Un número con la forma correcta y una aduana o patente que el SAT no reconoce puede terminar en **422** («SAT rejection or idempotency key reuse»). Ese 422 también cubre reusar una `Idempotency-Key` con otro body: no leas todo 422 como “el pedimento está mal”. El OpenAPI no publica un código de error del SAT específico para `NumeroPedimento`.

OpenAPI tampoco publica una tarifa distinta para este campo. El pedimento viaja en el mismo timbre de ingreso o de egreso. En la [landing de la API](https://cfdi.express/api) el precio publicado sigue siendo **$1 MXN por timbre** en producción, sandbox con `sk_test_` y el mismo saldo prepagado.

## Preguntas frecuentes

### ¿El pedimento es obligatorio en toda factura?

En el estándar el nodo es opcional (de cero a ilimitado): no va en toda factura. La validación adicional del Anexo 20 pide registrarlo en la venta de primera mano nacional, el CFDI sin complemento de comercio exterior. Si omites `pedimentos`, la API acepta el request, porque en el OpenAPI el campo es opcional. Si esa operación sí debía llevar el nodo, el rechazo lo pone el SAT: la spec lo devuelve como **422**, el mismo estatus de un rechazo de timbrado. Cuando el nodo va, `NumeroPedimento` es obligatorio y de 21 caracteres.

### ¿Un concepto puede llevar varios pedimentos?

Sí. De 1 a 100 por concepto. Cada string se emite como su propio nodo. Los duplicados se descartan.

### ¿Lo mando con espacios o sin espacios?

Las dos formas. El patrón acepta los 15 dígitos juntos o separados por espacios (el ejemplo de la spec usa un espacio). Lo que se timbra es la cadena de 21 caracteres con dos espacios entre grupos.

### ¿La nota de crédito también lleva pedimento?

Sí, en `POST /v1/credit_notes`, el mismo arreglo `items[].pedimentos`. Mándalo en los conceptos de la nota que deban declararlo. `tipoRelacion` `03` es la devolución de mercancía.

### ¿Tengo que cambiar la integración si no vendo importados?

El campo es opcional. Un `POST /v1/invoices` que hoy no lo envía sigue igual. Lo agregas solo en el concepto que corresponda.

### ¿Lo puedo pedir desde un agente, sin escribir el JSON?

Sí. En el MCP, `create_invoice` y `create_credit_note` reciben el mismo `items[].pedimentos`. Una instrucción en español alcanza, con el pedimento de cada concepto y los datos fiscales del receptor. El ejemplo de arriba usa el número de la spec, no un pedimento real.

## Empieza hoy

1. Abre las [docs interactivas](https://api.cfdi.express/docs) y prueba `POST /v1/invoices` con `sk_test_` y un `pedimentos` de ejemplo.
2. Crea o entra a tu cuenta en [dash.cfdi.express](https://dash.cfdi.express).
3. En producción, arma los 15 dígitos desde el pedimento real (año, aduana, patente, consecutivo) y déjale a la API los dos espacios.
4. ¿Sin JSON? Conecta el [MCP](https://api.cfdi.express/mcp) y pide la factura con el pedimento en español.
5. Si aún no tienes la API, el contexto está en [Lanzamos CFDI Express API](/blog/lanzamiento-cfdi-express-api) y en la [landing](https://cfdi.express/api).

¿Quieres ver el JSON en una llamada? [Agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).

El pedimento no se “pega” a la factura. Se declara en el concepto, con 21 caracteres, y solo cuando la venta es de primera mano.
