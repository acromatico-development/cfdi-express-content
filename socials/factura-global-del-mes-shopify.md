# Social Pack | Cómo hacer la factura global del mes en Shopify (público en general)

Corresponde al blog post: [`pages/factura-global-del-mes-shopify.md`](../pages/factura-global-del-mes-shopify.md)

Recursos oficiales para enlazar:

- Post (tras el merge): https://cfdi.express/blog/factura-global-del-mes-shopify
- Guía: https://cfdi.express/docs/facturacion-global-mensual
- Un CFDI por orden (el otro camino): https://cfdi.express/blog/factura-global-shopify
- Docs de público en general por orden: https://cfdi.express/docs/facturacion-al-publico-en-general-manual-y-automatica
- Shopify Flow: https://cfdi.express/docs/uso-shopify-flow-automatizaciones-cfdi-express
- POS: https://cfdi.express/blog/facturar-shopify-pos
- App de Shopify: https://apps.shopify.com/cfdi-express
- Landing: https://cfdi.express
- Demo: https://cal.com/team/acromatico-development/cfdi-express

---

## Cómo usar este pack

1. Genera la imagen hero con el prompt de abajo (Gemini) y súbela a `videos.acromatico.dev`.
2. Sustituye en el frontmatter de `pages/factura-global-del-mes-shopify.md` la URL `PENDIENTE-factura-global-del-mes-shopify.png` por el asset publicado, **sin query**. El cuerpo del post usa esa misma URL con `?w=1600`.
3. Publica con el **Texto** sugerido. El CTA es la guía y, en segundo lugar, instalar la app.
4. Esta pieza es la factura global **del mes**: hasta 1,000 órdenes por comprobante, planes **Plus, Pro y Enterprise**, desde la app **3.26.0**. No la mezcles con el camino de un CFDI por orden (ese es el post `factura-global-shopify`).
5. No prometas otra periodicidad. Aquí es mensual (`04`). El RFC de público en general es `XAXX010101000`.
6. No publiques el link del blog hasta que el post esté en `https://cfdi.express/blog/factura-global-del-mes-shopify`. **No mergear hasta que Rafael revise.**

---

## Imagen hero del blog

**Pendiente.** El frontmatter apunta a:

`https://videos.acromatico.dev/api/images/assets/PENDIENTE-factura-global-del-mes-shopify.png?w=1600`

Al publicarla, el `image` del frontmatter queda sin query (la plantilla también lo usa como `og:image`).

Copy en el arte, ortografía exacta:

- Título: «Factura global del mes»
- Subtítulo: «Público en general en Shopify»

**Prompt Gemini — hero 1600×900:**

> Ilustración flat y moderna, profesional B2B, alto contraste, sin fotografías de stock y sin logos de otras marcas, para el hero de un blog de facturación en México. Formato 1600×900 (16:9). Fondo teal oscuro #055D5E. Al centro, un documento blanco grande con sello circular dorado #FFD700 y la leyenda «PÚBLICO EN GENERAL». Hacia el documento entran, en filas ordenadas, hasta mil tickets de venta pequeños, en blanco y gris #D7D7D7. A un lado, un calendario de pared con un mes marcado en dorado. Título grande en Racing Sans One, blanco, texto exacto: «Factura global del mes». Subtítulo en Quicksand, acento dorado #FFD700: «Público en general en Shopify». Sin código, sin rostros fotográficos, sin logos del SAT.

**Aspecto:** 16:9 · 1600×900.

**Prompt Gemini — OG 1200×630:**

> Imagen horizontal 1200×630 (1.91:1), ilustración flat B2B. Fondo teal oscuro #055D5E. Un documento con sello dorado #FFD700 y la leyenda «PÚBLICO EN GENERAL», tickets de venta que se agrupan en ese documento y un calendario con un mes marcado. Título en Racing Sans One, blanco, texto exacto: «Factura global del mes». Subtítulo en Quicksand, dorado #FFD700: «Público en general en Shopify». Deja aire para que el título no se corte en el recorte de LinkedIn. Sin fotos de stock, sin logos ajenos, sin código.

**Aspecto:** 1200×630 (1.91:1).

---

## LinkedIn

### Texto

Las ventas de Shopify que nadie facturó no se quedan sin comprobante. Van a público en general.

En CFDI 4.0 esa factura lleva el RFC genérico `XAXX010101000` y la información global del periodo: periodicidad, mes y año. La del cierre de mes es la periodicidad mensual.

Desde la versión 3.26.0 de CFDI Express, en Plus, Pro y Enterprise, ya puedes juntarlas:

→ **A mano.** El primer elemento del menú de la app es Facturación global. Desde ahí lanzas la corrida.
→ **Con Shopify Flow.** La acción se llama Facturación global. Al terminar, dispara Facturación global terminada. Si algo falla, te llega un correo.

Qué toma la corrida: órdenes pagadas, en MXN y sin factura. Hasta 1,000 órdenes por comprobante. No duplica: cada orden guarda el folio y el UUID. Si después cancelas o reembolsas una, la app te avisa para emitir la nota de crédito. El cierre baja en CSV y en Excel, una fila por orden, con IVA al 0%, 8% y 16% e IEPS.

Guía: https://cfdi.express/docs/facturacion-global-mensual
Post: https://cfdi.express/blog/factura-global-del-mes-shopify

#CFDI #CFDI40 #FacturacionElectronica #SAT #Shopify #CFDIExpress #FacturaGlobal #PublicoEnGeneral

### Media

**Prompt Gemini:**

> Imagen horizontal 1200×627 (1.91:1), ilustración flat B2B, sin fotos de stock. Fondo teal oscuro #055D5E. A la izquierda, un calendario con un mes marcado en dorado #FFD700. A la derecha, un documento blanco con sello y la leyenda «PÚBLICO EN GENERAL», y una pila de órdenes que entran en él. Título en Racing Sans One, blanco, texto exacto: «La factura global del mes». Subtítulo en Quicksand: «Hasta 1,000 órdenes · Shopify». Acento dorado #FFD700, gris #D7D7D7, alto contraste, sin logos ajenos, sin código.

**Aspecto:** 1200×627 (1.91:1).

**Prompt alterno (mismo aspecto):**

> Misma pieza 1200×627, fondo #055D5E. Tres tarjetas blancas en fila: «Pagada», «MXN», «Sin factura». Debajo, una flecha dorada #FFD700 hacia un solo CFDI con el texto «1,000 órdenes». Título exacto en Racing Sans One: «Facturación global». Subtítulo en Quicksand: «Plus, Pro y Enterprise». Sin fotos, sin logos de terceros.

---

## X (Twitter)

### Texto (post principal)

La factura global del mes en Shopify ya se puede agrupar.

Público en general (XAXX010101000), periodicidad mensual, hasta 1,000 órdenes por CFDI. A mano o con la acción Facturación global de Flow.

→ https://cfdi.express/blog/factura-global-del-mes-shopify

### Hilo opcional

1. En CFDI 4.0 las ventas sin factura van a público en general: RFC XAXX010101000, con periodicidad, mes y año. La del cierre es la mensual (04).
2. En CFDI Express 3.26.0, planes Plus, Pro y Enterprise, la corrida junta esas órdenes. Tope: 1,000 por comprobante. Si hay más, salen varias facturas.
3. Solo entran órdenes pagadas, en MXN y sin factura. No duplica. Cada orden guarda folio y UUID.
4. La lanzas desde el primer elemento del menú, Facturación global, o con esa acción en Shopify Flow. Al terminar dispara Facturación global terminada. Si falla, llega un correo.
5. Si una orden de la global se cancela o se reembolsa, la app avisa para emitir la nota de crédito. El reporte baja en CSV y Excel, una fila por orden. Guía: https://cfdi.express/docs/facturacion-global-mensual

### Media

**Prompt Gemini:**

> Imagen horizontal 1600×900 (16:9) para X. Fondo teal oscuro #055D5E. Un documento con sello dorado #FFD700 y la leyenda «PÚBLICO EN GENERAL», y un mes marcado en un calendario. Título grande en Racing Sans One, blanco, texto exacto: «Factura global del mes». Línea menor en Quicksand: «Hasta 1,000 órdenes». Estilo flat, alto contraste, sin fotografías, sin logos ajenos.

**Aspecto:** 1600×900 (16:9).

**Prompt alterno:**

> Misma medida 1600×900. Fondo #055D5E. Texto exacto en Racing Sans One, blanco: «XAXX010101000». Debajo, en Quicksand y dorado #FFD700: «Público en general · mes». Un sello de factura simple, sin logos de terceros, sin código.

---

## Facebook

### Texto

¿Ya cerraste las ventas de Shopify que nadie facturó?

Esas van a público en general. En CFDI 4.0 el comprobante lleva el RFC XAXX010101000 y el periodo: periodicidad mensual, mes y año.

Con CFDI Express, en Plus, Pro y Enterprise, las juntas en facturas globales de hasta 1,000 órdenes. La corres desde el menú de la app o la programas con la acción Facturación global de Shopify Flow.

Solo entran órdenes pagadas, en pesos y sin factura. No se duplican. Si después hay una cancelación o un reembolso, la app te avisa para hacer la nota de crédito. El reporte sale en CSV y en Excel, una fila por orden.

Guía: https://cfdi.express/docs/facturacion-global-mensual

### Media

**Prompt Gemini:**

> Imagen horizontal 1200×630 (1.91:1), ilustración flat, fondo teal oscuro #055D5E. Un comerciante estilizado, sin rostro realista, junto a un calendario y un documento con sello dorado #FFD700 que dice «PÚBLICO EN GENERAL». Título en Racing Sans One, blanco, texto exacto: «Cierra el mes en Shopify». Subtítulo en Quicksand: «Factura global a público en general». Gris #D7D7D7, alto contraste, sin fotos de stock, sin logos ajenos.

**Aspecto:** 1200×630 (1.91:1).

**Prompt alterno:**

> Misma medida 1200×630. Fondo #055D5E. Ícono de engranaje y un reloj en dorado #FFD700, más un documento blanco. Texto exacto: «Facturación global». Subtítulo: «A mano o con Shopify Flow». Tipografía Racing Sans One y Quicksand. Sin código, sin logos de terceros.

---

## Instagram

### Texto (feed)

La factura global del mes, sin capturarla orden por orden.

RFC de público en general: XAXX010101000
Periodicidad: mensual
Hasta 1,000 órdenes por CFDI
Planes Plus, Pro y Enterprise

La lanzas desde la app o con Shopify Flow. Si algo falla, te llega un correo. El reporte baja en CSV y Excel.

Guía en bio → cfdi.express/docs/facturacion-global-mensual
Post: cfdi.express/blog/factura-global-del-mes-shopify
App: apps.shopify.com/cfdi-express

#CFDI #CFDI40 #FacturacionElectronica #SAT #CFDIExpress #FacturaGlobal #PublicoEnGeneral #Shopify #ShopifyMexico #ShopifyFlow #FacturaElectronica #Mexico #PyME #TiendaEnLinea #Ecommerce #EcommerceMexico #Emprendedores #Comerciantes #Contadores #Contabilidad #SATMexico #Factura40 #CFDIShopify #FacturacionShopify #NegociosMexico #Automatizacion #VentasEnLinea #XAXX010101000 #CFDIExpressApp #FacturaGlobalDelMes

### Media (feed cuadrado)

**Prompt Gemini:**

> Imagen cuadrada 1080×1080, ilustración flat B2B. Fondo teal oscuro #055D5E. Centro: documento blanco con sello dorado #FFD700 y la leyenda «PÚBLICO EN GENERAL», tickets que se ordenan hacia el documento y un calendario con un mes marcado. Título en Racing Sans One, blanco, texto exacto: «Factura global del mes». Texto menor en Quicksand: «En tu tienda Shopify». Alto contraste, sin fotos de stock, sin hashtags pintados, sin logos ajenos, sin código.

**Aspecto:** 1080×1080 (1:1).

**Prompt alterno (1:1):**

> Cuadrado 1080×1080, fondo #055D5E. Tres líneas en Quicksand, blanco: «Pagada», «En pesos», «Sin factura». Abajo, en Racing Sans One: «Hasta 1,000». Acento dorado #FFD700. Sin fotos, sin logos.

### Media (historia opcional)

**Prompt Gemini:**

> Historia vertical 1080×1920, ilustración flat. Fondo teal #055D5E con un halo dorado #FFD700. Centro: documento «PÚBLICO EN GENERAL» y un calendario de un mes. Texto grande exacto, Racing Sans One: «Factura global del mes». Texto menor en Quicksand: «Público en general». CTA abajo: «Lee la guía». Deja el tercio superior e inferior limpios para la interfaz de Instagram. Sin fotos reales, sin logos ajenos, sin código.

**Aspecto:** 1080×1920 (historia).

---

## Checklist de publicación

- [ ] Hero generado y subido. Frontmatter `image` sin query. Cuerpo del post con `?w=1600`.
- [ ] Rafael revisa el post antes del merge. **No mergear:** en este repo el merge publica en vivo.
- [ ] No publicar el URL del blog hasta que esté vivo en `https://cfdi.express/blog/factura-global-del-mes-shopify`.
- [ ] CTA = guía de facturación global mensual + instalar la app.
- [ ] Datos alineados al post: planes Plus, Pro y Enterprise; app 3.26.0; hasta 1,000 órdenes; solo pagadas, MXN y sin factura; folio y UUID; sin duplicar; aviso para nota de crédito; CSV y Excel con IVA 0/8/16% e IEPS; acción y disparador de Flow. Periodicidad mensual (`04`). RFC `XAXX010101000`.
- [ ] No prometer la corrida agrupada en el plan Básico (Standard). Ese plan sigue con un CFDI por orden, a mano o con la plantilla de Shopify Flow. No presentes ese camino como si fuera la factura global del mes.
- [ ] Ortografía del arte: «Factura global del mes» / «Público en general en Shopify» / «PÚBLICO EN GENERAL».
- [ ] Sin logos del SAT ni de terceros en las imágenes.
