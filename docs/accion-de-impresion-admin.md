# Acción de Impresión de CFDI en el Admin de Shopify

CFDI Express agrega una **acción de impresión** en la página de detalle de la orden dentro del Admin de Shopify, para que puedas imprimir el PDF del CFDI directamente sin abrir el app.

## Cómo usarla

1. En el Admin de Shopify abre la orden que ya tiene un CFDI timbrado.
2. En la zona de acciones de la orden, busca la acción **"Imprimir CFDI"** (u "Print CFDI").
3. Si la orden ya cuenta con un CFDI (`url_cfdi` en metafield), se abrirá el **PDF del CFDI** listo para imprimir o guardar.
4. Si la orden **no** tiene un CFDI generado, la acción mostrará el mensaje "Esta orden no tiene un CFDI generado".

## Detrás de escena

- La acción consulta el metafield `$cfdiexpress.url_cfdi` de la orden.
- Si existe, CFDI Express extrae el **PDF** del ZIP almacenado (PDF + XML) mediante el endpoint `/api/print-cfdi` (autenticado con la sesión del admin) y lo sirve inline para impresión.
- No abre el editor del app; es un acceso directo al PDF desde el Admin de Shopify.

> Para regenerar o cancelar el CFDI usa el [flujo de facturación del admin](/docs/facturacion-desde-admin-shopify). La acción de impresión es de sólo lectura.