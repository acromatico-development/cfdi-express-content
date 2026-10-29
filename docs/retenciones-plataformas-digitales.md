# CFDI de Retenciones para plataformas digitales vía API

URL de esta página: https://cfdi.express/docs/retenciones-plataformas-digitales

Disponible a partir de la versión **v1.43.0** de la API.

La **constancia de retenciones plataformas tecnológicas** es el CFDI de Retenciones 2.0 que la plataforma emite cada mes a cada vendedor o prestador de servicios. Lleva la clave de retención `26` (`cveRetenc`) y el complemento Servicios de Plataformas Tecnológicas 1.0, estándar Rev. E. Un CFDI por receptor por mes. Esta página cubre cómo crearlo, listarlo, consultarlo y cancelarlo por API.

El OpenAPI vive en https://api.cfdi.express/openapi.json y las docs interactivas en https://api.cfdi.express/docs. La referencia general está en [la API](/docs/api).

## Qué es y quién la emite

El emisor es la plataforma. En la API ese emisor es el comercio (`merchantId`). El receptor es el prestador:

- Persona física: RFC de 13 caracteres.
- Persona moral: RFC de 12 caracteres.
- Sin RFC: `receiver.rfc` = `XAXX010101000`.
- Prestador extranjero: `receiver.rfc` = `XEXX010101000`. En ese caso se omite `receiver.zip`.

`receiver.name` va tal como en la Constancia de Situación Fiscal: mayúsculas y sin acentos. `receiver.zip` (5 dígitos) es obligatorio en receptores nacionales. `receiver.curp` es opcional.

La obligación de emitir esta constancia depende del caso de la plataforma. Confírmala con tu contador antes de timbrar el primer mes.

El RFC `XEXX010101000` de una factura de ingreso usa otros campos (`residenciaFiscal`, `numRegIdTrib`). Ese flujo está en [Facturación al extranjero](/docs/facturacion-al-extranjero). En esta constancia el receptor extranjero lleva el RFC genérico y el nombre, y se omite el código postal.

## Flujo mensual

1. El comercio emisor es la plataforma y tiene CSD. Cómo subirlo está en [Subir CSD](/docs/api/subir-csd).
2. Por cada prestador y cada mes se timbra un CFDI. `ejercicio` es el año y `mes` es el mes (`MesIni` = `MesFin`). `periodicidad` por default es `02` (mensual).
3. El cuerpo manda los servicios de ese periodo, con la comisión de la plataforma en cada uno. La forma de pago `09` no lleva comisión.
4. La API calcula el ISR retenido, el IVA retenido, el IVA trasladado, las exclusiones en efectivo y los totales que valida el SAT.
5. `POST /v1/retenciones` espera hasta 20 segundos. `201` trae el UUID. Con carga extrema responde `202` y el estatus queda en `stamping`: se consulta con `GET /v1/retenciones/{id}`.
6. Cuando `files.status` es `ready`, el recurso trae `pdfUrl` (PDF con QR), `xmlUrl` y `zipUrl`.
7. El webhook `retencion.stamped` avisa que quedó timbrada. `retencion.stamp_failed` avisa que falló.

El header `Idempotency-Key` es obligatorio en el timbrado y en la cancelación. Una llave estable por operación lógica (por ejemplo periodo + id del vendedor) hace que un reintento devuelva la misma respuesta, con `Idempotency-Replayed: true`.

Autenticación: `Authorization: Bearer sk_test_…` o `sk_live_…`. La llave define el modo. En un workspace, el header `x-account-id` indica la cuenta. Ver [Equipo y workspaces](/docs/equipo-y-workspaces).

## Endpoints

| Método | Ruta | Qué hace |
|---|---|---|
| `POST` | `/v1/retenciones` | Timbra la constancia. |
| `GET` | `/v1/retenciones` | Lista las constancias de la cuenta. |
| `GET` | `/v1/retenciones/{id}` | Consulta una constancia. |
| `POST` | `/v1/retenciones/{id}/cancel` | Cancela una constancia timbrada. |
| `GET` | `/v1/catalogs/retenciones/{catalog}` | Catálogos del complemento. |

Base: `https://api.cfdi.express`.

## Crear la constancia

> `POST https://api.cfdi.express/v1/retenciones`

Header `Idempotency-Key` (1 a 255 caracteres), requerido.

### Cuerpo

| Campo | Tipo | Requerido | Descripción |
|---|---|---|---|
| `merchantId` | string | sí | Comercio emisor: la plataforma. |
| `ejercicio` | integer | sí | Año del periodo. De 2021 a 2100. |
| `mes` | integer | sí | Mes del periodo, 1 a 12. |
| `tipoServicio` | string | sí | `c_TipoDeServ`, 2 dígitos. Aplica a todos los servicios del CFDI y define las tasas. Catálogo: `GET /v1/catalogs/retenciones/tipo-servicio`. |
| `receiver` | object | sí | Prestador. Ver abajo. |
| `servicios` | array | sí | Servicios del periodo. De 1 a 2000. |
| `folio` | string | no | `FolioInt` impreso. 1 a 20 letras o dígitos. El esquema trae el ejemplo `202609C0042`. El folio numérico interno se asigna igual. |
| `periodicidad` | string | no | `c_Periodicidad`. Default `02` mensual. También `01` semanal, `03` diario, `04` quincenal, `05` otro. |
| `isrTipoPagoRet` | string | no | ISR de personas físicas: `03` pago definitivo (default) o `04` pago provisional. En personas morales siempre es `04`. |

`tipoServicio` nombrado en el contrato, entre otras claves del catálogo: `05` comercio de bienes, `06` otro tipo de servicios, `07` contenido digital, `08` clubes en línea, `09` enseñanza a distancia.

### Receptor

| Campo | Tipo | Requerido | Descripción |
|---|---|---|---|
| `rfc` | string | sí | RFC del prestador. `XAXX010101000` si no dio RFC. `XEXX010101000` si es extranjero. |
| `name` | string | sí | Nombre o razón social, 1 a 300 caracteres, sin `|`. |
| `zip` | string | nacional | Código postal de 5 dígitos. Se omite con `XEXX010101000`. |
| `curp` | string | no | CURP del prestador. |

### Cada servicio

| Campo | Tipo | Requerido | Descripción |
|---|---|---|---|
| `fecha` | string | sí | `FechaServ`, `YYYY-MM-DD`. Tiene que caer en el mes reportado. |
| `formaPago` | string | sí | `c_FormaPagoServ`, 2 dígitos. |
| `precioSinIva` | number | sí | Precio de la venta o del servicio antes de IVA. Mayor que 0. |
| `comision` | object | salvo `09` | Comisión de la plataforma (`ComisionDelServicio`). |
| `ivaTasa` | number | no | IVA trasladado: `0.16` (default), `0.08` región fronteriza o `0`. Se ignora si `formaPago` es `09`. |
| `subTipo` | string | no | `c_SubTipoServ`. Solo con `tipoServicio` `01`: `01` personal, `02` compartido, `03` plus, `04` cancelación. |
| `rfcTerceroAutorizado` | string | no | RFC de 13 posiciones (`^[A-ZÑ&]{4}\d{6}[A-Z0-9]{3}$`). |
| `contribucionGubernamental` | object | no | `importe` (mayor que 1.00) y `entidad` (`c_EntidadesFederativas` `01`–`32`; `09` es Ciudad de México). |

`formaPago` nombrado en el contrato, entre otras claves del catálogo: `01` efectivo, `02` transferencia, `03` tarjeta de crédito, `06` tarjeta de débito, `08` intermediario de pagos, `09` premios o bonificaciones.

`comision.importe` es obligatorio dentro de `comision` y es lo que la plataforma cobró al prestador por esa venta. `comision.porcentaje` (de `0.001` a `1`) y `comision.base` son opcionales.

### Respuestas del timbrado

| Status | Cuándo |
|---|---|
| `201` | Timbrada. El cuerpo es el recurso `retencion`. |
| `202` | Sigue en `stamping`. Consulta `GET /v1/retenciones/{id}`. |
| `400` | Validación: reglas del SAT o claves de catálogo desconocidas. |
| `402` | Sin créditos o tope mensual alcanzado. |
| `404` | El comercio no existe. |
| `409` | Esa `Idempotency-Key` ya está en curso. |
| `422` | El SAT rechazó el comprobante, o la misma llave se reutilizó con otro cuerpo. |
| `503` | El servicio de timbrado no está disponible. |

<h2 id="que-calcula-la-api">Qué calcula la API</h2>

Tú mandas periodo, receptor, servicios y comisión. La API deriva los importes que el SAT valida: `MontoTotOperacion`, el IVA trasladado de cada servicio, el ISR retenido, el IVA retenido, las exclusiones en efectivo y `DifIVAEntregadoPrestServ`.

Tasas que el contrato publica tal cual. Confírmalas con tu contador; el tipo de servicio y el ejercicio pueden cambiar el caso:

| Caso | Tasa en el contrato |
|---|---|
| Persona moral | ISR retenido del **2.5%**. `isrTipoPagoRet` queda en `04`. |
| Persona física | La tasa de ISR es la de `c_TipoDeServ` para ese `ejercicio`. El contrato no lista esos porcentajes. |
| Sin RFC (`XAXX010101000`) | **20%** de ISR y **100%** de IVA. |
| IVA retenido | **50%** o **100%**. El 100% está explícito para `XAXX010101000`. El contrato no desglosa cuándo aplica el 50%. |
| IVA trasladado | `ivaTasa`: `0.16`, `0.08` o `0`. |
| Exclusiones en efectivo | La API las aplica al calcular. El contrato no publica la fórmula ni un monto de corte. |

`tipoServicio` decide las tasas de ISR e IVA. El catálogo está en `GET /v1/catalogs/retenciones/tipo-servicio`.

El recurso ya timbrado trae el resultado:

| Campo | Qué es |
|---|---|
| `impRetenidos[]` | Cada retención: `impuesto` (`001` ISR, `002` IVA), `baseRet`, `montoRet`, `tipoPagoRet` y `tasa`. |
| `isrRetenido` | ISR retenido. |
| `ivaRetenido` | IVA retenido. |
| `ivaTrasladado` | IVA trasladado. |
| `montoTotOperacion` | `MontoTotOperacion`. |
| `montoTotRet` | Total de retenciones. |
| `comisiones` | Comisiones. |
| `difIvaEntregadoPrestServ` | `DifIVAEntregadoPrestServ`. Puede venir `null`. |
| `currency` | Moneda. |

La `tasa` de cada elemento de `impRetenidos` es la que la API aplicó en ese CFDI.

## Listar y consultar

> `GET https://api.cfdi.express/v1/retenciones`

La lista trae `object` = `list`, `data` (recursos `retencion`) y `has_more`. Paginación por cursor: `limit` (1 a 100, default 10) y `starting_after` con el id del último elemento mientras `has_more` sea `true`. Igual que el resto de la API: [referencia](/docs/api).

Filtros:

| Query | Descripción |
|---|---|
| `merchantId` | Un comercio. |
| `status` | `stamping`, `stamped`, `stamp_failed`, `cancel_pending`, `cancelled`. |
| `uuid` | UUID del CFDI. |
| `receiverRfc` | Un prestador. Con `XEXX010101000` lista a todos los prestadores extranjeros. |
| `ejercicio` | Año reportado. |
| `mes` | Mes reportado, 1 a 12. |
| `createdFrom`, `createdTo` | Fecha-hora de creación. |

> `GET https://api.cfdi.express/v1/retenciones/{id}`

`200` devuelve el recurso. `404` si no existe.

Campos del recurso, además de los totales de arriba: `id`, `object` (`retencion`), `livemode`, `status`, `merchantId`, `uuid`, `serie`, `folio`, `folioText`, `fecha`, `cveRetenc`, `ejercicio`, `mes`, `periodicidad`, `tipoServicio`, `receiver`, `servicios`, `files`, `stampedAt`, `createdAt`.

`files.status`: `pending`, `processing`, `ready` o `failed`. Con archivos listos vienen `xmlUrl`, `pdfUrl` y `zipUrl`. El PDF incluye el QR.

## Cancelar

> `POST https://api.cfdi.express/v1/retenciones/{id}/cancel`

Header `Idempotency-Key`, requerido. El cuerpo es el mismo de [cancelar una factura](/docs/api/cancelar-factura):

| Campo | Tipo | Requerido | Descripción |
|---|---|---|---|
| `motivo` | string | sí | `01` errores con relación (exige `folioSustitucion`) · `02` errores sin relación · `03` la operación no se llevó a cabo · `04` operación nominativa relacionada a una global. |
| `folioSustitucion` | string (uuid) | con `01` | UUID del CFDI que sustituye al cancelado. |

`202` cuando la cancelación quedó procesada: el estatus y el acuse vienen en el recurso. Otros status: `400` validación, `402` sin créditos, `404` no existe, `422` el SAT rechazó la cancelación, `503` servicio de timbrado no disponible.

Estatus posibles del recurso: `stamping`, `stamped`, `stamp_failed`, `cancel_pending`, `cancelled`.

## Webhooks

Tres eventos nuevos. Se suscriben en `events` al crear o actualizar un endpoint (`POST /v1/webhook_endpoints`, `PATCH /v1/webhook_endpoints/{id}`). El catálogo está en `GET /v1/webhook_event_types`; estos tres traen `object` = `retencion`.

| Evento | Cuándo |
|---|---|
| `retencion.stamped` | La constancia quedó timbrada. |
| `retencion.stamp_failed` | El timbrado falló. |
| `retencion.cancelled` | La constancia quedó cancelada. |

Los eventos que ya existían (`invoice.*`, `payment.*`, `nomina.*`) siguen en el mismo catálogo.

## MCP

La v1.43.0 agrega **4 herramientas** al servidor MCP. El total queda en **22**. Esas cuatro cubren este flujo: crear, listar, consultar y cancelar la constancia.

- Producción: https://api.cfdi.express/mcp
- Pruebas: https://api.cfdi.express/mcp/test

Cómo conectar el servidor, OAuth y el workspace: [Servidor MCP](/docs/api/mcp) y [Equipo y workspaces](/docs/equipo-y-workspaces).

## Dashboard

En el [dashboard](https://dash.cfdi.express), Documentos tiene la pestaña **Retenciones**. Ahí se consultan las constancias de la cuenta. Las API keys y las conexiones MCP siguen en el mismo dashboard: [Equipo y workspaces](/docs/equipo-y-workspaces).

## Costo

Timbrar esta constancia cuesta **1 timbre**. Sin saldo, el `POST` responde `402`. El saldo y los timbres restantes se consultan en [Consultar saldo](/docs/api/saldo).

`sk_test_` opera en el modo de pruebas y `sk_live_` en producción. Cada modo tiene su propio saldo.

## Qué sigue igual

Facturas (`/v1/invoices`), notas de crédito, complementos de pago y nómina quedan como estaban. Los webhooks de esos documentos también. Esta versión agrega el recurso `retencion`, sus cuatro operaciones, el catálogo `/v1/catalogs/retenciones/{catalog}` y los tres eventos `retencion.*`.

## Catálogos

> `GET https://api.cfdi.express/v1/catalogs/retenciones/{catalog}`

Un catálogo por llamada. La respuesta es `object` = `list` y `data[]` con `clave` y `nombre`.

| `{catalog}` | Catálogo SAT |
|---|---|
| `tipo-servicio` | `c_TipoDeServ` |
| `subtipo-servicio` | `c_SubTipoServ` |
| `forma-pago-servicio` | `c_FormaPagoServ` |
| `periodicidad` | `c_Periodicidad` |
| `tipo-pago-retencion` | `c_TipoPagoRet` |
| `entidad-federativa` | `c_EntidadesFederativas` |

## Preguntas frecuentes

### ¿Qué es la constancia de retenciones plataformas tecnológicas?

Es el CFDI de Retenciones 2.0 con complemento de Servicios de Plataformas Tecnológicas que la plataforma emite a cada vendedor o prestador, uno por mes, clave de retención `26`.

### ¿La API calcula el ISR y el IVA?

Sí. El request lleva precios sin IVA, forma de pago y comisión. La API calcula ISR retenido, IVA retenido, IVA trasladado, exclusiones en efectivo y totales. Las tasas que publica el contrato están en [Qué calcula la API](#que-calcula-la-api). Confírmalas con tu contador. La tasa aplicada en cada CFDI vuelve en `impRetenidos[].tasa`.

### ¿Cuánto cuesta?

1 timbre por constancia timbrada. Ver [Consultar saldo](/docs/api/saldo).

### ¿Dónde la veo en el dashboard?

En Documentos, pestaña **Retenciones**: https://dash.cfdi.express

### ¿Qué webhooks escucho?

`retencion.stamped`, `retencion.stamp_failed` y `retencion.cancelled`.

### ¿Cambia algo de mi integración de facturas?

Las rutas y los eventos de facturas, notas de crédito, complementos de pago y nómina siguen igual.

## Relacionados

- [Referencia de la API](/docs/api)
- [Consultar saldo](/docs/api/saldo)
- [Subir CSD](/docs/api/subir-csd)
- [Cancelar una factura](/docs/api/cancelar-factura) — mismos `motivo` y `folioSustitucion`
- [Servidor MCP](/docs/api/mcp)
- [Equipo y workspaces](/docs/equipo-y-workspaces)
- [Facturación al extranjero](/docs/facturacion-al-extranjero) — `XEXX010101000` en una factura de ingreso
