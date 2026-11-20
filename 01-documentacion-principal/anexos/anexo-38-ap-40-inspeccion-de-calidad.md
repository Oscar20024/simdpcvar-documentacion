# Anexo 38 (AP-40): Inspección de Calidad

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
<td rowspan="2">Grace<br>Lucia Peña Reyes</td>
<td rowspan="2">Oscar Antonio Gutiérrez<br>Rojas</td>
<td rowspan="2">Jefe de Fundo (Agrícola Riachuelo S.A.C.)</td>
<td></td>
<td rowspan="2">Inspección formal de calidad del Backend Central unificado en Django y los servicios cloud en Supabase y<br>Cloudinary.</td>
</tr>
<tr>
<td></td>
<td>02/10/2026</td>
</tr>
</table>

### INSPECCIÓN DE CALIDAD Nº01

| Nombre del Proyecto | Siglas del Proyecto |
|---|---|
| Sistema inteligente para el monitoreo y detección de plagas en cultivos de vid en Agrícola Riachuelo | SIMDPCVAR |

<table>
<tr>
<th colspan="14">Datos del Entregable Inspeccionado</th>
</tr>
<tr>
<td colspan="3">Fase</td>
<td colspan="4">ENTREGABLE 2º<br>NIVEL</td>
<td colspan="4">ENTREGABLE 3º<br>NIVEL</td>
<td colspan="3">Paquete de Trabajo</td>
</tr>
<tr>
<td colspan="3">Fase 3: Desarrollo Tecnológico</td>
<td colspan="4">3.3<br>Plataforma Web Centralizada</td>
<td colspan="4">Backend REST unificado y servicios de integración</td>
<td colspan="3">3.3.1 API REST v1 Django,<br>Supabase BaaS y CDN Cloudinary</td>
</tr>
<tr>
<td colspan="14">Elaborado por</td>
</tr>
<tr>
<td colspan="14">Oscar Manuel Guerrero Figari (Desarrollador Backend, Web e IA) y Grace Lucia Peña Reyes (Analista Funcional).</td>
</tr>
<tr>
<td colspan="14">Estándar, norma o especificación de referencia para realizar la inspección</td>
</tr>
<tr>
<td colspan="14">ISO/IEC 25010 (Métricas de calidad de software: Eficiencia de rendimiento, usabilidad y seguridad).<br>OWASP API Security Top 10 (Validación de autenticación JWT y control de acceso basado en roles).<br>Regla de Arquitectura de Negocio: Prohibición estricta de acceso directo desde clientes móviles o web hacia Supabase/Cloudinary; consumo obligatorio a través de la API central en Django.</td>
</tr>
<tr>
<td colspan="14">Datos de la Inspección</td>
</tr>
<tr>
<td colspan="14">Objetivos de la Inspección</td>
</tr>
<tr>
<td colspan="14">Verificar que el tiempo de respuesta de la API REST v1 en Django sea inferior a en la devolución de indicios e información fitosanitaria.<br>![](../media/image57.png)</td>
</tr>
<tr>
<td colspan="14">Comprobar que la regla estricta de arquitectura se cumpla al (clientes acceden exclusivamente mediante Django REST Framework con tokens JWT).<br>![](../media/image58.png)</td>
</tr>
<tr>
<td colspan="14">Validar la persistencia relacional en Supabase (PostgreSQL) y el almacenamiento optimizado de fotografías en Cloudinary con URLs seguras.</td>
</tr>
<tr>
<td colspan="14">Verificar el desacoplamiento correcto del procesamiento de IA en el worker de Django (cola ai_tasks) (YOLO exportado a ONNX).</td>
</tr>
<tr>
<td colspan="14">Grupo de Inspección</td>
</tr>
<tr>
<td colspan="2">PERSONA</td>
<td colspan="4">ROL EN EL<br>PROYECTO</td>
<td colspan="6">ROL DURANTE LA INSPECCIÓN</td>
<td colspan="2">OBSERVACIONES</td>
</tr>
<tr>
<td colspan="2">Oscar Antonio Gutiérrez<br>Rojas</td>
<td colspan="4">Project Manager<br>/ Líder</td>
<td colspan="6">Inspector Principal / Líder</td>
<td colspan="2">Evaluación de rendimiento y gobernanza.</td>
</tr>
<tr>
<td colspan="2">Grace Lucia<br>Peña Reyes</td>
<td colspan="4">Analista<br>Funcional</td>
<td colspan="6">Inspector de QA y Reglas de Negocio</td>
<td colspan="2">Verificación de flujos y criterios de aceptación.</td>
</tr>
<tr>
<td colspan="2">Oscar Manuel Guerrero<br>Figari</td>
<td colspan="3">Desarrollador<br>Backend IA</td>
<td>e</td>
<td colspan="6">Responsable del Entregable</td>
<td colspan="2">Ejecución de suite de pruebas de API y despliegue.</td>
</tr>
<tr>
<td colspan="2">Especialista<br>Fitosanita rio</td>
<td colspan="3">Validador<br>Técnico Agrónomo</td>
<td>/</td>
<td colspan="6">Usuario Validador Externo</td>
<td colspan="2">Validación funcional de interfaz web y tiempos.</td>
</tr>
<tr>
<td colspan="14">Modo de Inspección</td>
</tr>
<tr>
<td colspan="2">MÉTODO</td>
<td colspan="2">FECHA</td>
<td colspan="4">LUGAR</td>
<td colspan="5">HORARIO</td>
<td>OBSERVACIONES</td>
</tr>
<tr>
<td colspan="2">Pruebas de Estrés y Performa nce de API</td>
<td colspan="2">01/10/20 26</td>
<td colspan="4">Lab.deIngenier ía UTP / Servidor Cloud</td>
<td>09:00</td>
<td>-</td>
<td>11:30</td>
<td></td>
<td></td>
<td>Medición con Postman/JMet er sobre endpoints REST en<br>Django.</td>
</tr>
<tr>
<td>Auditoría<br>Código</td>
<td>de y</td>
<td colspan="2">01/10/20 26</td>
<td colspan="4">Repositorio<br>GitHub</td>
<td>11:30</td>
<td>-</td>
<td>13:00</td>
<td></td>
<td></td>
<td>Revisión de middleware y políticas de</td>
</tr>
</table>

<table>
<tr>
<th>Seguridad<br>JWT</th>
<th colspan="3"></th>
<th>Backend<br>Central</th>
<th colspan="4"></th>
<th colspan="3">CORS/Supaba se.</th>
</tr>
<tr>
<td>Prueba<br>Integral End-to-End (E2E)</td>
<td colspan="3">02/10/20 26</td>
<td>Fundo<br>Agrícola Riachuel o /<br>Entorno<br>Web</td>
<td colspan="4">15:00 - 17:00</td>
<td colspan="3">Simulación de subida de fotos y renderizado en mapa Leaflet.</td>
</tr>
<tr>
<td colspan="2">Resultados<br>Inspección</td>
<td>DE</td>
<td>LA</td>
<td colspan="2">Conforme</td>
<td>x</td>
<td colspan="3">No conforme</td>
<td colspan="2"></td>
</tr>
<tr>
<td colspan="4">Lista de defectos a<br>CORREGIR O MEJORAS A REALIZAR</td>
<td>RESPONSABLE</td>
<td colspan="3">FECHA<br>REQUERI DA</td>
<td colspan="4">OBSERVACIONES</td>
</tr>
<tr>
<td colspan="4">Ajuste de expiración de token JWT: Extender la validez del token de sesión local a para evitar cierres involuntarios durante jornadas largas de campo.<br>![](../media/image59.png)</td>
<td>Oscar<br>Manuel Guerrer o</td>
<td colspan="3">05/10/2026</td>
<td colspan="3">Mejora de experiencia usuario (UX) para operadores.</td>
<td>de los</td>
</tr>
<tr>
<td colspan="4">Caché en miniatura de Cloudinary: Configurar transformación automática de resolución en<br>Cloudinary para agilizar la carga del mapa interactivo en Leaflet.</td>
<td>Oscar<br>Manuel Guerrer o</td>
<td colspan="3">06/10/2026</td>
<td colspan="3">Optimización del tiempo renderizado web.</td>
<td>de</td>
</tr>
<tr>
<td colspan="12">Observaciones complementarias</td>
</tr>
<tr>
<td colspan="12">Se constató que el tiempo promedio de respuesta de la API REST v1 fue de , superando el requerimiento no funcional RNF04 ().<br>![](../media/image60.png)<br>![](../media/image61.png)</td>
</tr>
<tr>
<td colspan="12">Quedó verificado que ni la app móvil ni la web acceden directamente a las credenciales de<br>Supabase o Cloudinary, cumpliendo la regla de seguridad del proyecto.</td>
</tr>
<tr>
<td colspan="12">El Asesor Académico y el Especialista Fitosanitario mostraron conformidad con la agilidad del backend unificado en Django.</td>
</tr>
<tr>
<td colspan="12">Documentos Adjuntos</td>
</tr>
<tr>
<td colspan="12">Diagrama de Arquitectura simplificada con backend unificado</td>
</tr>
<tr>
<td colspan="12">Reporte de Métricas de Rendimiento HTTP / Postman Test Suite</td>
</tr>
<tr>
<td colspan="12">Acta de Validación Funcional de APIs v1 con Tokens JWT.</td>
</tr>
</table>
