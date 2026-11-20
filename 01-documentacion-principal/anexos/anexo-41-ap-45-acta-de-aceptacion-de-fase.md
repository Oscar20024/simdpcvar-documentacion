# Anexo 41 (AP-45): Acta de Aceptación de Fase

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
<td>Grace<br>Lucia Peña Reyes</td>
<td>Oscar Antonio<br>Gutiérrez Rojas</td>
<td>Jefe de Fundo (Agrícola Riachuelo S.A.C.)</td>
<td>03/10/2026</td>
<td>Emisión del Acta de Aceptación Formal de la Fase 3: Desarrollo Tecnológico y<br>Software / IA.</td>
</tr>
</table>

## ACTA DE ACEPTACIÓN DE FASE

| Nombre del Proyecto | Siglas del Proyecto |
|---|---|
| Sistema inteligente para el monitoreo y detección de plagas en cultivos de vid en Agrícola Riachuelo (SIMDPCVAR) | SIMDPCVAR |

**Nombre del Cliente o patrocinador:**

**Agrícola Riachuelo S.A.C. / Jefe de Fundo (Patrocinador Operativo)**

| DECLARACIÓN DE LA ACEPTACIÓN FORMAL: DEFINIR DETALLADAMENTE LAS CONDICIONES QUE SE CUMPLIERON PARA LA ACEPTACIÓN FORMAL DE LA FASE. |
|---|
| Aprobación de la Aplicación Móvil React Native (Paquete 3.1): Verificación del correcto funcionamiento offline-first con persistencia local en SQLite y el sistema de archivos privado. Cumplimiento del tiempo de validación local de nitidez y exposición en ≤ 1.45 segundos por toma. |
| Aprobación del Servicio de Inteligencia Artificial (Paquete 3.2): Entrega y validación del modelo de visión artificial YOLO exportado a ONNX y ejecutado por el worker de IA de Django, logrando una precisión (mAP) del 87.5% en el conjunto de prueba para indicios visuales de Planococcus ficus. Cumplimiento estricto de la regla de negocio que clasifica el resultado como "indicio sugerido" no vinculante. |
| Aprobación de la Plataforma Web Centralizada y Backend (Paquete 3.3): Validación de la nueva arquitectura unificada en Django, Supabase (PostgreSQL) y Cloudinary (aprobada mediante la Solicitud de Cambio N° 01). Se comprobó un tiempo medio de respuesta de la API REST v1 de 1.18 segundos (superando la meta de ≤ 1.5 s) y la carga del mapa interactivo geoespacial en Leaflet. |
| Verificación de Seguridad y Arquitectura: Cumplimiento del 100% de las políticas de acceso mediante tokens JWT y cumplimiento de la regla estricta de prohibir el acceso |

<table>
<tr>
<th colspan="4">directo de clientes a la base de datos Supabase o almacenamiento Cloudinary sin pasar por la API central en Django.</th>
</tr>
<tr>
<td colspan="4">OBSERVACIONES ADICIONALES: ESPECIFICAR OTROS COMENTARIOS U OBSERVACIONES ADICIONALES.</td>
</tr>
<tr>
<td colspan="4">La aceptación de esta fase autoriza el paso formal hacia la Fase 4 (Sincronización, Alertas y<br>Pruebas Piloto en Campo - Sprint 5 y 6).</td>
</tr>
<tr>
<td colspan="4">El prototipo físico vestible calibrado con amortiguación de neopreno (entregable 2.2 de la<br>Fase 2) opera en perfecta integración con la aplicación móvil inspeccionada.</td>
</tr>
<tr>
<td colspan="4">Se deja constancia de que los gastos de la fase se mantuvieron dentro del presupuesto autorizado, generando un ahorro favorable para el proyecto.</td>
</tr>
<tr>
<td colspan="2">ACEPTADO POR: DEFINIR LA FECHA, Y LA<br>PERSONA RESPONSABLE DE LA ACEPTACIÓN DE LA FASE.</td>
<td colspan="2">DISTRIBUIDO Y ACEPTADO: DEFINIR LA FECHA,<br>Y LOS INTERESADOS A QUIENES SERÁDISTRIBUIDO LA ACEPTACIÓN FORMAL DE LA FASE.</td>
</tr>
<tr>
<td>Nombre del Patrocinador, Cliente, u otro<br>Funcionario</td>
<td>Fecha</td>
<td>Nombre del Interesado</td>
<td>Fecha</td>
</tr>
<tr>
<td>Jefe de Fundo</td>
<td>03/10/2026</td>
<td>Gerencia General</td>
<td>03/10/2026</td>
</tr>
<tr>
<td>Especialista<br>Fitosanitario</td>
<td>03/10/2026</td>
<td>Jean Pierre Pacheco Tamariz</td>
<td>03/10/2026</td>
</tr>
<tr>
<td>Oscar Antonio Gutiérrez Rojas</td>
<td>03/10/2026</td>
<td>Grace Lucia Peña Reyes</td>
<td>03/10/2026</td>
</tr>
<tr>
<td colspan="2" rowspan="2"></td>
<td>María Fernanda Zúñiga<br>Matos</td>
<td>03/10/2026</td>
</tr>
<tr>
<td>Oscar Manuel Guerrero<br>Figari</td>
<td>03/10/2026</td>
</tr>
</table>
