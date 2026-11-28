# CAPÍTULO XIV. GESTIÓN DEL SERVICIO

## 14.1. Definición del servicio

El servicio SIMDPCVAR es un servicio tecnológico de apoyo al monitoreo fitosanitario en campo, que Agrícola Riachuelo S.A.C. ofrece a sus áreas operativas. Sustituye el registro manual en Excel por un flujo digital de captura, análisis y validación de evidencia para la detección temprana de Planococcus ficus en el viñedo.

El servicio combina cuatro elementos:

- Una aplicación móvil offline-first, operada en tres celulares Android (controlador, Cámara 1 y Cámara 2) montados en el soporte tipo T, que captura dos fotografías del mismo punto de cada planta y recorre ambas caras de la hilera con las pasadas lateral A y lateral B (SC-04) y las asocia a lote, hilera, zona, lado, marcador, secuencia y coordenadas GPS.

- Un modelo de inteligencia artificial (YOLO26n exportado a ONNX) que analiza las imágenes sincronizadas y señala posibles indicios de plaga.

- Una plataforma web (Django + HTMX, con mapas Leaflet sobre teselas satelitales Esri y OpenStreetMap) donde el especialista fitosanitario revisa, confirma o descarta cada hallazgo.

- Alertas por Telegram (Telegram Bot API, SC-02) al jefe de fundo y al supervisor de zona, emitidas cuando el especialista confirma un hallazgo o, como alerta temprana marcada «falta la revisión del especialista», cuando la IA encuentra evidencia visual fuerte (SC-03; ver 14.7.4).

### 14.1.1 Propósito y valor para el negocio:

El servicio responde a los problemas identificados en el análisis AS-IS (Cap. IV) y se alinea con los objetivos empresariales de la organización:

| Problema del proceso actual | Valor que aporta el servicio |
|---|---|
| Registro manual propenso a error humano | Captura estructurada con validación automática de calidad de imagen |
| Sin evidencia clara ni trazable del trabajo | Evidencia fotográfica asociada a cada hallazgo, con historial de revisiones |
| GPS impreciso entre hileras cercanas | Ubicación por lote, hilera, marcador y secuencia, además de coordenadas GPS |
| Consolidación lenta en Excel | Sincronización automática hacia una plataforma centralizada |
| Sin alertas oportunas | Notificación por Telegram tras la confirmación del especialista |

Con ello el servicio contribuye a reducir las horas-persona del monitoreo, acortar el tiempo de respuesta ante hallazgos y conservar registros verificables para SENASA y GlobalG.A.P.

### 14.1.2 Usuarios y clientes del servicio:

| Rol | Interesado | Uso del servicio |
|---|---|---|
| Operador de monitoreo | STK-05 | Captura fotográfica en campo con la app móvil |
| Especialista fitosanitario | STK-03 | Revisa indicios, confirma o descarta hallazgos y registra la observación (Product Owner y validador técnico) |
| Jefe de fundo | STK-02 | Recibe alertas y consulta el dashboard y el mapa de hallazgos |
| Supervisor de zona | STK-04 | Recibe alertas y da seguimiento a los casos de su zona |
| Gerencia General | STK-01 | Patrocinador; recibe informes de estadoy resultados del piloto |

### 14.1.3 Alcance del servicio:

- Incluye:

  - Captura bilateral de fotografías, con validación de nitidez, exposición y movimiento (CU1, CU2).

  - Almacenamiento local y sincronización automática al recuperar la conexión (CU3).

  - Análisis de imágenes con IA (CU4).

  - Revisión, confirmación o descarte de hallazgos y registro de la observación fitosanitaria (CU5, CU6, CU7).

  - Notificación por Telegram de hallazgos confirmados (CU8).

  - Dashboard y mapa georreferenciado de hallazgos (CU9).

  - Autenticación y control de acceso por roles.

- No incluye:

  - La detección de plagas ocultas o no visibles en las fotografías.

  - La decisión o ejecución del tratamiento fitosanitario, que corresponde al especialista y al personal de campo.

  - El suministro de los celulares y los soportes tipo T, salvo los entregados para el piloto.

  - Una app móvil para el supervisor o el jefe de fundo; ellos consumen alertas por Telegram y la plataforma web.

  - Ampliaciones a otros cultivos, plagas o fundos fuera del alcance del piloto.

### 14.1.4 Catálogo de servicios:

| Código | Servicio | Descripción | Destinatario |
|---|---|---|---|
| SRV-01 | Monitoreo fotográfico en campo | Captura bilateral con validación de calidad y funcionamiento sin conexión | Operadores de monitoreo |
| SRV-02 | Detección asistida por IA | Análisis automático de imágenes y marcado de indicios | Especialista fitosanitario |
| SRV-03 | Validación fitosanitaria | Revisión, confirmación o descarte de hallazgos y registro de observaciones | Especialista fitosanitario |
| SRV-04 | Alertas de hallazgos confirmados | Notificación por Telegram con foto, ubicación y enlace de acceso controlado | Jefe de fundo y supervisores |
| SRV-05 | Consulta de dashboard y mapa | Avance de recorridos, ubicación de hallazgos y estado de revisiones | Jefatura, supervisores y especialista |
| SRV-06 | Trazabilidad fitosanitaria | Historial de evidencias y revisiones para auditorías | Gerencia de Calidad y Certificaciones |

## 14.2. Solicitudes de servicio

Una solicitud de servicio es un pedido formal de un usuario para obtener algo que el servicio ya contempla: un acceso, una consulta, una configuración o una mejora menor.

- Catálogo de solicitudes:

| Código | Solicitud | Solicitante | Atendido por | Ejemplo |
|---|---|---|---|---|
| SOL-01 | Alta, baja o modificación de usuarios y roles | Jefe de fundo | Administrador | Registrar un nuevo operador o supervisor |
| SOL-02 | Registro o cambio de dispositivos | Supervisor de zona | Administrador | Vincular un nuevo juego de controlador y cámaras |
| SOL-03 | Actualización de catálogos | Especialista fitosanitario | Administrador | Agregar un lote, una hilera o un marcador |
| SOL-04 | Gestión de destinatarios de alertas | Jefe de fundo | Administrador | Agregar o quitar un destinatario de Telegram por lote o zona |
| SOL-05 | Consulta o reporte de casos | Supervisor, jefe de fundo, Gerencia de Calidad | Especialista / Administrador | Extraer el historial de hallazgos de un lote para una auditoría |
| SOL-06 | Reasignación de zonas o rutas | Supervisor de zona | Administrador | Cambiar la zona asignada a un operador |
| SOL-07 | Capacitación o soporte de uso | Cualquier usuario | Equipo de desarrollo | Reforzar el uso de la app antes del recorrido |
| SOL-08 | Ajuste menor del servicio | Product Owner | Equipo de desarrollo | Mejora que entra al Product Backlog como historia de usuario |

- Quién puede solicitar qué:

| Rol | Puede solicitar | No puede solicitar |
|---|---|---|
| Administrador | Todas; también las ejecuta | N/A |
| Especialista fitosanitario | SOL-03, SOL-05, SOL-08 | Alta de usuarios, cambio de destinatarios |
| Supervisor o jefe de fundo | SOL-01, SOL-02, SOL-04, SOL-05, SOL-06, SOL-07 | Cambios al modelo de IA o a reglas de negocio |
| Operador de campo | SOL-07 (vía su supervisor) | Cualquier acceso a la plataforma web |

- Flujo de atención

  - Registro: el solicitante comunica el pedido por un canal; se registra con código, solicitante, descripción y fecha.

  - Clasificación: se identifica si es solicitud de servicio, incidente o cambio.

  - Validación: se verifica que el rol del solicitante tenga permiso.

  - Atención: la ejecuta el responsable asignado.

  - Confirmación y cierre: el solicitante confirma la atención y se cierra el registro.

## 14.3. Solicitudes de acceso

La gestión de solicitudes de acceso del Sistema Inteligente para el Monitoreo y Detección de Plagas en Cultivos de Vid en Agrícola Riachuelo (SIMDPCVAR) permitirá registrar y atender las altas, modificaciones, suspensiones y bajas de usuarios, de acuerdo con las responsabilidades que desempeñan en el monitoreo fitosanitario. Este procedimiento corresponde a la solicitud SOL-01 descrita en el apartado 14.2 y comprende la aplicación móvil, la plataforma web y los servicios de información vinculados. El objetivo es impedir el acceso no autorizado y asegurar que cada operación pueda relacionarse con un usuario identificado. Cada usuario contará con una cuenta individual y con permisos limitados a sus actividades. El operador registrará recorridos, capturará fotografías bilaterales y sincronizará evidencias; el especialista fitosanitario revisará las imágenes y confirmará o descartará hallazgos; los supervisores y el Jefe de Fundo consultarán los casos e indicadores de sus áreas de responsabilidad. El administrador gestionará las cuentas, los roles y las configuraciones autorizadas, sin que sus atribuciones administrativas sustituyan la validación fitosanitaria. La Gerencia General accederá a los reportes habilitados para su función, sin permisos de edición operativa por defecto. La solicitud deberá identificar al solicitante, usuario beneficiario, rol requerido, zona o ámbito de acceso, justificación, fecha y responsable. El Jefe de Fundo o la autoridad operativa designada verificará la necesidad y aprobará la asignación; en caso de permisos privilegiados, la autorización deberá ser explícita y quedar documentada. Posteriormente, el administrador creará o modificará la cuenta y comprobará que el usuario puede ejecutar las funciones permitidas, pero no acciones restringidas. La atención finalizará con la comunicación al solicitante y el registro de la fecha de cierre. Cuando una persona cambie de función o deje de participar en el proyecto, sus permisos deberán revisarse o revocarse oportunamente. Como controles complementarios se aplicarán el principio de mínimo privilegio, el uso de conexiones cifradas y el registro de inicios de sesión, modificaciones de permisos, intentos fallidos y revocaciones, según los mecanismos implementados en el sistema. Las credenciales no deberán compartirse ni incluirse en los documentos del proyecto. La revisión de accesos verificará también la protección de fotografías, ubicaciones y datos fitosanitarios. Su cumplimiento se evaluará mediante las autorizaciones conservadas, las pruebas de permisos y el seguimiento de accesos activos

y revocados; estos registros constituirán evidencia del funcionamiento del procedimiento durante el piloto, sin anticipar resultados que todavía no hayan sido medidos.

## 14.4. Gestión de cambios

La gestión de cambios de SIMDPCVAR regulará toda modificación que pueda afectar los requisitos funcionales o no funcionales, la arquitectura, las interfaces, el modelo de visión artificial, la información almacenada, la seguridad, el presupuesto, el cronograma o los criterios de aceptación. Se distinguirán las correcciones de defectos, las acciones preventivas, las mejoras funcionales y los cambios de configuración o infraestructura. Las solicitudes de servicio rutinarias seguirán el procedimiento del apartado 14.2; cuando un pedido modifique una línea base o introduzca nuevas obligaciones, deberá tramitarse como cambio formal. Toda propuesta se registrará con un identificador, solicitante, fecha, motivo, descripción, prioridad, versión afectada y entregables comprometidos. El responsable del proyecto, con apoyo del equipo técnico y los interesados involucrados, analizará los efectos sobre alcance, EDT, historias de usuario, dependencias, plazos, costos, riesgos, pruebas y operación en campo. Para cambios relativos a la detección de indicios de Planococcus ficus se solicitará la evaluación del especialista fitosanitario, dado que el resultado automatizado constituye una sugerencia que requiere validación humana. Cuando se altere una línea base, el cambio necesitará la aprobación de la autoridad del proyecto antes de su ejecución. La decisión de aprobar, rechazar o diferir la solicitud se justificará por su necesidad, viabilidad, esfuerzo, beneficio y riesgo. En el análisis económico se contrastará el efecto con la línea base de costos y las reservas previstas; en el temporal, con el cronograma y los compromisos de los sprints. También se determinará si el cambio afecta la captura bilateral, el funcionamiento sin conectividad, la integridad de la sincronización, la georreferenciación, los roles o la continuidad del seguimiento de hallazgos. Ninguna propuesta se considerará aprobada únicamente por haber sido incorporada al Product Backlog o comentada en una reunión. Los cambios autorizados se planificarán en el sprint o ventana correspondiente y se implementarán bajo control de versiones. Antes de su liberación se revisarán el código, la configuración y, cuando corresponda, las migraciones de la base de datos, el almacenamiento de imágenes y la versión del modelo de inteligencia artificial. El equipo efectuará pruebas funcionales, de integración, regresión y seguridad; en modificaciones fitosanitarias, la comprobación incluirá evidencias revisadas por el especialista. Para una liberación que pueda comprometer los datos o la continuidad del servicio se conservará un respaldo y un

procedimiento de reversión. Los registros locales pendientes no se eliminarán hasta verificar su sincronización. Finalmente, el responsable del proyecto actualizará los requisitos, la matriz de trazabilidad, los planes y los documentos técnicos que resulten afectados, comunicará la decisión a los usuarios correspondientes y cerrará la solicitud con las evidencias de implementación y pruebas. El seguimiento permitirá identificar cambios pendientes, aprobados, rechazados o diferidos, así como defectos posteriores, reversiones y desviaciones de alcance, tiempo o costo. Los resultados y métricas se reportarán únicamente cuando existan registros verificables; de este modo se mantendrá la coherencia entre la solución implementada y la documentación aprobada.

## 14.5. Capacidad

La gestión de capacidad del Sistema Inteligente para el Monitoreo y Detección de Plagas en Cultivos de Vid en Agrícola Riachuelo (SIMDPCVAR) se orientará a determinar si los recursos tecnológicos permiten atender las operaciones de captura bilateral, almacenamiento local, sincronización de evidencias, procesamiento mediante inteligencia artificial y consulta de hallazgos fitosanitarios. Se considerarán la demanda generada durante las jornadas de monitoreo, el crecimiento de los registros y las limitaciones de conectividad del entorno agrícola.

La planificación tomará como referencia la arquitectura de despliegue final: aplicación móvil React Native y Expo con SQLite, backend y plataforma web en Django, PostgreSQL administrado en Supabase (sin PostGIS), almacenamiento de fotografías en Cloudinary y un worker asíncrono de Django con YOLO26n exportado a ONNX. La suficiencia de estos componentes se evaluará en función del procesamiento, la memoria, el almacenamiento y la transferencia de información.

**Tabla 38. Matriz de planificación y gestión de capacidad del sistema SIMDPCVAR**

| Componente | Recurso requerido | Criterio de dimensionamiento | Acción de gestión propuesta |
|---|---|---|---|
| Aplicación móvil (React<br>Native, Expo y SQLite) | Almacenamiento local y memoria | Fotografías y metadatos acumulados en periodos sin conectividad. | Verificar el espacio disponible y liberar archivos locales solo después de confirmar su sincronización. |
| Backend<br>Django / API REST | CPU, RAM y atención concurrente | Solicitudes de autenticación, registro, sincronización y consultas web. | Medir el consumo de recursos, optimizar operaciones y evaluar ampliaciones según la demanda. |
| Supabase (PostgreSQL) | Espacio, conexiones y consultas | Crecimiento de sesiones, capturas, coordenadas, casos y tareas. | Examinar límites contratados, uso de almacenamiento, índices y consultas. |
| Cloudinary | Espacio y transferencia | Número, tamaño y frecuencia de carga de fotografías. | Revisar cuotas y consumo; establecer criterios de conservación de evidencias. |
| Worker de IA (YOLO26n/ONNX) | Procesamiento, memoria y cola | Volumen de imágenes pendientes y acumulación en ai_tasks. | Supervisar la cola y ajustar recursos o ejecución del worker de ser necesario. |
| Conectividad y sincronización | Ancho de banda y transferencia | Fotografías pendientes y calidad variable de la conexión en campo. | Mantener sincronización diferida, reintentos controlados y verificación de integridad. |

Para estimar la demanda se emplearán los registros por jornada de sesiones, pasadas, secuencias y fotografías capturadas. El almacenamiento fotográfico proyectado se calculará a partir de la cantidad estimada de imágenes y su tamaño promedio; la demanda de Django y del worker se determinará mediante la frecuencia de solicitudes y de tareas de análisis. Las mediciones iniciales del piloto servirán como referencia de volumen observado, no como prueba de capacidad máxima ni de concurrencia.

Se contrastará la demanda estimada con la capacidad disponible, verificando consumo de CPU y RAM, espacio de almacenamiento, utilización de conexiones y acumulación de tareas pendientes. Ante incrementos de demanda se priorizarán la optimización de consultas e índices, el control de cargas fotográficas, la gestión segura de archivos sincronizados y la organización de tareas de inteligencia artificial. Solo cuando estas medidas resulten insuficientes se evaluará ampliar los recursos o los planes cloud, considerando el presupuesto del proyecto.

La capacidad se validará con escenarios representativos de las jornadas de monitoreo, incluidos periodos sin conectividad y sincronización posterior de evidencias acumuladas, además de solicitudes y tareas coincidentes. Se considerará adecuada cuando la carga prevista pueda atenderse sin saturación sostenida, las evidencias se sincronicen sin pérdidas y los registros conserven su integridad. Las evidencias incluirán registros de uso de recursos, consumo de almacenamiento, cola de tareas y resultados de las pruebas, sin atribuir cifras a mediciones aún no realizadas.

## 14.6. Rendimiento

La gestión del rendimiento de SIMDPCVAR permitirá evaluar el comportamiento de sus componentes durante las operaciones de monitoreo fitosanitario. Se medirán los tiempos de respuesta y la estabilidad de los servicios bajo condiciones representativas de utilización, considerando las actividades realizadas en campo y las consultas efectuadas desde la plataforma web.

La evaluación priorizará la validación local de fotografías, la sincronización de capturas, la respuesta de la API REST de Django, las consultas a Supabase y el procesamiento asíncrono de imágenes mediante YOLO26n (ONNX). Como referencia se utilizarán los requisitos no funcionales y los criterios del servicio ya documentados, diferenciando las metas de los resultados efectivamente medidos.

**Tabla 39. Matriz de criterios para la evaluación del rendimiento del sistema SIMDPCVAR**

| Operación crítica | Indicador | Método y herramienta propuestos | Criterio de aceptación |
|---|---|---|---|
| Validación local de fotografías | Tiempo de validación P95 | Marcas temporales en la aplicación móvil. | ≤ 2 segundos, según RNF02. |
| API REST Django | Respuesta P95 y tasa de errores | Solicitudes HTTP con Apache JMeter y registros de Django. | P95 ≤ 1,5 segundos; registrar errores y evaluar su causa. |
| Sincronización de evidencias | Duración e integridad | Contraste de registros locales, Cloudinary y confirmaciones en Django. | Sin pérdidas ni duplicados; completar durante la jornada si hay conectividad. |
| Procesamiento de IA (YOLO26n/ONNX) | Tiempo desde confirmación hasta resultado (P95) | Marcas temporales en ai_tasks y registros del worker. | Al menos 95 % en ≤ 10 minutos. |
| Consultas de hallazgos | Latencia de consultas | Registros de Django y análisis de consultas PostgreSQL. | Objetivo por definir tras mediciones iniciales. |
| Plataforma web | Tiempo de carga y errores | Chrome DevTools en bandeja, mapas y evidencias. | Objetivo por definir tras mediciones iniciales. |

*Los umbrales establecidos corresponden a criterios documentados, no a resultados alcanzados. Apache JMeter y Chrome DevTools se proponen para pruebas futuras; los indicadores sin umbral definido deberán establecerse a partir de mediciones*

*verificables.*

Para la evaluación se preparará un entorno controlado con datos representativos de las jornadas de monitoreo. En la API REST se ejecutarán solicitudes normales y concurrentes mediante Apache JMeter, aumentando gradualmente la carga dentro de límites seguros y registrando tiempos de respuesta y errores. Se conservarán las condiciones de prueba para permitir comparaciones posteriores; las pruebas de estrés no se ejecutarán directamente sobre el entorno productivo sin medidas de control.

En la aplicación móvil se medirá el tiempo de validación de calidad y la duración de la sincronización, distinguiendo la espera por conectividad de la transferencia efectiva. El procesamiento de IA se evaluará con las marcas temporales de ai_tasks, separando el tiempo en cola del tiempo de inferencia. Para la plataforma web se examinarán la bandeja de casos, los mapas y las evidencias mediante Chrome DevTools y el registro de consultas PostgreSQL.

Las mediciones se compararán con los criterios definidos para identificar limitaciones de procesamiento, conectividad y acceso a datos. Si se detectan desviaciones, se propondrán acciones como mejorar la validación local de fotografías, optimizar consultas e índices, reducir operaciones innecesarias y ajustar la ejecución del worker. Cada cambio deberá verificarse con una nueva medición realizada bajo condiciones equivalentes, conservando registros y reportes que permitan contrastar los resultados.

Como referencia inicial, el apartado 14.8.4 documenta un P95 de 2,90 segundos para la validación local, superior al objetivo de 2 segundos de RNF02. También registra un promedio de inferencia de 1,35 segundos para las fotografías procesadas por IA y la

confirmación de 196 de 196 fotografías sincronizadas, sin duplicados. Estos resultados proceden de la primera medición del piloto y no demuestran el rendimiento del sistema con una concurrencia máxima de usuarios.

La evaluación se sustentará con reportes de tiempos de respuesta, registros de errores, resultados de solicitudes HTTP, mediciones de consultas y registros del worker de IA. Las evidencias permitirán comprobar el cumplimiento de los criterios documentados y orientar las mejoras antes de extender la operación del sistema.

## 14.7. SLA

El Acuerdo de Nivel de Servicio (SLA, Service Level Agreement) establece los compromisos medibles que el equipo del proyecto, como proveedor del servicio SIMDPCVAR durante el piloto, asume frente a Agrícola Riachuelo S.A.C. Toma como base el catálogo de servicios (14.1.4), el catálogo de solicitudes (14.2), los requerimientos no funcionales (Tabla 28) y los objetivos del Plan de Gestión de la Calidad (Anexo 22). Cada nivel de servicio se mide con registros que el propio sistema genera: marcas de tiempo en la base de datos, cola de tareas de IA, historial de revisiones, auditoría y registro de peticiones. Así su cumplimiento puede verificarse con datos y no con apreciaciones.

### 14.7.1 Partes, alcance y vigencia del acuerdo:

| Elemento | Detalle |
|---|---|
| Proveedor del servicio | Equipo del proyecto SIMDPCVAR (Universidad Tecnológica del Perú). Administra la plataforma web, la API, la base de datos, el almacenamiento de evidencias, el modelo de IA y la app móvil durante el piloto. |
| Cliente | Agrícola Riachuelo S.A.C., representada por el jefe de fundo, Pedro Jesús Palacios Chanco (STK-02), patrocinador operativo del piloto. |
| Validador técnico | Ing. Max Daniel Gutiérrez Huamán (STK-03), especialista fitosanitario y validador técnico. |
| Servicios cubiertos | SRV-01 a SRV-06 del catálogo de servicios (14.1.4). |
| Usuarios | Operadores de monitoreo (app móvil); especialista fitosanitario, supervisores de zona y jefe de fundo (plataforma web y alertas). |
| Vigencia | Desde la puesta en producción del piloto hasta el cierre del proyecto (20/11/2026, según el Acta de Constitución). Puede renovarse si la empresa decide continuar con el servicio. |
| Revisión del acuerdo | Quincenal, en la reunión con la jefatura de fundo prevista en la gestión de comunicaciones (6.13), y al cierre de cada sprint. Un cambio de metas se tramita por gestión de cambios (6.16). |
| Naturaleza | Acuerdo académico sin penalidades económicas. Cada incumplimiento genera una acción correctiva en el registro de impedimentos (7.17) y, si corresponde, una historia en el Product Backlog. |

### 14.7.2 Horario de servicio y canales de soporte:

Por su diseño offline-first, la captura en campo no depende de la plataforma: la app guarda las fotos y sus metadatos en el celular y los sincroniza después. Por eso el horario distingue la operación de campo, la disponibilidad de la plataforma y la atención del equipo de soporte.

| Concepto | Horario o condición |
|---|---|
| Jornada de monitoreo en campo | Lunes a sábado, de 06:00 a 14:00 (hora de Lima). |
| Disponibilidad de la plataforma web y la API | Todos los días, 24 horas. El compromiso de disponibilidad (SLA-01) se mide dentro del horario de servicio: lunes a sábado, de 06:00 a 14:00. |
| Horario de soporte | Lunes a sábado, de 06:00 a 14:00. Fuera de ese horario solo se atienden incidentes de prioridad crítica, sin plazo comprometido. |
| Día de servicio | Lunes a sábado, salvo feriados sin jornada de campo. Los plazos expresados en «días de servicio» u «horas de servicio» se cuentan con este calendario. |
| Mantenimiento programado | Domingos, o de lunes a sábado después de las 14:00, con aviso al grupo del piloto con al menos 24 horas de anticipación. Cada actualización de la plataforma deja la web sin respuesta entre 1 y 2 minutos. |
| Canales de soporte | Grupo de Telegram del piloto (canal principal), correo del equipo del proyecto y registro de cada caso en el tablero Jira del proyecto para su trazabilidad. |

### 14.7.3 Niveles de servicio comprometidos:

| Código | Indicador | Servicio | Meta | Medición y fuente |
|---|---|---|---|---|
| SLA-01 | Disponibilidad de la plataforma web y la API | SRV-02 a<br>SRV-06 | ≥ 99,5 % mensual en el horario de servicio | (Minutos del horario de servicio − minutos de caída no programada)÷ minutos del horario de servicio × 100. Monitor externo que consulta<br>/api/v1/health cada 5 minutos y registro de incidentes. |
| SLA-02 | Tiempo de respuesta de la API REST | SRV-01,<br>SRV-05 | Percentil 95 ≤ 1,5 s (RNF04) | Registro de peticiones de Django con traceId; las que superan 1 500ms quedan marcadas como lentas. No incluye la subida de la foto, que va directo a Cloudinary. |
| SLA-03 | Validación local de calidad de imagen | SRV-01 | ≤ 2 s por foto (RNF02) | Registro de eventos de la app (exportación de diagnóstico del controlador y de las cámaras). |
| SLA-04 | Conservación de evidencias sin conexión | SRV-01 | 0 capturasperdidas; ninguna fotose borra delcelular antes de confirmar susincronización(RN-05) | Capturas por sesión en la app frente a las registradas en la tabla captures. |
| SLA-05 | Sincronización de capturas | SRV-01 | 100 % sin pérdida de lote, hilera,lateral, segmento, marcador y secuencia,el mismo día de la jornada si el controlador tieneWi-Fi | Cola de sincronización de la app y fecha de confirmación (confirmed_at) en captures. |
| SLA-06 | Análisis de imágenes por IA | SRV-02 | 95 % de las fotos analizadas en ≤ 10 min desdesu confirmaciónen la plataforma | Tabla ai_tasks: finished_at − requested_at; tiempo de inferencia en processing_ms. |
| SLA-07 | Emisión de alertas | SRV-04 | Aviso generado en la misma transacción que la confirmación; 95<br>% entregados por Telegram en ≤5min | Tabla notifications: hora de creación, hora de envíoy estado de cada aviso. |
| SLA-08 | Integridad dela decisión ytrazabilidad | SRV-03,<br>SRV-06 | 100 % de casosreconstruibles desde la captura hasta la decisión;<br>0 avisos sobre casos pendientes o descartados (RN-02) | Tablas review_cases, human_reviews(solo inserción, una decisión vigente por caso) yaudit_events. |
| SLA-09 | Respaldo y recuperación de datos | Todos | Respaldo semanal de la base (pérdida máxima de 7 días de registros); restablecimiento en ≤ 4 h de servicio | Registro de respaldos (pg_dump) y una prueba de restauración durante el piloto. Las fotos se conservan en Cloudinary y en el controlador hasta su sincronización. |

Con una ventana de lunes a sábado de 06:00 a 14:00 (alrededor de 208 horas al mes), la meta de 99,5 % admite como máximo unos 62 minutos de indisponibilidad no programada al mes.

### 14.7.4 Reglas de emisión de alertas:

El servicio SRV-04 emite avisos por dos vías. La vía regular es la confirmación del especialista fitosanitario. La segunda es una alerta temprana cuando el modelo de IA encuentra evidencia visual fuerte. Esta alerta no reemplaza la decisión humana: el caso queda como «Confirmado por IA» hasta que el especialista lo ratifique o lo descarte, y solo con su confirmación se registra la observación fitosanitaria (CU7). Las reglas vigentes con el modelo 2026.10-v2 (umbral de confianza 0,40) son las siguientes:

| Resultado | Condición | Acción del servicio | Plazo |
|---|---|---|---|
| Descartada porlaIA | Confianza máxima < 0,40, o menos de2 cajas con confianza<br>≥ 0,40 | No abre caso. La foto queda en «Descartadas por la IA», de donde el especialista toma la muestra semanal de KPI-18. | — |
| Pendiente de revisión | No cumple la regla de descarte ni la de confirmaciónporIA | Abre un caso en la bandeja del especialista, sin aviso. | Decisión en ≤ 24 h de servicio (compromiso del cliente) |
| Confirmadopor IA (alertatemprana) | 2 o más cajascon confianza ≥ 0,70, o 3 o más cajascon confianza ≥ 0,50 | Aviso al jefe de fundo y al supervisor dellote, identificado como indicio confirmado por la IA. | Aviso según SLA-07;<br>ratificación en<br>≤ 24 h de servicio |
| Confirmado por el especialista | Decisión del especialista en la plataforma web | Aviso con lote, hilera, lateral, segmento, marcadory enlace al caso; registro dela observación fitosanitaria. | Aviso según SLA-07 |
| Descartado o evidencia insuficiente | Decisión del especialista | Sin aviso. Si el caso tuvo alerta temprana, el descarte se comunica por el grupo del piloto. | — |

Los umbrales se guardan en la configuración del modelo activo y se aplican con el comando aplicar_triage. Todo cambio de umbrales o de modelo pasa por gestión de cambios (6.16) con aprobación del Product Owner, porque modifica qué hallazgos generan avisos. Los avisos se envían por Telegram Bot API, que reemplazó a WhatsApp Cloud API por los problemas de configuración de la cuenta de empresa de Meta (número verificado y plantillas aprobadas) (SC-02); cada aviso queda registrado en el módulo de Notificaciones de la plataforma.

### 14.7.5 Prioridad y tiempos de atención de incidentes:

Un incidente es una interrupción o degradación no planificada del servicio. Se clasifica según su impacto en la jornada de campo y en la revisión de hallazgos. Los tiempos corren dentro del horario de soporte.

| Prioridad | Criterio y ejemplos | Respuesta | Restablecimiento |
|---|---|---|---|
| P1 –<br>Crítica | El servicio no puede usarse y no hay alternativa: la app no permite capturar en ningún juego de celulares; la web o la API no responden para ningún usuario; pérdida o corrupción de evidencias; se emitió un aviso sobre un caso no confirmado. | ≤ 30 min | ≤ 4 h |
| P2 – Alta | Una función crítica falla, pero el trabajo continúa: el worker de IA está detenido y las fotos esperan análisis; un controlador no sincroniza; los avisos no salen; falla la vinculación de cámaras en un juego. | ≤ 1 h | ≤ 8 h, antes del inicio de la siguiente jornada |
| P3 – Media | Falla parcial sin impacto en la jornada: el mapa no muestra capas, las miniaturas no cargan, un reporte CSV falla o un catálogo quedó mal cargado. | ≤ 4 h | ≤ 3 días de servicio |
| P4 – Baja | Consultas, ajustes visuales o de textos y mejoras menores. | ≤ 1 día de servicio | Se planifica en el siguiente sprint |

El tiempo de respuesta va desde el reporte hasta que un integrante del equipo confirma la recepción e inicia el diagnóstico. El tiempo de restablecimiento termina cuando el servicio vuelve a operar, aunque sea con una solución temporal; la causa raíz se analiza y se registra después.

### 14.7.6 Tiempos de atención de solicitudes de servicio:

| Código | Solicitud | Plazo de atención | Condición |
|---|---|---|---|
| SOL-01 | Alta, bajao modificación de usuarios y roles | ≤ 1 día de servicio | El bloqueo de una cuenta por seguridad se atiende en ≤ 2 h. |
| SOL-02 | Registro o cambio de dispositivos | ≤ 1 día de servicio | La revocación de un celular perdido se atiende en ≤ 2 h. Los tres celulares de un juego quedan con la misma versión de la app. |
| SOL-03 | Actualización de catálogos | ≤ 2 días de servicio | Se pide antes de la jornada; luego el controlador usa «Catálogos → Actualizar». |
| SOL-04 | Gestión de destinatarios de alertas | ≤ 1 día de servicio | Solo destinatarios de Telegram que hayan dado su consentimiento. |
| SOL-05 | Consulta o reporte de casos | ≤ 2 días de servicio | Los roles web descargan el reporte CSV por su cuenta; el plazo aplica a extracciones especiales para auditoría. |
| SOL-06 | Reasignación de zonas o rutas | ≤ 1 día de servicio | — |
| SOL-07 | Capacitación o soporte de uso | Soporte enjornada:inmediato.<br>Capacitación: se programa en ≤2 días de servicio | — |
| SOL-08 | Ajuste menor del servicio | Se evalúa en el siguienteSprintPlanning | Sin plazo de entrega comprometido; depende de su prioridad enel Product Backlog. |

### 14.7.7 Responsabilidades de las partes:

**Del proveedor (equipo del proyecto):**

- Operar y vigilar el servidor, los contenedores de la web y del worker de IA, la base de datos en Supabase y el almacenamiento en Cloudinary.

- Publicar las versiones fuera de la jornada de campo y mantener la misma versión de la app en los tres celulares de cada juego.

- Mantener activo un modelo de IA aprobado y conservar el anterior para volver atrás si el nuevo falla.

- Custodiar las credenciales solo en el servidor; la app y el navegador nunca las reciben.

- Ejecutar los respaldos, medir los indicadores y entregar el informe quincenal de niveles de servicio.

**Del cliente (Agrícola Riachuelo S.A.C.):**

- Revisar los casos pendientes y ratificar o descartar los «Confirmado por IA» en un plazo de 24 horas de servicio (especialista fitosanitario).

- Disponer de una red Wi-Fi o punto de acceso para vincular los celulares en campo y para sincronizar al cierre de cada jornada.

- Entregar los celulares cargados, con power bank, al inicio de la jornada y conservar los equipos del piloto.

- Reportar los incidentes por el canal acordado apenas ocurran, indicando el celular, la sesión y la hora.

- Mantener al día la lista de destinatarios de alertas y proporcionar las coordenadas reales de inicio y fin de las hileras para la ubicación de los hallazgos.

### 14.7.8 Exclusiones:

No se consideran incumplimiento del SLA, aunque sí se registran y se reportan:

- La falta de conectividad en campo: la app opera sin internet por diseño y sincroniza después.

- La indisponibilidad de servicios de terceros (Supabase, Cloudinary, Telegram Bot API, Hostinger o los proveedores de mapas). Si supera 4 horas, se activa el plan de contingencia y se informa a la jefatura de fundo.

- El mantenimiento programado y avisado según 14.7.2.

- El daño, la pérdida o la descarga de los equipos de campo, o el uso de una versión de la app distinta de la publicada.

- Los retrasos de revisión imputables al cliente, que se miden como KPI y no como incumplimiento del proveedor.

- La entrega de avisos por Telegram mientras Telegram Bot API no esté disponible (SC-02). En ese periodo SLA-07 se mide solo en la generación del aviso.

- La exactitud del modelo de IA, que no es un nivel de servicio sino un indicador de desempeño (KPI-17 a KPI-19), porque la decisión final es humana.

- Las plagas ocultas o no visibles en las fotografías (14.1.3).

### 14.7.9 Seguimiento, reporte y escalamiento:

La responsable de calidad (QA Lead) consolida cada quincena el informe de niveles de servicio con los datos de la base, el registro de peticiones, el monitor de disponibilidad y el registro de incidentes y solicitudes. El informe se revisa con la jefatura de fundo y el especialista fitosanitario, y un resumen mensual se envía a la Gerencia General (STK-01). Si un indicador incumple su meta dos quincenas seguidas, se analiza la causa raíz y la acción correctiva entra al Product Backlog con prioridad alta.

| Nivel | Responsable | Interviene cuando |
|---|---|---|
| Nivel 1– Recepción ysolicitudes | Administrador de la plataforma: Oscar Antonio Gutiérrez Rojas. Registro y seguimiento: Grace Lucia Peña Reyes (QA Lead). | Llega cualquier reporte. Se clasifica como incidente, solicitud o cambio y se atienden las solicitudes SOL-01 a SOL-06. |
| Nivel 2 – Soporte técnico especializado | Backend, web e IA: Oscar Manuel Guerrero Figari. Appmóvil, base de datos e infraestructura: Oscar Antonio Gutiérrez Rojas. Prototipo y equipos: María Fernanda Zúñiga Matos. | El incidente requiere diagnóstico o corrección técnica, o es de prioridad P1 o P2. |
| Nivel 3 – Decisión | Project Manager, jefe de fundo y Product Owner (Pedro Jesús Palacios Chanco) y especialista fitosanitario (Ing. Max Daniel Gutiérrez Huamán). | Se excede el tiempo de restablecimiento, el incidente afecta las mediciones del postest o la solución exige cambiar reglas de negocio, umbrales de IA o el alcance. |

### 14.7.10 Registro y medición en Jira Service Management:

Los incidentes y las solicitudes de 14.7.5 y 14.7.6 se registran en el espacio de asistencia «Soporte» (clave SOP) de Jira Service Management, en su plan gratuito. Los incidentes usan el tipo de solicitud «INC - Reportar un incidente del sistema» y las solicitudes, el tipo de su código (SOL-01 a SOL-08). Las prioridades P1, P2, P3 y P4 corresponden a Highest, High, Medium y Low de Jira, y todos los plazos se cuentan con el calendario «Horario piloto Riachuelo» (lunes a sábado, de 06:00 a 14:00, hora de Lima). Se configuraron tres SLA:

| SLA en Jira | Qué mide | Inicio, pausa y fin | Metas configuradas |
|---|---|---|---|
| Tiempo de primera respuesta | Respuesta a incidentes (14.7.5) | Inicia al crear el incidente. Se detiene con el primer comentario al cliente, al pasar a «En curso» o «Esperando por el cliente», o al resolverse. | P1: 30 min; P2: 1 h; P3: 4 h; P4: 8 h (1 día de servicio). |
| Tiempo de restablecimiento de incidentes | Restablecimiento de incidentes (14.7.5) | Inicia al crear el incidente. Se pausa en «Esperando por el cliente» y se detiene al resolverse. | P1: 4 h; P2: 8 h; P3: 24 h (3 días de servicio); P4: 96 h (un sprint de dos semanas). |
| Plazo de atención de solicitudes | Solicitudes de servicio (14.7.6) | Inicia al crear la solicitud. Se pausa en «Esperando por el cliente» y se detiene al resolverse. | 2 h: bloqueo de cuenta (SOL-01) y revocación de celular perdido (SOL-02); 8 h: SOL-01, SOL-02, SOL-04 y SOL-06; 16 h: SOL-03, SOL-05 y SOL-07; SOL-08 sin plazo. |

Jira no admite un plazo como «se planifica en el siguiente sprint», por lo que la P4 se configuró con 96 horas de servicio (12 días de servicio de 8 horas). Los indicadores SLA-01 a SLA-09 no se cronometran en Jira, porque se miden con el monitor de disponibilidad, el registro de peticiones y las tablas de la base de datos (14.7.3); cuando uno se incumple, se registra un incidente P1 o P2 y corren los tiempos de esta tabla. Los reportes de cumplimiento de estos SLA son la fuente de KPI-14 y KPI-15 y se incluyen en el informe quincenal (14.7.9).

## 14.8. KPI

Los indicadores clave de desempeño (KPI) muestran si el servicio cumple el propósito definido en 14.1.1 y si contribuye a los objetivos empresariales (4.3). Se organizan en tres grupos: los KPI del proceso de monitoreo fitosanitario, que son los indicadores de la variable dependiente (Tabla 6) y se miden por segmento en el pretest (O₁) y el postest (O₂); los KPI de operación del servicio, que se calculan con los registros de la plataforma; y los KPI de desempeño de la IA frente a la decisión del especialista. Los niveles de servicio SLA-01 a SLA-09 se reportan cada quincena y se resumen en KPI-13.

### 14.8.1 KPI del proceso de monitoreo fitosanitario:

Estos KPI son los nueve indicadores definidos en el Capítulo III, con la misma fórmula, unidad e instrumento, de modo que el resultado del servicio y el de la investigación sean el mismo dato. Su meta es una mejora estadísticamente significativa respecto del pretest (α = 0,05, con la prueba t para muestras relacionadas o la de Wilcoxon, según 3.18) y, en los indicadores que lo permiten, un umbral absoluto. En estos KPI, «segmento» es la

unidad de análisis de 25 plantas consecutivas definida en 3.10; no debe confundirse con los segmentos del catálogo de la plataforma (10 por hilera, de unos 20 m), que sirven para ubicar cada foto.

| Código | KPI | Fórmula | Meta en el postest |
|---|---|---|---|
| KPI-01 | Tiempo total de monitoreo | Hora de fin − hora de inicio del recorrido del segmento (min) | Disminución significativa frente a O₁ |
| KPI-02 | Personas-hora empleadas | N.° de personas participantes × horas empleadas | Disminución significativa frente a O₁ |
| KPI-03 | Segmentos inspeccionados por hora | Segmentos inspeccionados ÷ tiempo total empleado (h) | Aumento significativo frente a O₁ |
| KPI-04 | Cobertura alcanzada | Segmentos monitoreados ÷ segmentos programados × 100 | 100 % de los12segmentos, conambos laterales cerrados |
| KPI-05 | Evidencia utilizable | Evidencias aptas para análisis ÷ evidencias obtenidas × 100 | ≥ 90 % y aumento frente a O₁ |
| KPI-06 | Registros correctamente documentados | Registros completos, sin inconsistencias ni duplicados ÷ total de registros × 100 | 100 % |
| KPI-07 | Hallazgos correctamente localizables | Hallazgos ubicados de nuevo ÷ hallazgos sometidos a la prueba de localización × 100 | ≥ 90 % y aumento frente a O₁ |
| KPI-08 | Tiempo para localizar un hallazgo | Hora de confirmación del sector − hora de inicio de la búsqueda (min) | Disminución significativa frente a O₁ |
| KPI-09 | Casos con seguimiento completo | Casos con registro desde la detección hasta su atención, verificación o cierre ÷ casos aplicables × 100 | 100 % |

| Código | Fuente del dato en el postest | Responsable |
|---|---|---|
| KPI-01 a<br>KPI-03 | Ficha de registro de tiempos y ficha de observación. Como fuente complementaria, el inicio y el cierre de las pasadas que registra la app. | Analista funcional, con el supervisor de zona |
| KPI-04 | Ficha de observación y estado de cobertura de las hileras en el plano de la plataforma. | Analista funcional |
| KPI-05 | Resultado de calidad de cada foto (utilizable, repetir por exposición, repetir por nitidez) y ficha de evaluación de evidencias. | QA Lead |
| KPI-06 | Auditoría de capturas: campos obligatorios (RN-03) e identificadores únicos que evitan duplicados al sincronizar. | QA Lead |
| KPI-07 y<br>KPI-08 | Ficha de prueba de localización y cronómetro. El operador o el especialista llega al lugar con la función «Ubicar plaga» de la app. | Equipo de campo y especialista fitosanitario |
| KPI-09 | Ficha de seguimiento de casos e historial del caso en la plataforma (captura, análisis, decisión y aviso). | Especialista fitosanitario |

Cuando un segmento no tenga hallazgos, KPI-07, KPI-08 y KPI-09 se registran como no aplicables, sin generar valores artificiales (3.17).

### 14.8.2 KPI de operación del servicio:

<table style="width:100%;">
<colgroup>
<col style="width: 10%" />
<col style="width: 19%" />
<col style="width: 29%" />
<col style="width: 15%" />
<col style="width: 24%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Código</strong></th>
<th><p><strong>KPI</strong></p>
</th>
<th><p><strong>Fórmula</strong></p>
</th>
<th><p><strong>Meta</strong></p>
</th>
<th><p><strong>Fuente</strong></p>
</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>KPI-10</td>
<td><p>Tiempo de ciclo del hallazgo</p>
</td>
<td><p>Mediana de (hora de decisión del</p>
<p>especialista − hora de captura)</p>
</td>
<td><p>≤ 24 h</p>
</td>
<td><p>review_cases</p>
<p>(captured_at, decided_at)</p>
</td>
</tr>
<tr class="even">
<td>KPI-11</td>
<td><p>Tiempo hasta el primer aviso</p>
</td>
<td><p>Mediana de (hora del primer aviso − hora de captura), por vía de alerta</p>
</td>
<td><p>≤ 24 h</p>
</td>
<td><p>notifications y review_cases</p>
</td>
</tr>
<tr class="odd">
<td>KPI-12</td>
<td><p>Casos revisados dentrodelplazo</p>
</td>
<td><p>Casos decididos en ≤ 24 h de servicio desde su apertura ÷casos abiertos × 100</p>
</td>
<td><p>≥ 90 %</p>
</td>
<td><p>review_cases</p>
<p>(opened_at, decided_at)</p>
</td>
</tr>
<tr class="even">
<td>KPI-13</td>
<td><p>Cumplimiento</p>
<p>del SLA</p>
</td>
<td><p>Indicadores SLA-01 a SLA-09 que cumplensu meta ÷ indicadores medidos × 100</p>
</td>
<td><p>≥ 90 % por</p>
<p>quincena</p></td>
<td><p>Informe de nivelesdeservicio</p>
</td>
</tr>
<tr class="odd">
<td>KPI-14</td>
<td><p>Incidentes</p>
<p>restablecidos en plazo</p>
</td>
<td><p>Incidentes restablecidos dentro del tiempo de su prioridad ÷ incidentes cerrados × 100</p>
</td>
<td><p>≥ 90 %</p>
</td>
<td><p>Registro de incidentes (Jira)</p>
</td>
</tr>
<tr class="even">
<td>KPI-15</td>
<td><p>Solicitudes</p>
<p>atendidas en plazo</p>
</td>
<td><p>Solicitudes atendidas dentro del plazo de 14.7.6÷ solicitudes cerradas ×100</p>
</td>
<td><p>≥ 90 %</p>
</td>
<td><p>Registro de solicitudes (Jira)</p>
</td>
</tr>
<tr class="odd">
<td>KPI-16</td>
<td><p>Uso del sistema en las</p>
<p>jornadas</p>
</td>
<td><p>Jornadas de los segmentos piloto registradas como sesiones en la app ÷ jornadas programadas</p>
<p>× 100</p>
</td>
<td><p>100 % en elpostest</p>
</td>
<td><p>monitoring_sessions y plan de jornadas</p>
</td>
</tr>
</tbody>
</table>

### 14.8.3 KPI de desempeño de la IA frente al especialista:

La IA solo sugiere indicios; su calidad se mide comparando lo que sugirió con lo que decidió el especialista. Estos KPI usan las metas del Plan de Gestión de la Calidad y orientan cuándo reentrenar el modelo o ajustar los umbrales de triage.

<table style="width:100%;">
<colgroup>
<col style="width: 10%" />
<col style="width: 18%" />
<col style="width: 28%" />
<col style="width: 15%" />
<col style="width: 26%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Código</strong></th>
<th><p><strong>KPI</strong></p>
</th>
<th><p><strong>Fórmula</strong></p>
</th>
<th><p><strong>Meta</strong></p>
</th>
<th><p><strong>Uso del resultado</strong></p>
</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>KPI-17</td>
<td><p>Precisión de las sugerencias de la IA</p>
</td>
<td><p>Casos de origenIA confirmados por el especialista ÷ casos de origen IA condecisión final (confirmados + descartados) × 100</p>
</td>
<td><p>≥ 85 %</p>
</td>
<td><p>Si no se cumple, se reentrena el modelo usando los casos revisadoscomoejemplos.</p>
</td>
</tr>
<tr class="even">
<td>KPI-18</td>
<td><p>Tasa de falsos negativos</p>
</td>
<td><p>Fotos con plaga no detectadas (halladas en la muestra de descartadas, proyectadas al total) ÷ (fotos con plaga detectadas + no detectadas) × 100</p>
</td>
<td><p>≤ 5 %</p>
</td>
<td><p>El especialista revisa cada semanauna muestra aleatoria del</p>
<p>10 % de las fotos descartadas porlaIA.</p>
</td>
</tr>
<tr class="odd">
<td>KPI-19</td>
<td><p>Concordancia de la alerta temprana</p>
</td>
<td><p>Casos «Confirmadopor IA» ratificados por el especialista ÷ casos</p>
<p>«Confirmado por IA» revisados × 100</p>
</td>
<td><p>≥ 90 %</p>
</td>
<td><p>Si baja de 90 % en una quincena, se suben los umbrales de confirmación por IA o se apaga esa vía, con aprobación del Product Owner.</p>
</td>
</tr>
<tr class="even">
<td>KPI-20</td>
<td><p>Carga de</p>
<p>revisión del especialista</p>
</td>
<td><p>Fotos que pasan a</p>
<p>«Pendiente de revisión» ÷ fotos analizadas × 100</p>
</td>
<td><p>Seguimiento</p>
</td>
<td><p>Dimensiona el tiempo que el especialista dedica a la bandeja; debe bajar con cada versión del modelo.</p>
</td>
</tr>
</tbody>
</table>

Además, un modelo nuevo solo se activa si detecta al menos el 85 % de las fotos de prueba con plaga y no genera falsas alarmas frecuentes en fotos sanas. El modelo anterior se conserva para volver atrás.

### 14.8.4 Línea base y primera medición:

La línea base de los KPI-01 a KPI-09 es la medición O₁ del pretest, registrada por segmento con los instrumentos del Capítulo III. Para los indicadores del servicio, la primera medición se tomó de la base de datos del piloto (corte al 29/10/2026). Corresponde a las pruebas de campo e integración del 21/10/2026: 5 sesiones, 7 pasadas, 86 secuencias y 196 fotos tomadas con la app 0.6.0 en los lotes LOTE01, LOTE02 y

LOTE08, analizadas con el modelo 2026.10-v2. No reemplaza al postest, que se mide sobre los 12 segmentos de SWG 1, SWG 2 y SWG 5.

| Indicador | Valor medido | Observación |
|---|---|---|
| SLA-03 Validación local de calidad | Promedio 2,09 s; P95 2,90 s; 49 % de las fotos en ≤ 2 s | No cumple RNF02. Depende del celular: la Cámara 2 tarda una mediana de 2,80 s y la Cámara 1, 1,78 s. |
| SLA-04 y SLA-05<br>Conservación y sincronización | 196 de 196 fotos confirmadas;0 duplicados; mediana de 14 min y P95 de 29 min entre captura y confirmación | Cumple. Todas las fotos se sincronizaron el mismo día de la captura. |
| KPI-05 Evidencia utilizable | 83,7 % al primer intento (164 de 196); 95,9 % después de repetir la foto (164 de 171 posiciones) | Cumple la meta de 90 % gracias a la repetición del lado afectado (RN-04). |
| KPI-06 Registros correctamente documentados | 100 % de las fotos con lote, hilera, lateral, segmento, marcador y secuencia; 0 duplicados | Cumple. El GPS está presente en 84 de 86 secuencias (97,7 %), con precisión mediana de 5,3 m. |
| SLA-06 Análisis por IA | 164 fotos analizadas; P95 de unos 5 s entre la confirmación y el resultado; inferencia promedio de 1,35 s; 0 errores | Cumple con amplio margen. |
| KPI-20 Carga de revisión y triage | Sin indicios: 40,2 %. Descartadas por la IA: 23,8 %. Confirmadas por IA: 17 casos (10,4 %).<br>Pendientes de revisión: 29 casos (17,7 %) | El especialista recibe solo el 17,7 % de las fotos analizadas. |
| Ubicación de los casos (apoyoaKPI-07) | 45 de 46 casos con coordenadas GPS (97,8 %) | El caso restante solo tiene lote, hilera, lateral, segmento y marcador. |
| KPI-12 Revisión del especialista | 0 de 46 casos con decisión del especialista; los 29 pendientes superan las 190 horas abiertos | No cumple el compromiso de 24 horas. KPI-10, KPI-17 y KPI- 19 todavía no se puedencalcular. |
| SLA-07 Emisión de alertas | 0 avisos generados; la auditoría registra 23 eventos de aviso sin destinatarios | Los destinatarios están asignados a SWG 1, SWG 2 y SWG 5, pero la prueba se hizo en LOTE01, LOTE02 y LOTE08. |
| Incidencias registradas por la app | 44 en total: 31 de calidad de imagen, 6 desconexiones entre celulares y 7 de otro tipo | Insumo para la clasificación de incidentes (14.7.5) y para KPI-14. |

Antes del postest se deben corregir tres puntos que esta primera medición dejó a la vista: asignar los destinatarios de alertas a los lotes donde se monitorea, que el especialista

revise y decida los casos pendientes, y reducir el tiempo de validación local en el celular de la Cámara 2.

### 14.8.5 Frecuencia, semáforo y relación con los objetivos:

Los KPI de operación y de IA se calculan cada quincena con consultas de solo lectura a la base de datos y se presentan en el informe de niveles de servicio. Los KPI del proceso se calculan al cerrar el pretest y el postest. Cada KPI se muestra con un semáforo:

- **Verde:** cumple la meta.

- **Ámbar:** no cumple, pero está a menos de 10 % de la meta; se vigila en la siguiente quincena.

- **Rojo:** se aleja más de 10 % de la meta; requiere análisis de causa raíz y una acción correctiva.

La siguiente tabla relaciona cada objetivo empresarial con los KPI que permiten verificar su avance:

| Objetivo empresarial (4.3) | KPI que lo miden |
|---|---|
| Cumplir con los estándares fitosanitarios que exigen los mercados de exportación | KPI-06, KPI-09 y KPI-13 (incluye SLA- 08, trazabilidad) |
| Optimizar el uso de mano de obra en las labores de monitoreo de campo | KPI-01, KPI-02, KPI-03 y KPI-20 |
| Reducir el riesgo de impacto de plagas mediante la detección temprana de indicios de Planococcus ficus | KPI-04, KPI-05, KPI-17 y KPI-18 |
| Mejorar el tiempo de respuesta frente a hallazgos fitosanitarios | KPI-08, KPI-10, KPI-11, KPI-12 y KPI-19 |
| Digitalizar la información generada en el campo y hacerla trazable | KPI-06, KPI-07, KPI-09 y KPI-16 |
