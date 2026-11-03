---
title: "CFDI 4.0 en dólares: moneda, tipo de cambio y pagos REP"
description: "Cómo facturar en USD con CFDI 4.0: campo Moneda, tipo de cambio FIX del DOF, notas de crédito y complemento de pago en dólares. Con ejemplos de la API."
image: "https://videos.acromatico.dev/api/images/assets/b8d441cb-9d64-4a00-b7c7-ff6971ae6420.jpg"
author: "Rafael González"
date: "2026-11-04"
keywords: "cfdi en dólares, factura en dólares CFDI 4.0, tipo de cambio CFDI, TipoCambio, Moneda c_Moneda, complemento de pago en dólares, TipoCambioP, nota de crédito en dólares, factura global otra moneda, FIX Banxico DOF, CFDI Express API"
---

# CFDI 4.0 en dólares: moneda, tipo de cambio y pagos REP

![Cómo emitir un CFDI 4.0 en dólares con la API de CFDI Express](https://videos.acromatico.dev/api/images/assets/b8d441cb-9d64-4a00-b7c7-ff6971ae6420.jpg?w=1600)

Si exportas servicios o productos, o cobras contratos en dólares, puedes emitir un **CFDI en dólares** sin pasar tus importes a pesos. En esta guía te explicamos qué pide el SAT, qué tipo de cambio usar y cómo resolver las notas de crédito y los complementos de pago. Al final te mostramos cómo hacerlo con la API de CFDI Express (v1.49.0).

## ¿Se puede emitir un CFDI 4.0 en dólares?

Sí. En el CFDI 4.0 el atributo `Moneda` acepta cualquier clave del catálogo `c_Moneda`, y cuando la moneda no es MXN debes incluir `TipoCambio`. En el comprobante, todos los importes (conceptos, descuentos, impuestos y total) van en la moneda del documento. No se convierten a pesos al timbrar.

## Campos Moneda y TipoCambio: qué pide el SAT

- **Moneda**: clave de `c_Moneda` (por ejemplo `USD` o `EUR`). La clave `XXX` solo es para comprobantes sin importes.
- **TipoCambio**: es obligatorio si la moneda es distinta de MXN y de XXX. En pesos se puede omitir y, si viene, debe ser `1`.
- **Formato**: hasta 18 enteros y **6 decimales** como máximo.
- **Decimales de los importes**: deben respetar los decimales que tiene la moneda en el catálogo.

Para el detalle campo por campo, consulta [Facturación al extranjero: moneda extranjera](/docs/facturacion-al-extranjero#moneda-extranjera).

## Qué tipo de cambio usar: FIX del DOF y monedas distintas a USD

La guía de llenado del Anexo 20 pide el tipo de cambio **FIX** que publica Banxico en el DOF para el dólar. Para otras monedas se usa la tabla de equivalencias de Banxico. El tipo de cambio se expresa en **pesos por 1 unidad** de la moneda extranjera, por ejemplo `18.4567` pesos por dólar.

La validación del SAT contra el tipo de cambio publicado (CFDI40118) todavía no está en vigor. Por eso un tipo de cambio incorrecto no necesariamente se rechaza: se timbra tal como lo mandaste, y la responsabilidad es del emisor. Usa siempre el tipo de cambio real.

## Factura en dólares a cliente extranjero (XEXX010101000 y IVA 0%)

Si tu cliente está fuera de México, el receptor lleva el RFC genérico `XEXX010101000`, su `ResidenciaFiscal` (clave de país), su `NumRegIdTrib` (su identificación fiscal), uso `S01`, régimen `616` y el código postal del emisor. La moneda y el receptor son independientes: puedes facturar en dólares a un cliente extranjero o a uno mexicano.

El IVA no depende de la moneda. Según tu operación, puede corresponder la tasa 0% (por ejemplo, en algunas exportaciones), la tasa general u otro tratamiento: no es una regla fija para toda factura en dólares. La API no decide el impuesto: tú mandas en cada concepto el que corresponda (`ivaRate`, o `ivaExempt` si está exento). Si tienes dudas sobre tu caso, valídalo con tu contador.

## Nota de crédito en dólares

Una nota de crédito sobre una factura en dólares **va en la misma moneda que la factura relacionada**. No se puede mezclar: una nota en MXN sobre una factura en USD se rechaza. Lleva su propio tipo de cambio, el de la fecha de la nota. Más detalle en [Crear nota de crédito](/docs/api/crear-nota-de-credito).

## Complemento de pago en dólares

Cuando cobras una factura PPD en dólares, el REP (Pagos 2.0) va en la moneda de la factura (`MonedaP` = `MonedaDR`, con `EquivalenciaDR` = `1`). El `TipoCambioP` es el **del día del pago**, no el de la factura. Los importes del documento relacionado y sus impuestos se quedan en dólares, y el nodo `Totales` se convierte a pesos (importe × `TipoCambioP`, redondeado a 2 decimales), como lo pide el complemento. Revisa [Complementos de pago](/docs/complementos-de-pago).

## Factura global en otra moneda

La guía de llenado de la factura global permite cualquier clave de `c_Moneda` con su `TipoCambio`, así que aplican las mismas reglas. Ve [Facturación global mensual](/docs/facturacion-global-mensual).

## Cómo hacerlo con la API de CFDI Express

Desde la v1.49.0, la API acepta `currency` (clave c_Moneda, por omisión `MXN`) y `exchangeRate` (MXN por 1 unidad, máximo 6 decimales). Además de pesos, acepta 18 monedas de 2 decimales: USD, EUR, CAD, GBP, CHF, AUD, NZD, CNY, HKD, SGD, SEK, NOK, DKK, BRL, ARS, COP, PEN y GTQ.

**Factura en USD** ([Timbrar factura](/docs/api/timbrar-factura)):

```bash
curl -X POST https://api.cfdi.express/v1/invoices \
  -H "Authorization: Bearer $CFDI_API_KEY" \
  -H "Idempotency-Key: factura-usd-001" \
  -H "Content-Type: application/json" \
  -d '{
    "merchantId": "mer_...",
    "metodoPago": "PUE",
    "formaPago": "03",
    "usoCfdi": "G03",
    "currency": "USD",
    "exchangeRate": 18.4567,
    "pricesIncludeTax": true,
    "receiver": { "rfc": "URE180429TM6", "name": "UNIVERSIDAD ROBOTICA ESPAÑOLA", "zip": "86991", "regimenFiscal": "601" },
    "items": [
      { "productCode": "81112101", "unitCode": "E48", "description": "Servicio de software", "quantity": 1, "unitPrice": 116, "ivaRate": 0.16 }
    ]
  }'
```

La respuesta trae `"currency": "USD"`, `"exchangeRate": 18.4567` y `"total": 116`, en dólares. El PDF muestra el tipo de cambio y el equivalente en pesos.

**Nota de crédito**: hereda la moneda de la factura y solo mandas su tipo de cambio:

```json
{ "merchantId": "mer_...", "relatedInvoiceId": "inv_...", "formaPago": "03", "exchangeRate": 18.4567,
  "items": [{ "productCode": "81112101", "unitCode": "E48", "description": "Bonificación", "quantity": 1, "unitPrice": 11.6, "ivaRate": 0.16 }] }
```

**Complemento de pago** (`POST /v1/invoices/{id}/payments`, ver [Timbrar complemento de pago](/docs/api/timbrar-complemento-de-pago)):

```json
{ "monto": 58, "formaPago": "03", "exchangeRate": 18.4567 }
```

Si usas agentes de IA, las herramientas del [servidor MCP](/docs/api/mcp) (`create_invoice`, pagos y reportes) también reconocen la moneda.

## Errores comunes

- **Mandar `currency: "USD"` sin `exchangeRate`**: la API responde 400. Antes de la v1.49.0 estos campos se ignoraban y se timbraba en pesos, así que revisa tu integración si ya los mandabas.
- **Mandar MXN con un tipo de cambio distinto de 1**: responde 400.
- **Usar una moneda no soportada** (por ejemplo, JPY): responde 400.
- **Nota de crédito en otra moneda** que la de la factura: responde 400.
- **Usar el tipo de cambio de la factura en el REP**: usa el de la fecha de pago.
- **Más de 6 decimales** en el tipo de cambio: el SAT no lo acepta.

## Preguntas frecuentes

**¿Es obligatorio el tipo de cambio en una factura en dólares?**
Sí. Si la moneda no es MXN (ni XXX), `TipoCambio` es obligatorio. En la API, sin `exchangeRate` la respuesta es 400.

**¿Qué tipo de cambio uso en el complemento de pago?**
El de la fecha del pago (`TipoCambioP`). Para USD es el FIX de Banxico publicado en el DOF.

**¿Puedo facturar en dólares a un cliente mexicano?**
Sí. La moneda no depende del receptor: puedes emitir un CFDI en USD a un RFC mexicano con su régimen y uso de CFDI normales.

**¿Cuántos decimales lleva el tipo de cambio?**
Hasta 6. Los importes del comprobante usan los decimales de la moneda (2 en todas las que acepta la API).

**¿Puedo pagar en pesos una factura en dólares?**
Hoy no se puede en la API de CFDI Express: el REP va en la misma moneda que la factura (`EquivalenciaDR` = 1).
