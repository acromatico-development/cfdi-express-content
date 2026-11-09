---
title: "Facturar anticipos con CFDI Express: guía paso a paso"
description: "Factura un anticipo en CFDI 4.0 con la API o el MCP de CFDI Express: factura del anticipo, factura por el total con relación 07 y nota con forma 30."
keywords: "facturar anticipos, anticipo cfdi 4.0, tipo relacion 07, forma de pago 30, aplicación de anticipo, 84111506, ACT, nota de crédito anticipo, CFDI Express API, MCP"
---
# Cómo facturar anticipos con CFDI Express (paso a paso)

URL de esta página: https://cfdi.express/docs/facturar-anticipos

Disponible a partir de la versión **v1.51.0** de la API.

La API timbra anticipos con el **procedimiento A** del Apéndice 6 de la Guía de llenado del Anexo 20 («Facturación aplicando anticipo con CFDI de egreso»). Son tres CFDI, en este orden:

1. **Factura del anticipo**: ingreso con un solo concepto `84111506` / `ACT`, cantidad 1, `PUE` con la forma de pago real y sin CFDI relacionados.
2. **Factura por el valor total** de la operación: ingreso con los conceptos reales y `cfdiRelacionados` con `tipoRelacion` `"07"` al UUID del anticipo.
3. **Nota de crédito** (egreso) por el anticipo aplicado: `POST /v1/credit_notes` con `tipoRelacion` `"07"`, `formaPago` `"30"` y `relatedInvoiceId` de la **factura por el total**, no del anticipo.

La API rechaza las combinaciones que no siguen este orden. Cada error dice qué mandar en su lugar; la lista está en la sección «Errores al facturar anticipos».

Cada paso timbra un CFDI y consume un timbre. Las tres llamadas piden `Authorization: Bearer sk_test_…` o `sk_live_…` y un `Idempotency-Key` (1 a 255 caracteres). En un workspace, el header `x-account-id` indica la cuenta.

**Antes de empezar: ¿es un anticipo?** Según la Guía de llenado, hay anticipo cuando se recibe un pago y todavía no se conoce o no se ha determinado el bien o servicio que se va a adquirir, su precio, o ninguno de los dos. Si ya hay acuerdo sobre el bien o servicio **y** su precio (aunque no sea por escrito) y el cliente paga una parte, es una venta en parcialidades, no un anticipo. Un depósito en garantía tampoco es anticipo.

## Ejemplo de esta guía

- El cliente entrega **$11,600.00** (IVA incluido) de anticipo por transferencia.
- Después se concreta la venta por **$58,000.00** (IVA incluido).
- El remanente es **$46,400.00**.

Los ejemplos usan `pricesIncludeTax: true` (el valor por omisión): `unitPrice` es el monto con IVA y la API calcula el `ValorUnitario` antes de impuestos que pide el SAT. Con $11,600.00 e IVA 16%, el concepto queda en 10,000.00 más 1,600.00 de IVA.

`merchantId`, receptor y conceptos son de ejemplo: sirven para ver la forma del JSON.

## Paso 1. Factura del anticipo

> `POST https://api.cfdi.express/v1/invoices`

Se emite al recibir el dinero. El cuerpo es el de [Timbrar una factura](/docs/api/timbrar-factura), con estas reglas:

| Campo | Valor |
|---|---|
| `items` | Exactamente **un** concepto: `productCode` `"84111506"`, `unitCode` `"ACT"`, `quantity` `1`, `description` `"Anticipo del bien o servicio"`, `unitPrice` = el anticipo recibido. |
| `metodoPago` | `"PUE"`. |
| `formaPago` | La forma real en que llegó el dinero (`"03"` transferencia, `"01"` efectivo, `"04"` tarjeta de crédito…). Nunca `"30"`. |
| `cfdiRelacionados` | No se manda. |
| `shipping`, `cartDiscount` | No se mandan. |

```bash
curl -X POST https://api.cfdi.express/v1/invoices \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: pedido_2001_anticipo" \
  -H "Content-Type: application/json" \
  -d '{
    "merchantId": "mkq2f8v3x1t7p9d4n6c0b5rz",
    "metodoPago": "PUE",
    "formaPago": "03",
    "usoCfdi": "G03",
    "receiver": {
      "rfc": "XIA190128J61",
      "name": "XENON INDUSTRIAL ARTICLES",
      "zip": "01160",
      "regimenFiscal": "601"
    },
    "items": [
      {
        "productCode": "84111506",
        "unitCode": "ACT",
        "description": "Anticipo del bien o servicio",
        "quantity": 1,
        "unitPrice": 11600.00,
        "ivaRate": 0.16
      }
    ],
    "pricesIncludeTax": true
  }'
```

La petición espera hasta 20 segundos. **201** trae la factura timbrada: guarda su `uuid` (folio fiscal), que es el que relaciona el paso 2. Bajo carga extrema responde **202** con `status` `stamping`; consulta [`GET /v1/invoices/{id}`](/docs/api/obtener-factura) hasta que pase a `stamped`.

## Paso 2. Factura por el valor total

> `POST https://api.cfdi.express/v1/invoices`

Se emite cuando la operación se concreta. Lleva los conceptos reales con sus claves del SAT y el **precio completo**: no restes el anticipo ni uses `84111506` / `ACT`.

| Campo | Valor |
|---|---|
| `items` | Los bienes o servicios reales, a precio completo. |
| `receiver` | El mismo receptor (RFC) del anticipo. |
| `cfdiRelacionados` | `{ "tipoRelacion": "07", "uuids": ["<UUID del anticipo>"] }`. Puede llevar varios anticipos. |
| `metodoPago` / `formaPago` | Describen el remanente: `"PUE"` con la forma real si se paga en ese momento, o `"PPD"` con `"99"` si se paga después. |

Si el remanente se paga después (PPD), cada pago se registra con un complemento de pago (REP) en [`POST /v1/payments`](/docs/api/timbrar-complemento-de-pago).

`formaPago` `"30"` en esta factura solo se acepta cuando los anticipos relacionados (timbrados en CFDI Express, en la misma moneda) cubren el total completo.

```bash
curl -X POST https://api.cfdi.express/v1/invoices \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: pedido_2001_total" \
  -H "Content-Type: application/json" \
  -d '{
    "merchantId": "mkq2f8v3x1t7p9d4n6c0b5rz",
    "metodoPago": "PUE",
    "formaPago": "03",
    "usoCfdi": "G03",
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
        "quantity": 20,
        "unitPrice": 2900.00,
        "ivaRate": 0.16
      }
    ],
    "pricesIncludeTax": true,
    "cfdiRelacionados": {
      "tipoRelacion": "07",
      "uuids": ["119038DB-1B9F-4BC5-8A71-712E95AED2C9"]
    }
  }'
```

Si el remanente se paga después, cambia `"metodoPago": "PPD"` y `"formaPago": "99"`.

Guarda el `id` (`inv…`) de la respuesta: es el `relatedInvoiceId` del paso 3. El recurso devuelve `cfdiRelacionados` con `tipoRelacion` `"07"`.

## Paso 3. Nota de crédito por el anticipo aplicado

> `POST https://api.cfdi.express/v1/credit_notes`

Se emite después de la factura por el total. La Guía de llenado indica que ambas lleven, preferentemente, la misma fecha, y que primero vaya el ingreso y después el egreso.

| Campo | Valor |
|---|---|
| `relatedInvoiceId` | El `id` de la **factura por el total** (paso 2). Nunca el de la factura del anticipo. |
| `tipoRelacion` | `"07"`. |
| `formaPago` | `"30"` (Aplicación de anticipos). |
| `items` | Exactamente **un** concepto: `productCode` `"84111506"`, `unitCode` `"ACT"`, `quantity` `1`, `description` `"Aplicación de anticipo"`, `unitPrice` = el anticipo aplicado, con los mismos impuestos que la factura del anticipo. |

El receptor se copia de la factura relacionada. La referencia completa está en [Crear una nota de crédito](/docs/api/crear-nota-de-credito).

```bash
curl -X POST https://api.cfdi.express/v1/credit_notes \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: pedido_2001_aplicacion_anticipo" \
  -H "Content-Type: application/json" \
  -d '{
    "merchantId": "mkq2f8v3x1t7p9d4n6c0b5rz",
    "relatedInvoiceId": "inv7k3q9x2m4v1t8p6d0n5cb",
    "tipoRelacion": "07",
    "formaPago": "30",
    "items": [
      {
        "productCode": "84111506",
        "unitCode": "ACT",
        "description": "Aplicación de anticipo",
        "quantity": 1,
        "unitPrice": 11600.00,
        "ivaRate": 0.16
      }
    ],
    "pricesIncludeTax": true
  }'
```

Cuando los anticipos se timbraron en CFDI Express, el total de la nota no puede pasar del anticipo que falta por aplicar en esa factura (la suma de los anticipos relacionados menos las notas `07` vigentes que ya los aplicaron).

## Con el servidor MCP

El [servidor MCP](/docs/api/mcp) sigue el mismo flujo. Al conectarse, manda a tu agente las instrucciones de los tres pasos:

- Pasos 1 y 2: `create_invoice`.
- Paso 3: `create_credit_note` con `tipoRelacion` `"07"`, `formaPago` `"30"` y `relatedInvoiceId` de la factura por el total.

Si no le dices al agente si el remanente se paga en ese momento o después, las instrucciones le piden que te pregunte. Los errores son los mismos que en la API REST, así que el agente corrige y reintenta.

## Errores al facturar anticipos

Las reglas de anticipo responden **400** con `code` `validation_error` y `subcode` `anticipo_rules`, antes de timbrar. Las excepciones se indican en cada tabla. `errors[].path` señala el campo, y `detail` explica qué mandar en su lugar y repite el flujo de tres pasos.

```json
{
  "type": "https://api.cfdi.express/docs/errors/validation_error",
  "title": "Validation error",
  "status": 400,
  "code": "validation_error",
  "subcode": "anticipo_rules",
  "detail": "Invoice inv… is the anticipo itself. The tipoRelacion 07 credit note relates the full-value invoice (the one stamped with cfdiRelacionados 07 → this anticipo) — stamp that invoice first and pass its id. Anticipo flow (SAT Apéndice 6-A): …",
  "errors": [{ "path": "relatedInvoiceId", "message": "Invoice inv… is the anticipo itself. …" }]
}
```

### Factura del anticipo (paso 1)

| Error | `path` |
|---|---|
| Más de un concepto, o cantidad distinta de 1, con `84111506` / `ACT`. | `items` |
| `metodoPago` `"PPD"`. El anticipo siempre es `PUE`. | `metodoPago` |
| `shipping` o `cartDiscount` en la factura del anticipo. | `items` |
| `formaPago` `"30"`. La forma 30 va en la nota de crédito. | `formaPago` |

### Factura por el total (paso 2)

| Error | `path` |
|---|---|
| Un concepto `84111506` / `ACT` junto con la relación `07`. La factura por el total lleva los conceptos reales. | `items` |
| Relación `07` en una factura global (público en general). Factura al cliente de forma nominativa. | `cfdiRelacionados.tipoRelacion` |
| El UUID relacionado no es una factura de anticipo (un solo concepto `84111506` / `ACT`). Para sustituir una factura se usa `04`. | `cfdiRelacionados.uuids` |
| El UUID es de una nota de crédito, de otro emisor, o de un anticipo cancelado o sin timbrar. | `cfdiRelacionados.uuids` |
| El anticipo ya se aplicó en otra factura vigente. Un anticipo se aplica una sola vez. | `cfdiRelacionados.uuids` |
| El anticipo se facturó a otro RFC. | `receiver.rfc` |
| `formaPago` `"30"` sin que los anticipos cubran el total. | `formaPago` |
| `metodoPago` `"PPD"` con una forma distinta de `"99"`. Es un `validation_error` general de cualquier factura, sin `subcode` `anticipo_rules`. | — |

Las reglas de los UUID solo se pueden revisar cuando el anticipo se timbró en CFDI Express. Un UUID timbrado en otro sistema pasa sin esas validaciones.

### Nota de crédito (paso 3)

| Error | `path` |
|---|---|
| `tipoRelacion` `"07"` sin `formaPago` `"30"`, o `formaPago` `"30"` sin `tipoRelacion` `"07"`. | `formaPago` |
| Más de un concepto, o un concepto distinto de `84111506` / `ACT` con cantidad 1. | `items` |
| `relatedInvoiceId` es la factura del anticipo. | `relatedInvoiceId` |
| `relatedInvoiceId` es una factura sin `cfdiRelacionados` `07`. | `relatedInvoiceId` |
| Una nota `01` o `03` con el concepto de anticipo sobre una factura que aplica anticipos. Usa `07` y forma `30`. | `tipoRelacion` |

Si el total de la nota pasa del anticipo pendiente de aplicar, el `subcode` es `over_apply_anticipo` y la respuesta trae `available` con el monto que aún se puede aplicar.

Para **devolverle** el anticipo al cliente (en lugar de aplicarlo), usa una nota de crédito `01` con la forma de pago real, relacionada con la factura del anticipo.

## Limitaciones

- **Solo procedimiento A.** El procedimiento B del Apéndice 6 («Facturación aplicando anticipo con remanente de la contraprestación», con `Descuento` por concepto) no está soportado.
- **La nota `07` no reduce el saldo pendiente** (`saldoPendiente`) de una factura por el total emitida como PPD. Lo mismo pasa hoy con las notas `01`.
- **No se combinan `04` y `07`.** Una factura lleva un solo grupo de `cfdiRelacionados`. Para corregir una factura por el total que ya aplicó un anticipo, cancélala con motivo `02` ([Cancelar una factura](/docs/api/cancelar-factura)) y vuelve a timbrarla con la relación `07`.

## Preguntas frecuentes

**¿Qué clave lleva la factura del anticipo?**
Un solo concepto con `productCode` `84111506`, `unitCode` `ACT`, cantidad 1 y descripción «Anticipo del bien o servicio». Es `PUE` con la forma de pago real y no lleva CFDI relacionados.

**¿A qué factura se relaciona la nota de crédito con forma 30?**
A la factura por el valor total de la operación, la que lleva `cfdiRelacionados` `07`. Si mandas el `id` de la factura del anticipo, la API responde 400.

**¿La factura por el total puede ser PPD?**
Sí. Si el remanente se paga después, va con `PPD` y forma `99`, y cada pago se registra con un REP. Si se paga en ese momento, va con `PUE` y la forma real.

**¿Puedo aplicar el mismo anticipo en dos facturas?**
No. Si el anticipo ya está relacionado con `07` en otra factura vigente, la API responde 400. Si estás corrigiendo esa factura, cancélala primero con motivo `02`.

**¿Funciona con un agente de IA?**
Sí. El servidor MCP usa `create_invoice` para los pasos 1 y 2 y `create_credit_note` para el paso 3, con las mismas validaciones.

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {"@type": "Question", "name": "¿Qué clave lleva la factura del anticipo?", "acceptedAnswer": {"@type": "Answer", "text": "Un solo concepto con productCode 84111506, unitCode ACT, cantidad 1 y descripción «Anticipo del bien o servicio». Es PUE con la forma de pago real y no lleva CFDI relacionados."}},
    {"@type": "Question", "name": "¿A qué factura se relaciona la nota de crédito con forma 30?", "acceptedAnswer": {"@type": "Answer", "text": "A la factura por el valor total de la operación, la que lleva cfdiRelacionados 07. Si mandas el id de la factura del anticipo, la API responde 400."}},
    {"@type": "Question", "name": "¿La factura por el total puede ser PPD?", "acceptedAnswer": {"@type": "Answer", "text": "Sí. Si el remanente se paga después, va con PPD y forma 99, y cada pago se registra con un REP. Si se paga en ese momento, va con PUE y la forma real."}},
    {"@type": "Question", "name": "¿Puedo aplicar el mismo anticipo en dos facturas?", "acceptedAnswer": {"@type": "Answer", "text": "No. Si el anticipo ya está relacionado con 07 en otra factura vigente, la API responde 400. Si estás corrigiendo esa factura, cancélala primero con motivo 02."}},
    {"@type": "Question", "name": "¿Funciona con un agente de IA?", "acceptedAnswer": {"@type": "Answer", "text": "Sí. El servidor MCP usa create_invoice para los pasos 1 y 2 y create_credit_note para el paso 3, con las mismas validaciones."}}
  ]
}
</script>

## Relacionados

- [Timbrar una factura](/docs/api/timbrar-factura): `POST /v1/invoices`
- [Crear una nota de crédito](/docs/api/crear-nota-de-credito): `POST /v1/credit_notes`
- [Timbrar un complemento de pago](/docs/api/timbrar-complemento-de-pago): REP del remanente PPD
- [Cancelar una factura](/docs/api/cancelar-factura): motivos 01–04
- [Servidor MCP](/docs/api/mcp)

---

Referencia completa: https://cfdi.express/docs/api · OpenAPI: https://api.cfdi.express/openapi.json · Llaves: https://dash.cfdi.express
