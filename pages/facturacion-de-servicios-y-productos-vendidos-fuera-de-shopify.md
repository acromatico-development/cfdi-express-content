---
title: "Facturación de productos y servicios que vendes fuera de Shopify en CFDI Express"
description: "Este tutorial explica cómo utilizar CFDI Express para facturar productos o servicios que no están registrados en el catálogo de Shopify. Esta funcionalidad fue agregada en la versión 2.8 (lanzada el 16 de diciembre) y permite facturar ventas de mayoreo, servicios externos, o cualquier producto que se venda fuera de Shopify"
image: "https://videos.acromatico.dev/api/images/assets/bd362e3e-14d0-449b-9a1d-718b969b61b8.png?w=1600"
author: "Rafael González"
date: "2026-01-30"
keywords: "CFDI, facturación, México, SAT, Shopify, auto-facturación, portal"
---
# Tutorial | Facturación de productos y servicios que vendes fuera de Shopify en CFDI Express

![Factura todo en tu negocio desde Shopify](https://videos.acromatico.dev/api/images/assets/bd362e3e-14d0-449b-9a1d-718b969b61b8.png?w=2048)

¿Vendes servicios profesionales, productos personalizados o artículos que no están en tu catálogo de Shopify? En este tutorial te explicamos cómo puedes facturar estos conceptos de manera correcta utilizando **Draft Orders** (órdenes borrador) y una sintaxis especial que CFDI Express reconoce automáticamente.

### Video Tutorial

Si prefieres ver el proceso paso a paso, aquí te dejo el video tutorial completo:

<iframe width="100%" height="400" src="https://www.youtube.com/embed/ft5JKbanEqU" title="Facturación de productos y servicios que vendes fuera de Shopify en CFDI Express" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## El problema: productos que no existen en tu catálogo

Muchos negocios venden servicios o productos que no forman parte de su inventario regular en Shopify. Por ejemplo:

- Servicios de consultoría o asesoría
- Trabajos personalizados o a la medida
- Ventas realizadas por otros canales (WhatsApp, llamada, en persona)
- Productos únicos que no vale la pena dar de alta en el catálogo

El desafío es que estos conceptos necesitan un **código del SAT** y una **unidad del SAT** específicos para generar una factura válida.

## La solución: sintaxis especial en Draft Orders

CFDI Express permite usar una **sintaxis especial** en el nombre del producto personalizado para indicar exactamente qué código y unidad del SAT debe usar. El formato es:

```
[Nombre del producto] cfdi:[código_SAT],[unidad_SAT]
```

Por ejemplo:
- `Servicio de consultoría cfdi:80101500,E48`
- `Diseño de logo personalizado cfdi:82111500,E48`
- `Reparación de equipo cfdi:81112100,E48`

Cuando CFDI Express detecta esta sintaxis, **parsea automáticamente** el código y la unidad del SAT, asegurando que la factura se genere con los datos correctos.

## ¿Cómo crear una orden con producto personalizado?

### Paso 1: Crea una Draft Order en Shopify

1. Ve a tu administrador de Shopify
2. Navega a **Orders > Drafts** (Órdenes > Borradores)
3. Haz clic en **Create order** (Crear orden)

### Paso 2: Agrega el producto personalizado

1. En la sección de productos, haz clic en **Add custom item** (Agregar artículo personalizado)
2. En el nombre del producto, usa la sintaxis especial:
   - Ejemplo: `Servicio de desarrollo web cfdi:81111500,E48`
3. Ingresa el precio y la cantidad
4. Agrega los datos del cliente (nombre y correo electrónico)

### Paso 3: Completa la orden

1. Revisa que todos los datos estén correctos
2. Haz clic en **Collect payment** o **Mark as paid** según corresponda
3. La orden quedará registrada en Shopify

### Paso 4: Genera la factura

1. Ve a CFDI Express
2. Busca la orden recién creada
3. CFDI Express detectará automáticamente el código y la unidad del SAT que indicaste en el nombre
4. Completa los datos fiscales del cliente y genera la factura

## Códigos del SAT más comunes para servicios

| Servicio | Código SAT | Unidad |
|----------|------------|--------|
| Servicios de consultoría | 80101500 | E48 (Unidad de servicio) |
| Desarrollo de software | 81111500 | E48 |
| Diseño gráfico | 82111500 | E48 |
| Servicios legales | 80111600 | E48 |
| Servicios contables | 84111500 | E48 |
| Reparación de equipos | 81112100 | E48 |

## Ventajas de este método

- **Flexibilidad total**: Factura cualquier concepto sin darlo de alta en tu catálogo
- **Precisión fiscal**: Cada producto lleva el código del SAT correcto
- **Sin configuración adicional**: Solo necesitas conocer la sintaxis
- **Integración completa**: Las órdenes quedan registradas en Shopify como cualquier otra venta

---

¿Tienes dudas sobre qué código del SAT usar para tu servicio? Contáctanos y con gusto te orientamos.
