---
title: "La nueva API de CFDI Express: facturación CFDI 4.0 para cualquier sistema"
description: "Lanzamos la API pública de CFDI Express: una API REST multi-tenant para timbrar facturas CFDI 4.0 (PUE, PPD y globales), complementos de pago, cancelaciones y notas de crédito desde cualquier sistema. Sandbox gratuito, saldo prepagado desde $1 MXN por timbre y servidor MCP para agentes de IA."
image: "https://videos.acromatico.dev/api/images/assets/f9ebf8a3-6606-4ce4-a0ce-8eb53fe3a645.jpeg?w=1600"
author: "Rafael González"
date: "2026-08-17"
keywords: "CFDI Express API, API facturación, CFDI 4.0, API CFDI México, facturación para desarrolladores, facturación desde cualquier sistema, MCP, REST API, complementos de pago"
---
# Lanzamos CFDI Express API: facturación CFDI 4.0 para cualquier sistema

<!-- TODO: imagen hero del post — generar con Gemini usando el prompt de media (sección "Imagen hero del blog") de socials/lanzamiento-cfdi-express-api.md, subirla a videos.acromatico.dev y actualizar el campo `image:` del frontmatter -->
<!-- ENDTODO -->

Desde que lanzamos CFDI Express, nuestra misión ha sido una sola: que **facturar en México deje de ser un dolor de cabeza**. Lo empezamos dentro de Shopify — donde hoy somos una aplicación **Built for Shopify** — y ahora damos el siguiente paso: la infraestructura de facturación CFDI 4.0 que mueve a CFDI Express se vuelve una **API REST pública** que cualquier sistema puede usar.

[Descubre la API de CFDI Express](https://cfdi.express/api)

## ¿Qué es la CFDI Express API?

La API de CFDI Express es la misma plataforma que timbra los CFDIs de las tiendas Shopify, ahora disponible como **API REST multi-tenant para desarrolladores y empresas**. Con una sola petición a `POST /v1/invoices` puedes generar facturas CFDI 4.0 100% válidas ante el SAT:

- **Facturas de ingreso (PUE y PPD)**, nominativas y globales (público en general).
- **Complementos de pago (REP / Pagos 2.0)** con parcialidades y saldos calculados en servidor.
- **Cancelaciones con acuse** ante el SAT, con motivos 01–04.
- **Notas de crédito (egresos)** para devoluciones y descuentos posteriores.

Y no necesitas Shopify para usarla: si tu sistema puede hacer una petición HTTPS, puede timbrar. **ERPs, marketplaces, apps móviles, tiendas propias o backends internos**, todos pueden conectarse.

## ¿Para quién es?

- **Desarrolladores y agencias** que integran facturación en sistemas propios o de sus clientes.
- **Negocios** que venden por canales fuera de Shopify y quieren unificar su facturación.
- **Equipos que usan IA**: la API incluye un servidor MCP para que un agente como Claude o ChatGPT timbre facturas por ti.

## Todo el ciclo del CFDI en una API

La API cubre el ciclo de vida completo del comprobante, sin portales, sin Excel y sin calcular impuestos a mano.

### 🧾 Facturación completa (PUE, PPD y globales)

El motor de impuestos de CFDI Express corre del lado del servidor: **tú no calculas IVA, IEPS ni totales**. Las reglas del SAT se validan automáticamente, incluida la factura global para público en general con su `informacionGlobal` y receptor `XAXX010101000`.

### 💳 Complementos de pago (REP)

Registra pagos contra facturas PPD con **Pagos 2.0**: `numParcialidad` y saldos pendientes se calculan solos. Los sobrepagos, pagos fuera de secuencia y facturas liquidadas se rechazan automáticamente. Cancelar un REP restaura el saldo de la factura.

### ❌ Cancelaciones con acuse

Cancela facturas y complementos con motivos 01–04, `folioSustitucion` cuando aplica, y recibe el **acuse XML del SAT** guardado con URL firmada. Si una factura PPD tiene complementos activos, la API te lo dice y te guía a cancelarlos primero.

### 🏢 Múltiples emisores y múltiples RFC

Registra **varios emisores (merchants)** en una sola cuenta, cada uno con su propio **CSD** (vigencia, llave↔cert y RFC validados localmente) y su **RFC**. Todos permanecen activos al mismo tiempo: tú decides con cuál RFC timbrar en cada factura. Tus certificados se guardan encriptados con **AES-256-GCM** y sin tu contraseña nadie puede usarlos.

### 📚 Catálogos SAT locales

**52,513 claves de producto** (con sinónimos), **2,418 unidades**, usos de CFDI, regímenes fiscales y formas de pago — servidos desde `GET /v1/catalogs/...` sin integraciones externas.

### 📄 PDF, XML y ZIP listos para entregar

El worker de CFDI Express renderiza el **PDF Anexo 20** con QR, sellos y cadena original, y te lo entrega junto con el XML y un ZIP firmado vía **URLs firmadas de 15 minutos** — listas para enviarse a tu cliente o guardarse en tu sistema.

### 🔁 Confiabilidad de nivel Stripe

- **Idempotency-Key persistente**: si reintentas, recibes el documento original — un replay nunca gasta un segundo timbre. Duplicados concurrentes reciben `409`.
- **Rate limiting** de 300 requests por minuto por llave, límite de body de 1 MB y **registro de auditoría** en cada mutación.
- **Reembolso automático** si el SAT rechaza o el PAC falla, con ledger completo para auditar cada centavo.
- La misma infraestructura verificada bajo ráfagas: **35 timbres concurrentes, cero dobles timbres, p95 de 1.57 s**.

### ⚡ DX estilo Stripe

Llaves `sk_test_`/`sk_live_`, `livemode` en cada recurso, errores `problem+json` con subcódigos, IDs opacos y paginación por cursor. **Si ya usaste Stripe, ya sabes usar esto.** Y las [docs interactivas](https://api.cfdi.express/docs) te dejan probar cada endpoint desde el navegador.

## Para developers: de registro a primer timbre en minutos

1. **Crea tu cuenta** en [dash.cfdi.express](https://dash.cfdi.express) y obtén tu llave `sk_test_` al instante, con saldo de prueba para el sandbox del SAT.
2. **Sube tu CSD** con `POST /v1/merchants` (archivos `.cer` y `.key`). Validamos vigencia, llave↔cert y RFC antes de guardarlo encriptado.
3. **Timbra tu primer CFDI** con `POST /v1/invoices` — el motor de impuestos calcula todo por ti:

```bash
curl https://api.cfdi.express/v1/invoices \
  -H "Authorization: Bearer sk_test_..." \
  -H "Idempotency-Key: orden-1042" \
  -H "Content-Type: application/json" \
  -d '{
    "merchantId": "mer_...",
    "metodoPago": "PUE",
    "formaPago": "03",
    "usoCfdi": "G03",
    "receiver": {
      "rfc": "EKU9003173C9",
      "name": "ESCUELA KEMPER URGATE",
      "zip": "76000",
      "regimenFiscal": "601"
    },
    "items": [{
      "productCode": "01010101",
      "unitCode": "E48",
      "description": "Desarrollo de software",
      "quantity": 1,
      "unitPrice": 1500
    }]
  }'
```

4. **Entrega XML y PDF**: la respuesta trae las URLs firmadas del XML, PDF y ZIP listas para enviar a tu cliente.

Los primeros 3 pasos también los puedes hacer desde el dashboard, sin programar.

## Tu agente de IA también factura

La API incluye un **servidor MCP (Model Context Protocol)** en `https://api.cfdi.express/mcp`, con **13 herramientas** que cubren todo el ciclo: `create_invoice`, `create_payment`, `cancel_invoice`, `list_merchants`, búsqueda de códigos SAT, consulta de saldo y más.

### Video tutorial: conectar tu agente de IA (Claude, ChatGPT y más) a la API

Si prefieres ver el proceso completo de configuración paso a paso — desde crear tu cuenta, registrarte, generar tu llave y conectar tu agente hasta timbrar tu primer CFDI — aquí tienes el tutorial:

<iframe width="100%" height="400" src="https://www.youtube.com/embed/urs7klK6p1U" title="Conecta tu agente de IA (Claude, ChatGPT) a la API de CFDI Express" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

[Ver tutorial en YouTube](https://youtu.be/urs7klK6p1U)

- **Con OAuth**: agrégalo como conector personalizado en **claude.ai, ChatGPT o Claude Desktop** con `https://api.cfdi.express/mcp`, inicia sesión con tu cuenta del dashboard y pídele en lenguaje natural que timbre facturas.
- **Con API key**: en **Claude Code, Cursor o Windsurf** solo agrega el servidor con tu `sk_test_`/`sk_live_`.
- **Para experimentar**: conecta `https://api.cfdi.express/mcp/test` y tu agente timbra contra el sandbox del SAT, gratis.

Un agente puede pedirle los datos fiscales al cliente, buscar la clave de producto en el catálogo, timbrar la factura y entregarle el PDF — horas de trabajo convertidas en minutos.

## Precios claros y por uso

La API se paga **solo por lo que timbras**, sin mensualidad, sin mínimos y sin contratos:

- **Sandbox gratis** e ilimitado con tu llave `sk_test_`.
- **$1 MXN por timbre** en producción, con saldo prepagado.
- Recargas de **$100 a $50,000 MXN** con tarjeta vía Stripe.
- **Descuentos por volumen** automáticos: entre más timbras, menos pagas por timbre.
- Complementos de pago, cancelaciones, multi-CSD y catálogos **sin costo extra**.

Cada compra de saldo incluye su propia factura (expedida por CFDI Express) — porque también somos nuestros propios primeros clientes.

## ¿Cómo empiezo?

1. **Si eres developer o agencia**: crea tu cuenta gratis en [dash.cfdi.express](https://dash.cfdi.express) y timbra tu primer CFDI en modo test hoy mismo.
2. **Si vendes en Shopify**: la app de CFDI Express ya incluye todo esto integrado — visita la [app en la tienda de Shopify](https://apps.shopify.com/cfdi-express) y su [landing pública](https://cfdi.express).
3. **Si tienes dudas**: [agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escríbenos a [hola@cfdi.express](mailto:hola@cfdi.express).

La facturación electrónica en México lleva años siendo una pesadilla de portales del SAT, Excel y procesos manuales. Con la API de CFDI Express, queremos hacer por la facturación de cualquier negocio lo que ya hemos hecho por la de las tiendas Shopify: **automatizarla de verdad, desde donde tú ya operas.**
