---
title: "CFDI 4.0 en Shopify 2026: qué app elegir (y qué no)"
description: "Compara apps de CFDI 4.0 para Shopify en 2026: qué pide el SAT, tres caminos para facturar (app nativa, PAC o portal) y cómo emitir sin capturar a mano."
image: "https://videos.acromatico.dev/api/images/assets/1d839620-c45b-444e-bdc1-160dd7b61efc.png"
author: "Rafael González"
date: "2026-09-08"
keywords: "cfdi 4.0 shopify, facturación shopify, factura 4.0 shopify, app facturación shopify, facturama shopify, built for shopify cfdi"
---
# CFDI 4.0 en Shopify 2026: qué app elegir (y qué no)

![CFDI 4.0 en Shopify](https://videos.acromatico.dev/api/images/assets/1d839620-c45b-444e-bdc1-160dd7b61efc.png)

En 2026 cada venta de una tienda Shopify en México sigue siendo un **CFDI 4.0**. El SAT valida RFC, nombre, régimen y código postal del receptor contra su padrón. Si no coinciden, el comprobante no se timbra.

Hacerlo a mano — copiar datos fiscales, abrir el portal del SAT, exportar un CSV — no escala cuando vendes en línea, en piso de venta y a veces por WhatsApp el mismo día. Aquí te dejo qué pide el SAT en una venta de tienda, tres caminos reales para cumplir y una comparación de las apps que sí aparecen en Shopify.

## ¿Shopify emite CFDI 4.0 por sí solo?

**Respuesta corta:** no. Shopify cobra, arma el pedido y te da el admin. El PAC timbra el XML. Tú eliges si eso ocurre dentro de Shopify, en otro sistema o en el portal del SAT.

Eso no cambió en 2026. Lo que sí cambió para muchas tiendas es el volumen: más canales, más cajeros, más clientes que piden factura al instante. El cuello de botella ya no es “entender la 4.0”; es **no capturar los mismos datos dos veces**.

## Qué pide el SAT en un CFDI 4.0 de tu tienda

**Respuesta corta:** RFC, nombre o razón social, régimen fiscal, código postal del domicilio fiscal y uso de CFDI. Son los mismos campos que aparecen en una Constancia de Situación Fiscal; el SAT los valida contra su padrón. Con esos datos debes poder intentar timbrar: el cliente no está obligado a entregarte el PDF. Para ventas sin factura nominativa, un CFDI global a público en general.

No voy a repetir aquí toda la guía de la versión 4.0. Eso ya está en [CFDI facturas 4.0: qué es, qué cambió y cómo emitirlas](/blog/cfdi-facturas-4-0). Para una venta de Shopify te alcanza con esto:

- **Receptor nominativo.** El SAT cruza RFC, nombre, régimen y CP fiscal. Un `S.A. DE C.V.` de más, un CP de sucursal o un uso de CFDI incompatible con el régimen tumba el timbrado.
- **Conceptos con catálogo SAT.** Cada producto o servicio lleva clave de producto/servicio y unidad vigentes. Shopify no las trae solas: las configuras en la app o en el producto.
- **Público en general.** Quien no pide factura no se “olvida”. Esas ventas van a un CFDI global (RFC `XAXX010101000`) con la información global que marca el [Anexo 20, versión 4.0](http://omawww.sat.gob.mx/tramitesyservicios/Paginas/anexo_20.htm): periodicidad, meses y año.
- **Cancelación con motivo.** Si te equivocaste, no “borras” el folio. Eliges un motivo 01–04. El detalle oficial está en la [guía de cancelación del SAT](https://www.sat.gob.mx/minisitio/Factura/cancela_procesocancelacion.htm).

Pide los **datos fiscales** (RFC, nombre o razón social, régimen, CP fiscal y uso de CFDI), no la Constancia en PDF. El cliente puede dictártelos, llenarlos en un formulario o, si quiere, compartir una cédula de datos fiscales. No está obligado a mandarte el archivo. Con esos campos ya puedes intentar el timbrado; si el SAT rechaza, no coinciden con el padrón.

## Tres caminos: app nativa, PAC / CSV o portal del SAT

**Respuesta corta:** la app nativa factura en el mismo flujo de la venta. El PAC genérico o el CSV te sacan de Shopify. El portal del SAT sirve para un folio suelto, no para un mes de pedidos.

### 1. App nativa de Shopify

Instalas desde la [tienda de aplicaciones](https://apps.shopify.com) y la factura sale del admin, del POS o del checkout. El cliente captura sus datos fiscales; el PAC timbra; el XML/PDF se asocia al pedido.

Eso es lo que buscas si no tienes un equipo de desarrollo y no quieres un segundo sistema para cada venta.

### 2. PAC genérico, CSV o API

Exportas pedidos o los subes al portal de un PAC. Si el sistema no es Shopify y quieres timbrar por API, el camino es [CFDI Express API](https://cfdi.express/api): REST, sandbox y **~$1 MXN por timbre**, sin mensualidad. El anuncio está en [Lanzamos CFDI Express API](/blog/lanzamiento-cfdi-express-api). No es el camino de “instala la app y factura mañana”: sirve si ya tienes ingeniería o un sistema propio. No sustituye la app de Shopify para el dueño de tienda o el contador.

El CSV o el portal web de un PAC también funcionan. El costo oculto es el vaivén: pedido en Shopify, captura en otro lado, UUID que luego tienes que reconciliar.

### 3. Portal del SAT

Puedes emitir desde el portal de Factura electrónica del SAT. Es válido. También es captura folio por folio, sin POS, sin Thank You Page y sin automatizar el global de fin de mes. Las reglas técnicas están en el [Anexo 20](http://omawww.sat.gob.mx/tramitesyservicios/Paginas/anexo_20.htm). Úsalo para un caso excepcional; no para operar la tienda.

## Comparación: CFDI Express, Facturama y Fiscal Pop

**Respuesta corta:** las tres son apps de Shopify y timbran CFDI 4.0. La diferencia está en qué tan adentro del admin, el POS y Flow viven, y en cómo te cobran.

Cifras y funciones de la tabla salen de fichas públicas (septiembre 2026). Si una app no anuncia una integración, no la doy por hecha.

|  | [CFDI Express](https://apps.shopify.com/cfdi-express) | [Facturama](https://apps.shopify.com/facturama) | [Fiscal Pop](https://apps.shopify.com/fiscalpop) |
| --- | --- | --- | --- |
| **Shopify-native / Built for Shopify** | App en Shopify App Store. [Built for Shopify](/blog/cfdi-expres-ya-es-built-for-shopify). | App en Shopify App Store. Su [ficha](https://apps.shopify.com/facturama) no anuncia el distintivo Built for Shopify. | App en Shopify App Store. Su [ficha](https://apps.shopify.com/fiscalpop) no anuncia el distintivo Built for Shopify. |
| **POS** | Formulario en la confirmación de venta de [Shopify POS](/docs/facturacion-pos-punto-de-venta-shopify). La ficha lista Shopify POS. | No lista Shopify POS en “Works with”. En su [página de Shopify](https://facturama.mx/shopify) mencionan un **link de autofacturación** desde el punto de venta, no un formulario nativo en la confirmación. | Lista Shopify POS. Sus [docs](https://docs.fiscalpop.com/ecommerce/shopify/facturacion-en-pos/) describen facturar desde el detalle del pedido en POS (app “Facturar Pedido”). |
| **Thank You / checkout** | Bloque en la [Thank You Page](/docs/facturacion-checkout-thank-you-page) y en la [página de estado de la orden](/docs/facturacion-pagina-estado-de-orden). | Autofacturación en checkout o desde la tienda, según su [ficha](https://apps.shopify.com/facturama) y [facturama.mx/shopify](https://facturama.mx/shopify). | Facturación en la Thank You Page del checkout, según su [ficha](https://apps.shopify.com/fiscalpop) y su [página de Shopify](https://fiscalpop.com/ecommerce/shopify/). |
| **Shopify Flow** | Disparadores y acciones: timbrar, cancelar (motivos 01–04) y [global al público en general](/docs/uso-shopify-flow-automatizaciones-cfdi-express). La ficha lista Shopify Flow. | No aparece en su ficha pública ni en su página de Shopify. | No aparece en su ficha pública ni en su página de Shopify. |
| **Portal de autofacturación** | El cliente entra con su correo al portal de cuentas de Shopify. Tutorial: [portal en 5 minutos](/blog/crea-un-portal-de-auto-facturacion-cfdi-en-5-minutos). | Autofacturación desde la tienda o el checkout (misma ficha). | Página de autofacturación que agregas a la tienda (una línea de HTML), según [fiscalpop.com](https://fiscalpop.com/ecommerce/shopify/). |
| **Modelo de precio** | Cobro en Shopify: mensualidad + uso por CFDI. El plan de entrada publicado en la [App Store](https://apps.shopify.com/cfdi-express) es **15 USD/mes + 0.10 USD por CFDI**; hay planes con cupo incluido. Prueba de 7 días. Detalle en [Planes y precios](/docs/planes-y-precios). | **13 USD/mes**, facturas, complementos de pago y cancelaciones ilimitadas dentro de la app, según [facturama.mx/shopify](https://facturama.mx/shopify). Prueba de 7 días. | **4 USD/mes + 0.10 USD por factura emitida**, según su [ficha](https://apps.shopify.com/fiscalpop). Prueba de 14 días. En su sitio, cancelaciones y notas de crédito no llevan cargo extra. |

Si necesitas timbrar desde un sistema que no es Shopify, CFDI Express también tiene [API](https://cfdi.express/api). Este artículo es de la app de Shopify.

Ninguna de las tres “gana” en todos los renglones. Facturama cobra un precio plano e ilimitado. Fiscal Pop entra más barato al mes. CFDI Express es la que publica **Built for Shopify**, POS en la confirmación de la venta, Flow y el portal de cuentas en el mismo ecosistema.

## Cómo emitir el primer CFDI 4.0 con una app nativa

**Respuesta corta:** cargas tu CSD, asignas claves SAT a los productos y timbras desde el admin, el POS o la Thank You Page. El cliente no tiene que escribirte por WhatsApp para pedir la factura.

El onboarding es el mismo en lo fiscal: RFC emisor, razón social, CP y régimen (tus datos fiscales), más el [Certificado de Sello Digital](/docs/onboarding) (`.cer`, `.key` y contraseña). Después eliges el canal.

### Desde el Admin de Shopify

Abres la app, buscas el pedido y capturas los **datos fiscales** del cliente. Si esa venta no pide factura nominativa, usas público en general (`XAXX010101000`). En CFDI Express el flujo está en [Facturación desde el admin](/docs/facturacion-desde-admin-shopify).

### Desde Shopify POS

Al cerrar la venta en piso, el cajero factura en la confirmación (o, en otras apps, desde el detalle del pedido). No sales a otro sistema ni le pides al cliente que “luego se meta a un link” si la app trae el formulario en el POS. Guía de CFDI Express: [facturación en Shopify POS](/docs/facturacion-pos-punto-de-venta-shopify).

### Desde el checkout (Thank You Page)

El cliente termina de pagar y, en la página de agradecimiento, llena RFC, nombre, régimen, CP y uso de CFDI. Si los datos cuadran con el SAT, se timbra ahí. Guía: [Thank You Page](/docs/facturacion-checkout-thank-you-page). Si se le olvidó, puede hacerlo después en la [página de estado de la orden](/docs/facturacion-pagina-estado-de-orden) o en el [portal de autofacturación](/blog/crea-un-portal-de-auto-facturacion-cfdi-en-5-minutos).

### Si no quieres capturar cada folio

Con [Shopify Flow](/docs/uso-shopify-flow-automatizaciones-cfdi-express) automatizas el timbrado, la cancelación con motivo SAT y el CFDI global de las ventas que nadie facturó. Hay [plantillas de global](/docs/facturacion-al-publico-en-general-manual-y-automatica) al final del día, a los 3 o 10 días, o al cierre de mes.

## Errores típicos al facturar ventas de Shopify

**Respuesta corta:** el SAT rechaza por datos del receptor, por claves de producto viejas, por dejar ventas sin global, o por cancelar sin el motivo correcto.

### El RFC “está bien” y el timbrado falla

Casi nunca es el RFC. Es el nombre abreviado, el régimen que “siempre usamos”, el CP de envío o un uso de CFDI que no aplica a ese régimen. En 4.0 el SAT no perdona la diferencia con su padrón. Si rechaza el timbre, los datos no coinciden: pídele que confirme los campos, no que te mande la Constancia. No copies de un pedido anterior.

### Claves SAT de producto

Shopify trae título, SKU y precio. El Anexo 20 pide **clave de producto o servicio** y **unidad**. Un default genérico en toda la tienda puede pasar un rato y luego estorbar en auditorías o en productos con IEPS. En CFDI Express las configuras por producto o con un default; la guía está en [Códigos SAT por producto](/docs/codigos-sat-producto).

### La factura global de fin de mes

Las ventas que el cliente no facturó no se van a “sin comprobante”. Van a un CFDI global a público en general, con periodicidad y mes. Si dejas el mes abierto, tu contador (y el SAT) te lo cobran después. Decide el periodo — diario, semanal o mensual — y apégate a él. Si usas Flow, no dependas de acordarte el día 1.

### Cancelación: motivos 01 a 04

Los nombres oficiales del SAT:

| Clave | Motivo |
| --- | --- |
| 01 | Comprobantes emitidos con errores con relación |
| 02 | Comprobantes emitidos con errores sin relación |
| 03 | No se llevó a cabo la operación |
| 04 | Operación nominativa relacionada en una factura global |

El **01** pide primero el CFDI que sustituye (relación tipo 04) y, al cancelar, el UUID nuevo. El **04** es el de “el cliente pidió factura después de que ya iba en el global”. El motivo no decide si el receptor debe aceptar; eso lo marcan tipo, monto y fecha. En CFDI Express el flujo está en [Cancelación y acuse](/docs/cancelacion-y-acuse).

## Preguntas frecuentes

### ¿Shopify factura CFDI 4.0 nativo?

No. Shopify no es PAC. Necesitas una app de facturación, [CFDI Express API](https://cfdi.express/api) si timbras desde otro sistema, o el portal del SAT. La venta vive en Shopify; el timbre, en un PAC.

### ¿Cuál app de facturación para Shopify elijo en 2026?

Si quieres quedarte dentro de Shopify — **Built for Shopify**, POS en la confirmación de la venta, Thank You Page, Flow y portal de cuentas —, la app es [CFDI Express](https://apps.shopify.com/cfdi-express). En la tabla también están Facturama (precio plano e ilimitado) y Fiscal Pop (mensualidad baja + cobro por folio). [CFDI Express API](https://cfdi.express/api) es otra pregunta: sirve si facturas desde un sistema que no es Shopify.

### ¿Puedo facturar desde Shopify POS?

Sí, si la app lo trae. CFDI Express y Fiscal Pop lo documentan en POS. Facturama, en su material público, habla de un link de autofacturación desde el punto de venta. Confirma en la ficha y en una venta de prueba antes de capacitar cajeros.

### ¿Qué hago con las ventas que nadie facturó?

Las incluyes en un CFDI global a público en general (RFC `XAXX010101000`) en el periodo que hayas definido. No las dejes “para después del año”.

### ¿Y si no uso Shopify, tengo API?

Sí. [CFDI Express API](https://cfdi.express/api) timbra CFDI 4.0 desde cualquier sistema: REST, sandbox, **~$1 MXN por timbre**, sin mensualidad. Este post es el camino de la app. El de la API está en [el lanzamiento](/blog/lanzamiento-cfdi-express-api).

### ¿Puedo seguir usando el portal del SAT?

Sí, para un folio suelto. No escala a checkout, POS ni al global automático de fin de mes.

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "¿Shopify factura CFDI 4.0 nativo?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No. Shopify no es un PAC. Para emitir CFDI 4.0 necesitas una app de facturación de la Shopify App Store, CFDI Express API si timbras desde otro sistema, o el portal del SAT. La venta vive en Shopify; el timbre lo da el PAC."
      }
    },
    {
      "@type": "Question",
      "name": "¿Cuál app de facturación para Shopify elijo en 2026?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Si quieres quedarte dentro de Shopify — Built for Shopify, POS en la confirmación de la venta, Thank You Page, Flow y portal de cuentas —, la app es CFDI Express. En la tabla también están Facturama (precio plano e ilimitado) y Fiscal Pop (mensualidad baja y cobro por folio). CFDI Express API es otra pregunta: sirve si facturas desde un sistema que no es Shopify."
      }
    },
    {
      "@type": "Question",
      "name": "¿Puedo facturar desde Shopify POS?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Sí, si la app lo soporta. CFDI Express incluye formulario en la confirmación de Shopify POS. Fiscal Pop documenta facturación desde el detalle del pedido en POS. Facturama menciona un link de autofacturación desde el punto de venta en su material público."
      }
    },
    {
      "@type": "Question",
      "name": "¿Qué hago con las ventas de Shopify que nadie facturó?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Deben ir a un CFDI global a público en general (RFC XAXX010101000) en el periodo que definas — diario, semanal o mensual — con la información global que pide el SAT. No las dejes sin comprobante."
      }
    },
    {
      "@type": "Question",
      "name": "¿Y si no uso Shopify, tengo API?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Sí. CFDI Express API timbra CFDI 4.0 desde cualquier sistema: REST, sandbox, alrededor de 1 peso MXN por timbre, sin mensualidad. Este artículo es el camino de la app de Shopify. La API está en cfdi.express/api."
      }
    },
    {
      "@type": "Question",
      "name": "¿Puedo emitir CFDI 4.0 de mis ventas Shopify en el portal del SAT?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Sí, el portal del SAT emite CFDI 4.0 válidos. Es captura folio por folio: no cubre Thank You Page, POS nativo ni el global automático de fin de mes. Sirve para un caso excepcional, no para operar la tienda."
      }
    }
  ]
}
</script>

## Instala la app y factura en el mismo flujo de la venta

Si ya viste la tabla y quieres quedarte dentro de Shopify — checkout, POS, Flow y portal de cuentas —, [instala CFDI Express](https://apps.shopify.com/cfdi-express). Es la opción nativa de este comparativo.

Si facturas desde otro sistema, está la [API](https://cfdi.express/api).

Para el marco SAT (datos del receptor, cancelación 01–04, global), vuelve a [CFDI facturas 4.0](/blog/cfdi-facturas-4-0). Si quieres ver el producto en tu tienda, [agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).
