# Estado actual, versiones y pendientes

Corte de este repositorio: **27/11/2026** (estado del sistema al 19/11/2026; el 27/11 se actualizó el informe a la versión de 353 págs. y se transcribieron todas las figuras).

## Qué está ejecutándose ahora

| Pieza | Dónde | Versión / estado |
|---|---|---|
| Web + API `/api/v1` | VPS Hostinger (Docker: contenedores `web` y `worker`, Traefik con HTTPS) en <https://monitoreo.agricolariachuelo.org> | En marcha (`/api/v1/health` → UP). GitHub tiene la **v1.6.2** (`630e4c5`); falta confirmar que el VPS se actualizó con `deploy/actualizar.sh` (si no, corre la v1.6) |
| Worker de IA | Mismo VPS, contenedor `worker` | Modelo **2026.10-v2** (provisional) con triage 2 cajas ≥ 0,70 o 3 ≥ 0,50 |
| Base de datos | Supabase PostgreSQL (proyecto del piloto, plan Free) | En uso; solo Django se conecta |
| Fotos | Cloudinary (plan Free, fotos privadas) | En uso |
| Avisos | Telegram Bot API | En uso (WhatsApp retirado) |
| App móvil | APK en los 3 celulares Android del piloto | **0.6.1** |
| Monitoreo externo | UptimeRobot, cada 5 min a `/api/v1/health` | Activo; alertas por correo |
| Mesa de servicio | Jira Service Management Free, espacio SOP, portal «Soporte SIMDPCVAR · Agrícola Riachuelo» | Activo: 3 SLA con calendario, INC y SOL-01…08, 2 reglas automáticas, 3 agentes. Detalle en `06-gestion-del-servicio-sla-kpi-jira.md` |
| Gestión ágil | Jira Software, tablero SCRUM | Activo; algunas historias desactualizadas (WhatsApp) |
| Repositorio de contexto | GitHub `Oscar20024/simdpcvar-documentacion` (público) | Este repositorio |

## Versiones

| Componente | Versión | Notas |
|---|---|---|
| Plataforma web + API | **v1.6.2** en código (19/11/2026) | v1.6.1 (14/11): aprobar cuentas de la app sin rol (SC-08), manuales con cuentas de prueba. v1.6.2 (19/11): botón «Reportar un problema» al portal de Jira en Ayuda y pie de página (`SOPORTE_URL`). 309 pruebas OK. Subida a GitHub (`630e4c5`). **Pendiente:** confirmar `deploy/actualizar.sh` en el VPS |
| App móvil | **0.6.1** | Último commit en GitHub: `7a95da6` (30/10/2026). CFG-11; SQLite con migraciones 001–005. Sin cambios por v1.6.2 |
| Modelo IA | **2026.10-v2** (provisional) | Activo desde el 21/10/2026 |
| Informe principal | Noviembre 2026 | 353 págs. (PDF); incluye 14.7.10 (Jira) y SC-02 a SC-09, Esri + OpenStreetMap, «soporte» en vez de «prototipo vestible», cámaras A/B y Telegram en RF10. Aún no menciona UptimeRobot ni las reglas automáticas |
| Doc. del sistema y manuales | v1.6.1 | Con cuentas de prueba |

## Cambios del 27/11/2026 (solo documentación)

- **Informe principal** actualizado a la versión de 353 págs.: Leaflet sobre teselas Esri y OpenStreetMap (glosario, cap. 6, 7, 8, 11, 14 y SC del anexo 37); acta y presupuesto con «soporte» en lugar de «prototipo vestible»/«mochila»; RF10 por Telegram; RF11 con cámara A y B; SC-04 reformulada; canales de soporte por Telegram (cap. 14, anexos 79 y 81); instrumentos y resultados con «alerta por Telegram» (anexos 45 y 50); matriz de trazabilidad (anexo 11).
- **Nueva carpeta `06-figuras/`:** 51 figuras del informe y 67 capturas de los manuales transcritas a texto, enlazadas debajo de cada imagen.
- **Manuales:** se quitaron 112 imágenes vacías que dejó la conversión del PDF (sombras de marco, bandas de cabecera, fondo de portada).
- **Filtro de Gmail** de UptimeRobot ya reenvía «Monitor is DOWN» y «Monitor is UP» a Jira (verificado).

## Cambios del 19/11/2026

- **Jira:** corregida la parada de los 3 SLA (no se detenían por falta de resoluciones; ahora paran en Completado, Resuelta, Cerrada y Cancelado). Nueva regla «Cerrar el incidente cuando el monitor vuelve (SLA-01)». Portal renombrado y en español, sin los 5 grupos de ejemplo de TI. Organización «Agrícola Riachuelo S.A.C.».
- **Web v1.6.2:** botón «Reportar un problema» (Ayuda y pie de página) que abre el portal de Jira; variable `SOPORTE_URL` en `.env.example` y `deploy/env.piloto.plantilla`; 2 pruebas nuevas; reporte en `REPORTE_AVANCE.md`.
- **Este repositorio:** creado con toda la documentación en Markdown.

## Historia breve

**Web:** v1.0 (base según MAESTRO_WEB) → 05/10 arquitectura de datos y seguridad de la base; alta de cuentas desde la web (ADR-W-005) → 07/10 gestión de catálogos (ADR-W-006, v1.2) → 08/10 herramientas YOLO; **v1.3** alertas automáticas de la IA, mapa satelital y «Ubicar plaga» (ADR-W-007) → v1.3.1 triage en tres franjas, limpieza y miniaturas (ADR-W-008) → v1.3.2 «Descartadas por la IA» → v1.3.3 decisión por lugar con dos cámaras (ADR-W-009) → 21/10 triage 2 altas / 3 medianas → **v1.4** segmentos fijos por hilera (ADR-W-010) → v1.4.1 pulido de interfaz → **v1.5** avisos por Telegram, papelera y rediseño (ADR-W-011, ADR-W-012) → v1.5.1 solo Telegram → **v1.6** ayuda y manuales, destinatarios guiados, auditoría con contexto, cabeceras de seguridad (ADR-W-013) → **v1.6.1** aprobar cuentas de la app sin rol (ADR-W-014) → **v1.6.2** enlace «Reportar un problema» al portal de Jira.

**App:** 0.2.0 → Fase 4 0.4.0 (sincronización y subida directa a Cloudinary) → 0.4.1–0.4.6 correcciones de campo (07/10: QR, prueba corta, 12 MP, JPEG 85, cierre de sesión con monitoreo abierto, batería) → **0.5.0** «Ubicar plaga» (ADR 0009) → 0.5.1 borrar en el celular lo que el administrador eliminó (ADR 0010) → **0.6.0** segmentos fijos por hilera (ADR 0011) → **0.6.1** límite de repeticiones (ADR 0012).

Bitácoras completas: `05-repositorios-de-codigo/plataforma-web/docs/REPORTE_AVANCE.md` y `05-repositorios-de-codigo/app-movil/docs/CAMBIOS.md`.

## Qué quedó desactualizado (cuidado al leer)

| Fuente | Dice | Hoy es |
|---|---|---|
| Informe RF10, Tabla 30, `MAESTRO_WEB` y partes de `CONTEXTO_WEB.md` | Avisos por **WhatsApp** Cloud API | **Telegram** (SC-02, web v1.5.1 eliminó WhatsApp) |
| Tabla 30 y algunos diagramas | Supabase Auth, PostGIS, microservicio FastAPI, «React Native P2P», iPhone | No se implementaron (SC-07) |
| Maestro de la app y docs de octubre | El especialista no entra a la app; la API no devuelve resultados de IA (§28.10) | El especialista entra solo para «Ubicar plaga»; existe `GET /mobile/pest-reports` (ADR-W-007 / ADR 0009) |
| Docs de octubre | Segmento «hilera completa» (`-S1`, `-INI`, `-FIN`) | **10 segmentos fijos** de ≈20 m por hilera (ADR-W-010 / ADR 0011) |
| `CONTEXTO_WEB.md` (08/10) | «No hay modelo en producción» | Modelo 2026.10-v2 activo |
| Algunas secciones del informe | Las cámaras miran lados opuestos | Las dos cámaras miran el **mismo** lateral; laterales A y B en dos pasadas (SC-04) |
| `AGENTS.md` de la app | CFG-4 / CFG-9; migraciones 001–004 | CFG-11; migraciones 001–005 |
| `MAESTRO_APP_MOVIL.md` en `04-maestros/` | Encabezado de la v2.0 | El repo de la app guarda la v1.0 en PDF como histórico; la v2.0 es la vigente |

## Pendientes conocidos

1. **Desplegar la web v1.6.2** (ya en GitHub, `630e4c5`): `git pull && bash deploy/actualizar.sh` en el VPS.
2. **Corregir el Word del informe:** el reemplazo de «WhatsApp» por «Telegram» también cambió textos históricos que deben conservar «WhatsApp» (en SC-02: «vía Telgram», «Telegram Bot API requiere una cuenta de empresa en Meta», «Se retira el cliente de Telegram»; en 14.7.5: «reemplazó a Telegram Bot API»; en la retrospectiva: «Telegram Cloud API, luego reemplazado por Telegram»). En este repo se dejaron con «WhatsApp».
3. **Aprobación de SC-02 a SC-09** por el Comité de Control de Cambios.
4. **Modelo IA v3** con 150–200 fotos propias (100+ con chanchito, incluir sanas); medir falsas alarmas.
5. **Marcadores reales con coordenadas** y contornos de lotes, para que «Ubicar plaga» y el mapa sean exactos.
6. Prueba de campo completa: jornada larga, cierre, sincronización con el piloto y decisión de casos reales.
7. **iPhone** (Fase 5) sin validar.
8. Seguridad del VPS: restringir la red de Supabase a la IP del VPS, firewall, respaldos periódicos con `python manage.py respaldo` y prueba de restauración (SLA-09); evaluar plan pagado de Supabase (el Free se pausa).
9. Login sin internet lento en celulares de gama baja (PBKDF2 ≈ 9,7 s en Galaxy A03): calibrar.
10. Si el controlador pasa a segundo plano, las cámaras se desconectan: mantener la app del controlador en pantalla.
11. Jira: que la plataforma reporte sus propias fallas (health con estado del worker o tickets desde Django, SC-10), tablero SCRUM desactualizado, avisos antes de vencer un SLA, reporte mensual de SLA, GitHub for Jira, actualizar 14.7.10 del informe. Detalle en `06-gestion-del-servicio-sla-kpi-jira.md`.
12. Anexos técnicos AT-01 a AT-17 solo remiten a los capítulos donde está el contenido; AT-18 a AT-20 (monitoreo, incidentes, continuidad) están completos.
