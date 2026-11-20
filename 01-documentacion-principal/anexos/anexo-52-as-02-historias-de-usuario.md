# Anexo 52 (AS-02): Historias de usuario

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
<td>Equipo<br>Scru m</td>
<td>Scrum Master</td>
<td>Product<br>Owner</td>
<td>06/10/2 026</td>
<td>Descripcion breve de una funcionalidad que un usuario necesita dentro de un sistema o aplicación</td>
</tr>
</table>

![](../media/image70.png)

**CÓDIGO Y NOMBRE DEL PROYECTO**

**SIMDPCVAR – Sistema Inteligente para el Monitoreo y Detección de Plagas en Cultivos de Vid en Agrícola Riachuelo S.A.C.**

| Nombre Corto de la Historia de Usuario: Prototipo Físico de Captura Bilateral |
|---|
| DESCRIPCIÓN DE LA HISTORIA DE USUARIO: COMO OPERADOR DE MONITOREO, YO DEBERÍA PORTAR UN PROTOTIPO FÍSICO DE CAPTURA BILATERAL FIJADO AL BRAZO, CON SOPORTE TIPO T PARA DOS DISPOSITIVOS MÓVILES, A FIN DE OBTENER DOBLE EVIDENCIA DEL MISMO PUNTO DE LA HILERA EN CADA PASADA (LATERAL A Y LATERAL B, SC-04) SIN TENER QUE SOSTENER LOS EQUIPOS CON LAS MANOS. |
| CRITERIOS DE ACEPTACIÓN: CADA HISTORIA DE USUARIO CUENTA CON SUS RESPECTIVOS CRITERIOS DE ACEPTACIÓN. LAS HISTORIAS DE USUARIO SON SUBJETIVAS DE TAL FORMA QUE LOS CRITERIOS DE ACEPTACIÓN BRINDA LA OBJETIVIDAD REQUERIDA PARA QUE SE CONSIDEREN TERMINADAS DURANTE LA REVISIÓN DEL SPRINT. |
| Criterio de Aceptación 1:<br>El sistema permite capturar fotos bilaterales de cada planta por medio de los dispositivos móviles montados en el arnés. |
| Criterio de Aceptación 2:<br>Exige la vinculación exitosa de los tres dispositivos (controlador, Cámara 1 y Cámara 2; SC-04) antes de habilitar el inicio del recorrido en la app. |
| Criterio de Aceptación 3:<br>El soporte mantiene la alineación física y fijación segura de los teléfonos sin interrumpir la caminata del operador. |
| CASOS LÍMITE: SON PROBLEMAS O SITUACIONES QUE OCURREN EN CASOS EXTREMOS (MÁXIMOS O MÍNIMOS). SE DEBE ESPECIFICAR QUÉ ES LO QUE SE DEBE HACER EN TALES CASOS PARA CONSIDERAR COMO ACEPTADA LA HISTORIA DE USUARIO. |
| Caso Límite 1:<br>Pérdida de conexión o vinculación de uno de los dispositivos secundarios (Cámara 1 o Cámara 2) antes de iniciar el tramo → El sistema bloquea el inicio del recorrido hasta restablecer la vinculación de los 3 dispositivos (RF11). |
| Caso Límite 2:<br>Terreno irregular con sacudidas bruscas del brazo → El soporte físico absorbe el movimiento manteniendo el enfoque en las hileras. |
| ELEMENTO NO FUNCIONAL: EN CASO DE QUE LA HISTORIA DE USUARIO SE TRATE DE UN REQUISITO O ELEMENTO NO FUNCIONAL PUEDE SER NECESARIO ESPECIFICAR DATOS ADICIONALES. |
| FRECUENCIA: ¿CUÁN FRECUENTEMENTE LA PERSONA DEL ROL IDENTIFICADO NECESITARÁ ESTA HISTORIA DE<br>Usuario? |
| Diaria (durante las jornadas de evaluación en campo). |
| URGENCIA: ¿CUÁN RÁPIDAMENTE TENDRÁ QUE RESPONDER LA APLICACIÓN A LAS NECESIDADES DEL USUARIO? |
| Inmediata; la interfaz debe ser intuitiva y permitir iniciar un recorrido en menos de 3 pasos (RNF03). |
| VOLUMEN: ¿CUÁNTO VOLUMEN DE DATOS TENDRÁ QUE MANTENER LA APLICACIÓN PARA ESTA HISTORIA DE USUARIO? |
| Soporta sesiones de caminata continua a lo largo de las hileras del fundo. |
| PRECISIÓN: ¿CUÁN PRECISA Y OPORTUNA TIENE QUE SER LA DATA DESDE LA PERSPECTIVA DEL NEGOCIO? |
| Alta alineación visual para asegurar encuadre de racimos y hojas en ambas cámaras. |
| USABILIDAD: ¿CUÁLES CARACTERÍSTICAS HARÁN QUE LA APLICACIÓN SEA FÁCILMENTE USABLE POR EL ROL IDENTIFICADO? |
| OPERACIÓN HANDS-FREE FIJADA AL BRAZO PARA REDUCIR EL CANSANCIO DEL OPERADOR. |
| FACILIDAD DE APRENDIZAJE: ¿CUÁN RÁPIDAMENTE PUEDEN LOS NUEVOS USUARIOS, APRENDER A USAR LA APLICACIÓN? |
| Muy rápida; configuración inicial guiada en la app móvil. |
| FLEXIBILIDAD/ESCALABILIDAD: ¿CUÁN RÁPIDAMENTE SE PUEDE ADAPTAR LA APLICACIÓN A LOS CAMBIOS EN FRECUENCIA Y VOLUMEN? |
| Adaptable a smartphones de diferentes dimensiones en el soporte T. |
| CONFIABILIDAD: ¿CUÁN CRÍTICA ES LA APLICACIÓN PARA QUE NO DEBA FALLAR? |
| Alta fijación mecánica para evitar caídas de los equipos de captura. |
| Nombre Corto de la Historia de Usuario: Validación Automática de Calidad y Almacenamiento Offline |
| DESCRIPCIÓN DE LA HISTORIA DE USUARIO: COMO OPERADOR, YO DEBERÍA SOLICITAR QUE LA APLICACIÓN MÓVIL VERIFIQUE AUTOMÁTICAMENTE LA NITIDEZ Y LA ILUMINACIÓN DE CADA FOTO ANTES DE CAMBIAR DE ZONA, PARA EVITAR REGISTRAR IMÁGENES INSERVIBLES. |
| CRITERIOS DE ACEPTACIÓN: CADA HISTORIA DE USUARIO CUENTA CON SUS RESPECTIVOS CRITERIOS DE ACEPTACIÓN. LAS HISTORIAS DE USUARIO SON SUBJETIVAS DE TAL FORMA QUE LOS CRITERIOS DE ACEPTACIÓN BRINDA LA OBJETIVIDAD REQUERIDA PARA QUE SE CONSIDEREN TERMINADAS DURANTE LA REVISIÓN DEL SPRINT. |
| Criterio de Aceptación 1: |
| La aplicación valida automáticamente la calidad básica de cada captura, considerando nitidez, iluminación/exposición y movimiento antes de almacenarla (RF02). |
| Criterio de Aceptación 2:<br>Si una toma presenta deficiencias de calidad, la interfaz muestra una alerta visual indicando la cámara o captura que debe repetirse (RF03, SC-05). |
| Criterio de Aceptación 3:<br>Las imágenes aprobadas se almacenan como archivos locales privados en el dispositivo móvil, mientras que los metadatos asociados (lote, hilera, zona, lado, marcador, secuencia y GPS) se registran en SQLite en espera de sincronización<br>AUTOMÁTICA AL RECUPERAR SEÑAL (RF04, RF05, RF06). |
| CASOS LÍMITE: SON PROBLEMAS O SITUACIONES QUE OCURREN EN CASOS EXTREMOS (MÁXIMOS O MÍNIMOS). SE debe especificar qué es lo que se debe hacer en tales casos para considerar como aceptada la Historia de usuario. |
| Caso Límite 1:<br>Foto rechazada por desenfoque o movimiento → La app habilita inmediatamente la<br>OPCIÓN DE REPETIR LA CAPTURA ÚNICAMENTE DE LA CÁMARA QUE FALLÓ SIN PERDER LA FOTO VÁLIDA DE LA OTRA CÁMARA (RF03, SC-05). |
| Caso Límite 2:<br>Nula conectividad a internet en sectores alejados del fundo → La aplicación opera 𝟏𝟎𝟎% en modo sin conexión (RNF06), custodiando las fotos en el almacenamiento privado de la app (expo-file-system) y los tokens en expo-secure-store (SC-06) hasta detectar red estable (RNF07). |
| ELEMENTO NO FUNCIONAL: EN CASO DE QUE LA HISTORIA DE USUARIO SE TRATE DE UN REQUISITO O ELEMENTO NO FUNCIONAL PUEDE SER NECESARIO ESPECIFICAR DATOS ADICIONALES. |
| FRECUENCIA: ¿CUÁN FRECUENTEMENTE LA PERSONA DEL ROL IDENTIFICADO NECESITARÁ ESTA HISTORIA DE<br>Usuario? |
| Continua (por cada foto capturada durante la evaluación). |
| URGENCIA: ¿CUÁN RÁPIDAMENTE TENDRÁ QUE RESPONDER LA APLICACIÓN A LAS NECESIDADES DEL USUARIO? |
| La validación de calidad se realiza en un tiempo máximo de 2 segundos por fotografía (RNF02). |
| VOLUMEN: ¿CUÁNTO VOLUMEN DE DATOS TENDRÁ QUE MANTENER LA APLICACIÓN PARA ESTA HISTORIA DE USUARIO? |
| La aplicación móvil mantiene almacenamiento seguro local para cientos de imágenes por jornada sin saturar la memoria (RNF07). |
| PRECISIÓN: ¿CUÁN PRECISA Y OPORTUNA TIENE QUE SER LA DATA DESDE LA PERSPECTIVA DEL NEGOCIO? |
| Exactitud en la detección de desenfoque e iluminación inservible. |
| USABILIDAD: ¿CUÁLES CARACTERÍSTICAS HARÁN QUE LA APLICACIÓN SEA FÁCILMENTE USABLE POR EL ROL IDENTIFICADO? |
| Alertas visuales claras indicando la cámara exacta a repetir. |
| FACILIDAD DE APRENDIZAJE: ¿CUÁN RÁPIDAMENTE PUEDEN LOS NUEVOS USUARIOS, APRENDER A USAR LA APLICACIÓN? |
| Inmediata; retroalimentación automática en pantalla. |
| FLEXIBILIDAD/ESCALABILIDAD: ¿CUÁN RÁPIDAMENTE SE PUEDE ADAPTAR LA APLICACIÓN A LOS CAMBIOS EN FRECUENCIA Y VOLUMEN? |
| Sincronización automática desatendida al recuperar señal Wi-Fi/4G (RF05, RNF06). |
| CONFIABILIDAD: ¿CUÁN CRÍTICA ES LA APLICACIÓN PARA QUE NO DEBA FALLAR? |
| Crítica; custodia obligatoria en almacenamiento privado e de la app (expo-file-system, tokens en expo- secure-store) hasta confirmar la subida (RNF07). |
| Nombre Corto de la Historia de Usuario: Visualización Cartográfica e Indicios de IA |
| DESCRIPCIÓN DE LA HISTORIA DE USUARIO: COMO ESPECIALISTA FITOSANITARIO, YO DEBERÍA VISUALIZAR LOS POSIBLES HALLAZGOS SEÑALADOS POR LA INTELIGENCIA ARTIFICIAL SOBRE UN MAPA INTERACTIVO DEL FUNDO, JUNTO CON SU EVIDENCIA FOTOGRÁFICA, PARA CONFIRMAR O DESCARTAR EL CASO<br>CON MAYOR TRAZABILIDAD. |
| CRITERIOS DE ACEPTACIÓN: CADA HISTORIA DE USUARIO CUENTA CON SUS RESPECTIVOS CRITERIOS DE ACEPTACIÓN. LAS HISTORIAS DE USUARIO SON SUBJETIVAS DE TAL FORMA QUE LOS CRITERIOS DE ACEPTACIÓN BRINDA LA OBJETIVIDAD REQUERIDA PARA QUE SE CONSIDEREN TERMINADAS DURANTE LA REVISIÓN DEL SPRINT. |
| Criterio de Aceptación 1:<br>EL SISTEMA ANALIZA LAS IMÁGENES SINCRONIZADAS MEDIANTE EL MODELO DE IA (YOLO) Y SEÑALA POSIBLES INDICIOS DE PLAGA (PLANOCOCCUS FICUS) (RF07). |
| Criterio de Aceptación 2:<br>El sistema permite al especialista fitosanitario visualizar los indicios y las evidencias georreferenciadas sobre la plataforma web (RF08). |
| Criterio de Aceptación 3:<br>La plataforma web permite al especialista confirmar o descartar manualmente cada hallazgo registrado (RF09). |
| CASOS LÍMITE: SON PROBLEMAS O SITUACIONES QUE OCURREN EN CASOS EXTREMOS (MÁXIMOS O MÍNIMOS). SE DEBE ESPECIFICAR QUÉ ES LO QUE SE DEBE HACER EN TALES CASOS PARA CONSIDERAR COMO ACEPTADA LA HISTORIA<br>DE USUARIO. |
| Caso Límite 1:<br>Carga masiva de puntos en el mapa web (> 𝟓𝟎, 𝟎𝟎𝟎 plantas registradas) → El sistema soporta la consulta manteniendo el tiempo de respuesta de la API REST menor a 1.5<br>SEGUNDOS (RNF04). |
| Caso Límite 2:<br>Imagen con indicio dudoso → La plataforma permite al especialista hacer zoom sobre la evidencia en alta resolución provista por Cloudinary CDN antes de dictaminar. |
| ELEMENTO NO FUNCIONAL: EN CASO DE QUE LA HISTORIA DE USUARIO SE TRATE DE UN REQUISITO O ELEMENTO NO FUNCIONAL PUEDE SER NECESARIO ESPECIFICAR DATOS ADICIONALES. |
| FRECUENCIA: ¿CUÁN FRECUENTEMENTE LA PERSONA DEL ROL IDENTIFICADO NECESITARÁ ESTA HISTORIA DE<br>Usuario? |
| Diaria / Por lotes de revisión al cierre de jornada |
| URGENCIA: ¿CUÁN RÁPIDAMENTE TENDRÁ QUE RESPONDER LA APLICACIÓN A LAS NECESIDADES DEL USUARIO? |
| Tiempo de respuesta de la API menor a 1.5 segundos en la consulta web (RNF04). |
| VOLUMEN: ¿CUÁNTO VOLUMEN DE DATOS TENDRÁ QUE MANTENER LA APLICACIÓN PARA ESTA HISTORIA DE USUARIO? |
| Capacidad para soportar la consulta web y almacenamiento de datos de 50,000+<br>PLANTAS (RNF04). |
| PRECISIÓN: ¿CUÁN PRECISA Y OPORTUNA TIENE QUE SER LA DATA DESDE LA PERSPECTIVA DEL NEGOCIO? |
| Inferencia del modelo de IA orientada a alta sensibilidad para no omitir posibles focos de plaga |
| USABILIDAD: ¿CUÁLES CARACTERÍSTICAS HARÁN QUE LA APLICACIÓN SEA FÁCILMENTE USABLE POR EL ROL IDENTIFICADO? |
| Mapa interactivo con marcadores de estado (Pendiente, Confirmado, Descartado). |
| FACILIDAD DE APRENDIZAJE: ¿CUÁN RÁPIDAMENTE PUEDEN LOS NUEVOS USUARIOS, APRENDER A USAR LA APLICACIÓN? |
| Intuitiva para ingenieros agrónomos y especialistas. |
| FLEXIBILIDAD/ESCALABILIDAD: ¿CUÁN RÁPIDAMENTE SE PUEDE ADAPTAR LA APLICACIÓN A LOS CAMBIOS EN FRECUENCIA Y VOLUMEN? |
| Arquitectura desplegada en servicios cloud administrados sin servidores físicos locales (RNF01). |
| CONFIABILIDAD: ¿CUÁN CRÍTICA ES LA APLICACIÓN PARA QUE NO DEBA FALLAR? |
| Alta trazabilidad del diagnóstico respaldada por autenticación y control de roles (RNF05). |
| Nombre Corto de la Historia de Usuario: Emisión de Alertas de Casos Confirmados |
| DESCRIPCIÓN DE LA HISTORIA DE USUARIO: COMO SUPERVISOR DE ZONA, YO DEBERÍA RECIBIR NOTIFICACIONES MEDIANTE MENSAJERÍA OFICIAL CUANDO UN CASO HAYA SIDO CONFIRMADO POR EL<br>ESPECIALISTA FITOSANITARIO, PARA FACILITAR EL SEGUIMIENTO OPERATIVO CORRESPONDIENTE |
| CRITERIOS DE ACEPTACIÓN: CADA HISTORIA DE USUARIO CUENTA CON SUS RESPECTIVOS CRITERIOS DE aceptación. Las historias de Usuario son subjetivas de tal forma que los Criterios de Aceptación brinda la objetividad requerida para que se consideren Terminadas durante la revisión del Sprint. |
| Criterio de Aceptación 1: |
| El sistema envía una notificación automática vía Telegram (SC-02) al jefe de fundo y<br>SUPERVISOR DE ZONA INMEDIATAMENTE DESPUÉS DE QUE EL ESPECIALISTA CONFIRMA UN HALLAZGO FITOSANITARIO (RF10). |
| Criterio de Aceptación 2:<br>La notificación incluye la ubicación exacta (lote, hilera, lado, segmento y marcador), la foto y los botones «Ver caso» y «Cómo llegar». |
| Criterio de Aceptación 3:<br>Ninguna alerta se emite de forma autónoma por la IA sin la confirmación previa del especialista fitosanitario (RF09, RF10), salvo la alerta temprana de los casos «Confirmado por IA», que se envía marcada «falta la revisión del especialista» (SC-03). |
| CASOS LÍMITE: SON PROBLEMAS O SITUACIONES QUE OCURREN EN CASOS EXTREMOS (MÁXIMOS O MÍNIMOS). SE DEBE ESPECIFICAR QUÉ ES LO QUE SE DEBE HACER EN TALES CASOS PARA CONSIDERAR COMO ACEPTADA LA HISTORIA<br>DE USUARIO. |
| Caso Límite 1:<br>Descarte del hallazgo por parte del especialista → El sistema registra el evento en el historial de auditoría pero NO emite ninguna notificación por Telegram. |
| Caso Límite 2:<br>Falla temporal en el servicio externo de Telegram Bot API → La plataforma encola la notificación y reintenta el envío sin bloquear la confirmación realizada en la web. |
| ELEMENTO NO FUNCIONAL: EN CASO DE QUE LA HISTORIA DE USUARIO SE TRATE DE UN REQUISITO O ELEMENTO NO FUNCIONAL PUEDE SER NECESARIO ESPECIFICAR DATOS ADICIONALES. |
| FRECUENCIA: ¿CUÁN FRECUENTEMENTE LA PERSONA DEL ROL IDENTIFICADO NECESITARÁ ESTA HISTORIA DE<br>Usuario? |
| Eventual (asociada a la confirmación de hallazgos positivos). |
| URGENCIA: ¿CUÁN RÁPIDAMENTE TENDRÁ QUE RESPONDER LA APLICACIÓN A LAS NECESIDADES DEL USUARIO? |
| Envío de la alerta inmediatamente después de la confirmación humana (RF10). |
| VOLUMEN: ¿CUÁNTO VOLUMEN DE DATOS TENDRÁ QUE MANTENER LA APLICACIÓN PARA ESTA HISTORIA DE USUARIO? |
| Escalable según la cantidad de focos detectados en el fundo. |
| PRECISIÓN: ¿CUÁN PRECISA Y OPORTUNA TIENE QUE SER LA DATA DESDE LA PERSPECTIVA DEL NEGOCIO? |
| Mensaje 𝟏𝟎𝟎% verídico respaldado por la validación del agrónomo. |
| USABILIDAD: ¿CUÁLES CARACTERÍSTICAS HARÁN QUE LA APLICACIÓN SEA FÁCILMENTE USABLE POR EL ROL IDENTIFICADO? |
| Notificación directa en el teléfono móvil del supervisor vía Telegram. |
| FACILIDAD DE APRENDIZAJE: ¿CUÁN RÁPIDAMENTE PUEDEN LOS NUEVOS USUARIOS, APRENDER A USAR LA APLICACIÓN? |
| Ninguna curva de aprendizaje; lectura de mensaje en app de uso cotidiano. |
| FLEXIBILIDAD/ESCALABILIDAD: ¿CUÁN RÁPIDAMENTE SE PUEDE ADAPTAR LA APLICACIÓN A LOS CAMBIOS EN FRECUENCIA Y VOLUMEN? |
| Configuración de múltiples destinatarios de Telegram (supervisores por zona). |
| CONFIABILIDAD: ¿CUÁN CRÍTICA ES LA APLICACIÓN PARA QUE NO DEBA FALLAR? |
| Acceso restringido por autenticación JWT y roles en el backend para evitar alertas no autorizadas (RNF05). |
