# Anexo 79 (AT-18): Monitoreo

**1. Objetivo y alcance**

Establecer qué componentes serán supervisados, con qué indicadores, frecuencias y responsables, para detectar desviaciones en la captura bilateral, almacenamiento local, sincronización, API, revisión web, análisis de IA y notificaciones. Se aplicará durante las pruebas y el piloto, sin confundir una pérdida temporal de conectividad de campo con una falla del modo offline-first.

**2. Componentes sujetos a monitoreo**

*Tabla AT18-01. Matriz de componentes y métricas*

| Componente | Aspecto por monitorear | Criticidad propuesta |
|---|---|---|
| Móvil React Native/Expo | Validación de imagen, capturas locales y errores | Alta |
| Sincronización | Pendientes, confirmaciones, duplicados e integridad de metadatos | Alta |
| Django / API<br>REST | Disponibilidad, latencia HTTP, errores y autenticación | Alta |
| Web Django +<br>HTMX | Acceso, revisión de casos y carga de vistas | Alta |
| Supabase PostgreSQL | Conectividad, consultas, integridad y tareas ai_tasks | Alta |
| Cloudinary | Carga y consulta controlada de imágenes | Alta |
| Worker YOLO | Cola, tiempos, errores y resultados de IA | Alta |
| Telegram Bot<br>API | Creación y estado de avisos tras confirmación humana | Media |

**3. Herramientas y fuentes**

*Tabla AT18-02. Instrumentos de supervisión*

| Herramienta / fuente | Uso previsto | Situación |
|---|---|---|
| Logs de Django con traceId | Medir peticiones, errores y solicitudes lentas | Previsto en SLA |
| Monitor HTTP<br>/api/v1/health | Verificar disponibilidad cada 5 minutos | Verificar implementación |
| Logs/diagnóstico móvil | Medir calidad de imagen y sincronización | Instrumentación por verificar |
| Supabase / tabla ai_tasks | Consultar registros, cola y tiempos IA | Fuente documentada |
| Cloudinary / registros de captura | Contrastar cargas y confirmaciones | Acceso por verificar |
| Tabla notifications | Seguimiento de avisos | Fuente documentada |
| Postman o Bruno | Pruebas controladas de API | Propuesta |

**4. Indicadores, metas y umbrales**

Se usan las metas SLA-01 a SLA-09 del capítulo XIV. La aprobación del objetivo no constituye evidencia de cumplimiento; cada resultado deberá medirse y registrarse.

*Tabla AT18-03. Matriz de indicadores de desempeño*

| ID /<br>SLA | Indicador y método | Meta del proyecto | Revisión |
|---|---|---|---|
| MON-01<br>/ 01 | Disponibilidad de web y API en horario de servicio | ≥ 99,5 % mensual | Cada 5 min; quincenal |
| MON-02<br>/ 02 | Percentil 95 de respuesta API | ≤ 1,5 s | Quincenal |
| MON-03<br>/ 03 | Tiempo de validación local por foto | ≤ 2 s | Por jornada |
| MON-04<br>/ 04 | Capturas offline perdidas | 0 perdidas | Tras sincronizar |
| MON-05<br>/ 05 | Integridad de metadatos sincronizados | 100 %; mismo día si hay Wi-Fi | Por jornada |
| MON-06<br>/ 06 | Fotos analizadas por IA desde confirmación | 95 % en ≤ 10 min | Quincenal |
| MON-07<br>/ 07 | Entrega de alertas por<br>Telegram | 95 % en ≤ 5 min* | Quincenal |
| MON-08<br>/ 08 | Casos trazables; avisos indebidos | 100 % trazables; 0 indebidos | Quincenal |
| MON-09<br>/ 09 | Respaldo BD y restauración | Semanal; ≤ 4 h servicio | Semanal / prueba |

**5. Procedimiento de monitoreo**

*Figura AT18-01. Flujo propuesto de monitoreo y derivación*

↓

↓

↓

↓

| SIN ANOMALÍA<br>Continuar seguimiento programado | ANOMALÍA CONFIRMADA<br>Registrar y derivar al AT-19 si constituye incidente |
|---|---|

Los registros incluirán fecha, servicio, valor, meta, posible causa y evidencia. Tras una corrección se repetirá la medición, si existe una interrupción mayor, se evaluará el AT-20.

**6. Frecuencias y responsables**

*Tabla AT18-04. Responsabilidades de supervisión*

| Actividad | Frecuencia | Responsable |
|---|---|---|
| Disponibilidad HTTP | Cada 5 min, horario de servicio | Administración de plataforma |
| API, web y worker IA | Durante operación / consolidación quincenal | Oscar Manuel Guerrero Figari |
| App, sincronización e infraestructura | Por jornada / revisión quincenal | Oscar Antonio Gutiérrez Rojas |
| Equipos y prototipo físico | Antes de jornadas / incidentes | María Fernanda Zúñiga Matos |
| Consolidación y reporte de SLA | Quincenal | Grace Lucia Peña Reyes (QA Lead) |
| Revisión con responsables y resumen | Quincenal / mensual | Equipo y dirección del proyecto |

**7. Clasificación y comunicación de alertas**

Se propone clasificar las mediciones como NORMAL (cumple meta), ADVERTENCIA (desviación a verificar) o CRÍTICA (interrupción, afectación significativa o pérdida de integridad). El nivel de prioridad del incidente se definirá en AT-19 y no dependerá únicamente de un valor aislado.

*Figura AT18-02. Relación de supervisión, incidentes y continuidad*

**AT-18: detecta desviación y verifica impacto**

↓

**AT-19: clasifica, asigna, atiende y cierra el incidente**

↓

Los canales de seguimiento previstos son Telegram del piloto, correo y Jira. No se transmitirán secretos, tokens ni evidencias fotográficas sensibles mediante alertas.

**8. Registro editable de mediciones**

*Tabla AT18-05. Bitácora de monitoreo*

| ID | Fecha/hora | Componente / indicador | Valor / meta | Estado | Evidencia / acción |
|---|---|---|---|---|---|
| REG- 001 | Pendiente | Pendiente | Pendiente | Sin evaluar | Pendiente |
| REG- 002 | Pendiente | Pendiente | Pendiente | Sin evaluar | Pendiente |
| REG- 003 | Pendiente | Pendiente | Pendiente | Sin evaluar | Pendiente |

**9. Validación y evidencias previstas**

*Tabla AT18-06. Comprobaciones de monitoreo pendientes*

| Prueba | Criterio verificable | Evidencia prevista | Estado |
|---|---|---|---|
| Salud de API | Se registra respuesta y hora | Log HTTP | Pendiente |
| Latencia API | Se calcula duración/percentil | Logs Django | Pendiente |
| Modo offline | Capturas permanecen guardadas | Registro local | Pendiente |
| Retorno de conexión | Sincroniza sin pérdidas o duplicados | BD y cola local | Pendiente |
| Procesamiento<br>IA | Tarea con inicio, fin y resultado | ai_tasks / worker | Pendiente |
| Confirmación y aviso | Aviso solo en casos confirmados | Registros de revisión y notificación | Pendiente |
| Respaldo | Existe copia verificable y prueba controlada | Bitácora y acta de restauración | Pendiente |
