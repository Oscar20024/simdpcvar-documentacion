# Anexo 51 (AS-01): Product Backlog

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
<td>Equipo<br>Scrum (Grace Peña, Oscar Gutiér rez,</td>
<td>Scrum Master</td>
<td>Product Owner / Agrícola Riachuelo<br>S.A.C.</td>
<td>06/10/2 026</td>
<td>Actualización por reformulación de arquitectura a Django<br>+ Supabasey asignación de Sprints.</td>
</tr>
</table>

|  | Oscar Manue l, María Fernanda Zúñiga) |  |  |  |  |
|---|---|---|---|---|---|

BACKLOG PRIORIZADO DEL PRODUCTO

**Código y Nombre del Proyecto**

**SIMDPCVAR – Sistema Inteligente para el Monitoreo y<br>Detección de Plagas en Cultivos de Vid en Agrícola Riachuelo S.A.C.**

| ÉPICAS HISTORIAS DE USUARIO, SOLICITUDES DE CAMBIO<br>O RIESGOS | PRIORIDAD | ESTIMACIÓN DE TAMAÑO (PUNTOS DE HISTORIA) | COMPROMETIDA PARA SPRINT NÚMERO | OBSERVACIONES |
|---|---|---|---|---|
| Épica 1: Prototipo físico de captura bilateral | Alta | 8 | Sprint 1 | Diseño y calibración del soporte fijado al brazo del operador. |
| HU01: Soporte físico tipo T para doble captura | Alta | 5 | Sprint 1 | Permite obtener doble evidencia del mismo punto de la hilera en cada<br>PASADA SIN MANOS. |
| Riesgo 2: Resistencia al cambio de operadores | Media | 3 | Sprint 1 | Mitigado con diseño ergonómico y capacitaciones<br>PREVIAS. |
| Épica 2: Módulo móvil de captura offline y calidad | Alta | 13 | Sprint 2 | App móvil multiplataforma con validación local y<br>ALMACENAMIENTO SIN CONEXIÓN. |
| HU02: Validación de calidad (nitidez/iluminación) | Alta | 8 | Sprint 2 | Verifica fotos antes de cambiar de zona para evitar capturas<br>INSERVIBLES. |
| Riesgo 1: Limitada o nula conectividad en fundo | Alta | 5 | Sprint 2 | Mitigado con arquitectura<br>OFFLINE-FIRST (SQLite/FileSystem). |
| Épica 3: Inteligencia artificial y georreferenciación | Alta | 21 | Sprint 3 | Modelo YOLO para indicios de plaga vinculados a metadatos de<br>UBICACIÓN. |
| HU03: Visualización de hallazgos en mapa e IA | Alta | 13 | Sprint 3 | Mapa interactivo con evidencias para confirmación o descarte del<br>AGRÓNOMO. |
| Riesgo 3: Falsos positivos o baja precisión de IA | Alta | 8 | Sprint 3 | Mitigado con validación humana obligatoria antes de<br>NOTIFICAR. |
| SC-01: Solicitud de Cambio - Arquitectura Django/Supabase | Alta | 13 | Sprint 4 | Sustitución de Spring Boot/Angular por Django Templates +<br>Supabase. |
| Épica 4: Plataforma web, mapas y revisión de casos | Alta | 13 | Sprint 4 | Panel web centralizado para revisión cartográfica y<br>VALIDACIÓN HUMANA. |
| HU04: Notificaciones por mensajería oficial | Alta | 8 | Sprint 5 | Envío por Telegram Bot API (SC-02) tras confirmación del<br>ESPECIALISTA. |
| Mantenimiento y Pruebas Integrales E2E | Media | 8 | Sprint 5 | Sincronización automática cloud y pruebas integrales<br>DEL SISTEMA. |
| Pruebas Piloto y Despliegue Final en Fundo | Alta | 13 | Sprint 6 | Entrega de prototipo, pruebas piloto y medición<br>COMPARATIVA DE INDICADORES. |
