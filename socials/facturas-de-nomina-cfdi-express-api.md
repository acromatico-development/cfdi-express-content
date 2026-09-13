# Social Pack | Facturas de nómina CFDI: timbra Nómina 1.2 desde la API de CFDI Express

Corresponde al blog post: [`pages/facturas-de-nomina-cfdi-express-api.md`](../pages/facturas-de-nomina-cfdi-express-api.md)

Recursos oficiales para enlazar:
- Post (tras el merge): https://cfdi.express/blog/facturas-de-nomina-cfdi-express-api
- Landing API: https://cfdi.express/api
- Docs interactivas: https://api.cfdi.express/docs
- Dashboard (llaves `sk_test_` / `sk_live_`): https://dash.cfdi.express
- Post relacionado (lanzamiento de la API): https://cfdi.express/blog/lanzamiento-cfdi-express-api
- Post relacionado (webhooks, eventos `nomina.*`): https://cfdi.express/blog/webhooks-cfdi-express-api
- Demo: https://cal.com/team/acromatico-development/cfdi-express

---

## Cómo usar este pack

1. **No regeneres el hero.** Ya está publicado. Usa las URLs de abajo (HTTPS, **sin query**).
2. Publica con el **Texto** sugerido, adaptándolo a tu tono si hace falta.
3. Si el CMS tiene campo `og:image` aparte del hero, usa el recorte OG. Si no, el `image` del frontmatter (1600×900) es el que ya lleva el post.
4. **Banner de email:** solo documentado aquí. Mark lo carga en Klaviyo; no va en el frontmatter del blog.
5. El CTA principal es **docs + dashboard**. Este anuncio es de la **API pública de nómina**, no de la app de Shopify.

---

## Imagen hero del blog

**Ya publicada (usar esta, no generar otra).** Copy en artes: «Nómina en la API» / «CFDI 4.0 + Nómina 1.2». Slug: `facturas-de-nomina-cfdi-express-api`.

| Uso | Medida | URL (sin query) |
| --- | --- | --- |
| Hero / `image` del frontmatter | 1600×900 | https://videos.acromatico.dev/api/images/assets/818cfae7-c2b4-4f32-bd6a-617ce0e877a7.png |
| OG / compartir (`og:image`) | 1200×630 | https://videos.acromatico.dev/api/images/assets/a5b5900b-d85a-4866-bf65-15ee719be5f8.png |
| Banner email (Klaviyo — Mark) | — | https://videos.acromatico.dev/api/images/assets/8f54950f-5e4f-4d7a-99ec-2220f88ebcd4.png |

**Prompt Gemini (solo si hubiera que rehacer una variante):**

> Ilustración estilo Pixar / personaje 3D amigable (no foto real, no stock) para el hero de un blog de developers y RH en México. Personaje: integrador mexicano de unos 30 años, piel morena clara, cabello corto oscuro, hoodie oversized color teal oscuro #055D5E con un pequeño recibo bordado en el pecho, audífonos colgando del cuello. Está de tres cuartos, sonrisa confiada, sosteniendo un recibo de nómina blanco con sello circular dorado #FFD700. Fondo estudio suave en teal #055D5E. Texto grande en tipografía Racing Sans One / Quicksand, blanco, ortografía exacta: «Nómina en la API». Subtítulo: «CFDI 4.0 + Nómina 1.2». Iluminación cinematográfica, alto contraste, sin logos de otras marcas.

**Aspecto:** 16:9 · 1600×900.

---

## LinkedIn

### Texto

> La nómina en México no se rompe en el sueldo. Se rompe en el XML.
>
> Hoy la API de CFDI Express timbra **recibos de nómina**: CFDI 4.0 + complemento **Nómina 1.2**. Un `POST /v1/nominas` por empleado y por periodo.
>
> Tú mandas lo que sí es tuyo — fechas, empleado, percepciones, deducciones. El servidor deriva TotalPercepciones, gravado/exento, ISR, SubTotal, Descuento y Total, y deja el comprobante como el SAT lo exige: TipoDeComprobante N, PUE, sin FormaPago, UsoCFDI CN01, concepto 84111505.
>
> También valida lo que tumba una corrida a las 2 a.m.: registro patronal (obligatorio en contratos 01–08, prohibido en 09/10/99), NSS junto al registro (NOM44) y régimen del empleado siempre 605 (NOM11). Catálogos Nómina 1.2 en `GET /v1/catalogs/nomina/{catalog}`.
>
> Misma cuenta, mismo saldo prepagado, **$1 MXN por timbre**. `Idempotency-Key` obligatoria para que un retry no selle dos veces. Si el SAT tarda, 202 + webhook `nomina.stamped`.
>
> No es la app de Shopify. Es la API para tu ERP, tu motor de RH o la nómina que ya operas.
>
> 📘 Docs: https://api.cfdi.express/docs
> 🔑 Sandbox: https://dash.cfdi.express
> 🧾 Historia de la API: https://cfdi.express/api
>
> #CFDI #FacturacionElectronica #SAT #CFDIExpress #Nomina #API #DesarrolloDeSoftware #RecursosHumanos #Fintech

### Media

**Usar:** recorte OG 1200×630
https://videos.acromatico.dev/api/images/assets/a5b5900b-d85a-4866-bf65-15ee719be5f8.png

**Prompt Gemini (variante, solo si se necesita otra toma):**

> Imagen horizontal 1200×627 (1.91:1), ilustración 3D estilo Pixar/humano para LinkedIn B2B. Integrador con hoodie teal #055D5E frente a un escritorio. Recibo de nómina blanco y laptop con «POST /v1/nominas». Fondo teal #055D5E. Texto exacto en Racing Sans One / Quicksand: «Nómina en la API» y «CFDI 4.0 + Nómina 1.2». Sin fotos de stock.

**Aspecto:** 1200×627 (horizontal, 1.91:1).

---

## X (Twitter)

### Texto (post principal)

> Ya puedes timbrar facturas de nómina CFDI desde la API.
>
> POST /v1/nominas → CFDI 4.0 + Nómina 1.2
> Totales SAT en servidor · $1 MXN/timbre
>
> → https://api.cfdi.express/docs

### Hilo opcional (desarrolla el post)

1. Un recibo de nómina no es “una factura con otro uso”. El SAT cruza gravado/exento, ISR, CN01 y el complemento 1.2. Un peso de más y se cae el XML.
2. POST /v1/nominas: mandas empleado + percepciones/deducciones. La API arma TotalPercepciones, Descuento, Total y fija N / PUE / sin FormaPago / CN01 / 84111505.
3. Empleado como Constancia (mayúsculas, sin acentos). Régimen 605. Registro patronal + NSS según contrato. Catálogos: GET /v1/catalogs/nomina/{catalog}.
4. Idempotency-Key obligatoria. 201 en ≤20 s o 202 bajo carga. Webhooks: nomina.stamped / stamp_failed / cancelled.
5. Mismo saldo que el resto de la API. Docs: https://api.cfdi.express/docs · llaves: https://dash.cfdi.express · contexto: https://cfdi.express/api

### Media

**Usar:** hero 1600×900
https://videos.acromatico.dev/api/images/assets/818cfae7-c2b4-4f32-bd6a-617ce0e877a7.png

**Prompt Gemini (variante):**

> Imagen horizontal 1600×900 (16:9) para X, estilo Pixar/humano. Close-up del integrador con hoodie teal #055D5E, recibo de nómina blanco, sello dorado #FFD700. Fondo teal #055D5E. Texto exacto Racing Sans One: «Nómina en la API». Texto menor Quicksand: «CFDI 4.0 + Nómina 1.2». Sin fotos.

**Aspecto:** 1600×900 (16:9).

---

## Facebook

### Texto

> 📢 La API de CFDI Express ya timbra **recibos de nómina** válidos ante el SAT.
>
> Mandas el empleado, las percepciones y las deducciones. El servidor calcula los totales del complemento Nómina 1.2 y deja el CFDI 4.0 como pide el SAT (tipo N, CN01, sin FormaPago).
>
> Sirve para el ERP o el sistema de RH que ya usas — no es un extra de Shopify. Mismo saldo de siempre: **$1 MXN por timbre**, sandbox gratis.
>
> Docs: https://api.cfdi.express/docs
> Dashboard: https://dash.cfdi.express
> Landing: https://cfdi.express/api
>
> #CFDI #FacturacionElectronica #SAT #CFDIExpress #Nomina

### Media

**Usar:** recorte OG 1200×630
https://videos.acromatico.dev/api/images/assets/a5b5900b-d85a-4866-bf65-15ee719be5f8.png

**Prompt Gemini (variante):**

> Imagen horizontal 1200×630 (1.91:1) para Facebook. Ilustración 3D estilo Pixar: integrador de hoodie teal #055D5E y compañera de RH. Sello dorado #FFD700. Texto exacto: «Nómina en la API» / «CFDI 4.0 + Nómina 1.2». Fondo teal #055D5E. Sin fotos reales.

**Aspecto:** 1200×630 (1.91:1).

---

## Instagram

### Texto (feed)

> 🧾⚡ Las facturas de nómina CFDI ya salen por API.
>
> POST /v1/nominas
> CFDI 4.0 + complemento Nómina 1.2
>
> ✅ Tú mandas empleado y partidas
> ✅ La API calcula los totales SAT
> ✅ Catálogos Nómina 1.2 incluidos
> ✅ $1 MXN por timbre · sandbox gratis
>
> Para tu ERP o tu nómina — no es la app de Shopify.
>
> 👉 Docs y cuenta test: link en bio (api.cfdi.express/docs · dash.cfdi.express)
>
> #CFDI #FacturacionElectronica #SAT #CFDIExpress #Nomina #API #FacturacionDigital #Mexico #Developer #Fintech #CFDI40 #ReciboDeNomina #RecursosHumanos #ERP #IMSS #NominaElectronica #SATMexico #Software #Integraciones #DesarrolloDeSoftware #EmprendedorTech #RH #Payroll #ComplementoNomina #FacturaElectronica

### Media (feed cuadrado)

**Prompt (Gemini):**

> Imagen cuadrada 1080×1080 estilo Pixar/humano para feed. El integrador con hoodie teal #055D5E al centro, recibo blanco y sello dorado #FFD700. Fondo teal oscuro #055D5E. Texto grande blanco, Racing Sans One, exacto: «Nómina en la API». Texto menor Quicksand: «CFDI 4.0 + Nómina 1.2». Alto contraste, sin fotos de stock, sin hashtags pintados en la imagen.

**Aspecto:** 1080×1080 (1:1) para feed.

### Media (historia opcional)

**Prompt (Gemini):**

> Imagen vertical 1080×1920 para historia de Instagram, estilo Pixar. Fondo teal #055D5E con halo dorado #FFD700. Arriba: wordmark CFDI Express. Centro: integrador de hoodie teal con el recibo. Texto grande exacto: «Nómina en la API». Texto menor: «CFDI 4.0 + Nómina 1.2». Deja el tercio superior e inferior limpios para la UI de Instagram. Sin fotos reales.

**Aspecto:** 1080×1920 (historia).

---

## Checklist de publicación

- [ ] Usar las URLs publicadas (hero 1600×900 / OG 1200×630), **sin query string**. Instagram feed/historia sigue el prompt Gemini (no hay recorte 1:1 publicado).
- [ ] Hero del blog: ya está en el frontmatter (`818cfae7-c2b4-4f32-bd6a-617ce0e877a7.png`). No regenerar ni añadir `?w=`.
- [ ] Banner email (`8f54950f-5e4f-4d7a-99ec-2220f88ebcd4.png`): Mark lo pone en Klaviyo. No va en el frontmatter.
- [ ] Esperar el merge del post antes de pegar https://cfdi.express/blog/facturas-de-nomina-cfdi-express-api (si aún no está live, usa docs + dash + landing).
- [ ] Verificar enlaces: docs, dash, landing API, post de lanzamiento, post de webhooks.
- [ ] No enlazar la app de Shopify ni hablar de planes USD de la tienda: este pack es **API de nómina**.
- [ ] Programar: LinkedIn y Facebook primero (mañana), X al mediodía, Instagram por la tarde/noche.
- [ ] Responder comentarios de integradores (totales SAT, registro patronal, 201 vs 202, `Idempotency-Key`).
