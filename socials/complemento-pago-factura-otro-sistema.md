# Social Pack | Cómo emitir el complemento de pago de una factura timbrada en otro sistema

Corresponde al blog post: [`pages/complemento-pago-factura-otro-sistema.md`](../pages/complemento-pago-factura-otro-sistema.md)

Recursos oficiales para enlazar:

- Post (tras el merge; **no publicar todavía**): https://cfdi.express/blog/complemento-pago-factura-otro-sistema
- Dashboard (llaves `sk_test_` / `sk_live_`): https://dash.cfdi.express
- Docs interactivas: https://api.cfdi.express/docs
- Demo: https://cfdi.express
- Contacto: hola@cfdi.express

---

## Cómo usar este pack

1. **No regeneres las imágenes.** Diego ya las entregó. Usa solo las dos URLs de abajo, sin query y sin inventar otras.
2. Publica con el **Texto** sugerido. El anuncio es de la **API pública** (`https://api.cfdi.express`), versión 1.59.0. No es la app de Shopify ni el dashboard: por ahora no hay carga de XML ahí.
3. El `image` del frontmatter es el **OG 1200×630**. El hero 1600×900 va en el cuerpo del post, debajo del H1. El template no pinta el `image` del frontmatter dentro del artículo.
4. El CTA principal es **docs + dashboard**. La guía de importación de Pablo aún no está en cfdi.express; no escribas una página de docs. Enlaza https://api.cfdi.express/docs.
5. **No mergear ni publicar** hasta que Rafael lo revise. En este repo el merge publica en vivo.

---

## Imagen hero del blog

**Ya entregada (usar estas, no generar otras):**

| Uso | Medida | URL (sin query) |
| --- | --- | --- |
| OG / `image` del frontmatter | 1200×630 | https://videos.acromatico.dev/api/images/assets/d9faa378-3e95-4068-80ab-4609bfb117b8.png |
| Hero del artículo (debajo del H1) | 1600×900 | https://videos.acromatico.dev/api/images/assets/5a28303c-24bf-4f7e-b34c-9a57a3d2e88a.png |

Alt del hero: «Personaje 3D con hoodie teal entre facturas genéricas que entran y documentos REP con sello dorado que salen, con el logo de CFDI Express.»

**Prompt Gemini (solo si hubiera que rehacer el hero; no sustituye el asset entregado):**

> Crea una imagen wide 16:9 (1600×900), ilustración 3D. Un personaje con hoodie teal #055D5E está al centro. A la izquierda entran facturas genéricas (sin marcas de otros sistemas). A la derecha salen documentos REP con sello dorado #FFD700. Incluye el logo de CFDI Express. Fondo teal oscuro #055D5E, acentos dorados, blanco y gris #D7D7D7. Tipografía Racing Sans One en el título y Quicksand en el texto menor. Texto exacto: «Complemento de pago» / «Facturas de otro sistema». Estilo profesional B2B, alto contraste, sin fotografías de stock, sin logos de terceros, sin nombres de otros sistemas de facturación.

**Aspecto:** 16:9 · 1600×900.

---

## LinkedIn

### Texto

> Cambiaste de sistema de facturación y te quedaron facturas PPD con complementos de pago pendientes.
>
> Desde la versión 1.59.0 de CFDI Express API puedes traer esos CFDI 4.0 de ingreso e emitir el REP desde tu backend, o pedírselo a tu agente.
>
> Qué entra: facturas de ingreso CFDI 4.0, PUE o PPD, timbradas en otro sistema, con el RFC emisor de uno de tus merchants. La API revisa el XML, los sellos y que el SAT las reporte como Vigentes.
>
> `POST /v1/invoices/import` acepta de 1 a 20 XML en base64. En cada PPD, `previousPayments` es obligatorio: `false` si no había REP, o el saldo y la última parcialidad si ya los habías emitido.
>
> El REP sale por `POST /v1/payments` (o `preview` antes, sin timbrar ni cobrar). Puedes adjuntar el XML o usar el `invoiceId` ya importado (`inv_…`).
>
> Precio de la API: $1 MXN por timbre, sin suscripción. Cada XML importado cuesta el 20 % del precio por timbre de tu cuenta. No cuenta como timbre. Si el XML va adjunto al REP, la importación no se cobra: solo el timbre del complemento.
>
> Por MCP: `import_invoices` (XML en texto, hasta 5) y `create_payment` / `preview_payment`.
>
> 📘 Referencia: https://api.cfdi.express/docs
> 🔑 Prueba con `sk_test_`: https://dash.cfdi.express
>
> #CFDI #FacturacionElectronica #SAT #CFDIExpress #API #ComplementoDePago #REP #PPD #DesarrolloDeSoftware

### Media

**Usar:** recorte OG 1200×630
https://videos.acromatico.dev/api/images/assets/d9faa378-3e95-4068-80ab-4609bfb117b8.png

**Prompt Gemini (variante, solo si se necesita otra toma):**

> Crea una imagen horizontal 1200×627 (1.91:1), ilustración 3D. Personaje con hoodie teal #055D5E entre facturas genéricas que entran y documentos REP con sello dorado #FFD700 que salen. Logo de CFDI Express. Texto exacto en Racing Sans One: «Complemento de pago». Subtítulo en Quicksand: «De una factura timbrada en otro sistema». Fondo #055D5E, alto contraste, sin fotos de stock, sin logos de terceros, sin nombres de otros facturadores.

**Aspecto:** 1200×627 (horizontal, 1.91:1).

---

## X (Twitter)

### Texto (post principal)

> ¿Te quedaron REP pendientes de facturas PPD timbradas en otro sistema?
>
> CFDI Express API 1.59.0 importa el CFDI 4.0 y emite el complemento.
>
> POST /v1/invoices/import · POST /v1/payments
>
> → https://api.cfdi.express/docs

### Hilo opcional

1. Solo facturas de ingreso CFDI 4.0, PUE o PPD. El RFC emisor tiene que ser un merchant de tu cuenta, en `sk_test_` o `sk_live_`. No entran CFDI 3.3, notas de crédito, nómina, traslado ni retenciones.
2. Antes de aceptar el XML, la API revisa que esté bien formado, que sea ingreso con TimbreFiscalDigital, los sellos y que el SAT lo reporte Vigente. Esa consulta se repite antes de cada REP.
3. `POST /v1/invoices/import`: 1 a 20 XML en base64. `Idempotency-Key` opcional. En PPD, `previousPayments` es obligatorio (`false`, o saldo + última parcialidad). En PUE no se permite.
4. Cada XML vuelve `imported`, `already_imported` o `rejected`. Rechazados y duplicados no se cobran. Si no hay saldo o llegaste al tope mensual: 402 y no se importa nada.
5. El REP va en `POST /v1/payments`. En `documents[]`, exactamente uno: `xml` o `invoiceId`. Con el XML adjunto, la importación no se cobra. Con `inv_…`, el saldo y la parcialidad siguen lo guardado.
6. Precio: $1 MXN por timbre, sin suscripción. Importar cuesta el 20 % del precio por timbre de tu cuenta. MCP: `import_invoices`, `create_payment`, `preview_payment`. Docs: https://api.cfdi.express/docs · llaves: https://dash.cfdi.express

### Media

**Usar:** hero 1600×900
https://videos.acromatico.dev/api/images/assets/5a28303c-24bf-4f7e-b34c-9a57a3d2e88a.png

**Prompt Gemini (variante):**

> Crea una imagen horizontal 1600×900 (16:9). Ilustración 3D: personaje con hoodie teal #055D5E, facturas genéricas entrando a la izquierda y documentos REP con sello dorado #FFD700 saliendo a la derecha. Logo de CFDI Express. Texto grande en Racing Sans One: «Emite el REP». Texto menor en Quicksand: «CFDI 4.0 importado». Fondo #055D5E, alto contraste, sin fotografías, sin logos de terceros.

**Aspecto:** 1600×900 (16:9).

---

## Facebook

### Texto

> Si cambiaste de sistema y te quedaron facturas PPD sin complemento de pago, ya puedes traerlas a CFDI Express y emitir el REP.
>
> Es la API (versión 1.59.0), no una carga de XML en el dashboard. Importas el CFDI 4.0 de ingreso y timbras el complemento con `POST /v1/payments`, o se lo pides a tu agente.
>
> $1 MXN por timbre, sin suscripción. Importar un XML cuesta el 20 % del precio por timbre de tu cuenta.
>
> Prueba con una llave `sk_test_` en https://dash.cfdi.express
> Referencia: https://api.cfdi.express/docs
>
> #CFDI #FacturacionElectronica #SAT #CFDIExpress #ComplementoDePago #REP

### Media

**Usar:** recorte OG 1200×630
https://videos.acromatico.dev/api/images/assets/d9faa378-3e95-4068-80ab-4609bfb117b8.png

**Prompt Gemini (variante, solo si se necesita otra toma):**

> Imagen 1200×630 (1.91:1), ilustración 3D. Personaje con hoodie teal #055D5E entre facturas que entran y REP con sello dorado #FFD700 que salen. Logo de CFDI Express. Texto exacto: «Complemento de pago» / «Facturas de otro sistema». Fondo #055D5E. Sin fotos reales, sin logos de terceros.

**Aspecto:** 1200×630 (1.91:1).

---

## Instagram

### Texto (feed)

> Trae tus facturas de antes. Emite tus REP de hoy.
>
> CFDI Express API 1.59.0 importa un CFDI 4.0 de ingreso timbrado en otro sistema y emite el complemento de pago.
>
> PUE o PPD. El RFC emisor tiene que ser tuyo.
> En PPD declaras si ya había parcialidades.
> Por API o con tu agente (MCP).
>
> $1 MXN por timbre, sin suscripción.
> Importar: 20 % del precio por timbre de tu cuenta.
>
> Prueba en sandbox → dash.cfdi.express
> Docs → api.cfdi.express/docs
>
> #CFDI #CFDI40 #FacturacionElectronica #SAT #CFDIExpress #ComplementoDePago #REP #PPD #FacturaElectronica #API #MCP #Mexico #PyME #DesarrolloDeSoftware #ERP #Integraciones #SATMexico #Timbrado #FacturaPPD #AgenteIA #ImportarXML #SaldoInsoluto #Parcialidad #CFDIExpressAPI

### Media (feed)

No hay recorte 1080×1080. **Usar el OG 1200×630:**
https://videos.acromatico.dev/api/images/assets/d9faa378-3e95-4068-80ab-4609bfb117b8.png

**Prompt Gemini (variante, solo si se necesita un 1:1):**

> Imagen cuadrada 1080×1080, ilustración 3D. Personaje con hoodie teal #055D5E al centro, facturas genéricas a un lado y documentos REP con sello dorado #FFD700 al otro. Logo de CFDI Express. Fondo #055D5E. Texto grande en Racing Sans One, exacto: «Complemento de pago». Texto menor en Quicksand: «Facturas de otro sistema». Alto contraste, sin fotos de stock, sin logos de terceros, sin hashtags pintados en la imagen.

**Aspecto:** 1080×1080 (1:1).

### Media (historia opcional)

**Usar el OG** y enmarcar en 1080×1920 con fondo #055D5E, o generar:

> Historia vertical 1080×1920, ilustración 3D. Fondo teal #055D5E. Centro: personaje con hoodie teal, facturas genéricas que entran y REP con sello dorado #FFD700 que salen. Logo de CFDI Express. Texto grande exacto: «Emite el REP». Texto menor: «CFDI 4.0 importado». CTA: «Lee la guía». Deja el tercio superior e inferior limpios para la UI de Instagram. Sin fotos reales, sin logos ajenos.

**Aspecto:** 1080×1920 (historia).

---

## Checklist de publicación

- [x] Diego entregó el OG 1200×630 y el hero 1600×900. Frontmatter `image` = OG, sin query. El hero va en el cuerpo, debajo del H1, sin query. No inventar otras URLs.
- [ ] Rafael revisa el post antes del merge. **No mergear y no publicar:** en este repo el merge publica en vivo.
- [ ] No pegar https://cfdi.express/blog/complemento-pago-factura-otro-sistema hasta que el post esté en vivo. Mientras tanto, CTA a https://api.cfdi.express/docs y https://dash.cfdi.express.
- [ ] No escribir una página de docs. Pablo es dueño de la guía oficial; su doc de importación aún no está en cfdi.express.
- [ ] CTA principal = docs + dashboard. La app de Shopify y el dashboard no cargan XML en esta versión.
- [ ] Cifras alineadas al post: API v1.59.0; $1 MXN por timbre, sin suscripción; importación = 20 % del precio por timbre de la cuenta; 1–20 XML en `POST /v1/invoices/import`; MCP máximo 5; 402 si no hay saldo o se llegó al tope mensual.
- [ ] Sin nombres de otros sistemas de facturación ni de clientes.
- [ ] Sin endpoints, campos ni precios que no estén en el post.
- [ ] Ortografía del arte: el alt del hero describe al personaje, las facturas, los REP y el logo. No agregar texto distinto en un asset nuevo salvo que se regenere a propósito.
- [ ] LinkedIn y Facebook primero; X al mediodía; Instagram por la tarde.
- [ ] Responder comentarios el día de publicación (sobre todo “¿qué XML acepta?” y “¿cuánto cuesta importar?”).
