# Social Pack | Factura con pedimento en CFDI 4.0: cuándo va y cómo emitirla por API

Corresponde al blog post: [`pages/factura-con-pedimento-cfdi-express-api.md`](../pages/factura-con-pedimento-cfdi-express-api.md)

Recursos oficiales para enlazar:
- Post (tras el merge): https://cfdi.express/blog/factura-con-pedimento-cfdi-express-api
- Landing API: https://cfdi.express/api
- Docs interactivas: https://api.cfdi.express/docs
- OpenAPI en vivo: https://api.cfdi.express/openapi.json
- Dashboard (llaves `sk_test_` / `sk_live_`): https://dash.cfdi.express
- Anexo 20 (RMF 2022, PDF): http://omawww.sat.gob.mx/tramitesyservicios/Paginas/documentos/Anexo20_2022.pdf
- Post relacionado (datos fiscales del receptor): https://cfdi.express/blog/cfdi-facturas-4-0
- Post relacionado (lanzamiento de la API): https://cfdi.express/blog/lanzamiento-cfdi-express-api
- Demo: https://cal.com/team/acromatico-development/cfdi-express

---

## Cómo usar este pack

1. **No regeneres el hero.** Diego ya subió el arte. Usa las URLs de abajo (HTTPS, **sin query**).
2. El `image` del frontmatter es el **hero 1600×900**. El recorte **OG 1200×630** es para redes y para un `og:image` aparte, si el CMS lo tiene. Si no hay campo OG, el hero del frontmatter es el que ya lleva el post.
3. Publica con el **Texto** sugerido. El CTA principal es **docs + dashboard**. Este anuncio es de la **API pública** (`items[].pedimentos` en facturas y notas de crédito), no de la app de Shopify.
4. El pedimento va en el **concepto**, en venta de **primera mano** de mercancía importada. No digas que toda factura de importación lo lleva, ni que el MCP lo arma: el servidor MCP no expone el campo.
5. Pide **datos fiscales** (RFC, nombre o razón social, régimen y CP). No pidas el PDF de la Constancia de Situación Fiscal.
6. No publiques el link del blog hasta que el post esté en `https://cfdi.express/blog/factura-con-pedimento-cfdi-express-api` (después del merge). **No mergear hasta que Rafael revise.**

---

## Imagen hero del blog

**Ya publicada (usar esta, no generar otra).** Copy en artes: «Factura con pedimento» / «CFDI 4.0 por API». Slug: `factura-con-pedimento-cfdi-express-api`.

| Uso | Medida | URL (sin query) |
| --- | --- | --- |
| Hero / `image` del frontmatter | 1600×900 | https://videos.acromatico.dev/api/images/assets/9b42be87-d026-4d28-a6fe-6f0f2c878dec.png |
| OG / compartir (`og:image`, Meta / LinkedIn) | 1200×630 | https://videos.acromatico.dev/api/images/assets/77e7112a-a6bd-4763-8e00-76293fb3d5fe.png |

El frontmatter `image` usa el hero, sin query. En el cuerpo del post la misma URL lleva `?w=1600`. El OG queda para los packs de redes y para usarlo después si el sitio separa `og:image`.

Copy en el arte, ortografía exacta:
- Título: «Factura con pedimento»
- Subtítulo: «CFDI 4.0 por API»

Personaje: humano estilo Pixar (no foto, no stock), playera blanca y overshirt verde olivo, documento de aduana con sello. Sin logos del SAT ni de otras marcas.

**Prompt Gemini (solo si hubiera que rehacer una variante; el arte oficial es el de Diego) — hero 1600×900:**

> Ilustración 3D estilo Pixar / personaje humano amigable (no foto real, no stock) para el hero de un blog B2B de facturación en México. Personaje: hombre mexicano de unos 32 años, piel morena, cabello corto oscuro, barba cuidada, playera blanca y overshirt verde olivo, de pie de tres cuartos con una sonrisa. En las manos, un documento blanco de pedimento aduanal con sello circular dorado #FFD700 y filas de texto; en la esquina, un recuadro con llaves `{ }`. Fondo estudio en teal oscuro #055D5E, acentos dorados #FFD700, gris #D7D7D7. Texto grande en tipografía Racing Sans One, blanco, ortografía exacta: «Factura con pedimento». Subtítulo en Quicksand, acento dorado: «CFDI 4.0 por API». Iluminación cinematográfica, alto contraste, sin fotografías, sin logos de otras marcas.

**Aspecto:** 16:9 · 1600×900.

**Prompt Gemini — OG 1200×630:**

> Imagen horizontal 1200×630 (1.91:1) estilo Pixar / humano para Open Graph. El mismo personaje con playera blanca y overshirt verde olivo, documento de pedimento con sello dorado #FFD700. Fondo teal oscuro #055D5E. Texto exacto en Racing Sans One, blanco: «Factura con pedimento». Subtítulo en Quicksand, dorado #FFD700: «CFDI 4.0 por API». Sin fotos de stock, sin logos de otras marcas. Alto contraste, profesional. Deja aire para que el título no se corte en el recorte de LinkedIn.

**Aspecto:** 1200×630 (1.91:1).

---

## LinkedIn

### Texto

> Si vendes mercancía importada de primera mano, el número de pedimento no va en la cabecera del CFDI 4.0. Va en el concepto: nodo InformacionAduanera, atributo NumeroPedimento.
>
> El Anexo 20 lo describe para esa venta de primera mano (y pide no registrarlo cuando el CFDI lleva el complemento de comercio exterior). Son 15 dígitos —año, aduana, patente y consecutivo— escritos en 21 caracteres, con dos espacios entre cada grupo.
>
> En CFDI Express API el campo es `items[].pedimentos`, opcional, en `POST /v1/invoices` y en `POST /v1/credit_notes`. Acepta el número con o sin espacios (de 1 a 100 por concepto) y lo emite en la forma del SAT. Los duplicados se descartan. La aduana, la patente y el consecutivo siguen siendo validación del SAT: la API publica el patrón, no esos catálogos.
>
> Una reventa no lleva pedimento solo porque el producto algún día se importó. Y el servidor MCP no expone este campo: va en el JSON del REST.
>
> Del receptor, pide datos fiscales (RFC, nombre o razón social, régimen y CP).
>
> Guía: https://cfdi.express/blog/factura-con-pedimento-cfdi-express-api
> Docs: https://api.cfdi.express/docs
> Sandbox: https://dash.cfdi.express
>
> #CFDI #FacturacionElectronica #SAT #CFDIExpress #Pedimento #Importacion #CFDI40 #API #ComercioExterior

### Media

**Usar:** recorte OG de Diego (1200×630)
https://videos.acromatico.dev/api/images/assets/77e7112a-a6bd-4763-8e00-76293fb3d5fe.png

**Prompt Gemini (variante, solo si se necesita otra toma):**

> Imagen horizontal 1200×627 (1.91:1) estilo Pixar / humano para LinkedIn B2B. Personaje con playera blanca y overshirt verde olivo, documento de pedimento con sello dorado #FFD700 y un bloque `{ }` en teal. Fondo teal oscuro #055D5E. Texto exacto en Racing Sans One / Quicksand, blanco y dorado: «Factura con pedimento» y «CFDI 4.0 por API». Sin fotos de stock, sin logos de otras marcas. Alto contraste, profesional.

**Aspecto:** 1200×627 (1.91:1).

---

## X (Twitter)

### Texto (post principal)

> Factura con pedimento en CFDI 4.0: va en el concepto, no en la cabecera.
>
> 15 dígitos → 21 caracteres, con dos espacios.
>
> API: items[].pedimentos
> → https://cfdi.express/blog/factura-con-pedimento-cfdi-express-api

### Hilo opcional (desarrolla el post)

1. InformacionAduanera es opcional en el concepto. El Anexo 20 la describe para la venta de primera mano de mercancía importada. Si el nodo va, NumeroPedimento es obligatorio. En una reventa, no corresponde.
2. Formato: 2 (año de validación) + 2 (aduana) + 4 (patente) + 7 (último dígito del año en curso, con las salvedades del anexo, y 6 de folio progresivo). Entre grupos, dos espacios. Longitud 21.
3. Errores de siempre: un solo espacio en el XML (18 caracteres), guiones, dígitos de menos, o poner el pedimento a nivel factura. En la API, con o sin espacios se acepta; el XML sale con los dos espacios. Guiones o longitud mala → 400.
4. POST /v1/invoices y POST /v1/credit_notes. Campo opcional items[].pedimentos, 1 a 100 por concepto, duplicados se descartan. Idempotency-Key obligatoria. El MCP no trae este campo.
5. La API no consulta c_Aduana ni c_PatenteAduanal. Eso lo valida el SAT. Docs: https://api.cfdi.express/docs · llaves: https://dash.cfdi.express

### Media

**Usar:** hero 1600×900
https://videos.acromatico.dev/api/images/assets/9b42be87-d026-4d28-a6fe-6f0f2c878dec.png

**Prompt Gemini (variante, solo si se necesita otra toma):**

> Imagen 1600×900 (16:9) para X, estilo Pixar / humano. Close-up del personaje con overshirt verde olivo, documento de pedimento blanco, sello dorado #FFD700. Fondo teal #055D5E. Texto exacto Racing Sans One: «Factura con pedimento». Texto menor Quicksand: «CFDI 4.0 por API». Sin fotos, sin logos ajenos.

**Aspecto:** 1600×900 (16:9).

---

## Facebook

### Texto

> ¿Te pidieron factura con pedimento?
>
> En CFDI 4.0 el número va en el concepto (información aduanera), cuando vendes de primera mano mercancía que tú importaste. Son 15 dígitos escritos con dos espacios entre año, aduana, patente y consecutivo: 21 caracteres.
>
> Si timbras por API, el campo es items[].pedimentos. Lo puedes mandar con o sin espacios; CFDI Express lo deja en la forma del SAT. También en la nota de crédito.
>
> Pide datos fiscales (RFC, nombre, régimen y CP). La guía está aquí: https://cfdi.express/blog/factura-con-pedimento-cfdi-express-api
>
> Prueba en sandbox: https://dash.cfdi.express
> Docs: https://api.cfdi.express/docs
>
> #CFDI #FacturacionElectronica #SAT #CFDIExpress #Pedimento #Importacion

### Media

**Usar:** recorte OG 1200×630
https://videos.acromatico.dev/api/images/assets/77e7112a-a6bd-4763-8e00-76293fb3d5fe.png

**Prompt Gemini (variante, solo si se necesita otra toma):**

> Imagen 1200×630 (1.91:1) para Facebook. Ilustración 3D estilo Pixar: personaje de playera blanca y overshirt verde olivo con un pedimento y sello dorado #FFD700. Texto exacto: «Factura con pedimento» / «CFDI 4.0 por API». Fondo teal #055D5E. Sin fotos reales, sin logos de terceros.

**Aspecto:** 1200×630 (1.91:1).

---

## Instagram

### Texto (feed)

> Factura con pedimento en CFDI 4.0: el número va en el concepto.
>
> Venta de primera mano de mercancía importada.
> 15 dígitos. 21 caracteres. Dos espacios entre grupos.
>
> En la API:
> items[].pedimentos
> POST /v1/invoices y notas de crédito
> Con o sin espacios. Hasta 100 por concepto.
>
> Pide datos fiscales (RFC, nombre, régimen y CP).
>
> Guía en bio → cfdi.express/blog/factura-con-pedimento-cfdi-express-api
> Docs: api.cfdi.express/docs
>
> #CFDI #CFDI40 #FacturacionElectronica #SAT #CFDIExpress #Pedimento #Importacion #ComercioExterior #Aduana #NumeroPedimento #InformacionAduanera #FacturaElectronica #API #Mexico #PyME #Importadores #Anexo20 #Factura40 #DesarrolloDeSoftware #ERP #Integraciones #SATMexico #Logistica #ComercioInternacional #PrimeraMano

### Media (feed cuadrado)

No hay recorte 1080×1080 en CDN. **Usar el OG 1200×630** (o recortar el hero al cuadrado):
https://videos.acromatico.dev/api/images/assets/77e7112a-a6bd-4763-8e00-76293fb3d5fe.png

**Prompt Gemini (variante, solo si se necesita un 1:1):**

> Imagen cuadrada 1080×1080 estilo Pixar / humano para feed. El personaje con playera blanca y overshirt verde olivo al centro, documento de pedimento y sello dorado #FFD700. Fondo teal oscuro #055D5E. Texto grande blanco, Racing Sans One, exacto: «Factura con pedimento». Texto menor Quicksand: «CFDI 4.0 por API». Alto contraste, sin fotos de stock, sin logos de otras marcas, sin hashtags pintados en la imagen.

**Aspecto:** 1080×1080 (1:1).

### Media (historia opcional)

**Usar el OG** y enmarcar en 1080×1920 con fondo #055D5E, o generar:

> Historia vertical 1080×1920, estilo Pixar. Fondo teal #055D5E con halo dorado #FFD700. Arriba: wordmark CFDI Express. Centro: personaje con el documento de pedimento. Texto grande exacto: «Factura con pedimento». Texto menor: «CFDI 4.0 por API». CTA: «Lee la guía». Deja el tercio superior e inferior limpios para la UI de Instagram. Sin fotos reales, sin logos ajenos.

**Aspecto:** 1080×1920 (historia).

---

## Checklist de publicación

- [x] Diego subió hero 1600×900 y OG 1200×630 a `videos.acromatico.dev`. Frontmatter `image:` = hero **sin query**. El cuerpo del post usa `?w=1600`. El OG queda para redes / `og:image` opcional.
- [ ] Usar las URLs de Diego (hero / OG), **sin query string**.
- [ ] Rafael revisa el post antes del merge. **No mergear:** en este repo el merge publica en vivo.
- [ ] No publicar el URL del blog hasta `https://cfdi.express/blog/factura-con-pedimento-cfdi-express-api`.
- [ ] CTA principal = docs + dashboard. No empujar la app de Shopify ni decir que el MCP timbra pedimentos.
- [ ] Datos fiscales (RFC, nombre, régimen, CP). No pedir el PDF de la Constancia de Situación Fiscal.
- [ ] Cifras alineadas al post: 15 dígitos, 21 caracteres, dos espacios, 1–100 por concepto, patrón del OpenAPI, $1 MXN por timbre solo como precio publicado de la API (no hay tarifa aparte para pedimento).
- [ ] Sin códigos de error del SAT inventados. El 400 y el 422 son los de la spec (validación / rechazo SAT o reuso de Idempotency-Key).
- [ ] Ortografía en artes: «Factura con pedimento» / «CFDI 4.0 por API».
- [ ] Sin logos del SAT ni de terceros en las imágenes.
- [ ] LinkedIn y Facebook primero; X al mediodía; Instagram por la tarde.
- [ ] Responder comentarios el día de publicación (sobre todo “¿es obligatorio?” y “¿con o sin espacios?”).
