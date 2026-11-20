# Anexo 81 (AT-20): Continuidad

**1. Objetivo y alcance**

Establecer las medidas de continuidad y recuperación que se propondrán para SIMDPCVAR ante interrupciones de conectividad, plataforma, almacenamiento, procesamiento o equipos de campo. Comprende la captura offline-first, la API/web Django, Supabase, Cloudinary, el worker YOLO y las notificaciones. Se aplicará en pruebas y operación del piloto, sin dar por ejecutadas restauraciones o copias que no cuenten con evidencia.

**2. Servicios críticos y dependencias**

*Tabla AT20-01. Servicios esenciales para la continuidad*

| Servicio / proceso | Dependencias | Impacto ante interrupción |
|---|---|---|
| Captura móvil bilateral | Tres dispositivos, app y almacenamiento local | No se generan nuevas evidencias si falla la captura |
| Sincronización | Conectividad, API Django, Cloudinary y Supabase | Capturas pendientes de transferir |
| API y web Django | VPS Hostinger (Docker Compose, Traefik con HTTPS), red y base de datos | Consultas y revisión de casos suspendidas |
| Datos y evidencias | Supabase PostgreSQL y Cloudinary | Riesgo de indisponibilidad o pérdida de trazabilidad |
| Procesamiento YOLO | Worker, ai_tasks y fotografías | Acumulación de análisis pendientes |
| Avisos autorizados | Decisión humana y<br>Telegram Bot API | Retraso de notificaciones confirmadas |

**3. Escenarios de interrupción y respuestas**

*Tabla AT20-02. Matriz de escenarios de contingencia*

| Escenario | Impacto / prioridad orientativa | Medida preventiva y respuesta prevista |
|---|---|---|
| Pérdida temporal de internet en campo | Sin sincronización inmediata; no implica caída de captura | Guardar imágenes y metadatos offline; reintentar al retornar señal |
| Caída de API/web Django | Sin revisión y consulta central | Registrar incidente, diagnosticar el VPS y los contenedores (web, worker y Traefik) y restablecer versión operativa |
| Indisponibilidad de<br>Supabase | Consultas, sesiones y cola de IA afectadas | Preservar datos locales; revisar proveedor y recuperación autorizada |
| Fallo de Cloudinary | Carga o lectura de fotografías suspendida | Retener originales locales no confirmados; reintentar transferencias |
| Worker IA detenido | Fotos sin procesar; revisión diferida | Conservar cola ai_tasks; revisar logs y reiniciar worker de forma controlada |
| Pérdida/corrupción de registros BD | Integridad y trazabilidad comprometidas | Aislar escritura riesgosa y evaluar restauración de respaldo verificable |
| Daño/pérdida de dispositivo | Capturas locales o jornada afectadas | Proteger accesos, revocar dispositivo y verificar copias confirmadas |
| Fallo de Telegram | Avisos externos retrasados | Conservar notificaciones generadas y revisar reintento autorizado |

**4. Estrategias de continuidad**

Se priorizará preservar la información en el controlador mientras no exista confirmación de recepción, mantener sin cambios las decisiones humanas registradas y evitar duplicaciones en reintentos. Durante fallas de nube, la captura podrá continuar únicamente cuando los dispositivos, el espacio local y la aplicación estén operativos. No se asumirá redundancia de servidores o proveedores que el proyecto no documente.

*Tabla AT20-03. Alternativas operativas por interrupción*

| Servicio afectado | Continuidad parcial prevista | Límite / retorno |
|---|---|---|
| Conectividad de campo | Capturar y conservar localmente | Sin revisión en nube hasta sincronizar |
| Backend / base de datos | Custodiar capturas en controladores | Revisión, IA y consulta central quedan pendientes |
| Cloudinary | Mantener fotos pendientes en app | Subir tras recuperar almacenamiento |
| Worker YOLO | Mantener tareas registradas | Reanudar procesamiento tras verificación |
| Mensajería Telegram | Conservar estado del aviso | Enviar cuando servicio y permisos lo permitan |

**5. Respaldos, RTO y RPO**

El SLA-09 establece respaldo semanal de base de datos mediante pg_dump, pérdida máxima de siete días de registros y restablecimiento en un máximo de cuatro horas de servicio. Se trata de objetivos documentados; deberán comprobarse mediante bitácoras y una restauración controlada. No se equiparará una copia de PostgreSQL con respaldo completo de imágenes almacenadas en Cloudinary.

*Tabla AT20-04. Respaldo y custodia de datos (política prevista)*

| Información | Método / frecuencia | Verificación necesaria |
|---|---|---|
| PostgreSQL / Supabase | Exportación pg_dump semanal según SLA-09 | Archivo legible, fecha, integridad y restauración de prueba |
| Fotografías pendientes | Conservar originales locales hasta confirmación | Conciliar cola local y captures.confirmed_at |
| Fotografías confirmadas | Almacenamiento en Cloudinary | Verificar accesibilidad y política externa de retención |
| Configuración y versión | Git / variables de entorno custodiadas | Comprobar versión restaurable y secretos fuera de repositorio |

*Tabla AT20-05. Objetivos de recuperación y verificación*

| Proceso | RTO / RPO documentado | Condición |
|---|---|---|
| Servicio y datos incluidos en SLA-09 | RTO ≤ 4 h de servicio; RPO ≤ 7 días de registros | Objetivo documental sujeto a validación |
| Capturas móviles no sincronizadas | No borrar antes de confirmación; cero pérdidas (SLA-04) | Dependiente de integridad y custodia del dispositivo |
| Fotos Cloudinary / mensajes / worker | No se especifican RTO/RPO individuales | Pendiente de definir si se requiere compromiso separado |

**6. Activación y recuperación**

El incidente se registrará y clasificará primero conforme al AT-19. Se evaluará activar el plan ante pérdida de integridad, imposibilidad de continuar procesos críticos sin alternativa o indisponibilidad de proveedor externo superior a cuatro horas, condición recogida en la sección 14.7.8. La coordinación comunicará el impacto a la jefatura de fundo.

*Figura AT20-01. Flujo de decisión y restablecimiento*

**AT-18 detecta o usuario reporta → AT-19 registra incidente**

↓

**Evaluar gravedad, datos afectados y posibilidad de operación offline**

↓

**Decidir activación de continuidad y comunicar a responsables**

↓

**Preservar información y ejecutar recuperación autorizada**

↓

**Verificar integridad, accesos, sincronización y decisiones humanas**

↓

**Restablecer servicio, registrar tiempos, evidencias y cierre**

**7. Responsables y comunicación**

*Tabla AT20-06. Responsables durante la contingencia*

| Responsable | Función prevista |
|---|---|
| Oscar Antonio Gutiérrez Rojas | Coordinar respuesta y atención móvil, base de datos e infraestructura |
| Oscar Manuel Guerrero Figari | Diagnosticar y restablecer backend, web y worker IA |
| María Fernanda Zúñiga Matos | Revisar dispositivos y soporte de captura |
| Grace Lucia Peña Reyes (QA Lead) | Registrar evidencia, seguimiento y verificación de calidad |
| Project Manager / jefe de fundo /<br>Product Owner | Evaluar activación, impacto operativo y decisiones excepcionales |

Los avisos se canalizarán mediante el grupo WhatsApp del piloto, correo del equipo y Jira. La comunicación no incluirá contraseñas, tokens ni archivos sensibles.

**8. Pruebas y registro de evidencias**

*Tabla AT20-07. Pruebas controladas de continuidad*

| Prueba | Criterio verificable | Evidencia | Estado |
|---|---|---|---|
| Corte de conexión en campo | La app conserva capturas y permite reanudar envío | Registro móvil / BD | Pendiente |
| Interrupción API en ambiente de prueba | No se pierden capturas no confirmadas | Bitácora de prueba | Pendiente |
| Detención worker<br>IA | Tareas se mantienen y reanudan sin duplicación | ai_tasks / logs | Pendiente |
| Restauración BD aislada | Se recupera copia verificable sin afectar entorno productivo | Acta, pg_dump y consultas | Pendiente |
| Fallo de notificaciones | Aviso permanece registrado y no se falsifica entrega | notifications | Pendiente |

*Tabla AT20-08. Bitácora de activación y recuperación*

| ID / fecha | Incidente y sistema | Acción / responsable | Inicio-fin / resultado | Evidencia |
|---|---|---|---|---|
| Pendiente | Pendiente | Pendiente | Sin evaluar | Pendiente |
| Pendiente | Pendiente | Pendiente | Sin evaluar | Pendiente |

*Figura AT20-02. Relación entre los anexos técnicos*

**AT-18: identificar y verificar desviación**

↓

**AT-19: clasificar, registrar y atender incidente**

↓

**AT-20: activar recuperación extraordinaria cuando corresponda**
