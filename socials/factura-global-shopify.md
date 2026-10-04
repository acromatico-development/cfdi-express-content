# Social Pack | Factura global en Shopify: cómo hacerla en CFDI 4.0

Corresponde al blog post: [`pages/factura-global-shopify.md`](../pages/factura-global-shopify.md)

Recursos oficiales para enlazar:
- Post (tras el merge): https://cfdi.express/blog/factura-global-shopify
- Docs del app: https://cfdi.express/docs/facturacion-al-publico-en-general-manual-y-automatica
- Docs de Shopify Flow: https://cfdi.express/docs/uso-shopify-flow-automatizaciones-cfdi-express
- Post relacionado (POS): https://cfdi.express/blog/facturar-shopify-pos
- App de Shopify: https://apps.shopify.com/cfdi-express
- Landing: https://cfdi.express
- Demo: https://cal.com/team/acromatico-development/cfdi-express

---

## Cómo usar este pack

1. **Arte pendiente.** Diego genera el hero 1600×900 y el OG 1200×630 con los prompts de abajo y los sube a videos.acromatico.dev. Después se reemplaza el placeholder `PENDIENTE-factura-global-shopify.png` del frontmatter `image` (sin query) y el del cuerpo del post (con `?w=1600`). En este pack, LinkedIn, X, Facebook e Instagram usan el OG, sin query.
2. Publica con el **Texto** sugerido. El CTA es **instalar la app** y la guía de docs.
3. En la app, la factura a público en general es **un CFDI por orden**, con periodicidad **mensual** y el mes y año que elijas. No prometas "un solo CFDI con todas las ventas" ni periodicidad diaria o semanal.
4. Lo automático va con **Shopify Flow** (plantillas de fin de día, 3 días, 10 días y fin de mes). Antes de activarlas se revisan código postal, forma de pago, correo y el mes de la factura global.
5. Pide **datos fiscales** (RFC, nombre o razón social, régimen y CP). No pidas el PDF de la Constancia de Situación Fiscal.
6. No publiques el link del blog hasta que el post esté en `https://cfdi.express/blog/factura-global-shopify` (después del merge). **No mergear hasta que Rafael revise.**

---

## Imagen hero del blog

**Pendiente (Diego).** Slug: `factura-global-shopify`.

| Uso | Medida | URL (sin query) |
| --- | --- | --- |
| Hero / `image` del frontmatter (la plantilla lo usa también como `og:image`) | 1600×900 | PENDIENTE |
| OG / redes de este pack | 1200×630 | PENDIENTE |

No hay un campo OG aparte en el frontmatter. El cuerpo del post usa el hero con `?w=1600`.

Copy en el arte, ortografía exacta:
- Título: «Factura global en Shopify»
- Subtítulo: «Ventas a público en general»

**Prompt Gemini — hero 1600×900 (pieza nueva, comerciante):**

> Ilustración 3D estilo Pixar / personaje humano amigable (no foto real, no stock) para el hero de un blog B2B de facturación en México, dirigido a dueños de tienda Shopify. Personaje: hombre mexicano de unos 38 años, piel morena clara, barba corta, camisa de mezclilla, de pie de tres cuartos con una sonrisa tranquila. Junto a él, una pila de tickets de venta que se ordenan y entran a un documento blanco grande con sello circular dorado #FFD700 y la leyenda «PÚBLICO EN GENERAL». Detrás, un calendario de pared con un mes marcado en dorado. Fondo estudio en teal oscuro #055D5E, acentos dorados #FFD700, gris #D7D7D7. Texto grande en tipografía Racing Sans One, blanco, ortografía exacta: «Factura global en Shopify». Subtítulo en Quicksand, acento dorado: «Ventas a público en general». Iluminación cinematográfica, alto contraste, sin fotografías, sin logos de otras marcas, sin logos del SAT, sin llaves de código.

**Aspecto:** 16:9 · 1600×900.

**Prompt Gemini — OG 1200×630:**

> Imagen horizontal 1200×630 (1.91:1) estilo Pixar / humano para Open Graph. El mismo comerciante con camisa de mezclilla, la pila de tickets entrando a un documento con sello dorado #FFD700 y la leyenda «PÚBLICO EN GENERAL», y un calendario con un mes marcado. Fondo teal oscuro #055D5E. Texto exacto en Racing Sans One, blanco: «Factura global en Shopify». Subtítulo en Quicksand, dorado #FFD700: «Ventas a público en general». Sin fotos de stock, sin logos de otras marcas, sin código. Alto contraste, profesional. Deja aire para que el título no se corte en el recorte de LinkedIn.

**Aspecto:** 1200×630 (1.91:1).

---

## LinkedIn

### Texto

¿Qué pasa con las ventas de tu tienda Shopify que nadie facturó?

No se quedan sin comprobante. Van a público en general (RFC XAXX010101000), con la información global del CFDI 4.0: periodicidad, mes y año.

Con CFDI Express lo haces de dos formas:

→ **A mano desde el admin.** Abres la orden, oprimes «Facturar al Publico en General», revisas mes, año y forma de pago, y generas el CFDI.
→ **Automático con Shopify Flow.** Importas una plantilla que busca las órdenes pagadas y no facturadas y las timbra al final del día, a los 3 días, a los 10 días o al final del mes.

Tres cosas que conviene saber:
• La app timbra un CFDI por orden, con periodicidad mensual.
• Puedes dejar fuera una orden con el campo «Ignorar auto-facturación».
• Alinea el global con el plazo de autofactura de tus clientes, para que alcancen a pedir la suya.

Guía paso a paso: https://cfdi.express/blog/factura-global-shopify
Instala la app: https://apps.shopify.com/cfdi-express

#CFDI #CFDI40 #FacturacionElectronica #SAT #Shopify #CFDIExpress #FacturaGlobal #PublicoEnGeneral

### Media

**Usar (OG, sin query):** PENDIENTE (Diego)

**Prompt Gemini (pieza de comerciantes, 1200×627):**

> Imagen horizontal 1200×627 estilo Pixar / humano. Comerciante con camisa de mezclilla frente a una laptop; en la pantalla, una lista de órdenes con etiquetas «No facturado» que se convierten, con una flecha dorada #FFD700, en documentos con sello «PÚBLICO EN GENERAL». Un pequeño engranaje dorado con un rayo representa la automatización. Fondo teal oscuro #055D5E. Texto exacto en Racing Sans One, blanco: «Factura global en Shopify». Subtítulo en Quicksand, dorado: «Manual o automática con Flow». Sin fotos de stock, sin logos de otras marcas, sin código.

**Aspecto:** 1200×627 (1.91:1).

---

## X (Twitter)

### Texto (post principal)

> Las ventas de Shopify que nadie facturó van a público en general (XAXX010101000).
>
> Con CFDI Express las timbras desde el admin o en automático con Shopify Flow. Guía paso a paso 👇
> https://cfdi.express/blog/factura-global-shopify

### Hilo opcional

1. A mano: abre la orden → «Facturar al Publico en General» → revisa mes, año y forma de pago → Generar CFDI.
2. Con Flow: plantillas de fin de día, 3 días, 10 días o fin de mes. Antes de activarlas revisa CP, forma de pago, correo y el mes.
3. Alinea el global con el plazo de autofactura de tus clientes. Si corre muy pronto, no alcanzan a pedir la suya.
4. ¿Un cliente pide factura después del global? Lo resuelves con nota de crédito o cancelación y una nueva factura. Decídelo con tu contador.
5. Instala la app: https://apps.shopify.com/cfdi-express

### Media

**Usar (OG, sin query):** PENDIENTE (Diego)

**Prompt Gemini (pieza nueva):**

> Imagen horizontal 1600×900 estilo Pixar / humano. Comerciante con camisa de mezclilla junto a un calendario gigante; cuatro días marcados con chips dorados #FFD700: «Fin de día», «3 días», «10 días», «Fin de mes». Fondo teal oscuro #055D5E. Texto exacto en Racing Sans One, blanco: «Factura global automática». Subtítulo en Quicksand: «Con Shopify Flow». Sin fotos de stock, sin logos de otras marcas, sin código.

**Aspecto:** 1600×900 (16:9).

---

## Facebook

### Texto

¿Vendes en Shopify y no todos tus clientes piden factura? 🧾

Esas ventas también se facturan: van a público en general, con el mes y el año del periodo.

Con CFDI Express lo haces desde el admin de Shopify, orden por orden, o lo dejas automático con Shopify Flow.

En la guía te explicamos:
✅ Cómo hacerla a mano desde la orden
✅ Qué plantilla de Flow elegir y qué revisar antes de activarla
✅ Cómo convive con la autofactura de tus clientes
✅ Qué hacer si alguien pide su factura después

👉 https://cfdi.express/blog/factura-global-shopify
App: https://apps.shopify.com/cfdi-express

### Media

**Usar (OG, sin query):** PENDIENTE (Diego)

**Prompt Gemini (pieza nueva):**

> Imagen horizontal 1200×630 estilo Pixar / humano. Comerciante con camisa de mezclilla sonriendo detrás del mostrador de su tienda, sosteniendo un documento con sello dorado #FFD700 y la leyenda «PÚBLICO EN GENERAL». Al fondo, cajas de envío y una laptop. Fondo teal oscuro #055D5E. Texto exacto en Racing Sans One, blanco: «Factura global en Shopify». Subtítulo en Quicksand, dorado: «Ventas sin factura, resueltas». Sin fotos de stock, sin logos de otras marcas, sin código.

**Aspecto:** 1200×630 (1.91:1).

---

## Instagram

### Texto (feed)

> Ventas sin factura en tu tienda Shopify = factura a público en general 🧾
>
> ✅ A mano desde el admin
> ✅ Automática con Shopify Flow
> ✅ Mes y año del periodo en cada CFDI
> ✅ Órdenes que puedes dejar fuera
>
> Guía en bio → cfdi.express/blog/factura-global-shopify
> Docs: cfdi.express/docs/facturacion-al-publico-en-general-manual-y-automatica
> App: apps.shopify.com/cfdi-express
>
> #CFDI #CFDI40 #FacturacionElectronica #SAT #CFDIExpress #FacturaGlobal #PublicoEnGeneral #Shopify #ShopifyMexico #ShopifyFlow #FacturaElectronica #Mexico #PyME #TiendaEnLinea #Ecommerce #EcommerceMexico #Emprendedores #Comerciantes #Contadores #Contabilidad #SATMexico #Factura40 #CFDIShopify #FacturacionShopify #NegociosMexico #Automatizacion #VentasEnLinea #XAXX010101000 #CFDIExpressApp

### Media (feed cuadrado)

**Usar:** PENDIENTE (Diego). Si no hay recorte 1080×1080, usar el OG, sin query.

**Prompt Gemini (1:1, pieza nueva):**

> Imagen cuadrada 1080×1080 estilo Pixar / humano para feed. Comerciante con camisa de mezclilla al centro, pila de tickets entrando a un documento con sello dorado #FFD700 y la leyenda «PÚBLICO EN GENERAL», y un calendario con un mes marcado. Fondo teal oscuro #055D5E. Texto grande blanco, Racing Sans One, exacto: «Factura global». Texto menor Quicksand: «En tu tienda Shopify». Alto contraste, sin fotos de stock, sin logos de otras marcas, sin hashtags pintados en la imagen, sin código.

**Aspecto:** 1080×1080 (1:1).

### Media (historia opcional)

> Historia vertical 1080×1920, estilo Pixar. Fondo teal #055D5E con halo dorado #FFD700. Arriba: wordmark CFDI Express. Centro: comerciante con el documento «PÚBLICO EN GENERAL» y un calendario. Texto grande exacto: «Factura global en Shopify». Texto menor: «Manual o automática». CTA: «Lee la guía». Deja el tercio superior e inferior limpios para la UI de Instagram. Sin fotos reales, sin logos ajenos, sin código.

**Aspecto:** 1080×1920 (historia).

---

## Checklist de publicación

- [ ] Diego genera el hero 1600×900 y el OG 1200×630. Se reemplaza `PENDIENTE-factura-global-shopify.png` en el frontmatter `image` (sin query) y en el cuerpo (con `?w=1600`). Las URLs del OG se ponen en este pack, sin query.
- [ ] Rafael revisa el post antes del merge. **No mergear:** en este repo el merge publica en vivo.
- [ ] No publicar el URL del blog hasta que esté vivo en `https://cfdi.express/blog/factura-global-shopify`.
- [ ] CTA = instalar la app + docs de público en general.
- [ ] Datos fiscales (RFC, nombre, régimen y CP). No pedir el PDF de la Constancia de Situación Fiscal.
- [ ] Datos alineados al post: un CFDI por orden, periodicidad mensual, mes y año elegibles, cuatro plantillas de Flow, campo «Ignorar auto-facturación». Sin precios ni cifras de clientes.
- [ ] Ortografía del arte nuevo: «Factura global en Shopify» / «Ventas a público en general» / «PÚBLICO EN GENERAL».
- [ ] Sin logos del SAT ni de terceros en las imágenes.
- [ ] LinkedIn y Facebook primero; X al mediodía; Instagram por la tarde.
