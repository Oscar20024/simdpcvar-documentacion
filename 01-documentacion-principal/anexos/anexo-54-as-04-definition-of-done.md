# Anexo 54 (AS-04): Definition of Done

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
<td>Grace<br>Peña<br>/ Equip o Scru m</td>
<td>Scrum Master</td>
<td>Product<br>Owner</td>
<td>06/10/2 026</td>
<td>Actualización de la Definition of Done alineada a la nueva arquitectura unificada (Django +<br>Supabase + Cloudinary) y estándares de calidad.</td>
</tr>
</table>

CRITERIOS DE HECHO (DEFINITION OF DONE -DoD)

**Código y Nombre del Proyecto**

**SIMDPCVAR – Sistema Inteligente para el Monitoreo y Detección de Plagas en Cultivos de Vid en Agrícola Riachuelo S.A.C.**

1\. ***<span class="smallcaps">Propósito y alcance</span>***

**Propósito y Alcance**

**La Definition of Done (DoD) define la lista de comprobación de calidad, pruebas, código y documentación que debe satisfacer cada Historia de Usuario antes de ser aceptada por el Product Owner y considerada como un incremento de software terminado al cierre de cada Sprint.**

2\. ***<span class="smallcaps">Lista de verificación transversal de la Definition of Done (DoD)</span>***

Para que una Historia de Usuario o Tarea Técnica se declare Terminada (Done), debe cumplir obligatoriamente con las siguientes 6 dimensiones de control:

<table>
<tr>
<th colspan="3">1. Calidad deCódigo yArquitectura</th>
</tr>
<tr>
<td>Cumple</td>
<td>Criterio</td>
<td>Descripción</td>
</tr>
<tr>
<td></td>
<td>Estándares de Código</td>
<td>El código backend en Python cumple con las convenciones PEP 8, y el código móvil en React Native sigue los principios de componentes limpios y modulares.</td>
</tr>
<tr>
<td></td>
<td>Revisión por Pares (Peer Review)</td>
<td>Todo cambio en el código fuente ha pasado por un Pull Request (PR) en GitHub con la aprobación obligatoria de al menos un par desarrollador antes de fusionarse a la rama principal (main/develop).</td>
</tr>
<tr>
<td></td>
<td>Arquitectura<br>Unificada</td>
<td>Las peticiones del cliente móvil y web consumen directamente la API REST v1 centralizada en Django, evitando capas o colas intermedias obsoletas.</td>
</tr>
</table>

<table>
<tr>
<th colspan="3">2. Pruebas de Software y Rendimiento (Testing &amp; QA)</th>
</tr>
<tr>
<td>Cumple</td>
<td>Criterio</td>
<td>Descripción</td>
</tr>
<tr>
<td></td>
<td>Pruebas Unitarias e Integradas</td>
<td>Las pruebas automatizadas en Django REST Framework han alcanzado una cobertura de código ≥ 80 % sin fallos en los tests críticos.</td>
</tr>
<tr>
<td>☐</td>
<td>Métricas de Rendimiento Requeridas</td>
<td>• Tiempo de respuesta de los endpoints de la API REST backend ≤ 1.5 segundos (RNF04).<br>• Tiempo de validación local de nitidez e iluminación en la app móvil<br>≤ 2.0 segundos por imagen (RNF02).<br>• Precisión media del modelo de visión artificial YOLO mAP ≥ 85 % sobre el dataset de test.</td>
</tr>
<tr>
<td></td>
<td>Prueba de<br>Persistencia Offline</td>
<td>Se ha verificado en el cliente móvil que la captura y almacenamiento en SQLite / almacenamiento privado de la app (expo-file-system) funcionan al 100 % en modo avión o sin conectividad.</td>
</tr>
</table>

<table>
<tr>
<th colspan="3">3. Seguridad y Custodiade Datos</th>
</tr>
<tr>
<td>Cumple</td>
<td>Criterio</td>
<td>Descripción</td>
</tr>
<tr>
<td></td>
<td>Autenticación y Autorización</td>
<td>Todos los endpoints sensibles de la API exigen tokens JWT válidos y validación de roles de usuario (Operador de campo, Especialista fitosanitario, Supervisor y Administrador).</td>
</tr>
<tr>
<td></td>
<td>Carga de Evidencias</td>
<td>Las imágenes capturadas en campo se suben directamente al CDN de Cloudinary utilizando únicamente tickets o firmas temporales seguras desde el cliente móvil.</td>
</tr>
<tr>
<td></td>
<td>Seguridad en BD</td>
<td>La persistencia relacional en Supabase (PostgreSQL) tiene RLS activado sin políticas, de modo que solo el backend Django se conecta a la base de datos.</td>
</tr>
</table>

<table>
<tr>
<th colspan="3">4. Integración y Despliegue (DevOps &amp; Deployment)</th>
</tr>
<tr>
<td>Cumple</td>
<td>Criterio</td>
<td>Descripción</td>
</tr>
<tr>
<td></td>
<td>Despliegue Cloud Operativo</td>
<td>Los contenedores web (Django) y worker de IA (YOLO/ONNX) están correctamente desplegados con Docker Compose en el VPS Hostinger (Traefik con HTTPS) sin errores 500.</td>
</tr>
<tr>
<td></td>
<td>Sincronización Transaccional</td>
<td>Se ha probado que la sincronización móvil-nube encola y sube los metadatos de las fotos locales de forma idempotente, sin duplicar registros de hileras ni perder información espacial.</td>
</tr>
</table>

<table>
<tr>
<th colspan="3">5. Documentación y Trazabilidad(Documentación)</th>
</tr>
<tr>
<td>Cumple</td>
<td>Criterio</td>
<td>Descripción</td>
</tr>
<tr>
<td></td>
<td>Criterios de Aceptación</td>
<td>Se han validado y ejecutado satisfactoriamente el 100 % de los criterios de aceptación específicos de la Historia de Usuario (Anexo AS-03).</td>
</tr>
<tr>
<td></td>
<td>Documentación de API</td>
<td>Los nuevos endpoints implementados en Django están documentados en la especificación OpenAPI / Swagger de la plataforma.</td>
</tr>
<tr>
<td></td>
<td>Historial de Cambios</td>
<td>Las modificaciones aprobadas están registradas en el Log de Control de Cambios del Proyecto.</td>
</tr>
</table>

<table>
<tr>
<th colspan="3">6. Validación y Aceptación por el ProductOwner</th>
</tr>
<tr>
<td>Cumple</td>
<td>Criterio</td>
<td>Descripción</td>
</tr>
<tr>
<td>☐</td>
<td>Demostración en Sprint Review</td>
<td>La funcionalidad fue demostrada operativamente al Product Owner y Especialista Fitosanitario durante la sesión de Sprint Review.</td>
</tr>
<tr>
<td>☐</td>
<td>Aprobación Formal</td>
<td>El Product Owner ha firmado la conformidad del incremento funcional, autorizando su integración al producto final.</td>
</tr>
</table>
