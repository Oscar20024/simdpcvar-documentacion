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

Estado al **19/11/2026, 23:00 (hora de Lima)**. Plan **Jira Service Management Free** (sitio del equipo en Atlassian; espacio de servicio con clave **SOP**, nombre «Soporte»). Todo lo de esta sección está **activo**.

### Portal y canales

| Elemento | Estado |
|---|---|
| Portal de clientes | **«Soporte SIMDPCVAR · Agrícola Riachuelo»** — <https://oscoscantonio.atlassian.net/servicedesk/customer/portal/1>. Introducción en español: qué elegir (INC para fallas, SOL para pedidos) y horario de atención |
| Grupos del portal | Solo **«SIMDPCVAR - Incidentes y solicitudes»**. Los 5 grupos de ejemplo de la plantilla de TI (Common Requests, Computers, Logins and Accounts, Applications, Servers and Infrastructure) se eliminaron el 19/11; sus formularios en inglés siguen existiendo pero ya no se ven en el portal |
| Tipos de solicitud | «INC - Reportar un incidente del sistema» (categoría Incidentes) y SOL-01 a SOL-08 (categoría Solicitudes de servicio), todos con resumen y descripción |
| Canal de correo | Una dirección del portal crea tickets INC (es la que recibe los reenvíos de UptimeRobot) |
| Acceso | **Abierto**: cualquier persona puede escribir al portal o al correo y se registra sola como cliente. No se cargaron clientes a mano |
| Organización | «Agrícola Riachuelo S.A.C.» creada y vinculada al espacio (sin miembros todavía) |
| Agentes | 3 de 3 plazas del plan gratis: el Project Manager y dos integrantes del equipo |
| Enlace desde la web | Botón «Reportar un problema» en Ayuda y en el pie de página (web **v1.6.2**, variable `SOPORTE_URL`) — hecho en el código, **pendiente de subir a GitHub y desplegar** |

### Calendario y SLA

**Calendario:** «Horario piloto Riachuelo», America/Lima, lunes a sábado 06:00–14:00. Fuera de ese horario los temporizadores no avanzan.

| SLA en Jira | Inicia | Se detiene | Metas |
|---|---|---|---|
| Tiempo de primera respuesta | Al crear el ticket | Comentario para el cliente; paso a «En curso», «Esperando por el cliente» o «Work in progress»; o paso a un estado final | Incidentes: Highest 30 min · High 1 h · Medium 4 h · Low 8 h; resto 8 h |
| Tiempo de restablecimiento de incidentes | Al crear | Paso a un estado final (pausa en «Esperando por el cliente») | Highest 4 h · High 8 h · Medium 24 h · Low 96 h |
| Plazo de atención de solicitudes | Al crear | Paso a un estado final (pausa en «Esperando por el cliente») | 2 h: SOL-01 Bloqueo, SOL-02 Revocación · 8 h: SOL-01 Alta, SOL-02 Registro, SOL-04, SOL-06 · 16 h: SOL-03, SOL-05, SOL-07 · SOL-08 sin meta |

Estados finales que detienen los tres SLA: **Completado, Resuelta, Cerrada y Cancelado** (además de «Resolución: configurada»).

> **Corrección del 19/11/2026.** El sitio de Jira no tiene ninguna «resolución» configurada, así que la condición «Resolución: configurada» nunca se cumplía y los SLA seguían corriendo en tickets ya resueltos o cancelados. Se agregaron como condición de parada los estados finales. Si en el futuro se crean resoluciones, ambas condiciones conviven sin problema.

Correspondencia de prioridades: P1 = Highest, P2 = High, P3 = Medium, P4 = Low.

Flujo de trabajo de los incidentes (`SOP: Incident Management workflow`): Abierta → Investigate → Work in progress → Resolve → **Completado** → Close → Cerrada; Pending y Cancel desde Abierta/Work in progress; «Back to work in progress» reabre un Completado. En JQL los estados se escriben por **ID** (Abierta = 1, Work in progress = 10005, Pending = 10006, Completado = 10000, Cancelado = 10003): los nombres en español no funcionan en las búsquedas.

### Monitoreo automático (SLA-01)

```text
UptimeRobot (cada 5 min a /api/v1/health)
   └─ correo «Monitor is DOWN» / «Monitor is UP» a la cuenta de Gmail del equipo
        └─ filtro de Gmail: reenvía al correo del portal de Jira
             ├─ «DOWN» → ticket INC → regla 1 le pone prioridad Highest (P1): corren 30 min / 4 h
             └─ «UP»   → ticket INC → regla 2 resuelve el INC de caída abierto y cancela el ticket «UP»
```

| Pieza | Detalle | Estado |
|---|---|---|
| UptimeRobot | Monitor «SIMDPCVAR - API health (SLA-01)», plan gratis, cada 5 min, alertas por correo al titular | Activo |
| Filtro de Gmail | Remitente `alert@uptimerobot.com`, sin la palabra TEST, reenvío al correo del portal. Asunto actual: «Monitor is DOWN» | Activo para DOWN. **Pendiente:** cambiar el asunto a `"Monitor is DOWN" OR "Monitor is UP"` (hay que hacerlo en Chrome; el navegador integrado no guarda la edición). Sin esto la regla 2 nunca se dispara |
| Regla 1 «P1 para caídas detectadas por el monitor (SLA-01)» | Al crear un ticket cuyo resumen contiene «Monitor is DOWN» → prioridad Highest | Activa, probada (SOP-3, SOP-4) |
| Regla 2 «Cerrar el incidente cuando el monitor vuelve (SLA-01)» | Al crear un ticket cuyo resumen contiene «Monitor is UP»: busca INC «Monitor is DOWN» en Abierta o Work in progress, les deja una nota interna y los pasa a **Completado**; luego comenta y **cancela** el ticket «UP». Así el SLA de restablecimiento se detiene a la hora real de la recuperación | Activa, probada el 19/11 (SOP-4 → Completado, SOP-6 → Cancelado). No cierra caídas en «Pending»: esas se pasan a mano |

Tickets de prueba en el espacio: SOP-1 a SOP-6 (configuración y pruebas, todos cerrados o cancelados con nota) y SOP-7 (prueba del portal hecha por el equipo).

### Pendientes / opciones no implementadas

1. Editar el filtro de Gmail para que también reenvíe «Monitor is UP» (ver arriba).
2. Subir a GitHub y desplegar la web v1.6.2 (botón «Reportar un problema»).
3. Que la plataforma avise de sus propias fallas: worker de IA detenido (SLA-06), avisos de Telegram que no salen (SLA-07), controlador que no sincroniza. Opción simple recomendada: que `/api/v1/health` responda error si el worker o la cola de IA fallan, así UptimeRobot lo detecta. Opción completa: Django crea tickets en Jira (necesita una clave de Jira en el servidor y una SC-10).
4. Tablero Scrum (SCRUM): actualizar SCRUM-77 y SCRUM-155 (hablan de WhatsApp; hoy es Telegram), cerrar SCRUM-73 y SCRUM-78 según la prueba de campo, resolver el duplicado SCRUM-21 = SCRUM-15 y renombrar SCRUM-153/154 («trampas»).
5. Avisos a los agentes antes de que venza un SLA (automatización; el plan gratis limita las ejecuciones al mes).
6. Reporte mensual de cumplimiento de SLA para 14.7.9.
7. GitHub for Jira (enlazar commits con claves SOP-xx).
8. Actualizar 14.7.10 del informe con UptimeRobot, las dos reglas, los agentes y el portal.
