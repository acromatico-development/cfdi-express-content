# Social Pack | Nota de crédito por reembolso en Shopify (CFDI 4.0)

Corresponde al blog post: [`pages/nota-de-credito-reembolso-shopify.md`](../pages/nota-de-credito-reembolso-shopify.md)

Recursos oficiales para enlazar:

- Post (tras el merge): https://cfdi.express/blog/nota-de-credito-reembolso-shopify
- Docs: https://cfdi.express/docs/notas-de-credito
- Shopify Flow: https://cfdi.express/docs/uso-shopify-flow-automatizaciones-cfdi-express
- Pilar: https://cfdi.express/blog/facturar-pedidos-shopify-automaticamente
- De público en general a factura del cliente: https://cfdi.express/blog/factura-cliente-despues-de-publico-en-general-shopify
- App de Shopify: https://apps.shopify.com/cfdi-express
- Landing: https://cfdi.express
- Demo: https://cal.com/team/acromatico-development/cfdi-express

---

## Cómo usar este pack

1. **Usa el arte publicado.** No lo regeneres. El `image` del frontmatter es el hero 1600×900, sin query (la plantilla también lo usa como `og:image`). En este pack, LinkedIn, X, Facebook e Instagram usan el OG 1200×630, sin query. En el cuerpo del post el hero lleva `?w=1600`.
2. Publica con el **Texto** sugerido. El CTA es instalar la app.
3. La nota es un CFDI de egreso ligado a la original. No digas que cancela la factura.
4. En Flow, la acción documentada es **Crear Nota de Crédito** (total o parcial). No digas que el monto, la forma de pago y el concepto se llenan solos, ni que la acción devuelve folio, UUID y liga, ni que cada reembolso manda el correo por sí solo. El correo documentado es el del formulario, si capturas el correo del receptor.
5. La app no acredita más de lo que queda disponible.
6. No publiques el link del blog hasta que el post esté en línea. **No mergear hasta que Rafael revise.**

---

## Imagen hero del blog

**Publicada. Usar estas URLs, sin query.** Slug: `nota-de-credito-reembolso-shopify`.

| Uso | Medida | URL (sin query) |
| --- | --- | --- |
| Hero / `image` del frontmatter (la plantilla lo usa también como `og:image`) | 1600×900 | https://videos.acromatico.dev/api/images/assets/dd6881bb-99fa-4f31-a129-64c9bd0fc5b0.jpg |
| OG / redes de este pack | 1200×630 | https://videos.acromatico.dev/api/images/assets/2f2cc977-1b64-467d-a099-ef42bb481634.jpg |

No hay un campo OG aparte en el frontmatter. El cuerpo del post usa el hero con `?w=1600`.

Alt del hero: «Hero del blog: Nota de crédito por reembolso en Shopify (CFDI 4.0), reembolso ligado a la factura original».

Alt del OG: «OG del blog: Nota de crédito por reembolso en Shopify (CFDI 4.0)».

Copy en el arte, ortografía exacta:

- Título: «Nota de crédito»
- Subtítulo: «Reembolso en Shopify»

**Prompt Gemini — hero 16:9 (1600×900):**

> Ilustración flat y moderna, profesional B2B, alto contraste, sin fotografías de stock y sin logos de otras marcas. Fondo teal oscuro #055D5E. Una factura blanca queda vigente, con sello dorado #FFD700, y a su lado una nota de crédito más pequeña con una flecha de devolución en dorado. Un candado abierto en gris #D7D7D7 indica que la original no se cancela. Tipografía Racing Sans One en el título y Quicksand en el subtítulo. Título exacto, en blanco: «Nota de crédito». Subtítulo exacto, en dorado #FFD700: «Reembolso en Shopify». Aspecto 16:9, 1600×900.

**Aspecto:** 16:9 · 1600×900.

---

## LinkedIn

### Texto

Un reembolso en Shopify no se resuelve cancelando la factura.

La nota de crédito es un CFDI de egreso ligado a la original. En CFDI Express la timbras desde la orden:

→ Total o parcial. El campo muestra lo que todavía se puede acreditar.
→ Si el monto se pasa, la app no timbra.
→ Si capturas el correo del receptor, la nota se envía por correo.
→ En Shopify Flow, la acción Crear Nota de Crédito emite la nota del pedido, por el total o por una parte.

La factura original sigue vigente ante el SAT.

Guía: https://cfdi.express/blog/nota-de-credito-reembolso-shopify
Instala la app: https://apps.shopify.com/cfdi-express

#CFDI #CFDI40 #FacturacionElectronica #SAT #Shopify #CFDIExpress #NotaDeCredito

### Media

**Usar (OG, sin query):** https://videos.acromatico.dev/api/images/assets/2f2cc977-1b64-467d-a099-ef42bb481634.jpg

**Prompt Gemini (1200×627):**

> Imagen horizontal 1200×627, ilustración flat B2B. Fondo #055D5E. Factura vigente con sello dorado #FFD700 y una nota de crédito al lado, unidas por una línea. Título en Racing Sans One, blanco, exacto: «Nota de crédito». Subtítulo en Quicksand, dorado: «Reembolso en Shopify». Sin fotos, sin logos ajenos. Aire para el recorte de LinkedIn.

**Aspecto:** 1200×627 (1.91:1).

---

## X (Twitter)

### Texto

Reembolso en Shopify = nota de crédito, no cancelar la factura.

El CFDI de egreso queda ligado a la original. Total o parcial. La app no acredita más de lo que queda.

Con Flow, la acción Crear Nota de Crédito timbra la nota del pedido.

Guía: https://cfdi.express/blog/nota-de-credito-reembolso-shopify
App: https://apps.shopify.com/cfdi-express

### Media

**Usar (OG, sin query):** https://videos.acromatico.dev/api/images/assets/2f2cc977-1b64-467d-a099-ef42bb481634.jpg

**Prompt Gemini (1600×900):**

> Imagen 1600×900, ilustración flat. Fondo #055D5E. Dos documentos: factura y nota de crédito, unidos por una flecha dorada #FFD700. Título en Racing Sans One, blanco, exacto: «Nota de crédito». Subtítulo en Quicksand: «Reembolso en Shopify». Sin fotos, sin logos ajenos.

**Aspecto:** 16:9 · 1600×900.

---

## Facebook

### Texto

Si reembolsas un pedido que ya tenía factura, el comprobante que corresponde es la nota de crédito.

Queda ligada a la factura original. Puede ser por el total o por una parte, y la app no deja acreditar de más. La original sigue vigente.

En Shopify Flow puedes agregar la acción Crear Nota de Crédito para timbrar esa nota desde un workflow.

Instala CFDI Express: https://apps.shopify.com/cfdi-express

### Media

**Usar (OG, sin query):** https://videos.acromatico.dev/api/images/assets/2f2cc977-1b64-467d-a099-ef42bb481634.jpg

**Prompt Gemini (1200×630):**

> Imagen 1200×630 para Facebook. Fondo teal #055D5E. Factura blanca y nota de crédito con flecha dorada #FFD700. Título exacto en Racing Sans One: «Nota de crédito». Subtítulo en Quicksand: «Reembolso en Shopify». Ilustración flat, sin fotos, sin logos de terceros.

**Aspecto:** 1200×630 (1.91:1).

---

## Instagram

### Texto

La nota de crédito del reembolso queda ligada a la factura original.

Total o parcial. Sin acreditar de más. La original sigue vigente ante el SAT.

Instala la app desde el enlace de la bio.

#CFDI #CFDI40 #FacturacionElectronica #SAT #Shopify #CFDIExpress #NotaDeCredito #Reembolso #Mexico #Ecommerce

### Media

**Usar (OG, sin query):** https://videos.acromatico.dev/api/images/assets/2f2cc977-1b64-467d-a099-ef42bb481634.jpg

**Prompt Gemini — feed 1080×1080:**

> Cuadrado 1080×1080, ilustración flat, fondo #055D5E. Factura con sello dorado #FFD700 y una nota más pequeña. Título en Racing Sans One, blanco, exacto: «Nota de crédito». Subtítulo en Quicksand: «Reembolso en Shopify». Sin fotos, sin logos ajenos, sin hashtags en la imagen.

**Prompt Gemini — historia 1080×1920:**

> Historia 1080×1920. Fondo #055D5E. Centro: factura y nota de crédito. Texto grande exacto: «Nota de crédito». Texto menor: «Reembolso en Shopify». CTA: «Instala la app». Tercios superior e inferior limpios. Sin fotos, sin logos ajenos.

**Aspecto:** feed 1080×1080. Historia 1080×1920.

---

## Checklist de publicación

- [x] Hero 1600×900 en el frontmatter `image`, sin query. OG 1200×630 en este pack, sin query. El cuerpo del post usa el hero con `?w=1600`.
- [ ] No decir que la nota cancela la factura.
- [ ] No decir que Flow rellena monto, forma de pago y concepto, ni que devuelve folio, UUID y liga, ni que manda el correo en cada reembolso.
- [ ] Sí decir que no acredita más del disponible.
- [ ] CTA a https://apps.shopify.com/cfdi-express
- [ ] Sin nombres de clientes ni de otras apps de facturación.
