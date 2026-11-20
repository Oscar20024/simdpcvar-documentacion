# Gestión del servicio: SLA, KPI, incidentes y Jira Service Management

Fuente completa: `01-documentacion-principal/capitulo-14-gestion-del-servicio.md` (14.1 a 14.8) y anexos 79 (AT-18 Monitoreo), 80 (AT-19 Incidentes) y 81 (AT-20 Continuidad). El formato sigue prácticas de ITIL 4 (gestión de niveles de servicio, incidentes y solicitudes) e ISO/IEC 20000-1.

## Definición del servicio (14.1)

Servicio tecnológico de apoyo al monitoreo fitosanitario que Agrícola Riachuelo ofrece a sus áreas operativas. Catálogo de servicios (14.1.4): SRV-01 Monitoreo fotográfico en campo · SRV-02 Detección asistida por IA · SRV-03 Validación fitosanitaria · SRV-04 Alertas de hallazgos confirmados · SRV-05 Consulta de dashboard y mapa · SRV-06 Trazabilidad fitosanitaria. Usuarios: operador (STK-05), especialista (STK-03), jefe de fundo (STK-02), supervisor (STK-04), gerencia (STK-01).

**Horario de servicio (calendario del piloto):** lunes a sábado, 06:00–14:00, hora de Lima.

## Niveles de servicio (14.7.3)

| Código | Indicador | Meta |
|---|---|---|
| SLA-01 | Disponibilidad de la web y la API | ≥ 99,5 % mensual en el horario de servicio (monitor externo) |
| SLA-02 | Tiempo de respuesta de la API | p95 ≤ 1,5 s (RNF04) |
| SLA-03 | Validación local de calidad | ≤ 2 s por foto (RNF02) |
| SLA-04 | Conservación de evidencias sin conexión | 0 capturas perdidas; nada se borra antes de sincronizar (RN-05) |
| SLA-05 | Sincronización | 100 % sin pérdida de metadatos, el mismo día si hay Wi-Fi |
| SLA-06 | Análisis de IA | 95 % de las fotos en ≤ 10 min desde su confirmación |
| SLA-07 | Emisión de alertas | Aviso en la misma transacción que la confirmación; 95 % entregados por Telegram en ≤ 5 min |
| SLA-08 | Integridad de la decisión y trazabilidad | 100 % de casos reconstruibles; 0 avisos sobre casos pendientes o descartados |
| SLA-09 | Respaldo y recuperación | Respaldo semanal (pérdida máx. 7 días); restablecimiento ≤ 4 h de servicio |

SLA-01 a SLA-09 son **métricas de la plataforma**: se miden con sus propias fuentes (monitor externo, registros de Django, tablas `ai_tasks`, `notifications`, `review_cases`, exportación de diagnóstico de la app), no con temporizadores de Jira.

## Incidentes y solicitudes (14.7.5 y 14.7.6)

| Prioridad | Criterio | Primera respuesta | Restablecimiento |
|---|---|---|---|
| P1 Crítica | El servicio no puede usarse y no hay alternativa (no se captura en ningún juego, web/API caídas para todos, pérdida de evidencias, aviso sobre caso no confirmado) | ≤ 30 min | ≤ 4 h |
| P2 Alta | Falla una función crítica pero el trabajo sigue (worker detenido, un controlador no sincroniza, avisos no salen, falla la vinculación en un juego) | ≤ 1 h | ≤ 8 h |
| P3 Media | Falla parcial sin impacto en la jornada (capas del mapa, miniaturas, CSV, catálogo mal cargado) | ≤ 4 h | ≤ 3 días de servicio |
| P4 Baja | Consultas, ajustes visuales, mejoras menores | ≤ 1 día de servicio | Siguiente sprint |

Solicitudes de servicio (14.2): SOL-01 Alta, baja o modificación de usuarios y roles · SOL-02 Registro o cambio de dispositivos · SOL-03 Actualización de catálogos · SOL-04 Gestión de destinatarios de alertas · SOL-05 Consulta o reporte de casos · SOL-06 Reasignación de zonas o rutas · SOL-07 Capacitación o soporte de uso · SOL-08 Ajuste menor del servicio (entra al Product Backlog). Plazos de 2 h, 8 h o 16 h de servicio según el tipo (SOL-08 sin meta). Detalle en 14.2 y 14.7.6. Escalamiento y reporte: 14.7.9. KPI-01 a KPI-18 del proceso, de la operación y del desempeño de la IA frente al especialista, con línea base y semáforo: 14.8.

## Configuración real en Jira Service Management (14.7.10)

Configurada en noviembre de 2026 sobre **Jira Service Management Free** (sitio del equipo en Atlassian, espacio de servicio con clave **SOP**). Portal: «SIMDPCVAR - Incidentes y solicitudes».

**Tipos de solicitud:** «INC - Reportar un incidente del sistema» (categoría Incidentes) y SOL-01 a SOL-08 (categoría Solicitudes de servicio), todos con resumen y descripción.

**Calendario:** «Horario piloto Riachuelo», America/Lima, lunes a sábado 06:00–14:00.

**SLA de Jira (temporizadores):**

| SLA en Jira | Inicia | Se detiene | Metas |
|---|---|---|---|
| Tiempo de primera respuesta | Al crear el ticket | Comentario al cliente o paso a «En curso» / «Esperando por el cliente» / «Work in progress», o resolución | Incidentes: Highest 30 min · High 1 h · Medium 4 h · Low 8 h; resto 8 h |
| Tiempo de restablecimiento de incidentes | Al crear | Resolución (pausa en «Esperando por el cliente») | Highest 4 h · High 8 h · Medium 24 h · Low 96 h |
| Plazo de atención de solicitudes | Al crear | Resolución | 2 h: SOL-01 Bloqueo, SOL-02 Revocación · 8 h: SOL-01 Alta, SOL-02 Registro, SOL-04, SOL-06 · 16 h: SOL-03, SOL-05, SOL-07 · SOL-08 sin meta |

Correspondencia de prioridades: P1 = Highest, P2 = High, P3 = Medium, P4 = Low.

**Canal de correo:** una dirección de correo del portal crea tickets del tipo INC.

**Integración de monitoreo (SLA-01):**
1. **UptimeRobot** (plan gratis) vigila `https://monitoreo.agricolariachuelo.org/api/v1/health` cada 5 minutos («SIMDPCVAR - API health (SLA-01)») y envía alertas por correo al titular de la cuenta.
2. Un **filtro de Gmail** reenvía los correos de `alert@uptimerobot.com` con asunto «Monitor is DOWN» (excluyendo pruebas) a la dirección de correo del portal de Jira.
3. Jira crea un ticket **INC**; la regla de automatización **«P1 para caídas detectadas por el monitor (SLA-01)»** (al crear un ticket cuyo resumen contiene «Monitor is DOWN») le pone prioridad **Highest** → arrancan los SLA de 30 min / 4 h. Probado con éxito; los tickets de prueba se cancelaron con notas internas.

**Agentes:** 3 de 3 plazas del plan gratis (el Project Manager y dos integrantes del equipo).

**Pendientes / opciones no implementadas:** crear tickets desde Django ante errores críticos (posible SC-10) y conectar GitHub con Jira.
