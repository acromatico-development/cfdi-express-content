---
title: "Factura global en Shopify: cómo hacerla en CFDI 4.0"
description: "Cómo hacer la factura global en Shopify con CFDI Express: ventas sin factura a público en general, mes y año del periodo y timbrado automático con Flow."
image: "https://videos.acromatico.dev/api/images/assets/PENDIENTE-factura-global-shopify.png?w=1600"
author: "Rafael González"
date: "2026-10-04"
keywords: "factura global shopify, cómo hacer factura global en shopify, factura global cfdi 4.0, público en general shopify, XAXX010101000, ventas sin factura shopify, información global periodicidad, CFDI Express"
---
<!-- TODO: Diego genera el hero 16:9 (1600×900) y el recorte OG con el prompt de socials/factura-global-shopify.md, lo sube a videos.acromatico.dev y reemplaza el placeholder del frontmatter y la imagen de abajo. -->
<!-- ENDTODO -->

# Factura global en Shopify: cómo hacerla en CFDI 4.0

![Factura global en Shopify con CFDI Express](https://videos.acromatico.dev/api/images/assets/PENDIENTE-factura-global-shopify.png?w=1600)

Cada venta de tu tienda Shopify lleva CFDI, la pida el cliente o no. Las que nadie facturó van a **público en general**: RFC `XAXX010101000`, con la información global (periodicidad, mes y año) que pide el CFDI 4.0.

Esta guía es para la app de Shopify [CFDI Express](https://apps.shopify.com/cfdi-express). Ves cómo se arma la factura global en la app, cómo hacerla a mano desde el admin, cómo automatizarla con Shopify Flow y qué pasa si el cliente pide su factura después. El paso a paso corto está en [Facturación al público en general](/docs/facturacion-al-publico-en-general-manual-y-automatica).

## ¿Qué es la factura global y cuándo la necesitas?

**Respuesta corta:** es el CFDI de las ventas que no se facturaron a nombre de un cliente. Se emite a público en general, con el periodo al que corresponden esas ventas.

En Shopify pasa todos los días: alguien compra en línea y no llena el formulario, o paga en caja y no pide factura. Esas órdenes no se quedan sin comprobante. Las facturas a `XAXX010101000` dentro del periodo que trabajes con tu contador.

El marco SAT completo (datos del receptor, motivos de cancelación, información global) está en [CFDI facturas 4.0](/blog/cfdi-facturas-4-0).

## Cómo arma CFDI Express la factura global

**Respuesta corta:** la app timbra **un CFDI a público en general por cada orden** de Shopify. La información global va con periodicidad mensual y el mes y año que elijas.

Esto es lo que lleva el CFDI cuando facturas una orden a público en general desde la app:

| Campo | Valor en CFDI Express |
| --- | --- |
| RFC del receptor | `XAXX010101000` |
| Nombre | `PUBLICO EN GENERAL` |
| Régimen fiscal | `616` (Sin obligaciones fiscales) |
| Uso de CFDI | `S01` (Sin efectos fiscales) |
| Método de pago | `PUE` |
| Código postal del receptor | El de tu negocio (lugar de expedición) |
| Periodicidad | `04` (mensual) |
| Mes y año | El que elijas; por omisión, el mes en curso (hora de la Ciudad de México) |

Dos cosas que conviene saber antes de empezar:

- **Una orden, un CFDI.** La app no junta varias órdenes en un solo comprobante. Cada orden queda con su propio UUID, su PDF y su XML, y con el estatus **Facturado** en la lista de órdenes.
- **La periodicidad no se elige en la app.** Siempre va `04` (mensual). Tú eliges el **mes** y el **año**.

Si necesitas un solo CFDI global que agrupe muchas ventas, ese camino es la [API de CFDI Express](https://cfdi.express/api) (más abajo).

## Paso a paso: factura global manual desde el admin

**Respuesta corta:** abre la orden en la app, oprime **Facturar al Publico en General**, revisa mes y año, elige la forma de pago y genera el CFDI.

1. En el admin de Shopify abre **Apps → CFDI Express - Facturación**.
2. En la [lista de órdenes](/docs/ordenes-estatus) filtra **Estado de pago: Órdenes pagadas** y **Estado de factura: No Facturado**. Ahí están las ventas pendientes.
3. Abre la orden. La pantalla se llama **CFDI para orden {nombre}**.
4. Oprime **Facturar al Publico en General**. El formulario se llena con RFC `XAXX010101000`, `PUBLICO EN GENERAL`, régimen `616`, el código postal de tu negocio, uso `S01`, tipo de pago `PUE` y el correo de tu tienda en **Correo de Facturación**.
5. Revisa **Mes de la Factura Global** y **Año de la Factura Global**. Por omisión es el mes en curso. Puedes elegir cualquier mes y el año actual o uno de los cuatro anteriores.
6. En **Método de Pago** elige la forma de pago SAT de esa venta. Este campo no se llena solo.
7. Oprime **Generar CFDI**. Puedes descargar el ZIP con PDF y XML desde la misma orden.

<!-- TODO: captura — pantalla "CFDI para orden" con el botón "Facturar al Publico en General" y los campos "Mes de la Factura Global" / "Año de la Factura Global" -->
<!-- ENDTODO -->

Sirve para pocas órdenes o para corregir una en particular. Si son decenas al día, automatízalo.

## Factura global automática con Shopify Flow

**Respuesta corta:** importa una plantilla de Flow. Cada cierto tiempo busca las órdenes pagadas y no facturadas y las timbra a público en general con la acción **Crear CFDI**.

CFDI Express agrega a [Shopify Flow](/docs/uso-shopify-flow-automatizaciones-cfdi-express) la acción **Crear CFDI**. Combinada con el disparador de horario de Flow, factura sola las ventas que se quedaron sin factura. Hay cuatro plantillas listas para importar:

- [Al final del día](https://assets.acromatico.dev/assets/5d6eebfc-9304-4860-8017-2145b2150e43.flow)
- [Después de 3 días](https://assets.acromatico.dev/assets/4b95bd41-4ba3-451d-babd-40b632821148.flow)
- [Después de 10 días](https://assets.acromatico.dev/assets/a4e4ea57-6d0a-42ad-be1e-b4eeec33d7dc.flow)
- [Al final del mes](https://assets.acromatico.dev/assets/3ef1789f-05c1-489e-9b12-a0acbfe437f6.flow)

La plantilla define **cuándo** se timbra. El CFDI sigue saliendo con periodicidad mensual, una orden a la vez.

### Cómo instalarla

1. Descarga el archivo `.flow` de la plantilla que quieras.
2. En el admin abre **Apps → Flow** e importa el archivo.
3. Revisa la acción **Crear CFDI** antes de activar (siguiente sección).
4. Activa el workflow.

<!-- TODO: captura — workflow importado en Shopify Flow: horario → buscar órdenes → para cada orden → Crear CFDI -->
<!-- ENDTODO -->

### Qué revisar antes de activarla

Las plantillas son una base. Ajusta esto a tu tienda:

- **Código Postal.** Pon el de tu negocio, el de tu Constancia. Es el lugar de expedición.
- **Metodo de Pago.** La plantilla trae una forma de pago de ejemplo. Cámbiala por la que corresponda a tus ventas.
- **Correo de aviso.** El último paso manda un resumen por correo. Pon tu dirección.
- **Búsqueda de órdenes.** La plantilla busca órdenes **pagadas** que no estén marcadas como **Facturado** ni con **Ignorar auto-facturación**. El query también trae una fecha de inicio. Si quieres otro periodo de espera u órdenes más viejas, edítalo en Flow.
- **Hasta 100 órdenes por corrida.** Si vendes más que eso por periodo, usa una plantilla que corra más seguido.
- **Moneda.** Las órdenes que no están en pesos (MXN) se brincan y quedan anotadas en el registro del workflow para que las factures aparte.

### Mes y año del periodo

La acción **Crear CFDI** tiene dos campos opcionales: **Mes Factura Global** (01–12) y **Año Factura Global**. Si los dejas vacíos, usa el mes y año en curso.

Ojo con la plantilla de 10 días: a principios de mes puede timbrar órdenes del mes anterior. Pregúntale a tu contador qué mes deben llevar y, si hace falta, llena esos campos.

### Órdenes que no deben entrar al global

CFDI Express agrega a cada orden el metacampo **Ignorar auto-facturación**. Lo editas en el admin de Shopify, dentro de la orden. Márcalo en las ventas que no quieres que la automatización toque, por ejemplo una que vas a facturar con los datos del cliente. Las plantillas ya lo respetan.

## Cómo convive con la autofactura del cliente

**Respuesta corta:** define hasta cuándo se puede autofacturar el cliente y programa el global después de esa fecha.

En [Configuraciones](/docs/configuraciones) eliges la regla de periodo de facturación:

- **Sin límite.**
- **Fin del mes de compra.**
- **Días después de la compra** (por omisión, 30).

La [Thank You Page](/docs/facturacion-checkout-thank-you-page) y la [página de estado de orden](/docs/facturacion-pagina-estado-de-orden) respetan esa regla. Pasado el periodo, el cliente ve **Período de facturación CFDI expirado** y un aviso para contactarte.

Si la orden ya se facturó a público en general, el formulario no le deja timbrar otra. Le dice que el comercio cerró esa orden y que te contacte.

Por eso conviene alinear las dos piezas. Si tu regla es **Fin del mes de compra**, la plantilla **Al final del mes** respeta ese plazo. Si corres la de **Al final del día**, tu cliente casi no alcanza a autofacturarse.

## ¿Y si el cliente pide factura después del global?

**Respuesta corta:** la app te deja cancelar el CFDI y volver a facturar, o compensarlo con nota de crédito y timbrar uno nuevo. Cuál usar lo decides con tu contador.

Estos caminos existen hoy en CFDI Express:

1. **Cancelar y volver a facturar.** En la orden oprime **Cancelar CFDI**. La orden queda libre para timbrar el CFDI con los datos fiscales del cliente. Detalle en [Cancelación y acuse](/docs/cancelacion-y-acuse).
2. **Nota de crédito y nueva factura.** Si una [nota de crédito](/docs/notas-de-credito) cubre el 100% del CFDI, la app te deja generar una nueva factura de esa orden. El CFDI original sigue vigente ante el SAT, compensado por la nota.
3. **Cancelar desde Flow.** La acción **Eliminar CFDI** de Flow recibe el motivo de cancelación SAT (`01`, `02`, `03` o `04`). El `04` es el de operación nominativa relacionada en una factura global.

En todos los casos, al cliente pídele **datos fiscales**: RFC, nombre o razón social, régimen, código postal y uso de CFDI. No necesitas el PDF de su Constancia.

## Ventas a clientes del extranjero

Para ventas a extranjeros la app tiene **Facturar al Publico en General Extranjero**, con RFC `XEXX010101000`. La guía está en [Facturación al extranjero](/docs/facturacion-al-extranjero).

## ¿Necesitas un solo CFDI global con muchas ventas?

**Respuesta corta:** en la app, cada orden va en su propio CFDI. Si quieres un comprobante que agrupe muchas ventas, se arma por API.

La [API de CFDI Express](https://cfdi.express/api) acepta la información global en `POST /v1/invoices` con periodicidad `01` diaria, `02` semanal, `03` quincenal, `04` mensual o `05` bimestral (meses `13`–`18`). Sirve si tu equipo de desarrollo junta las ventas de un sistema propio. La referencia está en la [documentación de la API](https://api.cfdi.express/docs).

## Preguntas frecuentes

### ¿Shopify hace la factura global solo?

No. Shopify registra la venta, pero no timbra CFDI. Necesitas una app de facturación. Con CFDI Express la haces desde el admin o la automatizas con Shopify Flow.

### ¿Qué periodicidad usa CFDI Express en la factura global?

`04` (mensual). En el admin eliges el mes y el año. En Flow, los campos **Mes Factura Global** y **Año Factura Global**; vacíos, toman el mes en curso.

### ¿CFDI Express junta todas las ventas en un solo CFDI global?

No. Timbra un CFDI a público en general por orden, con la información global en cada uno. Para un solo CFDI que agrupe ventas existe la API de CFDI Express.

### ¿Puedo dejar fuera una orden de la factura global automática?

Sí. Marca el metacampo **Ignorar auto-facturación** en la orden, desde el admin de Shopify. Las plantillas de Flow la brincan.

### ¿Las ventas de Shopify POS entran al global?

Las plantillas buscan órdenes pagadas y no facturadas sin filtrar canal, así que las de [Shopify POS](/blog/facturar-shopify-pos) también entran. Si el cliente pidió factura en caja, la orden ya está facturada y no se toca.

### ¿Qué pasa si el cliente pide su factura después?

En la página de estado de orden verá que el comercio cerró esa orden. Tú puedes cancelar y volver a facturar con sus datos, o compensar con nota de crédito y timbrar una nueva factura. Revisa con tu contador cuál aplica.

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "¿Shopify hace la factura global solo?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No. Shopify registra la venta, pero no timbra CFDI. Necesitas una app de facturación. Con CFDI Express la haces desde el admin o la automatizas con Shopify Flow."
      }
    },
    {
      "@type": "Question",
      "name": "¿Qué periodicidad usa CFDI Express en la factura global?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "04 (mensual). En el admin eliges el mes y el año. En Shopify Flow, los campos Mes Factura Global y Año Factura Global; si van vacíos, toman el mes en curso."
      }
    },
    {
      "@type": "Question",
      "name": "¿CFDI Express junta todas las ventas en un solo CFDI global?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No. Timbra un CFDI a público en general por orden, con la información global en cada uno. Para un solo CFDI que agrupe ventas existe la API de CFDI Express."
      }
    },
    {
      "@type": "Question",
      "name": "¿Puedo dejar fuera una orden de la factura global automática?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Sí. Marca el metacampo Ignorar auto-facturación en la orden, desde el admin de Shopify. Las plantillas de Shopify Flow de CFDI Express la brincan."
      }
    },
    {
      "@type": "Question",
      "name": "¿Las ventas de Shopify POS entran al global?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Sí. Las plantillas buscan órdenes pagadas y no facturadas sin filtrar canal, así que las ventas de Shopify POS también entran. Si el cliente pidió factura en caja, la orden ya está facturada y no se toca."
      }
    },
    {
      "@type": "Question",
      "name": "¿Qué pasa si el cliente pide su factura después del global?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "En la página de estado de orden verá que el comercio cerró esa orden. Puedes cancelar y volver a facturar con sus datos, o compensar con nota de crédito y timbrar una nueva factura. Revisa con tu contador cuál aplica."
      }
    }
  ]
}
</script>

## Instala CFDI Express y cierra tus ventas sin factura

1. Instala [CFDI Express en Shopify](https://apps.shopify.com/cfdi-express) y completa el [onboarding](/docs/onboarding) con tu RFC, régimen, código postal y CSD.
2. Factura a público en general una orden de prueba desde el admin.
3. Importa la plantilla de Flow que vaya con tu periodo de autofactura.

Si también vendes en tienda física, sigue con [cómo facturar en Shopify POS](/blog/facturar-shopify-pos).

¿Quieres verlo con las órdenes de tu tienda? [Agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).
