---
title: "Validar los datos fiscales del receptor antes de timbrar (API y MCP)"
description: "Revisa el RFC, la razón social, el régimen, el código postal y el uso de CFDI de un cliente antes de darlo de alta o timbrar, con POST /v1/receivers/validate o la herramienta MCP validate_receiver de CFDI Express."
keywords: "validar RFC, validar receptor, datos fiscales del cliente, régimen fiscal, código postal, uso de CFDI, SA DE CV razón social, CFDI40157, CFDI40158, CFDI40144, CFDI40145, constancia de situación fiscal, POST /v1/receivers/validate, validate_receiver, CFDI Express API, MCP"
---
# Validar los datos fiscales del receptor antes de timbrar

URL de esta página: https://cfdi.express/docs/validar-datos-del-receptor

Disponible a partir de la versión **v1.60.0** de la API.

`POST /v1/receivers/validate` revisa los datos fiscales de un cliente —RFC, nombre o razón social, régimen fiscal, código postal y uso de CFDI— **antes** de darlo de alta en tu sistema o de timbrarle una factura. Por MCP, la herramienta es `validate_receiver`.

- **No timbra, no cobra y no guarda nada.**
- **Usa el mismo validador que el timbrado.** `POST /v1/invoices` aplica exactamente estas reglas antes de timbrar y responde el mismo error. Si la prevalidación pasa, la factura no falla después por una de estas reglas.
- **No compara contra el padrón del SAT.** Revisa formatos y catálogos del SAT, no que los datos coincidan con lo que el SAT tiene registrado. El porqué está más abajo, en «Lo que no se revisa».

> `POST https://api.cfdi.express/v1/receivers/validate`
> Autenticación: `Authorization: Bearer sk_test_…` o `sk_live_…`. En un workspace, el header `x-account-id` indica la cuenta.

## Cuándo usarla

- En el formulario donde tu cliente captura sus datos fiscales, para marcar el campo con error antes de guardarlo.
- Al dar de alta un cliente en tu ERP o CRM.
- Desde tu agente de IA, cuando le pasas los datos de un cliente nuevo y quieres saber si están bien antes de facturar.

No es obligatoria: puedes timbrar directo, y `POST /v1/invoices` valida igual.

## Request

| Campo | Tipo | Requerido | Descripción |
|---|---|---|---|
| `merchantId` | string | sí | El comercio emisor. Su código postal es el lugar de expedición que deben llevar los RFC genéricos (CFDI40149). Si no existe en tu cuenta, la respuesta es **404**. |
| `customer.taxId` | string | sí | RFC del cliente: 12 caracteres para persona moral, 13 para persona física. `XAXX010101000` para público en general. |
| `customer.legalName` | string | sí | Nombre o razón social como aparece en la constancia de situación fiscal. Se pasa a mayúsculas solo. |
| `customer.taxRegime` | string | sí | Régimen fiscal, clave de 3 dígitos de `c_RegimenFiscal` (por ejemplo `601`, `612`, `626`). |
| `customer.zip` | string | sí | Código postal del domicilio fiscal, 5 dígitos. |
| `usage` | string | sí | Uso de CFDI, clave de `c_UsoCFDI` (por ejemplo `G01`, `G03`, `S01`). |

Sin `merchantId`, `customer` o `usage`, la respuesta es el 400 genérico de esquema, no `invalid_receiver`.

### Los nombres de los campos no son los de /v1/invoices

Este endpoint usa `customer.*` y `usage`. `POST /v1/invoices` y `POST /v1/credit_notes` usan `receiver.*` y `usoCfdi`. Los `path` de los errores siguen el nombre del cuerpo de cada endpoint:

| Dato | `POST /v1/receivers/validate` | `POST /v1/invoices`, `POST /v1/credit_notes` |
|---|---|---|
| RFC | `customer.taxId` | `receiver.rfc` |
| Nombre o razón social | `customer.legalName` | `receiver.name` |
| Régimen fiscal | `customer.taxRegime` | `receiver.regimenFiscal` |
| Código postal | `customer.zip` | `receiver.zip` |
| Uso de CFDI | `usage` | `usoCfdi` |

Para los mismos datos, `message`, `satCode`, `code` y `detail` son idénticos en los dos endpoints; solo cambia `path`.

## Ejemplo: datos que pasan

Una persona moral de pruebas del SAT, con la razón social escrita en minúsculas:

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

### 200 OK

| Campo | Tipo | Descripción |
|---|---|---|
| `valid` | boolean | Siempre `true` en un 200. Significa que pasó todas las revisiones locales, **no** que los datos coincidan con el SAT. |
| `normalized` | object | Los datos como se timbrarían: `customer` (`taxId`, `legalName`, `taxRegime`, `zip`) y `usage`, sin espacios sobrantes y en mayúsculas. Guarda este `legalName`. |
| `checked` | array | Lo que se revisó. Cada elemento es `{ check, description }`. |
| `notChecked` | array | Lo que **no** se revisó. Cada elemento es `{ check, description }`. |
| `notice` | string | El aviso de que los datos no se compararon con el padrón del SAT. |

```json
{
  "valid": true,
  "normalized": {
    "customer": {
      "taxId": "EKU9003173C9",
      "legalName": "ESCUELA KEMPER URGATE",
      "taxRegime": "601",
      "zip": "42501"
    },
    "usage": "G03"
  },
  "checked": [
    { "check": "rfc_format", "description": "Formato del RFC (patrón del Anexo 20: 3-4 letras, fecha AAMMDD válida y homoclave) y tipo de persona por su longitud (12 = moral, 13 = física)." },
    { "check": "zip_format", "description": "Código postal de 5 dígitos." },
    { "check": "regimen_catalog", "description": "Régimen fiscal en el catálogo c_RegimenFiscal del SAT (CFDI40157)." },
    { "check": "regimen_persona", "description": "Régimen compatible con persona física o moral según la longitud del RFC (CFDI40158)." },
    { "check": "regimen_uso_cfdi", "description": "Uso de CFDI en c_UsoCFDI y compatible con el régimen y el tipo de persona del receptor (CFDI40160, CFDI40161)." },
    { "check": "name_uppercase", "description": "Nombre o razón social en mayúsculas (se normaliza automáticamente)." },
    { "check": "name_societary_suffix", "description": "Nombre de persona moral sin régimen de capital (SA DE CV, S DE RL DE CV, …): si lo trae se bloquea; no se quita solo." },
    { "check": "generic_rfc", "description": "Reglas de RFC genéricos XAXX010101000 / XEXX010101000: régimen 616, uso S01, CP = lugar de expedición del emisor y nombre PUBLICO EN GENERAL solo con XAXX010101000 (CFDI40146, CFDI40149, CFDI40159)." }
  ],
  "notChecked": [
    { "check": "lrfc_exists", "description": "Que el RFC exista y esté activo en la lista de RFC inscritos no cancelados (LRFC / padrón) del SAT (CFDI40143)." },
    { "check": "name_matches_sat", "description": "Que el nombre o razón social coincida con el registrado en el SAT para ese RFC (CFDI40144, CFDI40145)." },
    { "check": "zip_matches_sat", "description": "Que el código postal coincida con el domicilio fiscal registrado en el SAT (CFDI40147, CFDI40148)." },
    { "check": "regimen_matches_sat", "description": "Que el régimen fiscal sea uno de los que el contribuyente tiene registrados en el SAT." }
  ],
  "notice": "Estos datos no se comparan con el padrón del SAT: no existe una API pública para hacerlo. Solo el validador oficial del SAT (o el timbrado) puede confirmar que el RFC exista y que el nombre, el código postal y el régimen coincidan con sus registros. Compáralos con la constancia de situación fiscal del cliente."
}
```

Una persona física de pruebas del SAT también pasa:

```json
{
  "merchantId": "mkq2f8v3x1t7p9d4n6c0b5rz",
  "customer": {
    "taxId": "CACX7605101P8",
    "legalName": "XOCHILT CASAS CHAVEZ",
    "taxRegime": "612",
    "zip": "36257"
  },
  "usage": "G03"
}
```

## Lo que sí se revisa

Todo se revisa localmente, contra los catálogos del SAT para CFDI 4.0. Son los `check` de `checked`:

| `check` | Qué revisa | Clave del SAT |
|---|---|---|
| `rfc_format` | RFC con el patrón del Anexo 20: 3 o 4 letras, una fecha AAMMDD válida y una homoclave de 3 caracteres. El tipo de persona sale de la longitud: 12 = moral, 13 = física. | — |
| `zip_format` | Código postal de 5 dígitos. | — |
| `regimen_catalog` | El régimen está entre las claves de `c_RegimenFiscal` que se aceptan para el receptor en CFDI 4.0: 601, 603, 605, 606, 607, 608, 610, 611, 612, 614, 615, 616, 620, 621, 622, 623, 624, 625 y 626. Desde la v1.60.0, 609, 628, 629 y 630 ya no se aceptan. | CFDI40157 |
| `regimen_persona` | El régimen aplica al tipo de persona del RFC. | CFDI40158 |
| `regimen_uso_cfdi` | El uso existe en `c_UsoCFDI` y aplica al tipo de persona y al régimen. `S01` se acepta para personas físicas y morales. `CP01` y `CN01` no se aceptan en facturas ni notas de crédito. | CFDI40160, CFDI40161 |
| `name_uppercase` | El nombre se pasa a mayúsculas y se quitan los espacios sobrantes. Hasta 300 caracteres y sin `\|`. | — |
| `name_societary_suffix` | En personas morales, el nombre no puede terminar en un régimen de capital como «SA DE CV». Ver «`name_societary_suffix`: sufijo societario», más abajo. | — |
| `generic_rfc` | Con `XAXX010101000` o `XEXX010101000`: régimen `616`, uso `S01` y código postal igual al del emisor. «PUBLICO EN GENERAL» solo con `XAXX010101000`. | CFDI40146, CFDI40149, CFDI40159 |

## Lo que no se revisa (y por qué)

Son los `check` de `notChecked`:

| `check` | Qué falta confirmar | Clave si el SAT lo rechaza al timbrar |
|---|---|---|
| `lrfc_exists` | Que el RFC exista y esté activo en la lista de RFC inscritos no cancelados (LRFC) del SAT. | CFDI40143 |
| `name_matches_sat` | Que el nombre o razón social sea el que el SAT tiene registrado para ese RFC. | CFDI40144, CFDI40145 |
| `zip_matches_sat` | Que el código postal sea el del domicilio fiscal registrado en el SAT. | CFDI40147, CFDI40148 |
| `regimen_matches_sat` | Que el régimen sea uno de los que el contribuyente tiene registrados en el SAT. | — |

**No hay una API pública del SAT para comparar estos datos contra el padrón.** Ni CFDI Express ni ningún PAC puede confirmarlos antes de timbrar. Solo hay dos formas:

1. **El validador oficial del SAT.** [Valida en línea RFC's uno a uno o de manera masiva hasta 5 mil registros](https://www.sat.gob.mx/aplicacion/79615/valida-en-linea-rfc%C2%B4s-uno-a-uno-o-de-manera-masiva-hasta-5-mil-registros), en el portal del SAT. Pide un captcha, así que no se puede automatizar. Revisa el RFC, el nombre y el código postal. En personas morales, el nombre se escribe sin el régimen societario.
2. **El timbrado.** Si algún dato no coincide, el SAT rechaza la factura (ver «Rechazos del SAT al timbrar», más abajo).

Por eso, la referencia para capturar los datos es la **constancia de situación fiscal** (CSF) del cliente, con fecha reciente. Copia de ahí el RFC, el nombre o razón social, el código postal y el régimen.

## Errores

### 400 `invalid_receiver`

Cuando los datos no pasan alguna revisión, la respuesta es `problem+json` con `code` `validation_error` y `subcode` `invalid_receiver`:

| Campo | Descripción |
|---|---|
| `detail` | «Los datos del receptor no pasan las validaciones del SAT:» seguido de todos los mensajes. |
| `errors[]` | Un elemento por cada problema, no solo el primero. |
| `errors[].path` | El campo a corregir: `customer.taxId`, `customer.legalName`, `customer.taxRegime`, `customer.zip` o `usage`. |
| `errors[].message` | Qué corregir, en español. Si la regla tiene clave del SAT, empieza con ella. |
| `errors[].satCode` | Opcional. La clave de la matriz de errores CFDI 4.0 (por ejemplo `CFDI40158`), solo si la regla tiene una. |
| `errors[].code` | Identificador estable de la regla, por si quieres mostrar tu propio texto. |
| `checked`, `notChecked`, `notice` | Los mismos que en el 200. |

Valores de `errors[].code`: `rfc_invalid`, `zip_invalid`, `name_invalid`, `name_societary_suffix`, `name_publico_en_general`, `regimen_unknown`, `regimen_persona_mismatch`, `regimen_generic_rfc`, `uso_cfdi_unknown`, `uso_cfdi_not_allowed`, `uso_cfdi_persona_mismatch`, `uso_cfdi_regimen_mismatch`, `uso_cfdi_generic_rfc` y `zip_generic_rfc`. En facturas y notas de crédito también pueden aparecer `residencia_fiscal_invalid`, `foreign_fields_not_allowed` y `num_reg_id_trib_invalid`, de los campos de extranjero (CFDI40150 a CFDI40154), que este endpoint no recibe.

### `name_societary_suffix`: sufijo societario

Aplica a personas morales (RFC de 12 caracteres). Si la razón social termina en un régimen de capital, se **bloquea**. El SAT registra la razón social sin él (la constancia lo muestra aparte) y la factura se rechazaría con CFDI40144 o CFDI40145.

Se detecta con o sin puntos y espacios, en mayúsculas o minúsculas. Por ejemplo: `SA DE CV`, `S.A.`, `S. A. DE C. V.`, `S DE RL DE CV`, `S. DE R.L.`, `SAPI DE CV`, `SAS DE CV`, `S.C.`, `A.C.` o `Sociedad Anónima de Capital Variable`.

**El nombre nunca se recorta solo.** Una razón social puede terminar de verdad en algo como «SA», así que la API no adivina: responde el error con el nombre sugerido y tú (o tu cliente) lo corriges. Si en la constancia la razón social realmente termina así, contacta a soporte.

Request con `"legalName": "ESCUELA KEMPER URGATE SA DE CV"` (RFC `EKU9003173C9`, régimen `601`, CP `42501`, uso `G03`):

```json
{
  "type": "https://api.cfdi.express/docs/errors/validation_error",
  "title": "Validation error",
  "status": 400,
  "code": "validation_error",
  "subcode": "invalid_receiver",
  "detail": "Los datos del receptor no pasan las validaciones del SAT: El nombre \"ESCUELA KEMPER URGATE SA DE CV\" termina con el régimen de capital «SA DE CV». El SAT registra la razón social sin él (la constancia de situación fiscal lo muestra aparte) y el timbrado se rechazaría con CFDI40144/CFDI40145. Quítalo y escribe la razón social tal como aparece en la constancia, p. ej. \"ESCUELA KEMPER URGATE\". Si en la constancia la razón social realmente termina así, contacta a soporte de CFDI Express.",
  "errors": [
    {
      "path": "customer.legalName",
      "message": "El nombre \"ESCUELA KEMPER URGATE SA DE CV\" termina con el régimen de capital «SA DE CV». El SAT registra la razón social sin él (la constancia de situación fiscal lo muestra aparte) y el timbrado se rechazaría con CFDI40144/CFDI40145. Quítalo y escribe la razón social tal como aparece en la constancia, p. ej. \"ESCUELA KEMPER URGATE\". Si en la constancia la razón social realmente termina así, contacta a soporte de CFDI Express.",
      "code": "name_societary_suffix"
    }
  ],
  "checked": [
    { "check": "rfc_format", "description": "…" }
  ],
  "notChecked": [
    { "check": "lrfc_exists", "description": "…" }
  ],
  "notice": "Estos datos no se comparan con el padrón del SAT: …"
}
```

`checked`, `notChecked` y `notice` vienen completos, igual que en el 200; aquí están recortados. En los siguientes ejemplos solo se muestra `errors`.

No se revisa el sufijo en personas físicas, en RFC genéricos ni en notas de crédito cuyo receptor sale de una factura ya timbrada (`relatedInvoiceId`, o `relatedUuid` de una factura de tu cuenta): ahí el SAT ya aceptó ese nombre exacto.

### `regimen_unknown` (CFDI40157)

El régimen no existe en `c_RegimenFiscal` para CFDI 4.0. Por ejemplo, `"taxRegime": "609"` con `EKU9003173C9`:

```json
"errors": [
  {
    "path": "customer.taxRegime",
    "message": "CFDI40157: el régimen fiscal del receptor \"609\" no existe en el catálogo c_RegimenFiscal del SAT. Usa la clave de 3 dígitos del régimen que aparece en la constancia de situación fiscal del receptor (p. ej. 601, 612, 626).",
    "satCode": "CFDI40157",
    "code": "regimen_unknown"
  }
]
```

### `regimen_persona_mismatch` (CFDI40158)

El régimen no aplica al tipo de persona del RFC. Por ejemplo, la persona física `CACX7605101P8` con `"taxRegime": "601"`:

```json
"errors": [
  {
    "path": "customer.taxRegime",
    "message": "CFDI40158: el régimen 601 (General de Ley Personas Morales) no aplica a personas físicas y el RFC CACX7605101P8 es de persona física (13 caracteres). Revisa el RFC y el régimen en la constancia de situación fiscal del receptor; regímenes para personas físicas: 605, 606, 607, 608, 610, 611, 612, 614, 615, 616, 621, 625, 626.",
    "satCode": "CFDI40158",
    "code": "regimen_persona_mismatch"
  }
]
```

### `zip_invalid`

El código postal no tiene 5 dígitos. Por ejemplo, `"zip": "3625"`:

```json
"errors": [
  {
    "path": "customer.zip",
    "message": "El código postal \"3625\" no es válido: deben ser 5 dígitos. Usa el CP del domicilio fiscal que aparece en la constancia de situación fiscal del receptor.",
    "code": "zip_invalid"
  }
]
```

Un código postal de 5 dígitos que no es el del domicilio fiscal pasa esta revisión: eso solo lo detecta el SAT (CFDI40147, CFDI40148).

### 404 Not Found

El `merchantId` no existe en tu cuenta (en el modo de la llave) o está eliminado:

```json
{
  "type": "https://api.cfdi.express/docs/errors/not_found",
  "title": "Not found",
  "status": 404,
  "code": "not_found",
  "detail": "No merchant mkq2f8v3x1t7p9d4n6c0b5rz"
}
```

### Resumen

| Status | code | Cuándo |
|---|---|---|
| 400 | `validation_error` + `subcode` `invalid_receiver` | Los datos del cliente no pasan alguna revisión. Trae `errors[]`, `checked`, `notChecked` y `notice`. |
| 400 | `validation_error` | Falta `merchantId`, `customer` o `usage`, o algún campo excede su longitud. |
| 401 | `unauthorized` | Falta la llave o es inválida. |
| 404 | `not_found` | El `merchantId` no existe o está eliminado. |
| 429 | `rate_limited` | Excediste el límite de requests por minuto. |

## Al timbrar: /v1/invoices aplica las mismas reglas

`POST /v1/invoices`, `POST /v1/invoices/preview`, `POST /v1/credit_notes` y `POST /v1/credit_notes/preview` corren exactamente estas revisiones antes de timbrar. Si algo falla, responden el mismo 400 `invalid_receiver`, con paths `receiver.*` y `usoCfdi`, y no se timbra ni se cobra nada. Para el mismo error del ejemplo de arriba, el elemento de `errors` de `POST /v1/invoices` es:

```json
{
  "path": "receiver.regimenFiscal",
  "message": "CFDI40158: el régimen 601 (General de Ley Personas Morales) no aplica a personas físicas …",
  "satCode": "CFDI40158",
  "code": "regimen_persona_mismatch"
}
```

Los errores de RFC genérico (`XAXX010101000`) y de receptor extranjero (`XEXX010101000`) también responden con este contrato, con mensajes en español.

### Rechazos del SAT al timbrar (CFDI40143 a CFDI40148)

Lo que la validación local no puede revisar lo revisa el SAT al timbrar. Si el RFC, el nombre o el código postal no coinciden con el padrón, la factura se rechaza con **422** `sat_rejected`. La respuesta trae un mensaje que explica qué corregir:

| Clave | Qué pasó | `field` |
|---|---|---|
| CFDI40143 | El RFC no está en la lista de RFC inscritos no cancelados: no existe, está dado de baja o tiene un carácter equivocado. | `receiver.rfc` |
| CFDI40144 | El nombre o razón social no está en la lista de RFC inscritos no cancelados. | `receiver.name` |
| CFDI40145 | El nombre no corresponde al RFC: el SAT tiene otro nombre registrado para ese RFC. | `receiver.name` |
| CFDI40147 | El código postal no está en la lista de RFC inscritos no cancelados. | `receiver.zip` |
| CFDI40148 | El código postal no corresponde al domicilio fiscal registrado para ese RFC. | `receiver.zip` |

```json
{
  "type": "https://api.cfdi.express/docs/errors/sat_rejected",
  "title": "Rejected by SAT validation",
  "status": 422,
  "code": "sat_rejected",
  "detail": "CFDI40145: el nombre o razón social no corresponde al RFC del receptor: el SAT tiene otro nombre registrado para ese RFC. Revisa la constancia de situación fiscal (CSF) del receptor, con fecha reciente, y corrige el dato: cópialo letra por letra, sin el régimen de capital (p. ej. sin «SA DE CV»), y confirma que el RFC sea el correcto.",
  "satRule": "CFDI40145",
  "field": "receiver.name",
  "satCode": "…texto original del PAC…",
  "satMessage": "…texto original del PAC…",
  "errors": [
    { "path": "receiver.name", "message": "CFDI40145: el nombre o razón social no corresponde al RFC del receptor: …", "satCode": "CFDI40145" }
  ],
  "invoiceId": "inv_..."
}
```

- `detail` y `errors[].message` son el texto para mostrar; `satCode` y `satMessage` conservan el texto original del PAC.
- Si el SAT rechaza la factura, el timbre se reembolsa.
- En `GET /v1/invoices/{id}` de esa factura (`stamp_failed`), `failure` trae `satRule`, `attribute` y `message`.

## Por MCP: `validate_receiver`

En el [servidor MCP](/docs/api/mcp), la herramienta `validate_receiver` recibe los mismos argumentos que el endpoint: `merchantId`, `customer` (`taxId`, `legalName`, `taxRegime`, `zip`) y `usage`. Es de solo lectura: no timbra, no cobra y no guarda nada.

Su respuesta lista lo que se revisó y lo que no, tanto si los datos pasan como si no. Si la razón social trae un sufijo societario, el agente te pide quitarlo; no lo recorta por su cuenta. Y te recuerda que solo el validador del SAT o el timbrado confirman los datos contra el padrón.

Por ejemplo, en Claude o ChatGPT:

> Antes de facturarle, valida estos datos de mi cliente: RFC EKU9003173C9, ESCUELA KEMPER URGATE SA DE CV, régimen 601, CP 42501, uso G03.

El agente llama `validate_receiver` y te dice que quites «SA DE CV». Con «ESCUELA KEMPER URGATE», los datos pasan.

## Preguntas frecuentes

**¿La prevalidación confirma que el RFC existe en el SAT?**
No. Revisa formatos y catálogos del SAT, no el padrón. No hay una API pública del SAT para eso. Que el RFC exista y que el nombre, el código postal y el régimen coincidan solo lo confirman el validador oficial del SAT o el timbrado.

**¿Cuesta timbres?**
No. No timbra, no cobra y no guarda nada.

**¿Por qué no puedo poner «SA DE CV» en la razón social?**
Porque el SAT registra la razón social de una persona moral sin el régimen de capital. Con el sufijo, la factura se rechazaría con CFDI40144 o CFDI40145. La API no lo quita sola porque una razón social puede terminar de verdad en algo parecido; te sugiere el nombre sin el sufijo.

**¿Tengo que llamar la prevalidación antes de cada factura?**
No. POST /v1/invoices aplica las mismas reglas antes de timbrar y responde el mismo error. La prevalidación sirve para detectar el problema antes, por ejemplo al dar de alta al cliente.

**¿Por qué el régimen 609 ya no se acepta?**
Desde la v1.60.0, el régimen del receptor se valida contra las claves de c_RegimenFiscal que se aceptan para el receptor en CFDI 4.0, y 609 no está entre ellas. Lo mismo pasa con 628, 629 y 630. Responden CFDI40157.

**¿Una persona moral puede usar el uso S01?**
Sí. S01 (Sin efectos fiscales) se acepta para personas físicas y morales.

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {"@type": "Question", "name": "¿La prevalidación confirma que el RFC existe en el SAT?", "acceptedAnswer": {"@type": "Answer", "text": "No. Revisa formatos y catálogos del SAT, no el padrón. No hay una API pública del SAT para eso. Que el RFC exista y que el nombre, el código postal y el régimen coincidan solo lo confirman el validador oficial del SAT o el timbrado."}},
    {"@type": "Question", "name": "¿Cuesta timbres?", "acceptedAnswer": {"@type": "Answer", "text": "No. No timbra, no cobra y no guarda nada."}},
    {"@type": "Question", "name": "¿Por qué no puedo poner «SA DE CV» en la razón social?", "acceptedAnswer": {"@type": "Answer", "text": "Porque el SAT registra la razón social de una persona moral sin el régimen de capital. Con el sufijo, la factura se rechazaría con CFDI40144 o CFDI40145. La API no lo quita sola porque una razón social puede terminar de verdad en algo parecido; te sugiere el nombre sin el sufijo."}},
    {"@type": "Question", "name": "¿Tengo que llamar la prevalidación antes de cada factura?", "acceptedAnswer": {"@type": "Answer", "text": "No. POST /v1/invoices aplica las mismas reglas antes de timbrar y responde el mismo error. La prevalidación sirve para detectar el problema antes, por ejemplo al dar de alta al cliente."}},
    {"@type": "Question", "name": "¿Por qué el régimen 609 ya no se acepta?", "acceptedAnswer": {"@type": "Answer", "text": "Desde la v1.60.0, el régimen del receptor se valida contra las claves de c_RegimenFiscal que se aceptan para el receptor en CFDI 4.0, y 609 no está entre ellas. Lo mismo pasa con 628, 629 y 630. Responden CFDI40157."}},
    {"@type": "Question", "name": "¿Una persona moral puede usar el uso S01?", "acceptedAnswer": {"@type": "Answer", "text": "Sí. S01 (Sin efectos fiscales) se acepta para personas físicas y morales."}}
  ]
}
</script>

## Relacionados

- [Timbrar una factura](https://cfdi.express/docs/api/timbrar-factura): `POST /v1/invoices`
- [Crear una nota de crédito](https://cfdi.express/docs/api/crear-nota-de-credito): `POST /v1/credit_notes`
- [Vista previa de la factura antes de timbrar](/docs/vista-previa-factura-antes-de-timbrar)
- [Facturación al extranjero (RFC XEXX010101000)](/docs/facturacion-al-extranjero)
- [Servidor MCP](https://cfdi.express/docs/api/mcp)

---

Referencia completa: https://cfdi.express/docs/api · OpenAPI: https://api.cfdi.express/openapi.json · Llaves: https://dash.cfdi.express
