# ADR-W-013 — Centro de ayuda, destinatarios guiados y cabeceras de seguridad

14/11/2026 · Estado: aceptada por Oscar · Autor: Claude (asistente de desarrollo)

**Contexto.** La web se entrega a ingenieros de sistemas que la probarán con cuentas de administrador y con datos y
fotos reales, y que pasarán una auditoría de seguridad. Hacían falta:

- manuales en PDF dentro de la web;
- pasos cortos en cada pantalla y mensajes claros cuando alguien se equivoca (sobre todo al registrar destinatarios de
  Telegram);
- más contexto en la auditoría;
- quitar de la vista el enlace a Django Admin;
- cabeceras de seguridad que se puedan comprobar desde el navegador.

**Decisión.**

1. **Ayuda de cada pantalla.** `web/ayudas.py` es la única fuente de los pasos y de «Si algo no funciona». La etiqueta
   `{% ayuda_pagina %}` (en `base.html`, sin consultas) muestra el recuadro plegable «Cómo se usa esta página». La
   página `/ayuda/` (permiso `ayuda.ver`: Administrador, Especialista y Supervisor) reúne las guías y los tres PDF.
2. **Manuales en PDF** en `web/manuales/`:
   - `Manual_de_usuario_Plataforma_Web.pdf`;
   - `Manual_de_usuario_App_Movil.pdf`;
   - `Documentacion_del_sistema_y_guia_de_pruebas.pdf`.

   Se sirven solo con sesión iniciada (`/ayuda/<manual>.pdf`, `FileResponse` en línea; un nombre desconocido da 404).
   No contienen datos de la infraestructura: ni proveedores, ni direcciones IP, ni rutas, ni comandos, ni variables,
   ni repositorios.
3. **Destinatarios guiados.**
   - La página muestra los pasos y el enlace al bot.
   - «Buscar chats» (`getUpdates`) lista las personas y grupos que escribieron al bot en las últimas 24 h; «Usar este
     chat» llena el formulario.
   - «Enviar prueba» (`sendMessage`) se registra en la auditoría como `DESTINATARIO_PRUEBA`.
   - El formulario explica el error si se escribe un celular o un @usuario en lugar del número de chat, y avisa al
     guardar sin consentimiento o inactivo.
   - `notificaciones.telegram.explicar_error` traduce los errores de Telegram.
   - Solo el ADMINISTRADOR (`destinatarios.gestionar`); el token nunca sale del servidor.
4. **Auditoría con contexto.** `web/auditoria_textos.py` explica cada acción en español y la agrupa en categorías
   (ingresos, cuentas, casos, fotos, campo, catálogos, avisos, IA). La página tiene tres tarjetas (para qué sirve, cómo
   leer una fila, garantías), filtro por categoría y el ID de la entidad como enlace para ver toda su historia.
5. **Django Admin fuera de la vista.** Se quitan los enlaces «Gestión» del menú y «Gestionar modelos» de Estado de la
   IA. La ruta sigue para el responsable técnico (solo superusuario).
6. **Cabeceras de seguridad** (`web.middleware.CabecerasSeguridadMiddleware`):
   - **Content-Security-Policy:** `default-src 'self'`; `script-src 'self'` sin `unsafe-eval`; imágenes solo del
     propio sitio, de las fotos y de los mapas; `frame-ancestors 'none'`; `object-src 'none'`; `form-action 'self'`.
   - **Permissions-Policy:** cámara, micrófono, ubicación y sensores desactivados.
   - **Cookies y HSTS:** `CSRF_COOKIE_HTTPONLY`, y HSTS de un año por defecto con HTTPS.
   - **HTMX sin `eval`:** los filtros `[...]` de `hx-trigger` se reemplazan por `data-solo-visible` y una cancelación
     en `htmx:beforeRequest` (app.js), con `allowEval:false` y `allowScriptTags:false`.
   - **Proxy:** quita la cabecera `Server`.

**Consecuencias.**

- Sin migraciones ni dependencias nuevas (W-21). La bandeja sigue en 6 consultas.
- Si se agrega un origen de imágenes, se declara en `CSP_IMG_EXTRA`.
- Si cambia una pantalla, se actualizan su texto en `web/ayudas.py` y los PDF.
