---
title: "Cómo emitir un REP para varias facturas PPD con un solo pago (CFDI 4.0, Pagos 2.0)"
description: "Un pago que cubre varias facturas PPD se timbra como un solo REP (Pagos 2.0) con POST /v1/payments: documents, un timbre y el saldo de cada factura."
image: "https://assets.acromatico.dev/marketing/diego/blog/hero-cfdi-facturas-4.0.png"
author: "Rafael González"
date: "2026-10-30"
keywords: "complemento de pago, REP varias facturas, PPD, Pagos 2.0, CFDI 4.0, POST /v1/payments, DoctoRelacionado, timbrar complemento de pago, saldo insoluto, numParcialidad, CFDI Express API"
---
# Cómo emitir un REP para varias facturas PPD con un solo pago (CFDI 4.0, Pagos 2.0)

Cuando un mismo pago abona varias facturas **PPD**, el SAT admite un solo CFDI de pago (complemento **Pagos 2.0**) con un `DoctoRelacionado` por factura.

Desde la **v1.46.0** de la [API de CFDI Express](https://cfdi.express/api), ese CFDI se timbra con [`POST /v1/payments`](https://cfdi.express/docs/api/timbrar-complemento-de-pago-varias-facturas). El arreglo `documents` lleva cada factura y el `importePagado` que le aplicas. El REP cuesta **un timbre**, no uno por factura.

La guía de complementos en la app está en [Complementos de pago](https://cfdi.express/docs/complementos-de-pago). Este post es el contrato de la API.

## Cuándo usar un solo REP

**Respuesta corta:** cuando un solo pago recibido liquida o abona varias facturas PPD del mismo emisor, el mismo RFC receptor y la misma moneda.

Cada factura ya tiene que estar timbrada, en PPD y con saldo pendiente. En `documents` mandas de **1 a 100** facturas. El caso de este post es el pago que cubre varias.

Si el pago es de una sola factura, [`POST /v1/invoices/{id}/payments`](https://cfdi.express/docs/api/timbrar-complemento-de-pago) sigue igual y ahora pasa por la misma ruta por dentro. `POST /v1/payments` también acepta un solo documento.

## Reglas del pago

Todas se revisan antes de gastar el timbre.

1. **Mismo emisor.** Todas las facturas salen del mismo comercio. Si no: `Invoice X was issued by a different merchant than Y`.
2. **Mismo RFC receptor.** La comparación no distingue mayúsculas. Si no: `Invoice X has a different receiver RFC than Y`.
3. **Misma moneda.** Un REP tiene una sola `MonedaP`. Si mezclas: `Invoice X is in USD; Y is in MXN`. El request no trae tipo de cambio ni `EquivalenciaDR`: en el XML ese atributo va fijo en `"1"`, porque todas las facturas comparten la moneda del pago.
4. **De 1 a 100 documentos.** Cada uno es `{ invoiceId, importePagado, numParcialidad? }`. `invoiceId` es una factura `inv_…` timbrada y PPD. `importePagado` es el `ImpPagado` de esa factura: mayor a cero y no mayor a su saldo pendiente (tolerancia de 0.005).
5. **Sin repetir factura.** `Invoice X appears more than once in documents`.
6. **Parcialidades por factura.** Cada factura lleva su propio `numParcialidad`. Si lo omites, se asigna. Si lo mandas, se valida contra la secuencia de esa factura. Un REP que relaciona varias facturas cuenta en la secuencia de cada una.
7. **Un timbre por REP.** Cancelar el REP también cuesta un timbre. Al cancelarlo, el saldo pendiente de cada factura recupera lo que ese REP le había aplicado.

`formaPago` es obligatorio: de 2 o 3 dígitos, clave de `c_FormaPago`, y no puede ser `99`. El ejemplo de la documentación es `"03"` (transferencia). El catálogo está en [formas de pago](https://cfdi.express/docs/api/catalogo-formas-pago).

`fechaPago` es opcional, en hora de Ciudad de México: `AAAA-MM-DDTHH:mm:ss`. Si lo omites, es el momento del request.

`monto` es opcional y positivo. Si lo mandas, tiene que ser igual a la suma de `documents[].importePagado`, con tolerancia de ±0.005. Si lo omites, el total es esa suma.

## Ejemplo: $5.80 = 2.32 + 2.32 + 1.16

El header `Idempotency-Key` es obligatorio (de 1 a 255 caracteres). Los `invoiceId` de abajo son el ejemplo publicado en la referencia; los importes son los del pago de $5.80.

```bash
curl https://api.cfdi.express/v1/payments \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: payment_88240" \
  -H "Content-Type: application/json" \
  -d '{
    "formaPago": "03",
    "fechaPago": "2026-10-30T12:00:00",
    "monto": 5.80,
    "documents": [
      { "invoiceId": "inv7k3q9x2m4v1t8p6d0n5cb", "importePagado": 2.32 },
      { "invoiceId": "inv2p8d4n6c0b5rzmkq2f8v3", "importePagado": 2.32 },
      { "invoiceId": "inv5c0btpay9x2m6q4k1v3p8", "importePagado": 1.16 }
    ]
  }'
```

`numParcialidad` no va en el ejemplo: el servidor asigna la que sigue en cada factura. Puedes mandarla en la entrada de esa factura si ya la conoces.

Si el SAT timbra en la misma petición, la respuesta es **201**. Si la cola sigue timbrando, es **202**: consultas [`GET /v1/payments/{id}`](https://cfdi.express/docs/api/obtener-complemento-de-pago) hasta que `status` sea `stamped`.

En el ejemplo, `monto` queda en 5.80 y `documents` trae tres entradas. Cada una sale con `numParcialidad` 1 y `saldoInsoluto` 0: el pago cubre el saldo completo de las tres, así que `saldoAnterior` coincide con `importePagado`. Este JSON es el ejemplo publicado en la referencia; los ids, el UUID y las fechas son de muestra:

```json
{
  "id": "pay3v1t8p6d0n5cbinv7k3q9x",
  "object": "payment",
  "livemode": false,
  "status": "stamped",
  "invoiceId": "inv7k3q9x2m4v1t8p6d0n5cb",
  "merchantId": "mkq2f8v3x1t7p9d4n6c0b5rz",
  "uuid": "33d7d158-5f9c-49ef-b1a1-b9c5557efea0",
  "numParcialidad": 1,
  "monto": 5.80,
  "formaPago": "03",
  "fechaPago": "2026-10-30T18:00:00.000Z",
  "saldoAnterior": 2.32,
  "saldoInsoluto": 0,
  "currency": "MXN",
  "documents": [
    {
      "invoiceId": "inv7k3q9x2m4v1t8p6d0n5cb",
      "numParcialidad": 1,
      "importePagado": 2.32,
      "saldoAnterior": 2.32,
      "saldoInsoluto": 0
    },
    {
      "invoiceId": "inv2p8d4n6c0b5rzmkq2f8v3",
      "numParcialidad": 1,
      "importePagado": 2.32,
      "saldoAnterior": 2.32,
      "saldoInsoluto": 0
    },
    {
      "invoiceId": "inv5c0btpay9x2m6q4k1v3p8",
      "numParcialidad": 1,
      "importePagado": 1.16,
      "saldoAnterior": 1.16,
      "saldoInsoluto": 0
    }
  ],
  "files": { "status": "ready" },
  "cancellation": null,
  "stampedAt": "2026-10-30T18:05:12.006Z",
  "createdAt": "2026-10-30T18:05:10.554Z"
}
```

`invoiceId`, `numParcialidad`, `saldoAnterior` y `saldoInsoluto` en la raíz son los de la **primera** factura. `monto` es el total del pago. El detalle de cada una está en `documents[]`: `invoiceId`, `numParcialidad`, `importePagado`, `saldoAnterior` y `saldoInsoluto`.

`fechaPago` del request es hora local de Ciudad de México (`2026-10-30T12:00:00`). En el recurso de este ejemplo queda el instante en UTC (`2026-10-30T18:00:00.000Z`).

El mismo `documents[]` viene en el GET, en el [listado](https://cfdi.express/docs/api/listar-complementos-de-pago) y en los webhooks `payment.*`. `GET /v1/payments?invoiceId=` encuentra el REP con cualquiera de sus facturas, no solo con la primera.

En el XML de este ejemplo hay un `Pago` y tres `DoctoRelacionado`. Cada documento usa el UUID de su factura en `IdDocumento`, `ImpPagado` con el `importePagado`, y sus saldos. `EquivalenciaDR` es `"1"`. Los impuestos de cada documento se prorratean desde los de su factura. `ImpuestosP` y `Totales` son la suma, en centavos. Con estas tres facturas de IVA 16% (bases 2.00, 2.00 y 1.00), el `TrasladoP` queda con base **5.00** e IVA **0.80**, y los totales traen `MontoTotalPagos` 5.80, `TotalTrasladosBaseIVA16` 5.00 y `TotalTrasladosImpuestoIVA16` 0.80.

En el CFDI, el primer documento es la primera factura de `documents` y el resto va ordenado por id.

Mandar `monto` 6 contra estos importes responde **400**: `monto 6.00 must equal the sum of documents[].importePagado (5.80)`.

Para probar, una llave `sk_test_` en [dash.cfdi.express](https://dash.cfdi.express). La referencia interactiva está en [api.cfdi.express/docs](https://api.cfdi.express/docs).

## Cómo se cuentan las parcialidades

**Respuesta corta:** por factura, no por REP.

La siguiente `numParcialidad` de una factura es uno más que sus REP en `stamping` o `stamped`. Un REP de varias facturas entra en la cuenta de cada factura que relaciona.

Si la factura A ya tiene dos REP en `stamping` o `stamped` y la B no tiene ninguno, en el mismo `POST /v1/payments` la parcialidad de A es 3 y la de B es 1. Si mandas otro número, la API responde 400: `Invoice {id}: numParcialidad {n} is out of sequence — expected {N}`.

## Errores de validación

Estas respuestas llegan **antes** de gastar un timbre. Las de 400 usan el código `validation_error`. La factura que no existe es 404 `not_found`. Los textos con prefijo `Invoice {id}:` son los de un REP con más de un documento.

| Situación | HTTP | Qué responde |
| --- | --- | --- |
| Factura repetida en `documents` | 400 | `Invoice X appears more than once in documents` |
| Factura que no existe en la cuenta | 404 `not_found` | `No invoice X` |
| No es PPD | 400 | `Invoice X: Payment complements (REP) only apply to PPD invoices` |
| No está timbrada | 400 | `Invoice X: Invoice is {status} — REPs require a stamped invoice` |
| Saldo pendiente en 0 | 400 | `Invoice X: Invoice is fully paid (saldo pendiente is 0)` |
| `importePagado` mayor al saldo | 400 `overpay` | `Invoice X: Payment of {importe} exceeds the outstanding balance {saldo}`. El cuerpo trae `subcode` `overpay`, `saldoPendiente` e `invoiceId` |
| `numParcialidad` fuera de secuencia | 400 | `expected N` |
| Otro emisor | 400 | `Invoice X was issued by a different merchant than Y` |
| Otro RFC receptor | 400 | `Invoice X has a different receiver RFC than Y` |
| Otra moneda | 400 | `Invoice X is in USD; Y is in MXN` |
| `monto` distinto de la suma | 400 | `monto A must equal the sum of documents[].importePagado (B)` |
| `formaPago` que no existe | 400 | `Unknown forma de pago "{clave}"` |
| `formaPago` `99` | 400 | `A REP requires the real forma de pago (not 99 'Por definir')` |

`Y` en los mensajes de emisor, RFC y moneda es la primera factura del arreglo.

Otros códigos del endpoint: **402** `insufficient_credits` si no hay saldo para el timbre, y **422** si el SAT rechaza el complemento o si se reutiliza la llave de idempotencia.

## Cancelar el REP y cancelar la factura

**Respuesta corta:** cancelar el REP devuelve el saldo de todas sus facturas. Una factura ligada a un REP vigente no se cancela.

`POST /v1/payments/{id}/cancel` usa los mismos motivos 01–04 que una factura. La guía está en [cancelar complemento de pago](https://cfdi.express/docs/api/cancelar-complemento-de-pago). En el ejemplo de $5.80, la cancelación aceptada (respuesta **202**, `cancellation.status` `accepted`) deja los saldos pendientes otra vez en 2.32, 2.32 y 1.16: cada factura recupera su `importePagado`.

Un REP anterior, de una sola factura y sin renglones de documento, recupera el `monto` del pago.

Cancelar una factura PPD que tenga un REP vigente —`stamping`, `stamped` o `cancel_pending`— responde **400** con `subcode` `cancel_reps_first`. Eso incluye las facturas que no son la primera del REP. Primero se cancela el complemento; después, la factura.

## Desde el servidor MCP

`create_payment` en [el servidor MCP](https://api.cfdi.express/mcp) timbra el mismo REP. Para varias facturas el argumento es `documents` (cada entrada con `invoiceId`, `importePagado` y, si la conoces, `numParcialidad`). `monto` y `fechaPago` siguen siendo opcionales; `formaPago` es el real, no `99`.

Para una sola factura el argumento es `invoiceId` más `monto`. Las dos formas no se combinan. Si mandas `invoiceId` junto con `documents`, la herramienta responde: `Pass either invoiceId (one invoice) or documents (several invoices), not both.` Si con `documents` pones `numParcialidad` en la raíz, responde: `With documents, set numParcialidad on each document entry instead.`

El número de herramientas no cambia. `cancel_payment` restaura el saldo de cada factura que el REP liquidaba. El sandbox del MCP está en `https://api.cfdi.express/mcp/test`.

Una instrucción que corresponde a este pago:

```text
Registra un pago con forma de pago 03 del 2026-10-30T12:00:00 que liquida tres facturas PPD: 2.32, 2.32 y 1.16, en un solo REP
```

## Preguntas frecuentes

**¿Cuándo emito un solo REP para varias facturas?**
Cuando un solo pago abona varias facturas PPD del mismo emisor, el mismo RFC receptor y la misma moneda. El endpoint es `POST /v1/payments`, con un `DoctoRelacionado` por factura.

**¿Cuántas facturas acepta?**
De 1 a 100 en `documents`. Cada `invoiceId` va una sola vez. `importePagado` es mayor a cero y no pasa el saldo pendiente de esa factura.

**¿Cuántos timbres cuesta?**
Uno por REP, no uno por factura. Cancelar el REP cuesta otro timbre.

**¿Las parcialidades se numeran por el pago o por la factura?**
Por factura. La siguiente es uno más que los REP de esa factura en `stamping` o `stamped`. Un REP de varias facturas cuenta en cada una. Si omites `numParcialidad`, se asigna; si la mandas y no sigue la secuencia, responde 400 con `expected N`.

**¿Qué saldos trae la respuesta?**
`documents[]` trae, por factura, `numParcialidad`, `importePagado`, `saldoAnterior` y `saldoInsoluto`. En la raíz, `invoiceId`, `numParcialidad`, `saldoAnterior` y `saldoInsoluto` son los de la primera factura, y `monto` es el total.

**¿Qué pasa si cancelo el REP?**
Cada factura recupera el `importePagado` que ese REP le aplicó. En el ejemplo de $5.80 los saldos vuelven a 2.32, 2.32 y 1.16.

**¿Puedo cancelar una factura que ya está en un REP?**
Si el REP está vigente (`stamping`, `stamped` o `cancel_pending`), la cancelación de la factura responde 400 con `subcode` `cancel_reps_first`, aunque esa factura no sea la primera del REP.

**¿Puedo pedirlo desde un agente?**
Sí. `create_payment` acepta `documents`. No se combina con `invoiceId` y `monto`. Con `documents`, `numParcialidad` va en cada entrada.

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {"@type": "Question", "name": "¿Cuándo emito un solo REP para varias facturas?", "acceptedAnswer": {"@type": "Answer", "text": "Cuando un solo pago abona varias facturas PPD del mismo emisor, el mismo RFC receptor y la misma moneda. El endpoint es POST /v1/payments, con un DoctoRelacionado por factura."}},
    {"@type": "Question", "name": "¿Cuántas facturas acepta?", "acceptedAnswer": {"@type": "Answer", "text": "De 1 a 100 en documents. Cada invoiceId va una sola vez. importePagado es mayor a cero y no pasa el saldo pendiente de esa factura."}},
    {"@type": "Question", "name": "¿Cuántos timbres cuesta?", "acceptedAnswer": {"@type": "Answer", "text": "Uno por REP, no uno por factura. Cancelar el REP cuesta otro timbre."}},
    {"@type": "Question", "name": "¿Las parcialidades se numeran por el pago o por la factura?", "acceptedAnswer": {"@type": "Answer", "text": "Por factura. La siguiente es uno más que los REP de esa factura en stamping o stamped. Un REP de varias facturas cuenta en cada una. Si omites numParcialidad, se asigna; si la mandas y no sigue la secuencia, responde 400 con expected N."}},
    {"@type": "Question", "name": "¿Qué saldos trae la respuesta?", "acceptedAnswer": {"@type": "Answer", "text": "documents trae, por factura, numParcialidad, importePagado, saldoAnterior y saldoInsoluto. En la raíz, invoiceId, numParcialidad, saldoAnterior y saldoInsoluto son los de la primera factura, y monto es el total."}},
    {"@type": "Question", "name": "¿Qué pasa si cancelo el REP?", "acceptedAnswer": {"@type": "Answer", "text": "Cada factura recupera el importePagado que ese REP le aplicó. En el ejemplo de $5.80 los saldos vuelven a 2.32, 2.32 y 1.16."}},
    {"@type": "Question", "name": "¿Puedo cancelar una factura que ya está en un REP?", "acceptedAnswer": {"@type": "Answer", "text": "Si el REP está vigente (stamping, stamped o cancel_pending), la cancelación de la factura responde 400 con subcode cancel_reps_first, aunque esa factura no sea la primera del REP."}},
    {"@type": "Question", "name": "¿Puedo pedirlo desde un agente?", "acceptedAnswer": {"@type": "Answer", "text": "Sí. create_payment acepta documents. No se combina con invoiceId y monto. Con documents, numParcialidad va en cada entrada."}}
  ]
}
</script>

## Timbrar el REP de varias facturas

1. Junta las facturas PPD timbradas del mismo emisor, el mismo RFC y la misma moneda.
2. Manda `POST /v1/payments` con `Idempotency-Key`, `formaPago` y `documents` (`invoiceId` e `importePagado` por factura). El ejemplo de $5.80 usa `formaPago` `"03"`.
3. Lee `documents[]`: `saldoAnterior`, `saldoInsoluto` y `numParcialidad` de cada factura.
4. Contrasta el cuerpo con [Timbrar un complemento de pago de varias facturas](https://cfdi.express/docs/api/timbrar-complemento-de-pago-varias-facturas).

El contexto de la API está en [el anuncio de lanzamiento](/blog/lanzamiento-cfdi-express-api) y en la [landing](https://cfdi.express/api).

¿Dudas de integración? [Agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).
