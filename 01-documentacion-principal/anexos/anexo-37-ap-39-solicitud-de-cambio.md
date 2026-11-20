# Anexo 37 (AP-39): Solicitud de Cambio

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
<td>Oscar Antonio Gutiérrez<br>Rojas</td>
<td>Comité de<br>Control de Cambios<br>/ Jefe de Fundo</td>
<td>22/09/2026</td>
<td>Solicitud formal de cambio de arquitectura tecnológica para la plataforma web y backend.</td>
</tr>
</table>

### SOLICITUD DE CAMBIO Nº01

| Nombre del Proyecto | Siglas del Proyecto | Solicitantes del Cambio |
|---|---|---|
| Sistema inteligente para el monitoreo y detección de plagas en cultivos de vid en Agrícola<br>Riachuelo | SIMDPCVAR | Oscar Manuel Guerrero Figari (Desarrollador), Grace<br>Lucia Peña Reyes (Analista Funcional) y Oscar Antonio Gutiérrez Rojas (Project<br>Manager). |

<table style="width:100%;">
<colgroup>
<col style="width: 45%" />
<col style="width: 4%" />
<col style="width: 44%" />
<col style="width: 5%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="4"><blockquote>
<p><strong><span class="smallcaps">Tipo de Cambio Requerido</span></strong></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><em><strong><span class="smallcaps">Generación de Acción Correctiva:</span></strong></em></p>
</blockquote></td>
<td>X</td>
<td><blockquote>
<p><em><strong><span class="smallcaps">Reparación de Defecto:</span></strong></em></p>
</blockquote></td>
<td></td>
</tr>
<tr class="even">
<td><blockquote>
<p><em><strong><span class="smallcaps">Generación de Acción Preventiva:</span></strong></em></p>
</blockquote></td>
<td>X</td>
<td><blockquote>
<p><em><strong><span class="smallcaps">Actualizaciones:</span></strong></em></p>
</blockquote></td>
<td><blockquote>
<p>X</p>
</blockquote></td>
</tr>
<tr class="odd">
<td colspan="4"><blockquote>
<p><strong>DEFINICIÓN DEL PROBLEMA O SITUACIÓN ACTUAL:</strong> <em>DEFINA Y ACOTE EL PROBLEMA QUE SE VA A RESOLVER, DISTINGUIENDO EL PROBLEMA DE SUS CAUSAS, Y DE SUS CONSECUENCIAS.</em></p>
</blockquote></td>
</tr>
<tr class="even">
<td colspan="4"><blockquote>
<p>En la arquitectura inicial basada en Spring Boot (Java), cliente web en Angular, colas de trabajo en RabbitMQ, PostgreSQL administrado en VPS, Amazon S3 y un microservicio desacoplado de IA en Python, se identificó un problema crítico de rendimiento, latencia y sobrecarga administrativa. Al enviar una consulta o fotografía desde la plataforma web o la app móvil, la información realizaba múltiples saltos síncronos y asíncronos entre distintos servicios e infraestructuras, lo que incrementaba los tiempos de respuesta e introducía puntos de fallo innecesarios. Además, la instalación y administración manual de servicios en VPS elevaba la complejidad del mantenimiento, dificultaba la depuración de errores y ponía en riesgo el cumplimiento del requerimiento no funcional RNF04 (tiempo de respuesta de la API menor a 1,5 segundos). Para solucionar estas limitaciones, se reformuló la solución hacia una arquitectura unificada y simplificada donde un backend central en Django (Python) y Django REST Framework sustituye a Spring Boot para asumir el control único de las reglas de negocio, la API REST versionada, la gestión de usuarios y la comunicación directa con el modelo de IA (FastAPI + Python + YOLO), eliminando la cola RabbitMQ e intermediarios. Asimismo, Django Templates reemplaza a Angular para generar la plataforma web directamente desde el backend, Supabase (PostgreSQL administrado) sustituye la instalación manual</p>
<p>en VPS, y Cloudinary reemplaza a Amazon S3 para la gestión y almacenamiento</p>
</blockquote></td>
</tr>
</tbody>
</table>

<table>
<tr>
<th colspan="2">optimizado de fotografías. Por su parte, se mantuvieron los componentes con desempeño óptimo: el cliente móvil en React Native + Expo, la persistencia local offline-first con SQLite / FileSystem y el renderizado cartográfico con Leaflet / OpenStreetMap, logrando un flujo lineal que garantiza el cumplimiento de los tiempos de respuesta requeridos.</th>
</tr>
<tr>
<td colspan="2">DESCRIPCIÓN DETALLADA DEL CAMBIO SOLICITADO: ESPECIFIQUE CON CLARIDAD EL CAMBIO SOLICITADO, PRECISANDO EL QUÉ, QUIÉN, CÓMO, CUÁNDO Y DÓNDE.</td>
</tr>
<tr>
<td colspan="2">Sustitución del Backend Java por Django (Python): Desarrollar la API REST unificada en Django REST Framework, permitiendo coordinar la lógica de negocio, las reglas de acceso con tokens JWT y las llamadas al modelo YOLO dentro del mismo ecosistema<br>Python sin requerir colas intermedias de mensajería (RabbitMQ).</td>
</tr>
<tr>
<td colspan="2">Adopción de Supabase como BaaS: Reemplazar el contenedor PostgreSQL Docker por Supabase (PostgreSQL administrado), utilizándolo de forma exclusiva desde el backend<br>Django para la gestión relacional, datos geoespaciales (PostGIS) y autenticación segura.</td>
</tr>
<tr>
<td colspan="2">Migración a Cloudinary para la gestión de media: Sustituir Amazon S3 por Cloudinary para el almacenamiento optimizado de fotografías, miniaturas y entrega de URLs seguras gestionadas directamente desde la API en Django.</td>
</tr>
<tr>
<td colspan="2">Integración directa del servicio de IA: Conectar el microservicio Python (FastAPI + YOLO) de forma directa e interna con el backend Django, manteniendo la regla estricta de que ni la app móvil ni la web acceden directamente a Supabase ni a Cloudinary.</td>
</tr>
<tr>
<td colspan="2">RAZÓN POR LA QUE SE SOLICITA EL CAMBIO: ESPECIFIQUE CON CLARIDAD PORQUE MOTIVOS O<br>RAZONES SOLICITA EL CAMBIO, PORQUE MOTIVOS ELIGEN ESTE CURSO DE ACCIÓN Y NO OTRO ALTERNATIVO, Y QUÉ SUCEDERÍA SI EL CAMBIO NO SE REALIZA.</td>
</tr>
<tr>
<td colspan="2">Garantía de rendimiento y menor latencia: Unificar el backend y la Inteligencia Artificial bajo Python elimina la sobrecarga de traducción de protocolos y reduce el tiempo de respuesta de la API a menos de 1.2s.</td>
</tr>
<tr>
<td colspan="2">Simplicidad de arquitectura y despliegue: Pasa de 6 componentes desacoplados a un backend central unificado en Django que consolida la lógica en un solo lugar, facilitando el mantenimiento y las pruebas piloto en campo.</td>
</tr>
<tr>
<td colspan="2">Alineación con la recomendación técnica y académica: La propuesta fue evaluada y respaldada positivamente por el Asesor Académico (Prof. Pacheco) y el equipo de desarrollo para asegurar el éxito del proyecto de tesis.</td>
</tr>
<tr>
<td colspan="2">Consecuencia de no realizar el cambio: Si el cambio no se realiza, la plataforma web experimentará demoras severas en la carga del mapa interactivo y la revisión de casos, afectando la usabilidad del Especialista Fitosanitario y rechazando el piloto por parte del negocio.</td>
</tr>
<tr>
<td colspan="2">EFECTOS EN EL PROYECTO: DEFINIR EL EFECTO DEL CAMBIO SOLICITADO A CORTO O LARGO PLAZO EN EL ALCANCE DEL PROYECTO.</td>
</tr>
<tr>
<td>En el corto plazo</td>
<td>En el largo plazo</td>
</tr>
<tr>
<td>Alcance: Modificación leve en los scripts de despliegue y en la configuración del backend del paquete 3.3 y 4.1 de la EDT/WBS.<br>Cronograma: Reorganización de 3 días dentro del Sprint 4 para la migración del esquema de datos a Supabase y configuración de credenciales de Cloudinary (absorbido sin afectar la fecha final del piloto).<br>Costo: Reducción del gasto mensual estimado en servicios cloud al utilizar las capas administradas de Supabase y Cloudinary (ahorro neto de S/ 70.00 en el periodo).</td>
<td>Calidad y Mantenibilidad: Incremento significativo en la velocidad de respuesta del sistema, código más limpio, arquitectura unificada y mayor facilidad para escalar la solución a otros fundos o cultivos en el futuro.</td>
</tr>
<tr>
<td colspan="2">Efectos en otros proyectos, programas, portafolios u operaciones.</td>
</tr>
</table>

| Ninguno negativo. En la operación de Agrícola Riachuelo S.A.C., el cambio es completamente transparente para los usuarios finales (Especialista y Jefe de Fundo), ya que las interfaces de la app móvil en React Native y las pantallas web mantienen el mismo diseño funcional, pero con respuestas mucho más ágiles. |
|---|
| Efectos extra empresariales en clientes, mercados, proveedores, gobierno,<br>ETC. |
| Positivo. Garantiza la disponibilidad continua de la plataforma web para auditorías de calidad extramuros (certificadoras como GlobalG.A.P. y SENASA), permitiendo visualizar en tiempo real la evidencia fotográfica alojada de forma rápida en la CDN de Cloudinary. |

<table>
<tr>
<th colspan="2">Observaciones y Comentarios adicionales.</th>
</tr>
<tr>
<td colspan="2">La app móvil en React Native no sufre cambios en su flujo de uso ni en su base de datos local SQLite; únicamente actualizará el endpoint base de sus peticiones HTTP/REST hacia la API en Django.</td>
</tr>
<tr>
<td colspan="2">Se adjuntó el diagrama formal de la Arquitectura simplificada con backend unificado para la revisión del comité.</td>
</tr>
<tr>
<td colspan="2">Revisión del Comité de Control de Cambios.</td>
</tr>
<tr>
<td>Fecha de Revisión</td>
<td>25 DE SEPTIEMBRE DE 2026</td>
</tr>
<tr>
<td>Efectuada Por</td>
<td>Oscar Antonio Gutiérrez Rojas (Project Manager), Jean Pierre Pacheco Tamariz (Asesor Académico) y Jefe de Fundo (Agrícola Riachuelo S.A.C.)</td>
</tr>
<tr>
<td>Resultados de Revisión (APROBADA/RECHAZADA)</td>
<td>APROBADA POR UNANIMIDAD</td>
</tr>
<tr>
<td>Responsable de Aplicar/Informar</td>
<td>Oscar Manuel Guerrero Figari (Desarrollador) y<br>Grace Lucia Peña Reyes (Analista Funcional).</td>
</tr>
<tr>
<td>Observaciones Especiales</td>
<td>Proceder con la actualización de la documentación de arquitectura en los Capítulos VI y VIII del informe final de tesis y actualizar la matriz de<br>RIESGOS E INCIDENTES CORRESPONDIENTES.</td>
</tr>
</table>

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
<td>Grace Lucia Peña Reyes</td>
<td>Oscar Antonio Gutiérrez Rojas</td>
<td>Comité de Control de Cambios / Jefe de Fundo</td>
<td>14/11/2026</td>
<td>Solicitud formal de cambio: avisos por Telegram en lugar de WhatsApp.</td>
</tr>
</table>

### SOLICITUD DE CAMBIO Nº02

| Nombre del Proyecto | Siglas del Proyecto | Solicitantes del Cambio |
|---|---|---|
| Sistema inteligente para el monitoreo y detección de plagas en cultivos de vid en Agrícola Riachuelo | SIMDPCVAR | Oscar Manuel Guerrero Figari (Desarrollador), Grace Lucia Peña Reyes (Analista Funcional) y Oscar Antonio Gutiérrez Rojas (Project Manager). |

<table style="width:100%;">
<colgroup>
<col style="width: 45%" />
<col style="width: 4%" />
<col style="width: 44%" />
<col style="width: 5%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="4"><blockquote>
<p><strong><span class="smallcaps">Tipo de Cambio Requerido</span></strong></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><em><strong><span class="smallcaps">Generación de Acción Correctiva:</span></strong></em></p>
</blockquote></td>
<td>X</td>
<td><blockquote>
<p><em><strong><span class="smallcaps">Reparación de Defecto:</span></strong></em></p>
</blockquote></td>
<td></td>
</tr>
<tr class="even">
<td><blockquote>
<p><em><strong><span class="smallcaps">Generación de Acción Preventiva:</span></strong></em></p>
</blockquote></td>
<td></td>
<td><blockquote>
<p><em><strong><span class="smallcaps">Actualizaciones:</span></strong></em></p>
</blockquote></td>
<td><blockquote>
<p>X</p>
</blockquote></td>
</tr>
<tr class="odd">
<td colspan="4"><blockquote>
<p><strong>DEFINICIÓN DEL PROBLEMA O SITUACIÓN ACTUAL:</strong> <em>DEFINA Y ACOTE EL PROBLEMA QUE SE VA A RESOLVER, DISTINGUIENDO EL PROBLEMA DE SUS CAUSAS, Y DE SUS CONSECUENCIAS.</em></p>
</blockquote></td>
</tr>
<tr class="even">
<td colspan="4"><blockquote>
<p>RF10 exige enviar la notificación «vía WhatsApp» al jefe de fundo y al supervisor de zona después de la confirmación del especialista. La integración con WhatsApp Cloud API requiere una cuenta de empresa en Meta, un número de teléfono verificado y plantillas de mensaje aprobadas. Por problemas de configuración esos trámites no avanzaron durante el piloto y los avisos quedaron en modo de prueba (consola), sin llegar a los responsables.</p>
</blockquote></td>
</tr>
</tbody>
</table>

<table>
<tr>
<th colspan="2">DESCRIPCIÓN DETALLADA DEL CAMBIO SOLICITADO: ESPECIFIQUE CON CLARIDAD EL CAMBIO SOLICITADO, PRECISANDO EL QUÉ, QUIÉN, CÓMO, CUÁNDO Y DÓNDE.</th>
</tr>
<tr>
<td colspan="2">Canal: los avisos se envían por la Bot API de Telegram, a personas o a un grupo por lote. Se retira el cliente de WhatsApp de la plataforma.</td>
</tr>
<tr>
<td colspan="2">Contenido del aviso: foto del caso con las cajas de la IA, lote, hilera, lado, segmento, marcador, fecha, confianza, observación del especialista y los botones «Ver caso» (con inicio de sesión) y «Cómo llegar» (Google Maps).</td>
</tr>
<tr>
<td colspan="2">Destinatarios: el administrador los registra en la web (Administración → Destinatarios de avisos), con «Buscar chats», «Usar este chat» y «Enviar prueba». Solo el worker envía los avisos y reintenta los errores temporales; el token del bot vive solo en el servidor.</td>
</tr>
<tr>
<td colspan="2">RAZÓN POR LA QUE SE SOLICITA EL CAMBIO: ESPECIFIQUE CON CLARIDAD PORQUE MOTIVOS O<br>RAZONES SOLICITA EL CAMBIO, PORQUE MOTIVOS ELIGEN ESTE CURSO DE ACCIÓN Y NO OTRO ALTERNATIVO, Y QUÉ SUCEDERÍA SI EL CAMBIO NO SE REALIZA.</td>
</tr>
<tr>
<td colspan="2">Telegram se activa en minutos con @BotFather, es gratuito y no depende de aprobaciones de terceros. Se mantiene la regla de negocio: el aviso sale solo cuando el hallazgo está confirmado (RN-02).</td>
</tr>
<tr>
<td colspan="2">Consecuencia de no realizar el cambio: RF10 no se podría cumplir en el piloto y los hallazgos confirmados no llegarían a tiempo a la jefatura de fundo.</td>
</tr>
<tr>
<td colspan="2">EFECTOS EN EL PROYECTO: DEFINIR EL EFECTO DEL CAMBIO SOLICITADO A CORTO O LARGO PLAZO EN EL ALCANCE DEL PROYECTO.</td>
</tr>
<tr>
<td>En el corto plazo</td>
<td>En el largo plazo</td>
</tr>
<tr>
<td>Alcance: RF10 operativo de inmediato con un canal distinto.<br>Cronograma: sin impacto; el cambio se hizo dentro del Sprint 5.</td>
<td>Mantenibilidad: el envío está aislado en el worker; si más adelante se aprueba la cuenta de WhatsApp, se puede cambiar el canal sin tocar la regla de negocio.<br>Costo: sin costo de mensajería.</td>
</tr>
<tr>
<td colspan="2">Efectos en otros proyectos, programas, portafolios u operaciones.</td>
</tr>
</table>

| Ninguno negativo. Las fotos de los casos confirmados quedan en los chats o grupos de Telegram de los destinatarios. |
|---|
| Efectos extra empresariales en clientes, mercados, proveedores, gobierno,<br>ETC. |
| Los destinatarios deben instalar Telegram y escribir una vez al bot para poder recibir avisos. |

<table>
<tr>
<th colspan="2">Observaciones y Comentarios adicionales.</th>
</tr>
<tr>
<td colspan="2">Requerimientos y documentos afectados: RF10, CU8, HU04 y Tabla 30 (notificaciones). Aplicado en el sistema: 05/11/2026 (web v1.5 y v1.5.1).<br>Guía de activación del bot y registro de destinatarios en la documentación técnica (ADR-W-011). Pruebas: CP-W12, CP-W21, CP-W25 y CP-W26 de la Documentación del sistema y guía de pruebas.</td>
</tr>
<tr>
<td colspan="2">Revisión del Comité de Control de Cambios.</td>
</tr>
<tr>
<td>Fecha de Revisión</td>
<td>Por programar</td>
</tr>
<tr>
<td>Efectuada Por</td>
<td>Oscar Antonio Gutiérrez Rojas (Project Manager), Jean Pierre Pacheco Tamariz (Asesor Académico) y Jefe de Fundo (Agrícola Riachuelo S.A.C.)</td>
</tr>
<tr>
<td>Resultados de Revisión (APROBADA/RECHAZADA)</td>
<td>PENDIENTE DE REVISIÓN (cambio ya aplicado en el piloto)</td>
</tr>
<tr>
<td>Responsable de Aplicar/Informar</td>
<td>Oscar Manuel Guerrero Figari (Desarrollador) y Grace Lucia Peña Reyes (Analista Funcional).</td>
</tr>
<tr>
<td>Observaciones Especiales</td>
<td>El texto de los requerimientos no se modifica; esta solicitud justifica la diferencia. Actualizar la matriz de trazabilidad y el registro de cambios (Anexo 42) con el resultado de la revisión.</td>
</tr>
</table>

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
<td>Grace Lucia Peña Reyes</td>
<td>Oscar Antonio Gutiérrez Rojas</td>
<td>Comité de Control de Cambios / Jefe de Fundo</td>
<td>14/11/2026</td>
<td>Solicitud formal de cambio: confirmación automática por la IA y decisión «Posible plaga».</td>
</tr>
</table>

### SOLICITUD DE CAMBIO Nº03

| Nombre del Proyecto | Siglas del Proyecto | Solicitantes del Cambio |
|---|---|---|
| Sistema inteligente para el monitoreo y detección de plagas en cultivos de vid en Agrícola Riachuelo | SIMDPCVAR | Oscar Manuel Guerrero Figari (Desarrollador), Grace Lucia Peña Reyes (Analista Funcional) y Oscar Antonio Gutiérrez Rojas (Project Manager). |

<table style="width:100%;">
<colgroup>
<col style="width: 45%" />
<col style="width: 4%" />
<col style="width: 44%" />
<col style="width: 5%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="4"><blockquote>
<p><strong><span class="smallcaps">Tipo de Cambio Requerido</span></strong></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><em><strong><span class="smallcaps">Generación de Acción Correctiva:</span></strong></em></p>
</blockquote></td>
<td></td>
<td><blockquote>
<p><em><strong><span class="smallcaps">Reparación de Defecto:</span></strong></em></p>
</blockquote></td>
<td></td>
</tr>
<tr class="even">
<td><blockquote>
<p><em><strong><span class="smallcaps">Generación de Acción Preventiva:</span></strong></em></p>
</blockquote></td>
<td>X</td>
<td><blockquote>
<p><em><strong><span class="smallcaps">Actualizaciones:</span></strong></em></p>
</blockquote></td>
<td><blockquote>
<p>X</p>
</blockquote></td>
</tr>
<tr class="odd">
<td colspan="4"><blockquote>
<p><strong>DEFINICIÓN DEL PROBLEMA O SITUACIÓN ACTUAL:</strong> <em>DEFINA Y ACOTE EL PROBLEMA QUE SE VA A RESOLVER, DISTINGUIENDO EL PROBLEMA DE SUS CAUSAS, Y DE SUS CONSECUENCIAS.</em></p>
</blockquote></td>
</tr>
<tr class="even">
<td colspan="4"><blockquote>
<p>En las primeras pruebas casi todas las fotos terminaban como «Pendiente de revisión» y el especialista no alcanzaba a revisarlas. Además, cuando la IA estaba muy segura, el aviso esperaba igualmente al especialista, y no existía una decisión para «no es seguro, hay que ir a mirar».</p>
</blockquote></td>
</tr>
</tbody>
</table>

<table>
<tr>
<th colspan="2">DESCRIPCIÓN DETALLADA DEL CAMBIO SOLICITADO: ESPECIFIQUE CON CLARIDAD EL CAMBIO SOLICITADO, PRECISANDO EL QUÉ, QUIÉN, CÓMO, CUÁNDO Y DÓNDE.</th>
</tr>
<tr>
<td colspan="2">Triage de la IA en tres franjas: sin indicios suficientes la foto se descarta sola (sin caso, pero se puede rescatar a mano); en la franja intermedia queda «Pendiente de revisión»; con 2 o más cajas de confianza ≥ 0,70, o 3 o más ≥ 0,50, el caso pasa a «Confirmado por IA» y se avisa, marcado «falta la revisión del especialista».</td>
</tr>
<tr>
<td colspan="2">El especialista sigue decidiendo: puede confirmar, descartar o corregir los casos confirmados por la IA; descartar anula los avisos pendientes.</td>
</tr>
<tr>
<td colspan="2">Nueva decisión «Posible plaga»: sin aviso, visible en «Ubicar plaga» de la app para que alguien vaya a revisar la planta.</td>
</tr>
<tr>
<td colspan="2">Interruptor: la confirmación automática se apaga dejando vacío el umbral automático del modelo; en ese caso el sistema vuelve a cumplir RN-06 al pie de la letra.</td>
</tr>
<tr>
<td colspan="2">RAZÓN POR LA QUE SE SOLICITA EL CAMBIO: ESPECIFIQUE CON CLARIDAD PORQUE MOTIVOS O<br>RAZONES SOLICITA EL CAMBIO, PORQUE MOTIVOS ELIGEN ESTE CURSO DE ACCIÓN Y NO OTRO ALTERNATIVO, Y QUÉ SUCEDERÍA SI EL CAMBIO NO SE REALIZA.</td>
</tr>
<tr>
<td colspan="2">Responder antes ante plagas evidentes y concentrar al especialista en los casos dudosos, sin perder la revisión humana ni la trazabilidad (todo queda en la auditoría).</td>
</tr>
<tr>
<td colspan="2">Consecuencia de no realizar el cambio: la bandeja se satura, los avisos de plagas evidentes se retrasan y el especialista revisa fotos sin indicios.</td>
</tr>
<tr>
<td colspan="2">EFECTOS EN EL PROYECTO: DEFINIR EL EFECTO DEL CAMBIO SOLICITADO A CORTO O LARGO PLAZO EN EL ALCANCE DEL PROYECTO.</td>
</tr>
<tr>
<td>En el corto plazo</td>
<td>En el largo plazo</td>
</tr>
<tr>
<td>Alcance: se agregan dos estados al caso (Confirmado por IA y Posible plaga).<br>Calidad: menos casos pendientes y avisos más rápidos.</td>
<td>Calidad de la IA: se miden falsos positivos y se ajustan los umbrales por versión de modelo.<br>Riesgo: un falso positivo de alta confianza genera un aviso; se mitiga con la etiqueta «falta la revisión del especialista» y con la corrección posterior.</td>
</tr>
<tr>
<td colspan="2">Efectos en otros proyectos, programas, portafolios u operaciones.</td>
</tr>
</table>

| Ninguno. |
|---|
| Efectos extra empresariales en clientes, mercados, proveedores, gobierno,<br>ETC. |
| El jefe de fundo y los supervisores reciben avisos sin esperar la revisión cuando la IA tiene alta confianza. |

<table>
<tr>
<th colspan="2">Observaciones y Comentarios adicionales.</th>
</tr>
<tr>
<td colspan="2">Requerimientos y documentos afectados: RF09, RF10, RN-01, RN-02, RN-06, CU6 y CU8. Aplicado en el sistema: 08/10/2026 y 09/10/2026 (web v1.3 y v1.3.1).<br>Flexibiliza RN-01 y RN-06: requiere la aprobación expresa del especialista fitosanitario y del jefe de fundo. Si no se aprueba, basta con vaciar el umbral automático (sin cambiar código). Decisiones de diseño ADR-W-007, ADR-W-008 y ADR-W-009.</td>
</tr>
<tr>
<td colspan="2">Revisión del Comité de Control de Cambios.</td>
</tr>
<tr>
<td>Fecha de Revisión</td>
<td>Por programar</td>
</tr>
<tr>
<td>Efectuada Por</td>
<td>Oscar Antonio Gutiérrez Rojas (Project Manager), Jean Pierre Pacheco Tamariz (Asesor Académico) y Jefe de Fundo (Agrícola Riachuelo S.A.C.)</td>
</tr>
<tr>
<td>Resultados de Revisión (APROBADA/RECHAZADA)</td>
<td>PENDIENTE DE REVISIÓN (cambio ya aplicado en el piloto)</td>
</tr>
<tr>
<td>Responsable de Aplicar/Informar</td>
<td>Oscar Manuel Guerrero Figari (Desarrollador) y Grace Lucia Peña Reyes (Analista Funcional).</td>
</tr>
<tr>
<td>Observaciones Especiales</td>
<td>El texto de los requerimientos no se modifica; esta solicitud justifica la diferencia. Actualizar la matriz de trazabilidad y el registro de cambios (Anexo 42) con el resultado de la revisión.</td>
</tr>
</table>

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
<td>Grace Lucia Peña Reyes</td>
<td>Oscar Antonio Gutiérrez Rojas</td>
<td>Comité de Control de Cambios / Jefe de Fundo</td>
<td>14/11/2026</td>
<td>Solicitud formal de cambio: cobertura bilateral de la planta con los laterales A y B.</td>
</tr>
</table>

### SOLICITUD DE CAMBIO Nº04

| Nombre del Proyecto | Siglas del Proyecto | Solicitantes del Cambio |
|---|---|---|
| Sistema inteligente para el monitoreo y detección de plagas en cultivos de vid en Agrícola Riachuelo | SIMDPCVAR | Oscar Manuel Guerrero Figari (Desarrollador), Grace Lucia Peña Reyes (Analista Funcional) y Oscar Antonio Gutiérrez Rojas (Project Manager). |

<table style="width:100%;">
<colgroup>
<col style="width: 45%" />
<col style="width: 4%" />
<col style="width: 44%" />
<col style="width: 5%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="4"><blockquote>
<p><strong><span class="smallcaps">Tipo de Cambio Requerido</span></strong></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><em><strong><span class="smallcaps">Generación de Acción Correctiva:</span></strong></em></p>
</blockquote></td>
<td></td>
<td><blockquote>
<p><em><strong><span class="smallcaps">Reparación de Defecto:</span></strong></em></p>
</blockquote></td>
<td></td>
</tr>
<tr class="even">
<td><blockquote>
<p><em><strong><span class="smallcaps">Generación de Acción Preventiva:</span></strong></em></p>
</blockquote></td>
<td></td>
<td><blockquote>
<p><em><strong><span class="smallcaps">Actualizaciones:</span></strong></em></p>
</blockquote></td>
<td><blockquote>
<p>X</p>
</blockquote></td>
</tr>
<tr class="odd">
<td colspan="4"><blockquote>
<p><strong>DEFINICIÓN DEL PROBLEMA O SITUACIÓN ACTUAL:</strong> <em>DEFINA Y ACOTE EL PROBLEMA QUE SE VA A RESOLVER, DISTINGUIENDO EL PROBLEMA DE SUS CAUSAS, Y DE SUS CONSECUENCIAS.</em></p>
</blockquote></td>
</tr>
<tr class="even">
<td colspan="4"><blockquote>
<p>RF01 y la propuesta de mejora describen una cámara izquierda y una derecha que fotografían a la vez los dos lados de cada planta. En la vid conducida en espaldera, desde el pasillo solo se ve una cara de la hilera: con una sola pasada no es posible fotografiar los dos lados de la misma planta.</p>
</blockquote></td>
</tr>
</tbody>
</table>

<table>
<tr>
<th colspan="2">DESCRIPCIÓN DETALLADA DEL CAMBIO SOLICITADO: ESPECIFIQUE CON CLARIDAD EL CAMBIO SOLICITADO, PRECISANDO EL QUÉ, QUIÉN, CÓMO, CUÁNDO Y DÓNDE.</th>
</tr>
<tr>
<td colspan="2">Las dos cámaras van en la barra del soporte tipo T y fotografían el mismo punto de la misma cara de la hilera (doble evidencia por punto). Cada hilera se recorre dos veces: lateral A de ida y lateral B de vuelta. La plataforma decide una sola vez por lugar usando las fotos de las dos cámaras.</td>
</tr>
<tr>
<td colspan="2">RAZÓN POR LA QUE SE SOLICITA EL CAMBIO: ESPECIFIQUE CON CLARIDAD PORQUE MOTIVOS O<br>RAZONES SOLICITA EL CAMBIO, PORQUE MOTIVOS ELIGEN ESTE CURSO DE ACCIÓN Y NO OTRO ALTERNATIVO, Y QUÉ SUCEDERÍA SI EL CAMBIO NO SE REALIZA.</td>
</tr>
<tr>
<td colspan="2">Así cada planta queda fotografiada por sus dos lados y cada punto tiene dos fotos para validar la calidad y el análisis de la IA.</td>
</tr>
<tr>
<td colspan="2">Consecuencia de no realizar el cambio: una cara de cada hilera quedaría sin evidencia.</td>
</tr>
<tr>
<td colspan="2">EFECTOS EN EL PROYECTO: DEFINIR EL EFECTO DEL CAMBIO SOLICITADO A CORTO O LARGO PLAZO EN EL ALCANCE DEL PROYECTO.</td>
</tr>
<tr>
<td>En el corto plazo</td>
<td>En el largo plazo</td>
</tr>
<tr>
<td>Procedimiento: dos pasadas por hilera; capacitación breve a los operadores.</td>
<td>IA: más evidencia por punto para reentrenar el modelo.</td>
</tr>
<tr>
<td colspan="2">Efectos en otros proyectos, programas, portafolios u operaciones.</td>
</tr>
</table>

| Ninguno. |
|---|
| Efectos extra empresariales en clientes, mercados, proveedores, gobierno,<br>ETC. |
| Ninguno. |

<table>
<tr>
<th colspan="2">Observaciones y Comentarios adicionales.</th>
</tr>
<tr>
<td colspan="2">Requerimientos y documentos afectados: RF01, CU1, HU01. Aplicado en el sistema: octubre de 2026 (app 0.4.x a 0.6.1).<br>El texto de RF01 se mantiene; esta solicitud aclara cómo se cumple «capturar fotos bilaterales». Pruebas: CP-A09, CP-A16 y CP-W18.</td>
</tr>
<tr>
<td colspan="2">Revisión del Comité de Control de Cambios.</td>
</tr>
<tr>
<td>Fecha de Revisión</td>
<td>Por programar</td>
</tr>
<tr>
<td>Efectuada Por</td>
<td>Oscar Antonio Gutiérrez Rojas (Project Manager), Jean Pierre Pacheco Tamariz (Asesor Académico) y Jefe de Fundo (Agrícola Riachuelo S.A.C.)</td>
</tr>
<tr>
<td>Resultados de Revisión (APROBADA/RECHAZADA)</td>
<td>PENDIENTE DE REVISIÓN (cambio ya aplicado en el piloto)</td>
</tr>
<tr>
<td>Responsable de Aplicar/Informar</td>
<td>Oscar Manuel Guerrero Figari (Desarrollador) y Grace Lucia Peña Reyes (Analista Funcional).</td>
</tr>
<tr>
<td>Observaciones Especiales</td>
<td>El texto de los requerimientos no se modifica; esta solicitud justifica la diferencia. Actualizar la matriz de trazabilidad y el registro de cambios (Anexo 42) con el resultado de la revisión.</td>
</tr>
</table>

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
<td>Grace Lucia Peña Reyes</td>
<td>Oscar Antonio Gutiérrez Rojas</td>
<td>Comité de Control de Cambios / Jefe de Fundo</td>
<td>14/11/2026</td>
<td>Solicitud formal de cambio: límite de repeticiones por calidad de imagen.</td>
</tr>
</table>

### SOLICITUD DE CAMBIO Nº05

| Nombre del Proyecto | Siglas del Proyecto | Solicitantes del Cambio |
|---|---|---|
| Sistema inteligente para el monitoreo y detección de plagas en cultivos de vid en Agrícola Riachuelo | SIMDPCVAR | Oscar Manuel Guerrero Figari (Desarrollador), Grace Lucia Peña Reyes (Analista Funcional) y Oscar Antonio Gutiérrez Rojas (Project Manager). |

<table style="width:100%;">
<colgroup>
<col style="width: 45%" />
<col style="width: 4%" />
<col style="width: 44%" />
<col style="width: 5%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="4"><blockquote>
<p><strong><span class="smallcaps">Tipo de Cambio Requerido</span></strong></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><em><strong><span class="smallcaps">Generación de Acción Correctiva:</span></strong></em></p>
</blockquote></td>
<td></td>
<td><blockquote>
<p><em><strong><span class="smallcaps">Reparación de Defecto:</span></strong></em></p>
</blockquote></td>
<td>X</td>
</tr>
<tr class="even">
<td><blockquote>
<p><em><strong><span class="smallcaps">Generación de Acción Preventiva:</span></strong></em></p>
</blockquote></td>
<td>X</td>
<td><blockquote>
<p><em><strong><span class="smallcaps">Actualizaciones:</span></strong></em></p>
</blockquote></td>
<td></td>
</tr>
<tr class="odd">
<td colspan="4"><blockquote>
<p><strong>DEFINICIÓN DEL PROBLEMA O SITUACIÓN ACTUAL:</strong> <em>DEFINA Y ACOTE EL PROBLEMA QUE SE VA A RESOLVER, DISTINGUIENDO EL PROBLEMA DE SUS CAUSAS, Y DE SUS CONSECUENCIAS.</em></p>
</blockquote></td>
</tr>
<tr class="even">
<td colspan="4"><blockquote>
<p>En campo, cuando una cámara fallaba una y otra vez (foto borrosa u oscura, celular en movimiento o sin respuesta), la repetición no terminaba nunca y bloqueaba el recorrido.</p>
</blockquote></td>
</tr>
</tbody>
</table>

<table>
<tr>
<th colspan="2">DESCRIPCIÓN DETALLADA DEL CAMBIO SOLICITADO: ESPECIFIQUE CON CLARIDAD EL CAMBIO SOLICITADO, PRECISANDO EL QUÉ, QUIÉN, CÓMO, CUÁNDO Y DÓNDE.</th>
</tr>
<tr>
<td colspan="2">Cada cámara se repite como máximo 2 veces y solo la cámara que falló. Mientras haya una repetición pendiente no sale otra secuencia. En el último intento la foto se acepta, se marca «pendiente de revisión técnica» y la IA de la plataforma la analiza. Si la cámara no responde, la secuencia queda parcial y el recorrido continúa.</td>
</tr>
<tr>
<td colspan="2">RAZÓN POR LA QUE SE SOLICITA EL CAMBIO: ESPECIFIQUE CON CLARIDAD PORQUE MOTIVOS O<br>RAZONES SOLICITA EL CAMBIO, PORQUE MOTIVOS ELIGEN ESTE CURSO DE ACCIÓN Y NO OTRO ALTERNATIVO, Y QUÉ SUCEDERÍA SI EL CAMBIO NO SE REALIZA.</td>
</tr>
<tr>
<td colspan="2">Evitar que una cámara con problemas detenga la jornada, sin dejar de exigir la repetición del lado afectado.</td>
</tr>
<tr>
<td colspan="2">Consecuencia de no realizar el cambio: recorridos bloqueados y pérdida de horas de campo.</td>
</tr>
<tr>
<td colspan="2">EFECTOS EN EL PROYECTO: DEFINIR EL EFECTO DEL CAMBIO SOLICITADO A CORTO O LARGO PLAZO EN EL ALCANCE DEL PROYECTO.</td>
</tr>
<tr>
<td>En el corto plazo</td>
<td>En el largo plazo</td>
</tr>
<tr>
<td>Operación: jornadas sin bloqueos.</td>
<td>Mantenimiento: las fotos marcadas sirven para revisar el equipo.</td>
</tr>
<tr>
<td colspan="2">Efectos en otros proyectos, programas, portafolios u operaciones.</td>
</tr>
</table>

| Ninguno. |
|---|
| Efectos extra empresariales en clientes, mercados, proveedores, gobierno,<br>ETC. |
| Ninguno. |

<table>
<tr>
<th colspan="2">Observaciones y Comentarios adicionales.</th>
</tr>
<tr>
<td colspan="2">Requerimientos y documentos afectados: RF03, RN-04. Aplicado en el sistema: 29/10/2026 (app 0.6.1).<br>Decisión de diseño ADR 0012 de la app. Pruebas: CP-A10 y CP-A11.</td>
</tr>
<tr>
<td colspan="2">Revisión del Comité de Control de Cambios.</td>
</tr>
<tr>
<td>Fecha de Revisión</td>
<td>Por programar</td>
</tr>
<tr>
<td>Efectuada Por</td>
<td>Oscar Antonio Gutiérrez Rojas (Project Manager), Jean Pierre Pacheco Tamariz (Asesor Académico) y Jefe de Fundo (Agrícola Riachuelo S.A.C.)</td>
</tr>
<tr>
<td>Resultados de Revisión (APROBADA/RECHAZADA)</td>
<td>PENDIENTE DE REVISIÓN (cambio ya aplicado en el piloto)</td>
</tr>
<tr>
<td>Responsable de Aplicar/Informar</td>
<td>Oscar Manuel Guerrero Figari (Desarrollador) y Grace Lucia Peña Reyes (Analista Funcional).</td>
</tr>
<tr>
<td>Observaciones Especiales</td>
<td>El texto de los requerimientos no se modifica; esta solicitud justifica la diferencia. Actualizar la matriz de trazabilidad y el registro de cambios (Anexo 42) con el resultado de la revisión.</td>
</tr>
</table>

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
<td>Grace Lucia Peña Reyes</td>
<td>Oscar Antonio Gutiérrez Rojas</td>
<td>Comité de Control de Cambios / Jefe de Fundo</td>
<td>14/11/2026</td>
<td>Solicitud formal de cambio: aclaraciones de interpretación de RF06, RNF03 y RNF07.</td>
</tr>
</table>

### SOLICITUD DE CAMBIO Nº06

| Nombre del Proyecto | Siglas del Proyecto | Solicitantes del Cambio |
|---|---|---|
| Sistema inteligente para el monitoreo y detección de plagas en cultivos de vid en Agrícola Riachuelo | SIMDPCVAR | Oscar Manuel Guerrero Figari (Desarrollador), Grace Lucia Peña Reyes (Analista Funcional) y Oscar Antonio Gutiérrez Rojas (Project Manager). |

<table style="width:100%;">
<colgroup>
<col style="width: 45%" />
<col style="width: 4%" />
<col style="width: 44%" />
<col style="width: 5%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="4"><blockquote>
<p><strong><span class="smallcaps">Tipo de Cambio Requerido</span></strong></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><em><strong><span class="smallcaps">Generación de Acción Correctiva:</span></strong></em></p>
</blockquote></td>
<td></td>
<td><blockquote>
<p><em><strong><span class="smallcaps">Reparación de Defecto:</span></strong></em></p>
</blockquote></td>
<td></td>
</tr>
<tr class="even">
<td><blockquote>
<p><em><strong><span class="smallcaps">Generación de Acción Preventiva:</span></strong></em></p>
</blockquote></td>
<td></td>
<td><blockquote>
<p><em><strong><span class="smallcaps">Actualizaciones:</span></strong></em></p>
</blockquote></td>
<td><blockquote>
<p>X</p>
</blockquote></td>
</tr>
<tr class="odd">
<td colspan="4"><blockquote>
<p><strong>DEFINICIÓN DEL PROBLEMA O SITUACIÓN ACTUAL:</strong> <em>DEFINA Y ACOTE EL PROBLEMA QUE SE VA A RESOLVER, DISTINGUIENDO EL PROBLEMA DE SUS CAUSAS, Y DE SUS CONSECUENCIAS.</em></p>
</blockquote></td>
</tr>
<tr class="even">
<td colspan="4"><blockquote>
<p>Tres requerimientos admiten más de una lectura: «zona» en RF06, «menos de 3 pasos» en RNF03 y la mención a expo-secure-store para custodiar imágenes en RNF07 (expo-secure-store es un almacén de claves pequeñas, no de archivos).</p>
</blockquote></td>
</tr>
</tbody>
</table>

<table>
<tr>
<th colspan="2">DESCRIPCIÓN DETALLADA DEL CAMBIO SOLICITADO: ESPECIFIQUE CON CLARIDAD EL CAMBIO SOLICITADO, PRECISANDO EL QUÉ, QUIÉN, CÓMO, CUÁNDO Y DÓNDE.</th>
</tr>
<tr>
<td colspan="2">RF06: «zona» equivale al segmento de la hilera (10 segmentos fijos de ≈20 m por hilera, con un marcador al inicio de cada uno).</td>
</tr>
<tr>
<td colspan="2">RNF03: iniciar el recorrido son 2 toques (Nueva sesión → Crear sesión); la vinculación por QR y la prueba corta son controles exigidos por RF11, no pasos de navegación.</td>
</tr>
<tr>
<td colspan="2">RNF07: las fotos se guardan en el almacenamiento privado de la app (no en la galería) y los metadatos en su base SQLite privada; los tokens y credenciales en expo-secure-store. Nada se borra hasta confirmar la sincronización.</td>
</tr>
<tr>
<td colspan="2">RAZÓN POR LA QUE SE SOLICITA EL CAMBIO: ESPECIFIQUE CON CLARIDAD PORQUE MOTIVOS O<br>RAZONES SOLICITA EL CAMBIO, PORQUE MOTIVOS ELIGEN ESTE CURSO DE ACCIÓN Y NO OTRO ALTERNATIVO, Y QUÉ SUCEDERÍA SI EL CAMBIO NO SE REALIZA.</td>
</tr>
<tr>
<td colspan="2">Dejar por escrito cómo se cumple cada requerimiento sin modificar su texto.</td>
</tr>
<tr>
<td colspan="2">EFECTOS EN EL PROYECTO: DEFINIR EL EFECTO DEL CAMBIO SOLICITADO A CORTO O LARGO PLAZO EN EL ALCANCE DEL PROYECTO.</td>
</tr>
<tr>
<td>En el corto plazo</td>
<td>En el largo plazo</td>
</tr>
<tr>
<td>Sin efecto en el alcance.</td>
<td>Mejora la verificación de los requerimientos.</td>
</tr>
<tr>
<td colspan="2">Efectos en otros proyectos, programas, portafolios u operaciones.</td>
</tr>
</table>

| Ninguno. |
|---|
| Efectos extra empresariales en clientes, mercados, proveedores, gobierno,<br>ETC. |
| Ninguno. |

<table>
<tr>
<th colspan="2">Observaciones y Comentarios adicionales.</th>
</tr>
<tr>
<td colspan="2">Requerimientos y documentos afectados: RF06, RNF03, RNF07. Aplicado en el sistema: 14/11/2026 (documentación).<br>Pruebas: CP-A05, CP-A09, CP-A13 y CP-W19.</td>
</tr>
<tr>
<td colspan="2">Revisión del Comité de Control de Cambios.</td>
</tr>
<tr>
<td>Fecha de Revisión</td>
<td>Por programar</td>
</tr>
<tr>
<td>Efectuada Por</td>
<td>Oscar Antonio Gutiérrez Rojas (Project Manager), Jean Pierre Pacheco Tamariz (Asesor Académico) y Jefe de Fundo (Agrícola Riachuelo S.A.C.)</td>
</tr>
<tr>
<td>Resultados de Revisión (APROBADA/RECHAZADA)</td>
<td>PENDIENTE DE REVISIÓN (cambio ya aplicado en el piloto)</td>
</tr>
<tr>
<td>Responsable de Aplicar/Informar</td>
<td>Oscar Manuel Guerrero Figari (Desarrollador) y Grace Lucia Peña Reyes (Analista Funcional).</td>
</tr>
<tr>
<td>Observaciones Especiales</td>
<td>El texto de los requerimientos no se modifica; esta solicitud justifica la diferencia. Actualizar la matriz de trazabilidad y el registro de cambios (Anexo 42) con el resultado de la revisión.</td>
</tr>
</table>

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
<td>Grace Lucia Peña Reyes</td>
<td>Oscar Antonio Gutiérrez Rojas</td>
<td>Comité de Control de Cambios / Jefe de Fundo</td>
<td>14/11/2026</td>
<td>Solicitud formal de cambio: ajustes de la arquitectura tecnológica (Tabla 30).</td>
</tr>
</table>

### SOLICITUD DE CAMBIO Nº07

| Nombre del Proyecto | Siglas del Proyecto | Solicitantes del Cambio |
|---|---|---|
| Sistema inteligente para el monitoreo y detección de plagas en cultivos de vid en Agrícola Riachuelo | SIMDPCVAR | Oscar Manuel Guerrero Figari (Desarrollador), Grace Lucia Peña Reyes (Analista Funcional) y Oscar Antonio Gutiérrez Rojas (Project Manager). |

<table style="width:100%;">
<colgroup>
<col style="width: 45%" />
<col style="width: 4%" />
<col style="width: 44%" />
<col style="width: 5%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="4"><blockquote>
<p><strong><span class="smallcaps">Tipo de Cambio Requerido</span></strong></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><em><strong><span class="smallcaps">Generación de Acción Correctiva:</span></strong></em></p>
</blockquote></td>
<td></td>
<td><blockquote>
<p><em><strong><span class="smallcaps">Reparación de Defecto:</span></strong></em></p>
</blockquote></td>
<td></td>
</tr>
<tr class="even">
<td><blockquote>
<p><em><strong><span class="smallcaps">Generación de Acción Preventiva:</span></strong></em></p>
</blockquote></td>
<td></td>
<td><blockquote>
<p><em><strong><span class="smallcaps">Actualizaciones:</span></strong></em></p>
</blockquote></td>
<td><blockquote>
<p>X</p>
</blockquote></td>
</tr>
<tr class="odd">
<td colspan="4"><blockquote>
<p><strong>DEFINICIÓN DEL PROBLEMA O SITUACIÓN ACTUAL:</strong> <em>DEFINA Y ACOTE EL PROBLEMA QUE SE VA A RESOLVER, DISTINGUIENDO EL PROBLEMA DE SUS CAUSAS, Y DE SUS CONSECUENCIAS.</em></p>
</blockquote></td>
</tr>
<tr class="even">
<td colspan="4"><blockquote>
<p>La Tabla 30 y algunos diagramas conservan elementos que no llegaron a implementarse después de la SC-01: Supabase Auth, PostGIS, un microservicio de IA con FastAPI, comunicación «React Native P2P» y celulares iPhone.</p>
</blockquote></td>
</tr>
</tbody>
</table>

<table>
<tr>
<th colspan="2">DESCRIPCIÓN DETALLADA DEL CAMBIO SOLICITADO: ESPECIFIQUE CON CLARIDAD EL CAMBIO SOLICITADO, PRECISANDO EL QUÉ, QUIÉN, CÓMO, CUÁNDO Y DÓNDE.</th>
</tr>
<tr>
<td colspan="2">Autenticación: usuarios, roles y tokens JWT propios de Django (Django REST Framework + SimpleJWT).</td>
</tr>
<tr>
<td colspan="2">Datos: Supabase PostgreSQL sin PostGIS; coordenadas en columnas numéricas con validación en la base.</td>
</tr>
<tr>
<td colspan="2">IA: worker de Django con el modelo YOLO exportado a ONNX, alimentado por la cola ai_tasks de la propia base (sin FastAPI ni colas externas).</td>
</tr>
<tr>
<td colspan="2">Red de campo: el controlador abre un servidor local WebSocket/HTTP en la Wi-Fi del equipo.</td>
</tr>
<tr>
<td colspan="2">Equipos: celulares Android (iPhone pendiente de validar).</td>
</tr>
<tr>
<td colspan="2">Servidor: VPS con Docker Compose (contenedores web y worker) detrás de Traefik con HTTPS.</td>
</tr>
<tr>
<td colspan="2">RAZÓN POR LA QUE SE SOLICITA EL CAMBIO: ESPECIFIQUE CON CLARIDAD PORQUE MOTIVOS O<br>RAZONES SOLICITA EL CAMBIO, PORQUE MOTIVOS ELIGEN ESTE CURSO DE ACCIÓN Y NO OTRO ALTERNATIVO, Y QUÉ SUCEDERÍA SI EL CAMBIO NO SE REALIZA.</td>
</tr>
<tr>
<td colspan="2">Menos piezas que mantener, un solo lenguaje en el servidor y uso de los equipos disponibles para el piloto.</td>
</tr>
<tr>
<td colspan="2">EFECTOS EN EL PROYECTO: DEFINIR EL EFECTO DEL CAMBIO SOLICITADO A CORTO O LARGO PLAZO EN EL ALCANCE DEL PROYECTO.</td>
</tr>
<tr>
<td>En el corto plazo</td>
<td>En el largo plazo</td>
</tr>
<tr>
<td>Despliegue: un solo servidor con dos contenedores.</td>
<td>Pendiente: validar la app en iPhone.</td>
</tr>
<tr>
<td colspan="2">Efectos en otros proyectos, programas, portafolios u operaciones.</td>
</tr>
</table>

| Ninguno. |
|---|
| Efectos extra empresariales en clientes, mercados, proveedores, gobierno,<br>ETC. |
| Ninguno. |

<table>
<tr>
<th colspan="2">Observaciones y Comentarios adicionales.</th>
</tr>
<tr>
<td colspan="2">Requerimientos y documentos afectados: Tabla 30, RNF01 y diagramas de arquitectura (complementa la SC-01). Aplicado en el sistema: octubre de 2026.<br>Ver Capítulos VIII (8.9 a 8.13) y XIII.</td>
</tr>
<tr>
<td colspan="2">Revisión del Comité de Control de Cambios.</td>
</tr>
<tr>
<td>Fecha de Revisión</td>
<td>Por programar</td>
</tr>
<tr>
<td>Efectuada Por</td>
<td>Oscar Antonio Gutiérrez Rojas (Project Manager), Jean Pierre Pacheco Tamariz (Asesor Académico) y Jefe de Fundo (Agrícola Riachuelo S.A.C.)</td>
</tr>
<tr>
<td>Resultados de Revisión (APROBADA/RECHAZADA)</td>
<td>PENDIENTE DE REVISIÓN (cambio ya aplicado en el piloto)</td>
</tr>
<tr>
<td>Responsable de Aplicar/Informar</td>
<td>Oscar Manuel Guerrero Figari (Desarrollador) y Grace Lucia Peña Reyes (Analista Funcional).</td>
</tr>
<tr>
<td>Observaciones Especiales</td>
<td>El texto de los requerimientos no se modifica; esta solicitud justifica la diferencia. Actualizar la matriz de trazabilidad y el registro de cambios (Anexo 42) con el resultado de la revisión.</td>
</tr>
</table>

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
<td>Grace Lucia Peña Reyes</td>
<td>Oscar Antonio Gutiérrez Rojas</td>
<td>Comité de Control de Cambios / Jefe de Fundo</td>
<td>14/11/2026</td>
<td>Solicitud formal de cambio: aprobación de cuentas de la app sin elegir rol.</td>
</tr>
</table>

### SOLICITUD DE CAMBIO Nº08

| Nombre del Proyecto | Siglas del Proyecto | Solicitantes del Cambio |
|---|---|---|
| Sistema inteligente para el monitoreo y detección de plagas en cultivos de vid en Agrícola Riachuelo | SIMDPCVAR | Oscar Manuel Guerrero Figari (Desarrollador), Grace Lucia Peña Reyes (Analista Funcional) y Oscar Antonio Gutiérrez Rojas (Project Manager). |

<table style="width:100%;">
<colgroup>
<col style="width: 45%" />
<col style="width: 4%" />
<col style="width: 44%" />
<col style="width: 5%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="4"><blockquote>
<p><strong><span class="smallcaps">Tipo de Cambio Requerido</span></strong></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><em><strong><span class="smallcaps">Generación de Acción Correctiva:</span></strong></em></p>
</blockquote></td>
<td></td>
<td><blockquote>
<p><em><strong><span class="smallcaps">Reparación de Defecto:</span></strong></em></p>
</blockquote></td>
<td>X</td>
</tr>
<tr class="even">
<td><blockquote>
<p><em><strong><span class="smallcaps">Generación de Acción Preventiva:</span></strong></em></p>
</blockquote></td>
<td></td>
<td><blockquote>
<p><em><strong><span class="smallcaps">Actualizaciones:</span></strong></em></p>
</blockquote></td>
<td></td>
</tr>
<tr class="odd">
<td colspan="4"><blockquote>
<p><strong>DEFINICIÓN DEL PROBLEMA O SITUACIÓN ACTUAL:</strong> <em>DEFINA Y ACOTE EL PROBLEMA QUE SE VA A RESOLVER, DISTINGUIENDO EL PROBLEMA DE SUS CAUSAS, Y DE SUS CONSECUENCIAS.</em></p>
</blockquote></td>
</tr>
<tr class="even">
<td colspan="4"><blockquote>
<p>Al aprobar en la web una cuenta pedida desde la app, la pantalla ofrecía los cuatro roles, incluidos los de la web (Administrador, Especialista fitosanitario y Supervisor). Un operador de campo podía quedar con un rol que no le corresponde.</p>
</blockquote></td>
</tr>
</tbody>
</table>

<table>
<tr>
<th colspan="2">DESCRIPCIÓN DETALLADA DEL CAMBIO SOLICITADO: ESPECIFIQUE CON CLARIDAD EL CAMBIO SOLICITADO, PRECISANDO EL QUÉ, QUIÉN, CÓMO, CUÁNDO Y DÓNDE.</th>
</tr>
<tr>
<td colspan="2">La aprobación ya no pide rol: la cuenta queda como «Operador de campo». El editor de roles solo aparece en las cuentas de la web y solo ofrece Especialista, Supervisor y Administrador. Las cuentas de la web se siguen creando únicamente con «Usuarios → Nueva cuenta».</td>
</tr>
<tr>
<td colspan="2">RAZÓN POR LA QUE SE SOLICITA EL CAMBIO: ESPECIFIQUE CON CLARIDAD PORQUE MOTIVOS O<br>RAZONES SOLICITA EL CAMBIO, PORQUE MOTIVOS ELIGEN ESTE CURSO DE ACCIÓN Y NO OTRO ALTERNATIVO, Y QUÉ SUCEDERÍA SI EL CAMBIO NO SE REALIZA.</td>
</tr>
<tr>
<td colspan="2">Mantener separadas las cuentas de campo y de oficina protege la auditoría y el control de roles (RNF05).</td>
</tr>
<tr>
<td colspan="2">Consecuencia de no realizar el cambio: riesgo de dar permisos de la web a un operador por error.</td>
</tr>
<tr>
<td colspan="2">EFECTOS EN EL PROYECTO: DEFINIR EL EFECTO DEL CAMBIO SOLICITADO A CORTO O LARGO PLAZO EN EL ALCANCE DEL PROYECTO.</td>
</tr>
<tr>
<td>En el corto plazo</td>
<td>En el largo plazo</td>
</tr>
<tr>
<td>Se elimina el error de configuración (pruebas automáticas actualizadas).</td>
<td>Ninguno.</td>
</tr>
<tr>
<td colspan="2">Efectos en otros proyectos, programas, portafolios u operaciones.</td>
</tr>
</table>

| Ninguno. |
|---|
| Efectos extra empresariales en clientes, mercados, proveedores, gobierno,<br>ETC. |
| Ninguno. |

<table>
<tr>
<th colspan="2">Observaciones y Comentarios adicionales.</th>
</tr>
<tr>
<td colspan="2">Requerimientos y documentos afectados: RNF05, HU05. Aplicado en el sistema: 14/11/2026 (web v1.6.1).<br>Decisión de diseño ADR-W-014. Pruebas: CP-W35, CP-W36 y CP-A21.</td>
</tr>
<tr>
<td colspan="2">Revisión del Comité de Control de Cambios.</td>
</tr>
<tr>
<td>Fecha de Revisión</td>
<td>Por programar</td>
</tr>
<tr>
<td>Efectuada Por</td>
<td>Oscar Antonio Gutiérrez Rojas (Project Manager), Jean Pierre Pacheco Tamariz (Asesor Académico) y Jefe de Fundo (Agrícola Riachuelo S.A.C.)</td>
</tr>
<tr>
<td>Resultados de Revisión (APROBADA/RECHAZADA)</td>
<td>PENDIENTE DE REVISIÓN (cambio ya aplicado en el piloto)</td>
</tr>
<tr>
<td>Responsable de Aplicar/Informar</td>
<td>Oscar Manuel Guerrero Figari (Desarrollador) y Grace Lucia Peña Reyes (Analista Funcional).</td>
</tr>
<tr>
<td>Observaciones Especiales</td>
<td>El texto de los requerimientos no se modifica; esta solicitud justifica la diferencia. Actualizar la matriz de trazabilidad y el registro de cambios (Anexo 42) con el resultado de la revisión.</td>
</tr>
</table>

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
<td>Grace Lucia Peña Reyes</td>
<td>Oscar Antonio Gutiérrez Rojas</td>
<td>Comité de Control de Cambios / Jefe de Fundo</td>
<td>14/11/2026</td>
<td>Solicitud formal de cambio: funciones agregadas al alcance durante el piloto.</td>
</tr>
</table>

### SOLICITUD DE CAMBIO Nº09

| Nombre del Proyecto | Siglas del Proyecto | Solicitantes del Cambio |
|---|---|---|
| Sistema inteligente para el monitoreo y detección de plagas en cultivos de vid en Agrícola Riachuelo | SIMDPCVAR | Oscar Manuel Guerrero Figari (Desarrollador), Grace Lucia Peña Reyes (Analista Funcional) y Oscar Antonio Gutiérrez Rojas (Project Manager). |

<table style="width:100%;">
<colgroup>
<col style="width: 45%" />
<col style="width: 4%" />
<col style="width: 44%" />
<col style="width: 5%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="4"><blockquote>
<p><strong><span class="smallcaps">Tipo de Cambio Requerido</span></strong></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><em><strong><span class="smallcaps">Generación de Acción Correctiva:</span></strong></em></p>
</blockquote></td>
<td></td>
<td><blockquote>
<p><em><strong><span class="smallcaps">Reparación de Defecto:</span></strong></em></p>
</blockquote></td>
<td></td>
</tr>
<tr class="even">
<td><blockquote>
<p><em><strong><span class="smallcaps">Generación de Acción Preventiva:</span></strong></em></p>
</blockquote></td>
<td></td>
<td><blockquote>
<p><em><strong><span class="smallcaps">Actualizaciones:</span></strong></em></p>
</blockquote></td>
<td><blockquote>
<p>X</p>
</blockquote></td>
</tr>
<tr class="odd">
<td colspan="4"><blockquote>
<p><strong>DEFINICIÓN DEL PROBLEMA O SITUACIÓN ACTUAL:</strong> <em>DEFINA Y ACOTE EL PROBLEMA QUE SE VA A RESOLVER, DISTINGUIENDO EL PROBLEMA DE SUS CAUSAS, Y DE SUS CONSECUENCIAS.</em></p>
</blockquote></td>
</tr>
<tr class="even">
<td colspan="4"><blockquote>
<p>Durante el piloto aparecieron necesidades no escritas como requerimiento: llegar a pie hasta la planta con plaga, quitar fotos de prueba o equivocadas, ubicar mejor cada foto en la hilera y ayudar a los usuarios y evaluadores.</p>
</blockquote></td>
</tr>
</tbody>
</table>

<table>
<tr>
<th colspan="2">DESCRIPCIÓN DETALLADA DEL CAMBIO SOLICITADO: ESPECIFIQUE CON CLARIDAD EL CAMBIO SOLICITADO, PRECISANDO EL QUÉ, QUIÉN, CÓMO, CUÁNDO Y DÓNDE.</th>
</tr>
<tr>
<td colspan="2">«Ubicar plaga» en la app (mapa satelital, distancia y «Cómo llegar» con Google Maps).</td>
</tr>
<tr>
<td colspan="2">Papelera de fotos (30 días, con «Restaurar») y limpieza controlada, solo para el administrador.</td>
</tr>
<tr>
<td colspan="2">10 segmentos fijos por hilera con marcadores.</td>
</tr>
<tr>
<td colspan="2">Ayuda en cada pantalla y manuales en PDF dentro de la web; cabeceras de seguridad y auditoría con contexto.</td>
</tr>
<tr>
<td colspan="2">RAZÓN POR LA QUE SE SOLICITA EL CAMBIO: ESPECIFIQUE CON CLARIDAD PORQUE MOTIVOS O<br>RAZONES SOLICITA EL CAMBIO, PORQUE MOTIVOS ELIGEN ESTE CURSO DE ACCIÓN Y NO OTRO ALTERNATIVO, Y QUÉ SUCEDERÍA SI EL CAMBIO NO SE REALIZA.</td>
</tr>
<tr>
<td colspan="2">Facilitan la operación y la evaluación sin cambiar ningún requerimiento.</td>
</tr>
<tr>
<td colspan="2">EFECTOS EN EL PROYECTO: DEFINIR EL EFECTO DEL CAMBIO SOLICITADO A CORTO O LARGO PLAZO EN EL ALCANCE DEL PROYECTO.</td>
</tr>
<tr>
<td>En el corto plazo</td>
<td>En el largo plazo</td>
</tr>
<tr>
<td>Mejor uso en campo y menos fotos inservibles en la nube.</td>
<td>Más funciones que mantener y documentar.</td>
</tr>
<tr>
<td colspan="2">Efectos en otros proyectos, programas, portafolios u operaciones.</td>
</tr>
</table>

| Ninguno. |
|---|
| Efectos extra empresariales en clientes, mercados, proveedores, gobierno,<br>ETC. |
| Ninguno. |

<table>
<tr>
<th colspan="2">Observaciones y Comentarios adicionales.</th>
</tr>
<tr>
<td colspan="2">Requerimientos y documentos afectados: RF06, RF10, CU8, CU9 (sin modificar su texto). Aplicado en el sistema: octubre y noviembre de 2026.<br>Decisiones de diseño ADR-W-007, ADR-W-010, ADR-W-012 y ADR-W-013.</td>
</tr>
<tr>
<td colspan="2">Revisión del Comité de Control de Cambios.</td>
</tr>
<tr>
<td>Fecha de Revisión</td>
<td>Por programar</td>
</tr>
<tr>
<td>Efectuada Por</td>
<td>Oscar Antonio Gutiérrez Rojas (Project Manager), Jean Pierre Pacheco Tamariz (Asesor Académico) y Jefe de Fundo (Agrícola Riachuelo S.A.C.)</td>
</tr>
<tr>
<td>Resultados de Revisión (APROBADA/RECHAZADA)</td>
<td>PENDIENTE DE REVISIÓN (cambio ya aplicado en el piloto)</td>
</tr>
<tr>
<td>Responsable de Aplicar/Informar</td>
<td>Oscar Manuel Guerrero Figari (Desarrollador) y Grace Lucia Peña Reyes (Analista Funcional).</td>
</tr>
<tr>
<td>Observaciones Especiales</td>
<td>El texto de los requerimientos no se modifica; esta solicitud justifica la diferencia. Actualizar la matriz de trazabilidad y el registro de cambios (Anexo 42) con el resultado de la revisión.</td>
</tr>
</table>
