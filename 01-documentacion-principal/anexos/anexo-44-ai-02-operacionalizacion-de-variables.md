# Anexo 44 (AI-02): Operacionalización de variables

<table>
<tr>
<th>Variable</th>
<th>Dimensión</th>
<th>Indicador</th>
<th>Unidad / Escala</th>
<th>Instrumento</th>
</tr>
<tr>
<td rowspan="4">Independiente (X): Sistema tecnológico de monitoreo fitosanitario (SIMDPCVAR)</td>
<td rowspan="2">D1:<br>Desarrollo móvil offline</td>
<td>1.1 Tiempo de validación local de nitidez y exposición por toma fotográfica (≤ 2.0 s)</td>
<td>Segundos / Razón (continua)</td>
<td>Logs de auditoría del cliente React Native / Cronómetro digital</td>
</tr>
<tr>
<td>1.2 Tasa de éxito de sincronización de datos de capturas locales (SQLite) hacia la nube</td>
<td>Porcentaje (%) / Razón (continua)</td>
<td>Ficha de control de sincronización de peticiones HTTP/REST</td>
</tr>
<tr>
<td rowspan="2">D2: Servicio de visión artificial (IA)</td>
<td>2.1 Precisión media del modelo de detección YOLO (mAP ≥ 85%)</td>
<td>Porcentaje (%) / Razón (continua)</td>
<td>Matriz de confusión e informe de evaluación del conjunto de prueba (Test Dataset)</td>
</tr>
<tr>
<td>2.2 Tasa de verdaderos positivos en la generación de indicios sugeridos de Planococcus ficus</td>
<td>Porcentaje (%) / Razón (continua)</td>
<td>Ficha de validación del Especialista Fitosanitario</td>
</tr>
</table>

<table>
<tr>
<th>Variable</th>
<th>Dimensión</th>
<th>Indicador</th>
<th>Unidad / Escala</th>
<th>Instrumento</th>
</tr>
<tr>
<td rowspan="2"></td>
<td rowspan="2">D3:<br>Plataforma backend y web centralizada</td>
<td>3.1 Tiempo de respuesta de la API REST v1 en Django (≤ 1.5 s)</td>
<td>Segundos / Razón (continua)</td>
<td>Suite de pruebas de rendimiento HTTP (Postman / JMeter)</td>
</tr>
<tr>
<td>3.2 Tiempo de renderizado de marcadores de casos en el mapa interactivo (Leaflet/OSM)</td>
<td>Segundos / Razón (continua)</td>
<td>Herramientas de desarrollo de navegador (Chrome DevTools)</td>
</tr>
<tr>
<td rowspan="5">Dependiente (Y): Desempeño del proceso de monitoreo fitosanitario</td>
<td rowspan="2">D1:<br>Eficiencia operativa</td>
<td>1.1 Tiempo total de monitoreo (desde el inicio hasta la finalización del recorrido evaluado)</td>
<td>Minutos u horas / Razón (continua)</td>
<td>Observación directa estructurada / ficha de registro de tiempos</td>
</tr>
<tr>
<td>1.2 Personas-hora empleadas y segmentos inspeccionados por hora</td>
<td>Personas-hora y segmentos/hora / Razón (continua)</td>
<td>Ficha de observación del monitoreo fitosanitario y ficha de registro de tiempos</td>
</tr>
<tr>
<td rowspan="2">D2:<br>Efectividad del monitoreo</td>
<td>2.1 Cobertura alcanzada (segmentos monitoreados respecto de los programados × 100)</td>
<td>Porcentaje (%) / Razón (continua)</td>
<td>Ficha de observación del monitoreo fitosanitario</td>
</tr>
<tr>
<td>2.2 Evidencia utilizable y registros correctamente documentados</td>
<td>Porcentaje (%)<br>/ Razón (continua)</td>
<td>Ficha de evaluación de evidencias y registros</td>
</tr>
<tr>
<td>D3:<br>Trazabilidad y seguimiento</td>
<td>3.1 Hallazgos correctamente localizables, tiempo para localizar un hallazgo y casos con seguimiento completo</td>
<td>Porcentaje (%) y minutos / Razón (continua)</td>
<td>Ficha de prueba de localización y ficha de seguimiento de casos</td>
</tr>
</table>
