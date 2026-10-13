---
title: "Portal de autofacturación para Shopify | CFDI Express"
description: "Portal de autofacturación en Shopify: tu cliente genera su CFDI 4.0 desde la página de su pedido, con RFC, régimen y CP validados. Sin portal aparte."
image: "https://videos.acromatico.dev/api/images/assets/ed3a2e3d-fc9b-4021-a336-5c8a1898f904.png?w=1600&format=webp"
author: "Rafael González"
date: "2026-10-12"
keywords: "autofacturación, portal de autofacturación, autofactura shopify, portal de facturación para clientes, formulario de facturación shopify, CFDI 4.0, CFDI Express"
---
# Portal de autofacturación para tu tienda Shopify

Tu cliente no debería escribirte para pedir su factura. Con [CFDI Express](https://apps.shopify.com/cfdi-express), el formulario de autofacturación aparece en la página de **gracias por tu compra** y en la página de **estado del pedido** de su cuenta de cliente. Llena sus datos fiscales, da clic y el CFDI 4.0 le llega a su correo.

No tienes que montar un portal aparte ni pedirle el número de orden: el formulario ya sabe de qué pedido se trata.

![Formulario de autofacturación CFDI Express en la página de estado del pedido de Shopify, con campos de razón social, RFC, régimen fiscal, código postal y uso del CFDI](https://videos.acromatico.dev/api/images/assets/ed3a2e3d-fc9b-4021-a336-5c8a1898f904.png?w=1600&format=webp)

## ¿Qué es un portal de autofacturación?

**Respuesta corta:** es el lugar donde tu cliente genera su propia factura sin que tú la hagas a mano.

En CFDI Express ese portal vive dentro de Shopify, en dos lugares:

- La página de gracias, justo después de pagar.
- La página de estado del pedido, en la cuenta del cliente, por si quiere facturar después.

## Cómo funciona para tu cliente

1. Entra a la página de su pedido.
2. Si la orden está pagada y todavía no tiene factura, ve el aviso **«¿Requieres Factura (CFDI)?»** con el formulario.
3. Llena sus datos y da clic en **«Generar CFDI y enviar por correo»**.
4. Ve el mensaje **«Factura generada»** y recibe en el correo que capturó un zip con el PDF y el XML.
5. Si regresa después, ve **«Facturación CFDI»** con un enlace para **descargar** su factura.

### Qué datos pide el formulario

| Campo | Obligatorio | Nota |
| --- | --- | --- |
| Razón social | Sí | Se pasa a mayúsculas y sin acentos (la Ñ se respeta), como la pide el SAT |
| RFC | Sí | Se valida el formato de persona física o moral |
| Régimen fiscal | Sí | Catálogo del SAT |
| Código postal | Sí | 5 dígitos |
| Método de pago | Sí | Catálogo del SAT |
| Uso del CFDI | Sí | Catálogo del SAT |
| Correo electrónico | Sí | Se llena solo con el correo de la compra |
| Teléfono (WhatsApp) | No | Dato de contacto opcional; se puede ocultar desde la configuración |
| Domicilio completo | Opcional | Calle, número, colonia, ciudad, municipio y estado, si lo activas |
| Constancia de Situación Fiscal (PDF) | Opcional | Si la activas, el cliente la sube en el formulario |

Si al cliente le falta un dato, el formulario le dice exactamente cuál. Si ya facturó antes en tu tienda, su razón social, RFC, régimen y código postal pueden aparecer precargados.

## Qué ve tu cliente en cada caso

El formulario cambia según el estado de la orden:

- **Orden pagada y sin factura:** aparece el formulario.
- **Orden sin pagar por completo:** no puede facturar. Ve un aviso de que la orden debe estar totalmente pagada.
- **Fuera de tu periodo de facturación:** ve **«Período de facturación CFDI expirado»** y que contacte a soporte.
- **Ya facturada a su nombre:** ve que el CFDI ya se generó y un enlace para descargarlo.
- **Ya incluida en la factura a público en general:** ve que el comercio cerró esa orden y que contacte a soporte si necesita su CFDI.
- **Orden en moneda distinta a pesos (MXN):** el formulario no se muestra.

## Lo que configuras tú como comercio

- **Periodo de facturación:** sin límite, hasta fin del mes de la compra o un número de días después de la compra.
- **Domicilio completo:** pídelo solo si tu contador lo necesita.
- **Constancia de Situación Fiscal:** actívala si quieres que el cliente la suba.
- **Teléfono (WhatsApp):** muestra u oculta el campo opcional de teléfono.
- **Modo Shopify Flow:** en lugar de timbrar, CFDI Express dispara un Shopify Flow con todos los datos de facturación que capturó tu cliente. Sirve si quieres que esa información llegue a otro sistema, como tu ERP, y que ahí se emita la factura.

Para ponerlo en tu tienda, sigue el [tutorial para crear tu portal de autofacturación en 5 minutos](/blog/crea-un-portal-de-auto-facturacion-cfdi-en-5-minutos).

## ¿Y las órdenes que nadie facturó?

Esas van a la [factura global](/blog/factura-global-shopify), a público en general (`XAXX010101000`). En CFDI Express la factura global **siempre es mensual (04)**. Con tu periodo de facturación decides hasta cuándo puede el cliente facturar antes de que la orden entre al global.

Si el cliente pide su factura después del global, tendrás que cancelar y volver a facturar. Lo explicamos en [cancelación y acuse](/docs/cancelacion-y-acuse) y en la guía de [acuse de cancelación](/blog/acuse-de-cancelacion).

## Precio

El plan Pro cuesta **$250 USD al mes** y ya incluye timbres (5,000 CFDIs). Todos los planes están en [planes y precios](/docs/planes-y-precios).

## Preguntas frecuentes

**¿Mi cliente necesita crear cuenta para facturar?**
Puede facturar en la página de gracias justo después de pagar. Para facturar después, tiene que iniciar sesión en su cuenta de cliente de tu tienda y entrar a la página de estado de su pedido.

**¿Tengo que pedirle el número de orden?**
No. El formulario está en la página del propio pedido.

**¿Por qué un cliente no ve el formulario?**
Puede que la orden no esté pagada por completo, que ya pasó tu periodo de facturación, que ya esté facturada o que no esté en pesos mexicanos.

**¿Cómo recibe la factura mi cliente?**
Le llega al correo que capturó un archivo zip con el PDF y el XML, y también la puede descargar desde la página del pedido.

**¿Puedo limitar hasta cuándo se puede facturar?**
Sí: sin límite, hasta fin de mes o X días después de la compra.

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {"@type": "Question", "name": "¿Tengo que pedirle el número de orden a mi cliente?", "acceptedAnswer": {"@type": "Answer", "text": "No. El formulario de autofacturación está en la página del propio pedido."}},
    {"@type": "Question", "name": "¿Por qué un cliente no ve el formulario de autofacturación?", "acceptedAnswer": {"@type": "Answer", "text": "Puede que la orden no esté pagada por completo, que ya pasó el periodo de facturación de la tienda, que ya esté facturada o que no esté en pesos mexicanos."}},
    {"@type": "Question", "name": "¿Puedo limitar hasta cuándo se puede facturar?", "acceptedAnswer": {"@type": "Answer", "text": "Sí. CFDI Express permite sin límite, hasta fin del mes de la compra o un número de días después de la compra."}}
  ]
}
</script>

## Activa la autofacturación en tu tienda

1. Instala [CFDI Express en Shopify](https://apps.shopify.com/cfdi-express) y completa el [onboarding](/docs/onboarding).
2. Agrega el bloque del formulario siguiendo el [tutorial](/blog/crea-un-portal-de-auto-facturacion-cfdi-en-5-minutos).
3. Haz una compra de prueba y factúrala tú mismo.

¿Lo quieres ver en tu tienda? [Agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).
