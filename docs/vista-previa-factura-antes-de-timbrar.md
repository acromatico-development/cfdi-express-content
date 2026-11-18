---
title: "Vista previa de la factura antes de timbrar (MCP y API)"
description: "Revisa un CFDI 4.0 antes de timbrarlo desde ChatGPT, Claude o la API de CFDI Express: vista previa sin timbre, sin cobro y sin folio, y opcional."
keywords: "vista previa factura, revisar factura antes de timbrar, preview CFDI, preview_invoice, POST /v1/invoices/preview, MCP, agente de IA, ChatGPT, Claude, CFDI Express API"
---
# Vista previa de la factura antes de timbrar

URL de esta página: https://cfdi.express/docs/vista-previa-factura-antes-de-timbrar

Disponible a partir de la versión **v1.53.0** de la API.

La vista previa arma la factura exactamente como se va a timbrar y la muestra como se va a imprimir, **sin timbrarla**. Sirve para que una persona la revise antes de que llegue al SAT, desde su agente de IA o desde tu sistema.

- **No timbra, no cobra y no gasta folio.** No manda nada al SAT, no consume timbre ni saldo, no aparta el siguiente folio y no guarda la factura.
- **Valida lo mismo que el timbrado.** Corre todas las validaciones de [Timbrar una factura](/docs/api/timbrar-factura). Una vista previa que pasa no falla después por una validación de datos de la API. El SAT valida al timbrar, así que puede haber un rechazo del SAT (`sat_rejected`) que la vista previa no detecta.
- **Es opcional.** Puedes timbrar directo, como antes.

Solo aplica a facturas de ingreso (`POST /v1/invoices` y `create_invoice`). No hay vista previa de notas de crédito, complementos de pago, nómina ni retenciones.

## La vista previa en el chat (servidor MCP)

En el [servidor MCP](/docs/api/mcp), la herramienta es `preview_invoice`. Recibe exactamente los mismos argumentos que `create_invoice`, sin `idempotency_key`.

Las instrucciones del servidor le piden al agente que llame `preview_invoice` antes de cada `create_invoice`, salvo que el usuario pida saltarla.

### La tarjeta de vista previa (ChatGPT y Claude)

En los clientes que muestran apps de MCP, como ChatGPT y Claude, `preview_invoice` abre una tarjeta con la factura tal como se va a imprimir. El encabezado dice **Vista previa · CFDI 4.0 · sin timbrar** y, con llaves de prueba, aparece la etiqueta **Modo prueba**. Abajo están estos botones:

| Botón | Qué hace |
|---|---|
| **Autorizar y timbrar** | Llama `create_invoice` con los mismos argumentos de la vista previa y su `idempotency_key`. Se timbra ante el SAT y se consume 1 timbre. La tarjeta muestra **Factura timbrada.** con el folio fiscal (UUID) y el botón **Abrir PDF** para el PDF timbrado. |
| **Pedir cambios** | Escribe en el chat «Quiero hacer cambios a esta factura antes de timbrarla.». Dile al agente qué cambiar; él ajusta los argumentos y llama `preview_invoice` otra vez, con una tarjeta nueva y el total recalculado. |
| **Descargar PDF** | Descarga el PDF de la vista previa. Si el cliente permite descargas, guarda el archivo (Claude te pide confirmar). Si solo permite abrir ligas, abre una liga temporal de unos 15 minutos. Si no permite ninguna de las dos, el botón no aparece. |

Debajo de los botones, la tarjeta dice: «Revisa los datos. Al autorizar, la factura se timbra ante el SAT y se consume 1 timbre.»

Después de timbrar desde la tarjeta, el agente recibe el aviso de no volver a llamar `create_invoice` para esa factura.

Si pediste cambios, autoriza la tarjeta más reciente: cada vista previa tiene su propia llave y la tarjeta anterior conserva los datos que tenía.

### En clientes sin tarjeta

En un cliente que no muestra apps de MCP (por ejemplo, un agente propio), `preview_invoice` devuelve un resumen en texto: emisor, receptor, serie y folio, método y forma de pago, uso del CFDI, subtotal, descuento y total, y la `idempotency_key`. El agente te lo resume y espera tu confirmación. Cuando dices que sí, llama `create_invoice` con los mismos argumentos más esa `idempotency_key`.

### Una sola factura, aunque confirmes dos veces

La vista previa genera una llave de idempotencia y el botón **Autorizar y timbrar** la usa. Si el agente timbra después de un «sí» en el chat, las instrucciones le piden usar esa misma llave. Por eso un doble clic, un reintento o un «sí» repetido terminan en **un solo CFDI y un solo timbre**:

- Si la primera llamada ya terminó, la segunda recibe la misma factura (`Idempotency-Replayed: true` en la API).
- Si la primera sigue en curso, la segunda recibe `409 request_in_flight` y no timbra.
- Si la llave se usa con argumentos distintos, la respuesta es `422 idempotency_key_reuse` y no timbra.

Si `create_invoice` responde con error, el botón se vuelve a habilitar: reintentar con la misma llave nunca timbra dos veces.

Esto depende de que se use la llave de la vista previa. Si el agente llama `create_invoice` sin `idempotency_key`, se genera una llave nueva en cada llamada.

### Saltarse la vista previa

La vista previa no es obligatoria. Pídele a tu agente que timbre directo, por ejemplo: «timbra sin vista previa». Las instrucciones del servidor le permiten saltarla cuando el usuario lo pide explícitamente, y `create_invoice` funciona igual que antes de la v1.53.0.

## Qué muestra la vista previa

El documento usa la misma plantilla que el PDF timbrado, con estas diferencias:

- Marca de agua diagonal repetida en cada página: **PREVIEW**.
- Leyenda al inicio: **VISTA PREVIA — SIN VALIDEZ FISCAL**, con el texto «Este documento aún no ha sido timbrado. El folio fiscal (UUID), los sellos digitales y el código QR se generan al timbrar.»
- En lugar del folio fiscal (UUID): «Se asigna al timbrar». La fecha de timbrado aparece como «—».
- En lugar del timbre fiscal digital: «Pendiente — el SAT asigna el timbre, los sellos y el QR de verificación cuando se autoriza y timbra esta factura.»
- Pie de página: «Vista previa de un CFDI versión 4.0 — no es un comprobante fiscal.»

Dos datos pueden cambiar al timbrar:

- **Folio.** Si no mandas `folio`, la vista previa muestra el siguiente folio del emisor (`folioIsProjected: true`). Si se timbra otra factura antes, el folio final es otro.
- **Fecha.** Si no mandas `fecha`, la vista previa usa la hora del momento en que se generó. Al timbrar, la Fecha del CFDI es la del timbrado.

## Vista previa por API

Los dos endpoints usan `Authorization: Bearer sk_test_…` o `sk_live_…` y no piden `Idempotency-Key`. En un workspace, el header `x-account-id` indica la cuenta.

### POST /v1/invoices/preview

> `POST https://api.cfdi.express/v1/invoices/preview`

**Request.** El mismo cuerpo de [POST /v1/invoices](/docs/api/timbrar-factura): `merchantId`, `receiver`, `items`, `metodoPago`, `formaPago`, `usoCfdi` y los campos opcionales (`serie`, `folio`, `fecha`, `currency`, `exchangeRate`, `cfdiRelacionados`, `informacionGlobal`, `logoUrl`, etc.).

```bash
curl -X POST https://api.cfdi.express/v1/invoices/preview \
  -H "Authorization: Bearer sk_test_..." \
  -H "Content-Type: application/json" \
  -d '{
    "merchantId": "mkq2f8v3x1t7p9d4n6c0b5rz",
    "metodoPago": "PPD",
    "formaPago": "99",
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
        "unitPrice": 2500,
        "ivaRate": 0.16
      }
    ],
    "pricesIncludeTax": false
  }'
```

**Respuesta 200.** Un objeto `invoice_preview`:

| Campo | Tipo | Descripción |
|---|---|---|
| `object` | string | Siempre `invoice_preview`. |
| `livemode` | boolean | `false` con llave `sk_test_`. |
| `merchantId` | string | Emisor. |
| `emisor` | object | `rfc` y `name` del emisor. |
| `kind` | string | `nominal` o `global`. |
| `serie` | string | La del cuerpo o la del emisor. |
| `folio` | string | El del cuerpo o el siguiente folio del emisor. |
| `folioIsProjected` | boolean | `true` cuando `folio` es el siguiente folio del emisor y puede cambiar al timbrar. |
| `fecha` | string | Fecha del CFDI con la que se armó la vista previa, en hora local del lugar de expedición. |
| `metodoPago`, `formaPago`, `usoCfdi` | string | Los del cuerpo. |
| `currency`, `exchangeRate` | string, number | Moneda y tipo de cambio (1 con MXN). |
| `subtotal`, `discount`, `total` | number | Totales calculados por el motor de impuestos. |
| `receiver` | object | Receptor como se timbra: `rfc`, `nombre`, `domicilioFiscal`, `regimenFiscal` (y `residenciaFiscal`, `numRegIdTrib` si aplican). En una global, `nombre` es `PUBLICO EN GENERAL`. |
| `cfdi` | object | Los datos del CFDI 4.0 como se mandarían al PAC, sin sellar. |
| `html` | string | La representación impresa (misma plantilla que el PDF) con la marca de agua PREVIEW. |

```json
{
  "object": "invoice_preview",
  "livemode": false,
  "merchantId": "mkq2f8v3x1t7p9d4n6c0b5rz",
  "emisor": { "rfc": "EKU9003173C9", "name": "ESCUELA KEMPER URGATE" },
  "kind": "nominal",
  "serie": "A",
  "folio": "53",
  "folioIsProjected": true,
  "fecha": "2026-11-17T11:30:00",
  "metodoPago": "PPD",
  "formaPago": "99",
  "usoCfdi": "G03",
  "currency": "MXN",
  "exchangeRate": 1,
  "subtotal": 50000,
  "discount": 0,
  "total": 58000,
  "receiver": {
    "rfc": "XIA190128J61",
    "nombre": "XENON INDUSTRIAL ARTICLES",
    "domicilioFiscal": "01160",
    "regimenFiscal": "601"
  },
  "cfdi": { "Version": "4.0", "TipoDeComprobante": "I", "...": "recortado" },
  "html": "<!DOCTYPE html>…"
}
```

Para timbrar lo que el usuario aprobó, manda el **mismo cuerpo** a `POST /v1/invoices` con tu `Idempotency-Key`.

### POST /v1/invoices/preview/pdf

> `POST https://api.cfdi.express/v1/invoices/preview/pdf?delivery=file`

El mismo cuerpo y las mismas validaciones, con el resultado en PDF (marca de agua PREVIEW, sin UUID, sellos ni QR).

| Query | Valores | Descripción |
|---|---|---|
| `delivery` | `file` (default) o `url` | `file` responde el PDF. `url` sube el PDF y responde JSON con una liga temporal. |

- **`delivery=file`**: 200 con `Content-Type: application/pdf`, `Cache-Control: no-store` y `Content-Disposition: attachment`. El archivo se llama `Vista previa factura {serie} {folio}.pdf`, por ejemplo `Vista previa factura A 53.pdf`.
- **`delivery=url`**: 200 con JSON. Es el único caso en que se guarda algo: el PDF temporal de la liga.

```json
{
  "object": "invoice_preview_pdf",
  "filename": "Vista previa factura A 53.pdf",
  "url": "https://files.cfdi.express/previews/...pdf?X-Amz-Expires=900&...",
  "expiresAt": "2026-11-17T17:45:00.000Z"
}
```

La liga vence en 15 minutos.

### Errores

| Status | code | Cuándo |
|---|---|---|
| 400 | `validation_error` | El mismo cuerpo lo rechazaría `POST /v1/invoices`. Trae el mismo `detail`, `subcode` y `errors` que el timbrado. También cuando la factura da un total en cero o no tiene conceptos. |
| 401 | `unauthorized` | Falta la llave o es inválida. |
| 403 | `forbidden` | No eres miembro de la cuenta del header `x-account-id`. |
| 404 | `not_found` | El `merchantId` no existe en tu cuenta. |
| 429 | `rate_limited` | Excediste el límite de requests por minuto. |
| 503 | `pdf_unavailable` | Solo en `/pdf`: el generador de PDF no está disponible, o el almacenamiento no está disponible con `delivery=url` (usa `delivery=file`). Reintenta en unos momentos. |

La vista previa nunca responde `402 insufficient_credits`: no usa saldo. Si no hay saldo, el error aparece hasta el timbrado.

## Relacionado

- [Servidor MCP de CFDI Express](/docs/api/mcp)
- [Timbrar una factura (POST /v1/invoices)](/docs/api/timbrar-factura)
- [Factura desde tu agente de IA](https://cfdi.express/agente)
