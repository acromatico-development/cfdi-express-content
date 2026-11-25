# Importar facturas de otros sistemas

> `POST https://api.cfdi.express/v1/invoices/import` — Importa hasta 20 facturas de ingreso CFDI 4.0 (PUE o PPD) que uno de tus emisores timbró en otro sistema: el portal del SAT u otro PAC. Una vez importadas, puedes emitirles complementos de pago (REP) y notas de crédito desde CFDI Express.

Disponible a partir de la versión **v1.59.0** de la API.

URL de esta página: https://cfdi.express/docs/api/importar-facturas
Autenticación: requiere API key (`Authorization: Bearer sk_test_…` o `sk_live_…`)

Cada XML se verifica antes de guardar nada y vuelve con su propio resultado: `imported`, `already_imported` o `rejected` con el motivo. Una factura importada no se timbra otra vez: CFDI Express guarda el XML original tal como lo timbró el otro sistema.

El paso a paso, con el REP y las notas de crédito de esas facturas, está en [Importar CFDIs de otros sistemas](https://cfdi.express/docs/importar-cfdis-de-otros-sistemas).

## Qué se verifica

Cada XML pasa por estas revisiones, en este orden. La primera que falla define el motivo del rechazo.

1. **Formato.** El `xml` es base64 válido, UTF-8, XML bien formado y sin `DOCTYPE` ni `ENTITY`, de hasta 4 MB.
2. **Versión y tipo.** CFDI **4.0** (un CFDI 3.3 se rechaza), `TipoDeComprobante` `I` (ingreso), `MetodoPago` `PUE` o `PPD`, y timbrado con `TimbreFiscalDigital` 1.1.
3. **Sello del emisor.** El certificado del XML es legible, su número es el `NoCertificado`, su RFC es el del emisor, estaba vigente en la `Fecha` del CFDI, y el `Sello` coincide con la cadena original 4.0. Si alguien modificó el XML después de timbrarlo, aquí se rechaza.
4. **Timbre.** El `SelloCFD` del timbre es el `Sello` del comprobante.
5. **Repetidos en la petición.** El mismo UUID (sin importar mayúsculas) dos veces en `invoices`.
6. **Emisor.** El RFC emisor es uno de tus emisores en el modo de la llave (`sk_test_` o `sk_live_`).
7. **UUID ya en la cuenta.** Si ya existe, el resultado es `already_imported` y no se revisa nada más.
8. **Pagos previos.** Las facturas PPD necesitan `previousPayments`; las PUE no lo aceptan.
9. **SAT.** El `SelloSAT` se verifica con el certificado que el SAT publica para ese `NoCertificadoSAT`, y el servicio de consulta de CFDI del SAT debe responder **Vigente**.

En modo de pruebas (`sk_test_`) el paso 9 se simula: el estatus siempre es Vigente y no se descarga el certificado del SAT, porque los UUID del sandbox no existen en el SAT. Los pasos 1 a 8 sí se aplican: el XML tiene que estar sellado con el CSD de un emisor que exista en modo de pruebas.

## Petición

### Headers

| Header | Requerido | Descripción |
|---|---|---|
| `Authorization` | sí | `Bearer sk_test_…` o `Bearer sk_live_…`. |
| `Idempotency-Key` | no | De 1 a 255 caracteres. Repetir la petición con la misma llave devuelve el resultado guardado sin volver a consultar al SAT ni cobrar. Ver [Idempotencia](#idempotencia). |
| `x-account-id` | no | En un workspace, la cuenta. |

El cuerpo puede pesar hasta **10 MB** (el resto de la API acepta 1 MB).

### Cuerpo

| Campo | Tipo | Requerido | Descripción |
|---|---|---|---|
| `invoices` | array | sí | De **1 a 20** facturas. |
| `invoices[].xml` | string | sí | El XML timbrado del CFDI 4.0, **en base64** (UTF-8). |
| `invoices[].previousPayments` | `false` u objeto | solo PPD | Obligatorio en facturas **PPD**; no se acepta en **PUE**. `false` si el otro sistema no emitió ningún REP para esa factura. Si sí emitió, el objeto con los datos del **último** REP que emitió. |
| `invoices[].previousPayments.saldoInsoluto` | number | sí, en el objeto | `ImpSaldoInsoluto` del último REP emitido en el otro sistema. Entre 0 y el `Total` de la factura. |
| `invoices[].previousPayments.lastNumParcialidad` | integer | sí, en el objeto | `NumParcialidad` de ese último REP, 1 o mayor. El siguiente REP que timbres en CFDI Express lleva `lastNumParcialidad + 1`. |

Con `previousPayments: false`, el saldo pendiente de la factura importada es su `Total` y su primer REP en CFDI Express es la parcialidad 1. Con el objeto, el saldo pendiente es `saldoInsoluto`.

## Ejemplos

### cURL

Tres facturas del emisor de pruebas `EKU9003173C9`: una PPD sin REP previos, una PPD que ya tuvo dos REP en otro PAC y una PUE. Los `xml` están recortados.

```bash
curl https://api.cfdi.express/v1/invoices/import \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: import_lote_2026_11_01" \
  -H "Content-Type: application/json" \
  -d '{
    "invoices": [
      { "xml": "PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4...", "previousPayments": false },
      {
        "xml": "PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4...",
        "previousPayments": { "saldoInsoluto": 5800.00, "lastNumParcialidad": 2 }
      },
      { "xml": "PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4..." }
    ]
  }'
```

Para codificar un archivo en base64 desde la terminal: `base64 -w0 factura.xml` (Linux) o `base64 -i factura.xml` (macOS).

## Respuesta

### 200 OK

La respuesta es **200** aunque algún XML se rechace: el resultado de cada uno viene en `results`, en el mismo orden que `invoices`.

```json
{
  "object": "invoice_import",
  "livemode": false,
  "unitPriceCentavos": 20,
  "chargedCentavos": 20,
  "imported": 1,
  "alreadyImported": 1,
  "rejected": 1,
  "results": [
    {
      "index": 0,
      "status": "imported",
      "uuid": "5B1E9A43-7C2D-4E8F-9A1B-3C4D5E6F7A8B",
      "invoiceId": "inv4m8q2x6v0t3p9d1n7cbk5",
      "chargedCentavos": 20,
      "reason": null
    },
    {
      "index": 1,
      "status": "already_imported",
      "uuid": "9D3F6B21-0A4C-4B7E-8F2D-6E5C4B3A2F10",
      "invoiceId": "inv8t2v6p0d4n9c3b7mkq1x5",
      "chargedCentavos": 0,
      "reason": null
    },
    {
      "index": 2,
      "status": "rejected",
      "uuid": "C7A2E5D8-3B6F-4A19-8C0E-2D7F1B4A9E63",
      "invoiceId": null,
      "chargedCentavos": 0,
      "reason": {
        "code": "invalid_seal",
        "message": "The emisor's Sello does not match the CFDI content (cadena original 4.0) — the XML was altered or is not the stamped original"
      }
    }
  ]
}
```

| Campo | Descripción |
|---|---|
| `object` | Siempre `invoice_import`. |
| `livemode` | `false` con `sk_test_`, `true` con `sk_live_`. |
| `unitPriceCentavos` | Precio por XML importado, en centavos de MXN. Ver [Precio](#precio). |
| `chargedCentavos` | Total cobrado del saldo en esta llamada: `unitPriceCentavos` × importadas. |
| `imported`, `alreadyImported`, `rejected` | Cuántos XML quedaron en cada resultado. |
| `results[].index` | Posición del XML en `invoices`. |
| `results[].status` | `imported`, `already_imported` o `rejected`. |
| `results[].uuid` | UUID (folio fiscal) del XML, con las mayúsculas del XML. `null` si el XML no se pudo leer. |
| `results[].invoiceId` | La factura (`inv…`) en CFDI Express, en `imported` y `already_imported`. `null` en `rejected`. |
| `results[].chargedCentavos` | Lo cobrado por ese XML: solo los `imported` cobran. |
| `results[].reason` | `{ code, message }` en `rejected`; `null` en los demás. |

## Resultados por XML

### `imported`

La factura se creó en tu cuenta con `origin` `imported` y se cobró. Guarda el `invoiceId`: es el que usas en [`POST /v1/payments`](https://cfdi.express/docs/api/timbrar-complemento-de-pago-varias-facturas), [`POST /v1/invoices/{id}/payments`](https://cfdi.express/docs/api/timbrar-complemento-de-pago) o como `relatedInvoiceId` de una nota de crédito.

### `already_imported`

El UUID ya estaba en tu cuenta, en el mismo modo, y `invoiceId` es esa factura. No se cobra ni se vuelve a consultar al SAT. También sale así un XML que CFDI Express timbró en tu cuenta: la factura ya existe.

Cuando la factura existente es una importación, la importación repetida vuelve a ligar las notas de crédito pendientes (ver [Notas de crédito emitidas antes de importar](#notas-de-credito-emitidas-antes-de-importar)).

<h3 id="rejected"><code>rejected</code></h3>

El XML no se importó y no se cobró. `reason.code` dice por qué:

| `reason.code` | Qué significa | Qué hacer |
|---|---|---|
| `invalid_base64` | `xml` no es base64 válido. | Codifica el archivo completo en base64. |
| `malformed_xml` | No es UTF-8, no es XML bien formado, trae `DOCTYPE`/`ENTITY` o pasa de 4 MB. | Manda el XML original, sin modificar. |
| `unsupported_version` | Es un CFDI 3.3 (o 3.2) u otra versión distinta de 4.0. | Solo se importa CFDI 4.0. |
| `not_a_cfdi` | La raíz no es un `cfdi:Comprobante` 4.0 o le faltan Emisor, Receptor, Conceptos o atributos obligatorios. | Revisa que sea el XML del CFDI y no el acuse u otro archivo. |
| `unsupported_type` | `TipoDeComprobante` distinto de `I`. | Solo se importan facturas de ingreso. No se importan egresos, pagos, nóminas ni traslados. |
| `unsupported_metodo_pago` | `MetodoPago` distinto de `PUE` o `PPD`, o ausente. | — |
| `missing_tfd` | No está timbrado: no trae `TimbreFiscalDigital` 1.1 o su UUID no es válido. | Manda el XML ya timbrado. |
| `invalid_certificate` | El certificado del XML no se lee, no corresponde al `NoCertificado` o al RFC del emisor, o no estaba vigente en la `Fecha` del CFDI. | — |
| `invalid_seal` | El `Sello` del emisor no coincide con el contenido: el XML se alteró después de timbrarse o no es el original. | Descarga otra vez el XML del portal del SAT o del PAC y no lo abras con editores que cambien espacios o acentos. |
| `invalid_tfd_seal` | El `SelloCFD` del timbre no es el `Sello` del comprobante, o el `SelloSAT` no se verifica. | El timbre no corresponde a este XML o fue alterado. |
| `sat_certificate_unavailable` | El SAT no publica el certificado que selló el timbre. | — |
| `sat_cancelled` | El SAT reporta el CFDI como **cancelado**. | No se importa una factura cancelada. |
| `sat_not_found` | El SAT no conoce el CFDI (**No Encontrado**). | Si se timbró hace muy poco, reintenta más tarde. |
| `sat_unavailable` | El SAT no respondió a tiempo. | Reintenta ese XML. Ver [Idempotencia](#idempotencia). |
| `merchant_not_found` | El RFC emisor no es uno de tus emisores en este modo. Mensaje: `Emisor EKU9003173C9 is not one of your merchants in test mode — create it first`. | Crea el emisor con [`POST /v1/merchants`](https://cfdi.express/docs/api/crear-emisor) en el mismo modo y vuelve a importar. |
| `duplicate_in_request` | El mismo UUID aparece más de una vez en `invoices`. Se importa la primera; las demás se rechazan. | Manda cada XML una sola vez. |
| `previous_payments_required` | Es una factura **PPD** y no trae `previousPayments`. | Pregunta si el otro sistema ya emitió REP para esa factura y manda `false` o `{ saldoInsoluto, lastNumParcialidad }`. |
| `invalid_previous_payments` | `previousPayments` en una factura PUE, `saldoInsoluto` fuera de 0 a `Total`, o `lastNumParcialidad` menor que 1. | — |

#### `previous_payments_required`

CFDI Express no puede saber si el otro sistema ya emitió REP para una factura PPD. Si adivina mal, el siguiente REP sale con el saldo anterior o la parcialidad equivocados. Por eso toda factura PPD nueva para la cuenta pide la declaración:

- **Sin REP previos:** `"previousPayments": false`. El saldo pendiente es el `Total` y el primer REP aquí es la parcialidad 1.
- **Con REP previos:** `"previousPayments": { "saldoInsoluto": 5800.00, "lastNumParcialidad": 2 }`, con los datos del último REP emitido en el otro sistema. El saldo pendiente es 5,800.00 y el siguiente REP aquí es la parcialidad 3.

Mensaje: `PPD invoices need previousPayments: false when no REP was issued for it elsewhere, or { saldoInsoluto, lastNumParcialidad } from the last REP issued by the other system`.

No aplica a un UUID que ya está en la cuenta: ese resultado es `already_imported` y `previousPayments` se ignora.

<h2 id="precio">Precio</h2>

Cada XML con resultado `imported` cuesta el **20% del precio de timbre de tu cuenta**. Con el precio de lista de $1 MXN por timbre son **20 centavos ($0.20 MXN)**. `unitPriceCentavos` trae el precio que se aplicó.

- `already_imported` y `rejected` no se cobran.
- Una importación no cuenta como timbre en tu uso mensual, pero sí suma al gasto del mes, así que respeta el límite mensual de la cuenta.
- El cobro va en la misma transacción que crea las facturas. Si no alcanza el saldo para todas las facturas que pasaron las revisiones, la respuesta es **402** y no se importa ni se cobra ninguna.
- Los cargos aparecen en [el ledger de transacciones](https://cfdi.express/docs/api/transacciones) como consumo, uno por factura importada.

Importar la factura dentro de un REP con `documents[].xml` no cuesta nada aparte del timbre del REP. Está en [Importar CFDIs de otros sistemas](https://cfdi.express/docs/importar-cfdis-de-otros-sistemas#importar-dentro-del-rep).

<h2 id="idempotencia">Idempotencia</h2>

`Idempotency-Key` es **opcional** en este endpoint.

| Situación | Respuesta |
|---|---|
| Misma llave, mismo cuerpo, petición ya resuelta | El mismo **200** guardado, con el header `Idempotency-Replayed: true`. No se vuelve a consultar al SAT y no se cobra otra vez. |
| Misma llave, otra petición todavía en proceso | **409** `request_in_flight`. |
| Misma llave, cuerpo distinto | **422** `idempotency_key_reuse`. |
| La petición terminó en 402 o en otro error | La llave se libera: puedes reintentar con la misma llave cuando lo resuelvas. |

Sin llave, repetir la petición tampoco duplica nada: los UUID que ya se importaron regresan como `already_imported`, sin costo.

Un replay devuelve el resultado guardado **tal cual**, incluidos los `rejected` por `sat_unavailable`. Para reintentar esos XML, mándalos en una petición nueva, con otra llave o sin llave.

<h2 id="notas-de-credito-emitidas-antes-de-importar">Notas de crédito emitidas antes de importar</h2>

Si ya habías timbrado notas de crédito con [`relatedUuid`](https://cfdi.express/docs/importar-cfdis-de-otros-sistemas#notas-de-credito-por-uuid) apuntando a esta factura cuando todavía no estaba en la cuenta, al importarla se ligan solas. Se ligan las notas que cumplen todo esto:

- Son de la misma cuenta y el mismo modo (`livemode`).
- No tienen `relatedInvoiceId`.
- Su `relatedUuid` es el UUID de la factura, sin importar mayúsculas y minúsculas. El portal del SAT entrega los UUID en mayúsculas y otros sistemas en minúsculas.

Desde ese momento esas notas traen `relatedInvoiceId` con el id de la factura importada y cuentan para el tope de crédito (`alreadyCredited` y el error `over_credit`).

Lo mismo pasa cuando el resultado es `already_imported` y la factura existente es una importación: se ligan las notas emitidas por UUID desde la importación anterior. Una nota ya ligada no se vuelve a tocar.

## La factura importada en `GET /v1/invoices/{id}`

Desde la v1.59.0, el recurso de factura trae `origin`:

- `stamped`: timbrada por CFDI Express.
- `imported`: timbrada en otro sistema e importada con este endpoint o con `documents[].xml` en un REP.

Las importadas traen además el objeto `import`:

```json
{
  "id": "inv4m8q2x6v0t3p9d1n7cbk5",
  "object": "invoice",
  "livemode": false,
  "status": "stamped",
  "kind": "nominal",
  "uuid": "5B1E9A43-7C2D-4E8F-9A1B-3C4D5E6F7A8B",
  "serie": "F",
  "folio": null,
  "folioText": "1024",
  "logoUrl": null,
  "fecha": "2026-11-03T10:15:00",
  "metodoPago": "PPD",
  "formaPago": "99",
  "usoCfdi": "G03",
  "currency": "MXN",
  "exchangeRate": 1,
  "subtotal": 10000,
  "discount": 0,
  "total": 11600,
  "saldoPendiente": 11600,
  "cfdiRelacionados": null,
  "receiver": {
    "rfc": "XIA190128J61",
    "nombre": "XENON INDUSTRIAL ARTICLES",
    "domicilioFiscal": "01160",
    "regimenFiscal": "601"
  },
  "merchantId": "mkq2f8v3x1t7p9d4n6c0b5rz",
  "origin": "imported",
  "import": {
    "status": "active",
    "paymentId": null,
    "externalNumParcialidad": 0,
    "importedAt": "2026-11-25T18:20:41.302Z"
  },
  "files": {
    "status": "ready",
    "xmlUrl": "https://files.cfdi.express/...xml?X-Amz-Expires=900&..."
  },
  "cancellation": null,
  "stampedAt": "2026-11-03T16:15:12.000Z",
  "createdAt": "2026-11-25T18:20:41.302Z"
}
```

| Campo | Descripción |
|---|---|
| `origin` | `stamped` o `imported`. Viene en todas las facturas. |
| `import.status` | `active`: importación normal. `pending`: llegó en `documents[].xml` de un REP que todavía se está timbrando. |
| `import.paymentId` | El REP (`pay…`) que la importó con `documents[].xml`. `null` si se importó con este endpoint. |
| `import.externalNumParcialidad` | `lastNumParcialidad` declarado en `previousPayments`; `0` si no hubo REP previos o es PUE. |
| `import.importedAt` | Cuándo quedó activa la importación. |

Los datos de la factura salen del XML: `serie` es la `Serie` del XML (vacía si no trae), `folio` es `null` y el folio del XML va en `folioText`. `formaPago`, `usoCfdi`, `receiver`, `currency`, `exchangeRate`, totales y `cfdiRelacionados` son los del CFDI. `stampedAt` es la fecha de timbrado del otro sistema.

**Archivos.** Solo hay XML: `files.xmlUrl` apunta al XML original. No se genera PDF ni ZIP. Mientras se sube el XML, `files.status` es `pending`.

`GET /v1/invoices` acepta el filtro `origin=stamped` o `origin=imported`.

## Lo que no se puede hacer con una factura importada

- **Cancelarla.** `POST /v1/invoices/{id}/cancel` responde **400** `validation_error` con `subcode` `imported_not_cancellable`: `This invoice was stamped by another system and imported — cancel it where it was issued (SAT portal or that PAC); CFDI Express doesn't cancel imported CFDIs`.
- **Timbrarla otra vez o editarla.**
- **Recibir webhooks.** La importación no dispara `invoice.stamped`.

Sí puede recibir REP y notas de crédito. Antes de cada REP sobre una factura importada, CFDI Express vuelve a preguntar al SAT si sigue Vigente, por si la cancelaron en el otro sistema.

## Errores

| Status | code | Cuándo aparece |
|---|---|---|
| 400 | `validation_error` | `invoices` vacío o con más de 20 elementos, `xml` vacío, `previousPayments` con otra forma, o cuerpo de más de 10 MB (`Request body exceeds 10 MB`). Los problemas de cada XML no son 400: vienen en `results`. |
| 402 | `insufficient_credits` | El saldo no alcanza para todas las facturas que pasaron las revisiones: `Saldo is insufficient to import {n} CFDI(s) at {precio} centavos each — nothing was imported`. No se importa ni se cobra nada. |
| 402 | `monthly_limit_reached` | Se alcanzó el límite de gasto mensual de la cuenta: `Monthly spend cap reached — nothing was imported`. |
| 409 | `request_in_flight` | Otra petición con el mismo `Idempotency-Key` sigue en proceso. |
| 422 | `idempotency_key_reuse` | El `Idempotency-Key` ya se usó con otro cuerpo. |

## Servidor MCP

La herramienta `import_invoices` del [servidor MCP](https://cfdi.express/docs/api/mcp) llama a este endpoint:

- `invoices`: de 1 a **5** elementos, cada uno con `xml` **como texto** (no base64; la herramienta lo codifica) y `previousPayments`.
- `idempotency_key`: opcional. Si no la mandas, se genera una al azar.

Responde lo mismo que la API, con el mismo precio. `list_invoices` acepta `origin`.

## Relacionados

- [Importar CFDIs de otros sistemas](https://cfdi.express/docs/importar-cfdis-de-otros-sistemas) — guía completa: importación, REP con `documents[].xml` y notas de crédito por UUID
- [POST /v1/payments](https://cfdi.express/docs/api/timbrar-complemento-de-pago-varias-facturas) — REP de una o varias facturas
- [POST /v1/credit_notes](https://cfdi.express/docs/api/crear-nota-de-credito) — Nota de crédito
- [GET /v1/invoices/{id}](https://cfdi.express/docs/api/obtener-factura) — Obtener factura
- [GET /v1/invoices](https://cfdi.express/docs/api/listar-facturas) — Listar facturas
- [POST /v1/merchants](https://cfdi.express/docs/api/crear-emisor) — Crear emisor

---

Referencia completa: https://cfdi.express/docs/api · OpenAPI: https://api.cfdi.express/openapi.json · Llaves: https://dash.cfdi.express
