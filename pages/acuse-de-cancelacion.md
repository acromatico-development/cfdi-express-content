---
title: "Cómo descargar el acuse de cancelación del SAT (CFDI 4.0)"
description: "Cómo descargar el acuse de cancelación del SAT de una factura: qué es, los 4 motivos del CFDI 4.0 y cómo obtenerlo en PDF desde Shopify con CFDI Express."
image: "https://assets.acromatico.dev/marketing/diego/blog/hero-cfdi-facturas-4.0.png"
author: "Rafael González"
date: "2026-10-12"
keywords: "acuse de cancelación, acuse de cancelación sat, acuse de cancelación de factura, motivos de cancelación cfdi, cancelar factura 4.0, cancelar cfdi shopify, CFDI Express"
---
# Acuse de cancelación SAT: qué es y cómo obtenerlo

Cancelar una factura no termina cuando das clic en «cancelar». Termina cuando el SAT la registra como cancelada, y la prueba de eso es el **acuse de cancelación**. Aquí ves qué es, qué motivo usar y cómo cancelar y obtener el acuse si vendes en Shopify con [CFDI Express](https://apps.shopify.com/cfdi-express).

El paso a paso dentro de la app está en [Cancelación y acuse](/docs/cancelacion-y-acuse).

## ¿Qué es el acuse de cancelación?

**Respuesta corta:** es el comprobante de que el SAT procesó la cancelación de un CFDI. Trae el UUID de la factura, la fecha y el estatus de la cancelación.

Guárdalo. Es lo que te respalda ante tu contador, tu cliente o una revisión del SAT si alguien pregunta por qué esa factura ya no cuenta.

## Los 4 motivos de cancelación del CFDI 4.0

| Motivo | Cuándo se usa | ¿Pide CFDI que la sustituye? |
| --- | --- | --- |
| **01** Comprobante emitido con errores con relación | Hay un error y ya timbraste la factura correcta | Sí |
| **02** Comprobante emitido con errores sin relación | Hay un error y no vas a relacionar una factura nueva | No |
| **03** No se llevó a cabo la operación | La venta no sucedió | No |
| **04** Operación nominativa relacionada en una factura global | La venta estaba en la factura global y ahora se factura a nombre del cliente | No |

El detalle de los datos SAT está en [CFDI facturas 4.0](/blog/cfdi-facturas-4-0).

## ¿Tu cliente tiene que aceptar la cancelación?

Depende. Para algunas facturas el SAT le pide al receptor que acepte o rechace la cancelación desde el portal del SAT, y mientras tanto la factura queda «en proceso». Si no responde en el plazo que marca el SAT, la cancelación se toma como aceptada. Hay excepciones en las que no se pide aceptación, por ejemplo según el monto o el tipo de comprobante (como las facturas a público en general). Las reglas cambian, así que confirma con tu contador las vigentes antes de cancelar.

## Cómo cancelar y obtener el acuse en CFDI Express

Tienes dos caminos:

### Desde el admin de Shopify

Cancelas el CFDI desde la orden en el admin de Shopify. **Por ahora el admin solo usa el motivo 02** (errores sin relación).

### Con Shopify Flow

La acción **«Eliminar CFDI»** de Shopify Flow acepta los **motivos 01 a 04**. Úsala si necesitas cancelar con relación (01), porque la venta no se hizo (03) o para sacar una orden de la factura global (04). Más en [automatizaciones con Shopify Flow](/docs/uso-shopify-flow-automatizaciones-cfdi-express).

### Descarga del acuse

El acuse se obtiene en PDF, queda guardado y lo descargas desde CFDI Express cuando lo necesites. Las instrucciones vigentes están en [Cancelación y acuse](/docs/cancelacion-y-acuse).

## Cancelar y volver a facturar

Un caso común: el cliente pide su factura cuando su orden ya entró a la factura global (que en CFDI Express siempre es mensual, 04). Puedes cancelar y timbrar la factura a su nombre, o compensar con nota de crédito. Revisa con tu contador cuál te conviene. Lo explicamos en la guía de [factura global en Shopify](/blog/factura-global-shopify).

Para que tus clientes facturen solos y no tengas que cancelar tanto, activa el [portal de autofacturación](/blog/portal-de-autofacturacion). Así se configura: [tutorial de autofacturación](/blog/crea-un-portal-de-auto-facturacion-cfdi-en-5-minutos).

## Preguntas frecuentes

**¿Dónde descargo el acuse de cancelación?**
La cancelación la registra el SAT. En CFDI Express el acuse queda guardado en PDF y lo descargas desde la app.

**¿Qué motivo uso desde el admin de Shopify?**
Por ahora el admin solo cancela con motivo 02. Para 01, 03 o 04 usa la acción «Eliminar CFDI» de Shopify Flow.

**¿Puedo cancelar una factura con motivo 01 en CFDI Express?**
Sí, con la acción «Eliminar CFDI» de Shopify Flow, que acepta los motivos 01 a 04.

**¿Una factura cancelada se puede volver a emitir?**
Puedes timbrar una factura nueva para la misma venta. Si la relacionas con la cancelada, usa el motivo 01.

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {"@type": "Question", "name": "¿Qué es el acuse de cancelación?", "acceptedAnswer": {"@type": "Answer", "text": "Es el comprobante de que el SAT procesó la cancelación de un CFDI. Trae el UUID de la factura, la fecha y el estatus de la cancelación."}},
    {"@type": "Question", "name": "¿Qué motivo de cancelación uso desde el admin de Shopify?", "acceptedAnswer": {"@type": "Answer", "text": "En CFDI Express el admin cancela por ahora con motivo 02. Para los motivos 01, 03 o 04 usa la acción Eliminar CFDI de Shopify Flow."}},
    {"@type": "Question", "name": "¿Una factura cancelada se puede volver a emitir?", "acceptedAnswer": {"@type": "Answer", "text": "Puedes timbrar una factura nueva para la misma venta. Si la relacionas con la cancelada, usa el motivo 01."}}
  ]
}
</script>

## Cancela y factura desde Shopify

1. Instala [CFDI Express en Shopify](https://apps.shopify.com/cfdi-express) y completa el [onboarding](/docs/onboarding).
2. Revisa el paso a paso en [Cancelación y acuse](/docs/cancelacion-y-acuse).
3. Si cancelas seguido, arma tu flujo con «Eliminar CFDI» en Shopify Flow.

Plan Pro: **$250 USD al mes, ya incluye timbres (5,000 CFDIs)**, ver [planes y precios](/docs/planes-y-precios). ¿Dudas? [Agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).
