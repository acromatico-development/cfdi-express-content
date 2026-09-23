# Social Pack | Todo CFDI vs CFDI Express: ¿portal de timbres o app de Shopify?

Corresponde al blog post: [`pages/todo-cfdi-vs-cfdi-express.md`](../pages/todo-cfdi-vs-cfdi-express.md)

Recursos oficiales para enlazar:
- Post (tras el merge): https://cfdi.express/blog/todo-cfdi-vs-cfdi-express
- Guía CFDI 4.0: https://cfdi.express/blog/cfdi-facturas-4-0
- Mapa Shopify 2026: https://cfdi.express/blog/cfdi-4-0-shopify-2026
- Comparativo de apps (no mezclar con este portal): https://cfdi.express/blog/facturama-vs-cfdi-express
- Comparativo de APIs: https://cfdi.express/blog/facturapi-vs-cfdi-express-api
- App Shopify: https://apps.shopify.com/cfdi-express
- API (integradores, no es el CTA principal): https://cfdi.express/api
- Todo CFDI, login público: https://www.todocfdi.com/xcfdifacturas40/loginx
- Captura Digital, precios públicos: https://www.captura-digital.com/Precios
- Captura Digital, programas: https://www.captura-digital.com/Programas
- Demo: https://cal.com/team/acromatico-development/cfdi-express

---

## Cómo usar este pack

1. **Diego todavía no sube el arte.** Este post se abrió sin `image:` de CDN a propósito: no hay UUID inventado. Genera hero y OG con los prompts de abajo (estilo Pixar, humano, hoodie teal) y súbelos a `videos.acromatico.dev`.
2. Medidas: **hero 1600×900** (frontmatter `image:` y cuerpo) y **OG 1200×630**. Paleta: teal `#055D5E`, acento dorado `#FFD700`, gris `#D7D7D7`.
3. Al subir, actualiza el frontmatter `image:` (URL sin query) y sustituye el comentario `<!-- HERO: Diego Pixar hoodie-teal; update frontmatter image + ![] after CDN -->` por el `![]()` del hero, como en Facturama y Facturapi.
4. Publica con el **Texto** sugerido. El CTA principal, para quien tiene tienda, es **instalar la app**. El portal de Todo CFDI se menciona con respeto y con link a su sitio, no como ataque. La API va como mención corta.
5. Es un comparativo **justo**: no insultes a Captura Digital, no inventes precios, no digas que CFDI Express es “siempre más barata”. Califica “según páginas públicas, 23 de septiembre de 2026”.
6. No publiques el link del blog hasta que el post esté en `https://cfdi.express/blog/todo-cfdi-vs-cfdi-express` (después del merge). **No merges hasta que Rafael revise y el hero/OG estén en el CDN.**

---

## Brief para Diego (hero + OG)

| Uso | Medida | Estado |
| --- | --- | --- |
| Hero / `image` del frontmatter | 1600×900 | Pendiente de CDN. Al subir, pegar la URL en `pages/todo-cfdi-vs-cfdi-express.md`. |
| OG / compartir (`og:image`, Meta / LinkedIn) | 1200×630 | Pendiente de CDN. Documentar la URL aquí, en la fila de cada red. |

Copy en el arte, ortografía exacta:
- Título: «Todo CFDI vs CFDI Express»
- Subtítulo: «¿Portal de timbres o app de Shopify?»

Personaje: humano estilo Pixar (no foto, no stock), hoodie oversized teal `#055D5E`. Sin logos de Captura Digital, Todo CFDI, Shopify ni de otras marcas.

**Prompt Gemini — hero 1600×900:**

> Ilustración 3D estilo Pixar / personaje humano amigable (no foto real, no stock) para el hero de un blog B2B de facturación en México. Personaje: comerciante mexicano de unos 34 años, piel morena clara, cabello corto oscuro, hoodie oversized color teal oscuro #055D5E con un pequeño recibo bordado en el pecho. Está de tres cuartos, sonrisa confiada, con una factura blanca en una mano (sello circular dorado #FFD700) y un mostrador de tienda muy simple al fondo, como si comparara un portal de timbres con una app de tienda. No uses logos de marcas ajenas (nada de Captura Digital, Todo CFDI, Shopify, SAT ni App Store). Fondo estudio suave en teal #055D5E, acentos dorados #FFD700, gris #D7D7D7. Texto grande en tipografía Racing Sans One, blanco, ortografía exacta: «Todo CFDI vs CFDI Express». Subtítulo en Quicksand, acento dorado: «¿Portal de timbres o app de Shopify?». Iluminación cinematográfica, alto contraste, sin fotografías.

**Aspecto:** 16:9 · 1600×900.

**Prompt Gemini — OG 1200×630:**

> Imagen horizontal 1200×630 (1.91:1) estilo Pixar / humano para Open Graph. El mismo comerciante con hoodie oversized teal #055D5E, una factura blanca con sello dorado #FFD700. Fondo teal oscuro #055D5E. Texto exacto en Racing Sans One, blanco: «Todo CFDI vs CFDI Express». Subtítulo en Quicksand, dorado #FFD700: «¿Portal de timbres o app de Shopify?». Sin fotos de stock, sin logos de otras marcas. Alto contraste, profesional. Deja aire para que el título no se corte en el recorte de LinkedIn.

**Aspecto:** 1200×630 (1.91:1).

---

## Imagen hero del blog

Pendiente. Cuando Diego publique, reemplaza esta nota con la URL sin query y úsala también en el frontmatter.

**Prompt:** el del brief de arriba (hero 1600×900).

---

## LinkedIn

### Texto

> Si buscaste **todo cfdi** o **todo cfdi 4.0**, no estás viendo una app de Shopify. Estás viendo el portal de facturación de Captura Digital: login en todocfdi.com, paquetes de timbres y módulos para escuela, taller, hotel o donatarias.
>
> Publicamos un comparativo justo: **Todo CFDI vs CFDI Express**. Según su página de Precios (23 de septiembre de 2026, IVA incluido, vigencia de un año): 100 timbres **$365 MXN**, 200 **$698**, 300 **$1,062**, 500 **$1,629**, 1,000 **$3,030**. Usuarios nuevos: 100 timbres gratis por 180 días, según su registro. No publican app en la Shopify App Store.
>
> CFDI Express es la app **Built for Shopify**: Thank You, POS, notas de crédito y Flow. Basic **$15 USD/mes + $0.10 USD** por factura generada o cancelada. La API, aparte, anda en **~$1 MXN por timbre**, sin mensualidad del API.
>
> Nadie es “siempre más barato”. Sin tienda, el paquete en pesos es el número fácil. Con Shopify, la app se paga porque timbra la orden. Por timbre agotado, la API queda debajo del paquete, y no te da su portal.
>
> Lee el comparativo: https://cfdi.express/blog/todo-cfdi-vs-cfdi-express
>
> ¿Vendes en Shopify? Instala la app → https://apps.shopify.com/cfdi-express
>
> #CFDI #FacturacionElectronica #SAT #Shopify #ShopifyMexico #CFDIExpress #TodoCFDI #CapturaDigital #EcommerceMexico

### Media

**Usar (cuando exista):** OG 1200×630 de Diego. Hoy: generar con el prompt del brief.

**Prompt Gemini (variante LinkedIn, 1200×627):**

> Imagen horizontal 1200×627 (1.91:1) estilo Pixar / humano para LinkedIn B2B. Comerciante con hoodie oversized teal #055D5E, factura blanca, sello dorado #FFD700. Fondo teal oscuro #055D5E. Texto exacto en Racing Sans One / Quicksand, blanco y dorado: «Todo CFDI vs CFDI Express» y «¿Portal de timbres o app de Shopify?». Sin fotos de stock, sin logos de otras marcas. Alto contraste, profesional.

**Aspecto:** 1200×627 (1.91:1).

---

## X (Twitter)

### Texto (post principal)

> Todo CFDI vs CFDI Express (2026), con cifras públicas.
>
> Portal de timbres (desde $365 MXN / 100) vs app de Shopify ($15 + $0.10/CFDI). No es “siempre más barato”.
>
> → https://cfdi.express/blog/todo-cfdi-vs-cfdi-express

### Hilo opcional (desarrolla el post)

1. «Todo CFDI» es el portal de Captura Digital (login en todocfdi.com, CFDI 4.0). No es una app de la Shopify App Store. Programas web: facturas, honorarios, nómina, escolar, taller, hotel, donatarias.
2. Precios públicos, 23 sep 2026, IVA incluido, vigencia 1 año: 100 timbres $365 MXN, 200 $698, 300 $1,062, 500 $1,629, 1,000 $3,030. Registro: 100 timbres gratis, 180 días, usuarios nuevos.
3. Si agotas el paquete, el timbre les sale entre ~$3.03 y $3.65 MXN. La API de CFDI Express es ~$1 MXN, sin cuota de API, y sin su portal. La app de Shopify es otro SKU: $15 USD + $0.10 por factura.
4. Elige Todo CFDI si capturas en portal, pagas en OXXO y no vendes en Shopify. Elige CFDI Express si la orden, el POS y Flow ya están en la tienda. Guía: https://cfdi.express/blog/todo-cfdi-vs-cfdi-express

### Media

**Usar (cuando exista):** hero 1600×900.

**Prompt Gemini (variante):**

> Imagen 1600×900 (16:9) para X, estilo Pixar / humano. Close-up del comerciante con hoodie teal #055D5E, factura blanca, sello dorado #FFD700. Fondo teal #055D5E. Texto exacto Racing Sans One: «Todo CFDI vs CFDI Express». Texto menor Quicksand: «¿Portal de timbres o app de Shopify?». Sin fotos, sin logos ajenos.

**Aspecto:** 1600×900 (16:9).

---

## Facebook

### Texto

> ¿Todo CFDI o CFDI Express?
>
> Todo CFDI es el portal de Captura Digital: paquetes de timbres (desde $365 MXN por 100, IVA incluido, vigencia de un año; precios del 23 de septiembre de 2026) y módulos para escuela, taller, hotel o donatarias. No publican app de Shopify.
>
> CFDI Express es la app para tu tienda: autofactura en el checkout, POS y Flow, desde $15 USD al mes + $0.10 USD por CFDI. Sin hablar mal de nadie y sin un “siempre más barato”.
>
> Lee la tabla: https://cfdi.express/blog/todo-cfdi-vs-cfdi-express
>
> Si vendes en Shopify, instala la app: https://apps.shopify.com/cfdi-express
>
> #CFDI #FacturacionElectronica #SAT #Shopify #CFDIExpress #TodoCFDI

### Media

**Usar (cuando exista):** OG 1200×630.

**Prompt Gemini (variante):**

> Imagen 1200×630 (1.91:1) para Facebook. Ilustración 3D estilo Pixar: comerciante de hoodie teal #055D5E comparando una factura con sello dorado #FFD700. Texto exacto: «Todo CFDI vs CFDI Express» / «¿Portal de timbres o app de Shopify?». Fondo teal #055D5E. Sin fotos reales, sin logos de terceros.

**Aspecto:** 1200×630 (1.91:1).

---

## Instagram

### Texto (feed)

> ¿Todo CFDI o CFDI Express? Si buscaste «todo cfdi 4.0», esta es la diferencia.
>
> Todo CFDI (Captura Digital): portal y Windows, paquetes desde $365 MXN / 100 timbres (sep 2026), escuela, taller, hotel, donatarias. No es app de Shopify.
>
> CFDI Express: app Built for Shopify, Thank You + POS + Flow, $15 USD + $0.10 por CFDI. La API, aparte, ~$1 MXN por timbre.
>
> Sin tienda, el paquete. Con Shopify, la app. Nadie es siempre más barato.
>
> Link del comparativo en bio → cfdi.express/blog/todo-cfdi-vs-cfdi-express
>
> App: apps.shopify.com/cfdi-express
>
> #CFDI #CFDI40 #FacturacionElectronica #SAT #Shopify #ShopifyMexico #FacturaElectronica #CFDIExpress #TodoCFDI #CapturaDigital #EcommerceMexico #TiendaEnLinea #POS #FacturacionShopify #PyME #EmprendedorMexicano #Contador #RFC #Anexo20 #Factura40 #BuiltForShopify #Mexico #AppShopify #Autofacturacion #Timbres

### Media (feed cuadrado)

No hay recorte 1080×1080. Cuando exista el OG, úsalo o recorta el hero. Mientras, generar:

> Imagen cuadrada 1080×1080 estilo Pixar / humano para feed. El comerciante con hoodie teal #055D5E al centro, factura blanca y sello dorado #FFD700. Fondo teal oscuro #055D5E. Texto grande blanco, Racing Sans One, exacto: «Todo CFDI vs CFDI Express». Texto menor Quicksand: «¿Portal de timbres o app de Shopify?». Alto contraste, sin fotos de stock, sin logos de otras marcas, sin hashtags pintados en la imagen.

**Aspecto:** 1080×1080 (1:1).

### Media (historia opcional)

> Historia vertical 1080×1920, estilo Pixar. Fondo teal #055D5E con halo dorado #FFD700. Arriba: wordmark CFDI Express. Centro: comerciante de hoodie teal con una factura. Texto grande exacto: «Todo CFDI vs CFDI Express». Texto menor: «¿Portal de timbres o app de Shopify?». CTA: «Lee el comparativo». Deja el tercio superior e inferior limpios para la UI de Instagram. Sin fotos reales, sin logos ajenos.

**Aspecto:** 1080×1920 (historia).

---

## Checklist de publicación

- [ ] Diego genera hero **1600×900** y OG **1200×630** (Pixar, humano, hoodie teal `#055D5E`).
- [ ] Subir al CDN y poner la URL **sin query** en `image:` del frontmatter.
- [ ] Sustituir el comentario HERO del cuerpo por `![]()`.
- [ ] Rafael revisa el post antes del merge. **No mergear:** en este repo el merge publica en vivo.
- [ ] No publicar el URL del blog hasta `https://cfdi.express/blog/todo-cfdi-vs-cfdi-express`.
- [ ] CTA principal para tiendas = App Store. Portal de Captura Digital citado con link, sin descalificar. API solo como producto aparte.
- [ ] Tono justo: no “Todo CFDI es malo”; no “siempre más baratos”.
- [ ] Cifras alineadas al post (23 sep 2026): $365 / $698 / $1,062 / $1,629 / $3,030 MXN; 100 timbres gratis × 180 días; app $15 + $0.10 USD; API ~$1 MXN.
- [ ] Ortografía en artes: «Todo CFDI vs CFDI Express» / «¿Portal de timbres o app de Shopify?»
- [ ] Sin logos de Captura Digital, Todo CFDI ni de terceros en las imágenes.
- [ ] LinkedIn y Facebook primero; X al mediodía; Instagram por la tarde.
- [ ] Responder comentarios el día de publicación (sobre todo “¿es app de Shopify?” y “¿cuál es más barata?”).
