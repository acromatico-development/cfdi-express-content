---
title: "Todo CFDI vs CFDI Express: ¿portal de timbres o app de Shopify?"
description: "Compara Todo CFDI 4.0 (Captura Digital) y CFDI Express en 2026: paquetes de timbres, módulos sectoriales y la app de Shopify. Guía justa, con precios públicos."
# image: PLACEHOLDER — Diego sube el hero al CDN (Pixar, humano, hoodie teal #055D5E, 1600×900). No inventar UUID. Actualizar este campo y el comentario HERO del cuerpo.
author: "Rafael González"
date: "2026-09-23"
keywords: "todo cfdi, todo cfdi 4.0, captura digital, alternativa todo cfdi, app cfdi shopify, todocfdi, facturación electrónica méxico"
---
# Todo CFDI vs CFDI Express: ¿portal de timbres o app de Shopify?

<!-- TODO: hero pendiente de CDN -->
<!-- HERO: Diego Pixar hoodie-teal; update frontmatter image + ![] after CDN -->
<!-- ENDTODO -->

Si buscaste **todo cfdi** o **todo cfdi 4.0**, el producto que te sale es el portal de facturación de [Captura Digital](https://www.captura-digital.com/). El acceso está en [todocfdi.com](https://www.todocfdi.com/xcfdifacturas40/loginx) (pantalla «CFDi Facturas 4.0»). Compras **paquetes de timbres** y capturas el CFDI en su web o en sus programas de Windows. No es una app de la Shopify App Store.

[CFDI Express](https://apps.shopify.com/cfdi-express) es otra cosa: una app **Built for Shopify** para que la factura salga de la orden, el POS y Flow. El mismo equipo tiene, aparte, [CFDI Express API](https://cfdi.express/api) para quien timbra desde su propio sistema.

Este post es un comparativo **justo y con cifras públicas**. Todo CFDI gana cuando no vendes en Shopify, facturas poco y te sirve un portal con módulos de escuela, taller, hotel o donatarias. CFDI Express gana cuando la venta ya vive en Shopify. Ninguna cubre los dos trabajos.

El marco fiscal (datos del receptor, global, cancelación) está en [CFDI facturas 4.0](/blog/cfdi-facturas-4-0). Si tu canal es la tienda, el mapa de apps está en [CFDI 4.0 en Shopify 2026](/blog/cfdi-4-0-shopify-2026) y el duelo de apps en [Facturama vs CFDI Express](/blog/facturama-vs-cfdi-express). Aquí comparamos **portal de timbres vs app (y, al final, la API)**.

Cifras de páginas públicas, **23 de septiembre de 2026**. Si un producto no anuncia una integración, no la damos por hecha.

## Tabla rápida: Todo CFDI vs CFDI Express

**Respuesta corta:** Todo CFDI es el portal y los programas de Captura Digital (CFDI 4.0, paquetes de timbres en MXN). CFDI Express es la app de Shopify (mensualidad en USD + uso) y, en otro producto, una API de prepago.

|  | [Todo CFDI / Captura Digital](https://www.captura-digital.com/) | [CFDI Express](https://apps.shopify.com/cfdi-express) |
| --- | --- | --- |
| **Qué es** | Portal web y programas Windows para capturar CFDI 4.0. Login: [todocfdi.com](https://www.todocfdi.com/xcfdifacturas40/loginx). Marca comercial de Captura Digital. | App de Shopify (Acromático), **Built for Shopify**. La API es otro producto: [cfdi.express/api](https://cfdi.express/api). |
| **Precio público** | Paquetes con **vigencia de un año**, **IVA incluido**, en [Precios](https://www.captura-digital.com/Precios): 100 timbres **$365 MXN**, 200 **$698**, 300 **$1,062**, 500 **$1,629**, 1,000 **$3,030**. Sin cuota mensual del programa, según su página de [Programas](https://www.captura-digital.com/Programas). | App: **Basic $15 USD/mes + $0.10 USD** por factura generada o cancelada. Plus, Pro y Enterprise en la [ficha](https://apps.shopify.com/cfdi-express) y en [Planes y precios](/docs/planes-y-precios). Prueba de 7 días. API: **~$1 MXN por timbre**, sin mensualidad del producto API. |
| **Arranque** | Registro público: **100 timbres gratis**, vigencia **180 días**, solo usuarios nuevos ([registro](https://www.captura-digital.com/registro.html)). | Prueba de 7 días en la app. Sandbox `sk_test_` en la API. |
| **Shopify** | **No publican** una app en la Shopify App Store (búsqueda «todo cfdi», septiembre 2026, sin listado de Captura Digital). | Sí. Thank You, estado de la orden, admin y cuentas de cliente. |
| **POS y Flow** | No publican Shopify POS ni Shopify Flow. | Formulario en la confirmación de [Shopify POS](/docs/facturacion-pos-punto-de-venta-shopify). **Disparadores y acciones** de [Shopify Flow](/docs/uso-shopify-flow-automatizaciones-cfdi-express). |
| **Notas de crédito (egreso)** | Su página de Programas **no lista** CFDI de egreso (septiembre 2026). No afirmamos que el portal carezca de esa función fuera de lo publicado. | Sí, en la app: egreso por devolución, reembolso o descuento, sin cancelar la factura original. Guía: [Notas de crédito](/docs/notas-de-credito). |
| **Comprobantes que sí publican** | En [Programas](https://www.captura-digital.com/Programas), web: facturas, honorarios y arrendamiento, nómina 4.0, escolar (complemento iedu), taller automotriz, hotel (ISH) y donatarias. En el home también nombran fletes y complementos de pago. El login enlaza un portal de [CFDI Pagos](https://www.elcfdi.com/xcfdipagos40/loginx) y un video de factura global. | Ingreso desde la orden de Shopify, global al público en general, PPD y complementos de pago, cancelación con motivos 01–04. |
| **PPD** | En la versión Windows de CFDi Facturas publican: «No cuenta con facturas en PPD, solo en plataforma web». La web sí anuncia complementos de pago. | PPD en la misma app. Guía: [Complementos de pago](/docs/complementos-de-pago). |
| **Descarga masiva del SAT** | Producto **aparte**, Windows: [Descarga masiva](https://www.captura-digital.com/descargaMasiva). Licencia 1 RFC **$116 MX** y 25 RFC **$398 MX** en esa página (no es el paquete de timbres). | No es una herramienta de descarga masiva de XML del SAT. En la app descargas el PDF y el XML de lo que timbraste en Shopify. |
| **Varios RFC** | Su FAQ: el sistema es mono-usuario; **una cuenta por RFC emisor**. | La app va con la tienda. La [API](https://cfdi.express/api) admite varios emisores (merchants) en la misma cuenta. |
| **Pago del servicio** | Tarjeta, transferencia SPEI u OXXO. La recarga se aplica al RFC, según Precios. | La app se cobra en **Shopify Billing** (USD). La API se recarga con tarjeta (Stripe), en MXN. |

Todo CFDI es un portal de timbres. CFDI Express es la app de la tienda. Si mezclas el paquete de $365 MXN con los $15 USD de la app, estás cotizando dos productos distintos.

## Fortalezas de Todo CFDI

**Respuesta corta:** precio de paquete en pesos, sin cuota del programa, pago en OXXO y módulos para giros que una tienda Shopify no trae (escuela, taller, hotel, donataria), más descarga masiva para el contador.

Captura Digital publica el portal que la gente busca como Todo CFDI. En [Programas](https://www.captura-digital.com/Programas) un solo paquete de timbres sirve para sus sistemas, y ellos mismos dicen que no hay plazos forzosos ni cuotas adicionales: pagas timbres. El home habla de timbrar con **tres PAC** y de más de diez años en el mercado.

Lo que su material público pone sobre la mesa (septiembre 2026):

- **CFDI 4.0** en la nube: facturas, honorarios, arrendamiento, nómina, complementos de pago, fletes, y “muchos más”, según el [home](https://www.captura-digital.com/).
- Módulos web con dato de giro: **escolar** (datos del padre o tutor, colegiaturas, complemento iedu), **taller** (serie, modelo, placas), **hotel** (impuesto sobre hospedaje) y **donatarias** (complemento de donatarias).
- Versión **Windows** de facturas, nómina y descarga masiva. En facturas de escritorio avisan que el PPD está solo en la web.
- **100 timbres gratis** al registrarte, con vigencia de 180 días, solo si eres usuario nuevo.
- Paquetes chicos. Con 100 timbres a **$365 MXN** al año (IVA incluido) un profesionista no tiene que abrir una suscripción en dólares.
- Pago en **OXXO**, SPEI o tarjeta, y la recarga cae al RFC.
- **Descarga masiva** de XML emitidos y recibidos (Windows 8 o superior, e.firma o contraseña del SAT, exportación a Excel). Es otra licencia, no un extra silencioso del paquete de timbres.

Si facturas desde el escritorio o el navegador, tu cliente no compra en Shopify y tu contador quiere bajar XML del SAT, este es el hueco que ellos cubren bien. Su FAQ de Programas también dice que **no** tienen complemento Carta Porte. Nosotros tampoco lo anunciamos en la API: no es un punto a favor de nadie en esta página.

## Fortalezas de CFDI Express

**Respuesta corta:** la factura sale de la orden de Shopify — Thank You, estado de la orden, POS, notas de crédito y Flow — y, si mañana timbras fuera de la tienda, hay API y MCP en el mismo stack.

CFDI Express nació para la tienda. En la [App Store](https://apps.shopify.com/cfdi-express) publica:

- Facturación en **Thank You Page** y en la **página de estado de la orden**.
- Facturación en **Shopify POS**.
- **Factura global** automática al público en general.
- Facturación **parcial / PPD** y complementos de pago.
- **Notas de crédito** (la ficha lista *Credit notes*).
- **Shopify Flow** con disparadores y acciones, no solo un renglón de “Works with”.
- Envío de PDF y XML por correo.

En operación, el cajero no saca al cliente del POS, el comprador se autofactura en el checkout o en el [portal de cuentas](/blog/crea-un-portal-de-auto-facturacion-cfdi-en-5-minutos), y una devolución puede ir a nota de crédito o a cancelación con motivo 01–04. Las guías están en [POS](/docs/facturacion-pos-punto-de-venta-shopify), [notas de crédito](/docs/notas-de-credito) y [Flow](/docs/uso-shopify-flow-automatizaciones-cfdi-express).

El plan de entrada es **Basic: $15 USD al mes + $0.10 USD** por factura generada o cancelada. Si el volumen crece, Plus ($99 USD, 1,500 CFDIs), Pro y Enterprise están en [Planes y precios](/docs/planes-y-precios); confirma el cupo incluido en la ficha al suscribirte. Eso no lo hace más barato que un paquete de 100 timbres. Lo hace el producto que sí habla con la orden.

Pide **datos fiscales** (RFC, nombre o razón social, régimen, código postal fiscal y uso de CFDI). No pidas el PDF de la Constancia de Situación Fiscal: el SAT valida contra su padrón y con esos campos ya puedes intentar el timbre. Detalle en [CFDI facturas 4.0](/blog/cfdi-facturas-4-0).

## Escenarios de precio (ilustrativos)

**Respuesta corta:** Todo CFDI sale más claro en volumen bajo **sin** Shopify. La API de CFDI Express sale más baja **por timbre** si ya tienes sistema propio y agotas lo que timbras. La app de Shopify no se compara en pesos con un paquete anual: cobra en USD porque vive dentro de la tienda. **Nadie es siempre más barato.**

Precios de Captura Digital tomados del HTML de [Precios](https://www.captura-digital.com/Precios) el **23 de septiembre de 2026** (IVA incluido, licencia con vigencia de un año). Si esa página cambia, manda la página, no este post.

Costo por timbre **si agotas el paquete** antes de que venza:

| Paquete | Precio publicado | Por timbre, si lo agotas |
| --- | --- | --- |
| 100 | $365 MXN | $3.65 MXN |
| 200 | $698 MXN | $3.49 MXN |
| 300 | $1,062 MXN | $3.54 MXN |
| 500 | $1,629 MXN | $3.26 MXN |
| 1,000 | $3,030 MXN | $3.03 MXN |

El paquete de 300 no baja el costo por timbre frente al de 200. A 500 y a 1,000 sí baja. Si te sobran timbres al cumplir el año, la licencia caduca y el costo de cada CFDI que sí usaste sube.

| Escenario | Todo CFDI / Captura Digital | CFDI Express |
| --- | --- | --- |
| **~100 CFDIs en el año**, portal, sin Shopify | **$365 MXN** (paquete de 100). Usuario nuevo: **100 timbres gratis**, 180 días, según el registro. | La app no aplica. La API, a ~$1 MXN por timbre, serían **~$100 MXN**, sin portal de captura ni módulo escolar, taller u hotel. |
| **~500 CFDIs en el año**, portal | **$1,629 MXN** (paquete de 500). | API **~$500 MXN**. Misma salvedad: no incluye su programa. |
| **Tienda Shopify, ~40 CFDIs al mes** | No publican app. Un paquete de 500 ($1,629 MXN al año) cubre ese volumen **si** capturas cada factura en su portal, fuera de la orden. | Basic: $15 + (40 × $0.10) = **$19 USD/mes**, dentro de Thank You, POS y Flow. |

Lectura honesta:

- Profesionista, arrendador, escuela o taller **sin** tienda Shopify: el producto es Todo CFDI. El paquete y el OXXO son el argumento. Compararlo con $15 USD al mes es mezclar canales.
- Integrador con ERP propio: el peso por timbre de la [API](https://cfdi.express/api) (~$1, sin cuota del API) queda debajo de estos paquetes **cuando agotas lo que timbras**. No te entrega la pantalla de Captura Digital. El duelo de APIs con mensualidad está en [Facturapi vs CFDI Express API](/blog/facturapi-vs-cfdi-express-api).
- Tienda Shopify: CFDI Express cuesta la suscripción porque timbra la orden. Todo CFDI puede ser más barato en pesos y, aun así, dejarte copiando RFC a mano.

La descarga masiva no entra en esta tabla. En su página son **$116 MX** (1 RFC) y **$398 MX** (25 RFC), licencia distinta.

## ¿Cuándo elegir Todo CFDI y cuándo CFDI Express?

**Respuesta corta:** Todo CFDI si capturas el CFDI en un portal o en Windows y pagas timbres. CFDI Express si vendes en Shopify o si vas a timbrar desde código o desde un agente.

### Elige Todo CFDI si…

- No vendes en Shopify y quieres **capturar** facturas, honorarios, arrendamiento o nómina en un portal.
- Tu giro es **escuela, taller, hotel o donataria** y te sirve el módulo que ellos publican (iedu, placas, ISH, complemento de donatarias).
- Facturas **poco** y prefieres un paquete de 100 a 500 timbres, con vigencia de un año, pagable en OXXO.
- Eres usuario nuevo y quieres probar con los **100 timbres gratis** (180 días).
- Tu contador necesita **descarga masiva** de XML del SAT (producto Windows, licencia aparte).
- Te basta **una cuenta por RFC**, como indica su FAQ.

### Elige CFDI Express si…

- La venta ya está en **Shopify** y quieres autofactura en Thank You y en el estado de la orden.
- Tienes **piso de venta** y el cajero factura en Shopify POS.
- Quieres **notas de crédito** y **Flow** (disparadores y acciones) atados a la orden, el reembolso y el global.
- Aceptas mensualidad en USD + uso a cambio de no recapturar el pedido en otro portal.
- Más adelante timbras **fuera** de la tienda: la [API](https://cfdi.express/api) (~$1 MXN por timbre, sin mensualidad), el servidor MCP y el skill `npx skills add CFDI-Express/skills` son el mismo stack, otro producto.

Si ya estás contento en Todo CFDI y no vas a abrir una tienda Shopify, no hay premio por migrar. Si tu operación es la tienda, el portal no sustituye la app por más barato que salga el timbre suelto.

## Si timbras desde código: CFDI Express API (aparte)

Este artículo es de **portal vs app**. Lo siguiente es solo para integradores.

[CFDI Express API](https://cfdi.express/api) no es el login de Todo CFDI ni la app de $15 USD. Es REST, sandbox con `sk_test_`, saldo prepagado, **alrededor de $1 MXN por timbre** y sin mensualidad del producto API. Hay servidor MCP en `https://api.cfdi.express/mcp` y el skill `npx skills add CFDI-Express/skills`. El anuncio está en [Lanzamos CFDI Express API](/blog/lanzamiento-cfdi-express-api).

Captura Digital no publica en estas páginas una API de timbrado para terceros. No afirmamos que no exista un convenio privado: sí afirmamos que **no está en el material público** que usamos para este comparativo (home, Programas, Precios y el login de Todo CFDI, septiembre 2026).

## Preguntas frecuentes

### ¿Todo CFDI o CFDI Express?

Depende de dónde nace la factura. **Todo CFDI** (Captura Digital) es un portal y programas Windows de CFDI 4.0, con paquetes de timbres en pesos y módulos de escuela, taller, hotel y donatarias. **CFDI Express** es la app de Shopify para Thank You, POS, notas de crédito y Flow, más una API aparte si timbras desde tu sistema. Si no tienes tienda Shopify, el portal encaja. Si la orden ya está en Shopify, la app encaja.

### ¿Todo CFDI es una app de Shopify?

No publican una app en la Shopify App Store. El servicio es el portal en [todocfdi.com](https://www.todocfdi.com/xcfdifacturas40/loginx) y los programas de [captura-digital.com](https://www.captura-digital.com/). Una búsqueda de «todo cfdi» en la App Store, en septiembre 2026, no devuelve un listado suyo. Eso no describe integraciones privadas que no hayan publicado.

### ¿Cuánto cuestan los timbres de Todo CFDI?

Según [Precios](https://www.captura-digital.com/Precios), consultado el 23 de septiembre de 2026, IVA incluido y vigencia de un año: **$365 MXN** (100), **$698** (200), **$1,062** (300), **$1,629** (500) y **$3,030** (1,000). El [registro](https://www.captura-digital.com/registro.html) ofrece 100 timbres gratis por 180 días a usuarios nuevos. La descarga masiva se cobra en otra licencia.

### ¿Cuál es más barata?

En volumen bajo y **sin** Shopify, el paquete de Todo CFDI (desde $365 MXN al año, o 100 timbres gratis si eres usuario nuevo) es el número fácil. Por timbre agotado, la API de CFDI Express (~$1 MXN) queda debajo de esos paquetes ($3.03 a $3.65), sin darte su portal. En una tienda Shopify, la app empieza en **$15 USD + $0.10 USD** por CFDI y se paga porque conecta la orden. No hay un ganador único.

### ¿Tengo que pedir la Constancia de Situación Fiscal en PDF?

No. Pide **datos fiscales**: RFC, nombre o razón social, régimen, código postal fiscal y uso de CFDI. El PDF no es un requisito para intentar el timbrado.

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "¿Todo CFDI o CFDI Express?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Depende de dónde nace la factura. Todo CFDI, de Captura Digital, es un portal y programas Windows de CFDI 4.0, con paquetes de timbres en pesos y módulos de escuela, taller, hotel y donatarias. CFDI Express es la app de Shopify para Thank You, POS, notas de crédito y Flow, más una API aparte si timbras desde tu sistema. Sin tienda Shopify encaja el portal. Si la orden ya está en Shopify, encaja la app."
      }
    },
    {
      "@type": "Question",
      "name": "¿Todo CFDI es una app de Shopify?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No publican una app en la Shopify App Store. El servicio es el portal en todocfdi.com y los programas de captura-digital.com. En septiembre de 2026, una búsqueda de «todo cfdi» en la App Store no devuelve un listado de Captura Digital. Eso no describe integraciones privadas que no hayan publicado."
      }
    },
    {
      "@type": "Question",
      "name": "¿Cuánto cuestan los timbres de Todo CFDI?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Según la página de Precios de Captura Digital, consultada el 23 de septiembre de 2026, con IVA incluido y vigencia de un año: 365 MXN por 100 timbres, 698 por 200, 1,062 por 300, 1,629 por 500 y 3,030 por 1,000. El registro ofrece 100 timbres gratis por 180 días a usuarios nuevos. La descarga masiva es otra licencia."
      }
    },
    {
      "@type": "Question",
      "name": "¿Todo CFDI o CFDI Express es más barato?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Sin Shopify y con poco volumen, el paquete de Todo CFDI (desde 365 MXN al año, o 100 timbres gratis para usuarios nuevos) es el número fácil. Por timbre agotado, la API de CFDI Express, alrededor de 1 MXN, queda debajo de esos paquetes, que salen entre 3.03 y 3.65 MXN si se agotan, y no incluye el portal de Captura Digital. En Shopify la app empieza en 15 USD al mes más 0.10 USD por CFDI. No hay un ganador único."
      }
    },
    {
      "@type": "Question",
      "name": "¿Tengo que pedir la Constancia de Situación Fiscal en PDF?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No. Pide datos fiscales: RFC, nombre o razón social, régimen, código postal fiscal y uso de CFDI. El PDF de la Constancia no es un requisito para intentar el timbrado."
      }
    }
  ]
}
</script>

## Instala la app, abre el portal o prueba la API

Si vendes en Shopify y la factura tiene que salir de la orden, [instala CFDI Express](https://apps.shopify.com/cfdi-express). Prueba de 7 días.

Si tu caso es el portal de timbres —colegiatura, taller, hotel, honorarios— el sitio de ellos es [captura-digital.com](https://www.captura-digital.com/) y el alta está en su [registro](https://www.captura-digital.com/registro.html). Evaluarlo con sus páginas es lo justo.

Si timbras desde un sistema o un agente: [CFDI Express API](https://cfdi.express/api) y las llaves en [dash.cfdi.express](https://dash.cfdi.express).

Para ver la app en tu tienda, [agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).
