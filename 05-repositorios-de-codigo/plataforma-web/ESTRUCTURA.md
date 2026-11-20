# Estructura del repositorio de la plataforma web (Django)

Repositorio **`agricola-riachuelo-platform`** (GitHub `OskitarBB/agricola-riachuelo-platform`, rama de trabajo `prueba2`). Plataforma Django 5.2 que expone la API `/api/v1` para la app, la web de revisión (Django Templates + HTMX + Leaflet), el worker de IA y los avisos por Telegram. Versión documentada: **web v1.6.1** (último commit local revisado: `97575dc`, 14/11/2026).

Cómo está organizado: un proyecto Django (`config/`) con **una app de Django por dominio** (`cuentas`, `campo`, `monitoreo`, `evidencias`, `ia`, `revision`, `notificaciones`, `auditoria`), más `api` (contrato con la app), `web` (vistas y plantillas), `diagnostico` y `simulador`. Cada app de dominio tiene `models.py` (tablas), `services.py` (reglas de negocio: **la única forma permitida de cambiar datos**), `admin.py`, `migrations/` y `tests/`. Las vistas web y la API nunca escriben en los modelos directamente: llaman a los servicios.

Flujo dentro del código: `api/v1/views.py` recibe la sincronización → `monitoreo/services.py` y `evidencias/services.py` guardan → al confirmar cada foto se crea una tarea en `ia` → `ia/management/commands/worker_ia.py` la procesa con `ia/inference/onnx.py` → `revision/services.py` aplica el triage y abre/actualiza el caso → si queda confirmado, `notificaciones/services.py` + `telegram.py` avisan → todo se registra en `auditoria`. La web (`web/views.py`, `web/queries.py`, `web/permissions.py`) muestra y decide.

## Árbol completo de archivos versionados

Generado con `git ls-files` (sin `node_modules/` ni librerías de terceros copiadas en `vendor/`).

```text
├── api/
│   ├── tests/
│   │   ├── __init__.py
│   │   ├── test_api.py
│   │   └── test_ubicar_plaga.py
│   ├── v1/
│   │   ├── __init__.py
│   │   ├── serializers.py
│   │   ├── urls.py
│   │   └── views.py
│   ├── __init__.py
│   ├── apps.py
│   ├── authentication.py
│   ├── errors.py
│   ├── permissions.py
│   └── schema.py
├── auditoria/
│   ├── migrations/
│   │   ├── 0001_initial.py
│   │   ├── 0002_initial.py
│   │   ├── 0003_tabla_cache.py
│   │   ├── 0004_auditoria_solo_insercion.py
│   │   └── __init__.py
│   ├── tests/
│   │   ├── __init__.py
│   │   └── test_seguridad_bd.py
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── seguridad_bd.py
│   └── services.py
├── campo/
│   ├── management/
│   │   ├── commands/
│   │   │   ├── __init__.py
│   │   │   └── crear_lotes_estandar.py
│   │   └── __init__.py
│   ├── migrations/
│   │   ├── 0001_initial.py
│   │   ├── 0002_restricciones_check.py
│   │   ├── 0003_catalogo_activo.py
│   │   ├── 0004_puntos_del_fundo.py
│   │   ├── 0005_largo_de_hilera.py
│   │   └── __init__.py
│   ├── tests/
│   │   ├── __init__.py
│   │   ├── test_catalogos.py
│   │   ├── test_mapa_satelital.py
│   │   └── test_segmentos_fijos.py
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   └── services.py
├── config/
│   ├── __init__.py
│   ├── asgi.py
│   ├── restricciones.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── cuentas/
│   ├── migrations/
│   │   ├── 0001_initial.py
│   │   ├── 0002_restricciones_check.py
│   │   └── __init__.py
│   ├── tests/
│   │   ├── __init__.py
│   │   ├── test_editar_cuentas.py
│   │   └── test_reglas_cuentas.py
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── services.py
│   └── validators.py
├── deploy/
│   ├── modelos/
│   │   └── LEEME.md
│   ├── actualizar.sh
│   ├── desinstalar.sh
│   ├── docker-compose.yml
│   ├── env.piloto.plantilla
│   ├── instalar.sh
│   └── LEEME.md
├── diagnostico/
│   ├── management/
│   │   ├── commands/
│   │   │   ├── __init__.py
│   │   │   ├── comprobar_bd.py
│   │   │   ├── diagnostico.py
│   │   │   ├── mantenimiento.py
│   │   │   └── respaldo.py
│   │   └── __init__.py
│   ├── __init__.py
│   ├── apps.py
│   ├── arranque.py
│   ├── bd.py
│   ├── checks.py
│   ├── contexto.py
│   ├── logs.py
│   ├── middleware.py
│   ├── urls.py
│   └── views.py
├── docs/
│   ├── adr/
│   │   ├── ADR-W-001-cloudinary-simulado.md
│   │   ├── ADR-W-002-actividad-en-vivo.md
│   │   ├── ADR-W-003-subida-multipart-v1.md
│   │   ├── ADR-W-004-integridad-en-la-base.md
│   │   ├── ADR-W-005-alta-de-cuentas-y-tipos.md
│   │   ├── ADR-W-006-gestion-de-catalogos.md
│   │   ├── ADR-W-007-alertas-ia-mapa-satelital-ubicar-plaga.md
│   │   ├── ADR-W-008-triage-ia-y-limpieza-de-fotos.md
│   │   ├── ADR-W-009-decision-por-lugar-dos-camaras.md
│   │   ├── ADR-W-010-segmentos-fijos-por-hilera.md
│   │   ├── ADR-W-011-avisos-por-telegram.md
│   │   ├── ADR-W-012-papelera-de-fotos.md
│   │   └── ADR-W-013-ayuda-manuales-y-cabeceras.md
│   ├── img/
│   │   ├── web/
│   │   │   ├── en-vivo-panel.webp
│   │   │   ├── en-vivo.webp
│   │   │   ├── movil-caso.webp
│   │   │   ├── movil-panel.webp
│   │   │   ├── pc-bandeja.webp
│   │   │   ├── pc-caso.webp
│   │   │   ├── pc-esp-422.webp
│   │   │   ├── pc-esp-atajos.webp
│   │   │   ├── pc-login.webp
│   │   │   ├── pc-mapa.webp
│   │   │   ├── pc-panel.webp
│   │   │   ├── pc-plano.webp
│   │   │   ├── pc-usuarios.webp
│   │   │   └── tablet-panel.webp
│   │   ├── arquitectura_logica_v13.png
│   │   ├── arquitectura_produccion.png
│   │   └── arquitectura_produccion_v13.png
│   ├── perf/
│   │   └── datos_sinteticos.sql
│   ├── ARQUITECTURA_DATOS.md
│   ├── CONTEXTO_WEB.md
│   ├── INTEGRACION_APP.md
│   ├── Plataforma_Riachuelo_como_funciona.pdf
│   ├── Plataforma_Riachuelo_como_funciona_v2.pdf
│   ├── REPORTE_AVANCE.md
│   └── TELEGRAM.md
├── evidencias/
│   ├── migrations/
│   │   ├── 0001_initial.py
│   │   ├── 0002_restricciones_check.py
│   │   ├── 0003_fotos_eliminadas.py
│   │   ├── 0004_papelera.py
│   │   └── __init__.py
│   ├── tests/
│   │   ├── __init__.py
│   │   ├── test_limpieza.py
│   │   └── test_papelera.py
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── limpieza.py
│   ├── media.py
│   ├── models.py
│   ├── nube.py
│   ├── papelera.py
│   └── services.py
├── ia/
│   ├── inference/
│   │   ├── weights/
│   │   │   └── LEEME.md
│   │   ├── __init__.py
│   │   ├── onnx.py
│   │   ├── simulado.py
│   │   └── yolo.py
│   ├── management/
│   │   ├── commands/
│   │   │   ├── __init__.py
│   │   │   ├── exportar_dataset.py
│   │   │   └── worker_ia.py
│   │   └── __init__.py
│   ├── migrations/
│   │   ├── 0001_initial.py
│   │   ├── 0002_restricciones_check.py
│   │   ├── 0003_umbral_auto_y_estados_v13.py
│   │   ├── 0004_triage_tres_franjas.py
│   │   ├── 0005_minimo_cajas_confirmacion.py
│   │   ├── 0006_minimo_cajas_para_caso.py
│   │   └── __init__.py
│   ├── tests/
│   │   ├── __init__.py
│   │   ├── test_exportar_dataset.py
│   │   ├── test_inferencia.py
│   │   └── test_services.py
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   └── services.py
├── monitoreo/
│   ├── migrations/
│   │   ├── 0001_initial.py
│   │   ├── 0002_restricciones_check.py
│   │   └── __init__.py
│   ├── tests/
│   │   └── __init__.py
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   └── services.py
├── notificaciones/
│   ├── management/
│   │   ├── commands/
│   │   │   ├── __init__.py
│   │   │   └── telegram.py
│   │   └── __init__.py
│   ├── migrations/
│   │   ├── 0001_initial.py
│   │   ├── 0002_restricciones_check.py
│   │   ├── 0003_aviso_confirmado_por_ia.py
│   │   ├── 0004_canal_telegram.py
│   │   ├── 0005_solo_telegram.py
│   │   └── __init__.py
│   ├── tests/
│   │   ├── __init__.py
│   │   ├── test_services.py
│   │   └── test_telegram.py
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── errores.py
│   ├── models.py
│   ├── services.py
│   └── telegram.py
├── revision/
│   ├── management/
│   │   ├── commands/
│   │   │   ├── __init__.py
│   │   │   └── aplicar_triage.py
│   │   └── __init__.py
│   ├── migrations/
│   │   ├── 0001_initial.py
│   │   ├── 0002_restricciones_check.py
│   │   ├── 0003_estados_ia_y_posible_plaga.py
│   │   └── __init__.py
│   ├── tests/
│   │   ├── __init__.py
│   │   ├── test_alertas_ia.py
│   │   ├── test_dos_camaras.py
│   │   ├── test_services.py
│   │   └── test_triage.py
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   └── services.py
├── scripts/
│   ├── empaquetar.sh
│   ├── iniciar.ps1
│   └── iniciar.sh
├── simulador/
│   ├── tests/
│   │   ├── __init__.py
│   │   └── test_simulador.py
│   ├── __init__.py
│   ├── apps.py
│   ├── fotos.py
│   ├── urls.py
│   └── views.py
├── tools/
│   └── ia/
│       ├── cortar_mosaicos.py
│       ├── entrenar_yolo_colab.ipynb
│       ├── LEEME.md
│       └── verificar_onnx.py
├── web/
│   ├── management/
│   │   ├── commands/
│   │   │   ├── __init__.py
│   │   │   ├── medir_rendimiento.py
│   │   │   └── sembrar_demo.py
│   │   └── __init__.py
│   ├── manuales/
│   │   ├── Documentacion_del_sistema_y_guia_de_pruebas.pdf
│   │   ├── Manual_de_usuario_App_Movil.pdf
│   │   └── Manual_de_usuario_Plataforma_Web.pdf
│   ├── static/
│   │   └── web/
│   │       ├── fuentes/
│   │       │   ├── Inter-LICENSE.txt
│   │       │   └── InterVariable-latin.woff
│   │       ├── img/
│   │       │   ├── apple-touch-icon.png
│   │       │   ├── favicon.ico
│   │       │   ├── favicon.png
│   │       │   ├── hoja.svg
│   │       │   ├── logo-riachuelo-2x.webp
│   │       │   └── logo-riachuelo.webp
│   │       ├── sonidos/
│   │       │   ├── confirm.wav
│   │       │   ├── error.wav
│   │       │   ├── notify.wav
│   │       │   ├── success.wav
│   │       │   ├── tap.wav
│   │       │   └── toggle.wav
│   │       ├── app.css
│   │       ├── app.js
│   │       ├── caso.js
│   │       ├── iconos.svg
│   │       └── mapa.js
│   ├── templates/
│   │   ├── web/
│   │   │   ├── partials/
│   │   │   │   ├── actividad_poll.html
│   │   │   │   ├── ayuda_pagina.html
│   │   │   │   ├── bandeja_tabla.html
│   │   │   │   ├── descartadas_resumen.html
│   │   │   │   ├── error.html
│   │   │   │   ├── evento.html
│   │   │   │   ├── mensajes.html
│   │   │   │   ├── panel_decision.html
│   │   │   │   └── telegram_chats.html
│   │   │   ├── 403.html
│   │   │   ├── auditoria.html
│   │   │   ├── ayuda.html
│   │   │   ├── bandeja.html
│   │   │   ├── base.html
│   │   │   ├── cambiar_contrasena.html
│   │   │   ├── captura.html
│   │   │   ├── caso.html
│   │   │   ├── catalogo_hilera.html
│   │   │   ├── catalogo_lote.html
│   │   │   ├── catalogos.html
│   │   │   ├── dashboard.html
│   │   │   ├── descartadas_ia.html
│   │   │   ├── destinatarios.html
│   │   │   ├── dispositivos.html
│   │   │   ├── ia.html
│   │   │   ├── limpieza.html
│   │   │   ├── login.html
│   │   │   ├── mapa.html
│   │   │   ├── notificaciones.html
│   │   │   ├── papelera.html
│   │   │   ├── plano.html
│   │   │   ├── reportes.html
│   │   │   ├── sesion.html
│   │   │   ├── sesion_eliminar.html
│   │   │   ├── sesiones.html
│   │   │   ├── usuario_editar.html
│   │   │   └── usuarios.html
│   │   ├── 404.html
│   │   └── 500.html
│   ├── templatetags/
│   │   ├── __init__.py
│   │   └── web_tags.py
│   ├── tests/
│   │   ├── __init__.py
│   │   ├── factories.py
│   │   ├── test_avisos_simulados.py
│   │   ├── test_ayuda.py
│   │   ├── test_descartadas_ia.py
│   │   ├── test_extras.py
│   │   └── test_views.py
│   ├── __init__.py
│   ├── apps.py
│   ├── auditoria_textos.py
│   ├── ayudas.py
│   ├── context_processors.py
│   ├── demo.py
│   ├── errors.py
│   ├── forms.py
│   ├── messages.py
│   ├── middleware.py
│   ├── permissions.py
│   ├── queries.py
│   ├── urls.py
│   └── views.py
├── .dockerignore
├── .env.example
├── .gitattributes
├── .gitignore
├── AGENTS.md
├── CLAUDE.md
├── Dockerfile
├── iniciar.bat
├── manage.py
├── README.md
├── requirements-ia.txt
└── requirements.txt
```

## Carpeta por carpeta

### `(raíz)`

Archivos de arranque y configuración del proyecto Django: `manage.py`, `Dockerfile` (imagen única para web y worker), dependencias (`requirements.txt` y `requirements-ia.txt` para onnxruntime), `.env.example` (plantilla sin secretos; el `.env` real nunca va a Git), `iniciar.bat` (arranque local en Windows) y las instrucciones para agentes (`AGENTS.md`/`CLAUDE.md`, que obligan a leer el Maestro Web).

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `.dockerignore` |  |  |
| `.env.example` |  |  |
| `.gitattributes` |  |  |
| `.gitignore` |  |  |
| `AGENTS.md` | 20 | Instrucciones para agentes de IA |
| `CLAUDE.md` | 20 | Instrucciones para agentes de IA |
| `Dockerfile` | 31 | Imagen única de la plataforma (web + API con gunicorn, y worker_ia). La usan deploy/docker-compose.yml en el VPS. Sin secretos: el .env.piloto se inyecta al arrancar, nunca se copia a la imagen (.dockerignore). |
| `README.md` | 193 | Agrícola Riachuelo · Plataforma de monitoreo fitosanitario de la vid |
| `iniciar.bat` | 5 | rem Arranque local en Windows: doble clic o "iniciar.bat" en la terminal. Opciones: -Demo, -IA, -SinWorker, -Puerto 8080 |
| `manage.py` | 21 | !/usr/bin/env python |
| `requirements-ia.txt` | 7 |  |
| `requirements.txt` | 16 |  |

### `config/`

Proyecto Django: `settings.py` (lee variables con django-environ; SQLite en desarrollo y PostgreSQL de Supabase en el piloto; cabeceras de seguridad, JWT, Cloudinary, Telegram), `urls.py` (monta la web, `/api/v1`, `/gestion/` = Django Admin solo para superusuarios, diagnóstico y simulador) y `restricciones.py` (restricciones CHECK que se aplican a la base en las migraciones `0002_restricciones_check`).

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `config/__init__.py` |  |  |
| `config/asgi.py` | 6 |  |
| `config/restricciones.py` | 48 | Restricciones CHECK en la base de datos para los campos con estados controlados. Los TextChoices de Django solo validan en formularios y serializadores; la base de datos aceptaría cualquier texto. Este decorador agrega, a partir de la |
| `config/settings.py` | 296 | Configuración de la plataforma Django (API /api/v1, web de revisión y worker de IA). Todo lo sensible sale de variables de entorno (sección 5.5 del Maestro Web). Nunca subir .env a Git. Base: Anexo B.1 del Maestro Web v1.0. Agregados de es |
| `config/urls.py` | 31 | Rutas raíz: /gestion/ (Django Admin), /api/v1/ (app móvil), / (web de revisión). |
| `config/wsgi.py` | 6 |  |

### `api/`

Contrato `/api/v1` que consume la app móvil (Maestro App Móvil §15). `v1/views.py` (endpoints: health, auth, bootstrap de catálogos, sesiones, pasadas, secuencias en lote, incidencias, ticket de subida a Cloudinary, confirmación de foto, borrados ordenados y alertas para «Ubicar plaga»), `v1/serializers.py` (validación y DTO), `authentication.py` (JWT + `X-Device-Id`, celulares revocados), `permissions.py`, `errors.py` (cuerpo de error único `ApiErrorBody` con `traceId`) y `schema.py` (OpenAPI con drf-spectacular). Regla: se pueden agregar campos y rutas, nunca quitar ni renombrar (hay celulares con versiones anteriores).

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `api/__init__.py` |  |  |
| `api/apps.py` | 10 |  |
| `api/authentication.py` | 64 | JWT de SimpleJWT + X-Device-Id no revocado (§28.6 del maestro móvil). Respuestas del contrato: token ausente, inválido o vencido → 401 TOKEN_EXPIRED (la app renueva y reintenta); cuenta que ya no puede usar la app → 403 ACCOUNT_BLOCKED  |
| `api/errors.py` | 121 | Errores del contrato /api/v1 (Maestro App Móvil §15.1 y §15.3, ApiErrorBody). Toda respuesta de error de la API tiene la forma { code, message, fieldErrors?, traceId } con un código de ApiErrorCode. Los servicios que atienden a la app (cuentas, |
| `api/permissions.py` | 28 | QUÉ HACE: v1.3 (ADR-W-007) permisos por rol dentro de la API de la app. · FieldWork: monitoreo y sincronización (sesiones, pasadas, secuencias, incidencias, fotos) → solo operador de campo y administrador. El especialista ingresa a la app  |
| `api/schema.py` | 12 | Describe en el OpenAPI (/api/v1/schema) la autenticación de la app: JWT Bearer + X-Device-Id. |
| `api/tests/__init__.py` |  |  |
| `api/tests/test_api.py` | 486 | Contrato /api/v1 de la app móvil (Maestro App Móvil §15): forma de errores, autenticación con celular, sincronización idempotente y flujo de fotos ticket → Cloudinary → confirmación → cola de IA. |
| `api/tests/test_ubicar_plaga.py` | 48 | QUÉ HACE: v1.3 (ADR-W-007) prueba GET /api/v1/mobile/pest-reports («Ubicar plaga»): qué alertas salen (confirmadas, posibles y en revisión; no descartadas), los datos de ubicación y capas del fundo, y que el especialista ingres |
| `api/v1/__init__.py` |  |  |
| `api/v1/serializers.py` | 287 | DTO del contrato /api/v1 en camelCase (src/api/dto.ts del Maestro App Móvil, §15.3). Solo validan forma; las reglas (estados, idempotencia, firmas) están en los servicios de cada app. |
| `api/v1/urls.py` | 37 | Rutas del contrato /api/v1 (Maestro App Móvil §15.2). La app llama SIN barra final (p. ej. /api/v1/auth/login); se acepta también con barra para que nada dependa de APPEND_SLASH. |
| `api/v1/views.py` | 396 | Endpoints /api/v1 de la app móvil (Maestro App Móvil §15.2). Cada vista: valida la forma con su serializador → llama al servicio de su app → responde el DTO del contrato. Hasta la v1.2 no exponían URLs de fotos, estados de IA ni datos de revi |

### `cuentas/`

Usuarios, roles (ADMINISTRADOR, ESPECIALISTA_FITOSANITARIO, SUPERVISOR, OPERADOR_CAMPO), celulares registrados y pedidos de contraseña. `services.py` concentra las reglas (alta de cuentas de la web por el administrador, aprobación de cuentas de la app sin rol → Operador de campo, bloqueo por intentos, contraseña temporal, revocación de celulares).

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `cuentas/__init__.py` |  |  |
| `cuentas/admin.py` | 51 |  |
| `cuentas/apps.py` | 7 |  |
| `cuentas/migrations/0001_initial.py` | 288 | Generated by Django 5.2.17 on 2026-10-05 00:47 |
| `cuentas/migrations/0002_restricciones_check.py` | 59 | Generated by Django 5.2.17 on 2026-10-05 05:55 |
| `cuentas/migrations/__init__.py` |  |  |
| `cuentas/models.py` | 183 | Usuarios, roles y dispositivos (tablas users, user_roles, devices, password_reset_requests). |
| `cuentas/services.py` | 458 | Reglas de cuentas y celulares. · Acciones del ADMINISTRADOR en la web (WEB-13, WEB-14; Anexo C.5 del Maestro Web). · v1.1 (ADR-W-005): alta de cuentas desde la web y tipos de cuenta (app o web) — validate_roles, create_account. · v1.3 (AD |
| `cuentas/tests/__init__.py` |  |  |
| `cuentas/tests/test_editar_cuentas.py` | 98 | QUÉ HACE: v1.3 (ADR-W-007) prueba que solo el administrador edita datos de ingreso y asigna contraseñas a otras cuentas, con validaciones, cierre de sesión en la app y auditoría. |
| `cuentas/tests/test_reglas_cuentas.py` | 59 | v1.1 (ADR-W-005): tipos de cuenta y alta desde la web, a nivel de servicio. |
| `cuentas/validators.py` | 21 | Misma política que la app (validatePassword, Anexo C.2 del maestro móvil): al menos 8 caracteres (MinimumLengthValidator), letras y números, sin espacios al inicio ni al final. |

### `campo/`

Catálogos del fundo: lotes, hileras (con largo y número de plantas), 10 segmentos fijos de ≈20 m por hilera (ADR-W-010), marcadores, puntos del fundo y perfiles de calidad. Los catálogos se desactivan, nunca se borran; sus IDs no cambian. Comando `crear_lotes_estandar`.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `campo/__init__.py` |  |  |
| `campo/admin.py` | 44 | Catálogos en Django Admin (/gestion/, solo superusuarios). v1.2 (ADR-W-006, W-03): el día a día se hace en la web (Administración → Catálogos, campo/services.py). Aquí no se borra nada («eliminar» = desactivar) y el ID no se edita: pasadas, se |
| `campo/apps.py` | 7 |  |
| `campo/management/__init__.py` |  |  |
| `campo/management/commands/__init__.py` |  |  |
| `campo/management/commands/crear_lotes_estandar.py` | 91 | v1.4 (ADR-W-010): carga los lotes estándar del fundo. python manage.py crear_lotes_estandar --simular # muestra lo que haría, sin escribir python manage.py crear_lotes_estandar # 10 lotes «LOTE 01»…«LOTE 10»  |
| `campo/migrations/0001_initial.py` | 166 | Generated by Django 5.2.17 on 2026-10-05 00:47 |
| `campo/migrations/0002_restricciones_check.py` | 42 | Generated by Django 5.2.17 on 2026-10-05 05:55 |
| `campo/migrations/0003_catalogo_activo.py` | 24 | v1.2 (ADR-W-006): segmentos y marcadores se desactivan en vez de borrarse. Solo agrega dos columnas con valor por defecto true (las filas existentes quedan activas). Sin tablas nuevas. |
| `campo/migrations/0004_puntos_del_fundo.py` | 93 | Generated by Django 5.2.17 on 2026-10-08 23:45 |
| `campo/migrations/0005_largo_de_hilera.py` | 14 | v1.4 (ADR-W-010): largo aproximado de la hilera en metros. Migración aditiva (columna NULL, sin tocar datos). |
| `campo/migrations/__init__.py` |  |  |
| `campo/models.py` | 138 | Catálogos del fundo (tablas field_lots, field_rows, field_segments, markers, quality_profiles). |
| `campo/services.py` | 789 | v1.2 (ADR-W-006): reglas de los catálogos del fundo (lotes, hileras, segmentos y marcadores). Reglas que no se negocian (traspaso HANDOFF_CATALOGOS_WEB, §3): · «Eliminar» = desactivar. Nunca se borran filas: hay celulares sin internet con e |
| `campo/tests/__init__.py` |  |  |
| `campo/tests/test_catalogos.py` | 146 | v1.2 (ADR-W-006): reglas de campo/services.py (IDs, validaciones, cascadas, auditoría). |
| `campo/tests/test_mapa_satelital.py` | 108 | QUÉ HACE: v1.3 (ADR-W-007) prueba la edición del mapa satelital: contorno de lotes, inicio y fin de hileras (crea los marcadores si faltan), puntos con nombre, permisos y la vista JSON. |
| `campo/tests/test_segmentos_fijos.py` | 150 | v1.4 (ADR-W-010): cantidad fija de segmentos por hilera, largo en metros, reparto estándar, estandarizar un lote, coordenadas al dividir y el comando crear_lotes_estandar. |

### `monitoreo/`

Lo que sincroniza el controlador: sesiones de monitoreo, cámaras de la sesión, pasadas (LATERAL_A / LATERAL_B), cambios de marcador, secuencias (con GPS del controlador) e incidencias. La web no crea ni edita datos de campo (RN-W08).

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `monitoreo/__init__.py` |  |  |
| `monitoreo/admin.py` | 7 |  |
| `monitoreo/apps.py` | 7 |  |
| `monitoreo/migrations/0001_initial.py` | 473 | Generated by Django 5.2.17 on 2026-10-05 00:47 |
| `monitoreo/migrations/0002_restricciones_check.py` | 214 | Generated by Django 5.2.17 on 2026-10-05 05:55 |
| `monitoreo/migrations/__init__.py` |  |  |
| `monitoreo/models.py` | 221 | Lo que sincroniza la app (tablas monitoring_sessions, session_devices, monitoring_passes, marker_changes, capture_sequences, incidents). La web solo LEE estas tablas: nunca las edita (RN-W08). Las escribe únicamente la API /api/v1 (monito |
| `monitoreo/services.py` | 238 | Recepción idempotente de lo que sincroniza la app (§15.2, §15.5 y §15.8 del maestro móvil). Upsert por ID: repetir una llamada no duplica y responde lo mismo. Se aceptan actualizaciones de pasadas y secuencias ya recibidas (fotos tardía |
| `monitoreo/tests/__init__.py` |  |  |

### `evidencias/`

Capturas (fotos) y su calidad. Las fotos viven en Cloudinary (`type=authenticated`); la base guarda `public_id`, versión, bytes y formato, nunca URLs. `nube.py` firma tickets y URLs temporales, `media.py` sirve miniaturas, `papelera.py` (papelera de 30 días, ADR-W-012) y `limpieza.py` (limpieza ordenada por el administrador, ADR-W-008).

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `evidencias/__init__.py` |  |  |
| `evidencias/admin.py` | 6 |  |
| `evidencias/apps.py` | 13 |  |
| `evidencias/limpieza.py` | 186 | QUÉ HACE: v1.3.1 (ADR-W-008) borra fotos PARA SIEMPRE, a pedido del ADMINISTRADOR: · una sesión completa (pasadas, secuencias, fotos, análisis, casos, decisiones y avisos de esa sesión); · las fotos descartadas por la IA (o sin indicio |
| `evidencias/media.py` | 6 | URLs firmadas de Cloudinary para la web (W-10, DW-26). Nunca se guardan en la base de datos. Contrato de la sección 12 del Maestro Web: signed_image_url(capture, variant) con variantes miniatura, revision y original. La implementación (Cl |
| `evidencias/migrations/0001_initial.py` | 188 | Generated by Django 5.2.17 on 2026-10-05 00:47 |
| `evidencias/migrations/0002_restricciones_check.py` | 67 | Generated by Django 5.2.17 on 2026-10-05 05:55 |
| `evidencias/migrations/0003_fotos_eliminadas.py` | 94 | Generated by Django 5.2.17 on 2026-10-09 07:11 |
| `evidencias/migrations/0004_papelera.py` | 44 | Generated by Django 5.2.17 on 2026-11-05 15:25 |
| `evidencias/migrations/__init__.py` |  |  |
| `evidencias/models.py` | 119 | Capturas confirmadas y su calidad (tablas captures, quality_results). Una fila en `captures` significa: foto en Cloudinary + confirmación aceptada por Django (RN-23 del maestro móvil). |
| `evidencias/nube.py` | 231 | Único punto que conoce a Cloudinary (§28.7 del Maestro App Móvil, sección 14 del Maestro Web). Dos modos, elegidos UNA vez al arrancar: real → CLOUDINARY_URL válido: el SDK oficial firma tickets, verifica respuestas y genera URLs firmadas  |
| `evidencias/papelera.py` | 85 | QUÉ HACE: v1.5 (ADR-W-012) papelera de fotos, a pedido del ADMINISTRADOR. · enviar(): la foto sale de todas las listas (bandeja, mapa, plano, panel, sesiones, descartadas, exportación y «Ubicar plaga» de la app) y no se analiza. Si es  |
| `evidencias/services.py` | 178 | Ticket de subida y confirmación de capturas (§15.7 y §28.7 del maestro móvil). 1. upload_ticket: valida padres (sesión, pasada, secuencia), md5 y tamaño; si la captura ya está confirmada igual responde alreadyConfirmed; si no, firma lo |
| `evidencias/tests/__init__.py` |  |  |
| `evidencias/tests/test_limpieza.py` | 154 | QUÉ HACE: v1.3.1 (ADR-W-008) prueba la limpieza de fotos: borrar una sesión completa (con casos, decisiones y avisos), borrar las descartadas por la IA antiguas sin tocar fotos con caso, el registro para los celulares (GET / |
| `evidencias/tests/test_papelera.py` | 134 | QUÉ HACE: v1.5 (ADR-W-012) prueba la papelera de fotos: la foto (y su caso) salen de todas las listas, no se analizan ni se deciden, se restauran tal cual, se borran para siempre (a mano, al vaciar o solas a los PAPELERA_DIA |

### `ia/`

Inteligencia artificial: modelos configurados (clases, umbrales de triage, mosaicos), tareas `ai_tasks`, detecciones y el **worker** (`management/commands/worker_ia.py`, cola con `SELECT … FOR UPDATE SKIP LOCKED`). Detectores en `inference/`: `onnx.py` (producción, YOLO26n en ONNX Runtime con mosaicos de 1280 px y 20 % de solape), `simulado.py` (desarrollo) y `yolo.py` (ultralytics). Los pesos `.onnx` no se versionan. `exportar_dataset` genera datos de reentrenamiento a partir de las decisiones del especialista.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `ia/__init__.py` |  |  |
| `ia/admin.py` | 22 |  |
| `ia/apps.py` | 7 |  |
| `ia/inference/__init__.py` | 62 | ia/inference — Detectores que usa el worker (§28.8 del maestro móvil; tarea T-W11 del Maestro Web). IA_DETECTOR=simulado → cajas pseudoaleatorias deterministas (solo dev; para probar el flujo sin pesos). IA_DETECTOR=onnx → modelo YOLO exportado a ONNX (Ultraly |
| `ia/inference/onnx.py` | 180 | YOLO exportado a ONNX con onnxruntime (CPU). Formatos de salida admitidos: · Ultralytics YOLOv8 / YOLO11: (1, 4 + clases, N) con cx, cy, w, h y puntaje por clase → umbral + NMS. · Ultralytics YOLO26 / YOLOv10 (end2end, sin NMS): (1, N, 6 |
| `ia/inference/simulado.py` | 48 | Detector SIMULADO (solo APP_ENV=dev): no hay pesos todavía, pero el flujo completo (cola → cajas → caso → bandeja → decisión → aviso) se puede probar. Determinista por captura: la misma foto siempre produce el mismo resultado. Probab |
| `ia/inference/weights/LEEME.md` | 18 | Pesos del modelo de IA |
| `ia/inference/yolo.py` | 39 | Detector con la librería ultralytics (pesos .pt u .onnx). Opcional: requirements-ia.txt. |
| `ia/management/__init__.py` |  |  |
| `ia/management/commands/__init__.py` |  |  |
| `ia/management/commands/exportar_dataset.py` | 180 | QUÉ HACE: baja fotos del piloto (Cloudinary) para entrenar o reentrenar el modelo YOLO, en el formato que leen Roboflow, CVAT y tools/ia/cortar_mosaicos.py. --modo fotos Fotos aceptadas por calidad (filtros por lote |
| `ia/management/commands/worker_ia.py` | 149 | Proceso worker: `python manage.py worker_ia` (28.8 del maestro móvil). Nunca atiende HTTP. En cada vuelta: toma una tarea de ai_tasks (SKIP LOCKED), descarga el original de Cloudinary, ejecuta el detector cargado UNA vez,  |
| `ia/migrations/0001_initial.py` | 194 | Generated by Django 5.2.17 on 2026-10-05 00:47 |
| `ia/migrations/0002_restricciones_check.py` | 49 | Generated by Django 5.2.17 on 2026-10-05 05:55 |
| `ia/migrations/0003_umbral_auto_y_estados_v13.py` | 76 | Generated by Django 5.2.17 on 2026-10-08 23:41 |
| `ia/migrations/0004_triage_tres_franjas.py` | 74 | Generated by Django 5.2.17 on 2026-10-09 07:08 |
| `ia/migrations/0005_minimo_cajas_confirmacion.py` | 53 | 21/10/2026: reglas de confirmación automática por cantidad de cajas (aditiva). · auto_confirm_min_boxes (2): cajas que deben alcanzar el umbral de confirmación automática (una sola no basta). · auto_confirm_many_boxes (3) + auto_confirm_many_threshold (0,50):  |
| `ia/migrations/0006_minimo_cajas_para_caso.py` | 23 | 21/10/2026: «mínimo de cajas para abrir caso» (aditiva). Con menos cajas (≥ umbral de revisión) la IA descarta la foto: una sola caja (p. ej. un reflejo blanco en la uva) ya no abre caso. Los modelos ya registrados quedan con 2. |
| `ia/migrations/__init__.py` |  |  |
| `ia/models.py` | 165 | Modelos de detector, cola de análisis y cajas (tablas model_configs, ai_tasks, detections). |
| `ia/services.py` | 173 | Cola de análisis (§28.8 del maestro móvil; Anexo C.4 del Maestro Web). Una sola fuente de reglas para la API (encolar al confirmar), el worker (tomar, guardar, fallar) y la web (reencolar). |
| `ia/tests/__init__.py` |  |  |
| `ia/tests/test_exportar_dataset.py` | 90 | QUÉ HACE: prueba el comando exportar_dataset (fotos y revisados), con la descarga reemplazada por un JPEG en memoria: nombres de archivo, etiquetas YOLO, cajas rechazadas y negativas. |
| `ia/tests/test_inferencia.py` | 172 | Decodificación de salidas YOLO (ONNX), NMS, recortes, detector ONNX con una sesión falsa (sin pesos reales) y el worker_ia de punta a punta en modo --once. |
| `ia/tests/test_services.py` | 45 |  |

### `revision/`

Casos de revisión y decisiones del especialista (RN-W01 a RN-W07). `services.py` es la única puerta para abrir, decidir o corregir un caso; aplica el triage (descartada por IA · pendiente de revisión · confirmada por IA), la decisión por lugar con las dos cámaras (ADR-W-009) y dispara los avisos. Comando `aplicar_triage` para reevaluar con umbrales nuevos.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `revision/__init__.py` |  |  |
| `revision/admin.py` | 7 |  |
| `revision/apps.py` | 7 |  |
| `revision/management/__init__.py` |  |  |
| `revision/management/commands/__init__.py` |  |  |
| `revision/management/commands/aplicar_triage.py` | 119 | v1.3.1 (ADR-W-008): fija los umbrales del modelo activo y los aplica a los casos «Pendiente de revisión» que abrió la IA. python manage.py aplicar_triage --revision 0.50 --auto 0.85 --simular (solo cuenta, no ca |
| `revision/migrations/0001_initial.py` | 297 | Generated by Django 5.2.17 on 2026-10-05 00:47 |
| `revision/migrations/0002_restricciones_check.py` | 131 | Generated by Django 5.2.17 on 2026-10-05 05:55 |
| `revision/migrations/0003_estados_ia_y_posible_plaga.py` | 92 | Generated by Django 5.2.17 on 2026-10-08 23:41 |
| `revision/migrations/__init__.py` |  |  |
| `revision/models.py` | 130 | Casos y decisiones del especialista (tablas review_cases, human_reviews). |
| `revision/services.py` | 469 | Reglas de la revisión (RN-W01 a RN-W07). La web, Django Admin y cualquier script usan SOLO estas funciones para abrir, decidir o corregir un caso: así la regla se aplica igual en todas partes. v1.3 (ADR-W-007): la IA confirma sola los ca |
| `revision/tests/__init__.py` |  |  |
| `revision/tests/test_alertas_ia.py` | 173 | QUÉ HACE: v1.3 (ADR-W-007) prueba la confirmación automática de la IA (umbral por modelo, aviso sin decisión humana), la decisión «Posible plaga» y la revisión posterior del especialista. |
| `revision/tests/test_dos_camaras.py` | 119 | QUÉ HACE: v1.3.3 (ADR-W-009) prueba que las dos cámaras de un mismo lugar (secuencia) se deciden juntas con la regla «basta una cámara»: un solo caso por lugar, anclado en la foto de mayor confianza; la foto descartada de l |
| `revision/tests/test_services.py` | 209 |  |
| `revision/tests/test_triage.py` | 93 | QUÉ HACE: v1.3.1 (ADR-W-008) prueba el triage en tres franjas (descarta la IA, revisa el especialista, confirma la IA), el comando aplicar_triage y que un descartado se pueda abrir a mano. |

### `notificaciones/`

Destinatarios por lote y avisos. Solo Telegram (SC-02; WhatsApp se retiró en la migración `0005_solo_telegram`). `telegram.py` envía foto + texto + botones «Ver caso» y «Cómo llegar»; comando `telegram` para vincular chats y enviar pruebas.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `notificaciones/__init__.py` |  |  |
| `notificaciones/admin.py` | 6 |  |
| `notificaciones/apps.py` | 7 |  |
| `notificaciones/errores.py` | 9 | Error común de los canales de aviso (Telegram, v1.5 ADR-W-011). |
| `notificaciones/management/__init__.py` |  |  |
| `notificaciones/management/commands/__init__.py` |  |  |
| `notificaciones/management/commands/telegram.py` | 51 | Ayudas para conectar los avisos de Telegram (v1.5, ADR-W-011). python manage.py telegram estado → comprueba el token y muestra el nombre del bot python manage.py telegram chats → chats que escribieron al bot (o  |
| `notificaciones/migrations/0001_initial.py` | 187 | Generated by Django 5.2.17 on 2026-10-05 00:47 |
| `notificaciones/migrations/0002_restricciones_check.py` | 45 | Generated by Django 5.2.17 on 2026-10-05 05:55 |
| `notificaciones/migrations/0003_aviso_confirmado_por_ia.py` | 59 | Generated by Django 5.2.17 on 2026-10-08 23:41 |
| `notificaciones/migrations/0004_canal_telegram.py` | 34 | Generated by Django 5.2.17 on 2026-11-05 15:19 |
| `notificaciones/migrations/0005_solo_telegram.py` | 53 | Generated by Django 5.2.17 on 2026-11-05 16:09 v1.5 (ADR-W-011): los avisos son solo por Telegram. Los avisos de WhatsApp que seguían pendientes pasan a Telegram si el destinatario ya tiene chat; si no, se anulan (NO_APLICA) con el motivo. Los enviados quedan  |
| `notificaciones/migrations/__init__.py` |  |  |
| `notificaciones/models.py` | 94 | Destinatarios y cola de avisos (tablas notification_recipients, notifications). v1.5 (ADR-W-011): los avisos van por Telegram, al chat de cada destinatario. |
| `notificaciones/services.py` | 246 | Avisos posteriores a la confirmación (CU8, RF10 del curso). Desde la v1.3 (ADR-W-007) también se avisa cuando la IA confirma sola un caso de alta confianza (enqueue_for_ai_case). v1.5 (ADR-W-011): los avisos van por Telegram (foto  |
| `notificaciones/telegram.py` | 162 | Avisos por Telegram (v1.5, ADR-W-011). Los avisos de casos los envía solo el worker. Desde la v1.6 (ADR-W-013) la web usa este cliente para dos ayudas del administrador en «Destinatarios»: buscar los chats que escribieron al bot y  |
| `notificaciones/tests/__init__.py` |  |  |
| `notificaciones/tests/test_services.py` | 87 | Cola de avisos (CU8): encolar en la decisión, enviar con el worker, reintentar con espera, error definitivo y arriendo entre workers. v1.5: el canal es Telegram (ADR-W-011). |
| `notificaciones/tests/test_telegram.py` | 146 | v1.5 (ADR-W-011): avisos por Telegram (canal por destinatario, mensaje con lugar, botones y foto con cajas, clasificación de errores de la Bot API y formulario de destinatarios). |

### `auditoria/`

Auditoría de solo inserción (trigger en PostgreSQL que impide UPDATE/DELETE), `services.py` para registrar eventos y `seguridad_bd.py` (RLS sin políticas y REVOKE a `anon`/`authenticated` en Supabase: solo Django lee la base).

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `auditoria/__init__.py` |  |  |
| `auditoria/admin.py` | 23 |  |
| `auditoria/apps.py` | 13 |  |
| `auditoria/migrations/0001_initial.py` | 41 | Generated by Django 5.2.17 on 2026-10-05 00:47 |
| `auditoria/migrations/0002_initial.py` | 39 | Generated by Django 5.2.17 on 2026-10-05 00:47 |
| `auditoria/migrations/0003_tabla_cache.py` | 12 | Crea la tabla de caché (web_cache) dentro de `migrate` para que la alcance el post_migrate de seguridad (RLS). |
| `auditoria/migrations/0004_auditoria_solo_insercion.py` | 53 | audit_events es de solo inserción (Maestro Web, Anexo B: «Solo se inserta; nunca se edita ni se borra»), también a nivel de base de datos (PostgreSQL/Supabase). Un trigger rechaza UPDATE y DELETE. Única e |
| `auditoria/migrations/__init__.py` |  |  |
| `auditoria/models.py` | 24 | Historial de cambios (tabla audit_events). Solo se inserta; nunca se edita ni se borra. |
| `auditoria/seguridad_bd.py` | 31 | Segunda barrera en Supabase (28.4 del maestro móvil): después de cada `migrate` activa RLS en todas las tablas del esquema public y quita privilegios a los roles de la Data API. Django se conecta con el dueño de las tablas, al que R |
| `auditoria/services.py` | 15 | Registro de auditoría. Lo llaman los servicios dentro de su transacción. |
| `auditoria/tests/__init__.py` |  |  |
| `auditoria/tests/test_seguridad_bd.py` | 68 |  |

### `diagnostico/`

Herramientas de operación: comando `diagnostico` (salud de BD, Cloudinary, IA, red), `comprobar_bd`, `mantenimiento`, `respaldo` (pg_dump), middleware de `traceId` por petición y registros.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `diagnostico/__init__.py` |  |  |
| `diagnostico/apps.py` | 13 |  |
| `diagnostico/arranque.py` | 71 | Resumen de configuración al iniciar `runserver` o `worker_ia`: qué base de datos, qué Cloudinary (real o simulado), qué cliente de Telegram y qué detector se usan, y en qué dirección escucha la API. |
| `diagnostico/bd.py` | 104 | Revisa DATABASE_URL antes de conectar y traduce los errores de conexión a pasos concretos. Nunca imprime la contraseña: solo su largo y si tiene caracteres que rompen la URL. |
| `diagnostico/checks.py` | 53 | Comprobaciones de configuración que Django ejecuta en `check`, `runserver` y `migrate`. Errores (E) impiden arrancar en piloto con una configuración insegura o incompleta; avisos (W) se muestran en la consola con la forma de corregirlos |
| `diagnostico/contexto.py` | 22 | traceId de la petición (o de la tarea del worker) disponible en cualquier capa: la API lo devuelve en ApiErrorBody, la auditoría lo guarda y cada línea del registro lo muestra. |
| `diagnostico/logs.py` | 51 | Formato del registro en consola: hora, nivel con color e ícono, origen y traceId. Pensado para leerlo «en vivo» mientras se usa la plataforma (python manage.py runserver / worker_ia). Sin dependencias: códigos ANSI (Windows 10+ los admite |
| `diagnostico/management/__init__.py` |  |  |
| `diagnostico/management/commands/__init__.py` |  |  |
| `diagnostico/management/commands/comprobar_bd.py` | 42 | Prueba la conexión a la base de datos ANTES de migrar y, si falla, dice en una línea qué está mal y cómo arreglarlo (sin mostrar la contraseña). Lo usa iniciar.bat. python manage.py comprobar_bd |
| `diagnostico/management/commands/diagnostico.py` | 418 | Revisión completa de la instalación, en lenguaje claro. python manage.py diagnostico # configuración, base de datos, catálogos, Cloudinary, Telegram, IA y cola python manage.py diagnostico --red # además prueba  |
| `diagnostico/management/commands/mantenimiento.py` | 32 | Limpieza periódica (semanal) de datos técnicos que crecen solos. python manage.py mantenimiento Borra solo datos técnicos vencidos: sesiones web expiradas (django_session), refresh tokens vencidos de la app (t |
| `diagnostico/management/commands/respaldo.py` | 50 | Copia de seguridad de la base de datos con pg_dump. python manage.py respaldo # respaldos/riachuelo-<entorno>-AAAAMMDD-HHMM.dump (formato custom) python manage.py respaldo --carpeta D:\copias El plan Free de Supaba |
| `diagnostico/middleware.py` | 66 | Una línea en consola por petición (método, ruta, estado, tiempo, usuario y traceId), en amarillo los 4xx, en rojo los 5xx y marcadas «LENTA» las que superan LOG_SLOW_MS (RNF-W01). La traza completa de un error 500 la escribe el logg |
| `diagnostico/urls.py` | 8 |  |
| `diagnostico/views.py` | 35 | Errores del navegador (JavaScript, HTMX, fotos o mapa que no cargan) → consola del servidor. Así un problema que ve el especialista en su pantalla queda escrito en la terminal donde corre la plataforma, con la página, el usuario y el tra |

### `simulador/`

Simulador de Cloudinary y de fotos para desarrollo local (ADR-W-001): permite probar la subida y la IA sin servicios reales.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `simulador/__init__.py` |  |  |
| `simulador/apps.py` | 7 |  |
| `simulador/fotos.py` | 154 | Fotos de DEMOSTRACIÓN generadas en el servidor (solo modo simulado de desarrollo). Las capturas de `sembrar_demo` no tienen archivo: para evaluar el diseño sin Cloudinary se dibuja, de forma determinista por public_id, una escena de vid ve |
| `simulador/tests/__init__.py` |  |  |
| `simulador/tests/test_simulador.py` | 145 | Flujo completo de la app contra la laptop sin Cloudinary real (solo APP_ENV=dev): login → sesión → pasada → secuencia → ticket → subida al Cloudinary simulado → confirmación → worker → caso. |
| `simulador/urls.py` | 13 | SOLO desarrollo (APP_ENV=dev). config/urls.py no incluye estas rutas en piloto y las vistas responden 404 si Cloudinary real está configurado. |
| `simulador/views.py` | 114 | Cloudinary simulado para desarrollo (evidencias/nube.py, modo «simulado»). subir: recibe el multipart que la app envía a `ticket.url` (file + los `fields` firmados), comprueba la firma con el mismo algoritmo del SDK, guarda el JPEG en medi |

### `deploy/`

Despliegue en el VPS: `docker-compose.yml` (contenedores `web` y `worker`, red de Traefik, límites de memoria/CPU, solo lectura), `instalar.sh`, `actualizar.sh` (git pull + build + migrate + restart), `desinstalar.sh`, `env.piloto.plantilla` (variables sin valores) y `modelos/` (aquí se copia `modelo.onnx` en el servidor).

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `deploy/LEEME.md` | 48 | Despliegue en el VPS (Hostinger KVM 1, Ubuntu 24.04) — monitoreo.agricolariachuelo.org |
| `deploy/actualizar.sh` | 20 | Aplica cambios del código o de deploy/.env.piloto: reconstruye y reinicia web + worker. Las migraciones se aplican solas al arrancar la web. bash /srv/riachuelo/deploy/actualizar.sh |
| `deploy/desinstalar.sh` | 13 | Retira la plataforma del VPS cuando termine el proyecto y deja el servidor como estaba. No toca OpenClaw ni Traefik. Los datos siguen en Supabase y las fotos en Cloudinary (bórralos en sus paneles si ya no se necesitan, después de expor |
| `deploy/docker-compose.yml` | 85 | Plataforma Agrícola Riachuelo en el VPS, junto a OpenClaw sin tocarlo. web gunicorn (web + API /api/v1). Traefik (el que ya corre en el VPS) le da HTTPS en ${DOMINIO}. worker python manage.py worker_ia (fotos → IA → casos; avisos de |
| `deploy/env.piloto.plantilla` | 46 | instalar.sh la copia a deploy/.env.piloto (secretos del VPS, permisos 600). Nunca a Git ni al chat. Editar: nano /srv/riachuelo/deploy/.env.piloto Aplicar cambios: bash /srv/riachuelo/deploy/actualizar.sh |
| `deploy/instalar.sh` | 157 | Instala (o reinstala) la plataforma en el VPS, junto al Traefik y al OpenClaw que ya corren. bash /srv/riachuelo/deploy/instalar.sh 1) revisa Docker y detecta el Traefik existente (red, entrypoints 80/443 y resolvedor de certificados), 2)  |
| `deploy/modelos/LEEME.md` | 4 | deploy/modelos/ |

### `docs/`

Documentación técnica del repositorio: `CONTEXTO_WEB.md` (contexto del servidor y la web), `ARQUITECTURA_DATOS.md`, `INTEGRACION_APP.md` (cómo conectar la app), `TELEGRAM.md`, `REPORTE_AVANCE.md` (bitácora de versiones, regla W-24), `adr/` (decisiones ADR-W-001 a 014), `img/` (diagramas y capturas), `perf/` (datos sintéticos para medir rendimiento) y `fuente_documentacion/` (generador del PDF «Documentación del sistema y guía de pruebas»). Copias en `05-repositorios-de-codigo/plataforma-web/docs/` de este repo.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `docs/ARQUITECTURA_DATOS.md` | 223 | Arquitectura de datos y seguridad — Supabase + Cloudinary |
| `docs/CONTEXTO_WEB.md` | 1009 | Contexto completo de la plataforma web Riachuelo (estado al 08/10/2026) |
| `docs/INTEGRACION_APP.md` | 102 | Integración con la app móvil (`agricola-riachuelo-mobile`) |
| `docs/Plataforma_Riachuelo_como_funciona.pdf` |  |  |
| `docs/Plataforma_Riachuelo_como_funciona_v2.pdf` |  |  |
| `docs/REPORTE_AVANCE.md` | 433 | Reporte de avance (Maestro Web §23.2) |
| `docs/TELEGRAM.md` | 35 | Avisos por Telegram — cómo activarlos (v1.5, ADR-W-011) |
| `docs/adr/ADR-W-001-cloudinary-simulado.md` | 23 | ADR-W-001 — Cloudinary simulado en desarrollo |
| `docs/adr/ADR-W-002-actividad-en-vivo.md` | 20 | ADR-W-002 — Actividad en vivo en la web (ingresos, cuentas, sesiones y casos) |
| `docs/adr/ADR-W-003-subida-multipart-v1.md` | 20 | ADR-W-003 — Aceptar la subida multipart de la app actual (v1) además del ticket v2.0 |
| `docs/adr/ADR-W-004-integridad-en-la-base.md` | 32 | ADR-W-004 — Integridad y seguridad garantizadas por la base de datos |
| `docs/adr/ADR-W-005-alta-de-cuentas-y-tipos.md` | 35 | ADR-W-005 — Alta de cuentas desde la web y tipos de cuenta (app o web) |
| `docs/adr/ADR-W-006-gestion-de-catalogos.md` | 39 | ADR-W-006 — Gestión de catálogos del fundo desde la web |
| `docs/adr/ADR-W-007-alertas-ia-mapa-satelital-ubicar-plaga.md` | 49 | ADR-W-007 — Alertas automáticas de la IA, mapa satelital, «Ubicar plaga» y edición de cuentas (web v1.3) |
| `docs/adr/ADR-W-008-triage-ia-y-limpieza-de-fotos.md` | 39 | ADR-W-008 — Triage de la IA en tres franjas y limpieza de fotos |
| `docs/adr/ADR-W-009-decision-por-lugar-dos-camaras.md` | 32 | ADR-W-009 — La IA decide por lugar con las dos cámaras |
| `docs/adr/ADR-W-010-segmentos-fijos-por-hilera.md` | 39 | ADR-W-010 — Cantidad fija de segmentos por hilera y lotes estándar |
| `docs/adr/ADR-W-011-avisos-por-telegram.md` | 29 | ADR-W-011 — Avisos por Telegram con foto, texto y enlace |
| `docs/adr/ADR-W-012-papelera-de-fotos.md` | 26 | ADR-W-012 — Papelera de fotos (borrado individual a mano) |
| `docs/adr/ADR-W-013-ayuda-manuales-y-cabeceras.md` | 55 | ADR-W-013 — Centro de ayuda, destinatarios guiados y cabeceras de seguridad |
| `docs/img/arquitectura_logica_v13.png` |  |  |
| `docs/img/arquitectura_produccion.png` |  |  |
| `docs/img/arquitectura_produccion_v13.png` |  |  |
| `docs/img/web/en-vivo-panel.webp` |  |  |
| `docs/img/web/en-vivo.webp` |  |  |
| `docs/img/web/movil-caso.webp` |  |  |
| `docs/img/web/movil-panel.webp` |  |  |
| `docs/img/web/pc-bandeja.webp` |  |  |
| `docs/img/web/pc-caso.webp` |  |  |
| `docs/img/web/pc-esp-422.webp` |  |  |
| `docs/img/web/pc-esp-atajos.webp` |  |  |
| `docs/img/web/pc-login.webp` |  |  |
| `docs/img/web/pc-mapa.webp` |  |  |
| `docs/img/web/pc-panel.webp` |  |  |
| `docs/img/web/pc-plano.webp` |  |  |
| `docs/img/web/pc-usuarios.webp` |  |  |
| `docs/img/web/tablet-panel.webp` |  |  |
| `docs/perf/datos_sinteticos.sql` |  |  |

### `scripts/`

Scripts de arranque para Linux/macOS/PowerShell y `empaquetar.sh`.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `scripts/empaquetar.sh` | 7 | Crea riachuelo-plataforma.tar.gz (código sin secretos, sin .venv ni datos) para subirlo al VPS. |
| `scripts/iniciar.ps1` | 113 |  |
| `scripts/iniciar.sh` | 40 | Arranque local en Linux o macOS (equivalente a iniciar.bat / scripts/iniciar.ps1). ./scripts/iniciar.sh web + API en :8000 y worker de IA en segundo plano DEMO=1 ./scripts/iniciar.sh vuelve a cargar los datos de demostración IA=1 ./scripts |

### `tools/`

`tools/ia/`: entrenamiento del modelo (notebook de Google Colab `entrenar_yolo_colab.ipynb`, `cortar_mosaicos.py` para recortes de 1280 px, `verificar_onnx.py` que exige ≥ 85 % de fotos con plaga detectadas antes de activar un modelo).

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `tools/ia/LEEME.md` | 29 | tools/ia — Entrenar, verificar y activar el modelo YOLO |
| `tools/ia/cortar_mosaicos.py` | 272 | QUÉ HACE: prepara el dataset de entrenamiento de YOLO a partir de fotos completas etiquetadas (formato YOLO: images/ + labels/ o la exportación «YOLO» de Roboflow/CVAT). 1. Junta varias fuentes: --propias (fotos del fundo) y --ext |
| `tools/ia/entrenar_yolo_colab.ipynb` |  |  |
| `tools/ia/verificar_onnx.py` | 204 | QUÉ HACE: prueba un modelo.onnx FUERA del servidor con exactamente el mismo código que usa el worker del VPS (ia.inference.onnx.DetectorOnnx.analizar_imagen: letterbox, decodificación, NMS y tiling). Si aquí funciona, en el worker  |

### `web/`

Vistas y lógica de la web de revisión: `views.py` (todas las vistas; cada una con `@web_view("permiso")`, W-07), `urls.py`, `permissions.py` (matriz de permisos por rol), `queries.py` (consultas optimizadas: la bandeja hace exactamente 6), `forms.py`, `context_processors.py` (no agregar consultas aquí), `middleware.py` (cabeceras de seguridad/CSP), `ayudas.py` (texto de «Cómo se usa esta página»), `auditoria_textos.py`, `messages.py` y `errors.py`.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `web/__init__.py` |  |  |
| `web/apps.py` | 7 |  |
| `web/auditoria_textos.py` | 84 | Qué significa cada evento de la auditoría (v1.6, ADR-W-013). Solo textos para la pantalla Auditoría: el código de la acción se guarda tal cual en audit_events y aquí se explica en lenguaje sencillo. |
| `web/ayudas.py` | 241 | Ayuda corta de cada pantalla (v1.6, ADR-W-013): pasos exactos y qué hacer si algo falla. Es la única fuente de estos textos: los muestra la cabecera de cada página («Cómo se usa esta página»), la página /ayuda/ y los manuales en PDF los repiten |
| `web/context_processors.py` | 15 | web/context_processors.py |
| `web/demo.py` | 259 | Datos de demostración para desarrollar la web sin la app (T-W02). NUNCA en el entorno piloto. Catálogos: lotes del piloto del Anexo D del maestro móvil. Segmentos, marcadores y coordenadas son FICTICIOS (Q-01). Base: Anexo G.2 del Maestro Web. v1 |
| `web/errors.py` | 7 | 403 legible (también para HTMX: el fragmento muestra el motivo en lugar de romper la pantalla). |
| `web/forms.py` | 325 | Formularios de la web. Validan forma; las reglas de negocio están en los servicios. |
| `web/messages.py` | 81 | Textos de la web que no son etiquetas de campos (es-PE). Centralizados para revisarlos juntos. |
| `web/middleware.py` | 41 | web/middleware.py |
| `web/permissions.py` | 67 | Matriz única de permisos de la web (sección 7.2). Las vistas, las plantillas y las pruebas leen ESTA tabla; los servicios vuelven a comprobar las reglas críticas (RN-W01) por su cuenta. |
| `web/queries.py` | 371 | Consultas de solo lectura de la web (bandeja, mapa, plano, dashboard, exportaciones, actividad). Reglas: sin N+1 (select_related / agregados en la BD), filtros siempre acotados, nada de escrituras aquí. |
| `web/urls.py` | 57 | Rutas de la web (sección 6.3). Prefijo raíz; la API vive en /api/v1/. |
| `web/views.py` | 1093 | Vistas de la web. Cada vista: decorador @web_view (permiso) → formulario → servicio → plantilla. |

### `web/templates/`

Plantillas Django de la web (HTML del servidor + HTMX). `base.html` es el esqueleto; `partials/` son fragmentos que HTMX refresca (bandeja cada 15 s con exactamente 6 consultas, actividad en vivo, panel de decisión, mensajes).

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `web/templates/404.html` | 18 |  |
| `web/templates/500.html` | 7 |  |
| `web/templates/web/403.html` | 22 |  |
| `web/templates/web/auditoria.html` | 49 |  |
| `web/templates/web/ayuda.html` | 38 |  |
| `web/templates/web/bandeja.html` | 34 |  |
| `web/templates/web/base.html` | 115 | htmx 2 no reemplaza respuestas 4xx por defecto: 403/409/422 traen un fragmento con el motivo (6.4, P-4). |
| `web/templates/web/cambiar_contrasena.html` | 37 |  |
| `web/templates/web/captura.html` | 45 |  |
| `web/templates/web/caso.html` | 75 |  |
| `web/templates/web/catalogo_hilera.html` | 143 |  |
| `web/templates/web/catalogo_lote.html` | 90 |  |
| `web/templates/web/catalogos.html` | 44 |  |
| `web/templates/web/dashboard.html` | 151 |  |
| `web/templates/web/descartadas_ia.html` | 54 |  |
| `web/templates/web/destinatarios.html` | 40 |  |
| `web/templates/web/dispositivos.html` | 23 |  |
| `web/templates/web/ia.html` | 25 |  |
| `web/templates/web/limpieza.html` | 40 |  |
| `web/templates/web/login.html` | 67 |  |
| `web/templates/web/mapa.html` | 98 |  |
| `web/templates/web/notificaciones.html` | 19 |  |
| `web/templates/web/papelera.html` | 42 |  |
| `web/templates/web/partials/actividad_poll.html` | 7 | v1.0+: respuesta del sondeo de actividad (P-2). Se reemplaza a sí mismo con el último id visto; app.js lee los |
| `web/templates/web/partials/ayuda_pagina.html` | 8 |  |
| `web/templates/web/partials/bandeja_tabla.html` | 37 | Se reemplaza a sí mismo: el sondeo usa siempre los filtros y la página vigentes (6.4, patrón P-2). |
| `web/templates/web/partials/descartadas_resumen.html` | 22 | v1.3.2: franja de la bandeja con las últimas fotos descartadas por la IA (se carga aparte con HTMX). |
| `web/templates/web/partials/error.html` | 1 |  |
| `web/templates/web/partials/evento.html` | 6 |  |
| `web/templates/web/partials/mensajes.html` | 1 |  |
| `web/templates/web/partials/panel_decision.html` | 72 |  |
| `web/templates/web/partials/telegram_chats.html` | 14 |  |
| `web/templates/web/plano.html` | 41 |  |
| `web/templates/web/reportes.html` | 32 |  |
| `web/templates/web/sesion.html` | 53 |  |
| `web/templates/web/sesion_eliminar.html` | 30 |  |
| `web/templates/web/sesiones.html` | 27 |  |
| `web/templates/web/usuario_editar.html` | 53 |  |
| `web/templates/web/usuarios.html` | 93 |  |

### `web/static/`

Estáticos propios: `app.css`, `app.js`, `caso.js` y `mapa.js` (todo el JavaScript va aquí: no hay JavaScript en línea, W-17), íconos SVG, fuente Inter local, sonidos y logos. `vendor/` (no listado) contiene HTMX y Leaflet copiados localmente: sin CDN (W-01).

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `web/static/web/app.css` | 1326 | Estilos de la web de revisión (sin framework ni compilación, W-01, DW-21). Tokens en :root con la paleta de la app móvil y del logo (verde hoja, verde profundo, dorado del marco). Los colores de estado siguen DW-22 / W-12: indican ESTA |
| `web/static/web/app.js` | 693 | Comportamiento común de la web (sin frameworks, W-01; sin JavaScript en línea, W-17). Animaciones, sonidos, avisos flotantes, barra de progreso, actividad en vivo, confirmaciones, reporte de errores del navegador a la consola del servid |
| `web/static/web/caso.js` | 232 | Visor de evidencia del caso (zoom, desplazamiento, pantalla completa, cajas de la IA, foto original) y atajos de la decisión. Las cajas viven en un SVG con el viewBox de la imagen analizada, así que escalan junto con la foto sin recalc |
| `web/static/web/fuentes/Inter-LICENSE.txt` | 92 |  |
| `web/static/web/fuentes/InterVariable-latin.woff` |  |  |
| `web/static/web/iconos.svg` |  |  |
| `web/static/web/img/apple-touch-icon.png` |  |  |
| `web/static/web/img/favicon.ico` |  |  |
| `web/static/web/img/favicon.png` |  |  |
| `web/static/web/img/hoja.svg` |  |  |
| `web/static/web/img/logo-riachuelo-2x.webp` |  |  |
| `web/static/web/img/logo-riachuelo.webp` |  |  |
| `web/static/web/mapa.js` | 510 | Mapa satelital del fundo con Leaflet 1.9.4 servido localmente (W-01). · v1.0: un punto por caso; el color indica ESTADO DE REVISIÓN, nunca gravedad (W-12). Borde punteado = ubicación aproximada por marcador. · v1.3 (ADR-W-007): fondo s |
| `web/static/web/sonidos/confirm.wav` |  |  |
| `web/static/web/sonidos/error.wav` |  |  |
| `web/static/web/sonidos/notify.wav` |  |  |
| `web/static/web/sonidos/success.wav` |  |  |
| `web/static/web/sonidos/tap.wav` |  |  |
| `web/static/web/sonidos/toggle.wav` |  |  |

### `web/tests/`

Pruebas de la web (vistas, permisos, ayuda, avisos simulados, `test_extras.py` que verifica que no haya JavaScript en línea).

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `web/tests/__init__.py` |  |  |
| `web/tests/factories.py` | 107 | Datos mínimos y deterministas para las pruebas (sin depender de web/demo.py). |
| `web/tests/test_avisos_simulados.py` | 34 | QUÉ HACE: v1.3.1 prueba que un aviso enviado por la consola (Telegram sin token, ConsoleClient) se muestre como «Simulado (no enviado)» y no como «Enviado». |
| `web/tests/test_ayuda.py` | 211 | v1.6 (ADR-W-013): ayuda de cada página, manuales en PDF, destinatarios guiados, auditoría con contexto y cabeceras de seguridad. |
| `web/tests/test_descartadas_ia.py` | 70 | QUÉ HACE: v1.3.2 prueba la página «Descartadas por la IA»: muestra las fotos que la IA descartó (indicio débil o sin indicios) y sin caso, filtra por resultado y deja abrir la foto para rescatarla. |
| `web/tests/test_extras.py` | 180 | v1.0+: actividad en vivo, reporte de errores del navegador, diagnóstico, datos demo y reglas de frontend del Maestro Web que se pueden verificar leyendo las plantillas (W-01 sin CDN, W-17 sin JS en línea). |
| `web/tests/test_views.py` | 526 |  |

### `web/manuales/`

PDF servidos desde «Ayuda» de la web: manual web, manual app y documentación del sistema/guía de pruebas.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `web/manuales/Documentacion_del_sistema_y_guia_de_pruebas.pdf` |  |  |
| `web/manuales/Manual_de_usuario_App_Movil.pdf` |  |  |
| `web/manuales/Manual_de_usuario_Plataforma_Web.pdf` |  |  |

### `web/management/`

Comandos `sembrar_demo` (SOLO desarrollo; prohibido en el piloto, W-22) y `medir_rendimiento`.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `web/management/__init__.py` |  |  |
| `web/management/commands/__init__.py` |  |  |
| `web/management/commands/medir_rendimiento.py` | 56 | Mide el tiempo de servidor de las pantallas principales (sección 18). Uso: DATABASE_URL=<base con datos (Anexo H.1)> python manage.py medir_rendimiento --email especialista@demo.pe |
| `web/management/commands/sembrar_demo.py` | 35 | Catálogos del piloto y, opcionalmente, cuentas y evidencia de demostración. Prohibido con APP_ENV=piloto (W-22: nunca mezclar datos de prueba con evidencia del piloto). |

### `web/templatetags/`

Filtros de plantilla (`pctnum`, formatos sin localizar para atributos y CSS, W-16).

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `web/templatetags/__init__.py` |  |  |
| `web/templatetags/web_tags.py` | 215 | Insignias de estado (color + icono + texto; nunca solo color: RNF-W06) y utilidades de presentación. Los números que van a atributos o CSS se escriben sin localizar (W-16). |
