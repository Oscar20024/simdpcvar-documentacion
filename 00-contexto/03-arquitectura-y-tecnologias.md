# Arquitectura y tecnologías

## Vista de conjunto

```text
                 CAMPO (sin internet)                               NUBE
 ┌──────────────────────────────────────────┐
 │ Cámara 1 ─┐   Wi-Fi local (router/hotspot)│      HTTPS /api/v1 (JWT + X-Device-Id)
 │ Cámara 2 ─┼─▶ CONTROLADOR (SQLite, GPS)  ─┼──────────────▶ Django 5.2 (web + API) ──▶ Supabase PostgreSQL
 │           │   WS :8765 control            │                    │   ▲                    (RLS, solo Django)
 │           │   HTTP :8766 fotos            │                    │   │ cola ai_tasks
 └───────────┴──────────────────────────────┘                    ▼   │
           │ foto original con ticket firmado (≤ 10 MB)      worker_ia (YOLO26n ONNX)
           └──────────────────────────────────────▶ Cloudinary ◀──┘ descarga por URL firmada
                                                                     │
  Navegador ──HTTPS──▶ Traefik ──▶ Django (HTMX + Leaflet)           └──▶ Telegram Bot API (avisos)
  App «Ubicar plaga» ──▶ GET /mobile/pest-reports · mapa Leaflet embebido + teselas Esri · Google Maps «Cómo llegar»
```

Principios (documento 02, capítulo «Infraestructura y ecosistema»):

1. **Primero sin internet.** El campo nunca depende de la señal; el controlador es la base de datos de campo.
2. **Un solo portero.** App y web solo hablan con Django; la app nunca habla con Supabase ni tiene claves de Cloudinary.
3. **Las fotos no pasan por el servidor.** Van del celular a Cloudinary con un ticket firmado de un solo uso; la base guarda `public_id`, versión, bytes y formato, nunca URLs (W-10). Las URLs para ver fotos se firman al mostrar.
4. **Secretos solo en el servidor** (archivo de entorno con permisos restringidos). Nada en la app, en el navegador ni en Git.

## Componentes

| Componente | Tecnología | Responsabilidad |
|---|---|---|
| App móvil «Riachuelo Monitoreo» | Expo SDK 57, React Native, TypeScript estricto, Expo Router, Zustand, zod, expo-sqlite, expo-camera, expo-location, expo-sensors, expo-secure-store, react-native-tcp-socket, react-native-webview + Leaflet embebido; build con Expo EAS (APK Android, paquete `pe.riachuelo.monitoreo`) | Controlador y cámaras: sesión, vinculación por QR, prueba corta, pasadas, captura MANUAL/AUTOMÁTICO, calidad, repetición, almacenamiento local, sincronización, «Ubicar plaga» |
| Red local entre celulares | Servidor TCP propio en el controlador: WebSocket de control (puerto 8765; latido 2 s, inestable 3 s, perdido 6 s) y HTTP de fotos (puerto 8766, `POST /local/captures`, md5) | Órdenes de captura y transferencia de fotos sin internet |
| API `/api/v1` | Django REST Framework, SimpleJWT (acceso 15 min, renovación 14 días con rotación y lista negra), drf-spectacular (OpenAPI en `/api/v1/schema`) | Contrato con la app. Errores con cuerpo `ApiErrorBody` y `traceId`. Agregar campos sí; quitar o renombrar no |
| Plataforma web | Django 5.2 LTS, plantillas del servidor + HTMX 2, CSS propio, fuente Inter local, Leaflet 1.9.4 local con teselas Esri; gunicorn + WhiteNoise | Panel, bandeja, caso y decisión, descartadas por IA, mapa satelital, plano de hileras, sesiones, reportes CSV, avisos, estado de la IA, usuarios, celulares, destinatarios, catálogos, papelera, limpieza, auditoría, ayuda |
| Worker de IA | `python manage.py worker_ia`; ONNX Runtime + NumPy; cola `ai_tasks` con `SELECT … FOR UPDATE SKIP LOCKED` | Analiza cada foto (mosaicos de 1280 px, 20 % de solape), aplica el triage, abre/actualiza casos, envía avisos y vacía la papelera vencida |
| Base de datos | Supabase PostgreSQL (plan Free, São Paulo); psycopg 3; SQLite en desarrollo | ~38 tablas creadas por migraciones de Django (26 del dominio). CHECK de estados y rangos, auditoría de solo inserción con trigger, RLS sin políticas y REVOKE a `anon`/`authenticated`. Supabase Auth, Storage, Realtime y Data API **no se usan** |
| Fotos | Cloudinary (plan Free), tipo `authenticated`, carpeta `riachuelo/piloto/{sesión}/{pasada}/{captura}`, transformaciones con nombre (miniatura) | Originales privados; miniaturas y foto del aviso por URL firmada |
| Avisos | Telegram Bot API | Mensaje con foto, lote, hilera, lado, segmento, marcador y botones «Ver caso» y «Cómo llegar» |
| Servidor | VPS Hostinger KVM 1 (Ubuntu 24.04, 1 vCPU, 3,8 GB RAM, 48 GB), Docker Compose, Traefik con Let's Encrypt; contenedores `web` (~700 MB, 0,8 CPU) y `worker` (~1,2 GB, 0,7 CPU), usuario sin privilegios y sistema de archivos de solo lectura | Producción del piloto en `monitoreo.agricolariachuelo.org` |
| Monitoreo externo | UptimeRobot (plan gratis, cada 5 min) sobre `/api/v1/health` | Medición de SLA-01; sus correos «DOWN»/«UP» crean y cierran incidentes en Jira (ver `06-…`) |
| Mesa de servicio | Jira Service Management Free, portal «Soporte SIMDPCVAR · Agrícola Riachuelo» | Incidentes (INC) y solicitudes (SOL-01…08) con SLA. La web enlaza al portal con «Reportar un problema» (variable `SOPORTE_URL`, desde v1.6.2) |

## API `/api/v1` (resumen; detalle en `05-repositorios-de-codigo/plataforma-web/docs/INTEGRACION_APP.md` y `04-maestros/MAESTRO_APP_MOVIL.md` §15)

| Método y ruta | Uso |
|---|---|
| `GET /health` | Salud y hora del servidor (`{"status":"UP"}`; 503 sin base) |
| `POST /auth/register` · `login` · `refresh` · `logout` · `GET /auth/me` · `POST /auth/change-password` · `POST /auth/password-reset-requests` | Cuentas y sesión |
| `GET /mobile/bootstrap` | Catálogos activos (lotes, hileras con largo, segmentos, marcadores, laterales) y perfil de calidad, con `catalogVersion` |
| `POST /sessions` · `/sessions/{id}/passes` · `/sessions/{id}/sequences/batch` (≤ 200) · `/sessions/{id}/incidents/batch` (≤ 200) | Sincronización idempotente de lo capturado |
| `POST /captures/{id}/upload-ticket` → subida a Cloudinary → `POST /captures/upload` | Fotos (ticket v2.0; el multipart v1 queda por compatibilidad) |
| `GET /captures/{id}` | Estado de una captura |
| `GET /mobile/pest-reports?days=30` | Alertas para «Ubicar plaga» (desde web v1.3, ADR-W-007) |

Orden de sincronización de la app: SESSION → PASS → SEQUENCE_BATCH → CAPTURE → INCIDENT_BATCH → SESSION_CLOSE. Solo con Wi-Fi, app abierta y sin sesión de monitoreo abierta (o manual desde PANT-20).

## Datos principales

| App Django | Tablas |
|---|---|
| `cuentas` | `users`, `user_roles`, `devices`, `password_reset_requests` |
| `campo` | `field_lots`, `field_rows`, `field_segments`, `markers`, `quality_profiles`, `points_of_interest` |
| `monitoreo` | `monitoring_sessions`, `session_devices`, `monitoring_passes`, `marker_changes`, `capture_sequences`, `incidents` (UUID generados en la app: reenvío idempotente) |
| `evidencias` | `captures` (columnas `cloudinary_*`, sin URL), `quality_results` |
| `ia` | `model_configs` (un solo modelo activo), `ai_tasks` (cola), `detections` (cajas en píxeles del original) |
| `revision` | `review_cases` (un caso por foto), `human_reviews` (historial; una decisión vigente) |
| `notificaciones` | `notification_recipients`, `notification_recipient_lots`, `notifications` (un aviso por decisión y destinatario) |
| `auditoria` | `audit_events` (solo inserción, con `trace_id`) |

Códigos de catálogo estables compartidos por app y web, p. ej. lote `SWG1`, hilera `SWG1-H05`. Diccionario completo: capítulo 10 y `05-…/plataforma-web/docs/ARQUITECTURA_DATOS.md`.

En el celular (SQLite, migraciones 001–005): `monitoring_sessions`, `session_devices`, `monitoring_passes`, `marker_changes`, `capture_sequences`, `captures`, `quality_results`, `incidents`, `sync_queue`, `remote_uploads`, `transfer_queue`, catálogos `cat_*`, `event_log`, etc.

## Ubicación de un hallazgo

El controlador lee el GPS cada 1 s; cada **secuencia** (una foto por cámara en el mismo punto) guarda lat/lon/precisión solo si la lectura tiene ≤ 10 s. Las fotos no llevan EXIF de ubicación. Además cada secuencia lleva lote, hilera, lateral, segmento y marcador. Al abrir un caso se copia la ubicación: GPS de la secuencia → si no hay, coordenadas del marcador → si no, ninguna. El GPS de gama de entrada da 5–30 m; por eso la ubicación útil es la combinación hilera + lateral + segmento (≈20 m).

## Entornos

- **Desarrollo:** `iniciar.bat` en Windows (SQLite, datos de demostración `@demo.pe`, Cloudinary e IA simulados). App con Expo Go o APK `preview` contra backend simulado.
- **Piloto:** VPS + Supabase `riachuelo-piloto` + Cloudinary real + Telegram real; APK perfil `piloto`.
- **Despliegue web:** pruebas locales (307 en v1.6.1) → `git push` (rama `prueba2`) → en el VPS `git pull && bash deploy/actualizar.sh` (reconstruye, migra, reinicia en 1–2 min).
- **Despliegue app:** `npm run validate` → subir versión → `npm run build:apk:piloto` (EAS) → instalar la misma versión en los 3 celulares.

## Herramientas del equipo

VS Code, Git y GitHub (un repo por componente), Jira (Scrum y Jira Service Management para el servicio), Docker, Google Colab (entrenamiento), Roboflow (etiquetado), UptimeRobot, Claude como asistente de desarrollo y documentación.
