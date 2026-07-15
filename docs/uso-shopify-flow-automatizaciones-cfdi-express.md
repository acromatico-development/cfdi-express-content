# Uso de Flow para automatizaciones con CFDI Express

Shopify Flow es la herramienta de Shopify que permite construir flujos de trabajo automatizados a partir de **disparadores** (triggers) y **acciones**. CFDI Express se integra con Shopify Flow para que puedas automatizar el timbrado, la cancelación, la facturación al público en general y los reportes, todo sin código.

## Disparadores (triggers) que emite CFDI Express

Estos eventos los dispara el app. Los usas como punto de partida de tus automatizaciones:

- **`cfdi-created`** — se dispara cuando un CFDI es timbrado con éxito (en Modo PAC). Incluye RFC receptor, razón social, régimen, código postal, método/tipo de pago, uso CFDI, correo, UUID, URL de descarga del ZIP, URL de la CSF y dirección completa si se capturó.
- **`cfdi-deleted`** — se dispara cuando un CFDI es cancelado. Incluye la referencia de la orden.
- **`cfdi-flow-mode`** — se dispara cuando llega una solicitud de facturación en [Modo Flow](/docs/modo-flow-cfdi-express-para-usar-tu-propio-pac-de-facturacion). Lleva los datos fiscales del cliente para que tu automatización los reenvíe a tu PAC.
- **`report-generated`** — se dispara cuando un [reporte](/docs/reportes) de CFDIs/complementos termina de generarse. Incluye `startdate`, `enddate`, `reporturl`, `recordcount` y el título del reporte.

## Acciones (actions) que CFDI Express recibe

 Estas acciones aparecen dentro de tus flujos de Flow como pasos que ejecuta CFDI Express:

- **"Crear CFDI"** (`cfdi-create-action`) — timbra un CFDI a partir de una referencia de orden y los datos fiscales que proporciones (RFC, razón social, régimen, código postal, método de pago, uso CFDI, correo y dirección opcional). Devuelve la `download_url` del ZIP.
- **"Eliminar CFDI"** (`cfdi-delete-action`) — cancela un CFDI dado su `order_reference` y un **motivo** de cancelación del SAT (`01`, `02`, `03` o `04`).

## Cómo crear una automatización con Flow

1. Entra a tu Admin de Shopify → **Apps** → **Flow**.
2. Crea un nuevo workflow o importa una de nuestras [plantillas](#plantillas-de-shopify-flow).
3. Selecciona un **disparador** (por ejemplo, el disparador nativo de recurrencia/cron de Flow para ejecutar cada cierto tiempo).
4. Agrega una **acción de búsqueda** de órdenes de Shopify (por ejemplo, "Get order data" o un query de órdenes pagadas y no facturadas).
5. Agrega una **condición** para filtrar las órdenes que quieres timbrar (por ejemplo, que el campo `$app.facturado` sea falso).
6. Agrega una **acción** de CFDI Express ("Crear CFDI") para timbrar las órdenes que cumplan la condición.
7. (Opcional) Encadena un envío de correo, una etiqueta u otra acción de Shopify.

## Facturación automática al Público en General

La automatización más común combina el **disparador de recurrencia** con la **acción "Crear CFDI"** usando los datos del Público en General (`XAXX010101000`), para timbrar de forma periódica las órdenes que no fueron facturadas. Esto se detalla junto con los campos a usar y las plantillas en [Facturación al Público en General](/docs/facturacion-al-publico-en-general-manual-y-automatica).

## Plantillas de Shopify Flow

Hemos prediseñado plantillas `.flow` que puedes importar y usar como base:

- [Plantilla de auto facturación al final del día](https://assets.acromatico.dev/assets/5d6eebfc-9304-4860-8017-2145b2150e43.flow)
- [Plantilla de auto facturación despues de 3 días](https://assets.acromatico.dev/assets/4b95bd41-4ba3-451d-babd-40b632821148.flow)
- [Plantilla de auto facturación despues de 10 días](https://assets.acromatico.dev/assets/a4e4ea57-6d0a-42ad-be1e-b4eeec33d7dc.flow)
- [Plantilla de auto facturación al final del mes](https://assets.acromatico.dev/assets/3ef1789f-05c1-489e-9b12-a0acbfe437f6.flow)

### Cómo importar una plantilla

1. Descarga el archivo `.flow` del enlace de arriba.
2. Entra a Shopify Flow → selecciona tu tienda.
3. Haz clic en **"Importar"** (o arrastra el archivo al editor de workflows).
4. Flow abrirá el workflow en modo edición; ajusta los parámetros del query de búsqueda de órdenes si quieres cambiar el periodo de espera.
5. Activa el workflow.

> Estas plantillas están diseñadas para facturar las órdenes **pagadas y no facturadas** generadas durante el día, hace más de 3 días, hace más de 10 días y al final del mes. Para expandir el tiempo de espera, edita los parámetros del query de búsqueda de órdenes en el editor de Flow. Si requieres una automatización más avanzada o que se integre con otras apps o acciones de Shopify Flow, contáctanos a través de [Acromático Development](https://acromatico.dev).

## Modo Flow: pasar los datos a tu propio PAC

En [Modo Flow](/docs/modo-flow-cfdi-express-para-usar-tu-propio-pac-de-facturacion) usas el disparador `cfdi-flow-mode` como punto de partida. Tu workflow entonces ejecuta una acción HTTP o un conector que reenvía los datos fiscales a la API de tu PAC para que éste timbre el CFDI. CFDI Express se encarga de la recolección; tu PAC del timbrado.

## Cancelación automática

Usa la acción "Eliminar CFDI" con el motivo del SAT:

- `01` — Comprobante emitido con errores con relación.
- `02` — Comprobante emitido sin errores con relación.
- `03` — No se llevó a cabo la operación.
- `04` — Operación nominativa relacionada con la facturación global.