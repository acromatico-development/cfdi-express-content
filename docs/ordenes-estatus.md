# Página de lista de ordenes con estatus

La página principal que se abre al iniciar el app de CFDI Express es la lista de ordenes con estatus. La tabla está páginada, cáda página tiene 20 ordenes ordenadas de la más nueva a la más antigua. Los encabezados de la tabla son: 

- Orden: El número de orden de la tienda de Shopify (Ej. #1024)
- Fecha: La fecha en la que se generó la orden en Shopify
- Cliente: El correo del cliente que generó la orden de Shopify
- Total: El total de la orden de compra de Shopify
- Estado de Orden: El estádo de la orden, puede estár "Activa" o "Cerrada"
- Estado de Pago: El estado del pago de la orden, puede estar "Pendiente", "Parcial" o "Pagado"
- Estatus de Factura: El estado de facturación de la orden, puede estár "Pendiente", "Facturado" o "Cancelado"

## Filtrado de Ordenes

Inicialmente la tabla muestra todas las ordenes pagadas en orden de más nuevo a más antiguo. Los filtros dan la posibilidad de mostrar también las ordenes no pagadas y cuenta con un input de texto para filtrar por correo de cliente o número de orden. La siguiente imágen muestra los filtros disponibles:

![Filtros de Tabla de Ordenes CFDI Express](https://cdn.shopify.com/s/files/1/0804/5540/1763/files/Acr_2025-07-17_at_17.07.11_2x_c424e145-809f-479c-9587-509c7d0ef3ad.png?v=1752793652)