---
title: "Equipo y workspaces: invita a tu contador sin compartir la contraseña"
description: "Cada cuenta de CFDI Express es un workspace. El dueño invita miembros por correo, el miembro opera todo menos saldo y facturación, y la API usa el header x-account-id."
keywords: "workspace CFDI Express, equipo facturación, invitar contador, miembros dashboard, x-account-id, API keys CFDI, conexiones MCP, OAuth CFDI Express"
---
# Equipo y workspaces

URL de esta página: https://cfdi.express/docs/equipo-y-workspaces

Cada cuenta de CFDI Express es un **workspace** compartido. El dueño invita por correo a quien necesita operar esa facturación, por ejemplo tu contador, y esa persona entra con su propio acceso.

El selector de workspace, la página **Equipo**, las API keys y las conexiones OAuth/MCP están en el [dashboard](https://dash.cfdi.express).

## Qué es un workspace

El workspace es la cuenta. Quien crea la cuenta es el **dueño**. Quien acepta una invitación es **miembro** de ese mismo workspace. Ahí conviven el saldo, las API keys y las conexiones OAuth/MCP.

Puedes pertenecer a varios workspaces: el tuyo y aquellos a los que te invitaron. En el dashboard eliges el workspace activo y esa elección queda **en la pestaña** del navegador. Dos pestañas pueden mostrar dos workspaces distintos.

## Roles

| Acción | Dueño | Miembro |
| --- | --- | --- |
| Operar el workspace | Sí | Sí |
| Crear y revocar API keys | Sí | Sí |
| Ver facturación | Sí | Solo lectura |
| Comprar saldo | Sí | No |
| Administrar facturación | Sí | No |

Un miembro hace el trabajo del workspace. Las dos cosas que se quedan con el dueño son **comprar saldo** y **administrar la facturación**. El miembro ve la facturación y no la modifica.

Crear y revocar API keys ya no exige ser dueño.

## Invitar, aceptar y quitar

1. Entra al dashboard y abre **Equipo**.
2. El dueño invita por correo. El número de miembros es ilimitado.
3. El correo trae un enlace con la ruta `/invite/:token`.
4. El enlace **vence a los 7 días**.
5. Para aceptarlo hay que entrar con **el mismo correo** al que se envió la invitación.

<!-- TODO: captura — página Equipo con la invitación por correo -->
![TODO: Página Equipo](TODO-screenshot)
<!-- ENDTODO -->

Límites al enviar invitaciones:

- El dueño tiene que estar **verificado**.
- **50 correos al día** por workspace.
- **60 segundos** entre reenvíos.

Si un miembro sale o lo quitas, se revocan sus conexiones OAuth y MCP.

El workspace guarda un **registro de auditoría**.

## Selector de workspace

El selector está en el dashboard. La selección es **por pestaña**.

También está en **Conexiones** y en la **pantalla de consentimiento**, para elegir a qué workspace se liga una conexión nueva.

<!-- TODO: captura — selector de workspace en el dashboard -->
![TODO: Selector de workspace](TODO-screenshot)
<!-- ENDTODO -->

## Conexiones MCP y OAuth

Cada conexión OAuth o MCP queda ligada a un workspace.

- Las conexiones que ya existían se ligaron solas al **workspace propio** de quien las tenía. No hace falta reconectarlas.
- Al crear una conexión nueva, eliges el workspace en **Conexiones** o en la pantalla de consentimiento.
- Si un miembro sale o lo quitan, se revocan sus conexiones.

Las integraciones que ya autentican con **API key** siguen igual.

## Notas para la API

Para apuntar una llamada a una cuenta, envía el header `x-account-id`.

Si quien llama no es miembro de esa cuenta, la API responde **403**.

Comprar saldo y administrar la facturación son del dueño. Si un miembro intenta esas operaciones, la API responde **403**. En el dashboard la facturación se consulta en solo lectura.

Una integración que ya usa API key no cambia: sigue autenticándose como hasta ahora.

## Preguntas frecuentes

### ¿Mi contador necesita mi contraseña?

Entra con el correo de su invitación, a tu workspace. Tú sigues siendo el dueño de la cuenta.

### ¿Cuántas personas puedo invitar?

Los miembros no tienen límite. El envío sí: 50 correos al día por workspace y 60 segundos entre reenvíos. El dueño tiene que estar verificado.

### ¿Qué pasa si el enlace ya venció?

A los 7 días deja de servir. Vuelve a enviar la invitación desde **Equipo**, con el intervalo de 60 segundos entre reenvíos.

### ¿Puede aceptar con otro correo?

Tiene que aceptar con el mismo correo al que invitaste.

### ¿El miembro puede comprar saldo o cambiar la facturación?

Ve la facturación en solo lectura. Comprar saldo y administrar la facturación responden **403** si las intenta un miembro.

### ¿Puede crear API keys?

Sí. Crear y revocar API keys ya no es exclusivo del dueño. Las integraciones que ya usan una API key no cambian.

### ¿Tengo que reconectar Claude, ChatGPT u otro cliente MCP?

Las conexiones OAuth/MCP que ya tenías quedaron ligadas a tu workspace propio. Una conexión nueva pide el workspace en **Conexiones** o en la pantalla de consentimiento.

### ¿Qué pasa si lo quito o se sale?

Se revocan sus conexiones. El workspace tiene registro de auditoría.

## Siguiente paso

Abre [Equipo en el dashboard](https://dash.cfdi.express) e invita a tu contador con su correo.
