# Timbrar una nota de crédito (CFDI de egreso)

> `POST https://api.cfdi.express/v1/credit_notes` — Timbra un CFDI 4.0 de egreso para una devolución, un reembolso o un descuento posterior a la factura.

El campo `fecha` está disponible a partir de la versión **v1.36.0** de la API.

URL de esta página: https://cfdi.express/docs/api/crear-nota-de-credito
Autenticación: requiere API key (`Authorization: Bearer sk_test_…` o `sk_live_…`)
Idempotencia: requiere el header `Idempotency-Key`.

Relaciona la nota con la factura original de dos formas: con `relatedInvoiceId` si el CFDI de ingreso se timbró aquí (el receptor se copia solo), o con `relatedUuid` si se timbró en otro sistema (en ese caso el `receiver` es obligatorio).

El resultado es un recurso `invoice` con `kind: "credit_note"`: lo consultas con [GET /v1/invoices/{id}](https://cfdi.express/docs/api/obtener-factura) y lo listas con `GET /v1/invoices?kind=credit_note`.

Igual que el timbrado de facturas, espera hasta 20 segundos por el UUID y requiere `Idempotency-Key`.

Los conceptos aceptan `pedimentos` igual que en la factura de ingreso (información aduanera, v1.28.0). La guía está en [cómo facturar con pedimento](https://cfdi.express/blog/factura-con-pedimento-cfdi-express-api).

Desde la v1.36.0 puedes mandar `fecha` para timbrar la nota con una fecha anterior. Las reglas son las mismas que en [POST /v1/invoices](https://cfdi.express/docs/api/timbrar-factura): hora local de Ciudad de México, hasta 70 horas atrás, no futura, y el CSD del emisor vigente en esa fecha. Si no la mandas, se usa la hora de timbrado. Por MCP, `create_credit_note` acepta el mismo campo.

### Headers

| Campo | Tipo | Requerido | Descripción |
|---|---|---|---|
| `Idempotency-Key` | string | sí | Llave única por devolución. Los replays devuelven la nota original. |

### Cuerpo (JSON)

| Campo | Tipo | Requerido | Descripción |
|---|---|---|---|
| `merchantId` | string | sí | Emisor de la nota. Debe ser el mismo que timbró la factura relacionada. |
| `fecha` | string | no | Fecha de emisión del CFDI, en hora local de Ciudad de México (UTC-6, todo el año desde 2022), sin zona. Formato `YYYY-MM-DDTHH:mm:ss`, por ejemplo `2026-10-13T09:30:00`. No mandes `Z` ni `-06:00`. Si la omites, se usa la hora de timbrado. Hasta 70 horas atrás, inclusive: el SAT acepta 72 horas antes de la certificación y el margen de 2 horas cubre el timbrado asíncrono. No puede ser futura (la hora actual sí). El CSD del emisor tiene que haber estado vigente en ese instante. Desde la v1.36.0. |
| `relatedInvoiceId` | string | no | Id de la factura de ingreso timbrada aquí. Usa este o relatedUuid. |
| `relatedUuid` | string | no | UUID de un CFDI de ingreso timbrado fuera de esta API. Exige receiver explícito. |
| `tipoRelacion` | string | no | 01 nota de crédito de los documentos relacionados (default) o 03 devolución de mercancía. |
| `usoCfdi` | string | no | Default G02 (devoluciones, descuentos o bonificaciones). |
| `formaPago` | string | sí | Cómo se devuelve el dinero (03 transferencia, 01 efectivo…). Usa 15 condonación o 99 cuando no hay movimiento de dinero. |
| `folio` | string | no | Folio visible, por ejemplo tu número de devolución (RET-1001). |
| `receiver` | object | no | Receptor (rfc, name, zip, regimenFiscal). Se hereda de la factura relacionada cuando usas relatedInvoiceId. |
| `items[]` | array | sí | La mercancía devuelta, o un solo concepto por el monto acreditado. Misma forma que en facturas. |
| `items[].pedimentos` | string[] | no | Igual que en POST /v1/invoices. Números de pedimento de la mercancía importada que vendes de primera mano (InformacionAduanera). Opcional. De 1 a 100 cadenas de 15 dígitos — año (2), aduana (2), patente (4) y consecutivo (7) — con o sin espacios; ejemplo: "25 47 3807 5001234". Se emite en 21 caracteres del SAT, con dos espacios entre grupos. Los duplicados se descartan. |
| `pricesIncludeTax` | boolean | no | true por default. |

## Ejemplos

### cURL — devolución de una factura timbrada aquí

```bash
curl -X POST https://api.cfdi.express/v1/credit_notes \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: return_1001" \
  -H "Content-Type: application/json" \
  -d '{
    "merchantId": "mkq2f8v3x1t7p9d4n6c0b5rz",
    "relatedInvoiceId": "inv7k3q9x2m4v1t8p6d0n5cb",
    "tipoRelacion": "03",
    "formaPago": "03",
    "folio": "RET-1001",
    "items": [
      {
        "productCode": "43201808",
        "unitCode": "H87",
        "description": "Teclado mecánico 75% (devolución)",
        "quantity": 1,
        "unitPrice": 1740.00
      }
    ]
  }'
```

### cURL — fecha anterior (v1.36.0)

```bash
curl -X POST https://api.cfdi.express/v1/credit_notes \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: return_1001_fecha" \
  -H "Content-Type: application/json" \
  -d '{
    "merchantId": "mkq2f8v3x1t7p9d4n6c0b5rz",
    "fecha": "2026-10-13T09:30:00",
    "relatedInvoiceId": "inv7k3q9x2m4v1t8p6d0n5cb",
    "tipoRelacion": "03",
    "formaPago": "03",
    "folio": "RET-1001",
    "items": [
      {
        "productCode": "43201808",
        "unitCode": "H87",
        "description": "Teclado mecánico 75% (devolución)",
        "quantity": 1,
        "unitPrice": 1740.00
      }
    ]
  }'
```

### cURL — CFDI externo por UUID

```bash
curl -X POST https://api.cfdi.express/v1/credit_notes \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: return_ext_88" \
  -H "Content-Type: application/json" \
  -d '{
    "merchantId": "mkq2f8v3x1t7p9d4n6c0b5rz",
    "relatedUuid": "9f1b2c3d-4e5f-4a6b-8c9d-0e1f2a3b4c5d",
    "tipoRelacion": "01",
    "formaPago": "99",
    "receiver": {
      "rfc": "XIA190128J61",
      "name": "XENON INDUSTRIAL ARTICLES",
      "zip": "01160",
      "regimenFiscal": "601"
    },
    "items": [
      {
        "productCode": "01010101",
        "unitCode": "ACT",
        "description": "Bonificación comercial",
        "quantity": 1,
        "unitPrice": 500.00
      }
    ]
  }'
```

## Respuesta

La nota es un recurso `invoice`. `fecha` sigue las mismas reglas que en la factura: la Fecha del CFDI ya armado; antes, la que pediste; `null` si no pediste fecha y el comprobante todavía no está armado. Hora local de Ciudad de México, `YYYY-MM-DDTHH:mm:ss`, sin zona.

### 201 Created

Este ejemplo no manda `fecha`. `stampedAt` `2026-08-18T18:15:02.441Z` es las 12:15:02 en Ciudad de México; sin `fecha` en el request, el worker deja la Fecha 5 minutos antes.

```json
{
  "id": "cnt4m9x2q7v1p8d3t6n0c5br",
  "object": "invoice",
  "livemode": false,
  "status": "stamped",
  "kind": "credit_note",
  "uuid": "2b7d5a11-93c4-4f0e-b6a8-77c1e9d43f10",
  "serie": "A",
  "folio": 129,
  "folioText": "RET-1001",
  "fecha": "2026-08-18T12:10:02",
  "metodoPago": "PUE",
  "formaPago": "03",
  "usoCfdi": "G02",
  "currency": "MXN",
  "subtotal": 1500.00,
  "discount": 0,
  "total": 1740.00,
  "saldoPendiente": null,
  "tipoRelacion": "03",
  "relatedUuid": "9f1b2c3d-4e5f-4a6b-8c9d-0e1f2a3b4c5d",
  "relatedInvoiceId": "inv7k3q9x2m4v1t8p6d0n5cb",
  "receiver": {
    "rfc": "XIA190128J61",
    "name": "XENON INDUSTRIAL ARTICLES",
    "zip": "01160",
    "regimenFiscal": "601"
  },
  "merchantId": "mkq2f8v3x1t7p9d4n6c0b5rz",
  "files": { "status": "ready" },
  "cancellation": null,
  "stampedAt": "2026-08-18T18:15:02.441Z",
  "createdAt": "2026-08-18T18:15:00.902Z"
}
```

### 400 — `fecha` fuera de rango

Igual que en la factura: `code` `validation_error`, `subcode` `fecha_out_of_range` y `errors[{path:'fecha'}]`. La fecha es inválida, futura, de más de 70 horas, o el CSD no estaba vigente.

```json
{
  "type": "https://api.cfdi.express/docs/errors/validation_error",
  "title": "Validation error",
  "status": 400,
  "code": "validation_error",
  "detail": "fecha can be at most 70 hours in the past (the SAT allows 72 hours before stamping)",
  "subcode": "fecha_out_of_range",
  "errors": [
    {
      "path": "fecha",
      "message": "fecha can be at most 70 hours in the past (the SAT allows 72 hours before stamping)"
    }
  ]
}
```

| Situación | `detail` |
|---|---|
| Fecha que no existe | `fecha 2026-02-30T10:00:00 is not a valid date` |
| Futura | `fecha cannot be in the future` |
| Más de 70 horas atrás | `fecha can be at most 70 hours in the past (the SAT allows 72 hours before stamping)` |
| CSD no vigente en esa fecha | `The merchant's CSD was not valid on fecha` |

Un formato que no es `YYYY-MM-DDTHH:mm:ss` (con `Z`, con offset, solo la fecha, o con espacio en lugar de `T`) también responde `validation_error` con `errors[].path` `fecha`, sin `subcode` `fecha_out_of_range`. El `message` es `fecha must be local Mexico City time: YYYY-MM-DDTHH:mm:ss`.

## Errores

| Status | code | Cuándo aparece |
|---|---|---|
| 400 | `validation_error` | Faltan relatedInvoiceId y relatedUuid, el receptor no viene con relatedUuid, algún concepto es inválido, o `fecha` no pasa. Con `subcode` `fecha_out_of_range` y `errors[{path:'fecha'}]`, la fecha es inválida, futura, de más de 70 horas o el CSD no estaba vigente. Un formato distinto de `YYYY-MM-DDTHH:mm:ss` es el mismo `code`, sin ese `subcode`. |
| 401 | `unauthorized` | Falta el header Authorization o la llave es inválida. |
| 402 | `insufficient_credits` | No hay saldo suficiente para el timbre. |
| 404 | `not_found` | El emisor o la factura relacionada no existen. |
| 409 | `request_in_flight` | Otra petición con el mismo Idempotency-Key está en proceso. |
| 422 | `sat_rejected` | El SAT rechazó la nota de crédito. |
| 503 | `pac_unavailable` | El PAC no está disponible. |

## Notas

- Una nota de crédito no cancela la factura: la corrige contablemente. Si lo que quieres es anular el comprobante, usa [la cancelación](https://cfdi.express/docs/api/cancelar-factura).
- `items[].pedimentos` queda en el XML como `Concepto/InformacionAduanera/@NumeroPedimento` y también se imprime en el PDF. El servidor MCP lo acepta igual: `create_invoice` y `create_credit_note` llevan el mismo arreglo, así que un agente de IA puede timbrar con el pedimento de cada concepto.
- Si omites `fecha`, el worker usa la hora de timbrado y le resta 5 minutos por desfase de reloj del PAC. Si la fecha pedida queda a menos de 5 minutos de ese momento, la ajusta a ese margen. Ya con el CFDI armado, el recurso expone la Fecha del comprobante.

## Relacionados

- [POST /v1/invoices](https://cfdi.express/docs/api/timbrar-factura) — Timbrar factura
- [GET /v1/invoices/{id}](https://cfdi.express/docs/api/obtener-factura) — Obtener factura
- [POST /v1/invoices/{id}/cancel](https://cfdi.express/docs/api/cancelar-factura) — Cancelar factura
- [Servidor MCP](https://cfdi.express/docs/api/mcp) — `create_credit_note` acepta `fecha`

---

Referencia completa: https://cfdi.express/docs/api · OpenAPI: https://api.cfdi.express/openapi.json · Llaves: https://dash.cfdi.express
