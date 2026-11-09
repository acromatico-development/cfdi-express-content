---
title: "Cómo facturar pedidos de Shopify automáticamente (CFDI 4.0)"
description: "Factura cada pedido de Shopify en automático: autofactura en la página de gracias, envío por correo y WhatsApp, factura global con Flow y POS."
image: "https://videos.acromatico.dev/api/images/assets/aab48b2d-67f6-43e0-ac2c-09264025b30f.jpg"
author: "Rafael González"
date: "2026-11-09"
keywords: "facturar pedidos shopify, facturación automática shopify, cfdi shopify, autofactura shopify, factura global shopify, CFDI 4.0, CFDI Express"
---
# Cómo facturar pedidos de Shopify automáticamente (CFDI 4.0)

![Hero del blog: Cómo facturar pedidos de Shopify automáticamente (CFDI 4.0), pedidos que se convierten en facturas por correo y chat](https://videos.acromatico.dev/api/images/assets/aab48b2d-67f6-43e0-ac2c-09264025b30f.jpg?w=1600)

Facturar pedidos de Shopify es el recorrido de esta guía. Shopify cobra y guarda la orden. El CFDI 4.0 lo timbra [CFDI Express](https://apps.shopify.com/cfdi-express): el cliente se autofactura, tú la emites desde el admin o el POS, o Shopify Flow cierra a público en general lo que nadie facturó.

La facturación automática en Shopify cabe en seis momentos. Cada uno tiene su pantalla en la app.

| Momento | Dónde ocurre | Quién la emite |
| --- | --- | --- |
| El cliente acaba de pagar | Página de gracias del checkout | El cliente, con autofactura |
| El cliente vuelve después | Portal de cuentas y página de estado de la orden | El cliente |
| Nadie pidió factura | Factura global con Shopify Flow, o un CFDI por orden | Tú o la automatización |
| La venta fue en caja | Shopify POS | El cajero |
| Hay un reembolso | Nota de crédito ligada a la factura original | Tú, o la acción de Flow |
| Pidió factura después de público en general | Notas de crédito y **Volver a facturar** | Tú, desde el admin |

## Autofactura en la página de gracias

**Respuesta corta:** al terminar el pago, el cliente llena sus datos fiscales en la página de gracias y la app timbra el CFDI de esa orden.

El bloque va en la Thank You Page del checkout publicado. El paso a paso está en [facturación desde la página de gracias](/docs/facturacion-checkout-thank-you-page).

El formulario pide razón social, RFC, régimen fiscal, código postal, método de pago, uso de CFDI y correo. El teléfono de WhatsApp es opcional. Si ya facturó antes en tu tienda, razón social, RFC, régimen y código postal pueden aparecer precargados. La orden tiene que estar pagada. Si tu periodo de facturación ya venció, el formulario se lo dice y tú puedes facturar esa orden desde el admin.

En [Configuraciones](/docs/configuraciones) eliges hasta cuándo puede autofacturarse: sin límite, hasta el fin del mes de la compra, o un número de días después de la compra.

## Portal de autofacturación

**Respuesta corta:** el mismo formulario vive en la cuenta del cliente, en la página de estado de la orden. No hace falta un portal aparte.

Shopify concentra las órdenes de todos los canales. Una venta de la tienda en línea o del POS aparece en esa cuenta. La guía del bloque está en [facturación en la página de estado de orden](/docs/facturacion-pagina-estado-de-orden). Cómo queda armado para el cliente está en el [portal de autofacturación](/blog/portal-de-autofacturacion).

Si quieres una página de la tienda donde el cliente busque su orden por número y total, el bloque es el [formulario de facturación](/docs/formulario-de-facturacion-theme-block). La búsqueda no distingue canal, así que también sirve para ventas de POS.

Lo que ve el cliente depende de la orden:

- Pagada y sin factura: ve el formulario.
- Ya facturada a su nombre: ve el enlace para descargar el CFDI. No se timbra otra.
- Ya facturada a público en general en un CFDI de esa sola orden: la app le dice que el comercio cerró esa orden. Tú puedes emitir la factura a su nombre con el camino de [cambiar la factura de público en general a nombre del cliente](/blog/factura-cliente-despues-de-publico-en-general-shopify).
- Ya incluida en la factura global del mes (Plus, Pro y Enterprise, desde la app **3.27.0**): la página de gracias, el formulario y el POS le explican que su compra ya está facturada y que tú puedes emitirle la factura a su nombre. En el plan Básico no hay factura global.

## Correo y WhatsApp

**Respuesta corta:** al timbrar, el cliente recibe el PDF y el XML por correo. Si capturas el teléfono, también le llega un WhatsApp con el folio, el nombre de tu tienda y la liga de descarga.

El correo lleva el logo de tu negocio y un ZIP con el PDF y el XML. Lo puedes apagar en [Configuraciones](/docs/configuraciones), con `skipCfdiEmail`. El detalle de los mensajes está en [notificaciones por correo](/docs/notificaciones-por-correo).

El campo **Teléfono (WhatsApp, opcional)** está en el admin y en la autofactura de la página de gracias y de estado de orden. El número va a 10 dígitos, o con lada. Si el número no es válido, la factura se timbra igual y solo se omite el WhatsApp. Para no enviarlo, activa **Omitir envío de WhatsApp con CFDI**. La pantalla está en [facturación desde el admin](/docs/facturacion-desde-admin-shopify).

El logo que cargas en Configuraciones es el que sale en el CFDI y en esos correos.

## Factura global de fin de mes con Shopify Flow

**Respuesta corta:** en Plus, Pro y Enterprise, la acción **Facturación global** junta las órdenes pagadas y sin factura en comprobantes de hasta 1,000 órdenes. Ninguna orden entra dos veces.

La programas con el disparador de horario de Shopify Flow, en el momento en que cierras el mes con tu contador. También puedes lanzarla a mano en **Facturación global**, el primer elemento del menú, desde la app **3.26.0**. Al terminar, cada orden guarda el folio y el UUID de su factura global. Si la corres otra vez, lo ya facturado se queda fuera. Si hay más de 1,000 órdenes, salen varios comprobantes, cada uno con ese tope.

Entran órdenes pagadas y sin factura. En pesos salen en MXN. Las cobradas en dólares entran cuando la tienda factura en USD, y esa factura global sale en USD. El paso de la moneda está en [cómo facturar en dólares desde Shopify](/blog/facturar-en-dolares-shopify).

Desde la versión **3.28.0** hay una plantilla de factura global diaria, en los mismos planes. Cada noche agrega las ventas de ayer y las del mes que se pagaron después. El día 1 cierra el mes anterior completo.

En el plan Básico (en [Planes y precios](/docs/planes-y-precios) se llama Standard) esta corrida no está. Ahí el público en general sigue siendo un CFDI por orden: el botón **Facturar al Publico en General**, o las plantillas de Flow de fin de día, 3 días, 10 días y fin de mes. Esas plantillas usan la acción **Crear CFDI**, no **Facturación global**.

La guía de la corrida está en [facturación global mensual](/docs/facturacion-global-mensual). Qué lleva el comprobante de público en general está en [la factura global del mes en Shopify](/blog/factura-global-del-mes-shopify). El camino de un CFDI por orden está en [factura global en Shopify](/blog/factura-global-shopify). Las acciones y las plantillas están en [Shopify Flow](/docs/uso-shopify-flow-automatizaciones-cfdi-express).

## Facturar pedidos desde Shopify POS

**Respuesta corta:** el cajero timbra en la caja, con **CFDI / Factura**, o días después desde la orden, con **CFDI / Factura (Órdenes)**.

La factura del POS sale en pago en una sola exhibición (`PUE`). Los conceptos salen de la orden. El cliente recibe PDF y XML por correo, salvo que hayas apagado ese envío. Si la orden ya tiene factura, el cajero ve folio, fecha y receptor.

Las ventas de mostrador que nadie facturó son órdenes de Shopify. Si están pagadas y sin factura, entran a la factura global igual que las de la tienda en línea, en Plus, Pro y Enterprise. En el plan Básico cada una es un CFDI a público en general.

El recorrido de caja, de una orden ya cerrada y de la autofactura del cliente está en [facturar en Shopify POS](/blog/facturar-shopify-pos). La activación del canal está en [facturación desde el POS](/docs/facturacion-pos-punto-de-venta-shopify).

## Sidekick rellena los datos fiscales

**Respuesta corta:** Sidekick abre el formulario del admin y rellena los datos fiscales de la orden. Tú confirmas y la factura se timbra.

Desde el admin le puedes pedir, en lenguaje natural, que facture una orden o que consulte su estatus. **Generar factura CFDI** abre el mismo formulario de siempre. La guía está en [facturación con Sidekick](/docs/facturacion-con-sidekick-ia). El recorrido en video está en el [tutorial de facturación con Sidekick](https://youtu.be/urs7klK6p1U).

<!-- TODO: docs/facturacion-con-sidekick-ia.md dice que Sidekick abre el formulario del admin y rellena datos fiscales. No dice que lea la CSF del cliente. La CSF es una carga opcional (enableCsfUpload) en los formularios públicos; su URL viaja en los disparadores de Flow. No afirmar que Sidekick llena la factura desde la CSF hasta que docs/ lo diga. -->
<!-- ENDTODO -->

Si activas la carga de la Constancia de Situación Fiscal, el cliente puede subir el PDF en la página de gracias, en la página de estado de orden y en el formulario de la tienda. El archivo se guarda y su URL va en los disparadores de Shopify Flow. El interruptor está en [Configuraciones](/docs/configuraciones).

## Notas de crédito y volver a facturar

**Respuesta corta:** un reembolso se ampara con una nota de crédito ligada a la factura original. Si el cliente pide factura después de público en general, cubres el total con notas de crédito y timbras la nueva sin cancelar la original.

La nota es un CFDI de egreso. La suma de las notas activas no puede pasar el total de la factura. En Shopify Flow, la acción **Crear Nota de Crédito** emite la nota de un pedido, por el total o por una parte. El paso a paso está en [nota de crédito por reembolso en Shopify](/blog/nota-de-credito-reembolso-shopify).

Desde la app **v3.23**, cuando las notas cubren el total, el botón **Volver a facturar** emite otra factura de la misma orden. La original sigue vigente ante el SAT, compensada por sus notas. Si el pedido ya estaba en la factura global, desde la **3.27.0** acreditas ese pedido y emites la nominativa desde la misma página. La global sigue válida para los demás. Ese camino está en [factura a nombre del cliente después de público en general](/blog/factura-cliente-despues-de-publico-en-general-shopify). La pantalla está en [notas de crédito](/docs/notas-de-credito).

Antes de timbrar, la app revisa si el pedido ya tiene una factura vigente. Si ya la tiene, te muestra esa factura en lugar de timbrar otra.

## Preguntas frecuentes

### ¿Shopify emite facturas CFDI?

No. Shopify cobra y guarda el pedido. El CFDI 4.0 lo timbra un PAC a través de una app. Con CFDI Express sale desde la página de gracias, el portal de cuentas, el admin, el POS o Shopify Flow.

### ¿Puedo facturar en automático a público en general?

Sí. En Plus, Pro y Enterprise, la acción **Facturación global** de Shopify Flow junta las órdenes pagadas y sin factura, hasta 1,000 por comprobante. En el plan Básico se timbra un CFDI por orden, a mano o con las plantillas de Flow de fin de día, 3 días, 10 días y fin de mes.

### ¿La misma orden se puede facturar dos veces?

No. La factura global solo toma órdenes pagadas y sin factura, y cada orden guarda folio y UUID. En el admin, si el pedido ya tiene una factura vigente, la app te muestra esa factura y no timbra otra.

### ¿El cliente puede pedir su factura después de público en general?

Sí. Cubres el total con una o varias notas de crédito y oprimes **Volver a facturar**. La original sigue vigente ante el SAT. Si el pedido ya está en la factura global (Plus, Pro y Enterprise), acreditas ese pedido y emites la factura a su nombre desde la página del pedido. La global no se cancela.

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "Cómo facturar pedidos de Shopify automáticamente (CFDI 4.0)",
  "description": "Factura cada pedido de Shopify en automático: autofactura en la página de gracias, envío por correo y WhatsApp, factura global con Flow y POS.",
  "image": "https://videos.acromatico.dev/api/images/assets/aab48b2d-67f6-43e0-ac2c-09264025b30f.jpg",
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
    "@id": "https://cfdi.express/blog/facturar-pedidos-shopify-automaticamente"
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
      "name": "¿Shopify emite facturas CFDI?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No. Shopify cobra y guarda el pedido. El CFDI 4.0 lo timbra un PAC a través de una app. Con CFDI Express sale desde la página de gracias, el portal de cuentas, el admin, el POS o Shopify Flow."
      }
    },
    {
      "@type": "Question",
      "name": "¿Puedo facturar en automático a público en general?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Sí. En Plus, Pro y Enterprise, la acción Facturación global de Shopify Flow junta las órdenes pagadas y sin factura, hasta 1,000 por comprobante. En el plan Básico se timbra un CFDI por orden, a mano o con las plantillas de Flow de fin de día, 3 días, 10 días y fin de mes."
      }
    },
    {
      "@type": "Question",
      "name": "¿La misma orden se puede facturar dos veces?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No. La factura global solo toma órdenes pagadas y sin factura, y cada orden guarda folio y UUID. En el admin, si el pedido ya tiene una factura vigente, la app te muestra esa factura y no timbra otra."
      }
    },
    {
      "@type": "Question",
      "name": "¿El cliente puede pedir su factura después de público en general?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Sí. Cubres el total con una o varias notas de crédito y oprimes Volver a facturar. La original sigue vigente ante el SAT. Si el pedido ya está en la factura global (Plus, Pro y Enterprise), acreditas ese pedido y emites la factura a su nombre desde la página del pedido. La global no se cancela."
      }
    }
  ]
}
</script>

## Deja el recorrido armado en tu tienda

1. Instala [CFDI Express](https://apps.shopify.com/cfdi-express) y publica el formulario en la [página de gracias](/docs/facturacion-checkout-thank-you-page) y en el [portal de autofacturación](/blog/portal-de-autofacturacion).
2. En Plus, Pro o Enterprise, programa la [factura global del mes](/blog/factura-global-del-mes-shopify) con Shopify Flow. En el plan Básico, usa la plantilla de un CFDI por orden.
3. Revisa una venta de [Shopify POS](/blog/facturar-shopify-pos) y deja lista la [nota de crédito por reembolso](/blog/nota-de-credito-reembolso-shopify) por si hay una devolución.

La app está en [cfdi.express](https://cfdi.express). ¿Quieres ver el recorrido con las órdenes de tu tienda? [Agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).
