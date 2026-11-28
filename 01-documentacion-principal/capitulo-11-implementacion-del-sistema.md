# CAPÍTULO XI. IMPLEMENTACIÓN DEL SISTEMA

## 11.1. Entorno de desarrollo

El entorno de desarrollo del sistema se encuentra dividido de acuerdo con los dos principales componentes del proyecto: la aplicación móvil y la plataforma central. Esta separación permite que ambos componentes puedan desarrollarse y probarse de manera independiente, manteniendo contratos de comunicación definidos entre ellos.

Para la aplicación móvil se utiliza React Native con Expo y TypeScript, permitiendo desarrollar una única aplicación compatible con dispositivos Android e iOS; en el piloto se usan celulares Android y el uso en iPhone queda pendiente de validar (SC-07). El entorno de trabajo considera Node.js, Git y Visual Studio Code como herramientas principales de desarrollo, mientras que Android Studio y JDK se utilizan para las compilaciones y pruebas específicas en dispositivos Android. La arquitectura móvil incorpora además almacenamiento local, acceso a cámara, ubicación, manejo de archivos y comunicación entre los dispositivos utilizados durante el monitoreo. El documento técnico establece React Native, React, TypeScript y Node.js como componentes base del entorno móvil.

Para la plataforma central se utiliza Python y Django, junto con Django REST Framework para los servicios REST. La interfaz web emplea plantillas de Django, HTMX y CSS, mientras que Leaflet sobre teselas satelitales Esri y OpenStreetMap permiten representar geográficamente los hallazgos. Como servicios externos se utilizan Supabase para la base de datos PostgreSQL y Cloudinary para el almacenamiento de las evidencias fotográficas.

De esta manera, el entorno de desarrollo mantiene dos proyectos principales: riachuelo-mobile, correspondiente a la aplicación móvil, y riachuelo-platform, que concentra la API, plataforma web y procesamiento de inteligencia artificial. Esta división permite que los integrantes desarrollen componentes diferentes sin mantener todos los elementos del sistema dentro de un único proyecto.

## 11.2. Git/GitHub

El proyecto utiliza Git como sistema de control de versiones y GitHub como plataforma central para almacenar y compartir el código fuente entre los integrantes del equipo. Esto permite mantener un historial de los cambios realizados, identificar qué integrante realizó cada modificación y recuperar versiones anteriores cuando sea necesario.

El código se encuentra organizado principalmente en los repositorios riachuelo-mobile para la aplicación móvil y riachuelo-platform para la plataforma Django. De forma adicional, puede mantenerse un repositorio independiente para el entrenamiento y versionamiento del modelo de inteligencia artificial.

El procedimiento de trabajo considera que cada funcionalidad desarrollada sea probada antes de registrar sus cambios mediante un commit. La documentación técnica establece como parte del flujo de desarrollo probar el resultado, revisar el código y posteriormente

realizar el commit correspondiente, permitiendo mantener evidencia del avance de cada tarea.

GitHub también facilita el trabajo colaborativo mediante la utilización de ramas y solicitudes de integración, evitando que los integrantes modifiquen directamente la versión estable del sistema mientras se encuentran desarrollando o probando nuevas funcionalidades.

## 11.3. Estrategia de ramas

Los documentos técnicos del proyecto establecen el uso de repositorios Git y el registro de los cambios mediante commits, pero no determinan de forma explícita los nombres de las ramas. Por ello, para el desarrollo del proyecto se adopta una estrategia simplificada basada en una rama estable y una rama de integración, compatible con la forma en que actualmente se está trabajando en GitHub.

La rama main representa la versión estable del sistema y contendrá únicamente funcionalidades previamente desarrolladas y verificadas. La rama development será utilizada como rama principal de integración durante el desarrollo de los Sprint. Los integrantes trabajarán desde ramas temporales derivadas de development, por ejemplo feature/login, feature/mapa, feature/sincronizacion o fix/error-captura.

Una vez terminada y probada una funcionalidad, los cambios se enviarán nuevamente hacia development mediante una solicitud de integración o Pull Request. Cuando las funcionalidades correspondientes a una versión o Sprint hayan sido verificadas de forma conjunta, development podrá integrarse con main. De esta manera se evita realizar modificaciones directas sobre la versión estable y se reducen los conflictos cuando varios integrantes trabajan simultáneamente.

Las ramas utilizadas únicamente para experimentación, como una rama denominada prueba, pueden emplearse temporalmente; sin embargo, las funcionalidades aprobadas deberán incorporarse posteriormente a development para conservar un flujo de trabajo ordenado.

## 11.4. Desarrollo Front-End

El desarrollo Front-End del proyecto comprende dos interfaces principales: la aplicación móvil y la plataforma web.

La aplicación móvil es desarrollada mediante React Native, Expo y TypeScript. Está orientada principalmente al operador de campo y funciona en los tres dispositivos utilizados durante el monitoreo: CONTROLADOR, CAMERA_1 y CAMERA_2. La aplicación permite gestionar las sesiones de monitoreo, realizar capturas fotográficas, validar la calidad de las imágenes, obtener información de ubicación y almacenar temporalmente los datos cuando no existe conexión a Internet. Posteriormente, el dispositivo controlador sincroniza la información con la plataforma central.

La plataforma web utiliza plantillas Django + HTMX, reemplazando la propuesta anterior basada en Angular. HTMX permite actualizar determinadas partes de las páginas mediante solicitudes al servidor sin implementar una aplicación SPA independiente. Para las representaciones geográficas se utiliza Leaflet sobre teselas satelitales Esri y OpenStreetMap.

La interfaz web permite al especialista fitosanitario consultar la bandeja de casos, visualizar fotografías con las cajas generadas por la IA, revisar su ubicación y confirmar, marcar como posible plaga, descartar o marcar un caso como evidencia insuficiente, y corregir una decisión indicando el motivo. Asimismo, los supervisores pueden acceder al panel, mapas, planos de hileras y sesiones registradas.

## 11.5. Desarrollo Back-End

El Back-End del sistema se desarrolla utilizando Python con Django y Django REST Framework. Django actúa como plataforma central y concentra la lógica de negocio, autenticación, API REST, gestión de cuentas, consultas de la plataforma web y coordinación del procesamiento de inteligencia artificial.

Esta arquitectura reemplaza al backend originalmente planteado con Spring Boot. La decisión vigente establece a Django como backend central y mantiene /api/v1 como ruta base para los servicios consumidos por la aplicación móvil.

El backend se encuentra organizado por módulos relacionados con las principales funciones del sistema, como cuentas, campo, monitoreo, evidencias, inteligencia artificial, revisión, notificaciones y auditoría. Las reglas del negocio se mantienen en servicios internos y tanto la API como la plataforma web utilizan estas mismas reglas, evitando duplicar lógica entre diferentes componentes. La estructura definida contempla

además módulos específicos para las API, vistas web, procesamiento de IA y generación de URLs seguras para las imágenes.

La inteligencia artificial también forma parte de la plataforma Django mediante un worker independiente que ejecuta el modelo YOLO26n exportado a ONNX (ONNX Runtime, CPU) para procesar las fotografías. El worker consulta las tareas pendientes, obtiene las imágenes almacenadas en Cloudinary, ejecuta el modelo y registra las detecciones en la base de datos. Con esta reformulación se eliminan RabbitMQ, FastAPI y los callbacks que se habían propuesto en la arquitectura anterior.

## 11.6. APIs

La comunicación entre la aplicación móvil y la plataforma central se realiza mediante una API REST desarrollada con Django REST Framework, cuya ruta base es /api/v1. Los datos son intercambiados principalmente mediante JSON y la autenticación de la aplicación móvil se realiza utilizando tokens JWT emitidos por Django mediante SimpleJWT.

La API permite realizar operaciones relacionadas con autenticación, usuarios, descarga de catálogos, sesiones de monitoreo, pasadas, secuencias, incidencias, capturas y sincronización. La aplicación móvil no accede directamente a la base de datos Supabase, al worker de inteligencia artificial ni al servicio de Telegram; estas operaciones son administradas desde Django.

Para las evidencias fotográficas se utiliza un flujo particular con Cloudinary. La aplicación solicita primero a Django un ticket firmado mediante POST

/captures/{captureId}/upload-ticket. Luego la fotografía se carga directamente en Cloudinary y, una vez finalizada la transferencia, la aplicación confirma la operación en Django mediante POST /captures/upload. De esta forma, el archivo de gran tamaño no necesita atravesar el servidor Django, reduciendo procesamiento y tráfico innecesario en el backend.

La plataforma web no consume una API Front-End independiente para cada acción, debido a que las páginas son generadas directamente por Django. HTMX permite ejecutar determinadas solicitudes parciales al servidor para actualizar secciones específicas, como la bandeja de casos, formularios y estados, manteniendo la seguridad mediante sesiones y protección CSRF.

## 11.7. Integración con BD

La arquitectura utiliza Supabase como servicio de base de datos PostgreSQL administrado. Esta base funciona como repositorio central de la información del sistema y almacena usuarios, lotes, hileras, sesiones, capturas, resultados del modelo de IA, revisiones del especialista, notificaciones y registros de auditoría.

Por razones de seguridad, Supabase no es accedido directamente desde la aplicación móvil ni desde el navegador. Solamente los procesos controlados por Django y el worker tienen acceso a la base de datos. La plataforma web confirma igualmente que Supabase se utiliza sin acceso público y que únicamente los procesos Django se conectan al servicio.

La aplicación móvil dispone adicionalmente de una base de datos SQLite local, necesaria para soportar el funcionamiento sin conexión. Durante el recorrido de campo se almacenan temporalmente las sesiones, secuencias, capturas, estados y otros metadatos. Cuando se recupera la conectividad, esta información es sincronizada mediante /api/v1 con Django, que posteriormente la registra en la base de datos central. El alcance móvil establece expresamente una sincronización idempotente de los datos con Django y una subida separada de las fotografías hacia Cloudinary.

Las fotografías no se almacenan directamente como archivos dentro de PostgreSQL. Cloudinary almacena las imágenes, mientras que Supabase conserva la información necesaria para relacionarlas con la sesión, captura, ubicación, análisis y revisión correspondiente. La arquitectura muestra esta separación entre Cloudinary para fotografías, Django para las reglas y Supabase como base de datos única.

Por lo tanto, el flujo de persistencia queda organizado como Aplicación móvil → API Django → Supabase para los datos estructurados, mientras que las fotografías siguen el flujo Aplicación móvil → Cloudinary → referencia gestionada por Django/Supabase. Esta separación disminuye la carga sobre el backend y mantiene centralizado en Django el control de seguridad y las reglas del negocio.

## 11.8. Integración Front-End/Back-End

La integración entre el Front-End y el Back-End se realiza de manera diferente según el componente cliente. La aplicación móvil se comunica con la plataforma central mediante

la API REST /api/v1 desarrollada con Django REST Framework, utilizando información en formato JSON y autenticación mediante tokens JWT. A través de esta API se realizan operaciones como autenticación, descarga de catálogos, registro de sesiones, sincronización de recorridos y confirmación de capturas.

Para las fotografías se emplea un flujo independiente con Cloudinary. La aplicación solicita primero a Django un ticket firmado, realiza posteriormente la carga directa de la imagen hacia Cloudinary y finalmente comunica al Back-End el resultado de la operación. De esta manera, Django conserva el control de la operación sin recibir directamente el archivo fotográfico, disminuyendo la cantidad de transferencias que debe procesar el servidor.

La plataforma web presenta una integración más directa debido a que el Front-End y el Back-End pertenecen al mismo proyecto riachuelo-platform. Las páginas son generadas mediante plantillas Django, mientras que HTMX permite realizar peticiones parciales al servidor para actualizar determinados componentes sin recargar completamente la página. Por este motivo, la solución no requiere un Front-End separado basado en Angular, React o Vue.

El Back-End concentra las reglas de negocio y el acceso a Supabase, mientras que las interfaces únicamente presentan información y solicitan operaciones autorizadas. Esto permite evitar que el navegador o la aplicación móvil tengan acceso directo a las credenciales y estructuras internas de la base de datos. La arquitectura establece expresamente que Supabase permanece sin acceso público y que únicamente los procesos Django se conectan directamente a la base de datos.

De forma general, la integración queda organizada mediante el flujo Aplicación móvil → API Django → Supabase, mientras que en la plataforma web el flujo corresponde a Navegador → Django/HTMX → Supabase. Para las imágenes se utiliza Aplicación móvil

→ Cloudinary, previa autorización de Django, manteniendo así una integración controlada entre los componentes del sistema.

## 11.9. Control de versiones

El control de versiones del proyecto se realiza mediante Git y GitHub, permitiendo registrar de manera ordenada las modificaciones efectuadas en el código fuente y

conservar un historial de desarrollo de cada componente. Cada cambio significativo es registrado mediante commits, facilitando identificar las funcionalidades incorporadas, correcciones realizadas y responsables de cada modificación.

El proyecto mantiene separados los principales componentes en repositorios como riachuelo-mobile, correspondiente a la aplicación móvil, y riachuelo-platform, que contiene la API, plataforma web y worker de inteligencia artificial. Esta separación permite que cada componente evolucione de manera independiente sin perder la integración establecida mediante sus contratos.

Antes de incorporar una modificación a las ramas principales, el código debe ser revisado y probado. La gestión de calidad del proyecto contempla la revisión de commits y *Pull Requests* antes de fusionar los cambios, así como el uso de reglas de protección sobre las ramas principales.

En la aplicación móvil, el avance también se relaciona con versiones identificables del software. Durante el desarrollo pueden utilizarse versiones de prueba y compilaciones internas; posteriormente se generará una versión congelada destinada al piloto, identificada en la documentación como **v1.0.0-piloto**, la cual deberá superar las pruebas correspondientes antes de ser utilizada en campo. Durante el piloto no se realizarán modificaciones en dicha versión, excepto cuando exista una corrección crítica debidamente registrada.

Asimismo, los documentos técnicos forman parte del control de versiones. Cuando se modifica un contrato relevante, como la estructura de la API, el modelo de datos o las reglas de sincronización, debe actualizarse también la versión de la documentación correspondiente. El archivo maestro de la aplicación establece que cada cambio de contrato debe generar una nueva versión, evitando que el código y la documentación queden desalineados.

De esta manera, el control de versiones no solamente permite conservar el historial del código, sino también mantener la correspondencia entre código, arquitectura, documentación y versiones utilizadas durante las pruebas y el piloto.
