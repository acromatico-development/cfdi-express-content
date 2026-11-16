# Facturación desde el admin de Shopify

Para poder completar una factura desde el admin de Shopify, sigue los siguientes pasos:

1. Abre la aplicación de CFDI Express desde el admin de Shopify en apps -> CFDI Express - Facturación
![Apps CFDI Express - Facturación](https://cdn.shopify.com/s/files/1/0804/5540/1763/files/Acr_2025-09-22_at_15.42.25.png?v=1758577368)

2. Busca la orden que deseas facturar en el buscador de órdenes de CFDI Express y da click en la orden que desees facturar. Para más información de cómo usar el buscador de órdenes, consulta nuestra documentación de [órdenes y estatus](/docs/ordenes-estatus).

3. Dentro de la orden a facturar, llena los datos de facturación con los datos fiscales del cliente. Da clic en facturar. Puedes facturar al público en general llenando los datos de facturación de público en general:  RFC genérico XAXX010101000 y el nombre "PÚBLICO EN GENERAL", con régimen fiscal "616 – Sin Obligaciones Fiscales" y el código postal del emisor.

Si el producto o la variante ya tienen pedimentos de importación, la tabla **Productos** muestra la columna **Pedimentos**. Esa columna no aparece si ningún concepto los trae. La guía está en [Pedimentos de importación](/docs/pedimentos-de-importacion).

Desde la versión **3.27.0**, la vista previa de la factura incluye todos los artículos del pedido, hasta **1,000** (el máximo que permite el SAT), con el total correcto. Antes la vista se quedaba en los primeros 50 y, si había descuentos, podía mostrar conceptos en $0 o un total negativo.

Antes de timbrar, la app revisa si el pedido ya tiene una factura vigente. Si ya la tiene, te muestra esa factura en lugar de timbrar otra, aunque el timbrado tarde o alguien vuelva a dar clic.
![Datos de Facturación](https://cdn.shopify.com/s/files/1/0804/5540/1763/files/Acr_2025-09-22_at_15.50.44.png?v=1758577857)

### Cambiar el Código SAT y la Unidad SAT de un concepto

Desde la versión **3.32.0**, en la tabla **Productos** del formulario de facturación, las columnas **Código SAT** (clave de producto o servicio) y **Unidad SAT** (clave de unidad) son campos que puedes editar en cada concepto antes de timbrar, igual que el SKU o número de serie. También la fila de **Envío**. Está en todos los planes.

<!-- TODO: captura — tabla Productos con los campos Código SAT y Unidad SAT editables -->
![TODO: Campos Código SAT y Unidad SAT en la tabla Productos](TODO-screenshot)
<!-- ENDTODO -->

- **De dónde salen los valores.** Los campos ya vienen llenos con las claves que la app usaría sin cambios. En cada producto, primero la etiqueta del título (`CFDI:78102200,E48`), luego los códigos guardados en el producto dentro de CFDI Express ([Códigos del SAT por producto](/docs/codigos-sat-producto)) y, si no hay, los predeterminados de tu tienda en [Configuraciones](/docs/configuraciones). La fila de envío usa el código y la unidad de envío predeterminados. Si no cambias nada, la factura sale igual que antes.
- **Solo vale para esa factura.** El cambio no se guarda en el producto de Shopify ni en CFDI Express. Si la clave correcta es otra para siempre, cámbiala en [Códigos del SAT por producto](/docs/codigos-sat-producto).
- **Qué revisa la app.** El Código SAT solo acepta números, hasta 8, y la Unidad SAT se pasa a mayúsculas con hasta 3 letras o números (por ejemplo `H87` o `E48`). Al dar clic en **Generar CFDI**, si un Código SAT no tiene 8 dígitos o una unidad no es válida, el campo se marca (**8 dígitos** o **Clave no válida**), aparece el aviso `Revisa los códigos SAT: la clave de producto lleva 8 dígitos y la unidad es una clave como H87 o E48.` y no se timbra hasta que lo corrijas.
- **La app revisa el formato, no el catálogo.** Una clave de 8 dígitos que no existe en el catálogo del SAT pasa esta revisión. Búscala antes en el [buscador de códigos SAT](/buscador-de-codigos-sat-productos-servicios).
- **Con una factura vigente.** Si la orden ya tiene un CFDI vigente, los campos quedan deshabilitados y muestran las claves con las que se timbró. Si la orden entró en una factura global, ves el concepto de esa global.
- **Botones que reinician el formulario.** **Facturar al Publico en General**, **Facturar al Publico en General Extranjero** y **Descartar** regresan las claves a las originales.
- **Solo desde el admin.** Tus clientes no pueden cambiar las claves al pedir su factura: la [Thank You Page](/docs/facturacion-checkout-thank-you-page), el [estado de la orden](/docs/facturacion-pagina-estado-de-orden), el [bloque del tema](/docs/formulario-de-facturacion-theme-block), el [POS](/docs/facturacion-pos-punto-de-venta-shopify) y la acción **Crear CFDI** de [Shopify Flow](/docs/uso-shopify-flow-automatizaciones-cfdi-express) usan las claves del producto.
- **Factura global mensual.** No usa estos campos. Cada concepto de la [factura global mensual](/docs/facturacion-global-mensual) (planes Plus, Pro y Enterprise) va con la clave `01010101` y la unidad `ACT`.

#### Preguntas frecuentes sobre el Código SAT y la Unidad SAT

**¿Cambiar la clave en la factura modifica mi producto?**
No. El cambio solo vale para esa factura. Para que todas las facturas de ese producto usen otra clave, cámbiala en Códigos del SAT por producto.

**¿Mi cliente puede cambiar la clave al pedir su factura?**
No. Solo tú puedes cambiarla desde la app en el admin de Shopify. La autofacturación, el bloque del tema, el POS y Shopify Flow usan las claves del producto.

**¿La app revisa que la clave exista en el catálogo del SAT?**
No. Revisa que el Código SAT tenga 8 dígitos y que la Unidad SAT tenga hasta 3 letras o números. Para confirmar que la clave existe, usa el buscador de códigos SAT de CFDI Express.

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {"@type": "Question", "name": "¿Cambiar la clave en la factura modifica mi producto?", "acceptedAnswer": {"@type": "Answer", "text": "No. El cambio solo vale para esa factura. Para que todas las facturas de ese producto usen otra clave, cámbiala en Códigos del SAT por producto."}},
    {"@type": "Question", "name": "¿Mi cliente puede cambiar la clave al pedir su factura?", "acceptedAnswer": {"@type": "Answer", "text": "No. Solo tú puedes cambiarla desde la app en el admin de Shopify. La autofacturación, el bloque del tema, el POS y Shopify Flow usan las claves del producto."}},
    {"@type": "Question", "name": "¿La app revisa que la clave exista en el catálogo del SAT?", "acceptedAnswer": {"@type": "Answer", "text": "No. Revisa que el Código SAT tenga 8 dígitos y que la Unidad SAT tenga hasta 3 letras o números. Para confirmar que la clave existe, usa el buscador de códigos SAT de CFDI Express."}}
  ]
}
</script>

### Enviar la factura por WhatsApp

En los datos de facturación aparece el campo **Teléfono (WhatsApp, opcional)**. Si lo llenas, al timbrar el CFDI le enviamos al cliente un mensaje de WhatsApp con el folio, el nombre de tu tienda y la liga para descargar su factura. El envío por correo sigue igual.

- Escribe el número a 10 dígitos (por ejemplo `5512345678`). También se acepta con lada de país (`52` o `521`). Para números de otros países usa el formato internacional con lada.
- Si el número no es válido, la factura se timbra de todos modos y solo se omite el WhatsApp.
- El mismo campo aparece en el formulario de autofacturación de la [página de agradecimiento y de estado de orden](/docs/facturacion-checkout-thank-you-page).
- Si no quieres enviar WhatsApps, activa **Omitir envío de WhatsApp con CFDI** en [Configuraciones](/docs/configuraciones) y el campo deja de mostrarse. Puedes usar [Shopify Flow](/docs/uso-shopify-flow-automatizaciones-cfdi-express) para mandar tus propios mensajes.

4. Para cancelar una factura después de haberla timbrado, puedes dar clic en el botón "Cancelar Factura" que se encuentra arriba a la derecha dentro de la orden ya facturada. Por ahora se cancela con el motivo `02` (comprobante emitido con errores sin relación); elegir el motivo se está trabajando. Esto generará una constancia de cancelación que podrás descargar en todo momento desde la misma orden, y podrás nuevamente facturar la orden con otros datos fiscales.
![Descarga de acuses de cancelación](https://cdn.shopify.com/s/files/1/0804/5540/1763/files/Acr_2025-09-22_at_15.51.47.png?v=1758577929)

Puedes ver nuestro video introductorio si lo prefieres.

- Embed:
  <iframe width="100%" style="aspect-ratio:16/9;" src="https://www.youtube.com/embed/G2_Wl8bJqus?si=QvoOtfC8A5_In0Rr" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
- Link:
  https://www.youtube.com/watch?v=G2_Wl8bJqus
