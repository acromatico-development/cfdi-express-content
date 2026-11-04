---
title: "Cancelar CFDI con motivo 01: factura sustituta paso a paso"
description: "Cómo cancelar una factura por sustitución (motivo 01) en CFDI 4.0: emite la sustituta con TipoRelacion 04 y cancela la original por API. Con ejemplos."
keywords: "cancelar cfdi motivo 01, factura sustituta, tipo relacion 04, folioSustitucion, cfdiRelacionados, cancelar factura API, CFDI 4.0"
---
# Cancelar CFDI con motivo 01: factura sustituta paso a paso

URL de esta página: https://cfdi.express/docs/cancelar-factura-sustitucion

Disponible a partir de la versión **v1.50.0** de la API. Desde la **v1.50.1** los UUID se aceptan en minúsculas.

El motivo **01** (comprobante emitido con errores con relación) se usa cuando ya timbraste la factura correcta y vas a cancelar la que tenía el error. El SAT pide dos cosas: que la nueva relacione a la anterior con TipoRelacion **04** (sustitución de los CFDI previos) y que, al cancelar, indiques el UUID de esa sustituta.

En la API son dos llamadas, en este orden:

1. [`POST /v1/invoices`](/docs/api/timbrar-factura) — timbras la sustituta con `cfdiRelacionados`.
2. `POST /v1/invoices/{id}/cancel` — cancelas la original con `motivo` `01` y `folioSustitucion`.

En la sustituta el tipo de relación es `04`. En la cancelación el motivo es `01`.

Timbrar la sustituta cuesta un timbre. Cancelar la original cuesta otro. Si la sustituta no está relacionada con la original, la cancelación responde 400 con `code` `validation_error` y `subcode` `substitute_not_related`, y no gasta ese timbre. El detalle está más abajo.

Esta guía es de la API. En la app de Shopify el admin cancela por ahora con motivo 02. Ese flujo está en [Cancelación y acuse](/docs/cancelacion-y-acuse).

Autenticación: `Authorization: Bearer sk_test_…` o `sk_live_…`. En un workspace, el header `x-account-id` indica la cuenta. Las dos llamadas piden `Idempotency-Key` (1 a 255 caracteres).

## 1. Emitir la factura sustituta

> `POST https://api.cfdi.express/v1/invoices`

El cuerpo es el de [Timbrar una factura](/docs/api/timbrar-factura), más `cfdiRelacionados`:

| Campo | Tipo | Requerido | Descripción |
|---|---|---|---|
| `cfdiRelacionados` | object | sí, en este flujo | CFDIs relacionados (Anexo 20, nodo `CfdiRelacionados`). |
| `cfdiRelacionados.tipoRelacion` | string | sí | Catálogo `c_TipoRelacion`. En facturas de ingreso solo se acepta `"04"` (sustitución de los CFDI previos). |
| `cfdiRelacionados.uuids` | string[] | sí | Folios fiscales de los CFDI que esta factura sustituye. De 1 a 100. Pueden estar timbrados en esta API o en otro sistema. |

`uuids` lleva el UUID (folio fiscal) de la factura original. El `id` (`inv…`) de esa misma factura es el que va en la ruta del paso 2.

La petición espera hasta 20 segundos. **201** trae la factura timbrada: copia `uuid`, que es el folio fiscal de la sustituta. Bajo carga extrema responde **202** con `status` `stamping`. En ese caso consultas [`GET /v1/invoices/{id}`](/docs/api/obtener-factura) hasta que pase a `stamped` y tomas el `uuid`. Cancela la original cuando ya tengas ese UUID.

### cURL

El UUID de la original en este ejemplo es el del esquema publicado. `merchantId`, receptor y concepto son los de la referencia de timbrado: sirven para ver la forma del JSON.

```bash
curl -X POST https://api.cfdi.express/v1/invoices \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: order_1001_sustituta" \
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
        "quantity": 2,
        "unitPrice": 1740.00,
        "ivaRate": 0.16
      }
    ],
    "pricesIncludeTax": true,
    "cfdiRelacionados": {
      "tipoRelacion": "04",
      "uuids": ["119038DB-1B9F-4BC5-8A71-712E95AED2C9"]
    }
  }'
```

En la respuesta **201**, el campo que vas a usar en el paso 2 es `uuid`. El `id` de esa misma respuesta es el de la sustituta. La cancelación del paso 2 va sobre el `id` de la **original**.

## 2. Cancelar la original con motivo 01

> `POST https://api.cfdi.express/v1/invoices/{id}/cancel`

`{id}` es el id de la factura original (`inv…`). El UUID de la original ya fue en `cfdiRelacionados.uuids` del paso 1. En esta ruta entra el id de la original; el id de la sustituta se queda en el recurso del paso 1. La referencia general de esta operación está en [Cancelar una factura](/docs/api/cancelar-factura).

| Campo | Tipo | Requerido | Descripción |
|---|---|---|---|
| `motivo` | string | sí | `"01"` para sustitución. |
| `folioSustitucion` | string (uuid) | sí, con `01` | UUID de la sustituta (`uuid` del paso 1). Sin este campo, motivo 01 responde 400 `validation_error`. |

### cURL

`folioSustitucion` es el `uuid` que devolvió el timbrado de la sustituta. El de abajo es un ejemplo; sustitúyelo por el de tu **201**.

```bash
curl -X POST https://api.cfdi.express/v1/invoices/inv7k3q9x2m4v1t8p6d0n5cb/cancel \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: cancel_order_1001_sustitucion" \
  -H "Content-Type: application/json" \
  -d '{
    "motivo": "01",
    "folioSustitucion": "7a2c8e10-55b4-4c31-9a12-1d0e6f3b8c44"
  }'
```

La cancelación aceptada responde **202**. El recurso queda con `status` `cancelled` y `cancellation.motivo` `01`. `cancellation.folioSustitucion` es el UUID que enviaste. El acuse XML del SAT queda en `cancellation.acuseUrl`, URL firmada de 15 minutos. La forma completa de la respuesta está en [Cancelar una factura](/docs/api/cancelar-factura).

Una factura PPD con complementos de pago vigentes se cancela después de cancelar esos REP con `POST /v1/payments/{id}/cancel`.

## Error 400 `substitute_not_related`

`substitute_not_related` es el `subcode`. El `code` sigue siendo `validation_error`.

La API busca la sustituta por `folioSustitucion` entre las facturas timbradas en CFDI Express. Responde **400** cuando esa factura no trae `cfdiRelacionados` con `tipoRelacion` `"04"` que incluya el UUID de la original. La validación corre antes de enviar la cancelación al SAT y **antes de gastar un timbre**.

```json
{
  "type": "https://api.cfdi.express/docs/errors/validation_error",
  "title": "Validation error",
  "status": 400,
  "code": "validation_error",
  "subcode": "substitute_not_related"
}
```

El `detail` sugiere timbrar la sustituta con `cfdiRelacionados` `{ tipoRelacion: "04", uuids: [UUID de la original] }` o cancelar con motivo `02`.

Si `folioSustitucion` es el UUID de la misma factura que estás cancelando, la respuesta también es 400 `validation_error`.

Si omites `folioSustitucion` con motivo `01`, o el motivo no es `01`–`04`, la respuesta es 400 `validation_error`. Ese caso está en [Cancelar una factura](/docs/api/cancelar-factura).

## UUID en minúsculas (v1.50.1)

Desde la **v1.50.1** un UUID se acepta en minúsculas, en mayúsculas o mezclado, en `cfdiRelacionados.uuids` y en `folioSustitucion`. El formato es 8-4-4-4-12 en hexadecimal.

`119038db-1b9f-4bc5-8a71-712e95aed2c9` y `119038DB-1B9F-4BC5-8A71-712E95AED2C9` son el mismo folio fiscal.

## Los otros motivos

Los motivos `02`, `03` y `04` cubren los casos sin factura sustituta. El cuerpo lleva solo `motivo`:

| Motivo | Cuándo |
|---|---|
| `02` | Comprobante emitido con errores sin relación. |
| `03` | La operación no se llevó a cabo. |
| `04` | Operación nominativa relacionada en una factura global. |

El cuerpo, los ejemplos y el resto de errores (402 sin saldo, 422 rechazo del SAT, 503 PAC no disponible) están en [Cancelar una factura](/docs/api/cancelar-factura).

## Relacionados

- [Timbrar una factura](/docs/api/timbrar-factura) — `POST /v1/invoices`
- [Cancelar una factura](/docs/api/cancelar-factura) — motivos 01–04 y acuse
- [Acuse de cancelación](/blog/acuse-de-cancelacion) — qué es el acuse y los cuatro motivos
- [Cancelación y acuse](/docs/cancelacion-y-acuse) — cancelar desde la app de Shopify

---

Referencia completa: https://cfdi.express/docs/api · OpenAPI: https://api.cfdi.express/openapi.json · Llaves: https://dash.cfdi.express
