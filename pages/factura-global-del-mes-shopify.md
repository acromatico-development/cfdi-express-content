---
title: "Cómo hacer la factura global del mes en Shopify (público en general)"
description: "Qué es la factura global del mes en CFDI 4.0, el RFC de público en general y la periodicidad, y cómo agruparla en Shopify con CFDI Express."
image: "https://videos.acromatico.dev/api/images/assets/e43c88f5-c1d1-4c3e-9fac-aef5329048d2.png"
author: "Rafael González"
date: "2026-10-21"
keywords: "factura global del mes, factura global shopify, público en general, XAXX010101000, periodicidad cfdi 4.0, facturación global mensual, CFDI Express"
---
# Cómo hacer la factura global del mes en Shopify (público en general)

![Factura global del mes en Shopify, ventas a público en general](https://videos.acromatico.dev/api/images/assets/e43c88f5-c1d1-4c3e-9fac-aef5329048d2.png?w=1600)

Cada venta de tu tienda Shopify lleva comprobante, la pida el cliente o no. Las que nadie facturó a su nombre van a **público en general**. En CFDI 4.0 ese comprobante es una factura global: lleva el RFC genérico y el periodo al que corresponden esas ventas.

Desde la versión **3.26.0** de [CFDI Express](https://apps.shopify.com/cfdi-express), en los planes Plus, Pro y Enterprise, puedes juntar esas ventas en **facturas globales de hasta 1,000 órdenes**. La lanzas a mano o la programas con Shopify Flow. El paso a paso está en [Facturación global mensual](/docs/facturacion-global-mensual).

## Qué es la factura global en CFDI 4.0

**Respuesta corta:** es el CFDI de las ventas que no se facturaron a un cliente. Se emite a público en general y declara el periodo de esas ventas.

En el [Anexo 20, versión 4.0](http://omawww.sat.gob.mx/tramitesyservicios/Paginas/anexo_20.htm), la factura a público en general lleva **información global**: periodicidad, mes y año. El marco del comprobante está en [CFDI facturas 4.0](/blog/cfdi-facturas-4-0). El plazo con el que cierras el mes lo ves con tu contador; esta guía no lo sustituye.

El receptor genérico de público en general es este. Es el mismo que ya usa la app cuando facturas una orden suelta a público en general:

| Campo | En la factura a público en general |
| --- | --- |
| RFC | `XAXX010101000` |
| Nombre | `PUBLICO EN GENERAL` |
| Régimen fiscal | `616` (Sin obligaciones fiscales) |
| Uso de CFDI | `S01` (Sin efectos fiscales) |
| Información global | Periodicidad, mes y año del periodo |

`XAXX010101000` es el RFC de público en general. El de operaciones con extranjeros es otro, `XEXX010101000`, y se documenta en [Facturación al extranjero](/docs/facturacion-al-extranjero).

## Periodicidad: la factura es del mes

**Respuesta corta:** la información global pide una periodicidad. La de esta factura es la **mensual** (clave `04`).

El catálogo de periodicidad del CFDI 4.0 también tiene diaria (`01`), semanal (`02`), quincenal (`03`) y bimestral (`05`). En la app de Shopify, esta corrida arma la **factura global del mes**: periodicidad mensual (`04`), con el mes y el año de ese periodo.

Si tu equipo arma el comprobante desde otro sistema y necesita otra periodicidad, eso se timbra por la [API de CFDI Express](https://cfdi.express/api). La referencia está en la [documentación de la API](https://api.cfdi.express/docs).

## Cómo hacerla en Shopify con CFDI Express

**Respuesta corta:** en Plus, Pro o Enterprise abres **Facturación global** (el primer elemento del menú) o programas la acción del mismo nombre en Shopify Flow.

Hay dos formas de correrla. Las dos agrupan las mismas órdenes y respetan el tope de 1,000 por comprobante.

### A mano, desde el menú de la app

1. En el admin abre **Apps → CFDI Express - Facturación**.
2. Entra a **Facturación global**. Es el primer elemento del menú. Si no lo ves, actualiza la app a la 3.26.0 o posterior y confirma que el plan es Plus, Pro o Enterprise. El cambio de plan está en [Planes y precios](/docs/planes-y-precios).
3. Lanza la corrida.
4. Al terminar, cada orden incluida guarda el folio y el UUID de su factura global.

<!-- TODO: captura — primer elemento del menú, Facturación global, y la pantalla para lanzar la corrida -->
<!-- ENDTODO -->

### Programada, con Shopify Flow

1. Abre **Apps → Flow** y crea un workflow.
2. Pon el disparador de horario de Shopify en el momento en que cierras el mes con tu contador.
3. Agrega la acción **Facturación global** de CFDI Express.
4. Activa el workflow.

Cuando la corrida termina, CFDI Express dispara **Facturación global terminada**. Puedes usarlo para avisar a contabilidad. Si algo falla, te llega un correo. Los disparadores y las acciones están en [Shopify Flow](/docs/uso-shopify-flow-automatizaciones-cfdi-express).

<!-- TODO: captura — workflow con el horario de Flow y la acción Facturación global -->
<!-- ENDTODO -->

La acción **Crear CFDI** es distinta: timbra **un CFDI por orden**. Las plantillas de fin de día, 3 días, 10 días y fin de mes usan esa acción. Si eso es lo que buscas, la guía es [Factura global en Shopify](/blog/factura-global-shopify).

## Qué órdenes junta, y cuáles no

**Respuesta corta:** solo órdenes **pagadas**, en **MXN** y **sin factura**. No duplica, y cada orden guarda folio y UUID.

- Una orden en dólares u otra moneda no entra.
- Una orden que el cliente ya facturó no entra.
- Una orden que esta misma corrida ya metió en una factura global tampoco vuelve a entrar. Puedes lanzarla de nuevo: lo ya facturado se queda fuera.
- Si pasan de 1,000, salen varias facturas globales, cada una con hasta 1,000 órdenes.

En el plan Básico (en [Planes y precios](/docs/planes-y-precios) se llama Standard) esta corrida no está. Ahí se sigue facturando al público en general un CFDI por orden, a mano o con la plantilla de Shopify Flow, como hasta ahora. La guía está en [Facturación al público en general](/docs/facturacion-al-publico-en-general-manual-y-automatica).

Las ventas de mostrador también son órdenes de Shopify. Si están pagadas, en MXN y sin factura, entran igual que las de la tienda en línea. Cómo se factura en caja está en [Facturar en Shopify POS](/blog/facturar-shopify-pos).

## Si una orden se cancela o se reembolsa

**Respuesta corta:** la app te avisa para que emitas la nota de crédito. No la timbra sola.

Cuando una orden que ya está en la factura global se cancela o se reembolsa, ves un aviso en la app. La nota de crédito es un CFDI de egreso ligado a esa factura. La haces desde la orden, con el monto de la devolución. El paso a paso está en [Notas de crédito](/docs/notas-de-credito).

Si el cliente pide factura a su nombre después de haber quedado en la global, esa orden ya tiene folio y UUID. Con tu contador defines si cancelas o si compensas con nota de crédito, y después timbras con sus datos fiscales. La cancelación en la app está en [Cancelación y acuse](/docs/cancelacion-y-acuse).

## El reporte: una fila por orden

**Respuesta corta:** al correr la facturación global puedes bajar un **CSV** y un **Excel**. Cada fila es una orden, con IVA al 0%, 8% y 16%, e IEPS.

Sirve para pasarle el cierre a tu contador sin armar el desglose a mano. El CSV de todos los CFDIs por rango de fechas es otro archivo, el de [Reportes](/docs/reportes).

## Preguntas frecuentes

### ¿Shopify hace sola la factura global del mes?

No. Shopify cobra y guarda la orden. El CFDI lo timbra una app. Con CFDI Express la agrupas desde el menú o con la acción **Facturación global** de Shopify Flow.

### ¿Qué RFC lleva el público en general?

`XAXX010101000`, con el nombre `PUBLICO EN GENERAL`. En CFDI 4.0 el comprobante lleva además la información global: periodicidad, mes y año.

### ¿Qué periodicidad usa la factura global del mes?

Mensual, clave `04`. Esta corrida de la app no cambia la periodicidad a diaria, semanal, quincenal o bimestral.

### ¿En qué planes está?

Agrupar en la factura global del mes está en Plus, Pro y Enterprise, desde la versión 3.26.0. En el plan Básico (Standard) se sigue facturando al público en general un CFDI por orden, a mano o con la plantilla de Shopify Flow.

### ¿Se puede facturar dos veces la misma orden?

No. Solo entran órdenes pagadas, en MXN y sin factura. Cada orden que entra guarda el folio y el UUID, y una corrida posterior la deja fuera.

### ¿Qué hago si reembolsé una orden que ya iba en la global?

La app muestra un aviso para que emitas la nota de crédito. No la genera sola. La guía está en [Notas de crédito](/docs/notas-de-credito).

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "¿Shopify hace sola la factura global del mes?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No. Shopify cobra y guarda la orden. El CFDI lo timbra una app. Con CFDI Express la agrupas desde el menú o con la acción Facturación global de Shopify Flow."
      }
    },
    {
      "@type": "Question",
      "name": "¿Qué RFC lleva el público en general?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "XAXX010101000, con el nombre PUBLICO EN GENERAL. En CFDI 4.0 el comprobante lleva además la información global: periodicidad, mes y año."
      }
    },
    {
      "@type": "Question",
      "name": "¿Qué periodicidad usa la factura global del mes?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Mensual, clave 04. Esta corrida de la app no cambia la periodicidad a diaria, semanal, quincenal o bimestral."
      }
    },
    {
      "@type": "Question",
      "name": "¿En qué planes de CFDI Express está la factura global del mes?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Agrupar en la factura global del mes está en Plus, Pro y Enterprise, desde la versión 3.26.0. En el plan Básico (Standard) se sigue facturando al público en general un CFDI por orden, a mano o con la plantilla de Shopify Flow."
      }
    },
    {
      "@type": "Question",
      "name": "¿Se puede facturar dos veces la misma orden en la factura global?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No. Solo entran órdenes pagadas, en MXN y sin factura. Cada orden que entra guarda el folio y el UUID, y una corrida posterior la deja fuera."
      }
    },
    {
      "@type": "Question",
      "name": "¿Qué hago si reembolsé una orden que ya iba en la factura global?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "La app muestra un aviso para que emitas la nota de crédito. No la genera sola."
      }
    }
  ]
}
</script>

## Cierra el mes con la factura global

1. Confirma que la tienda está en Plus, Pro o Enterprise y que la app está en la 3.26.0 o posterior. La guía corta es [Facturación global mensual](/docs/facturacion-global-mensual).
2. Lanza una corrida de prueba desde **Facturación global**, o arma el workflow con la acción del mismo nombre.
3. Baja el CSV o el Excel y revísalo con tu contador. Si una orden se reembolsa después, emite la nota de crédito.

Si facturas un CFDI por orden, con el botón o con la plantilla de Flow, la guía sigue en [Factura global en Shopify](/blog/factura-global-shopify). Si también vendes en tienda física, el cierre de caja está en [Facturar en Shopify POS](/blog/facturar-shopify-pos).

¿Quieres ver la corrida con las órdenes de tu tienda? [Agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express). Instala la app en [Shopify](https://apps.shopify.com/cfdi-express).
