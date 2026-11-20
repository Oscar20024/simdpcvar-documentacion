# Anexo 80 (AT-19): Incidentes

**1. Objetivo y alcance**

Definir cómo se detectarán, clasificarán, registrarán, atenderán y cerrarán los incidentes técnicos del sistema SIMDPCVAR durante las pruebas y el piloto. Incluye aplicación móvil, sincronización, API y web Django, Supabase, Cloudinary, procesamiento YOLO y notificaciones. Los incidentes operativos se distinguirán de solicitudes de servicio, riesgos e impedimentos de desarrollo.

**2. Tipos de incidentes**

*Tabla AT19-01. Clasificación de incidentes técnicos*

| Código | Incidente / componente | Consecuencia posible |
|---|---|---|
| INC-01 | Captura o vinculación móvil | Captura bilateral incompleta |
| INC-02 | Sincronización móvil / API | Capturas pendientes, duplicadas o sin confirmar |
| INC-03 | Backend Django / API | Interrupción del servicio o errores HTTP |
| INC-04 | Supabase / PostgreSQL | Consultas o persistencia fallidas |
| INC-05 | Cloudinary | Fallo de carga o acceso a evidencia |
| INC-06 | Worker YOLO / ai_tasks | Fotografías pendientes de análisis |
| INC-07 | Autenticación / roles | Acceso legítimo bloqueado o acceso indebido |
| INC-08 | Web Django / HTMX | Imposibilidad de consultar o revisar casos |
| INC-09 | Notificaciones Telegram | Avisos no entregados tras confirmación |
| INC-10 | Trazabilidad / auditoría | Datos o decisiones inconsistentes |

**3. Prioridades y tiempos de atención**

Los objetivos siguientes proceden de la sección 14.7.5 del proyecto y se medirán dentro del horario de soporte. La respuesta implica recepción e inicio del diagnóstico; el restablecimiento corresponde al retorno del servicio, incluso mediante una medida temporal.

*Tabla AT19-02. Matriz de prioridades del servicio*

| Nivel | Criterio de impacto | Respuesta | Restablecimi ento |
|---|---|---|---|
| P1 Crítica | Servicio inutilizable, pérdida/corrupción de evidencia o aviso no autorizado | ≤ 30 min | ≤ 4 h |
| P2 Alta | Función crítica afectada; operación parcial posible | ≤ 1 h | ≤ 8 h; antes de la siguiente jornada |
| P3 Media | Falla parcial sin afectar la jornada | ≤ 4 h | ≤ 3 días de servicio |
| P4 Baja | Consulta, ajuste visual o mejora menor | ≤ 1 día de servicio | Siguiente sprint |

**4. Procedimiento de atención**

*Figura AT19-01. Flujo propuesto de gestión de incidentes*

↓

**Clasificar impacto y prioridad P1–P4**

↓

**Asignar responsable y diagnosticar**

↓

**Corregir o escalar según impacto**

↓

**Verificar restablecimiento e integridad**

↓

**Documentar solución y cerrar**

Toda incidencia se registrará con fecha, servicio y evidencia disponible. Si el evento procede del monitoreo AT-18, se incorporará la referencia de alerta. Tras una corrección se repetirá la comprobación, el cierre requerirá verificación del funcionamiento y conservación del historial.

**5. Responsables y escalamiento**

*Tabla AT19-03. Responsables según nivel de atención*

| Nivel | Responsable | Función |
|---|---|---|
| 1 | Oscar Antonio Gutiérrez Rojas | Recepción y administración de plataforma |
| 1 | Grace Lucia Peña Reyes (QA Lead) | Registro, seguimiento y control |
| 2 | Oscar Manuel Guerrero Figari | Backend, web y worker IA |
| 2 | Oscar Antonio Gutiérrez Rojas | App, base de datos e infraestructura |
| 2 | María Fernanda Zúñiga Matos | Prototipo y equipos |
| 3 | Project Manager, jefe de fundo y<br>Product Owner | Decisiones sobre continuidad, alcance o reglas de negocio |

**6. Registro editable de incidentes**

*Tabla AT19-04. Ficha de registro de incidente*

| Campo | Registro pendiente |
|---|---|
| ID / fecha y hora | Pendiente |
| Tipo y componente | Pendiente |
| Involucrados y descripción | Pendiente |
| Impacto y prioridad | Pendiente |
| Responsable y estado | Pendiente |
| Diagnóstico / acciones | Pendiente |
| Fecha de solución / solución final | Pendiente |
| Verificación y evidencia | Pendiente |

**7. Comunicación y derivación**

*Tabla AT19-05. Comunicación y escalamiento propuestos*

| Situación | Acción | Canal / destinatario |
|---|---|---|
| Recepción | Registrar, clasificar y asignar | Administrador, QA / Jira |
| P1 / P2 | Activar diagnóstico especializado | Equipo técnico y dirección según nivel |
| P3 | Programar corrección según impacto | Responsable del componente |
| P4 | Evaluar solicitud o mejora | Product Backlog |
| Interrupción mayor | Evaluar activación de continuidad | Dirección y AT-20 |
| Cierre | Comunicar recuperación verificada | Usuarios afectados / Jira |

**8. Evaluación de continuidad**

*Figura AT19-02. Decisión para activar el AT-20*

**Incidente confirmado y prioridad asignada**

↓

**Evaluar impacto, alternativa operativa y tiempo de restablecimiento**

↓

**Decidir si requiere recuperación extraordinaria**

↓

| Si continúa operación | Si requiere recuperación extraordinaria |
|---|---|
| ('Atender según AT-19 y verificar solución', 'Escalar, evaluar AT-20 y registrar recuperación') |  |

Una falla temporal de conectividad en campo no obliga a activar continuidad si la aplicación mantiene sus registros locales y puede sincronizarlos posteriormente. La activación del AT-20 se evaluará ante pérdida de integridad o interrupciones mayores que excedan la atención ordinaria.

**9. Validación y evidencias previstas**

*Tabla AT19-06. Comprobaciones del procedimiento de incidentes*

| Prueba | Criterio verificable | Evidencia | Estado |
|---|---|---|---|
| Registro | Ficha con campos mínimos completos | Ficha de prueba | Pendiente |
| Priorización | Clasificación correcta P1–P4 | Matriz aplicada | Pendiente |
| Asignación | Responsable técnico identificado | Bitácora / Jira | Pendiente |
| Escalamiento | Comunicación al nivel correspondiente | Registro de aviso | Pendiente |
| Restablecimiento | Componente operativo e integridad verificada | Log / prueba | Pendiente |
| Continuidad | Decisión de activar o no AT-20 documentada | Evaluación de impacto | Pendiente |
