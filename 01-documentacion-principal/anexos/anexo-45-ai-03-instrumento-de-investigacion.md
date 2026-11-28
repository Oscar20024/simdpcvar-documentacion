# Anexo 45 (AI-03): Instrumento de investigación

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
<td>Versión final del instrumento ajustada para el avance APF2 y validada para la toma de datos del Pretest/Postest.</td>
</tr>
</table>

INSTRUMENTOS DE INVESTIGACIÓN

**Código y Nombre del Proyecto**

**SIMDPCVAR – Sistema Inteligente para el Monitoreo y Detección de Plagas en Cultivos de Vid en Agrícola Riachuelo S.A.C.**

<table>
<tr>
<th colspan="2">INSTRUMENTO N° 1: FICHA DE OBSERVACIÓN Y REGISTRO TÉCNICO EN<br>CAMPO</th>
</tr>
<tr>
<td>Objetivo</td>
<td>Registrar cuantitativamente los tiempos de procesamiento, la precisión diagnóstica de la IA y la cobertura operativa del monitoreo fitosanitario antes (Pretest) y después (Postest) de la implementación del sistema SIMDPCVAR.</td>
</tr>
<tr>
<td>Población / Muestra</td>
<td>Lotes y hileras de cultivo de vid en producción.</td>
</tr>
<tr>
<td>Responsable del Registro</td>
<td>Evaluador de Campo / Especialista Fitosanitario.</td>
</tr>
</table>

<table>
<tr>
<th colspan="4">1. Datos Generales delaEvaluación</th>
</tr>
<tr>
<td>Fecha de Evaluación</td>
<td>06 / 10 / 2026</td>
<td>Fase de<br>Medición</td>
<td>• Pretest (O₁) x Postest (O₂)</td>
</tr>
<tr>
<td>Lote / Sector</td>
<td>Lote L-04 (Sector Sur)</td>
<td>N° de Hilera Evaluada</td>
<td>Hilera 12 a Hilera 18</td>
</tr>
<tr>
<td>Nombre del Evaluador</td>
<td colspan="3">Ing. Max Daniel Gutiérrez Huamán - Fundo Agrícola Riachuelo S.A.C.</td>
</tr>
</table>

<table>
<tr>
<th colspan="5">2. Sección I: Tiempos y Rendimiento Técnico del Sistema (VariableIndependienteX)</th>
</tr>
<tr>
<td>Ítem</td>
<td>Indicador Técnico</td>
<td>Método / Herramienta de Medición</td>
<td>Valor Medido</td>
<td>Meta / Criterio de<br>Aceptación</td>
</tr>
<tr>
<td>1.1</td>
<td>Tiempo de validación local de nitidez e iluminación por foto</td>
<td>Cronómetro interno app React Native</td>
<td>1.2 seg.</td>
<td>≤ 2.0 segundos</td>
</tr>
<tr>
<td>1.2</td>
<td>Tiempo de respuesta de la API REST v1 Django</td>
<td>Medición de peticiones (Postman / JMeter)</td>
<td>0.8 seg.</td>
<td>≤ 1.5 segundos</td>
</tr>
<tr>
<td>1.3</td>
<td>Tiempo de renderizado de marcadores en el mapa web</td>
<td>Herramientas de navegador (Chrome DevTools)</td>
<td>0.6 seg.</td>
<td>≤ 1.0 segundo</td>
</tr>
<tr>
<td>1.4</td>
<td>Tasa de éxito en la sincronización de datos locales a la nube</td>
<td>Log de auditoría de peticiones sincronizadas</td>
<td>100 %</td>
<td>100 % de datos guardados</td>
</tr>
</table>

<table>
<tr>
<th colspan="2">3. Sección II: Precisión Diagnóstica del Modelo de IA (YOLO)</th>
</tr>
<tr>
<td>Concepto</td>
<td>Valor</td>
</tr>
<tr>
<td>Total de imágenes procesadas por el modelo (N)</td>
<td>150 capturas</td>
</tr>
<tr>
<td>Verdaderos Positivos (VP): presencia real de plaga correctamente identificada por la IA</td>
<td>132</td>
</tr>
<tr>
<td>Falsos Positivos (FP): falsa alarma de la IA donde no existe plaga</td>
<td>11</td>
</tr>
<tr>
<td>Falsos Negativos (FN): presencia real de plaga no detectada por la IA</td>
<td>7</td>
</tr>
<tr>
<td colspan="2">Cálculo de Métricas</td>
</tr>
</table>

<table>
<tr>
<th colspan="2">3. Sección II: Precisión Diagnóstica del Modelo de IA (YOLO)</th>
</tr>
<tr>
<td>Concepto</td>
<td>Valor</td>
</tr>
<tr>
<td>Precisión (Precision) = VP / (VP + FP) × 100</td>
<td>Precisión (Precision)<br>𝑉𝑃<br>𝑉𝑃 + 𝐹𝑃 × 100 132 132 + 11 × 100<br>= 𝟗𝟐. 𝟑𝟏%</td>
</tr>
<tr>
<td>Sensibilidad (Recall) = VP / (VP + FN) × 100</td>
<td>Sensibilidad (Recall)<br>𝑉𝑃<br>= × 100<br>+ 𝐹𝑁 132<br>= × 100<br>+ 7<br>= 𝟗𝟒. 𝟗𝟔%</td>
</tr>
</table>

<table>
<tr>
<th colspan="4">4. Sección III: Eficiencia y Oportunidad Fitosanitaria (Variable Dependiente Y)</th>
</tr>
<tr>
<td>Ítem</td>
<td>Indicador de Campo</td>
<td>Medición Pretest (Manual)</td>
<td>Medición Postest (SIMDPCVAR)</td>
</tr>
<tr>
<td>3.1</td>
<td>Tiempo total desde la toma de muestra hasta la confirmación del agrónomo</td>
<td>48.0 horas</td>
<td>1.5 horas</td>
</tr>
<tr>
<td>3.2</td>
<td>Tiempo transcurrido para la emisión de la alerta oficial por Telegram</td>
<td>24.0 horas</td>
<td>8.5 segundos</td>
</tr>
<tr>
<td>3.3</td>
<td>Hectáreas / Lotes evaluados por jornada de trabajo</td>
<td>2.5 ha/día</td>
<td>6.0 ha/día</td>
</tr>
</table>

<table>
<tr>
<th colspan="2">INSTRUMENTO N° 2: CUESTIONARIO DE USABILIDAD Y SATISFACCIÓN (ESCALA SUS)</th>
</tr>
<tr>
<td>Objetivo</td>
<td>Evaluar el nivel de usabilidad, percepción de eficiencia y aceptación del sistema por parte del personal operativo y el Especialista Fitosanitario.</td>
</tr>
<tr>
<td>Instrucciones</td>
<td>Califique cada afirmación marcando con una &quot;X&quot; una sola opción según la siguiente escala Likert: 1: Totalmente en Desacuerdo | 2: En Desacuerdo | 3: Neutral | 4: De Acuerdo | 5: Totalmente de Acuerdo.</td>
</tr>
</table>

<table>
<tr>
<th colspan="7">Cuestionario Estandarizado System Usability Scale (SUS)</th>
</tr>
<tr>
<td>N°</td>
<td>Afirmación / Criterio de Usabilidad</td>
<td>1</td>
<td>2</td>
<td>3</td>
<td>4</td>
<td>5</td>
</tr>
<tr>
<td>1</td>
<td>Creo que me gustaría utilizar este sistema con frecuencia durante mis jornadas de monitoreo en el fundo.</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td>x</td>
</tr>
<tr>
<td>2</td>
<td>Encontré que el sistema es innecesariamente complejo para las tareas de captura y consulta.</td>
<td>x</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>3</td>
<td>Considero que la aplicación móvil y la plataforma web son muy fáciles de usar.</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td>x</td>
</tr>
<tr>
<td>4</td>
<td>Creo que necesitaría del soporte constante de un técnico para poder operar el sistema en campo.</td>
<td></td>
<td>x</td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>5</td>
<td>Encontré que las diversas funciones del sistema (fotos, mapa, alertas) están bien integradas.</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td>x</td>
</tr>
<tr>
<td>6</td>
<td>Pensé que había demasiada inconsistencia en el funcionamiento de la aplicación móvil y web.</td>
<td>x</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>7</td>
<td>Imagino que la mayoría de evaluadores de campo aprenderían a usar este sistema muy rápidamente.</td>
<td></td>
<td></td>
<td></td>
<td>x</td>
<td></td>
</tr>
</table>

<table>
<tr>
<th colspan="7">Cuestionario Estandarizado System Usability Scale (SUS)</th>
</tr>
<tr>
<td>N°</td>
<td>Afirmación / Criterio de Usabilidad</td>
<td>1</td>
<td>2</td>
<td>3</td>
<td>4</td>
<td>5</td>
</tr>
<tr>
<td>8</td>
<td>Encontré que el sistema es muy intuitivo de manejar durante el recorrido por las hileras.</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td>x</td>
</tr>
<tr>
<td>9</td>
<td>Me sentí muy confiado/a al utilizar el sistema para registrar y verificar la presencia de plagas.</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>10</td>
<td>Necesité aprender muchas cosas antes de poder empezar a operar el sistema de manera fluida.</td>
<td></td>
<td>x</td>
<td></td>
<td></td>
<td></td>
</tr>
</table>

<table>
<tr>
<th colspan="7">Preguntas de Percepción Operativa Complementarias</th>
</tr>
<tr>
<td>N°</td>
<td>Criterio de Operación en Campo</td>
<td>1</td>
<td>2</td>
<td>3</td>
<td>4</td>
<td>5</td>
</tr>
<tr>
<td>11</td>
<td>La capacidad de capturar y guardar datos sin conexión (offline) facilitó el trabajo en zonas alejadas.</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td>X</td>
</tr>
<tr>
<td>12</td>
<td>Las cajas delimitadoras de la Inteligencia Artificial me ayudaron a ubicar la plaga más rápido.</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td>X</td>
</tr>
<tr>
<td>13</td>
<td>El envío automático de alertas por Telegram agilizó la atención del foco detectado.</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td>X</td>
</tr>
</table>
