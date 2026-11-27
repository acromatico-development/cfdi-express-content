---
title: "Errores CFDI40143 a CFDI40148: qué significan"
description: "Qué significan CFDI40143 a CFDI40148, cómo corregir RFC, nombre y CP del receptor con la constancia, y cómo validarlos antes de timbrar, sin costo."
image: "https://videos.acromatico.dev/api/images/assets/07b58356-303a-4107-9a46-b6c67e0b4afb.png"
author: "Rafael González"
date: "2026-11-26"
keywords: "CFDI40143, CFDI40144, CFDI40145, CFDI40146, CFDI40147, CFDI40148, nombre del receptor, régimen fiscal receptor, código postal receptor, DomicilioFiscalReceptor, validar receptor, Constancia de Situación Fiscal, RFC inscritos no cancelados, PUBLICO EN GENERAL, validate_receiver, POST /v1/receivers/validate, CFDI Express API"
---
# Errores CFDI40143 a CFDI40148: qué significan y cómo evitarlos validando al receptor

![A un lado, el error del SAT CFDI40145 sobre el nombre del receptor; al otro, el mensaje claro de CFDI Express que pide copiar el nombre de la constancia.](https://videos.acromatico.dev/api/images/assets/b40bcd98-8e31-4ff5-9472-2363f5bf628b.png?format=webp)

**Respuesta corta:** CFDI40143 a CFDI40148 son rechazos del SAT al RFC, el nombre o el código postal del receptor, o al usar «PUBLICO EN GENERAL» con un RFC distinto de XAXX010101000. Se corrigen con los datos de la Constancia de Situación Fiscal. `POST /v1/receivers/validate` y la herramienta MCP `validate_receiver` revisan formato, catálogos, régimen, uso de CFDI y las reglas del RFC genérico antes de timbrar: no timbran, no cobran y no guardan nada. No consultan el padrón del SAT, así que CFDI40143, CFDI40144, CFDI40145, CFDI40147 y CFDI40148 solo se confirman al timbrar; si el PAC los rechaza, la API devuelve el código y qué corregir, en español.

Estos códigos salen de la [Matriz de errores del CFDI 4.0](http://omawww.sat.gob.mx/tramitesyservicios/Paginas/documentos/MatrizDeErrores_CFDI_v40_20260720.xls), el archivo Excel vigente que el SAT publica en su página de [Factura electrónica](https://www.sat.gob.mx/minisitio/Factura/emite_quenecesitoparafacturar.htm) y en la documentación técnica del [Anexo 20](http://omawww.sat.gob.mx/tramitesyservicios/Paginas/anexo_20.htm). El texto de cada código, más abajo, está citado de esa matriz.

Esta guía es de la [API de CFDI Express](https://cfdi.express/api) (v1.60.0): `https://api.cfdi.express`. El timbre cuesta [$1 MXN, sin mensualidad](https://cfdi.express/api). La validación del receptor no se cobra. El paso a paso está en [Validar los datos fiscales del receptor](/docs/validar-datos-del-receptor). El contrato de timbrado está en [Timbrar una factura](/docs/api/timbrar-factura).

## CFDI40143: el RFC no está en la lista del SAT

**Atributo:** `Rfc` del nodo `cfdi:Receptor`.

**Validación (matriz del SAT):** «Cuando no se utilice un RFC genérico, el RFC debe estar en la lista de RFC inscritos no cancelados en el SAT.»

**Mensaje oficial:** «Este RFC del receptor no existe en la lista de RFC inscritos no cancelados del SAT.»

**Causa típica:** un carácter cambiado en la homoclave, un RFC que nunca se inscribió o un RFC cancelado. Conviene revisar el RFC antes que el nombre o el código postal, porque esos dos se comparan contra lo registrado para ese RFC.

**Cómo corregirlo:** copia el RFC de la Constancia de Situación Fiscal del cliente, carácter por carácter. Si no puede confirmarlo, no inventes uno: usa el RFC genérico que corresponda (XAXX010101000 o, si es extranjero, XEXX010101000) y sus reglas. Puedes contrastarlo en [Valida en línea RFC's](https://www.sat.gob.mx/aplicacion/79615/valida-en-linea-rfc%C2%B4s-uno-a-uno-o-de-manera-masiva-hasta-5-mil-registros), el validador oficial del SAT.

`POST /v1/receivers/validate` sí revisa que el RFC cumpla el patrón del Anexo 20, pero no consulta esa lista. Un `valid: true` no garantiza que el SAT acepte el RFC al timbrar.

## CFDI40144: el nombre no aparece en la lista del SAT

**Atributo:** `Nombre` del nodo `cfdi:Receptor`.

**Validación (matriz del SAT):** «Este atributo, debe encontrarse en la lista de RFC inscritos no cancelados en el SAT.»

**Mensaje oficial:** «El campo Nombre del receptor, debe encontrarse en la lista de RFC inscritos no cancelados en el SAT.»

**Causa típica:** el nombre no está, o no se reconoce, en la lista asociada a los RFC inscritos no cancelados. Suele ir junto a un nombre mal capturado o a un RFC que no corresponde.

**Cómo corregirlo:** usa la denominación o razón social tal como viene en la constancia, en el campo «Denominación/Razón Social», sin el régimen de capital (por ejemplo, sin «S.A. de C.V.», que la constancia muestra aparte).

## CFDI40145: el nombre no corresponde a ese RFC

**Atributo:** `Nombre` del nodo `cfdi:Receptor`.

**Validación (matriz del SAT):** «Este atributo, debe pertenecer al nombre asociado al RFC registrado en el atributo Rfc del Nodo Receptor.»

**Mensaje oficial:** «El campo Nombre del receptor, debe pertenecer al nombre asociado al RFC registrado en el campo Rfc del Receptor.»

**Causa típica:** el RFC sí existe, pero el nombre no es el que el SAT tiene registrado para ese RFC. Un acento de menos, un espacio de más o haber incluido el régimen de capital son causas habituales.

**Cómo corregirlo:** igual que en el CFDI40144: cópialo letra por letra de la constancia y deja fuera el régimen de capital. En la API, si el RFC es de persona moral y el nombre termina en un régimen de capital («SA DE CV», «S DE RL DE CV», «SAPI», «SC», «AC», etc.), `POST /v1/receivers/validate` y el timbrado lo bloquean antes de gastar un timbre, con `code` `name_societary_suffix`. No lo recortan por su cuenta: hay que corregirlo. Las mayúsculas sí se normalizan solas.

## CFDI40146: «PUBLICO EN GENERAL» solo con XAXX010101000

**Atributo:** `Nombre` del nodo `cfdi:Receptor`.

**Validación (matriz del SAT):** «Si el valor registrado en este atributo es “PUBLICO EN GENERAL”, el valor del atributo RFC del receptor debe ser “XAXX010101000”.»

**Mensaje oficial:** «El campo RFC del receptor debe contener el valor "XAXX010101000".»

**Causa típica:** el nombre va como «PUBLICO EN GENERAL» y el RFC es el de un cliente, o no es el RFC genérico nacional.

**Cómo corregirlo:** ese nombre solo se usa con el RFC XAXX010101000 (factura global al público en general). Si facturas a un cliente nominativo, usa su RFC y el nombre de su constancia, no «PUBLICO EN GENERAL». El flujo de la factura global está en [Cómo hacer la factura global en Shopify](/blog/factura-global-shopify) y en la guía [Facturación global mensual](/docs/facturacion-global-mensual).

Esta sí la puede atrapar `POST /v1/receivers/validate` antes de timbrar, porque es una regla local del RFC genérico (junto con el régimen 616, el uso S01 y el código postal del lugar de expedición, CFDI40149 y CFDI40159). El caso del cliente extranjero, con XEXX010101000, está en [Cómo facturar a un cliente extranjero](/blog/facturar-cliente-extranjero-xexx010101000).

## CFDI40147: el código postal no aparece en la lista del SAT

**Atributo:** `DomicilioFiscalReceptor` del nodo `cfdi:Receptor`. En la API ese dato es `receiver.zip` (o `customer.zip` en la prevalidación). Son los cinco dígitos del domicilio fiscal, no una calle.

**Validación (matriz del SAT):** «Este atributo, debe encontrarse en la lista de RFC inscritos no cancelados en el SAT.»

**Mensaje oficial:** «El campo DomicilioFiscalReceptor del receptor, debe encontrarse en la lista de RFC inscritos no cancelados en el SAT.»

**Causa típica:** un código postal que no se reconoce en esa lista, a menudo porque el RFC está mal o el CP no es el fiscal.

**Cómo corregirlo:** usa el código postal del domicilio fiscal que viene en la constancia del cliente, no el de entrega, el de una sucursal o el de un local.

## CFDI40148: el código postal no corresponde a ese RFC

**Atributo:** `DomicilioFiscalReceptor` del nodo `cfdi:Receptor`.

**Validación (matriz del SAT):** «Este atributo, debe pertenecer al nombre asociado al RFC registrado en el atributo Rfc del Nodo Receptor.»

**Mensaje oficial:** «El campo DomicilioFiscalReceptor del receptor, debe pertenecer al nombre asociado al RFC registrado en el campo Rfc del Receptor.»

El SAT reutiliza en la matriz la misma redacción del nombre. En la práctica, el campo es el código postal de cinco dígitos del domicilio fiscal, y el rechazo significa que ese CP no es el registrado para ese RFC.

**Causa típica:** capturar el CP de entrega, de una sucursal o del local, en lugar del fiscal.

**Cómo corregirlo:** el dato correcto está en la Constancia de Situación Fiscal del cliente. `POST /v1/receivers/validate` sí revisa que el CP tenga cinco dígitos, pero no comprueba que coincida con el padrón.

## Cómo evitarlos validando al receptor antes de timbrar

Desde la v1.60.0, `POST /v1/receivers/validate` y la herramienta MCP `validate_receiver` corren el mismo validador local que `POST /v1/invoices` y [`POST /v1/invoices/preview`](/docs/api/vista-previa-factura). No timbran, no cobran y no guardan nada.

`merchantId` es obligatorio: su código postal es el lugar de expedición que deben llevar los RFC genéricos (CFDI40149). Si el emisor no existe, la respuesta es 404.

**Sí revisa:** el formato del RFC (patrón del Anexo 20 y tipo de persona por la longitud), que el CP tenga cinco dígitos, que el régimen esté en `c_RegimenFiscal` y corresponda al tipo de persona, que el uso de CFDI esté en `c_UsoCFDI` y sea compatible con el régimen y el tipo de persona, que el nombre de una persona moral no termine en régimen de capital, y las reglas de XAXX010101000 / XEXX010101000, incluido el CFDI40146. Los catálogos están en [regímenes fiscales](/docs/api/catalogo-regimenes) y [usos de CFDI](/docs/api/catalogo-usos-cfdi).

**No revisa** (solo lo confirma el validador oficial del SAT o el timbrado): que el RFC exista y esté activo en la lista de RFC inscritos no cancelados (CFDI40143), que el nombre coincida con el registrado para ese RFC (CFDI40144 y CFDI40145), que el CP coincida con el domicilio fiscal registrado (CFDI40147 y CFDI40148), ni que el régimen sea uno de los que el contribuyente tiene dados de alta. Un `valid: true` significa que pasó las reglas locales, no que los datos coincidan con el padrón.

Los campos del body no son los de `POST /v1/invoices` (`receiver.*` y `usoCfdi`). En este endpoint van así; el contrato está en la [referencia de Validar receptor](/docs/api/validar-receptor):

```bash
curl -X POST https://api.cfdi.express/v1/receivers/validate \
  -H "Authorization: Bearer sk_test_..." \
  -H "Content-Type: application/json" \
  -d '{
    "merchantId": "mkq2f8v3x1t7p9d4n6c0b5rz",
    "customer": {
      "taxId": "EKU9003173C9",
      "legalName": "Escuela Kemper Urgate",
      "taxRegime": "601",
      "zip": "42501"
    },
    "usage": "G03"
  }'
```

Si pasa, la respuesta es **200** con `valid: true`, los datos normalizados en `normalized` (el nombre queda en mayúsculas y sin espacios sobrantes) y dos listas: `checked` y `notChecked`, más un `notice` que recuerda lo que no se comparó con el padrón. El ejemplo es el de esa referencia: una persona moral de pruebas del SAT, con la razón social en minúsculas; `normalized.customer.legalName` queda `ESCUELA KEMPER URGATE`.

Si no pasa, la respuesta es **400** `validation_error` con `subcode` `invalid_receiver`. `errors[]` trae `path` (`customer.taxId`, `customer.legalName`, `customer.taxRegime`, `customer.zip` o `usage`), `message` en español —si hay clave de la matriz, el texto empieza con ella—, `satCode` cuando aplica, y un `code` estable de la regla. También vienen `checked`, `notChecked` y `notice`. Para los mismos datos, `message`, `satCode`, `code` y `detail` son idénticos a los del 400 de `POST /v1/invoices` (allá con paths `receiver.*` y `usoCfdi`).

Por MCP, `validate_receiver` recibe los mismos argumentos (`merchantId`, `customer`, `usage`) y lista lo revisado y lo no revisado, tanto si los datos pasan como si no. El servidor está en [la guía MCP](/docs/api/mcp) y en [cfdi.express/skill](https://cfdi.express/skill).

Cuando el PAC rechaza el timbrado con CFDI40143, CFDI40144, CFDI40145, CFDI40147 o CFDI40148, la API responde **422** `sat_rejected`. El `detail` va en español, empieza con la clave y dice qué corregir (revisar la constancia). `satRule` es la clave; `field` apunta a `receiver.rfc`, `receiver.name` o `receiver.zip`.

La forma de evitar esos cinco es capturar RFC, nombre y código postal tal cual están en la constancia de situación fiscal del cliente, con fecha reciente, y contrastarlos en [Valida en línea RFC's](https://www.sat.gob.mx/aplicacion/79615/valida-en-linea-rfc%C2%B4s-uno-a-uno-o-de-manera-masiva-hasta-5-mil-registros) si hace falta: esa herramienta revisa RFC, nombre y código postal, pide un captcha y no se puede automatizar. La prevalidación te ahorra un viaje al PAC cuando el problema es de formato, de catálogo, de régimen de capital o de RFC genérico, y no gasta saldo.

## Preguntas frecuentes

**¿Qué significan los errores CFDI40143 a CFDI40148?**
Son rechazos del SAT al receptor en un CFDI 4.0. CFDI40143: el RFC no está en la lista de RFC inscritos no cancelados. CFDI40144 y CFDI40145: el nombre no está en esa lista o no corresponde a ese RFC. CFDI40146: el nombre «PUBLICO EN GENERAL» solo va con el RFC XAXX010101000. CFDI40147 y CFDI40148: el código postal del domicilio fiscal no está en esa lista o no corresponde a ese RFC. El texto oficial está en la Matriz de errores del CFDI 4.0 del SAT.

**¿Cómo se corrige un CFDI40145?**
Copia la denominación o razón social de la Constancia de Situación Fiscal, letra por letra, y deja fuera el régimen de capital (por ejemplo, «S.A. de C.V.»). Un acento, un espacio o el sufijo societario bastan para que el SAT lo rechace.

**¿POST /v1/receivers/validate evita los errores CFDI40143 a CFDI40148?**
Evita el CFDI40146 y otros rechazos locales (formato, catálogos, régimen, uso de CFDI y sufijo societario) sin timbrar ni cobrar. No consulta el padrón del SAT, así que CFDI40143, CFDI40144, CFDI40145, CFDI40147 y CFDI40148 pueden aparecer igual al timbrar, como 422. Esos se evitan copiando RFC, nombre y código postal de la constancia.

**¿La validación del receptor tiene costo?**
No. POST /v1/receivers/validate y validate_receiver no timbran, no cobran y no guardan nada. El timbre de la API cuesta $1 MXN, sin mensualidad. Si el SAT rechaza al timbrar, el reembolso del timbre es automático.

**¿Qué hago si el SAT rechaza con CFDI40143 o CFDI40148 al timbrar?**
La API responde 422 sat_rejected con la clave y un texto en español que indica el campo y qué revisar. Corrige el dato con la constancia del cliente y vuelve a timbrar. Para el RFC, también puedes usar el validador de RFC del SAT.

**¿Puedo validar al receptor desde un agente?**
Sí. La herramienta MCP validate_receiver recibe los mismos argumentos que el endpoint: merchantId, customer y usage. En la respuesta lista lo que sí revisó y lo que no, haya pasado o no la validación local.

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {"@type": "Question", "name": "¿Qué significan los errores CFDI40143 a CFDI40148?", "acceptedAnswer": {"@type": "Answer", "text": "Son rechazos del SAT al receptor en un CFDI 4.0. CFDI40143: el RFC no está en la lista de RFC inscritos no cancelados. CFDI40144 y CFDI40145: el nombre no está en esa lista o no corresponde a ese RFC. CFDI40146: el nombre «PUBLICO EN GENERAL» solo va con el RFC XAXX010101000. CFDI40147 y CFDI40148: el código postal del domicilio fiscal no está en esa lista o no corresponde a ese RFC. El texto oficial está en la Matriz de errores del CFDI 4.0 del SAT."}},
    {"@type": "Question", "name": "¿Cómo se corrige un CFDI40145?", "acceptedAnswer": {"@type": "Answer", "text": "Copia la denominación o razón social de la Constancia de Situación Fiscal, letra por letra, y deja fuera el régimen de capital (por ejemplo, «S.A. de C.V.»). Un acento, un espacio o el sufijo societario bastan para que el SAT lo rechace."}},
    {"@type": "Question", "name": "¿POST /v1/receivers/validate evita los errores CFDI40143 a CFDI40148?", "acceptedAnswer": {"@type": "Answer", "text": "Evita el CFDI40146 y otros rechazos locales (formato, catálogos, régimen, uso de CFDI y sufijo societario) sin timbrar ni cobrar. No consulta el padrón del SAT, así que CFDI40143, CFDI40144, CFDI40145, CFDI40147 y CFDI40148 pueden aparecer igual al timbrar, como 422. Esos se evitan copiando RFC, nombre y código postal de la constancia."}},
    {"@type": "Question", "name": "¿La validación del receptor tiene costo?", "acceptedAnswer": {"@type": "Answer", "text": "No. POST /v1/receivers/validate y validate_receiver no timbran, no cobran y no guardan nada. El timbre de la API cuesta $1 MXN, sin mensualidad. Si el SAT rechaza al timbrar, el reembolso del timbre es automático."}},
    {"@type": "Question", "name": "¿Qué hago si el SAT rechaza con CFDI40143 o CFDI40148 al timbrar?", "acceptedAnswer": {"@type": "Answer", "text": "La API responde 422 sat_rejected con la clave y un texto en español que indica el campo y qué revisar. Corrige el dato con la constancia del cliente y vuelve a timbrar. Para el RFC, también puedes usar el validador de RFC del SAT."}},
    {"@type": "Question", "name": "¿Puedo validar al receptor desde un agente?", "acceptedAnswer": {"@type": "Answer", "text": "Sí. La herramienta MCP validate_receiver recibe los mismos argumentos que el endpoint: merchantId, customer y usage. En la respuesta lista lo que sí revisó y lo que no, haya pasado o no la validación local."}}
  ]
}
</script>

## Valida el receptor y luego timbra

1. Entra a [dash.cfdi.express](https://dash.cfdi.express) y prueba con una llave `sk_test_`.
2. Sigue [Validar los datos fiscales del receptor](/docs/validar-datos-del-receptor): llama a `POST /v1/receivers/validate` (o pídele a tu agente que use `validate_receiver`) con el RFC, el nombre, el régimen, el CP y el uso de CFDI de la constancia, con fecha reciente. Revisa `checked` y `notChecked`.
3. Si pasa, timbra con [`POST /v1/invoices`](/docs/api/timbrar-factura). Si el PAC rechaza con CFDI40143, CFDI40144, CFDI40145, CFDI40147 o CFDI40148, el 422 ya trae la clave y qué corregir, en español.

El paso a paso está en [Validar los datos fiscales del receptor](/docs/validar-datos-del-receptor). El contrato del endpoint está en [Validar receptor](/docs/api/validar-receptor). El contexto de la API está en la [landing](https://cfdi.express/api).

¿Dudas de integración? [Agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).
