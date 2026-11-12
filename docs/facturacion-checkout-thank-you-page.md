# Facturación desde la Thank You Page del Checkout de Shopify

Uno de los formularios más utilizados en CFDI Express es el de la Thank You Page del Checkout de Shopify. Para poder activar este formulario, sigue los siguientes pasos:

1. Después de haber instalado CFDI Express, ve a las configuraciones generales de tu tienda. Da click en "Checkout". Ahí te aparecerá una ventana con los diferentes checkouts que tienes disponibles para configurar.

2. En tu check out, que esté publicado, da click en "Customize" o "Personalizar".
[Personalización de Checkout](https://cdn.shopify.com/s/files/1/0804/5540/1763/files/Acr_2025-09-22_at_16.17.05.png?v=1758579434)

3. Dentro del personalizador de Checkouts, da clic en el menú que se encuentra arriba en el centro de la página donde dice Checkout. Se desplegará una lista de pantallas, entre ellas la Thank You Page. Da clic en la Thank You Page.
[Lista de páginas para editar](https://cdn.shopify.com/s/files/1/0804/5540/1763/files/Acr_2025-09-22_at_16.17.45.png?v=1758579537)

4. Dentro de la página de agradecimiento, en la barra de navegación izquierda, da click en "Secciones" y, en la sección "Main", da click en "Agregar block". Ahí aparecerá el formulario de facturación de CFDI Express.
[Bloque de Formulario de Thankyou Page](https://cdn.shopify.com/s/files/1/0804/5540/1763/files/Acr_2025-09-22_at_16.20.15.png?v=1758579662)

5. Arrastre el formulario a la posición que desees para colocarlo.

El **Formulario de autofacturación** no pide el número de pedimento. Si el producto o la variante ya lo tienen, la factura lo incluye. Guía: [Pedimentos de importación](/docs/pedimentos-de-importacion).

![Posicionamiento de formulario](https://cdn.shopify.com/s/files/1/0804/5540/1763/files/Acr_2025-09-22_at_16.22.08.png?v=1758579760)

- Embed:
  <iframe width="100%" style="aspect-ratio:16/9;" src="https://www.youtube.com/embed/iKweIA7HyHI?si=SG-PAU9U4-CwvAZM&amp;start=254" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
- Link:
  https://youtu.be/iKweIA7HyHI?si=psdb9qSMODcNfRrs

## Órdenes en dólares

Desde la versión **3.29.0**, el formulario también aparece en las órdenes cobradas en **dólares (USD)**, en la página de gracias y en la [página de estado de orden](/docs/facturacion-pagina-estado-de-orden). La factura sale en USD cuando tu tienda factura en dólares: la moneda de la tienda es el dólar, o activaste **Facturar en dólares (USD) las órdenes cobradas en dólares** en [Configuraciones](/docs/configuraciones). Si no, sale en pesos.

Si la moneda de la orden no se puede facturar, el cliente ve un aviso con el motivo y la indicación de contactar a la tienda.

## Pedido que ya está en la factura global

Desde la versión **3.27.0**, si el cliente intenta facturar un pedido que ya está en la factura global del mes, el formulario le explica que su compra ya está facturada y que tú puedes emitirle la factura a su nombre desde la app. No timbra otra.

La factura global es de los planes **Plus**, **Pro** y **Enterprise**. En el plan **Básico** cada venta a público en general es un CFDI por pedido. Cómo emitir la factura a nombre del cliente está en [Notas de crédito](/docs/notas-de-credito).
