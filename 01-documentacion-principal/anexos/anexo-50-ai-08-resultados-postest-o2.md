# Anexo 50 (AI-08): Resultados Postest (O2)

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
<td rowspan="2">Grace Peña<br>/ Equipo Scrum</td>
<td rowspan="2">Max Gutiérrez<br>Huamán (Especialista Fitosanitario)</td>
<td rowspan="2">Pedro Palacios<br>Chanco / Dulio Gonzales</td>
<td></td>
<td rowspan="2">Consolidado inicial de procesamiento estadístico de los resultados de la etapa Postest (O₂).</td>
</tr>
<tr>
<td></td>
<td>29/10/2026</td>
</tr>
</table>

ANEXO AI-08: RESULTADOS POSTEST (O₂)

**Código y Nombre del Proyecto**

**SIMDPCVAR – Sistema Inteligente para el Monitoreo y Detección de Plagas en Cultivos de Vid en Agrícola<br>Riachuelo S.A.C.**

<table>
<tr>
<th colspan="5">RESULTADOS DEL POSTEST (O2): CONSOLIDADO ESTADÍSTICO TRAS LA PUESTA EN OPERACIÓN DEL SISTEMA<br>SIMDPCVAR.</th>
</tr>
<tr>
<td colspan="5">1.- RESUMEN EJECUTIVO DEL POSTEST (O2)</td>
</tr>
<tr>
<td colspan="5">El análisis cuantitativo tras la puesta en operación del sistema SIMDPCVAR demuestra una optimización sustancial en todas las dimensiones fitosanitarias respecto a la línea base tradicional:</td>
</tr>
<tr>
<td colspan="5">Muestra evaluada (N): 15 jornadas de monitoreo técnico con la solución tecnológica.</td>
</tr>
<tr>
<td colspan="5">Cobertura promedio por jornada: Se incrementó de 339.3 a 840.67 plantas/operador/día (+147.76 %).</td>
</tr>
<tr>
<td colspan="5">Reducción de errores de registro: Eliminación completa (0.0 %) de incoherencias en georreferenciación e identificación de hileras/lotes.</td>
</tr>
<tr>
<td colspan="5">2.- Resultados Estadísticos Descriptivos y Comparativos</td>
</tr>
<tr>
<td colspan="5">Tabla 1: Comparativa General Pretest (O1) vs. Postest (O2)</td>
</tr>
<tr>
<td>Variable / Indicador</td>
<td>MEDICIÓN PRETEST (O1)</td>
<td>MEDICIÓN POSTEST (O2)</td>
<td>Variación Absoluta</td>
<td>Impacto / Eficiencia (%)</td>
</tr>
<tr>
<td>Tiempo de confirmación fitosanitaria</td>
<td>51.06 horas</td>
<td>1.23 horas</td>
<td>-49.83 horas</td>
<td>-97.59 % (Atención ágil)</td>
</tr>
<tr>
<td>Tiempo de emisión de alerta por<br>Telegram</td>
<td>31.20 horas</td>
<td>8.50 segundos</td>
<td>-31.20 horas</td>
<td>-99.99 % (Alerta inmediata)</td>
</tr>
<tr>
<td>Cobertura diaria por operador</td>
<td>339.33 plantas</td>
<td>840.67 plantas</td>
<td>+501.34 plantas</td>
<td>+147.76 % (Mayor alcance)</td>
</tr>
<tr>
<td>Tasa de error en datos/ubicación</td>
<td>66.67 %</td>
<td>0.00 %</td>
<td>-66.67 %</td>
<td>-100.00 % (Datos exactos)</td>
</tr>
<tr>
<td>Imágenes descartadas por mala calidad</td>
<td>41.33 %</td>
<td>0.00 %</td>
<td>-41.33 %</td>
<td>-100.00 % (Filtro local activo)</td>
</tr>
<tr>
<td colspan="5">3.- Diagnóstico por Dimensión tras la Intervención</td>
</tr>
<tr>
<td>Tiempo de Respuesta<br>Fitosanitaria</td>
<td colspan="4">La API REST v1 centralizada en Django y las notificaciones automáticas por Telegram redujeron el ciclo de validación de 2 días a solo 1.23 horas.</td>
</tr>
<tr>
<td>Precisión y Calidad de<br>Evidencias</td>
<td colspan="4">La validación local instantánea en la app móvil impidió la subida de fotos borrosas, garantizando que el 100 % de las imágenes recibidas en Cloudinary CDN fueran útiles para el análisis del agrónomo Max Gutiérrez.</td>
</tr>
<tr>
<td>Desempeño del Modelo de IA (YOLO26n)</td>
<td colspan="4">El worker de IA procesó las tareas encoladas en Supabase logrando un mAP@0.5 de 88.75 %, superando holgadamente la meta del 85 %.</td>
</tr>
</table>

**ANEXOS SCRUM**
