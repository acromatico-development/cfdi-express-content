# Social Pack | Facturapi vs CFDI Express API: ¿cuál API de CFDI elegir?

Corresponde al blog post: [`pages/facturapi-vs-cfdi-express-api.md`](../pages/facturapi-vs-cfdi-express-api.md)

Recursos oficiales para enlazar:
- Post (tras el merge): https://cfdi.express/blog/facturapi-vs-cfdi-express-api
- Landing API: https://cfdi.express/api
- Dashboard (llaves `sk_test_` / `sk_live_`): https://dash.cfdi.express
- Docs interactivas: https://api.cfdi.express/docs
- MCP (agentes): https://api.cfdi.express/mcp
- Post relacionado (lanzamiento API): https://cfdi.express/blog/lanzamiento-cfdi-express-api
- Post relacionado (webhooks): https://cfdi.express/blog/webhooks-cfdi-express-api
- Post relacionado (nómina): https://cfdi.express/blog/facturas-de-nomina-cfdi-express-api
- Comparativo de **apps** Shopify (no mezclar): https://cfdi.express/blog/facturama-vs-cfdi-express
- Facturapi (pricing público, para no inventar): https://www.facturapi.io/pricing
- Demo: https://cal.com/team/acromatico-development/cfdi-express

---

## Cómo usar este pack

1. **Hero pendiente.** Diego genera el arte con el prompt de abajo (Pixar / humano, hoodie teal, **sin logos de Facturapi ni de terceros**), lo sube a `videos.acromatico.dev` y sustituye el `PENDIENTE-…` del frontmatter. Hasta entonces no publiques el recorte como si ya estuviera en CDN.
2. Publica con el **Texto** sugerido. El CTA principal es **API + dashboard** (`cfdi.express/api`, `dash.cfdi.express`). La app de Shopify va como mención corta (“otro producto”), no como gancho.
3. Es un comparativo **justo**: no insultes a Facturapi, no inventes precios, no digas que CFDI Express es “siempre más barata”. Califica “según pricing público, septiembre 2026”.
4. No publiques el link del blog hasta que el post esté en `https://cfdi.express/blog/facturapi-vs-cfdi-express-api` (después del merge). Mientras, usa la landing de la API.
5. Si el CMS tiene `og:image` aparte, pide a Diego un recorte 1200×630 del mismo arte.

---

## Imagen hero del blog

**Placeholder en frontmatter (hasta que Diego suba el archivo):**

`https://videos.acromatico.dev/api/images/assets/PENDIENTE-facturapi-vs-cfdi-express-api.png?w=1600`

Copy en artes: «Facturapi vs CFDI Express API» / «¿Cuál API de CFDI elegir?». Slug: `facturapi-vs-cfdi-express-api`.

**Prompt (Gemini) — Diego, este es el oficial del hero:**

> Ilustración 3D estilo Pixar / personaje humano amigable (no foto real, no stock) para el hero de un blog B2B de developers en México. Personaje: integrador mexicano de unos 30 años, piel morena clara, cabello corto oscuro, hoodie oversized color teal oscuro #055D5E con un pequeño recibo bordado en el pecho, audífonos colgando del cuello. Está de tres cuartos, sonrisa confiada, sosteniendo dos facturas blancas lado a lado (sello circular dorado #FFD700 en cada una) como si comparara dos APIs; detrás, nodos de API y llaves «{ }» discretas. No uses logos de marcas ajenas (nada de Facturapi, Stripe, Shopify, ChatGPT ni Claude). Fondo estudio suave en teal #055D5E, acentos dorados #FFD700, gris #D7D7D7. Texto grande en tipografía Racing Sans One, blanco, ortografía exacta: «Facturapi vs CFDI Express API». Subtítulo en Quicksand, acento dorado: «¿Cuál API de CFDI elegir?». Iluminación cinematográfica, alto contraste, sin fotografías.

**Aspecto:** 16:9 · 1600×900 · subir a `videos.acromatico.dev` y copiar la URL al campo `image:` del blog (sin `?w=`).

**Recorte OG (pedir a Diego junto con el hero):** 1200×630 (1.91:1), mismo personaje y mismo copy.

---

## LinkedIn

### Texto

> Si tu SaaS, ERP o marketplace tiene que **timbrar CFDI 4.0**, no estás eligiendo una app de Shopify. Estás eligiendo una **API REST**.
>
> Publicamos un comparativo justo: **Facturapi vs CFDI Express API**. Facturapi es una marca madura — docs, SDKs (Node, .NET, PHP), multi-RFC, Carta Porte documentada — y su pricing público (septiembre 2026) anda en **~$299 MXN/mes + ~$0.60 MXN por timbre**. CFDI Express API es **~$1 MXN por timbre**, saldo prepagado, **sin mensualidad** del producto API, webhooks, nómina y servidor **MCP** para agentes.
>
> Cuenta ilustrativa (lista, sin descuentos): a 100 timbres/mes gana el prepago; a 2,000, si pagas la mensualidad, gana el $0.60. El cruce está cerca de **750 timbres/mes**. **No somos siempre más baratos.**
>
> Ojo: esto no es [Facturama vs CFDI Express](https://cfdi.express/blog/facturama-vs-cfdi-express). Eso es la app de Shopify (USD/mes). Facturapi ≠ Facturama. La app nuestra es otro SKU.
>
> Tabla, escenarios en MXN y un “quién debería elegir cuál” (developers e integradores).
>
> Lee el comparativo: https://cfdi.express/blog/facturapi-vs-cfdi-express-api
>
> Prueba el sandbox (gratis, `sk_test_`): https://dash.cfdi.express
> Landing API: https://cfdi.express/api
>
> #CFDI #FacturacionElectronica #SAT #API #CFDIExpress #Facturapi #MCP #DesarrolloDeSoftware #Fintech

### Media

**Usar:** recorte OG 1200×630 (cuando Diego lo suba). Hasta entonces, generar con el prompt.

**Prompt (Gemini):**

> Imagen horizontal 1200×627 (1.91:1) estilo Pixar / humano para LinkedIn B2B. Integrador con hoodie oversized teal #055D5E, dos facturas blancas en las manos, sello dorado #FFD700, nodos de API «{ }» al fondo. Fondo teal oscuro #055D5E. Texto exacto en Racing Sans One / Quicksand, blanco y dorado: «Facturapi vs CFDI Express API» y «¿Cuál API de CFDI elegir?». Sin fotos de stock, sin logos de otras marcas. Alto contraste, profesional.

**Aspecto:** 1200×627 (horizontal, 1.91:1).

---

## X (Twitter)

### Texto (post principal)

> Facturapi vs CFDI Express API (2026), con pricing público.
>
> ~$299/mes + $0.60/timbre vs $1/timbre sin cuota. MCP, sandbox, multi-RFC. Quién debería elegir cuál.
>
> → https://cfdi.express/blog/facturapi-vs-cfdi-express-api

### Hilo opcional (desarrolla el post)

1. Esto es API REST para SaaS/ERP, no la app de Shopify. El duelo de apps es Facturama vs CFDI Express. Facturapi ≠ Facturama.
2. Según pricing público (sep. 2026): Facturapi ~$299 MXN/mes + ~$0.60/timbre (consumibles al mes siguiente). CFDI Express API ~$1 MXN/timbre, prepago, $0 de cuota API. Sandbox: ellos 14 días; nosotros `sk_test_` gratis.
3. Fortalezas Facturapi: docs, SDKs Node/.NET/PHP, multi-RFC sin extra, Carta Porte publicada. Fortalezas nuestras: MCP (api.cfdi.express/mcp), Stripe-like keys, webhooks, nómina, app hermana de Shopify (otro precio).
4. Ilustrativo: 100 timbres → ~$359 vs ~$100. 500 → ~$599 vs ~$500. 2,000 → ~$1,499 vs ~$2,000. Cruce ~750/mes. Nunca “siempre más baratos”.
5. Guía: https://cfdi.express/blog/facturapi-vs-cfdi-express-api · sandbox: https://dash.cfdi.express · landing: https://cfdi.express/api

### Media

**Usar:** hero 1600×900 (cuando Diego lo suba).

**Prompt (Gemini):**

> Imagen 1600×900 (16:9) para X, estilo Pixar / humano. Close-up del integrador con hoodie teal #055D5E, dos facturas, sello dorado #FFD700, un nodo «{ }» discreto. Fondo teal #055D5E. Texto exacto Racing Sans One: «Facturapi vs CFDI Express API». Texto menor Quicksand: «¿Cuál API de CFDI elegir?». Sin fotos, sin logos ajenos.

**Aspecto:** 1600×900 (16:9).

---

## Facebook

### Texto

> ¿Facturapi o CFDI Express API para timbrar CFDI desde tu sistema?
>
> Publicamos un comparativo con lo que cada una anuncia en público (septiembre 2026): ~$299 MXN al mes + ~$0.60 por timbre vs ~$1 por timbre **sin mensualidad** de la API. Docs y SDKs de un lado; MCP y prepago del otro. Sin inventar precios y sin hablar mal de nadie.
>
> En la cuenta ilustrativa, a poco volumen gana el prepago; a mucho volumen (si pagas la cuota) gana el $0.60. Nadie es “siempre más barato”.
>
> No es la app de Shopify: eso va en otro post (Facturama vs CFDI Express).
>
> Lee la tabla: https://cfdi.express/blog/facturapi-vs-cfdi-express-api
>
> Sandbox gratis: https://dash.cfdi.express
> API: https://cfdi.express/api
>
> #CFDI #FacturacionElectronica #SAT #CFDIExpress #API

### Media

**Usar:** recorte OG 1200×630 (cuando Diego lo suba).

**Prompt (Gemini):**

> Imagen 1200×630 (1.91:1) para Facebook. Ilustración 3D estilo Pixar: integrador de hoodie teal #055D5E y una compañera developer con laptop (`POST /v1/invoices` apenas legible, sin logos). Dos facturas, sello dorado #FFD700. Texto exacto: «Facturapi vs CFDI Express API» / «¿Cuál API de CFDI elegir?». Fondo teal #055D5E. Sin fotos reales, sin logos de terceros.

**Aspecto:** 1200×630 (1.91:1).

---

## Instagram

### Texto (feed)

> ¿Facturapi o CFDI Express API? Si timbras CFDI desde código en México, esta es la pregunta de 2026.
>
> Facturapi (pricing público, sep. 2026): ~$299 MXN/mes + ~$0.60/timbre, docs, SDKs, Carta Porte.
>
> CFDI Express API: ~$1 MXN/timbre, sin cuota API, sandbox `sk_test_`, MCP para agentes.
>
> A 100 timbres gana el prepago. A 2,000, si pagas la mensualidad, gana el $0.60. No es “siempre más barata”.
>
> Esto no es la app de Shopify (ese vs es Facturama).
>
> Link del comparativo en bio → cfdi.express/blog/facturapi-vs-cfdi-express-api
>
> API: cfdi.express/api
>
> #CFDI #CFDI40 #FacturacionElectronica #SAT #API #Facturapi #CFDIExpress #MCP #FacturaElectronica #Mexico #Developer #Fintech #ERP #SaaS #Integraciones #DesarrolloDeSoftware #RFC #Anexo20 #FacturacionDigital #PyME #EmprendedorTech #IA #AgentesIA #RESTAPI #Sandbox #Nomina #Webhooks #SATMexico

### Media (feed cuadrado)

No hay recorte 1080×1080 en CDN todavía. Generar 1:1 o recortar el hero:

**Prompt (Gemini):**

> Imagen cuadrada 1080×1080 estilo Pixar / humano para feed. El integrador con hoodie teal #055D5E al centro, dos facturas blancas y sello dorado #FFD700. Fondo teal oscuro #055D5E. Texto grande blanco, Racing Sans One, exacto: «Facturapi vs CFDI Express API». Texto menor Quicksand: «¿Cuál API de CFDI elegir?». Alto contraste, sin fotos de stock, sin logos de otras marcas, sin hashtags pintados en la imagen.

**Aspecto:** 1080×1080 (1:1) para feed.

### Media (historia opcional)

**Prompt (Gemini):**

> Historia vertical 1080×1920, estilo Pixar. Fondo teal #055D5E con halo dorado #FFD700. Arriba: wordmark CFDI Express. Centro: integrador de hoodie teal con dos facturas. Texto grande exacto: «Facturapi vs CFDI Express API». Texto menor: «¿Cuál API de CFDI elegir?». CTA: «Lee el comparativo». Deja el tercio superior e inferior limpios para la UI de Instagram. Sin fotos reales, sin logos ajenos.

**Aspecto:** 1080×1920 (historia).

---

## Checklist de publicación

- [ ] Esperar a que Diego suba hero 1600×900 y OG 1200×630 a `videos.acromatico.dev`; actualizar `image:` del post (quitar `PENDIENTE` y `?w=`).
- [ ] No publicar el URL del blog hasta `https://cfdi.express/blog/facturapi-vs-cfdi-express-api` (después del merge). Mientras: `https://cfdi.express/api`.
- [ ] CTA principal = landing API + dashboard. App Shopify solo como “otro producto / no mezclar USD”.
- [ ] Tono justo: no “Facturapi es mala”; no “siempre más baratos”. Calificar pricing público sep. 2026.
- [ ] Cifras alineadas al post: Facturapi ~$299 + $0.60; CFDI Express API ~$1, $0 cuota; cruce ~750; escenarios 100 / 500 / 2000.
- [ ] Ortografía en artes: «Facturapi vs CFDI Express API» / «¿Cuál API de CFDI elegir?»
- [ ] Sin logos de Facturapi ni de terceros en las imágenes.
- [ ] LinkedIn y Facebook primero; X al mediodía; Instagram por la tarde.
- [ ] Responder comentarios el día de publicación (sobre todo “¿cuál es más barata?” y “¿esto es Facturama?”).
