# Anexo 53 (AS-03): Criterios de aceptación

| 2.0 | Grace<br>Peña<br>/ Equip o Scru m | Scrum Master | Product<br>Owner | 06/10/2 026 | Actualización por reformulación de arquitectura a Django + Supabase y definición de criterios de aceptación verificables. |
|---|---|---|---|---|---|

![](../media/image71.png)

**Código y Nombre del Proyecto**

**SIMDPCVAR – Sistema Inteligente para el Monitoreo y Detección de Plagas en Cultivos de Vid en Agrícola Riachuelo S.A.C.**

***<span class="smallcaps">Tabla general de criterios de aceptación por historia de usuario</span>***

<table>
<tr>
<th colspan="5">HU01 – Prototipo Físico de Captura Bilateral</th>
</tr>
<tr>
<td>N.º</td>
<td>Criterio de<br>Aceptación</td>
<td>Dado que</td>
<td>Cuando</td>
<td>Entonces</td>
</tr>
<tr>
<td>1.1</td>
<td>Ajuste Mecánico</td>
<td>el operador de campo debe recorrer las hileras de cultivo de vid</td>
<td>se fije el arnés ergonómico al brazo del usuario con el soporte en T y los dos smartphones montados</td>
<td>el equipo debe mantenerse firme sin desprendimientos ni desalineación visual durante caminatas continuas.</td>
</tr>
<tr>
<td>1.2</td>
<td>VinculacióndeDispositivos (RF11)</td>
<td>el sistema requiere la captura sincronizada de las dos cámaras sobre el mismo punto de la hilera</td>
<td>el usuario inicie una sesión de recorrido en la aplicación móvil</td>
<td>el sistema debe exigir y verificar la conexión exitosa de los tres dispositivos (dispositivo controlador, Cámara 1 y Cámara 2) antes de permitir el inicio de la captura (RF11).</td>
</tr>
</table>

<table>
<tr>
<th colspan="5">HU02 – Validación Local de Calidad y Almacenamiento Offline</th>
</tr>
<tr>
<td>N.º</td>
<td>Criterio de<br>Aceptación</td>
<td>Dado que</td>
<td>Cuando</td>
<td>Entonces</td>
</tr>
<tr>
<td>2.1</td>
<td>Validación de Imagen (RF02)</td>
<td>el operador realiza capturas fotográficas en campo</td>
<td>tome una fotografía de la planta</td>
<td>el algoritmo local de la app móvil debe evaluar la nitidez, iluminación/exposición y movimiento en un tiempo máximo de 2.0 segundos por foto (RNF02).</td>
</tr>
<tr>
<td>2.2</td>
<td>Alerta y<br>Repetición de Toma (RF03)</td>
<td>una fotografía capturada presente borrosidad o mala iluminación</td>
<td>la validación de calidad falle</td>
<td>la app debe mostrar una alerta visual en pantalla indicando la cámara afectada y habilitar el botón para repetir únicamente esa toma defectuosa sin descartar la toma válida de la otra cámara.</td>
</tr>
</table>

<table>
<tr>
<th colspan="5">HU02 – Validación Local de Calidad y Almacenamiento Offline</th>
</tr>
<tr>
<td>N.º</td>
<td>Criterio de<br>Aceptación</td>
<td>Dado que</td>
<td>Cuando</td>
<td>Entonces</td>
</tr>
<tr>
<td>2.3</td>
<td>Persistencia<br>Offline y Seguridad (RF04, RNF06, RNF07)</td>
<td>en sectores alejados del fundo no existe conectividad a internet (Riesgo 1)</td>
<td>se apruebe la calidad de una foto</td>
<td>la imagen debe guardarse en el almacenamiento privado de la app (expo-file-system) y los metadatos de ubicación (lote, hilera, zona, lado, marcador y GPS) en SQLite (RF06, RNF07), para sincronizarse automáticamente al detectar red estable (RF05, RNF06).</td>
</tr>
</table>

<table>
<tr>
<th colspan="5">HU03 – Visualización Cartográfica e Indicios de IA</th>
</tr>
<tr>
<td>N.º</td>
<td>Criterio de<br>Aceptación</td>
<td>Dado que</td>
<td>Cuando</td>
<td>Entonces</td>
</tr>
<tr>
<td>3.1</td>
<td>Inferencia de IA (RF07)</td>
<td>las fotografías y metadatos han sido sincronizados en la nube (metadatos en Supabase y fotos en Cloudinary)</td>
<td>el worker de IA de Django (YOLO en ONNX Runtime) procese la imagen</td>
<td>debe generar las cajas delimitadoras (bounding boxes) y el porcentaje de confianza sobre los indicios de Planococcus ficus alcanzando una precisión media mAP ≥ 85 %.</td>
</tr>
<tr>
<td>3.2</td>
<td>Visualización en Mapa Web (RF08, RNF04)</td>
<td>el especialista fitosanitario ingresa a la plataforma web unificada en Django</td>
<td>consulte el mapa interactivo (Leaflet/OSM)</td>
<td>el sistema debe renderizar los marcadores georreferenciados del fundo con sus evidencias fotográficas manteniendo un tiempo de respuesta de la API REST ≤ 1.5 segundos (RNF04).</td>
</tr>
<tr>
<td>3.3</td>
<td>Validación<br>Humana (RF09)</td>
<td>el especialista revisa los indicios sugeridos por la IA</td>
<td>examine la evidencia en la web</td>
<td>debe contar con los botones de acción para dictaminar el caso como &quot;Confirmado&quot;, &quot;Posible plaga&quot;, &quot;Descartado&quot; o &quot;Evidencia insuficiente&quot; , registrando la trazabilidad del usuario y hora exacta.</td>
</tr>
</table>

<table>
<tr>
<th colspan="5">HU04 – Emisión de Alertas de Casos Confirmados</th>
</tr>
<tr>
<td>N.º</td>
<td>Criterio de<br>Aceptación</td>
<td>Dado que</td>
<td>Cuando</td>
<td>Entonces</td>
</tr>
<tr>
<td>4.1</td>
<td>Disparo<br>Restringido de Alerta (RF10)</td>
<td>el especialista fitosanitario acaba de confirmar un hallazgo de plaga en la plataforma web</td>
<td>se guarde el dictamen en la basededatos</td>
<td>el sistema debe enviar automáticamente una notificación vía Telegram Bot API (SC-02) al jefe de fundo y supervisor de zona en menos de 10 segundos (RF10).</td>
</tr>
</table>

<table>
<tr>
<th colspan="5">HU04 – Emisión de Alertas de Casos Confirmados</th>
</tr>
<tr>
<td>N.º</td>
<td>Criterio de<br>Aceptación</td>
<td>Dado que</td>
<td>Cuando</td>
<td>Entonces</td>
</tr>
<tr>
<td>4.2</td>
<td>Contenido de la Alerta</td>
<td>se emitela notificación dealerta</td>
<td>el supervisor reciba el mensaje ensuteléfono</td>
<td>el texto debe detallar la foto, el lote, la hilera, el lado, el segmento, el marcador y los botones «Ver caso» y «Cómo llegar».</td>
</tr>
<tr>
<td>4.3</td>
<td>Control de Falsas Alarmas</td>
<td>el especialista fitosanitario clasifique un indicio como &quot;Descartado&quot;</td>
<td>se actualice el registro</td>
<td>el sistema NO debe emitir ninguna notificación por Telegram, asegurando que no se alerten casos descartados por el especialista.</td>
</tr>
</table>
