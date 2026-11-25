---
title: "Cómo emitir un REP para varias facturas PPD con un solo pago (CFDI 4.0, Pagos 2.0)"
description: "Un pago que cubre varias facturas PPD se timbra como un solo REP (Pagos 2.0) con POST /v1/payments: documents, un timbre y el saldo de cada factura."
image: "https://assets.acromatico.dev/marketing/diego/blog/hero-cfdi-facturas-4.0.png"
author: "Rafael González"
date: "2026-10-30"
keywords: "complemento de pago, REP varias facturas, PPD, Pagos 2.0, CFDI 4.0, POST /v1/payments, DoctoRelacionado, timbrar complemento de pago, saldo insoluto, numParcialidad, monto mayor, unappliedAmount, remanente, referencia del pago, factura de otro PAC, importar CFDI, previousPayments, CFDI Express API"
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
4. **De 1 a 100 documentos.** Cada uno es `{ invoiceId, importePagado, numParcialidad? }`. `invoiceId` es una factura `inv_…` timbrada y PPD. Desde la v1.59.0, en lugar de `invoiceId` puede ir `xml`: el XML de una factura PPD timbrada en otro sistema (ver «Facturas timbradas en otro sistema», más abajo). `importePagado` es el `ImpPagado` de esa factura: mayor a cero y no mayor a su saldo pendiente (tolerancia de 0.005).
5. **Sin repetir factura.** `Invoice X appears more than once in documents`.
6. **Parcialidades por factura.** Cada factura lleva su propio `numParcialidad`. Si lo omites, se asigna. Si lo mandas, se valida contra la secuencia de esa factura. Un REP que relaciona varias facturas cuenta en la secuencia de cada una.
7. **Un timbre por REP.** Cancelar el REP también cuesta un timbre. Al cancelarlo, el saldo pendiente de cada factura recupera lo que ese REP le había aplicado.

`formaPago` es obligatorio: de 2 o 3 dígitos, clave de `c_FormaPago`, y no puede ser `99`. El ejemplo de la documentación es `"03"` (transferencia). El catálogo está en [formas de pago](https://cfdi.express/docs/api/catalogo-formas-pago).

`fechaPago` es opcional, en hora de Ciudad de México: `AAAA-MM-DDTHH:mm:ss`. Si lo omites, es el momento del request.

`monto` es opcional y positivo. Si lo omites, el total es la suma de `documents[].importePagado`. Desde la **v1.58.0**, si lo mandas puede ser **mayor o igual** que esa suma, como permite el SAT (regla CRP20217), con tolerancia de 0.005. Solo si es menor responde 400 `validation_error`.

`Monto` y `MontoTotalPagos` del XML llevan el `monto` que mandas. El saldo de cada factura baja solo por su `importePagado`. Lo que sobra sale en `unappliedAmount`; qué hacer con ello está más abajo, en «El remanente».

`reference` es opcional, desde la v1.58.0: una referencia tuya del pago, de 1 a 100 caracteres y sin `|`, como el id del depósito o de la transacción. Se guarda, el recurso la devuelve y sirve para buscar el REP. No se imprime en el XML.

## Ejemplo: $5.80 = 2.32 + 2.32 + 1.16

Este es el caso sin remanente: `monto` es exactamente la suma de los tres importes. El header `Idempotency-Key` es obligatorio (de 1 a 255 caracteres). Los `invoiceId` de abajo son el ejemplo publicado en la referencia; los importes son los del pago de $5.80.

```bash
curl https://api.cfdi.express/v1/payments \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: payment_88240" \
  -H "Content-Type: application/json" \
  -d '{
    "formaPago": "03",
    "fechaPago": "2026-10-30T12:00:00",
    "monto": 5.80,
    "reference": "DEP-20261030-01",
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
  "unappliedAmount": 0,
  "reference": "DEP-20261030-01",
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

`invoiceId`, `numParcialidad`, `saldoAnterior` y `saldoInsoluto` en la raíz son los de la **primera** factura. `monto` es el total del pago. `unappliedAmount` es `monto` menos la suma de `importePagado`, con 2 decimales: aquí 0, porque no sobra nada. `reference` es la que mandaste, o `null` si no mandaste ninguna. El detalle de cada una está en `documents[]`: `invoiceId`, `numParcialidad`, `importePagado`, `saldoAnterior` y `saldoInsoluto`.

`fechaPago` del request es hora local de Ciudad de México (`2026-10-30T12:00:00`). En el recurso de este ejemplo queda el instante en UTC (`2026-10-30T18:00:00.000Z`).

El mismo `documents[]` viene en el GET, en el [listado](https://cfdi.express/docs/api/listar-complementos-de-pago) y en los webhooks `payment.*`. `GET /v1/payments?invoiceId=` encuentra el REP con cualquiera de sus facturas, no solo con la primera.

Con `reference` lo encuentras por tu propia referencia: `GET /v1/payments?reference=DEP-20261030-01` filtra por coincidencia exacta, sin distinguir mayúsculas. La búsqueda libre `?q=` también la encuentra, siempre que escribas la referencia completa (`?q=dep-20261030-01` sirve; un fragmento no).

En el XML de este ejemplo hay un `Pago` y tres `DoctoRelacionado`. Cada documento usa el UUID de su factura en `IdDocumento`, `ImpPagado` con el `importePagado`, y sus saldos. `EquivalenciaDR` es `"1"`. Los impuestos de cada documento se prorratean desde los de su factura. `ImpuestosP` y `Totales` son la suma, en centavos. Con estas tres facturas de IVA 16% (bases 2.00, 2.00 y 1.00), el `TrasladoP` queda con base **5.00** e IVA **0.80**, y los totales traen `MontoTotalPagos` 5.80, `TotalTrasladosBaseIVA16` 5.00 y `TotalTrasladosImpuestoIVA16` 0.80.

En el CFDI, el primer documento es la primera factura de `documents` y el resto va ordenado por id.

Para probar, una llave `sk_test_` en [dash.cfdi.express](https://dash.cfdi.express). La referencia interactiva está en [api.cfdi.express/docs](https://api.cfdi.express/docs).

## Ejemplo: depósito de $6.00 sobre facturas de $5.80

**Respuesta corta:** desde la v1.58.0 se timbra. Las facturas se liquidan igual y los 0.20 que sobran salen en `unappliedAmount`.

El cliente deposita 6.00 y tú le aplicas 5.80 a las mismas tres facturas:

```bash
curl https://api.cfdi.express/v1/payments \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: payment_88241" \
  -H "Content-Type: application/json" \
  -d '{
    "formaPago": "03",
    "fechaPago": "2026-10-30T12:00:00",
    "monto": 6.00,
    "reference": "DEP-20261030-02",
    "documents": [
      { "invoiceId": "inv7k3q9x2m4v1t8p6d0n5cb", "importePagado": 2.32 },
      { "invoiceId": "inv2p8d4n6c0b5rzmkq2f8v3", "importePagado": 2.32 },
      { "invoiceId": "inv5c0btpay9x2m6q4k1v3p8", "importePagado": 1.16 }
    ]
  }'
```

Responde **201** si el SAT timbra en la misma petición, o **202** si queda en cola, igual que el ejemplo anterior. Fragmento del recurso:

```json
{
  "id": "pay8d4n6c0b5rzmkq2f8v3inv",
  "object": "payment",
  "status": "stamped",
  "monto": 6.00,
  "unappliedAmount": 0.20,
  "reference": "DEP-20261030-02",
  "documents": [
    { "invoiceId": "inv7k3q9x2m4v1t8p6d0n5cb", "importePagado": 2.32, "saldoAnterior": 2.32, "saldoInsoluto": 0 },
    { "invoiceId": "inv2p8d4n6c0b5rzmkq2f8v3", "importePagado": 2.32, "saldoAnterior": 2.32, "saldoInsoluto": 0 },
    { "invoiceId": "inv5c0btpay9x2m6q4k1v3p8", "importePagado": 1.16, "saldoAnterior": 1.16, "saldoInsoluto": 0 }
  ]
}
```

En el XML, `Monto` y `MontoTotalPagos` son 6.00. Los tres `DoctoRelacionado` llevan los mismos `ImpPagado` (2.32, 2.32 y 1.16) y cada factura queda con saldo 0, como en el caso sin remanente. Los impuestos se prorratean por `ImpPagado`, no por `Monto`: `TotalTrasladosBaseIVA16` sigue en 5.00 y `TotalTrasladosImpuestoIVA16` en 0.80.

Lo que no se acepta es lo contrario: `monto` 5.00 contra estos importes responde **400** `validation_error`: `monto 5.00 can't be less than the sum of documents[].importePagado (5.80)`.

## El remanente: `unappliedAmount`

**Respuesta corta:** `unappliedAmount` es solo informativo. No es un saldo a favor del cliente y no se aplica con otro REP del mismo pago.

- **Si el remanente cubre otra factura PPD que ya existe**, inclúyela en `documents` de este mismo REP, con su `importePagado`. No se timbra un segundo REP por la diferencia.
- **Si no**, el remanente se documenta fuera del REP como **anticipo** (Apéndice 6 de la Guía de llenado del Anexo 20, con `TipoRelacion` `07`), y las ventas que se hagan después se facturan como **PUE** si ya están pagadas. La otra salida es devolverle el dinero al cliente.

Cuál aplica lo decide el contador del emisor. El procedimiento del anticipo está en [Facturar anticipos](https://cfdi.express/docs/facturar-anticipos) y, con más detalle, en [Cómo facturar anticipos en CFDI 4.0](/blog/facturar-anticipos-cfdi-4-0).

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
| `monto` menor que la suma | 400 | `monto A can't be less than the sum of documents[].importePagado (B)`. Mayor o igual se acepta |
| `reference` vacía, de más de 100 caracteres o con barra vertical | 400 | Error en el campo `reference`: de 1 a 100 caracteres, sin barra vertical |
| `formaPago` que no existe | 400 | `Unknown forma de pago "{clave}"` |
| `formaPago` `99` | 400 | `A REP requires the real forma de pago (not 99 'Por definir')` |

`Y` en los mensajes de emisor, RFC y moneda es la primera factura del arreglo.

Otros códigos del endpoint: **402** `insufficient_credits` si no hay saldo para el timbre, y **422** si el SAT rechaza el complemento o si se reutiliza la llave de idempotencia.

## Cancelar el REP y cancelar la factura

**Respuesta corta:** cancelar el REP devuelve el saldo de todas sus facturas. Una factura ligada a un REP vigente no se cancela.

`POST /v1/payments/{id}/cancel` usa los mismos motivos 01–04 que una factura. La guía está en [cancelar complemento de pago](https://cfdi.express/docs/api/cancelar-complemento-de-pago). En el ejemplo de $5.80, la cancelación aceptada (respuesta **202**, `cancellation.status` `accepted`) deja los saldos pendientes otra vez en 2.32, 2.32 y 1.16: cada factura recupera su `importePagado`.

Un REP anterior, de una sola factura y sin renglones de documento, recupera el `monto` del pago.

Cancelar una factura PPD que tenga un REP vigente —`stamping`, `stamped` o `cancel_pending`— responde **400** con `subcode` `cancel_reps_first`. Eso incluye las facturas que no son la primera del REP. Primero se cancela el complemento; después, la factura.

## Facturas timbradas en otro sistema

**Respuesta corta:** desde la v1.59.0, una factura PPD timbrada en el portal del SAT o con otro PAC entra al mismo REP. Se importa antes, o su XML va en `documents[].xml`.

- **Importarla antes** con [`POST /v1/invoices/import`](https://cfdi.express/docs/api/importar-facturas). Queda con su `invoiceId` y `origin` `imported`, y se usa como cualquier otra factura en `documents`. Cuesta el 20% del precio del timbre por XML importado.
- **Mandar el XML en el REP**: la entrada de `documents` lleva `xml` (el CFDI 4.0 timbrado, en base64) en lugar de `invoiceId`. Se verifica igual que en el import (sellos, Vigente en el SAT, emisor tuyo) y se importa gratis, solo si el REP se timbra. Si el UUID ya está en tu cuenta, se usa esa factura.

Si la factura es nueva para la cuenta, `previousPayments` es obligatorio: `false` si el otro sistema no emitió REP para ella, o `{ saldoInsoluto, lastNumParcialidad }` del último que emitió. Sin él, la API responde 400 con `subcode` `previous_payments_required`. Con la declaración, el REP sigue la secuencia: parcialidad `lastNumParcialidad` + 1 y saldo anterior `saldoInsoluto`.

```json
{
  "formaPago": "03",
  "fechaPago": "2026-11-25T12:00:00",
  "documents": [
    { "invoiceId": "inv7k3q9x2m4v1t8p6d0n5cb", "importePagado": 2.32 },
    {
      "xml": "PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4...",
      "previousPayments": false,
      "importePagado": 1.16
    }
  ]
}
```

Las reglas de este post (mismo emisor, mismo RFC receptor, misma moneda, saldo, parcialidad) aplican igual. Antes de cada REP sobre una factura importada, CFDI Express vuelve a consultar al SAT que siga Vigente. Una factura importada no se cancela desde CFDI Express.

`POST /v1/payments` acepta cuerpos de hasta 10 MB por los XML. El paso a paso, los errores de `documents[].xml` y las notas de crédito por UUID están en [Importar CFDIs de otros sistemas](https://cfdi.express/docs/importar-cfdis-de-otros-sistemas).

## Desde el servidor MCP

`create_payment` en [el servidor MCP](https://api.cfdi.express/mcp) timbra el mismo REP. Para varias facturas el argumento es `documents` (cada entrada con `invoiceId`, `importePagado` y, si la conoces, `numParcialidad`). `monto` y `fechaPago` siguen siendo opcionales; `formaPago` es el real, no `99`. Como en la API, `monto` puede ser mayor que la suma de los `importePagado`, nunca menor, y el resultado dice cuánto no se aplicó a ninguna factura. `create_payment` también acepta `reference` (1 a 100 caracteres, sin `|`), y `list_payments` busca por ella.

Desde la v1.59.0, una entrada de `documents` puede llevar `xml` en lugar de `invoiceId`: en el MCP va el XML **como texto**, no en base64, con `previousPayments`. Como máximo 5 entradas con `xml` por llamada. Para importar sin timbrar el REP está la herramienta `import_invoices`.

Para una sola factura el argumento es `invoiceId` más `monto`. Las dos formas no se combinan. Si mandas `invoiceId` junto con `documents`, la herramienta responde: `Pass either invoiceId (one invoice) or documents (several invoices), not both.` Si con `documents` pones `numParcialidad` en la raíz, responde: `With documents, set numParcialidad on each document entry instead.`

El REP de varias facturas no tiene una herramienta aparte. `cancel_payment` restaura el saldo de cada factura que el REP liquidaba. El sandbox del MCP está en `https://api.cfdi.express/mcp/test`.

Una instrucción que corresponde a este pago:

```text
Registra un pago con forma de pago 03 del 2026-10-30T12:00:00 que liquida tres facturas PPD: 2.32, 2.32 y 1.16, en un solo REP
```

Y una para el depósito de 6.00 con referencia:

```text
Registra un depósito de 6.00 con referencia DEP-20261030-02, forma de pago 03 del 2026-10-30T12:00:00, que liquida tres facturas PPD por 2.32, 2.32 y 1.16 en un solo REP
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

**¿El monto puede ser mayor que la suma de los importes?**
Sí, desde la v1.58.0. `monto` puede ser mayor o igual que la suma de `documents[].importePagado`; solo si es menor responde 400. `Monto` y `MontoTotalPagos` del XML llevan el `monto` pedido, cada factura baja solo por su `importePagado` y lo que sobra sale en `unappliedAmount`.

**¿Qué hago con el remanente de `unappliedAmount`?**
Es solo informativo: no es un saldo a favor ni se aplica con otro REP del mismo pago. Si cubre otra factura PPD existente, va en el mismo REP. Si no, se documenta fuera del REP como anticipo (Apéndice 6 del Anexo 20, TipoRelacion 07), facturando después las ventas como PUE si ya están pagadas, o se devuelve. El criterio es del contador del emisor.

**¿Puedo guardar mi propia referencia del pago?**
Sí, con `reference` (1 a 100 caracteres, sin `|`). El recurso la devuelve, no se imprime en el XML y se busca con `GET /v1/payments?reference=` (coincidencia exacta) o con `?q=` (la referencia completa, sin distinguir mayúsculas).

**¿Qué pasa si cancelo el REP?**
Cada factura recupera el `importePagado` que ese REP le aplicó. En el ejemplo de $5.80 los saldos vuelven a 2.32, 2.32 y 1.16.

**¿Puedo cancelar una factura que ya está en un REP?**
Si el REP está vigente (`stamping`, `stamped` o `cancel_pending`), la cancelación de la factura responde 400 con `subcode` `cancel_reps_first`, aunque esa factura no sea la primera del REP.

**¿Puedo pedirlo desde un agente?**
Sí. `create_payment` acepta `documents`. No se combina con `invoiceId`. Con `documents`, `numParcialidad` va en cada entrada. También acepta `reference`.

**¿Puedo incluir una factura PPD timbrada en otro PAC o en el portal del SAT?**
Sí, desde la v1.59.0. Impórtala antes con `POST /v1/invoices/import` o manda su XML en base64 en `documents[].xml`, con `previousPayments` (`false` si no hubo REP en el otro sistema, o el saldo y la parcialidad del último). Dentro del REP se importa gratis, solo si el REP se timbra.

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
    {"@type": "Question", "name": "¿El monto puede ser mayor que la suma de los importes?", "acceptedAnswer": {"@type": "Answer", "text": "Sí, desde la v1.58.0. monto puede ser mayor o igual que la suma de documents[].importePagado; solo si es menor responde 400. Monto y MontoTotalPagos del XML llevan el monto pedido, cada factura baja solo por su importePagado y lo que sobra sale en unappliedAmount."}},
    {"@type": "Question", "name": "¿Qué hago con el remanente de unappliedAmount?", "acceptedAnswer": {"@type": "Answer", "text": "Es solo informativo: no es un saldo a favor ni se aplica con otro REP del mismo pago. Si cubre otra factura PPD existente, va en el mismo REP. Si no, se documenta fuera del REP como anticipo (Apéndice 6 del Anexo 20, TipoRelacion 07), facturando después las ventas como PUE si ya están pagadas, o se devuelve. El criterio es del contador del emisor."}},
    {"@type": "Question", "name": "¿Puedo guardar mi propia referencia del pago?", "acceptedAnswer": {"@type": "Answer", "text": "Sí, con reference (1 a 100 caracteres, sin barra vertical). El recurso la devuelve, no se imprime en el XML y se busca con GET /v1/payments?reference= (coincidencia exacta) o con ?q= (la referencia completa, sin distinguir mayúsculas)."}},
    {"@type": "Question", "name": "¿Qué pasa si cancelo el REP?", "acceptedAnswer": {"@type": "Answer", "text": "Cada factura recupera el importePagado que ese REP le aplicó. En el ejemplo de $5.80 los saldos vuelven a 2.32, 2.32 y 1.16."}},
    {"@type": "Question", "name": "¿Puedo cancelar una factura que ya está en un REP?", "acceptedAnswer": {"@type": "Answer", "text": "Si el REP está vigente (stamping, stamped o cancel_pending), la cancelación de la factura responde 400 con subcode cancel_reps_first, aunque esa factura no sea la primera del REP."}},
    {"@type": "Question", "name": "¿Puedo pedirlo desde un agente?", "acceptedAnswer": {"@type": "Answer", "text": "Sí. create_payment acepta documents. No se combina con invoiceId. Con documents, numParcialidad va en cada entrada. También acepta reference."}},
    {"@type": "Question", "name": "¿Puedo incluir una factura PPD timbrada en otro PAC o en el portal del SAT?", "acceptedAnswer": {"@type": "Answer", "text": "Sí, desde la v1.59.0. Impórtala antes con POST /v1/invoices/import o manda su XML en base64 en documents[].xml, con previousPayments (false si no hubo REP en el otro sistema, o el saldo y la parcialidad del último). Dentro del REP se importa gratis, solo si el REP se timbra."}}
  ]
}
</script>

## Timbrar el REP de varias facturas

1. Junta las facturas PPD timbradas del mismo emisor, el mismo RFC y la misma moneda.
2. Manda `POST /v1/payments` con `Idempotency-Key`, `formaPago` y `documents` (`invoiceId` e `importePagado` por factura). Si el depósito fue mayor, manda `monto` con el total recibido; si quieres conciliarlo, agrega `reference`. El ejemplo de $5.80 usa `formaPago` `"03"`.
3. Lee `documents[]`: `saldoAnterior`, `saldoInsoluto` y `numParcialidad` de cada factura. Si `unappliedAmount` es mayor que 0, revisa con tu contador cómo documentar el remanente.
4. Contrasta el cuerpo con [Timbrar un complemento de pago de varias facturas](https://cfdi.express/docs/api/timbrar-complemento-de-pago-varias-facturas).

El contexto de la API está en [el anuncio de lanzamiento](/blog/lanzamiento-cfdi-express-api) y en la [landing](https://cfdi.express/api).

¿Dudas de integración? [Agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).
