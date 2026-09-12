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

1. Genera la imagen de cada red con **Gemini** (gemini.google.com) pegando el prompt de la sección **Media**. Dirección visual: **personaje humano/Pixar** (integrador con hoodie teal), no solo diagramas abstractos.
2. Publica con el **Texto** sugerido, adaptándolo a tu tono si hace falta.
3. **Imagen hero del blog**: usa el prompt de más abajo. Diego genera el asset, se sube a `videos.acromatico.dev` y **Mark reemplaza** el placeholder `PENDIENTE-…` del frontmatter.
4. El CTA principal es **docs + dashboard**. Este anuncio es de la **API pública de nómina**, no de la app de Shopify.

---

## Imagen hero del blog

**Prompt (Gemini):**

> Ilustración estilo Pixar / personaje 3D amigable (no foto real, no stock) para el hero de un blog de developers y RH en México. Personaje: integrador mexicano de unos 30 años, piel morena clara, cabello corto oscuro, hoodie oversized color teal oscuro #055D5E con un pequeño recibo bordado en el pecho, audífonos colgando del cuello. Está de tres cuartos, sonrisa confiada, sosteniendo un recibo de nómina blanco tamaño carta con sello circular dorado #FFD700 que dice «SAT ✓» y un renglón «Nómina 1.2». A su lado, una terminal flotante de vidrio con el texto exacto «POST /v1/nominas → 201». Fondo estudio suave en teal #055D5E con cuadrícula muy sutil y acentos dorados. Texto grande en tipografía Racing Sans One / Quicksand, blanco, ortografía correcta: «Facturas de nómina CFDI — desde la API». Subtítulo pequeño: «CFDI Express · complemento Nómina 1.2». Iluminación cinematográfica, alto contraste, sin logos de otras marcas, sin texto extra inventado.

**Aspecto:** 16:9 · 1600×900 · subir a `videos.acromatico.dev` y que Mark copie la URL al campo `image:` del blog (reemplaza `PENDIENTE-facturas-de-nomina-cfdi-express-api`).

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

**Prompt (Gemini):**

> Imagen horizontal 1200×627 (1.91:1), ilustración 3D estilo Pixar/humano para LinkedIn B2B. Misma dirección de personaje: integrador con hoodie teal #055D5E, de pie frente a un escritorio limpio. En la mesa, un recibo de nómina blanco con sello dorado #FFD700 «Nómina 1.2» y un laptop que muestra «POST /v1/nominas». Fondo degradado teal #055D5E a azul noche, acentos dorados. Texto principal en Racing Sans One / Quicksand, blanco, sin errores: «Nómina CFDI. Tú las partidas. La API los totales.» Esquina: wordmark «CFDI Express». Estilo cinematográfico, profesional, sin fotos de stock, sin gente fotorealista.

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

**Prompt (Gemini):**

> Imagen horizontal 1600×900 (16:9) para X, estilo Pixar/humano. Close-up del integrador con hoodie teal #055D5E guiñando un ojo, levantando un recibo de nómina blanco con sello dorado #FFD700 «SAT ✓». Detrás, partículas de código «{ }» y la línea «POST /v1/nominas». Fondo teal oscuro #055D5E, alto contraste. Texto grande Racing Sans One: «Nómina CFDI por API». Texto menor Quicksand: «Complemento 1.2 · totales en servidor». Sin fotos, sin marcas ajenas, ortografía perfecta.

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

**Prompt (Gemini):**

> Imagen horizontal 1200×630 (1.91:1) para Facebook. Ilustración 3D estilo Pixar: el integrador de hoodie teal #055D5E entrega un recibo de nómina a una compañera de RH (personaje también Pixar, blazer blanco, tablet en la mano). Entre ellos flota un sello dorado #FFD700 «Nómina 1.2». Fondo oficina suave teñida de teal #055D5E. Texto arriba en Racing Sans One / Quicksand: «Recibos de nómina desde tu sistema». Abajo: «CFDI Express API». Cálido, profesional, sin fotos reales.

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

> Imagen cuadrada 1080×1080 estilo Pixar/humano para feed. El integrador con hoodie teal #055D5E al centro, sosteniendo un recibo blanco con QR teal y sello dorado #FFD700. Alrededor, tres pills: «percepciones», «deducciones», «totales SAT». Fondo teal oscuro #055D5E. Texto grande blanco, Racing Sans One: «NÓMINA CFDI». Texto menor Quicksand: «API · complemento 1.2». Alto contraste, personaje nítido, sin fotos de stock, sin hashtags pintados en la imagen.

**Aspecto:** 1080×1080 (1:1) para feed.

### Media (historia opcional)

**Prompt (Gemini):**

> Imagen vertical 1080×1920 para historia de Instagram, estilo Pixar. Fondo teal #055D5E con halo dorado #FFD700 detrás de la cabeza del personaje. Arriba: wordmark CFDI Express. Centro: integrador de hoodie teal mostrando el recibo «Nómina 1.2» a cámara, gesto de “listo”. Texto grande: «Timbra nómina desde tu API». Texto menor: «POST /v1/nominas · $1 MXN/timbre». Deja el tercio superior e inferior limpios para la UI de Instagram. Sin fotos reales.

**Aspecto:** 1080×1920 (historia).

---

## Checklist de publicación

- [ ] Generar imágenes con Gemini (dirección **humano/Pixar**, hoodie teal). Diego sube el hero; Mark reemplaza `PENDIENTE-facturas-de-nomina-cfdi-express-api` en el frontmatter.
- [ ] Revisar que el texto dentro de cada imagen no tenga errores (Nómina, CFDI, SAT, `/v1/nominas`).
- [ ] Esperar el merge del post antes de pegar https://cfdi.express/blog/facturas-de-nomina-cfdi-express-api (si aún no está live, usa docs + dash + landing).
- [ ] Verificar enlaces: docs, dash, landing API, post de lanzamiento, post de webhooks.
- [ ] No enlazar la app de Shopify ni hablar de planes USD de la tienda: este pack es **API de nómina**.
- [ ] Programar: LinkedIn y Facebook primero (mañana), X al mediodía, Instagram por la tarde/noche.
- [ ] Responder comentarios de integradores (totales SAT, registro patronal, 201 vs 202, `Idempotency-Key`).
