---
title: "Cómo emitir el complemento de pago de una factura timbrada en otro sistema"
description: "Importa un CFDI 4.0 de ingreso timbrado fuera de CFDI Express y emite el complemento de pago (REP) pendiente, por API o con tu agente vía MCP."
image: "https://videos.acromatico.dev/api/images/assets/d9faa378-3e95-4068-80ab-4609bfb117b8.png"
author: "Rafael González"
date: "2026-12-02"
keywords: "complemento de pago, REP factura otro sistema, importar CFDI 4.0, importar XML factura, POST /v1/invoices/import, previousPayments, saldo insoluto, factura PPD pendiente, CFDI Express API, import_invoices, MCP complemento de pago"
---
# Cómo emitir el complemento de pago de una factura timbrada en otro sistema

![Personaje 3D con hoodie teal entre facturas genéricas que entran y documentos REP con sello dorado que salen, con el logo de CFDI Express.](https://videos.acromatico.dev/api/images/assets/5a28303c-24bf-4f7e-b34c-9a57a3d2e88a.png)

Si cambiaste de sistema de facturación y te quedaron facturas PPD con complementos de pago (REP) pendientes, ahora puedes traer esos CFDI 4.0 a CFDI Express e emitir los REP desde la API o con tu agente vía MCP.

Esto es la API pública (`https://api.cfdi.express`), no la app de Shopify ni el dashboard. La función salió en la versión 1.59.0. Por ahora no hay carga de XML en el dashboard ni en la app.

Si aún no tienes cuenta, créala en [dash.cfdi.express](https://dash.cfdi.express). El precio de la API es $1 MXN por timbre, sin suscripción; la importación se cobra aparte, como se explica más abajo.

## Qué puedes importar

Solo facturas de ingreso **CFDI 4.0**, PUE o PPD, timbradas en otro sistema. El RFC del emisor tiene que ser uno de tus merchants en esa cuenta y en ese modo (`sk_test_` o `sk_live_`).

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
        "xml": "<XML del CFDI 4.0, en base64>",
        "previousPayments": false
      },
      {
        "xml": "<XML del CFDI 4.0, en base64>",
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

Hay dos caminos, ambos en `POST /v1/payments` (o `POST /v1/payments/preview` si quieres revisar antes, sin timbrar ni cobrar).

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
        "xml": "<XML del CFDI 4.0 PPD, en base64>",
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

Si el timbrado del REP responde 202, la importación queda pendiente hasta que el worker lo resuelva. Puedes revisar el mismo cuerpo, sin escribir nada, en `POST /v1/payments/preview`.

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

## Empieza hoy

1. Entra a [dash.cfdi.express](https://dash.cfdi.express) y usa una llave `sk_test_` para probar el flujo.
2. Importa uno o varios XML con `POST /v1/invoices/import`, o pide a tu agente que use `import_invoices`.
3. Emite el REP con `POST /v1/payments` (o `preview` primero). En PPD, declara `previousPayments` cuando adjuntas el XML.
4. Cuando el flujo esté listo, cambia a `sk_live_`.

La referencia interactiva está en [api.cfdi.express/docs](https://api.cfdi.express/docs). Si quieres revisarlo en una llamada: [agenda una demo](https://cfdi.express) o escribe a hola@cfdi.express.

Trae tus facturas de antes. Emite tus REP de hoy.
