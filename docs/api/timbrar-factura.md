# Timbrar una factura (CFDI 4.0 de ingreso)

> `POST https://api.cfdi.express/v1/invoices` — Timbra un CFDI 4.0 de ingreso: nominativo PUE o PPD, o global al público en general. El motor de impuestos calcula IVA e IEPS por ti.

El campo `fecha` está disponible a partir de la versión **v1.36.0** de la API.

URL de esta página: https://cfdi.express/docs/api/timbrar-factura
Autenticación: requiere API key (`Authorization: Bearer sk_test_…` o `sk_live_…`)
Idempotencia: requiere el header `Idempotency-Key`.

El timbrado es síncrono pero mediado por cola: la petición espera hasta 20 segundos por el UUID del SAT y responde **201** con la factura timbrada. Bajo carga extrema puede degradar a **202**; en ese caso el recurso queda en `status: "stamping"` y consultas [GET /v1/invoices/{id}](https://cfdi.express/docs/api/obtener-factura) hasta que pase a `stamped`.

El header `Idempotency-Key` es **obligatorio**. Usa un valor estable por operación lógica (el id de tu pedido, por ejemplo): si reintentas, recibes la factura original sin gastar otro timbre.

Reglas del SAT que validamos por ti: PUE exige una forma de pago real (no `99`); PPD exige forma de pago `99` y deja la factura con `saldoPendiente` para sus complementos de pago.

Desde la v1.28.0 cada concepto acepta `pedimentos` cuando vendes de primera mano mercancía importada: un arreglo opcional de 1 a 100 números de pedimento. La guía está en [cómo facturar con pedimento](https://cfdi.express/blog/factura-con-pedimento-cfdi-express-api).

Desde la v1.36.0 puedes mandar `fecha` para que el CFDI salga con una fecha anterior. Si no la mandas, se usa la hora de timbrado. El mismo campo está en [POST /v1/credit_notes](https://cfdi.express/docs/api/crear-nota-de-credito) y, por MCP, en `create_invoice` y `create_credit_note`.

### Headers

| Campo | Tipo | Requerido | Descripción |
|---|---|---|---|
| `Idempotency-Key` | string | sí | Llave única por operación lógica, 1 a 255 caracteres. Las respuestas repetidas traen Idempotency-Replayed: true. |

### Cuerpo (JSON)

| Campo | Tipo | Requerido | Descripción |
|---|---|---|---|
| `merchantId` | string | sí | Id del emisor que factura. |
| `fecha` | string | no | Fecha de emisión del CFDI (Fecha del comprobante), en hora local de Ciudad de México. Formato `YYYY-MM-DDTHH:mm:ss`, sin zona: por ejemplo `2026-10-13T09:30:00`. CDMX es UTC-6 todo el año desde 2022; no mandes `Z` ni `-06:00`. Si la omites, se usa la hora de timbrado. Puede ir hasta 70 horas atrás (el instante de hace exactamente 70 horas sí pasa). El SAT acepta 72 horas antes de la certificación; el margen de 2 horas es porque el timbrado es asíncrono y el comprobante no debe vencerse en la cola. No puede ser futura: la hora actual sí. El CSD del emisor tiene que haber estado vigente en ese instante. Desde la v1.36.0. |
| `metodoPago` | string | no | PUE o PPD. Default PUE. |
| `formaPago` | string | sí | Clave de c_FormaPago (03 transferencia, 04 tarjeta de crédito…). Con PPD debe ser 99. |
| `usoCfdi` | string | sí | Clave de c_UsoCFDI (G03 gastos en general, S01 sin efectos fiscales…). |
| `folio` | string | no | Folio visible que se imprime en el CFDI, por ejemplo el nombre del pedido (#1001). El folio numérico interno se sigue asignando. |
| `receiver.rfc` | string | sí | RFC del receptor. Para facturas globales, XAXX010101000. |
| `receiver.name` | string | sí | Razón social exacta de la Constancia de Situación Fiscal, en mayúsculas. |
| `receiver.zip` | string | sí | Código postal del domicilio fiscal del receptor. |
| `receiver.regimenFiscal` | string | sí | Régimen fiscal del receptor (clave de 3 dígitos). |
| `items[]` | array | sí | De 1 a 500 conceptos. |
| `items[].productCode` | string | sí | Clave del producto/servicio del SAT, 8 dígitos. |
| `items[].unitCode` | string | sí | Clave de unidad del SAT (E48, H87, KGM…). |
| `items[].description` | string | sí | Descripción del concepto, hasta 1000 caracteres. |
| `items[].quantity` | number | sí | Cantidad, mayor a cero. |
| `items[].unitPrice` | number | sí | Precio unitario, mayor a cero. |
| `items[].unit` | string | no | Unidad en texto libre (Pieza, Servicio…). |
| `items[].noIdentificacion` | string | no | SKU, número de serie o número de parte del concepto (NoIdentificacion del Anexo 20). De 1 a 100 caracteres, sin el carácter \|. Se imprime tal cual debajo de la descripción. |
| `items[].sku` | string | no | Obsoleto: alias de items[].noIdentificacion. Si envías ambos, gana noIdentificacion. |
| `items[].pedimentos` | string[] | no | Números de pedimento de la mercancía importada que vendes de primera mano (InformacionAduanera del Anexo 20). Opcional. De 1 a 100 cadenas. Cada una son 15 dígitos — año (2), aduana (2), patente (4) y consecutivo (7) — con o sin espacios; ejemplo: "25 47 3807 5001234". Se emite en el formato de 21 caracteres del SAT, con dos espacios entre grupos. Un nodo por pedimento; los duplicados se descartan. También se imprime en el PDF. |
| `items[].taxable` | boolean | no | Marca el concepto como exento cuando es false. |
| `items[].ivaRate` | number | no | 0, 0.08 o 0.16. Default 0.16. |
| `items[].iepsRate` | number | no | Tasa de IEPS entre 0 y 3 cuando aplica. |
| `shipping.amount` | number | no | Costo de envío como concepto adicional. |
| `shipping.ivaRate` | number | no | 0, 0.08 o 0.16 para el envío. |
| `shipping.productCode` | string | no | Clave SAT del envío. Default la de servicios de flete. |
| `shipping.unitCode` | string | no | Clave de unidad del envío. |
| `pricesIncludeTax` | boolean | no | true (default) si tus precios ya traen impuestos incluidos, como en Shopify. |
| `cartDiscount` | number | no | Descuento global a prorratear entre los conceptos. |
| `informacionGlobal.periodicidad` | string | no | Sólo facturas globales: 01 diario, 02 semanal, 03 quincenal, 04 mensual, 05 bimestral. |
| `informacionGlobal.meses` | string | no | Sólo facturas globales: 01–12, o 13–18 para bimestres. |
| `informacionGlobal.anio` | integer | no | Sólo facturas globales: año del periodo. |

## Ejemplos

### cURL — factura nominativa PUE

```bash
curl -X POST https://api.cfdi.express/v1/invoices \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: order_1001" \
  -H "Content-Type: application/json" \
  -d '{
    "merchantId": "mkq2f8v3x1t7p9d4n6c0b5rz",
    "metodoPago": "PUE",
    "formaPago": "03",
    "usoCfdi": "G03",
    "folio": "#1001",
    "receiver": {
      "rfc": "XIA190128J61",
      "name": "XENON INDUSTRIAL ARTICLES",
      "zip": "01160",
      "regimenFiscal": "601"
    },
    "items": [
      {
        "productCode": "43201808",
        "unitCode": "H87",
        "description": "Teclado mecánico 75%",
        "noIdentificacion": "KB-75-BLK",
        "quantity": 2,
        "unitPrice": 1740.00,
        "ivaRate": 0.16
      }
    ],
    "shipping": { "amount": 190.00 },
    "pricesIncludeTax": true
  }'
```

### cURL — fecha anterior (v1.36.0)

`2026-10-13T09:30:00` es las 09:30 en Ciudad de México, sin zona. Tiene que caer dentro de las últimas 70 horas y dentro de la vigencia del CSD.

```bash
curl -X POST https://api.cfdi.express/v1/invoices \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: order_1001_fecha" \
  -H "Content-Type: application/json" \
  -d '{
    "merchantId": "mkq2f8v3x1t7p9d4n6c0b5rz",
    "fecha": "2026-10-13T09:30:00",
    "metodoPago": "PUE",
    "formaPago": "03",
    "usoCfdi": "G03",
    "folio": "#1001",
    "receiver": {
      "rfc": "XIA190128J61",
      "name": "XENON INDUSTRIAL ARTICLES",
      "zip": "01160",
      "regimenFiscal": "601"
    },
    "items": [
      {
        "productCode": "43201808",
        "unitCode": "H87",
        "description": "Teclado mecánico 75%",
        "quantity": 2,
        "unitPrice": 1740.00,
        "ivaRate": 0.16
      }
    ],
    "pricesIncludeTax": true
  }'
```

### Node.js

```javascript
const res = await fetch("https://api.cfdi.express/v1/invoices", {
  method: "POST",
  headers: {
    Authorization: `Bearer ${process.env.CFDI_API_KEY}`,
    "Idempotency-Key": `order_${order.id}`,
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    merchantId,
    metodoPago: "PUE",
    formaPago: "03",
    usoCfdi: "G03",
    folio: order.name,
    receiver: {
      rfc: "XIA190128J61",
      name: "XENON INDUSTRIAL ARTICLES",
      zip: "01160",
      regimenFiscal: "601",
    },
    items: order.lineItems.map((li) => ({
      productCode: li.satCode ?? "01010101",
      unitCode: "H87",
      description: li.title,
      noIdentificacion: li.sku,
      quantity: li.quantity,
      unitPrice: li.price,
    })),
    pricesIncludeTax: true,
  }),
});

const invoice = await res.json();

if (invoice.status === "stamping") {
  // 202 bajo carga extrema: consulta hasta que quede "stamped"
}
```

### cURL — factura global

```bash
curl -X POST https://api.cfdi.express/v1/invoices \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: global_2026_07" \
  -H "Content-Type: application/json" \
  -d '{
    "merchantId": "mkq2f8v3x1t7p9d4n6c0b5rz",
    "metodoPago": "PUE",
    "formaPago": "01",
    "usoCfdi": "S01",
    "receiver": {
      "rfc": "XAXX010101000",
      "name": "PUBLICO EN GENERAL",
      "zip": "42501",
      "regimenFiscal": "616"
    },
    "items": [
      {
        "productCode": "01010101",
        "unitCode": "ACT",
        "description": "Venta al público en general",
        "quantity": 1,
        "unitPrice": 128450.00
      }
    ],
    "informacionGlobal": {
      "periodicidad": "04",
      "meses": "07",
      "anio": 2026
    }
  }'
```

## Respuesta

El recurso trae `fecha` (string o `null`), en el mismo formato `YYYY-MM-DDTHH:mm:ss` de hora local de Ciudad de México:

- Ya con el CFDI armado: la Fecha que quedó en el comprobante.
- Antes de armarlo, si mandaste `fecha`: la que pediste.
- Antes de armarlo y sin `fecha` en el request: `null`.

[GET /v1/invoices/{id}](https://cfdi.express/docs/api/obtener-factura) devuelve el mismo campo. Las notas de crédito también: son este recurso con `kind: "credit_note"`.

### 201 Created

Este ejemplo no manda `fecha`. `stampedAt` `2026-08-18T17:42:10.512Z` es las 11:42:10 en Ciudad de México; el worker deja la Fecha 5 minutos antes por el desfase de reloj del PAC.

```json
{
  "id": "inv7k3q9x2m4v1t8p6d0n5cb",
  "object": "invoice",
  "livemode": false,
  "status": "stamped",
  "kind": "nominal",
  "uuid": "9f1b2c3d-4e5f-4a6b-8c9d-0e1f2a3b4c5d",
  "serie": "A",
  "folio": 128,
  "folioText": "#1001",
  "fecha": "2026-08-18T11:37:10",
  "metodoPago": "PUE",
  "formaPago": "03",
  "usoCfdi": "G03",
  "currency": "MXN",
  "subtotal": 3163.79,
  "discount": 0,
  "total": 3670.00,
  "saldoPendiente": null,
  "receiver": {
    "rfc": "XIA190128J61",
    "name": "XENON INDUSTRIAL ARTICLES",
    "zip": "01160",
    "regimenFiscal": "601"
  },
  "merchantId": "mkq2f8v3x1t7p9d4n6c0b5rz",
  "files": { "status": "ready" },
  "cancellation": null,
  "stampedAt": "2026-08-18T17:42:10.512Z",
  "createdAt": "2026-08-18T17:42:08.907Z"
}
```

Si hubieras mandado `fecha` con más de 5 minutos de antelación, aquí verías esa misma hora local. Si el CFDI ya está armado y el worker la ajustó, ves la del comprobante.

### 202 Accepted

Sin `fecha` en el request y sin CFDI armado, el campo es `null`. Si la mandaste, aquí ves la hora local pedida; cuando el comprobante quede armado, la del CFDI.

```json
{
  "id": "inv7k3q9x2m4v1t8p6d0n5cb",
  "object": "invoice",
  "livemode": false,
  "status": "stamping",
  "kind": "nominal",
  "uuid": null,
  "fecha": null,
  "files": { "status": "pending" },
  "stampedAt": null,
  "createdAt": "2026-08-18T17:42:08.907Z"
}
```

### 400 — `fecha` fuera de rango

`problem+json` con `code` `validation_error` y `subcode` `fecha_out_of_range`. `errors[].path` es `fecha`. Ocurre cuando la fecha es de calendario imposible (`2026-02-30T10:00:00`, `2026-10-13T25:00:00`), futura, de más de 70 horas, o el CSD del emisor no estaba vigente en ese instante. El `detail` y el `message` son el mismo texto.

```json
{
  "type": "https://api.cfdi.express/docs/errors/validation_error",
  "title": "Validation error",
  "status": 400,
  "code": "validation_error",
  "detail": "fecha cannot be in the future",
  "subcode": "fecha_out_of_range",
  "errors": [
    { "path": "fecha", "message": "fecha cannot be in the future" }
  ]
}
```

Otros `detail` de ese `subcode`:

| Situación | `detail` |
|---|---|
| Fecha que no existe | `fecha 2026-02-30T10:00:00 is not a valid date` |
| Futura | `fecha cannot be in the future` |
| Más de 70 horas atrás | `fecha can be at most 70 hours in the past (the SAT allows 72 hours before stamping)` |
| CSD no vigente en esa fecha | `The merchant's CSD was not valid on fecha` |

### 400 — formato de `fecha`

Un valor que no cumple `YYYY-MM-DDTHH:mm:ss` falla en la validación del cuerpo, antes de revisar el rango. También es `validation_error` con `errors[].path` `fecha`, y no trae `subcode` `fecha_out_of_range`. Lo rechaza, por ejemplo, `2026-10-13`, `2026-10-13T09:30:00Z`, `2026-10-13T09:30:00-06:00` o `2026-10-13 09:30:00`.

```json
{
  "type": "https://api.cfdi.express/docs/errors/validation_error",
  "title": "Validation error",
  "status": 400,
  "code": "validation_error",
  "detail": "Request validation failed",
  "errors": [
    {
      "path": "fecha",
      "message": "fecha must be local Mexico City time: YYYY-MM-DDTHH:mm:ss"
    }
  ]
}
```

### 422 — rechazo del SAT

```json
{
  "type": "https://api.cfdi.express/docs/errors/sat_rejected",
  "title": "Rejected by SAT validation",
  "status": 422,
  "code": "sat_rejected",
  "detail": "El nombre del receptor no coincide con el registrado ante el SAT",
  "satCode": "CFDI40147"
}
```

## Errores

| Status | code | Cuándo aparece |
|---|---|---|
| 400 | `validation_error` | Datos inválidos. El arreglo `errors` trae el `path` de cada campo. Con `subcode` `fecha_out_of_range` y `errors[{path:'fecha'}]`, la fecha es inválida, futura, de más de 70 horas o el CSD no estaba vigente. Un formato distinto de `YYYY-MM-DDTHH:mm:ss` es el mismo `code`, sin ese `subcode`. |
| 401 | `unauthorized` | Falta el header Authorization o la llave es inválida. |
| 402 | `insufficient_credits` | No hay saldo suficiente para el timbre. Recarga y reintenta. |
| 409 | `request_in_flight` | Otra petición con el mismo Idempotency-Key está en proceso. |
| 422 | `idempotency_key_reuse` | Reusaste la llave de idempotencia con un cuerpo distinto. |
| 422 | `sat_rejected` | El SAT rechazó el comprobante. El detalle trae el código del SAT. |
| 503 | `pac_unavailable` | El PAC no está disponible. Reintenta con la misma llave de idempotencia. |

## Notas

- El XML, el PDF y el ZIP se generan justo después del timbrado. Cuando `files.status` es `ready`, [GET /v1/invoices/{id}](https://cfdi.express/docs/api/obtener-factura) incluye `files.xmlUrl`, `files.pdfUrl` y `files.zipUrl` firmadas por 15 minutos.
- Ante una falla ambigua con el PAC nunca reintentamos a ciegas: el documento se marca con `needsReconciliation: true` y tu saldo queda reservado hasta resolverlo.
- `items[].pedimentos` queda en el XML como `Concepto/InformacionAduanera/@NumeroPedimento` y también se imprime en el PDF. El servidor MCP lo acepta igual: `create_invoice` y `create_credit_note` llevan el mismo arreglo, así que un agente de IA puede timbrar con el pedimento de cada concepto.
- Si omites `fecha`, el worker usa la hora de timbrado y le resta 5 minutos por desfase de reloj del PAC.
- Si la `fecha` que mandaste queda a menos de 5 minutos del momento en que el worker timbra, la ajusta a ese mismo margen para que no se adelante al reloj del PAC. En el recurso, ya con el CFDI armado, `fecha` es la del comprobante.

## Relacionados

- [GET /v1/invoices/{id}](https://cfdi.express/docs/api/obtener-factura) — Obtener factura
- [GET /v1/invoices](https://cfdi.express/docs/api/listar-facturas) — Listar facturas
- [POST /v1/invoices/{id}/cancel](https://cfdi.express/docs/api/cancelar-factura) — Cancelar factura
- [POST /v1/credit_notes](https://cfdi.express/docs/api/crear-nota-de-credito) — Timbrar nota de crédito
- [Servidor MCP](https://cfdi.express/docs/api/mcp) — `create_invoice` acepta `fecha`

---

Referencia completa: https://cfdi.express/docs/api · OpenAPI: https://api.cfdi.express/openapi.json · Llaves: https://dash.cfdi.express
