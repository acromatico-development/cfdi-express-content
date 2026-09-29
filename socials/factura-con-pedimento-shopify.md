# Social Pack | Factura con pedimento en Shopify: cómo ponerlo en el CFDI

Corresponde al blog post: [`pages/factura-con-pedimento-shopify.md`](../pages/factura-con-pedimento-shopify.md)

Recursos oficiales para enlazar:
- Post (tras el merge): https://cfdi.express/blog/factura-con-pedimento-shopify
- Docs del app: https://cfdi.express/docs/pedimentos-de-importacion
- Post de la API (devs): https://cfdi.express/blog/factura-con-pedimento-cfdi-express-api
- App de Shopify: https://apps.shopify.com/cfdi-express
- Landing: https://cfdi.express
- Demo: https://cal.com/team/acromatico-development/cfdi-express

---

## Cómo usar este pack

1. **Hero y OG son temporales.** Usan la misma URL del post `factura-con-pedimento-cfdi-express-api`, sin query. Ese arte dice «CFDI 4.0 por API». No lo publiques como pieza final de comerciantes: el diseñador hará una imagen propia. Los prompts de abajo son para esa pieza.
2. Publica con el **Texto** sugerido. El CTA es **instalar o actualizar la app** y la guía de docs. La API se menciona al final, como camino aparte.
3. El pedimento va en el producto o en la variante, para venta de **primera mano**. En la factura la columna solo aparece si algún concepto ya los trae. No se mezclan producto y variante.
4. Pide **datos fiscales** (RFC, nombre o razón social, régimen y CP). No pidas el PDF de la Constancia de Situación Fiscal.
5. La nota de crédito **de la app** no lleva pedimento. La de la API sí puede.
6. No publiques el link del blog hasta que el post esté en `https://cfdi.express/blog/factura-con-pedimento-shopify` (después del merge). **No mergear hasta que Rafael revise.**

---

## Imagen hero del blog

**Temporal (no es el arte final).** Slug: `factura-con-pedimento-shopify`.

| Uso | Medida pedida | URL temporal (sin query) |
| --- | --- | --- |
| Hero / `image` del frontmatter | 1600×900 | https://videos.acromatico.dev/api/images/assets/9b42be87-d026-4d28-a6fe-6f0f2c878dec.png |
| OG / compartir (`og:image`, Meta / LinkedIn) | 1200×630 | https://videos.acromatico.dev/api/images/assets/9b42be87-d026-4d28-a6fe-6f0f2c878dec.png |

El frontmatter `image` usa esa URL, sin query. En el cuerpo del post la misma URL lleva `?w=1600`. Cuando exista el arte de comerciantes, sustituir hero y OG. El copy de la pieza nueva no debe decir «por API».

Copy propuesto para el arte nuevo, ortografía exacta:
- Título: «Factura con pedimento»
- Subtítulo: «En tu tienda Shopify»

**Prompt Gemini — hero 1600×900 (pieza nueva, comerciante):**

> Ilustración 3D estilo Pixar / personaje humano amigable (no foto real, no stock) para el hero de un blog B2B de facturación en México, dirigido a dueños de tienda Shopify. Personaje: mujer mexicana de unos 34 años, piel morena, cabello oscuro recogido, playera blanca y overshirt verde olivo, de pie de tres cuartos con una sonrisa. En una mano, un documento blanco de pedimento aduanal con sello circular dorado #FFD700; en la otra, una caja de mercancía importada con una etiqueta pequeña. Fondo estudio en teal oscuro #055D5E, acentos dorados #FFD700, gris #D7D7D7. Texto grande en tipografía Racing Sans One, blanco, ortografía exacta: «Factura con pedimento». Subtítulo en Quicksand, acento dorado: «En tu tienda Shopify». Iluminación cinematográfica, alto contraste, sin fotografías, sin logos de otras marcas, sin llaves de código.

**Aspecto:** 16:9 · 1600×900.

**Prompt Gemini — OG 1200×630:**

> Imagen horizontal 1200×630 (1.91:1) estilo Pixar / humano para Open Graph. La misma comerciante con playera blanca y overshirt verde olivo, documento de pedimento con sello dorado #FFD700 y una caja de importación. Fondo teal oscuro #055D5E. Texto exacto en Racing Sans One, blanco: «Factura con pedimento». Subtítulo en Quicksand, dorado #FFD700: «En tu tienda Shopify». Sin fotos de stock, sin logos de otras marcas, sin código. Alto contraste, profesional. Deja aire para que el título no se corte en el recorte de LinkedIn.

**Aspecto:** 1200×630 (1.91:1).

---

## LinkedIn

### Texto

> Si vendes en Shopify mercancía importada de primera mano, el número de pedimento no va en la cabecera de la factura. Va en el producto, y de ahí sale en cada concepto del CFDI.
>
> En CFDI Express el bloque se llama Pedimentos de importación, en la misma pantalla del código SAT. Son 15 dígitos (año, aduana, patente y consecutivo). La app los guarda con dos espacios entre grupos. Si la variante tiene los suyos, se usan esos y los del producto no. En la orden, la columna Pedimentos solo aparece cuando algún concepto ya los trae: ahí puedes ajustarlos para ese timbrado.
>
> También entran en la autofacturación, el POS, Flow y la factura global, tomados del producto o de la variante. La nota de crédito que emite la app no los lleva.
>
> Hace falta la app actualizada y el proveedor CFDI Express. No depende del plan. Del cliente, pide datos fiscales (RFC, nombre o razón social, régimen y CP).
>
> Guía: https://cfdi.express/blog/factura-con-pedimento-shopify
> Paso a paso: https://cfdi.express/docs/pedimentos-de-importacion
> Instalar: https://apps.shopify.com/cfdi-express
>
> #CFDI #FacturacionElectronica #SAT #CFDIExpress #Pedimento #Importacion #Shopify #CFDI40

### Media

**Temporal:** https://videos.acromatico.dev/api/images/assets/9b42be87-d026-4d28-a6fe-6f0f2c878dec.png

**Prompt Gemini (pieza de comerciantes, 1200×627):**

> Imagen horizontal 1200×627 (1.91:1) estilo Pixar / humano para LinkedIn B2B. Comerciante con playera blanca y overshirt verde olivo, documento de pedimento con sello dorado #FFD700 y una caja de mercancía. Fondo teal oscuro #055D5E. Texto exacto en Racing Sans One / Quicksand, blanco y dorado: «Factura con pedimento» y «En tu tienda Shopify». Sin fotos de stock, sin logos de otras marcas, sin código. Alto contraste, profesional.

**Aspecto:** 1200×627 (1.91:1).

---

## X (Twitter)

### Texto (post principal)

> Factura con pedimento en Shopify: el número va en el producto, no en la cabecera.
>
> 15 dígitos. La variante reemplaza al producto.
>
> → https://cfdi.express/blog/factura-con-pedimento-shopify

### Hilo opcional

1. Lo necesitas en la venta de primera mano de mercancía que tú importaste. En una reventa no corresponde. La app no lo infiere: si lo guardas, lo manda en el concepto.
2. En CFDI Express: Productos → Configuración Producto → Pedimentos de importación. La variante se edita en Shopify, campo Pedimentos de importación.
3. En la factura, la columna solo aparece si algún concepto ya trae pedimentos. No se agregan ahí por primera vez. El envío no lleva.
4. Autofacturación, POS, Flow y factura global toman los del producto o la variante. La nota de crédito de la app no los incluye.
5. Formato: 15 dígitos, se guardan como 21 caracteres con dos espacios. Máximo 100 por concepto. La app no valida que la aduana y la patente existan en el SAT; el SAT puede rechazar.
6. Guía: https://cfdi.express/docs/pedimentos-de-importacion · App: https://apps.shopify.com/cfdi-express · Si timbras por API: https://cfdi.express/blog/factura-con-pedimento-cfdi-express-api

### Media

**Temporal:** https://videos.acromatico.dev/api/images/assets/9b42be87-d026-4d28-a6fe-6f0f2c878dec.png

**Prompt Gemini (pieza nueva):**

> Imagen 1600×900 (16:9) para X, estilo Pixar / humano. Comerciante con overshirt verde olivo, documento de pedimento blanco, sello dorado #FFD700, caja de importación. Fondo teal #055D5E. Texto exacto Racing Sans One: «Factura con pedimento». Texto menor Quicksand: «En tu tienda Shopify». Sin fotos, sin logos ajenos, sin código.

**Aspecto:** 1600×900 (16:9).

---

## Facebook

### Texto

> ¿Te pidieron factura con pedimento y vendes en Shopify?
>
> Si la mercancía es de importación y esta venta es de primera mano, el número se guarda en el producto. CFDI Express lo manda en el concepto de la factura.
>
> La variante, si tiene el suyo, reemplaza al del producto. En la orden puedes ajustarlo solo cuando la columna ya está visible. El envío no lleva pedimento. La nota de crédito de la app tampoco.
>
> Actualiza la app y revisa que tu proveedor sea CFDI Express. Pide datos fiscales (RFC, nombre, régimen y CP).
>
> Guía: https://cfdi.express/blog/factura-con-pedimento-shopify
> Paso a paso: https://cfdi.express/docs/pedimentos-de-importacion
> Instalar: https://apps.shopify.com/cfdi-express
>
> #CFDI #FacturacionElectronica #SAT #CFDIExpress #Pedimento #Importacion #Shopify

### Media

**Temporal:** https://videos.acromatico.dev/api/images/assets/9b42be87-d026-4d28-a6fe-6f0f2c878dec.png

**Prompt Gemini (pieza nueva):**

> Imagen 1200×630 (1.91:1) para Facebook. Ilustración 3D estilo Pixar: comerciante de playera blanca y overshirt verde olivo con un pedimento y sello dorado #FFD700, junto a una caja de mercancía importada. Texto exacto: «Factura con pedimento» / «En tu tienda Shopify». Fondo teal #055D5E. Sin fotos reales, sin logos de terceros, sin código.

**Aspecto:** 1200×630 (1.91:1).

---

## Instagram

### Texto (feed)

> Factura con pedimento en Shopify.
>
> Venta de primera mano de mercancía importada.
> El número vive en el producto.
> Si la variante tiene lista, se usa esa.
>
> 15 dígitos. Se guardan con dos espacios.
> En la factura, la columna aparece cuando ya hay pedimentos.
> El envío no lleva. La nota de crédito de la app tampoco.
>
> Pide datos fiscales (RFC, nombre, régimen y CP).
>
> Guía en bio → cfdi.express/blog/factura-con-pedimento-shopify
> Docs: cfdi.express/docs/pedimentos-de-importacion
> App: apps.shopify.com/cfdi-express
>
> #CFDI #CFDI40 #FacturacionElectronica #SAT #CFDIExpress #Pedimento #Importacion #Shopify #Aduana #NumeroPedimento #InformacionAduanera #FacturaElectronica #Mexico #PyME #Importadores #ComercioExterior #PrimeraMano #Factura40 #SATMexico #Logistica #TiendaEnLinea #Ecommerce #ShopifyMexico #CFDIShopify #MercanciaImportada #AduanaMexico #Comerciantes #FacturacionShopify #PedimentoAduanal #CFDIExpressApp

### Media (feed cuadrado)

No hay recorte 1080×1080. **Temporal:** el mismo hero, hasta que exista el arte cuadrado:
https://videos.acromatico.dev/api/images/assets/9b42be87-d026-4d28-a6fe-6f0f2c878dec.png

**Prompt Gemini (1:1, pieza nueva):**

> Imagen cuadrada 1080×1080 estilo Pixar / humano para feed. Comerciante con playera blanca y overshirt verde olivo al centro, documento de pedimento, sello dorado #FFD700 y una caja de importación. Fondo teal oscuro #055D5E. Texto grande blanco, Racing Sans One, exacto: «Factura con pedimento». Texto menor Quicksand: «En tu tienda Shopify». Alto contraste, sin fotos de stock, sin logos de otras marcas, sin hashtags pintados en la imagen, sin código.

**Aspecto:** 1080×1080 (1:1).

### Media (historia opcional)

> Historia vertical 1080×1920, estilo Pixar. Fondo teal #055D5E con halo dorado #FFD700. Arriba: wordmark CFDI Express. Centro: comerciante con el documento de pedimento y una caja. Texto grande exacto: «Factura con pedimento». Texto menor: «En tu tienda Shopify». CTA: «Lee la guía». Deja el tercio superior e inferior limpios para la UI de Instagram. Sin fotos reales, sin logos ajenos, sin código.

**Aspecto:** 1080×1920 (historia).

---

## Checklist de publicación

- [ ] Hero y OG temporales = URL del post de API, **sin query**. El arte dice «CFDI 4.0 por API». Sustituir antes de publicar en redes.
- [ ] Rafael revisa el post antes del merge. **No mergear:** en este repo el merge publica en vivo.
- [ ] No publicar el URL del blog hasta `https://cfdi.express/blog/factura-con-pedimento-shopify`.
- [ ] CTA = docs + instalar la app. La API queda como enlace aparte.
- [ ] Datos fiscales (RFC, nombre, régimen, CP). No pedir el PDF de la Constancia de Situación Fiscal.
- [ ] Cifras alineadas al post: 15 dígitos, 21 caracteres, dos espacios, máximo 100, una sola lista (factura si cambió, si no variante, si no producto). La columna de la factura no aparece si nadie tiene pedimentos. La nota de crédito de la app no lleva pedimento.
- [ ] Sin decir que el PDF imprime el pedimento.
- [ ] Ortografía del arte nuevo: «Factura con pedimento» / «En tu tienda Shopify».
- [ ] Sin logos del SAT ni de terceros en las imágenes.
- [ ] LinkedIn y Facebook primero; X al mediodía; Instagram por la tarde.
