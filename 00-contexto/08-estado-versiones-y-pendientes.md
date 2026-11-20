# Estado actual, versiones y pendientes

Corte de este repositorio: **19/11/2026**.

## Versiones vigentes

| Componente | Versión | Notas |
|---|---|---|
| Plataforma web + API | **v1.6.1** (14/11/2026) | Último commit en GitHub: `97575dc` (v1.6). Los cambios de v1.6.1 (aprobar cuentas de la app sin rol, SC-08; documentación con cuentas de prueba) estaban en la copia local al hacer este corte. 307 pruebas automáticas |
| App móvil | **0.6.1** | Último commit en GitHub: `7a95da6` (30/10/2026). Configuración CFG-11 en `src/config/defaults.ts`; SQLite con migraciones 001–005 |
| Modelo IA | **2026.10-v2** (provisional) | Activo desde el 21/10/2026 |
| Informe principal | 19/11/2026 revisado | 348 págs.; incluye 14.7.10 (Jira) y SC-02 a SC-09 |
| Doc. del sistema y manuales | v1.6.1 | Con cuentas de prueba |

## Historia breve

**Web:** v1.0 (base según MAESTRO_WEB) → 05/10 arquitectura de datos y seguridad de la base; alta de cuentas desde la web (ADR-W-005) → 07/10 gestión de catálogos (ADR-W-006, v1.2) → 08/10 herramientas YOLO; **v1.3** alertas automáticas de la IA, mapa satelital y «Ubicar plaga» (ADR-W-007) → v1.3.1 triage en tres franjas, limpieza y miniaturas (ADR-W-008) → v1.3.2 «Descartadas por la IA» → v1.3.3 decisión por lugar con dos cámaras (ADR-W-009) → 21/10 triage 2 altas / 3 medianas → **v1.4** segmentos fijos por hilera (ADR-W-010) → v1.4.1 pulido de interfaz → **v1.5** avisos por Telegram, papelera y rediseño (ADR-W-011, ADR-W-012) → v1.5.1 solo Telegram → **v1.6** ayuda y manuales, destinatarios guiados, auditoría con contexto, cabeceras de seguridad (ADR-W-013) → **v1.6.1** aprobar cuentas de la app sin rol (ADR-W-014).

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

1. **Aprobación de SC-02 a SC-09** por el Comité de Control de Cambios.
2. **Modelo IA v3** con 150–200 fotos propias (100+ con chanchito, incluir sanas); medir falsas alarmas.
3. **Marcadores reales con coordenadas** y contornos de lotes, para que «Ubicar plaga» y el mapa sean exactos.
4. Prueba de campo completa: jornada larga, cierre, sincronización con el piloto y decisión de casos reales.
5. **iPhone** (Fase 5) sin validar.
6. Seguridad del VPS: restringir la red de Supabase a la IP del VPS, firewall, respaldos periódicos con `python manage.py respaldo` y prueba de restauración (SLA-09); evaluar plan pagado de Supabase (el Free se pausa).
7. Login sin internet lento en celulares de gama baja (PBKDF2 ≈ 9,7 s en Galaxy A03): calibrar.
8. Si el controlador pasa a segundo plano, las cámaras se desconectan: mantener la app del controlador en pantalla.
9. Jira: posible integración Django → Jira para errores críticos (SC-10 a futuro) y GitHub ↔ Jira; bloque de aprobación/firma del SLA.
10. Anexos técnicos AT-01 a AT-17 solo remiten a los capítulos donde está el contenido; AT-18 a AT-20 (monitoreo, incidentes, continuidad) están completos.
