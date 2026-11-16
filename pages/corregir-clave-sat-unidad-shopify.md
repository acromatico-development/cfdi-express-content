---
title: "Clave SAT y unidad en Shopify: corrígelas antes de timbrar"
description: "En Shopify ya puedes corregir el Código SAT y la Unidad SAT de cada concepto antes de timbrar, sin editar el producto. Cómo hacerlo con CFDI Express."
author: "Rafael González"
date: "2026-11-16"
keywords: "clave SAT Shopify, clave de producto o servicio, clave de unidad SAT, ClaveProdServ, ClaveUnidad, corregir clave SAT factura, CFDI 4.0, CFDI Express"
---
# Cómo corregir la clave de producto y unidad SAT antes de timbrar en Shopify

<!-- TODO: imagen de portada (frontmatter image) -->

Cada concepto de un CFDI 4.0 lleva una **clave de producto o servicio** (ClaveProdServ) y una **clave de unidad** (ClaveUnidad) del catálogo del SAT. En Shopify, esas claves salen de la configuración de cada producto. Si al facturar una orden ves que una no corresponde, desde la versión **3.32.0** de [CFDI Express](https://apps.shopify.com/cfdi-express) la corriges en la misma factura, sin editar el producto.

## Cómo cambiar la clave en la factura

**Respuesta corta:** en la tabla **Productos** de la orden, escribe la clave nueva en **Código SAT** o **Unidad SAT** y da clic en **Generar CFDI**.

1. En la app, abre la orden que vas a facturar desde el buscador de [órdenes](/docs/ordenes-estatus).
2. Llena los datos fiscales del cliente, como siempre.
3. En la tabla **Productos**, cada concepto tiene los campos **Código SAT** y **Unidad SAT**, ya llenos. La fila de **Envío** también.
4. Cambia la clave que no corresponde. Si no sabes cuál es, búscala en el [buscador de códigos SAT](/buscador-de-codigos-sat-productos-servicios).
5. Da clic en **Generar CFDI**.

<!-- TODO: captura — tabla Productos con un Código SAT corregido -->
![TODO: Código SAT y Unidad SAT editables en la factura de una orden](TODO-screenshot)
<!-- ENDTODO -->

El detalle de la pantalla está en [Facturación desde el admin de Shopify](/docs/facturacion-desde-admin-shopify).

## De dónde salen las claves que ves

**Respuesta corta:** del título del producto, de su configuración en CFDI Express o de los predeterminados de tu tienda, en ese orden.

1. La etiqueta en el título del producto o la variante, como `CFDI:78102200,E48`.
2. El código y la unidad guardados en el producto dentro de CFDI Express.
3. Los predeterminados de tu tienda en [Configuraciones](/docs/configuraciones). El envío usa el código y la unidad de envío predeterminados.

Si no tocas los campos, la factura sale con esas claves.

## Qué revisa la app antes de timbrar

**Respuesta corta:** el formato. El Código SAT lleva 8 dígitos y la Unidad SAT es una clave de hasta 3 letras o números, como `H87` (pieza) o `E48` (unidad de servicio).

- Mientras escribes, el Código SAT solo acepta números y la Unidad SAT se pasa a mayúsculas.
- Al dar clic en **Generar CFDI**, si una clave no tiene el formato, el campo se marca (**8 dígitos** o **Clave no válida**) y la factura no se timbra hasta que la corrijas.
- La app no consulta el catálogo del SAT: una clave de 8 dígitos que no existe pasa esta revisión. Confírmala antes en el [buscador](/buscador-de-codigos-sat-productos-servicios).

## Lo que no cambia

- **Tu producto.** La corrección solo vale para esa factura. Si la clave correcta es otra para siempre, cámbiala en [Códigos del SAT por producto](/docs/codigos-sat-producto) o con la [carga masiva por CSV](/docs/carga-masiva-productos-csv).
- **Una factura ya timbrada.** Si la orden tiene un CFDI vigente, los campos quedan bloqueados y muestran las claves con las que se timbró. Para cambiarlas hay que [cancelar](/docs/cancelacion-y-acuse) y volver a facturar.
- **La autofacturación.** Tus clientes no pueden cambiar las claves al pedir su factura en la Thank You Page, el estado de la orden, el bloque del tema o el [POS](/docs/facturacion-pos-punto-de-venta-shopify). Ahí se usan las del producto.
- **La factura global mensual.** Va con la clave `01010101` y la unidad `ACT` en cada concepto, sin estos campos. Está en los planes Plus, Pro y Enterprise: [Facturación global mensual](/docs/facturacion-global-mensual).

La edición por concepto está en todos los planes.

## Preguntas frecuentes

**¿Corregir la clave en la factura cambia el producto en Shopify?**
No. El cambio solo vale para esa factura. Para que las siguientes facturas usen otra clave, cámbiala en la configuración del producto en CFDI Express.

**¿Puedo cambiar la clave de unidad del envío?**
Sí. La fila de envío también tiene Código SAT y Unidad SAT. Vienen con el código y la unidad de envío predeterminados de tu tienda.

**¿La app valida que la clave exista en el catálogo del SAT?**
No. Revisa que el Código SAT tenga 8 dígitos y que la Unidad SAT tenga hasta 3 letras o números. Para confirmar que la clave existe, usa el buscador de códigos SAT de CFDI Express.

**¿Mi cliente puede elegir la clave al autofacturarse?**
No. Solo tú puedes cambiarla desde la app en el admin de Shopify. La autofacturación y el POS usan las claves del producto.

**¿Puedo cambiar la clave de una factura ya timbrada?**
No. Con un CFDI vigente los campos muestran las claves timbradas y no se pueden editar. Hay que cancelar la factura y volver a facturar la orden.

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {"@type": "Question", "name": "¿Corregir la clave en la factura cambia el producto en Shopify?", "acceptedAnswer": {"@type": "Answer", "text": "No. El cambio solo vale para esa factura. Para que las siguientes facturas usen otra clave, cámbiala en la configuración del producto en CFDI Express."}},
    {"@type": "Question", "name": "¿Puedo cambiar la clave de unidad del envío?", "acceptedAnswer": {"@type": "Answer", "text": "Sí. La fila de envío también tiene Código SAT y Unidad SAT. Vienen con el código y la unidad de envío predeterminados de tu tienda."}},
    {"@type": "Question", "name": "¿La app valida que la clave exista en el catálogo del SAT?", "acceptedAnswer": {"@type": "Answer", "text": "No. Revisa que el Código SAT tenga 8 dígitos y que la Unidad SAT tenga hasta 3 letras o números. Para confirmar que la clave existe, usa el buscador de códigos SAT de CFDI Express."}},
    {"@type": "Question", "name": "¿Mi cliente puede elegir la clave al autofacturarse?", "acceptedAnswer": {"@type": "Answer", "text": "No. Solo tú puedes cambiarla desde la app en el admin de Shopify. La autofacturación y el POS usan las claves del producto."}},
    {"@type": "Question", "name": "¿Puedo cambiar la clave de una factura ya timbrada?", "acceptedAnswer": {"@type": "Answer", "text": "No. Con un CFDI vigente los campos muestran las claves timbradas y no se pueden editar. Hay que cancelar la factura y volver a facturar la orden."}}
  ]
}
</script>

¿Quieres verlo sobre una orden de tu tienda? Instala [CFDI Express en Shopify](https://apps.shopify.com/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).
