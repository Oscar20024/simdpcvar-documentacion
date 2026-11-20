# CAPÍTULO V. INICIACIÓN DEL PROYECTO

## 5.1. Acta de Constitución

*Tabla 21. Acta de Constitución*

| Nombre del Proyecto | Siglas del Proyecto |
|---|---|
| Sistema inteligente para el monitoreo y detección de plagas en cultivos de vid en Agrícola Riachuelo. | SIMDPCVAR |

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
<td>0.1</td>
<td>P, J,Z</td>
<td>G</td>
<td>G</td>
<td>01/09/26</td>
<td>Versión Original.</td>
</tr>
</table>

Project Charter

**DESCRIPCIÓN DEL PROYECTO: ¿QUÉ, QUIÉN, CÓMO, CUÁNDO Y DÓNDE?**

**El proyecto consiste en diseñar, construir y validar en campo un sistema inteligente de monitoreo y detección de plagas para los cultivos de vid de Agrícola Riachuelo S.A.C., ubicada en el distrito de La Tinguiña, provincia y región Ica.<br>El sistema estará compuesto por un prototipo vestible con dos dispositivos móviles para la captura fotográfica bilateral de cada planta, una aplicación móvil que valida la calidad de las imágenes y las almacena sin conexión, un modelo de inteligencia artificial que detecta indicios visuales de plaga, y una plataforma web centralizada donde el especialista fitosanitario confirma los hallazgos y se generan alertas automáticas por WhatsApp.<br>El desarrollo estará a cargo del siguiente equipo:<br>-Gutiérrez Rojas, Oscar Antonio -> Project Manager / Líder de arquitectura.**

| -Peña Reyes, Grace Lucia -> Analista funcional / Scrum Master.<br>-Zuñiga Matos, María Fernanda -> Analista técnica / Responsable de hardware.<br>-Guerrero Figari, Oscar Manuel -> Desarrollador de backend, plataforma web e inteligencia artificial.<br>El proyecto se ejecutará bajo un enfoque híbrido: gestión predictiva para costos, riesgos y adquisiciones, y marco ágil Scrum (Sprints de dos semanas) para el desarrollo del software, el modelo de IA y la construcción del prototipo.<br>El proyecto se desarrollará desde el 01/09/2026 hasta el 20/11/2026 (tres meses, seis Sprints), con actividades de análisis y desarrollo en la UTP (Ica) y pruebas piloto en las instalaciones y campos de cultivo de Agrícola Riachuelo, en la carretera a Los Molinos, La Tinguiña, Ica. |
|---|
| DEFINICIÓN DEL PRODUCTO DEL PROYECTO: DESCRIPCIÓN DEL<br>PRODUCTO, SERVICIO O CAPACIDAD A GENERAR |
| El producto final estará compuesto por cuatro componentes integrados:<br>1. Prototipo vestible de captura bilateral: mochila ergonómica con soportes ajustables para dos dispositivos móviles que fotografían simultáneamente los lados izquierdo y derecho de cada planta, y un tercer dispositivo de control de sesión.<br>2. Aplicación móvil (iOS) de captura offline: valida en tiempo real la nitidez, exposición y ausencia de movimiento de cada fotografía, almacena localmente cuando no hay conexión y sincroniza automáticamente al restablecerse la señal. Modelo de inteligencia artificial: entrenado para detectar indicios visuales de chanchito en racimos y hojas, generando “indicios sugeridos” no vinculantes hasta la validación del especialista fitosanitario.<br>3. Plataforma web centralizada: gestión de casos, registro de tratamientos, trazabilidad histórica y envío de alertas automáticas por WhatsApp.<br>4. Documentación a entregar: manual de usuario de la aplicación móvil, manual técnico de la plataforma, especificación de casos de uso e informe final de resultados del piloto. |

<table>
<tr>
<th colspan="3">DEFINICIÓN DE REQUISITOS DEL PROYECTO: REQUERIMIENTOS FUNCIONALES, NO FUNCIONALES Y DE CALIDAD</th>
</tr>
<tr>
<td colspan="3">El patrocinador operativo, el Jefe de Fundo, requiere:<br>-Reducir la dependencia de 10 a 20 operadores que recorren manualmente más de 52,900 plantas para el registro de hallazgos.<br>-Contar con evidencia fotográfica trazable y georreferenciada de cada hallazgo fitosanitario.<br>-Disminuir el tiempo de respuesta entre la detección de un indicio y la aplicación del tratamiento correspondiente.<br>-Mantener el cumplimiento de los estándares fitosanitarios exigidos por SENASA y las certificaciones de exportación, sin reemplazar la validación del especialista fitosanitario.</td>
</tr>
<tr>
<td colspan="3">OBJETIVOS DEL PROYECTO: METAS EN TÉRMINOS DE LA TRIPLE RESTRICCIÓN</td>
</tr>
<tr>
<td>Concepto</td>
<td>Objetivo</td>
<td>Criterio de éxito</td>
</tr>
<tr>
<td>1. Alcance</td>
<td>Entregar elprototipo vestible, la aplicación móvil, el modelo de IA y la plataforma web funcionando de manera integrada.</td>
<td>Aprobación del piloto por el Jefe de Fundo y elEspecialista Fitosanitario, y sustentación aprobada ante el docente del curso.</td>
</tr>
<tr>
<td>2. Tiempo</td>
<td>Concluir el proyecto en el plazo académico establecido.</td>
<td>Finalizar dentro del periodo comprendido entre el 01/09/2026 y el 20/11/2026, cumpliendo los seis Sprints planificados.</td>
</tr>
<tr>
<td>3. Costo</td>
<td>Cumplir con el presupuesto preliminar estimado para hardware,</td>
<td>No exceder el presupuesto total aprobado, incluyendo la reserva de contingencia.</td>
</tr>
</table>

<table>
<tr>
<th></th>
<th>servicios en la nube y el piloto en campo.</th>
<th></th>
</tr>
<tr>
<td colspan="3">FINALIDAD DEL PROYECTO: PROPÓSITO GENERAL U OBJETIVO DE NIVEL SUPERIOR</td>
</tr>
<tr>
<td colspan="3">Fortalecer la sanidad del cultivo y la trazabilidad del proceso de monitoreo fitosanitario de Agrícola Riachuelo, sosteniendo el cumplimiento de las certificaciones de exportación y contribuyendo a la visión de la empresa de destacar como agroexportadora mediante la innovación tecnológica en el manejo fitosanitario de sus cultivos.</td>
</tr>
<tr>
<td colspan="3">JUSTIFICACIÓN DEL PROYECTO: MOTIVOS QUE JUSTIFICAN SU EJECUCIÓN</td>
</tr>
<tr>
<td colspan="2">Justificación cualitativa</td>
<td>Justificación cuantitativa</td>
</tr>
<tr>
<td colspan="2">• Reduce el error humano enel registro visual de hallazgos.<br>• Genera evidencia fotográfica y georreferenciada trazable de cada planta inspeccionada.<br>• Agiliza la respuesta ante hallazgos críticos mediante alertas automáticas.<br>• Reemplaza el archivo Excel de 12 hojas y más de 400 columnas por una plataforma centralizadayescalable.</td>
<td>• Reducción estimada dehoras-persona invertidas en el recorrido manual del viñedo frente al esquema actual de 10 a 20 operadores.<br>• Disminución esperadade inconsistencias de registro respecto al margen de error actual detectado en el Excelmaestro.<br>• Por tratarse de un proyecto académico piloto, no se calculan indicadores financieros formales (VAN, TIR); el análisis se limita a costos operativos referenciales del piloto.</td>
</tr>
<tr>
<td colspan="3">DESIGNACIÓN DEL PROJECT MANAGER DEL PROYECTO</td>
</tr>
</table>

<table>
<tr>
<th>Nombre</th>
<th colspan="2">Gutiérrez Rojas, Oscar Antonio</th>
<th>Project Manager</th>
</tr>
<tr>
<td>Reporta a</td>
<td colspan="2">Jefe de Fundo (patrocinador operativo).</td>
<td>Coordinar con el especialista fitosanitario los criterios de validación y exigir el cumplimiento de los Sprints establecidos.</td>
</tr>
<tr>
<td>Supervisa a</td>
<td colspan="2">Equipo del proyecto:Peña Reyes, Grace Lucia; Zuñiga Matos, María Fernanda; Guerrero Figari, Oscar Manuel.</td>
<td>Asignar tareas delProduct Backlog y gestionar los impedimentos técnicos del equipo.</td>
</tr>
<tr>
<td colspan="4">CRONOGRAMA DE HITOS DEL PROYECTO</td>
</tr>
<tr>
<td colspan="2">Hito o evento significativo</td>
<td colspan="2">Fecha programada</td>
</tr>
<tr>
<td colspan="2">1. Gestión del proyecto (Sprint 0).</td>
<td colspan="2">01/09/2026 al 20/11/2026</td>
</tr>
<tr>
<td colspan="2">2. Iniciación y arquitectura (Sprint 1).</td>
<td colspan="2">01/09/2026 al 12/09/2026</td>
</tr>
<tr>
<td colspan="2">3. Ensamblaje y calibración del prototipo vestible (Sprint 2).</td>
<td colspan="2">14/09/2026 al 25/09/2026</td>
</tr>
<tr>
<td colspan="2">4. Entrenamiento del modelo de inteligencia artificial (Sprint 3).</td>
<td colspan="2">28/09/2026 al 09/10/2026</td>
</tr>
<tr>
<td colspan="2">5. Desarrollo de la plataforma web y módulo cartográfico (Sprint 4).</td>
<td colspan="2">12/10/2026 al 23/10/2026</td>
</tr>
<tr>
<td colspan="2">6. Integración, sincronización y pasarela de alertas (Sprint 5).</td>
<td colspan="2">26/10/2026 al 06/11/2026</td>
</tr>
<tr>
<td colspan="2">7. Piloto en campo, métricas comparativas y cierre (Sprint 6).</td>
<td colspan="2">09/11/2026 al 20/11/2026</td>
</tr>
</table>

<table>
<tr>
<th colspan="2">ORGANIZACIONES O GRUPOS ORGANIZACIONALES QUE INTERVIENEN EN EL PROYECTO</th>
</tr>
<tr>
<td>Organización o grupo</td>
<td>Rol que desempeña</td>
</tr>
<tr>
<td>Gerencia General de Agrícola Riachuelo S.A.C.</td>
<td>Autoriza y financia el desarrollo del piloto en campo.</td>
</tr>
<tr>
<td>Jefe de Fundo (Agrícola Riachuelo).</td>
<td>Patrocinador operativo; coordina al personal de campo durante el piloto.</td>
</tr>
<tr>
<td>Especialista fitosanitario (Agrónomo).</td>
<td>Valida los hallazgos y aprueba los criterios deaceptaciónfuncional.</td>
</tr>
<tr>
<td>Equipo de proyecto (estudiantes UTP).</td>
<td>Desarrolla el software, el modelo de IA, la plataforma web y ensambla el prototipo.</td>
</tr>
</table>

| PRINCIPALES AMENAZAS DEL PROYECTO (RIESGOS NEGATIVOS) |
|---|
| Conectividad limitada o nula a internet en sectores alejados del fundo, que podría retrasar la sincronización de evidencia. |
| Resistencia al cambio por parte de los operadores habituados al registro manual en Excel. |
| Falsos positivos o baja precisión inicial del modelo de visión artificial durante las primeras semanas del piloto. |

<table>
<tr>
<th>PRINCIPALES POSITIVOS)</th>
<th>OPORTUNIDADES</th>
<th>DEL</th>
<th>PROYECTO</th>
<th>(RIESGOS</th>
</tr>
<tr>
<td colspan="5">Fortalecimiento de la posición de Agrícola Riachuelo frente a certificadoras y clientes/importadores gracias a la trazabilidad digital del monitoreo.</td>
</tr>
</table>

**Posibilidad de escalar el prototipo a otros fundos o cultivos de la organización si el piloto resulta exitoso.**

<table>
<tr>
<th colspan="2">PRESUPUESTO PRELIMINAR DEL PROYECTO</th>
</tr>
<tr>
<td>Concepto</td>
<td>Monto estimado (S/)</td>
</tr>
<tr>
<td>1. Dispositivos móviles y componentes del prototipo vestible.</td>
<td>S/ 4,200.00</td>
</tr>
<tr>
<td>2. Servicios administrados en la nube (almacenamiento, cómputo y API de mensajería).</td>
<td>S/ 1,100.00</td>
</tr>
<tr>
<td>3. Materiales para ensamblaje de la mochila (arneses, soportes, tornillería).</td>
<td>S/ 650.00</td>
</tr>
<tr>
<td>Total línea base</td>
<td>S/ 5,950.00</td>
</tr>
<tr>
<td>4. Reserva de contingencia.</td>
<td>S/ 595.00</td>
</tr>
<tr>
<td>5. Reserva de gestión.</td>
<td>S/ 300.00</td>
</tr>
<tr>
<td>Total presupuesto (referencial, proyecto académico)</td>
<td>S/ 6,845.00</td>
</tr>
</table>

<table>
<tr>
<th colspan="4">SPONSOR QUE AUTORIZA EL PROYECTO</th>
</tr>
<tr>
<td>Nombre</td>
<td>Empresa</td>
<td>Cargo</td>
<td>Fecha</td>
</tr>
<tr>
<td>Pedro Jesús Palacios Chanco (Jefe de Fundo)</td>
<td>Agrícola Riachuelo S.A.C.</td>
<td>Patrocinador operativo del piloto</td>
<td>01/09/2026</td>
</tr>
</table>

*Fuente: Elaboración propia.*

## 5.2. Identificación de interesados

**LISTA DE STAKEHOLDERS**

**- POR ROL GENERAL EN EL PROYECTO -**

*Tabla 22. Lista de interesados*

<table style="width:100%;">
<colgroup>
<col style="width: 26%" />
<col style="width: 8%" />
<col style="width: 15%" />
<col style="width: 16%" />
<col style="width: 13%" />
<col style="width: 9%" />
<col style="width: 9%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="2"><blockquote>
<p><strong>Rol general</strong></p>
</blockquote></th>
<th colspan="5"><blockquote>
<p><strong>Stakeholders</strong></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td colspan="2"><blockquote>
<p><strong>SPONSOR</strong></p>
</blockquote></td>
<td colspan="5"><blockquote>
<p>Gerencia General de Agrícola Riachuelo S.A.C.</p>
</blockquote></td>
</tr>
<tr class="even">
<td colspan="2"><blockquote>
<p><strong>EQUIPO DE PROYECTO</strong></p>
</blockquote></td>
<td><blockquote>
<p>PROJECT</p>
<p>Antonio.</p>
</blockquote></td>
<td><blockquote>
<p>MANAGER:</p>
</blockquote></td>
<td><blockquote>
<p>Gutiérrez</p>
</blockquote></td>
<td><blockquote>
<p>Rojas,</p>
</blockquote></td>
<td><blockquote>
<p>Oscar</p>
</blockquote></td>
</tr>
<tr class="odd">
<td colspan="2"><blockquote>
<p><strong>EQUIPO DE PROYECTO</strong></p>
</blockquote></td>
<td colspan="5"><blockquote>
<p>EQUIPO DE GESTIÓN: Peña Reyes, Grace Lucia (analista funcional); Zuñiga Matos, María Fernanda (analista técnica).</p>
</blockquote></td>
</tr>
<tr class="even">
<td colspan="2"><blockquote>
<p><strong>EQUIPO DE PROYECTO</strong></p>
</blockquote></td>
<td colspan="5"><blockquote>
<p>OTROS MIEMBROS: Guerrero Figari, Oscar Manuel (desarrollador de backend, plataforma web e IA).</p>
</blockquote></td>
</tr>
<tr class="odd">
<td colspan="2"><blockquote>
<p><strong>ASESOR ACADÉMICO</strong></p>
</blockquote></td>
<td colspan="5"><blockquote>
<p>Pacheco Tamariz, Jean Pierre.</p>
</blockquote></td>
</tr>
<tr class="even">
<td colspan="2"><blockquote>
<p><strong>GERENTES FUNCIONALES</strong></p>
</blockquote></td>
<td colspan="5"><blockquote>
<p>Jefe de Fundo; Especialista fitosanitario (agrónomo).</p>
</blockquote></td>
</tr>
<tr class="odd">
<td colspan="2"><blockquote>
<p><strong>USUARIOS / CLIENTES</strong></p>
</blockquote></td>
<td colspan="5"><blockquote>
<p>Operadores de monitoreo; Supervisores de zona.</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>PROVEEDORES SOCIOS NEGOCIO</strong></p>
</blockquote></td>
<td><blockquote>
<p><strong>/ DE</strong></p>
</blockquote></td>
<td colspan="5"><blockquote>
<p>Proveedores de insumos agrícolas; proveedor de servicios en la nube.</p>
</blockquote></td>
</tr>
<tr class="odd">
<td colspan="2"><blockquote>
<p><strong>OTROS</strong></p>
<p><strong>STAKEHOLDERS</strong></p>
</blockquote></td>
<td colspan="5"><blockquote>
<p>SENASA; certificadoras de buenas prácticas agrícolas (GlobalG.A.P.); clientes/importadores.</p>
</blockquote></td>
</tr>
</tbody>
</table>

*Fuente: Elaboración propia.*

## 5.3. Registro de interesados

*Tabla 23. Registro de interesados*

| Nombre / Rol | Empresa y puesto | Rol en el proyec to | Expectati vas princi pales | Influe nc ia | Fase de mayo r interé s | Ori g e n | Post u r a |
|---|---|---|---|---|---|---|---|
| Gerencia<br>Gener al | Agrícola<br>Riachu elo S.A.C. | Sponsor | Reducir costos operati vosymejora r la rentabi lidad media nte tecnol ogía. | Fuerte | Inicio y cierre | Inter n o | Apo y o |
| Jefe de Fundo | Agrícola<br>Riachu elo – Gerenci a de operaci ones de campo | Patrocinad or operati vo / gerent e funcio nal | Que el sistem a mejore el trabajo de campo sin afectar la produc ción. | Fuerte | Todo el proye cto | Inter n o | Apo y o |
| Especialis ta fitosa nitario | Agrícola<br>Riachu elo – Agróno mo | Product<br>Owner<br>/ valida dor técnico | Evidencia fotogr áfica confia ble que agilice | Fuerte | Todo el proye cto | Inter n o | Apo y o |
|  |  |  | y respal de su diagnó stico. |  |  |  |  |
| Gutiérrez<br>Rojas, Oscar Anton io | UTP –<br>Estudia nte de<br>Ing.deSistema s | Project<br>Manag er | Cumplir el plan de proyec to y culmin ar exitosa mente. | Media no | Todo el proye cto | Inter n o | Apo y o |
| Peña<br>Reyes<br>,<br>Grace Lucia | UTP –<br>Estudia nte de<br>Ing.deSistema s | Analista funcio nal | Que el sistem a cumpl a el alcanc e ylosrequisi tos levant ados. | Media no | Todo el proye cto | Inter n o | Apo y o |
| Zuñiga<br>Matos<br>,<br>María Ferna nda | UTP –<br>Estudia nte de<br>Ing.deSistema s | Analista técnica<br>/ hardw are | Que el prototi po vestibl e sea ergonó mico y | Media no | Hardware y protot ipo | Inter n o | Apo y o |
|  |  |  | funcio nal. |  |  |  |  |
| Guerrero<br>Figari, Oscar Manu el | UTP –<br>Estudia nte de<br>Ing.deSistema s | Desarrolla dor de IA, backen d y platafo rma | Buen desem peño del model o de IA y estabil idad delaplatafo rma. | Media no | IA y plataf orma web | Inter n o | Apo y o |
| Pacheco<br>Tamar iz, Jean Pierre | UTP –<br>Docent edelcurso | Asesor acadé mico / evalua dor | Que el proyec to cumpl a la metod ología y los objetiv os del curso. | Fuerte | Todo el proye cto | Inter n o | Apo y o |
| Operadore s de | Agrícola<br>Riachu elo – | Usuarios directo s del | Queelequipo sea | Bajo | Piloto en camp o | Inter n o | Neut r |
| monit oreo | Operad oresdecampo | prototi po | cómod o, funcio nal y no aumen te su carga de trabajo<br>. |  |  |  | a l |
| Superviso res de zona | Agrícola<br>Riachu elo – Coordi nación de zona | Usuarios / coordi nadore s | Que el sistem a facilite la asigna ción de rutas y el segui miento de casos. | Bajo | Piloto en camp o | Inter n o | Apo y o |
| SENASA | Entidad regulad ora fitosani taria del Estado | Regulador extern o | Cumplimi ento de la normat iva fitosan itaria | Fuerte | Cierre / certifi cació n | Exte r n o | Neut r a l |
|  |  |  | vigent e para export ación. |  |  |  |  |
| Certificad oras | Organismo s certific adores | Auditor de estánd ares | Verificar trazabi lidad y control de plagas confor me al estánd ar. | Fuerte | Cierre / certifi cació n | Exte r n o | Neut r a l |
| Clientes / import adores | Comprador es interna cionale s de uva | Cliente final indirec to | Recibir produc to de calida d, libre de plagas y con trazabi lidad. | Fuerte | Cierre | Exte r n o | Apo y o |
| Proveedor esdeinsum os | Proveedore s agrícol as del fundo | Proveedor extern o al sistem a | Mantener su relació n comer cial habitu al, sin | Bajo | No aplica | Exte r n o | Neut r a l |
|  |  |  | verse afecta dos. |  |  |  |  |

*Fuente: Elaboración propia.*

## 5.4. Matriz poder/interés

*Tabla 24. Matriz poder/interés*

<table>
<tr>
<th></th>
<th></th>
<th colspan="2">PODER SOBRE EL PROYECTO</th>
<th rowspan="3"></th>
</tr>
<tr>
<th></th>
<th></th>
<th>BAJO</th>
<th>ALTO</th>
</tr>
<tr>
<th rowspan="2">INTERÉS<br>SOBRE EL PROYECTO</th>
<th>ALTO</th>
<th>Operadores de monitoreo.<br>Supervisores de zona.</th>
<th>Gerencia<br>General.<br>Jefe de Fundo.<br>Especialista fitosanitario.</th>
</tr>
<tr>
<th></th>
<th>BAJO</th>
<th>Proveedores de insumos.</th>
<th>SENASA.<br>Certificadoras.<br>Clientes / importadores.</th>
</tr>
</table>

*Fuente: Elaboración propia.*

## 5.5. Matriz influencia/impacto

*Tabla 25. Matriz influencia/impacto*

<table>
<tr>
<th></th>
<th></th>
<th colspan="2">IMPACTO SOBRE EL PROYECTO</th>
</tr>
<tr>
<td></td>
<td></td>
<td>BAJO</td>
<td>ALTO</td>
</tr>
<tr>
<td>INFLUENCIA SOBRE EL PROYECTO</td>
<td>ALTA</td>
<td>Clientes / importadores.</td>
<td>Gerencia<br>General.<br>Jefe de Fundo.</td>
</tr>
</table>

<table>
<tr>
<th rowspan="2"></th>
<th></th>
<th></th>
<th>Especialista fitosanitario.<br>SENASA.<br>Certificadoras.</th>
<th></th>
</tr>
<tr>
<th></th>
<th>BAJA</th>
<th>Supervisores de zona.<br>Proveedores de insumos.</th>
<th>Operadores de monitoreo.</th>
</tr>
</table>

*Fuente: Elaboración propia.*

## 5.6. Estrategia de involucramiento

*Tabla 26. Estrategia de involucramiento*

| Stakeholder | Interés en el proyecto | Evaluació ndelimpac to | Estrategia potencial para ganar soporte o reducir obstáculos | Observaciones |
|---|---|---|---|---|
| Gerencia<br>General | Reducir costos de monitoreo y mejorar la rentabilid ad. | Muy alto | Informar periódicament e sobre el retorno esperado y los avances del piloto. | Aprueba el presupuesto y la implementaci ón. |
| Jefe de Fundo | Que el sistema no afecte la producció n diaria de campo. | Muy alto | Involucrarlo en la planificación del piloto y en la capacitación de los operadores. | Coordina al personal de monitoreo. |
| Especialista fitosanitari o | Contar con evidencia confiable para agilizar su diagnóstic o. | Muy alto | Validar junto a él los criterios de aceptación del modelo deIA en cada SprintReview. | Suconfirmación es obligatoria para todo hallazgo. |
| Equipo del proyecto | Culminar el proyecto en el tiempo y con la calidad establecid a. | Muy alto | Informar continuamente sobre avances mediante reuniones semanales y retrospectivas. | Equipo académico de cuatro estudiantes. |
| Operadores de monitoreo | Que el equipo sea cómodo y no aumente su carga de trabajo. | Alto | Brindar capacitación práctica previa al piloto y recogersuretroalimentaci ón. | Usuarios directos delprototipovestible. |
| Supervisores de zona | Facilitar la asignació n de rutas y el seguimien to de casos. | Alto | Mantenerlos informados sobre el funcionamient o del panel de casos de la plataforma. | Coordinan alos operadores en campo. |
| SENASA /<br>Certificado ras | Cumplimiento de la normativa fitosanitar ia ydetrazabilida d. | Alto | Documentar la trazabilidad generada por la plataforma para futuras auditorías. | No participan directamente en el piloto. |
| Clientes / importador es | Recibir producto libre de plagas y con trazabilida d demostrab le. | Alto | Comunicar los resultados del piloto como mejora en la calidad exportable. | Interesados externos, sin involucramie nto directo. |
| Proveedores de insumos | Mantener su relación comercial habitual. | Bajo | No requieren estrategia específica; monitorear que no se vean afectados. | Sin vínculo con el sistema de monitoreo. |

*Fuente: Elaboración propia.*
