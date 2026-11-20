# Anexo 22 (AP-23): Plan de Gestión de Calidad

## 

## PLAN DE GESTIÓN DE LA CALIDAD

| Nombre del Proyecto | Siglas del Proyecto |
|---|---|
| Sistema inteligente para el monitoreo y detección de plagas en cultivos de vid en Agrícola Riachuelo | SIMDPCVAR |

<table>
<tr>
<th colspan="3">Estándar o Norma de Calidad aplicable</th>
</tr>
<tr>
<td colspan="2">Hardware y Prototipo Físico de Captura Bilateral</td>
<td>ISO/IEC 25010 (Usabilidad y Ergonomía) y normas de seguridad en ergonomía<br>OCUPACIONAL PARA REDUCIR LA FATIGA DEL<br>OPERADOR.</td>
</tr>
<tr>
<td colspan="2">Desarrollo De Software Y Modelos De Inteligencia<br>Artificial</td>
<td>ISO/IEC 25010 (Calidad del Software), ISO/IEC 29110 (Ingeniería de Software para pequeñas entidades) y estándares IEEE 829 para pruebas de software.</td>
</tr>
<tr>
<td colspan="2">Modelo De Inteligencia Artificial</td>
<td>ISO/IEC 22989 (Conceptos y terminología de Inteligencia<br>Artificial) y métricas estándar de Visión Artificial (mAP, Precision, Recall, Matriz de Confusión).</td>
</tr>
<tr>
<td colspan="2">Infraestructura Y Despliegue</td>
<td>ISO/IEC 27001 (Seguridad de la Información), OWASP Top 10 API Security y HTTPS/TLS para transmisión cifrada de datos en la nube</td>
</tr>
<tr>
<td colspan="3">Objetivos de Calidad</td>
</tr>
<tr>
<td colspan="3">Garantizar la alta precisión del modelo de IA: Lograr una precisión ≥ 85% y una tasa de falsos negativos ≤ 5% en la sugerencia no vinculante de indicios visuales de Planococcus ficus.</td>
</tr>
<tr>
<td colspan="3">Asegurar la velocidad de validación en campo: Mantener un tiempo de validación local de calidad de imagen (nitidez, iluminación, movimiento) ≤ 2 segundos por toma en el dispositivo móvil.</td>
</tr>
<tr>
<td colspan="3">Asegurar la integridad y trazabilidad total de la información: Garantizar un 100% de sincronización exitosa de las capturas locales (SQLite/Archivos) hacia la nube sin pérdida de metadatos (lote, hilera, zona, lado, marcador)</td>
</tr>
<tr>
<td colspan="3">Cumplir con el umbral de rendimiento de la API y disponibilidad: Mantener un tiempo de respuesta de la API REST backend ≤ 1.5 segundos y una disponibilidad de la plataforma web del 99.5% durante las pruebas piloto.</td>
</tr>
<tr>
<td colspan="3">ROLES PARA LA GESTIÓN DE LA CALIDAD: ESPECIFICAR LOS ROLES QUE SERÁN NECESARIOS EN EL EQUIPO DE PROYECTO PARA DESARROLLAR LOS ENTREGABLES Y ACTIVIDADES DE GESTIÓN DE LA CALIDAD.</td>
</tr>
<tr>
<td rowspan="8">Rol No 1 : Responsable de Control de<br>Calidad de<br>Software y Plataforma (QA Lead)</td>
<td colspan="2">Objetivos del rol: Garantizar que la aplicación móvil offline, la plataforma web Django y la API backend Django REST cumplan con los criterios funcionales, de rendimiento y seguridad.</td>
</tr>
<tr>
<td colspan="2">Funciones del rol:<br>• Diseñar y ejecutar casos de prueba unitarios, de integración y end-to-end<br>• Auditar el cumplimiento de los criterios de aceptación en cada historia de usuario (DoD).<br>• Validar la integridad de los datos sincronizados entre SQLite y<br>Supabase (PostgreSQL).</td>
</tr>
<tr>
<td colspan="2">Niveles de autoridad: Rechazar incrementos de software que no cumplan la Definition of Done (DoD) o presenten errores críticos de sincronización.</td>
</tr>
<tr>
<td colspan="2">Reporta a: Project Manager (Oscar Antonio Gutiérrez Rojas).</td>
</tr>
<tr>
<td colspan="2">Supervisa a: Desarrollador Backend y Plataforma.</td>
</tr>
<tr>
<td colspan="2">Requisitos de conocimientos: Pruebas de software, API REST, bases de datos SQL/PostgreSQL, React Native, Django Templates (HTMX/JS) y metodología Scrum.</td>
</tr>
<tr>
<td colspan="2">Requisitos de habilidades: Atención al detalle, pensamiento analítico, comunicación asertiva y resolución de problemas técnicos.</td>
</tr>
<tr>
<td colspan="2">Requisitos de experiencia: Experiencia en desarrollo y QA de aplicaciones móviles o web en entornos académicos o profesionales.</td>
</tr>
</table>

<table>
<tr>
<th rowspan="8">Rol No 2 : Responsable de Calidad de Prototipo Físico y<br>Hardware</th>
<th>Objetivos del rol: Asegurar la viabilidad ergonómica, estabilidad mecánica y durabilidad del prototipo vestible de captura bilateral durante el recorrido en campo.</th>
</tr>
<tr>
<th>Funciones del rol:<br>• Inspeccionar y probar la resistencia de arneses, barras tipo T, soportes universales y correas.<br>• Calibrar la altura, ángulo de enfoque y alineación de las cámaras móviles antes de cada sesión.<br>• Evaluar la comodidad y reducción de fatiga en los operadores de monitoreo.</th>
</tr>
<tr>
<th>Niveles de autoridad : Detener o solicitar ajustes estructurales al prototipo si presenta desalineación o incomodidad excesiva para el usuario.</th>
</tr>
<tr>
<th>Reporta a: Project Manager (Oscar Antonio Gutiérrez Rojas).</th>
</tr>
<tr>
<th>Supervisa a: Operadores de monitoreo durante las pruebas físicas.</th>
</tr>
<tr>
<th>Requisitos de conocimientos: Ensamble de hardware, ergonomía de dispositivos, calibración de lentes fotográficos y pruebas de estrés mecánico.</th>
</tr>
<tr>
<th>Requisitos de habilidades: Habilidad manual, trabajo en campo, observación técnica y adaptabilidad.</th>
</tr>
<tr>
<th>Requisitos de experiencia: Experiencia en diseño de prototipos, integración de hardware o soporte técnico.</th>
</tr>
<tr>
<td rowspan="8">Rol No 3 : Validador Fitosanitario y de Calidad del<br>Modelo de IA (<br>Consultor Experto)</td>
<td>Objetivos del rol: Garantizar que el modelo de IA y el flujo de validación humana cumplan con las exigencias agronómicas de la empresa.</td>
</tr>
<tr>
<td>Funciones del rol :<br>• Validar la calidad del etiquetado (bounding boxes) del dataset de Planococcus ficus.<br>• Revisar y evaluar la tasa de precisión, falsos positivos y falsos negativos del modelo de visión artificial.<br>• Aprobar los criterios de aceptación funcionales del panel de revisión y envío de alertas por Telegram (SC-02).</td>
</tr>
<tr>
<td>Niveles de autoridad: Autoridad máxima para confirmar o descartar hallazgos fitosanitarios y aprobar la salida a producción del piloto.</td>
</tr>
<tr>
<td>Reporta a: Sponsor / Gerencia General de Agrícola Riachuelo S.A.C.</td>
</tr>
<tr>
<td>Supervisa a: Proceso de validación de casos fitosanitarios</td>
</tr>
<tr>
<td>Requisitos de conocimientos: Sanidad vegetal, manejo integrado de plagas en vid (Planococcus ficus), criterios de auditoría fitosanitaria.</td>
</tr>
<tr>
<td>Requisitos de habilidades: Criterio técnico agronómico, toma de decisiones críticas y visión de negocio agrícola.</td>
</tr>
<tr>
<td>Requisitos de experiencia: Experiencia demostrada como Especialista<br>Fitosanitario o Agrónomo de fundo.</td>
</tr>
</table>

<table>
<tr>
<th colspan="2">Revisiones de Calidad</th>
</tr>
<tr>
<td>Entregables/ Procesos</td>
<td>Revisiones de Calidad</td>
</tr>
<tr>
<td>Prototipo Físico Vestible</td>
<td>Inspección mecánica de ajuste, alineación y prueba ergonómica en recorrido de campo.</td>
</tr>
<tr>
<td>Aplicación Móvil React Native</td>
<td>Pruebas de usabilidad en ≤ 3 pasos, validación local de nitidez en ≤ 2 s y almacenamiento seguro offline.</td>
</tr>
<tr>
<td>Modelo de Inteligencia Artificial (YOLO)</td>
<td>Evaluación matemática de matriz de confusión (Precisión ≥ 85% , falsos negativos ≤ 5% ) sobre el conjunto de test.</td>
</tr>
</table>

<table>
<tr>
<th colspan="2">Plataforma Web Django &amp; API REST</th>
<th>Pruebas de estrés y rendimiento de API REST (≤ 1.5 s), disponibilidad (≥ 99.5%) y prueba de carga con +50,000 plantas.</th>
</tr>
<tr>
<td colspan="2">Flujo Completo de Alertas (Telegram)</td>
<td>Pruebas formales de integración End-to-End para verificar que las alertas por Telegram se emitan únicamente tras la confirmación del especialista</td>
</tr>
<tr>
<td colspan="3">Actividades de Control y Gestión de la Calidad</td>
</tr>
<tr>
<td rowspan="4">Actividades de Control de la Calidad</td>
<td colspan="2">Revision de Código por Pares (Peer Reviews): Inspección obligatoria de todo commit/pull request en Git antes de fusionar a ramas principales.</td>
</tr>
<tr>
<td colspan="2">Ejecución de Pruebas Unitarias e Integradas: Pruebas automatizadas en backend Django (Python), validación de endpoints REST y del esquema relacional en Supabase (PostgreSQL).</td>
</tr>
<tr>
<td colspan="2">Medición de Calidad de Imágenes en Móvil: Calibración y ajuste de los algoritmos locales de análisis de exposición, nitidez y desenfoque.</td>
</tr>
<tr>
<td colspan="2">Verificación Fitosanitaria de Dataset: Evaluación quincenal de las muestras fotográficas de plagas etiquetadas junto al agrónomo.</td>
</tr>
<tr>
<td rowspan="4">Actividades de Gestión de la Calidad</td>
<td colspan="2">Auditorías Internas de Procesos: Evaluación quincenal del cumplimiento de la Definition of Ready (DoR) y Definition of Done (DoD) en cada Sprint.</td>
</tr>
<tr>
<td colspan="2">Sesiones de Sprint Review: Presentación de incrementos funcionales ante la Jefatura de Fundo y Especialista Fitosanitario para obtener retroalimentación.</td>
</tr>
<tr>
<td colspan="2">Análisis de Mejora Continua en Retrospectivas: Evaluación de cuellos de botella técnicos o fallas de calibración al cierre de cada<br>Sprint para aplicar acciones correctivas.</td>
</tr>
<tr>
<td colspan="2">Capacitación a Operadores: Entrenamiento práctico previo al inicio del piloto sobre el uso correcto del equipo vestible y manejo de alertas en la app.</td>
</tr>
<tr>
<td colspan="3">Herramientas de Calidad</td>
</tr>
<tr>
<td colspan="3">Gestión de Código y Pull Requests: Repositorios Git centralizados en GitHub con reglas de protección de ramas.</td>
</tr>
<tr>
<td colspan="3">Monitoreo y Métricas de IA: Matriz de Confusión, curvas Precision-Recall y TensorBoard / MLflow para el seguimiento del entrenamiento del modelo YOLO.</td>
</tr>
<tr>
<td colspan="3">Pruebas de Software y API: Postman / Bruno para pruebas de endpoints REST, y Jest para pruebas unitarias en React Native, y pruebas de Django (manage.py test) para la plataforma web.</td>
</tr>
<tr>
<td colspan="3">Gestión Ágil e Impedimentos: Tablero Scrum en Jira y Bitácora de Registro de Impedimentos a cargo del Scrum Master.</td>
</tr>
<tr>
<td colspan="3">Análisis Físico y Fotográfico: Calibradores mecánicos, luxómetros y patrones de prueba de nitidez visual (test chart) para dispositivos móviles.</td>
</tr>
<tr>
<td colspan="3">Procedimientos Relevantes de la Calidad</td>
</tr>
<tr>
<td colspan="3">Procedimiento de Solicitud y Control de Cambios: Cualquier modificación en los requisitos, diseño del prototipo o alcance debe registrarse formalmente, evaluar su impacto en tiempo/costo y ser aprobada por la jefatura.</td>
</tr>
<tr>
<td colspan="3">Procedimiento de Manejo de No Conformidades (Repetición de Captura): Si una foto tomada en campo no cumple los umbrales de nitidez, el sistema alerta visualmente y pide repetir solo la cámara que falló, con un máximo de 2 intentos; en el último la foto se acepta y se marca para revisión técnica (SC-05).</td>
</tr>
<tr>
<td colspan="3">Procedimiento de Sincronización Segura y Reintento (Offline-First): Las imágenes y metadatos se custodian localmente en SQLite y almacenamiento privado de la app (expo-file-system; los tokens, en expo-secure-store, SC-06). La sincronización se ejecuta automáticamente al detectar conexión y no elimina los archivos locales hasta recibir la confirmación de recepción HTTP 200/201 del servidor.</td>
</tr>
<tr>
<td colspan="3">Procedimiento de Confirmación Fitosanitaria y Disparo de Alerta: Bloqueo por regla de negocio (RN-01, RN-02 y RN-06) que prohíbe pasar un caso a &quot;confirmado&quot; o enviar notificaciones por Telegram (SC-02) sin la acción síncrona y autenticada del Especialista Fitosanitario.</td>
</tr>
</table>
