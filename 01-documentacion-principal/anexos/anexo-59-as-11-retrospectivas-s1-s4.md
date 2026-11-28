# Anexo 59 (AS-11): Retrospectivas (S1-S4)

<table>
<tr>
<th colspan="6">CONTROL DE VERSIONES</th>
</tr>
<tr>
<td>Versión</td>
<td>Hecho por</td>
<td>Revisado por</td>
<td>Aprobado por</td>
<td>Fecha</td>
<td>Motivo</td>
</tr>
<tr>
<td>2.0</td>
<td>Equipo<br>Scru m</td>
<td>Scrum Master</td>
<td>Product<br>Owner</td>
<td>23/10/2 026</td>
<td>Registro consolidado de lecciones aprendidas y acuerdos de mejora continua para los Sprints 1 a 4.</td>
</tr>
</table>

LOG DE RETROSPECTIVA DEL SPRINT

| Código y Nombre del Proyecto | Sprints Cubiertos |
|---|---|
| SIMDPCVAR – Sistema Inteligente para el Monitoreo y Detección de Plagas en Cultivos de Vid en Agrícola<br>Riachuelo S.A.C. | 1 a 4 |

**LOG DE RETROSPECTIVA - SPRINT 1**

<table>
<tr>
<th colspan="2">Fecha de Reunión de Retrospectiva: 12/09/2026</th>
</tr>
<tr>
<td>Registro</td>
<td>Detalle</td>
</tr>
<tr>
<td>¿Qué se hizo bien?</td>
<td>Se logró la calibración y montaje inicial del prototipo físico de captura bilateral en T sin interrupciones ergonómicas. Se configuró exitosamente la estructura base del repositorio en GitHub.</td>
</tr>
<tr>
<td>¿Qué salió mal o se dificultó?</td>
<td>Cierta rigidez del arnés al ajustarse al brazo de operadores con distintas contexturas físicas, generando leve molestia tras caminatas prolongadas.</td>
</tr>
<tr>
<td>¿Qué aprendimos?</td>
<td>Es indispensable realizar pruebas ergonómicas con el personal real de campo desde las primeras fases de prototipado.</td>
</tr>
<tr>
<td>Acciones de Mejora (Acuerdos)</td>
<td>Rediseñar las correas del soporte físico con acolchado regulable para mejorar el confort del operador antes del inicio de las pruebas de captura.</td>
</tr>
</table>

**LOG DE RETROSPECTIVA - SPRINT 2**

<table>
<tr>
<th colspan="2">Fecha de Reunión de Retrospectiva: 25/09/2026</th>
</tr>
<tr>
<td>Registro</td>
<td>Detalle</td>
</tr>
<tr>
<td>¿Qué se hizo bien?</td>
<td>Se implementó con éxito el algoritmo de validación de calidad local (nitidez e iluminación) en React Native en menos de 2 segundos por toma. Se validó la persistencia offline con SQLite.</td>
</tr>
<tr>
<td>¿Qué salió mal o se dificultó?</td>
<td>Falsos rechazos por sobreexposición solar en las fotos tomadas al mediodía en las hileras sin sombra.</td>
</tr>
<tr>
<td>¿Qué aprendimos?</td>
<td>Los umbrales de iluminación de la cámara móvil deben calibrarse considerando las variaciones de luz natural extrema en el fundo de Ica.</td>
</tr>
<tr>
<td>Acciones de Mejora (Acuerdos)</td>
<td>Ajustar los rangos de exposición del algoritmo de calidad y agregar una sugerencia en pantalla para enfocar con ligera inclinación o sombra manual.</td>
</tr>
</table>

**LOG DE RETROSPECTIVA - SPRINT 3**

<table>
<tr>
<th colspan="2">Fecha de Reunión de Retrospectiva: 09/10/2026</th>
</tr>
<tr>
<td>Registro</td>
<td>Detalle</td>
</tr>
<tr>
<td>¿Qué se hizo bien?</td>
<td>Se completó el etiquetado del dataset inicial y la integración del modelo YOLO para inferir indicios de Planococcus ficus vinculados a coordenadas GPS y metadatos de hilera.</td>
</tr>
<tr>
<td>¿Qué salió mal o se dificultó?</td>
<td>Alta latencia de respuesta (&gt; 10 segundos) al procesar las fotos debido a la sobrecarga de saltos entre el backend inicial (Spring Boot), la cola RabbitMQ y el microservicio de IA.</td>
</tr>
<tr>
<td>¿Qué aprendimos?</td>
<td>La arquitectura basada en microservicios desacoplados y colas intermedias generaba un cuello de botella crítico para las pruebas en tiempo real.</td>
</tr>
<tr>
<td>Acciones de Mejora (Acuerdos)</td>
<td>Ejecutar en el Sprint 4 la Solicitud de Cambio SC-01 (aprobada el 25/09/2026) para simplificar y unificar la arquitectura en un backend central en Django + Supabase sin colas complejas.</td>
</tr>
</table>

**LOG DE RETROSPECTIVA - SPRINT 4**

<table>
<tr>
<th colspan="2">Fecha de Reunión de Retrospectiva: 23/10/2026</th>
</tr>
<tr>
<td>Registro</td>
<td>Detalle</td>
</tr>
<tr>
<td>¿Qué se hizo bien?</td>
<td>Se ejecutó la migración hacia la nueva arquitectura unificada en Django, logrando tiempos de respuesta de la API REST menores a 1.5 segundos y una integración fluida con Cloudinary.</td>
</tr>
</table>

| ¿Qué salió mal o se dificultó? | Reajuste de tiempo en el equipo para adaptar las plantillas web de Django Templates e integrar Leaflet/OSM en el nuevo stack. |
|---|---|
| ¿Qué aprendimos? | Eliminar intermediarios de software (RabbitMQ/Angular) redujo drásticamente los puntos de fallo y facilitó las pruebas de integración del sistema. |
| Acciones de Mejora (Acuerdos) | Consolidar la suite de pruebas automatizadas E2E en Django antes de pasar a la integración con el módulo de notificaciones (WhatsApp Cloud API, luego reemplazado por Telegram, SC-02) en el Sprint 5. |
