# Anexo 9 (AP-09): Documentación de Requisitos

Nota. Este anexo conserva los requisitos con la codificación y la redacción con que se levantaron al inicio del proyecto. Los requerimientos vigentes son los de las Tablas 27, 28 y 29 del Capítulo VIII (RF01–RF11, RNF01–RNF07 y RN-01–RN-06); su cumplimiento en el sistema implementado se verifica con las pruebas de la «Documentación del sistema y guía de pruebas» y las diferencias se justifican con las solicitudes de cambio del Anexo 37.

<table style="width:100%;">
<colgroup>
<col style="width: 12%" />
<col style="width: 13%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 11%" />
<col style="width: 30%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="6"><blockquote>
<p>CONTROL DE VERSIONES</p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><em><strong>Versión</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Hecha por</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Revisada por</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Aprobada por</strong></em></p>
</blockquote></td>
<td><em><strong>Fecha</strong></em></td>
<td><blockquote>
<p><em><strong>Motivo</strong></em></p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>1.1</p>
</blockquote></td>
<td><blockquote>
<p>Equipo del proyecto</p>
</blockquote></td>
<td><blockquote>
<p>Oscar Antonio Gutiérrez Rojas</p>
</blockquote></td>
<td><blockquote>
<p>Pendiente de aprobación formal</p>
</blockquote></td>
<td>30/10/2026</td>
<td><blockquote>
<p>Desarrollo del anexo a partir del informe del curso</p>
</blockquote></td>
</tr>
</tbody>
</table>

| Nombre del proyecto | Siglas del Proyecto |
|---|---|
| Sistema inteligente para el monitoreo y detección de plagas en cultivos de vid en Agrícola Riachuelo | SIMDPCVAR |

<table>
<tr>
<th colspan="4">I. REQUISITOS DEL NEGOCIO: DESCRIBIR LAS NECESIDADES DE ALTO NIVEL DE LA ORGANIZACIÓN; TALES COMO OPORTUNIDADES DE NEGOCIO Y RAZONES POR LAS QUE SE HA EMPRENDIDO EL PROYECTO.</th>
</tr>
<tr>
<td>Código del<br>REQUISITO</td>
<td>Descripción del Requisito</td>
<td>Fuente</td>
<td>Prioridad</td>
</tr>
<tr>
<td>RN-01</td>
<td>Reducir desarticulación del seguimiento fitosanitario</td>
<td>AS-IS / organización</td>
<td>Alta</td>
</tr>
<tr>
<td>RN-02</td>
<td>Mejorar localización y consulta de hallazgos</td>
<td>Cap. I-IV</td>
<td>Alta</td>
</tr>
<tr>
<td>RN-03</td>
<td>Medir mejora de eficiencia y trazabilidad</td>
<td>Diseño O1-X-O2</td>
<td>Alta</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td colspan="4">II. REQUISITOS DE LOS INTERESADOS: DESCRIBIR DETALLADAMENTE LAS NECESIDADES DE LOS INTERESADOS O GRUPO DE<br>INTERESADOS.<br>![](../media/image51.png)</td>
</tr>
<tr>
<td>Código del<br>REQUISITO</td>
<td>Descripción del Requisito</td>
<td>Fuente</td>
<td>Prioridad</td>
</tr>
<tr>
<td>RI-01</td>
<td>Captura ágil en campo sin conectividad estable</td>
<td>Operador</td>
<td>Alta</td>
</tr>
<tr>
<td>RI-02</td>
<td>Confirmación fitosanitaria por especialista</td>
<td>Especialista</td>
<td>Alta</td>
</tr>
<tr>
<td>RI-03</td>
<td>Seguimiento georreferenciado y alertas</td>
<td>Jefe de Fundo</td>
<td>Alta</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td colspan="4">III. REQUISITOS DE SOLUCIONES: DESCRIBIR LAS FUNCIONES Y CARACTERÍSTICAS DEL PRODUCTO, SERVICIO O RESULTADO, QUE SATISFAGA LOS REQUERIMIENTOS DEL NEGOCIO Y DE LOS INTERESADOS.</td>
</tr>
<tr>
<td colspan="4">3.1 REQUISITOS FUNCIONALES: DESCRIBIR EL FUNCIONAMIENTO DEL PRODUCTO. SE PUEDE INCLUIR ACCIONES, PROCESOS, DATOS E INTERACCIONES QUE EL PRODUCTO DEBE EJECUTAR.</td>
</tr>
<tr>
<td>Código del<br>REQUISITO</td>
<td>Descripción del Requisito</td>
<td>Fuente</td>
<td>Prioridad</td>
</tr>
<tr>
<td>RF-01</td>
<td>Gestionar sesiones y recorridos de monitoreo</td>
<td>Operador / supervisor</td>
<td>Alta</td>
</tr>
<tr>
<td>RF-02</td>
<td>Capturar fotografías bilaterales de una unidad de monitoreo</td>
<td>Operador</td>
<td>Alta</td>
</tr>
<tr>
<td>RF-03</td>
<td>Guardar lote, hilera, zona, lado y marcador</td>
<td>Operador / especialista</td>
<td>Alta</td>
</tr>
<tr>
<td>RF-04</td>
<td>Validar nitidez, exposición y movimiento localmente</td>
<td>Aplicación móvil</td>
<td>Alta</td>
</tr>
<tr>
<td colspan="4">3.2 REQUISITOS NO FUNCIONALES: DESCRIBIR LAS CONDICIONES O CUALIDADES AMBIENTALES REQUERIDAS PARA QUE EL producto sea efectivo. Se puede incluir nivel de servicio, capacidad de soporte, fiabilidad, seguridad, rendimiento, etc.</td>
</tr>
<tr>
<td>Código del<br>REQUISITO</td>
<td>Descripción del Requisito</td>
<td>Fuente</td>
<td>Prioridad</td>
</tr>
</table>

<table style="width:100%;">
<colgroup>
<col style="width: 17%" />
<col style="width: 32%" />
<col style="width: 25%" />
<col style="width: 24%" />
</colgroup>
<thead>
<tr class="header">
<th><blockquote>
<p>RNF-01</p>
</blockquote></th>
<th><blockquote>
<p>Disponibilidad de captura local con conectividad limitada</p>
</blockquote></th>
<th><blockquote>
<p>Operador</p>
</blockquote></th>
<th><blockquote>
<p>Alta</p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p>RNF-02</p>
</blockquote></td>
<td><blockquote>
<p>Validación rápida de calidad de imágenes</p>
</blockquote></td>
<td><blockquote>
<p>Aplicación móvil</p>
</blockquote></td>
<td><blockquote>
<p>Alta</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>RNF-03</p>
</blockquote></td>
<td><blockquote>
<p>Autenticación y transmisión segura</p>
</blockquote></td>
<td><blockquote>
<p>Administrador</p>
</blockquote></td>
<td><blockquote>
<p>Alta</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>RNF-04</p>
</blockquote></td>
<td><blockquote>
<p>Persistencia segura de fotos y metadatos</p>
</blockquote></td>
<td><blockquote>
<p>Administrador</p>
</blockquote></td>
<td><blockquote>
<p>Alta</p>
</blockquote></td>
</tr>
</tbody>
</table>

<table>
<tr>
<th colspan="4">IV. REQUISITOS DE TRANSICIÓN Y PREPARACIÓN: DESCRIBIR LAS CAPACIDADES TEMPORALES; TALES COMO LA<br>CONVERSIÓN DE DATOS Y REQUISITOS DE ENTRENAMIENTO, NECESARIOS PARA LA TRANSICIÓN DEL ESTADO ACTUAL AL ESTADO FUTURO DESEADO.</th>
</tr>
<tr>
<td>Código del<br>REQUISITO</td>
<td>Descripción del Requisito</td>
<td>Fuente</td>
<td>Prioridad</td>
</tr>
<tr>
<td>RT-01</td>
<td>Capacitación y validación en campo</td>
<td>Equipo de proyecto</td>
<td>Alta</td>
</tr>
<tr>
<td>RT-02</td>
<td>Transferencia de roles y acceso web</td>
<td>Jefe de Fundo</td>
<td>Media</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td colspan="4">V. REQUISITOS DEL PROYECTO: DESCRIBIR LAS ACCIONES, PROCESOS U OTRAS CONDICIONES QUE SE NECESITAN PARA CUMPLIR EL PROYECTO. SE PUEDE INCLUIR FECHAS DE HITOS, OBLIGACIONES CONTRACTUALES, RESTRICCIONES, ETC.</td>
</tr>
<tr>
<td>Código del<br>REQUISITO</td>
<td>Descripción del Requisito</td>
<td>Fuente</td>
<td>Prioridad</td>
</tr>
<tr>
<td>RP-01</td>
<td>Cierre de seis sprints y evaluación piloto</td>
<td>Cronograma</td>
<td>Alta</td>
</tr>
<tr>
<td>RP-02</td>
<td>Control de costo S/ 6,845 y alcance</td>
<td>Presupuesto</td>
<td>Alta</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td colspan="4">VI. REQUISITOS DE CALIDAD: DESCRIBIR LAS CONDICIONES O CRITERIOS NECESARIOS PARA VALIDAR EL ÉXITO DEL PROYECTO entregado, o el cumplimiento de otros requisitos del proyecto. Se puede incluir pruebas, certificaciones, validaciones, etc.</td>
</tr>
<tr>
<td>Código del<br>REQUISITO<br>![](../media/image51.png)</td>
<td>Descripción del Requisito</td>
<td>Fuente</td>
<td>Prioridad</td>
</tr>
<tr>
<td>RC-01</td>
<td>Precisión IA ≥85%, falsos negativos ≤5%</td>
<td>Plan de calidad</td>
<td>Alta</td>
</tr>
<tr>
<td>RC-02</td>
<td>Sincronización 100% sin pérdida de metadatos</td>
<td>Plan de calidad</td>
<td>Alta</td>
</tr>
<tr>
<td>RC-03</td>
<td>API ≤1.5 s; plataforma 99.5%</td>
<td>Plan de calidad</td>
<td>Alta</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
