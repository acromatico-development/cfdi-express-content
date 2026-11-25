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

El REP es el CFDI con complemento de [recepción de pagos (Pagos 2.0)](http://omawww.sat.gob.mx/tramitesyservicios/Paginas/recepcion_de_pagos.htm). Si un mismo pago cubre varias facturas, el arreglo `documents` está en [Cómo emitir un REP para varias facturas PPD](/blog/emitir-rep-varias-facturas-ppd).

Si aún no tienes cuenta, créala en [dash.cfdi.express](https://dash.cfdi.express). El precio de la API es [$1 MXN por timbre, sin suscripción](https://cfdi.express/api): recargas de $100 a $50,000 MXN y descuentos por volumen. La importación se cobra aparte, como se explica más abajo.

## Qué puedes importar

Solo facturas de ingreso **CFDI 4.0**, PUE o PPD, timbradas en otro sistema. Puedes importar una PUE, pero el complemento de pago solo aplica a facturas **PPD**. El RFC del emisor tiene que ser uno de tus merchants en esa cuenta y en ese modo (`sk_test_` o `sk_live_`).

No se importan CFDI 3.3, notas de crédito, nómina, traslado ni retenciones.

Antes de aceptar cada XML, la API verifica:

- que el XML esté bien formado y sea un CFDI 4.0 de ingreso con TimbreFiscalDigital;
- el sello del emisor y el sello del SAT;
- que el SAT lo reporte como **Vigente**.

Esa consulta al SAT se vuelve a hacer antes de cada REP sobre una factura importada.

## Cómo importar las facturas

Usa `POST /v1/invoices/import`. El cuerpo lleva de 1 a 20 XML, cada uno en **base64** (UTF-8). El `Idempotency-Key` es opcional: si lo mandas y reintentas, recibes el resultado guardado sin volver a consultar al SAT.

```bash
curl https://api.cfdi.express/v1/invoices/import \
  -H "Authorization: Bearer sk_test_..." \
  -H "Content-Type: application/json" \
  -H "Idempotency-Key: import_pedidos_noviembre" \
  -d '{
    "invoices": [
      {
        "xml": "<base64 del XML del CFDI 4.0>",
        "previousPayments": false
      },
      {
        "xml": "<base64 del XML del CFDI 4.0>",
        "previousPayments": {
          "saldoInsoluto": 460.00,
          "lastNumParcialidad": 2
        }
      }
    ]
  }'
```

`previousPayments` es **obligatorio en toda PPD** y no se permite en una PUE:

- `false` si esa factura no tiene REP emitidos en el otro sistema: el saldo queda en el Total y el siguiente complemento es la parcialidad 1.
- `{ "saldoInsoluto", "lastNumParcialidad" }` si ya había REP en el otro sistema: el siguiente complemento usa ese saldo como saldo anterior y `lastNumParcialidad + 1` como número de parcialidad.

Cada XML del lote regresa como `imported`, `already_imported` o `rejected` (con un `reason.code`). Los rechazados y los que ya estaban en la cuenta no se cobran. Si el saldo no alcanza o llegaste al tope mensual, la respuesta es 402 y no se importa nada.

El precio de cada XML importado es el **20 % del precio por timbre de tu cuenta**. No cuenta como un timbre, aunque sí cuenta para el tope mensual de gasto.

Si usas un agente, la herramienta MCP es `import_invoices`: los XML van como **texto** (no en base64) y el máximo es 5 por llamada.

Las facturas importadas no se pueden cancelar, volver a timbrar ni editar aquí. El XML se sirve en `files.xmlUrl`; no se genera PDF.

## Cómo emitir el REP

Hay dos caminos, ambos en [`POST /v1/payments`](/docs/api/timbrar-complemento-de-pago-varias-facturas) (o `POST /v1/payments/preview` si quieres revisar antes, sin timbrar ni cobrar). El header `Idempotency-Key` es obligatorio. La referencia de un solo documento, [`POST /v1/invoices/{id}/payments`](/docs/api/timbrar-complemento-de-pago), es la guía de una factura que ya está en la cuenta.

En `documents[]` mandas **exactamente uno** de estos por factura: `invoiceId` o `xml`.

### 1. Adjuntar el XML al pago

Sirve si aún no importaste la factura. El XML va en base64, igual que en la importación. Adjunto a un REP, **la importación no se cobra**; solo se cobra el timbre del complemento. En una PPD, `previousPayments` también es obligatorio cuando mandas `xml`.

```bash
curl https://api.cfdi.express/v1/payments \
  -H "Authorization: Bearer sk_live_..." \
  -H "Content-Type: application/json" \
  -H "Idempotency-Key: pago_88231" \
  -d '{
    "formaPago": "03",
    "documents": [
      {
        "xml": "<base64 del XML del CFDI 4.0>",
        "previousPayments": {
          "saldoInsoluto": 460.00,
          "lastNumParcialidad": 2
        },
        "importePagado": 460.00
      }
    ]
  }'
```

En el ejemplo de arriba, el siguiente REP queda como parcialidad 3 y el saldo anterior es 460.00. Si no había complementos previos, usa `"previousPayments": false`.

Si `POST /v1/payments` responde 202, el REP sigue en timbrado y la importación queda pendiente hasta que el worker lo resuelva. Consulta `GET /v1/payments/{id}` hasta que `status` sea `stamped`. Puedes revisar el mismo cuerpo, sin escribir nada, en `POST /v1/payments/preview`.

### 2. Usar el id de una factura ya importada

Si ya corriste `POST /v1/invoices/import`, manda el `invoiceId` que te devolvió (`inv_…`). No hace falta volver a adjuntar el XML ni `previousPayments`: la numeración y el saldo siguen desde lo que se guardó al importar.

```bash
curl https://api.cfdi.express/v1/payments \
  -H "Authorization: Bearer sk_live_..." \
  -H "Content-Type: application/json" \
  -H "Idempotency-Key: pago_88232" \
  -d '{
    "formaPago": "03",
    "documents": [
      {
        "invoiceId": "inv_…",
        "importePagado": 460.00
      }
    ]
  }'
```

Por MCP, `create_payment` y `preview_payment` aceptan `documents[].xml` en texto, hasta 5 por llamada.

## Notas de crédito ligadas por UUID

Si ya habías emitido una nota de crédito en CFDI Express con `relatedUuid` apuntando a esa factura, al importarla la nota queda ligada (`relatedInvoiceId`) y cuenta para el tope de crédito.

A la inversa: una nota nueva con `relatedUuid` de una factura que ya está en tu cuenta (importada o timbrada aquí) también se liga y pasa por las mismas reglas que si mandaras `relatedInvoiceId`. Si es de otro emisor o de otra moneda, ahora puede responder 400, cuando antes pasaba.

## Preguntas frecuentes

**¿Se puede emitir el REP de una factura de otro sistema?**
Sí. Importa el CFDI 4.0 de ingreso con `POST /v1/invoices/import`, o adjúntalo en `documents[].xml` al timbrar el REP con `POST /v1/payments`.

**¿Qué facturas se pueden importar?**
Solo facturas de ingreso CFDI 4.0, PUE o PPD, vigentes ante el SAT y con el sello del emisor y el sello del SAT válidos. El complemento de pago solo aplica a PPD.

**¿Cuánto cuesta importar?**
El 20 % del precio por timbre de tu cuenta. Los rechazados y los duplicados no se cobran. Si el XML va adjunto al REP en `documents[].xml`, no se cobra extra: solo el timbre del REP. La API cuesta $1 MXN por timbre, sin mensualidad.

**¿Qué es previousPayments y cómo sigo la numeración?**
`previousPayments` es obligatorio en toda PPD y no se permite en una PUE. `false` si no había REP en el otro sistema: el saldo queda en el Total y el siguiente complemento es la parcialidad 1. Si ya había REP, manda `saldoInsoluto` y `lastNumParcialidad`: el siguiente usa ese saldo como saldo anterior y `lastNumParcialidad + 1` como número de parcialidad.

**¿Puedo adjuntar el XML sin importarlo antes?**
Sí. En `POST /v1/payments`, `documents[].xml` lleva el CFDI en base64. En una PPD, `previousPayments` también va en ese documento.

**¿Puedo cancelar o editar una factura importada?**
No. No se puede cancelar, volver a timbrar ni editar aquí. El XML se sirve en `files.xmlUrl` y no se genera PDF.

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {"@type": "Question", "name": "¿Se puede emitir el REP de una factura de otro sistema?", "acceptedAnswer": {"@type": "Answer", "text": "Sí. Importa el CFDI 4.0 de ingreso con POST /v1/invoices/import, o adjúntalo en documents[].xml al timbrar el REP con POST /v1/payments."}},
    {"@type": "Question", "name": "¿Qué facturas se pueden importar?", "acceptedAnswer": {"@type": "Answer", "text": "Solo facturas de ingreso CFDI 4.0, PUE o PPD, vigentes ante el SAT y con el sello del emisor y el sello del SAT válidos. El complemento de pago solo aplica a PPD."}},
    {"@type": "Question", "name": "¿Cuánto cuesta importar?", "acceptedAnswer": {"@type": "Answer", "text": "El 20 % del precio por timbre de tu cuenta. Los rechazados y los duplicados no se cobran. Si el XML va adjunto al REP en documents[].xml, no se cobra extra: solo el timbre del REP. La API cuesta $1 MXN por timbre, sin mensualidad."}},
    {"@type": "Question", "name": "¿Qué es previousPayments y cómo sigo la numeración?", "acceptedAnswer": {"@type": "Answer", "text": "previousPayments es obligatorio en toda PPD y no se permite en una PUE. false si no había REP en el otro sistema: el saldo queda en el Total y el siguiente complemento es la parcialidad 1. Si ya había REP, manda saldoInsoluto y lastNumParcialidad: el siguiente usa ese saldo como saldo anterior y lastNumParcialidad + 1 como número de parcialidad."}},
    {"@type": "Question", "name": "¿Puedo adjuntar el XML sin importarlo antes?", "acceptedAnswer": {"@type": "Answer", "text": "Sí. En POST /v1/payments, documents[].xml lleva el CFDI en base64. En una PPD, previousPayments también va en ese documento."}},
    {"@type": "Question", "name": "¿Puedo cancelar o editar una factura importada?", "acceptedAnswer": {"@type": "Answer", "text": "No. No se puede cancelar, volver a timbrar ni editar aquí. El XML se sirve en files.xmlUrl y no se genera PDF."}}
  ]
}
</script>

## Empieza hoy

1. Entra a [dash.cfdi.express](https://dash.cfdi.express) y usa una llave `sk_test_` para probar el flujo.
2. Importa uno o varios XML con `POST /v1/invoices/import`, o pide a tu agente que use `import_invoices`.
3. Emite el REP con `POST /v1/payments` y el header `Idempotency-Key` (obligatorio). Puedes revisar antes con `preview`. En PPD, declara `previousPayments` cuando adjuntas el XML. Si responde 202, consulta `GET /v1/payments/{id}` hasta que `status` sea `stamped`.
4. Cuando el flujo esté listo, cambia a `sk_live_`.

La referencia interactiva está en [api.cfdi.express/docs](https://api.cfdi.express/docs). Si quieres revisarlo en una llamada: [agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).

Trae tus facturas de antes. Emite tus REP de hoy.
