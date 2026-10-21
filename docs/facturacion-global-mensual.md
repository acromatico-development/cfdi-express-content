---
title: "Facturación global mensual en Shopify: cómo correrla"
description: "Agrupa las ventas pagadas en MXN sin factura en comprobantes globales de hasta 1,000 órdenes. Planes Plus, Pro y Enterprise, a mano o con Shopify Flow."
keywords: "facturación global mensual, factura global del mes, público en general shopify, XAXX010101000, shopify flow facturación global, CFDI Express"
---
# Facturación global mensual

URL de esta página: https://cfdi.express/docs/facturacion-global-mensual

Desde la versión **3.26.0** de la app, CFDI Express puede juntar las ventas a público en general en **facturas globales de hasta 1,000 órdenes** cada una. La corres a mano desde el menú de la app o la programas con la acción **Facturación global** de Shopify Flow.

El camino de un CFDI por orden (el botón **Facturar al Publico en General** y las plantillas que timbran orden por orden) sigue en [Facturación al público en general](/docs/facturacion-al-publico-en-general-manual-y-automatica). Esta guía es la de la factura global del mes. Qué es ese comprobante en CFDI 4.0 está en [Cómo hacer la factura global del mes en Shopify](/blog/factura-global-del-mes-shopify).

## Requisitos y planes

La facturación global mensual está en los planes **Plus**, **Pro** y **Enterprise**. No está en Standard. Los precios y el cambio de plan están en [Planes y precios](/docs/planes-y-precios).

Antes de lanzarla:

1. La tienda ya tiene que facturar con CFDI Express. Si acabas de instalar, termina el [onboarding](/docs/onboarding): RFC, régimen, código postal y CSD.
2. La app tiene que estar en la **3.26.0** o una posterior. Si no ves **Facturación global** como primer elemento del menú, actualiza la app.
3. El plan de la tienda es Plus, Pro o Enterprise.

## Qué órdenes entran

La corrida solo toma órdenes que cumplan las tres cosas:

- Están **pagadas**.
- Están en **pesos (MXN)**.
- **No tienen factura**.

Una orden en otra moneda, una que sigue sin pagar o una que ya tiene CFDI no entra. La corrida **no duplica**: si la lanzas otra vez, las órdenes que ya quedaron facturadas se quedan fuera.

Cada orden que sí entra guarda el **folio** y el **UUID** de la factura global a la que se sumó. Los ves en la orden, dentro de la app.

Si hay más de 1,000 órdenes que cumplen, salen varias facturas globales. Cada una lleva como máximo 1,000 órdenes.

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

La acción **Crear CFDI** es otra: timbra un CFDI por orden. Las plantillas de fin de día, 3 días, 10 días y fin de mes usan esa acción. El detalle está en [Shopify Flow](/docs/uso-shopify-flow-automatizaciones-cfdi-express) y en [Facturación al público en general](/docs/facturacion-al-publico-en-general-manual-y-automatica).

## El disparador Facturación global terminada

Cuando la corrida termina, CFDI Express dispara **Facturación global terminada** en Shopify Flow. Úsalo como inicio de otro workflow: avisar a contabilidad, etiquetar el cierre o guardar el reporte.

Si algo falla, la app manda un correo de aviso. El listado de correos está en [Notificaciones por correo](/docs/notificaciones-por-correo).

## Órdenes canceladas o reembolsadas

Si una orden que ya entró a la factura global se **cancela** o se **reembolsa**, la app muestra un aviso para que emitas la **nota de crédito**. No la timbra sola.

La nota se hace desde la orden, como cualquier otra devolución. El paso a paso está en [Notas de crédito](/docs/notas-de-credito).

<!-- TODO: captura — aviso de orden cancelada o reembolsada dentro de una factura global, con el camino a la nota de crédito -->
![TODO: Aviso para nota de crédito](TODO-screenshot)
<!-- ENDTODO -->

## Reportes en CSV y Excel

La facturación global mensual genera un reporte en **CSV** y otro en **Excel**. Cada fila es una orden. El desglose de impuestos de la fila trae **IVA al 0%**, **IVA al 8%**, **IVA al 16%** e **IEPS**.

Ese archivo es el de las órdenes de la factura global. El CSV de CFDIs y complementos por rango de fechas sigue en [Reportes](/docs/reportes).

<!-- TODO: captura — descarga del reporte CSV y Excel de la facturación global, con una fila por orden -->
![TODO: Reportes CSV y Excel de la facturación global](TODO-screenshot)
<!-- ENDTODO -->

## Preguntas frecuentes

### ¿En qué planes está?

En Plus, Pro y Enterprise. En Standard no aparece. El cambio de plan está en [Planes y precios](/docs/planes-y-precios).

### ¿Qué órdenes junta?

Solo órdenes pagadas, en MXN y sin factura. No vuelve a facturar una orden que ya tiene folio y UUID.

### ¿Cuántas órdenes caben en cada factura global?

Hasta 1,000. Si hay más, la corrida arma las facturas globales que hagan falta, cada una con ese tope.

### ¿Puedo lanzarla dos veces?

Sí. La segunda corrida no duplica: las órdenes que ya quedaron en una factura global se quedan fuera.

### ¿Sustituye al botón Facturar al Publico en General?

No. Ese botón sigue timbrando un CFDI por orden. La facturación global mensual junta muchas órdenes en un comprobante. Los dos caminos conviven.

### ¿Qué hago si el cliente pide su factura después?

Esa orden ya tiene folio y UUID de la global. Revisa con tu contador si corresponde cancelar o compensar con nota de crédito, y luego timbrar la factura con los datos del cliente. La nota de crédito está en [Notas de crédito](/docs/notas-de-credito) y la cancelación en [Cancelación y acuse](/docs/cancelacion-y-acuse).

### ¿Qué pasa si una orden de la global se reembolsa?

La app te avisa para que emitas la nota de crédito. No la genera en automático.
