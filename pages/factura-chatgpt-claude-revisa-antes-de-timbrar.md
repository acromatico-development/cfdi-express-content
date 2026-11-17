---
title: "Factura desde ChatGPT o Claude: revisa antes de timbrar"
description: "Tu agente de IA te muestra la factura como se va a imprimir antes de timbrarla. La autorizas, pides cambios o la descargas. Sin timbre ni folio."
author: "Rafael González"
date: "2026-11-18"
keywords: "factura desde ChatGPT, factura con Claude, vista previa factura, revisar factura antes de timbrar, CFDI 4.0, agente de IA, MCP, CFDI Express"
---
# Factura desde ChatGPT o Claude sin sorpresas: revisa antes de timbrar

<!-- TODO: imagen de portada (frontmatter image) -->

Pedirle una factura a tu agente de IA es rápido. Lo que da nervios es el momento de timbrar: una vez que el CFDI llega al SAT, corregirlo significa cancelar y volver a emitir. Por eso, desde la versión **v1.53.0** de CFDI Express, tu agente te muestra la factura **tal como se va a imprimir antes de timbrarla**, y tú decides qué sigue.

## Cómo funciona la vista previa

**Respuesta corta:** le pides la factura a tu agente, él arma una vista previa con todos los datos y espera a que la autorices.

1. Conecta CFDI Express a ChatGPT o Claude desde tu cuenta en [dash.cfdi.express](https://dash.cfdi.express). Los pasos están en [Factura desde tu agente](/agente).
2. Pídele la factura en español, con los datos del cliente y de la venta.
3. El agente valida los datos y te muestra una tarjeta con la factura como se va a imprimir: receptor, conceptos, impuestos y total.
4. Elige qué hacer con los botones de la tarjeta.

<!-- TODO: still del demo — tarjeta de vista previa en el chat con los tres botones -->
![TODO: Vista previa de una factura CFDI 4.0 dentro del chat, con los botones Pedir cambios, Descargar PDF y Autorizar y timbrar](TODO-screenshot)
<!-- ENDTODO -->

## Los tres botones de la tarjeta

**Autorizar y timbrar.** Si todo está bien, la autorizas y la factura se timbra ante el SAT con tu certificado. Es el único momento en que se consume un timbre. La tarjeta te muestra el folio fiscal (UUID) y un botón para abrir el PDF timbrado.

**Pedir cambios.** Le dices al agente qué corregir en el mismo chat. Por ejemplo: «ponle 50000 más IVA y que sea PPD». El agente ajusta los datos y te muestra una vista previa nueva con el total recalculado, en este caso $58,000.00. Con PPD, la forma de pago va en 99 (Por definir), como pide el SAT, y el agente la ajusta.

**Descargar PDF.** Te bajas la vista previa en PDF para revisarla con calma o mandársela a tu cliente antes de timbrar.

<!-- TODO: still del demo — la vista previa actualizada después de pedir cambios -->
![TODO: Vista previa actualizada después de pedir un cambio en el chat](TODO-screenshot)
<!-- ENDTODO -->

## Lo que la vista previa no hace

**Respuesta corta:** no timbra, no cobra y no gasta folio.

- **No va al SAT.** La factura todavía no existe como CFDI. No hay UUID, sellos ni código QR: en su lugar, el documento dice «Se asigna al timbrar».
- **No consume timbre ni saldo.** Puedes revisar y corregir las veces que necesites.
- **No aparta folio.** El folio que ves es el siguiente disponible. Si timbras otra factura antes de autorizar esta, el folio final cambia.

El PDF de la vista previa lleva la marca de agua **PREVIEW** en cada página y la leyenda **VISTA PREVIA — SIN VALIDEZ FISCAL**. Así nadie lo confunde con una factura real si se lo reenvías a tu cliente.

## Un solo timbre, aunque confirmes dos veces

¿Le diste doble clic a **Autorizar y timbrar**? ¿Contestaste «sí» dos veces en el chat? Se timbra **una sola factura**. La vista previa y el timbrado comparten la misma referencia, así que la segunda confirmación regresa la factura que ya se timbró, en vez de emitir otra.

Si pides cambios, el agente arma una vista previa nueva. Autoriza esa, la más reciente.

## Si ya revisaste los datos, sáltatela

La vista previa no es obligatoria. El agente la muestra por omisión, pero si ya tienes los datos claros, pídele que timbre directo: «timbra sin vista previa». Funciona igual que antes.

En agentes que no muestran la tarjeta, la vista previa llega como resumen en el chat: el agente te dice receptor, conceptos y total, y espera tu «sí» antes de timbrar.

## Para developers

Lo mismo está en la API: `POST /v1/invoices/preview` regresa los totales, los datos del CFDI y la representación impresa sin timbrar, y `POST /v1/invoices/preview/pdf` regresa el PDF. Los detalles están en la guía de [vista previa de la factura](/docs/vista-previa-factura-antes-de-timbrar) y en la documentación del [servidor MCP](/docs/api/mcp).

## Preguntas frecuentes

**¿La vista previa cuesta?**
No. No consume timbre ni saldo. Solo se cobra el timbre cuando autorizas y la factura se timbra ante el SAT.

**¿El PDF de la vista previa sirve como factura?**
No. Lleva la marca de agua PREVIEW y la leyenda «VISTA PREVIA — SIN VALIDEZ FISCAL». No tiene UUID, sellos ni código QR, porque esos datos se generan al timbrar.

**¿Qué pasa si confirmo dos veces?**
Se timbra una sola factura. La vista previa y el timbrado comparten la misma referencia, así que un doble clic o un «sí» repetido regresan la factura que ya se timbró.

**¿Tengo que ver la vista previa siempre?**
No. Puedes pedirle a tu agente que se la salte y timbre directo.

**¿Funciona con ChatGPT y con Claude?**
Sí. En los dos ves la tarjeta con los botones. En agentes que no la muestran, el agente te resume la factura en el chat y espera tu confirmación.

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {"@type": "Question", "name": "¿La vista previa cuesta?", "acceptedAnswer": {"@type": "Answer", "text": "No. No consume timbre ni saldo. Solo se cobra el timbre cuando autorizas y la factura se timbra ante el SAT."}},
    {"@type": "Question", "name": "¿El PDF de la vista previa sirve como factura?", "acceptedAnswer": {"@type": "Answer", "text": "No. Lleva la marca de agua PREVIEW y la leyenda «VISTA PREVIA — SIN VALIDEZ FISCAL». No tiene UUID, sellos ni código QR, porque esos datos se generan al timbrar."}},
    {"@type": "Question", "name": "¿Qué pasa si confirmo dos veces?", "acceptedAnswer": {"@type": "Answer", "text": "Se timbra una sola factura. La vista previa y el timbrado comparten la misma referencia, así que un doble clic o un «sí» repetido regresan la factura que ya se timbró."}},
    {"@type": "Question", "name": "¿Tengo que ver la vista previa siempre?", "acceptedAnswer": {"@type": "Answer", "text": "No. Puedes pedirle a tu agente que se la salte y timbre directo."}},
    {"@type": "Question", "name": "¿Funciona con ChatGPT y con Claude?", "acceptedAnswer": {"@type": "Answer", "text": "Sí. En los dos ves la tarjeta con los botones. En agentes que no la muestran, el agente te resume la factura en el chat y espera tu confirmación."}}
  ]
}
</script>

¿Quieres probarlo? Crea tu cuenta gratis en [dash.cfdi.express](https://dash.cfdi.express), conecta tu agente y haz una factura de prueba, sin validez fiscal, antes de timbrar de verdad.
