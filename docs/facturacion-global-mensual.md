---
title: "Facturación global mensual en Shopify: cómo correrla"
description: "Agrupa las ventas pagadas sin factura en comprobantes globales de hasta 1,000 órdenes, en MXN o en USD si la tienda factura en dólares. Planes Plus, Pro y Enterprise, a mano o con Shopify Flow."
keywords: "facturación global mensual, factura global del mes, público en general shopify, XAXX010101000, shopify flow facturación global, CFDI Express"
---
# Facturación global mensual

URL de esta página: https://cfdi.express/docs/facturacion-global-mensual

Desde la versión **3.26.0** de la app, CFDI Express puede juntar las ventas a público en general en **facturas globales de hasta 1,000 órdenes** cada una. La corres a mano desde el menú de la app o la programas con la acción **Facturación global** de Shopify Flow.

El camino de un CFDI por orden (el botón **Facturar al Publico en General** y las plantillas que timbran orden por orden) sigue en [Facturación al público en general](/docs/facturacion-al-publico-en-general-manual-y-automatica). Esta guía es la de la factura global del mes. Qué es ese comprobante en CFDI 4.0 está en [Cómo hacer la factura global del mes en Shopify](/blog/factura-global-del-mes-shopify).

Desde la versión **3.28.0** también hay una plantilla de Shopify Flow de factura global diaria. Cada noche agrega a la factura global a público en general las ventas de ayer y las del mes que se pagaron después. El día 1 cierra el mes anterior completo. Está en los mismos planes. El paso a paso de la moneda en el pedido está en [Cómo facturar en dólares desde Shopify](/blog/facturar-en-dolares-shopify).

## Requisitos y planes

Agrupar órdenes en la factura global del mes está en los planes **Plus**, **Pro** y **Enterprise**. El plan **Básico** (en [Planes y precios](/docs/planes-y-precios) se llama **Standard**) sigue facturando al público en general **un CFDI por orden**, a mano o con la plantilla de Shopify Flow, como hasta ahora.

Antes de lanzarla:

1. La tienda ya tiene que facturar con CFDI Express. Si acabas de instalar, termina el [onboarding](/docs/onboarding): RFC, régimen, código postal y CSD.
2. La app tiene que estar en la **3.26.0** o una posterior. Si no ves **Facturación global** como primer elemento del menú, actualiza la app.
3. El plan de la tienda es Plus, Pro o Enterprise.

## Qué órdenes entran

La corrida solo toma órdenes **pagadas** y **sin factura**, en la moneda que la tienda factura:

- En **pesos (MXN)** entran como hasta ahora. Esa factura global sale en MXN.
- Las órdenes **cobradas en dólares** entran cuando la tienda ya factura en USD: la moneda de la tienda es el dólar, o activaste **Facturar en dólares (USD) las órdenes cobradas en dólares** en [Configuraciones](/docs/configuraciones). Esa factura global sale en USD, con el tipo de cambio FIX de Banxico de la fecha del comprobante.
- Pesos y dólares van en comprobantes distintos. Cada factura global lleva una sola moneda.

Una orden en otra moneda, una que sigue sin pagar o una que ya tiene CFDI no entra. La corrida **no duplica**: si la lanzas otra vez, las órdenes que ya quedaron facturadas se quedan fuera.

Cada orden que sí entra guarda el **folio** y el **UUID** de la factura global a la que se sumó. Los ves en la orden, dentro de la app.

Si hay más de 1,000 órdenes que cumplen, salen varias facturas globales. Cada una lleva como máximo 1,000 órdenes.

## Moneda en la app y tipo de cambio por API

En la app, la corrida junta las órdenes de la sección anterior: pesos, y dólares cuando la tienda factura en USD. En la página del pedido puedes capturar otro tipo de cambio y ver la vista previa antes de timbrar una factura nominativa. Eso está en [Cómo facturar en dólares desde Shopify](/blog/facturar-en-dolares-shopify).

La factura global que se timbra por API (`POST /v1/invoices` con `informacionGlobal`) acepta `currency` y `exchangeRate` desde la **v1.49.0**, con el mismo contrato que cualquier factura. Los importes van en esa moneda. `exchangeRate` son los pesos mexicanos por 1 unidad, con máximo 6 decimales, de la fecha del comprobante. Si la moneda no es MXN, el campo es obligatorio; con MXN se omite o va en `1`. Para USD se usa el FIX de Banxico publicado en el Diario Oficial de la Federación. La referencia está en [Timbrar una factura](/docs/api/timbrar-factura). Otras monedas de esa lista se timbran por API: la corrida de la app no las junta.

## Lanzarla a mano

1. En el admin de Shopify abre **Apps → CFDI Express - Facturación**.
2. Abre **Facturación global**. Es el primer elemento del menú de la app.
3. Lanza la corrida desde esa pantalla.
4. Al terminar, revisa las órdenes incluidas. Cada una queda con su folio y su UUID.

<!-- TODO: captura — menú de la app con Facturación global como primer elemento, y la pantalla para lanzar la corrida a mano -->
![TODO: Menú y pantalla de Facturación global](TODO-screenshot)
<!-- ENDTODO -->

Sirve para cerrar el mes cuando tú decides el momento. Si quieres que corra sola, prográmalo en Flow.

## Programarla con Shopify Flow

La acción se llama **Facturación global**. No le pases una orden: junta las que cumplen las condiciones de arriba.

1. En el admin abre **Apps → Flow**.
2. Crea un workflow.
3. Elige el disparador de horario de Shopify (el de recurrencia) y ponlo en el momento en que cierras el mes con tu contador. Por ejemplo, al final del mes o al empezar el siguiente.
4. Agrega la acción **Facturación global** de CFDI Express.
5. Activa el workflow.

<!-- TODO: captura — workflow de Shopify Flow con el disparador de horario y la acción Facturación global -->
![TODO: Acción Facturación global en Shopify Flow](TODO-screenshot)
<!-- ENDTODO -->

La acción **Crear CFDI** es otra: timbra un CFDI por orden. Las plantillas de fin de día, 3 días, 10 días y fin de mes usan esa acción.

Desde la versión **3.28.0** hay una plantilla distinta, de **factura global diaria**. Usa esta acción, **Facturación global**. Cada noche agrega a la factura global a público en general las ventas de ayer y las del mes que se pagaron después. El día 1 cierra el mes anterior completo. Actívala en Shopify Flow. Está en Plus, Pro y Enterprise. En el plan Básico se sigue facturando al público en general un CFDI por orden.

El detalle de las plantillas está en [Shopify Flow](/docs/uso-shopify-flow-automatizaciones-cfdi-express) y en [Facturación al público en general](/docs/facturacion-al-publico-en-general-manual-y-automatica).

## El disparador Facturación global terminada

Cuando la corrida termina, CFDI Express dispara **Facturación global terminada** en Shopify Flow. Úsalo como inicio de otro workflow: avisar a contabilidad, etiquetar el cierre o guardar el reporte.

Desde la versión **3.30.0**, el disparador también trae las facturas que se timbraron: UUID, serie y folio, total, número de órdenes y la liga al ZIP con el PDF y el XML. El paso a paso para mandárselas a tu contador está en [Cómo mandarle a tu contador las facturas globales con Shopify Flow](/docs/enviar-facturas-globales-al-contador-shopify-flow).

Si algo falla, la app manda un correo de aviso. El listado de correos está en [Notificaciones por correo](/docs/notificaciones-por-correo).

## Nota de crédito de un pedido de la global

Desde la versión **3.27.0**, si un pedido que ya está en la factura global se devuelve o se reembolsa, emites la nota de crédito desde la página de ese pedido. La app no la timbra sola.

Puede ser por el pedido completo o por una parte. Cada impuesto se acredita con su propia tasa: **IVA al 0%**, **IVA al 8%**, **IVA al 16%** e **IEPS**.

La nota acredita solo ese pedido. La factura global no se cancela: sigue válida para los demás. Si esa factura global es en dólares, la nota sale en dólares, con el tipo de cambio de la fecha de la nota.

La acción **Crear Nota de Crédito** de Shopify Flow también acepta estos pedidos. El paso a paso está en [Notas de crédito](/docs/notas-de-credito).

<!-- TODO: captura — nota de crédito parcial o total de un pedido que ya está en la factura global, emitida desde la página del pedido -->
![TODO: Nota de crédito de un pedido de la factura global](TODO-screenshot)
<!-- ENDTODO -->

## Si el cliente pide su factura después

Ese pedido ya tiene el folio y el UUID de la global. Desde la página del pedido:

1. Emite la nota de crédito por el **pedido completo**.
2. Genera la factura a nombre del cliente en la misma página.

La factura a su nombre la emites tú desde la app. Si el cliente intenta facturar ese pedido en la página de gracias, en el formulario de la tienda o en el POS, la app le explica que su compra ya está facturada y que tú puedes emitirle la factura a su nombre.

Esto es de Plus, Pro y Enterprise, que son los planes con factura global. En el plan Básico cada venta a público en general sigue siendo un CFDI por pedido.

<!-- TODO: captura — página del pedido en la global: nota de crédito por el pedido completo y factura a nombre del cliente en la misma página -->
![TODO: Factura a nombre del cliente después de la global](TODO-screenshot)
<!-- ENDTODO -->

## Cancelar una factura global

Desde la versión **3.29.0** puedes cancelar una factura global, o todas las de una corrida, con un clic desde la app. Igual que la factura global, está en los planes **Plus**, **Pro** y **Enterprise**.

La app cancela siempre con el **motivo 02** del SAT (comprobante emitido con errores sin relación). El motivo no se elige.

1. En el admin de Shopify abre **Apps → CFDI Express - Facturación → Facturación global**.
2. Para una sola factura, ábrela y da clic en **Cancelar factura global**. Para toda una corrida, da clic en **Cancelar corrida** en la lista de corridas.
3. Revisa lo que se va a cancelar: el folio, las órdenes y el total de cada factura, y los timbres que se van a usar.
4. Confirma.

Qué pasa después:

- La cancelación corre en segundo plano. Las órdenes siguen facturadas hasta que el SAT la acepta.
- Cuando el SAT la acepta, las órdenes quedan otra vez **sin facturar**. Vuelve a correr la factura global del periodo para incluirlas en una nueva.
- Shopify tarda unos minutos en actualizar su búsqueda. Si una corrida lanzada enseguida no toma todas las órdenes, la siguiente corrida del periodo toma las que falten, sin duplicar.
- Se usa **1 timbre por factura cancelada**, no por orden.
- Al terminar te mandamos un correo con el resumen.
- Si el SAT tarda en responder, la app vuelve a revisar hasta por 72 horas. Si la cancelación no termina, la factura muestra el error y el botón **Reintentar**.

Cuándo no se puede cancelar:

- Si alguna orden de la factura tiene una [nota de crédito](/docs/notas-de-credito) vigente. Cancela primero esa nota. Si la orden ya se volvió a facturar a nombre del cliente, su nota no se puede cancelar y la factura global tampoco: corrígela con notas de crédito por orden.
- Una corrida se cancela completa o no se cancela. Si una de sus facturas está bloqueada, no se cancela ninguna.
- Mientras hay una corrida de facturación global en curso. Espera a que termine.

La cancelación de una factura global se hace desde la app.

<!-- TODO: captura — botón Cancelar factura global y ventana de confirmación con folio, órdenes, total y timbres -->
![TODO: Cancelar una factura global](TODO-screenshot)
<!-- ENDTODO -->

## Reportes en CSV y Excel

La facturación global mensual genera un reporte en **CSV** y otro en **Excel**. Cada fila es una orden. El desglose de impuestos de la fila trae **IVA al 0%**, **IVA al 8%**, **IVA al 16%** e **IEPS**.

Ese archivo es el de las órdenes de la factura global. El CSV de CFDIs y complementos por rango de fechas sigue en [Reportes](/docs/reportes).

<!-- TODO: captura — descarga del reporte CSV y Excel de la facturación global, con una fila por orden -->
![TODO: Reportes CSV y Excel de la facturación global](TODO-screenshot)
<!-- ENDTODO -->

## Preguntas frecuentes

### ¿En qué planes está?

Agrupar órdenes está en Plus, Pro y Enterprise. En el plan Básico (Standard) esa corrida no aparece: ahí el público en general sigue siendo un CFDI por orden, con el botón **Facturar al Publico en General** y con la plantilla de Shopify Flow. El cambio de plan está en [Planes y precios](/docs/planes-y-precios).

### ¿Qué órdenes junta?

Órdenes pagadas y sin factura. En pesos entran como hasta ahora. Las cobradas en dólares entran cuando la tienda factura en USD, y esa factura global sale en USD. Una orden en otra moneda no entra. No vuelve a facturar una orden que ya tiene folio y UUID.

### ¿Cuántas órdenes caben en cada factura global?

Hasta 1,000. Si hay más, la corrida arma las facturas globales que hagan falta, cada una con ese tope.

### ¿Puedo lanzarla dos veces?

Sí. La segunda corrida no duplica: las órdenes que ya quedaron en una factura global se quedan fuera.

### ¿Sustituye al botón Facturar al Publico en General?

No. Ese botón sigue timbrando un CFDI por orden. La facturación global mensual junta muchas órdenes en un comprobante. Los dos caminos conviven.

### ¿Qué hago si el cliente pide su factura después?

Desde la versión **3.27.0**, en la página del pedido emites la nota de crédito por el pedido completo y luego la factura a su nombre. La factura global sigue válida para los demás. Esa factura nominativa la emites tú desde la app. La guía está en [Notas de crédito](/docs/notas-de-credito).

### ¿Puedo cancelar una factura global?

Sí, desde la versión **3.29.0**: una factura o una corrida completa, desde la pantalla **Facturación global** de la app. Se cancela ante el SAT con motivo 02 y, cuando el SAT la acepta, las órdenes quedan libres para otra factura global. Usa 1 timbre por factura cancelada. No se puede si alguna orden tiene una nota de crédito vigente.

### ¿Qué pasa si una orden de la global se reembolsa?

Emites la nota de crédito desde la página de ese pedido, por el total o por una parte. Cada impuesto (IVA 0%, 8% y 16%, e IEPS) se acredita con su tasa. La nota cubre solo ese pedido; la global sigue válida. La app no la genera sola.
