# Anexo 40 (AP-42): Informe de Monitoreo de Riesgos

<table>
<tr>
<th colspan="6">CONTROL DE VERSIONES</th>
</tr>
<tr>
<td>Versión</td>
<td>Hecha por</td>
<td>Revisada por</td>
<td>Aprobada por</td>
<td>Fecha</td>
<td>Motivo</td>
</tr>
<tr>
<td>1.0</td>
<td>Grace<br>Lucia Peña<br>Reyes</td>
<td>Oscar Antonio Gutiérrez<br>Rojas</td>
<td>Jefe de Fundo (Agrícola Riachuelo<br>S.A.C.)</td>
<td>03/10/2026</td>
<td>Emisión del Informe de Monitoreo de Riesgos al corte del<br>Sprint 3.</td>
</tr>
</table>

## INFORME DE MONITOREO DE RIESGOS

| Nombre del Proyecto | Siglas del Proyecto |
|---|---|
| Sistema inteligente para el monitoreo y detección de plagas en cultivos de vid en Agrícola<br>Riachuelo (SIMDPCVAR) | SIMDPCVAR |

<table>
<tr>
<th>Proyecto</th>
<th colspan="3">Sistema inteligente para el monitoreo y detección de plagas en cultivos de vid en Agrícola Riachuelo</th>
</tr>
<tr>
<td>Fecha y hora</td>
<td>03 de octubre de 2026 – 10:00 a.m. a 11:30 a.m</td>
<td>Convocada por</td>
<td>Oscar Antonio Gutiérrez Rojas (Project Manager)</td>
</tr>
<tr>
<td>Lugar</td>
<td>Sala de Coordinación del Fundo Agrícola Riachuelo S.A.C.<br>/<br>Videoconferencia</td>
<td>Facilitador</td>
<td>Oscar Antonio Gutiérrez Rojas</td>
</tr>
</table>

| Objetivo | Auditar la efectividad de los planes de respuesta a riesgos, revisar el estado de los triggers tras las pruebas de hardware/software del Sprint 3 e identificar nuevos riesgos emergentes derivados de la integración tecnológica. |
|---|---|

<table>
<tr>
<th colspan="3">Asistentes</th>
</tr>
<tr>
<td>Persona</td>
<td>Cargo/Área</td>
<td>Empresa</td>
</tr>
<tr>
<td>Oscar Antonio Gutiérrez Rojas</td>
<td>Project Manager /<br>Líder de Riesgos</td>
<td>UTP</td>
</tr>
<tr>
<td>Grace Lucia Peña Reyes</td>
<td>Analista Funcional /<br>QA Lead</td>
<td>UTP</td>
</tr>
<tr>
<td>María Fernanda Zúñiga Matos</td>
<td>Analista Técnica de<br>Hardware</td>
<td>UTP</td>
</tr>
<tr>
<td>Oscar Manuel Guerrero Figari</td>
<td>Desarrollador<br>Backend, Web e IA</td>
<td>UTP</td>
</tr>
<tr>
<td>Jefe de Fundo</td>
<td>Product Owner /<br>Patrocinador Operativo</td>
<td>Agrícola Riachuelo S.A.C.</td>
</tr>
<tr>
<td colspan="3">Documentación</td>
</tr>
<tr>
<td>Qué se debe leer previamente</td>
<td colspan="2">Responsable</td>
</tr>
<tr>
<td>Plan de Gestión de Riesgos (FGPR_350) y<br>Registro de Riesgos Cualitativo (FGPR_360).</td>
<td colspan="2">Grace Lucia Peña Reyes.</td>
</tr>
<tr>
<td>Registro de Incidentes (FGPR_530) y<br>Solicitud de Cambio Aprobada N° 01.</td>
<td colspan="2">Grace Lucia Peña Reyes.</td>
</tr>
<tr>
<td>Qué se debe presentar en la reunión</td>
<td colspan="2">Responsable</td>
</tr>
<tr>
<td>Reporte de métricas de rendimiento de la API REST Django ( ) y precisión del modelo YOLO ( ).</td>
<td colspan="2">Oscar Manuel Guerrero Figari / María Fernanda Zúñiga Matos</td>
</tr>
<tr>
<td>Bitácora de seguimiento de descalibraciones en el prototipo vestible.</td>
<td colspan="2">Oscar Manuel Guerrero Figari / María Fernanda<br>Zúñiga Matos</td>
</tr>
</table>

<table>
<tr>
<th colspan="3">Agenda</th>
</tr>
<tr>
<td>Actividad</td>
<td>Responsable</td>
<td>Tiempo<br>Programado</td>
</tr>
<tr>
<td>1. Revisión de Implementación de Planes de<br>Respuesta a los Riesgos</td>
<td>Oscar Gutiérrez</td>
<td>30 min</td>
</tr>
<tr>
<td>2. Seguimiento de Riesgos Identificados y<br>Estado de Triggers</td>
<td>Grace Peña / Ma.<br>Fernanda Zúñiga</td>
<td>30 min</td>
</tr>
<tr>
<td>3. Identificación de Nuevos Riesgos y<br>Programación de Acciones</td>
<td>Equipo de Proyecto /<br>Product Owner</td>
<td>30 min</td>
</tr>
</table>

| Revisión de Implementación de Planes de Respuesta a los Riesgos |
|---|
| Revisión de adecuación de Respuestas Planificadas para los riesgos identificados inicialmente: Descripción de los resultados de la revisión de las respuestas planificadas. |
| R-01 (Conectividad limitada en campo): La implementación del esquema offline-first en la app React Native (almacenando fotos en el sistema de archivos y metadatos en SQLite) ha resultado ser totalmente adecuada. La sincronización se ejecuta automáticamente al retornar a zonas de cobertura mediante HTTP/REST con tokens JWT. |
| R-03 (Falsos positivos o baja precisión de IA): La respuesta de establecer el resultado de la IA como "indicio sugerido" (no vinculante) y exigir la firma síncrona del agrónomo en la plataforma web fue validada por el especialista. |
| Revisión de Planes de Contingencia para los riesgos identificados inicialmente: Descripción de los resultados de la revisión de los planes de contingencia. |
| R-04 (Descalibración o vibración del soporte vestible): El plan de contingencia de añadir empaquetaduras de neopreno y ajustar las contratuercas con arandelas de presión se ejecutó satisfactoriamente, reduciendo el descarte de fotos borrosas de 35% a menos de 4% |
| Verificación de Ejecución de respuestas planificadas: Comprobación de la ejecución de las respuestas planificadas que se utilizaron en los riesgos identificados. |
| Se comprobó que la migración arquitectónica a Django + Supabase + Cloudinary (Solicitud de Cambio N° 01) mitigó con éxito el riesgo de latencia excesiva en la API REST, reduciendo el tiempo de respuesta de > 3.0 s a 1.18 s. |

**Elaboración de Planes de Emergencia: Descripción de los Planes de Emergencia<br>Implementados, si fuera el caso.**

**No fue necesario activar planes de emergencia debido a que las contingencias aplicadas en hardware y software contuvieron todas las desviaciones dentro de la línea base del proyecto.**

| Seguimiento de Riesgos Identificados |
|---|
| Revisión de Triggers para los riesgos identificados inicialmente: Descripción de los resultados de la revisión de los triggers para detectar los riesgos potenciales del proyecto. |
| Trigger R-01 (Pérdida de señal celular en marcha): Activado en los Lotes 12 y 14. La app móvil gestionó la captura 100% offline en SQLite sin interrupción. |
| Trigger R-04 (Alertas continuas de foto borrosa): Inactivo desde que se incorporaron las gomas antivibración y la regla de repetición de foto unilateral. |
| Revisión y Confirmación de Probabilidad e Impacto Estimados inicialmente: Descripción de los resultados de la revisión de la probabilidad e impacto de los riesgos, detallando si se mantienen, disminuyen o incrementan con respecto a la estimación inicial. |
| R-01 (Conectividad): Probabilidad se mantiene en 0.7, pero el impacto bajó a Moderado (0.20) por la arquitectura offline. Exposición revisada: 0.14 (Riesgo Moderado). |
| R-03 (Baja precisión de IA): La probabilidad disminuyó de 0.5 a 0.3 tras alcanzar un 87.5% de precisión en el dataset de prueba. |
| Identificación de Nuevos Riesgos |
| Definición de Triggers: Definir detalladamente nuevos triggers detectados o síntomas de la ocurrencia real de un nuevo evento de riesgo. |
| Nuevo Trigger R-06 (Saturación de memoria o time-out al sincronizar lotes masivos de fotos a Cloudinary): Acumulación de más de 500 fotografías locales no sincronizadas en el dispositivo móvil durante recorridos de jornada completa sin red. |
| Evaluación Cualitativa y Categorización de Riesgos: Descripción del método comparativo y generalizado para categorizar los riesgos, analizando los riesgos y priorizándolos en base a la probabilidad e impacto de su ocurrencia, determinando su magnitud y prioridad del nuevo riesgo. |
| Código: R-06 |
| Descripción: Posibilidad de fallo o time-out por límite de tiempo de conexión al intentar cargar simultáneamente un lote extenso de fotografías hacia la CDN de Cloudinary al reestablecer internet. |
| Probabilidad: 0.3 (Relativamente Probable) \| Impacto: 0.40 (Alto en el proceso de<br>Sincronización). |
| Probabilidad × Impacto: 0.12 \| Categorización: Riesgo Moderado. |
| Definición de Respuestas Planificadas: Definición detallada de las respuestas planificadas para los nuevos riesgos detectados. |
| Implementar en el backend Django un procesador de sincronización en lotes pequeños (chunking) que envíe paquetes de máximo 10 fotografías con cola de reintentos exponenciales (exponential backoff). |
| Definición de Planes de Contingencia: Definición detallada de las acciones predefinidas que el equipo del proyecto debe tomar si un nuevo riesgo detectado ocurre. |
| Si la red inalámbrica es inestable, habilitar la compresión local previa de miniaturas en React<br>Native antes del envío, transmitiendo primero las fotos con indicios críticos. |
| Programación de Ejecución de Respuestas Planificadas: Descripción de cómo se ejecutarán las respuestas planificadas para los nuevos riesgos detectados. |
| Responsable: Oscar Manuel Guerrero Figari (Desarrollador). |
| Fecha Planificada: 05/10/2026 al 08/10/2026 (Durante el desarrollo del Sprint 3) |
