---
title: "Facturar en Shopify POS: CFDI 4.0 en tu tienda física"
description: "Cómo facturar ventas de Shopify POS con CFDI Express: CFDI en caja, ventas pasadas desde la orden, autofactura del cliente y global de lo no facturado."
image: "https://videos.acromatico.dev/api/images/assets/PENDIENTE-facturar-shopify-pos.png?w=1600"
author: "Rafael González"
date: "2026-10-04"
keywords: "facturar shopify pos, cfdi shopify pos, factura shopify pos, facturación punto de venta shopify, autofactura tienda física, factura global ventas pos, CFDI 4.0, CFDI Express"
---
<!-- TODO: Diego genera el hero 16:9 (1600×900) y el recorte OG con el prompt de socials/facturar-shopify-pos.md, lo sube a videos.acromatico.dev y reemplaza el placeholder del frontmatter y la imagen de abajo. -->
<!-- ENDTODO -->

# Facturar en Shopify POS: CFDI 4.0 en tu tienda física

![Cajero facturando una venta en Shopify POS con CFDI Express](https://videos.acromatico.dev/api/images/assets/PENDIENTE-facturar-shopify-pos.png?w=1600)

En tienda física la factura se pide en caja, con fila detrás. O llega tres días después por WhatsApp. Shopify POS cobra y registra la orden, pero no timbra CFDI.

Con la app [CFDI Express](https://apps.shopify.com/cfdi-express) facturas desde el mismo POS. Esta guía cubre los tres casos de una tienda física: el cliente pide factura al pagar, la pide días después, o no la pide nunca.

## Cómo se factura cada venta de POS

**Respuesta corta:** el cajero factura en caja, el cliente se autofactura después con su número de orden, y lo que nadie facturó va al global a público en general.

| Caso | Dónde se factura | Quién captura |
| --- | --- | --- |
| El cliente pide factura al pagar | **CFDI / Factura**, al terminar la venta en el POS | El cajero |
| El cliente regresa por su factura | **CFDI / Factura (Órdenes)**, en el detalle de la orden en el POS | El cajero |
| El cliente quiere facturar solo | Formulario de autofactura en tu tienda en línea o portal de cuentas | El cliente |
| Nadie pidió factura | Factura global a público en general, en el admin o con Shopify Flow | Tú o la automatización |

## Antes de empezar: activa CFDI Express en el POS

1. Instala [CFDI Express](https://apps.shopify.com/cfdi-express) y termina el [onboarding](/docs/onboarding): RFC, razón social, régimen, código postal, CSD y claves SAT por omisión.
2. En el admin de Shopify abre **Canales de venta → Point of Sale → Configuración → POS apps**.
3. Busca **CFDI Express** y oprime **Agregar**.

El paso a paso con capturas está en [Facturación desde el POS de Shopify](/docs/facturacion-pos-punto-de-venta-shopify). En la pantalla de inicio de la app, la sección **Estado de Formularios de Facturación** te dice si **Formulario de POS** y **Facturar desde órdenes en POS** ya están activos.

Si todavía no implementas Shopify POS en tu tienda, Acromático Development, el equipo detrás de CFDI Express, lo [implementa contigo](https://cal.com/team/acromatico-development/exploration).

## Caso 1: el cliente pide factura al pagar

**Respuesta corta:** al terminar la venta, el cajero abre **CFDI / Factura**, captura los datos fiscales del cliente y oprime **Generar CFDI**.

1. Cobra la venta como siempre en Shopify POS.
2. En la pantalla de confirmación de la venta, abre la acción **CFDI / Factura**.
3. Llena el formulario **Generar CFDI**:
   - **RFC**
   - **Razón Social**
   - **Código Postal** (el del domicilio fiscal del cliente)
   - **Correo electrónico**
   - **Teléfono (WhatsApp, opcional)**
   - **Régimen fiscal**, **método de pago** y **uso del CFDI**, con buscador
4. Oprime **Generar CFDI**.

<!-- TODO: captura — formulario "Generar CFDI" de CFDI / Factura en Shopify POS (iPad) -->
<!-- ENDTODO -->

Lo que hace la app:

- **Precarga datos.** Si la venta está ligada a un cliente que ya facturó antes en tu tienda, el RFC, la razón social, el código postal, el régimen y el correo se llenan solos.
- **Timbra la orden completa.** Los conceptos salen de la orden de Shopify, con las claves SAT de cada producto y los [pedimentos](/blog/factura-con-pedimento-shopify) que ya tenga el producto o la variante. Una línea reembolsada no se factura.
- **Pago en una exhibición.** La factura del POS sale con método `PUE`.
- **Envía el CFDI.** El cliente recibe PDF y XML por correo, salvo que hayas apagado ese envío en [Configuraciones](/docs/configuraciones).

Al cliente pídele **datos fiscales**, no el PDF de la Constancia. Si el SAT rechaza el timbrado, casi siempre es el nombre, el régimen o el código postal. Lo explicamos en [CFDI facturas 4.0](/blog/cfdi-facturas-4-0).

## Caso 2: el cliente regresa por su factura

**Respuesta corta:** busca la orden en el POS, abre **CFDI / Factura (Órdenes)** y factura. Si la orden ya tiene CFDI, ahí mismo lo ves y lo puedes cancelar.

1. En Shopify POS abre **Órdenes** y busca la venta.
2. En el detalle de la orden abre **CFDI / Factura (Órdenes)**.
3. Si la orden no tiene factura, ves el mismo formulario **Generar CFDI** del caso 1.
4. Si ya tiene una factura activa, ves **Esta orden ya tiene una factura activa**, con folio, fecha y receptor.

Desde esa misma pantalla el cajero puede **Cancelar CFDI**. Pide una segunda confirmación y cancela ante el SAT con motivo `02` (comprobante emitido con errores sin relación). Después, la orden queda libre para facturarse otra vez con los datos correctos.

<!-- TODO: captura — "CFDI / Factura (Órdenes)" en el detalle de una orden en Shopify POS, con factura activa y botón "Cancelar CFDI" -->
<!-- ENDTODO -->

Si la factura tiene notas de crédito o complementos de pago activos, la cancelación no procede. Esos casos se resuelven desde el admin: [Notas de crédito](/docs/notas-de-credito) y [Cancelación y acuse](/docs/cancelacion-y-acuse).

## Caso 3: el cliente se autofactura después

**Respuesta corta:** pon en el ticket el camino a tu formulario de facturación. El cliente busca su orden y timbra sin pasar por caja.

Tienes dos opciones:

### Formulario de facturación en tu tienda en línea

El bloque de tema [Formulario de Facturación](/docs/formulario-de-facturacion-theme-block) va en cualquier página de tu tienda en línea, por ejemplo una página «Facturación». El cliente escribe el **número de orden** y el **total**, y si coinciden llena sus datos fiscales. La búsqueda no distingue canal, así que sirve para ventas de POS.

Si la orden ya está facturada, el formulario se lo dice y no timbra otra.

Necesitas el canal de tienda en línea con un tema publicado. El total tiene que ser exacto: avísale al cliente que lo copie del ticket.

### Portal de cuentas de cliente

Si la venta quedó ligada a un cliente con correo y agregaste el formulario a la [página de estado de orden](/docs/facturacion-pagina-estado-de-orden), puede entrar a su portal de cuentas y facturar desde ahí. Aplica la [regla de periodo de facturación](/docs/configuraciones) que configures. La guía completa, incluido el QR en el ticket del POS, está en [Crea un portal de auto-facturación CFDI en 5 minutos](/blog/crea-un-portal-de-auto-facturacion-cfdi-en-5-minutos).

<!-- TODO: captura — ticket de Shopify POS con QR hacia la página de facturación -->
<!-- ENDTODO -->

## Ventas de POS que nadie facturó: factura global

**Respuesta corta:** van a público en general (`XAXX010101000`), una orden por CFDI, a mano desde el admin o automático con Shopify Flow.

En el admin abre la orden y oprime **Facturar al Publico en General**. Para no hacerlo una por una, importa una plantilla de [Shopify Flow](/docs/uso-shopify-flow-automatizaciones-cfdi-express): busca las órdenes pagadas y no facturadas, sin filtrar canal, así que también toma las del POS.

Elige cuándo corre según el plazo que le das al cliente para autofacturarse. El detalle (periodicidad, mes, qué ajustar en la plantilla y qué hacer si el cliente pide factura después) está en [Factura global en Shopify](/blog/factura-global-shopify).

## Errores comunes en caja

- **CP de la sucursal.** El código postal es el del domicilio fiscal del cliente, no el de tu tienda ni el de su casa si no lo actualizó ante el SAT.
- **Uso de CFDI que no va con el régimen.** Si el SAT lo rechaza, revisa la combinación con el cliente.
- **Razón social con «S.A. de C.V.»** u otras abreviaturas que no están en su Constancia.
- **Producto sin clave SAT.** Si un producto no tiene clave, la factura usa la clave por omisión de tu tienda. Configúralas en [Códigos SAT por producto](/docs/codigos-sat-producto).
- **Orden con todo reembolsado.** No hay conceptos que facturar y la app te lo avisa.

## Preguntas frecuentes

### ¿Shopify POS factura CFDI por sí solo?

No. Shopify POS cobra y registra la orden, pero no timbra. Con CFDI Express el cajero genera el CFDI 4.0 dentro del POS.

### ¿Puedo facturar una venta de POS de días anteriores?

Sí. Busca la orden en el POS y abre **CFDI / Factura (Órdenes)**. También puedes facturarla desde el admin, en la app de CFDI Express.

### ¿El cliente puede autofacturar una compra de tienda física?

Sí. Con el bloque **Formulario de Facturación** en tu tienda en línea busca su orden por número y total. Si la venta quedó ligada a su correo, también puede hacerlo desde su portal de cuentas.

### ¿Cómo cancelo una factura desde el POS?

En **CFDI / Factura (Órdenes)**, si la orden tiene factura activa, oprime **Cancelar CFDI** y confirma. Se cancela ante el SAT con motivo 02 y la orden queda libre para facturarse otra vez.

### ¿Qué hago con las ventas de POS que nadie facturó?

Factúralas a público en general. Con CFDI Express lo haces desde el admin o con plantillas de Shopify Flow que timbran las órdenes pagadas y no facturadas, incluidas las del POS.

### ¿El cliente recibe la factura por correo?

Sí. CFDI Express manda el PDF y el XML al correo que captura el cajero, salvo que hayas desactivado ese envío en Configuraciones.

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "¿Shopify POS factura CFDI por sí solo?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No. Shopify POS cobra y registra la orden, pero no timbra. Con la app CFDI Express el cajero genera el CFDI 4.0 dentro del POS."
      }
    },
    {
      "@type": "Question",
      "name": "¿Puedo facturar una venta de Shopify POS de días anteriores?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Sí. Busca la orden en el POS y abre la acción CFDI / Factura (Órdenes). También puedes facturarla desde el admin de Shopify, en la app de CFDI Express."
      }
    },
    {
      "@type": "Question",
      "name": "¿El cliente puede autofacturar una compra de tienda física?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Sí. Con el bloque Formulario de Facturación de CFDI Express en tu tienda en línea, el cliente busca su orden por número y total. Si la venta quedó ligada a su correo, también puede hacerlo desde su portal de cuentas."
      }
    },
    {
      "@type": "Question",
      "name": "¿Cómo cancelo una factura desde Shopify POS?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "En CFDI / Factura (Órdenes), si la orden tiene factura activa, oprime Cancelar CFDI y confirma. Se cancela ante el SAT con motivo 02 y la orden queda libre para facturarse otra vez."
      }
    },
    {
      "@type": "Question",
      "name": "¿Qué hago con las ventas de POS que nadie facturó?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Factúralas a público en general. Con CFDI Express lo haces desde el admin o con plantillas de Shopify Flow que timbran las órdenes pagadas y no facturadas, incluidas las de Shopify POS."
      }
    },
    {
      "@type": "Question",
      "name": "¿El cliente recibe la factura de POS por correo?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Sí. CFDI Express manda el PDF y el XML al correo que captura el cajero, salvo que el comercio haya desactivado ese envío."
      }
    }
  ]
}
</script>

## Instala CFDI Express y factura desde la caja

1. Instala [CFDI Express en Shopify](https://apps.shopify.com/cfdi-express).
2. Agrega la app en **Point of Sale → Configuración → POS apps**.
3. Haz una venta de prueba y factúrala con **CFDI / Factura**.
4. Publica tu formulario de autofactura y decide cómo cierras el global.

¿Quieres verlo en tu tienda antes de capacitar a tus cajeros? [Agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).
