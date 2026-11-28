# Guía para agentes de IA

Este repositorio es la **memoria documental** del proyecto SIMDPCVAR (monitoreo de plagas en vid para Agrícola Riachuelo S.A.C.). No tiene código ejecutable. Úsalo para responder preguntas, redactar documentos, planificar cambios o preparar tareas para los repositorios de código. Todo está en español.

## 1. Cómo leer (no leas todo de golpe)

Los archivos grandes son: `04-maestros/MAESTRO_WEB.md` (~400 KB), `04-maestros/MAESTRO_APP_MOVIL.md` (~420 KB), `05-repositorios-de-codigo/plataforma-web/docs/CONTEXTO_WEB.md` (~67 KB) y algunos anexos (37, 36, 22). Léelos por secciones (busca el título con grep) en lugar de cargarlos completos.

Orden mínimo para cualquier tarea:

1. `00-contexto/01-proyecto-en-una-pagina.md`
2. `00-contexto/05-reglas-limites-y-restricciones.md`
3. `00-contexto/08-estado-versiones-y-pendientes.md` (qué es actual y qué quedó viejo)
4. Lo específico de la tarea (índice de la sección 3).

## 2. Hechos que no debes contradecir

- La IA **sugiere indicios**; nunca «diagnostica». La palabra final es del **especialista fitosanitario** (RN-01, RN-06, SC-03). Excepción acotada: la IA puede marcar «Confirmado por IA» (alerta temprana) con 2+ cajas ≥ 0,70 o 3+ cajas ≥ 0,50; el aviso dice «falta la revisión del especialista».
- Los avisos salen por **Telegram**, no por WhatsApp (SC-02). El informe original (RF10) dice WhatsApp; la diferencia está justificada, no se corrige el requerimiento.
- Las **dos cámaras miran el mismo lateral** de la hilera desde el soporte T (dos fotos del mismo punto = doble evidencia); cada hilera se recorre en **dos pasadas**: LATERAL_A y LATERAL_B (SC-04).
- Cada hilera tiene **10 segmentos fijos** de ≈20 m («segmento» = «zona» del informe). Los marcadores son los postes al inicio de cada segmento.
- La app habla **solo con Django** (`/api/v1`). Nunca con Supabase. Las fotos van **directo del celular a Cloudinary** con un ticket firmado por Django.
- Nadie salvo Django lee la base de datos (RLS sin políticas y REVOKE a `anon`/`authenticated` en Supabase).
- Los colores de la web indican **estado de revisión, nunca gravedad** (W-12).
- La web **no crea ni edita datos de campo** (sesiones, fotos, secuencias); sí administra catálogos, cuentas, celulares y destinatarios.
- Roles: Administrador, Especialista fitosanitario, Supervisor (web) y Operador de campo (app). Las cuentas de la web las crea el administrador; las de la app se piden desde la app y el administrador las aprueba sin elegir rol (SC-08).
- Los requerimientos del informe (Tablas 27, 28 y 29: RF01–RF11, RNF01–RNF07, RN-01–RN-06) **no se reescriben**. Toda diferencia con el sistema real se justifica con una **solicitud de cambio** (SC-01 a SC-09, Anexo 37 y registro en Anexo 42).

## 3. Índice «tema → archivo»

| Si necesitas… | Lee |
|---|---|
| Visión general, problema, objetivos, hipótesis | `01-documentacion-principal/capitulo-01-planteamiento-de-la-investigacion.md` |
| Marco teórico, variables, indicadores | `capitulo-02-marco-teorico.md`, `capitulo-03-metodologia-de-investigacion.md` |
| La empresa, stakeholders, AS-IS / TO-BE | `capitulo-04-analisis-empresarial.md`, anexos 2–5 y 31 |
| Gestión del proyecto (PMBOK): acta, alcance, cronograma, costos, calidad, riesgos, comunicaciones | `capitulo-05…`, `capitulo-06…`, anexos 1–42 (`AP-xx`) |
| Scrum: backlog, historias, sprints, DoD | `capitulo-07-gestion-agil.md`, anexos 51–61 (`AS-xx`) |
| Requerimientos RF/RNF/RN, casos de uso, diagramas | `capitulo-08-analisis-y-diseno-del-sistema.md`, anexo 9, anexo 11, y `02-…/documentacion-del-sistema-y-guia-de-pruebas.md` (cómo se cumple cada uno hoy) |
| Solicitudes de cambio SC-01…SC-09 | `01-documentacion-principal/anexos/anexo-37-ap-39-solicitud-de-cambio.md`, `anexo-42-…`, capítulo «Solicitudes de cambio» del doc 02 |
| UX/UI, pantallas | `capitulo-09-diseno-ux-ui.md`, `03-manuales/` |
| Base de datos (tablas, campos) | `capitulo-10-base-de-datos.md`, `05-…/plataforma-web/docs/ARQUITECTURA_DATOS.md` |
| Implementación, Git, APIs | `capitulo-11-implementacion-del-sistema.md`, `05-…/plataforma-web/docs/INTEGRACION_APP.md`, `04-maestros/MAESTRO_APP_MOVIL.md` §15 |
| Seguridad y pruebas | `capitulo-12-seguridad-y-pruebas.md`, guías de prueba CP-W/CP-A del doc 02 |
| Despliegue, VPS, Docker | `capitulo-13-despliegue.md`, `05-…/plataforma-web/docs/CONTEXTO_WEB.md` |
| SLA, KPI, incidentes, continuidad, Jira | `capitulo-14-gestion-del-servicio.md` (14.7 SLA, 14.8 KPI), anexos 79–81, `00-contexto/06-…` |
| Investigación (instrumento, pretest, postest) | `capitulo-03…`, anexos 43–50 (`AI-xx`) |
| Modelo YOLO, triage, entrenamiento | `00-contexto/07-inteligencia-artificial-yolo.md`, `05-…/plataforma-web/docs/adr/ADR-W-008…` |
| Dónde está cada cosa en el código | `05-repositorios-de-codigo/plataforma-web/ESTRUCTURA.md`, `05-repositorios-de-codigo/app-movil/ESTRUCTURA.md` |
| Por qué se decidió algo | ADR: `05-…/plataforma-web/docs/adr/` (ADR-W-001…014) y `05-…/app-movil/docs/adr/` (0001…0012) |
| Qué cambió en cada versión | `05-…/plataforma-web/docs/REPORTE_AVANCE.md`, `05-…/app-movil/docs/CAMBIOS.md` |
| Cómo se usa cada pantalla | `03-manuales/manual-de-usuario-plataforma-web.md`, `03-manuales/manual-de-usuario-app-movil.md` |
| Qué dice o muestra una figura o captura (sin abrir la imagen) | `06-figuras/` — `informe-principal/`, `manual-web/`, `manual-app/`; cada carpeta tiene un `README.md` con el índice. Debajo de cada imagen de los capítulos y manuales hay un enlace «Transcripción completa» |
| Términos (pasada, secuencia, marcador, triage…) | `00-contexto/09-glosario-rapido.md`, `01-documentacion-principal/00-glosario.md` |

## 4. Códigos que vas a encontrar

| Prefijo | Significado | Dónde se define |
|---|---|---|
| RF01–RF11 / RNF01–RNF07 / RN-01–RN-06 | Requerimientos funcionales, no funcionales y reglas de negocio del informe (Tablas 27–29) | Capítulo 8, anexo 9 |
| CU1–CU9 | Casos de uso | Capítulo 8 |
| HU01–HU17 | Historias de usuario | Anexo 52, doc 02 |
| SC-01–SC-09 | Solicitudes de cambio (formato FGPR-410-06) | Anexo 37, anexo 42 |
| STK-01–STK-05 | Interesados (gerencia, jefe de fundo, especialista, supervisor, operador) | Capítulo 4, anexo 5 |
| SRV-01–SRV-06 | Servicios del catálogo de servicios | 14.1.4 |
| SLA-01–SLA-09, KPI-01–KPI-18 | Niveles de servicio e indicadores | 14.7, 14.8 |
| SOL-01–SOL-08, P1–P4 | Tipos de solicitud de servicio y prioridades de incidente | 14.2, 14.7.5–14.7.6 |
| CP-W01…, CP-A01… | Casos de prueba de la web y de la app | Doc 02, capítulos «Guía de pruebas» |
| AP-xx / AI-xx / AS-xx / AT-xx | Anexos de proyecto (PMBOK) / investigación / Scrum / técnicos | `01-documentacion-principal/README.md` |
| W-01–W-24 | Reglas de construcción de la web | `04-maestros/MAESTRO_WEB.md` §1 |
| R-xx, RN-xx (app), RF-xx (app), PANT-xx, CFG-x | Reglas, requisitos, pantallas y versión de configuración **del maestro de la app** (numeración distinta a la del informe) | `04-maestros/MAESTRO_APP_MOVIL.md` |
| RN-W01–RN-W08 | Reglas de negocio de la web (revisión, avisos) | `04-maestros/MAESTRO_WEB.md`, `revision/services.py` |
| ADR-W-xxx / ADR 00xx | Decisiones de arquitectura de la web / de la app | `05-repositorios-de-codigo/*/docs/adr/` |

Ojo: «RN-01» del informe y «RN-01» del maestro de la app **no son la misma regla**. Indica siempre de qué documento hablas.

## 5. Reglas para trabajar con este proyecto

- **No inventes.** Si un dato no está aquí (una IP, un número de teléfono, una contraseña, una cifra del piloto), dilo y pide la fuente. Los datos personales y secretos se omitieron a propósito.
- **Cita la fuente** con la ruta del archivo y la sección (por ejemplo «14.7.5 en `capitulo-14-gestion-del-servicio.md`»).
- **No necesitas abrir las imágenes.** Toda figura tiene su texto en `06-figuras/`. En las del informe, la sección «Vigencia» dice si el diagrama o mockup quedó desactualizado (por ejemplo, wireframes con WhatsApp o nombres antiguos de la app).
- **Distingue informe vs. sistema real.** El informe describe lo planificado y lo aprobado; el sistema real puede diferir y esas diferencias están en las SC.
- Si vas a **modificar código**, no lo hagas aquí: este repo solo documenta. En los repos de código lee su `AGENTS.md` (copias en `05-repositorios-de-codigo/*/AGENTS_del_repositorio.md`) y respeta sus validaciones:
  - Web: `python manage.py check`, `python manage.py makemigrations --check --dry-run`, `python manage.py test` y el reporte en `docs/REPORTE_AVANCE.md` (W-24).
  - App: `npm run validate` (typecheck + lint + jest) y, si tocas la sincronización o el contrato, `tools/verificacion`.
- Si actualizas este repositorio, mantén el formato: Markdown GFM, nombres de archivo en minúsculas con guiones, y actualiza `README.md`, este índice y `00-contexto/08-estado-versiones-y-pendientes.md`.
