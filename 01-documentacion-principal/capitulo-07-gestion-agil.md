# CAPÍTULO VII. GESTIÓN ÁGIL

## 7.1. Equipo Scrum

El equipo de trabajo opera bajo los roles definidos por el marco de trabajo Scrum:

- **Product Owner:** Asumido por la jefatura de fundo, quien representa las necesidades del negocio, prioriza el Product Backlog, valida los criterios de aceptación y aprueba los incrementos funcionales. El especialista fitosanitario participa como consultado técnico para validar los aspectos relacionados con la identificación y revisión de posibles indicios de plaga.

- Scrum Master: Asumido por Grace Lucia Peña Reyes (analista funcional); lidera el proceso ágil, facilita los eventos del marco Scrum y remueve los impedimentos técnicos u operativos que puedan afectar el avance del equipo.

- **Developers:** Estudiantes de ingeniería de sistemas responsables del desarrollo de la app móvil, plataforma Django, base de datos, integración

de Cloudinary, worker de IA, web de revisión, pruebas y apoyo técnico al prototipo físico.

## 7.2. Product Backlog

El Product Backlog es el repositorio dinámico de requisitos del producto, ordenado por prioridad y valor para el negocio. Contiene épicas, historias de usuario y tareas técnicas necesarias para transformar el proceso manual basado en Excel en un sistema digital con captura en campo, almacenamiento offline, sincronización, análisis asistido por IA, revisión especializada, mapas, reportes y avisos oficiales.

## 7.3. Épicas

- **Épica 1: Prototipo físico de captura bilateral.** Diseño, construcción y calibración del soporte fijado al brazo del operador, compuesto por poste vertical y barra tipo T para sostener dos dispositivos móviles a la misma altura.

- **Épica 2: Módulo móvil de captura offline y calidad.** Desarrollo con React Native, Expo y TypeScript para gestionar sesiones, vincular dispositivos, capturar evidencia, validar nitidez y exposición, almacenar en SQLite y sincronizar con la API.

- **Épica 3: Inteligencia artificial y georreferenciación.** Implementación de API REST, Supabase PostgreSQL, Cloudinary, tabla ai_tasks y worker YOLO para procesar capturas y registrar indicios preliminares.

- **Épica 4: Plataforma web, mapas y revisión de casos.** Creación de la web con Django Templates, HTMX y Leaflet/OpenStreetMap para bandeja de casos, revisión de evidencia, decisión del especialista, reportes y envío de avisos por Telegram tras confirmación (SC-02).

## 7.4. Historias de usuario

- **HU01:** Como operador de monitoreo, quiero portar un prototipo físico de captura bilateral fijado al brazo, con soporte tipo T para dos dispositivos móviles, para capturar ambos lados de las plantas en una sola pasada sin sostener los equipos con las manos.

- **HU02:** Como operador, quiero que la aplicación móvil verifique automáticamente la nitidez y la iluminación de cada foto antes de cambiar de zona, para evitar registrar imágenes inservibles.

- **HU03:** Como especialista fitosanitario, quiero visualizar los posibles indicios señalados por la IA sobre un mapa interactivo del fundo, junto con evidencia fotográfica y cajas de detección, para confirmar o descartar el caso con trazabilidad.

- **HU04:** Como supervisor o jefe de fundo, quiero recibir notificaciones por mensajería oficial solo cuando un caso haya sido confirmado por el especialista fitosanitario, para coordinar la respuesta operativa correspondiente.

- **HU05:** Como administrador de la plataforma, quiero gestionar usuarios, roles, dispositivos, catálogos y destinatarios de avisos desde Django Admin o pantallas internas, para mantener el sistema controlado durante el piloto.

## 7.5. Criterios de aceptación

- La app móvil valida la calidad básica de cada captura considerando nitidez y exposición antes de almacenarla como evidencia utilizable.

- Si una captura presenta deficiencias de calidad, la interfaz muestra una alerta visual indicando la cámara (Cámara 1 o Cámara 2) cuya toma debe repetirse, con un máximo de 2 intentos; en el último la foto se acepta y se marca para revisión técnica (SC-05).

- Las imágenes aprobadas se almacenan como archivos locales privados y los metadatos se registran en SQLite hasta completar la sincronización.

- La sincronización con Django debe ser idempotente, de modo que reintentar no duplique sesiones, capturas ni tareas de IA.

- La foto se sube a Cloudinary mediante ticket firmado por Django y la captura se confirma en la API antes de crear su tarea ai_tasks.

- La plataforma web permite que solo el especialista fitosanitario confirme, descarte o marque evidencia insuficiente.

- El aviso por Telegram (SC-02) se envía después de una confirmación humana registrada en la plataforma; los casos «Confirmado por IA» generan una alerta temprana marcada «falta la revisión del especialista» (SC-03).

## 7.6. Definition of Ready

- La historia de usuario cumple con el formato estándar y los principios INVEST.

- Los criterios de aceptación están redactados y validados por el Product Owner.

- Las dependencias técnicas, de datos o de hardware necesarias se encuentran disponibles.

- La historia identifica si afecta a la app móvil, plataforma Django, IA, prototipo físico o flujo de campo.

## 7.7. Definition of Done

1\. El código fuente ha sido desarrollado, probado y revisado en el repositorio correspondiente.

2\. La funcionalidad cumple los criterios de aceptación establecidos.

3\. La funcionalidad se encuentra integrada en el entorno de pruebas, validada en el prototipo físico o demostrada mediante flujo end-to-end.

4\. Los cambios relevantes de arquitectura, seguridad, datos o operación quedan documentados.

## 7.8. Sprint 1

1\. Configuración de los repositorios riachuelo-mobile y riachuelo-platform.

2\. Definición de la arquitectura inicial; la arquitectura vigente se adoptó luego con la SC-01 (22/09/2026) y se ajustó con la SC-07: app móvil React Native + Expo + TypeScript; plataforma Django; Supabase; Cloudinary; ai_tasks; worker YOLO; web HTMX.

3\. Diseño conceptual y selección de materiales para el prototipo físico de captura bilateral fijado al brazo.

## 7.9. Sprint 2

1\. Desarrollo de la interfaz móvil para control de sesiones, pasadas y capturas.

2\. Implementación de validaciones locales de nitidez, exposición, permisos, GPS y almacenamiento offline.

3\. Pruebas iniciales de ensamblaje del soporte físico, ubicación de dispositivos y estabilidad durante el recorrido.

## 7.10. Sprint 3

1\. Implementación del almacenamiento local con SQLite, archivos privados y SecureStore.

2\. Definición del contrato de sincronización con la API REST de Django y manejo de reintentos.

3\. Recolección y etiquetado inicial de muestras fotográficas para el entrenamiento del modelo de IA.

## 7.11. Sprint 4

1\. Desarrollo de la plataforma Django: modelos, migraciones, API REST, autenticación y permisos.

2\. Construcción de la web de revisión con plantillas Django, HTMX y Leaflet/OpenStreetMap.

3\. Creación de la bandeja de casos y la interfaz donde el especialista confirma, descarta o solicita nueva evidencia.

## 7.12. Sprint 5

1\. Integración de la sincronización móvil con Django y carga de imágenes a Cloudinary mediante ticket firmado.

2\. Implementación de la tabla ai_tasks, worker YOLO, registro de detecciones y apertura de casos de revisión.

3\. Integración de Telegram Bot API para avisos posteriores a la confirmación del especialista (SC-02: reemplaza a WhatsApp Cloud API, retirada por los requisitos de cuenta de empresa Meta, número verificado y plantillas aprobadas).

4\. Pruebas integrales end-to-end desde captura móvil hasta notificación confirmada.

## 7.13. Sprint 6

1\. Despliegue de la versión piloto de la plataforma y preparación de dispositivos móviles.

2\. Ejecución de pruebas operativas en condiciones reales de campo con el prototipo físico.

3\. Medición y análisis comparativo de indicadores: tiempo de ejecución, cobertura, errores, trazabilidad y costos frente al proceso tradicional en Excel.

4\. Cierre del piloto, registro de lecciones aprendidas y recomendaciones de escalamiento.

## 7.14. Sprint Reviews

Son reuniones de demostración funcional al finalizar cada Sprint. El equipo presenta avances tangibles de la app móvil, plataforma Django, web de revisión, IA o prototipo físico ante la jefatura de fundo y el especialista fitosanitario, obteniendo retroalimentación directa para ajustar el Product Backlog.

## 7.15. Sprint Retrospectives

Son espacios de análisis interno del equipo Scrum al cierre de cada Sprint. Permiten evaluar desempeño, identificar cuellos de botella técnicos u operativos y definir acciones de mejora para el siguiente ciclo, especialmente en sincronización, calibración física, calidad de imágenes y revisión de casos.

## 7.16. Burndown Chart

El Burndown Chart representa el trabajo restante frente al tiempo disponible en cada Sprint. Su uso permite medir la velocidad del equipo, detectar desviaciones y ajustar el alcance de tareas sin comprometer los entregables críticos del piloto.

## 7.17. Registro de impedimentos

Es una bitácora administrada por el Scrum Master para registrar bloqueos técnicos, problemas de conectividad, fallas de sincronización, errores de Cloudinary, indisponibilidad de Supabase, retrasos en el dataset, problemas de calibración del prototipo o limitaciones temporales de recursos. Cada impedimento debe tener responsable, fecha de registro, estado y acción correctiva.
