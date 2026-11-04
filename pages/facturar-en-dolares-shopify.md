---
title: "Cómo facturar en dólares desde Shopify (CFDI 4.0 en USD)"
description: "Activa la facturación en USD en Shopify: tipo de cambio FIX de Banxico editable, notas de crédito y complementos de pago en dólares. Paso a paso."
image: "https://videos.acromatico.dev/api/images/assets/PENDIENTE-facturar-en-dolares-shopify.png"
author: "Rafael González"
date: "2026-11-04"
keywords: "facturar en dólares shopify, factura en dólares Shopify, CFDI 4.0 en USD, tipo de cambio FIX Banxico, factura global diaria, CFDI Express"
---
# Cómo facturar en dólares desde Shopify (CFDI 4.0 en USD)

<!-- TODO: hero pendiente. Generar la imagen 16:9, subirla a videos.acromatico.dev y sustituir PENDIENTE-facturar-en-dolares-shopify.png en el frontmatter y en la imagen de abajo. -->
<!-- ENDTODO -->

![Cómo facturar en dólares desde Shopify con CFDI Express](https://videos.acromatico.dev/api/images/assets/PENDIENTE-facturar-en-dolares-shopify.png?w=1600)

Desde la versión **3.28.0** de [CFDI Express](https://cfdi.express) puedes facturar en dólares las órdenes que Shopify cobró en dólares. El CFDI sale en USD, con el tipo de cambio FIX de Banxico del día. Esta guía es el paso a paso dentro de la app.

Lo que pide el SAT (el campo Moneda, el TipoCambio y cómo van la nota de crédito y el complemento de pago) está en [CFDI 4.0 en dólares](/blog/cfdi-4-0-en-dolares). El detalle de moneda extranjera está en [Facturación al extranjero](/docs/facturacion-al-extranjero#moneda-extranjera).

## Activa la facturación en USD en Configuración

**Respuesta corta:** la opción viene apagada. Si la moneda de tu tienda ya es el dólar, tus facturas salen en dólares sin que la prendas.

Hay dos casos.

Si la **moneda de la tienda** es el dólar, desde la 3.28.0 las facturas salen en USD con su tipo de cambio. Antes, esas ventas se timbraban como si fueran pesos.

Si la tienda está en pesos y algunas órdenes se cobraron en dólares, activa la opción. Hasta que lo hagas, nada cambia en esas órdenes.

1. En el admin abre **Apps → CFDI Express - Facturación**.
2. Entra a **Configuraciones**.
3. Activa **Facturar en dólares (USD) las órdenes cobradas en dólares**.
4. Si no ves la opción, actualiza la app a la **3.28.0**.

El nombre de la opción es ese. Las órdenes cobradas en pesos siguen en pesos. La pantalla está en [Configuraciones](/docs/configuraciones).

## Tipo de cambio FIX y cómo editarlo con vista previa

**Respuesta corta:** la app propone el FIX de Banxico del día. En la página del pedido puedes capturar otro y ver la vista previa con ese valor antes de timbrar.

1. Abre la orden en la app.
2. Revisa el tipo de cambio. Es el FIX del día, en pesos por un dólar.
3. Si esa venta lleva otro tipo de cambio, captúralo en la misma página.
4. La vista previa se arma con el valor que dejaste.
5. Timbra cuando la vista previa sea la que quieres emitir.

Si no lo cambias, se timbra el FIX del día. El tipo de cambio es de la fecha de la factura. Cuántos decimales acepta el SAT y por qué se usa el FIX está en [CFDI 4.0 en dólares](/blog/cfdi-4-0-en-dolares).

## Notas de crédito y complementos de pago en USD

**Respuesta corta:** si la factura es en dólares, la nota de crédito y el complemento de pago salen en dólares. Cada uno lleva el tipo de cambio de su fecha.

La nota se hace donde ya la hacías: en la orden, **Devolución (Nota de Crédito)**. El monto va en dólares, como la factura. El tipo de cambio es el de la fecha de la nota, no el de la factura. El paso a paso de la pantalla está en [Notas de crédito](/docs/notas-de-credito).

El complemento es el de una factura **PPD** en dólares. El abono se captura en dólares. El tipo de cambio es el de la fecha del pago. La pantalla está en [Complementos de pago](/docs/complementos-de-pago). Cómo arma el SAT ese comprobante está en [CFDI 4.0 en dólares](/blog/cfdi-4-0-en-dolares).

## Totales al centavo

**Respuesta corta:** el total del CFDI es el que cobró Shopify. $674.00 se factura en $674.00, no en $673.99.

Eso vale con descuentos y con envío. La nota de crédito cuadra igual: el monto que acreditas es el que queda en el comprobante, al centavo. Aplica en pesos y en dólares.

## Factura global diaria y mensual

**Respuesta corta:** agrupar ventas en la factura global está en Plus, Pro y Enterprise. En el plan Básico se factura al público en general un CFDI por pedido.

Desde la **3.28.0** hay una plantilla de Shopify Flow de **factura global diaria**. Cada noche agrega a la factura global a público en general las ventas de ayer y las del mes que se pagaron después. El día 1 cierra el mes anterior completo. La activas y la corrida queda programada. Está en Plus, Pro y Enterprise.

La factura global del mes sigue desde la **3.26.0**: la lanzas en **Facturación global** (el primer elemento del menú) o con la acción del mismo nombre en Flow. El paso a paso está en [Facturación global mensual](/docs/facturacion-global-mensual). Qué es ese comprobante está en [Cómo hacer la factura global del mes en Shopify](/blog/factura-global-del-mes-shopify).

Qué órdenes entran:

- Pagadas, en **pesos**, y sin factura: entran como hasta ahora. El comprobante sale en MXN.
- Cobradas en **dólares**, cuando la tienda ya factura en USD (moneda de la tienda en dólares, o la opción de Configuración activada): entran, y esa factura global sale en USD con el FIX de la fecha del comprobante.
- Otra moneda, sin pagar, o que ya tiene CFDI: no entra.

Pesos y dólares van en comprobantes distintos. Cada factura global lleva una sola moneda.

En el plan **Básico** (en [Planes y precios](/docs/planes-y-precios) se llama Standard) esta corrida no está. Ahí el público en general sigue siendo **un CFDI por pedido**: el botón **Facturar al Publico en General**, o las plantillas de Flow de fin de día, 3 días, 10 días y fin de mes. Esas plantillas usan la acción **Crear CFDI**. La plantilla diaria nueva usa **Facturación global**. No son la misma. El camino de un CFDI por pedido está en [Factura global en Shopify](/blog/factura-global-shopify) y en [Facturación al público en general](/docs/facturacion-al-publico-en-general-manual-y-automatica). Las acciones de Flow están en [Shopify Flow](/docs/uso-shopify-flow-automatizaciones-cfdi-express).

## Preguntas frecuentes

### ¿Tengo que activar la opción si mi tienda ya está en dólares?

No. Si la moneda de la tienda es el dólar, las facturas salen en USD desde la 3.28.0. La opción **Facturar en dólares (USD) las órdenes cobradas en dólares** es para una tienda en pesos que también cobra órdenes en dólares. Viene apagada.

### ¿Qué tipo de cambio usa la app?

El FIX de Banxico del día. En la página del pedido puedes capturar otro. La vista previa se arma con ese valor antes de timbrar.

### ¿La nota de crédito de una factura en dólares sale en pesos?

No. Sale en dólares, con el tipo de cambio de la fecha de la nota. El complemento de pago de una factura PPD en dólares también sale en dólares, con el tipo de cambio de la fecha del pago.

### ¿El total puede diferir un centavo del de Shopify?

No. El total del CFDI es el que cobró Shopify. Un producto de $674.00 se factura en $674.00, no en $673.99, aunque haya descuentos o envío. La nota de crédito cuadra igual.

### ¿La factura global diaria está en el plan Básico?

No. La plantilla diaria y la factura global del mes están en Plus, Pro y Enterprise. En el plan Básico se factura al público en general un CFDI por pedido, a mano o con la plantilla de Shopify Flow.

### ¿Qué hace la plantilla el día 1?

Cierra el mes anterior completo. Las demás noches agrega las ventas de ayer y las del mes que se pagaron después.

### ¿Esta guía sustituye a la de CFDI 4.0 en dólares?

No. [CFDI 4.0 en dólares](/blog/cfdi-4-0-en-dolares) es la regla fiscal y el contrato de la API. Esta es el paso a paso dentro de Shopify.

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "Cómo facturar en dólares desde Shopify (CFDI 4.0 en USD)",
  "description": "Activa la facturación en USD en Shopify: tipo de cambio FIX de Banxico editable, notas de crédito y complementos de pago en dólares. Paso a paso.",
  "image": "https://videos.acromatico.dev/api/images/assets/PENDIENTE-facturar-en-dolares-shopify.png",
  "author": {
    "@type": "Person",
    "name": "Rafael González"
  },
  "publisher": {
    "@type": "Organization",
    "name": "CFDI Express",
    "url": "https://cfdi.express"
  },
  "datePublished": "2026-11-04",
  "dateModified": "2026-11-04",
  "inLanguage": "es-MX",
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "https://cfdi.express/blog/facturar-en-dolares-shopify"
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
      "name": "¿Tengo que activar la opción si mi tienda ya está en dólares?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No. Si la moneda de la tienda es el dólar, las facturas salen en USD desde la 3.28.0. La opción Facturar en dólares (USD) las órdenes cobradas en dólares es para una tienda en pesos que también cobra órdenes en dólares. Viene apagada."
      }
    },
    {
      "@type": "Question",
      "name": "¿Qué tipo de cambio usa la app?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "El FIX de Banxico del día. En la página del pedido puedes capturar otro. La vista previa se arma con ese valor antes de timbrar."
      }
    },
    {
      "@type": "Question",
      "name": "¿La nota de crédito de una factura en dólares sale en pesos?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No. Sale en dólares, con el tipo de cambio de la fecha de la nota. El complemento de pago de una factura PPD en dólares también sale en dólares, con el tipo de cambio de la fecha del pago."
      }
    },
    {
      "@type": "Question",
      "name": "¿El total puede diferir un centavo del de Shopify?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No. El total del CFDI es el que cobró Shopify. Un producto de $674.00 se factura en $674.00, no en $673.99, aunque haya descuentos o envío. La nota de crédito cuadra igual."
      }
    },
    {
      "@type": "Question",
      "name": "¿La factura global diaria está en el plan Básico?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No. La plantilla diaria y la factura global del mes están en Plus, Pro y Enterprise. En el plan Básico se factura al público en general un CFDI por pedido, a mano o con la plantilla de Shopify Flow."
      }
    },
    {
      "@type": "Question",
      "name": "¿Qué hace la plantilla de factura global diaria el día 1?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Cierra el mes anterior completo. Las demás noches agrega las ventas de ayer y las del mes que se pagaron después."
      }
    },
    {
      "@type": "Question",
      "name": "¿Esta guía sustituye a la de CFDI 4.0 en dólares?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No. La guía CFDI 4.0 en dólares es la regla fiscal y el contrato de la API. Esta es el paso a paso dentro de Shopify."
      }
    }
  ]
}
</script>

## Factura en dólares la siguiente orden

1. Confirma que la app está en la **3.28.0** o posterior. Si la tienda está en dólares, abre una orden y revisa que la vista previa salga en USD. Si la tienda está en pesos, activa la opción en [Configuraciones](/docs/configuraciones).
2. En una orden cobrada en dólares, revisa el FIX del día o captura el tipo de cambio que corresponda. Timbra cuando la vista previa cuadre.
3. Si cierras el público en general en una sola factura, en Plus, Pro o Enterprise activa la plantilla diaria o sigue la [facturación global mensual](/docs/facturacion-global-mensual). En el plan Básico, factura pedido por pedido.

La app está en [cfdi.express](https://cfdi.express). Instálala desde [Shopify](https://apps.shopify.com/cfdi-express). ¿Quieres verlo con las órdenes de tu tienda? [Agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).
