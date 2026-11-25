---
title: "Complemento de pago de una factura de otro sistema"
description: "Importa un CFDI 4.0 de ingreso timbrado en otro sistema y emite el complemento de pago (REP) pendiente, por API o MCP."
image: "https://videos.acromatico.dev/api/images/assets/d9faa378-3e95-4068-80ab-4609bfb117b8.png"
author: "Rafael González"
date: "2026-12-02"
keywords: "complemento de pago, complemento de pago factura de otro sistema, REP factura otro sistema, importar CFDI 4.0, API complemento de pago, saldo insoluto, factura PPD, importar XML factura"
---
# Cómo emitir el complemento de pago de una factura de otro sistema

![Personaje 3D con hoodie teal entre facturas genéricas que entran y documentos REP con sello dorado que salen, con el logo de CFDI Express.](https://videos.acromatico.dev/api/images/assets/5a28303c-24bf-4f7e-b34c-9a57a3d2e88a.png)

**Respuesta corta:** para emitir el complemento de pago de una factura timbrada en otro sistema, importa el CFDI 4.0 de ingreso con `POST /v1/invoices/import` (o adjúntalo al pago) y timbra el REP con `POST /v1/payments`.

Si cambiaste de sistema de facturación y te quedaron facturas PPD con complementos de pago (REP) pendientes, ahora puedes traer esos CFDI 4.0 a CFDI Express y emitir los REP desde la API o con tu agente vía MCP.

Esto es la API pública (`https://api.cfdi.express`), no la app de Shopify ni el dashboard. La guía [Complementos de pago](/docs/complementos-de-pago) es de la app de Shopify. La función salió en la versión 1.59.0. Por ahora no hay carga de XML en el dashboard ni en la app.

El paso a paso —qué XML entra, cómo sigue la numeración y cómo ligar una nota de crédito— está en [Importar CFDIs de otros sistemas](/docs/importar-cfdis-de-otros-sistemas). Si un mismo pago cubre varias facturas, el caso de facturas que ya están en la cuenta está en [Cómo emitir un REP para varias facturas PPD](/blog/emitir-rep-varias-facturas-ppd).

El REP es el CFDI con complemento de [recepción de pagos (Pagos 2.0)](http://omawww.sat.gob.mx/tramitesyservicios/Paginas/recepcion_de_pagos.htm).

Si aún no tienes cuenta, créala en [dash.cfdi.express](https://dash.cfdi.express). El timbre cuesta [$1 MXN, sin suscripción](https://cfdi.express/api). Importar un XML cuesta el 20 % del precio por timbre de tu cuenta: $0.20 con el precio de lista. Si el XML va adjunto al REP, esa importación no se cobra; solo se cobra el timbre del complemento.

## Qué puedes importar

Solo facturas de ingreso **CFDI 4.0**, PUE o PPD, timbradas en otro sistema. Puedes importar una PUE, pero el complemento de pago solo aplica a facturas **PPD**. El RFC del emisor tiene que ser uno de tus merchants en esa cuenta y en ese modo (`sk_test_` o `sk_live_`).

No se importan CFDI 3.3, notas de crédito, nómina, traslado ni retenciones.

En **producción** (`sk_live_`) la API revisa el sello del emisor, el sello del SAT y que el SAT reporte el CFDI como **Vigente**. Esa consulta al SAT se vuelve a hacer antes de cada REP de una factura importada.

Con una llave `sk_test_` la consulta al SAT se simula y siempre responde Vigente, y el sello del SAT no se valida. El sello del emisor sí es obligatorio.

## Dos caminos para el REP

Puedes importar la factura primero y después timbrar el complemento, o adjuntar el XML en el mismo pago. En el segundo caso la importación no se cobra.

En una PPD hay que declarar si el otro sistema ya emitió complementos, para que el saldo y la numeración continúen. Con el XML adjunto al REP, esa declaración va en el mismo documento. Si la factura ya se importó, el REP sigue desde lo que quedó guardado.

El contrato, los ejemplos y los errores están en [Importar CFDIs de otros sistemas](/docs/importar-cfdis-de-otros-sistemas).

Por MCP, `import_invoices` trae los XML, y `create_payment` o `preview_payment` pueden llevar el XML de la factura en el pago.

## Notas de crédito

Si ya habías emitido aquí una nota de crédito apuntando al UUID de esa factura, al importarla la nota queda ligada y cuenta para el tope de crédito. Una nota nueva contra una factura que ya está en tu cuenta también se liga. Si es de otro emisor o de otra moneda, puede responder 400. El detalle está en la misma guía.

## Preguntas frecuentes

**¿Se puede emitir el REP de una factura de otro sistema?**
Sí. Importa el CFDI 4.0 de ingreso, o adjunta el XML al timbrar el complemento de pago. El paso a paso está en [Importar CFDIs de otros sistemas](/docs/importar-cfdis-de-otros-sistemas).

**¿Qué facturas se pueden importar?**
Solo facturas de ingreso CFDI 4.0, PUE o PPD. El complemento de pago solo aplica a PPD. En producción el SAT tiene que reportarlas Vigentes y se validan los dos sellos. Con `sk_test_` la consulta al SAT se simula (siempre Vigente) y el sello del SAT no se valida; el sello del emisor sí.

**¿Cuánto cuesta importar?**
El 20 % del precio por timbre de tu cuenta por cada XML importado: $0.20 con el precio de lista. Los rechazados y los que ya estaban en la cuenta no se cobran. Si el XML va adjunto al REP, la importación no se cobra: solo el timbre del REP. La API cuesta $1 MXN por timbre, sin suscripción.

**¿Qué es previousPayments y cómo sigo la numeración?**
En una factura PPD sirve para declarar si el otro sistema ya emitió complementos, y así el saldo y la numeración continúan. No aplica en una PUE. Los campos y un ejemplo están en [Importar CFDIs de otros sistemas](/docs/importar-cfdis-de-otros-sistemas).

**¿Puedo adjuntar el XML sin importarlo antes?**
Sí. Puedes mandarlo en el mismo complemento de pago. Esa importación no se cobra; solo se cobra el timbre del REP.

**¿Puedo cancelar o editar una factura importada?**
No. No se puede cancelar, volver a timbrar ni editar aquí. Se cancela donde se timbró.

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {"@type": "Question", "name": "¿Se puede emitir el REP de una factura de otro sistema?", "acceptedAnswer": {"@type": "Answer", "text": "Sí. Importa el CFDI 4.0 de ingreso, o adjunta el XML al timbrar el complemento de pago. El paso a paso está en la guía Importar CFDIs de otros sistemas."}},
    {"@type": "Question", "name": "¿Qué facturas se pueden importar?", "acceptedAnswer": {"@type": "Answer", "text": "Solo facturas de ingreso CFDI 4.0, PUE o PPD. El complemento de pago solo aplica a PPD. En producción el SAT tiene que reportarlas Vigentes y se validan los dos sellos. Con sk_test_ la consulta al SAT se simula (siempre Vigente) y el sello del SAT no se valida; el sello del emisor sí."}},
    {"@type": "Question", "name": "¿Cuánto cuesta importar?", "acceptedAnswer": {"@type": "Answer", "text": "El 20 % del precio por timbre de tu cuenta por cada XML importado: $0.20 con el precio de lista. Los rechazados y los que ya estaban en la cuenta no se cobran. Si el XML va adjunto al REP, la importación no se cobra: solo el timbre del REP. La API cuesta $1 MXN por timbre, sin suscripción."}},
    {"@type": "Question", "name": "¿Qué es previousPayments y cómo sigo la numeración?", "acceptedAnswer": {"@type": "Answer", "text": "En una factura PPD sirve para declarar si el otro sistema ya emitió complementos, y así el saldo y la numeración continúan. No aplica en una PUE. Los campos y un ejemplo están en la guía Importar CFDIs de otros sistemas."}},
    {"@type": "Question", "name": "¿Puedo adjuntar el XML sin importarlo antes?", "acceptedAnswer": {"@type": "Answer", "text": "Sí. Puedes mandarlo en el mismo complemento de pago. Esa importación no se cobra; solo se cobra el timbre del REP."}},
    {"@type": "Question", "name": "¿Puedo cancelar o editar una factura importada?", "acceptedAnswer": {"@type": "Answer", "text": "No. No se puede cancelar, volver a timbrar ni editar aquí. Se cancela donde se timbró."}}
  ]
}
</script>

## Empieza hoy

1. Entra a [dash.cfdi.express](https://dash.cfdi.express) y prueba con una llave `sk_test_`. En pruebas la consulta al SAT se simula.
2. Sigue [Importar CFDIs de otros sistemas](/docs/importar-cfdis-de-otros-sistemas): importa el XML o pídele a tu agente que use `import_invoices`, y emite el REP.
3. Cuando el flujo esté listo, cambia a `sk_live_`. Ahí sí se consulta al SAT y se valida su sello.

Si quieres revisarlo en una llamada: [agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).

Trae tus facturas de antes. Emite tus REP de hoy.
