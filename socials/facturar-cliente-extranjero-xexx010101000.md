# Social Pack | Cómo facturar a un cliente extranjero en CFDI 4.0 (XEXX010101000)

Corresponde al blog post: [`pages/facturar-cliente-extranjero-xexx010101000.md`](../pages/facturar-cliente-extranjero-xexx010101000.md)

Recursos oficiales para enlazar:

- Post (solo cuando esté publicado): https://cfdi.express/blog/facturar-cliente-extranjero-xexx010101000
- Docs, sección de receptor: https://cfdi.express/docs/facturacion-al-extranjero#receptor-extranjero
- Docs de la app: https://cfdi.express/docs/facturacion-al-extranjero
- Notas de crédito: https://cfdi.express/docs/notas-de-credito
- Complementos de pago (REP): https://cfdi.express/docs/complementos-de-pago
- Cancelación y acuse: https://cfdi.express/docs/cancelacion-y-acuse
- API: https://cfdi.express/api
- Docs de la API: https://api.cfdi.express/docs
- MCP: https://api.cfdi.express/mcp
- Dashboard: https://dash.cfdi.express
- App de Shopify: https://apps.shopify.com/cfdi-express
- Demo: https://cal.com/team/acromatico-development/cfdi-express

---

## Cómo usar este pack

1. El hero del post **reutiliza** `https://assets.acromatico.dev/marketing/diego/blog/hero-cfdi-facturas-4.0.png` (el de CFDI facturas 4.0). Sirve para Open Graph. No inventes otra URL hasta tener el archivo subido.
2. Para redes, genera la pieza con los prompts de abajo. Texto exacto en la imagen: «Factura a un extranjero» y «XEXX010101000».
3. Hechos que sí puedes decir: RFC `XEXX010101000`; `ResidenciaFiscal` (clave `c_Pais`, 3 letras, se pasa a mayúsculas, nunca `MEX`); `NumRegIdTrib` (tax ID, 1 a 40 caracteres); uso `S01`, régimen `616` y CP del emisor; se emiten en factura, nota de crédito (copiados de la factura relacionada) y REP; se imprimen en el PDF de la factura y del REP.
4. En la app de Shopify cita solo el botón **Facturar al Público en General Extranjero** y el IVA 0% por línea, como está en la guía. No describas pantallas de residencia fiscal o tax ID.
5. No nombres proveedores de timbrado. No des asesoría fiscal: cierra con «confírmalo con tu contador» si hablas de IVA.
6. **No publiques** el link del blog hasta que el post esté en `https://cfdi.express/blog/facturar-cliente-extranjero-xexx010101000`. Este PR no se mergea ni se publica solo.

## Imagen hero del blog

**En el frontmatter hoy** (hero reutilizado, no es una pieza nueva):

https://assets.acromatico.dev/marketing/diego/blog/hero-cfdi-facturas-4.0.png

**Prompt Gemini — pieza propia 1600×900, por si se sustituye el hero:**

> Ilustración flat 1600×900, fondo teal oscuro #055D5E. Una factura blanca con sello y, a la derecha, una ficha de receptor con tres líneas legibles: RFC XEXX010101000, Residencia fiscal ARG, Tax ID. Un pin de mapa dorado #FFD700 marca un país fuera de México, sin banderas reales. Texto grande en Racing Sans One, blanco, ortografía exacta: «Factura a un extranjero». Subtítulo en Quicksand, dorado: «XEXX010101000». Gris #D7D7D7 en detalles. Sin fotografías, sin logos del SAT, sin logos de otras marcas.

**Aspecto:** 16:9 · 1600×900.

## LinkedIn

### Texto

Facturar a un cliente fuera de México en CFDI 4.0 usa el RFC genérico XEXX010101000. No es el de público en general (ese es XAXX010101000).

Al receptor le pides el nombre, la clave de país (catálogo c_Pais, 3 letras, nunca MEX) y su tax ID. En el comprobante van como ResidenciaFiscal y NumRegIdTrib.

La API también pide uso S01, régimen 616 y el código postal del emisor, igual que con el RFC de público en general. Esos atributos salen en la factura, en la nota de crédito y en el REP, y se imprimen en el PDF de la factura y del REP.

El ejemplo JSON está en la guía: https://cfdi.express/docs/facturacion-al-extranjero#receptor-extranjero

El artículo: https://cfdi.express/blog/facturar-cliente-extranjero-xexx010101000

El IVA de tu operación, confírmalo con tu contador.

#CFDI #FacturacionElectronica #SAT #CFDIExpress #XEXX010101000

### Media

Pieza 1200×627 (1.91:1):

> Imagen 1200×627, ilustración flat B2B. Fondo #055D5E. Factura blanca y ficha de receptor con «XEXX010101000», «Residencia fiscal» y «Tax ID». Pin dorado #FFD700. Título en Racing Sans One, blanco: «Factura a un extranjero». Subtítulo en Quicksand: «S01 · 616 · CP del emisor». Sin fotos, sin logos ajenos. Alto contraste.

## X (Twitter)

### Texto

Cliente extranjero en CFDI 4.0:

RFC XEXX010101000
ResidenciaFiscal (3 letras, nunca MEX)
NumRegIdTrib (su tax ID)
Uso S01, régimen 616, CP del emisor

Guía: https://cfdi.express/blog/facturar-cliente-extranjero-xexx010101000

#CFDI #SAT #CFDIExpress

### Media

La pieza 1600×900 del prompt de hero. Texto exacto: «Factura a un extranjero» y «XEXX010101000».

## Facebook

### Texto

Si tu cliente está fuera de México y te pide factura, el RFC genérico es XEXX010101000.

Pide su país (clave de 3 letras) y su tax ID. En el CFDI el código postal del receptor es el de tu emisor, el uso es S01 y el régimen es 616.

Te dejamos el JSON y los errores más comunes: https://cfdi.express/blog/facturar-cliente-extranjero-xexx010101000

¿Dudas con la integración? Agenda una demo: https://cal.com/team/acromatico-development/cfdi-express

### Media

Recorte 1200×630 de la misma ilustración. Título «Factura a un extranjero» en Racing Sans One.

## Instagram

### Texto

Factura a un cliente extranjero en CFDI 4.0.

XEXX010101000
País (c_Pais, nunca MEX)
Tax ID
S01 · régimen 616 · CP de tu emisor

El link de la guía va en la bio.

#CFDI #FacturacionElectronica #SAT #CFDIExpress #XEXX010101000 #FacturaElectronica #CFDI40 #ResidenciaFiscal #Mexico #Contabilidad #Emprendedores #NegociosMexico #ComprobanteFiscal #Facturacion

### Media

- Feed 1080×1080: factura y ficha con XEXX010101000. Fondo #055D5E. Título «Factura a un extranjero».
- Historia 1080×1920: los tres datos (RFC, país, tax ID) y sticker al post.

**Prompt Gemini — feed 1080×1080:**

> Cuadrado 1080×1080, ilustración flat. Fondo teal #055D5E. Factura blanca con sello y una ficha que dice «XEXX010101000». Pin de mapa en dorado #FFD700. Título en Racing Sans One, blanco: «Factura a un extranjero». Subtítulo en Quicksand, dorado: «Residencia fiscal y tax ID». Sin fotos, sin logos del SAT, sin banderas.

## Checklist de publicación

- [ ] No publicar hasta `https://cfdi.express/blog/facturar-cliente-extranjero-xexx010101000`.
- [ ] No describir pantallas de la app que no estén en la guía (el botón documentado es «Facturar al Público en General Extranjero»).
- [ ] No nombrar proveedores de timbrado.
- [ ] RFC de ejemplo XEXX010101000, país ARG, tax ID 30715436228, uso S01, régimen 616.
- [ ] CTA a la guía o a la demo en cada red.
- [ ] Si se habla de IVA, remitir al contador.
