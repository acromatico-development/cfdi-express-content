---
title: "Cómo facturar a un cliente extranjero en CFDI 4.0 (XEXX010101000)"
description: "RFC genérico XEXX010101000 en CFDI 4.0: residencia fiscal, tax ID extranjero, uso S01, régimen 616 y el código postal del emisor, con el JSON de la API."
image: "https://assets.acromatico.dev/marketing/diego/blog/hero-cfdi-facturas-4.0.png"
author: "Rafael González"
date: "2026-10-28"
keywords: "XEXX010101000, facturar a cliente extranjero, RFC genérico extranjero, ResidenciaFiscal, NumRegIdTrib, CFDI 4.0 extranjero, residencia fiscal, número de registro de identidad tributaria, CFDI Express API"
---
# Cómo facturar a un cliente extranjero en CFDI 4.0 (XEXX010101000)

<!-- TODO: el hero reutiliza https://assets.acromatico.dev/marketing/diego/blog/hero-cfdi-facturas-4.0.png (el mismo de CFDI facturas 4.0). Imagen propia: prompt en socials/facturar-cliente-extranjero-xexx010101000.md; al subirla, sustituye `image`. -->
<!-- ENDTODO -->

Cuando el cliente no es residente fiscal en México y no tiene RFC mexicano, el CFDI 4.0 usa el RFC genérico **`XEXX010101000`**, la clave de su país y su número de registro de identidad tributaria (tax ID).

Esta guía es el contrato de la [API de CFDI Express](https://cfdi.express/api) **v1.42.0**. El servidor MCP (`https://api.cfdi.express/mcp`) explica el mismo flujo en sus instrucciones. El paso a paso de la app de Shopify está en [Facturación al extranjero](/docs/facturacion-al-extranjero); los campos del receptor, en [receptor extranjero](/docs/facturacion-al-extranjero#receptor-extranjero).

Confirma con tu contador si esta venta lleva ese RFC y cómo va el IVA.

## Qué es el RFC XEXX010101000

**Respuesta corta:** es el RFC genérico del SAT para operaciones con extranjeros.

`XAXX010101000` es público en general. `XEXX010101000` es el genérico de operaciones con extranjeros. Esa diferencia está en [CFDI facturas 4.0](/blog/cfdi-facturas-4-0) y en [público en general](/docs/facturacion-al-publico-en-general-manual-y-automatica).

## Cuándo aplica

**Respuesta corta:** cuando el cliente no es residente fiscal en México y no tiene RFC mexicano.

La [guía de la app](/docs/facturacion-al-extranjero) documenta ese caso con IVA 0% por línea. Si tu contador clasifica la operación de otra forma, sigue su criterio antes de timbrar.

## Qué datos pedir al cliente

1. **Nombre o razón social**, como debe ir en el receptor.
2. **País de residencia fiscal:** clave de 3 letras del catálogo `c_Pais` del SAT. Ejemplo: `ARG`. Nunca `MEX`.
3. **Tax ID** de ese país: de 1 a 40 caracteres.

En el CFDI, el código postal del receptor es el **código postal del emisor**, no el de la dirección del cliente en el extranjero. El uso de CFDI es `S01` y el régimen fiscal del receptor es `616`. Es la misma terna que con `XAXX010101000` (CFDI40149).

## ResidenciaFiscal y NumRegIdTrib

| JSON | Atributo en el XML |
| --- | --- |
| `receiver.residenciaFiscal` | `cfdi:Receptor@ResidenciaFiscal` |
| `receiver.numRegIdTrib` | `cfdi:Receptor@NumRegIdTrib` |

`residenciaFiscal` es la clave de `c_Pais`. La API la pasa a mayúsculas y solo valida que sean 3 letras. La pertenencia al catálogo no se valida en la API.

`numRegIdTrib` es el tax ID (1 a 40 caracteres). Si lo mandas, `residenciaFiscal` es obligatorio (CFDI40153).

Esos atributos se emiten en la factura, en la nota de crédito y en el REP.

- En la **nota de crédito** se copian de la factura relacionada. En la app: [Notas de crédito](/docs/notas-de-credito).
- En el **REP** también se emiten, y se imprimen en su PDF. En la app: [Complementos de pago](/docs/complementos-de-pago).
- En el **PDF de la factura** también se imprimen.

Si el comprobante ya se timbró con el receptor mal, la cancelación y el acuse están en [Cancelación y acuse](/docs/cancelacion-y-acuse).

## Requisitos: S01, 616 y el CP del emisor

Con `XEXX010101000` la API exige lo mismo que con `XAXX010101000`:

- `usoCfdi`: `S01`
- `receiver.regimenFiscal`: `616`
- `receiver.zip`: el código postal del emisor

Esa regla es CFDI40149. Una factura o una nota de crédito a ese RFC sin alguno de los tres responde **400**.

## Ejemplo con la API

El receptor va en el JSON de `POST /v1/invoices`. `ARG` y `30715436228` son el ejemplo de país y de tax ID. Sustituye el nombre y escribe el código postal de tu emisor.

```json
{
  "usoCfdi": "S01",
  "receiver": {
    "rfc": "XEXX010101000",
    "name": "<nombre o razón social>",
    "zip": "<código postal del emisor>",
    "regimenFiscal": "616",
    "residenciaFiscal": "ARG",
    "numRegIdTrib": "30715436228"
  }
}
```

El resto del cuerpo (emisor, conceptos, forma y método de pago) es el de cualquier factura. La referencia interactiva está en [api.cfdi.express/docs](https://api.cfdi.express/docs). Para probar, una llave `sk_test_` en [dash.cfdi.express](https://dash.cfdi.express). El contexto de la API está en [el anuncio de lanzamiento](/blog/lanzamiento-cfdi-express-api).

Las instrucciones del [servidor MCP](https://api.cfdi.express/mcp) describen este flujo de cliente extranjero. El sandbox del MCP es `https://api.cfdi.express/mcp/test`.

## Cómo se ve en CFDI Express

En el XML, `cfdi:Receptor` lleva `ResidenciaFiscal` y `NumRegIdTrib` junto al RFC `XEXX010101000`.

Esos datos se imprimen en el PDF de la factura y en el PDF del REP. En la nota de crédito, los atributos van en el XML porque se copian de la factura relacionada.

En la app de Shopify, la guía publicada cubre el botón **Facturar al Público en General Extranjero** (prellena el RFC `XEXX010101000`, la razón social y el régimen, el código postal y el uso de CFDI que correspondan) y los checkboxes de **IVA 0%** por línea. Eso está en [Facturación al extranjero](/docs/facturacion-al-extranjero). `residenciaFiscal` y `numRegIdTrib` de este artículo son los campos de la API v1.42.0: [receptor extranjero](/docs/facturacion-al-extranjero#receptor-extranjero).

## Errores comunes

| Qué mandaste | Qué pasa |
| --- | --- |
| `residenciaFiscal` o `numRegIdTrib` con un RFC distinto de `XEXX010101000` | **400**. Esos campos solo van con ese RFC (CFDI40151). |
| `numRegIdTrib` sin `residenciaFiscal` | Validación antes de timbrar (CFDI40153). |
| `residenciaFiscal` en `MEX` | Validación antes de timbrar. Nunca `MEX` (CFDI40152). |
| Factura o nota de crédito a `XEXX010101000` sin `S01`, sin `616` o sin el CP del emisor | **400**. La regla es CFDI40149. |
| Clave de 3 letras que no está en `c_Pais` | La API no valida la pertenencia al catálogo. |
| Tax ID con un formato que ese país no usa | El formato por país lo valida el PAC (CFDI40156). |

## Preguntas frecuentes

**¿XEXX010101000 y XAXX010101000 sirven para el mismo caso?**
`XAXX010101000` es público en general. `XEXX010101000` es el genérico de operaciones con extranjeros.

**¿Qué le pido al cliente?**
Nombre, clave de país (`c_Pais`, 3 letras, nunca `MEX`) y su tax ID (1 a 40 caracteres). En el CFDI el código postal del receptor es el de tu emisor, el uso es `S01` y el régimen es `616`.

**¿La nota de crédito y el REP llevan la residencia fiscal?**
Sí. En la nota de crédito, `ResidenciaFiscal` y `NumRegIdTrib` se copian de la factura relacionada. En el REP también se emiten. El PDF de la factura y el PDF del REP los imprimen.

**¿`MEX` cabe en residencia fiscal?**
`residenciaFiscal` nunca es `MEX` (CFDI40152).

**¿La API rechaza un país fuera de c_Pais?**
Valida que la clave tenga 3 letras. La pertenencia a `c_Pais` no se valida en la API. El formato del tax ID por país lo valida el PAC (CFDI40156).

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {"@type": "Question", "name": "¿XEXX010101000 y XAXX010101000 sirven para el mismo caso?", "acceptedAnswer": {"@type": "Answer", "text": "XAXX010101000 es público en general. XEXX010101000 es el RFC genérico de operaciones con extranjeros."}},
    {"@type": "Question", "name": "¿Qué le pido al cliente?", "acceptedAnswer": {"@type": "Answer", "text": "Nombre, clave de país del catálogo c_Pais (3 letras, nunca MEX) y su tax ID (1 a 40 caracteres). En el CFDI el código postal del receptor es el del emisor, el uso es S01 y el régimen es 616."}},
    {"@type": "Question", "name": "¿La nota de crédito y el REP llevan la residencia fiscal?", "acceptedAnswer": {"@type": "Answer", "text": "Sí. En la nota de crédito, ResidenciaFiscal y NumRegIdTrib se copian de la factura relacionada. En el REP también se emiten. El PDF de la factura y el PDF del REP los imprimen."}},
    {"@type": "Question", "name": "¿MEX cabe en residencia fiscal?", "acceptedAnswer": {"@type": "Answer", "text": "residenciaFiscal nunca es MEX (CFDI40152)."}},
    {"@type": "Question", "name": "¿La API rechaza un país fuera de c_Pais?", "acceptedAnswer": {"@type": "Answer", "text": "Valida que la clave tenga 3 letras. La pertenencia a c_Pais no se valida en la API. El formato del tax ID por país lo valida el PAC (CFDI40156)."}}
  ]
}
</script>

## Timbrar a un cliente extranjero

1. Reúne nombre, clave de país y tax ID. Arma el receptor con `XEXX010101000`, `S01`, `616` y el código postal de tu emisor.
2. Contrasta el JSON con [receptor extranjero](/docs/facturacion-al-extranjero#receptor-extranjero).
3. Timbra por la [API](https://cfdi.express/api) o pídeselo al [MCP](https://api.cfdi.express/mcp).
4. Si vendes en Shopify, sigue [Facturación al extranjero](/docs/facturacion-al-extranjero). La app está en [Shopify](https://apps.shopify.com/cfdi-express).

¿Dudas de integración? [Agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).
