---
name: blog-content-pack
description: Create a complete CFDI Express content package — a blog post in `pages/` plus a corresponding social pack in `socials/` with suggested copy for LinkedIn, X, Facebook and Instagram and Gemini media prompts. Load when asked to write a blog entry, launch/feature announcement, or social media posts for CFDI Express. Auto-loads when creating/editing files under `pages/` or `socials/`.
user-invocable: true
---

# Blog + Social Pack para CFDI Express

Crea juntos: **el blog post** (`pages/<slug>.md`) y **su pack de redes** (`socials/<slug>.md`) con texto sugerido y prompts de media para Gemini. Siempre se crean en paquete — un post sin socials no es un deliverable completo.

## Flujo de trabajo

1. **Reunir hechos.** Investigar antes de escribir. Si el tema toca el API, leer la landing oficial de `https://cfdi.express/api` y/o el repo `cfdi-express-api` (docs técnicos, endpoints reales, precios). Si es una funcionalidad del app de Shopify, revisar `docs/` del contenido y el changelog. Nunca inventar datos, precios o endpoints.
2. **Definir slug.** Nombre descriptivo en español, minúsculas, guiones, sin acentos (ej. `lanzamiento-cfdi-express-api`). El blog y su socials comparten el **mismo slug**.
3. **Escribir el blog post** en `pages/<slug>.md` (ver convenciones).
4. **Escribir el social pack** en `socials/<slug>.md` (ver convenciones).
5. **Auto-revisar** con el checklist final, incluyendo enlaces vivos y datos correctos.

## Convenciones del blog post (`pages/`)

### Frontmatter (obligatorio, en este orden)

```yaml
---
title: "..."
description: "..."
image: ""
author: "Rafael González"
date: "YYYY-MM-DD"
keywords: "palabra clave, otra"
---
```

- `author`: los posts recientes son de **Rafael González** (preguntar si hay duda).
- `date`: fecha de publicación en formato ISO (hoy).
- `keywords`: separadas por comas, incluye términos de búsqueda reales (ej. "CFDI, facturación, México, SAT").
- `image`: URL de hero. Si no hay imagen generada aún, usar el placeholder
  `https://videos.acromatico.dev/api/images/assets/PENDIENTE-<SLUG>.png?w=1600`
  y añadir un comentario `<!-- TODO: ... -->` + `<!-- ENDTODO -->` en el cuerpo explicando que se genera con prompt de Gemini y se sube a videos.acromatico.dev. Nunca dejar la imagen sin marcar.

### Estructura y tono

- Idioma: **español de México**, trato directo en segunda persona, tono práctico y cercano (no académico).
- Título con `# `, secciones con `## `/`### `. Longitud libre, posts de lanzamiento ~130–200 líneas.
- Incluir un CTA claro al final (registrarse, instalar, agendar demo, contactar).
- Botones/enlaces de referencia: landing `https://cfdi.express`, API `https://cfdi.express/api`, dashboard `https://dash.cfdi.express`, docs API `https://api.cfdi.express/docs`, app `https://apps.shopify.com/cfdi-express`, contacto `hola@cfdi.express`, demo `https://cal.com/team/acromatico-development/cfdi-express`.
- Embeds de video (patrón establecido):
  ```html
  <iframe width="100%" height="400" src="https://www.youtube.com/embed/<VIDEO_ID>" title="..." frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
  ```
  Más el link `[Ver tutorial en YouTube](https://youtu.be/<VIDEO_ID>)`.
- Pendientes (capturas, videos, imágenes) siempre como bloques `<!-- TODO: descripción -->` … `<!-- ENDTODO -->` (el renderer los elimina).

### Datos clave del API (últimos verificados, agosto 2026)

- Endpoints: facturas `POST /v1/invoices` (PUE/PPD/globales), complementos `POST /v1/invoices/{id}/payments`, cancelaciones `POST /v1/invoices/{id}/cancel`, notas de crédito `POST /v1/credit_notes`, emisores/CSDs `/v1/merchants`, catálogos `/v1/catalogs/*` (52,513 claves de producto, 2,418 unidades), saldo `/v1/balance`, billing `/v1/billing/*`.
- Precio: **$1 MXN por timbre**, saldo prepagado, recargas $100–$50,000 MXN vía Stripe, descuentos por volumen. Sandbox (`sk_test_`) gratis.
- Convenciones estilo Stripe: `sk_test_`/`sk_live_`, `livemode`, idempotencia (`Idempotency-Key`), errores `problem+json`, docs interactivas Scalar.
- MCP server en `https://api.cfdi.express/mcp` (OAuth con claude.ai/ChatGPT; API key con Claude Code/Cursor/Windsurf; test en `/mcp/test`). 13 herramientas. Video tutorial: `https://youtu.be/urs7klK6p1U`.
- Confiabilidad verificada: 35 timbres concurrentes sin dobles timbres, p95 1.57 s, reembolso automático si el SAT rechaza, multi-CSD/multi-RFC con AES-256-GCM.

## Convenciones del social pack (`socials/`)

Mismo slug que el post: `socials/<slug>.md`.

### Estructura del archivo

```markdown
# Social Pack | <Título del post>

Corresponde al blog post: [`pages/<slug>.md`](../pages/<slug>.md)

Recursos oficiales para enlazar: (landing, dashboard, docs, video, app…)

---

## Cómo usar este pack
(instrucciones breves: generar media con Gemini, publicar con el texto sugerido)

## Imagen hero del blog
(prompt para la imagen del frontmatter del post + aspecto 16:9)

## LinkedIn
### Texto
### Media (prompt Gemini + aspecto)

## X (Twitter)
### Texto (post principal + hilo opcional)
### Media

## Facebook
### Texto
### Media

## Instagram
### Texto (feed)
### Media (feed cuadrado + historia opcional)

## Checklist de publicación
```

### Reglas por plataforma

| Plataforma | Tono | Largo texto | Media |
|---|---|---|---|
| LinkedIn | profesional, B2B | 150–250 palabras | 1200×627 (1.91:1) |
| X (Twitter) | directo, hook arriba | ~280 caracteres + hilo opcional | 1600×900 (16:9) |
| Facebook | cercano, negocio | párrafos cortos, 3–6 líneas | 1200×630 (1.91:1) |
| Instagram | visual, emoji, hashtags | caption + hasta 30 hashtags | feed 1080×1080 (1:1); historia 1080×1920 |

- Siempre incluir `hashtags` adecuados (#CFDI, #FacturacionElectronica, #SAT, #CFDIExpress).
- Siempre incluir 1–2 **prompts completos para Gemini** por plataforma (textuales, descriptivos, en español, con la paleta de marca).
- Incluir al menos un CTA por texto (registrarse, ver tutorial, instalar app).
- Si hay video del tema, añadirlo como recurso y CTA en las publicaciones relevantes.

### Guía de marca para los prompts de media (Gemini)

- **Color primario:** teal oscuro `#055D5E`. **Acentos:** dorado `#FFD700`, blanco, gris `#D7D7D7`.
- **Tipografías:** Racing Sans One (títulos), Quicksand (cuerpo).
- **Iconografía:** factura/recibo con sello, código QR/barras, `{ }`/`</>` código, nodos/API, burbuja de chat IA, engranajes, reloj de pagos, flecha de cancelación, nube.
- **Estilo:** ilustración flat/moderna, minimalista, profesional B2B, alto contraste, sin fotografías de stock salvo que el tema lo amerite.
- Incluir SIEMPRE en el prompt: paleta `#055D5E` + acento dorado, tipografía, aspecto/medidas exactas y qué texto debe llevar la imagen (revisar ortografía).

## Checklist final

- [ ] Blog con frontmatter completo y `date` correcto.
- [ ] Slug del socials idéntico al del blog.
- [ ] Sin datos, precios o endpoints inventados (verificar contra la fuente).
- [ ] Enlaces vivos y con la ruta correcta.
- [ ] Al menos un CTA por publicación y un CTA al final del blog.
- [ ] TODOs marcados con `<!-- TODO -->` / `<!-- ENDTODO -->`.
- [ ] Prompts de media con paleta de marca, aspecto y texto correctos.
- [ ] Si el usuario no pidió lo contrario: texto en español de México.