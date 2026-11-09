---
title: "Nota de crédito por reembolso en Shopify (CFDI 4.0)"
description: "Timbra la nota de crédito de un reembolso total o parcial en Shopify, ligada a la factura original, a mano o en automático con Shopify Flow."
image: "https://videos.acromatico.dev/api/images/assets/dd6881bb-99fa-4f31-a129-64c9bd0fc5b0.jpg"
author: "Rafael González"
date: "2026-11-09"
keywords: "nota de crédito shopify, nota de crédito por devolución cfdi 4.0, reembolso shopify factura, CFDI de egreso, Shopify Flow, CFDI Express"
---
# Nota de crédito por reembolso en Shopify (CFDI 4.0)

![Hero del blog: Nota de crédito por reembolso en Shopify (CFDI 4.0), reembolso ligado a la factura original](https://videos.acromatico.dev/api/images/assets/dd6881bb-99fa-4f31-a129-64c9bd0fc5b0.jpg?w=1600)

Un reembolso de Shopify no cancela la factura. Ante el SAT se ampara con una **nota de crédito**: un CFDI de egreso ligado a la factura original. En [CFDI Express](https://apps.shopify.com/cfdi-express) la timbras desde la orden, por el total o por una parte, o con la acción **Crear Nota de Crédito** de Shopify Flow.

Esta guía es la de la nota de crédito por devolución en CFDI 4.0. El mapa de toda la facturación automática está en [cómo facturar pedidos de Shopify automáticamente](/blog/facturar-pedidos-shopify-automaticamente). La pantalla está en [notas de crédito](/docs/notas-de-credito).

## Qué queda ligado a la factura original

**Respuesta corta:** la nota es un CFDI de egreso relacionado con la factura de ingreso. La original sigue vigente ante el SAT.

Para generarla hacen falta tres cosas:

- La orden tiene un CFDI activo, timbrado por CFDI Express.
- El proveedor de facturación es CFDI Express. Una factura timbrada en Modo Flow no entra a esta sección: la nota se genera con el mismo emisor.
- La suma de las notas de crédito activas no pasa el total de la factura.

En la orden, la sección **Devoluciones (Notas de Crédito)** lista las notas ya emitidas: folio, fecha, monto, estado (Activa o Cancelada) y las acciones de descarga y cancelación. Si el total ya quedó acreditado, el botón se sustituye por el aviso de que la factura ya fue acreditada con notas de crédito.

## Reembolso total o parcial, desde la factura

**Respuesta corta:** capturas el monto, la forma de pago y la descripción. La app no deja acreditar más de lo que queda.

1. Abre la orden en la app.
2. En **Devoluciones (Notas de Crédito)**, oprime **Devolución (Nota de Crédito)**.
3. Elige el tipo de relación: `01` (nota de crédito por reembolso o descuento) o `03` (devolución de mercancía).
4. Elige la forma de pago del reembolso. El catálogo del SAT incluye, por ejemplo, `01` transferencia, `03` cheque y `15` condonación.
5. Captura el **monto a acreditar, impuestos incluidos**. El campo muestra el disponible: el total de la factura menos lo ya acreditado con notas activas.
6. Escribe la descripción.
7. Si capturas el correo del receptor, la app envía la nota por correo.
8. Oprime **Generar Nota de Crédito**.

La app timbra el CFDI de egreso con tu CSD, arma el ZIP con PDF y XML, y registra el uso de tu plan. Puedes emitir varias notas parciales sobre la misma factura. Si el monto se pasa del disponible, la app muestra un error con lo que todavía se puede acreditar.

El comprobante sale en un solo concepto genérico, clave `84111506` y unidad `ACT`. La descripción es el texto que escribiste. La nota que emites en esta pantalla, y la acción de Flow, no llevan pedimentos.

Si la factura es en dólares, la nota sale en dólares, con el tipo de cambio de la fecha de la nota. El monto que capturas va en esa moneda. Desde la app **3.28.0**. El paso está en [cómo facturar en dólares desde Shopify](/blog/facturar-en-dolares-shopify).

Cada nota tiene **Descargar**: el ZIP con el PDF y el XML. Cancelar una nota activa la cancela ante el SAT y su monto vuelve a quedar disponible, salvo que la factura ya se haya reemplazado. Esas notas ya no se cancelan. Mientras haya notas activas, la factura original no se puede cancelar: primero cancelas las notas.

## La nota de crédito con Shopify Flow

**Respuesta corta:** la acción **Crear Nota de Crédito** emite la nota de un pedido, por el total o por una parte.

1. En el admin abre **Apps → Flow**.
2. Crea un workflow.
3. Agrega la acción **Crear Nota de Crédito** de CFDI Express.
4. Activa el workflow.

Desde la versión **3.27.0** la misma acción sirve si el pedido ya está en una factura global. Esa factura es de Plus, Pro y Enterprise. La nota puede ser por el pedido completo o por una parte, y acredita IVA al 0%, al 8% y al 16%, e IEPS, cada uno con su tasa. La global sigue válida para los demás. En el plan Básico no hay factura global.

El tope es el de la factura: la suma de las notas activas no puede exceder el total. La guía de la acción está en [Shopify Flow para automatizar el CFDI](/docs/uso-shopify-flow-automatizaciones-cfdi-express).

<!-- TODO: docs/uso-shopify-flow-automatizaciones-cfdi-express.md documenta la acción Crear Nota de Crédito (total o parcial, y desde la 3.27.0 también si el pedido está en la factura global). No documenta que el monto, la forma de pago y el concepto se llenen solos, ni que la acción devuelva folio, UUID y liga, ni que cada reembolso de Shopify envíe el correo por sí solo. El correo que sí está en docs/notas-de-credito.md es el del formulario, cuando capturas el correo del receptor. No afirmar esos tres puntos hasta que estén en docs/. Las versiones 3.13 y 3.15 no aparecen en docs/. -->
<!-- ENDTODO -->

El correo documentado es el del formulario: si capturas el correo del receptor, CFDI Express envía la nota. En la pantalla tú eliges la forma de pago, el monto y la descripción. El concepto del XML es el genérico de arriba.

## Si el pedido ya iba en la factura global

**Respuesta corta:** desde la **3.27.0** emites la nota en la página de ese pedido. La global no se cancela.

Acreditas el pedido completo o solo una parte. La nota cubre nada más ese pedido. Si el cliente quiere la factura a su nombre, la nota es por el pedido completo y la nominativa la emites tú desde la misma página. El cliente no la timbra en la página de gracias, en el formulario de la tienda ni en el POS. Ese camino está en [factura a nombre del cliente después de público en general](/blog/factura-cliente-despues-de-publico-en-general-shopify).

## Preguntas frecuentes

### ¿La nota de crédito cancela la factura del reembolso?

No. Genera un CFDI de egreso relacionado. La factura original sigue vigente ante el SAT. Cancelar el CFDI de ingreso es otro camino, y no procede mientras tenga notas de crédito activas.

### ¿Puedo timbrar una nota de crédito por un reembolso parcial?

Sí. El monto es el que capturas, impuestos incluidos, y puede ser menor al total. Puedes emitir varias notas parciales. La suma de las activas no puede pasar el total de la factura.

### ¿Shopify Flow timbra la nota de crédito?

La acción **Crear Nota de Crédito** emite la nota de un pedido, por el total o por una parte. La agregas a un workflow de Shopify Flow. Desde la **3.27.0** también sirve si el pedido ya está en la factura global de Plus, Pro o Enterprise.

### ¿La app acredita más de lo que queda por acreditar?

No. El campo muestra el disponible por acreditar. Si el monto lo rebasa, la app muestra un error con ese disponible y no timbra la nota.

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "Nota de crédito por reembolso en Shopify (CFDI 4.0)",
  "description": "Timbra la nota de crédito de un reembolso total o parcial en Shopify, ligada a la factura original, a mano o en automático con Shopify Flow.",
  "image": "https://videos.acromatico.dev/api/images/assets/dd6881bb-99fa-4f31-a129-64c9bd0fc5b0.jpg",
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
    "@id": "https://cfdi.express/blog/nota-de-credito-reembolso-shopify"
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
      "name": "¿La nota de crédito cancela la factura del reembolso?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No. Genera un CFDI de egreso relacionado. La factura original sigue vigente ante el SAT. Cancelar el CFDI de ingreso es otro camino, y no procede mientras tenga notas de crédito activas."
      }
    },
    {
      "@type": "Question",
      "name": "¿Puedo timbrar una nota de crédito por un reembolso parcial?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Sí. El monto es el que capturas, impuestos incluidos, y puede ser menor al total. Puedes emitir varias notas parciales. La suma de las activas no puede pasar el total de la factura."
      }
    },
    {
      "@type": "Question",
      "name": "¿Shopify Flow timbra la nota de crédito?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "La acción Crear Nota de Crédito emite la nota de un pedido, por el total o por una parte. La agregas a un workflow de Shopify Flow. Desde la 3.27.0 también sirve si el pedido ya está en la factura global de Plus, Pro o Enterprise."
      }
    },
    {
      "@type": "Question",
      "name": "¿La app acredita más de lo que queda por acreditar?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No. El campo muestra el disponible por acreditar. Si el monto lo rebasa, la app muestra un error con ese disponible y no timbra la nota."
      }
    }
  ]
}
</script>

## Timbra la nota del siguiente reembolso

1. Abre una orden con CFDI activo de CFDI Express y entra a **Devoluciones (Notas de Crédito)**. La guía está en [notas de crédito](/docs/notas-de-credito).
2. Emite la nota por el monto del reembolso. Si quieres que el cliente la reciba, captura su correo.
3. Si ese paso debe correr en un workflow, agrega la acción **Crear Nota de Crédito**. La referencia está en [Shopify Flow](/docs/uso-shopify-flow-automatizaciones-cfdi-express).

Si el cliente pide la factura a su nombre después de público en general, el siguiente paso es [volver a facturar sin cancelar la original](/blog/factura-cliente-despues-de-publico-en-general-shopify). El recorrido completo está en [cómo facturar pedidos de Shopify automáticamente](/blog/facturar-pedidos-shopify-automaticamente).

[Instala CFDI Express](https://apps.shopify.com/cfdi-express) en tu tienda. ¿Quieres ver la nota con una devolución de tu tienda? [Agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).
