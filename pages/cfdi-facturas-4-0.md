---
title: "CFDI facturas 4.0: qué es, qué cambió y cómo emitirlas sin errores"
description: "CFDI facturas 4.0 es la única versión válida del SAT desde abril 2023. Qué cambió vs 3.3, qué datos pide el receptor y cómo emitir o cancelar sin rechazos."
image: "https://assets.acromatico.dev/assets/689cc71d-6965-44a2-9cb8-c5478c037304.jpg"
author: "Rafael González"
date: "2026-08-31"
keywords: "cfdi facturas 4.0, factura 4.0, cfdi 4.0, facturas 4.0, facturación 4.0, factura electrónica 4.0, SAT"
---
<!-- image: se reemplaza cuando Diego entregue el poster/OG 16:9. -->
# ¿Qué es CFDI facturas 4.0? Qué cambió y cómo emitirlas sin errores

**CFDI facturas 4.0** (también llamada factura 4.0 o CFDI 4.0) es la versión vigente del Comprobante Fiscal Digital por Internet. Desde el **1 de abril de 2023** es la única que el SAT acepta: la 3.3 ya no se puede timbrar.

En la práctica, el SAT valida al emitir que el RFC, el nombre, el régimen fiscal y el código postal del receptor coincidan con su Constancia de Situación Fiscal. Si no coinciden, la factura se rechaza. Aquí te dejo qué cambió, qué datos no pueden fallar y cómo cancelar con los motivos oficiales.

<iframe width="100%" height="400" src="https://www.youtube.com/embed/M7ImVfrJuH8" title="Qué es CFDI facturas 4.0" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## ¿Qué es la factura 4.0 (CFDI 4.0)?

**Respuesta corta:** es el formato actual de factura electrónica en México. Toda venta, servicio o ingreso que debas comprobar ante el SAT se documenta con un CFDI 4.0: un XML timbrado, con sello digital, que el receptor puede descargar y validar.

El SAT publica las reglas técnicas en el [Anexo 20, versión 4.0](http://omawww.sat.gob.mx/tramitesyservicios/Paginas/anexo_20.htm). Esa versión empezó a existir el 1 de enero de 2022 (convivió un tiempo con la 3.3) y, desde abril de 2023, es **la única válida**.

No es un “tipo extra” de factura ni un trámite aparte. Es la misma factura de siempre, con validaciones más estrictas: el SAT cruza los datos del receptor contra su padrón antes de dejar que el PAC timbre.

Si compras, vendes, das servicios o necesitas deducir, las facturas que pides y las que emites son CFDI 4.0.

## ¿Desde cuándo es obligatoria la factura 4.0?

**Respuesta corta:** desde el **1 de abril de 2023**. A partir de esa fecha la versión 3.3 dejó de ser válida.

El SAT lo dice así en su documentación del Anexo 20: la versión se actualizó a 4.0 el 1 de enero de 2022 y **desde el 1 de abril de 2023 es la única válida**.

Aplica a personas físicas, personas morales, comercios, freelancers y plataformas digitales. Si alguien todavía intenta emitir en 3.3, ese comprobante no es válido.

## ¿Qué cambió en CFDI 4.0 respecto a la 3.3?

**Respuesta corta:** el SAT se puso más estricto con la identidad del receptor y con la cancelación. Los tipos de CFDI (ingreso, egreso, traslado, nómina, pago) no cambiaron; cambió **cómo se llenan** y **cuándo el timbrado los acepta**.

### Datos del receptor que ahora se validan

En 4.0 el nombre, el régimen fiscal y el código postal del domicilio fiscal del receptor son **obligatorios** y deben coincidir con la Constancia de Situación Fiscal (CSF). Un dígito mal en el CP, un uso de CFDI incompatible con el régimen o un nombre abreviado suelen terminar en rechazo al timbrar.

### Cancelación con motivo oficial

Desde el esquema de cancelación vigente (alineado con la 4.0), ya no basta con “borrar” el comprobante. Tienes que elegir uno de cuatro motivos del SAT (claves 01 a 04). En muchos casos el receptor debe aceptar la solicitud en su Buzón Tributario.

### Otros campos que trajo el Anexo 20 versión 4.0

El SAT también incorporó, entre otros:

- **Exportación:** hay que indicar si la operación es de exportación o no aplica.
- **Información global** (periodicidad, meses y año) cuando facturas al público en general.
- **Objeto del impuesto** en los conceptos.

Lo operativo del día a día, para la mayoría de las ventas, sigue siendo: datos del receptor exactos + catálogos SAT vigentes (producto, unidad, forma de pago, uso de CFDI).

## ¿Qué datos del receptor son obligatorios en la factura 4.0?

**Respuesta corta:** RFC, nombre o razón social, régimen fiscal, código postal del domicilio fiscal y uso del CFDI. Pídelos en la Constancia de Situación Fiscal, no de memoria ni de una tarjeta de presentación.

El SAT exige que estos datos coincidan con su padrón (la lista de RFC inscritos no cancelados):

- **RFC completo y activo.** Un dígito de más o de menos tumba el timbrado.
- **Nombre o razón social.** Tal como aparece en la CSF / Cédula de Identificación Fiscal: nombres, apellidos, espacios y signos. Si la constancia dice `JUAN CARLOS`, no pongas `JUAN C.`. Para personas morales, el artículo 29-A del CFF indica tomar el nombre de la constancia **sin incorporar el régimen de capital** (por ejemplo, sin `S.A. DE C.V.`).
- **Régimen fiscal.** El que está en la constancia, no “el que siempre usamos”. Tiene que ser compatible con el tipo de persona (física o moral) y con el uso de CFDI.
- **Código postal del domicilio fiscal.** El que el SAT tiene registrado, no el de la sucursal, el de entrega ni el “domicilio actual” si no se actualizó ante el SAT.
- **Uso del CFDI.** Lo elige el receptor (gastos, compras, inversiones, etc.) y debe ser una clave del catálogo `c_UsoCFDI` compatible con su régimen. Si no lo es, el PAC rechaza el timbrado.

La forma más segura de no fallar: pide la Constancia de Situación Fiscal actualizada (o la cédula de datos fiscales) y captura desde ahí.

## ¿Qué errores impiden emitir una factura 4.0?

**Respuesta corta:** cualquier diferencia entre lo que capturas y lo que el SAT tiene en su base. El comprobante no se emite; no “sale mal y luego se cancela”.

Los rechazos más comunes:

- Nombre o razón social distinto al del padrón (abreviaturas, régimen de capital de más, un acento o un espacio que no está en la CSF).
- RFC, régimen o código postal incorrectos, o un CP que el cliente ya cambió en la vida real pero no ante el SAT.
- Uso de CFDI que no aplica al régimen del receptor.
- RFC inactivo o cancelado.
- Catálogos desactualizados (clave de producto, unidad, forma o método de pago que el SAT ya no admite).

Si el cliente te da datos “actuales” que todavía no están en el SAT, la factura 4.0 no pasa. Primero tiene que actualizar su situación fiscal.

## ¿Cómo se cancelan las facturas 4.0?

**Respuesta corta:** eliges un motivo oficial (01–04), envías la solicitud por el portal del SAT o por tu PAC, y en varios casos el receptor debe aceptar. El silencio de 3 días hábiles cuenta como aceptación.

El SAT publica el proceso en su [minisitio de cancelación](https://www.sat.gob.mx/minisitio/Factura/cancela_procesocancelacion.htm). Los motivos son estos (nombres oficiales):

| Clave | Motivo |
| --- | --- |
| 01 | Comprobantes emitidos con errores con relación |
| 02 | Comprobantes emitidos con errores sin relación |
| 03 | No se llevó a cabo la operación |
| 04 | Operación nominativa relacionada en una factura global |

Con el motivo **01** primero emites el CFDI que sustituye (relación tipo 04, “Sustitución de CFDI previos”) y al cancelar indicas el folio (UUID) del nuevo comprobante.

Cuando la cancelación **requiere aceptación**, el receptor recibe un aviso en su Buzón Tributario y tiene **tres días hábiles** para aceptar o rechazar. Si no responde, el SAT la tiene por aceptada y la factura se cancela.

### ¿Cuándo no se pide aceptación del receptor?

El SAT lista supuestos en los que la cancelación es inmediata. Entre los más usados en un comercio:

- CFDI con valor total de **$1,000.00**
- Nómina, egreso o traslado
- Operaciones con **público en general**
- Receptor residente en el extranjero
- Cancelación **dentro del día hábil siguiente** a la expedición

Hay más casos (RIF, retenciones, sector primario, etc.) en la misma página del SAT. El **motivo no decide** si se pide aceptación: lo decide el tipo de comprobante, el monto y la fecha.

El SAT también limita **hasta cuándo** puedes cancelar un CFDI (el CFF lo ata al ejercicio de emisión y a la declaración anual). Confirma el plazo vigente en el portal del SAT o con tu contador antes de dejar comprobantes “para después”.

## ¿Qué tipos de CFDI existen en facturación 4.0?

**Respuesta corta:** los mismos de siempre. La 4.0 no inventó tipos nuevos; cambió las reglas de llenado.

- **Ingreso (I):** la factura típica cuando vendes o prestas un servicio. Puede ser PUE (pago en una sola exhibición) o PPD (parcialidades o diferido).
- **Egreso (E):** devoluciones, descuentos o notas de crédito.
- **Traslado (T):** mueves mercancía sin que haya venta.
- **Pago (P):** complemento de recepción de pagos (REP), cuando la factura original fue PPD o no se liquidó al emitir.
- **Nómina (N):** la que recibe el trabajador por su sueldo.

Para ventas al público en general se usa el RFC genérico `XAXX010101000` y los campos de información global. Para residentes en el extranjero, el RFC genérico `XEXX010101000`.

## Cómo emitir CFDI 4.0 en Shopify (sin hacerlo a mano)

Cuando ya sabes **qué** pide el SAT, el cuello de botella suele ser el proceso: pedir la constancia, capturar sin typos, timbrar, mandar XML/PDF y, si hace falta, cancelar con motivo.

Si vendes en Shopify, [CFDI Express](https://cfdi.express) timbra CFDI 4.0 desde el mismo flujo de la venta:

- **Thank You Page** del checkout: el cliente captura sus datos fiscales al terminar de pagar. Guía: [facturación en la Thank You Page](https://cfdi.express/docs/facturacion-checkout-thank-you-page).
- **POS:** el cajero factura en la confirmación de la venta en tienda. Guía: [facturación en Shopify POS](https://cfdi.express/docs/facturacion-pos-punto-de-venta-shopify).
- **Shopify Flow:** automatizas timbrado, cancelación (motivos 01–04) y factura global al público en general. Guía: [automatizaciones con Flow](https://cfdi.express/docs/uso-shopify-flow-automatizaciones-cfdi-express).
- **Portal de auto-facturación:** el cliente entra con su correo y se factura solo, aunque la venta haya sido en otro canal. Tutorial: [portal en 5 minutos](https://cfdi.express/blog/crea-un-portal-de-auto-facturacion-cfdi-en-5-minutos).

También puedes [instalar la app en Shopify](https://apps.shopify.com/cfdi-express), [agendar una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribir a [hola@cfdi.express](mailto:hola@cfdi.express).

## Preguntas frecuentes

### ¿Qué es CFDI facturas 4.0?

Es el nombre con el que mucha gente busca la **factura electrónica vigente en México**: el CFDI versión 4.0 del SAT. Es el mismo documento que “factura 4.0” o “CFDI 4.0”.

### ¿Sigue existiendo la factura 3.3?

No para emitir. Desde el 1 de abril de 2023 el SAT solo acepta la versión 4.0. Los CFDI 3.3 que se timbraron cuando esa versión era válida siguen existiendo como historial; no puedes emitir unos nuevos.

### ¿Qué necesito para emitir una factura 4.0 sin que la rechacen?

La Constancia de Situación Fiscal del receptor (RFC, nombre, régimen y CP fiscal) y un uso de CFDI compatible con su régimen. Captura esos datos tal cual están en la constancia.

### ¿Cómo cancelo un CFDI 4.0?

Elige el motivo 01, 02, 03 o 04, envía la solicitud en el SAT o en tu PAC y, si aplica, espera la aceptación del receptor (3 días hábiles; el silencio acepta). El detalle está en la [guía de cancelación del SAT](https://www.sat.gob.mx/minisitio/Factura/cancela_procesocancelacion.htm). Si usas CFDI Express, el flujo está en [Cancelación y acuse](https://cfdi.express/docs/cancelacion-y-acuse).

### ¿Toda cancelación pide que el cliente acepte?

No. El SAT exceptúa, entre otros, montos de $1,000, público en general, nómina/egreso/traslado y la cancelación al día hábil siguiente. El resto, como regla general, sí pide aceptación.

### Llegué buscando “todo CFDI 4.0”. ¿Esto es lo que necesito?

Casi siempre esa búsqueda quiere el panorama completo de la **versión 4.0**: qué es, qué cambió y cómo facturar sin errores. Eso es esta guía. Comparar sistemas o PACs es otro artículo.

### ¿Puedo emitir CFDI 4.0 desde Shopify?

Sí. Con CFDI Express lo haces en checkout (Thank You Page), POS, admin, portal de auto-facturación o Flow, con validación de los datos que el SAT exige en 4.0.

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "¿Qué es CFDI facturas 4.0?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "CFDI facturas 4.0 es la versión vigente del Comprobante Fiscal Digital por Internet del SAT. Es el mismo documento que factura 4.0 o CFDI 4.0: desde el 1 de abril de 2023 es la única versión válida para emitir facturas electrónicas en México."
      }
    },
    {
      "@type": "Question",
      "name": "¿Sigue existiendo la factura 3.3?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No para emitir. Desde el 1 de abril de 2023 el SAT solo acepta la versión 4.0. Los CFDI 3.3 timbrados cuando esa versión era válida siguen como historial; no puedes emitir unos nuevos."
      }
    },
    {
      "@type": "Question",
      "name": "¿Qué datos del receptor son obligatorios en la factura 4.0?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "RFC, nombre o razón social, régimen fiscal, código postal del domicilio fiscal y uso del CFDI. Deben coincidir con la Constancia de Situación Fiscal del receptor. Si no coinciden, el SAT rechaza el timbrado."
      }
    },
    {
      "@type": "Question",
      "name": "¿Cómo se cancela un CFDI 4.0?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Debes elegir un motivo oficial del SAT (01 errores con relación, 02 errores sin relación, 03 no se llevó a cabo la operación, 04 operación nominativa en factura global) y enviar la solicitud por el portal del SAT o un PAC. En varios casos el receptor tiene 3 días hábiles para aceptar o rechazar; si no responde, se considera aceptada."
      }
    },
    {
      "@type": "Question",
      "name": "¿Toda cancelación de factura 4.0 necesita aceptación del receptor?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No. El SAT no pide aceptación, entre otros casos, cuando el CFDI es de 1,000 pesos, es de nómina, egreso o traslado, es al público en general, o se cancela dentro del día hábil siguiente a su expedición."
      }
    },
    {
      "@type": "Question",
      "name": "¿Puedo emitir CFDI 4.0 desde Shopify?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Sí. Con CFDI Express puedes timbrar CFDI 4.0 desde la Thank You Page del checkout, el POS, el admin, un portal de auto-facturación o Shopify Flow."
      }
    }
  ]
}
</script>
