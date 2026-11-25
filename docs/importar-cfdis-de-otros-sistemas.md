---
title: "Importar CFDIs de otros sistemas: REP y notas de crédito de facturas timbradas en el SAT u otro PAC"
description: "Importa facturas CFDI 4.0 timbradas en el portal del SAT u otro PAC y emíteles complementos de pago (REP) y notas de crédito por UUID con la API o el MCP de CFDI Express."
keywords: "importar CFDI, importar XML, factura timbrada en otro PAC, portal del SAT, REP de factura externa, complemento de pago factura de otro sistema, documents xml, previousPayments, nota de crédito por UUID, relatedUuid, POST /v1/invoices/import, CFDI Express API, MCP"
---
# Importar CFDIs de otros sistemas

URL de esta página: https://cfdi.express/docs/importar-cfdis-de-otros-sistemas

Disponible a partir de la versión **v1.59.0** de la API.

Si una factura PPD se timbró fuera de CFDI Express —en el portal del SAT o con otro PAC— ahora puedes emitirle sus complementos de pago (REP) y sus notas de crédito desde CFDI Express. Hay tres piezas:

1. **Importar la factura** con [`POST /v1/invoices/import`](https://cfdi.express/docs/api/importar-facturas): mandas el XML timbrado y queda en tu cuenta como una factura con `origin` `imported`.
2. **Mandar el XML dentro del REP** con `documents[].xml` en [`POST /v1/payments`](https://cfdi.express/docs/api/timbrar-complemento-de-pago-varias-facturas): la factura se importa en el mismo paso, sin costo extra.
3. **Relacionar notas de crédito por UUID** con `relatedUuid` en [`POST /v1/credit_notes`](https://cfdi.express/docs/api/crear-nota-de-credito): si el UUID está en tu cuenta, la nota se liga a esa factura y se valida igual que con `relatedInvoiceId`.

Las tres funcionan también desde el [servidor MCP](#servidor-mcp).

## Cuándo sirve

- Cambiaste de PAC y tienes facturas PPD con saldo pendiente.
- Una factura se timbró de emergencia en el portal del SAT y ahora te pagan.
- Facturas desde otro sistema y quieres emitir los REP y las notas de crédito desde CFDI Express.

Solo se importan **facturas de ingreso** (`TipoDeComprobante` `I`) **CFDI 4.0**, **PUE** o **PPD**, de un RFC que ya sea emisor en tu cuenta. Un CFDI 3.3 se rechaza.

## Antes de empezar

- El RFC emisor de la factura tiene que existir como emisor en tu cuenta **en el mismo modo** que tu llave: `sk_test_` o `sk_live_`. Si no existe, el XML se rechaza con `merchant_not_found`. Créalo con [`POST /v1/merchants`](https://cfdi.express/docs/api/crear-emisor).
- Para **timbrar** el REP o la nota de crédito, el emisor necesita su CSD ([`POST /v1/merchants/{id}/csd`](https://cfdi.express/docs/api/subir-csd)). Para solo importar no.
- Manda el **XML original**, sin abrirlo en editores que cambien espacios o acentos. Se verifica el sello del emisor y el del SAT: cualquier cambio en el XML lo invalida.

En modo de pruebas, la consulta al SAT se simula (siempre Vigente) porque los UUID del sandbox no existen en el SAT. El sello del emisor sí se verifica.

## Paso 1. Importar la factura

> `POST https://api.cfdi.express/v1/invoices/import`

Hasta 20 XML por llamada, cada uno en base64. Las facturas **PPD** necesitan `previousPayments`:

- `false` si el otro sistema **no** emitió ningún REP para esa factura.
- `{ "saldoInsoluto": …, "lastNumParcialidad": … }` con los datos del **último** REP que emitió el otro sistema.

Ejemplo: una factura PPD de $11,600.00 del emisor de pruebas `EKU9003173C9` que el otro PAC ya abonó con dos REP, y quedó un saldo de $5,800.00.

```bash
curl https://api.cfdi.express/v1/invoices/import \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: import_factura_F1024" \
  -H "Content-Type: application/json" \
  -d '{
    "invoices": [
      {
        "xml": "PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4...",
        "previousPayments": { "saldoInsoluto": 5800.00, "lastNumParcialidad": 2 }
      }
    ]
  }'
```

La respuesta es **200** con un resultado por XML:

```json
{
  "object": "invoice_import",
  "livemode": false,
  "unitPriceCentavos": 20,
  "chargedCentavos": 20,
  "imported": 1,
  "alreadyImported": 0,
  "rejected": 0,
  "results": [
    {
      "index": 0,
      "status": "imported",
      "uuid": "5B1E9A43-7C2D-4E8F-9A1B-3C4D5E6F7A8B",
      "invoiceId": "inv4m8q2x6v0t3p9d1n7cbk5",
      "chargedCentavos": 20,
      "reason": null
    }
  ]
}
```

- **`imported`**: la factura quedó en tu cuenta y se cobró. Guarda `invoiceId`.
- **`already_imported`**: el UUID ya estaba en tu cuenta (importado antes o timbrado aquí). No se cobra.
- **`rejected`**: no se importó ni se cobró. `reason.code` explica por qué: `invalid_seal`, `unsupported_version`, `merchant_not_found`, `previous_payments_required`, `sat_cancelled` y los demás de la [tabla de motivos](https://cfdi.express/docs/api/importar-facturas).

**Costo:** 20% del precio de timbre de tu cuenta por cada XML importado: **20 centavos** con el precio de lista de $1 MXN. `already_imported` y `rejected` no cuestan. `unitPriceCentavos` trae el precio aplicado.

**Idempotencia:** el header `Idempotency-Key` es opcional. Con la misma llave y el mismo cuerpo, la API devuelve el resultado guardado con `Idempotency-Replayed: true`, sin volver a consultar al SAT ni cobrar. Sin llave, repetir tampoco duplica: los UUID ya importados regresan como `already_imported`.

El contrato completo —campos, motivos de rechazo, precio, errores y el recurso de la factura importada— está en [Importar facturas](https://cfdi.express/docs/api/importar-facturas).

### Cómo queda la factura importada

En [`GET /v1/invoices/{id}`](https://cfdi.express/docs/api/obtener-factura) la factura trae `origin` `imported` y el objeto `import`:

```json
{
  "id": "inv4m8q2x6v0t3p9d1n7cbk5",
  "object": "invoice",
  "status": "stamped",
  "uuid": "5B1E9A43-7C2D-4E8F-9A1B-3C4D5E6F7A8B",
  "metodoPago": "PPD",
  "total": 11600,
  "saldoPendiente": 5800,
  "origin": "imported",
  "import": {
    "status": "active",
    "paymentId": null,
    "externalNumParcialidad": 2,
    "importedAt": "2026-11-25T18:20:41.302Z"
  },
  "files": { "status": "ready", "xmlUrl": "https://files.cfdi.express/...xml?X-Amz-Expires=900&..." }
}
```

- `saldoPendiente` es el `saldoInsoluto` que declaraste, o el `Total` con `previousPayments: false`.
- `import.externalNumParcialidad` es el `lastNumParcialidad` declarado (0 si no hubo REP previos).
- Solo hay XML (`files.xmlUrl`): no se genera PDF.
- Las facturas timbradas por CFDI Express traen `origin` `stamped`. `GET /v1/invoices?origin=imported` lista solo las importadas.
- Una factura importada **no se cancela** desde CFDI Express: `POST /v1/invoices/{id}/cancel` responde 400 con `subcode` `imported_not_cancellable`. Se cancela donde se timbró.

## Paso 2. Timbrar el REP

Con la factura importada, el REP es el de siempre: [`POST /v1/invoices/{id}/payments`](https://cfdi.express/docs/api/timbrar-complemento-de-pago) para una factura, o [`POST /v1/payments`](https://cfdi.express/docs/api/timbrar-complemento-de-pago-varias-facturas) con `documents` para varias. Una factura importada se puede mezclar con facturas timbradas en CFDI Express en el mismo REP, si comparten emisor, RFC receptor y moneda.

```bash
curl https://api.cfdi.express/v1/payments \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: pago_F1024_03" \
  -H "Content-Type: application/json" \
  -d '{
    "formaPago": "03",
    "fechaPago": "2026-11-25T12:00:00",
    "documents": [
      { "invoiceId": "inv4m8q2x6v0t3p9d1n7cbk5", "importePagado": 5800.00 }
    ]
  }'
```

Con la declaración del paso 1, este REP sale con `NumParcialidad` **3** (`lastNumParcialidad` 2 + 1), `ImpSaldoAnt` 5,800.00 y `ImpSaldoInsoluto` 0. Los impuestos de cada `DoctoRelacionado` se prorratean desde los del XML importado.

La parcialidad siguiente de una factura importada es `lastNumParcialidad` + los REP que ya timbraste aquí para ella + 1. Si mandas `numParcialidad` y no sigue esa secuencia, la API responde 400 con `expected N`.

**Revisión en el SAT antes de cada REP.** Antes de timbrar —y en la vista previa— CFDI Express vuelve a consultar al SAT si la factura importada sigue **Vigente**, por si la cancelaron en el otro sistema:

| Situación | HTTP | Respuesta |
|---|---|---|
| El SAT la reporta cancelada | 400 `validation_error`, `subcode` `sat_cancelled` | `Imported invoice inv…: The SAT reports CFDI … as cancelled` |
| El SAT no la encuentra | 400 `validation_error`, `subcode` `sat_not_found` | `Imported invoice inv…: The SAT does not know CFDI … (No Encontrado)` |
| El SAT no responde | 503 `pac_unavailable`, `subcode` `sat_unavailable` | `The SAT status service did not answer for imported invoice inv… — safe to retry` |

Todo esto pasa antes de gastar el timbre.

<h2 id="importar-dentro-del-rep">Importar dentro del REP: <code>documents[].xml</code></h2>

Si todavía no importaste la factura, puedes mandar su XML directamente en el REP. En lugar de `invoiceId`, la entrada de `documents` lleva `xml`:

| Campo | Tipo | Descripción |
|---|---|---|
| `documents[].invoiceId` | string | Factura PPD timbrada (`inv…`), aquí o importada. Exactamente uno de `invoiceId` o `xml`. |
| `documents[].xml` | string | El XML timbrado CFDI 4.0 de una factura **PPD** timbrada en otro sistema, **en base64**. Exactamente uno de `invoiceId` o `xml`. |
| `documents[].previousPayments` | `false` u objeto | Solo con `xml`. **Obligatorio cuando el UUID es nuevo en la cuenta**: `false` si el otro sistema no emitió REP para esa factura, o `{ saldoInsoluto, lastNumParcialidad }` del último que emitió. Con `invoiceId` no se acepta. |
| `documents[].importePagado` | number | Igual que siempre. |
| `documents[].numParcialidad` | integer | Opcional, igual que siempre. |

Funciona en `POST /v1/payments`, en [`POST /v1/payments/preview`](https://cfdi.express/docs/api/vista-previa-complemento-de-pago) y en [`POST /v1/payments/preview/pdf`](https://cfdi.express/docs/api/vista-previa-complemento-de-pago-pdf). `POST /v1/invoices/{id}/payments` sigue recibiendo solo el id.

```bash
curl https://api.cfdi.express/v1/payments \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: pago_F1031_01" \
  -H "Content-Type: application/json" \
  -d '{
    "formaPago": "03",
    "fechaPago": "2026-11-25T12:00:00",
    "documents": [
      {
        "xml": "PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4...",
        "previousPayments": false,
        "importePagado": 11600.00
      },
      { "invoiceId": "inv2p8d4n6c0b5rzmkq2f8v3", "importePagado": 2320.00 }
    ]
  }'
```

Así se comporta:

- **Se verifica igual que en el import**: sellos, estatus Vigente en el SAT y emisor que sea uno de tus emisores. Todo antes de reservar el timbre.
- **Si el UUID ya está en tu cuenta** (importado o timbrado aquí), se usa esa factura: no se importa otra vez y `previousPayments` se ignora.
- **Si el UUID es nuevo**, la factura se importa **gratis**, pero **solo si el REP se timbra**. Mientras el REP se timbra, la factura existe con `import.status` `pending` e `import.paymentId` con el id del REP. Cuando el REP queda timbrado pasa a `active`.
- **Si el SAT rechaza el REP o el PAC falla**, la importación se descarta: esa factura deja de existir para la API y puedes volver a intentar el REP con el XML.
- **El REP cuesta un timbre**, como siempre. La importación dentro del REP no se cobra aparte.
- **Parcialidad y saldo**: con `previousPayments: false` el REP es la parcialidad 1 y el saldo anterior es el `Total`; con el objeto, la parcialidad es `lastNumParcialidad` + 1 y el saldo anterior es `saldoInsoluto`.
- **Hasta 10 MB.** `POST /v1/payments` acepta cuerpos de hasta 10 MB para dar cabida a los XML. La vista previa y su PDF mantienen el límite general de 1 MB.

En la respuesta del REP, `documents[].invoiceId` trae el id de la factura importada. En la **vista previa** no se escribe nada: la entrada de un XML nuevo sale con `invoiceId` `null` y su `uuid`, porque la factura solo se importa al timbrar.

### Errores de `documents[].xml`

Ninguno gasta el timbre: casi todos llegan antes de reservarlo, y en `import_race` el saldo reservado se devuelve. El rechazo de un XML rechaza el REP completo.

| Situación | HTTP | Respuesta |
|---|---|---|
| La entrada trae `invoiceId` y `xml`, o ninguno | 400 `validation_error` | `Provide exactly one of invoiceId or xml` (en `errors[]`, path `invoiceId`) |
| `previousPayments` sin `xml` | 400 `validation_error` | `previousPayments only applies with xml` |
| PPD nueva sin `previousPayments` | 400 `validation_error`, `subcode` `previous_payments_required` | `documents xml #1 (UUID …): PPD invoices need previousPayments: false when no REP was issued for it elsewhere, or { saldoInsoluto, lastNumParcialidad } from the last REP issued by the other system` |
| El XML no pasa una revisión (sello, versión, formato, SAT…) | 400 `validation_error`, `subcode` = el motivo (`invalid_seal`, `unsupported_version`, `sat_cancelled`…) | `documents xml #N: …` o `documents xml #N (UUID …): …`, con el mismo mensaje que el import |
| El emisor no es tuyo en este modo | 400 `validation_error`, `subcode` `merchant_not_found` | `documents xml #1 (UUID …): Emisor EKU9003173C9 is not one of your merchants in test mode` |
| El SAT no respondió a tiempo | 503 `pac_unavailable`, `subcode` `sat_unavailable` | Se puede reintentar. |
| El mismo `invoiceId` dos veces | 400 `validation_error` | `Invoice inv… appears more than once in documents` |
| El mismo UUID dos veces (dos XML iguales, o un XML de una factura que también mandaste por `invoiceId`) | 400 `validation_error` | `Invoice … (UUID …) appears more than once in documents` |
| El UUID está ligado a otro REP que todavía se timbra | 409 `conflict`, `subcode` `import_pending` | `This imported invoice is attached to payment pay…, which is still stamping — retry once that payment is resolved`. Trae `invoiceId` y `paymentId`. |
| Otra petición importó el mismo UUID al mismo tiempo | 409 `conflict`, `subcode` `import_race` | `CFDI … was imported by another request meanwhile — retry this payment` |

El número `#N` cuenta solo las entradas que traen `xml`, empezando en 1. Las reglas del REP (PPD, saldo, emisor, RFC receptor, moneda, `monto`) son las de siempre; en sus mensajes, una factura que todavía no está importada aparece como `UUID …` en lugar de `inv…`. Esas reglas están en [Cómo emitir un REP para varias facturas PPD](https://cfdi.express/blog/emitir-rep-varias-facturas-ppd), sección «Errores de validación».

### `previous_payments_required` en el import

El mismo código aparece en `POST /v1/invoices/import` como `reason.code` de un XML rechazado: la factura es PPD, el UUID es nuevo y no trae `previousPayments`. El XML no se importa ni se cobra; los demás XML de la petición siguen su curso. Vuelve a mandarlo con `false` o con `{ saldoInsoluto, lastNumParcialidad }`.

CFDI Express no puede saber si el otro sistema ya emitió REP para la factura. Si la declaración está mal, el siguiente REP sale con el saldo anterior o la parcialidad equivocados. Confírmalo con quien emitió los REP anteriores.

<h2 id="notas-de-credito-por-uuid">Notas de crédito por UUID: <code>relatedUuid</code></h2>

[`POST /v1/credit_notes`](https://cfdi.express/docs/api/crear-nota-de-credito) relaciona la nota con la factura de dos formas, y siempre se manda **exactamente una**:

- `relatedInvoiceId`: el id (`inv…`) de la factura en CFDI Express.
- `relatedUuid`: el UUID (folio fiscal) de la factura.

Desde la v1.59.0, `relatedUuid` busca primero ese UUID en tu cuenta, en el mismo modo, **sin importar mayúsculas y minúsculas**. Encuentra facturas timbradas en CFDI Express y facturas importadas.

**Si lo encuentra**, la nota se liga a esa factura (`relatedInvoiceId` queda con su id) y se valida exactamente igual que con `relatedInvoiceId`:

- La factura tiene que ser de **ingreso** (no otra nota de crédito) y estar **timbrada** (`stamped`): no `stamping`, `stamp_failed`, `cancel_pending` ni `cancelled`.
- **Mismo emisor**: el `merchantId` de la nota es el de la factura.
- **Misma moneda**: si no mandas `currency`, se toma la de la factura; si la mandas distinta, responde 400.
- **Anticipos**: las reglas de la relación `07` y la forma `30` de [Facturar anticipos](/docs/facturar-anticipos).
- **Tope de crédito**: la suma de las notas vigentes (`stamping` o `stamped`) ligadas a la factura más esta nota no puede pasar del total de la factura. Si pasa: `subcode` `over_credit`, con `invoiceTotal` y `alreadyCredited`.
- `receiver` es opcional: si no lo mandas, se copia de la factura.

En el CFDI, el UUID relacionado va como lo mandaste en `relatedUuid`.

**Si no lo encuentra**, la nota se trata como relacionada a un CFDI externo, igual que antes de la v1.59.0: `receiver` es obligatorio, `currency` es MXN si no la mandas y no hay tope de crédito, porque no hay factura contra la cual contar. Una factura que llegó en `documents[].xml` de un REP que todavía se timbra (`import.status` `pending`) también se trata como externa.

Aplica igual en [`POST /v1/credit_notes/preview`](https://cfdi.express/docs/api/vista-previa-nota-de-credito) y en [`POST /v1/credit_notes/preview/pdf`](https://cfdi.express/docs/api/vista-previa-nota-de-credito-pdf). La vista previa devuelve `relatedInvoiceId`: si no es `null`, el UUID se encontró y la nota se va a ligar.

```bash
curl https://api.cfdi.express/v1/credit_notes \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: devolucion_F1024_01" \
  -H "Content-Type: application/json" \
  -d '{
    "merchantId": "mkq2f8v3x1t7p9d4n6c0b5rz",
    "relatedUuid": "5b1e9a43-7c2d-4e8f-9a1b-3c4d5e6f7a8b",
    "tipoRelacion": "01",
    "formaPago": "03",
    "items": [
      {
        "productCode": "43201808",
        "unitCode": "H87",
        "description": "Devolución de teclado mecánico",
        "quantity": 1,
        "unitPrice": 1160.00,
        "ivaRate": 0.16
      }
    ],
    "pricesIncludeTax": true
  }'
```

El UUID va en minúsculas y la factura importada lo tiene en mayúsculas: se encuentra igual. No hace falta `receiver` porque se copia de la factura.

### Errores de `relatedUuid`

Todos son **400** `validation_error`, antes de gastar el timbre.

| Situación | `detail` |
|---|---|
| Mandas `relatedInvoiceId` y `relatedUuid`, o ninguno | `Provide exactly one of relatedInvoiceId (invoice stamped here) or relatedUuid (external CFDI)` |
| `relatedUuid` no tiene formato de UUID | `relatedUuid must be a SAT UUID (folio fiscal)` (en `errors[]`) |
| El UUID es de una nota de crédito | `Cannot relate a credit note to another credit note` |
| La factura no está timbrada o está cancelada | `Invoice inv… (relatedUuid …) is cancelled — credit notes require a stamped invoice` (con el estatus que tenga: `cancelled`, `cancel_pending`, `stamping`, `stamp_failed`) |
| La factura es de otro emisor de tu cuenta | `Invoice inv… (relatedUuid …) belongs to a different merchant (emisor must match)` |
| `currency` distinta de la de la factura | `The related invoice is in USD — the credit note must be in the same currency` |
| La nota pasa del saldo por acreditar | `Credit note of 2000.00 exceeds the remaining creditable amount 1160.00 of invoice inv…`, con `subcode` `over_credit`, `invoiceTotal` y `alreadyCredited` |
| Reglas de anticipo | `subcode` `anticipo_rules` u `over_apply_anticipo`. Están en [Facturar anticipos](/docs/facturar-anticipos#errores-al-facturar-anticipos). |
| El UUID no está en tu cuenta y no mandas `receiver` | `receiver is required when relating by relatedUuid` |

Ejemplo de `over_credit`:

```json
{
  "type": "https://api.cfdi.express/docs/errors/validation_error",
  "title": "Validation error",
  "status": 400,
  "code": "validation_error",
  "detail": "Credit note of 2000.00 exceeds the remaining creditable amount 1160.00 of invoice inv4m8q2x6v0t3p9d1n7cbk5",
  "subcode": "over_credit",
  "invoiceTotal": 11600,
  "alreadyCredited": 10440
}
```

### Notas emitidas antes de importar la factura

Si timbraste notas con `relatedUuid` cuando la factura todavía no estaba en tu cuenta, al importarla se ligan solas: las notas de la misma cuenta y modo, sin `relatedInvoiceId`, cuyo `relatedUuid` coincide con el UUID de la factura (sin importar mayúsculas). Desde ese momento traen `relatedInvoiceId` y cuentan para el tope de crédito de la factura.

También pasa cuando el import devuelve `already_imported` sobre una factura importada: se ligan las notas emitidas por UUID desde la importación anterior. Una nota ya ligada no cambia.

<h2 id="servidor-mcp">Servidor MCP</h2>

El [servidor MCP](https://cfdi.express/docs/api/mcp) suma una herramienta nueva, `import_invoices`, y amplía otras.

| Herramienta | Qué cambia |
|---|---|
| `import_invoices` | **Nueva.** Llama a `POST /v1/invoices/import`. `invoices`: de 1 a **5** elementos, cada uno con `xml` **como texto** (no base64; la herramienta lo codifica) y `previousPayments`. `idempotency_key` opcional; si no la mandas, se genera una al azar. Mismo precio y mismos resultados que la API. |
| `create_payment`, `preview_payment`, `download_payment_preview_pdf` | Cada entrada de `documents` acepta `xml` (el XML **como texto**) en lugar de `invoiceId`, con `previousPayments`. Como máximo **5** entradas con `xml` por llamada; si mandas más: `At most 5 documents may carry xml in one call.` La vista previa muestra la factura nueva como `UUID … (imported if this REP stamps)`. |
| `create_credit_note`, `preview_credit_note`, `download_credit_note_preview_pdf` | `relatedUuid` se liga a la factura de la cuenta cuando el UUID coincide, con las mismas reglas que `relatedInvoiceId`. |
| `list_invoices` | Nuevo filtro `origin`: `stamped` o `imported`. |

Las instrucciones del servidor le piden al agente que, por cada factura PPD, te pregunte si ya se emitieron REP en otro sistema antes de mandar `previousPayments`.

Instrucciones de ejemplo:

```text
Importa este XML de una factura PPD que timbré en el portal del SAT. No se le ha emitido ningún complemento de pago.
```

```text
Registra un pago por transferencia de 5,800.00 del 25 de noviembre para la factura F-1024 que importé. El otro PAC ya había emitido dos REP y el saldo quedó en 5,800.00.
```

```text
Emite una nota de crédito por devolución de 1,160.00 IVA incluido relacionada al UUID 5b1e9a43-7c2d-4e8f-9a1b-3c4d5e6f7a8b.
```

## Limitaciones

- **Solo CFDI 4.0 de ingreso**, PUE o PPD. No se importan CFDI 3.3, egresos, pagos, nóminas ni traslados.
- **Sin PDF.** La factura importada solo tiene XML.
- **No se cancela ni se vuelve a timbrar** desde CFDI Express. Se cancela en el portal del SAT o en el PAC donde se timbró.
- **Los REP del otro sistema no se importan.** Solo se declara el último saldo y la última parcialidad con `previousPayments`.
- **El emisor tiene que existir** en tu cuenta y en el mismo modo antes de importar.
- **Sin webhooks de importación.** Importar no dispara `invoice.stamped`. Los REP y las notas que timbres después sí disparan sus eventos.

## Preguntas frecuentes

**¿Qué facturas puedo importar?**
Facturas de ingreso CFDI 4.0, PUE o PPD, timbradas en el portal del SAT o con otro PAC, cuyo RFC emisor ya sea emisor en tu cuenta en el mismo modo. El SAT tiene que reportarlas Vigentes.

**¿Cuánto cuesta importar?**
El 20% del precio de timbre de tu cuenta por cada XML importado: 20 centavos con el precio de lista de $1 MXN. Los XML ya importados o rechazados no cuestan. Importar dentro de un REP con documents[].xml no se cobra aparte del timbre del REP.

**¿Qué es previousPayments?**
La declaración de los REP que el otro sistema ya emitió para una factura PPD. false si no emitió ninguno; si emitió, el saldo insoluto y la parcialidad del último. Es obligatoria para toda factura PPD nueva en la cuenta; sin ella el XML se rechaza con previous_payments_required.

**¿Puedo emitir el REP sin importar antes la factura?**
Sí. Manda el XML en base64 en documents[].xml de POST /v1/payments, con previousPayments. La factura se importa gratis, solo si el REP se timbra.

**¿Puedo relacionar una nota de crédito solo con el UUID?**
Sí, con relatedUuid. Si el UUID está en tu cuenta (sin importar mayúsculas), la nota se liga a esa factura y se valida como con relatedInvoiceId: factura de ingreso timbrada, mismo emisor, misma moneda, anticipos y tope de crédito. Si no está, se trata como CFDI externo y necesita receiver.

**¿Qué pasa si cancelan la factura en el otro sistema?**
Antes de cada REP, CFDI Express consulta al SAT. Si la factura importada ya no está Vigente, el REP responde 400 con subcode sat_cancelled o sat_not_found, antes de gastar el timbre.

**¿Puedo cancelar una factura importada desde CFDI Express?**
No. La cancelación responde 400 con subcode imported_not_cancellable. Se cancela en el portal del SAT o en el PAC donde se timbró.

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {"@type": "Question", "name": "¿Qué facturas puedo importar?", "acceptedAnswer": {"@type": "Answer", "text": "Facturas de ingreso CFDI 4.0, PUE o PPD, timbradas en el portal del SAT o con otro PAC, cuyo RFC emisor ya sea emisor en tu cuenta en el mismo modo. El SAT tiene que reportarlas Vigentes."}},
    {"@type": "Question", "name": "¿Cuánto cuesta importar?", "acceptedAnswer": {"@type": "Answer", "text": "El 20% del precio de timbre de tu cuenta por cada XML importado: 20 centavos con el precio de lista de $1 MXN. Los XML ya importados o rechazados no cuestan. Importar dentro de un REP con documents[].xml no se cobra aparte del timbre del REP."}},
    {"@type": "Question", "name": "¿Qué es previousPayments?", "acceptedAnswer": {"@type": "Answer", "text": "La declaración de los REP que el otro sistema ya emitió para una factura PPD. false si no emitió ninguno; si emitió, el saldo insoluto y la parcialidad del último. Es obligatoria para toda factura PPD nueva en la cuenta; sin ella el XML se rechaza con previous_payments_required."}},
    {"@type": "Question", "name": "¿Puedo emitir el REP sin importar antes la factura?", "acceptedAnswer": {"@type": "Answer", "text": "Sí. Manda el XML en base64 en documents[].xml de POST /v1/payments, con previousPayments. La factura se importa gratis, solo si el REP se timbra."}},
    {"@type": "Question", "name": "¿Puedo relacionar una nota de crédito solo con el UUID?", "acceptedAnswer": {"@type": "Answer", "text": "Sí, con relatedUuid. Si el UUID está en tu cuenta (sin importar mayúsculas), la nota se liga a esa factura y se valida como con relatedInvoiceId: factura de ingreso timbrada, mismo emisor, misma moneda, anticipos y tope de crédito. Si no está, se trata como CFDI externo y necesita receiver."}},
    {"@type": "Question", "name": "¿Qué pasa si cancelan la factura en el otro sistema?", "acceptedAnswer": {"@type": "Answer", "text": "Antes de cada REP, CFDI Express consulta al SAT. Si la factura importada ya no está Vigente, el REP responde 400 con subcode sat_cancelled o sat_not_found, antes de gastar el timbre."}},
    {"@type": "Question", "name": "¿Puedo cancelar una factura importada desde CFDI Express?", "acceptedAnswer": {"@type": "Answer", "text": "No. La cancelación responde 400 con subcode imported_not_cancellable. Se cancela en el portal del SAT o en el PAC donde se timbró."}}
  ]
}
</script>

## Relacionados

- [Importar facturas](https://cfdi.express/docs/api/importar-facturas): `POST /v1/invoices/import`
- [REP de varias facturas](https://cfdi.express/docs/api/timbrar-complemento-de-pago-varias-facturas): `POST /v1/payments`
- [Cómo emitir un REP para varias facturas PPD](https://cfdi.express/blog/emitir-rep-varias-facturas-ppd)
- [Crear una nota de crédito](https://cfdi.express/docs/api/crear-nota-de-credito): `POST /v1/credit_notes`
- [Facturar anticipos](/docs/facturar-anticipos)
- [Complementos de pago (PPD)](/docs/complementos-de-pago)
- [Servidor MCP](https://cfdi.express/docs/api/mcp)

---

Referencia completa: https://cfdi.express/docs/api · OpenAPI: https://api.cfdi.express/openapi.json · Llaves: https://dash.cfdi.express
