# Anexo 34 (AP-35): Lección Aprendida

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
<td rowspan="2">Documentación de la Lección Aprendida N° 01 sobre la calibración del prototipo vestible y validación de calidad de imagen.</td>
</tr>
<tr>
<td></td>
<td>25/09/2026</td>
</tr>
</table>

### LECCIÓN APRENDIDA Nº 01

| Nombre del Proyecto | Siglas del Proyecto |
|---|---|
| Sistema inteligente para el monitoreo y detección de plagas en cultivos de vid en Agrícola Riachuelo | SIMDPCVAR |

<table>
<tr>
<th colspan="2">Fase</th>
<th>Entregable</th>
</tr>
<tr>
<td colspan="2">Hardware y Prototipo / Desarrollo Tecnológico (Sprint 2)</td>
<td>2.2 Ensamblaje y pruebas estructurales del prototipo de captura bilateral / 3.1 App móvil de captura y validación de nitidez</td>
</tr>
<tr>
<td colspan="3">Temas de Referencia</td>
</tr>
<tr>
<td>1</td>
<td colspan="2">Gestión de Calidad e Imagen: Validación local de nitidez, desenfoque por movimiento y ajuste de exposición en dispositivos móviles.</td>
</tr>
<tr>
<td>2</td>
<td colspan="2">Ingeniería de Hardware y Ergonomía: Estabilidad mecánica de soportes tipo T, amortiguación de vibraciones y fijación al brazo del operador.</td>
</tr>
<tr>
<td>3</td>
<td colspan="2">Gestión de Riesgos y Usabilidad: Mitigación de la resistencia al cambio y prevención de frustración en los operadores de campo por repetición continua de fotos.</td>
</tr>
<tr>
<td colspan="3">DESCRIPCIÓN DEL ENTREGABLE: DESCRIBIR LA INFORMACIÓN DEL ENTREGABLE AFECTADO POR LA LECCIÓN APRENDIDA DE ACUERDO AL DICCIONARIO DE LA EDT.</td>
</tr>
</table>

| El entregable comprende el diseño, ensamble, calibración y validación del prototipo físico de captura bilateral (soporte con poste vertical y barra horizontal tipo T fijado al brazo del operador) en conjunto con el módulo de software móvil programado en React Native. Este sistema se encarga de tomar con dos cámaras fotografías simultáneas del mismo punto de la cara de la hilera (doble evidencia; cada hilera se recorre por sus dos lados, SC-04) y validar automáticamente en el dispositivo la nitidez y exposición antes de guardar la evidencia localmente en SQLite. |
|---|
| DESCRIPCIÓN DEL PROBLEMA: DESCRIBIR EL PROBLEMA SURGIDO DURANTE EL CICLO DE VIDA DEL<br>Proyecto y por el cual se generó la Lección Aprendida. |
| Durante las primeras pruebas de marcha en terreno irregular del fundo Agrícola Riachuelo, el balanceo natural del cuerpo del operador al caminar generó vibraciones continuas en la barra horizontal tipo T. Esto causó que las cámaras de los celulares sufrieran descalibraciones en el ángulo de enfoque y produjeran una alta tasa de fotografías borrosas o desenfocadas (> 35% de las tomas iniciales). En consecuencia, la aplicación móvil rechazaba constantemente las capturas obligando al operador a detenerse y repetir la toma de manera recurrente, incrementando el tiempo de recorrido por hilera y generando frustración e incomodidad en el usuario de campo. |
| DESCRIPCIÓN DE LAS CAUSAS: DESCRIBIR LAS CAUSAS QUE MOTIVARON EL ORIGEN DEL PROBLEMA Y GENERARON LA LECCIÓN APRENDIDA. (ADJUNTAR DIAGRAMA DE ISHIKAWA) |
| Falta de amortiguación rígida en los soportes de cámara: Las abrazaderas de sujeción comerciales utilizadas inicialmente eran de plástico rígido sin recubrimiento antideslizante o absorbente de impacto, transmitiendo todo el impacto del paso del operador hacia el lente del celular. |
| Aflojamiento de tornillería por movimiento: La vibración constante de la caminata aflojaba gradualmente los pernos de ajuste de la barra tipo T, perdiendo la alineación horizontal de paralela a la hilera de vid. |
| Umbrales del algoritmo de nitidez inflexibles: El algoritmo local de validación de imagen ejecutado en la app móvil tenía un umbral de detección de movimiento demasiado estricto que rechazaba imágenes con leve desenfoque de fondo que mantenían un racimo o hoja perfectamente legible para el modelo de IA. |
| ACCIONES CORRECTIVAS TOMADAS: DESCRIBIR LAS ACCIONES CORRECTIVAS QUE SE EFECTUARON PARA SOLUCIONAR EL PROBLEMA IDENTIFICADO. |
| Rediseño mecánico del soporte antivibración: Se añadieron empaquetaduras de goma de alta densidad (neopreno) dentro de las abrazaderas universales y se incorporaron contratuercas con arandelas de presión para fijar la barra tipo T. |
| Repetición unilateral y localizada en app móvil: Se ajustó la regla de negocio y la app móvil para que, en caso de detectarse una toma borrosa, el sistema alerte de forma visual y permita repetir únicamente la toma de la cámara que falló (máximo 2 intentos, SC-05) en lugar de reiniciar la captura de ambas cámaras o toda la planta. |
| Calibración adaptativa del algoritmo de nitidez: Se reajustó el algoritmo de validación de nitidez para enfocarse en el área central donde se ubican los racimos, otorgando una ligera tolerancia al fondo en movimiento. |
| RAZONAMIENTO DETRÁS DE LAS ACCIONES: DESCRIBIR EL RAZONAMIENTO DE CÓMO LAS ACCIONES CORRECTIVAS TOMADAS, IMPACTARÁN SOBRE EL PROBLEMA IDENTIFICADO. |
| Al incorporar elementos flexibles de absorción mecánica y contratuercas, se amortigua la transferencia de masa del cuerpo en marcha hacia los dispositivos móviles, manteniendo la estabilidad del ángulo óptico. Por otra parte, al flexibilizar el algoritmo hacia la zona central de interés (racimos/hojas) y permitir la repetición unilateral en lugar de doble captura, se reduce el tiempo de parada por planta, eliminando la fatiga en el operador sin sacrificar la calidad técnica necesaria para que el modelo de IA detecte la plaga<br>Planococcus ficus. |
| RESULTADOS OBTENIDOS: DESCRIPCIÓN DE LOS RESULTADOS OBTENIDOS DESPUÉS DE APLICAR LAS ACCIONES CORRECTIVAS EN EL ENTREGABLE AFECTADO. |
| Reducción del descarte de fotos: La tasa de fotos rechazadas por desenfoque o movimiento disminuyó del 35% a menos del 4% durante los recorridos de prueba. |
| Optimización del tiempo de monitoreo: El tiempo medio empleado por planta se redujo a menos de 3 segundos, manteniendo la validación de nitidez en <=2s en el dispositivo móvil. |
| Satisfacción del operador: Los operadores de campo manifestaron una experiencia de uso mucho más fluida y cómoda al no tener que detenerse continuamente a repetir tomas bilaterales completas. |
| LECCIÓN APRENDIDA: DESCRIBIR DETALLADAMENTE EL CONOCIMIENTO REUTILIZABLE QUE SE PUEDA APROVECHAR PARA MANEJAR LA PERFORMANCE FUTURA DE PROYECTOS. |
| Priorizar la absorción mecánica desde el diseño inicial: No basta con fijar estructuralmente un dispositivo; la vibración por marcha en terreno agrícola exige incorporar siempre elementos de amortiguación pasiva (gomas, resortes o neopreno) y fijadores de rosca en los puntos de articulación. |
| Diseñar algoritmos de calidad de software orientados al contexto real: La validación de nitidez en movilidad no debe evaluar la imagen completa con criterios de laboratorio; debe priorizar la Región de Interés (RoI) donde se ubica el objetivo agronómico (hoja/racimo), evitando descartar imágenes que son totalmente útiles para la inferencia de Inteligencia Artificial. |
| Permitir la tolerancia a fallos parciales en UX: Cuando un sistema captura múltiples canales simultáneos (ej. cámaras bilaterales), la interfaz del usuario debe permitir corregir de forma aislada el canal defectuoso para no penalizar la productividad ni degradar la experiencia del usuario final. |
