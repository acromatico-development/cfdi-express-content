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

1. Genera la imagen hero con el prompt de abajo (Gemini) y súbela a `videos.acromatico.dev`.
2. Sustituye en el frontmatter de `pages/acceso-contador-sin-compartir-contrasena.md` la URL `PENDIENTE-acceso-contador-sin-compartir-contrasena.png` por el asset publicado, sin dejar el placeholder.
3. Publica cada red con el **Texto** sugerido. El hero 16:9 va al blog; LinkedIn y Facebook usan 1.91:1; X usa 16:9; Instagram feed es 1:1.
4. El CTA principal es **invitar desde Equipo** en el dashboard y leer la guía. La API se menciona como la misma cuenta, sin pedir que cambien las API keys.

---

## Imagen hero del blog

**Pendiente.** El frontmatter apunta a:

`https://videos.acromatico.dev/api/images/assets/PENDIENTE-acceso-contador-sin-compartir-contrasena.png?w=1600`

**Prompt Gemini:**

> Crea una imagen wide 16:9 (1600×900), ilustración flat y moderna, profesional B2B, alto contraste, sin fotografías de stock y sin logos de otras marcas. Fondo teal oscuro #055D5E. A la izquierda, una tarjeta blanca de “Dueño” con un candado dorado #FFD700 cerrado y el texto “Tu cuenta”. A la derecha, una tarjeta blanca de “Contador” con un sello de factura y una palomita. Entre las dos, un sobre de invitación blanco con el texto “/invite”. Tipografía Racing Sans One en el título y Quicksand en el resto. Título exacto, en blanco: “Acceso para tu contador”. Subtítulo exacto: “Sin compartir tu contraseña”. Pie pequeño en gris #D7D7D7: “CFDI Express · Equipo”.

**Aspecto:** 16:9 · 1600×900.

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

**Prompt Gemini:**

> Crea una imagen horizontal 1200×627 (1.91:1), ilustración flat B2B, sin fotos de stock. Fondo teal oscuro #055D5E. Centro: dos personas estilizadas, sin rostros realistas, una con una llave dorada #FFD700 y otra con una factura blanca. Un tercer elemento, un sobre, dice “Invitación · 7 días”. Título en Racing Sans One, blanco, texto exacto: “Tu contador, en tu workspace”. Subtítulo en Quicksand: “Tú conservas el saldo”. Acento dorado #FFD700, gris #D7D7D7, alto contraste, marca CFDI Express sugerida con un recibo simple, sin logos ajenos.

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

**Prompt Gemini:**

> Crea una imagen horizontal 1600×900 (16:9) para X. Fondo teal oscuro #055D5E. A la izquierda un candado dorado #FFD700. A la derecha una factura blanca con sello y el texto “Solo lectura”. Título grande en Racing Sans One, blanco, texto exacto: “Sin tu contraseña”. Línea menor en Quicksand: “Invita a tu contador · CFDI Express”. Estilo flat, minimalista, alto contraste, sin fotografías.

**Aspecto:** 1600×900 (16:9).

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

**Prompt Gemini:**

> Crea una imagen horizontal 1200×630 (1.91:1) para Facebook. Fondo teal oscuro #055D5E con una cuadrícula sutil en gris #D7D7D7. Al centro, un sobre blanco abierto y una factura con sello dorado #FFD700. Texto principal en Racing Sans One, blanco, exacto: “Invita a tu contador”. Texto secundario en Quicksand: “Tu contraseña se queda contigo”. Estilo ilustración flat, profesional, sin fotos de personas reales, sin logos de terceros.

**Aspecto:** 1200×630 (1.91:1).

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

**Prompt Gemini:**

> Crea una imagen cuadrada 1080×1080, ilustración flat, fondo teal oscuro #055D5E. Centro: un recibo blanco con sello dorado #FFD700 y, al lado, un sobre con el texto exacto “Equipo”. Título en Racing Sans One, blanco: “Tu contador entra”. Subtítulo en Quicksand: “Sin tu contraseña”. Pie en gris #D7D7D7: “CFDI Express”. Alto contraste, minimalista, sin fotografías, sin logos ajenos.

**Aspecto:** 1080×1080 (1:1) para feed.

### Media (historia opcional)

**Prompt Gemini:**

> Crea una imagen vertical 1080×1920 para historia de Instagram. Fondo teal oscuro #055D5E. Deja el tercio superior y el inferior despejados para la interfaz de Instagram. Al centro, un candado dorado #FFD700 junto a una factura blanca. Texto grande en Racing Sans One, blanco, exacto: “Invita a tu contador”. Texto menor en Quicksand: “7 días · mismo correo”. Estilo flat, sin fotos.

**Aspecto:** 1080×1920 (historia).

---

## Checklist de publicación

- [ ] Generar el hero 1600×900, subirlo a videos.acromatico.dev y reemplazar `PENDIENTE-acceso-contador-sin-compartir-contrasena` en el frontmatter del post.
- [ ] Quitar el bloque `<!-- TODO -->` / `<!-- ENDTODO -->` del post cuando el hero ya esté publicado.
- [ ] Publicar el post y usar https://cfdi.express/blog/acceso-contador-sin-compartir-contrasena solo cuando la URL responda. Mientras tanto, enlazar la guía y el dashboard.
- [ ] Confirmar que la guía https://cfdi.express/docs/equipo-y-workspaces ya está en línea (sale en el otro PR).
- [ ] Revisar que el copy diga: invitación por correo, 7 días, mismo correo, facturación en solo lectura, saldo y facturación del dueño, API keys sin cambio, conexiones existentes ligadas al workspace propio.
- [ ] CTA en cada red: dashboard o guía.
