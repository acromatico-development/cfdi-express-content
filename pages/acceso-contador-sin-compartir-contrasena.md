---
title: "Cómo darle acceso a tu contador a tu facturación sin compartir tu contraseña"
description: "Invita a tu contador a tu workspace de CFDI Express por correo. Entra con su acceso, opera el workspace y las API keys; el saldo y la facturación se quedan contigo."
image: "https://videos.acromatico.dev/api/images/assets/PENDIENTE-acceso-contador-sin-compartir-contrasena.png?w=1600"
author: "Rafael González"
date: "2026-10-16"
keywords: "acceso contador facturación, compartir cuenta CFDI, workspace CFDI Express, invitar miembro dashboard, equipo facturación electrónica, API keys contador, sin compartir contraseña, x-account-id"
---
# Cómo darle acceso a tu contador a tu facturación sin compartir tu contraseña

<!-- TODO: imagen hero del post — generar con Gemini usando el prompt de media (sección "Imagen hero del blog") de socials/acceso-contador-sin-compartir-contrasena.md, subirla a videos.acromatico.dev y actualizar el campo `image:` del frontmatter -->
<!-- ENDTODO -->

Tu contador te pide el XML, el saldo o que revises una factura que no cuadra. La salida rápida es pasarle el usuario y la contraseña de tu cuenta. Con eso también le pasas comprar saldo, cambiar la facturación y entrar a todo lo demás con tu nombre.

En el [dashboard de CFDI Express](https://dash.cfdi.express) cada cuenta ya es un **workspace** compartido. Tú sigues como dueño. Tu contador entra como **miembro**, con su correo, por una invitación que tú envías.

La guía completa está en [Equipo y workspaces](https://cfdi.express/docs/equipo-y-workspaces).

## Qué es el workspace

El workspace es tu cuenta. Ahí están el saldo, las API keys y las conexiones de OAuth y MCP.

Quien abrió la cuenta es el dueño. Quien acepta una invitación es miembro de esa misma cuenta. Puedes estar en la tuya y en las que te inviten: el selector del dashboard elige el workspace activo y esa elección queda **en la pestaña**. Dos pestañas pueden trabajar dos cuentas distintas.

## Qué puede hacer tu contador

Un miembro opera el workspace. Comprar saldo y administrar la facturación se quedan con el dueño. La facturación el miembro la ve en **solo lectura**.

| Acción | Dueño | Miembro (tu contador) |
| --- | --- | --- |
| Operar el workspace | Sí | Sí |
| Crear y revocar API keys | Sí | Sí |
| Ver facturación | Sí | Solo lectura |
| Comprar saldo | Sí | No |
| Administrar facturación | Sí | No |

Crear y revocar API keys ya no pide ser dueño. Si tu contador arma una integración, puede crear la llave en el workspace y revocarla cuando deje de usarla.

Las integraciones que **ya** usan una API key siguen igual. El equipo nuevo no les cambia la autenticación.

## Cómo invitarlo

1. Entra a [dash.cfdi.express](https://dash.cfdi.express) y abre **Equipo**.
2. Invítalo con su correo. Los miembros no tienen límite.
3. Le llega un enlace `/invite/:token`.
4. Ese enlace **vence a los 7 días**.
5. Tiene que aceptarlo con **el mismo correo** de la invitación.

Para poder enviar, el dueño tiene que estar **verificado**. El workspace manda como máximo **50 correos al día**. Entre un reenvío y el siguiente hay que esperar **60 segundos**.

Si el enlace ya venció, vuelve a enviarlo desde **Equipo**.

## Conexiones de MCP y OAuth

Cada conexión OAuth o MCP queda ligada a un workspace.

Si ya tenías conexiones, se ligaron solas a **tu workspace propio**. Siguen en tu cuenta, sin un paso de reconexión.

Cuando alguien conecta un cliente nuevo, elige el workspace en **Conexiones** o en la pantalla de consentimiento.

Si el miembro sale o lo quitas de **Equipo**, se revocan sus conexiones.

El workspace lleva un **registro de auditoría**.

## Si también timbras por API

El acceso del equipo es de la cuenta. Tus llamadas con API key no cambian.

Cuando una llamada tiene que ir a una cuenta concreta, la API acepta el header `x-account-id`. Si quien llama no es miembro de esa cuenta, la respuesta es **403**.

Comprar saldo y administrar la facturación también responden **403** si las intenta un miembro. En el dashboard esa información se consulta en solo lectura.

El detalle, con el flujo de invitación y las preguntas frecuentes, está en la doc: [Equipo y workspaces](https://cfdi.express/docs/equipo-y-workspaces). La referencia del API sigue en [api.cfdi.express/docs](https://api.cfdi.express/docs).

## Preguntas que salen al invitar

### ¿Le creo un usuario con mi misma contraseña?

Invítalo a tu workspace. Él entra con su correo. La contraseña de tu cuenta se queda contigo.

### ¿Puede recargar el saldo?

El miembro ve la facturación y no compra saldo ni administra la facturación. Esas operaciones responden 403.

### ¿Y si ya no trabaja conmigo?

Lo quitas en **Equipo**, o él sale. Se revocan sus conexiones OAuth y MCP. Las integraciones por API key siguen hasta que alguien con acceso las revoque: crear y revocar llaves ya lo puede hacer un miembro.

### ¿Se rompe lo que ya tengo conectado con API key?

Sigue igual. El cambio es quién puede entrar al dashboard y a qué workspace apunta cada conexión OAuth o MCP.

## Invítalo hoy

1. Entra al [dashboard](https://dash.cfdi.express) y abre **Equipo**.
2. Manda la invitación al correo de tu contador y pídele que acepte con ese mismo correo antes de 7 días.
3. Si quieres el detalle de roles, límites y el header `x-account-id`, lee [Equipo y workspaces](https://cfdi.express/docs/equipo-y-workspaces).

¿Timbras desde tu propio sistema? La cuenta es la misma de la [API de CFDI Express](https://cfdi.express/api). Si prefieres verlo en una llamada, [agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).
