# Social Pack | Webhooks CFDI: notificaciones en tiempo real en CFDI Express API

Corresponde al blog post: [`pages/webhooks-cfdi-express-api.md`](../pages/webhooks-cfdi-express-api.md)

Recursos oficiales para enlazar:
- Post (tras el merge): https://cfdi.express/blog/webhooks-cfdi-express-api
- Landing API: https://cfdi.express/api
- Docs interactivas (Webhooks): https://api.cfdi.express/docs
- Dashboard (llaves `sk_test_` / `sk_live_`): https://dash.cfdi.express
- Post relacionado (lanzamiento de la API): https://cfdi.express/blog/lanzamiento-cfdi-express-api
- Demo: https://cal.com/team/acromatico-development/cfdi-express

---

## Cómo usar este pack

1. Genera la imagen de cada red con **Gemini** (gemini.google.com) pegando el prompt de la sección **Media**.
2. Publica con el **Texto** sugerido, adaptándolo a tu tono si hace falta.
3. **Imagen hero del blog**: usa el prompt de más abajo, súbela a `videos.acromatico.dev` y reemplaza el placeholder `PENDIENTE-webhooks-cfdi-express-api` del frontmatter. Mark/Diego hacen el reemplazo.
4. El CTA principal es **docs + dashboard** (developers). No empujes la app de Shopify ni webhooks de Shopify: este anuncio es de **webhooks de salida de la API**.

---

## Imagen hero del blog

**Prompt (Gemini):**

> Crea una imagen wide 16:9 (1600×900) estilo ilustración flat/moderna para un blog de developers sobre webhooks de facturación electrónica en México. Fondo teal oscuro #055D5E con una cuadrícula sutil. Al centro, un nodo de API (círculo blanco con «{ }») dispara tres flechas doradas #FFD700 hacia la derecha, cada una llegando a una tarjeta: factura con sello SAT, reloj de complemento de pago y documento de nómina. A la izquierda, un servidor con un check y el texto «POST firmado». Texto grande en tipografía Racing Sans One / Quicksand: «Webhooks CFDI — sin polling». Subtítulo pequeño: «CFDI Express API · CFDI-Signature». Estilo minimalista, profesional B2B, alto contraste, sin fotografías de stock, sin logos de otras marcas.

**Aspecto:** 16:9 · 1600×900 · subir a `videos.acromatico.dev` y copiar la URL al campo `image:` del blog.

---

## LinkedIn

### Texto

> ¿Tu backend sigue preguntando cada 10 segundos si el CFDI ya se timbró?
>
> Hoy lanzamos **webhooks de salida** en CFDI Express API. En lugar de hacer polling a `/v1/invoices`, tu URL recibe un POST firmado cuando cambia el estado:
>
> ✅ `invoice.stamped` / `invoice.stamp_failed` / `invoice.cancelled`
> ✅ Lo mismo en complementos de pago: `payment.*`
> ✅ Y en nómina: `nomina.*`
>
> El flujo es el de siempre, estilo que ya conoces si usaste Stripe:
> 1. `POST /v1/webhook_endpoints` con tu HTTPS
> 2. Guardas el `whsec_…` (se muestra una sola vez)
> 3. Verificas el header `CFDI-Signature` en cada entrega
> 4. Idempotencia con `CFDI-Delivery` / `event.id` — los reintentos existen y tu handler tiene que aguantarlos
>
> En test usas `sk_test_` (hasta `http://` si hace falta). En live, `sk_live_` y HTTPS. Hay bitácora de entregas, `retry` de fallidos y rotación de secreto con 24 h de overlap.
>
> Adiós al cron que consume rate limit. El SAT no espera a tu polling.
>
> 📘 Guía y endpoints: https://api.cfdi.express/docs
> 🔑 Llave de prueba: https://dash.cfdi.express
> 🧾 Contexto de la API: https://cfdi.express/api
>
> #CFDI #FacturacionElectronica #SAT #CFDIExpress #API #Webhooks #DesarrolloDeSoftware #FacturacionDigital

### Media

**Prompt (Gemini):**

> Crea una imagen horizontal 1200×627 (1.91:1) estilo ilustración corporativa B2B. Fondo teal oscuro #055D5E degradado a un azul noche. Al centro, un diagrama de tres nodos conectados por líneas doradas #FFD700: «API CFDI Express» → «POST /webhooks» → «Tu ERP». Debajo, tres pills blancas con el texto exacto: invoice.stamped · payment.stamped · nomina.stamped. Esquina superior: logo de CFDI Express (recibo). Texto principal en Quicksand/Racing Sans One: «Webhooks CFDI. Sin polling.» Estilo flat, alto contraste, sin gente, sin fotos de stock.

**Aspecto:** 1200×627 (horizontal, 1.91:1).

---

## X (Twitter)

### Texto (post principal)

> ¿Polling para saber si el CFDI ya se timbró? Ya no.
>
> CFDI Express API ahora manda webhooks firmados: invoice.stamped, pagos y nómina.
>
> Header: CFDI-Signature · secreto: whsec_…
>
> → https://api.cfdi.express/docs

### Hilo opcional (desarrolla el post)

1. El patrón malo: un cron que pega a GET /v1/invoices cada N segundos. Se come el rate limit y te enteras tarde.
2. El patrón bueno: POST /v1/webhook_endpoints + guardas el whsec_ (una sola vez) + verificas CFDI-Signature.
3. Eventos: invoice.stamped / stamp_failed / cancelled. Igual en payment.* (REP) y nomina.*.
4. Operación: bitácora de deliveries, retry de fallidos, rotación de secreto con 24 h de overlap. Test con sk_test_, live con sk_live_.
5. Guía: https://api.cfdi.express/docs · llaves: https://dash.cfdi.express · landing: https://cfdi.express/api

### Media

**Prompt (Gemini):**

> Crea una imagen horizontal 1600×900 (16:9) impactante para X. Fondo teal oscuro #055D5E. Izquierda: un icono de reloj tachado (anti-polling) en gris #D7D7D7. Derecha: un rayo dorado #FFD700 que conecta un nodo «{ }» con una factura blanca sellada. Texto grande en Racing Sans One: «Webhooks CFDI». Texto menor en Quicksand: «invoice.stamped → tu backend». Estilo flat vector, alto contraste, minimalista, sin fotografías.

**Aspecto:** 1600×900 (16:9).

---

## Facebook

### Texto

> 📢 La API de CFDI Express ya puede **avisarle a tu sistema** cuando una factura se timbra, falla o se cancela — sin que tengas que preguntar cada rato.
>
> Son **webhooks firmados**: tu URL recibe un POST, verificas el header CFDI-Signature con el secreto `whsec_` y actualizas tu ERP. También cubre complementos de pago y nómina.
>
> Si ya usas la API (o la estás evaluando), este es el siguiente paso: menos polling, menos huecos, misma cuenta de siempre.
>
> Docs: https://api.cfdi.express/docs
> Dashboard: https://dash.cfdi.express
> Landing: https://cfdi.express/api
>
> #CFDI #FacturacionElectronica #SAT #CFDIExpress

### Media

**Prompt (Gemini):**

> Crea una imagen horizontal 1200×630 (1.91:1) para Facebook. Fondo teal oscuro #055D5E con nodos y llaves de código «{ }» dispersos en gris #D7D7D7. Al centro, una factura blanca con sello dorado #FFD700 y una notificación tipo campana que dice «invoice.stamped». Arriba el texto: «Tu sistema se entera al instante». Abajo: «Webhooks · CFDI Express API». Tipografía Racing Sans One / Quicksand. Estilo flat, profesional, sin fotos.

**Aspecto:** 1200×630 (1.91:1).

---

## Instagram

### Texto (feed)

> 🧾⚡ ¿Tu sistema sigue preguntando si el CFDI ya se timbró?
>
> Lanzamos webhooks en CFDI Express API:
>
> ✅ invoice.stamped / cancelado / falló
> ✅ Complementos de pago (payment.*)
> ✅ Nómina (nomina.*)
>
> Tú pones la URL, guardas el `whsec_` y verificas **CFDI-Signature**. Adiós al polling.
>
> 👉 Docs y cuenta de prueba: link en bio (api.cfdi.express/docs · dash.cfdi.express)
>
> #CFDI #FacturacionElectronica #SAT #CFDIExpress #API #Webhooks #FacturacionDigital #Mexico #Developer #Fintech #CFDI40 #FacturaElectronica #ERP #Nómina #ComplementoDePago #SATMexico #Software #Integraciones #DesarrolloDeSoftware #EmprendedorTech

### Media (feed cuadrado)

**Prompt (Gemini):**

> Crea una imagen cuadrada 1080×1080 estilo ilustración flat para Instagram. Fondo teal oscuro #055D5E. Al centro un círculo blanco con un icono de nodo/API «{ }» y tres flechas doradas #FFD700 que salen hacia iconos de factura, reloj de pago y documento. Texto grande en blanco, Racing Sans One: «WEBHOOKS CFDI». Texto menor en Quicksand: «Sin polling · CFDI Express API». Alto contraste, minimalista, profesional, sin fotografías de stock.

**Aspecto:** 1080×1080 (1:1) para feed.

### Media (historia opcional)

**Prompt (Gemini):**

> Crea una imagen vertical 1080×1920 para historia de Instagram. Fondo teal oscuro #055D5E con degradado radial dorado #FFD700 suave detrás del centro. Arriba: logo CFDI Express. Centro: una factura blanca que “empuja” una notificación «invoice.stamped» hacia un celular/ERP estilizado. Texto grande: «Deja de hacer polling». Texto menor: «Webhooks firmados · CFDI-Signature». Deja tercios superior e inferior limpios para la UI de Instagram. Estilo flat, sin fotos.

**Aspecto:** 1080×1920 (historia).

---

## Checklist de publicación

- [ ] Generar imágenes con Gemini y revisar ortografía del texto en la imagen (`invoice.stamped`, `CFDI-Signature`, sin acentos rotos).
- [ ] Esperar el merge del post antes de pegar https://cfdi.express/blog/webhooks-cfdi-express-api (si aún no está live, usa https://cfdi.express/api y https://api.cfdi.express/docs).
- [ ] Verificar enlaces: docs, dash, landing API. No enlazar webhooks de Shopify ni `app/routes/webhooks.tsx`.
- [ ] Hero del blog: Mark/Diego reemplazan `PENDIENTE-webhooks-cfdi-express-api` en `videos.acromatico.dev`.
- [ ] Programar: LinkedIn y Facebook primero (mañana), X al mediodía, Instagram por la tarde/noche.
- [ ] Responder los primeros comentarios el día del anuncio (sobre todo dudas de firma y de `sk_test_` vs `sk_live_`).
