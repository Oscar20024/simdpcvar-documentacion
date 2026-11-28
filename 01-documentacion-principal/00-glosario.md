# Glosario

Agroexportación: Actividad económica orientada al cultivo y procesamiento de productos agrícolas para su comercialización en mercados internacionales.

Almacenamiento Local (Offline-First): Mecanismo de persistencia de datos en el dispositivo móvil que permite capturar y custodiar fotografías y metadatos sin conexión a Internet, sincronizándolos automáticamente con el servidor al restablecerse la señal.

API REST: Interfaz de programación de aplicaciones basada en el protocolo HTTP que permite la comunicación y el intercambio de datos entre la aplicación móvil, la plataforma web y los servicios backend.

Bounding Box (Caja Delimitadora): Región rectangular generada por el modelo de visión artificial sobre una imagen para delimitar la ubicación exacta de un indicio de plaga detectado.

Captura Bilateral: Procedimiento de registro fotográfico en el que cada hilera se recorre por sus dos lados, con la pasada «lateral A» (ida) y la pasada «lateral B» (vuelta); en cada punto, los dos celulares de cámara (Cámara 1 y Cámara 2) fotografían simultáneamente la misma cara de la hilera para obtener doble evidencia (SC-04).

Chanchito Blanco (Planococcus ficus): Plaga de importancia económica que afecta los cultivos de vid mediante la succión de savia, secreción de melaza (asociada a la fumagina) y transmisión del virus de enrollamiento de la hoja (GLRaV-3).

Cloudinary: Servicio de almacenamiento de imágenes en la nube utilizado para alojar de forma privada las fotografías del monitoreo fitosanitario; la app las sube directamente con un ticket firmado por Django y se consultan mediante URL firmadas (reemplaza a Amazon S3, SC-01).

Coeficiente V de Aiken: Coeficiente estadístico utilizado para determinar la validez de contenido de los instrumentos de recolección de datos a partir de la evaluación otorgada por el juicio de expertos.

Cola de tareas de IA (ai_tasks): Tabla de la base de datos en la que el backend Django registra cada fotografía pendiente de análisis; el worker de IA (comando worker_ia) toma las tareas, descarga la foto de Cloudinary con URL firmada, ejecuta el modelo y guarda el resultado directamente, sin callbacks HTTP ni webhooks (SC-01, SC-07).

Django: Marco de trabajo (framework) en Python utilizado para construir el backend principal y la plataforma web; con Django REST Framework expone la API REST (/api/v1) y con SimpleJWT gestiona la autenticación, además de la lógica de negocio y la persistencia (reemplaza a Spring Boot, SC-01).

Efectividad del Monitoreo: Dimensión metodológica que evalúa la calidad, cobertura alcanzada y precisión del registro de evidencias e información sobre los hallazgos fitosanitarios.

Eficiencia Operativa: Dimensión metodológica orientada a medir la optimización de recursos temporales y humanos, evaluando indicadores como personas-hora, tiempo total de monitoreo y segmentos inspeccionados por hora.

Especialista Fitosanitario: Rol profesional encargado de revisar los indicios sugeridos por la Inteligencia Artificial en la plataforma web y confirmar o descartar formalmente los hallazgos en campo.

Esquema O₁ – X – O₂: Representación formal del diseño preexperimental donde

O₁ corresponde a la medición inicial (pretest), X representa la intervención tecnológica (sistema de monitoreo) y O₂ a la medición final (postest).

Fumagina (Negrilla): Capa de hongo oscuro que se desarrolla sobre la melaza excretada por insectos como el chanchito blanco, obstaculizando la fotosíntesis de la planta.

Georreferenciación: Asociación de coordenadas geográficas (GPS) a cada registro fotográfico para aproximar la ubicación espacial del hallazgo dentro del viñedo.

GLRaV-3 (Grapevine Leafroll-associated Virus 3): Virus causante del enrollamiento de la hoja de la vid, cuya transmisión está asociada a la presencia del chanchito blanco.

Hilera: Disposición lineal estructurada de plantas de vid dentro de un lote agrícola.

HTMX (Django Templates): Biblioteca ligera de JavaScript utilizada junto con las plantillas de Django (Django Templates) para construir las páginas de la plataforma web del administrador, el especialista y el supervisor, sin un framework SPA (reemplaza a Angular, SC-01).

Indicio Sugerido: Resultado de carácter no vinculante generado por el modelo de visión artificial que señala posibles áreas con plaga para la evaluación del especialista.

Inferencia: Proceso mediante el cual un modelo de Inteligencia Artificial entrenado analiza una nueva imagen y emite predicciones sobre la presencia de plagas.

Juicio de Expertos: Procedimiento de validación de contenido en el cual metodólogos, ingenieros de sistemas y especialistas agronómicos evalúan la claridad, pertinencia, relevancia y coherencia de los instrumentos de investigación.

JWT (JSON Web Token): Estándar de seguridad utilizado para la autenticación y transmisión segura de información de identidad entre el cliente y el backend.

Leaflet sobre teselas satelitales Esri y OpenStreetMap: Herramientas de cartografía digital utilizadas en la plataforma web para representar interactivamente los mapas de hallazgos en los lotes.

Lote: División territorial o parcela delimitada dentro del fundo agrícola destinada al cultivo de vid.

Marcador Físico: Elemento identificador situado en el campo al inicio de cada segmento de la hilera (10 segmentos fijos de ≈20 m por hilera) que permite referenciar operativamente la ubicación de una captura o hallazgo cuando la señal GPS es limitada.

Monitoreo Fitosanitario: Proceso de inspección periódica en campo destinado a evaluar las condiciones sanitarias del cultivo y detectar plagas u orígenes de infección.

Muestra (Segmentos de Monitoreo): Conjunto representativo de 12 segmentos (300 plantas en total, organizadas en bloques de 25 plantas consecutivas) seleccionados en los lotes del piloto para las evaluaciones comparativas.

Muestreo No Probabilístico Intencional: Criterio de selección de la muestra basado en la accesibilidad, delimitación y disponibilidad operativa del piloto en lugar de una elección aleatoria.

Observación Fitosanitaria: Registro formal complementario efectuado por el especialista tras confirmar la presencia de un hallazgo fitosanitario en la plataforma.

ONNX Runtime: Motor de inferencia con el que el worker de IA de Django (comando worker_ia, en un contenedor propio) ejecuta en CPU el modelo YOLO exportado a formato ONNX, sin un microservicio aparte (reemplaza a FastAPI, SC-07).

Operador de Campo: Personal técnico que porta el soporte tipo T con tres celulares (un controlador y dos cámaras) y se encarga de realizar los recorridos de inspección y efectuar las capturas fotográficas bilaterales con la app móvil (SC-04).

Personas-Hora: Unidad de medida utilizada para cuantificar el esfuerzo del personal empleado en el proceso de inspección fitosanitaria.

Postest (O₂): Medición final de los indicadores de desempeño efectuada tras la implementación e intervención del sistema tecnológico.

PostgreSQL (Supabase): Sistema de gestión de bases de datos relacional, alojado en Supabase, utilizado para almacenar la información de usuarios, recorridos, capturas, casos y ubicaciones (latitud y longitud, sin la extensión PostGIS); solo el backend Django se conecta a la base (SC-07).

Pretest (O₁): Medición inicial de los indicadores de desempeño del proceso de monitoreo fitosanitario ejecutado bajo las condiciones habituales o manuales de la empresa.

Prueba de Shapiro-Wilk: Test estadístico aplicado sobre las diferencias entre pretest y postest para comprobar el supuesto de normalidad en las distribuciones de los datos.

Prueba t de Student / Wilcoxon: Pruebas estadísticas para muestras relacionadas utilizadas en la contrastación de hipótesis, seleccionando la prueba paramétrica (t de Student) o no paramétrica (rangos de Wilcoxon) según el cumplimiento del supuesto de normalidad.

React Native + Expo: Marco de trabajo (framework) multiplataforma basado en TypeScript y React utilizado para la construcción de la aplicación móvil que ejecutan los dispositivos de captura y control en campo.

Segmento de Monitoreo: Unidad operativa de análisis de la investigación conformada por 25 plantas consecutivas sobre una misma hilera de vid. No debe confundirse con el «segmento» del catálogo del sistema (10 segmentos fijos por hilera, de ≈20 m, con un marcador al inicio de cada uno; equivale a la «zona» del informe), que se usa para ubicar capturas y hallazgos.

Telegram Bot API: Servicio de mensajería integrado con el backend para enviar avisos automáticos de los hallazgos con la foto, el lote, la hilera, el lado, el segmento y el marcador, y los botones «Ver caso» y «Cómo llegar» (reemplaza a WhatsApp Cloud API, SC-02).

Trazabilidad: Capacidad de reconstruir el historial, ubicación geográfica y evolución de un hallazgo fitosanitario desde su captura inicial en campo hasta su confirmación y seguimiento.

Unidad de Análisis: Elemento básico sobre el cual se realizan las mediciones e inspecciones del estudio, definido operativamente como el segmento de monitoreo de 25 plantas.

Variable Dependiente (VD): Desempeño del proceso de monitoreo fitosanitario, evaluado a través de las dimensiones de eficiencia operativa, efectividad del monitoreo y trazabilidad y seguimiento.

Variable Independiente (VI): Sistema tecnológico de monitoreo fitosanitario, correspondiente a la solución tecnológica implementada en el estudio.

Vid (Vitis vinifera): Planta leñosa cultivada en la región de Ica para la producción y agroexportación de uva de mesa.

Visión Artificial: Rama de la Inteligencia Artificial enfocada en el procesamiento y análisis automático de imágenes digitales para identificar patrones o la presencia de plagas.

YOLO (You Only Look Once): Algoritmo de detección de objetos en tiempo real utilizado por el worker de IA de Django (modelo YOLO26n exportado a ONNX) para identificar indicios de chanchito blanco y de melaza/fumagina en las fotografías.
