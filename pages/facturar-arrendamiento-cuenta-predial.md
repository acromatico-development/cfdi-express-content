---
title: "Cómo facturar arrendamiento con número de cuenta predial"
description: "Número de cuenta predial en la factura de arrendamiento CFDI 4.0: cómo enviarlo con cuentasPrediales en el POST /v1/invoices y cómo se emite en el XML."
image: "https://assets.acromatico.dev/marketing/diego/blog/hero-cfdi-facturas-4.0.png"
author: "Rafael González"
date: "2026-10-28"
keywords: "cuenta predial, número de cuenta predial, facturar arrendamiento, renta de inmueble, CuentaPredial, cuentasPrediales, CFDI 4.0 arrendamiento, factura de renta, boleta predial, CFDI Express API"
---
# Cómo facturar arrendamiento con número de cuenta predial

En una factura de arrendamiento, el número de cuenta predial va en el concepto: nodo `CuentaPredial`, atributo `Numero`.

Desde la **v1.41.0** de la [API de CFDI Express](https://cfdi.express/api), ese dato es el campo opcional `items[].cuentasPrediales` de [`POST /v1/invoices`](https://cfdi.express/docs/api/timbrar-factura). El ejemplo que publica la documentación es `"cuentasPrediales": ["12345678901234"]`.

Las retenciones de ISR y de IVA de una renta dependen del caso. Confírmalas con tu contador antes de timbrar. Este post no fija tasas.

## Qué es la cuenta predial en el CFDI 4.0

**Respuesta corta:** es el número con el que el inmueble está registrado en el catastro de la entidad federativa. En el CFDI 4.0 se asienta por concepto.

En el [Anexo 20](http://omawww.sat.gob.mx/tramitesyservicios/Paginas/documentos/Anexo20_2022.pdf), `CuentaPredial` es un nodo opcional de `Concepto`. Sirve para asentar ese número de cuenta predial, o los datos de identificación del certificado de participación inmobiliaria no amortizable.

Si el nodo va, el atributo `Numero` es requerido. El estándar lo define de 1 a 150 caracteres, letras o dígitos: `[0-9a-zA-Z]{1,150}`. Es el número de la cuenta predial del inmueble cubierto por ese concepto o, tratándose de arrendamiento, la identificación de ese certificado.

## Cuándo se usa en la factura de arrendamiento

**Respuesta corta:** cuando el concepto es la renta de un inmueble y vas a declarar la cuenta predial de ese inmueble.

El contrato de la API describe `cuentasPrediales` para facturas de arrendamiento: los números de cuenta predial del inmueble de ese concepto, como vienen en la boleta predial. El campo es opcional. Lo agregas en el concepto que corresponde. Si la cadena cumple el patrón, la API emite el nodo.

Qué cruza el SAT contra el padrón del receptor está en [CFDI facturas 4.0](/blog/cfdi-facturas-4-0).

## Qué datos tener a la mano

1. **Cuenta predial**, como está impresa en la boleta: letras o dígitos, de 1 a 150, sin espacios, guiones ni puntos. El ejemplo de la spec es `12345678901234`.
2. **Qué inmueble cubre cada concepto.** La descripción del concepto es el lugar para identificar la renta (hasta 1000 caracteres).
3. **Datos fiscales del receptor:** RFC, nombre o razón social, régimen fiscal y código postal. El uso de CFDI tiene que ser compatible con el régimen.
4. **Clave de producto y clave de unidad** del catálogo del SAT. En producción salen de `GET /v1/catalogs/products` y `GET /v1/catalogs/units`.

En el ejemplo de abajo, `01010101` y `E48` solo muestran la forma del JSON en sandbox, igual que en [el post de lanzamiento](/blog/lanzamiento-cfdi-express-api). `EKU9003173C9` es el receptor de prueba de ese post. `G03`, `PUE` y `03` también son valores de ejemplo de la [referencia de timbrado](https://cfdi.express/docs/api/timbrar-factura), para completar el cuerpo. En una renta real, el uso lo elige el receptor y la forma de pago es la de esa operación.

## Impuestos de la renta

**Respuesta corta:** las retenciones de ISR y de IVA dependen del caso. Confírmalas con tu contador.

Este JSON no trae tasas. Antes de usarlo en una factura real, define con tu contador los impuestos de ese arrendamiento y mándalos en el concepto según corresponda. La cuenta predial es independiente de esa decisión.

## Ejemplo: POST /v1/invoices

El campo va dentro de cada elemento de `items`. Es un arreglo de strings. `POST /v1/invoices` pide el header `Idempotency-Key` (1 a 255 caracteres). La petición espera hasta 20 segundos el UUID y responde **201**. Si la cola está saturada, responde **202** con `status: "stamping"`: consultas [`GET /v1/invoices/{id}`](https://cfdi.express/docs/api/obtener-factura) hasta que pase a `stamped`.

```bash
curl https://api.cfdi.express/v1/invoices \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: renta-inmueble-1042" \
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
      "description": "Renta de inmueble",
      "quantity": 1,
      "unitPrice": 1500,
      "cuentasPrediales": ["12345678901234"]
    }]
  }'
```

`12345678901234` es el ejemplo publicado en la documentación y en el OpenAPI (`https://api.cfdi.express/openapi.json`). No es una boleta real. `1500` solo completa el cuerpo.

El arreglo, cuando lo mandas, admite de **1 a 100** cuentas en ese concepto. Cada una cumple:

```text
^[0-9a-zA-Z]{1,150}$
```

Los espacios en los extremos se recortan. Se rechazan guiones, puntos, espacios internos, cadenas vacías y un arreglo vacío. Para no emitir el nodo, no envíes la propiedad.

Si llega **201**, el XML y el PDF quedan en el recurso cuando `files.status` es `ready`: [`GET /v1/invoices/{id}`](https://cfdi.express/docs/api/obtener-factura) incluye `files.xmlUrl`, `files.pdfUrl` y `files.zipUrl`. La cuenta predial viaja en el XML del concepto.

La referencia interactiva está en [api.cfdi.express/docs](https://api.cfdi.express/docs). Para probar, una llave `sk_test_` en [dash.cfdi.express](https://dash.cfdi.express).

## Cómo se ve en el XML

| JSON | Atributo en el XML |
| --- | --- |
| `items[].cuentasPrediales[]` | `cfdi:Concepto/cfdi:CuentaPredial/@Numero` |

Un nodo por cuenta. Con el ejemplo de la spec, el concepto queda así:

```xml
<cfdi:Concepto>
  <cfdi:CuentaPredial Numero="12345678901234"/>
</cfdi:Concepto>
```

Si el concepto también lleva información aduanera, `CuentaPredial` se emite después de `InformacionAduanera`. Los duplicados del arreglo se descartan: una sola vez cada número. El nodo también se emite en conceptos no gravados.

En el PDF se imprime debajo de la descripción como `Cuenta predial: a, b`.

El concepto de envío no lleva cuenta predial. El número va en el concepto de la renta.

## Varios inmuebles

**Respuesta corta:** un concepto por inmueble, con su arreglo. Si un mismo concepto tiene varias cuentas, van juntas en ese arreglo.

Dos rentas en el mismo CFDI son dos elementos de `items`. Cada uno declara la cuenta del inmueble que cubre:

```json
"items": [
  {
    "productCode": "01010101",
    "unitCode": "E48",
    "description": "Renta de inmueble, calle Ejemplo 100",
    "quantity": 1,
    "unitPrice": 1500,
    "cuentasPrediales": ["12345678901234"]
  },
  {
    "productCode": "01010101",
    "unitCode": "E48",
    "description": "Renta de inmueble, calle Ejemplo 200",
    "quantity": 1,
    "unitPrice": 1500,
    "cuentasPrediales": ["98765432109876"]
  }
]
```

`12345678901234` es el ejemplo de la spec. `98765432109876` solo muestra otra cadena de la misma forma. No es una cuenta publicada.

Varias cuentas del mismo concepto —hasta 100— salen como varios nodos:

```json
"cuentasPrediales": ["12345678901234", "98765432109876"]
```

```xml
<cfdi:Concepto>
  <cfdi:CuentaPredial Numero="12345678901234"/>
  <cfdi:CuentaPredial Numero="98765432109876"/>
</cfdi:Concepto>
```

## Nota de crédito

`POST /v1/credit_notes` acepta el mismo `items[].cuentasPrediales` desde la v1.41.0. El detalle está en [crear nota de crédito](https://cfdi.express/docs/api/crear-nota-de-credito). Si el concepto de la nota debe llevar la cuenta, va en su arreglo.

## Desde el servidor MCP

El mismo arreglo se puede pedir en el [servidor MCP](https://api.cfdi.express/mcp). Describes la renta y el número de cuenta predial; el timbrado de la factura usa `cuentasPrediales` en el concepto, con el mismo contrato de la API (1 a 100 cadenas, letras o dígitos, un nodo por cuenta).

El número de abajo es el ejemplo de la spec. `<cliente>` es un hueco: el agente necesita los datos fiscales del receptor.

```text
Factura la renta del inmueble a <cliente> con la cuenta predial 12345678901234
```

El sandbox del MCP está en `https://api.cfdi.express/mcp/test`.

## Preguntas frecuentes

**¿Dónde va el número de cuenta predial en el CFDI 4.0?**
En el concepto, como `cfdi:CuentaPredial`, atributo `Numero`. En la API es `items[].cuentasPrediales`.

**¿Cuántas cuentas acepta un concepto?**
De 1 a 100, si mandas el arreglo. Cada string es de 1 a 150 letras o dígitos. Cada cuenta se emite en su propio nodo. Los duplicados se descartan. Un arreglo vacío se rechaza: para omitir el nodo, no envíes la propiedad.

**¿Puedo mandar la cuenta con guiones o espacios, como en la boleta?**
Letras o dígitos, tal como el patrón `^[0-9a-zA-Z]{1,150}$`. Los espacios de los extremos se recortan. Guiones, puntos y espacios internos se rechazan. El ejemplo de la spec es `12345678901234`.

**¿El campo es obligatorio en toda factura?**
Es opcional. Se usa en el concepto de la factura de arrendamiento que declara la cuenta de ese inmueble. Si no lo envías, el request sigue siendo válido.

**¿Dos inmuebles van en un solo concepto?**
Un concepto por inmueble, cada uno con su `cuentasPrediales`. Si un concepto cubre más de una cuenta, esas cuentas van en el arreglo de ese concepto.

**¿Qué retención de ISR o de IVA lleva la renta?**
Depende del caso. Confírmala con tu contador. Este post no fija tasas ni reglas de retención.

**¿La nota de crédito también lleva la cuenta predial?**
Sí. `POST /v1/credit_notes` usa el mismo arreglo desde la v1.41.0. La referencia está en [crear nota de crédito](https://cfdi.express/docs/api/crear-nota-de-credito).

**¿Puedo pedirlo desde un agente?**
Sí. En el [MCP](https://api.cfdi.express/mcp) el timbrado de la factura acepta el mismo `cuentasPrediales`. Alcanza una instrucción en español con la cuenta de cada inmueble y los datos fiscales del receptor.

**¿Qué hago si ya timbré la cuenta mal?**
La corrección es otro flujo. La cancelación por API está en [cancelar factura](https://cfdi.express/docs/api/cancelar-factura). El acuse, en [Acuse de cancelación](/blog/acuse-de-cancelacion).

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {"@type": "Question", "name": "¿Dónde va el número de cuenta predial en el CFDI 4.0?", "acceptedAnswer": {"@type": "Answer", "text": "En el concepto, como cfdi:CuentaPredial, atributo Numero. En la API es items[].cuentasPrediales."}},
    {"@type": "Question", "name": "¿Cuántas cuentas acepta un concepto?", "acceptedAnswer": {"@type": "Answer", "text": "De 1 a 100, si mandas el arreglo. Cada string es de 1 a 150 letras o dígitos. Cada cuenta se emite en su propio nodo. Los duplicados se descartan. Un arreglo vacío se rechaza: para omitir el nodo, no envíes la propiedad."}},
    {"@type": "Question", "name": "¿Puedo mandar la cuenta con guiones o espacios, como en la boleta?", "acceptedAnswer": {"@type": "Answer", "text": "Letras o dígitos, con el patrón ^[0-9a-zA-Z]{1,150}$. Los espacios de los extremos se recortan. Guiones, puntos y espacios internos se rechazan. El ejemplo de la spec es 12345678901234."}},
    {"@type": "Question", "name": "¿El campo es obligatorio en toda factura?", "acceptedAnswer": {"@type": "Answer", "text": "Es opcional. Se usa en el concepto de la factura de arrendamiento que declara la cuenta de ese inmueble. Si no lo envías, el request sigue siendo válido."}},
    {"@type": "Question", "name": "¿Dos inmuebles van en un solo concepto?", "acceptedAnswer": {"@type": "Answer", "text": "Un concepto por inmueble, cada uno con su cuentasPrediales. Si un concepto cubre más de una cuenta, esas cuentas van en el arreglo de ese concepto."}},
    {"@type": "Question", "name": "¿Qué retención de ISR o de IVA lleva la renta?", "acceptedAnswer": {"@type": "Answer", "text": "Depende del caso. Confírmala con tu contador. Este post no fija tasas ni reglas de retención."}},
    {"@type": "Question", "name": "¿La nota de crédito también lleva la cuenta predial?", "acceptedAnswer": {"@type": "Answer", "text": "Sí. POST /v1/credit_notes usa el mismo arreglo desde la v1.41.0."}},
    {"@type": "Question", "name": "¿Puedo pedirlo desde un agente?", "acceptedAnswer": {"@type": "Answer", "text": "Sí. En el servidor MCP el timbrado de la factura acepta el mismo cuentasPrediales. Alcanza una instrucción en español con la cuenta de cada inmueble y los datos fiscales del receptor."}},
    {"@type": "Question", "name": "¿Qué hago si ya timbré la cuenta mal?", "acceptedAnswer": {"@type": "Answer", "text": "La corrección es otro flujo. La cancelación por API está en la documentación de cancelar factura. El acuse está en el post Acuse de cancelación."}}
  ]
}
</script>

## Timbrar la renta con cuenta predial

1. Toma el número de la boleta: letras o dígitos, sin guiones, puntos ni espacios internos.
2. Ponlo en `cuentasPrediales` del concepto de esa renta. El ejemplo de la spec es `12345678901234`.
3. Contrasta el cuerpo con [Timbrar una factura](https://cfdi.express/docs/api/timbrar-factura).
4. Timbra con `sk_test_` desde [dash.cfdi.express](https://dash.cfdi.express) o pídeselo al [MCP](https://api.cfdi.express/mcp).
5. Revisa el nodo `cfdi:CuentaPredial` en el XML.

El contexto de la API está en [el anuncio de lanzamiento](/blog/lanzamiento-cfdi-express-api) y en la [landing](https://cfdi.express/api).

¿Dudas de integración? [Agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).
