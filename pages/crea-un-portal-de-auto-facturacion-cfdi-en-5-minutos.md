---
title: "Crea un Portal de Auto-Facturación CFDI en 5 Minutos (o Menos)"
description: "Aprende cómo crear un portal de auto-facturación para tus clientes usando Shopify y CFDI Express. No importa si vendes en línea, en tienda física o por otros canales, tus clientes podrán facturar sus compras de manera autónoma."
image: "https://videos.acromatico.dev/api/images/assets/6c5df15b-23eb-4aff-bf81-0d7bdc56001d.png?w=1600"
author: "Rafael González"
date: "2026-01-13"
keywords: "CFDI, facturación, México, SAT, Shopify, auto-facturación, portal"
---
# Crea un Portal de Auto-Facturación CFDI en 5 Minutos

![Portal de Auto-Facturación CFDI Express](https://videos.acromatico.dev/api/images/assets/6c5df15b-23eb-4aff-bf81-0d7bdc56001d.png?w=2048)

¿Te gustaría que tus clientes pudieran facturar sus compras sin que tengas que intervenir? En este tutorial te mostraré cómo crear un **portal de auto-facturación** usando Shopify y CFDI Express en tan solo unos minutos.

## ¿Qué es un Portal de Auto-Facturación?

Un portal de auto-facturación es un sistema donde tus clientes pueden:

- **Ingresar sus datos fiscales** y generar su factura CFDI de manera autónoma
- **Ver el historial** de todas sus órdenes
- **Descargar sus facturas** cuando las necesiten

Lo mejor es que funciona **sin importar el canal de venta**: tienda en línea, tienda física, ventas B2B, redes sociales, o cualquier otro sistema que utilices.

## ¿Por qué usar Shopify para esto?

Shopify no es solo una plataforma para tiendas en línea. Shopify ofrece un **portal de cuentas de usuario** independiente del sitio web. Esto significa que:

- Puedes usar Shopify solo para registrar tus ventas sin necesidad de tener una tienda en línea
- Tus clientes pueden acceder al portal con solo su correo electrónico (sin necesidad de registro previo)
- Todas las órdenes se centralizan automáticamente, sin importar de dónde vengan

## Tutorial: Configuración Paso a Paso

### Video Tutorial

Si prefieres ver el proceso paso a paso, aquí te dejo el video tutorial completo:

<iframe width="560" height="315" src="https://www.youtube.com/embed/6NfKkmSrw4M" title="Crea un Portal de Auto-Facturación en 5 Minutos" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### Paso 1: Sincroniza tus ventas en Shopify

El primer paso es tener una cuenta de Shopify donde lleguen todas tus ventas. No importa si:

- Usas la tienda en línea de Shopify
- Tienes puntos de venta físicos con Shopify POS
- Sincronizas órdenes desde otros sistemas (ERP, sistemas propios, etc.)
- Creas órdenes manualmente desde el administrador (Draft Orders)

Lo importante es que **todas las órdenes estén en Shopify** con la información del cliente (principalmente su correo electrónico).

### Paso 2: Instala y configura CFDI Express

1. Ve a la [tienda de apps de Shopify](https://apps.shopify.com) y busca **CFDI Express**
2. Haz clic en **Instalar**
3. Completa el proceso de **onboarding** con los datos fiscales de tu empresa:
   - RFC y razón social
   - Código postal y régimen fiscal
   - Certificado de Sello Digital (CSD) - tu contador te puede ayudar con esto
   - Logo de tu empresa (opcional)
   - Código de producto SAT predeterminado
   - Código de unidad SAT predeterminado

### Paso 3: Configura los códigos SAT de tus productos (opcional)

Si vendes productos que requieren diferentes códigos del SAT, puedes configurarlos individualmente:

1. Dentro de CFDI Express, ve al módulo de **Productos**
2. Busca el producto que quieras configurar
3. Asigna el **código del SAT** y la **unidad del SAT** correspondientes

Si todos tus productos usan el mismo código, puedes dejarlo con el predeterminado que configuraste en el onboarding.

### Paso 4: Agrega los formularios de facturación

Este es el paso clave. Vamos a agregar el formulario de facturación en las páginas donde tus clientes podrán facturar.

1. Ve a **Settings > Customer accounts** en tu administrador de Shopify
2. Haz clic en **Customize**
3. En el editor, selecciona la página **Thank you** (página de confirmación de compra)
4. Haz clic en **Add block** y selecciona el **Formulario de auto-facturación** de CFDI Express
5. Repite el proceso para la página **Order status** (página de estado de orden)
6. Guarda los cambios

Ahora tus clientes verán el formulario de facturación tanto al terminar su compra como al consultar el estado de sus órdenes.

### Paso 5: Obtén la URL de tu portal

1. Ve a **Settings > Customer accounts** en Shopify
2. Copia la **URL del portal de cuentas** (algo como `https://shopify.com/<id-de-tienda>/account`)

Esta URL es la que compartirás con tus clientes para que accedan a su portal de facturación.

### Paso 6: Configura el correo de confirmación

Para que tus clientes reciban el link de facturación automáticamente:

1. Ve a **Settings > Notifications > Customer notifications**
2. Busca **Order confirmation** (Confirmación de orden)
3. El botón principal del correo ya lleva a la página de estado de orden
4. (Opcional) Puedes cambiar el texto del botón a algo como "Facturación" editando el código del correo

### Paso 7: Configura el ticket del POS (para tiendas físicas)

Si tienes puntos de venta físicos con Shopify POS:

1. Ve a las configuraciones del POS
2. Entra a **Recibos > Personalizar recibo**
3. Edita el código y agrega un QR al final con la URL del portal:

```liquid
<center>
  <p>Factura tu compra escaneando el código QR</p>
  {{ 'https://shopify.com/<id-de-tienda>/account' | qr_code }}
</center>
```

Pega este código aproximadamente en la línea 242 (al final del recibo).

## ¿Cómo funciona para el cliente?

1. El cliente realiza una compra (en línea, en tienda física, etc.)
2. Recibe un correo de confirmación con el botón de facturación
3. Al hacer clic, llega al portal donde ingresa su correo
4. Recibe un código de 6 dígitos para verificar su identidad
5. Ve todas sus órdenes y puede facturar la que necesite
6. Ingresa sus datos fiscales y genera su CFDI
7. Recibe la factura por correo y puede descargarla cuando quiera

## Ventajas de este sistema

- **Automático**: Los clientes facturan sin que tengas que hacer nada
- **Disponible 24/7**: Pueden facturar en cualquier momento
- **Historial completo**: Los clientes ven todas sus órdenes en un solo lugar
- **Sin registro**: Solo necesitan su correo electrónico
- **Multi-canal**: Funciona para ventas en línea, físicas o de cualquier otro sistema

---

¿Tienes dudas sobre cómo configurar tu portal de auto-facturación? Contáctanos y con gusto te ayudamos.
