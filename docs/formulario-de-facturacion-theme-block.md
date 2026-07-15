# Formulario de Facturación (Theme App Block)

Además de la [Thank You Page](/docs/facturacion-checkout-thank-you-page) y la [página de estado de orden](/docs/facturacion-pagina-estado-de-orden), CFDI Express ofrece un **bloque de tema** (Theme App Block) llamado **"Formulario de Facturación"** que puedes agregar a **cualquier página** de tu tienda para que tus clientes soliciten su factura. Es la opción ideal cuando no usas el checkout estándar de Shopify o quieres un punto de facturación dedicado.

## ¿Cómo funciona?

El cliente entra a la página donde agregaste el bloque y:

1. **Busca su orden** por **número de orden** y **total** (con redondeo a ±$0.01 como prueba de propiedad). Esto garantiza que solo el cliente que conoce su orden pueda facturarla.
2. Si la orden es válida y estás dentro del [periodo de facturación](/docs/configuraciones), aparece el formulario de datos fiscales.
3. El cliente captura RFC, razón social, régimen, código postal, uso CFDI, método de pago, dirección completa (si lo habilitaste) y la CSF (si `enableCsfUpload` está activo).
4. Al enviar, CFDI Express timbra el CFDI (o dispara Modo Flow) y envía el CFDI por correo al cliente.

<!-- TODO: captura — formulario de facturación visto por el cliente en el storefront (búsqueda por orden# + total) -->
![TODO: Formulario de Facturación en el storefront](TODO-screenshot)

> El formulario pasa por el **App Proxy** de Shopify, que verifica la firma HMAC para asegurar que las peticiones vienen genuinamente de tu tienda.

## Cómo agregar el bloque a tu tienda

1. En el Admin de Shopify entra a **Tienda virtual → Themes** (o "Plantillas").
2. Haz clic en **Personalizar** en tu tema publicado.
3. Ve a la página donde quieres el formulario (por ejemplo, una página nueva "Solicitar Factura").
4. Haz clic en **Agregar sección / bloque** y busca **"Formulario de Facturación"** (de CFDI Express).

<!-- TODO: captura — selector de bloques del tema mostrando "Formulario de Facturación" de CFDI Express -->
![TODO: Selector de bloques con Formulario de Facturación](TODO-screenshot)

5. Ajusta los **settings del bloque**:
    - **Título de la sección**
    - **Título de la búsqueda**
    - **Texto de facturación**
    - **Color del botón**
    - **Color de texto del botón**

<!-- TODO: captura — panel de settings del bloque (título, colores, textos) -->
![TODO: Settings del bloque Formulario de Facturación](TODO-screenshot)

6. Guarda. El formulario quedará visible en esa página.

## Diferencia con las extensiones de Checkout

| Aspecto | Bloque de Tema | Extensiones de Checkout |
|---|---|---|
| Ubicación | Cualquier página de tu tienda | Thank You Page / Estado de orden |
| Requisitos para el cliente | Sabe su número de orden y total | Ya completó el checkout |
| Auth | App Proxy (HMAC) | Session token de checkout |
| Periodo de facturación | Sí, se valida | Sí, se valida |

> Si usas el checkout estándar de Shopify, la [Thank You Page](/docs/facturacion-checkout-thank-you-page) suele ser más cómoda para el cliente. El bloque de tema es útil cuando clientes antiguos solicitan factura posteriormente o cuando llevas flujos fuera del checkout convencional.

## Consideraciones

- El bloque requiere que el cliente conozca el **total exacto** de su orden. Asegúrate de informarles que deben capturarlo con la precisión correcta.
- Si una orden ya está facturada, el formulario le indicará al cliente y le mostrará su folio anterior con su URL de descarga.
- El formulario respeta el [Modo Flow](/docs/modo-flow-cfdi-express-para-usar-tu-propio-pac-de-facturacion) si lo tienes activo.

### Video Tutorial

<!-- TODO: grabar tutorial del Formulario de Facturación (Theme App Block) -->

- Embed:
  <iframe width="100%" style="aspect-ratio:16/9;" src="https://www.youtube.com/embed/TODO_VIDEO_ID?si=TODO" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
- Link:
  https://www.youtube.com/watch?v=TODO_VIDEO_ID