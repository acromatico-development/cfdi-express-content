# Configuración Inicial

Para poder completar la configuración inicial de CFDI Express debes:

1. Seleccionar si quieres modo PAC o modo Flow.
![Selección PAC - Flow](https://cdn.shopify.com/s/files/1/0804/5540/1763/files/Acr_2025-07-17_at_16.46.41_2x_3663a166-5e89-4f17-b897-e0070bf0944d.png?v=1752792420)

2. Llenar el formulario correspondiente al modo del app que seleccionaste.

    En **Modo PAC**, el orden es: razón social, RFC y código postal; **Régimen Fiscal** en su propia fila; el [correo del responsable fiscal](/docs/correo-del-responsable-fiscal), que es opcional; el bloque **Certificado de Sello Digital**, con la contraseña, la llave (`.key`) y el certificado (`.cer`) juntos (la contraseña es requerida); y el **Logo** en su propia sección, fuera de ese bloque. En el onboarding no se muestra la fecha de vencimiento, porque todavía no hay un CSD previo.

    En **Modo Flow** no se piden contraseña ni archivos del CSD. Sí se piden los datos fiscales, el correo del responsable (opcional) y el logo.

    - Formulario Modo PAC
      <!-- TODO: captura desactualizada (17 jul 2025) — muestra la contraseña junto al régimen y el logo en la misma fila que la llave y el certificado, con el encabezado «Archivos». Hay que rehacerla con el bloque «Certificado de Sello Digital» (contraseña + llave + certificado) y el logo aparte. También falta el campo del correo del responsable. Se deja la imagen actual hasta tener la nueva. -->
      ![Formulario PAC](https://cdn.shopify.com/s/files/1/0804/5540/1763/files/Acr_2025-07-17_at_16.49.14_2x_ef94ccd8-a733-4925-a040-dcc9e5abf9a8.png?v=1752792575)
    - Formulario Modo Flow
      <!-- TODO: captura desactualizada (17 jul 2025) — no muestra el encabezado «Logo» ni el campo del correo del responsable. Se deja la imagen actual hasta tener la nueva. -->
      ![Formulario Flow](https://cdn.shopify.com/s/files/1/0804/5540/1763/files/Acr_2025-07-17_at_16.49.37_2x_4c1591ac-7bde-4864-9203-9259758c8113.png?v=1752792590)

3. Después de haber hecho el onboarding, puedes modificar cualquiera de los datos configurados y definir el código de producto y código de servicio del SAT predeterminados en la sección de configuraciones de la aplicación. Ahí también podrás definir si quieres solicitar la Constancia de Situación Fiscal (CSF) de los clientes cuando generan una factura.

Puedes ver nuestro video introductorio si lo prefieres.

- Embed:
  <iframe width="100%" style="aspect-ratio:16/9;" src="https://www.youtube.com/embed/g7wiNKPqh8k?si=lBq_dIJUD5BsB2ZM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
- Link:
  https://www.youtube.com/watch?v=g7wiNKPqh8k

## Información Requerida del SAT para Onboarding

Para poder configurar CFDI Express, requieres los siguientes datos:

- RFC
- Razón Social
- Código postal (El que aparece en tu Constancia de Situación Fiscal)
- Régimen Fiscal (El que aparece en tu Constancia de Situación Fiscal)
- Certificado de Sello Digital (estas tres van juntas en el formulario, en el bloque «Certificado de Sello Digital»; solo en Modo PAC):
  - Contraseña
  - Llave (archivo .key)
  - Certificado (archivo .cer)
- Logo (en su propia sección, no va con el CSD)

Junto a los datos fiscales también verás el campo **Correo del contador o equipo responsable de CFDI Express (opcional)**, en Modo PAC y en Modo Flow. No es obligatorio para terminar el alta: puedes dejarlo vacío y agregarlo después en [Configuraciones](/docs/configuraciones). Ese correo recibe copia de los avisos de la tienda. Ver [Correo del responsable fiscal](/docs/correo-del-responsable-fiscal).

Tu CSD tiene fecha de vencimiento. Cuando se acerque, la app te avisa con un banner y por correo. Ver [Avisos de CSD por caducar](/docs/avisos-de-csd-por-caducar).

Al guardar, el certificado y su contraseña no se envían de vuelta al navegador. Si ya está cargado, en [Configuraciones](/docs/configuraciones), en el bloque **Certificado de Sello Digital**, verás **Llave del CSD actual cargada** y **Certificado del CSD actual cargado**, más la fecha de vencimiento. Solo subes archivos cuando lo vas a cambiar.

Si requieres generar tu certificado de sello digital y no sabes cómo puedes ver el siguiente video tutorial:

- Embed:
  <iframe width="100%" style="aspect-ratio:16/9;" src="https://www.youtube.com/embed/e4W7Q5a0ULs?si=P2R7F6MM0SR7--HJ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
- Link:
  https://www.youtube.com/watch?v=e4W7Q5a0ULs
