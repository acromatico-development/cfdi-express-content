---
title: "Facturas de nómina CFDI: timbra Nómina 1.2 desde la API de CFDI Express"
description: "Timbra facturas de nómina CFDI 4.0 + complemento Nómina 1.2 con POST /v1/nominas. Tú mandas percepciones y el empleado; la API calcula los totales SAT. $1 MXN por timbre."
image: "https://videos.acromatico.dev/api/images/assets/818cfae7-c2b4-4f32-bd6a-617ce0e877a7.png"
author: "Rafael González"
date: "2026-09-12"
keywords: "facturas de nómina, CFDI nómina 1.2, timbrar nómina API, CFDI Express nómina, complemento nómina SAT, recibo de nómina electrónico, API nómina México, percepciones deducciones, registro patronal IMSS, CFDI 4.0 nómina, UsoCFDI CN01, tipoNomina, recibo de nómina API"
---
# Facturas de nómina CFDI: ya se timbran desde la API de CFDI Express

Si ya integraste facturas de ingreso con la [API de CFDI Express](https://cfdi.express/api), conoces el patrón: mandas el negocio, el servidor arma el CFDI 4.0 y el SAT lo sella. Faltaba la pieza que más duele en RH y en un ERP de nómina: el **recibo de nómina electrónico**.

Hoy la API timbra **facturas de nómina**: CFDI 4.0 con `TipoDeComprobante N` + **complemento Nómina 1.2**. Un recibo por empleado por periodo. Tú mandas fechas, empleado, percepciones y deducciones. La API deriva los totales que el SAT exige y fija los valores del comprobante que no puedes “inventar”.

Esto es la **API pública** para cualquier sistema de nómina, RH o ERP — no es un flujo de la app de Shopify.

## Por qué armar el CFDI de nómina a mano se rompe

Un recibo de nómina no es “una factura con otro uso de CFDI”. El SAT cruza el complemento Nómina 1.2 contra el comprobante: gravado y exento, ISR, descuento, subtotal, `CN01`, concepto `84111505`, registro patronal, NSS, régimen 605. Si sumas mal una percepción o dejas `FormaPago` donde no va, el PAC o el SAT te regresan el XML.

Lo que suele doler si lo haces tú:

- **Totales SAT.** `TotalPercepciones`, gravado/exento, `TotalDeducciones`, ISR retenido, `SubTotal`, `Descuento` y `Total` tienen que cuadrar entre sí. Un peso de diferencia tumba el timbre.
- **Campos que el SAT ya decidió.** Tipo de comprobante `N`, método `PUE`, **sin** `FormaPago`, uso `CN01`, un solo concepto `84111505`. Si los pones “como en una factura de ingreso”, fallas.
- **Reglas cruzadas de empleado.** `registroPatronal` es obligatorio en contratos 01–08 y está **prohibido** en 09/10/99 (NOM42/43). El NSS viaja junto con el registro patronal (NOM44). El régimen del receptor en nómina es **siempre 605**; cualquier otro lo rechaza el SAT (NOM11).
- **Catálogos del complemento.** Tipo de contrato, régimen de contratación, periodicidad, banco, riesgo de puesto, tipo de percepción/deducción… no son los catálogos de producto de una factura de venta.
- **Un XML por persona por periodo.** No hay “nómina masiva” en un solo CFDI. Si tu corrida son 400 empleados, son 400 timbres — y 400 oportunidades de duplicar si no hay idempotencia.

La API no te quita la obligación de mandar bien al empleado y las partidas. Te quita el armado SAT: totales, comprobante fijo y validación de claves.

## Qué puedes hacer hoy

Misma cuenta, mismas llaves `sk_test_` / `sk_live_`, mismo emisor (`merchantId` + CSD) que ya usas para facturas.

| Acción | Endpoint |
| --- | --- |
| Timbrar un recibo | `POST /v1/nominas` |
| Listar (cursor, filtros) | `GET /v1/nominas` |
| Consultar uno | `GET /v1/nominas/{id}` |
| Cancelar ante el SAT | `POST /v1/nominas/{id}/cancel` |
| Catálogos Nómina 1.2 | `GET /v1/catalogs/nomina/{catalog}` |

`POST /v1/nominas` espera hasta **20 s** el UUID del SAT y responde **201**. Si la cola está saturada, responde **202** y sigues con `GET /v1/nominas/{id}` (o con un webhook; más abajo). El header **`Idempotency-Key` es obligatorio**: una llave por corrida + empleado. Un replay te devuelve el recibo original (`Idempotency-Replayed: true`); si la misma llave sigue en vuelo, **409**.

La lista filtra por `merchantId`, `status`, `uuid`, `employeeRfc`, rango de `fechaPago` y fechas de alta. Los estatus son `stamping`, `stamped`, `stamp_failed`, `cancel_pending` y `cancelled`.

Cuando el SAT ya selló, el recurso trae UUID, totales derivados y archivos (`xmlUrl`, `pdfUrl`, `zipUrl`) con el mismo esquema de URLs firmadas que las facturas.

## Qué mandas (y qué calcula la API)

Obligatorios en `NominaCreate`: `merchantId`, `fechaPago`, `fechaInicialPago`, `fechaFinalPago`, `numDiasPagados` y `employee`.

Opcionales que sí usa una corrida real:

- `tipoNomina`: **`O`** ordinaria (default) o **`E`** extraordinaria (aguinaldo, PTU, finiquito, …).
- `folio`: texto de hasta 40 caracteres para tu corrida + empleado; el folio numérico interno se asigna igual.
- `registroPatronal`: 11 caracteres (`A` + 10 dígitos). Requerido en contratos 01–08; no lo mandes en 09/10/99.
- Partidas: `percepciones`, `deducciones`, `otrosPagos`, `incapacidades`.
- Bloques SAT cuando aplican: `separacionIndemnizacion` (percepciones 022/023/025), `jubilacionPensionRetiro` (039/044), `entidadSNCF` si eres entidad del SNCF.

**Tú no mandas** `TotalPercepciones`, gravado/exento, `TotalDeducciones`, ISR, `SubTotal`, `Descuento` ni `Total`. La API los deriva de las partidas. Tampoco armas el nodo `Comprobante`: queda en `N`, `PUE`, sin `FormaPago`, `UsoCFDI CN01` y concepto `84111505`.

Cada percepción lleva `tipoPercepcion`, tu `clave`, `concepto`, `importeGravado` e `importeExento`. Las horas extra (`019`) van en `horasExtra`. Cada deducción lleva `tipoDeduccion`, `clave`, `concepto` e `importe` mayor que cero (NOM96). El `total` del recurso es el **neto pagado al empleado**.

## El empleado: como en la Constancia, no “como en el ERP”

`employee` pide exactamente: `rfc`, `name`, `zip`, `curp`, `tipoContrato`, `tipoRegimen`, `numEmpleado`, `periodicidadPago` y `claveEntFed`.

Reglas que evitan el 400 de siempre:

- **`name`**: tal cual la Constancia de Situación Fiscal — mayúsculas, sin acentos.
- **`rfc`**: persona física (13 caracteres). Nómina no se timbra a una moral.
- **`regimenFiscal`**: siempre **605** (Sueldos y Salarios). Si lo omites, la API lo asume; si mandas otro, el SAT lo rechaza (NOM11).
- **`numSeguridadSocial`**: 11 dígitos, junto con `registroPatronal` (NOM44).
- **`antiguedad`**: si no la mandas, se calcula en semanas de `fechaInicioRelLaboral` a `fechaFinalPago` (ej. `P438W`).
- **`banco`**: obligatorio con cuenta de 10/11/16 dígitos; se omite si mandas CLABE de 18.
- Opcionales de catálogo: `tipoJornada`, `riesgoPuesto`, `salarioBaseCotApor`, `salarioDiarioIntegrado`, `departamento`, `puesto`, `subcontratacion`.

El `curpPatron` solo aplica si el emisor es persona física.

## Catálogos Nómina 1.2, sin Excel del SAT

`GET /v1/catalogs/nomina/{catalog}` sirve un catálogo por llamada. Filtro opcional `?keyword=` por clave o nombre.

Catálogos publicados: `tipo-nomina`, `tipo-contrato`, `tipo-jornada`, `tipo-regimen`, `periodicidad-pago`, `riesgo-puesto`, `tipo-percepcion`, `tipo-deduccion`, `tipo-otro-pago`, `tipo-incapacidad`, `tipo-horas`, `origen-recurso`, `estado`, `banco`.

No reutilices `/v1/catalogs/products` para una percepción. Son otros catálogos.

## De cero a primer recibo

1. Cuenta y llave `sk_test_` en [dash.cfdi.express](https://dash.cfdi.express).
2. Emisor con CSD vía `POST /v1/merchants` (o el que ya tienes).
3. Timbrar (abajo: RFC de prueba SAT `CACX7605101P8` / `XOCHILT CASAS CHAVEZ`; el `curp` y el NSS son de ejemplo — usa los de la Constancia):

```bash
curl https://api.cfdi.express/v1/nominas \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: nomina-2026-q18-0042" \
  -H "Content-Type: application/json" \
  -d '{
    "merchantId": "mer_...",
    "tipoNomina": "O",
    "fechaPago": "2026-09-15",
    "fechaInicialPago": "2026-09-01",
    "fechaFinalPago": "2026-09-15",
    "numDiasPagados": 15,
    "registroPatronal": "B5510768108",
    "employee": {
      "rfc": "CACX7605101P8",
      "name": "XOCHILT CASAS CHAVEZ",
      "zip": "36257",
      "curp": "CACX760510MGTSSL09",
      "numSeguridadSocial": "12345678901",
      "tipoContrato": "01",
      "tipoRegimen": "02",
      "numEmpleado": "0042",
      "periodicidadPago": "04",
      "claveEntFed": "GTO"
    },
    "percepciones": [{
      "tipoPercepcion": "001",
      "clave": "SUE",
      "concepto": "Sueldo quincenal",
      "importeGravado": 12000,
      "importeExento": 0
    }],
    "deducciones": [{
      "tipoDeduccion": "002",
      "clave": "ISR",
      "concepto": "ISR retenido",
      "importe": 1450
    }]
  }'
```

4. Si llega **201**, guarda `uuid` y las URLs de XML/PDF/ZIP. Si llega **202**, consulta `GET /v1/nominas/{id}` o espera el webhook.

Los valores de catálogo (`01`, `02`, `04`, `001`, `002`) salen de `GET /v1/catalogs/nomina/...`. No los copies de un Excel viejo.

## Cancelación y webhooks

`POST /v1/nominas/{id}/cancel` también exige `Idempotency-Key`. El body es el mismo `CancelRequest` de facturas: `motivo` 01–04 y `folioSustitucion` cuando el motivo es 01. Respuesta **202** con estatus y acuse en el recurso.

Si no quieres pollar el 202 ni el acuse, suscríbete a `nomina.stamped`, `nomina.stamp_failed` y `nomina.cancelled`. El flujo de endpoints, `whsec_` y `CFDI-Signature` está en [Webhooks CFDI: notificaciones en tiempo real](/blog/webhooks-cfdi-express-api).

## Precio: el mismo saldo, un timbre por recibo

OpenAPI no publica una tarifa aparte para nómina. Un recibo consume el **mismo saldo prepagado** que una factura de ingreso (`GET /v1/balance` expone `pricePerTimbreCentavos` y `timbresRemaining`). Si no hay créditos, `POST /v1/nominas` responde **402**.

En la [landing de la API](https://cfdi.express/api) el precio publicado sigue siendo **$1 MXN por timbre** en producción, con descuento por volumen, sandbox ilimitado con `sk_test_` y recargas de **$100 a $50,000 MXN** por Stripe. Un empleado × un periodo = un timbre. La `Idempotency-Key` evita que un retry te cobre el doble.

## Empieza hoy

1. Abre las [docs interactivas](https://api.cfdi.express/docs) y prueba `POST /v1/nominas` con `sk_test_`.
2. Crea (o entra a) tu cuenta en [dash.cfdi.express](https://dash.cfdi.express).
3. Si tu nómina ya corre en un ERP o un motor de RH, apunta la corrida a un recibo por empleado y deja los totales al servidor.

¿Aún no tienes la API? El contexto está en [Lanzamos CFDI Express API](/blog/lanzamiento-cfdi-express-api). ¿Quieres verlo en una llamada? [Agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).

El SAT no va a simplificar Nómina 1.2. Tu integrador de nómina sí puede dejar de calcularlo a mano.
