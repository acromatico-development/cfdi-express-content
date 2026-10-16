# Social Pack | Cómo darle acceso a tu contador a tu facturación sin compartir tu contraseña

Corresponde al blog post: [`pages/acceso-contador-sin-compartir-contrasena.md`](../pages/acceso-contador-sin-compartir-contrasena.md)

Recursos oficiales para enlazar:

- Post (tras el merge): https://cfdi.express/blog/acceso-contador-sin-compartir-contrasena
- Guía: https://cfdi.express/docs/equipo-y-workspaces
- Dashboard (página Equipo): https://dash.cfdi.express
- Landing API: https://cfdi.express/api
- Docs de la API: https://api.cfdi.express/docs
- Demo: https://cal.com/team/acromatico-development/cfdi-express
- Contacto: hola@cfdi.express

---

## Cómo usar este pack

1. **Usa el arte publicado.** No lo regeneres. El `image` del frontmatter es el hero y el OG: 1200×627, sin query (la plantilla también lo usa como `og:image`). En este pack, LinkedIn, X, Facebook e Instagram usan la misma URL, sin query. En el cuerpo del post el hero lleva `?w=1600`.
2. Publica cada red con el **Texto** sugerido.
3. El CTA principal es **invitar desde Equipo** en el dashboard y leer la guía. La API se menciona como la misma cuenta, sin pedir que cambien las API keys.

---

## Imagen hero del blog

**Publicada. Usar esta URL, sin query.** Es el hero y el OG. Título en el arte: «Factura en equipo».

| Uso | Medida | URL (sin query) |
| --- | --- | --- |
| Hero / `image` del frontmatter (la plantilla lo usa también como `og:image`) | 1200×627 | https://videos.acromatico.dev/api/images/assets/9f9ca7e5-b1c5-4075-b19e-945498b79d13.jpg |

No hay un campo OG aparte ni un recorte 1600×900. La misma pieza cubre el hero del post y el Open Graph. El cuerpo del post usa el hero con `?w=1600`. Alt: «Factura en equipo: página de Equipo de CFDI Express con miembros invitados».

---

## LinkedIn

### Texto

Tu contador no necesita tu contraseña para ver la facturación.

En CFDI Express cada cuenta es un workspace. Tú eres el dueño. Lo invitas por correo desde Equipo, en el dashboard, y él acepta con ese mismo correo. El enlace dura 7 días.

Qué puede hacer:

- Operar el workspace y crear o revocar API keys
- Ver la facturación en solo lectura

Qué se queda contigo:

- Comprar saldo
- Administrar la facturación

Si ya conectaste un cliente por OAuth o MCP, esa conexión quedó en tu workspace propio, sin reconectar. Si más adelante lo quitas del equipo, se revocan sus conexiones. El workspace guarda un registro de auditoría.

El enlace es `/invite/…`. Hay que aceptarlo con el mismo correo. El dueño tiene que estar verificado: máximo 50 correos al día por workspace y 60 segundos entre reenvíos.

Las integraciones que ya usan API key siguen igual.

Guía: https://cfdi.express/docs/equipo-y-workspaces
Invítalo en: https://dash.cfdi.express
Post: https://cfdi.express/blog/acceso-contador-sin-compartir-contrasena

#CFDI #FacturacionElectronica #SAT #CFDIExpress #Contabilidad #FacturacionDigital

### Media

**Usar:** hero/OG 1200×627, sin query
https://videos.acromatico.dev/api/images/assets/9f9ca7e5-b1c5-4075-b19e-945498b79d13.jpg

**Aspecto:** 1200×627 (1.91:1).

---

## X (Twitter)

### Texto (post principal)

Tu contador puede entrar a tu facturación en CFDI Express sin tu contraseña.

Lo invitas por correo. El enlace vence en 7 días y se acepta con ese mismo correo. Él ve la facturación; comprar saldo se queda contigo.

→ https://cfdi.express/docs/equipo-y-workspaces

### Hilo opcional

1. Cada cuenta del dashboard es un workspace. Tú eres el dueño. Tu contador entra como miembro, con su correo.
2. Puede operar el workspace y crear o revocar API keys. La facturación la ve en solo lectura. Comprar saldo y administrarla responden 403.
3. El enlace es /invite/… y vence a los 7 días. Dueño verificado, 50 correos al día por workspace, 60 segundos entre reenvíos.
4. Las conexiones OAuth y MCP quedan ligadas a un workspace. Las que ya tenías se ligaron al tuyo: no reconectes. Si lo quitas, se revocan las suyas.
5. Tus API keys actuales no cambian. Para apuntar a una cuenta, el header es x-account-id (403 si no es miembro). Guía: https://cfdi.express/docs/equipo-y-workspaces

### Media

**Usar:** la misma pieza 1200×627, sin query
https://videos.acromatico.dev/api/images/assets/9f9ca7e5-b1c5-4075-b19e-945498b79d13.jpg

**Aspecto:** 1200×627 (1.91:1).

---

## Facebook

### Texto

¿Tu contador te está pidiendo la contraseña de CFDI Express?

Invítalo desde Equipo. Cada cuenta es un workspace: tú sigues como dueño y él entra con su correo. El enlace vence a los 7 días y tiene que aceptarlo con el mismo correo de la invitación.

Puede operar la cuenta y crear API keys. La facturación la ve en solo lectura. Comprar saldo y administrarla se quedan contigo.

Si ya tenías conexiones con un asistente (OAuth o MCP), quedaron en tu workspace. No hace falta reconectarlas.

Guía: https://cfdi.express/docs/equipo-y-workspaces
Dashboard: https://dash.cfdi.express

#CFDI #FacturacionElectronica #SAT #CFDIExpress

### Media

**Usar:** hero/OG 1200×627, sin query
https://videos.acromatico.dev/api/images/assets/9f9ca7e5-b1c5-4075-b19e-945498b79d13.jpg

**Aspecto:** 1200×627 (1.91:1).

---

## Instagram

### Texto (feed)

Tu contador puede entrar a tu facturación sin tu contraseña.

En CFDI Express lo invitas por correo desde Equipo:

- Enlace /invite, vence en 7 días
- Acepta con el mismo correo
- Ve la facturación en solo lectura
- Comprar saldo se queda contigo
- Si sale del equipo, se revocan sus conexiones

Las API keys que ya usas siguen igual.

Guía en el enlace de la bio (cfdi.express/docs/equipo-y-workspaces).

#CFDI #FacturacionElectronica #SAT #CFDIExpress #Contabilidad #FacturacionDigital #Mexico #Contador #Pyme #Emprendedores #FacturaElectronica #CFDI40 #NegociosMexico #Administracion #Finanzas #Facturacion #Equipo #Dashboard #API #Seguridad #Acceso #Workspace #SATMexico #Negocios #TipsContables

### Media (feed cuadrado)

No hay recorte 1080×1080. **Usar la misma pieza 1200×627:**
https://videos.acromatico.dev/api/images/assets/9f9ca7e5-b1c5-4075-b19e-945498b79d13.jpg

### Media (historia opcional)

No hay recorte 1080×1920. **Usar la misma pieza 1200×627:**
https://videos.acromatico.dev/api/images/assets/9f9ca7e5-b1c5-4075-b19e-945498b79d13.jpg

---

## Checklist de publicación

- [x] Hero/OG 1200×627 en el frontmatter `image`, sin query. El cuerpo del post usa la misma URL con `?w=1600` y el alt «Factura en equipo: página de Equipo de CFDI Express con miembros invitados».
- [x] Placeholder `PENDIENTE-…` y el bloque `<!-- TODO -->` / `<!-- ENDTODO -->` ya no están en el post.
- [ ] Publicar y usar https://cfdi.express/blog/acceso-contador-sin-compartir-contrasena cuando la URL del post responda. Mientras tanto, enlazar la guía y el dashboard.
- [x] La guía https://cfdi.express/docs/equipo-y-workspaces ya está en el repo (PR #22).
- [ ] Revisar que el copy diga: invitación por correo, 7 días, mismo correo, facturación en solo lectura, saldo y facturación del dueño, API keys sin cambio, conexiones existentes ligadas al workspace propio.
- [ ] CTA en cada red: dashboard o guía.
