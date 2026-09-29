---
title: "Factura con pedimento en Shopify: cómo ponerlo en el CFDI"
description: "Cómo poner el número de pedimento en una factura de Shopify si vendes mercancía de importación de primera mano: producto, variante y factura."
image: "https://videos.acromatico.dev/api/images/assets/1ba3e371-618e-4ab3-91c9-b716a968a0fe.png"
author: "Rafael González"
date: "2026-09-29"
keywords: "factura con pedimento Shopify, cómo poner número de pedimento en factura, pedimento aduanal Shopify, número de pedimento CFDI, información aduanera, CFDI Express, mercancía de importación"
---
# Factura con pedimento en Shopify: cómo ponerlo en el CFDI

![Comerciante con cajas de importación y una factura con número de pedimento](https://videos.acromatico.dev/api/images/assets/1ba3e371-618e-4ab3-91c9-b716a968a0fe.png?w=1600)

Si un cliente te pide **factura con pedimento** y vendes en Shopify, el número no se captura en la cabecera del CFDI. Va en el producto importado, y de ahí sale en cada concepto de la factura.

Esta guía es para la app de Shopify [CFDI Express](https://apps.shopify.com/cfdi-express): en qué pantalla se escribe, cuándo gana la variante y qué ves en el XML y en el PDF. El paso a paso largo está en [Pedimentos de importación](/docs/pedimentos-de-importacion).

Si no ves el bloque **Pedimentos de importación** en el producto, actualiza la app. El campo solo aparece cuando el proveedor de facturación es CFDI Express. Si la tienda sigue en Facturama, no se muestra. No está atado a un plan.

## ¿Qué es el número de pedimento y cuándo lo pones?

**Respuesta corta:** es el pedimento que amparó la importación. Lo agregas cuando vendes esa mercancía de primera mano.

En el producto, el texto de ayuda dice: si vendes de primera mano mercancía importada, esos números se incluyen en cada factura del producto, como información aduanera del CFDI.

Si el bien ya se vendió antes en México, no corresponde. La app no adivina si tu venta es de primera mano. Si guardas un número válido, lo manda en el concepto.

El ejemplo de los formularios es `25  47  3807  5001234`. Muestra la forma (año, aduana, patente y consecutivo). No es un pedimento real.

Del cliente pide sus **datos fiscales**: RFC, nombre o razón social, régimen, código postal y uso de CFDI. El flujo de la orden está en [facturación desde el admin](/docs/facturacion-desde-admin-shopify).

## Dónde se escribe en la app

**Respuesta corta:** en el producto, dentro de CFDI Express. La variante se edita en Shopify. En la factura solo puedes ajustarlo si la columna ya está visible.

### En el producto

1. Entra a **Productos** y abre el producto. La pantalla se llama **Configuración Producto: {título}**.
2. Junto al código SAT, la unidad, el IEPS y el IVA está **Pedimentos de importación**.
3. En **Números de pedimento** pon uno por línea o separados por comas. Cuando se acaben las piezas de un pedimento, reemplázalo por el nuevo: eso mismo dice el campo.
4. Al salir del campo, si son válidos, quedan uno por línea.
5. Guarda con la barra de Shopify. El aviso es **Códigos SAT actualizados**.

**Limpiar códigos** también borra los pedimentos.

Si una variante tiene los suyos, un banner lista esas variantes y te manda a editarlas en Shopify. El aviso revisa las primeras 100 variantes.

Los códigos de esa pantalla están en [Códigos del SAT por producto](/docs/codigos-sat-producto).

### En la variante

CFDI Express no trae formulario de variante. En el admin de Shopify, abre la variante y llena **Pedimentos de importación**. Si esa lista tiene al menos un pedimento válido, en la factura se usa esa y la del producto no.

### En la factura de la orden

En **CFDI para orden {nombre}**, dentro de **Formulario de Facturación**, la tabla **Productos** muestra la columna **Pedimentos** solo cuando algún concepto ya trae pedimentos. Si el producto y la variante están vacíos, la columna no existe: ahí no se capturan por primera vez.

Si la columna está, es un campo de una línea. Lo que cambies vale para ese timbrado y no se guarda en el producto. Una celda vacía deja esa factura sin pedimentos. La fila de envío no lleva.

Con el CFDI ya timbrado, el campo queda deshabilitado y muestra lo que se envió, separado por coma.

**Facturar al Publico en General** y **Facturar al Publico en General Extranjero** no los borran: vuelven a cargar los del producto o la variante. En una factura global siguen en cada concepto. No hay un concepto único de “global”. Más contexto en [público en general](/docs/facturacion-al-publico-en-general-manual-y-automatica).

### Con un CSV

En **Productos de {razón social}**, **Importar códigos** abre **Importar códigos SAT**. La columna `pedimentos` separa varios con punto y coma. Reemplaza la lista del producto. Una celda vacía la borra. Si el archivo no trae la columna, no se toca.

El CSV no escribe la variante. Busca por SKU: dos filas de variantes del mismo producto se pisan. La guía del archivo está en [carga masiva](/docs/carga-masiva-productos-csv).

## Qué gana si hay números en varios lados

**Respuesta corta:** una sola lista por concepto. Factura, si la cambiaste; si no, variante; si la variante no tiene, producto.

1. Lo que escribiste en la columna de esa factura, y solo si cambió respecto a lo cargado.
2. La lista de la variante, completa, si tiene al menos un pedimento válido.
3. La lista del producto.

No se suman. El envío nunca lleva pedimentos, aunque escribas algo en esa fila.

La autofacturación (**Formulario de autofacturación** en la [Thank You Page](/docs/facturacion-checkout-thank-you-page) y en el [estado de la orden](/docs/facturacion-pagina-estado-de-orden)), el bloque [Formulario de Facturación](/docs/formulario-de-facturacion-theme-block), el [POS](/docs/facturacion-pos-punto-de-venta-shopify) (**CFDI / Factura** y **CFDI / Factura (Órdenes)**) y la acción **Crear CFDI** de [Shopify Flow](/docs/uso-shopify-flow-automatizaciones-cfdi-express) toman el producto o la variante. Esos formularios no traen campo de pedimento.

[Sidekick](/docs/facturacion-con-sidekick-ia), con **Generar factura CFDI**, abre el formulario del admin y rellena datos fiscales. Si el producto ya tiene pedimentos, salen al oprimir **Facturar**.

## Cómo escribir el número

**Respuesta corta:** 15 dígitos. La app los guarda con dos espacios entre grupos: 21 caracteres.

Año (2), aduana (2), patente (4) y consecutivo (7). Puedes pegarlos, separarlos con espacios o usar guiones. `254738075001234` y `25 47 3807 5001234` son el mismo ejemplo. Varios pedimentos van por línea, coma, punto y coma o `|`. En el CSV, el separador es `;`.

Lo que se guarda es:

```text
25  47  3807  5001234
```

Hasta **100** por concepto. Si repites uno, se queda una sola vez.

Un texto inválido, o más de 100, no se guarda. En el producto el toast dice **Revisa los pedimentos**. En la factura: `Revisa los pedimentos: cada uno lleva 15 dígitos (año, aduana, patente y consecutivo).`

La app no consulta los catálogos de aduana y patente del SAT. Si el SAT rechaza el timbrado y el mensaje ya dice “pedimento”, verás: `Verifica que la aduana y la patente del pedimento sean correctas (catálogos del SAT); puedes corregirlo en el producto o en la tabla de la factura.`

## Qué queda en el CFDI

**Respuesta corta:** en el XML del concepto, como información aduanera, y también en el PDF de la factura.

Cada pedimento es un nodo `InformacionAduanera` con `NumeroPedimento` de 21 caracteres. Ese mismo número se imprime en el PDF.

La [nota de crédito de la app](/docs/notas-de-credito) no lleva pedimento. Ni desde la orden ni con la acción de Flow **Crear Nota de Crédito**. Esa nota va en un solo concepto genérico (clave `84111506`, unidad `ACT`).

Una línea reembolsada o en ceros no se factura, así que tampoco lleva pedimento.

## Preguntas frecuentes

### ¿Todo producto importado lleva pedimento en la factura?

Lo pones en la venta de primera mano. Si lo guardas en el producto o en la variante, la app lo incluye en las facturas de ese concepto. En una reventa no corresponde.

### ¿Lo puedo escribir solo cuando estoy facturando la orden?

Si algún concepto ya trae pedimentos, la columna aparece y puedes cambiarlos para ese timbrado. Si nadie los tiene, la columna no está.

### ¿La variante y el producto se combinan?

Se usa una lista. La variante reemplaza a la del producto cuando tiene al menos un pedimento válido.

### ¿El cliente lo captura al autofacturarse?

El cliente llena sus datos fiscales. El pedimento ya tiene que estar en el producto o en la variante.

### ¿La nota de crédito también?

La de la app, no. Si timbras la nota por API, el contrato es otro: ahí el concepto sí puede llevar pedimentos. Está en [la guía de la API](/blog/factura-con-pedimento-cfdi-express-api).

## También por API, y cómo instalar la app

Si timbras fuera de la app, el mismo dato va en el concepto por API o por MCP. La guía para ese camino es [Factura con pedimento en CFDI 4.0 por API](/blog/factura-con-pedimento-cfdi-express-api).

Para la tienda:

1. Instala [CFDI Express en Shopify](https://apps.shopify.com/cfdi-express) y actualiza la app si aún no ves el bloque.
2. En **Productos**, abre **Configuración Producto** y llena **Números de pedimento**.
3. Factura la orden. Revisa la columna **Pedimentos** cuando ya haya números cargados.
4. El detalle de pantallas, CSV y límites está en [Pedimentos de importación](/docs/pedimentos-de-importacion).

¿Quieres verlo sobre una orden de tu tienda? [Agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).
