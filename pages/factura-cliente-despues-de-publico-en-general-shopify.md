---
title: "De público en general a factura del cliente en Shopify"
description: "¿Tu cliente pidió factura después de la de público en general? Emite la nota de crédito y vuelve a facturar la orden en Shopify sin cancelar."
image: "https://videos.acromatico.dev/api/images/assets/fa68d41b-d417-45c1-81ca-018b3d1b79ba.jpg"
author: "Rafael González"
date: "2026-11-09"
keywords: "factura a nombre del cliente después de público en general, refacturar público en general, cambiar factura público en general a cliente, volver a facturar, nota de crédito shopify, CFDI Express"
---
# De público en general a factura del cliente en Shopify

![Hero del blog: De público en general a factura del cliente en Shopify, ticket genérico que se convierte en factura nominativa](https://videos.acromatico.dev/api/images/assets/fa68d41b-d417-45c1-81ca-018b3d1b79ba.jpg?w=1600)

El cliente pidió su factura después de que la venta ya estaba a público en general. Desde la app **v3.23** de [CFDI Express](https://apps.shopify.com/cfdi-express) no cancelas la original. Cubres su total con una o varias notas de crédito y oprimes **Volver a facturar**. La factura nueva sale a nombre del cliente, sobre la misma orden.

La original sigue vigente ante el SAT, compensada por sus notas de crédito. No tienes que esperar a que alguien acepte una cancelación. Al generar la nueva, la original pasa de estado `active` a `credited`.

El mapa del resto del flujo está en [cómo facturar pedidos de Shopify automáticamente](/blog/facturar-pedidos-shopify-automaticamente). Cómo se arma cada nota está en [nota de crédito por reembolso en Shopify](/blog/nota-de-credito-reembolso-shopify). La pantalla está en [volver a facturar sin cancelar la original](/docs/notas-de-credito#volver-a-facturar-sin-cancelar-la-original).

## Notas de crédito por el total, y Volver a facturar

**Respuesta corta:** cuando las notas cubren el total de la factura, la página deja capturar los datos fiscales correctos y timbrar otra factura de esa orden.

La nota de crédito de esta pantalla pide un CFDI activo timbrado por CFDI Express. Este botón solo lo dispara la sesión de admin.

1. Abre la orden en la app.
2. Emite **una o varias notas de crédito por el total** de la factura. Puede ser una sola por el total, o varias parciales que juntas lo cubran. El paso de cada nota está en [notas de crédito](/docs/notas-de-credito).
3. En cuanto quedan cubiertas, captura RFC, razón social, régimen, código postal, uso de CFDI y el resto de los datos fiscales del cliente.
4. Oprime **Volver a facturar**. En Modo Flow el botón dice **Volver a facturar (enviar a Flow)**.

La factura original no se cancela. Sigue vigente ante el SAT y queda compensada por sus notas. En la misma página aparece **Facturas Acreditadas**, donde sigues descargando la original y sus notas de crédito.

Las notas se calculan por factura. La factura nueva empieza con su monto completo disponible para una devolución posterior. Para no facturar dos veces el mismo ingreso, las notas de una factura que ya se reemplazó ya no se pueden cancelar.

Este flujo no aplica a una factura PPD que tenga complementos de pago activos.

## Si el pedido ya está en la factura global

**Respuesta corta:** desde la app **3.27.0**, en Plus, Pro y Enterprise, acreditas ese pedido con nota de crédito y emites la factura a nombre del cliente desde la misma página. La global no se cancela.

La factura global junta muchas órdenes. La nota de este caso cubre **solo ese pedido**, no el resto del comprobante.

1. Abre la página del pedido.
2. Emite la nota de crédito por el **pedido completo**. Cada impuesto se acredita con su tasa: IVA al 0%, al 8% y al 16%, e IEPS.
3. Desde esa misma página captura los datos fiscales y genera la factura a nombre del cliente.

Esa factura la emites tú desde la app. Si el cliente intenta facturar ese pedido en la página de gracias, en el formulario de la tienda o en el POS, la app le explica que su compra ya está facturada y que tú puedes emitirle la factura a su nombre.

Desde la versión **3.27.0**, volver a facturar una factura ya acreditada al 100% funciona en la página del pedido. Es el paso que sigue cuando cubriste con nota de crédito un pedido que estaba en la factura global. La acción **Crear Nota de Crédito** de Shopify Flow también acepta estos pedidos. La guía de la corrida está en [facturación global mensual](/docs/facturacion-global-mensual).

En el plan Básico (en [Planes y precios](/docs/planes-y-precios) se llama Standard) no hay factura global. Cada venta a público en general es un CFDI por pedido, y el camino es el de la sección anterior: notas por el total de esa factura y **Volver a facturar**.

## Preguntas frecuentes

### ¿Tengo que cancelar la factura de público en general?

No. Desde la app v3.23 emites una o varias notas de crédito por el total y oprimes **Volver a facturar**. La original sigue vigente ante el SAT, compensada por sus notas, y pasa de `active` a `credited`. La descargas en **Facturas Acreditadas**.

### ¿Puedo cambiar la factura de público en general a nombre del cliente si el pedido ya está en la global?

Sí, en Plus, Pro y Enterprise, desde la app **3.27.0**. La nota de crédito es por ese pedido completo. La factura global sigue válida para los demás. La factura a nombre del cliente la emites tú desde la página del pedido.

### ¿El cliente puede hacer este cambio desde la página de gracias?

No. **Volver a facturar** lo dispara la sesión de admin. Si el pedido ya está en la factura global y el cliente lo intenta en la página de gracias, en el formulario o en el POS, la app le explica que su compra ya está facturada y que tú puedes emitirle la factura a su nombre.

### ¿Qué pasa con el plan Básico?

En el plan Básico no hay factura global. La venta a público en general es un CFDI por pedido. Para pasarla a nombre del cliente, cubres el total de esa factura con notas de crédito y oprimes **Volver a facturar**.

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "De público en general a factura del cliente en Shopify",
  "description": "¿Tu cliente pidió factura después de la de público en general? Emite la nota de crédito y vuelve a facturar la orden en Shopify sin cancelar.",
  "image": "https://videos.acromatico.dev/api/images/assets/fa68d41b-d417-45c1-81ca-018b3d1b79ba.jpg",
  "author": {
    "@type": "Person",
    "name": "Rafael González"
  },
  "publisher": {
    "@type": "Organization",
    "name": "CFDI Express",
    "url": "https://cfdi.express"
  },
  "datePublished": "2026-11-09",
  "dateModified": "2026-11-09",
  "inLanguage": "es-MX",
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "https://cfdi.express/blog/factura-cliente-despues-de-publico-en-general-shopify"
  }
}
</script>

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "¿Tengo que cancelar la factura de público en general?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No. Desde la app v3.23 emites una o varias notas de crédito por el total y oprimes Volver a facturar. La original sigue vigente ante el SAT, compensada por sus notas, y pasa de active a credited. La descargas en Facturas Acreditadas."
      }
    },
    {
      "@type": "Question",
      "name": "¿Puedo cambiar la factura de público en general a nombre del cliente si el pedido ya está en la global?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Sí, en Plus, Pro y Enterprise, desde la app 3.27.0. La nota de crédito es por ese pedido completo. La factura global sigue válida para los demás. La factura a nombre del cliente la emites tú desde la página del pedido."
      }
    },
    {
      "@type": "Question",
      "name": "¿El cliente puede hacer este cambio desde la página de gracias?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No. Volver a facturar lo dispara la sesión de admin. Si el pedido ya está en la factura global y el cliente lo intenta en la página de gracias, en el formulario o en el POS, la app le explica que su compra ya está facturada y que tú puedes emitirle la factura a su nombre."
      }
    },
    {
      "@type": "Question",
      "name": "¿Qué pasa con el plan Básico?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "En el plan Básico no hay factura global. La venta a público en general es un CFDI por pedido. Para pasarla a nombre del cliente, cubres el total de esa factura con notas de crédito y oprimes Volver a facturar."
      }
    }
  ]
}
</script>

## Pasa la siguiente orden a nombre del cliente

1. Confirma que la factura la timbró CFDI Express y que no es una PPD con complementos de pago activos.
2. Emite las notas de crédito por el total. Si el pedido está en la factura global, la nota es por ese pedido completo y el plan es Plus, Pro o Enterprise.
3. Oprime **Volver a facturar** y descarga la original en **Facturas Acreditadas**.

La guía corta está en [notas de crédito](/docs/notas-de-credito). [Instala CFDI Express](https://apps.shopify.com/cfdi-express) en tu tienda. ¿Quieres verlo con una orden de tu tienda? [Agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).
