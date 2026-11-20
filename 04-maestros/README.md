# Archivos maestros

Especificaciones con las que se construyeron los dos componentes, escritas para que una IA pudiera programar por módulos. Son documentos **grandes** (~400 KB cada uno): búscalos por sección.

| Archivo | Qué es | Secciones clave |
|---|---|---|
| [`MAESTRO_WEB.md`](MAESTRO_WEB.md) | Maestro de la web de revisión `riachuelo-platform` (v1.0; PDF original `MAESTRO_WEB_v1.0.pdf`, 158 págs.) | §1 reglas W-01 a W-24 · roles y permisos · pantallas · reglas RN-W · reporte de avance §23.2 |
| [`MAESTRO_APP_MOVIL.md`](MAESTRO_APP_MOVIL.md) | Maestro de la app `riachuelo-mobile` (v2.0), que además define el **contrato `/api/v1`** (§15) y la **arquitectura del servidor** (§28) | Pantallas PANT-xx · reglas R-xx · configuración CFG · protocolo local · sincronización §8.9 / §15.5–15.7 · §28 Django, worker y Cloudinary |

Importante: son la especificación **inicial**. Varias decisiones posteriores los modifican (avisos por Telegram en lugar de WhatsApp, «Ubicar plaga», segmentos fijos, triage de la IA…). Antes de afirmar algo a partir de un maestro, revisa `00-contexto/08-estado-versiones-y-pendientes.md` y los ADR en `05-repositorios-de-codigo/*/docs/adr/`.

Las capturas de pantalla que cita `MAESTRO_WEB.md` (`img/web/web_0x_*.png`) no se incluyen en este repositorio; las pantallas actuales están en `03-manuales/media/web/`.
