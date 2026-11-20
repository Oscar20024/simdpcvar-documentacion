# ARCHIVO MAESTRO — Web de revisión `riachuelo-platform`

**Sistema inteligente para el monitoreo y detección de plagas en cultivos de vid — Agrícola Riachuelo S.A.C. (Ica)**
Guía única para construir la web de la plataforma Django por módulos con ayuda de una IA.

| Campo | Valor |
|---|---|
| Archivo | `docs/MAESTRO_WEB.md` (este archivo), en el repositorio `riachuelo-platform` |
| Versión | 1.0 — 04/10/2026 (primera versión; arquitectura v3.0: plataforma Django + Supabase + Cloudinary) |
| Estado | Vigente para la construcción. Cada cambio de contrato (modelos, estados, permisos, rutas, formatos de salida) sube la versión (sección 25). |
| Uso | Documento de contexto y de construcción para el equipo y para la IA desarrolladora. **No es un documento de presentación**: prioriza el detalle técnico (arquitectura, tecnologías, estructuras, contratos, decisiones y código probado) sobre la redacción académica. |
| Alcance | La **web de revisión** de la plataforma Django (app `web` del repositorio `riachuelo-platform`): ingreso, panel, bandeja de casos, visor con cajas y decisión del especialista, mapa, plano de hileras, sesiones, reportes, avisos, administración de usuarios, celulares y destinatarios, estado de la IA y auditoría. Incluye además las piezas del servidor que la web necesita para funcionar: el **modelo de datos completo** de todas las apps de Django, los **servicios** de revisión, avisos, cuentas y cola de IA, y las **consultas** del mapa, el plano y el panel. No cubre la API `/api/v1` que usa la app móvil (sección 15 del Maestro App Móvil) ni el detector YOLO por dentro (solo su contrato con la web). |
| Base documental | Maestro App Móvil v2.0 (03/10/2026), en especial §15 (contrato con el backend) y §28 (arquitectura del sistema); ADR «Arquitectura v3.0 — Django + Supabase + Cloudinary» (03/10/2026); documento académico «Curso Integrador II» (§7.3–7.17 épicas, historias y sprints, §8.1–8.5 requerimientos, reglas y casos de uso, §9.3–9.8 arquitectura de información, wireframes, mockups, componentes y diseño responsive); archivo «MAPEO CHANCHITO MOLINOS actualizado»; Especificación técnico-funcional v2.0 y Stack Tecnológico v2.1 (a través del Maestro App Móvil). |
| Precedencia | Para construir la web, este archivo prevalece sobre los mockups y la arquitectura de información del Curso Integrador II (§9) y sobre las secciones de la web del Stack v2.1 (§26) y de la Especificación v2.0 (§18), que describían una web en Angular. En todo lo que comparte con la app (contrato `/api/v1`, nombres de tablas que llena la sincronización, estados de sincronización y de captura), **prevalece el Maestro App Móvil v2.0**; si se encuentra una diferencia, se corrige el archivo equivocado y se sube la versión de ambos. |

---

## 0. Cómo usar este archivo

### 0.1 Qué es

Es el contexto completo que necesita una IA (o un desarrollador) para construir la web **por partes**: reglas, decisiones, stack con versiones, estructura del código, roles y permisos, pantallas, rutas, flujos, requerimientos con criterios de aceptación, modelo de datos, servicios, estados, contratos de salida (mapa, CSV, WhatsApp), seguridad, rendimiento medido, pruebas, plan de tareas y **código de referencia probado** (Anexos B a H). Si un dato no está aquí ni en el Maestro App Móvil, la IA no debe inventarlo: debe preguntarlo (regla W-04).

El código de los anexos es un proyecto Django que **funciona**: se ejecutó contra PostgreSQL 16 y SQLite, pasa 66 pruebas automáticas y se revisó en un navegador real (sección 0.5). Se entrega también como paquete (`riachuelo-platform_web_referencia.zip`) para copiarlo al repositorio.

### 0.2 Cómo lo usa el equipo

1. Guardar este archivo en el repositorio `riachuelo-platform` como `docs/MAESTRO_WEB.md`, junto con `docs/MAESTRO_APP_MOVIL.md` (copia de referencia, sección 28.3 del maestro móvil).
2. Crear en la raíz del repositorio un archivo `AGENTS.md` (o `CLAUDE.md` si usan Claude Code) con esta instrucción: «Antes de cada tarea de la web lee `docs/MAESTRO_WEB.md` y cumple su sección 1. Si la tarea toca la API de la app, lee también `docs/MAESTRO_APP_MOVIL.md` secciones 15 y 28».
3. Si se parte del paquete de referencia, copiar su contenido a la raíz del repositorio (ver sección 5.7, tarea T-W00).
4. Pedir a la IA **una tarea por vez** de la sección 22 usando la plantilla de la sección 23.1.
5. Probar el resultado en el navegador, ejecutar las pruebas (W-20), revisar el código y hacer commit.
6. Actualizar la sección 24 (avance) y, si se decidió algo, la sección 25 (decisiones y preguntas).
7. Si la IA tiene poca memoria de contexto, darle siempre las secciones 1 a 7 más las secciones y anexos que cita la tarea.

### 0.3 Cómo lo lee la IA (orden obligatorio)

1. Sección 1 (reglas permanentes W-01 a W-24).
2. Secciones 2 a 7 (contexto, alcance, decisiones, stack, estructura, roles y permisos).
3. La tarea pedida en la sección 22 y todas las secciones y anexos que esa tarea cita.
4. La sección 24 para saber qué ya existe en el repositorio.
5. El Maestro App Móvil §15 y §28 cuando la tarea toque datos que llegan de la app, la cola de IA, Cloudinary o el despliegue. La web no implementa la API de la app, pero lee las tablas que esa API llena y debe interpretarlas igual.

### 0.4 Estados de validación usados en este archivo

| Estado | Significado |
|---|---|
| **Confirmado** | Decidido por el equipo o por los documentos vigentes del proyecto (Maestro App Móvil v2.0, ADR v3.0). |
| **Verificado** | Comprobado con el código de referencia de los anexos (pruebas automáticas, ejecución en PostgreSQL, navegador) o contra documentación y paquetes oficiales en la fecha indicada. |
| **Propuesto** | Diseño de este archivo. Se implementa tal cual hasta que el equipo lo cambie con evidencia (registrar el cambio en la sección 25). |
| **Decidir** | Depende de una pregunta abierta (sección 25.3); mientras tanto vale el valor propuesto. |
| **Medir** | Meta que se confirma con mediciones en el piloto. |

### 0.5 Qué se validó en esta versión

- **Versiones de paquetes:** consultadas en PyPI y en el registro npm el 03 y 04/10/2026 (sección 5.2 y 5.3). Django 5.2.17 es la última versión del LTS 5.2; Django 6.1.1 ya existe, pero `djangorestframework-simplejwt` 5.5.1 declara soporte solo hasta Django 5.2, por eso se mantiene el LTS. HTMX 2.0.11 es la versión estable (la etiqueta `next` apunta a 4.0.0) y Leaflet 1.9.4 la estable (2.0.0 sigue en alfa).
- **Código de referencia (Anexos B a H):** proyecto Django 5.2.17 con DRF 3.18.1, SimpleJWT 5.5.1, django-htmx 1.29.0, cloudinary 1.46.2, psycopg 3.3.6 y whitenoise 6.12.0 en Python 3.13 (compatible con 3.12, que es la versión de producción).
  - `python manage.py check` sin observaciones; `makemigrations --check` sin cambios pendientes; `check --deploy` con configuración de piloto solo advierte `SECURE_HSTS_INCLUDE_SUBDOMAINS` y `SECURE_HSTS_PRELOAD` (decisión consciente, sección 16).
  - **66 pruebas automáticas** pasan en **PostgreSQL 16** (incluidas una prueba de concurrencia real con dos hilos que deciden el mismo caso y dos pruebas de seguridad de Supabase: RLS en todas las tablas y roles `anon`/`authenticated` sin privilegios). En SQLite pasan 63 y se omiten esas 3 (sección 21.1).
  - La web se recorrió en **Chromium** (Playwright) a 1440 px (escritorio), 1024 px (tablet) y 390 px (celular), con fotos y teselas simuladas (Anexo A).
  - **Rendimiento medido** con 100 576 capturas, 93 872 análisis y 18 798 casos en PostgreSQL local: todas las pantallas responden en el servidor por debajo de 1,5 s (RNF04 del curso); la más pesada es el GeoJSON del mapa sin filtros (≈ 0,66 s y 258 KB comprimido). Detalle en la sección 18.
- **Hallazgos al validar** (ya corregidos en el código y convertidos en reglas):
  - `SECURE_REFERRER_POLICY` de Django vale `same-origin` por defecto y **no envía Referer** a otros dominios; la política de teselas de OpenStreetMap exige un Referer válido. Se usa `strict-origin-when-cross-origin` (sección 16).
  - **HTMX 2 no reemplaza el contenido cuando la respuesta es 4xx**: los errores 403, 409 y 422 de la web traen un fragmento con el motivo y se habilitan con `htmx-config` (patrón P-4, sección 6.4).
  - Con `LANGUAGE_CODE = "es-pe"` Django **escribe los decimales con coma**: un estilo `left: 46,48%` es inválido y el navegador lo ignora. Los números que van a atributos, CSS o JavaScript se escriben con `|stringformat` (W-16).
  - En PostgreSQL, `select_for_update()` junto con `select_related()` de una relación que puede ser nula falla («FOR UPDATE cannot be applied to the nullable side of an outer join»): se usa `select_for_update(of=("self",))` (W-06).
  - `createcachetable` crea la tabla de caché **fuera** de `migrate` y quedaba sin RLS: la tabla se crea en una migración (Anexo B.9).
  - La política de contraseñas de Django por defecto no exigía letras **y** números como la app (`validatePassword`): se agregó `LettersAndDigitsValidator`, y la contraseña temporal se genera cumpliéndola (sección 7.6).
- **Servicios externos consultados el 03–04/10/2026:** política de uso de teselas de OpenStreetMap (atribución visible, Referer o User-Agent identificable, sin descarga para uso sin conexión) y reglas de WhatsApp Business (fuera de la ventana de 24 h solo se pueden enviar **plantillas aprobadas**; según una fuente secundaria, desde el 01/10/2026 las plantillas de utilidad dentro de la ventana también se cobran). Fuentes en la sección 27.

### 0.6 Relación con el Maestro App Móvil

| Tema | Dónde está definido | Qué hace la web |
|---|---|---|
| Arquitectura del sistema, Supabase, Cloudinary, cola `ai_tasks`, worker | Maestro App Móvil §28 | La respeta; agrega los detalles que solo usa la web (casos, avisos, URLs firmadas, consultas). |
| Contrato `/api/v1` (login de la app, sincronización, ticket de subida, confirmación) | Maestro App Móvil §15 | No lo implementa ni lo cambia. Lee las tablas que llena. |
| Tablas de sincronización (`monitoring_*`, `capture_sequences`, `captures`, `quality_results`, `incidents`) | Maestro App Móvil §28.5 (nombres) y este archivo, Anexo B (modelos Django) | Solo lectura (W-14). |
| Usuarios, estados de cuenta, roles, celulares | Maestro App Móvil §7 y §28.6 | La web es donde el ADMINISTRADOR aprueba, bloquea, asigna roles, da contraseñas temporales y revoca celulares (sección 7). |
| Estados de IA, revisión y aviso | Maestro App Móvil §28.5 y §28.9 (nombres) | Este archivo define transiciones, quién las cambia y cómo se muestran (sección 13). |
| Casos, decisiones, destinatarios, auditoría de la web | **Este archivo** | Los define (secciones 11 a 15). |

---

## 1. Reglas permanentes para la IA (obligatorias)

- **W-01 Stack fijo.** Python 3.12, **Django 5.2 LTS** (5.2.x), Django REST Framework, django-htmx, whitenoise, cloudinary y psycopg 3 en las versiones de la sección 5.2. La web es **HTML generado en el servidor** con plantillas Django y **HTMX 2.0.x**; mapas con **Leaflet 1.9.4**. Ambas librerías se guardan como archivos estáticos en `web/static/web/vendor/` (sin CDN). Prohibido en la web: React, Angular, Vue, Svelte, bundlers (Vite, webpack), Tailwind con compilación, `npm` como parte del proyecto, HTMX 4 y Leaflet 2 (alfa).
- **W-02 Capas.** Plantilla → vista (`web/views.py`) → servicio (`<app>/services.py`) o consulta de solo lectura (`web/queries.py`) → modelo. Las vistas validan la entrada con formularios y llaman a servicios; no contienen reglas de negocio. Única excepción: la configuración de destinatarios (CRUD simple con auditoría).
- **W-03 Una regla, un lugar.** Las reglas de revisión viven en `revision/services.py`; las de cuentas en `cuentas/services.py`; las de avisos en `notificaciones/services.py`; las de la cola de IA en `ia/services.py`. Django Admin no ofrece acciones que salten esos servicios (sección 7.8).
- **W-04 No inventar.** No crear requisitos, campos, estados, permisos, rutas ni mensajes que no estén aquí o en el Maestro App Móvil. Si falta algo, detenerse y listarlo en «Preguntas» del reporte. Si se decide algo menor para avanzar, marcarlo como «Supuesto».
- **W-05 Contratos exactos.** Modelos (Anexo B), estados y transiciones (sección 13), matriz de permisos (sección 7.2), rutas (sección 6.3) y formatos de salida (GeoJSON, CSV y WhatsApp, sección 15) se implementan tal como están. Cambiarlos exige actualizar este archivo y su versión.
- **W-06 Transacciones y bloqueos.** Toda escritura que toque más de una tabla usa `transaction.atomic()`. Para decidir o corregir un caso, primero se bloquea la fila con `select_for_update(of=("self",))` (el `of` es obligatorio cuando hay `select_related` de relaciones que pueden ser nulas).
- **W-07 Permisos en el servidor.** Toda vista de la web, salvo ingresar, salir y cambiar la contraseña, lleva `@web_view("<permiso>")` con un permiso de la matriz (sección 7.2); una prueba automática falla si alguna ruta no lo tiene. Los servicios vuelven a comprobar las reglas críticas (RN-W01). Ocultar un botón nunca es la única protección.
- **W-08 Sesión y CSRF.** La web usa la sesión de Django (cookie) y CSRF. Todo formulario POST lleva `{% csrf_token %}`; las peticiones HTMX envían `X-CSRFToken` desde `hx-headers` del `<body>`. Prohibido `@csrf_exempt` en la web.
- **W-09 Sin secretos en el navegador.** Ninguna página, script, atributo ni log contiene `DJANGO_SECRET_KEY`, `DATABASE_URL`, `CLOUDINARY_URL` (API secret), `WHATSAPP_TOKEN` ni claves de Supabase. El navegador nunca habla con Supabase, con la API de administración de Cloudinary ni con WhatsApp.
- **W-10 Fotos.** Las URLs firmadas de Cloudinary se generan al renderizar (`evidencias/media.py`), nunca se guardan en la base de datos ni en logs, y solo aparecen en páginas con sesión iniciada. Variantes: `miniatura` (listas), `revision` (visor) y `original` (solo cuando el especialista pulsa «Original»).
- **W-11 Lenguaje de la IA.** La web dice «indicio sugerido por IA», nunca «plaga detectada», «diagnóstico», «infestación» ni «severidad». El aviso «Indicio sugerido por IA: no es un diagnóstico. La decisión es del especialista» (`messages.IA_AVISO`) es visible en la bandeja y en cada caso de origen IA. El campo `severity` de las incidencias viene del contrato de la app y se muestra como «Nivel».
- **W-12 Colores = estado de revisión.** Los colores de casos, mapa y plano indican el estado de revisión, nunca gravedad ni urgencia. Todo estado se muestra con **icono y texto** además del color.
- **W-13 Fuera de alcance.** No implementar tratamientos, dosis, calendarios de aplicación, niveles de urgencia, recomendaciones agronómicas, diagnóstico automático, simulador de la app en la web ni edición de datos de campo.
- **W-14 Evidencia inmutable.** La web nunca crea, edita ni borra sesiones, pasadas, secuencias, capturas, resultados de calidad ni incidencias, y nunca borra análisis, cajas, decisiones, avisos ni eventos de auditoría. Las únicas escrituras permitidas sobre esas tablas son las de RN-W08 y la sección 11.4: `detections.review_status` al decidir, `human_reviews.is_current = false` al corregir (la corrección crea otra fila, sección 8.5), un aviso pendiente anulado (`NO_APLICA`) y reencolar una tarea con error. Django Admin muestra esas tablas en solo lectura.
- **W-15 Auditoría.** Toda acción que cambie un estado oficial, una cuenta, un celular, un destinatario o la cola de IA registra un evento con `auditoria.services.record` **dentro de la misma transacción**.
- **W-16 Idioma y formatos.** Textos de interfaz en español (Perú); mensajes que no son etiquetas de campos en `web/messages.py`. Identificadores de código en inglés. Fechas guardadas en UTC y mostradas en hora de Lima (`dd/mm/aaaa HH:mm`). Los números que van a atributos HTML, CSS o JavaScript se escriben **sin localizar** (`|stringformat:".2f"` o `{% localize off %}`); los que lee una persona sí se localizan (coma decimal).
- **W-17 Sin JavaScript en línea.** Todo script va en `web/static/web/*.js`; los datos llegan por atributos `data-*` o `{{ valor|json_script }}`. Prohibidos `onclick=`, `onchange=` y `<script>` con código dentro de las plantillas (deja la puerta abierta a una CSP estricta, sección 16).
- **W-18 HTMX con patrones fijos.** Solo los patrones P-1 a P-9 de la sección 6.4. La misma URL devuelve página completa o fragmento según `request.htmx`; un POST exitoso responde `HX-Redirect`; los errores 403, 409 y 422 devuelven un fragmento con el motivo.
- **W-19 Consultas acotadas.** Sin consultas N+1 (`select_related`, `prefetch_related`, agregados en la base de datos); toda lista que crece con el uso (casos, sesiones, avisos, auditoría) se pagina; las listas administrativas pequeñas (usuarios, celulares, destinatarios) no; el detalle de una sesión muestra hasta 500 fotos; el mapa y la exportación tienen tope (sección 17). Las pruebas fijan el número de consultas de la bandeja (`assertNumQueries(6)`).
- **W-20 Pruebas.** Toda regla y toda vista tienen pruebas. Antes de entregar: `python manage.py check`, `python manage.py makemigrations --check --dry-run` y `python manage.py test` contra PostgreSQL sin errores.
- **W-21 Pocas dependencias.** Antes de agregar un paquete de Python o una librería JavaScript, justificarlo en el reporte; preferir lo que ya está en la sección 5.
- **W-22 Datos de prueba.** `python manage.py sembrar_demo` solo existe para desarrollo y se niega a correr con `APP_ENV=piloto`. Nunca mezclar datos o fotos de prueba con evidencia del piloto (prefijo de Cloudinary por entorno, D-32 del maestro móvil).
- **W-23 Accesibilidad.** Contraste AA, foco visible con teclado, `alt` en todas las fotos, `aria-current` en el menú, formularios con `<label>`, mensajes con `role="status"` o `role="alert"`.
- **W-24 Reporte.** Al terminar cada tarea, entregar el reporte de la sección 23.2.

---

## 2. Contexto del proyecto (visto desde la web)

- **Cómo se trabaja hoy.** El monitoreo del chanchito blanco se registra en matrices de Excel. El archivo «MAPEO CHANCHITO MOLINOS actualizado» tiene una hoja por lote (LOTE 1 «Piscina», LOTE 2 «Maíz», LOTE 3 «Cerro», LOTE 4 «Ciudad», LOTE 5 «Triángulo», LOTE 6 «Bota», LOTE 7 y LOTE 8), donde cada fila es una hilera con su número de plantas y cada celda una planta; las plantas con chanchito se pintan de rojo. La hoja RESUMEN suma por lote: SWG 1 con 97 de 9 987 plantas (0,97 %), SWG 2 con 331 de 4 843 (6,83 %), SWG 3 con 80 de 6 017 (1,33 %), SWG 4 con 604 de 13 398 (4,51 %), SWG 5 con 587 de 4 836 (12,14 %) y LOTE 6 con 0 de 1 656. No hay foto, ni hora, ni ubicación, ni quién lo vio.
- **Qué agrega el sistema.** La app captura fotos por hilera y lateral con contexto (lote, hilera, segmento, marcador, GPS, hora, operador y celular). El worker sugiere indicios con YOLO. **La web es donde la evidencia se convierte en una decisión**: el especialista ve la foto con las cajas de la IA y la ubicación, confirma, descarta o marca evidencia insuficiente, y la confirmación genera un aviso por WhatsApp a quien debe actuar. Todo queda auditado y se puede consultar en mapa, plano y panel.
- **Quién usa la web.** El **especialista fitosanitario** (revisa y decide), el **supervisor** (supervisor de zona y jefe de fundo: consulta casos y recibe el aviso; abre el enlace del WhatsApp en su celular) y el **administrador** (cuentas, celulares, destinatarios, catálogos y cola de IA). El operador de campo usa solo la app (DW-05).
- **Qué no hace la web.** No recibe fotos (las sube la app directo a Cloudinary), no ejecuta la IA (lo hace el worker), no envía WhatsApp durante la petición (lo hace el worker) y no decide tratamientos.
- **Piloto.** Lotes SWG 1, SWG 2 y SWG 5; 12 segmentos de 25 plantas (n = 12 pares pretest–postest); postest previsto del 9 al 18 de noviembre de 2026. La web debe estar desplegada y probada antes (sección 22, fase FW6).
- **Equipo.** El backend y la web viven en el mismo repositorio (`riachuelo-platform`) y los construye el responsable del backend con apoyo de IA. La app la construye el equipo móvil.

---

## 3. Alcance de la web

**Incluye:**

- Ingreso con correo y contraseña (sesión de Django), cambio de contraseña y contraseña temporal obligatoria; bloqueo temporal por intentos fallidos.
- **Panel** con indicadores del periodo: casos pendientes y su antigüedad, decisiones, tiempo hasta la decisión, sesiones, fotos y rechazos por calidad, cola y errores de IA, avisos y cobertura por lote (RN-13 del maestro móvil).
- **Bandeja de casos** con filtros, orden, paginación y actualización automática.
- **Caso**: visor de la foto con las cajas de la IA (acercar, mover, original bajo pedido), ubicación operativa, otra cámara de la misma secuencia, decisión del especialista con observación, clase confirmada opcional y cajas incorrectas, «Guardar y siguiente», corrección con motivo e historial.
- Apertura **manual** de un caso sobre una foto sin indicios o con error de análisis.
- **Mapa** (Leaflet + OpenStreetMap) con los casos que tienen coordenadas y los contornos de lote, y **plano de hileras** esquemático por lote con segmentos y conteos por estado.
- **Sesiones** de monitoreo en solo lectura: cámaras, pasadas, incidencias y fotos con su calidad.
- **Reportes**: exportación CSV de casos y decisiones; impresión del navegador a PDF.
- **Avisos**: lista de envíos de WhatsApp con su estado, y **destinatarios** con alcance por lote y consentimiento.
- **Administración**: usuarios (aprobar, rechazar, bloquear, desbloquear, roles, contraseña temporal), celulares (revocar), estado de la IA (conteos, errores y reencolar) y auditoría.
- Django Admin en `/gestion/` para catálogos, perfiles de calidad y modelos de IA, y consulta de solo lectura del resto.

**No incluye:** tratamientos, dosis, calendarios, niveles de urgencia o gravedad, recomendaciones agronómicas, diagnóstico automático, alertas «urgentes», el simulador móvil en la web, la edición de datos que llegan de la app, la subida de fotos desde la web, Supabase Auth, Realtime, PDF generado en el servidor (Q-W06) ni mapas sin conexión.

**Trazabilidad con la arquitectura de información del Curso Integrador II (§9.3):**

| Ítem del Integrador | Decisión en esta versión | Pantalla |
|---|---|---|
| Dashboard – indicadores generales del fundo | Incluye | WEB-03 |
| Sesiones – detalle de recorridos por lote/hilera | Incluye (solo lectura) | WEB-09, WEB-10 |
| Casos / IA – revisión y validación de detecciones | Incluye | WEB-04, WEB-05, WEB-06 |
| Mapa – visualización cartográfica de hallazgos | Incluye (mapa + plano esquemático) | WEB-07, WEB-08 |
| Tratamientos – registro y seguimiento de aplicaciones | **No incluye** (W-13; fuera de alcance de la Especificación v2.0) | — |
| Reportes – generación de documentos PDF | Incluye con alcance reducido: CSV + impresión del navegador (Q-W06) | WEB-11 |
| Dispositivos – gestión de cámaras y controladores | Incluye (celulares que iniciaron sesión) | WEB-14 |
| Usuarios – gestión de roles y accesos | Incluye | WEB-13 |
| Alertas WhatsApp – eventos y destinatarios | Incluye: un solo evento (caso confirmado) y destinatarios | WEB-12, WEB-15 |
| Configuración – parámetros generales | Estado de la IA en la web; catálogos, perfiles y modelos en Django Admin; parámetros técnicos por variables de entorno | WEB-16, WEB-18 |
| Simulador móvil (controlador y cámaras) | **No incluye**: lo cubre el simulador de cámaras de la app en Expo Go (RF-48 del maestro móvil) | — |

---

## 4. Decisiones vigentes

| ID | Decisión | Estado | Fuente |
|---|---|---|---|
| DW-01 | Web generada en el servidor con plantillas Django + HTMX 2.0.x; sin SPA. | Confirmado | D-30 del maestro móvil; ADR v3.0 |
| DW-02 | La web vive en el mismo repositorio y en el mismo proceso `web` (gunicorn) que la API `/api/v1`; comparte modelos y servicios con el worker. | Confirmado | D-24, D-30, D-34 |
| DW-03 | Ingreso con la sesión de Django (cookie + CSRF) sobre el mismo modelo de usuario que la app; sin Supabase Auth. | Confirmado | D-28 |
| DW-04 | Los roles se guardan en la tabla `user_roles` (un usuario puede tener varios roles). | Propuesto | Precisa «roles» de §28.5 del maestro móvil |
| DW-05 | Entran a la web ADMINISTRADOR, ESPECIALISTA_FITOSANITARIO y SUPERVISOR. OPERADOR_CAMPO no entra (usa la app). | Propuesto / Decidir (Q-W01) | Integrador §9.3 |
| DW-06 | **Caso** = unidad de revisión = una foto (captura) con su análisis; tabla `review_cases`, uno por captura. | Propuesto | Sección 11 |
| DW-07 | El worker abre el caso en la misma transacción en que guarda un `INDICIO_SUGERIDO_POR_IA`; el especialista puede abrir un caso manual sobre una foto `SIN_INDICIOS_IA` o `ERROR_DE_ANALISIS`. | Propuesto | Sección 8.7 |
| DW-08 | Una sola decisión vigente por caso; una corrección crea otra fila (`supersedes`) con motivo obligatorio; el historial nunca se borra. | Propuesto | Sección 8.5 |
| DW-09 | Avisos: al confirmar, se crean filas `notifications` (una por destinatario) en la misma transacción; el worker las envía con el mismo patrón de cola que `ai_tasks`. Destinatarios en `notification_recipients` con alcance por lote y consentimiento. | Propuesto | §28.8 paso 7 del maestro móvil |
| DW-10 | Si se corrige un caso confirmado, los avisos pendientes se anulan (`NO_APLICA`); los ya enviados no se retiran y no se envía mensaje de corrección. | Propuesto / Decidir (Q-W07) | Sección 8.5 |
| DW-11 | Solo se analizan con IA las fotos `UTILIZABLE` o `PENDIENTE_REVISION_TECNICA`; las rechazadas por calidad se guardan para auditoría y calibración, sin tarea de IA. No cambia nada de lo que la app ve (la respuesta de `POST /captures/upload` es la misma). | Propuesto / Decidir (Q-W17) | RN-14 del maestro móvil; exige actualizar §15.2 («Requisitos nuevos»), §15.8, §28.2, §28.8 y §28.9 (paso 7) del maestro móvil en su v2.1 |
| DW-12 | Visor: imagen `revision` + SVG con `viewBox` igual al tamaño de la imagen analizada; las cajas se dibujan en píxeles de esa imagen; acercar y mover en el navegador; original solo bajo pedido. | Propuesto / Verificado | Sección 14 |
| DW-13 | Ubicación del caso: GPS de la secuencia (o del contexto de la repetición) → coordenadas del marcador (aproximada) → sin coordenadas. Se copia al caso al abrirlo. | Propuesto | RN-07 y RN-18 del maestro móvil |
| DW-14 | Dos vistas de ubicación: **mapa** Leaflet con posiciones GPS y **plano de hileras** esquemático (lote → hilera → segmento) que no necesita coordenadas. | Propuesto | Integrador §9.7 «Mapa con marcadores»; archivo de mapeo |
| DW-15 | Teselas de OpenStreetMap estándar, con atribución visible, solo para ver; sin descarga para uso sin conexión. | Propuesto / Verificado (política OSMF) | Sección 16 |
| DW-16 | La bandeja se actualiza por sondeo HTMX cada 15 s, solo con la pestaña visible; no se usa Realtime. | Confirmado | §28.4 del maestro móvil |
| DW-17 | Reportes: CSV UTF-8 con BOM y protección contra inyección de fórmulas; PDF con «Imprimir» del navegador. | Propuesto / Decidir (Q-W06) | Sección 15.2 |
| DW-18 | KPIs del panel en caché compartida 60 s. | Propuesto | Sección 18 |
| DW-19 | Django Admin en `/gestion/` solo para superusuarios (responsables técnicos): catálogos, perfiles de calidad, modelos de IA, datos de contacto de usuarios y consulta de solo lectura. Aprobar, rechazar, bloquear, roles, contraseña temporal y revocar celulares se hacen en la web (WEB-13, WEB-14). | Propuesto (cambia §28.6 del maestro móvil) | W-03 |
| DW-20 | Archivos estáticos con whitenoise (`CompressedManifestStaticFilesStorage`); HTMX y Leaflet copiados al repositorio, sin CDN. | Propuesto | W-01 |
| DW-21 | CSS propio con tokens de diseño, sin framework, siguiendo los componentes del Integrador §9.7 y el responsive de §9.8. | Propuesto | Sección 6.5 |
| DW-22 | Paleta de estados de revisión: pendiente ámbar, confirmado rojo, descartado gris, evidencia insuficiente violeta (difiere del Integrador §9.7, donde verde = confirmado). | Propuesto / Decidir (Q-W04) | Sección 6.5 |
| DW-23 | Caché compartida en la base de datos (`web_cache`, creada por migración) para el bloqueo de ingreso y los KPIs. | Propuesto | Sección 16 |
| DW-24 | Pruebas con el ejecutor de Django (sin pytest) contra PostgreSQL local. | Propuesto | Sección 21 |
| DW-25 | La contraseña sigue la misma política que la app: 8 o más caracteres, letras y números, sin espacios al inicio ni al final, más los validadores estándar de Django. | Propuesto / Verificado | `validatePassword` del maestro móvil |
| DW-26 | El enlace del WhatsApp lleva al caso en la web (requiere iniciar sesión), nunca a la foto. | Confirmado | §28.7 del maestro móvil |

### 4.1 Cambios respecto de documentos anteriores

| Documento | Decía | Ahora |
|---|---|---|
| Stack v2.1 §26 y Esp. v2.0 §18 | Web en Angular + Leaflet en un repositorio aparte. | Plantillas Django + HTMX + Leaflet en `riachuelo-platform` (DW-01, DW-02). |
| Curso Integrador II §9.3 | Módulos Tratamientos y Simulador móvil en la web. | No se construyen (sección 3). |
| Curso Integrador II §9.3 | Reportes = documentos PDF. | CSV + impresión del navegador (DW-17, Q-W06). |
| Curso Integrador II §9.7 | Insignia verde para «confirmado» y roja para «descartado/crítico». | Paleta por estado de revisión (DW-22); «crítico» no existe (W-12). |
| Curso Integrador II CU7 | El especialista registra «la decisión fitosanitaria que corresponde aplicar». | Registra una **observación** de lo que ve en la evidencia, sin tratamiento ni dosis (W-13). |
| Curso Integrador II CU8 | Aviso «al jefe de fundo y supervisor de zona». | Destinatarios configurables con alcance por lote y consentimiento (DW-09). |
| Curso Integrador II RF11 y §9.3 | Cámara izquierda / derecha. | CAMERA_1 / CAMERA_2 (Stack v2.1 §7). |
| Maestro App Móvil §28.5 | `roles` como campo de `users`. | Tabla `user_roles` (DW-04). |
| Maestro App Móvil §28.5 | `human_reviews` con «`detection_id` o caso». | `human_reviews.case_id` → `review_cases` (DW-06). |
| Maestro App Móvil §15.2, §15.8, §28.2, §28.8 y §28.9 | Toda captura confirmada crea su `ai_task`. | Solo las aceptadas por calidad (DW-11, Q-W17). |
| Maestro App Móvil §28.6, §7.1, §7.7 y §26 | Acciones en Django Admin para aprobar, rechazar, bloquear, asignar contraseña temporal y revocar dispositivos («Django Admin o pantalla de usuarios»). | Solo en la web (WEB-13, WEB-14), para pasar por `cuentas.services` y la auditoría; Django Admin queda para superusuarios (DW-19). |
| Maestro App Móvil §28.5 | Sin `model_configs`, `review_cases` ni `notification_recipients`. | Tablas nuevas (sección 11.5). |

---

## 5. Stack, versiones y configuración

### 5.1 Base (Verificado el 04/10/2026)

| Pieza | Versión | Nota |
|---|---|---|
| Python | 3.12 (producción) | El código se probó también en 3.13. |
| Django | 5.2.17 (LTS) | Soporte de seguridad hasta abril de 2028. No subir a 6.x mientras SimpleJWT no declare soporte (W-01). |
| Base de datos | PostgreSQL 15 o superior (Supabase, `sa-east-1`) | Pruebas en PostgreSQL 16 local. |
| Navegadores objetivo | Chrome, Edge, Firefox y Safari actuales; Chrome y Safari de celular | Sin soporte para Internet Explorer. |

### 5.2 Librerías de Python (`requirements.txt`)

| Paquete | Versión | Uso en la web |
|---|---|---|
| `Django` | 5.2.17 | Framework, plantillas, sesión, CSRF, Admin. |
| `djangorestframework` | 3.18.1 | API de la app (no la usa la web); declara soporte de Django 5.2, 6.0 y 6.1. |
| `djangorestframework-simplejwt` | 5.5.1 | Tokens de la app; la web usa su lista negra al bloquear o dar contraseña temporal. |
| `drf-spectacular` | 0.30.0 | Esquema OpenAPI de la API (no la usa la web). |
| `django-htmx` | 1.29.0 | `request.htmx`, `HttpResponseClientRedirect` (soporta Django 5.2, 6.0 y 6.1). |
| `whitenoise` | 6.12.0 | Archivos estáticos comprimidos y con hash. |
| `cloudinary` | 1.46.2 | URLs firmadas (`cloudinary_url(..., sign_url=True)`). |
| `psycopg[binary]` | 3.3.6 | Conexión a Supabase. |
| `gunicorn` | 26.2.0 | Proceso web en producción. |
| `django-environ` | 0.14.0 | Variables de entorno y `DATABASE_URL`. |
| `requests` | 2.34.2 | Worker → WhatsApp Cloud API. |

```text
Django==5.2.17
djangorestframework==3.18.1
djangorestframework-simplejwt==5.5.1
drf-spectacular==0.30.0
django-htmx==1.29.0
whitenoise==6.12.0
cloudinary==1.46.2
psycopg[binary]==3.3.6
gunicorn==26.2.0
django-environ==0.14.0
requests==2.34.2
```

### 5.3 Librerías del navegador (archivos estáticos)

| Librería | Versión | Archivos en `web/static/web/vendor/` | Nota |
|---|---|---|---|
| HTMX (`htmx.org`) | 2.0.11 | `htmx.min.js` | La etiqueta `next` del registro npm apunta a 4.0.0: no usar. |
| Leaflet | 1.9.4 | `leaflet/leaflet.js`, `leaflet/leaflet.css`, `leaflet/images/*` | 2.0.0 está en alfa: no usar. |

Cómo se obtienen (una sola vez, en la laptop del desarrollador; npm no es parte del proyecto): `npm pack htmx.org@2.0.11 leaflet@1.9.4`, descomprimir y copiar los archivos de `dist/`. Las versiones quedan anotadas en `web/static/web/vendor/VERSIONES.txt`.

### 5.4 Configuración de Django que importa a la web

El archivo completo está en el Anexo B.1. Resumen:

| Ajuste | Valor | Por qué |
|---|---|---|
| `AUTH_USER_MODEL` | `cuentas.User` | Usuario propio con correo, estado y roles (DW-03). |
| `LOGIN_URL` / `LOGIN_REDIRECT_URL` | `web:login` / `web:dashboard` | Rutas de la web. |
| `SESSION_COOKIE_AGE` | 8 h (`SESSION_COOKIE_AGE`) | Una jornada. |
| `SESSION_EXPIRE_AT_BROWSER_CLOSE` | `True` | Computadoras compartidas en el fundo. |
| `SESSION_COOKIE_SECURE`, `CSRF_COOKIE_SECURE` | `True` fuera de desarrollo | Solo por HTTPS. |
| `SECURE_PROXY_SSL_HEADER` | `("HTTP_X_FORWARDED_PROTO", "https")` | Detrás del proxy HTTPS del hosting. |
| `SECURE_REFERRER_POLICY` | `strict-origin-when-cross-origin` | OpenStreetMap exige Referer (0.5). |
| `X_FRAME_OPTIONS` | `DENY` | Nadie incrusta la web. |
| `LANGUAGE_CODE` / `TIME_ZONE` | `es-pe` / `America/Lima` | Textos y horas locales (W-16). |
| `CACHES` | `DatabaseCache` en `web_cache` | Compartida entre procesos gunicorn (DW-23). |
| `STORAGES["staticfiles"]` | `CompressedManifestStaticFilesStorage` | whitenoise (DW-20). |
| `AUTH_PASSWORD_VALIDATORS` | Django + `LettersAndDigitsValidator` | Misma política que la app (DW-25). |
| `WEB` | Diccionario de parámetros | Sección 17. |

### 5.5 Variables de entorno

| Variable | Ejemplo | Uso |
|---|---|---|
| `DJANGO_SECRET_KEY` | (aleatoria, 50+ caracteres) | Firma de sesiones, CSRF y JWT. Distinta por entorno. |
| `DJANGO_DEBUG` | `false` | `true` solo en la laptop. |
| `APP_ENV` | `dev` / `piloto` | Muestra el entorno en la cabecera y bloquea `sembrar_demo` en piloto. |
| `ALLOWED_HOSTS`, `CSRF_TRUSTED_ORIGINS` | `riachuelo.ejemplo.pe`, `https://riachuelo.ejemplo.pe` | Dominio de la plataforma (Q-W12). |
| `PUBLIC_BASE_URL` | `https://riachuelo.ejemplo.pe` | Base del enlace del WhatsApp. |
| `DATABASE_URL` | `[DATABASE_URL omitida]` | Conexión directa o *session pooler* (§28.4 del maestro móvil). |
| `DB_SSLMODE` | `require` | `disable` solo con PostgreSQL local. |
| `DB_CONN_MAX_AGE` | `60` | Conexiones persistentes. |
| `CLOUDINARY_URL` | `[CLOUDINARY_URL omitida]` | URLs firmadas (secreto: W-09). |
| `CLOUDINARY_ENV_PREFIX` | `dev` / `piloto` | Prefijo del `public_id` (D-32). |
| `SESSION_COOKIE_AGE` | `28800` | Duración de la sesión web en segundos. |
| `SECURE_SSL_REDIRECT`, `SECURE_HSTS_SECONDS` | `true`, `31536000` | HTTPS obligatorio en piloto. |
| `WEB_BANDEJA_POLL_SECONDS` | `15` | Sondeo de la bandeja. |
| `STATICFILES_BACKEND` | (por defecto whitenoise) | En pruebas y desarrollo: `django.contrib.staticfiles.storage.StaticFilesStorage`. |
| `WHATSAPP_CLIENT` | `notificaciones.whatsapp.CloudApiClient` | `ConsoleClient` en desarrollo (no envía). |
| `WHATSAPP_TOKEN`, `WHATSAPP_PHONE_ID`, `WHATSAPP_GRAPH_VERSION` | — | WhatsApp Cloud API (solo el worker). La versión de la Graph API se toma de la documentación vigente de Meta. |
| `WHATSAPP_TEMPLATE`, `WHATSAPP_TEMPLATE_LANG` | `caso_confirmado`, `es` | Plantilla aprobada (sección 15.3). |
| `LOG_LEVEL` | `INFO` | — |

### 5.6 Entorno de desarrollo

1. PostgreSQL 15+ local (Docker Compose o instalado) **o** un proyecto Supabase de desarrollo (§28.4 del maestro móvil). Para que las pruebas de seguridad no se omitan en local, crear una vez los roles de Supabase: `CREATE ROLE anon NOLOGIN; CREATE ROLE authenticated NOLOGIN;`.
2. `cp .env.example .env` y completar (Anexo G.4). En local: `DB_SSLMODE=disable` y `STATICFILES_BACKEND=django.contrib.staticfiles.storage.StaticFilesStorage`.
3. `python manage.py migrate` → crea tablas, la tabla de caché y aplica RLS y `REVOKE` (Anexo B.9).
4. `python manage.py sembrar_demo` → catálogos del piloto (Anexo D del maestro móvil) con segmentos, marcadores y coordenadas **ficticios**, cuatro cuentas de demostración (`admin@demo.pe`, `especialista@demo.pe`, `supervisor@demo.pe`, `operador@demo.pe`, contraseña `Demo2026!`), una cuenta pendiente y tres sesiones con unas 576 fotos, sus análisis simulados y unos 80 casos.
5. `python manage.py runserver` y, en otra terminal, `python manage.py worker_ia` (con `WHATSAPP_CLIENT=notificaciones.whatsapp.ConsoleClient` los avisos se escriben en la consola).
6. Las fotos de demostración no existen en Cloudinary: el navegador mostrará imágenes rotas salvo que se suban fotos reales con esos `public_id` (o se use el truco del Anexo A para capturas de pantalla).

### 5.7 Creación del proyecto (tarea T-W00)

```bash
# en la raíz de riachuelo-platform (o partiendo del paquete de referencia)
python3.12 -m venv .venv && . .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env                       # completar
python manage.py migrate
python manage.py check && python manage.py makemigrations --check --dry-run
python manage.py test                      # 66 pruebas en PostgreSQL
python manage.py sembrar_demo              # solo dev
python manage.py runserver
```

Si el repositorio ya existe con otra estructura, copiar app por app (sección 6.2) respetando los `db_table` del Anexo B: son los nombres de tabla que también usa la API de la app.


---

## 6. Arquitectura y estructura del código de la web

### 6.1 Capas y recorrido de una petición

```text
Navegador ──HTTPS (cookie de sesión + CSRF)──► gunicorn: proceso web de riachuelo-platform
                                               │
   Security · WhiteNoise · Session · Common · CSRF · Auth · Messages · XFrameOptions · Htmx · NoStoreForAuthenticated
                                               │
                                     web/urls.py (sección 6.3)
                                               │
                         @web_view("permiso")  ← matriz 7.2 (W-07)
                                               │
                     web/views.py ── formularios (web/forms.py)
                        │                    │
           lectura      │                    │ escritura
                        ▼                    ▼
                web/queries.py        <app>/services.py  ── transaction.atomic + select_for_update
                        │                    │              + auditoria.services.record (W-15)
                        └────────► modelos (Anexo B) ◄──────┘
                                               │ SQL (psycopg 3, SSL)
                                     Supabase · PostgreSQL (sa-east-1)

Plantilla (página completa o fragmento HTMX) ── URLs firmadas de Cloudinary (evidencias/media.py)
Navegador ──► res.cloudinary.com (imagen firmada)      Navegador ──► tile.openstreetmap.org (teselas)
```

Lo que **no** pasa nunca en una petición de la web: llamar a WhatsApp, ejecutar YOLO, descargar una foto de Cloudinary en el servidor o escribir en las tablas de sincronización. Esas tareas son del worker o de la API de la app.

### 6.2 Estructura de carpetas

```text
riachuelo-platform/
├── config/
│   ├── settings.py              # Anexo B.1 (un solo archivo, todo por variables de entorno)
│   ├── urls.py                  # /gestion/ (Admin), /api/v1/ (app móvil), / (web)
│   └── wsgi.py
├── cuentas/                     # usuarios, roles, celulares, pedidos de contraseña
│   ├── models.py  services.py  validators.py  admin.py  migrations/
├── campo/                       # lotes (con contorno GeoJSON), hileras, segmentos, marcadores, perfiles de calidad
├── monitoreo/                   # sesiones, cámaras de la sesión, pasadas, cambios de marcador, secuencias, incidencias
├── evidencias/                  # capturas confirmadas, calidad, media.py (URLs firmadas)
├── ia/                          # model_configs, ai_tasks, detections, services.py (cola), management/commands/worker_ia.py
├── revision/                    # review_cases, human_reviews, services.py (reglas de la revisión)
├── notificaciones/              # notification_recipients, notifications, services.py, whatsapp.py
├── auditoria/                   # audit_events, services.py, seguridad_bd.py (RLS), migración de la tabla de caché
├── api/                         # API /api/v1 de la app (Maestro App Móvil §15) — fuera de este archivo
├── web/
│   ├── permissions.py           # matriz PERMISOS + decorador @web_view
│   ├── middleware.py            # no-store para usuarios autenticados + Vary: HX-Request
│   ├── context_processors.py    # `puede` (permisos para plantillas), APP_ENV, WEB
│   ├── forms.py  messages.py  queries.py  views.py  urls.py  errors.py  demo.py
│   ├── templatetags/web_tags.py # {% badge %}, |duracion, |pct, |get, {% bar_width %}
│   ├── management/commands/sembrar_demo.py  medir_rendimiento.py
│   ├── templates/web/
│   │   ├── base.html  login.html  cambiar_contrasena.html  403.html
│   │   ├── dashboard.html  bandeja.html  caso.html  captura.html  mapa.html  plano.html
│   │   ├── sesiones.html  sesion.html  reportes.html  notificaciones.html
│   │   ├── usuarios.html  dispositivos.html  destinatarios.html  ia.html  auditoria.html
│   │   └── partials/ bandeja_tabla.html  panel_decision.html  mensajes.html  error.html
│   ├── static/web/
│   │   ├── app.css  caso.js  mapa.js
│   │   └── vendor/ htmx.min.js  leaflet/  VERSIONES.txt
│   └── tests/ factories.py  test_views.py
├── docs/ MAESTRO_WEB.md  MAESTRO_APP_MOVIL.md  adr/  img/web/  perf/datos_sinteticos.sql
├── requirements.txt  .env.example  README.md  manage.py
└── (Dockerfile, docker-compose.yml: sección 28.3 del maestro móvil)
```

Las pruebas de cada app viven en `<app>/tests/` (Anexo F).

### 6.3 Mapa de rutas

Todas las rutas son de la app `web` (`app_name = "web"`), salvo Django Admin (`/gestion/`) y la API (`/api/v1/`).

| Ruta | Nombre | Métodos | Permiso | Plantilla / respuesta | Pantalla |
|---|---|---|---|---|---|
| `/ingresar/` | `login` | GET, POST | (público) | `login.html` | WEB-01 |
| `/salir/` | `logout` | POST | (sesión) | redirige a `/ingresar/` | — |
| `/cuenta/contrasena/` | `cambiar_contrasena` | GET, POST | (sesión) | `cambiar_contrasena.html` | WEB-02 |
| `/` | `dashboard` | GET | `dashboard.ver` | `dashboard.html` | WEB-03 |
| `/casos/` | `bandeja` | GET | `bandeja.ver` | `bandeja.html` o fragmento `partials/bandeja_tabla.html` | WEB-04 |
| `/casos/<uuid>/` | `caso` | GET | `caso.ver` | `caso.html` | WEB-05 |
| `/casos/<uuid>/decidir/` | `caso_decidir` | POST | `caso.decidir` | `HX-Redirect` (éxito) o fragmento `partials/panel_decision.html` (409/422) | WEB-05 |
| `/casos/<uuid>/corregir/` | `caso_corregir` | POST | `caso.corregir` | ídem | WEB-05 |
| `/capturas/<uuid>/` | `captura` | GET | `caso.ver` | `captura.html` | WEB-06 |
| `/capturas/<uuid>/abrir-caso/` | `captura_abrir_caso` | POST | `caso.abrir_manual` | redirige al caso | WEB-06 |
| `/mapa/` | `mapa` | GET | `mapa.ver` | `mapa.html` | WEB-07 |
| `/mapa/datos/` | `mapa_datos` | GET | `mapa.ver` | JSON GeoJSON comprimido (15.1) | WEB-07 |
| `/plano/` | `plano` | GET | `mapa.ver` | `plano.html` | WEB-08 |
| `/sesiones/` | `sesiones` | GET | `sesiones.ver` | `sesiones.html` | WEB-09 |
| `/sesiones/<uuid>/` | `sesion` | GET | `sesiones.ver` | `sesion.html` | WEB-10 |
| `/reportes/` | `reportes` | GET | `reportes.exportar` | `reportes.html` | WEB-11 |
| `/reportes/casos.csv` | `exportar_casos` | GET | `reportes.exportar` | CSV en flujo (15.2) | WEB-11 |
| `/notificaciones/` | `notificaciones` | GET | `notificaciones.ver` | `notificaciones.html` | WEB-12 |
| `/administracion/usuarios/` | `usuarios` | GET | `usuarios.gestionar` | `usuarios.html` | WEB-13 |
| `/administracion/usuarios/<uuid>/<accion>/` | `usuario_accion` | POST | `usuarios.gestionar` | redirige a usuarios | WEB-13 |
| `/administracion/dispositivos/` | `dispositivos` | GET | `dispositivos.gestionar` | `dispositivos.html` | WEB-14 |
| `/administracion/dispositivos/<uuid>/revocar/` | `dispositivo_revocar` | POST | `dispositivos.gestionar` | redirige | WEB-14 |
| `/administracion/destinatarios/` | `destinatarios` | GET, POST | `destinatarios.gestionar` | `destinatarios.html` | WEB-15 |
| `/administracion/destinatarios/<id>/` | `destinatario_editar` | GET, POST | `destinatarios.gestionar` | ídem | WEB-15 |
| `/ia/` | `ia` | GET | `ia.ver` | `ia.html` | WEB-16 |
| `/ia/tareas/<uuid>/reencolar/` | `ia_reencolar` | POST | `ia.reencolar` | redirige | WEB-16 |
| `/auditoria/` | `auditoria` | GET | `auditoria.ver` | `auditoria.html` | WEB-17 |
| `/gestion/` | (Django Admin) | — | `is_staff` | Admin | WEB-18 |

Acciones de `usuario_accion`: `aprobar`, `roles`, `rechazar`, `bloquear`, `desbloquear`, `contrasena-temporal` (cualquier otra responde 404).

### 6.4 Patrones HTMX (los únicos permitidos, W-18)

| ID | Patrón | Cómo se implementa | Dónde |
|---|---|---|---|
| P-1 | **Misma URL, página o fragmento** | La vista elige la plantilla con `request.htmx` (django-htmx). Sin JavaScript, la URL funciona igual como página completa. | Bandeja, panel de decisión |
| P-2 | **Sondeo que se reemplaza a sí mismo** | El fragmento de la bandeja es un `<div id="bandeja" hx-get="{{ request.get_full_path }}" hx-trigger="every 15s [document.visibilityState === 'visible']" hx-swap="outerHTML">`. Cada respuesta trae su propia URL (filtros y página vigentes), así el sondeo nunca vuelve a un filtro viejo. | Bandeja |
| P-3 | **Formulario HTMX con CSRF** | `<body hx-headers='{"X-CSRFToken": "{{ csrf_token }}"}'>` y, además, `{% csrf_token %}` en el formulario (funciona sin JavaScript). | Todo POST |
| P-4 | **Errores con fragmento** | HTMX 2 no reemplaza respuestas 4xx por defecto. En `base.html`: `<meta name="htmx-config" content='{"responseHandling":[{"code":"204","swap":false},{"code":"[23]..","swap":true},{"code":"403","swap":true,"error":false},{"code":"409","swap":true,"error":false},{"code":"422","swap":true,"error":false},{"code":"[45]..","swap":false,"error":true}]}'>`. La vista responde 422 (formulario inválido), 409 (otro especialista ya decidió o la decisión cambió) o 403 (sin permiso, fragmento `partials/error.html`). | Decisión, corrección |
| P-5 | **Sesión vencida en un fragmento** | `@web_view` responde `HX-Redirect: /ingresar/?next=<url actual>` (`HttpResponseClientRedirect`) en lugar de un 302 que HTMX metería dentro del fragmento. | Toda vista |
| P-6 | **Éxito de un POST** | `HX-Redirect` a la página del caso (o al siguiente caso con «Guardar y siguiente»). Recarga la página completa con los mensajes. | Decisión, corrección |
| P-7 | **Filtros con historial** | `hx-get` + `hx-push-url="true"` en el formulario de filtros y en la paginación; la URL del navegador refleja los filtros (se puede compartir o recargar). Paginación con `{% querystring pagina=… %}` (Django 5.1+). | Bandeja |
| P-8 | **Cabeceras** | `NoStoreForAuthenticatedMiddleware` agrega `Cache-Control: no-store` a toda respuesta autenticada y `Vary: HX-Request` (la misma URL devuelve dos formas). | Global |
| P-9 | **Condiciones en `hx-trigger`** | `[document.visibilityState === 'visible']` evita sondear con la pestaña oculta. HTMX evalúa la condición con `eval`: si algún día se activa una CSP sin `unsafe-eval`, quitar la condición (o usar `htmx.config.allowEval`). | Bandeja |

### 6.5 Diseño visual

**Disposición (Integrador §9.7 «Barra lateral (web)»):** barra lateral fija con el menú agrupado en «Plataforma» (Panel, Casos, Mapa, Plano de hileras, Sesiones, Reportes, Avisos, Estado de la IA) y «Administración» (Usuarios, Celulares, Destinatarios WhatsApp, Auditoría y, solo para superusuarios, Gestión), cada ítem con icono y texto y `aria-current` en el activo; cabecera con migas, insignia de entorno (oculta en piloto), nombre y roles del usuario, «Contraseña» y «Salir». Los ítems se muestran solo si el usuario tiene el permiso (`puede`), pero la protección real es `@web_view` (W-07).

**Tokens (`web/static/web/app.css`, Anexo E):** verdes de marca (`--verde-900` #173f31 a `--verde-50`), texto, bordes y fondo; un par de colores (texto/fondo) por estado. Tipografía del sistema (Inter si está instalada). Radio 8 px.

**Paleta de estados (DW-22, W-12):**

| Familia | Estado | Icono | Color | Nota |
|---|---|---|---|---|
| Revisión | Pendiente de revisión | ◷ | ámbar | Lo que falta hacer. |
| Revisión | Confirmado por especialista | ✔ | rojo | Hallazgo confirmado que requiere seguimiento operativo; **no** expresa gravedad. |
| Revisión | Descartado | ✕ | gris | — |
| Revisión | Evidencia insuficiente | ? | violeta | Se debe volver a mirar en campo. |
| IA | Pendiente de análisis / En análisis | ◷ / ⟳ | gris / azul | — |
| IA | Indicio sugerido por IA | ◆ | amarillo oscuro | Nunca rojo: no es una confirmación. |
| IA | Sin indicios de la IA | ○ | gris | No prueba ausencia de plaga. |
| IA | Error de análisis | ⚠ | naranja | No es un negativo. |
| Aviso | No aplica / Pendiente de envío / Enviado / Error de envío | – / ◷ / ✔ / ⚠ | gris / ámbar / verde / naranja | — |
| General | Activo, sincronizado, completa, utilizable | ✔ o ● | verde | Igual que el Integrador §9.7. |

En el mapa los puntos usan tonos más saturados de los mismos colores (`mapa.js`) y un borde punteado cuando la ubicación es aproximada (coordenadas del marcador).

**Componentes del Integrador §9.7 y su implementación:**

| Componente | Implementación |
|---|---|
| Badge de estado | `{% badge estado etiqueta %}` (`web_tags.py`): color + icono + texto. |
| Tarjeta de sesión / caso | `.tarjeta` y `.kpi` (panel, sesión, ficha del caso). |
| Indicador de conexión | No aplica a la web (siempre en línea); la cabecera muestra el **entorno** (`dev`). |
| Barra de progreso | Cobertura por lote en el panel (texto «4 / 16» y porcentaje). |
| Grilla de miniaturas | Bandeja (miniatura por caso) y sesión (lista de fotos). |
| Botón primario / secundario / destructivo | `.btn-primario` (verde), `.btn-secundario` (contorno), `.btn-peligro` (rojo: revocar, corregir). |
| Barra lateral (web) | `.lateral` con grupos Plataforma / Administración. |
| Mapa con marcadores | Mapa Leaflet (puntos por estado, contornos de lote) y plano de hileras (barras por hilera, recuadros por segmento). |
| Avatar de usuario | Nombre y roles en la cabecera (sin imagen). |

**Responsive (Integrador §9.8):**

| Ancho | Comportamiento |
|---|---|
| ≥ 1280 px (escritorio, referencia) | Barra lateral expandida; el caso en dos columnas (visor + ficha y decisión). |
| 768–1279 px (tablet) | Barra lateral solo con iconos; el caso apilado en una columna. |
| < 768 px (celular: consulta ocasional y **enlace del WhatsApp**) | Menú horizontal desplazable arriba; cabecera sin nombre ni entorno; las tablas se vuelven tarjetas (`data-label`); en el caso, primero la ubicación y la decisión y después la foto. |
| Impresión | Sin menú, cabecera ni filtros; fondo blanco (informe PDF desde el navegador). |

**Accesibilidad (W-23):** foco visible (`outline` de 3 px), estados con texto, fotos con `alt` descriptivo, mensajes con `role="status"`/`role="alert"`, objetivos de toque de al menos 40 px, atajos de teclado que solo seleccionan (sección 8.2).

### 6.6 Errores y páginas de error

| Situación | Respuesta |
|---|---|
| Sin sesión | 302 a `/ingresar/?next=…` (o `HX-Redirect` en HTMX, P-5). |
| Contraseña temporal pendiente | 302 a `/cuenta/contrasena/` desde cualquier vista. |
| Sin permiso | 403 con `403.html` (o fragmento `partials/error.html` en HTMX). Configurado con `handler403 = "web.errors.forbidden"`. |
| Caso inexistente | 404. |
| Formulario inválido | 422 con el fragmento y los errores por campo. |
| Conflicto de concurrencia | 409 con el fragmento y el mensaje «Este caso ya fue decidido por … el …». |
| Error interno | 500 de Django (sin detalle en piloto); el error se registra en el log del servidor. |

---

## 7. Usuarios, roles y permisos

### 7.1 Roles en la web

| Rol | Quién es | Qué hace en la web |
|---|---|---|
| `ESPECIALISTA_FITOSANITARIO` | Agrónomo o especialista de sanidad vegetal | Revisa casos, confirma, descarta, marca evidencia insuficiente, corrige decisiones y abre casos manuales; ve todo lo demás de la plataforma (no la administración). |
| `SUPERVISOR` | Supervisor de zona y jefe de fundo (Q-W02) | Consulta panel, casos (sin decidir), mapa, plano, sesiones, avisos y reportes. Abre el enlace del WhatsApp. |
| `ADMINISTRADOR` | Responsable del sistema | Todo lo que consulta el supervisor, más usuarios, celulares, destinatarios, estado de la IA (reencolar) y auditoría; con `is_staff`, Django Admin. **No decide casos** (RN-W01). |
| `OPERADOR_CAMPO` | Operador de la mochila | No entra a la web (DW-05); usa la app. |

Un usuario puede tener varios roles (DW-04): por ejemplo, un especialista que también administra. Los permisos se suman.

### 7.2 Matriz de permisos (`web/permissions.py`, contrato W-05)

| Permiso | ADMINISTRADOR | ESPECIALISTA | SUPERVISOR | Usado por |
|---|:---:|:---:|:---:|---|
| `dashboard.ver` | ✔ | ✔ | ✔ | WEB-03 |
| `bandeja.ver` | ✔ | ✔ | ✔ | WEB-04 |
| `caso.ver` | ✔ | ✔ | ✔ | WEB-05, WEB-06 |
| `caso.decidir` | — | ✔ | — | Decidir (RN-W01) |
| `caso.corregir` | — | ✔ | — | Corregir (RN-W05) |
| `caso.abrir_manual` | — | ✔ | — | Caso manual (RN-W09) |
| `mapa.ver` | ✔ | ✔ | ✔ | WEB-07, WEB-08 |
| `sesiones.ver` | ✔ | ✔ | ✔ | WEB-09, WEB-10 |
| `reportes.exportar` | ✔ | ✔ | ✔ | WEB-11 |
| `notificaciones.ver` | ✔ | ✔ | ✔ | WEB-12 |
| `ia.ver` | ✔ | ✔ | — | WEB-16 |
| `ia.reencolar` | ✔ | — | — | WEB-16 |
| `usuarios.gestionar` | ✔ | — | — | WEB-13 |
| `dispositivos.gestionar` | ✔ | — | — | WEB-14 |
| `destinatarios.gestionar` | ✔ | — | — | WEB-15 |
| `auditoria.ver` | ✔ | — | — | WEB-17 |

Reglas: `can(user, permiso)` exige sesión, cuenta `ACTIVO`, al menos un rol de la web y un rol del permiso. Las plantillas usan `puede.<permiso con _>` (por ejemplo `{% if puede.caso_decidir %}`), calculado por el procesador de contexto. Las pruebas `test_todas_las_rutas_estan_protegidas` y `test_paginas_por_rol` comprueban la matriz completa contra las rutas.

### 7.3 Inicio de sesión en la web

1. El usuario escribe correo y contraseña en `/ingresar/` (WEB-01).
2. Si el correo tiene 5 intentos fallidos en los últimos 15 minutos (`WEB.LOGIN_MAX_FAILED`, `WEB.LOGIN_LOCKOUT_MINUTES`), se responde «Demasiados intentos fallidos. Vuelve a intentarlo en 15 minutos.» sin comprobar la contraseña.
3. Se busca el usuario por correo (sin distinguir mayúsculas). Si no existe, se calcula igual un hash (mismo tiempo de respuesta) y se responde el mensaje genérico. Si la contraseña es incorrecta: mensaje genérico «Correo o contraseña incorrectos.» y se suma un intento.
4. **Solo con la contraseña correcta** se revela el estado: pendiente, rechazada o bloqueada, cada una con su mensaje (no permite averiguar qué correos existen).
5. Si la cuenta no tiene un rol de la web: «Tu cuenta no tiene acceso a la plataforma web. Usa la app móvil.»
6. Se inicia la sesión de Django, se registra `INGRESO_WEB` en auditoría y se redirige: a `/cuenta/contrasena/` si `must_change_password`; si no, a `next` (solo si es del mismo dominio) o al panel.
7. La sesión dura `SESSION_COOKIE_AGE` (8 h) y termina al cerrar el navegador. «Salir» es un POST.
8. Si el administrador bloquea o rechaza la cuenta, la sesión web se corta en la **siguiente petición**: `User.is_active` depende del estado y Django descarta la sesión de un usuario inactivo.

Mensajes exactos: `web/messages.py` (Anexo D.5). Son coherentes con los de la app (§19 del maestro móvil).

### 7.4 Estados de cuenta y transiciones en la web

| Desde | Hacia | Acción (WEB-13) | Efecto adicional |
|---|---|---|---|
| `PENDIENTE_APROBACION` | `ACTIVO` | Aprobar (con al menos un rol) | `approved_by`, `approved_at`; auditoría `CUENTA_APROBADA` y `ROLES_CAMBIADOS`. |
| `PENDIENTE_APROBACION` | `RECHAZADO` | Rechazar | Auditoría `CUENTA_ESTADO`. |
| `ACTIVO` | `BLOQUEADO` | Bloquear | Lista negra de todos los refresh de la app del usuario; la web lo saca en la siguiente petición. |
| `BLOQUEADO` | `ACTIVO` | Desbloquear | — |

Cualquier otro cambio responde «Cambio de estado no permitido.». Nadie puede cambiar el estado de su propia cuenta ni quitarse el rol de administrador a sí mismo.

### 7.5 Aprobación y roles

Las cuentas nacen pendientes desde la app (`POST /auth/register`, D-05 del maestro móvil). El administrador las ve en «Solicitudes pendientes de aprobación» con nombre, correo, código y fecha, marca los roles y pulsa «Aprobar». Puede cambiar roles después desde «Todas las cuentas» → «Roles» (acción `roles`, cuentas activas o bloqueadas). Para que alguien use la app, necesita `OPERADOR_CAMPO` o `ADMINISTRADOR` (RN-02 del maestro móvil); para la web, `ESPECIALISTA_FITOSANITARIO`, `SUPERVISOR` o `ADMINISTRADOR`.

### 7.6 Contraseña temporal (D-07)

- «Contraseña temporal» genera 12 caracteres sin letras ni números ambiguos (sin 0/O/1/l/I), **siempre con letras y números** (cumple la política de la app), la muestra **una sola vez** en un mensaje al administrador para que la entregue en persona, marca `must_change_password`, pone en la lista negra todos los refresh de la app del usuario y da por atendidos sus pedidos de restablecimiento pendientes (por `user_id` o, si la API solo guardó el correo, por correo).
- El usuario, al entrar a la web o a la app con internet, solo puede cambiarla.
- Límite conocido: un celular **sin internet** puede seguir entrando con el verificador anterior hasta que vuelva a validar con el servidor (ventana sin internet, §7.5 del maestro móvil). Ni la contraseña temporal ni la revocación de un celular tienen efecto hasta que ese celular se conecta.

### 7.7 Celulares

WEB-14 lista las instalaciones de la app (`devices`): modelo, plataforma, versión de la app, último usuario y última vez. «Revocar» guarda `revoked_at` y `revoked_by`; la API responde `DEVICE_REVOKED` a ese `X-Device-Id` (§28.6 del maestro móvil). No hay «des-revocar» en esta versión (una instalación nueva genera otro `deviceId`).

### 7.8 Django Admin (`/gestion/`)

- Acceso: **superusuarios** (responsables técnicos del backend, `python manage.py createsuperuser`); el enlace «Gestión» del menú solo aparece para ellos. Un usuario con `is_staff` pero sin permisos de modelo no vería nada; si se quiere que un ADMINISTRADOR edite catálogos, se le da `is_staff` y los permisos de `campo` e `ia.ModelConfig` (decisión a registrar).
- Editables: lotes (incluido el contorno GeoJSON), hileras, segmentos, marcadores, perfiles de calidad, modelos de IA (`model_configs`, uno activo) y datos de contacto de un usuario; `is_staff` e `is_superuser` solo por un superusuario (excepción a W-15: Django lo registra en `django_admin_log`).
- Solo lectura (`ReadOnlyAdmin`): celulares, pedidos de restablecimiento, sesiones, pasadas, secuencias, incidencias, capturas, tareas de IA, cajas, casos, decisiones, avisos y auditoría (W-14); los roles se muestran en línea sin poder editarlos. Las cuentas no se borran.
- El estado de la cuenta, los roles, la contraseña y la aprobación no se editan en Admin: se hacen en la web para pasar por `cuentas.services` y quedar auditados (DW-19).

---

## 8. Flujos de la web

### 8.1 Del campo a la bandeja

| Paso | Dónde | Resultado visible en la web |
|---|---|---|
| 1. El controlador sincroniza sesión, pasadas y secuencias | API (`/api/v1`) | WEB-09/10: la sesión aparece con sus pasadas e incidencias. |
| 2. Cada foto: ticket → subida a Cloudinary → confirmación | App + Cloudinary + API | WEB-10: la foto aparece con su calidad. |
| 3. En la misma transacción de la confirmación se crea su `ai_task` (si la foto es aceptada por calidad, DW-11) | API | WEB-16: «Pendiente de análisis». |
| 4. El worker toma la tarea (`SKIP LOCKED`), descarga el original firmado, ejecuta YOLO | Worker | WEB-16: «En análisis». |
| 5. El worker guarda cajas y estado; con cajas abre el **caso** en la misma transacción | Worker | WEB-04: el caso aparece en la bandeja en el siguiente sondeo (≤ 15 s). |
| 6. Sin cajas: `SIN_INDICIOS_IA`, sin caso | Worker | WEB-06: la foto se puede revisar y abrir un caso manual. |
| 7. Fallo: reintentos con espera; tras 3, `ERROR_DE_ANALISIS` | Worker | WEB-16: lista de errores con «Reencolar». |

Meta: caso visible en la bandeja menos de 1 minuto después de la confirmación (§28.11 del maestro móvil; Medir).

### 8.2 Revisar un caso (especialista)

1. Abrir la bandeja (por defecto: pendientes, más antiguos primero). Opcionalmente filtrar por lote, origen, fechas u ordenar por confianza de la IA.
2. Abrir el caso: el visor muestra la foto `revision` con las cajas numeradas y su confianza; a la derecha, la ubicación operativa (lote, hilera, lateral, segmento, marcador, coordenadas con precisión y fuente, hora, cámara, operador, análisis y modelo) y la otra cámara de la misma secuencia.
3. Acercar con la rueda o los botones (+, −, ⤢), mover arrastrando, ocultar las cajas (B) y, si hace falta resolución, pulsar «Original» (descarga unos 4 MB; consume ancho de banda de Cloudinary).
4. Elegir la decisión: «Confirmado por especialista», «Descartado» o «Evidencia insuficiente» (atajos C, D, I: **solo seleccionan** y llevan el foco a la observación).
5. Escribir la observación (obligatoria para confirmar o marcar evidencia insuficiente; sin tratamientos ni dosis), elegir la clase confirmada (opcional; lista del modelo) y marcar las cajas incorrectas (opcional; sirven para medir la precisión de la IA y reentrenar).
6. «Guardar decisión» (o Ctrl+Enter) o «Guardar y siguiente» (abre el siguiente pendiente más antiguo).
7. El servidor valida (RN-W01 a RN-W04), guarda en una transacción y responde `HX-Redirect`; el caso se recarga con la decisión vigente, el estado del aviso y el historial.

### 8.3 Confirmar → aviso por WhatsApp

Dentro de **una** transacción (`revision.services.decide_case`):

1. Bloquea la fila del caso (`SELECT … FOR UPDATE OF review_cases`). Si ya no está pendiente: `CaseAlreadyDecided` → 409.
2. Valida observación, clase y cajas.
3. Inserta `human_reviews` (decisión vigente), actualiza `detections.review_status` (confirmadas o descartadas) y el caso (`status`, `decided_at`, `decided_by`).
4. Busca los destinatarios activos, con consentimiento y con el lote del caso en su alcance; inserta una fila `notifications` `PENDIENTE_ENVIO` por destinatario (única por decisión y destinatario). Si no hay ninguno, registra `AVISO_SIN_DESTINATARIOS` y la web avisa «No hay destinatarios activos para este lote: no se envió ningún aviso».
5. Calcula `review_cases.notification_status` y registra `CASO_DECIDIDO` en auditoría.

Después, fuera de la petición (worker, `notificaciones.services.process_pending_notifications`):

6. Toma avisos pendientes con `SKIP LOCKED` y arrendamiento de 2 minutos; arma la plantilla (sección 15.3) y llama a WhatsApp Cloud API.
7. Éxito: `ENVIADO` con `provider_message_id`. Error temporal (red, 429, 5xx): espera 2, 4, 8 y 16 minutos; al quinto intento, `ERROR_ENVIO`. Error definitivo (4xx: número, plantilla, token): `ERROR_ENVIO` sin reintentos. Cada cambio recalcula el estado del aviso del caso. **Un error de envío nunca cambia la decisión** (RN-W03).

### 8.4 Descartar o marcar evidencia insuficiente

- **Descartado:** el indicio no corresponde a una plaga. Observación opcional (motivo). Todas las cajas pasan a `DESCARTADO`. Sin aviso (RN-02 del curso).
- **Evidencia insuficiente:** la foto no permite decidir (borrosa, ángulo, tamaño). Observación obligatoria con lo que falta. Sin aviso. El caso queda como pendiente de **volver a mirar en campo**: la bandeja filtrada por «Evidencia insuficiente» (y su CSV) es la lista de sectores para el siguiente recorrido.

### 8.5 Corregir una decisión

1. Solo el especialista, en un caso ya decidido, abre «Corregir la decisión».
2. Elige la nueva decisión, la observación y un **motivo de la corrección** (obligatorio).
3. El servidor bloquea el caso, comprueba que la decisión vigente sigue siendo la que el formulario vio (`expected_review_id`; si no: `StaleReview` → 409), marca la anterior como no vigente y crea la nueva con `supersedes` (W-14).
4. Avisos (DW-10): si la anterior era «Confirmado» y la nueva no, los avisos **pendientes** pasan a `NO_APLICA` («Anulado: la decisión se corrigió antes del envío»); los ya enviados no se retiran. Si la nueva es «Confirmado» y el caso nunca tuvo avisos efectivos, se encolan. Si ya se avisó una confirmación anterior, no se repite.
5. Auditoría `CASO_CORREGIDO` con el motivo; el historial del caso muestra todas las decisiones y cuál fue reemplazada.

### 8.6 Volver al sector (supervisor o jefe de fundo)

1. El WhatsApp trae lote, hilera, lateral, segmento/marcador, fecha y el enlace `https://<dominio>/casos/<id>/`.
2. En el celular, el enlace pide iniciar sesión (si no la tiene) y abre el caso; en pantallas angostas se muestra primero la ubicación y la decisión, después la foto.
3. «Cómo llegar» abre Google Maps con las coordenadas (si las hay); «Ver en el plano» abre el esquema del lote en la hilera del caso (ancla `#H<número>`), con su segmento; el marcador físico es la referencia en campo (el GPS es del controlador al emitir la secuencia, no de la planta exacta: RN-18 del maestro móvil).
4. Lo que se haga en campo después (acciones, tratamientos) no se registra en esta web (W-13).

### 8.7 Revisar fotos sin indicios (posibles falsos negativos)

1. Desde una sesión (WEB-10) o un caso, abrir una foto (WEB-06). Si su análisis fue `SIN_INDICIOS_IA` o `ERROR_DE_ANALISIS`, el especialista ve «Abrir caso para revisión».
2. Se crea un caso de origen `MANUAL`, sin cajas, que sigue el mismo flujo de decisión (y de aviso si se confirma).
3. Para el piloto se recomienda revisar una muestra de fotos sin indicios por lote para estimar falsos negativos (procedimiento y tamaño de muestra: Q-W10).

### 8.8 Errores de análisis

WEB-16 muestra conteos por estado de IA, el modelo activo y los últimos 100 errores con su mensaje. El administrador pulsa «Reencolar»: la tarea vuelve a `PENDIENTE_DE_ANALISIS` con `attempts = 0` (auditoría `TAREA_IA_REENCOLADA`). Solo se reencolan tareas con error.

### 8.9 Aprobación de cuentas y contraseña temporal

Ver 7.4 a 7.6. Los pedidos de restablecimiento (`POST /auth/password-reset-requests` desde la app) aparecen en WEB-13 y se atienden dando una contraseña temporal.

### 8.10 Mapa de navegación

```text
/ingresar/ ──► (contraseña temporal) /cuenta/contrasena/
     │
     ▼
Panel (/) ──► Casos (/casos/) ──► Caso (/casos/<id>/) ──► Foto (/capturas/<id>/) ──► Sesión
     │              ▲                 │      │                    │
     │              └── Guardar y siguiente  ├─► Mapa (?caso=) ───┘
     │                                       └─► Plano (?lote=)
     ├──► Mapa (/mapa/)        ├──► Plano (/plano/)       ├──► Sesiones (/sesiones/) ──► Sesión
     ├──► Reportes (/reportes/) ──► casos.csv            ├──► Avisos (/notificaciones/) ──► Caso
     ├──► Estado de la IA (/ia/) ──► Foto                 
     └──► Administración: Usuarios · Celulares · Destinatarios · Auditoría · Gestión (/gestion/)
WhatsApp ──► /casos/<id>/ (pide /ingresar/?next=… si no hay sesión)
```

---

## 9. Catálogo de pantallas

Formato: **ID — Nombre (ruta)** · Para quién · Contenido · Acciones · Validaciones y estados · Requerimientos. Las capturas de la implementación de referencia están en el Anexo A.

**WEB-01 — Ingresar (`/ingresar/`)** · Todos · Título, texto de la plataforma, correo, contraseña, aviso «¿Olvidaste tu contraseña? Pide una contraseña temporal al administrador.» · «Ingresar» · Mensajes de 7.3; bloqueo temporal; `next` solo del mismo dominio · RF-W01, RF-W02.

**WEB-02 — Cambiar contraseña (`/cuenta/contrasena/`)** · Usuario con sesión · Contraseña actual, nueva y confirmación; aviso si es obligatorio · «Guardar» · Política DW-25; quita `must_change_password`; mantiene la sesión · RF-W03.

**WEB-03 — Panel (`/`)** · Todos los roles de la web · Filtro de periodo (por defecto, últimos 30 días); tarjetas: casos pendientes (total) y antigüedad del más antiguo; confirmados, descartados y evidencia insuficiente del periodo; tiempo medio hasta la decisión; sesiones; fotos recibidas y rechazadas por calidad (%); fotos en cola de análisis, espera máxima y ms/foto; errores de análisis; avisos enviados y con error. Tabla de cobertura por lote (hileras con A y B cerrados / hileras activas, %, casos confirmados) con enlace al plano · «Aplicar» · KPIs con hasta 60 s de antigüedad; leyenda «los colores y conteos no expresan gravedad» · RF-W04, RF-W05.

**WEB-04 — Bandeja de casos (`/casos/`)** · Todos · Aviso de IA (`IA_AVISO` + `BANDEJA_AYUDA`); filtros (estado —por defecto pendientes—, lote, origen, desde, hasta, orden); tabla: miniatura, estado (y «manual»), ubicación (lote · hilera · lateral; segmento y marcador), hora de captura, cajas y confianza máxima, «abierto hace», estado del aviso; paginación de 25 · Abrir caso; filtrar; paginar · Se actualiza cada 15 s con la pestaña visible (P-2); la URL conserva los filtros (P-7) · RF-W06, RF-W07.

**WEB-05 — Caso (`/casos/<id>/`)** · Todos (decidir y corregir: especialista) · Estado; aviso de IA (origen IA); visor con cajas numeradas y controles (Cajas, +, −, ⤢, Original); tabla de cajas (clase sugerida, confianza, revisión); ficha de ubicación operativa y análisis; enlaces «Ver en el mapa», «Cómo llegar», «Ver en el plano», «Datos de la foto»; otra cámara de la misma secuencia; panel de decisión (pendiente) o decisión vigente, estado y lista de avisos, corrección e historial · «Guardar decisión», «Guardar y siguiente», «Guardar corrección»; atajos C, D, I, B, +, −, 0, Ctrl+Enter · RN-W01 a RN-W07; 409/422 en el mismo panel (P-4); el supervisor y el administrador ven «solo el especialista fitosanitario puede decidir este caso» · RF-W08 a RF-W14.

**WEB-06 — Foto (`/capturas/<id>/`)** · Todos · Foto (`revision`), lote, hilera, lateral, número de secuencia, cámara y celular, usuario de la cámara, hora, tamaño, calidad (perfil y motivos), «repite a», análisis (estado, modelo, error) · «Ver caso» o «Abrir caso para revisión» (especialista, foto sin caso con análisis `SIN_INDICIOS_IA` o `ERROR_DE_ANALISIS`) · Las fotos rechazadas por calidad no tienen análisis · RF-W15.

**WEB-07 — Mapa (`/mapa/`)** · Todos · Filtros (estado, lote, desde —por defecto hace 30 días—, hasta); leyenda; mapa con teselas OSM y atribución, contornos de lote, un punto por caso con coordenadas (color por estado, borde punteado si es aproximada); ventana emergente con lote · hilera · lateral (enlace al caso), estado, fecha, segmento y marcador; línea de resumen («N casos en el mapa · M sin coordenadas»; aviso si se alcanzó el tope) · Cambiar filtros recarga los datos; `?caso=<id>` centra y abre ese caso con su círculo de precisión · Texto «La posición es la del controlador al emitir la secuencia…» · RF-W16, RF-W17.

**WEB-08 — Plano de hileras (`/plano/`)** · Todos · Selector de lote, filtros de estado y fechas; «Hileras cubiertas (A y B): x / y»; una barra por hilera proporcional a su número de plantas, con ✔ si está cubierta, recuadros de los segmentos coloreados por el estado de mayor prioridad de sus casos (confirmado > pendiente > insuficiente > descartado) y conteos por estado a la derecha · «Aplicar»; clic en un segmento abre la bandeja del lote · No necesita coordenadas (casos sin GPS también aparecen) · RF-W18.

**WEB-09 — Sesiones (`/sesiones/`)** · Todos · Tabla: inicio, operador, modo, estado, pasadas, fotos, versión de la app; paginación · Abrir sesión · Solo lectura · RF-W19.

**WEB-10 — Sesión (`/sesiones/<id>/`)** · Todos · Operador, duración, versiones (app, configuración, perfil de calidad), calidad de fotos por estado; tablas de cámaras (rol, usuario, celular, vinculada, liberada), pasadas (hilera, lateral, orden, estado, secuencias completas/incompletas, horario), incidencias y fotos (hasta 500, con calidad y si tienen caso) · Abrir foto · Solo lectura · RF-W19.

**WEB-11 — Reportes (`/reportes/`)** · Todos · Formulario de filtros (estado, lote, fechas) y «Descargar CSV»; nota de impresión a PDF · Descargar · Hasta 50 000 filas; auditoría `EXPORTACION_CASOS` · RF-W20.

**WEB-12 — Avisos (`/notificaciones/`)** · Todos · Filtro por estado; tabla: creado, caso (enlace), destinatario, estado, intentos, enviado, error · Abrir caso · Solo lectura · RF-W21.

**WEB-13 — Usuarios (`/administracion/usuarios/`)** · Administrador · Solicitudes pendientes (nombre, correo, código, fecha, casillas de roles, «Aprobar», «Rechazar»); pedidos de restablecimiento; todas las cuentas (roles, estado, contraseña temporal pendiente) con «Bloquear», «Desbloquear», «Contraseña temporal» y «Roles» (casillas y «Guardar roles»); sin acciones sobre la propia cuenta · Ver 7.4 a 7.6 · RF-W22, RF-W23.

**WEB-14 — Celulares (`/administracion/dispositivos/`)** · Administrador · Celular (modelo e ID corto), plataforma y sistema, versión de la app, último usuario, última vez, estado · «Revocar» · Irreversible en esta versión · RF-W24.

**WEB-15 — Destinatarios WhatsApp (`/administracion/destinatarios/`)** · Administrador · Explicación (solo casos confirmados; consentimiento; cuenta de la web para abrir el enlace); tabla (nombre y correo de la cuenta asociada, función, celular, lotes o «Todos», consentimiento, activo, «Editar»); formulario (nombre, celular E.164, función, cuenta de la web, lotes, activo, «Aceptó recibir avisos por WhatsApp») · «Guardar» · Teléfono `^\+[1-9]\d{7,14}$`; sin consentimiento no recibe avisos · RF-W25.

**WEB-16 — Estado de la IA (`/ia/`)** · Administrador y especialista · Modelo activo (nombre, versión, clases, umbral); conteo por estado de IA; errores (foto, modelo, intentos, fecha, mensaje) · «Reencolar» (solo administrador) · RF-W26.

**WEB-17 — Auditoría (`/auditoria/`)** · Administrador · Búsqueda por acción o ID; tabla: fecha, usuario («Sistema» si es el worker), acción, entidad, antes → después; paginación de 50 · Solo lectura · RF-W27.

**WEB-18 — Gestión (`/gestion/`, Django Admin)** · Superusuario (responsable técnico) · Catálogos, perfiles de calidad, modelos de IA; consulta del resto · Ver 7.8 · RF-W28.


---

## 10. Requerimientos

### 10.1 Requerimientos funcionales

Prioridad: **P0** crítica para el piloto, **P1** alta, **P2** media. Fase: primera fase del plan (sección 22) en que debe funcionar.

| ID | Requerimiento | Prio. | Fase | Criterio de aceptación | Fuente |
|---|---|---|---|---|---|
| RF-W01 | Ingresar con correo y contraseña a la web y salir. | P0 | FW1 | Con credenciales correctas y rol de la web se llega al panel; «Salir» cierra la sesión (POST). | RNF05 del curso; D-28 |
| RF-W02 | Mostrar el motivo cuando no se puede entrar, sin revelar si un correo existe. | P0 | FW1 | Contraseña mala → mensaje genérico; el estado de la cuenta solo con la contraseña correcta; operador → «Usa la app móvil»; 5 fallos → bloqueo de 15 min. | Sección 7.3 |
| RF-W03 | Cambiar la contraseña y exigirlo con contraseña temporal. | P0 | FW1 | Con `must_change_password` toda ruta lleva a WEB-02; tras cambiarla se quita la marca y se mantiene la sesión. | D-07 |
| RF-W04 | Panel con indicadores del periodo y cobertura por lote. | P1 | FW5 | Los números coinciden con consultas directas a la base de datos de prueba (prueba `test_panel`). | CU9 del curso (dashboard) |
| RF-W05 | Cobertura por lote según RN-13 del maestro móvil. | P1 | FW5 | Una hilera cuenta solo con LATERAL_A y LATERAL_B cerrados (COMPLETED, o INCOMPLETE con incidencia). | RN-13 |
| RF-W06 | Bandeja de casos con filtros (estado, lote, origen, fechas), orden y paginación. | P0 | FW2 | Por defecto solo pendientes, más antiguos primero; filtros combinables; 25 por página. | HU03, CU5 del curso |
| RF-W07 | Actualizar la bandeja sin recargar. | P1 | FW2 | Un caso nuevo aparece en ≤ 15 s con la pestaña visible; el sondeo conserva filtros y página. | DW-16 |
| RF-W08 | Ver la foto del caso con las cajas de la IA alineadas, acercar, mover y cargar el original. | P0 | FW2 | Las cajas coinciden con la foto a cualquier tamaño (SVG con `viewBox`); «Original» carga la resolución completa bajo pedido. | RF08 y HU03 del curso |
| RF-W09 | Ver la ubicación operativa del caso y la otra cámara de la secuencia. | P0 | FW2 | Lote, hilera, lateral, segmento, marcador, coordenadas con precisión y fuente, hora, cámara y operador; enlace a la otra foto. | CU5 del curso; RN-03 del curso |
| RF-W10 | Confirmar, descartar o marcar evidencia insuficiente, con observación. | P0 | FW3 | Solo el especialista; observación obligatoria al confirmar o marcar insuficiente; una sola decisión vigente. | RF09, CU6, CU7, RN-01 del curso |
| RF-W11 | Marcar cajas incorrectas y la clase confirmada. | P1 | FW3 | Las cajas marcadas quedan `DESCARTADO` y las demás `CONFIRMADO_POR_ESPECIALISTA` al confirmar; la clase debe ser del modelo. | Métricas de la Esp. v2.0 §17.3 |
| RF-W12 | «Guardar y siguiente». | P1 | FW3 | Tras guardar, abre el pendiente más antiguo distinto del actual; si no hay, vuelve a la bandeja. | Productividad |
| RF-W13 | Evitar decisiones simultáneas. | P0 | FW3 | Si dos especialistas guardan a la vez, solo uno queda; el otro ve 409 con quién y cuándo decidió. | DW-08 |
| RF-W14 | Corregir una decisión con motivo y conservar el historial. | P1 | FW3 | La anterior queda no vigente y enlazada; motivo obligatorio; versión vieja → 409; avisos según DW-10. | DW-08, DW-10 |
| RF-W15 | Abrir un caso manual sobre una foto sin indicios o con error de análisis. | P1 | FW4 | Solo el especialista; idempotente; no se permite sobre fotos rechazadas por calidad o sin análisis terminado. | DW-07 |
| RF-W16 | Mapa con los casos que tienen coordenadas y los contornos de lote. | P0 | FW4 | Un punto por caso en `[lon, lat]`; color por estado; aproximados con borde punteado; resumen de casos sin coordenadas. | RF08, HU03, CU9 del curso |
| RF-W17 | Abrir el mapa centrado en un caso. | P2 | FW4 | `/mapa/?caso=<id>` centra a zoom 19, abre la ventana y dibuja el círculo de precisión. | Sección 8.6 |
| RF-W18 | Plano de hileras por lote con segmentos y conteos por estado. | P0 | FW4 | Hileras proporcionales al número de plantas; segmentos en su posición; cobertura por hilera. | Archivo de mapeo; DW-14 |
| RF-W19 | Consultar sesiones de monitoreo y su detalle. | P1 | FW4 | Pasadas, cámaras, incidencias y fotos con calidad; solo lectura. | Integrador §9.3 |
| RF-W20 | Exportar casos y decisiones a CSV. | P1 | FW5 | UTF-8 con BOM; columnas de 15.2; celdas que empiezan con `= + - @` se escapan; auditoría de la exportación. | Integrador §9.3 (reportes); pretest–postest |
| RF-W21 | Consultar el estado de los avisos por WhatsApp. | P1 | FW4 | Lista con estado, intentos, fecha de envío y error. | CU8 del curso |
| RF-W22 | Aprobar, rechazar, bloquear y desbloquear cuentas, y asignar o cambiar roles. | P0 | FW1 | Transiciones de 7.4; auditoría; un bloqueo corta la sesión web y la de la app; roles editables desde WEB-13. | D-05 del maestro móvil |
| RF-W23 | Dar una contraseña temporal. | P0 | FW1 | Se muestra una sola vez; cumple la política; marca el cambio obligatorio; revoca los refresh de la app. | D-07 |
| RF-W24 | Revocar un celular. | P1 | FW1 | `revoked_at` guardado; la API responde `DEVICE_REVOKED`. | §28.6 del maestro móvil |
| RF-W25 | Configurar destinatarios de avisos con alcance por lote y consentimiento. | P0 | FW4 | Sin consentimiento o inactivo no recibe; lotes vacíos = todos; teléfono E.164. | CU8, HU04 del curso |
| RF-W26 | Ver el estado de la IA y reencolar errores. | P1 | FW4 | Conteos por estado; últimos 100 errores; reencolar solo administrador y solo errores. | §28.8 del maestro móvil |
| RF-W27 | Consultar la auditoría. | P2 | FW5 | Búsqueda por acción o ID; paginación. | Trazabilidad |
| RF-W28 | Gestionar catálogos, perfiles de calidad y modelos de IA en Django Admin (superusuario). | P0 | FW1 | Un solo modelo activo; contorno GeoJSON por lote; evidencia y cuentas en solo lectura. | Q-11 del maestro móvil |
| RF-W29 | Avisar por WhatsApp después de cada confirmación. | P0 | FW4 | Un aviso por destinatario y decisión; reintentos; el error no cambia la decisión; enlace al caso. | RF10, CU8, RN-02 del curso |
| RF-W30 | Abrir el caso desde el enlace del WhatsApp en un celular. | P0 | FW4 | Pide ingresar y vuelve al caso; a 390 px se lee sin desplazamiento horizontal; primero ubicación y decisión. | CU8 del curso; §9.8 |

### 10.2 Requerimientos no funcionales

| ID | Atributo | Requerimiento | Cómo se verifica |
|---|---|---|---|
| RNF-W01 | Rendimiento (RNF04 del curso) | Toda pantalla de la web responde en el servidor en < 1,5 s con 50 000+ plantas en catálogo y al menos 100 000 fotos; la bandeja y el caso en < 300 ms. | Medición de la sección 18 (Anexo H) en local y repetida en el entorno piloto. |
| RNF-W02 | Seguridad (RNF05 del curso) | Autenticación obligatoria; permisos por rol en el servidor (matriz 7.2); CSRF; cookies seguras; sin secretos en el navegador. | `test_todas_las_rutas_estan_protegidas`, `test_paginas_por_rol`, `test_csrf_obligatorio`; `check --deploy`. |
| RNF-W03 | Integridad | Decisiones, avisos y auditoría en una sola transacción; concurrencia controlada. | `test_confirmar_registra_todo_en_una_transaccion`, `test_un_solo_ganador`. |
| RNF-W04 | Trazabilidad | Cada decisión guarda quién, cuándo, qué análisis y modelo vio, qué cajas rechazó y por qué se corrigió. | Pruebas de `revision`; auditoría. |
| RNF-W05 | Disponibilidad | La web no depende de WhatsApp, del worker ni de Cloudinary para responder (las fotos fallan aisladas). | Revisión de código: ninguna llamada externa en las vistas. |
| RNF-W06 | Usabilidad | Revisar un caso típico con decisión toma ≤ 30 s (meta) con «Guardar y siguiente»; estados con icono y texto. | Prueba con el especialista (Medir). |
| RNF-W07 | Responsive | Escritorio ≥ 1280, tablet 768–1279 y celular < 768 sin desplazamiento horizontal de la página. | Capturas del Anexo A; revisión en celulares reales. |
| RNF-W08 | Accesibilidad | W-23. | Revisión con teclado y con el inspector de accesibilidad del navegador. |
| RNF-W09 | Mantenibilidad | Reglas en servicios con pruebas; plantillas sin lógica de negocio; `check` y `makemigrations --check` limpios. | W-20. |
| RNF-W10 | Costos | La web no descarga originales salvo pedido explícito; miniaturas y `revision` por CDN; teselas de OSM sin costo. | Panel de uso de Cloudinary (§28.12 del maestro móvil). |
| RNF-W11 | Privacidad | Teléfonos de destinatarios solo visibles para el administrador; consentimiento registrado; fotos solo con sesión. | Matriz 7.2; WEB-15. |
| RNF-W12 | Hora local | Todas las fechas en pantalla en `America/Lima`; filtros por fecha local. | `TIME_ZONE`; pruebas de filtros. |

### 10.3 Reglas de negocio de la web

| ID | Regla |
|---|---|
| RN-W01 | Solo un usuario `ACTIVO` con rol `ESPECIALISTA_FITOSANITARIO` decide, corrige o abre casos manuales. El servicio lo comprueba aunque la vista ya lo haya hecho (RN-01 del curso). |
| RN-W02 | Un caso tiene como máximo una decisión vigente. Solo se decide un caso `PENDIENTE_REVISION`; después solo se corrige. |
| RN-W03 | Solo la decisión `CONFIRMADO_POR_ESPECIALISTA` genera avisos. Pendiente, descartado o evidencia insuficiente nunca avisan (RN-02 del curso). Un error de envío no cambia la decisión. |
| RN-W04 | La observación es obligatoria al confirmar (observación fitosanitaria, CU7) y al marcar evidencia insuficiente (qué falta); máximo 2 000 caracteres. No se registran tratamientos ni dosis (W-13). |
| RN-W05 | Corregir exige motivo y que la decisión vigente sea la que el especialista tenía en pantalla. La decisión anterior nunca se borra. |
| RN-W06 | Un análisis nuevo de la misma foto (otro modelo o reintento) **con cajas** actualiza el análisis y los conteos de un caso pendiente; uno **sin cajas** no cierra ni cambia el caso (queda pendiente con el análisis anterior). Ningún análisis cambia una decisión tomada. |
| RN-W07 | El resultado de la IA es un indicio sugerido y no vinculante (RN-06 del curso): sin decisión del especialista no hay aviso ni registro oficial. |
| RN-W08 | La web no crea, edita ni borra datos de campo (sesiones, pasadas, secuencias, capturas, calidad, incidencias), análisis, cajas (salvo su `review_status`), decisiones ni avisos enviados. |
| RN-W09 | Un caso manual solo se abre sobre una foto aceptada por calidad y con análisis terminado `SIN_INDICIOS_IA` o `ERROR_DE_ANALISIS`; una foto tiene como máximo un caso. |
| RN-W10 | Un destinatario recibe avisos solo si está activo, tiene consentimiento registrado y el lote del caso está en su alcance (o su alcance está vacío). |
| RN-W11 | Solo las fotos `UTILIZABLE` o `PENDIENTE_REVISION_TECNICA` se analizan (DW-11). |
| RN-W12 | La ubicación del caso se fija al abrirlo: contexto de la repetición si existe; si no, el de la secuencia; coordenadas del GPS, si no del marcador, si no ninguna (DW-13). |
| RN-W13 | Una hilera está cubierta en un periodo cuando tiene pasadas cerradas en LATERAL_A y LATERAL_B (COMPLETED, o INCOMPLETE con incidencia) iniciadas en ese periodo (RN-13 del maestro móvil). |
| RN-W14 | Nadie cambia el estado de su propia cuenta ni se quita su propio rol de administrador. |

### 10.4 Historias de usuario

| ID | Historia | Requerimientos |
|---|---|---|
| HU-W01 | Como especialista, quiero ver en una bandeja los indicios que la IA sugirió, los más antiguos primero, para no dejar casos sin revisar. | RF-W06, RF-W07 |
| HU-W02 | Como especialista, quiero ver la foto con las cajas y acercarme para distinguir el chanchito de polvo o restos. | RF-W08 |
| HU-W03 | Como especialista, quiero saber exactamente de qué lote, hilera, lateral y segmento viene la foto. | RF-W09, RF-W16, RF-W18 |
| HU-W04 | Como especialista, quiero confirmar, descartar o marcar evidencia insuficiente con una observación, y pasar al siguiente caso. | RF-W10, RF-W11, RF-W12 |
| HU-W05 | Como especialista, quiero corregir una decisión equivocada sin borrar la anterior. | RF-W14 |
| HU-W06 | Como especialista, quiero revisar fotos donde la IA no vio nada para detectar lo que se le pasó. | RF-W15 |
| HU-W07 | Como supervisor de zona, quiero recibir un WhatsApp cuando el especialista confirme un caso de mi zona y abrirlo en mi celular. | RF-W25, RF-W29, RF-W30 |
| HU-W08 | Como jefe de fundo, quiero ver en el panel y en el plano cuánto se cubrió y dónde hay casos confirmados. | RF-W04, RF-W05, RF-W18 |
| HU-W09 | Como administrador, quiero aprobar cuentas, asignar roles, dar contraseñas temporales y revocar celulares. | RF-W22, RF-W23, RF-W24 |
| HU-W10 | Como investigador del proyecto, quiero exportar casos y decisiones para el análisis pretest–postest. | RF-W20 |

### 10.5 Trazabilidad con el Curso Integrador II

| Integrador | Cómo lo cubre la web |
|---|---|
| Épica 4 (plataforma web, mapas y revisión de casos) | Secciones 3, 8 y 9 completas. |
| HU03 (especialista ve hallazgos en el mapa con evidencia y confirma o descarta) | WEB-04, WEB-05, WEB-07, WEB-08; RF-W06 a RF-W18. |
| HU04 (supervisor recibe notificaciones tras confirmación) | WEB-15, RF-W25, RF-W29, RF-W30. |
| RF07 (analizar con IA y señalar indicios) | Lo hace el worker (§28.8 del maestro móvil); la web lo muestra (WEB-05, WEB-16). |
| RF08 (visualizar indicios en la plataforma) | WEB-04, WEB-05, WEB-07. |
| RF09 (confirmar o descartar) | RF-W10, RN-W01. |
| RF10 (WhatsApp al jefe de fundo y supervisor tras confirmar) | RF-W29, RN-W03, RN-W10. |
| RNF04 (50 000+ plantas, API < 1,5 s) | RNF-W01; medición de la sección 18. |
| RNF05 (autenticación y roles) | Sección 7; RNF-W02. |
| RN-01 (solo el especialista confirma) | RN-W01. |
| RN-02 (sin observación ni aviso si está pendiente o descartado) | RN-W03, RN-W04. |
| RN-06 (IA no vinculante) | RN-W07, W-11. |
| CU5 (revisar indicios) | Sección 8.2. |
| CU6 (confirmar o descartar) | Sección 8.3 y 8.4. |
| CU7 (registrar observación) | Observación obligatoria al confirmar, sin tratamiento (RN-W04). |
| CU8 (notificación con ubicación y enlace controlado) | Sección 8.3 y 15.3. |
| CU9 (dashboard y mapa) | WEB-03, WEB-07, WEB-08. |
| Sprint 4 (frontend y backend web, módulo cartográfico, interfaz de validación) | Fases FW1 a FW4 (sección 22). |
| Sprint 5 (mensajería oficial, pruebas de extremo a extremo) | FW4 (avisos), FW5 (panel, reportes) y FW6 (pruebas de extremo a extremo). |
| Sprint 6 (despliegue y piloto; medición de indicadores) | FW6, usando el panel y la exportación CSV construidos en FW5. |

---

## 11. Modelo de datos del servidor (visto desde la web)

El código completo de los modelos está en el Anexo B. Todos usan `Meta.db_table` con los nombres de la Especificación v2.0 §21 y del Maestro App Móvil §28.5; las tablas marcadas *(web v1.0)* son nuevas.

### 11.1 Tablas, modelos y apps

| Tabla | Modelo | App | Quién escribe | Qué usa la web |
|---|---|---|---|---|
| `users` | `User` | `cuentas` | API (registro), web (administración) | Ingreso, nombres, estados. |
| `user_roles` *(web v1.0)* | `UserRole` | `cuentas` | Web (aprobación, roles), Admin | Permisos (DW-04). |
| `devices` | `Device` | `cuentas` | API (login) | WEB-14; revocar. |
| `password_reset_requests` | `PasswordResetRequest` | `cuentas` | API | WEB-13. |
| `field_lots` (+ `geometry`) | `FieldLot` | `campo` | Admin | Filtros, mapa (contorno), plano. |
| `field_rows`, `field_segments`, `markers` | `FieldRow`, `FieldSegment`, `Marker` | `campo` | Admin | Plano, ubicación del caso. |
| `quality_profiles` | `QualityProfile` | `campo` | Admin | — (bootstrap de la app). |
| `monitoring_sessions`, `session_devices`, `monitoring_passes`, `marker_changes`, `capture_sequences`, `incidents` | `MonitoringSession`, `SessionDevice`, `MonitoringPass`, `MarkerChange`, `CaptureSequence`, `Incident` | `monitoreo` | API (sincronización) | WEB-09/10, cobertura, ubicación del caso. |
| `captures`, `quality_results` | `Capture`, `QualityResult` | `evidencias` | API (confirmación) | Fotos, calidad, URLs firmadas. |
| `model_configs` *(web v1.0)* | `ModelConfig` | `ia` | Admin | Modelo activo, clases confirmables. |
| `ai_tasks` (+ `image_width`, `image_height`) | `AiTask` | `ia` | API (crear), worker (resultado), web (reencolar) | Estado de IA, tamaño para el visor. |
| `detections` | `Detection` | `ia` | Worker; web (`review_status`) | Cajas del visor. |
| `review_cases` *(web v1.0)* | `Case` | `revision` | Worker (abrir), web (decidir) | Bandeja, caso, mapa, plano, panel. |
| `human_reviews` | `HumanReview` | `revision` | Web | Decisión vigente e historial. |
| `notification_recipients` *(web v1.0)* (+ `notification_recipient_lots`) | `NotificationRecipient` | `notificaciones` | Web | Destinatarios. |
| `notifications` | `Notification` | `notificaciones` | Web (encolar), worker (enviar) | Estado del aviso. |
| `audit_events` | `AuditEvent` | `auditoria` | Servicios | WEB-17. |
| `web_cache` *(web v1.0)* | (caché de Django) | `auditoria` (migración) | Web | Bloqueo de ingreso, KPIs. |
| `token_blacklist_*` | SimpleJWT | — | API, web (bloqueo y contraseña temporal) | Revocar sesiones de la app. |
| `django_session`, `auth_*`, `django_admin_log`, `users_groups`, `users_user_permissions` | Django | — | Django | Sesión web, Admin. |

### 11.2 Relaciones principales

```text
users ─┬─< user_roles
       ├─< devices (último usuario)
       └─< monitoring_sessions (operator) ─┬─< session_devices
                                           ├─< monitoring_passes ─┬─< marker_changes
                                           │        (lot, row)    └─< capture_sequences ─< captures ─┬─ quality_results (1:1)
                                           └─< incidents                    (segment, marker, GPS)  ├─< ai_tasks ─< detections
                                                                                                    │      └── model_configs
                                                                                                    └── review_cases (1:1) ─┬─< human_reviews (supersedes → human_reviews)
                                                                                                         (lot,row,segment,  └─< notifications ── notification_recipients ─< lotes
                                                                                                          marker, lat/lon)
field_lots ─< field_rows ─┬─< field_segments ─< markers
                          └─< markers
audit_events (entity_type, entity_id) — referencia lógica a cualquier fila
```

### 11.3 Índices y para qué sirven

| Índice | Tabla | Consulta que acelera |
|---|---|---|
| `caso_bandeja_idx` (`status`, `opened_at`) | `review_cases` | Bandeja por defecto (pendientes por antigüedad) y «siguiente». |
| `caso_plano_idx` (`lot`, `row`, `status`) | `review_cases` | Plano de hileras y conteos por lote. |
| `caso_fecha_idx` (`captured_at`) | `review_cases` | Filtros por fecha, mapa, panel, exportación. |
| `ai_cola_idx` (`status`, `available_at`) | `ai_tasks` | Cola del worker y panel. |
| `aviso_cola_idx` (`status`, `available_at`) | `notifications` | Cola de avisos. |
| `captura_fecha_idx` (`captured_at`) | `captures` | Panel (fotos del periodo). |
| `pasada_cobertura_idx` (`row`, `lateral_code`, `status`) | `monitoring_passes` | Cobertura RN-13. |
| `sesion_inicio_idx` (`started_at`) | `monitoring_sessions` | Lista de sesiones, panel. |
| `auditoria_entidad_idx`, `auditoria_fecha_idx` | `audit_events` | Historial por entidad y búsqueda. |
| Únicos: `user_roles_unico`, `ai_task_por_modelo`, `una_decision_vigente_por_caso` (parcial), `aviso_unico_por_decision`, `un_solo_modelo_activo` (parcial), `captures.cloudinary_public_id`, `review_cases.capture_id` | — | Integridad e idempotencia. |

### 11.4 Qué lee y qué escribe la web

- **Lee:** todo.
- **Escribe:** `users` (estado, contraseña, `must_change_password`, aprobación), `user_roles`, `devices.revoked_*`, `password_reset_requests` (atendida), `review_cases` (decisión y estado del aviso; los manuales también se crean aquí), `human_reviews`, `detections.review_status`, `notifications` (crear, anular), `notification_recipients`, `ai_tasks` (solo reencolar un error), `audit_events`, `web_cache`, `django_session`, `token_blacklist_blacklistedtoken`.
- **Nunca escribe:** tablas de `monitoreo`, `captures`, `quality_results`, cajas (salvo `review_status`), resultados de análisis.

### 11.5 Precisiones al modelo de §28.5 del Maestro App Móvil

| Tema | §28.5 del maestro móvil | Esta versión | Acción |
|---|---|---|---|
| Roles | `users.roles` | Tabla `user_roles` (DW-04); el JWT de la app sigue llevando la lista de roles | Actualizar §28.5 en la v2.1 del maestro móvil. |
| Caso | «`detection_id` o caso» | `review_cases` y `human_reviews.case_id` | Ídem. |
| Modelos de IA | `ai_tasks.model_config_id` sin tabla descrita | `model_configs` con `classes`, umbrales y `active` | Ídem. |
| Tamaño analizado | — | `ai_tasks.image_width`, `image_height` | Ídem. |
| Destinatarios | `notifications.recipient` (texto) | `notification_recipients` + copia de nombre y teléfono en `notifications` | Ídem. |
| `NO_APLICA` | Estado de `notifications` | Estado del caso (`review_cases.notification_status`) y, en filas, aviso anulado por corrección | Ídem. |
| Análisis de rechazadas | Toda captura confirmada crea `ai_task` | Solo las aceptadas por calidad (DW-11) | Actualizar §15.8 y §28.8 del maestro móvil. |
| Contorno de lote | — | `field_lots.geometry` (GeoJSON) | Ídem. |
| `retake_context` | jsonb | Se guarda tal como llega de la app (`camelCase`: `segmentId`, `markerId`, `lat`, `lon`, `gpsAccuracyM`…) | La API debe guardarlo sin transformar. |
| `password_reset_requests` | `id`, `email`, `requested_at`, `handled_by`, `handled_at` | + `user_id` (si el correo existe; lo llena la API) y `status` (`PENDIENTE`, `ATENDIDA`, `DESCARTADA`); la web también atiende por correo | Ídem. |
| `devices` | `revoked_at` | + `revoked_by`, `first_seen_at` | Ídem. |
| `users` | Campos de 28.5 | + `is_staff`, `is_superuser`, `last_login` (Django), `accepted_privacy_notice_at`, `approved_by`, `approved_at` | Ídem. |
| `human_reviews` | `detection_id` o caso, `reviewer_id`, `decision`, `observation`, `reviewed_at` | `case_id`, + `ai_task_id`, `confirmed_class`, `rejected_detection_ids` (jsonb), `supersedes_id`, `correction_reason`, `is_current` | Ídem. |

---

## 12. Servicios (contratos de funciones)

Código en el Anexo C. Las funciones que cambian un estado oficial, una cuenta o la configuración usan `transaction.atomic()` y registran auditoría (W-06, W-15). Las del worker (`claim_next_task`, `save_analysis_result`, `mark_analysis_failed`, `mark_sent`, `mark_failed`) usan transacciones cortas y dejan su rastro en `ai_tasks` y `notifications`, no en `audit_events` (salvo la apertura de un caso). `enqueue_analysis` corre dentro de la transacción de la confirmación de la API.

| Función | Quién la llama | Qué hace | Errores |
|---|---|---|---|
| `ia.services.enqueue_analysis(capture)` | API, dentro de la confirmación de captura | Crea la `ai_task` del modelo activo si la foto es aceptada por calidad; idempotente. | `NoActiveModel` |
| `ia.services.claim_next_task(worker_id)` | Worker | Toma la tarea más antigua disponible con `SKIP LOCKED` y arrendamiento de 5 min. | — |
| `ia.services.save_analysis_result(task, boxes, w, h, ms, model_version, raw)` | Worker | En una transacción: reemplaza cajas, fija estado, tamaño analizado y tiempos; con cajas abre el caso. | — |
| `ia.services.mark_analysis_failed(task, error)` | Worker | Reintento con espera 30, 60 s…; al tercero `ERROR_DE_ANALISIS`. | — |
| `ia.services.requeue_failed(task_id, user)` | Web (WEB-16) | Vuelve a la cola una tarea con error. | `InvalidTaskState` |
| `revision.services.case_location(capture)` | Servicios de revisión | Ubicación según RN-W12. | — |
| `revision.services.open_case_from_analysis(task)` | `save_analysis_result` | Crea o actualiza el caso (RN-W06). | — |
| `revision.services.open_manual_case(capture_id, user)` | Web (WEB-06) | Caso `MANUAL` (RN-W09); devuelve `(caso, creado)`. | `PermissionDenied`, `ValidationError` |
| `revision.services.decide_case(case_id, user, decision, observation, confirmed_class, rejected_detection_ids)` | Web (WEB-05) | Decisión + cajas + caso + avisos + auditoría (sección 8.3). | `PermissionDenied`, `ValidationError` (por campo), `CaseAlreadyDecided` |
| `revision.services.correct_decision(case_id, user, decision, observation, correction_reason, expected_review_id, …)` | Web (WEB-05) | Corrección (sección 8.5). | `PermissionDenied`, `ValidationError`, `StaleReview` |
| `notificaciones.services.enqueue_for_review(review)` | `decide_case`, `correct_decision` | Crea avisos por destinatario (RN-W10). | — |
| `notificaciones.services.cancel_pending_for_case(case)` | `correct_decision` | Pendientes → `NO_APLICA`. | — |
| `notificaciones.services.case_notification_summary(case)` | Servicios | Resume el estado del aviso del caso. | — |
| `notificaciones.services.process_pending_notifications(client, worker_id, limit)` | Worker | Toma, envía y marca avisos. | — |
| `cuentas.services.approve_user`, `set_roles`, `change_status`, `set_temporary_password`, `revoke_device` | Web (WEB-13, WEB-14) | Sección 7. | `PermissionDenied`, `ValidationError` |
| `evidencias.media.signed_image_url(capture, variant)` | Vistas | URL firmada `miniatura`, `revision` u `original`. | `ValueError` |
| `auditoria.services.record(entity_type, entity_id, action, user, before, after)` | Servicios | Inserta el evento. | — |

---

## 13. Estados: transiciones y quién los cambia

### 13.1 Análisis de IA (`ai_tasks.status`)

| Desde | Hacia | Quién | Condición |
|---|---|---|---|
| (nueva) | `PENDIENTE_DE_ANALISIS` | API (confirmación) | Foto aceptada por calidad y modelo activo. |
| `PENDIENTE_DE_ANALISIS` | `EN_ANALISIS` | Worker (`claim_next_task`) | `available_at` vencido. |
| `EN_ANALISIS` | `EN_ANALISIS` (otro worker) | Worker | Arrendamiento vencido (`locked_until`). |
| `EN_ANALISIS` | `INDICIO_SUGERIDO_POR_IA` / `SIN_INDICIOS_IA` | Worker | Con cajas / sin cajas. |
| `EN_ANALISIS` | `PENDIENTE_DE_ANALISIS` | Worker | Fallo con intentos disponibles (espera creciente). |
| `EN_ANALISIS` | `ERROR_DE_ANALISIS` | Worker | Tercer fallo. |
| `ERROR_DE_ANALISIS` | `PENDIENTE_DE_ANALISIS` | Web (administrador) | «Reencolar». |

### 13.2 Caso (`review_cases.status`)

| Desde | Hacia | Quién | Efecto |
|---|---|---|---|
| (nuevo) | `PENDIENTE_REVISION` | Worker (origen IA) o especialista (origen MANUAL) | Aparece en la bandeja. |
| `PENDIENTE_REVISION` | `CONFIRMADO_POR_ESPECIALISTA` | Especialista | Avisos `PENDIENTE_ENVIO`. |
| `PENDIENTE_REVISION` | `DESCARTADO` / `EVIDENCIA_INSUFICIENTE` | Especialista | Sin avisos. |
| Cualquier decidido | Cualquier decisión | Especialista (corrección) | Nueva `human_review`; avisos según DW-10. |

Un caso nunca vuelve a `PENDIENTE_REVISION`.

### 13.3 Cajas (`detections.review_status`)

`PENDIENTE_REVISION` hasta la decisión; con «Confirmado», las marcadas como incorrectas pasan a `DESCARTADO` y el resto a `CONFIRMADO_POR_ESPECIALISTA`; con «Descartado» o «Evidencia insuficiente», todas toman ese valor. Una corrección las recalcula.

### 13.4 Aviso (`notifications.status`) y resumen del caso (`review_cases.notification_status`)

| Fila | Desde | Hacia | Quién |
|---|---|---|---|
| Aviso | (nuevo) | `PENDIENTE_ENVIO` | Confirmación |
| Aviso | `PENDIENTE_ENVIO` | `ENVIADO` | Worker (éxito) |
| Aviso | `PENDIENTE_ENVIO` | `PENDIENTE_ENVIO` (más tarde) | Worker (error temporal, intento < 5) |
| Aviso | `PENDIENTE_ENVIO` | `ERROR_ENVIO` | Worker (error definitivo o 5.º intento) |
| Aviso | `PENDIENTE_ENVIO` | `NO_APLICA` | Corrección de un confirmado |

Resumen del caso: sin avisos efectivos → `NO_APLICA`; alguno pendiente → `PENDIENTE_ENVIO`; si no, alguno con error → `ERROR_ENVIO`; si no → `ENVIADO`.

### 13.5 Cuenta (`users.status`)

Sección 7.4. La app crea las cuentas en `PENDIENTE_APROBACION`.

---

## 14. Fotos en la web (Cloudinary)

### 14.1 Variantes y cuándo se usan

| Variante | URL generada | Transformación (consola de Cloudinary) | Uso | Costo aproximado |
|---|---|---|---|---|
| `miniatura` | `…/image/authenticated/s--<firma>--/t_miniatura/v<versión>/<public_id>.jpg` | `c_limit,w_400,q_auto` | Bandeja, otra cámara, listas | Una transformación por foto (se cachea en la CDN) + pocos KB. |
| `revision` | `…/t_revision/v<versión>/<public_id>.jpg` | `c_limit,w_1600,q_auto` | Visor del caso y de la foto | Una transformación por foto + unos cientos de KB. |
| `original` | `…/v<versión>/<public_id>.jpg` (sin transformación) | — | Botón «Original» | Unos 4 MB de ancho de banda por apertura. |

- Las URLs las genera `evidencias.media.signed_image_url` con `type="authenticated"`, `sign_url=True`, `secure=True` y la `version` guardada en `captures.cloudinary_version` (verificado con el SDK 1.46.2: produce `/image/authenticated/s--…--/t_revision/v…/`).
- Las transformaciones `miniatura` y `revision` deben existir con esos nombres y con **transformaciones estrictas** activadas (§28.7 del maestro móvil). Si faltan, la web muestra imágenes rotas: es una tarea de configuración (T-W03).
- En el plan Free las URLs firmadas no caducan (§28.7): por eso solo se renderizan en páginas con sesión, las respuestas llevan `Cache-Control: no-store` (P-8) y el WhatsApp nunca lleva la URL de la foto (DW-26).

### 14.2 Cajas sobre la foto

- El worker guarda las cajas en **píxeles de la imagen que analizó**, después de aplicar la orientación EXIF (por ejemplo con `PIL.ImageOps.exif_transpose`), y guarda ese tamaño en `ai_tasks.image_width` y `image_height`.
- El visor pone la imagen y un `<svg viewBox="0 0 W H" preserveAspectRatio="none">` en el mismo contenedor; cada caja es un `<rect x y width height>` con esos píxeles. El navegador escala ambos juntos: no hay cálculo de escala en Python ni en JavaScript, y acercar o mover (transform CSS del contenedor) mantiene la alineación.
- Si `image_width` no existe (análisis antiguo), se usa `captures.width`/`height` (tamaño informado por la app).
- Los números del SVG se escriben con `|stringformat:".1f"` (W-16).
- Verificación pendiente en campo (CP-W14): una foto vertical tomada con el celular girado debe mostrar las cajas en el mismo lugar en la miniatura, en `revision` y en el original.

### 14.3 Reglas

W-10. Además: la web nunca llama a la API de administración de Cloudinary; no hay botón para borrar fotos (retención: Q-W13); el original se carga solo por acción del usuario.

---

## 15. Contratos de salida

### 15.1 GeoJSON del mapa (`GET /mapa/datos/`)

Parámetros opcionales: `estado`, `lote`, `origen`, `desde`, `hasta` (fechas locales `AAAA-MM-DD`). Respuesta comprimida con gzip (no contiene secretos).

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "geometry": {"type": "Point", "coordinates": [-75.7302, -14.0601]},
      "properties": {
        "id": "6175028e-…", "status": "PENDIENTE_REVISION", "statusLabel": "Pendiente de revisión",
        "lot": "SWG 1", "row": 5, "lateral": "Lateral A", "segment": "SWG 1-S1", "marker": "M1",
        "capturedAt": "02/10/2026 21:14", "locationSource": "GPS", "accuracyM": 4.0, "url": "/casos/6175028e-…/"
      }
    }
  ],
  "lots": [{"type": "Feature", "geometry": {"type": "Polygon", "coordinates": [[[-75.73, -14.06], …]]}, "properties": {"id": "SWG1", "code": "SWG 1"}}],
  "meta": {"sinUbicacion": 1, "truncated": false, "limit": 5000}
}
```

Reglas: coordenadas en orden GeoJSON `[lon, lat]`; solo casos con coordenadas (`locationSource` `GPS` o `MARCADOR`); los más recientes primero hasta `WEB.MAPA_MAX_FEATURES` (5 000) con `truncated`; `sinUbicacion` cuenta los casos filtrados sin coordenadas; `lots` trae los contornos de lotes activos que tienen `geometry`. El cliente (`mapa.js`) arma las ventanas emergentes con nodos de texto (nunca HTML del servidor).

### 15.2 CSV de casos (`GET /reportes/casos.csv`)

Mismos filtros que la bandeja. Codificación UTF-8 con BOM (Excel lo abre con tildes), separador coma, una fila por caso, ordenado por `capturada`, hasta 50 000 filas.

| Columna | Contenido |
|---|---|
| `caso` | UUID del caso |
| `estado` | Código del estado de revisión |
| `origen` | `IA` o `MANUAL` |
| `lote`, `hilera`, `lateral`, `segmento`, `marcador` | Ubicación operativa (códigos) |
| `lat`, `lon`, `fuente_ubicacion` | Coordenadas y `GPS`/`MARCADOR`/`NINGUNA` |
| `capturada`, `abierto`, `decidido` | ISO-8601 en hora de Lima |
| `decidido_por` | Nombre del especialista |
| `cajas`, `confianza_max` | Número de cajas y confianza máxima (3 decimales, punto) |
| `modelo` | `model_version` del análisis |
| `estado_aviso` | Resumen del aviso |
| `captura` | UUID de la captura |

Toda celda que empieza con `=`, `+`, `-`, `@`, tabulador o retorno se antepone con `'` (inyección de fórmulas). La exportación queda en auditoría (`EXPORTACION_CASOS`, con los filtros).

### 15.3 Mensaje de WhatsApp

**Plantilla propuesta** (categoría *Utility*, idioma `es`, nombre `caso_confirmado`; la aprueba Meta antes del piloto, Q-W08):

```text
Riachuelo · Caso fitosanitario confirmado por el especialista.
Lote {{1}}, hilera {{2}}, {{3}} ({{4}}).
Foto tomada el {{5}}.
Ver el caso (requiere iniciar sesión): {{6}}
Mensaje automático: no responder a este número.
```

| Parámetro | Valor | Ejemplo |
|---|---|---|
| `{{1}}` | Código del lote | `SWG 1` |
| `{{2}}` | Número de hilera | `5` |
| `{{3}}` | Lateral | `Lateral A` |
| `{{4}}` | Segmento y marcador | `segmento SWG 1-S1 · marcador M1` (o «sin segmento ni marcador») |
| `{{5}}` | Fecha y hora de la foto (Lima) | `02/10/2026 21:14` |
| `{{6}}` | Enlace al caso | `https://<dominio>/casos/<id>/` |

Cuerpo que envía el worker a `POST https://graph.facebook.com/<versión>/<WHATSAPP_PHONE_ID>/messages`:

```json
{
  "messaging_product": "whatsapp",
  "to": "51987654321",
  "type": "template",
  "template": {
    "name": "caso_confirmado",
    "language": {"code": "es"},
    "components": [{"type": "body", "parameters": [
      {"type": "text", "text": "SWG 1"}, {"type": "text", "text": "5"}, {"type": "text", "text": "Lateral A"},
      {"type": "text", "text": "segmento SWG 1-S1 · marcador M1"}, {"type": "text", "text": "02/10/2026 21:14"},
      {"type": "text", "text": "https://riachuelo.ejemplo.pe/casos/…/"}
    ]}]
  }
}
```

Reglas: el número va sin `+`; los parámetros se normalizan a una línea (sin saltos ni tabulaciones, que las plantillas no admiten); nunca se envía la foto ni su URL; el texto no menciona tratamientos ni urgencia. Respuesta 2xx → `messages[0].id` se guarda como `provider_message_id`; 429 y 5xx se reintentan; otros 4xx no (sección 8.3). Fuera de la ventana de 24 horas solo se aceptan plantillas aprobadas, y Meta puede reclasificar la categoría al revisarla.

### 15.4 Datos del plano (contexto de la plantilla)

`web.queries.plano(lot, filtros)` devuelve `{lot, rows: [{row, covered, counts: {estado: n}, segments: [{segment, counts, left_pct, width_pct}]}], max_plants, covered}`. `left_pct = 100·(start_plant − 1)/plant_count` y `width_pct = 100·(end_plant − start_plant + 1)/plant_count`; la barra de la hilera mide `100·plant_count/max_plants` % del ancho.


---

## 16. Seguridad y privacidad de la web

| Riesgo | Medida | Dónde | Estado |
|---|---|---|---|
| Acceso sin sesión o con rol equivocado | `@web_view` en toda ruta + matriz 7.2 + comprobación en servicios (RN-W01) | `web/permissions.py`, servicios | Verificado (pruebas) |
| CSRF | `CsrfViewMiddleware`, `{% csrf_token %}`, `X-CSRFToken` en HTMX; sin `csrf_exempt` | Plantillas, P-3 | Verificado (`test_csrf_obligatorio`) |
| Robo de cookie | `HttpOnly`, `Secure` (piloto), `SameSite=Lax`, 8 h, termina al cerrar el navegador | `settings.py` | Propuesto |
| Fuerza bruta en el ingreso | 5 intentos / 15 min por correo en caché compartida | `LoginForm` | Verificado (`test_bloqueo_tras_intentos_fallidos`) |
| Enumeración de cuentas | Mensaje genérico; estado solo con contraseña correcta; mismo tiempo exista o no | `LoginForm` | Verificado |
| Redirección abierta (`next`) | `url_has_allowed_host_and_scheme` | `ingresar` | Verificado |
| Cuenta bloqueada con sesión abierta | `User.is_active` depende del estado: Django corta la sesión en la siguiente petición; lista negra de refresh de la app | `cuentas/models.py`, `cuentas/services.py` | Verificado |
| Clickjacking | `X_FRAME_OPTIONS = "DENY"` | `settings.py` | Propuesto |
| HTTPS | `SECURE_SSL_REDIRECT`, HSTS (`SECURE_HSTS_SECONDS`: por defecto 3600 fuera de `DEBUG`; en piloto `31536000` por variable de entorno, `.env.example`), `SECURE_PROXY_SSL_HEADER` | `settings.py` | Propuesto (`check --deploy`) |
| Fugas por caché o historial | `Cache-Control: no-store` en respuestas autenticadas | `NoStoreForAuthenticatedMiddleware` | Verificado |
| URLs firmadas que no caducan (Cloudinary Free) | Solo en páginas con sesión; nunca en WhatsApp, CSV ni logs | W-10, DW-26 | Verificado (CSV y WhatsApp sin URL de foto) |
| Secretos | Solo en variables de entorno del servidor; la página nunca contiene el API secret | W-09 | Verificado (`test_caso_con_url_firmada…`) |
| Inyección de fórmulas en CSV | Prefijo `'` | `exportar_casos` | Verificado |
| XSS | Escape automático de Django; `format_html` en etiquetas; ventanas del mapa con nodos de texto | Plantillas, `mapa.js` | Propuesto |
| Inyección SQL | ORM de Django; el único SQL crudo (`seguridad_bd.py`) no recibe datos del usuario | — | Propuesto |
| Acceso directo a Supabase | Sin `GRANT` a `anon`/`authenticated`, RLS en todas las tablas (post-`migrate`), tabla de caché creada por migración | `auditoria/seguridad_bd.py` | Verificado en PostgreSQL 16 |
| Referer para OpenStreetMap | `SECURE_REFERRER_POLICY = "strict-origin-when-cross-origin"` | `settings.py` | Verificado (cabecera en pruebas) |
| CSP | Preparada (W-17: sin JS en línea). No se activa en v1.0: Django 5.2 no la trae y los estilos en línea del plano y la condición de sondeo (P-9) necesitarían ajustes. | — | Decidir (sección 25.3) |
| Privacidad de destinatarios | Teléfonos solo para el administrador; consentimiento con fecha | WEB-15 | Propuesto |
| Datos personales en auditoría | `before`/`after` guardan estados, no contraseñas ni tokens | Servicios | Propuesto |
| HSTS en subdominios y precarga | No se activan (`check --deploy` lo advierte): el dominio final aún no está definido (Q-W12) | — | Decidir |

Supabase: además de lo anterior, rige todo §28.4 del maestro móvil (Data API desactivada si el panel lo permite, contraseña de la base solo en `DATABASE_URL`, migraciones solo con `manage.py migrate`).

---

## 17. Parámetros de la web (`settings.WEB`)

| Clave | Valor inicial | Efecto |
|---|---|---|
| `BANDEJA_POLL_SECONDS` | 15 (env `WEB_BANDEJA_POLL_SECONDS`) | Intervalo de sondeo de la bandeja (P-2). |
| `BANDEJA_PAGE_SIZE` | 25 | Casos por página. |
| `MAPA_MAX_FEATURES` | 5 000 | Tope de puntos en el GeoJSON (los más recientes). |
| `DASHBOARD_CACHE_SECONDS` | 60 | Antigüedad máxima de los KPIs del panel. |
| `LOGIN_MAX_FAILED` | 5 | Intentos fallidos antes del bloqueo. |
| `LOGIN_LOCKOUT_MINUTES` | 15 | Duración del bloqueo. |
| `OBSERVATION_MAX_LENGTH` | 2 000 | Largo máximo de la observación. |
| `EXPORT_MAX_ROWS` | 50 000 | Filas máximas del CSV. |

Otros valores fijos del código: arrendamiento de tareas de IA 5 min y 3 intentos (`ia/services.py`); arrendamiento de avisos 2 min y 5 intentos con espera 2, 4, 8 y 16 min (`notificaciones/services.py`); periodo por defecto del panel y del mapa: 30 días.

---

## 18. Rendimiento

### 18.1 Meta

RNF04 del curso: consulta web con 50 000+ plantas y respuesta menor a 1,5 s. Se traduce en RNF-W01: toda pantalla < 1,5 s en el servidor con al menos 100 000 fotos; bandeja y caso < 300 ms.

### 18.2 Medición de referencia (04/10/2026, Verificado en local)

Base de datos: catálogos del piloto (83 hileras, 19 659 plantas), 100 576 capturas, 93 872 tareas de IA y 18 798 casos (4 759 pendientes), PostgreSQL 16 en la misma máquina, Django 5.2.17, 12 peticiones por pantalla (se descartan las 2 primeras), usuario especialista, respuesta comprimida cuando aplica. Se repite con `docs/perf/datos_sinteticos.sql` y `python manage.py medir_rendimiento` (Anexo H). Con las 50 000+ plantas del fundo completo solo crecen las tablas de catálogo (cientos de hileras), que no intervienen en estas consultas salvo el plano (una fila por hilera del lote elegido).

| Pantalla | Mediana (ms) | p95 (ms) | Consultas SQL | Tamaño |
|---|---:|---:|---:|---:|
| Bandeja (pendientes, página 1) | 39 | 42 | 6 | 25 KB |
| Bandeja (todos, orden por confianza, página 50) | 60 | 70 | 6 | 25 KB |
| Caso | 46 | 50 | 11 | 8 KB |
| Mapa: datos sin filtros (tope 5 000 puntos) | 658 | 719 | 6 | 258 KB (gzip) |
| Mapa: datos de un lote, confirmados | 254 | 292 | 6 | 91 KB (gzip) |
| Plano (SWG 1) | 26 | 27 | 10 | 19 KB |
| Panel (30 días, con caché) | 5 | 6 | 4 | 5 KB |
| Panel (30 días, sin caché) | ≈ 280 | ≈ 300 | 21 | 5 KB |

### 18.3 Cómo se logra y qué vigilar en Supabase

- Consultas con número fijo de sentencias (la bandeja hace exactamente 6 sin importar cuántos casos haya: `test_bandeja_sin_consultas_n_mas_1`).
- Ubicación copiada al caso (DW-13): mapa, plano y bandeja no recorren secuencias ni capturas.
- Índices de 11.3; paginación; tope y compresión del GeoJSON; mapa con 30 días por defecto; KPIs en caché 60 s.
- En Supabase, cada consulta cruza la red: con la plataforma en la **misma región** (`sa-east-1`) se suman del orden de 1 a 3 ms por consulta; en otra región, decenas de ms por consulta (21 consultas del panel sin caché pueden pasar el segundo). Repetir la medición en el entorno piloto (CP-W20) con `DB_CONN_MAX_AGE=60`.
- Fotos: el navegador las pide directo a la CDN de Cloudinary (no pasan por Django).

---

## 19. Mensajes y textos

Los mensajes que no son etiquetas de campos están en `web/messages.py` (Anexo D.5). Reglas: español de Perú, frases cortas, sin culpar al usuario, sin códigos internos (los códigos van en la auditoría). Coherencia con la app: los textos de cuenta pendiente, rechazada y bloqueada equivalen a los de §19 del maestro móvil.

---

## 20. Auditoría e indicadores

### 20.1 Acciones de auditoría

| Acción | Entidad | Quién | Antes → después |
|---|---|---|---|
| `INGRESO_WEB` | `user` | Usuario | — |
| `CONTRASENA_CAMBIADA_WEB` | `user` | Usuario | — |
| `CUENTA_APROBADA` | `user` | Administrador | estado |
| `ROLES_CAMBIADOS` | `user` | Administrador | roles |
| `CUENTA_ESTADO` | `user` | Administrador | estado |
| `CONTRASENA_TEMPORAL` | `user` | Administrador | `mustChangePassword` |
| `CELULAR_REVOCADO` | `device` | Administrador | `revokedAt` |
| `DESTINATARIO_GUARDADO` | `notification_recipient` | Administrador | activo, teléfono, consentimiento |
| `CASO_ABIERTO` | `case` | Worker (IA) o especialista (manual) | origen, tarea |
| `CASO_NUEVO_ANALISIS` / `ANALISIS_POSTERIOR_A_DECISION` | `case` | Worker | tarea |
| `CASO_DECIDIDO` | `case` | Especialista | estado, decisión, cajas rechazadas |
| `CASO_CORREGIDO` | `case` | Especialista | estado, decisión, motivo |
| `AVISO_SIN_DESTINATARIOS` | `case` | Sistema | lote |
| `TAREA_IA_REENCOLADA` | `ai_task` | Administrador | estado, intentos, error |
| `EXPORTACION_CASOS` | `export` | Usuario | filtros |

### 20.2 Indicadores que se pueden calcular con la web (piloto y pretest–postest)

| Indicador | Cómo |
|---|---|
| Tiempo de revisión (apertura → decisión) | Panel; CSV (`abierto`, `decidido`). |
| Tiempo de extremo a extremo (foto → decisión) | CSV (`capturada`, `decidido`). |
| Concordancia IA–especialista (precisión observada) | Casos de origen IA confirmados / (confirmados + descartados); por caja: `detections.review_status`. |
| Falsos negativos estimados | Casos de origen `MANUAL` confirmados sobre la muestra de fotos sin indicios (Q-W10). |
| Rechazo por calidad | Panel (fotos rechazadas / recibidas). |
| Cobertura | Panel y plano (RN-13). |
| Hallazgos confirmados por lote y hilera | Plano, CSV. |
| Oportunidad del aviso | `notifications.sent_at` − `human_reviews.reviewed_at`. |

---

## 21. Pruebas

### 21.1 Pruebas automáticas (66, Anexo F)

Ejecutar con `python manage.py test` (PostgreSQL). En SQLite se omiten 3 (concurrencia y seguridad de Supabase).

| Grupo | Archivo | Pruebas |
|---|---|---|
| Apertura de casos (9) | `revision/tests/test_services.py` | Indicio abre caso con ubicación GPS; sin GPS usa el marcador; sin GPS ni marcador queda sin coordenadas; la repetición usa su contexto; sin cajas no hay caso; foto rechazada por calidad no se analiza; un análisis nuevo no cambia una decisión; caso manual sobre foto sin indicios (permiso e idempotencia); el caso manual exige análisis terminado. |
| Decisiones (10) | ídem | Solo el especialista decide; confirmar registra todo en una transacción (cajas, avisos según alcance y consentimiento, auditoría); descartar no avisa; observación obligatoria; cajas y clase deben pertenecer al caso; no se decide dos veces; la corrección anula avisos pendientes y conserva el historial; de descartado a confirmado avisa una sola vez; corrección con versión vieja; corrección exige motivo. |
| Concurrencia (1, PostgreSQL) | ídem | Dos hilos deciden a la vez: un ganador y un 409. |
| Avisos (6) | `notificaciones/tests/test_services.py` | Plantilla con ubicación y enlace, sin URL de foto ni saltos de línea; envío exitoso; error temporal con espera y error final al quinto intento sin cambiar la decisión; error definitivo sin reintento; un aviso arrendado no lo toma otro worker; el cliente de Cloud API clasifica 429/5xx (reintentar) y 400/401 (no). |
| Cola de IA (3) | `ia/tests/test_services.py` | Reintentos con espera y error al tercero; arrendamiento vencido vuelve a la cola; reencolar solo errores, con auditoría. |
| Seguridad de Supabase (2, PostgreSQL) | `auditoria/tests/test_seguridad_bd.py` | RLS en todas las tablas de `public`; `anon` y `authenticated` sin `SELECT`. |
| Ingreso (8) | `web/tests/test_views.py` | Mensaje genérico; estado solo con contraseña correcta; operador sin acceso; bloqueo tras 5 fallos; ingreso y auditoría; contraseña temporal obliga a cambiarla; bloquear corta la sesión; `next` externo ignorado. |
| Matriz de permisos (4) | ídem | Toda ruta tiene `@web_view`; 17 páginas × 3 roles con el código esperado (200/403); anónimo → login y `HX-Redirect` en HTMX; decidir solo especialista. |
| Bandeja y caso (10) | ídem | Pendientes por defecto; fragmento HTMX que se reemplaza a sí mismo y `Vary`; exactamente 6 consultas con 3 u 8 casos; caso con URL firmada, `viewBox`, cajas, aviso de IA, `no-store` y `Referrer-Policy`; decisión HTMX → `HX-Redirect`; «Guardar y siguiente»; 422 con errores en el fragmento; 409 si otro decidió; CSRF obligatorio; el supervisor ve el caso sin formulario. |
| Mapa, plano, panel y exportación (4) | ídem | GeoJSON (orden `[lon, lat]`, fuente, sin ubicación, filtro); plano con conteos y cobertura RN-13 (INCOMPLETE solo con incidencia); KPIs; CSV con BOM y sin inyección de fórmulas. |
| Administración (9) | ídem | Aprobar con roles; cambiar roles y atender un pedido de restablecimiento guardado solo con el correo; contraseña temporal revoca la app; la contraseña temporal cumple la política; política igual a la app; no bloquearse a sí mismo; destinatario con consentimiento y teléfono E.164; reencolar error de IA (403 para el especialista); revocar celular. |

### 21.2 Casos de prueba funcionales (manuales)

| ID | Caso | Resultado esperado | Fase |
|---|---|---|---|
| CP-W01 | Ingresar con cada rol de la web y con un operador | Panel para los tres roles; mensaje «Usa la app móvil» para el operador | FW1 |
| CP-W02 | Seis contraseñas erróneas seguidas | Bloqueo de 15 min con el mensaje | FW1 |
| CP-W03 | Aprobar una cuenta creada desde la app y entrar con ella en la app | La app entra con el rol asignado | FW1 |
| CP-W04 | Contraseña temporal a un operador con la app abierta | La app pierde la sesión al renovar (`REFRESH_INVALID`) y exige cambiarla al volver a entrar con internet | FW1 |
| CP-W05 | Bloquear a un supervisor con la web abierta | En el siguiente clic vuelve al ingreso | FW1 |
| CP-W06 | Sincronizar una sesión real desde la app | Sesión, pasadas, fotos y casos visibles; caso en la bandeja < 1 min después de la confirmación | FW3 |
| CP-W07 | Revisar 10 casos seguidos con «Guardar y siguiente» y atajos | Sin recargas manuales; tiempo medio por caso anotado (RNF-W06) | FW3 |
| CP-W08 | Dos especialistas abren el mismo caso y guardan | Uno guarda; el otro ve 409 con nombre y hora | FW3 |
| CP-W09 | Confirmar con destinatarios de dos lotes distintos | Solo recibe el del lote del caso (y los de «todos») | FW4 |
| CP-W10 | Confirmar con el worker detenido; arrancarlo después | El aviso queda pendiente y se envía al arrancar | FW4 |
| CP-W11 | Confirmar con un token de WhatsApp inválido | `ERROR_ENVIO`; la decisión no cambia; se ve en WEB-12 | FW4 |
| CP-W12 | Abrir el enlace del WhatsApp en un celular sin sesión | Pide ingresar y vuelve al caso; se lee sin desplazamiento horizontal | FW4 |
| CP-W13 | Corregir un caso confirmado con aviso pendiente | El aviso pasa a «No aplica»; historial con dos decisiones | FW3 |
| CP-W14 | Foto vertical (celular girado) con cajas | Las cajas coinciden en `revision` y en el original | FW2 |
| CP-W15 | Mapa con casos sin GPS | Aparecen con borde punteado (marcador) o en «sin coordenadas»; todos visibles en el plano | FW4 |
| CP-W16 | Plano con un lateral INCOMPLETE sin incidencia | La hilera no figura como cubierta | FW4 |
| CP-W17 | Exportar CSV y abrirlo en Excel | Tildes correctas; columnas de 15.2; fórmulas escapadas | FW5 |
| CP-W18 | Reencolar un error de análisis | Vuelve a «Pendiente de análisis» y el worker lo procesa | FW4 |
| CP-W19 | Revisar la web con teclado solamente | Se puede filtrar, abrir un caso, decidir y guardar | FW5 |
| CP-W20 | Repetir la medición de la sección 18 en el entorno piloto | Toda pantalla < 1,5 s; registrar la tabla | FW6 |
| CP-W21 | Imprimir el panel y un caso a PDF desde el navegador | Sin menú ni filtros; legible | FW5 |
| CP-W22 | `python manage.py check --deploy` con la configuración del piloto | Solo los avisos de HSTS aceptados (sección 16) | FW6 |

### 21.3 Navegadores y dispositivos

Chrome y Edge (Windows) en la computadora del especialista; Chrome en Android y Safari en iPhone para el enlace del WhatsApp; anchos de 1440, 1024 y 390 px como mínimo.

### 21.4 Evidencia

Por cada fase: salida de `python manage.py test`, capturas de pantalla, tabla de rendimiento (en FW6) y el reporte de la sección 23.2.

---

## 22. Plan de construcción por tareas

Fases alineadas con los sprints 4 a 6 del Curso Integrador II y con el piloto (postest del 9 al 18/11/2026). Cada tarea parte del código de referencia del anexo indicado; «hecha» = criterio cumplido + pruebas en verde + reporte.

### Fase FW0 — Preparación

- **T-W00 Repositorio y proyecto.** Crear o adaptar `riachuelo-platform` con la estructura 6.2, `requirements.txt` (5.2), `settings.py` (Anexo B.1), `.env.example` y `README`. Hecha: `check` y `migrate` en PostgreSQL local; `docs/MAESTRO_WEB.md` y `docs/MAESTRO_APP_MOVIL.md` en el repositorio; `AGENTS.md` con la instrucción de 0.2. Secciones: 5, 6.2.
- **T-W01 Supabase de desarrollo.** Proyecto en `sa-east-1`, `DATABASE_URL` por conexión directa o *session pooler*, `migrate` y comprobación de RLS y privilegios (consulta del Anexo B.9). Hecha: `test_seguridad_bd` pasa contra Supabase de desarrollo. Secciones: 16; §28.4 del maestro móvil.

### Fase FW1 — Modelo de datos, cuentas y administración (Sprint 4)

- **T-W02 Modelos y migraciones.** Todas las apps del Anexo B; `makemigrations`; Django Admin (Anexo G.1); `sembrar_demo` (Anexo G.2). Hecha: `makemigrations --check` limpio; Admin muestra catálogos editables y evidencia en solo lectura. Depende de T-W00.
- **T-W03 Cloudinary para la web.** Crear las transformaciones con nombre `miniatura` y `revision`, activar transformaciones estrictas, probar `signed_image_url` con una foto real subida con el ticket de la app (o a mano con el mismo `public_id`). Hecha: la bandeja y el caso muestran fotos reales. Sección 14.
- **T-W04 Base visual y permisos.** `base.html`, `app.css`, `web_tags`, `permissions.py`, `middleware.py`, `context_processors.py`, `errors.py`. Hecha: `test_todas_las_rutas_estan_protegidas`. Secciones 6.4, 6.5, 7.2.
- **T-W05 Ingreso y contraseña.** `LoginForm`, vistas de ingreso, salida y cambio; validador de contraseña. Hecha: pruebas del grupo «Ingreso». Sección 7.3.
- **T-W06 Usuarios y celulares.** `cuentas/services.py`, WEB-13, WEB-14. Hecha: pruebas de «Administración» de cuentas; CP-W03 a CP-W05.

### Fase FW2 — Bandeja y visor (Sprint 4)

- **T-W07 Bandeja.** `queries.bandeja`, `FiltroCasosForm`, WEB-04 con P-1, P-2 y P-7. Hecha: pruebas de bandeja y número de consultas.
- **T-W08 Visor del caso.** `queries.case_detail`, `caso.html`, `caso.js` (acercar, mover, cajas, original), ficha de ubicación, otra cámara. Hecha: `test_caso_con_url_firmada…`; CP-W14 con una foto real.

### Fase FW3 — Decisión (Sprint 4)

- **T-W09 Servicios de revisión.** `revision/services.py` completo y `ia.services.save_analysis_result` abriendo casos. Hecha: grupos «Apertura de casos», «Decisiones» y «Concurrencia».
- **T-W10 Decisión y corrección en la web.** `DecisionForm`, `CorrectionForm`, `panel_decision.html`, P-4 y P-6, «Guardar y siguiente», atajos. Hecha: pruebas de decisión HTMX; CP-W07, CP-W08, CP-W13.
- **T-W11 Integración con el worker real.** Conectar `worker_ia` con el detector (`ia/inference/detector.py`, §28.8 del maestro móvil): `save_analysis_result` con cajas en píxeles tras `exif_transpose` y tamaño analizado. Hecha: CP-W06 con fotos reales.

### Fase FW4 — Mapa, plano, sesiones y avisos (Sprints 4 y 5)

- **T-W12 Mapa.** `queries.map_geojson`, `mapa.html`, `mapa.js`, contornos de lote (cargar el GeoJSON real de los lotes del piloto en Admin, Q-W05). Hecha: `test_geojson`; CP-W15.
- **T-W13 Plano de hileras.** `queries.plano`, `queries.covered_rows`, `plano.html`. Hecha: `test_plano_cuenta_casos_y_cobertura`; CP-W16.
- **T-W14 Sesiones y fotos.** WEB-09, WEB-10, WEB-06 con apertura de caso manual. Hecha: páginas por rol; RF-W15.
- **T-W15 Avisos.** `notificaciones/services.py`, `whatsapp.py`, WEB-12, WEB-15; plantilla aprobada en Meta (Q-W08); `CloudApiClient` en el worker. Hecha: grupo «Avisos»; CP-W09 a CP-W12.
- **T-W16 Estado de la IA.** WEB-16 y reencolar. Hecha: `test_reencolar_error_de_ia`; CP-W18.

### Fase FW5 — Panel, reportes y auditoría (Sprint 5)

- **T-W17 Panel.** `queries.dashboard` con caché; WEB-03. Hecha: `test_panel`.
- **T-W18 Reportes y auditoría.** CSV (15.2), WEB-11, WEB-17, hoja de impresión. Hecha: `test_csv…`; CP-W17, CP-W21.
- **T-W19 Accesibilidad y responsive.** Revisión con teclado, celular y tablet; ajustes. Hecha: CP-W12, CP-W19; capturas actualizadas en `docs/img/web/`.

### Fase FW6 — Despliegue y piloto (Sprints 5 y 6)

- **T-W20 Despliegue piloto.** Contenedores `web` (gunicorn) y `worker` en la región de Supabase, HTTPS, variables del piloto, `collectstatic`, `migrate`, superusuario, transformaciones de Cloudinary del entorno piloto, plantilla de WhatsApp aprobada, destinatarios con consentimiento. Hecha: CP-W20 y CP-W22; prueba de extremo a extremo con la app (CP-W06 a CP-W12) antes del 9/11/2026.

---

## 23. Plantillas

### 23.1 Pedido a la IA

```text
Tarea: T-W__ — <nombre>
Lee docs/MAESTRO_WEB.md: sección 1 (reglas), secciones 2 a 7, la tarea en la sección 22 y lo que cita: <secciones/anexos>.
Si toca datos de la app, cola de IA o Cloudinary: docs/MAESTRO_APP_MOVIL.md §15 y §28.
Estado actual del repositorio: <sección 24 / último commit>.
Entrega: código, migraciones si aplica, pruebas, y el reporte de la sección 23.2.
No inventes campos, estados, permisos, rutas ni mensajes (W-04); si algo falta, pregúntalo.
```

### 23.2 Reporte de avance (lo entrega la IA)

```text
Tarea: T-W__ · Fecha:
Qué hice:
Archivos creados/modificados:
Migraciones:
Pruebas: comando y resultado (check, makemigrations --check, test)
Capturas o evidencia:
Supuestos (W-04):
Preguntas abiertas:
Riesgos o deuda:
```

### 23.3 Registro de decisión (ADR) — `docs/adr/ADR-W-NNN-titulo.md`

```text
# ADR-W-NNN — Título
Fecha · Estado (propuesta/aceptada/reemplazada) · Autor
Contexto:
Decisión:
Alternativas consideradas:
Consecuencias:
Secciones del Maestro Web afectadas (y versión nueva):
```

---

## 24. Registro de avance

| Tarea | Fase | Estado | Fecha | Responsable | Evidencia o commit | Notas |
|---|---|---|---|---|---|---|
| T-W00 | FW0 | Pendiente | | | | El paquete de referencia cubre la estructura. |
| T-W01 | FW0 | Pendiente | | | | |
| T-W02 | FW1 | Pendiente (código de referencia listo) | | | Anexo B | |
| T-W03 | FW1 | Pendiente | | | | Requiere la cuenta de Cloudinary (Q-16 del maestro móvil). |
| T-W04 a T-W10 | FW1–FW3 | Pendiente (código de referencia listo) | | | Anexos C a F | |
| T-W11 | FW3 | Pendiente | | | | Depende del detector entrenado. |
| T-W12 a T-W19 | FW4–FW5 | Pendiente (código de referencia listo, salvo plantilla de WhatsApp y contornos reales) | | | | |
| T-W20 | FW6 | Pendiente | | | | Depende de Q-W12 (hosting y dominio). |

Estados: Pendiente · En curso · Hecho · Bloqueado (indicar por qué). «Código de referencia listo» significa que el anexo trae código probado; la tarea queda hecha cuando ese código está en el repositorio, integrado y con evidencia propia.

---

## 25. Decisiones registradas y preguntas abiertas

### 25.1 Versiones de este archivo

| Versión | Fecha | Cambio |
|---|---|---|
| 1.0 | 04/10/2026 | Primera versión: web en Django + HTMX + Leaflet sobre la arquitectura v3.0; modelo de datos completo; casos, decisiones con corrección, avisos con destinatarios; mapa y plano; 66 pruebas; rendimiento medido; código de referencia y capturas. |

### 25.2 Decisiones técnicas (ADR) de la web

| ADR | Decisión | Estado |
|---|---|---|
| ADR-W-001 | Web generada en el servidor con plantillas + HTMX 2, sin SPA (DW-01). | Aceptada (ADR-008 del maestro móvil) |
| ADR-W-002 | Caso = foto con su análisis (`review_cases`), decisión única con corrección auditada (DW-06, DW-08). | Propuesta |
| ADR-W-003 | Avisos por cola en `notifications` con destinatarios por lote y consentimiento (DW-09, DW-10). | Propuesta |
| ADR-W-004 | Visor con SVG en coordenadas de la imagen analizada (DW-12). | Propuesta |
| ADR-W-005 | Mapa Leaflet + OSM y plano esquemático (DW-14, DW-15). | Propuesta |
| ADR-W-006 | Solo se analizan fotos aceptadas por calidad (DW-11). | Propuesta (Q-W17; actualizar el maestro móvil) |
| ADR-W-007 | Roles en `user_roles` (DW-04). | Propuesta (actualizar el maestro móvil) |
| ADR-W-008 | Acciones sobre cuentas y celulares solo en la web; Django Admin para superusuarios (DW-19). | Propuesta (actualizar §28.6 del maestro móvil) |

### 25.3 Preguntas abiertas

| ID | Pregunta | Quién decide | Antes de |
|---|---|---|---|
| Q-W01 | ¿El operador de campo necesita ver algo en la web (por ejemplo, sus sesiones)? Propuesto: no. | Equipo | FW1 |
| Q-W02 | ¿El jefe de fundo tendrá cuenta con rol SUPERVISOR? ¿Cuántos supervisores de zona hay y qué lotes cubre cada uno (alcance de avisos)? | Equipo + empresa | FW4 |
| Q-W03 | Clases que el especialista puede confirmar (¿solo chanchito blanco? ¿otras plagas?) = clases del modelo (`model_configs.classes`). | Equipo de IA + especialista | FW3 |
| Q-W04 | Paleta de estados (confirmado en rojo, DW-22) frente al verde del Integrador §9.7: validar con el especialista. | Equipo + especialista | FW2 |
| Q-W05 | Contornos reales de los lotes del piloto (GeoJSON: ¿quién los levanta y cómo? GPS del perímetro o dibujo sobre imagen) y si se quiere capa satelital (proveedor y términos de uso). | Equipo | FW4 |
| Q-W06 | ¿Basta imprimir desde el navegador o se necesita un PDF con formato oficial generado en el servidor? | Equipo + empresa | FW5 |
| Q-W07 | Si un caso confirmado y ya avisado se corrige, ¿se envía un WhatsApp de corrección (otra plantilla)? Propuesto: no en v1.0. | Equipo + empresa | FW4 |
| Q-W08 | Número de WhatsApp Business, plantilla `caso_confirmado` aprobada por Meta, costo por mensaje y consentimiento escrito de los destinatarios. | PM + empresa | FW4 |
| Q-W09 | ¿Se asignan casos a un especialista (bloqueo suave) si revisan varios a la vez? Propuesto: no; basta el control 409. | Equipo | FW3 |
| Q-W10 | Tamaño de muestra y procedimiento para revisar fotos sin indicios (falsos negativos) en el piloto. | Equipo + especialista | FW6 |
| Q-W11 | Nombre del producto web (los mockups del Integrador usan «VitisWatch») y logotipo. | Equipo | FW2 |
| Q-W12 | Hosting y dominio de la plataforma (Q-22 del maestro móvil): `PUBLIC_BASE_URL`, `ALLOWED_HOSTS`, HSTS. | Backend + PM | FW6 |
| Q-W13 | Retención de fotos, decisiones y auditoría (Q-05 del maestro móvil). | Equipo + empresa | FW6 |
| Q-W14 | ¿Se sincronizarán los ciclos y las áreas no hechas de la app (Q-20 del maestro móvil) para mostrar cobertura por ciclo en el panel? | Equipo móvil + backend | FW5 |
| Q-W15 | ¿La bandeja debe ocultar indicios por debajo de cierta confianza? Propuesto: no ocultar; ordenar por confianza si se quiere. | Especialista | FW2 |
| Q-W16 | ¿Se activa una CSP en el piloto (django-csp o cabecera propia)? Requiere quitar estilos en línea del plano y la condición de sondeo (P-9). | Backend | FW6 |
| Q-W17 | Aprobar DW-11 (no analizar fotos rechazadas por calidad) y llevarlo al maestro móvil v2.1 (§15.2, §15.8, §28.2, §28.8, §28.9). Mientras no se apruebe, el código de referencia ya lo aplica; si se rechaza, basta quitar la condición de `ia.services.enqueue_analysis`. | Equipo + backend | FW1 |

---

## 26. Glosario

| Término | Definición |
|---|---|
| Bandeja | Lista de casos para revisar (WEB-04). |
| Caja | Rectángulo que la IA dibuja donde sugiere un indicio (`detections`), en píxeles de la imagen analizada. |
| Caso | Unidad de revisión: una foto con su análisis (`review_cases`). Origen IA (lo abre el worker) o MANUAL (lo abre el especialista). |
| Corrección | Nueva decisión que reemplaza a la vigente, con motivo; la anterior queda en el historial. |
| Consentimiento (opt-in) | Fecha en que un destinatario aceptó recibir avisos por WhatsApp. |
| Cobertura | Hileras con LATERAL_A y LATERAL_B cerrados en el periodo (RN-13). |
| Decisión vigente | La última decisión no reemplazada de un caso (`human_reviews.is_current`). |
| Destinatario | Persona que recibe avisos de casos confirmados de los lotes de su alcance. |
| Evidencia insuficiente | Decisión: la foto no permite decidir; hay que volver a mirar en campo. |
| Fragmento | Parte de una página que HTMX reemplaza sin recargar. |
| HTMX | Librería que agrega peticiones parciales a HTML con atributos `hx-*`. |
| HX-Redirect | Cabecera con la que el servidor pide a HTMX recargar otra página completa. |
| Indicio sugerido por IA | Resultado del modelo; no es diagnóstico ni decisión (RN-W07). |
| Leaflet | Librería de mapas del navegador. |
| OpenStreetMap (OSM) | Mapa libre cuyas teselas usa el mapa de la web. |
| Plano de hileras | Esquema del lote con una barra por hilera y los segmentos del piloto (WEB-08). |
| Plantilla de WhatsApp | Mensaje con parámetros aprobado por Meta; obligatorio para avisos iniciados por la empresa. |
| Sondeo | Petición periódica de HTMX para actualizar la bandeja. |
| Teselas | Imágenes cuadradas que forman el mapa base. |
| URL firmada | Dirección de Cloudinary con firma que permite ver una foto privada (`authenticated`). |
| `viewBox` | Sistema de coordenadas de un SVG; aquí, el tamaño en píxeles de la imagen analizada. |
| Ubicación aproximada | Coordenadas tomadas del marcador porque la secuencia no tenía GPS. |
| Worker | Proceso `python manage.py worker_ia`: analiza fotos y envía avisos. |

---

## 27. Fuentes y trazabilidad

**Documentos del proyecto:** Maestro App Móvil v2.0 (03/10/2026); ADR «Arquitectura v3.0 — Django + Supabase + Cloudinary» y documento del equipo (03/10/2026); Curso Integrador II (§7.3–7.17, §8.1–8.5, §9.3–9.8); archivo «MAPEO CHANCHITO MOLINOS actualizado» (hojas por lote y RESUMEN); Especificación técnico-funcional v2.0 y Stack Tecnológico v2.1 (a través del maestro móvil).

**Paquetes y documentación consultados el 03–04/10/2026:**

1. PyPI — Django (5.2.17, 6.1.1), djangorestframework 3.18.1, djangorestframework-simplejwt 5.5.1 (clasificadores hasta Django 5.2), drf-spectacular 0.30.0, django-htmx 1.29.0 (Django 5.2–6.1), whitenoise 6.12.0, cloudinary 1.46.2, psycopg 3.3.6, gunicorn 26.2.0, django-environ 0.14.0, requests 2.34.2: https://pypi.org/
2. Registro npm — htmx.org 2.0.11 (`next`: 4.0.0), leaflet 1.9.4 (`alpha`: 2.0.0-alpha.1): https://registry.npmjs.org/
3. Política de uso de teselas de OpenStreetMap (atribución, Referer/User-Agent, caché, sin uso sin conexión): https://operations.osmfoundation.org/policies/tiles/
4. WhatsApp Business: plantillas obligatorias fuera de la ventana de 24 h y cambios de precio del 01/10/2026 (fuente secundaria; verificar en la página de precios de Meta): https://mixdesk.com/blog/en/whatsapp-business-api-pricing-october-2026/
5. Comparación de precios de la API de WhatsApp 2026 (contexto): https://www.wati.io/en/blog/whatsapp-api-pricing-guide/
6. Documentación de Django 5.2 (`select_for_update(of=…)`, `{% querystring %}`, `SECURE_REFERRER_POLICY`, `DatabaseCache`, validadores de contraseña): https://docs.djangoproject.com/en/5.2/
7. Documentación de HTMX 2 (`hx-trigger` con sondeo y condiciones, `responseHandling`, `HX-Redirect`): https://htmx.org/docs/
8. Documentación de Leaflet 1.9: https://leafletjs.com/reference.html
9. Cloudinary (transformaciones con nombre, entrega `authenticated`, URLs firmadas): fuentes 21 a 25 del maestro móvil.

**Implementación de referencia:** Anexos B a H; paquete `riachuelo-platform_web_referencia.zip` (04/10/2026).

---

## Anexo A. Pantallas de la implementación de referencia

Capturas tomadas el 04/10/2026 con Chromium sobre los datos de `sembrar_demo`. Las fotos son una imagen de ejemplo y las teselas del mapa están simuladas (en el entorno de captura no había acceso a Cloudinary ni a OpenStreetMap); el resto es la web real del Anexo E. Los archivos están en `docs/img/web/`.

### A.1 Panel (WEB-03), especialista, 1440 px

![A.1 Panel (WEB-03), especialista, 1440 px](img/web/web_01_panel.png)

### A.2 Bandeja de casos (WEB-04)

![A.2 Bandeja de casos (WEB-04)](img/web/web_02_bandeja.png)

### A.3 Caso pendiente con cajas y panel de decisión (WEB-05)

![A.3 Caso pendiente con cajas y panel de decisión (WEB-05)](img/web/web_03_caso_pendiente.png)

### A.4 Respuesta 422 en el panel de decisión (observación obligatoria, patrón P-4)

![A.4 Respuesta 422 en el panel de decisión (observación obligatoria, patrón P-4)](img/web/web_04_caso_error_422.png)

### A.5 Caso confirmado: decisión vigente, aviso e historial

![A.5 Caso confirmado: decisión vigente, aviso e historial](img/web/web_05_caso_confirmado.png)

### A.6 Mapa (WEB-07) con contornos de lote y casos (teselas simuladas)

![A.6 Mapa (WEB-07) con contornos de lote y casos (teselas simuladas)](img/web/web_06_mapa.png)

### A.7 Plano de hileras del lote SWG 1 (WEB-08)

![A.7 Plano de hileras del lote SWG 1 (WEB-08)](img/web/web_07_plano.png)

### A.8 Usuarios (WEB-13), administrador

![A.8 Usuarios (WEB-13), administrador](img/web/web_08_usuarios.png)

### A.9 Bandeja a 1024 px (tablet): menú solo con iconos

![A.9 Bandeja a 1024 px (tablet): menú solo con iconos](img/web/web_09_bandeja_tablet.png)

### A.10 Caso abierto desde el WhatsApp en un celular (390 px): primero ubicación y decisión

![A.10 Caso abierto desde el WhatsApp en un celular (390 px): primero ubicación y decisión](img/web/web_10_caso_celular.png)

---

## Anexo B. Código de referencia probado: configuración y modelos

Código copiado tal cual del proyecto que pasa las 66 pruebas (sección 21.1). Las migraciones se generan con `python manage.py makemigrations` (no se transcriben). Comprobación manual de la seguridad en Supabase (T-W01):

```sql
-- Debe devolver 0 filas (todas las tablas con RLS)
SELECT c.relname FROM pg_class c JOIN pg_namespace n ON n.oid = c.relnamespace
WHERE n.nspname = 'public' AND c.relkind = 'r' AND NOT c.relrowsecurity;
-- Debe devolver false, false
SELECT has_table_privilege('anon', 'public.review_cases', 'SELECT'), has_table_privilege('authenticated', 'public.users', 'SELECT');
```

### B.1 Configuración — `config/settings.py`

`config/settings.py`

```python
# config/settings.py — Configuración de la plataforma Django (extracto completo para la web).
# Todo lo sensible sale de variables de entorno (sección 5.5). Nunca subir .env a Git.
import sys
from datetime import timedelta
from pathlib import Path

import environ

BASE_DIR = Path(__file__).resolve().parent.parent
env = environ.Env()
environ.Env.read_env(BASE_DIR / ".env", overwrite=False)

SECRET_KEY = env("DJANGO_SECRET_KEY")
DEBUG = env.bool("DJANGO_DEBUG", default=False)
ALLOWED_HOSTS = env.list("ALLOWED_HOSTS", default=["localhost", "127.0.0.1"])
CSRF_TRUSTED_ORIGINS = env.list("CSRF_TRUSTED_ORIGINS", default=[])
APP_ENV = env("APP_ENV", default="dev")  # dev | piloto
PUBLIC_BASE_URL = env("PUBLIC_BASE_URL", default="http://localhost:8000")  # enlaces en WhatsApp

INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "django_htmx",
    "rest_framework",
    "rest_framework_simplejwt.token_blacklist",
    "cuentas",
    "campo",
    "monitoreo",
    "evidencias",
    "ia",
    "revision",
    "notificaciones",
    "auditoria",
    "web",
]

MIDDLEWARE = [
    "django.middleware.security.SecurityMiddleware",
    "whitenoise.middleware.WhiteNoiseMiddleware",
    "django.contrib.sessions.middleware.SessionMiddleware",
    "django.middleware.common.CommonMiddleware",
    "django.middleware.csrf.CsrfViewMiddleware",
    "django.contrib.auth.middleware.AuthenticationMiddleware",
    "django.contrib.messages.middleware.MessageMiddleware",
    "django.middleware.clickjacking.XFrameOptionsMiddleware",
    "django_htmx.middleware.HtmxMiddleware",
    "web.middleware.NoStoreForAuthenticatedMiddleware",
]

ROOT_URLCONF = "config.urls"
WSGI_APPLICATION = "config.wsgi.application"

TEMPLATES = [
    {
        "BACKEND": "django.template.backends.django.DjangoTemplates",
        "DIRS": [],
        "APP_DIRS": True,
        "OPTIONS": {
            "context_processors": [
                "django.template.context_processors.request",
                "django.contrib.auth.context_processors.auth",
                "django.contrib.messages.context_processors.messages",
                "web.context_processors.web_context",
            ],
        },
    },
]

# Base de datos: Supabase (sección 5.5). Conexión directa o session pooler (puerto 5432); nunca el transaction pooler.
DATABASES = {"default": env.db("DATABASE_URL")}
if DATABASES["default"]["ENGINE"].endswith("postgresql"):
    DATABASES["default"].setdefault("OPTIONS", {})
    DATABASES["default"]["OPTIONS"].setdefault("sslmode", env("DB_SSLMODE", default="require"))
DATABASES["default"]["CONN_MAX_AGE"] = env.int("DB_CONN_MAX_AGE", default=60)
DATABASES["default"]["CONN_HEALTH_CHECKS"] = True
DEFAULT_AUTO_FIELD = "django.db.models.BigAutoField"

# Caché compartida entre procesos gunicorn (bloqueo de login, KPIs del dashboard). `python manage.py createcachetable`.
CACHES = {"default": {"BACKEND": "django.core.cache.backends.db.DatabaseCache", "LOCATION": "web_cache"}}

AUTH_USER_MODEL = "cuentas.User"
AUTHENTICATION_BACKENDS = ["django.contrib.auth.backends.ModelBackend"]
AUTH_PASSWORD_VALIDATORS = [
    {"NAME": "django.contrib.auth.password_validation.UserAttributeSimilarityValidator"},
    {"NAME": "django.contrib.auth.password_validation.MinimumLengthValidator", "OPTIONS": {"min_length": 8}},
    {"NAME": "django.contrib.auth.password_validation.CommonPasswordValidator"},
    {"NAME": "django.contrib.auth.password_validation.NumericPasswordValidator"},
    {"NAME": "cuentas.validators.LettersAndDigitsValidator"},  # misma política que la app (PASSWORD_POLICY)
]
if "test" in sys.argv:  # solo pruebas: hash rápido (PBKDF2 real en dev y piloto)
    PASSWORD_HASHERS = ["django.contrib.auth.hashers.MD5PasswordHasher"]
LOGIN_URL = "web:login"
LOGIN_REDIRECT_URL = "web:dashboard"
LOGOUT_REDIRECT_URL = "web:login"

# Sesión web (cookie) — sección 7.3
SESSION_COOKIE_AGE = env.int("SESSION_COOKIE_AGE", default=8 * 60 * 60)  # una jornada
SESSION_EXPIRE_AT_BROWSER_CLOSE = True
SESSION_COOKIE_HTTPONLY = True
SESSION_COOKIE_SAMESITE = "Lax"
CSRF_COOKIE_HTTPONLY = False  # HTMX lee el token del <meta> o de hx-headers; no hace falta leer la cookie
SESSION_COOKIE_SECURE = not DEBUG
CSRF_COOKIE_SECURE = not DEBUG
SECURE_SSL_REDIRECT = env.bool("SECURE_SSL_REDIRECT", default=not DEBUG)
SECURE_PROXY_SSL_HEADER = ("HTTP_X_FORWARDED_PROTO", "https")  # detrás del proxy HTTPS del hosting
SECURE_HSTS_SECONDS = env.int("SECURE_HSTS_SECONDS", default=0 if DEBUG else 3600)
SECURE_CONTENT_TYPE_NOSNIFF = True
X_FRAME_OPTIONS = "DENY"
# OpenStreetMap exige un Referer válido en las teselas: 'same-origin' (valor por defecto de Django) no lo envía.
SECURE_REFERRER_POLICY = "strict-origin-when-cross-origin"

LANGUAGE_CODE = "es-pe"
TIME_ZONE = "America/Lima"  # se muestra en hora de Lima; se guarda en UTC
USE_I18N = True
USE_TZ = True

STATIC_URL = "static/"
STATIC_ROOT = BASE_DIR / "staticfiles"
STORAGES = {
    "default": {"BACKEND": "django.core.files.storage.FileSystemStorage"},
    "staticfiles": {"BACKEND": env(
        "STATICFILES_BACKEND", default="whitenoise.storage.CompressedManifestStaticFilesStorage")},
}

# Cloudinary: el SDK lee CLOUDINARY_URL ([CLOUDINARY_URL omitida]) del entorno.
CLOUDINARY_ENV_PREFIX = env("CLOUDINARY_ENV_PREFIX", default=APP_ENV)

# WhatsApp Cloud API (lo usa solo el worker; la web nunca llama a WhatsApp)
WHATSAPP_TOKEN = env("WHATSAPP_TOKEN", default="")
WHATSAPP_PHONE_ID = env("WHATSAPP_PHONE_ID", default="")
WHATSAPP_GRAPH_VERSION = env("WHATSAPP_GRAPH_VERSION", default="")  # versión vigente indicada por Meta
WHATSAPP_TEMPLATE = env("WHATSAPP_TEMPLATE", default="caso_confirmado")
WHATSAPP_TEMPLATE_LANG = env("WHATSAPP_TEMPLATE_LANG", default="es")
WHATSAPP_CLIENT = env("WHATSAPP_CLIENT", default="notificaciones.whatsapp.ConsoleClient")

# Parámetros de la web (sección 17 de este archivo)
WEB = {
    "BANDEJA_POLL_SECONDS": env.int("WEB_BANDEJA_POLL_SECONDS", default=15),
    "BANDEJA_PAGE_SIZE": 25,
    "MAPA_MAX_FEATURES": 5000,
    "DASHBOARD_CACHE_SECONDS": 60,
    "LOGIN_MAX_FAILED": 5,
    "LOGIN_LOCKOUT_MINUTES": 15,
    "OBSERVATION_MAX_LENGTH": 2000,
    "EXPORT_MAX_ROWS": 50000,
}

REST_FRAMEWORK = {
    "DEFAULT_PERMISSION_CLASSES": ["rest_framework.permissions.IsAuthenticated"],
}
SIMPLE_JWT = {
    "ACCESS_TOKEN_LIFETIME": timedelta(minutes=15),
    "REFRESH_TOKEN_LIFETIME": timedelta(days=14),
    "ROTATE_REFRESH_TOKENS": True,
    "BLACKLIST_AFTER_ROTATION": True,
    "UPDATE_LAST_LOGIN": True,
    "USER_ID_FIELD": "id",
    "USER_ID_CLAIM": "user_id",
}

LOGGING = {
    "version": 1,
    "disable_existing_loggers": False,
    "handlers": {"console": {"class": "logging.StreamHandler"}},
    "root": {"handlers": ["console"], "level": env("LOG_LEVEL", default="INFO")},
}
```

### B.2 Rutas raíz — `config/urls.py` y `config/wsgi.py`

`config/urls.py`

```python
from django.contrib import admin
from django.urls import include, path

admin.site.site_header = "Riachuelo · Gestión"
admin.site.site_title = "Riachuelo · Gestión"

urlpatterns = [
    path("gestion/", admin.site.urls),  # Django Admin (solo ADMINISTRADOR con is_staff)
    # path("api/v1/", include("api.v1.urls")),  # API de la app móvil (maestro móvil, sección 15)
    path("", include("web.urls")),
]

handler403 = "web.errors.forbidden"
```

`config/wsgi.py`

```python
import os

from django.core.wsgi import get_wsgi_application

os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings")
application = get_wsgi_application()
```

### B.3 Usuarios, roles y celulares — `cuentas/models.py`

`cuentas/models.py`

```python
# cuentas/models.py — Usuarios, roles y dispositivos (tablas users, user_roles, devices, password_reset_requests).
import uuid

from django.contrib.auth.base_user import AbstractBaseUser, BaseUserManager
from django.contrib.auth.models import PermissionsMixin
from django.db import models
from django.utils import timezone


class Role(models.TextChoices):
    ADMINISTRADOR = "ADMINISTRADOR", "Administrador"
    OPERADOR_CAMPO = "OPERADOR_CAMPO", "Operador de campo"
    ESPECIALISTA_FITOSANITARIO = "ESPECIALISTA_FITOSANITARIO", "Especialista fitosanitario"
    SUPERVISOR = "SUPERVISOR", "Supervisor"


# Roles que pueden entrar a la web (DW-05). OPERADOR_CAMPO usa solo la app móvil.
WEB_ROLES = frozenset({Role.ADMINISTRADOR, Role.ESPECIALISTA_FITOSANITARIO, Role.SUPERVISOR})


class AccountStatus(models.TextChoices):
    PENDIENTE_APROBACION = "PENDIENTE_APROBACION", "Pendiente de aprobación"
    ACTIVO = "ACTIVO", "Activo"
    RECHAZADO = "RECHAZADO", "Rechazado"
    BLOQUEADO = "BLOQUEADO", "Bloqueado"


class UserManager(BaseUserManager):
    use_in_migrations = True

    def create_user(self, email, password=None, roles=(), **extra):
        if not email:
            raise ValueError("El correo es obligatorio")
        user = self.model(email=self.normalize_email(email).lower(), **extra)
        user.set_password(password)
        user.save(using=self._db)
        for role in roles:
            UserRole.objects.create(user=user, role=role)
        return user

    def create_superuser(self, email, password, **extra):
        extra.setdefault("full_name", "Administrador")
        extra.update(is_staff=True, is_superuser=True, status=AccountStatus.ACTIVO)
        return self.create_user(email, password, roles=[Role.ADMINISTRADOR], **extra)


class User(AbstractBaseUser, PermissionsMixin):
    id = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)
    full_name = models.CharField("nombre completo", max_length=150)
    email = models.EmailField("correo", unique=True)
    phone = models.CharField("celular", max_length=20, blank=True)
    employee_code = models.CharField("código de trabajador", max_length=30, blank=True)
    status = models.CharField(max_length=24, choices=AccountStatus.choices,
                              default=AccountStatus.PENDIENTE_APROBACION, db_index=True)
    must_change_password = models.BooleanField(default=False)
    is_staff = models.BooleanField("acceso a Django Admin", default=False)
    accepted_privacy_notice_at = models.DateTimeField(null=True, blank=True)
    created_at = models.DateTimeField(default=timezone.now)
    approved_by = models.ForeignKey("self", null=True, blank=True, on_delete=models.SET_NULL, related_name="+")
    approved_at = models.DateTimeField(null=True, blank=True)

    USERNAME_FIELD = "email"
    EMAIL_FIELD = "email"
    REQUIRED_FIELDS = ["full_name"]

    objects = UserManager()

    class Meta:
        db_table = "users"
        verbose_name = "usuario"

    def __str__(self):
        return f"{self.full_name} <{self.email}>"

    # Django invalida la sesión web en la siguiente petición si is_active es False (ModelBackend.get_user):
    # bloquear o rechazar una cuenta la saca de la web sin esperar a que caduque la cookie.
    @property
    def is_active(self):
        return self.status == AccountStatus.ACTIVO

    @property
    def roles(self) -> frozenset:
        cached = getattr(self, "_roles_cache", None)
        if cached is None:
            prefetched = getattr(self, "_prefetched_objects_cache", {}).get("user_roles")
            if prefetched is not None:  # listas con prefetch_related("user_roles"): sin consulta extra
                cached = frozenset(r.role for r in prefetched)
            else:
                cached = frozenset(UserRole.objects.filter(user_id=self.pk).values_list("role", flat=True))
            self._roles_cache = cached
        return cached

    @property
    def role_labels(self) -> list:
        return [Role(r).label for r in sorted(self.roles)]

    def has_role(self, *roles) -> bool:
        return bool(self.roles.intersection(roles))

    @property
    def can_use_web(self) -> bool:
        return self.is_active and bool(self.roles & WEB_ROLES)


class UserRole(models.Model):
    user = models.ForeignKey(User, on_delete=models.CASCADE, related_name="user_roles")
    role = models.CharField(max_length=32, choices=Role.choices)
    assigned_at = models.DateTimeField(default=timezone.now)

    class Meta:
        db_table = "user_roles"
        constraints = [models.UniqueConstraint(fields=["user", "role"], name="user_roles_unico")]

    def __str__(self):
        return f"{self.user_id}:{self.role}"


class Platform(models.TextChoices):
    ANDROID = "android", "Android"
    IOS = "ios", "iOS"


class Device(models.Model):
    """Instalación de la app móvil (deviceId generado en el celular, RF-11 del maestro móvil)."""

    device_id = models.UUIDField(primary_key=True)
    user = models.ForeignKey(User, null=True, blank=True, on_delete=models.SET_NULL,
                             related_name="devices", help_text="Último usuario que inició sesión")
    platform = models.CharField(max_length=10, choices=Platform.choices)
    model = models.CharField(max_length=80, blank=True)
    os_version = models.CharField(max_length=40, blank=True)
    app_version = models.CharField(max_length=40, blank=True)
    first_seen_at = models.DateTimeField(default=timezone.now)
    last_seen_at = models.DateTimeField(default=timezone.now)
    revoked_at = models.DateTimeField(null=True, blank=True)
    revoked_by = models.ForeignKey(User, null=True, blank=True, on_delete=models.SET_NULL, related_name="+")

    class Meta:
        db_table = "devices"

    def __str__(self):
        return f"{self.model or self.platform} ({str(self.device_id)[:8]})"


class PasswordResetRequest(models.Model):
    class Status(models.TextChoices):
        PENDIENTE = "PENDIENTE", "Pendiente"
        ATENDIDA = "ATENDIDA", "Atendida"
        DESCARTADA = "DESCARTADA", "Descartada"

    email = models.EmailField()
    user = models.ForeignKey(User, null=True, blank=True, on_delete=models.SET_NULL, related_name="+")
    requested_at = models.DateTimeField(default=timezone.now)
    status = models.CharField(max_length=12, choices=Status.choices, default=Status.PENDIENTE)
    handled_by = models.ForeignKey(User, null=True, blank=True, on_delete=models.SET_NULL, related_name="+")
    handled_at = models.DateTimeField(null=True, blank=True)

    class Meta:
        db_table = "password_reset_requests"
        ordering = ["-requested_at"]
```

### B.4 Política de contraseña — `cuentas/validators.py`

`cuentas/validators.py`

```python
# cuentas/validators.py — Misma política que la app (validatePassword, Anexo C.2 del maestro móvil):
# al menos 8 caracteres (MinimumLengthValidator), letras y números, sin espacios al inicio ni al final.
import re

from django.core.exceptions import ValidationError

LETTER = re.compile(r"[A-Za-zÁÉÍÓÚÑáéíóúñ]")
DIGIT = re.compile(r"\d")


class LettersAndDigitsValidator:
    def validate(self, password, user=None):
        if not LETTER.search(password) or not DIGIT.search(password):
            raise ValidationError("Debe incluir letras y números.", code="password_policy")
        if password.strip() != password:
            raise ValidationError("No debe empezar ni terminar con espacios.", code="password_policy")

    def get_help_text(self):
        return "Debe incluir letras y números, sin espacios al inicio ni al final."
```

### B.5 Catálogos — `campo/models.py`

`campo/models.py`

```python
# campo/models.py — Catálogos del fundo (tablas field_lots, field_rows, field_segments, markers, quality_profiles).
from django.db import models
from django.db.models import F, Q


class FieldLot(models.Model):
    id = models.CharField(primary_key=True, max_length=20)  # p. ej. "SWG1" (Anexo D del maestro móvil)
    code = models.CharField(max_length=20, unique=True)  # "SWG 1"
    name = models.CharField(max_length=80)  # "Lote 1 (Piscina)"
    active = models.BooleanField(default=True)
    # (web v1.0) Contorno del lote en GeoJSON (Polygon o MultiPolygon, WGS84) para el mapa. Opcional.
    geometry = models.JSONField(null=True, blank=True)

    class Meta:
        db_table = "field_lots"
        ordering = ["code"]

    def __str__(self):
        return self.code


class FieldRow(models.Model):
    id = models.CharField(primary_key=True, max_length=30)  # "SWG1-H01"
    lot = models.ForeignKey(FieldLot, on_delete=models.PROTECT, related_name="rows")
    number = models.PositiveIntegerField()
    plant_count = models.PositiveIntegerField()
    active = models.BooleanField(default=True)

    class Meta:
        db_table = "field_rows"
        ordering = ["lot_id", "number"]
        constraints = [models.UniqueConstraint(fields=["lot", "number"], name="field_rows_lote_numero")]

    def __str__(self):
        return f"{self.lot_id} H{self.number:02d}"


class FieldSegment(models.Model):
    id = models.CharField(primary_key=True, max_length=40)
    row = models.ForeignKey(FieldRow, on_delete=models.PROTECT, related_name="segments")
    code = models.CharField(max_length=40)
    start_plant = models.PositiveIntegerField()
    end_plant = models.PositiveIntegerField()
    is_pilot = models.BooleanField(default=False)

    class Meta:
        db_table = "field_segments"
        ordering = ["row_id", "start_plant"]
        constraints = [models.CheckConstraint(condition=Q(start_plant__lte=F("end_plant")), name="segmento_rango")]

    def __str__(self):
        return self.code


class MarkerPosition(models.TextChoices):
    INICIO = "INICIO", "Inicio"
    FIN = "FIN", "Fin"
    INTERMEDIO = "INTERMEDIO", "Intermedio"


class Marker(models.Model):
    id = models.CharField(primary_key=True, max_length=40)
    row = models.ForeignKey(FieldRow, on_delete=models.PROTECT, related_name="markers")
    segment = models.ForeignKey(FieldSegment, null=True, blank=True, on_delete=models.SET_NULL, related_name="markers")
    code = models.CharField(max_length=40)
    description = models.CharField(max_length=200, blank=True)
    position = models.CharField(max_length=12, choices=MarkerPosition.choices)
    lat = models.FloatField(null=True, blank=True)
    lon = models.FloatField(null=True, blank=True)

    class Meta:
        db_table = "markers"
        ordering = ["row_id", "code"]

    def __str__(self):
        return self.code


class QualityProfile(models.Model):
    version = models.CharField(max_length=20, unique=True)  # Q0, Q1…
    params = models.JSONField()
    published_at = models.DateTimeField(null=True, blank=True)

    class Meta:
        db_table = "quality_profiles"

    def __str__(self):
        return self.version
```

### B.6 Datos de campo sincronizados — `monitoreo/models.py`

`monitoreo/models.py`

```python
# monitoreo/models.py — Lo que sincroniza la app (tablas monitoring_sessions, session_devices, monitoring_passes,
# marker_changes, capture_sequences, incidents). La web solo LEE estas tablas: nunca las edita (RN-W08).
from django.db import models

from campo.models import FieldLot, FieldRow, FieldSegment, Marker
from cuentas.models import Device, Platform, User


class SessionStatus(models.TextChoices):  # SYNCED es solo local en la app: nunca llega al servidor
    DRAFT = "DRAFT", "Borrador"
    PREPARING = "PREPARING", "Preparando"
    READY = "READY", "Lista"
    ACTIVE = "ACTIVE", "En curso"
    PAUSED = "PAUSED", "En pausa"
    CLOSING = "CLOSING", "Cerrando"
    CLOSED = "CLOSED", "Cerrada"


class CaptureMode(models.TextChoices):
    MANUAL = "MANUAL", "Manual"
    AUTOMATICO = "AUTOMATICO", "Automático"
    MIXTO = "MIXTO", "Mixto"  # eliminado en la app v0.2.0 (Q-20 del maestro móvil); se conserva para datos antiguos


class LateralCode(models.TextChoices):
    LATERAL_A = "LATERAL_A", "Lateral A"
    LATERAL_B = "LATERAL_B", "Lateral B"


class CameraRole(models.TextChoices):
    CAMERA_1 = "CAMERA_1", "Cámara 1"
    CAMERA_2 = "CAMERA_2", "Cámara 2"


class Direction(models.TextChoices):
    ASCENDENTE = "ASCENDENTE", "Ascendente"
    DESCENDENTE = "DESCENDENTE", "Descendente"


class PassStatus(models.TextChoices):
    READY = "READY", "Lista"
    ACTIVE = "ACTIVE", "En curso"
    PAUSED = "PAUSED", "En pausa"
    COMPLETED = "COMPLETED", "Completa"
    INCOMPLETE = "INCOMPLETE", "Incompleta"


class SequenceStatus(models.TextChoices):
    CREATED = "CREATED", "Creada"
    COMMAND_SENT = "COMMAND_SENT", "Orden enviada"
    PARTIAL = "PARTIAL", "Parcial"
    COMPLETE = "COMPLETE", "Completa"
    INCOMPLETE = "INCOMPLETE", "Incompleta"
    CANCELLED = "CANCELLED", "Cancelada"


class MonitoringSession(models.Model):
    session_id = models.UUIDField(primary_key=True)
    operator = models.ForeignKey(User, on_delete=models.PROTECT, related_name="sessions",
                                 db_column="operator_user_id")
    controller_device = models.ForeignKey(Device, on_delete=models.PROTECT, related_name="sessions",
                                          db_column="controller_device_id")
    status = models.CharField(max_length=12, choices=SessionStatus.choices)
    mode = models.CharField(max_length=12, choices=CaptureMode.choices)
    interval_ms = models.PositiveIntegerField(default=0)
    started_at = models.DateTimeField(null=True, blank=True)
    ended_at = models.DateTimeField(null=True, blank=True)
    app_version = models.CharField(max_length=40)
    config_version = models.CharField(max_length=40)
    quality_profile_version = models.CharField(max_length=20)
    short_test_passed_at = models.DateTimeField(null=True, blank=True)
    received_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    class Meta:
        db_table = "monitoring_sessions"
        indexes = [models.Index(fields=["started_at"], name="sesion_inicio_idx")]


class SessionDevice(models.Model):
    session = models.ForeignKey(MonitoringSession, on_delete=models.CASCADE, related_name="cameras")
    role = models.CharField(max_length=10, choices=CameraRole.choices)
    device = models.ForeignKey(Device, on_delete=models.PROTECT, related_name="+")
    user = models.ForeignKey(User, on_delete=models.PROTECT, related_name="+")
    platform = models.CharField(max_length=10, choices=Platform.choices)
    model = models.CharField(max_length=80, blank=True)
    os_version = models.CharField(max_length=40, blank=True)
    app_version = models.CharField(max_length=40, blank=True)
    paired_at = models.DateTimeField()
    released = models.BooleanField(default=False)

    class Meta:
        db_table = "session_devices"
        constraints = [models.UniqueConstraint(fields=["session", "role", "device"], name="session_devices_unico")]


class MonitoringPass(models.Model):
    pass_id = models.UUIDField(primary_key=True)
    session = models.ForeignKey(MonitoringSession, on_delete=models.CASCADE, related_name="passes")
    lot = models.ForeignKey(FieldLot, on_delete=models.PROTECT, related_name="+")
    row = models.ForeignKey(FieldRow, on_delete=models.PROTECT, related_name="passes")
    lateral_code = models.CharField(max_length=10, choices=LateralCode.choices)
    pass_order = models.PositiveIntegerField()
    direction = models.CharField(max_length=12, choices=Direction.choices)
    start_marker = models.ForeignKey(Marker, null=True, blank=True, on_delete=models.SET_NULL, related_name="+")
    end_marker = models.ForeignKey(Marker, null=True, blank=True, on_delete=models.SET_NULL, related_name="+")
    status = models.CharField(max_length=12, choices=PassStatus.choices)
    started_at = models.DateTimeField(null=True, blank=True)
    ended_at = models.DateTimeField(null=True, blank=True)
    sequences_total = models.PositiveIntegerField(default=0)
    sequences_complete = models.PositiveIntegerField(default=0)
    sequences_incomplete = models.PositiveIntegerField(default=0)
    received_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    class Meta:
        db_table = "monitoring_passes"
        indexes = [models.Index(fields=["row", "lateral_code", "status"], name="pasada_cobertura_idx")]


class MarkerChange(models.Model):
    marker_change_id = models.UUIDField(primary_key=True)
    monitoring_pass = models.ForeignKey(MonitoringPass, on_delete=models.CASCADE, related_name="marker_changes",
                                        db_column="pass_id")
    marker = models.ForeignKey(Marker, null=True, blank=True, on_delete=models.SET_NULL, related_name="+")
    segment = models.ForeignKey(FieldSegment, null=True, blank=True, on_delete=models.SET_NULL, related_name="+")
    changed_at = models.DateTimeField()
    lat = models.FloatField(null=True, blank=True)
    lon = models.FloatField(null=True, blank=True)
    gps_accuracy_m = models.FloatField(null=True, blank=True)
    gps_timestamp = models.DateTimeField(null=True, blank=True)

    class Meta:
        db_table = "marker_changes"


class CaptureSequence(models.Model):
    sequence_id = models.UUIDField(primary_key=True)
    monitoring_pass = models.ForeignKey(MonitoringPass, on_delete=models.CASCADE, related_name="sequences",
                                        db_column="pass_id")
    session = models.ForeignKey(MonitoringSession, on_delete=models.CASCADE, related_name="sequences")
    sequence_number = models.PositiveIntegerField()
    mode = models.CharField(max_length=12, choices=CaptureMode.choices)
    status = models.CharField(max_length=14, choices=SequenceStatus.choices)
    segment = models.ForeignKey(FieldSegment, null=True, blank=True, on_delete=models.SET_NULL, related_name="+")
    marker = models.ForeignKey(Marker, null=True, blank=True, on_delete=models.SET_NULL, related_name="+")
    lat = models.FloatField(null=True, blank=True)
    lon = models.FloatField(null=True, blank=True)
    gps_accuracy_m = models.FloatField(null=True, blank=True)
    gps_timestamp = models.DateTimeField(null=True, blank=True)
    issued_at = models.DateTimeField()
    completed_at = models.DateTimeField(null=True, blank=True)
    expected_capture_ids = models.JSONField(default=dict)  # {"CAMERA_1": uuid, "CAMERA_2": uuid}
    slot_outcomes = models.JSONField(default=dict)  # {"CAMERA_1": "OK_RECIBIDA", …}
    received_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    class Meta:
        db_table = "capture_sequences"
        constraints = [models.UniqueConstraint(fields=["monitoring_pass", "sequence_number"],
                                               name="secuencia_numero_unico")]


class IncidentType(models.TextChoices):
    CALIDAD = "CALIDAD", "Calidad"
    DESCONEXION = "DESCONEXION", "Desconexión"
    BATERIA = "BATERIA", "Batería"
    TEMPERATURA = "TEMPERATURA", "Temperatura"
    ESPACIO = "ESPACIO", "Espacio"
    GPS = "GPS", "GPS"
    TRANSFERENCIA = "TRANSFERENCIA", "Transferencia"
    SINCRONIZACION = "SINCRONIZACION", "Sincronización"
    SOPORTE = "SOPORTE", "Soporte"
    OPERADOR = "OPERADOR", "Operador"
    OTRO = "OTRO", "Otro"


class Incident(models.Model):
    class Severity(models.TextChoices):
        INFO = "INFO", "Información"
        AVISO = "AVISO", "Aviso"
        ERROR = "ERROR", "Error"

    class CreatedBy(models.TextChoices):
        SISTEMA = "SISTEMA", "Sistema"
        OPERADOR = "OPERADOR", "Operador"

    incident_id = models.UUIDField(primary_key=True)
    session = models.ForeignKey(MonitoringSession, on_delete=models.CASCADE, related_name="incidents")
    monitoring_pass = models.ForeignKey(MonitoringPass, null=True, blank=True, on_delete=models.SET_NULL,
                                        related_name="incidents", db_column="pass_id")
    sequence = models.ForeignKey(CaptureSequence, null=True, blank=True, on_delete=models.SET_NULL,
                                 related_name="incidents")
    capture_id = models.UUIDField(null=True, blank=True)  # la captura puede no haberse sincronizado
    device_id = models.UUIDField(null=True, blank=True)
    type = models.CharField(max_length=16, choices=IncidentType.choices)
    severity = models.CharField(max_length=6, choices=Severity.choices)
    detail = models.TextField()
    occurred_at = models.DateTimeField()
    created_by = models.CharField(max_length=10, choices=CreatedBy.choices)
    received_at = models.DateTimeField(auto_now_add=True)

    class Meta:
        db_table = "incidents"
        ordering = ["occurred_at"]
```

### B.7 Capturas — `evidencias/models.py`

`evidencias/models.py`

```python
# evidencias/models.py — Capturas confirmadas y su calidad (tablas captures, quality_results).
# Una fila en `captures` significa: foto en Cloudinary + confirmación aceptada por Django (RN-23 del maestro móvil).
from django.db import models
from django.utils import timezone

from cuentas.models import Device, User
from monitoreo.models import CameraRole, CaptureSequence, LateralCode, MonitoringPass, MonitoringSession


class QualityStatus(models.TextChoices):
    UTILIZABLE = "UTILIZABLE", "Utilizable"
    REPETIR_NITIDEZ = "REPETIR_NITIDEZ", "Repetir por nitidez"
    REPETIR_EXPOSICION = "REPETIR_EXPOSICION", "Repetir por exposición"
    ERROR_CAMARA = "ERROR_CAMARA", "Error de cámara"
    PENDIENTE_REVISION_TECNICA = "PENDIENTE_REVISION_TECNICA", "Pendiente de revisión técnica"


# Solo estas capturas se analizan con IA (RN-W11). Las rechazadas se guardan para auditoría y calibración.
ANALYZABLE_QUALITY = (QualityStatus.UTILIZABLE, QualityStatus.PENDIENTE_REVISION_TECNICA)


class Capture(models.Model):
    capture_id = models.UUIDField(primary_key=True)
    sequence = models.ForeignKey(CaptureSequence, on_delete=models.PROTECT, related_name="captures")
    session = models.ForeignKey(MonitoringSession, on_delete=models.PROTECT, related_name="captures")
    monitoring_pass = models.ForeignKey(MonitoringPass, on_delete=models.PROTECT, related_name="captures",
                                        db_column="pass_id")
    lateral_code = models.CharField(max_length=10, choices=LateralCode.choices)
    device = models.ForeignKey(Device, on_delete=models.PROTECT, related_name="+")
    camera_role = models.CharField(max_length=10, choices=CameraRole.choices)
    camera_user = models.ForeignKey(User, on_delete=models.PROTECT, related_name="+")
    operator_user = models.ForeignKey(User, on_delete=models.PROTECT, related_name="+")
    captured_at = models.DateTimeField()
    width = models.PositiveIntegerField()
    height = models.PositiveIntegerField()
    size_bytes = models.PositiveBigIntegerField()
    md5 = models.CharField(max_length=32)
    quality_status = models.CharField(max_length=28, choices=QualityStatus.choices)
    # Sin restricción de clave foránea: la repetición puede confirmarse antes que la foto que reemplaza.
    replaces_capture = models.ForeignKey("self", null=True, blank=True, on_delete=models.DO_NOTHING,
                                         db_constraint=False, related_name="replaced_by")
    retake_context = models.JSONField(null=True, blank=True)
    app_version = models.CharField(max_length=40)
    cloudinary_public_id = models.CharField(max_length=255, unique=True)
    cloudinary_version = models.BigIntegerField()
    cloudinary_bytes = models.PositiveBigIntegerField()
    cloudinary_format = models.CharField(max_length=10, blank=True)
    cloudinary_width = models.PositiveIntegerField(null=True, blank=True)
    cloudinary_height = models.PositiveIntegerField(null=True, blank=True)
    confirmed_at = models.DateTimeField(default=timezone.now)

    class Meta:
        db_table = "captures"
        indexes = [models.Index(fields=["captured_at"], name="captura_fecha_idx")]

    def __str__(self):
        return str(self.capture_id)


class QualityResult(models.Model):
    capture = models.OneToOneField(Capture, primary_key=True, on_delete=models.CASCADE, related_name="quality")
    status = models.CharField(max_length=28, choices=QualityStatus.choices)
    reasons = models.JSONField(default=list)
    metrics = models.JSONField(null=True, blank=True)
    profile_version = models.CharField(max_length=20)
    duration_ms = models.PositiveIntegerField(null=True, blank=True)

    class Meta:
        db_table = "quality_results"
```

### B.8 IA — `ia/models.py`

`ia/models.py`

```python
# ia/models.py — Modelos de detector, cola de análisis y cajas (tablas model_configs, ai_tasks, detections).
import uuid

from django.db import models
from django.db.models import F, Q
from django.utils import timezone

from evidencias.models import Capture


class ModelConfig(models.Model):
    name = models.CharField(max_length=60)  # p. ej. "yolo11n-chanchito"
    version = models.CharField(max_length=40, unique=True)
    weights_uri = models.CharField(max_length=255)
    weights_sha256 = models.CharField(max_length=64, blank=True)
    classes = models.JSONField(default=list)  # clases que el modelo puede sugerir
    conf_threshold = models.FloatField(default=0.25)
    iou_threshold = models.FloatField(default=0.45)
    imgsz = models.PositiveIntegerField(default=640)
    tiling = models.JSONField(default=dict, blank=True)
    active = models.BooleanField(default=False)
    created_at = models.DateTimeField(default=timezone.now)

    class Meta:
        db_table = "model_configs"
        constraints = [models.UniqueConstraint(fields=["active"], condition=Q(active=True),
                                               name="un_solo_modelo_activo")]

    def __str__(self):
        return f"{self.name} {self.version}"


class AiStatus(models.TextChoices):
    PENDIENTE_DE_ANALISIS = "PENDIENTE_DE_ANALISIS", "Pendiente de análisis"
    EN_ANALISIS = "EN_ANALISIS", "En análisis"
    INDICIO_SUGERIDO_POR_IA = "INDICIO_SUGERIDO_POR_IA", "Indicio sugerido por IA"
    SIN_INDICIOS_IA = "SIN_INDICIOS_IA", "Sin indicios de la IA"
    ERROR_DE_ANALISIS = "ERROR_DE_ANALISIS", "Error de análisis"


class AiTask(models.Model):
    task_id = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)
    capture = models.ForeignKey(Capture, on_delete=models.PROTECT, related_name="ai_tasks")
    model_config = models.ForeignKey(ModelConfig, on_delete=models.PROTECT, related_name="tasks")
    status = models.CharField(max_length=24, choices=AiStatus.choices, default=AiStatus.PENDIENTE_DE_ANALISIS)
    requested_at = models.DateTimeField(default=timezone.now)
    available_at = models.DateTimeField(default=timezone.now)
    locked_until = models.DateTimeField(null=True, blank=True)
    locked_by = models.CharField(max_length=80, blank=True)
    attempts = models.PositiveSmallIntegerField(default=0)
    started_at = models.DateTimeField(null=True, blank=True)
    finished_at = models.DateTimeField(null=True, blank=True)
    processing_ms = models.PositiveIntegerField(null=True, blank=True)
    model_version = models.CharField(max_length=40, blank=True)
    # (web v1.0) Tamaño de la imagen tal como la analizó el worker (después de aplicar la orientación EXIF).
    # Las cajas están en píxeles de esta imagen; la web usa estos valores para el viewBox del visor.
    image_width = models.PositiveIntegerField(null=True, blank=True)
    image_height = models.PositiveIntegerField(null=True, blank=True)
    error_message = models.TextField(blank=True)
    raw_output = models.JSONField(null=True, blank=True)

    class Meta:
        db_table = "ai_tasks"
        constraints = [models.UniqueConstraint(fields=["capture", "model_config"], name="ai_task_por_modelo")]
        indexes = [models.Index(fields=["status", "available_at"], name="ai_cola_idx")]


class DetectionReview(models.TextChoices):
    PENDIENTE_REVISION = "PENDIENTE_REVISION", "Pendiente de revisión"
    CONFIRMADO_POR_ESPECIALISTA = "CONFIRMADO_POR_ESPECIALISTA", "Confirmada"
    DESCARTADO = "DESCARTADO", "Descartada"
    EVIDENCIA_INSUFICIENTE = "EVIDENCIA_INSUFICIENTE", "Evidencia insuficiente"


class Detection(models.Model):
    task = models.ForeignKey(AiTask, on_delete=models.CASCADE, related_name="detections")
    class_name = models.CharField(max_length=60)
    confidence = models.FloatField()
    x_min = models.FloatField()
    y_min = models.FloatField()
    x_max = models.FloatField()
    y_max = models.FloatField()
    review_status = models.CharField(max_length=28, choices=DetectionReview.choices,
                                     default=DetectionReview.PENDIENTE_REVISION)

    class Meta:
        db_table = "detections"
        ordering = ["-confidence"]
        constraints = [
            models.CheckConstraint(condition=Q(confidence__gte=0) & Q(confidence__lte=1), name="det_confianza"),
            models.CheckConstraint(condition=Q(x_min__lt=F("x_max")) & Q(y_min__lt=F("y_max")), name="det_caja"),
        ]

    @property
    def box_width(self):
        return self.x_max - self.x_min

    @property
    def box_height(self):
        return self.y_max - self.y_min
```

### B.9 Auditoría y seguridad de Supabase — `auditoria/models.py`, `auditoria/seguridad_bd.py`, `auditoria/apps.py`, `auditoria/migrations/0003_tabla_cache.py`

`auditoria/models.py`

```python
# auditoria/models.py — Historial de cambios (tabla audit_events). Solo se inserta; nunca se edita ni se borra.
from django.conf import settings
from django.db import models
from django.utils import timezone


class AuditEvent(models.Model):
    entity_type = models.CharField(max_length=40)  # "case", "user", "ai_task", "notification_recipient"…
    entity_id = models.CharField(max_length=64)
    action = models.CharField(max_length=40)  # "CASO_DECIDIDO", "CUENTA_APROBADA"…
    user = models.ForeignKey(settings.AUTH_USER_MODEL, null=True, blank=True, on_delete=models.SET_NULL,
                             related_name="+")
    timestamp = models.DateTimeField(default=timezone.now)
    before = models.JSONField(null=True, blank=True)
    after = models.JSONField(null=True, blank=True)
    trace_id = models.CharField(max_length=40, blank=True)

    class Meta:
        db_table = "audit_events"
        ordering = ["-timestamp"]
        indexes = [models.Index(fields=["entity_type", "entity_id"], name="auditoria_entidad_idx"),
                   models.Index(fields=["timestamp"], name="auditoria_fecha_idx")]
```

`auditoria/seguridad_bd.py`

```python
# auditoria/seguridad_bd.py — Segunda barrera en Supabase (28.4 del maestro móvil): después de cada `migrate`
# activa RLS en todas las tablas del esquema public y quita privilegios a los roles de la Data API.
# Django se conecta con el dueño de las tablas, al que RLS no restringe (sin FORCE ROW LEVEL SECURITY).
from django.db import connection

API_ROLES = ("anon", "authenticated")


def asegurar_bd(**kwargs):
    if connection.vendor != "postgresql":
        return
    with connection.cursor() as cur:
        cur.execute(
            "SELECT c.relname FROM pg_class c JOIN pg_namespace n ON n.oid = c.relnamespace "
            "WHERE n.nspname = 'public' AND c.relkind = 'r' AND NOT c.relrowsecurity"
        )
        for (table,) in cur.fetchall():
            cur.execute(f'ALTER TABLE public."{table}" ENABLE ROW LEVEL SECURITY')
        cur.execute("SELECT rolname FROM pg_roles WHERE rolname = ANY(%s)", [list(API_ROLES)])
        for (role,) in cur.fetchall():
            cur.execute(f'REVOKE ALL ON ALL TABLES IN SCHEMA public FROM "{role}"')
            cur.execute(f'REVOKE ALL ON ALL SEQUENCES IN SCHEMA public FROM "{role}"')
            cur.execute(f'ALTER DEFAULT PRIVILEGES IN SCHEMA public REVOKE ALL ON TABLES FROM "{role}"')
```

`auditoria/apps.py`

```python
from django.apps import AppConfig
from django.db.models.signals import post_migrate


class AuditoriaConfig(AppConfig):
    name = "auditoria"
    default_auto_field = "django.db.models.BigAutoField"

    def ready(self):
        from auditoria.seguridad_bd import asegurar_bd

        post_migrate.connect(asegurar_bd, sender=self, dispatch_uid="auditoria_asegurar_bd")
```

`auditoria/migrations/0003_tabla_cache.py`

```python
# Crea la tabla de caché (web_cache) dentro de `migrate` para que la alcance el post_migrate de seguridad (RLS).
from django.core.management import call_command
from django.db import migrations


def crear(apps, schema_editor):
    call_command("createcachetable", verbosity=0)


class Migration(migrations.Migration):
    dependencies = [("auditoria", "0002_initial")]
    operations = [migrations.RunPython(crear, migrations.RunPython.noop)]
```

### B.10 Casos y decisiones — `revision/models.py`

`revision/models.py`

```python
# revision/models.py — Casos y decisiones del especialista (tablas review_cases, human_reviews).
import uuid

from django.db import models
from django.db.models import Q
from django.utils import timezone

from campo.models import FieldLot, FieldRow, FieldSegment, Marker
from cuentas.models import User
from evidencias.models import Capture
from ia.models import AiTask
from monitoreo.models import CaptureSequence, LateralCode, MonitoringPass, MonitoringSession


class ReviewStatus(models.TextChoices):
    PENDIENTE_REVISION = "PENDIENTE_REVISION", "Pendiente de revisión"
    CONFIRMADO_POR_ESPECIALISTA = "CONFIRMADO_POR_ESPECIALISTA", "Confirmado por especialista"
    DESCARTADO = "DESCARTADO", "Descartado"
    EVIDENCIA_INSUFICIENTE = "EVIDENCIA_INSUFICIENTE", "Evidencia insuficiente"


DECISIONS = (
    ReviewStatus.CONFIRMADO_POR_ESPECIALISTA,
    ReviewStatus.DESCARTADO,
    ReviewStatus.EVIDENCIA_INSUFICIENTE,
)


class CaseNotificationStatus(models.TextChoices):
    NO_APLICA = "NO_APLICA", "No aplica"
    PENDIENTE_ENVIO = "PENDIENTE_ENVIO", "Pendiente de envío"
    ENVIADO = "ENVIADO", "Enviado"
    ERROR_ENVIO = "ERROR_ENVIO", "Error de envío"


class LocationSource(models.TextChoices):
    GPS = "GPS", "GPS del controlador"
    MARCADOR = "MARCADOR", "Coordenadas del marcador (aproximada)"
    NINGUNA = "NINGUNA", "Sin coordenadas"


class Case(models.Model):
    """(web v1.0) Unidad de revisión: una foto (captura) con su análisis. Se abre cuando la IA sugiere un indicio
    (origen IA) o cuando el especialista revisa una foto sin indicios o con error de análisis (origen MANUAL)."""

    class Origin(models.TextChoices):
        IA = "IA", "Indicio sugerido por IA"
        MANUAL = "MANUAL", "Abierto por el especialista"

    id = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)
    capture = models.OneToOneField(Capture, on_delete=models.PROTECT, related_name="case")
    ai_task = models.ForeignKey(AiTask, null=True, blank=True, on_delete=models.PROTECT, related_name="+")
    origin = models.CharField(max_length=8, choices=Origin.choices)
    status = models.CharField(max_length=28, choices=ReviewStatus.choices, default=ReviewStatus.PENDIENTE_REVISION)
    notification_status = models.CharField(max_length=16, choices=CaseNotificationStatus.choices,
                                           default=CaseNotificationStatus.NO_APLICA)
    detections_count = models.PositiveIntegerField(default=0)
    max_confidence = models.FloatField(null=True, blank=True)
    # Ubicación copiada al abrir el caso (consultas rápidas del mapa, del plano y de la bandeja; RNF-W01).
    session = models.ForeignKey(MonitoringSession, on_delete=models.PROTECT, related_name="+")
    monitoring_pass = models.ForeignKey(MonitoringPass, on_delete=models.PROTECT, related_name="+", db_column="pass_id")
    sequence = models.ForeignKey(CaptureSequence, on_delete=models.PROTECT, related_name="+")
    lot = models.ForeignKey(FieldLot, on_delete=models.PROTECT, related_name="+")
    row = models.ForeignKey(FieldRow, on_delete=models.PROTECT, related_name="+")
    lateral_code = models.CharField(max_length=10, choices=LateralCode.choices)
    segment = models.ForeignKey(FieldSegment, null=True, blank=True, on_delete=models.PROTECT, related_name="+")
    marker = models.ForeignKey(Marker, null=True, blank=True, on_delete=models.PROTECT, related_name="+")
    lat = models.FloatField(null=True, blank=True)
    lon = models.FloatField(null=True, blank=True)
    gps_accuracy_m = models.FloatField(null=True, blank=True)
    location_source = models.CharField(max_length=10, choices=LocationSource.choices)
    captured_at = models.DateTimeField()
    opened_at = models.DateTimeField(default=timezone.now)
    decided_at = models.DateTimeField(null=True, blank=True)
    decided_by = models.ForeignKey(User, null=True, blank=True, on_delete=models.PROTECT, related_name="+")

    class Meta:
        db_table = "review_cases"
        indexes = [
            models.Index(fields=["status", "opened_at"], name="caso_bandeja_idx"),
            models.Index(fields=["lot", "row", "status"], name="caso_plano_idx"),
            models.Index(fields=["captured_at"], name="caso_fecha_idx"),
        ]

    def __str__(self):
        return f"Caso {str(self.id)[:8]}"


class HumanReview(models.Model):
    """Decisión del especialista. Nunca se edita ni se borra: una corrección crea otra fila (supersedes)."""

    id = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)
    case = models.ForeignKey(Case, on_delete=models.PROTECT, related_name="reviews")
    ai_task = models.ForeignKey(AiTask, null=True, blank=True, on_delete=models.PROTECT, related_name="+")
    decision = models.CharField(max_length=28, choices=[(d.value, d.label) for d in DECISIONS])
    observation = models.TextField(blank=True)
    confirmed_class = models.CharField(max_length=60, blank=True)
    rejected_detection_ids = models.JSONField(default=list, blank=True)  # cajas que el especialista marcó incorrectas
    reviewer = models.ForeignKey(User, on_delete=models.PROTECT, related_name="+")
    reviewed_at = models.DateTimeField(default=timezone.now)
    supersedes = models.OneToOneField("self", null=True, blank=True, on_delete=models.PROTECT,
                                      related_name="superseded_by")
    correction_reason = models.TextField(blank=True)
    is_current = models.BooleanField(default=True)

    class Meta:
        db_table = "human_reviews"
        ordering = ["reviewed_at"]
        constraints = [models.UniqueConstraint(fields=["case"], condition=Q(is_current=True),
                                               name="una_decision_vigente_por_caso")]
```

### B.11 Destinatarios y avisos — `notificaciones/models.py`

`notificaciones/models.py`

```python
# notificaciones/models.py — Destinatarios y cola de avisos de WhatsApp (tablas notification_recipients, notifications).
import uuid

from django.core.validators import RegexValidator
from django.db import models
from django.utils import timezone

from campo.models import FieldLot
from cuentas.models import User
from revision.models import Case, HumanReview

phone_e164 = RegexValidator(r"^\+[1-9]\d{7,14}$", "Usa el formato internacional, p. ej. +51987654321.")


class NotificationRecipient(models.Model):
    """(web v1.0) Persona que recibe el aviso de un caso confirmado (CU8: jefe de fundo y supervisor de zona)."""

    class RoleLabel(models.TextChoices):
        JEFE_FUNDO = "JEFE_FUNDO", "Jefe de fundo"
        SUPERVISOR_ZONA = "SUPERVISOR_ZONA", "Supervisor de zona"
        OTRO = "OTRO", "Otro"

    full_name = models.CharField(max_length=150)
    phone_e164 = models.CharField(max_length=16, validators=[phone_e164])
    role_label = models.CharField(max_length=16, choices=RoleLabel.choices)
    user = models.ForeignKey(User, null=True, blank=True, on_delete=models.SET_NULL, related_name="+",
                             help_text="Cuenta de la web con la que abre el enlace del caso")
    lots = models.ManyToManyField(FieldLot, blank=True, related_name="+", db_table="notification_recipient_lots",
                                  help_text="Vacío = todos los lotes")
    opt_in_at = models.DateTimeField(null=True, blank=True, help_text="Fecha en que aceptó recibir avisos")
    active = models.BooleanField(default=True)
    created_at = models.DateTimeField(default=timezone.now)

    class Meta:
        db_table = "notification_recipients"
        ordering = ["full_name"]

    def __str__(self):
        return f"{self.full_name} ({self.get_role_label_display()})"


class NotificationStatus(models.TextChoices):
    NO_APLICA = "NO_APLICA", "No aplica"  # en filas: aviso pendiente anulado por una corrección (8.5)
    PENDIENTE_ENVIO = "PENDIENTE_ENVIO", "Pendiente de envío"
    ENVIADO = "ENVIADO", "Enviado"
    ERROR_ENVIO = "ERROR_ENVIO", "Error de envío"


class Notification(models.Model):
    class Kind(models.TextChoices):
        CASO_CONFIRMADO = "CASO_CONFIRMADO", "Caso confirmado"

    id = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)
    case = models.ForeignKey(Case, on_delete=models.PROTECT, related_name="notifications")
    review = models.ForeignKey(HumanReview, on_delete=models.PROTECT, related_name="notifications")
    recipient = models.ForeignKey(NotificationRecipient, on_delete=models.PROTECT, related_name="notifications")
    channel = models.CharField(max_length=10, default="WHATSAPP")
    kind = models.CharField(max_length=20, choices=Kind.choices, default=Kind.CASO_CONFIRMADO)
    recipient_name = models.CharField(max_length=150)  # copia al encolar (auditoría)
    recipient_phone = models.CharField(max_length=16)
    status = models.CharField(max_length=16, choices=NotificationStatus.choices,
                              default=NotificationStatus.PENDIENTE_ENVIO)
    attempts = models.PositiveSmallIntegerField(default=0)
    available_at = models.DateTimeField(default=timezone.now)
    locked_until = models.DateTimeField(null=True, blank=True)
    locked_by = models.CharField(max_length=80, blank=True)
    provider_message_id = models.CharField(max_length=120, blank=True)
    sent_at = models.DateTimeField(null=True, blank=True)
    error = models.TextField(blank=True)
    created_at = models.DateTimeField(default=timezone.now)

    class Meta:
        db_table = "notifications"
        constraints = [models.UniqueConstraint(fields=["review", "recipient"], name="aviso_unico_por_decision")]
        indexes = [models.Index(fields=["status", "available_at"], name="aviso_cola_idx")]
```

---

## Anexo C. Código de referencia probado: servicios

### C.1 Reglas de la revisión — `revision/services.py`

`revision/services.py`

```python
# revision/services.py — Reglas de la revisión (RN-W01 a RN-W07). La web, Django Admin y cualquier script usan
# SOLO estas funciones para abrir, decidir o corregir un caso: así la regla se aplica igual en todas partes.
from django.conf import settings
from django.core.exceptions import PermissionDenied, ValidationError
from django.db import transaction
from django.utils import timezone

from auditoria import services as audit
from campo.models import Marker
from cuentas.models import Role
from evidencias.models import Capture
from ia.models import AiStatus, DetectionReview
from notificaciones import services as notif
from revision.models import (
    DECISIONS,
    Case,
    CaseNotificationStatus,
    HumanReview,
    LocationSource,
    ReviewStatus,
)

OBSERVATION_REQUIRED = {ReviewStatus.CONFIRMADO_POR_ESPECIALISTA, ReviewStatus.EVIDENCIA_INSUFICIENTE}


class CaseAlreadyDecided(Exception):
    """Otro especialista decidió el caso mientras este tenía la pantalla abierta (HTTP 409 en la web)."""

    def __init__(self, case):
        super().__init__("El caso ya tiene una decisión")
        self.case = case


class StaleReview(Exception):
    """La decisión vigente cambió desde que se abrió el formulario de corrección (HTTP 409)."""

    def __init__(self, current):
        super().__init__("La decisión vigente cambió")
        self.current = current


def _require_specialist(user):
    # RN-W01 (RN-01 del curso): solo el ESPECIALISTA_FITOSANITARIO confirma, descarta o corrige.
    if not (user and user.is_authenticated and user.is_active and user.has_role(Role.ESPECIALISTA_FITOSANITARIO)):
        raise PermissionDenied("Solo el especialista fitosanitario puede decidir un caso")


def case_location(capture):
    """Ubicación del caso: contexto de la repetición (RN-07 del maestro móvil) o de la secuencia.
    Prioridad de coordenadas: GPS de la secuencia → coordenadas del marcador (aproximada) → ninguna."""
    ctx = capture.retake_context
    if ctx:  # se guarda tal como llega de la app (camelCase)
        segment_id, marker_id = ctx.get("segmentId"), ctx.get("markerId")
        lat, lon, acc = ctx.get("lat"), ctx.get("lon"), ctx.get("gpsAccuracyM")
    else:
        seq = capture.sequence
        segment_id, marker_id = seq.segment_id, seq.marker_id
        lat, lon, acc = seq.lat, seq.lon, seq.gps_accuracy_m
    source = LocationSource.GPS
    if lat is None or lon is None:
        marker = Marker.objects.filter(pk=marker_id).only("lat", "lon").first() if marker_id else None
        if marker is not None and marker.lat is not None and marker.lon is not None:
            lat, lon, acc, source = marker.lat, marker.lon, None, LocationSource.MARCADOR
        else:
            lat = lon = acc = None
            source = LocationSource.NINGUNA
    return {"segment_id": segment_id, "marker_id": marker_id, "lat": lat, "lon": lon,
            "gps_accuracy_m": acc, "location_source": source}


def _new_case(capture, task, origin, stats):
    p = capture.monitoring_pass
    return Case.objects.create(
        capture=capture, ai_task=task, origin=origin,
        session_id=capture.session_id, monitoring_pass_id=p.pk, sequence_id=capture.sequence_id,
        lot_id=p.lot_id, row_id=p.row_id, lateral_code=capture.lateral_code, captured_at=capture.captured_at,
        **stats, **case_location(capture),
    )


def _stats(task):
    confidences = list(task.detections.values_list("confidence", flat=True)) if task else []
    return {"detections_count": len(confidences), "max_confidence": max(confidences) if confidences else None}


def open_case_from_analysis(task):
    """La llama el worker dentro de la transacción que guarda las cajas (ia.services.save_analysis_result)."""
    capture = Capture.objects.select_related("sequence", "monitoring_pass").get(pk=task.capture_id)
    case = Case.objects.select_for_update().filter(capture=capture).first()
    stats = _stats(task)
    if case is None:
        case = _new_case(capture, task, Case.Origin.IA, stats)
        audit.record("case", case.pk, "CASO_ABIERTO", None, None, {"origin": "IA", "aiTask": str(task.pk)})
    elif case.status == ReviewStatus.PENDIENTE_REVISION:
        case.ai_task = task
        case.detections_count, case.max_confidence = stats["detections_count"], stats["max_confidence"]
        case.save(update_fields=["ai_task", "detections_count", "max_confidence"])
        audit.record("case", case.pk, "CASO_NUEVO_ANALISIS", None, None, {"aiTask": str(task.pk)})
    else:  # un análisis nuevo nunca cambia una decisión tomada (RN-W06)
        audit.record("case", case.pk, "ANALISIS_POSTERIOR_A_DECISION", None, None, {"aiTask": str(task.pk)})
    return case


def open_manual_case(capture_id, user):
    """El especialista abre un caso sobre una foto SIN_INDICIOS_IA o ERROR_DE_ANALISIS (falso negativo posible)."""
    _require_specialist(user)
    with transaction.atomic():
        capture = Capture.objects.select_related("sequence", "monitoring_pass").select_for_update().get(pk=capture_id)
        existing = Case.objects.filter(capture=capture).first()
        if existing is not None:
            return existing, False
        task = (capture.ai_tasks.filter(status__in=[AiStatus.SIN_INDICIOS_IA, AiStatus.ERROR_DE_ANALISIS])
                .order_by("-finished_at").first())
        if task is None:
            raise ValidationError("Solo se abre un caso sobre fotos aceptadas por calidad y con el análisis terminado.")
        case = _new_case(capture, task, Case.Origin.MANUAL, {"detections_count": 0, "max_confidence": None})
        audit.record("case", case.pk, "CASO_ABIERTO", user, None, {"origin": "MANUAL", "aiTask": str(task.pk)})
    return case, True


def _clean_inputs(case, decision, observation, confirmed_class, rejected_detection_ids):
    if decision not in DECISIONS:
        raise ValidationError({"decision": "Decisión no válida."})
    observation = (observation or "").strip()
    max_len = settings.WEB["OBSERVATION_MAX_LENGTH"]
    errors = {}
    if len(observation) > max_len:
        errors["observation"] = f"Máximo {max_len} caracteres."
    elif decision in OBSERVATION_REQUIRED and not observation:
        errors["observation"] = ("Registra la observación fitosanitaria." if decision == ReviewStatus.CONFIRMADO_POR_ESPECIALISTA
                                 else "Indica qué falta en la evidencia para decidir.")
    task = case.ai_task
    valid_ids = set(task.detections.values_list("id", flat=True)) if task else set()
    try:
        rejected = {int(x) for x in (rejected_detection_ids or [])}
    except (TypeError, ValueError):
        rejected = {-1}
    if not rejected <= valid_ids:
        errors["rejected_detection_ids"] = "Hay cajas que no pertenecen a este caso."
    if decision != ReviewStatus.CONFIRMADO_POR_ESPECIALISTA:
        confirmed_class = ""
    elif confirmed_class and task and confirmed_class not in (task.model_config.classes or []):
        errors["confirmed_class"] = "Clase no válida para el modelo de este análisis."
    if errors:
        raise ValidationError(errors)
    return observation, confirmed_class or "", sorted(rejected)


def _apply_detection_status(case, decision, rejected):
    if case.ai_task_id is None:
        return
    dets = case.ai_task.detections
    if decision == ReviewStatus.CONFIRMADO_POR_ESPECIALISTA:
        dets.exclude(id__in=rejected).update(review_status=DetectionReview.CONFIRMADO_POR_ESPECIALISTA)
        dets.filter(id__in=rejected).update(review_status=DetectionReview.DESCARTADO)
    else:
        dets.update(review_status=decision)


def decide_case(case_id, user, decision, observation="", confirmed_class="", rejected_detection_ids=()):
    """CU6/CU7: decisión del especialista. Una sola transacción: decisión + cajas + caso + avisos + auditoría."""
    _require_specialist(user)
    with transaction.atomic():
        case = Case.objects.select_for_update(of=("self",)).select_related("ai_task__model_config").get(pk=case_id)
        if case.status != ReviewStatus.PENDIENTE_REVISION:
            raise CaseAlreadyDecided(case)
        observation, confirmed_class, rejected = _clean_inputs(
            case, decision, observation, confirmed_class, rejected_detection_ids)
        review = HumanReview.objects.create(
            case=case, ai_task=case.ai_task, decision=decision, observation=observation,
            confirmed_class=confirmed_class, rejected_detection_ids=rejected, reviewer=user)
        _apply_detection_status(case, decision, rejected)
        before = {"status": case.status}
        case.status = decision
        case.decided_at = review.reviewed_at
        case.decided_by = user
        if decision == ReviewStatus.CONFIRMADO_POR_ESPECIALISTA:
            notif.enqueue_for_review(review)  # RN-W03: el aviso nace solo de una confirmación
        case.notification_status = notif.case_notification_summary(case)
        case.save(update_fields=["status", "decided_at", "decided_by", "notification_status"])
        audit.record("case", case.pk, "CASO_DECIDIDO", user, before,
                     {"status": decision, "review": str(review.pk), "rejectedDetections": rejected})
    return review


def correct_decision(case_id, user, decision, observation, correction_reason, expected_review_id,
                     confirmed_class="", rejected_detection_ids=()):
    """Corrección (8.5): nueva decisión que reemplaza a la vigente; la anterior se conserva en el historial."""
    _require_specialist(user)
    correction_reason = (correction_reason or "").strip()
    if not correction_reason:
        raise ValidationError({"correction_reason": "Explica por qué corriges la decisión."})
    with transaction.atomic():
        case = Case.objects.select_for_update(of=("self",)).select_related("ai_task__model_config").get(pk=case_id)
        if case.status == ReviewStatus.PENDIENTE_REVISION:
            raise ValidationError("El caso todavía no tiene una decisión que corregir.")
        current = case.reviews.get(is_current=True)
        if str(current.pk) != str(expected_review_id):
            raise StaleReview(current)
        observation, confirmed_class, rejected = _clean_inputs(
            case, decision, observation, confirmed_class, rejected_detection_ids)
        current.is_current = False
        current.save(update_fields=["is_current"])
        review = HumanReview.objects.create(
            case=case, ai_task=case.ai_task, decision=decision, observation=observation,
            confirmed_class=confirmed_class, rejected_detection_ids=rejected, reviewer=user,
            supersedes=current, correction_reason=correction_reason)
        _apply_detection_status(case, decision, rejected)
        before = {"status": case.status, "review": str(current.pk)}
        if current.decision == ReviewStatus.CONFIRMADO_POR_ESPECIALISTA and decision != current.decision:
            notif.cancel_pending_for_case(case)  # lo ya ENVIADO no se puede retirar (Q-W07)
        if decision == ReviewStatus.CONFIRMADO_POR_ESPECIALISTA and not notif.case_has_effective_notifications(case):
            notif.enqueue_for_review(review)
        case.status = decision
        case.decided_at = review.reviewed_at
        case.decided_by = user
        case.notification_status = notif.case_notification_summary(case)
        case.save(update_fields=["status", "decided_at", "decided_by", "notification_status"])
        audit.record("case", case.pk, "CASO_CORREGIDO", user, before,
                     {"status": decision, "review": str(review.pk), "reason": correction_reason})
    return review


__all__ = ["CaseAlreadyDecided", "StaleReview", "CaseNotificationStatus", "case_location", "open_case_from_analysis",
           "open_manual_case", "decide_case", "correct_decision"]
```

### C.2 Avisos — `notificaciones/services.py`

`notificaciones/services.py`

```python
# notificaciones/services.py — Avisos de WhatsApp posteriores a la confirmación (CU8, RF10 del curso).
# La web solo ENCOLA (filas en `notifications` dentro de la transacción de la decisión); el worker envía.
from datetime import timedelta

from django.conf import settings
from django.db import transaction
from django.db.models import Q
from django.urls import reverse
from django.utils import timezone

from auditoria import services as audit
from notificaciones.models import Notification, NotificationRecipient, NotificationStatus
from notificaciones.whatsapp import WhatsAppError
from revision.models import Case, CaseNotificationStatus

MAX_ATTEMPTS = 5
LEASE = timedelta(minutes=2)


def recipients_for_case(case):
    """Destinatarios activos, con consentimiento registrado y con el lote del caso en su alcance (vacío = todos)."""
    return (NotificationRecipient.objects.filter(active=True, opt_in_at__isnull=False)
            .filter(Q(lots__isnull=True) | Q(lots=case.lot_id)).distinct().order_by("pk"))


def enqueue_for_review(review):
    case = review.case
    recipients = list(recipients_for_case(case))
    for r in recipients:
        Notification.objects.get_or_create(
            review=review, recipient=r,
            defaults={"case": case, "recipient_name": r.full_name, "recipient_phone": r.phone_e164})
    if not recipients:
        audit.record("case", case.pk, "AVISO_SIN_DESTINATARIOS", None, None, {"lot": case.lot_id})
    return len(recipients)


def case_has_effective_notifications(case):
    return Notification.objects.filter(case=case).exclude(status=NotificationStatus.NO_APLICA).exists()


def case_notification_summary(case):
    statuses = set(Notification.objects.filter(case=case).exclude(status=NotificationStatus.NO_APLICA)
                   .values_list("status", flat=True))
    if not statuses:
        return CaseNotificationStatus.NO_APLICA
    if NotificationStatus.PENDIENTE_ENVIO in statuses:
        return CaseNotificationStatus.PENDIENTE_ENVIO
    if NotificationStatus.ERROR_ENVIO in statuses:
        return CaseNotificationStatus.ERROR_ENVIO
    return CaseNotificationStatus.ENVIADO


def cancel_pending_for_case(case):
    return (Notification.objects.filter(case=case, status=NotificationStatus.PENDIENTE_ENVIO)
            .update(status=NotificationStatus.NO_APLICA, error="Anulado: la decisión se corrigió antes del envío."))


def _refresh_case(case_id):
    case = Case.objects.select_for_update().get(pk=case_id)
    case.notification_status = case_notification_summary(case)
    case.save(update_fields=["notification_status"])


# ------------------------------------------------------------------ worker (python manage.py worker_ia)
def claim_next_notification(worker_id):
    now = timezone.now()
    with transaction.atomic():
        n = (Notification.objects.select_for_update(skip_locked=True)
             .filter(status=NotificationStatus.PENDIENTE_ENVIO, available_at__lte=now)
             .filter(Q(locked_until__isnull=True) | Q(locked_until__lte=now))
             .order_by("created_at").first())
        if n is None:
            return None
        n.locked_by, n.locked_until, n.attempts = worker_id, now + LEASE, n.attempts + 1
        n.save(update_fields=["locked_by", "locked_until", "attempts"])
        return n


def _one_line(text):
    # Los parámetros de una plantilla de WhatsApp no admiten saltos de línea ni tabulaciones.
    return " ".join(str(text).split())


def build_whatsapp_payload(n):
    case = (Case.objects.select_related("lot", "row", "segment", "marker").get(pk=n.case_id))
    where = " · ".join(x for x in [
        f"segmento {case.segment.code}" if case.segment_id else "",
        f"marcador {case.marker.code}" if case.marker_id else "",
    ] if x) or "sin segmento ni marcador"
    captured = timezone.localtime(case.captured_at).strftime("%d/%m/%Y %H:%M")
    link = settings.PUBLIC_BASE_URL.rstrip("/") + reverse("web:caso", args=[case.pk])
    params = [case.lot.code, str(case.row.number), case.get_lateral_code_display(), where, captured, link]
    return {
        "messaging_product": "whatsapp",
        "to": n.recipient_phone.lstrip("+"),
        "type": "template",
        "template": {
            "name": settings.WHATSAPP_TEMPLATE,
            "language": {"code": settings.WHATSAPP_TEMPLATE_LANG},
            "components": [{"type": "body",
                            "parameters": [{"type": "text", "text": _one_line(p)} for p in params]}],
        },
    }


def mark_sent(n, provider_message_id):
    with transaction.atomic():
        n = Notification.objects.select_for_update().get(pk=n.pk)
        n.status, n.provider_message_id, n.sent_at = NotificationStatus.ENVIADO, provider_message_id, timezone.now()
        n.locked_until, n.error = None, ""
        n.save()
        _refresh_case(n.case_id)
    return n


def mark_failed(n, error, retryable=True):
    with transaction.atomic():
        n = Notification.objects.select_for_update().get(pk=n.pk)
        n.error, n.locked_until = str(error)[:2000], None
        if retryable and n.attempts < MAX_ATTEMPTS:
            n.available_at = timezone.now() + timedelta(minutes=2 ** n.attempts)  # 2, 4, 8, 16 min
        else:
            n.status = NotificationStatus.ERROR_ENVIO  # no toca la decisión del especialista
        n.save()
        _refresh_case(n.case_id)
    return n


def process_pending_notifications(client, worker_id, limit=20):
    sent = 0
    for _ in range(limit):
        n = claim_next_notification(worker_id)
        if n is None:
            break
        try:
            message_id = client.send(build_whatsapp_payload(n))
        except WhatsAppError as exc:
            mark_failed(n, exc, retryable=exc.retryable)
        else:
            mark_sent(n, message_id)
            sent += 1
    return sent
```

### C.3 Clientes de WhatsApp — `notificaciones/whatsapp.py`

`notificaciones/whatsapp.py`

```python
# notificaciones/whatsapp.py — Clientes de WhatsApp. En desarrollo se usa ConsoleClient (no envía nada).
import logging
import uuid

from django.conf import settings
from django.utils.module_loading import import_string

log = logging.getLogger(__name__)


class WhatsAppError(Exception):
    def __init__(self, message, retryable=True):
        super().__init__(message)
        self.retryable = retryable


class ConsoleClient:
    def send(self, payload):
        log.info("WhatsApp (simulado) a %s: %s", payload["to"], payload["template"]["components"])
        return f"console-{uuid.uuid4()}"


class CloudApiClient:
    """WhatsApp Cloud API: POST https://graph.facebook.com/<versión>/<phone-number-id>/messages."""

    def __init__(self, token=None, phone_id=None, version=None, timeout=15):
        self.token = token or settings.WHATSAPP_TOKEN
        self.phone_id = phone_id or settings.WHATSAPP_PHONE_ID
        self.version = version or settings.WHATSAPP_GRAPH_VERSION
        self.timeout = timeout
        if not (self.token and self.phone_id and self.version):
            raise WhatsAppError("Faltan WHATSAPP_TOKEN, WHATSAPP_PHONE_ID o WHATSAPP_GRAPH_VERSION", retryable=False)

    def send(self, payload):
        import requests

        url = f"https://graph.facebook.com/{self.version}/{self.phone_id}/messages"
        try:
            resp = requests.post(url, json=payload, timeout=self.timeout,
                                 headers={"Authorization": f"Bearer {self.token}"})
        except requests.RequestException as exc:
            raise WhatsAppError(f"Red: {exc}") from exc
        if resp.status_code == 429 or resp.status_code >= 500:
            raise WhatsAppError(f"HTTP {resp.status_code}: {resp.text[:300]}")
        if resp.status_code >= 400:  # número inválido, plantilla no aprobada, token vencido…: no se reintenta solo
            raise WhatsAppError(f"HTTP {resp.status_code}: {resp.text[:300]}", retryable=False)
        return resp.json()["messages"][0]["id"]


def get_client():
    return import_string(settings.WHATSAPP_CLIENT)()
```

### C.4 Cola de análisis — `ia/services.py`

`ia/services.py`

```python
# ia/services.py — Cola de análisis (extracto que usa la web; el ciclo completo del worker está en 28.8 del maestro móvil).
from datetime import timedelta

from django.db import transaction
from django.utils import timezone

from auditoria import services as audit
from evidencias.models import ANALYZABLE_QUALITY
from ia.models import AiStatus, AiTask, Detection, ModelConfig

LEASE = timedelta(minutes=5)
MAX_ATTEMPTS = 3


class NoActiveModel(Exception):
    pass


class InvalidTaskState(Exception):
    pass


def enqueue_analysis(capture):
    """La llama la confirmación de captura (POST /captures/upload) DENTRO de su transacción.
    Solo se analizan fotos aceptadas por calidad (RN-W11); devuelve None para las rechazadas."""
    if capture.quality_status not in ANALYZABLE_QUALITY:
        return None
    model = ModelConfig.objects.filter(active=True).first()
    if model is None:
        raise NoActiveModel("No hay un modelo de IA activo (model_configs.active)")
    task, _ = AiTask.objects.get_or_create(capture=capture, model_config=model)
    return task


def claim_next_task(worker_id):
    now = timezone.now()
    with transaction.atomic():
        task = (
            AiTask.objects.select_for_update(skip_locked=True)
            .filter(status__in=[AiStatus.PENDIENTE_DE_ANALISIS, AiStatus.EN_ANALISIS], available_at__lte=now)
            .exclude(status=AiStatus.EN_ANALISIS, locked_until__gt=now)
            .order_by("requested_at")
            .first()
        )
        if task is None:
            return None
        task.status = AiStatus.EN_ANALISIS
        task.locked_by = worker_id
        task.locked_until = now + LEASE
        task.attempts += 1
        task.started_at = now
        task.save(update_fields=["status", "locked_by", "locked_until", "attempts", "started_at"])
        return task


def save_analysis_result(task, boxes, image_width, image_height, processing_ms, model_version, raw_output=None):
    """Guarda el resultado del worker en UNA transacción y, si hay cajas, abre el caso (sin callbacks)."""
    from revision.services import open_case_from_analysis  # import local: revision depende de ia

    with transaction.atomic():
        task = AiTask.objects.select_for_update().get(pk=task.pk)
        task.detections.all().delete()  # un reintento reemplaza un resultado parcial anterior
        Detection.objects.bulk_create([
            Detection(task=task, class_name=b["class_name"], confidence=b["confidence"],
                      x_min=b["x_min"], y_min=b["y_min"], x_max=b["x_max"], y_max=b["y_max"])
            for b in boxes
        ])
        task.status = AiStatus.INDICIO_SUGERIDO_POR_IA if boxes else AiStatus.SIN_INDICIOS_IA
        task.image_width, task.image_height = image_width, image_height
        task.processing_ms = processing_ms
        task.model_version = model_version
        task.raw_output = raw_output
        task.finished_at = timezone.now()
        task.locked_until = None
        task.error_message = ""
        task.save()
        case = open_case_from_analysis(task) if boxes else None
    return task, case


def mark_analysis_failed(task, error_message):
    with transaction.atomic():
        task = AiTask.objects.select_for_update().get(pk=task.pk)
        task.error_message = error_message[:2000]
        task.locked_until = None
        if task.attempts >= MAX_ATTEMPTS:
            task.status = AiStatus.ERROR_DE_ANALISIS
            task.finished_at = timezone.now()
        else:
            task.status = AiStatus.PENDIENTE_DE_ANALISIS
            task.available_at = timezone.now() + timedelta(seconds=30 * (2 ** (task.attempts - 1)))
        task.save()
    return task


def requeue_failed(task_id, user):
    """Acción del ADMINISTRADOR en la web (WEB-16): vuelve a encolar una tarea con ERROR_DE_ANALISIS."""
    with transaction.atomic():
        task = AiTask.objects.select_for_update().get(pk=task_id)
        if task.status != AiStatus.ERROR_DE_ANALISIS:
            raise InvalidTaskState("Solo se reencolan tareas con error de análisis")
        before = {"status": task.status, "attempts": task.attempts, "error": task.error_message}
        task.status = AiStatus.PENDIENTE_DE_ANALISIS
        task.attempts = 0
        task.available_at = timezone.now()
        task.error_message = ""
        task.save(update_fields=["status", "attempts", "available_at", "error_message"])
        audit.record("ai_task", task.pk, "TAREA_IA_REENCOLADA", user, before, {"status": task.status})
    return task
```

### C.5 Cuentas y celulares — `cuentas/services.py`

`cuentas/services.py`

```python
# cuentas/services.py — Acciones del ADMINISTRADOR sobre cuentas y celulares (WEB-13, WEB-14).
import secrets

from django.core.exceptions import PermissionDenied, ValidationError
from django.db import transaction
from django.db.models import Q
from django.utils import timezone

from auditoria import services as audit
from cuentas.models import AccountStatus, Device, PasswordResetRequest, Role, User, UserRole

TEMP_ALPHABET = "ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnpqrstuvwxyz23456789"  # sin 0/O/1/l/I


def _require_admin(admin):
    if not (admin and admin.is_authenticated and admin.is_active and admin.has_role(Role.ADMINISTRADOR)):
        raise PermissionDenied("Solo el administrador gestiona cuentas")


def _blacklist_refresh_tokens(user):
    """Cierra la sesión de la app en todos los celulares del usuario (lista negra de SimpleJWT)."""
    from rest_framework_simplejwt.token_blacklist.models import BlacklistedToken, OutstandingToken

    for token in OutstandingToken.objects.filter(user=user, expires_at__gt=timezone.now()):
        BlacklistedToken.objects.get_or_create(token=token)


def set_roles(user_id, admin, roles):
    _require_admin(admin)
    roles = set(roles)
    if not roles or not roles <= set(Role.values):
        raise ValidationError({"roles": "Elige al menos un rol válido."})
    with transaction.atomic():
        user = User.objects.select_for_update().get(pk=user_id)
        before = sorted(user.roles)
        if user.pk == admin.pk and Role.ADMINISTRADOR not in roles:
            raise ValidationError({"roles": "No puedes quitarte el rol de administrador a ti mismo."})
        UserRole.objects.filter(user=user).exclude(role__in=roles).delete()
        for role in roles - set(before):
            UserRole.objects.create(user=user, role=role)
        audit.record("user", user.pk, "ROLES_CAMBIADOS", admin, {"roles": before}, {"roles": sorted(roles)})
    return user


def approve_user(user_id, admin, roles):
    _require_admin(admin)
    with transaction.atomic():
        user = User.objects.select_for_update().get(pk=user_id)
        if user.status != AccountStatus.PENDIENTE_APROBACION:
            raise ValidationError("La cuenta no está pendiente de aprobación.")
        set_roles(user.pk, admin, roles)
        user.status, user.approved_by, user.approved_at = AccountStatus.ACTIVO, admin, timezone.now()
        user.save(update_fields=["status", "approved_by", "approved_at"])
        audit.record("user", user.pk, "CUENTA_APROBADA", admin,
                     {"status": AccountStatus.PENDIENTE_APROBACION}, {"status": AccountStatus.ACTIVO})
    return user


def change_status(user_id, admin, new_status):
    """Rechazar (pendiente → RECHAZADO), bloquear (ACTIVO → BLOQUEADO) o desbloquear (BLOQUEADO → ACTIVO)."""
    _require_admin(admin)
    allowed = {
        AccountStatus.PENDIENTE_APROBACION: {AccountStatus.RECHAZADO},
        AccountStatus.ACTIVO: {AccountStatus.BLOQUEADO},
        AccountStatus.BLOQUEADO: {AccountStatus.ACTIVO},
    }
    with transaction.atomic():
        user = User.objects.select_for_update().get(pk=user_id)
        if user.pk == admin.pk:
            raise ValidationError("No puedes cambiar el estado de tu propia cuenta.")
        if new_status not in allowed.get(user.status, set()):
            raise ValidationError("Cambio de estado no permitido.")
        before = user.status
        user.status = new_status
        user.save(update_fields=["status"])
        if new_status == AccountStatus.BLOQUEADO:
            _blacklist_refresh_tokens(user)  # la web la saca en la siguiente petición (User.is_active)
        audit.record("user", user.pk, "CUENTA_ESTADO", admin, {"status": before}, {"status": new_status})
    return user


def set_temporary_password(user_id, admin):
    """D-07: contraseña temporal + cambio obligatorio. Devuelve la contraseña UNA vez para entregarla en persona."""
    _require_admin(admin)
    while True:  # cumple la política (letras y números) para que la app la acepte sin internet después
        temp = "".join(secrets.choice(TEMP_ALPHABET) for _ in range(12))
        if any(ch.isdigit() for ch in temp) and any(ch.isalpha() for ch in temp):
            break
    with transaction.atomic():
        user = User.objects.select_for_update().get(pk=user_id)
        user.set_password(temp)
        user.must_change_password = True
        user.save(update_fields=["password", "must_change_password"])
        _blacklist_refresh_tokens(user)
        PasswordResetRequest.objects.filter(
            Q(user=user) | Q(email__iexact=user.email), status=PasswordResetRequest.Status.PENDIENTE).update(
            status=PasswordResetRequest.Status.ATENDIDA, handled_by=admin, handled_at=timezone.now())
        audit.record("user", user.pk, "CONTRASENA_TEMPORAL", admin, None, {"mustChangePassword": True})
    return temp


def revoke_device(device_id, admin):
    _require_admin(admin)
    with transaction.atomic():
        device = Device.objects.select_for_update().get(pk=device_id)
        if device.revoked_at is None:
            device.revoked_at, device.revoked_by = timezone.now(), admin
            device.save(update_fields=["revoked_at", "revoked_by"])
            audit.record("device", device.pk, "CELULAR_REVOCADO", admin, None, {"revokedAt": device.revoked_at.isoformat()})
    return device
```

### C.6 URLs firmadas — `evidencias/media.py`

`evidencias/media.py`

```python
# evidencias/media.py — URLs firmadas de Cloudinary para la web (W-10, DW-26). Nunca se guardan en la base de datos.
import cloudinary.utils

# Transformaciones con nombre creadas en la consola de Cloudinary (28.7 del maestro móvil).
VARIANTS = {
    "miniatura": "miniatura",  # c_limit,w_400,q_auto — bandeja, listas, sesiones
    "revision": "revision",  # c_limit,w_1600,q_auto — visor del caso
    "original": None,  # archivo original — solo al pulsar «Original» en el visor
}


def signed_image_url(capture, variant="revision"):
    if variant not in VARIANTS:
        raise ValueError(f"Variante desconocida: {variant}")
    options = {"type": "authenticated", "sign_url": True, "secure": True, "version": capture.cloudinary_version}
    named = VARIANTS[variant]
    if named:
        options.update(transformation=named, format="jpg")
    else:
        options["format"] = capture.cloudinary_format or "jpg"
    url, _ = cloudinary.utils.cloudinary_url(capture.cloudinary_public_id, **options)
    return url
```

### C.7 Auditoría — `auditoria/services.py`

`auditoria/services.py`

```python
# auditoria/services.py — Registro de auditoría. Lo llaman los servicios dentro de su transacción.
from auditoria.models import AuditEvent


def record(entity_type, entity_id, action, user=None, before=None, after=None, trace_id=""):
    return AuditEvent.objects.create(
        entity_type=entity_type,
        entity_id=str(entity_id),
        action=action,
        user=user if (user is not None and user.is_authenticated) else None,
        before=before,
        after=after,
        trace_id=trace_id,
    )
```

---

## Anexo D. Código de referencia probado: web (Python)

### D.1 Permisos — `web/permissions.py`

`web/permissions.py`

```python
# web/permissions.py — Matriz única de permisos de la web (sección 7.2). Las vistas, las plantillas y las pruebas
# leen ESTA tabla; los servicios vuelven a comprobar las reglas críticas (RN-W01) por su cuenta.
from functools import wraps

from django.conf import settings
from django.contrib.auth.views import redirect_to_login
from django.core.exceptions import PermissionDenied
from django.shortcuts import redirect, resolve_url
from django.utils.http import urlencode
from django_htmx.http import HttpResponseClientRedirect

from cuentas.models import Role

A, E, S = Role.ADMINISTRADOR, Role.ESPECIALISTA_FITOSANITARIO, Role.SUPERVISOR

PERMISOS = {
    "dashboard.ver": {A, E, S},
    "bandeja.ver": {A, E, S},
    "caso.ver": {A, E, S},
    "caso.decidir": {E},
    "caso.corregir": {E},
    "caso.abrir_manual": {E},
    "mapa.ver": {A, E, S},
    "sesiones.ver": {A, E, S},
    "reportes.exportar": {A, E, S},
    "notificaciones.ver": {A, E, S},
    "ia.ver": {A, E},
    "ia.reencolar": {A},
    "usuarios.gestionar": {A},
    "dispositivos.gestionar": {A},
    "destinatarios.gestionar": {A},
    "auditoria.ver": {A},
}


def can(user, perm):
    return bool(user and user.is_authenticated and user.can_use_web and user.has_role(*PERMISOS[perm]))


def web_view(perm):
    """Decorador obligatorio de toda vista de la web (W-07)."""
    if perm not in PERMISOS:
        raise KeyError(perm)

    def decorator(view):
        @wraps(view)
        def wrapper(request, *args, **kwargs):
            user = request.user
            if not user.is_authenticated:
                if request.htmx:  # sesión vencida durante un refresco HTMX: recargar la página completa en el login
                    nxt = request.htmx.current_url_abs_path or "/"
                    return HttpResponseClientRedirect(f"{resolve_url(settings.LOGIN_URL)}?{urlencode({'next': nxt})}")
                return redirect_to_login(request.get_full_path())
            if user.must_change_password:
                return redirect("web:cambiar_contrasena")
            if not can(user, perm):
                raise PermissionDenied
            return view(request, *args, **kwargs)

        wrapper.web_permission = perm
        return wrapper

    return decorator
```

### D.2 Middleware, contexto y errores — `web/middleware.py`, `web/context_processors.py`, `web/errors.py`

`web/middleware.py`

```python
# web/middleware.py
from django.utils.cache import add_never_cache_headers, patch_vary_headers


class NoStoreForAuthenticatedMiddleware:
    """Las páginas con datos de casos y URLs firmadas no se guardan en cachés ni en el historial del navegador."""

    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        response = self.get_response(request)
        if getattr(request, "user", None) is not None and request.user.is_authenticated:
            add_never_cache_headers(response)
        patch_vary_headers(response, ("HX-Request",))  # la misma URL devuelve página completa o fragmento
        return response
```

`web/context_processors.py`

```python
# web/context_processors.py
from django.conf import settings

from web.permissions import PERMISOS, can


def web_context(request):
    user = getattr(request, "user", None)
    puede = {perm.replace(".", "_"): can(user, perm) for perm in PERMISOS} if user is not None else {}
    return {"puede": puede, "APP_ENV": settings.APP_ENV, "WEB": settings.WEB}
```

`web/errors.py`

```python
# web/errors.py — 403 legible (también para HTMX: el fragmento muestra el motivo en lugar de romper la pantalla).
from django.shortcuts import render


def forbidden(request, exception=None):
    template = "web/partials/error.html" if getattr(request, "htmx", False) else "web/403.html"
    return render(request, template, {"mensaje": "No tienes permiso para esta acción."}, status=403)
```

### D.3 Formularios — `web/forms.py`

`web/forms.py`

```python
# web/forms.py — Formularios de la web. Validan forma; las reglas de negocio están en los servicios.
from django import forms
from django.conf import settings
from django.core.cache import cache

from cuentas.models import AccountStatus, Role, User
from notificaciones.models import NotificationRecipient
from revision.models import DECISIONS, ReviewStatus
from web import messages as M

DECISION_CHOICES = [(d.value, d.label) for d in DECISIONS]


def _lock_key(email):
    return f"web-login-fallos:{email.lower()}"


class LoginForm(forms.Form):
    email = forms.EmailField(label="Correo", widget=forms.EmailInput(attrs={"autocomplete": "username", "autofocus": True}))
    password = forms.CharField(label="Contraseña", strip=False,
                               widget=forms.PasswordInput(attrs={"autocomplete": "current-password"}))

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.user = None

    def clean(self):
        data = super().clean()
        email, password = data.get("email"), data.get("password")
        if not email or not password:
            return data
        key = _lock_key(email)
        fails = cache.get(key, 0)
        if fails >= settings.WEB["LOGIN_MAX_FAILED"]:
            raise forms.ValidationError(M.LOGIN_BLOQUEADO.format(minutos=settings.WEB["LOGIN_LOCKOUT_MINUTES"]))
        user = User.objects.filter(email__iexact=email).first()
        if user is None:
            User().set_password(password)  # mismo tiempo de respuesta exista o no la cuenta
            ok = False
        else:
            ok = user.check_password(password)
        if not ok:
            cache.set(key, fails + 1, settings.WEB["LOGIN_LOCKOUT_MINUTES"] * 60)
            raise forms.ValidationError(M.LOGIN_INVALIDO)
        cache.delete(key)
        # El estado solo se revela después de comprobar la contraseña (no permite averiguar qué correos existen).
        if user.status == AccountStatus.PENDIENTE_APROBACION:
            raise forms.ValidationError(M.CUENTA_PENDIENTE)
        if user.status == AccountStatus.RECHAZADO:
            raise forms.ValidationError(M.CUENTA_RECHAZADA)
        if user.status == AccountStatus.BLOQUEADO:
            raise forms.ValidationError(M.CUENTA_BLOQUEADA)
        if not user.can_use_web:
            raise forms.ValidationError(M.SIN_ACCESO_WEB)
        self.user = user
        return data


class DecisionForm(forms.Form):
    decision = forms.ChoiceField(choices=DECISION_CHOICES, widget=forms.RadioSelect)
    observation = forms.CharField(required=False, widget=forms.Textarea(attrs={"rows": 4}),
                                  max_length=settings.WEB["OBSERVATION_MAX_LENGTH"])
    confirmed_class = forms.ChoiceField(required=False, choices=[])
    rejected_detection_ids = forms.MultipleChoiceField(required=False, choices=[],
                                                       widget=forms.CheckboxSelectMultiple)

    def __init__(self, *args, case=None, **kwargs):
        super().__init__(*args, **kwargs)
        task = case.ai_task if case else None
        classes = (task.model_config.classes if task else []) or []
        self.fields["confirmed_class"].choices = [("", "— Sin especificar —")] + [(c, c) for c in classes]
        dets = list(task.detections.all()) if task else []
        self.fields["rejected_detection_ids"].choices = [(str(d.id), f"Caja {i}") for i, d in enumerate(dets, 1)]


class CorrectionForm(DecisionForm):
    correction_reason = forms.CharField(widget=forms.Textarea(attrs={"rows": 2}), max_length=1000)
    expected_review_id = forms.UUIDField(widget=forms.HiddenInput)


class ApproveForm(forms.Form):
    roles = forms.MultipleChoiceField(choices=Role.choices, widget=forms.CheckboxSelectMultiple)


class RecipientForm(forms.ModelForm):
    opt_in = forms.BooleanField(required=False, label="Aceptó recibir avisos por WhatsApp")

    class Meta:
        model = NotificationRecipient
        fields = ["full_name", "phone_e164", "role_label", "user", "lots", "active"]
        widgets = {"lots": forms.CheckboxSelectMultiple}

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.fields["user"].queryset = User.objects.filter(
            status=AccountStatus.ACTIVO, user_roles__role__in=[Role.SUPERVISOR, Role.ADMINISTRADOR,
                                                                Role.ESPECIALISTA_FITOSANITARIO]).distinct()
        self.fields["user"].required = False
        if self.instance.pk:
            self.fields["opt_in"].initial = self.instance.opt_in_at is not None


class FiltroCasosForm(forms.Form):
    ESTADOS = [("", "Todos")] + list(ReviewStatus.choices)
    ORDEN = [("antiguos", "Más antiguos primero"), ("recientes", "Más recientes primero"),
             ("confianza", "Mayor confianza de la IA")]
    estado = forms.ChoiceField(required=False, choices=ESTADOS)
    lote = forms.CharField(required=False)
    origen = forms.ChoiceField(required=False, choices=[("", "Todos"), ("IA", "IA"), ("MANUAL", "Manual")])
    desde = forms.DateField(required=False, widget=forms.DateInput(attrs={"type": "date"}))
    hasta = forms.DateField(required=False, widget=forms.DateInput(attrs={"type": "date"}))
    orden = forms.ChoiceField(required=False, choices=ORDEN)
```

### D.4 Consultas — `web/queries.py`

`web/queries.py`

```python
# web/queries.py — Consultas de solo lectura de la web (bandeja, mapa, plano, dashboard, exportaciones).
# Reglas: sin N+1 (select_related / agregados en la BD), filtros siempre acotados, nada de escrituras aquí.
from collections import defaultdict
from datetime import timedelta

from django.conf import settings
from django.db.models import Avg, Count, Exists, F, Min, OuterRef, Q
from django.urls import reverse
from django.utils import timezone

from campo.models import FieldLot, FieldRow
from evidencias.models import Capture, QualityStatus
from ia.models import AiStatus, AiTask
from monitoreo.models import Incident, LateralCode, MonitoringPass, MonitoringSession, PassStatus
from notificaciones.models import Notification
from revision.models import Case, ReviewStatus

ORDERING = {"antiguos": ("opened_at",), "recientes": ("-opened_at",), "confianza": (F("max_confidence").desc(nulls_last=True), "opened_at")}
REJECTED_QUALITY = (QualityStatus.REPETIR_NITIDEZ, QualityStatus.REPETIR_EXPOSICION, QualityStatus.ERROR_CAMARA)


def filter_cases(qs, f):
    """f = cleaned_data de FiltroCasosForm (o dict equivalente)."""
    if f.get("estado"):
        qs = qs.filter(status=f["estado"])
    if f.get("lote"):
        qs = qs.filter(lot_id=f["lote"])
    if f.get("origen"):
        qs = qs.filter(origin=f["origen"])
    if f.get("desde"):
        qs = qs.filter(captured_at__date__gte=f["desde"])  # fecha en hora de Lima (TIME_ZONE)
    if f.get("hasta"):
        qs = qs.filter(captured_at__date__lte=f["hasta"])
    return qs


def bandeja(f):
    qs = Case.objects.select_related("lot", "row", "segment", "marker", "capture", "decided_by")
    qs = filter_cases(qs, f)
    return qs.order_by(*ORDERING.get(f.get("orden") or "antiguos", ORDERING["antiguos"]))


def next_pending_case_id(after_case):
    """Siguiente caso pendiente en la bandeja (orden por defecto) para «Guardar y siguiente»."""
    return (Case.objects.filter(status=ReviewStatus.PENDIENTE_REVISION)
            .exclude(pk=after_case.pk).order_by("opened_at").values_list("pk", flat=True).first())


def case_detail(case_id):
    case = (Case.objects.select_related("lot", "row", "segment", "marker", "capture__quality", "capture__device",
                                        "capture__camera_user", "capture__operator_user", "ai_task__model_config",
                                        "decided_by", "session", "monitoring_pass", "sequence")
            .get(pk=case_id))
    detections = list(case.ai_task.detections.all()) if case.ai_task_id else []
    reviews = list(case.reviews.select_related("reviewer").order_by("reviewed_at"))
    notifications = list(case.notifications.order_by("created_at"))
    sibling = (Capture.objects.filter(sequence_id=case.sequence_id).exclude(pk=case.capture_id)
               .exclude(camera_role=case.capture.camera_role).order_by("-captured_at").first())
    return case, detections, reviews, notifications, sibling


def map_geojson(f):
    qs = filter_cases(Case.objects.select_related("lot", "row", "segment", "marker"), f)
    sin_ubicacion = qs.filter(lat__isnull=True).count()
    limit = settings.WEB["MAPA_MAX_FEATURES"]
    rows = list(qs.filter(lat__isnull=False, lon__isnull=False).order_by("-captured_at")[: limit + 1])
    truncated = len(rows) > limit
    features = [{
        "type": "Feature",
        "geometry": {"type": "Point", "coordinates": [c.lon, c.lat]},  # GeoJSON: [lon, lat]
        "properties": {
            "id": str(c.pk), "status": c.status, "statusLabel": c.get_status_display(),
            "lot": c.lot.code, "row": c.row.number, "lateral": c.get_lateral_code_display(),
            "segment": c.segment.code if c.segment_id else None, "marker": c.marker.code if c.marker_id else None,
            "capturedAt": timezone.localtime(c.captured_at).strftime("%d/%m/%Y %H:%M"),
            "locationSource": c.location_source, "accuracyM": c.gps_accuracy_m,
            "url": reverse("web:caso", args=[c.pk]),
        },
    } for c in rows[:limit]]
    lots = [{"type": "Feature", "geometry": lot.geometry, "properties": {"id": lot.pk, "code": lot.code}}
            for lot in FieldLot.objects.filter(active=True, geometry__isnull=False)]
    return {"type": "FeatureCollection", "features": features, "lots": lots,
            "meta": {"sinUbicacion": sin_ubicacion, "truncated": truncated, "limit": limit}}


def covered_rows(desde=None, hasta=None):
    """RN-13 del maestro móvil: hilera cubierta = LATERAL_A y LATERAL_B cerrados (COMPLETED, o INCOMPLETE con
    incidencia), en el periodo. Devuelve el conjunto de row_id."""
    passes = MonitoringPass.objects.annotate(
        con_incidencia=Exists(Incident.objects.filter(monitoring_pass=OuterRef("pk"))))
    passes = passes.filter(Q(status=PassStatus.COMPLETED) | Q(status=PassStatus.INCOMPLETE, con_incidencia=True))
    if desde:
        passes = passes.filter(started_at__date__gte=desde)
    if hasta:
        passes = passes.filter(started_at__date__lte=hasta)
    a = set(passes.filter(lateral_code=LateralCode.LATERAL_A).values_list("row_id", flat=True))
    b = set(passes.filter(lateral_code=LateralCode.LATERAL_B).values_list("row_id", flat=True))
    return a & b


def plano(lot, f):
    """Plano esquemático del lote: hileras → segmentos con conteo de casos por estado (sin coordenadas)."""
    cases = filter_cases(Case.objects.filter(lot=lot), f)
    by_row = defaultdict(lambda: defaultdict(int))
    by_segment = defaultdict(lambda: defaultdict(int))
    for row_id, segment_id, status, n in cases.values_list("row_id", "segment_id", "status").annotate(n=Count("id")):
        by_row[row_id][status] += n
        if segment_id:
            by_segment[segment_id][status] += n
    covered = covered_rows(f.get("desde"), f.get("hasta"))
    rows = []
    for row in FieldRow.objects.filter(lot=lot, active=True).prefetch_related("segments").order_by("number"):
        rows.append({
            "row": row, "covered": row.pk in covered, "counts": dict(by_row.get(row.pk, {})),
            "segments": [{
                "segment": s, "counts": dict(by_segment.get(s.pk, {})),
                # posición relativa dentro de la barra de la hilera (plantas 1..plant_count)
                "left_pct": round(100 * (s.start_plant - 1) / row.plant_count, 2) if row.plant_count else 0,
                "width_pct": round(100 * (s.end_plant - s.start_plant + 1) / row.plant_count, 2) if row.plant_count else 0,
            } for s in row.segments.all()],
        })
    max_plants = max((r["row"].plant_count for r in rows), default=1)
    return {"lot": lot, "rows": rows, "max_plants": max_plants, "covered": sum(1 for r in rows if r["covered"])}


def dashboard(desde, hasta):
    now = timezone.now()
    sessions = MonitoringSession.objects.filter(started_at__date__gte=desde, started_at__date__lte=hasta)
    captures = Capture.objects.filter(captured_at__date__gte=desde, captured_at__date__lte=hasta)
    cap = captures.aggregate(total=Count("pk"), rechazadas=Count("pk", filter=Q(quality_status__in=REJECTED_QUALITY)))
    tasks = AiTask.objects.filter(capture__in=captures)
    ia = {s: 0 for s in AiStatus.values}
    ia.update(dict(tasks.values_list("status").annotate(n=Count("pk"))))
    ia_tiempo = tasks.filter(processing_ms__isnull=False).aggregate(ms=Avg("processing_ms"))["ms"]
    cola = AiTask.objects.filter(status__in=[AiStatus.PENDIENTE_DE_ANALISIS, AiStatus.EN_ANALISIS]).aggregate(
        n=Count("pk"), mas_antigua=Min("requested_at"))
    cases = Case.objects.filter(captured_at__date__gte=desde, captured_at__date__lte=hasta)
    casos = {s: 0 for s in ReviewStatus.values}
    casos.update(dict(cases.values_list("status").annotate(n=Count("pk"))))
    decididos = cases.exclude(decided_at__isnull=True).aggregate(espera=Avg(F("decided_at") - F("opened_at")))
    pendientes = Case.objects.filter(status=ReviewStatus.PENDIENTE_REVISION).aggregate(
        n=Count("pk"), mas_antiguo=Min("opened_at"))
    avisos = dict(Notification.objects.filter(created_at__date__gte=desde, created_at__date__lte=hasta)
                  .values_list("status").annotate(n=Count("pk")))
    covered = covered_rows(desde, hasta)
    cobertura = []
    for lot in FieldLot.objects.filter(active=True).annotate(hileras=Count("rows", filter=Q(rows__active=True))):
        n_cov = FieldRow.objects.filter(lot=lot, pk__in=covered).count() if covered else 0
        confirmados = cases.filter(lot=lot, status=ReviewStatus.CONFIRMADO_POR_ESPECIALISTA).count()
        cobertura.append({"lot": lot, "hileras": lot.hileras, "cubiertas": n_cov, "confirmados": confirmados})
    total = cap["total"] or 0
    return {
        "desde": desde, "hasta": hasta,
        "sesiones": sessions.count(),
        "capturas": total,
        "rechazadas": cap["rechazadas"],
        "pct_rechazadas": round(100 * cap["rechazadas"] / total, 1) if total else None,
        "ia": ia, "ia_ms_promedio": round(ia_tiempo) if ia_tiempo else None,
        "cola": cola["n"], "cola_espera": (now - cola["mas_antigua"]) if cola["mas_antigua"] else None,
        "casos": casos,
        "espera_decision": decididos["espera"],
        "pendientes": pendientes["n"],
        "pendiente_mas_antiguo": (now - pendientes["mas_antiguo"]) if pendientes["mas_antiguo"] else None,
        "avisos": avisos,
        "cobertura": cobertura,
    }


def default_range(days=30):
    hoy = timezone.localdate()
    return hoy - timedelta(days=days - 1), hoy
```

### D.5 Mensajes — `web/messages.py`

`web/messages.py`

```python
# web/messages.py — Textos de la web que no son etiquetas de campos (es-PE). Centralizados para revisarlos juntos.
LOGIN_INVALIDO = "Correo o contraseña incorrectos."
LOGIN_BLOQUEADO = "Demasiados intentos fallidos. Vuelve a intentarlo en {minutos} minutos."
CUENTA_PENDIENTE = "Tu cuenta está pendiente de aprobación del administrador."
CUENTA_RECHAZADA = "Tu solicitud de cuenta fue rechazada. Comunícate con el administrador."
CUENTA_BLOQUEADA = "Tu cuenta está bloqueada. Comunícate con el administrador."
SIN_ACCESO_WEB = "Tu cuenta no tiene acceso a la plataforma web. Usa la app móvil."
CONTRASENA_OBLIGATORIA = "Debes cambiar tu contraseña temporal antes de continuar."
CONTRASENA_CAMBIADA = "Tu contraseña se cambió."

CASO_DECIDIDO = "Decisión registrada: {decision}."
CASO_YA_DECIDIDO = "Este caso ya fue decidido por {quien} el {cuando}. Se muestra la decisión vigente."
CASO_CORREGIDO = "La decisión se corrigió. La anterior queda en el historial."
DECISION_CAMBIO = "La decisión vigente cambió mientras corregías. Revisa la versión actual."
AVISO_ENCOLADO = "Se avisará por WhatsApp a {n} destinatario(s)."
AVISO_SIN_DESTINATARIOS = "No hay destinatarios activos para este lote: no se envió ningún aviso."
CASO_MANUAL_ABIERTO = "Caso abierto para revisión."

TAREA_REENCOLADA = "La tarea volvió a la cola de análisis."
CUENTA_APROBADA = "Cuenta aprobada."
CUENTA_ACTUALIZADA = "Cuenta actualizada."
CONTRASENA_TEMPORAL = ("Contraseña temporal de {nombre}: {clave} — entrégala en persona; no se volverá a mostrar. "
                       "Se cerró su sesión en la app.")
CELULAR_REVOCADO = "Celular revocado: ya no puede usar la API."
DESTINATARIO_GUARDADO = "Destinatario guardado."

IA_AVISO = "Indicio sugerido por IA: no es un diagnóstico. La decisión es del especialista."
BANDEJA_AYUDA = "Cada caso es una foto con un indicio sugerido por la IA o abierta por el especialista."
```

### D.6 Vistas — `web/views.py`

`web/views.py`

```python
# web/views.py — Vistas de la web. Cada vista: decorador @web_view (permiso) → formulario → servicio → plantilla.
import csv

from django.conf import settings

from django.contrib import messages
from django.contrib.auth import login, logout, update_session_auth_hash
from django.contrib.auth.forms import PasswordChangeForm
from django.core.cache import cache
from django.core.exceptions import ValidationError
from django.core.paginator import Paginator
from django.db.models import Count, Q
from django.http import HttpResponse, JsonResponse, StreamingHttpResponse
from django.shortcuts import get_object_or_404, redirect, render
from django.urls import reverse
from django.utils import timezone
from django.utils.http import url_has_allowed_host_and_scheme
from django.views.decorators.gzip import gzip_page
from django.views.decorators.http import require_GET, require_http_methods, require_POST

from auditoria import services as audit
from auditoria.models import AuditEvent
from campo.models import FieldLot
from cuentas import services as cuentas
from cuentas.models import AccountStatus, Device, PasswordResetRequest, Role, User
from evidencias.media import signed_image_url
from evidencias.models import Capture
from ia import services as ia
from ia.models import AiStatus, AiTask, ModelConfig
from monitoreo.models import MonitoringSession
from notificaciones.models import Notification, NotificationRecipient
from revision import services as revision
from revision.models import Case, ReviewStatus
from web import messages as M
from web import queries
from web.forms import ApproveForm, CorrectionForm, DecisionForm, FiltroCasosForm, LoginForm, RecipientForm
from web.permissions import can, web_view


# ------------------------------------------------------------------ cuenta
@require_http_methods(["GET", "POST"])
def ingresar(request):
    if request.user.is_authenticated and request.user.can_use_web:
        return redirect("web:dashboard")
    form = LoginForm(request.POST or None)
    if request.method == "POST" and form.is_valid():
        login(request, form.user, backend="django.contrib.auth.backends.ModelBackend")
        audit.record("user", form.user.pk, "INGRESO_WEB", form.user)
        nxt = request.POST.get("next") or request.GET.get("next")
        if form.user.must_change_password:
            return redirect("web:cambiar_contrasena")
        if nxt and url_has_allowed_host_and_scheme(nxt, allowed_hosts={request.get_host()}, require_https=request.is_secure()):
            return redirect(nxt)
        return redirect("web:dashboard")
    return render(request, "web/login.html", {"form": form, "next": request.GET.get("next", "")})


@require_POST
def salir(request):
    logout(request)
    return redirect("web:login")


@require_http_methods(["GET", "POST"])
def cambiar_contrasena(request):
    if not request.user.is_authenticated:
        return redirect("web:login")
    form = PasswordChangeForm(request.user, request.POST or None)
    if request.method == "POST" and form.is_valid():
        user = form.save()
        user.must_change_password = False
        user.save(update_fields=["must_change_password"])
        update_session_auth_hash(request, user)
        audit.record("user", user.pk, "CONTRASENA_CAMBIADA_WEB", user)
        messages.success(request, M.CONTRASENA_CAMBIADA)
        return redirect("web:dashboard")
    if request.user.must_change_password:
        messages.warning(request, M.CONTRASENA_OBLIGATORIA)
    return render(request, "web/cambiar_contrasena.html", {"form": form})


# ------------------------------------------------------------------ panel
@web_view("dashboard.ver")
def dashboard(request):
    desde, hasta = queries.default_range(30)
    f = FiltroCasosForm(request.GET or None)
    if f.is_valid():
        desde = f.cleaned_data.get("desde") or desde
        hasta = f.cleaned_data.get("hasta") or hasta
    k = cache.get_or_set(f"panel:{desde}:{hasta}", lambda: queries.dashboard(desde, hasta),
                         settings.WEB["DASHBOARD_CACHE_SECONDS"])  # KPIs con hasta 60 s de antigüedad
    return render(request, "web/dashboard.html", {"k": k, "filtro": f})


# ------------------------------------------------------------------ bandeja y casos
@web_view("bandeja.ver")
def bandeja(request):
    data = request.GET.copy()
    if "estado" not in data:
        data["estado"] = ReviewStatus.PENDIENTE_REVISION  # por defecto: lo que falta revisar
    f = FiltroCasosForm(data)
    filtros = f.cleaned_data if f.is_valid() else {"estado": ReviewStatus.PENDIENTE_REVISION}
    page = Paginator(queries.bandeja(filtros), settings.WEB["BANDEJA_PAGE_SIZE"]).get_page(request.GET.get("pagina"))
    for case in page.object_list:
        case.thumb_url = signed_image_url(case.capture, "miniatura")
    ctx = {"page": page, "filtro": f, "lotes": FieldLot.objects.filter(active=True),
           "aviso_ia": M.IA_AVISO, "ayuda": M.BANDEJA_AYUDA}
    template = "web/partials/bandeja_tabla.html" if request.htmx else "web/bandeja.html"
    return render(request, template, ctx)


def _case_context(request, case_id, form=None, correction_form=None):
    case, detections, reviews, notifications, sibling = queries.case_detail(case_id)
    current = next((r for r in reviews if r.is_current), None)
    width = (case.ai_task.image_width if case.ai_task_id and case.ai_task.image_width else case.capture.width)
    height = (case.ai_task.image_height if case.ai_task_id and case.ai_task.image_height else case.capture.height)
    return {
        "case": case, "detections": detections, "reviews": reviews, "current": current,
        "notifications": notifications, "sibling": sibling,
        "img_revision": signed_image_url(case.capture, "revision"),
        "img_original": signed_image_url(case.capture, "original"),
        "sibling_thumb": signed_image_url(sibling, "miniatura") if sibling else None,
        "vb_w": width, "vb_h": height, "label_size": max(12, round(max(width, height) / 45)),
        "form": form or DecisionForm(case=case),
        "correction_form": correction_form or CorrectionForm(case=case, initial={
            "expected_review_id": current.pk if current else None,
            "decision": current.decision if current else None,
            "observation": current.observation if current else "",
            "confirmed_class": current.confirmed_class if current else ""}),
        "aviso_ia": M.IA_AVISO,
    }


@web_view("caso.ver")
def caso(request, pk):
    get_object_or_404(Case, pk=pk)
    return render(request, "web/caso.html", _case_context(request, pk))


@web_view("caso.decidir")
@require_POST
def caso_decidir(request, pk):
    case = get_object_or_404(Case.objects.select_related("ai_task__model_config"), pk=pk)
    form = DecisionForm(request.POST, case=case)
    status = 200
    if form.is_valid():
        d = form.cleaned_data
        try:
            review = revision.decide_case(case.pk, request.user, d["decision"], d["observation"],
                                          d["confirmed_class"], d["rejected_detection_ids"])
        except revision.CaseAlreadyDecided as exc:
            c = exc.case
            messages.warning(request, M.CASO_YA_DECIDIDO.format(
                quien=c.decided_by.full_name, cuando=timezone.localtime(c.decided_at).strftime("%d/%m/%Y %H:%M")))
            status = 409
        except ValidationError as exc:
            _merge_errors(form, exc)
            status = 422
        else:
            messages.success(request, M.CASO_DECIDIDO.format(decision=review.get_decision_display()))
            n = review.notifications.count()
            if review.decision == ReviewStatus.CONFIRMADO_POR_ESPECIALISTA:
                messages.info(request, M.AVISO_ENCOLADO.format(n=n) if n else M.AVISO_SIN_DESTINATARIOS)
            if request.POST.get("siguiente"):
                nxt = queries.next_pending_case_id(case)
                if nxt:
                    return _go(request, reverse("web:caso", args=[nxt]))
                return _go(request, reverse("web:bandeja"))
            return _go(request, reverse("web:caso", args=[case.pk]))
    else:
        status = 422
    ctx = _case_context(request, case.pk, form=form if status == 422 else None)
    template = "web/partials/panel_decision.html" if request.htmx else "web/caso.html"
    return render(request, template, ctx, status=status)


@web_view("caso.corregir")
@require_POST
def caso_corregir(request, pk):
    case = get_object_or_404(Case.objects.select_related("ai_task__model_config"), pk=pk)
    form = CorrectionForm(request.POST, case=case)
    status = 422
    if form.is_valid():
        d = form.cleaned_data
        try:
            revision.correct_decision(case.pk, request.user, d["decision"], d["observation"], d["correction_reason"],
                                      d["expected_review_id"], d["confirmed_class"], d["rejected_detection_ids"])
        except revision.StaleReview:
            messages.warning(request, M.DECISION_CAMBIO)
            status = 409
        except ValidationError as exc:
            _merge_errors(form, exc)
        else:
            messages.success(request, M.CASO_CORREGIDO)
            return _go(request, reverse("web:caso", args=[case.pk]))
    ctx = _case_context(request, case.pk, correction_form=form if status == 422 else None)
    template = "web/partials/panel_decision.html" if request.htmx else "web/caso.html"
    return render(request, template, ctx, status=status)


def _merge_errors(form, exc):
    if hasattr(exc, "error_dict"):
        for field, errs in exc.error_dict.items():
            form.add_error(field if field in form.fields else None, errs)
    else:
        form.add_error(None, exc.messages)


def _go(request, url):
    from django_htmx.http import HttpResponseClientRedirect

    return HttpResponseClientRedirect(url) if request.htmx else redirect(url)


@web_view("caso.ver")
def captura(request, pk):
    capture = get_object_or_404(Capture.objects.select_related(
        "sequence", "monitoring_pass__lot", "monitoring_pass__row", "quality", "device", "camera_user"), pk=pk)
    tasks = list(capture.ai_tasks.select_related("model_config").prefetch_related("detections"))
    case = Case.objects.filter(capture=capture).first()
    return render(request, "web/captura.html", {
        "capture": capture, "tasks": tasks, "case": case,
        "img_revision": signed_image_url(capture, "revision"),
        "puede_abrir": can(request.user, "caso.abrir_manual") and case is None
        and any(t.status in (AiStatus.SIN_INDICIOS_IA, AiStatus.ERROR_DE_ANALISIS) for t in tasks)})


@web_view("caso.abrir_manual")
@require_POST
def captura_abrir_caso(request, pk):
    try:
        case, created = revision.open_manual_case(pk, request.user)
    except ValidationError as exc:
        messages.error(request, " ".join(exc.messages))
        return redirect("web:captura", pk=pk)
    if created:
        messages.success(request, M.CASO_MANUAL_ABIERTO)
    return redirect("web:caso", pk=case.pk)


# ------------------------------------------------------------------ mapa y plano
@web_view("mapa.ver")
def mapa(request):
    desde, _ = queries.default_range(30)  # por defecto, últimos 30 días: el GeoJSON completo pesa (RNF-W01)
    filtro = FiltroCasosForm(request.GET or None, initial={"desde": desde})
    return render(request, "web/mapa.html", {"filtro": filtro, "lotes": FieldLot.objects.filter(active=True)})


@web_view("mapa.ver")
@require_GET
@gzip_page  # JSON sin secretos: comprimirlo no expone tokens (a diferencia de páginas con CSRF)
def mapa_datos(request):
    f = FiltroCasosForm(request.GET)
    return JsonResponse(queries.map_geojson(f.cleaned_data if f.is_valid() else {}))


@web_view("mapa.ver")
def plano(request):
    lotes = FieldLot.objects.filter(active=True)
    lot = get_object_or_404(FieldLot, pk=request.GET.get("lote")) if request.GET.get("lote") else lotes.first()
    f = FiltroCasosForm(request.GET or None)
    data = queries.plano(lot, f.cleaned_data if f.is_valid() else {}) if lot else None
    return render(request, "web/plano.html", {"p": data, "lotes": lotes, "filtro": f})


# ------------------------------------------------------------------ sesiones
@web_view("sesiones.ver")
def sesiones(request):
    qs = (MonitoringSession.objects.select_related("operator", "controller_device")
          .annotate(n_pasadas=Count("passes", distinct=True), n_capturas=Count("captures", distinct=True))
          .order_by("-started_at"))
    page = Paginator(qs, 25).get_page(request.GET.get("pagina"))
    return render(request, "web/sesiones.html", {"page": page})


@web_view("sesiones.ver")
def sesion(request, pk):
    s = get_object_or_404(MonitoringSession.objects.select_related("operator", "controller_device"), pk=pk)
    passes = s.passes.select_related("lot", "row").order_by("started_at")
    captures = (s.captures.select_related("monitoring_pass__row", "quality").order_by("captured_at")
                .annotate(n_cases=Count("case")))
    quality = dict(s.captures.values_list("quality_status").annotate(n=Count("pk")))
    return render(request, "web/sesion.html", {
        "s": s, "passes": passes, "cameras": s.cameras.select_related("user"), "incidents": s.incidents.all(),
        "captures": captures[:500], "quality": quality})


# ------------------------------------------------------------------ reportes
@web_view("reportes.exportar")
def reportes(request):
    return render(request, "web/reportes.html", {"filtro": FiltroCasosForm(request.GET or None),
                                                 "lotes": FieldLot.objects.filter(active=True)})


class _Echo:
    def write(self, value):
        return value


def _csv_safe(value):
    """Evita inyección de fórmulas al abrir el CSV en Excel (celdas que empiezan con = + - @)."""
    text = "" if value is None else str(value)
    return "'" + text if text[:1] in ("=", "+", "-", "@", "\t", "\r") else text


@web_view("reportes.exportar")
@require_GET
def exportar_casos(request):
    f = FiltroCasosForm(request.GET)
    qs = queries.filter_cases(queries.bandeja({}), f.cleaned_data if f.is_valid() else {})
    qs = qs.select_related("ai_task").order_by("captured_at")[: settings.WEB["EXPORT_MAX_ROWS"]]
    header = ["caso", "estado", "origen", "lote", "hilera", "lateral", "segmento", "marcador", "lat", "lon",
              "fuente_ubicacion", "capturada", "abierto", "decidido", "decidido_por", "cajas", "confianza_max",
              "modelo", "estado_aviso", "captura"]

    def rows():
        yield header
        for c in qs:
            yield [c.pk, c.status, c.origin, c.lot.code, c.row.number, c.lateral_code,
                   c.segment.code if c.segment_id else "", c.marker.code if c.marker_id else "", c.lat, c.lon,
                   c.location_source, timezone.localtime(c.captured_at).isoformat(),
                   timezone.localtime(c.opened_at).isoformat(),
                   timezone.localtime(c.decided_at).isoformat() if c.decided_at else "",
                   c.decided_by.full_name if c.decided_by_id else "", c.detections_count,
                   f"{c.max_confidence:.3f}" if c.max_confidence is not None else "",
                   c.ai_task.model_version if c.ai_task_id else "", c.notification_status, c.capture_id]

    writer = csv.writer(_Echo())

    def stream():
        yield "\ufeff"  # BOM: Excel abre el archivo como UTF-8 (tildes y ñ)
        for row in rows():
            yield writer.writerow([_csv_safe(v) for v in row])

    response = StreamingHttpResponse(stream(), content_type="text/csv; charset=utf-8")
    name = f"casos_{timezone.localdate():%Y%m%d}.csv"
    response["Content-Disposition"] = f'attachment; filename="{name}"'
    audit.record("export", name, "EXPORTACION_CASOS", request.user, None, dict(request.GET.items()))
    return response


# ------------------------------------------------------------------ notificaciones
@web_view("notificaciones.ver")
def notificaciones(request):
    qs = Notification.objects.select_related("case__lot", "case__row").order_by("-created_at")
    estado = request.GET.get("estado")
    if estado:
        qs = qs.filter(status=estado)
    return render(request, "web/notificaciones.html", {"page": Paginator(qs, 50).get_page(request.GET.get("pagina")),
                                                       "estado": estado})


@web_view("destinatarios.gestionar")
@require_http_methods(["GET", "POST"])
def destinatarios(request, pk=None):
    instance = get_object_or_404(NotificationRecipient, pk=pk) if pk else None
    form = RecipientForm(request.POST or None, instance=instance)
    if request.method == "POST" and form.is_valid():
        before = None if instance is None else {"active": instance.active, "phone": instance.phone_e164}
        r = form.save(commit=False)
        if form.cleaned_data["opt_in"] and r.opt_in_at is None:
            r.opt_in_at = timezone.now()
        if not form.cleaned_data["opt_in"]:
            r.opt_in_at = None
        r.save()
        form.save_m2m()
        audit.record("notification_recipient", r.pk, "DESTINATARIO_GUARDADO", request.user, before,
                     {"active": r.active, "phone": r.phone_e164, "optIn": r.opt_in_at is not None})
        messages.success(request, M.DESTINATARIO_GUARDADO)
        return redirect("web:destinatarios")
    lista = NotificationRecipient.objects.prefetch_related("lots").select_related("user")
    return render(request, "web/destinatarios.html", {"form": form, "lista": lista, "editando": instance})


# ------------------------------------------------------------------ administración
@web_view("usuarios.gestionar")
def usuarios(request):
    pendientes = User.objects.filter(status=AccountStatus.PENDIENTE_APROBACION).order_by("created_at")
    todos = User.objects.prefetch_related("user_roles").order_by("full_name")
    solicitudes = PasswordResetRequest.objects.filter(status=PasswordResetRequest.Status.PENDIENTE)
    return render(request, "web/usuarios.html", {"pendientes": pendientes, "todos": todos,
                                                 "solicitudes": solicitudes, "approve_form": ApproveForm(),
                                                 "roles_choices": Role.choices})


@web_view("usuarios.gestionar")
@require_POST
def usuario_accion(request, pk, accion):
    try:
        if accion == "aprobar":
            form = ApproveForm(request.POST)
            if not form.is_valid():
                raise ValidationError("Elige al menos un rol.")
            cuentas.approve_user(pk, request.user, form.cleaned_data["roles"])
            messages.success(request, M.CUENTA_APROBADA)
        elif accion == "roles":
            form = ApproveForm(request.POST)
            if not form.is_valid():
                raise ValidationError("Elige al menos un rol.")
            cuentas.set_roles(pk, request.user, form.cleaned_data["roles"])
            messages.success(request, M.CUENTA_ACTUALIZADA)
        elif accion in ("rechazar", "bloquear", "desbloquear"):
            nuevo = {"rechazar": AccountStatus.RECHAZADO, "bloquear": AccountStatus.BLOQUEADO,
                     "desbloquear": AccountStatus.ACTIVO}[accion]
            cuentas.change_status(pk, request.user, nuevo)
            messages.success(request, M.CUENTA_ACTUALIZADA)
        elif accion == "contrasena-temporal":
            user = get_object_or_404(User, pk=pk)
            clave = cuentas.set_temporary_password(pk, request.user)
            messages.warning(request, M.CONTRASENA_TEMPORAL.format(nombre=user.full_name, clave=clave))
        else:
            return HttpResponse(status=404)
    except ValidationError as exc:
        messages.error(request, " ".join(exc.messages))
    return redirect("web:usuarios")


@web_view("dispositivos.gestionar")
def dispositivos(request):
    lista = Device.objects.select_related("user", "revoked_by").order_by("-last_seen_at")
    return render(request, "web/dispositivos.html", {"lista": lista})


@web_view("dispositivos.gestionar")
@require_POST
def dispositivo_revocar(request, pk):
    cuentas.revoke_device(pk, request.user)
    messages.success(request, M.CELULAR_REVOCADO)
    return redirect("web:dispositivos")


@web_view("ia.ver")
def ia_estado(request):
    conteo = dict(AiTask.objects.values_list("status").annotate(n=Count("pk")))
    errores = (AiTask.objects.filter(status=AiStatus.ERROR_DE_ANALISIS).select_related("capture", "model_config")
               .order_by("-finished_at")[:100])
    return render(request, "web/ia.html", {"conteo": conteo, "errores": errores,
                                           "modelo": ModelConfig.objects.filter(active=True).first(),
                                           "estados": AiStatus.choices})


@web_view("ia.reencolar")
@require_POST
def ia_reencolar(request, pk):
    try:
        ia.requeue_failed(pk, request.user)
        messages.success(request, M.TAREA_REENCOLADA)
    except ia.InvalidTaskState as exc:
        messages.error(request, str(exc))
    return redirect("web:ia")


@web_view("auditoria.ver")
def auditoria(request):
    qs = AuditEvent.objects.select_related("user")
    q = request.GET.get("q", "").strip()
    if q:
        qs = qs.filter(Q(entity_id__icontains=q) | Q(action__icontains=q))
    return render(request, "web/auditoria.html", {"page": Paginator(qs, 50).get_page(request.GET.get("pagina")),
                                                  "q": q})
```

### D.7 Rutas — `web/urls.py`

`web/urls.py`

```python
# web/urls.py — Rutas de la web (sección 6.3). Prefijo raíz; la API vive en /api/v1/ y Django Admin en /gestion/.
from django.urls import path

from web import views

app_name = "web"

urlpatterns = [
    path("ingresar/", views.ingresar, name="login"),
    path("salir/", views.salir, name="logout"),
    path("cuenta/contrasena/", views.cambiar_contrasena, name="cambiar_contrasena"),
    path("", views.dashboard, name="dashboard"),
    path("casos/", views.bandeja, name="bandeja"),
    path("casos/<uuid:pk>/", views.caso, name="caso"),
    path("casos/<uuid:pk>/decidir/", views.caso_decidir, name="caso_decidir"),
    path("casos/<uuid:pk>/corregir/", views.caso_corregir, name="caso_corregir"),
    path("capturas/<uuid:pk>/", views.captura, name="captura"),
    path("capturas/<uuid:pk>/abrir-caso/", views.captura_abrir_caso, name="captura_abrir_caso"),
    path("mapa/", views.mapa, name="mapa"),
    path("mapa/datos/", views.mapa_datos, name="mapa_datos"),
    path("plano/", views.plano, name="plano"),
    path("sesiones/", views.sesiones, name="sesiones"),
    path("sesiones/<uuid:pk>/", views.sesion, name="sesion"),
    path("reportes/", views.reportes, name="reportes"),
    path("reportes/casos.csv", views.exportar_casos, name="exportar_casos"),
    path("notificaciones/", views.notificaciones, name="notificaciones"),
    path("administracion/destinatarios/", views.destinatarios, name="destinatarios"),
    path("administracion/destinatarios/<int:pk>/", views.destinatarios, name="destinatario_editar"),
    path("administracion/usuarios/", views.usuarios, name="usuarios"),
    path("administracion/usuarios/<uuid:pk>/<slug:accion>/", views.usuario_accion, name="usuario_accion"),
    path("administracion/dispositivos/", views.dispositivos, name="dispositivos"),
    path("administracion/dispositivos/<uuid:pk>/revocar/", views.dispositivo_revocar, name="dispositivo_revocar"),
    path("ia/", views.ia_estado, name="ia"),
    path("ia/tareas/<uuid:pk>/reencolar/", views.ia_reencolar, name="ia_reencolar"),
    path("auditoria/", views.auditoria, name="auditoria"),
]
```

### D.8 Etiquetas de plantilla — `web/templatetags/web_tags.py`

`web/templatetags/web_tags.py`

```python
# web/templatetags/web_tags.py — Insignias de estado (color + icono + texto; nunca solo color: RNF-W06).
from django import template
from django.utils.html import format_html

register = template.Library()

# estado → (clase CSS, icono). Los colores de los casos indican ESTADO DE REVISIÓN, nunca gravedad (W-12).
BADGES = {
    # revisión del caso
    "PENDIENTE_REVISION": ("b-pendiente", "◷"),
    "CONFIRMADO_POR_ESPECIALISTA": ("b-confirmado", "✔"),
    "DESCARTADO": ("b-descartado", "✕"),
    "EVIDENCIA_INSUFICIENTE": ("b-insuficiente", "?"),
    # análisis de IA
    "PENDIENTE_DE_ANALISIS": ("b-neutro", "◷"),
    "EN_ANALISIS": ("b-proceso", "⟳"),
    "INDICIO_SUGERIDO_POR_IA": ("b-indicio", "◆"),
    "SIN_INDICIOS_IA": ("b-neutro", "○"),
    "ERROR_DE_ANALISIS": ("b-error", "⚠"),
    # avisos
    "NO_APLICA": ("b-neutro", "–"),
    "PENDIENTE_ENVIO": ("b-pendiente", "◷"),
    "ENVIADO": ("b-ok", "✔"),
    "ERROR_ENVIO": ("b-error", "⚠"),
    # cuentas, sesiones, pasadas, calidad
    "ACTIVO": ("b-ok", "●"), "PENDIENTE_APROBACION": ("b-pendiente", "◷"),
    "RECHAZADO": ("b-descartado", "✕"), "BLOQUEADO": ("b-error", "⛔"),
    "CLOSED": ("b-ok", "✔"), "COMPLETED": ("b-ok", "✔"), "INCOMPLETE": ("b-pendiente", "◐"),
    "UTILIZABLE": ("b-ok", "✔"), "PENDIENTE_REVISION_TECNICA": ("b-pendiente", "◷"),
    "REPETIR_NITIDEZ": ("b-descartado", "↺"), "REPETIR_EXPOSICION": ("b-descartado", "↺"),
    "ERROR_CAMARA": ("b-error", "⚠"),
}


@register.simple_tag
def badge(value, label=None):
    css, icon = BADGES.get(str(value), ("b-neutro", "•"))
    return format_html('<span class="badge {}"><span aria-hidden="true">{}</span> {}</span>', css, icon,
                       label or str(value))


@register.filter
def duracion(td):
    """timedelta → «3 d 4 h», «2 h 15 min» o «8 min»."""
    if td is None:
        return "—"
    minutes = int(td.total_seconds() // 60)
    days, rem = divmod(minutes, 1440)
    hours, mins = divmod(rem, 60)
    if days:
        return f"{days} d {hours} h"
    if hours:
        return f"{hours} h {mins} min"
    return f"{mins} min"


@register.filter
def pct(part, total):
    try:
        return f"{100 * part / total:.0f} %" if total else "—"
    except (TypeError, ZeroDivisionError):
        return "—"


@register.filter
def get(d, key):
    return (d or {}).get(key, 0)


@register.simple_tag
def bar_width(value, maximum, full=100):
    """Ancho relativo (0–full) para dibujar hileras proporcionales a su número de plantas en el plano."""
    try:
        return round(full * value / maximum, 2) if maximum else 0
    except (TypeError, ZeroDivisionError):
        return 0
```

---

## Anexo E. Plantillas y archivos estáticos

HTMX (`htmx.min.js`) y Leaflet (`leaflet/`) no se transcriben: se copian del paquete npm indicado en E.14.

### E.1 Base — `web/templates/web/base.html` y fragmentos comunes

`web/templates/web/base.html`

```html
{% load static %}<!doctype html>
<html lang="es-PE">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>{% block titulo %}Monitoreo fitosanitario{% endblock %} · Agrícola Riachuelo</title>
  <link rel="stylesheet" href="{% static 'web/app.css' %}">
  {# htmx 2 no reemplaza respuestas 4xx por defecto: 403/409/422 traen un fragmento con el motivo (6.4, P-4). #}
  <meta name="htmx-config" content='{"responseHandling":[{"code":"204","swap":false},{"code":"[23]..","swap":true},{"code":"403","swap":true,"error":false},{"code":"409","swap":true,"error":false},{"code":"422","swap":true,"error":false},{"code":"[45]..","swap":false,"error":true}]}'>
  {% block head %}{% endblock %}
  <script src="{% static 'web/vendor/htmx.min.js' %}" defer></script>
</head>
<body hx-headers='{"X-CSRFToken": "{{ csrf_token }}"}'>
{% if user.is_authenticated %}
<div class="layout">
  <nav class="lateral" aria-label="Menú principal">
    <div class="marca">Riachuelo · Monitoreo</div>
    {% with n=request.resolver_match.url_name %}
    <div class="grupo">Plataforma</div>
    {% if puede.dashboard_ver %}<a href="{% url 'web:dashboard' %}" {% if n == 'dashboard' %}aria-current="page"{% endif %}><span class="ico">▦</span><span class="txt">Panel</span></a>{% endif %}
    {% if puede.bandeja_ver %}<a href="{% url 'web:bandeja' %}" {% if n == 'bandeja' or n == 'caso' %}aria-current="page"{% endif %}><span class="ico">☑</span><span class="txt">Casos</span></a>{% endif %}
    {% if puede.mapa_ver %}<a href="{% url 'web:mapa' %}" {% if n == 'mapa' %}aria-current="page"{% endif %}><span class="ico">⌖</span><span class="txt">Mapa</span></a>
    <a href="{% url 'web:plano' %}" {% if n == 'plano' %}aria-current="page"{% endif %}><span class="ico">▤</span><span class="txt">Plano de hileras</span></a>{% endif %}
    {% if puede.sesiones_ver %}<a href="{% url 'web:sesiones' %}" {% if n == 'sesiones' or n == 'sesion' %}aria-current="page"{% endif %}><span class="ico">⇄</span><span class="txt">Sesiones</span></a>{% endif %}
    {% if puede.reportes_exportar %}<a href="{% url 'web:reportes' %}" {% if n == 'reportes' %}aria-current="page"{% endif %}><span class="ico">⎙</span><span class="txt">Reportes</span></a>{% endif %}
    {% if puede.notificaciones_ver %}<a href="{% url 'web:notificaciones' %}" {% if n == 'notificaciones' %}aria-current="page"{% endif %}><span class="ico">✉</span><span class="txt">Avisos</span></a>{% endif %}
    {% if puede.ia_ver %}<a href="{% url 'web:ia' %}" {% if n == 'ia' %}aria-current="page"{% endif %}><span class="ico">◎</span><span class="txt">Estado de la IA</span></a>{% endif %}
    {% if puede.usuarios_gestionar %}
    <div class="grupo">Administración</div>
    <a href="{% url 'web:usuarios' %}" {% if n == 'usuarios' %}aria-current="page"{% endif %}><span class="ico">☺</span><span class="txt">Usuarios</span></a>
    <a href="{% url 'web:dispositivos' %}" {% if n == 'dispositivos' %}aria-current="page"{% endif %}><span class="ico">▯</span><span class="txt">Celulares</span></a>
    <a href="{% url 'web:destinatarios' %}" {% if n == 'destinatarios' or n == 'destinatario_editar' %}aria-current="page"{% endif %}><span class="ico">☏</span><span class="txt">Destinatarios WhatsApp</span></a>
    <a href="{% url 'web:auditoria' %}" {% if n == 'auditoria' %}aria-current="page"{% endif %}><span class="ico">⌛</span><span class="txt">Auditoría</span></a>
    {% if user.is_superuser %}<a href="{% url 'admin:index' %}"><span class="ico">⚙</span><span class="txt">Gestión (Django Admin)</span></a>{% endif %}
    {% endif %}
    {% endwith %}
  </nav>
  <div class="principal">
    <header class="cabecera">
      <div>{% block migas %}{% endblock %}</div>
      <div class="usuario">
        {% if APP_ENV != 'piloto' %}<span class="entorno">Entorno: {{ APP_ENV }}</span>{% endif %}
        <span class="nombre">{{ user.full_name }} · {{ user.role_labels|join:", " }}</span>
        <a href="{% url 'web:cambiar_contrasena' %}">Contraseña</a>
        <form method="post" action="{% url 'web:logout' %}">{% csrf_token %}<button class="btn btn-secundario" type="submit">Salir</button></form>
      </div>
    </header>
    <main class="contenido" id="contenido">
      {% include "web/partials/mensajes.html" %}
      {% block contenido %}{% endblock %}
    </main>
  </div>
</div>
{% else %}
<main class="contenido" style="max-width:420px;margin:8vh auto;">{% include "web/partials/mensajes.html" %}{% block publico %}{% endblock %}</main>
{% endif %}
{% block scripts %}{% endblock %}
</body>
</html>
```

`web/templates/web/partials/mensajes.html`

```html
{% if messages %}<ul class="mensajes" role="status">{% for m in messages %}<li class="{{ m.tags }}">{{ m }}</li>{% endfor %}</ul>{% endif %}
```

`web/templates/web/partials/error.html`

```html
<div class="mensajes"><li class="error" role="alert">{{ mensaje }}</li></div>
```

`web/templates/web/403.html`

```html
{% extends "web/base.html" %}
{% block titulo %}Sin permiso{% endblock %}
{% block contenido %}<h1>Sin permiso</h1><p>{{ mensaje }}</p><p><a href="{% url 'web:dashboard' %}">Volver al panel</a></p>{% endblock %}
{% block publico %}<h1>Sin permiso</h1><p>{{ mensaje }}</p><p><a href="{% url 'web:login' %}">Ingresar</a></p>{% endblock %}
```

### E.2 Ingreso y contraseña

`web/templates/web/login.html`

```html
{% extends "web/base.html" %}
{% block titulo %}Ingresar{% endblock %}
{% block publico %}
<div class="tarjeta">
  <h1>Monitoreo fitosanitario</h1>
  <p style="color:var(--texto-2)">Agrícola Riachuelo · Plataforma web para especialistas, supervisores y administración.</p>
  <form method="post" novalidate>
    {% csrf_token %}
    <input type="hidden" name="next" value="{{ next }}">
    {% if form.non_field_errors %}<ul class="errorlist" role="alert">{% for e in form.non_field_errors %}<li>{{ e }}</li>{% endfor %}</ul>{% endif %}
    <p><label for="id_email">Correo</label><br>{{ form.email }}</p>
    <p><label for="id_password">Contraseña</label><br>{{ form.password }}</p>
    <button class="btn btn-primario" type="submit">Ingresar</button>
  </form>
  <p style="font-size:.85rem;color:var(--texto-2)">¿Olvidaste tu contraseña? Pide una contraseña temporal al administrador.</p>
</div>
{% endblock %}
```

`web/templates/web/cambiar_contrasena.html`

```html
{% extends "web/base.html" %}
{% block titulo %}Cambiar contraseña{% endblock %}
{% block contenido %}
<h1>Cambiar contraseña</h1>
<form method="post" class="tarjeta" style="max-width:460px">{% csrf_token %}{{ form.as_p }}<button class="btn btn-primario">Guardar</button></form>
{% endblock %}
```

### E.3 Panel — `dashboard.html`

`web/templates/web/dashboard.html`

```html
{% extends "web/base.html" %}{% load web_tags %}
{% block titulo %}Panel{% endblock %}
{% block contenido %}
<h1>Panel del fundo</h1>
<form class="filtros no-imprimir" method="get">
  <label>Desde {{ filtro.desde }}</label><label>Hasta {{ filtro.hasta }}</label>
  <button class="btn btn-secundario">Aplicar</button>
  <span style="color:var(--texto-2)">Periodo: {{ k.desde|date:"d/m/Y" }} – {{ k.hasta|date:"d/m/Y" }}</span>
</form>
<div class="kpis">
  <a class="tarjeta kpi" href="{% url 'web:bandeja' %}"><div class="valor">{{ k.pendientes }}</div><div class="etiqueta">Casos pendientes de revisión (total)</div><div class="etiqueta">Más antiguo: {{ k.pendiente_mas_antiguo|duracion }}</div></a>
  <div class="tarjeta kpi"><div class="valor">{{ k.casos.CONFIRMADO_POR_ESPECIALISTA }}</div><div class="etiqueta">Confirmados por especialista</div></div>
  <div class="tarjeta kpi"><div class="valor">{{ k.casos.DESCARTADO }}</div><div class="etiqueta">Descartados</div></div>
  <div class="tarjeta kpi"><div class="valor">{{ k.casos.EVIDENCIA_INSUFICIENTE }}</div><div class="etiqueta">Evidencia insuficiente</div></div>
  <div class="tarjeta kpi"><div class="valor">{{ k.espera_decision|duracion }}</div><div class="etiqueta">Tiempo medio hasta la decisión</div></div>
  <div class="tarjeta kpi"><div class="valor">{{ k.sesiones }}</div><div class="etiqueta">Sesiones de monitoreo</div></div>
  <div class="tarjeta kpi"><div class="valor">{{ k.capturas }}</div><div class="etiqueta">Fotos recibidas</div><div class="etiqueta">Rechazadas por calidad: {{ k.rechazadas }}{% if k.pct_rechazadas is not None %} ({{ k.pct_rechazadas }} %){% endif %}</div></div>
  <div class="tarjeta kpi"><div class="valor">{{ k.cola }}</div><div class="etiqueta">Fotos en cola de análisis</div><div class="etiqueta">Espera máx.: {{ k.cola_espera|duracion }} · {% if k.ia_ms_promedio %}{{ k.ia_ms_promedio }} ms/foto{% else %}—{% endif %}</div></div>
  <div class="tarjeta kpi"><div class="valor">{{ k.ia.ERROR_DE_ANALISIS }}</div><div class="etiqueta">Errores de análisis</div></div>
  <div class="tarjeta kpi"><div class="valor">{{ k.avisos|get:"ENVIADO" }}</div><div class="etiqueta">Avisos enviados · con error: {{ k.avisos|get:"ERROR_ENVIO" }}</div></div>
</div>
<h2>Cobertura y casos confirmados por lote</h2>
<table class="tabla">
  <thead><tr><th>Lote</th><th>Hileras cubiertas (A y B)</th><th>Avance</th><th>Casos confirmados</th><th></th></tr></thead>
  <tbody>{% for c in k.cobertura %}
  <tr><td data-label="Lote">{{ c.lot.code }} · {{ c.lot.name }}</td><td data-label="Cubiertas">{{ c.cubiertas }} / {{ c.hileras }}</td>
      <td data-label="Avance">{{ c.cubiertas|pct:c.hileras }}</td><td data-label="Confirmados">{{ c.confirmados }}</td>
      <td><a href="{% url 'web:plano' %}?lote={{ c.lot.pk }}">Ver plano</a></td></tr>
  {% empty %}<tr><td colspan="5">Sin lotes activos.</td></tr>{% endfor %}</tbody>
</table>
<p style="color:var(--texto-2);font-size:.85rem">Hilera cubierta = LATERAL_A y LATERAL_B cerrados en el periodo (RN-13). «Confirmado» es la decisión del especialista; los colores y conteos no expresan gravedad.</p>
{% endblock %}
```

### E.4 Bandeja — `bandeja.html` y `partials/bandeja_tabla.html`

`web/templates/web/bandeja.html`

```html
{% extends "web/base.html" %}
{% block titulo %}Casos{% endblock %}
{% block contenido %}
<h1>Bandeja de casos</h1>
<p class="aviso-ia">{{ aviso_ia }} {{ ayuda }}</p>
<form class="filtros" method="get" hx-get="{% url 'web:bandeja' %}" hx-target="#bandeja" hx-swap="outerHTML" hx-push-url="true" hx-trigger="change, submit">
  <label>Estado {{ filtro.estado }}</label>
  <label>Lote <select name="lote"><option value="">Todos</option>{% for l in lotes %}<option value="{{ l.pk }}" {% if filtro.lote.value == l.pk %}selected{% endif %}>{{ l.code }}</option>{% endfor %}</select></label>
  <label>Origen {{ filtro.origen }}</label>
  <label>Desde {{ filtro.desde }}</label><label>Hasta {{ filtro.hasta }}</label>
  <label>Orden {{ filtro.orden }}</label>
  <noscript><button class="btn btn-secundario">Filtrar</button></noscript>
</form>
{% include "web/partials/bandeja_tabla.html" %}
{% endblock %}
```

`web/templates/web/partials/bandeja_tabla.html`

```html
{% load web_tags %}
{# Se reemplaza a sí mismo: el sondeo usa siempre los filtros y la página vigentes (6.4, patrón P-2). #}
<div id="bandeja" hx-get="{{ request.get_full_path }}" hx-swap="outerHTML"
     hx-trigger="every {{ WEB.BANDEJA_POLL_SECONDS }}s [document.visibilityState === 'visible']">
<table class="tabla">
  <thead><tr><th>Foto</th><th>Estado</th><th>Ubicación</th><th>Capturada</th><th>IA</th><th>Abierto hace</th><th>Aviso</th></tr></thead>
  <tbody>
  {% for c in page.object_list %}
  <tr>
    <td data-label="Foto"><a href="{% url 'web:caso' c.pk %}"><img class="miniatura" src="{{ c.thumb_url }}" alt="Foto del caso {{ c.pk|stringformat:'s'|slice:':8' }}" loading="lazy"></a></td>
    <td data-label="Estado">{% badge c.status c.get_status_display %}{% if c.origin == 'MANUAL' %} <small>(manual)</small>{% endif %}</td>
    <td data-label="Ubicación"><a href="{% url 'web:caso' c.pk %}">{{ c.lot.code }} · H{{ c.row.number }} · {{ c.get_lateral_code_display }}</a><br><small>{% if c.segment %}Seg. {{ c.segment.code }}{% endif %}{% if c.marker %} · Marc. {{ c.marker.code }}{% endif %}</small></td>
    <td data-label="Capturada">{{ c.captured_at|date:"d/m/Y H:i" }}</td>
    <td data-label="IA">{% if c.detections_count %}{{ c.detections_count }} caja{{ c.detections_count|pluralize }} · máx. {{ c.max_confidence|floatformat:2 }}{% else %}—{% endif %}</td>
    <td data-label="Abierto hace">{{ c.opened_at|timesince }}</td>
    <td data-label="Aviso">{% badge c.notification_status c.get_notification_status_display %}</td>
  </tr>
  {% empty %}
  <tr><td colspan="7">No hay casos con estos filtros.</td></tr>
  {% endfor %}
  </tbody>
</table>
<div class="paginacion">
  {% if page.has_previous %}{% url 'web:bandeja' as base %}<a href="{{ base }}{% querystring pagina=page.previous_page_number %}" hx-get="{{ base }}{% querystring pagina=page.previous_page_number %}" hx-target="#bandeja" hx-swap="outerHTML" hx-push-url="true">← Anterior</a>{% endif %}
  <span>Página {{ page.number }} de {{ page.paginator.num_pages }} · {{ page.paginator.count }} caso{{ page.paginator.count|pluralize }}</span>
  {% if page.has_next %}{% url 'web:bandeja' as base %}<a href="{{ base }}{% querystring pagina=page.next_page_number %}" hx-get="{{ base }}{% querystring pagina=page.next_page_number %}" hx-target="#bandeja" hx-swap="outerHTML" hx-push-url="true">Siguiente →</a>{% endif %}
</div>
</div>
```

### E.5 Caso — `caso.html` y `partials/panel_decision.html`

`web/templates/web/caso.html`

```html
{% extends "web/base.html" %}{% load static web_tags %}
{% block titulo %}Caso {{ case.pk|stringformat:"s"|slice:":8" }}{% endblock %}
{% block migas %}<a href="{% url 'web:bandeja' %}">Casos</a> › {{ case.lot.code }} · H{{ case.row.number }} · {{ case.get_lateral_code_display }}{% endblock %}
{% block contenido %}
<h1>Caso {{ case.pk|stringformat:"s"|slice:":8" }} {% badge case.status case.get_status_display %}</h1>
{% if case.origin == 'IA' %}<p class="aviso-ia">{{ aviso_ia }}</p>{% endif %}
<div class="caso">
  <section aria-label="Evidencia fotográfica">
    <div class="visor" id="visor" data-original="{{ img_original }}">
      <div class="controles no-imprimir">
        <button type="button" data-accion="cajas" title="Mostrar u ocultar cajas (B)">Cajas</button>
        <button type="button" data-accion="mas" title="Acercar (+)">＋</button>
        <button type="button" data-accion="menos" title="Alejar (−)">－</button>
        <button type="button" data-accion="reiniciar" title="Ajustar (0)">⤢</button>
        <button type="button" data-accion="original" title="Cargar la foto original a resolución completa">Original</button>
      </div>
      <div class="lienzo">
        <img src="{{ img_revision }}" alt="Foto de {{ case.capture.get_camera_role_display }} — {{ case.lot.code }} hilera {{ case.row.number }}" width="{{ vb_w }}" height="{{ vb_h }}">
        <svg viewBox="0 0 {{ vb_w }} {{ vb_h }}" preserveAspectRatio="none" aria-hidden="true">
          {% for d in detections %}
          <rect class="caja {% if d.review_status == 'DESCARTADO' %}rechazada{% endif %}" x="{{ d.x_min|stringformat:'.1f' }}" y="{{ d.y_min|stringformat:'.1f' }}" width="{{ d.box_width|stringformat:'.1f' }}" height="{{ d.box_height|stringformat:'.1f' }}"></rect>
          <text x="{{ d.x_min|stringformat:'.1f' }}" y="{{ d.y_min|stringformat:'.1f' }}" dy="-0.3em" style="font-size: {{ label_size }}px">{{ forloop.counter }} · {{ d.confidence|floatformat:2 }}</text>
          {% endfor %}
        </svg>
      </div>
    </div>
    <p style="font-size:.85rem;color:var(--texto-2)">Cajas en coordenadas de la imagen analizada ({{ vb_w }}×{{ vb_h }} px). Rueda o botones para acercar; arrastra para mover.</p>
    {% if detections %}
    <table class="tabla"><thead><tr><th>Caja</th><th>Clase sugerida</th><th>Confianza</th><th>Revisión</th></tr></thead><tbody>
    {% for d in detections %}<tr><td data-label="Caja">{{ forloop.counter }}</td><td data-label="Clase">{{ d.class_name }}</td><td data-label="Confianza">{{ d.confidence|floatformat:2 }}</td><td data-label="Revisión">{% badge d.review_status d.get_review_status_display %}</td></tr>{% endfor %}
    </tbody></table>
    {% endif %}
  </section>
  <aside>
    <div class="tarjeta ficha">
      <h2 style="margin-top:0">Ubicación operativa</h2>
      <dl>
        <dt>Lote</dt><dd>{{ case.lot.code }} · {{ case.lot.name }}</dd>
        <dt>Hilera</dt><dd>{{ case.row.number }} ({{ case.row.plant_count }} plantas)</dd>
        <dt>Lateral</dt><dd>{{ case.get_lateral_code_display }}</dd>
        <dt>Segmento</dt><dd>{% if case.segment %}{{ case.segment.code }} (plantas {{ case.segment.start_plant }}–{{ case.segment.end_plant }}){% else %}—{% endif %}</dd>
        <dt>Marcador</dt><dd>{{ case.marker.code|default:"—" }}</dd>
        <dt>Coordenadas</dt><dd>{% if case.lat is not None %}{{ case.lat|floatformat:6 }}, {{ case.lon|floatformat:6 }}{% if case.gps_accuracy_m %} (± {{ case.gps_accuracy_m|floatformat:0 }} m){% endif %}<br><small>{{ case.get_location_source_display }}</small>{% else %}Sin coordenadas{% endif %}</dd>
        <dt>Capturada</dt><dd>{{ case.captured_at|date:"d/m/Y H:i" }} · {{ case.capture.get_camera_role_display }}</dd>
        <dt>Operador</dt><dd>{{ case.capture.operator_user.full_name }}</dd>
        <dt>Análisis</dt><dd>{% if case.ai_task %}{% badge case.ai_task.status case.ai_task.get_status_display %} · modelo {{ case.ai_task.model_version|default:"—" }}{% if case.ai_task.processing_ms %} · {{ case.ai_task.processing_ms }} ms{% endif %}{% else %}—{% endif %}</dd>
      </dl>
      <p class="no-imprimir">
        {% if case.lat is not None %}<a href="{% url 'web:mapa' %}?caso={{ case.pk }}">Ver en el mapa</a> · <a href="https://www.google.com/maps/search/?api=1&query={{ case.lat|stringformat:'.6f' }},{{ case.lon|stringformat:'.6f' }}" target="_blank" rel="noopener noreferrer">Cómo llegar</a> · {% endif %}
        <a href="{% url 'web:plano' %}?lote={{ case.lot_id }}#H{{ case.row.number }}">Ver en el plano</a> · <a href="{% url 'web:captura' case.capture_id %}">Datos de la foto</a>
      </p>
      {% if sibling %}<p><small>Otra cámara de la misma secuencia:</small><br><a href="{% url 'web:captura' sibling.pk %}"><img class="miniatura" src="{{ sibling_thumb }}" alt="Foto de {{ sibling.get_camera_role_display }}"></a></p>{% endif %}
    </div>
    {% include "web/partials/panel_decision.html" %}
  </aside>
</div>
{% endblock %}
{% block scripts %}<script src="{% static 'web/caso.js' %}" defer></script>{% endblock %}
```

`web/templates/web/partials/panel_decision.html`

```html
{% load web_tags %}
<div id="panel-decision" class="tarjeta decision" style="margin-top:12px">
  {% if request.htmx %}{% include "web/partials/mensajes.html" %}{% endif %}
  {% if case.status == 'PENDIENTE_REVISION' %}
    {% if puede.caso_decidir %}
    <h2 style="margin-top:0">Decisión del especialista</h2>
    <form method="post" action="{% url 'web:caso_decidir' case.pk %}" hx-post="{% url 'web:caso_decidir' case.pk %}" hx-target="#panel-decision" hx-swap="outerHTML" id="form-decision">
      {% csrf_token %}
      {% if form.non_field_errors %}<ul class="errorlist">{% for e in form.non_field_errors %}<li>{{ e }}</li>{% endfor %}</ul>{% endif %}
      <fieldset><legend class="sr-only">Decisión</legend>
        {% for radio in form.decision %}<label class="opcion">{{ radio.tag }} {% badge radio.data.value radio.choice_label %}</label>{% endfor %}
        {{ form.decision.errors }}
      </fieldset>
      <label for="id_observation">Observación (obligatoria para confirmar o marcar evidencia insuficiente; no registrar tratamientos ni dosis)</label>
      {{ form.observation }}{{ form.observation.errors }}
      {% if form.confirmed_class.field.choices|length > 1 %}<p><label for="id_confirmed_class">Clase confirmada (opcional)</label> {{ form.confirmed_class }}{{ form.confirmed_class.errors }}</p>{% endif %}
      {% if detections %}<p>Cajas incorrectas de la IA (opcional):</p>{{ form.rejected_detection_ids }}{{ form.rejected_detection_ids.errors }}{% endif %}
      <p style="display:flex;gap:8px;flex-wrap:wrap">
        <button class="btn btn-primario" type="submit">Guardar decisión</button>
        <button class="btn btn-secundario" type="submit" name="siguiente" value="1">Guardar y siguiente</button>
      </p>
      <p style="font-size:.8rem;color:var(--texto-2)">Atajos: C confirmar · D descartar · I evidencia insuficiente · Ctrl+Enter guardar.</p>
    </form>
    {% else %}
    <p>{% badge case.status case.get_status_display %} Pendiente: solo el especialista fitosanitario puede decidir este caso.</p>
    {% endif %}
  {% else %}
    <h2 style="margin-top:0">Decisión vigente</h2>
    {% if current %}<p>{% badge current.decision current.get_decision_display %}<br><small>{{ current.reviewer.full_name }} · {{ current.reviewed_at|date:"d/m/Y H:i" }}</small></p>
    {% if current.observation %}<p><strong>Observación:</strong> {{ current.observation|linebreaksbr }}</p>{% endif %}
    {% if current.confirmed_class %}<p><strong>Clase confirmada:</strong> {{ current.confirmed_class }}</p>{% endif %}{% endif %}
    <p>Aviso por WhatsApp: {% badge case.notification_status case.get_notification_status_display %}</p>
    {% if notifications %}<ul>{% for n in notifications %}<li>{{ n.recipient_name }} — {% badge n.status n.get_status_display %}{% if n.sent_at %} {{ n.sent_at|date:"d/m H:i" }}{% endif %}{% if n.error %} <small>{{ n.error|truncatechars:80 }}</small>{% endif %}</li>{% endfor %}</ul>{% endif %}
    {% if puede.caso_corregir %}
    <details {% if correction_form.errors %}open{% endif %}><summary>Corregir la decisión</summary>
      <form method="post" action="{% url 'web:caso_corregir' case.pk %}" hx-post="{% url 'web:caso_corregir' case.pk %}" hx-target="#panel-decision" hx-swap="outerHTML">
        {% csrf_token %}{{ correction_form.expected_review_id }}
        {% if correction_form.non_field_errors %}<ul class="errorlist">{% for e in correction_form.non_field_errors %}<li>{{ e }}</li>{% endfor %}</ul>{% endif %}
        <fieldset>{% for radio in correction_form.decision %}<label class="opcion">{{ radio.tag }} {{ radio.choice_label }}</label>{% endfor %}</fieldset>
        <label>Observación</label>{{ correction_form.observation }}{{ correction_form.observation.errors }}
        <label>Motivo de la corrección (obligatorio)</label>{{ correction_form.correction_reason }}{{ correction_form.correction_reason.errors }}
        <p><button class="btn btn-peligro" type="submit">Guardar corrección</button></p>
      </form>
    </details>
    {% endif %}
  {% endif %}
  {% if reviews|length > 1 or reviews and case.status != 'PENDIENTE_REVISION' %}
  <h2>Historial</h2>
  <ol class="historial">{% for r in reviews %}<li>{% badge r.decision r.get_decision_display %} {{ r.reviewer.full_name }} · {{ r.reviewed_at|date:"d/m/Y H:i" }}{% if r.correction_reason %}<br><small>Corrección: {{ r.correction_reason }}</small>{% endif %}{% if not r.is_current %} <small>(reemplazada)</small>{% endif %}</li>{% endfor %}</ol>
  {% endif %}
</div>
```

### E.6 Foto — `captura.html`

`web/templates/web/captura.html`

```html
{% extends "web/base.html" %}{% load web_tags %}
{% block titulo %}Foto {{ capture.pk|stringformat:"s"|slice:":8" }}{% endblock %}
{% block migas %}<a href="{% url 'web:sesion' capture.session_id %}">Sesión</a> › Foto{% endblock %}
{% block contenido %}
<h1>Foto {{ capture.pk|stringformat:"s"|slice:":8" }} {% badge capture.quality_status capture.get_quality_status_display %}</h1>
<div class="caso">
  <section><div class="visor"><div class="lienzo"><img src="{{ img_revision }}" alt="Foto {{ capture.get_camera_role_display }}"></div></div></section>
  <aside class="tarjeta ficha">
    <dl>
      <dt>Lote · hilera</dt><dd>{{ capture.monitoring_pass.lot.code }} · H{{ capture.monitoring_pass.row.number }} · {{ capture.get_lateral_code_display }}</dd>
      <dt>Secuencia</dt><dd>N.º {{ capture.sequence.sequence_number }}</dd>
      <dt>Cámara</dt><dd>{{ capture.get_camera_role_display }} · {{ capture.device }}</dd>
      <dt>Usuario en la cámara</dt><dd>{{ capture.camera_user.full_name }}</dd>
      <dt>Capturada</dt><dd>{{ capture.captured_at|date:"d/m/Y H:i:s" }}</dd>
      <dt>Tamaño</dt><dd>{{ capture.width }}×{{ capture.height }} · {{ capture.size_bytes|filesizeformat }}</dd>
      <dt>Calidad</dt><dd>{{ capture.quality.profile_version }}{% if capture.quality.reasons %} · {{ capture.quality.reasons|join:", " }}{% endif %}</dd>
      <dt>Repite a</dt><dd>{% if capture.replaces_capture_id %}<a href="{% url 'web:captura' capture.replaces_capture_id %}">{{ capture.replaces_capture_id|stringformat:"s"|slice:":8" }}</a>{% else %}—{% endif %}</dd>
    </dl>
    <h2>Análisis</h2>
    {% for t in tasks %}<p>{% badge t.status t.get_status_display %} · {{ t.model_config }}{% if t.error_message %}<br><small>{{ t.error_message|truncatechars:160 }}</small>{% endif %}</p>{% empty %}<p>Sin análisis (las fotos rechazadas por calidad no se analizan).</p>{% endfor %}
    {% if case %}<p><a class="btn btn-secundario" href="{% url 'web:caso' case.pk %}">Ver caso</a></p>
    {% elif puede_abrir %}<form method="post" action="{% url 'web:captura_abrir_caso' capture.pk %}">{% csrf_token %}<button class="btn btn-secundario">Abrir caso para revisión</button></form>
    <p style="font-size:.8rem;color:var(--texto-2)">Úsalo si ves un posible indicio que la IA no marcó (o si el análisis falló).</p>{% endif %}
  </aside>
</div>
{% endblock %}
```

### E.7 Mapa y plano — `mapa.html`, `plano.html`

`web/templates/web/mapa.html`

```html
{% extends "web/base.html" %}{% load static web_tags %}
{% block titulo %}Mapa{% endblock %}
{% block head %}<link rel="stylesheet" href="{% static 'web/vendor/leaflet/leaflet.css' %}">{% endblock %}
{% block contenido %}
<h1>Mapa de casos</h1>
<form class="filtros" id="filtros-mapa">
  <label>Estado {{ filtro.estado }}</label>
  <label>Lote <select name="lote"><option value="">Todos</option>{% for l in lotes %}<option value="{{ l.pk }}">{{ l.code }}</option>{% endfor %}</select></label>
  <label>Desde {{ filtro.desde }}</label><label>Hasta {{ filtro.hasta }}</label>
</form>
<div class="leyenda">{% badge "PENDIENTE_REVISION" "Pendiente" %}{% badge "CONFIRMADO_POR_ESPECIALISTA" "Confirmado" %}{% badge "EVIDENCIA_INSUFICIENTE" "Evidencia insuficiente" %}{% badge "DESCARTADO" "Descartado" %}<span>○ borde punteado = ubicación aproximada (marcador)</span></div>
<div id="mapa" data-url="{% url 'web:mapa_datos' %}" data-caso="{{ request.GET.caso|default:'' }}"></div>
<p id="mapa-meta" style="color:var(--texto-2);font-size:.85rem"></p>
<p style="color:var(--texto-2);font-size:.85rem">La posición es la del controlador al emitir la secuencia (GPS del celular), no la de una planta exacta. Los casos sin coordenadas aparecen en el <a href="{% url 'web:plano' %}">plano de hileras</a>.</p>
{% endblock %}
{% block scripts %}<script src="{% static 'web/vendor/leaflet/leaflet.js' %}" defer></script><script src="{% static 'web/mapa.js' %}" defer></script>{% endblock %}
```

`web/templates/web/plano.html`

```html
{% extends "web/base.html" %}{% load web_tags %}
{% block titulo %}Plano de hileras{% endblock %}
{% block contenido %}
<h1>Plano de hileras {% if p %}· {{ p.lot.code }} {{ p.lot.name }}{% endif %}</h1>
<form class="filtros" method="get">
  <label>Lote <select name="lote">{% for l in lotes %}<option value="{{ l.pk }}" {% if p and p.lot.pk == l.pk %}selected{% endif %}>{{ l.code }}</option>{% endfor %}</select></label>
  <label>Estado {{ filtro.estado }}</label><label>Desde {{ filtro.desde }}</label><label>Hasta {{ filtro.hasta }}</label>
  <button class="btn btn-secundario">Aplicar</button>
</form>
{% if p %}
<p>Hileras cubiertas (A y B): <strong>{{ p.covered }} / {{ p.rows|length }}</strong>. Cada barra es una hilera proporcional a su número de plantas; los recuadros son los segmentos del piloto.</p>
<div class="leyenda">{% badge "PENDIENTE_REVISION" "Pendiente" %}{% badge "CONFIRMADO_POR_ESPECIALISTA" "Confirmado" %}{% badge "EVIDENCIA_INSUFICIENTE" "Evidencia insuficiente" %}{% badge "DESCARTADO" "Descartado" %}</div>
<div class="plano tarjeta">
{% for r in p.rows %}
  <div class="hilera" id="H{{ r.row.number }}">
    <span>H{{ r.row.number }}{% if r.covered %} ✔{% endif %}</span>
    <div class="barra" style="width: {% bar_width r.row.plant_count p.max_plants %}%" title="{{ r.row.plant_count }} plantas">
      {% for s in r.segments %}
      {% with c=s.counts %}
      <a class="segmento {% if c.CONFIRMADO_POR_ESPECIALISTA %}c-CONFIRMADO_POR_ESPECIALISTA{% elif c.PENDIENTE_REVISION %}c-PENDIENTE_REVISION{% elif c.EVIDENCIA_INSUFICIENTE %}c-EVIDENCIA_INSUFICIENTE{% elif c.DESCARTADO %}c-DESCARTADO{% endif %}"
         style="left: {{ s.left_pct|stringformat:'.2f' }}%; width: {{ s.width_pct|stringformat:'.2f' }}%"
         href="{% url 'web:bandeja' %}?estado=&lote={{ p.lot.pk }}" title="{{ s.segment.code }} (plantas {{ s.segment.start_plant }}–{{ s.segment.end_plant }})">{{ s.segment.code }}</a>
      {% endwith %}
      {% endfor %}
    </div>
    <span>{% for k, v in r.counts.items %}{% badge k v %} {% empty %}<small>sin casos</small>{% endfor %}</span>
  </div>
{% endfor %}
</div>
{% else %}<p>No hay lotes activos.</p>{% endif %}
{% endblock %}
```

### E.8 Sesiones — `sesiones.html`, `sesion.html`

`web/templates/web/sesiones.html`

```html
{% extends "web/base.html" %}{% load web_tags %}
{% block titulo %}Sesiones{% endblock %}
{% block contenido %}
<h1>Sesiones de monitoreo</h1>
<table class="tabla"><thead><tr><th>Inicio</th><th>Operador</th><th>Modo</th><th>Estado</th><th>Pasadas</th><th>Fotos</th><th>App</th></tr></thead><tbody>
{% for s in page.object_list %}
<tr><td data-label="Inicio"><a href="{% url 'web:sesion' s.pk %}">{{ s.started_at|date:"d/m/Y H:i"|default:"—" }}</a></td><td data-label="Operador">{{ s.operator.full_name }}</td>
<td data-label="Modo">{{ s.get_mode_display }}</td><td data-label="Estado">{% badge s.status s.get_status_display %}</td>
<td data-label="Pasadas">{{ s.n_pasadas }}</td><td data-label="Fotos">{{ s.n_capturas }}</td><td data-label="App">{{ s.app_version }}</td></tr>
{% empty %}<tr><td colspan="7">Aún no hay sesiones sincronizadas.</td></tr>{% endfor %}
</tbody></table>
<div class="paginacion">{% if page.has_previous %}<a href="{% querystring pagina=page.previous_page_number %}">← Anterior</a>{% endif %}<span>Página {{ page.number }} de {{ page.paginator.num_pages }}</span>{% if page.has_next %}<a href="{% querystring pagina=page.next_page_number %}">Siguiente →</a>{% endif %}</div>
{% endblock %}
```

`web/templates/web/sesion.html`

```html
{% extends "web/base.html" %}{% load web_tags %}
{% block titulo %}Sesión{% endblock %}
{% block migas %}<a href="{% url 'web:sesiones' %}">Sesiones</a> › {{ s.started_at|date:"d/m/Y H:i" }}{% endblock %}
{% block contenido %}
<h1>Sesión del {{ s.started_at|date:"d/m/Y H:i"|default:"—" }} {% badge s.status s.get_status_display %}</h1>
<div class="kpis">
  <div class="tarjeta kpi"><div class="etiqueta">Operador</div><div>{{ s.operator.full_name }}</div></div>
  <div class="tarjeta kpi"><div class="etiqueta">Duración</div><div>{% if s.started_at and s.ended_at %}{{ s.started_at|timesince:s.ended_at }}{% else %}—{% endif %}</div></div>
  <div class="tarjeta kpi"><div class="etiqueta">Versiones</div><div>App {{ s.app_version }} · Config {{ s.config_version }} · Calidad {{ s.quality_profile_version }}</div></div>
  <div class="tarjeta kpi"><div class="etiqueta">Calidad de fotos</div><div>{% for k, v in quality.items %}{% badge k v %} {% endfor %}</div></div>
</div>
<h2>Cámaras</h2>
<table class="tabla"><thead><tr><th>Rol</th><th>Usuario</th><th>Celular</th><th>Vinculada</th><th>Liberada</th></tr></thead><tbody>
{% for c in cameras %}<tr><td data-label="Rol">{{ c.get_role_display }}</td><td data-label="Usuario">{{ c.user.full_name }}</td><td data-label="Celular">{{ c.model }} · {{ c.get_platform_display }} {{ c.os_version }}</td><td data-label="Vinculada">{{ c.paired_at|date:"H:i" }}</td><td data-label="Liberada">{{ c.released|yesno:"Sí,No" }}</td></tr>{% endfor %}
</tbody></table>
<h2>Pasadas</h2>
<table class="tabla"><thead><tr><th>Lote · hilera</th><th>Lateral</th><th>Orden</th><th>Estado</th><th>Secuencias (completas / incompletas)</th><th>Inicio – fin</th></tr></thead><tbody>
{% for p in passes %}<tr><td data-label="Hilera">{{ p.lot.code }} · H{{ p.row.number }}</td><td data-label="Lateral">{{ p.get_lateral_code_display }}</td><td data-label="Orden">{{ p.pass_order }}</td><td data-label="Estado">{% badge p.status p.get_status_display %}</td><td data-label="Secuencias">{{ p.sequences_total }} ({{ p.sequences_complete }} / {{ p.sequences_incomplete }})</td><td data-label="Horario">{{ p.started_at|date:"H:i" }} – {{ p.ended_at|date:"H:i" }}</td></tr>{% endfor %}
</tbody></table>
<h2>Incidencias</h2>
<table class="tabla"><thead><tr><th>Hora</th><th>Tipo</th><th>Nivel</th><th>Detalle</th><th>Origen</th></tr></thead><tbody>
{% for i in incidents %}<tr><td data-label="Hora">{{ i.occurred_at|date:"H:i:s" }}</td><td data-label="Tipo">{{ i.get_type_display }}</td><td data-label="Nivel">{{ i.get_severity_display }}</td><td data-label="Detalle">{{ i.detail }}</td><td data-label="Origen">{{ i.get_created_by_display }}</td></tr>{% empty %}<tr><td colspan="5">Sin incidencias.</td></tr>{% endfor %}
</tbody></table>
<h2>Fotos</h2>
<table class="tabla"><thead><tr><th>Hora</th><th>Hilera</th><th>Cámara</th><th>Calidad</th><th>Caso</th></tr></thead><tbody>
{% for c in captures %}<tr><td data-label="Hora"><a href="{% url 'web:captura' c.pk %}">{{ c.captured_at|date:"H:i:s" }}</a></td><td data-label="Hilera">H{{ c.monitoring_pass.row.number }} · {{ c.get_lateral_code_display }}</td><td data-label="Cámara">{{ c.get_camera_role_display }}</td><td data-label="Calidad">{% badge c.quality_status c.get_quality_status_display %}</td><td data-label="Caso">{% if c.n_cases %}Sí{% else %}—{% endif %}</td></tr>{% endfor %}
</tbody></table>
{% endblock %}
```

### E.9 Reportes, avisos y destinatarios

`web/templates/web/reportes.html`

```html
{% extends "web/base.html" %}
{% block titulo %}Reportes{% endblock %}
{% block contenido %}
<h1>Reportes y exportaciones</h1>
<div class="tarjeta">
  <h2 style="margin-top:0">Casos y decisiones (CSV)</h2>
  <p>Una fila por caso con ubicación, estado, decisión, modelo y aviso. Sirve para el análisis pretest–postest y para medir la concordancia IA–especialista. Se abre en Excel (UTF-8).</p>
  <form class="filtros" method="get" action="{% url 'web:exportar_casos' %}">
    <label>Estado {{ filtro.estado }}</label>
    <label>Lote <select name="lote"><option value="">Todos</option>{% for l in lotes %}<option value="{{ l.pk }}">{{ l.code }}</option>{% endfor %}</select></label>
    <label>Desde {{ filtro.desde }}</label><label>Hasta {{ filtro.hasta }}</label>
    <button class="btn btn-primario">Descargar CSV</button>
  </form>
  <p style="font-size:.85rem;color:var(--texto-2)">Para un informe en PDF: abre el Panel o un caso y usa «Imprimir → Guardar como PDF» del navegador (la hoja de estilos de impresión oculta los menús).</p>
</div>
{% endblock %}
```

`web/templates/web/notificaciones.html`

```html
{% extends "web/base.html" %}{% load web_tags %}
{% block titulo %}Avisos{% endblock %}
{% block contenido %}
<h1>Avisos por WhatsApp</h1>
<form class="filtros" method="get"><label>Estado <select name="estado"><option value="">Todos</option><option value="PENDIENTE_ENVIO" {% if estado == 'PENDIENTE_ENVIO' %}selected{% endif %}>Pendiente de envío</option><option value="ENVIADO" {% if estado == 'ENVIADO' %}selected{% endif %}>Enviado</option><option value="ERROR_ENVIO" {% if estado == 'ERROR_ENVIO' %}selected{% endif %}>Error de envío</option><option value="NO_APLICA" {% if estado == 'NO_APLICA' %}selected{% endif %}>Anulado</option></select></label><button class="btn btn-secundario">Filtrar</button></form>
<table class="tabla"><thead><tr><th>Creado</th><th>Caso</th><th>Destinatario</th><th>Estado</th><th>Intentos</th><th>Enviado</th><th>Error</th></tr></thead><tbody>
{% for n in page.object_list %}<tr><td data-label="Creado">{{ n.created_at|date:"d/m/Y H:i" }}</td><td data-label="Caso"><a href="{% url 'web:caso' n.case_id %}">{{ n.case.lot.code }} · H{{ n.case.row.number }}</a></td><td data-label="Destinatario">{{ n.recipient_name }}</td><td data-label="Estado">{% badge n.status n.get_status_display %}</td><td data-label="Intentos">{{ n.attempts }}</td><td data-label="Enviado">{{ n.sent_at|date:"d/m H:i"|default:"—" }}</td><td data-label="Error">{{ n.error|truncatechars:90 }}</td></tr>
{% empty %}<tr><td colspan="7">Sin avisos.</td></tr>{% endfor %}
</tbody></table>
{% endblock %}
```

`web/templates/web/destinatarios.html`

```html
{% extends "web/base.html" %}{% load web_tags %}
{% block titulo %}Destinatarios WhatsApp{% endblock %}
{% block contenido %}
<h1>Destinatarios de avisos por WhatsApp</h1>
<p>Reciben un aviso solo cuando el especialista <strong>confirma</strong> un caso de un lote de su alcance. Deben haber aceptado recibir mensajes (consentimiento) y, para abrir el enlace, tener una cuenta de la web.</p>
<table class="tabla"><thead><tr><th>Nombre</th><th>Función</th><th>Celular</th><th>Lotes</th><th>Consentimiento</th><th>Activo</th><th></th></tr></thead><tbody>
{% for r in lista %}<tr><td data-label="Nombre">{{ r.full_name }}{% if r.user %}<br><small>{{ r.user.email }}</small>{% endif %}</td><td data-label="Función">{{ r.get_role_label_display }}</td><td data-label="Celular">{{ r.phone_e164 }}</td><td data-label="Lotes">{% for l in r.lots.all %}{{ l.code }}{% if not forloop.last %}, {% endif %}{% empty %}Todos{% endfor %}</td><td data-label="Consentimiento">{{ r.opt_in_at|date:"d/m/Y"|default:"Falta" }}</td><td data-label="Activo">{{ r.active|yesno:"Sí,No" }}</td><td><a href="{% url 'web:destinatario_editar' r.pk %}">Editar</a></td></tr>
{% empty %}<tr><td colspan="7">Sin destinatarios: los casos confirmados no generarán avisos.</td></tr>{% endfor %}
</tbody></table>
<h2>{% if editando %}Editar {{ editando.full_name }}{% else %}Nuevo destinatario{% endif %}</h2>
<form method="post" class="tarjeta" style="max-width:640px" action="{% if editando %}{% url 'web:destinatario_editar' editando.pk %}{% else %}{% url 'web:destinatarios' %}{% endif %}">{% csrf_token %}{{ form.as_p }}<button class="btn btn-primario">Guardar</button></form>
{% endblock %}
```

### E.10 Administración — usuarios, celulares, IA y auditoría

`web/templates/web/usuarios.html`

```html
{% extends "web/base.html" %}{% load web_tags %}
{% block titulo %}Usuarios{% endblock %}
{% block contenido %}
<h1>Usuarios</h1>
<h2>Solicitudes pendientes de aprobación ({{ pendientes|length }})</h2>
<table class="tabla"><thead><tr><th>Nombre</th><th>Correo</th><th>Código</th><th>Solicitado</th><th>Aprobar con roles</th><th></th></tr></thead><tbody>
{% for u in pendientes %}<tr><td data-label="Nombre">{{ u.full_name }}</td><td data-label="Correo">{{ u.email }}</td><td data-label="Código">{{ u.employee_code|default:"—" }}</td><td data-label="Solicitado">{{ u.created_at|date:"d/m/Y H:i" }}</td>
<td data-label="Roles"><form method="post" action="{% url 'web:usuario_accion' u.pk 'aprobar' %}" style="display:flex;gap:8px;flex-wrap:wrap;align-items:center">{% csrf_token %}{% for c in approve_form.roles %}<label>{{ c.tag }} {{ c.choice_label }}</label>{% endfor %}<button class="btn btn-primario">Aprobar</button></form></td>
<td><form method="post" action="{% url 'web:usuario_accion' u.pk 'rechazar' %}">{% csrf_token %}<button class="btn btn-secundario">Rechazar</button></form></td></tr>
{% empty %}<tr><td colspan="6">No hay solicitudes pendientes.</td></tr>{% endfor %}
</tbody></table>
{% if solicitudes %}<h2>Pedidos de restablecimiento de contraseña</h2><ul>{% for s in solicitudes %}<li>{{ s.email }} · {{ s.requested_at|date:"d/m/Y H:i" }}</li>{% endfor %}</ul>{% endif %}
<h2>Todas las cuentas</h2>
<table class="tabla"><thead><tr><th>Nombre</th><th>Correo</th><th>Roles</th><th>Estado</th><th>Acciones</th></tr></thead><tbody>
{% for u in todos %}<tr><td data-label="Nombre">{{ u.full_name }}</td><td data-label="Correo">{{ u.email }}</td><td data-label="Roles">{% for r in u.user_roles.all %}{{ r.get_role_display }}{% if not forloop.last %}, {% endif %}{% empty %}—{% endfor %}</td><td data-label="Estado">{% badge u.status u.get_status_display %}{% if u.must_change_password %} <small>(contraseña temporal)</small>{% endif %}</td>
<td data-label="Acciones"><div style="display:flex;gap:6px;flex-wrap:wrap">
{% if u.pk != user.pk %}
{% if u.status == 'ACTIVO' %}<form method="post" action="{% url 'web:usuario_accion' u.pk 'bloquear' %}">{% csrf_token %}<button class="btn btn-secundario">Bloquear</button></form>
<form method="post" action="{% url 'web:usuario_accion' u.pk 'contrasena-temporal' %}">{% csrf_token %}<button class="btn btn-secundario">Contraseña temporal</button></form>{% endif %}
{% if u.status == 'BLOQUEADO' %}<form method="post" action="{% url 'web:usuario_accion' u.pk 'desbloquear' %}">{% csrf_token %}<button class="btn btn-secundario">Desbloquear</button></form>{% endif %}
{% endif %}
{% if u.status == 'ACTIVO' or u.status == 'BLOQUEADO' %}<details><summary>Roles</summary>
<form method="post" action="{% url 'web:usuario_accion' u.pk 'roles' %}">{% csrf_token %}{% for value, label in roles_choices %}<label style="display:block"><input type="checkbox" name="roles" value="{{ value }}" {% if value in u.roles %}checked{% endif %}> {{ label }}</label>{% endfor %}<button class="btn btn-secundario">Guardar roles</button></form></details>{% endif %}
</div></td></tr>{% endfor %}
</tbody></table>
{% endblock %}
```

`web/templates/web/dispositivos.html`

```html
{% extends "web/base.html" %}
{% block titulo %}Celulares{% endblock %}
{% block contenido %}
<h1>Celulares (instalaciones de la app)</h1>
<table class="tabla"><thead><tr><th>Celular</th><th>Plataforma</th><th>App</th><th>Último usuario</th><th>Última vez</th><th>Estado</th><th></th></tr></thead><tbody>
{% for d in lista %}<tr><td data-label="Celular">{{ d.model|default:"—" }}<br><small>{{ d.device_id|stringformat:"s"|slice:":8" }}</small></td><td data-label="Plataforma">{{ d.get_platform_display }} {{ d.os_version }}</td><td data-label="App">{{ d.app_version }}</td><td data-label="Usuario">{{ d.user.full_name|default:"—" }}</td><td data-label="Última vez">{{ d.last_seen_at|date:"d/m/Y H:i" }}</td>
<td data-label="Estado">{% if d.revoked_at %}Revocado {{ d.revoked_at|date:"d/m/Y" }}{% else %}Habilitado{% endif %}</td>
<td>{% if not d.revoked_at %}<form method="post" action="{% url 'web:dispositivo_revocar' d.pk %}">{% csrf_token %}<button class="btn btn-peligro">Revocar</button></form>{% endif %}</td></tr>
{% empty %}<tr><td colspan="7">Ningún celular ha iniciado sesión todavía.</td></tr>{% endfor %}
</tbody></table>
{% endblock %}
```

`web/templates/web/ia.html`

```html
{% extends "web/base.html" %}{% load web_tags %}
{% block titulo %}Estado de la IA{% endblock %}
{% block contenido %}
<h1>Estado del análisis con IA</h1>
<p>Modelo activo: <strong>{% if modelo %}{{ modelo }} · clases: {{ modelo.classes|join:", " }} · umbral {{ modelo.conf_threshold }}{% else %}ninguno (las capturas nuevas no podrán encolarse){% endif %}</strong></p>
<div class="kpis">{% for value, label in estados %}<div class="tarjeta kpi"><div class="valor">{{ conteo|get:value }}</div><div class="etiqueta">{% badge value label %}</div></div>{% endfor %}</div>
<h2>Errores de análisis (últimos 100)</h2>
<table class="tabla"><thead><tr><th>Foto</th><th>Modelo</th><th>Intentos</th><th>Terminó</th><th>Error</th><th></th></tr></thead><tbody>
{% for t in errores %}<tr><td data-label="Foto"><a href="{% url 'web:captura' t.capture_id %}">{{ t.capture_id|stringformat:"s"|slice:":8" }}</a></td><td data-label="Modelo">{{ t.model_config.version }}</td><td data-label="Intentos">{{ t.attempts }}</td><td data-label="Terminó">{{ t.finished_at|date:"d/m H:i" }}</td><td data-label="Error">{{ t.error_message|truncatechars:120 }}</td>
<td>{% if puede.ia_reencolar %}<form method="post" action="{% url 'web:ia_reencolar' t.pk %}">{% csrf_token %}<button class="btn btn-secundario">Reencolar</button></form>{% endif %}</td></tr>
{% empty %}<tr><td colspan="6">Sin errores.</td></tr>{% endfor %}
</tbody></table>
{% endblock %}
```

`web/templates/web/auditoria.html`

```html
{% extends "web/base.html" %}
{% block titulo %}Auditoría{% endblock %}
{% block contenido %}
<h1>Auditoría</h1>
<form class="filtros" method="get"><label>Buscar (acción o ID) <input name="q" value="{{ q }}"></label><button class="btn btn-secundario">Buscar</button></form>
<table class="tabla"><thead><tr><th>Fecha</th><th>Usuario</th><th>Acción</th><th>Entidad</th><th>Antes → después</th></tr></thead><tbody>
{% for e in page.object_list %}<tr><td data-label="Fecha">{{ e.timestamp|date:"d/m/Y H:i:s" }}</td><td data-label="Usuario">{{ e.user.full_name|default:"Sistema" }}</td><td data-label="Acción">{{ e.action }}</td><td data-label="Entidad">{{ e.entity_type }} {{ e.entity_id|truncatechars:12 }}</td><td data-label="Cambio"><small>{{ e.before|default:"" }} → {{ e.after|default:"" }}</small></td></tr>
{% empty %}<tr><td colspan="5">Sin eventos.</td></tr>{% endfor %}
</tbody></table>
<div class="paginacion">{% if page.has_previous %}<a href="{% querystring pagina=page.previous_page_number %}">← Anterior</a>{% endif %}<span>Página {{ page.number }} de {{ page.paginator.num_pages }}</span>{% if page.has_next %}<a href="{% querystring pagina=page.next_page_number %}">Siguiente →</a>{% endif %}</div>
{% endblock %}
```

### E.11 Estilos — `web/static/web/app.css`

`web/static/web/app.css`

```css
/* web/static/web/app.css — Estilos de la web (sin framework ni compilación). Tokens en :root (sección 6.5). */
:root {
  --verde-900: #173f31; --verde-700: #1f5f46; --verde-600: #2f7a5b; --verde-100: #e7f1ec; --verde-50: #f3f8f5;
  --texto: #1f2933; --texto-2: #52606d; --borde: #d3ddd8; --fondo: #f7f9f8; --blanco: #ffffff;
  /* estados de revisión (no gravedad) */
  --pend-fg: #93370d; --pend-bg: #fef0c7;
  --conf-fg: #a01b10; --conf-bg: #fee4e2;
  --desc-fg: #475467; --desc-bg: #eaecf0;
  --insu-fg: #4a1fb8; --insu-bg: #ebe9fe;
  --ok-fg: #05603a; --ok-bg: #d1fadf;
  --err-fg: #7a2e0e; --err-bg: #fbe5d6;
  --proc-fg: #175cd3; --proc-bg: #d1e9ff;
  --indi-fg: #854a0e; --indi-bg: #fdf3c4;
  --radio: 8px; --sombra: 0 1px 2px rgba(16, 24, 40, .08);
  --lateral: 248px;
  font-family: "Inter", system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;
  font-size: 15px; color: var(--texto); background: var(--fondo);
}
* { box-sizing: border-box; }
body { margin: 0; line-height: 1.45; }
a { color: var(--verde-700); }
a:focus-visible, button:focus-visible, input:focus-visible, select:focus-visible, textarea:focus-visible {
  outline: 3px solid #84caff; outline-offset: 2px;
}
.layout { display: grid; grid-template-columns: var(--lateral) 1fr; min-height: 100vh; }
.lateral { background: var(--verde-900); color: #fff; padding: 16px 12px; position: sticky; top: 0; height: 100vh; overflow-y: auto; }
.lateral .marca { font-weight: 700; font-size: 1.05rem; margin: 0 8px 18px; }
.lateral .grupo { font-size: .72rem; letter-spacing: .06em; text-transform: uppercase; opacity: .7; margin: 16px 8px 6px; }
.lateral a { display: flex; gap: 10px; align-items: center; color: #e8f2ed; text-decoration: none; padding: 9px 10px; border-radius: 6px; }
.lateral a[aria-current="page"], .lateral a:hover { background: rgba(255,255,255,.12); color: #fff; }
.lateral .ico { width: 20px; text-align: center; }
.principal { min-width: 0; }
.cabecera { display: flex; justify-content: space-between; align-items: center; gap: 12px; padding: 12px 24px;
  background: var(--blanco); border-bottom: 1px solid var(--borde); position: sticky; top: 0; z-index: 500; }
.cabecera .usuario { display: flex; gap: 10px; align-items: center; color: var(--texto-2); font-size: .9rem; }
.entorno { font-size: .75rem; padding: 2px 8px; border-radius: 99px; background: var(--pend-bg); color: var(--pend-fg); }
.contenido { padding: 20px 24px 40px; max-width: 1480px; }
h1 { font-size: 1.45rem; margin: 0 0 14px; color: var(--verde-900); }
h2 { font-size: 1.1rem; margin: 22px 0 10px; color: var(--verde-900); }
.tarjeta { background: var(--blanco); border: 1px solid var(--borde); border-radius: var(--radio); box-shadow: var(--sombra); padding: 16px; }
.kpis { display: grid; grid-template-columns: repeat(auto-fill, minmax(190px, 1fr)); gap: 12px; }
.kpi .valor { font-size: 1.7rem; font-weight: 700; color: var(--verde-900); }
.kpi .etiqueta { color: var(--texto-2); font-size: .85rem; }
.badge { display: inline-flex; gap: 5px; align-items: center; padding: 2px 9px; border-radius: 99px; font-size: .8rem; font-weight: 600; white-space: nowrap; }
.b-pendiente { color: var(--pend-fg); background: var(--pend-bg); }
.b-confirmado { color: var(--conf-fg); background: var(--conf-bg); }
.b-descartado { color: var(--desc-fg); background: var(--desc-bg); }
.b-insuficiente { color: var(--insu-fg); background: var(--insu-bg); }
.b-ok { color: var(--ok-fg); background: var(--ok-bg); }
.b-error { color: var(--err-fg); background: var(--err-bg); }
.b-proceso { color: var(--proc-fg); background: var(--proc-bg); }
.b-indicio { color: var(--indi-fg); background: var(--indi-bg); }
.b-neutro { color: var(--texto-2); background: #f2f4f7; }
table.tabla { width: 100%; border-collapse: collapse; background: var(--blanco); }
.tabla th, .tabla td { padding: 9px 10px; border-bottom: 1px solid var(--borde); text-align: left; vertical-align: middle; }
.tabla th { font-size: .78rem; text-transform: uppercase; letter-spacing: .04em; color: var(--texto-2); background: var(--verde-50); }
.tabla tr:hover td { background: #fafcfb; }
.miniatura { width: 72px; height: 54px; object-fit: cover; border-radius: 4px; background: #e4e7ec; }
.filtros { display: flex; flex-wrap: wrap; gap: 10px; align-items: end; margin-bottom: 14px; }
.filtros label { display: grid; gap: 4px; font-size: .8rem; color: var(--texto-2); }
input, select, textarea { font: inherit; padding: 7px 9px; border: 1px solid #c3cdc8; border-radius: 6px; background: #fff; }
textarea { width: 100%; }
.btn { display: inline-flex; align-items: center; gap: 6px; padding: 9px 14px; border-radius: 6px; border: 1px solid transparent;
  font: inherit; font-weight: 600; cursor: pointer; text-decoration: none; min-height: 40px; }
.btn-primario { background: var(--verde-700); color: #fff; }
.btn-secundario { background: #fff; color: var(--verde-700); border-color: var(--verde-700); }
.btn-peligro { background: #b42318; color: #fff; }
.btn:disabled { opacity: .5; cursor: not-allowed; }
.mensajes { list-style: none; padding: 0; margin: 0 0 14px; display: grid; gap: 8px; }
.mensajes li { padding: 10px 14px; border-radius: 6px; background: var(--verde-100); }
.mensajes .warning, .mensajes .error { background: var(--pend-bg); color: var(--pend-fg); }
.aviso-ia { padding: 8px 12px; border-left: 4px solid var(--indi-fg); background: var(--indi-bg); color: var(--indi-fg); border-radius: 4px; font-size: .9rem; }
.caso { display: grid; grid-template-columns: minmax(0, 1fr) 380px; gap: 16px; align-items: start; }
.visor { position: relative; background: #101828; border-radius: var(--radio); overflow: hidden; touch-action: none; }
.visor .lienzo { position: relative; transform-origin: 0 0; }
.visor img { display: block; width: 100%; height: auto; user-select: none; }
.visor svg { position: absolute; inset: 0; width: 100%; height: 100%; pointer-events: none; }
.visor rect.caja { fill: none; stroke: #fde047; stroke-width: 4; vector-effect: non-scaling-stroke; }
.visor rect.caja.rechazada { stroke: #98a2b3; stroke-dasharray: 6 4; }
.visor text { fill: #fde047; font-size: 14px; font-weight: 700; paint-order: stroke; stroke: #101828; stroke-width: 3px; }
.visor .controles { position: absolute; right: 8px; top: 8px; display: flex; gap: 6px; z-index: 2; }
.visor .controles button { background: rgba(16,24,40,.75); color: #fff; border: 0; border-radius: 6px; padding: 6px 10px; cursor: pointer; }
.ficha dl { display: grid; grid-template-columns: max-content 1fr; gap: 4px 12px; margin: 0; }
.ficha dt { color: var(--texto-2); }
.ficha dd { margin: 0; font-weight: 600; }
.decision fieldset { border: 0; padding: 0; margin: 0 0 10px; display: grid; gap: 6px; }
.decision label.opcion { display: flex; gap: 8px; align-items: center; padding: 8px 10px; border: 1px solid var(--borde); border-radius: 6px; cursor: pointer; }
.errorlist { color: #b42318; margin: 4px 0; padding-left: 18px; font-size: .88rem; }
.historial li { margin-bottom: 8px; }
#mapa { height: calc(100vh - 210px); min-height: 420px; border-radius: var(--radio); border: 1px solid var(--borde); }
.leyenda { display: flex; gap: 10px; flex-wrap: wrap; margin: 8px 0; }
.plano .hilera { display: grid; grid-template-columns: 70px 1fr 120px; gap: 8px; align-items: center; margin-bottom: 4px; font-size: .85rem; }
.plano .barra { position: relative; height: 22px; background: #eef2f0; border-radius: 4px; }
.plano .segmento { position: absolute; top: 2px; bottom: 2px; border-radius: 3px; background: #c9d6cf; display: flex; align-items: center; justify-content: center; font-size: .7rem; }
.plano .segmento.c-CONFIRMADO_POR_ESPECIALISTA { background: var(--conf-bg); outline: 2px solid var(--conf-fg); }
.plano .segmento.c-PENDIENTE_REVISION { background: var(--pend-bg); outline: 2px solid var(--pend-fg); }
.plano .segmento.c-EVIDENCIA_INSUFICIENTE { background: var(--insu-bg); outline: 2px solid var(--insu-fg); }
.plano .segmento.c-DESCARTADO { background: var(--desc-bg); }
.paginacion { display: flex; gap: 10px; align-items: center; margin-top: 12px; }
.solo-movil { display: none; }
.sr-only { position: absolute; width: 1px; height: 1px; overflow: hidden; clip: rect(0 0 0 0); white-space: nowrap; }
/* Tablet 768–1279 px: barra lateral solo con iconos; paneles de dos columnas apilados (Integrador §9.8) */
@media (max-width: 1279px) {
  .layout { grid-template-columns: 64px 1fr; }
  .lateral .marca, .lateral .grupo, .lateral .txt { display: none; }
  .lateral a { justify-content: center; }
  .caso { grid-template-columns: 1fr; }
}
/* < 768 px: consulta ocasional (enlace de WhatsApp en el celular); tablas como tarjetas */
@media (max-width: 767px) {
  .layout { grid-template-columns: 1fr; }
  .lateral { position: static; height: auto; display: flex; overflow-x: auto; padding: 6px; }
  .lateral a { flex: 0 0 auto; }
  .cabecera { padding: 10px 14px; }
  .contenido { padding: 14px; }
  .tabla thead { display: none; }
  .tabla tr { display: block; border: 1px solid var(--borde); border-radius: var(--radio); margin-bottom: 10px; background: #fff; }
  .tabla td { display: flex; justify-content: space-between; gap: 10px; border: 0; }
  .tabla td::before { content: attr(data-label); color: var(--texto-2); font-size: .8rem; }
  .solo-movil { display: initial; }
  .cabecera { flex-wrap: wrap; }
  .cabecera .usuario .nombre, .cabecera .entorno { display: none; }
  .caso aside { order: -1; }  /* en el celular primero la ubicación y la decisión (enlace de WhatsApp) */
}
@media print {
  .lateral, .cabecera, .filtros, .no-imprimir { display: none !important; }
  .layout { display: block; }
  body { background: #fff; }
}
```

### E.12 Visor del caso — `web/static/web/caso.js`

`web/static/web/caso.js`

```javascript
// web/static/web/caso.js — Visor del caso: acercar/alejar y mover la foto con sus cajas, cargar el original
// y atajos de teclado. La imagen y el SVG de cajas comparten el mismo contenedor transformado, así las cajas
// (en píxeles de la imagen analizada, viewBox del SVG) siempre coinciden con la foto, sea cual sea el tamaño.
(function () {
  "use strict";
  const visor = document.getElementById("visor");
  if (!visor) return;
  const lienzo = visor.querySelector(".lienzo");
  const img = lienzo.querySelector("img");
  const svg = lienzo.querySelector("svg");
  let escala = 1, x = 0, y = 0, arrastre = null;

  function aplicar() {
    escala = Math.min(8, Math.max(1, escala));
    const maxX = visor.clientWidth * (escala - 1), maxY = visor.clientHeight * (escala - 1);
    x = Math.min(0, Math.max(-maxX, x));
    y = Math.min(0, Math.max(-maxY, y));
    lienzo.style.transform = `translate(${x}px, ${y}px) scale(${escala})`;
  }
  function zoom(factor, cx, cy) {
    const r = visor.getBoundingClientRect();
    const px = (cx ?? r.width / 2), py = (cy ?? r.height / 2);
    const nueva = Math.min(8, Math.max(1, escala * factor));
    x = px - (px - x) * (nueva / escala);
    y = py - (py - y) * (nueva / escala);
    escala = nueva;
    aplicar();
  }
  visor.addEventListener("wheel", (e) => {
    e.preventDefault();
    const r = visor.getBoundingClientRect();
    zoom(e.deltaY < 0 ? 1.25 : 0.8, e.clientX - r.left, e.clientY - r.top);
  }, { passive: false });
  visor.addEventListener("pointerdown", (e) => {
    if (e.target.closest(".controles")) return;
    arrastre = { px: e.clientX, py: e.clientY, x, y };
    visor.setPointerCapture(e.pointerId);
  });
  visor.addEventListener("pointermove", (e) => {
    if (!arrastre) return;
    x = arrastre.x + (e.clientX - arrastre.px);
    y = arrastre.y + (e.clientY - arrastre.py);
    aplicar();
  });
  visor.addEventListener("pointerup", () => { arrastre = null; });

  const acciones = {
    cajas: () => { svg.style.display = svg.style.display === "none" ? "" : "none"; },
    mas: () => zoom(1.5),
    menos: () => zoom(1 / 1.5),
    reiniciar: () => { escala = 1; x = 0; y = 0; aplicar(); },
    original: (boton) => {  // ~4 MB: solo bajo pedido (consume ancho de banda de Cloudinary)
      if (img.dataset.cargado) return;
      boton.disabled = true; boton.textContent = "Cargando…";
      const alta = new Image();
      alta.onload = () => { img.src = alta.src; img.dataset.cargado = "1"; boton.textContent = "Original ✓"; };
      alta.onerror = () => { boton.disabled = false; boton.textContent = "Original"; };
      alta.src = visor.dataset.original;
    },
  };
  visor.querySelectorAll(".controles button").forEach((b) => b.addEventListener("click", () => acciones[b.dataset.accion](b)));

  // Atajos: solo seleccionan; guardar exige botón o Ctrl+Enter (nunca una decisión con una sola tecla).
  document.addEventListener("keydown", (e) => {
    const escribiendo = ["INPUT", "TEXTAREA", "SELECT"].includes(document.activeElement?.tagName);
    const form = document.getElementById("form-decision");
    if (e.key === "Enter" && (e.ctrlKey || e.metaKey) && form) { form.requestSubmit(); return; }
    if (escribiendo || e.ctrlKey || e.metaKey || e.altKey) return;
    const mapa = { c: "CONFIRMADO_POR_ESPECIALISTA", d: "DESCARTADO", i: "EVIDENCIA_INSUFICIENTE" };
    const valor = mapa[e.key.toLowerCase()];
    if (valor && form) {
      const radio = form.querySelector(`input[name="decision"][value="${valor}"]`);
      if (radio) { radio.checked = true; form.querySelector("textarea")?.focus(); e.preventDefault(); }
      return;
    }
    if (e.key === "b" || e.key === "B") acciones.cajas();
    if (e.key === "+") acciones.mas();
    if (e.key === "-") acciones.menos();
    if (e.key === "0") acciones.reiniciar();
  });
})();
```

### E.13 Mapa — `web/static/web/mapa.js`

`web/static/web/mapa.js`

```javascript
// web/static/web/mapa.js — Mapa Leaflet + OpenStreetMap. Pide GeoJSON a /mapa/datos/ con los filtros del formulario.
(function () {
  "use strict";
  const el = document.getElementById("mapa");
  if (!el || !window.L) return;
  const COLORES = {  // estado de revisión (no gravedad); tonos más saturados que las insignias para el mapa
    PENDIENTE_REVISION: "#f79009", CONFIRMADO_POR_ESPECIALISTA: "#d92d20",
    EVIDENCIA_INSUFICIENTE: "#7a5af8", DESCARTADO: "#98a2b3",
  };
  const mapa = L.map(el, { preferCanvas: true }).setView([-14.07, -75.73], 14);  // Ica (se ajusta a los datos)
  L.tileLayer("https://tile.openstreetmap.org/{z}/{x}/{y}.png", {
    maxZoom: 19,
    attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
  }).addTo(mapa);
  const capaLotes = L.geoJSON(null, { style: { color: "#1f5f46", weight: 2, fillOpacity: 0.05 } }).addTo(mapa);
  const capaCasos = L.layerGroup().addTo(mapa);
  const form = document.getElementById("filtros-mapa");
  const meta = document.getElementById("mapa-meta");

  function popup(p) {
    const div = document.createElement("div");
    const a = document.createElement("a");
    a.href = p.url; a.textContent = `${p.lot} · H${p.row} · ${p.lateral}`;
    div.append(a, document.createElement("br"),
      document.createTextNode(`${p.statusLabel} · ${p.capturedAt}`), document.createElement("br"),
      document.createTextNode([p.segment && `Seg. ${p.segment}`, p.marker && `Marc. ${p.marker}`].filter(Boolean).join(" · ")));
    return div;  // nodos de texto: nada del servidor se interpreta como HTML
  }

  async function cargar(ajustar) {
    const params = new URLSearchParams(new FormData(form));
    const resp = await fetch(`${el.dataset.url}?${params}`, { headers: { Accept: "application/json" }, credentials: "same-origin" });
    if (resp.status === 401 || resp.redirected) { window.location.reload(); return; }
    const data = await resp.json();
    capaLotes.clearLayers(); capaCasos.clearLayers();
    data.lots.forEach((f) => capaLotes.addData(f));
    const puntos = [];
    data.features.forEach((f) => {
      const [lon, lat] = f.geometry.coordinates, p = f.properties;
      const aprox = p.locationSource === "MARCADOR";
      const m = L.circleMarker([lat, lon], { radius: 7, color: COLORES[p.status] || "#475467", weight: 2,
        dashArray: aprox ? "3 3" : null, fillOpacity: aprox ? 0.25 : 0.8 }).bindPopup(popup(p));
      m.addTo(capaCasos); puntos.push([lat, lon]);
      if (p.accuracyM && p.id === el.dataset.caso) L.circle([lat, lon], { radius: p.accuracyM, weight: 1 }).addTo(capaCasos);
      if (p.id === el.dataset.caso) { mapa.setView([lat, lon], 19); m.openPopup(); ajustar = false; }
    });
    if (ajustar && puntos.length) mapa.fitBounds(puntos, { padding: [30, 30], maxZoom: 18 });
    meta.textContent = `${data.features.length} caso(s) en el mapa · ${data.meta.sinUbicacion} sin coordenadas` +
      (data.meta.truncated ? ` · se muestran los ${data.meta.limit} más recientes: acota los filtros` : "");
  }
  form.addEventListener("change", () => cargar(true));
  cargar(true);
})();
```

### E.14 Versiones de librerías — `web/static/web/vendor/VERSIONES.txt`

`web/static/web/vendor/VERSIONES.txt`

```text
htmx.org 2.0.11  (npm, dist/htmx.min.js)  — no usar la etiqueta "next" (4.x)
leaflet 1.9.4    (npm, dist/leaflet.js, dist/leaflet.css, dist/images/*) — no usar 2.0.0-alpha
Obtenidos con: npm pack htmx.org@2.0.11 leaflet@1.9.4  (03/10/2026)
```

---

## Anexo F. Pruebas de referencia

### F.1 Datos de prueba — `web/tests/factories.py`

`web/tests/factories.py`

```python
# web/tests/factories.py — Datos mínimos y deterministas para las pruebas (sin depender de web/demo.py).
import hashlib
import uuid
from datetime import timedelta

from django.utils import timezone

from campo.models import FieldLot, FieldRow, FieldSegment, Marker
from cuentas.models import AccountStatus, Device, Role, User
from evidencias.models import Capture, QualityResult, QualityStatus
from ia import services as ia
from ia.models import ModelConfig
from monitoreo.models import CaptureSequence, MonitoringPass, MonitoringSession
from notificaciones.models import NotificationRecipient

PASSWORD = "Clave-segura-2026"


def user(email, *roles, status=AccountStatus.ACTIVO, **extra):
    return User.objects.create_user(email, PASSWORD, roles=roles, full_name=email.split("@")[0].title(),
                                    status=status, **extra)


def world():
    """Catálogo pequeño (2 lotes), usuarios de cada rol, una sesión con una pasada y un modelo activo."""
    w = type("World", (), {})()
    w.lot = FieldLot.objects.create(id="SWG1", code="SWG 1", name="Lote 1 (Piscina)")
    w.lot2 = FieldLot.objects.create(id="SWG2", code="SWG 2", name="Lote 2 (Maíz)")
    w.row = FieldRow.objects.create(id="SWG1-H05", lot=w.lot, number=5, plant_count=385)
    w.row_b = FieldRow.objects.create(id="SWG1-H06", lot=w.lot, number=6, plant_count=386)
    w.row2 = FieldRow.objects.create(id="SWG2-H01", lot=w.lot2, number=1, plant_count=251)
    w.segment = FieldSegment.objects.create(id="SWG1-S1", row=w.row, code="SWG 1-S1", start_plant=180,
                                            end_plant=204, is_pilot=True)
    w.marker = Marker.objects.create(id="SWG1-M1", row=w.row, segment=w.segment, code="M1", position="INICIO",
                                     lat=-14.06, lon=-75.73)
    w.model = ModelConfig.objects.create(name="det", version="v1", weights_uri="x", classes=["chanchito_blanco"],
                                         active=True)
    w.admin = user("admin@x.pe", Role.ADMINISTRADOR, is_staff=True)
    w.esp = user("esp@x.pe", Role.ESPECIALISTA_FITOSANITARIO)
    w.esp2 = user("esp2@x.pe", Role.ESPECIALISTA_FITOSANITARIO)
    w.sup = user("sup@x.pe", Role.SUPERVISOR)
    w.op = user("op@x.pe", Role.OPERADOR_CAMPO)
    w.device = Device.objects.create(device_id=uuid.uuid4(), platform="android", model="Moto", user=w.op)
    now = timezone.now()
    w.session = MonitoringSession.objects.create(
        session_id=uuid.uuid4(), operator=w.op, controller_device=w.device, status="CLOSED", mode="MANUAL",
        started_at=now - timedelta(hours=2), ended_at=now - timedelta(hours=1), app_version="0.3.0",
        config_version="CFG-4", quality_profile_version="Q0")
    w.pass_a = make_pass(w, w.row, "LATERAL_A")
    w.seq_n = 0
    return w


def make_pass(w, row, lateral, status="COMPLETED"):
    return MonitoringPass.objects.create(
        pass_id=uuid.uuid4(), session=w.session, lot=row.lot, row=row, lateral_code=lateral, pass_order=1,
        direction="ASCENDENTE", status=status, started_at=timezone.now() - timedelta(minutes=90))


def capture(w, gps=True, marker=True, quality=QualityStatus.UTILIZABLE, monitoring_pass=None, retake_context=None,
            role="CAMERA_1", sequence=None):
    p = monitoring_pass or w.pass_a
    if sequence is None:
        w.seq_n += 1
        sequence = CaptureSequence.objects.create(
            sequence_id=uuid.uuid4(), monitoring_pass=p, session=w.session, sequence_number=w.seq_n, mode="MANUAL",
            status="COMPLETE", segment=w.segment if marker else None, marker=w.marker if marker else None,
            lat=-14.0601 if gps else None, lon=-75.7302 if gps else None, gps_accuracy_m=4.0 if gps else None,
            issued_at=timezone.now() - timedelta(minutes=80))
    cid = uuid.uuid4()
    cap = Capture.objects.create(
        capture_id=cid, sequence=sequence, session=w.session, monitoring_pass=p, lateral_code=p.lateral_code,
        device=w.device, camera_role=role, camera_user=w.op, operator_user=w.op,
        captured_at=timezone.now() - timedelta(minutes=80), width=3000, height=4000, size_bytes=3_900_000,
        md5=hashlib.md5(str(cid).encode()).hexdigest(), quality_status=quality, app_version="0.3.0",
        retake_context=retake_context, cloudinary_public_id=f"riachuelo/dev/{w.session.pk}/{p.pk}/{cid}",
        cloudinary_version=1759500000, cloudinary_bytes=3_900_000, cloudinary_format="jpg")
    QualityResult.objects.create(capture=cap, status=quality, profile_version="Q0")
    return cap


BOX = {"class_name": "chanchito_blanco", "confidence": 0.81, "x_min": 100, "y_min": 200, "x_max": 400, "y_max": 520}


def analyzed(w, boxes=(BOX,), **kw):
    """Captura confirmada → tarea encolada → worker la toma → guarda resultado (abre caso si hay cajas)."""
    cap = capture(w, **kw)
    ia.enqueue_analysis(cap)
    task = ia.claim_next_task("test")
    task, case = ia.save_analysis_result(task, list(boxes), 3000, 4000, 120, "v1", {"ok": True})
    return cap, task, case


def recipient(name="Jefe", phone="+51987654321", lots=(), opt_in=True, active=True, **extra):
    r = NotificationRecipient.objects.create(full_name=name, phone_e164=phone, role_label="JEFE_FUNDO",
                                             opt_in_at=timezone.now() if opt_in else None, active=active, **extra)
    if lots:
        r.lots.set(lots)
    return r
```

### F.2 `revision/tests/test_services.py`

`revision/tests/test_services.py`

```python
import threading

from django.core.exceptions import PermissionDenied, ValidationError
from django.db import connection
from django.test import TestCase, TransactionTestCase, skipUnlessDBFeature

from auditoria.models import AuditEvent
from evidencias.models import QualityStatus
from ia.models import AiStatus, AiTask, DetectionReview
from ia import services as ia
from notificaciones.models import Notification, NotificationStatus
from revision import services as rv
from revision.models import Case, CaseNotificationStatus, HumanReview, LocationSource, ReviewStatus
from web.tests import factories as F

C, D, I = (ReviewStatus.CONFIRMADO_POR_ESPECIALISTA, ReviewStatus.DESCARTADO, ReviewStatus.EVIDENCIA_INSUFICIENTE)


class AperturaDeCasos(TestCase):
    def setUp(self):
        self.w = F.world()

    def test_indicio_abre_caso_con_ubicacion_gps(self):
        cap, task, case = F.analyzed(self.w)
        self.assertEqual(task.status, AiStatus.INDICIO_SUGERIDO_POR_IA)
        self.assertEqual(case.origin, Case.Origin.IA)
        self.assertEqual((case.lot_id, case.row_id, case.segment_id, case.marker_id), ("SWG1", "SWG1-H05", "SWG1-S1", "SWG1-M1"))
        self.assertEqual(case.location_source, LocationSource.GPS)
        self.assertEqual((case.detections_count, case.max_confidence), (1, 0.81))
        self.assertTrue(AuditEvent.objects.filter(action="CASO_ABIERTO", entity_id=str(case.pk)).exists())

    def test_sin_gps_usa_coordenadas_del_marcador(self):
        _, _, case = F.analyzed(self.w, gps=False)
        self.assertEqual(case.location_source, LocationSource.MARCADOR)
        self.assertEqual((case.lat, case.lon, case.gps_accuracy_m), (-14.06, -75.73, None))

    def test_sin_gps_ni_marcador_queda_sin_coordenadas(self):
        _, _, case = F.analyzed(self.w, gps=False, marker=False)
        self.assertEqual(case.location_source, LocationSource.NINGUNA)
        self.assertIsNone(case.lat)

    def test_repeticion_usa_el_contexto_de_la_repeticion(self):
        ctx = {"requestedAt": "2026-11-09T14:00:00Z", "segmentId": None, "markerId": None, "lat": -14.07,
               "lon": -75.74, "gpsAccuracyM": 6.0, "gpsTimestamp": "2026-11-09T14:00:00Z"}
        _, _, case = F.analyzed(self.w, retake_context=ctx)
        self.assertEqual((case.lat, case.lon, case.gps_accuracy_m, case.segment_id), (-14.07, -75.74, 6.0, None))

    def test_sin_cajas_no_abre_caso(self):
        _, task, case = F.analyzed(self.w, boxes=())
        self.assertEqual(task.status, AiStatus.SIN_INDICIOS_IA)
        self.assertIsNone(case)
        self.assertFalse(Case.objects.exists())

    def test_foto_rechazada_por_calidad_no_se_analiza(self):
        cap = F.capture(self.w, quality=QualityStatus.REPETIR_NITIDEZ)
        self.assertIsNone(ia.enqueue_analysis(cap))
        self.assertFalse(AiTask.objects.exists())

    def test_analisis_nuevo_no_cambia_una_decision(self):
        cap, task, case = F.analyzed(self.w)
        rv.decide_case(case.pk, self.w.esp, D, "Mancha de polvo")
        task.status = AiStatus.PENDIENTE_DE_ANALISIS
        task.save()
        task = ia.claim_next_task("t")
        ia.save_analysis_result(task, [F.BOX, F.BOX], 3000, 4000, 100, "v1")
        case.refresh_from_db()
        self.assertEqual(case.status, D)
        self.assertTrue(AuditEvent.objects.filter(action="ANALISIS_POSTERIOR_A_DECISION").exists())

    def test_caso_manual_sobre_foto_sin_indicios(self):
        cap, _, _ = F.analyzed(self.w, boxes=())
        with self.assertRaises(PermissionDenied):
            rv.open_manual_case(cap.pk, self.w.sup)
        case, created = rv.open_manual_case(cap.pk, self.w.esp)
        self.assertTrue(created)
        self.assertEqual((case.origin, case.detections_count), (Case.Origin.MANUAL, 0))
        again, created = rv.open_manual_case(cap.pk, self.w.esp)
        self.assertEqual((again.pk, created), (case.pk, False))

    def test_caso_manual_exige_analisis_terminado(self):
        cap = F.capture(self.w)
        ia.enqueue_analysis(cap)  # PENDIENTE_DE_ANALISIS
        with self.assertRaises(ValidationError):
            rv.open_manual_case(cap.pk, self.w.esp)


class Decisiones(TestCase):
    def setUp(self):
        self.w = F.world()
        self.cap, self.task, self.case = F.analyzed(self.w, boxes=(F.BOX, dict(F.BOX, confidence=0.4)))
        self.jefe = F.recipient("Jefe de fundo")
        self.sup_lote = F.recipient("Supervisor SWG1", phone="+51911111111", lots=[self.w.lot])
        F.recipient("Supervisor SWG2", phone="+51922222222", lots=[self.w.lot2])
        F.recipient("Sin consentimiento", phone="+51933333333", opt_in=False)
        F.recipient("Inactivo", phone="+51944444444", active=False)

    def test_solo_el_especialista_decide(self):
        for u in (self.w.sup, self.w.admin, self.w.op):
            with self.assertRaises(PermissionDenied):
                rv.decide_case(self.case.pk, u, C, "x")
        self.assertFalse(HumanReview.objects.exists())

    def test_confirmar_registra_todo_en_una_transaccion(self):
        dets = list(self.task.detections.order_by("-confidence"))
        review = rv.decide_case(self.case.pk, self.w.esp, C, "Colonias en racimo", "chanchito_blanco", [dets[1].pk])
        self.case.refresh_from_db()
        self.assertEqual(self.case.status, C)
        self.assertEqual(self.case.decided_by, self.w.esp)
        self.assertEqual(review.rejected_detection_ids, [dets[1].pk])
        self.assertEqual({d.pk: d.review_status for d in self.task.detections.all()},
                         {dets[0].pk: DetectionReview.CONFIRMADO_POR_ESPECIALISTA, dets[1].pk: DetectionReview.DESCARTADO})
        avisos = Notification.objects.filter(review=review)
        self.assertEqual({n.recipient_name for n in avisos}, {"Jefe de fundo", "Supervisor SWG1"})  # alcance y consentimiento
        self.assertTrue(all(n.status == NotificationStatus.PENDIENTE_ENVIO for n in avisos))
        self.assertEqual(self.case.notification_status, CaseNotificationStatus.PENDIENTE_ENVIO)
        self.assertTrue(AuditEvent.objects.filter(action="CASO_DECIDIDO", entity_id=str(self.case.pk)).exists())

    def test_descartar_o_insuficiente_no_avisa(self):
        rv.decide_case(self.case.pk, self.w.esp, D, "")
        self.case.refresh_from_db()
        self.assertEqual(self.case.notification_status, CaseNotificationStatus.NO_APLICA)
        self.assertFalse(Notification.objects.exists())
        self.assertFalse(self.task.detections.exclude(review_status=DetectionReview.DESCARTADO).exists())

    def test_observacion_obligatoria_al_confirmar_y_en_insuficiente(self):
        for decision in (C, I):
            with self.assertRaises(ValidationError) as ctx:
                rv.decide_case(self.case.pk, self.w.esp, decision, "   ")
            self.assertIn("observation", ctx.exception.error_dict)

    def test_cajas_y_clase_deben_pertenecer_al_caso(self):
        otro_cap, otra_task, _ = F.analyzed(self.w)
        ajena = otra_task.detections.first().pk
        with self.assertRaises(ValidationError) as ctx:
            rv.decide_case(self.case.pk, self.w.esp, C, "obs", rejected_detection_ids=[ajena])
        self.assertIn("rejected_detection_ids", ctx.exception.error_dict)
        with self.assertRaises(ValidationError) as ctx:
            rv.decide_case(self.case.pk, self.w.esp, C, "obs", confirmed_class="filoxera")
        self.assertIn("confirmed_class", ctx.exception.error_dict)

    def test_no_se_decide_dos_veces(self):
        rv.decide_case(self.case.pk, self.w.esp, D, "")
        with self.assertRaises(rv.CaseAlreadyDecided) as ctx:
            rv.decide_case(self.case.pk, self.w.esp2, C, "obs")
        self.assertEqual(ctx.exception.case.decided_by, self.w.esp)
        self.assertEqual(HumanReview.objects.count(), 1)

    def test_correccion_anula_avisos_pendientes_y_conserva_historial(self):
        first = rv.decide_case(self.case.pk, self.w.esp, C, "Colonias")
        enviado = Notification.objects.filter(review=first).order_by("pk").first()
        enviado.status = NotificationStatus.ENVIADO
        enviado.save()
        second = rv.correct_decision(self.case.pk, self.w.esp2, D, "Era pelusa", "Revisé el original", first.pk)
        self.case.refresh_from_db()
        first.refresh_from_db()
        self.assertEqual(self.case.status, D)
        self.assertFalse(first.is_current)
        self.assertEqual(second.supersedes, first)
        estados = dict(Notification.objects.values_list("recipient_name", "status"))
        self.assertEqual(sorted(estados.values()), sorted([NotificationStatus.NO_APLICA, NotificationStatus.ENVIADO]))
        self.assertEqual(self.case.notification_status, CaseNotificationStatus.ENVIADO)  # lo enviado no se retira

    def test_correccion_de_descartado_a_confirmado_avisa_una_vez(self):
        first = rv.decide_case(self.case.pk, self.w.esp, D, "")
        second = rv.correct_decision(self.case.pk, self.w.esp, C, "Sí hay colonias", "Segunda mirada", first.pk)
        self.assertEqual(Notification.objects.filter(review=second).count(), 2)
        third = rv.correct_decision(self.case.pk, self.w.esp, C, "Colonias (texto corregido)", "Redacción", second.pk)
        self.assertEqual(Notification.objects.filter(review=third).count(), 0)  # no se repite el aviso

    def test_correccion_con_version_vieja(self):
        first = rv.decide_case(self.case.pk, self.w.esp, D, "")
        rv.correct_decision(self.case.pk, self.w.esp, I, "Borrosa", "Mejor no decidir", first.pk)
        with self.assertRaises(rv.StaleReview):
            rv.correct_decision(self.case.pk, self.w.esp2, C, "obs", "motivo", first.pk)

    def test_correccion_exige_motivo(self):
        first = rv.decide_case(self.case.pk, self.w.esp, D, "")
        with self.assertRaises(ValidationError):
            rv.correct_decision(self.case.pk, self.w.esp, C, "obs", " ", first.pk)


@skipUnlessDBFeature("has_select_for_update")
class DecisionConcurrente(TransactionTestCase):
    """Dos especialistas pulsan «Guardar» a la vez: SELECT … FOR UPDATE deja pasar solo a uno."""

    def test_un_solo_ganador(self):
        w = F.world()
        _, _, case = F.analyzed(w)
        results, barrier = [], threading.Barrier(2)

        def decide(user, decision):
            try:
                barrier.wait()
                rv.decide_case(case.pk, user, decision, "obs")
                results.append("ok")
            except rv.CaseAlreadyDecided:
                results.append("409")
            finally:
                connection.close()

        threads = [threading.Thread(target=decide, args=(w.esp, C)), threading.Thread(target=decide, args=(w.esp2, D))]
        for t in threads:
            t.start()
        for t in threads:
            t.join()
        self.assertEqual(sorted(results), ["409", "ok"])
        self.assertEqual(HumanReview.objects.filter(case=case).count(), 1)
```

### F.3 `notificaciones/tests/test_services.py`

`notificaciones/tests/test_services.py`

```python
from datetime import timedelta
from unittest import mock

from django.test import TestCase, override_settings
from django.utils import timezone

from notificaciones import services as notif
from notificaciones.models import Notification, NotificationStatus
from notificaciones.whatsapp import CloudApiClient, WhatsAppError
from revision import services as rv
from revision.models import CaseNotificationStatus, ReviewStatus
from web.tests import factories as F


class FakeClient:
    def __init__(self, fail=None):
        self.sent, self.fail = [], fail

    def send(self, payload):
        if self.fail:
            raise self.fail
        self.sent.append(payload)
        return f"wamid.{len(self.sent)}"


@override_settings(PUBLIC_BASE_URL="https://riachuelo.example.pe", WHATSAPP_TEMPLATE="caso_confirmado",
                   WHATSAPP_TEMPLATE_LANG="es")
class AvisosWhatsApp(TestCase):
    def setUp(self):
        self.w = F.world()
        _, _, self.case = F.analyzed(self.w)
        F.recipient("Jefe de fundo", phone="+51987654321")
        rv.decide_case(self.case.pk, self.w.esp, ReviewStatus.CONFIRMADO_POR_ESPECIALISTA, "Colonias")
        self.n = Notification.objects.get()

    def test_plantilla_con_ubicacion_y_enlace_al_caso(self):
        p = notif.build_whatsapp_payload(self.n)
        self.assertEqual(p["to"], "51987654321")
        self.assertEqual(p["type"], "template")
        self.assertEqual(p["template"]["name"], "caso_confirmado")
        params = [x["text"] for x in p["template"]["components"][0]["parameters"]]
        self.assertEqual(params[:4], ["SWG 1", "5", "Lateral A", "segmento SWG 1-S1 · marcador M1"])
        self.assertEqual(params[5], f"https://riachuelo.example.pe/casos/{self.case.pk}/")
        self.assertTrue(all("\n" not in t and "\t" not in t for t in params))
        self.assertNotIn("cloudinary", str(p))  # nunca la imagen: solo el enlace al caso (requiere login)

    def test_envio_exitoso(self):
        client = FakeClient()
        self.assertEqual(notif.process_pending_notifications(client, "w1"), 1)
        self.n.refresh_from_db()
        self.case.refresh_from_db()
        self.assertEqual((self.n.status, self.n.provider_message_id, self.n.attempts), (NotificationStatus.ENVIADO, "wamid.1", 1))
        self.assertEqual(self.case.notification_status, CaseNotificationStatus.ENVIADO)
        self.assertEqual(notif.process_pending_notifications(client, "w1"), 0)  # no se reenvía

    def test_error_temporal_reintenta_con_espera_y_luego_falla(self):
        client = FakeClient(fail=WhatsAppError("HTTP 503"))
        notif.process_pending_notifications(client, "w1")
        self.n.refresh_from_db()
        self.assertEqual((self.n.status, self.n.attempts), (NotificationStatus.PENDIENTE_ENVIO, 1))
        self.assertGreater(self.n.available_at, timezone.now())
        for _ in range(notif.MAX_ATTEMPTS):
            Notification.objects.update(available_at=timezone.now() - timedelta(seconds=1))
            notif.process_pending_notifications(client, "w1")
        self.n.refresh_from_db()
        self.case.refresh_from_db()
        self.assertEqual((self.n.status, self.n.attempts), (NotificationStatus.ERROR_ENVIO, notif.MAX_ATTEMPTS))
        self.assertEqual(self.case.notification_status, CaseNotificationStatus.ERROR_ENVIO)
        self.assertEqual(self.case.status, ReviewStatus.CONFIRMADO_POR_ESPECIALISTA)  # la decisión no cambia

    def test_error_definitivo_no_reintenta(self):
        notif.process_pending_notifications(FakeClient(fail=WhatsAppError("HTTP 400", retryable=False)), "w1")
        self.n.refresh_from_db()
        self.assertEqual((self.n.status, self.n.attempts), (NotificationStatus.ERROR_ENVIO, 1))

    def test_aviso_tomado_no_lo_toma_otro_worker(self):
        self.assertIsNotNone(notif.claim_next_notification("w1"))
        self.assertIsNone(notif.claim_next_notification("w2"))  # arrendado (locked_until)

    @override_settings(WHATSAPP_TOKEN="t", WHATSAPP_PHONE_ID="123", WHATSAPP_GRAPH_VERSION="v99.0")
    def test_cliente_cloud_api_clasifica_errores(self):
        client = CloudApiClient()
        for code, retryable in ((429, True), (503, True), (400, False), (401, False)):
            resp = mock.Mock(status_code=code, text="{}")
            with mock.patch("requests.post", return_value=resp):
                with self.assertRaises(WhatsAppError) as ctx:
                    client.send({"to": "51"})
                self.assertEqual(ctx.exception.retryable, retryable, code)
        ok = mock.Mock(status_code=200, json=lambda: {"messages": [{"id": "wamid.X"}]})
        with mock.patch("requests.post", return_value=ok) as post:
            self.assertEqual(client.send({"to": "51"}), "wamid.X")
            self.assertEqual(post.call_args.args[0], "https://graph.facebook.com/v99.0/123/messages")
```

### F.4 `ia/tests/test_services.py`

`ia/tests/test_services.py`

```python
from datetime import timedelta

from django.test import TestCase
from django.utils import timezone

from auditoria.models import AuditEvent
from ia import services as ia
from ia.models import AiStatus
from web.tests import factories as F


class ColaDeAnalisis(TestCase):
    def setUp(self):
        self.w = F.world()

    def test_reintentos_con_espera_y_error_final(self):
        cap = F.capture(self.w)
        ia.enqueue_analysis(cap)
        for intento in range(1, ia.MAX_ATTEMPTS + 1):
            task = ia.claim_next_task("w")
            self.assertEqual(task.attempts, intento)
            task = ia.mark_analysis_failed(task, "timeout descargando")
            if intento < ia.MAX_ATTEMPTS:
                self.assertEqual(task.status, AiStatus.PENDIENTE_DE_ANALISIS)
                self.assertGreater(task.available_at, timezone.now())
                type(task).objects.filter(pk=task.pk).update(available_at=timezone.now() - timedelta(seconds=1))
        self.assertEqual(task.status, AiStatus.ERROR_DE_ANALISIS)

    def test_arrendamiento_vencido_vuelve_a_la_cola(self):
        cap = F.capture(self.w)
        ia.enqueue_analysis(cap)
        task = ia.claim_next_task("w1")
        self.assertIsNone(ia.claim_next_task("w2"))
        type(task).objects.filter(pk=task.pk).update(locked_until=timezone.now() - timedelta(seconds=1))
        self.assertEqual(ia.claim_next_task("w2").pk, task.pk)

    def test_reencolar_solo_errores_y_queda_auditado(self):
        cap = F.capture(self.w)
        task = ia.enqueue_analysis(cap)
        with self.assertRaises(ia.InvalidTaskState):
            ia.requeue_failed(task.pk, self.w.admin)
        type(task).objects.filter(pk=task.pk).update(status=AiStatus.ERROR_DE_ANALISIS, attempts=3)
        task = ia.requeue_failed(task.pk, self.w.admin)
        self.assertEqual((task.status, task.attempts), (AiStatus.PENDIENTE_DE_ANALISIS, 0))
        self.assertTrue(AuditEvent.objects.filter(action="TAREA_IA_REENCOLADA", user=self.w.admin).exists())
```

### F.5 `auditoria/tests/test_seguridad_bd.py`

`auditoria/tests/test_seguridad_bd.py`

```python
from unittest import skipUnless

from django.db import connection
from django.test import TestCase


@skipUnless(connection.vendor == "postgresql", "Solo PostgreSQL (Supabase)")
class SeguridadSupabase(TestCase):
    """Tras `migrate` (la BD de pruebas también se crea con migrate): RLS en todas las tablas y sin privilegios
    para los roles de la Data API. En el PostgreSQL local de pruebas se crean antes `anon` y `authenticated`."""

    def test_rls_en_todas_las_tablas_de_public(self):
        with connection.cursor() as cur:
            cur.execute("SELECT c.relname FROM pg_class c JOIN pg_namespace n ON n.oid = c.relnamespace "
                        "WHERE n.nspname = 'public' AND c.relkind = 'r' AND NOT c.relrowsecurity")
            self.assertEqual(cur.fetchall(), [])

    def test_roles_de_la_data_api_sin_acceso(self):
        with connection.cursor() as cur:
            cur.execute("SELECT rolname FROM pg_roles WHERE rolname IN ('anon', 'authenticated')")
            roles = [r for (r,) in cur.fetchall()]
            if not roles:
                self.skipTest("El clúster no tiene los roles de Supabase")
            for role in roles:
                for table in ("users", "review_cases", "captures", "notifications", "audit_events"):
                    cur.execute("SELECT has_table_privilege(%s, %s, 'SELECT')", [role, f"public.{table}"])
                    self.assertFalse(cur.fetchone()[0], f"{role} puede leer {table}")
```

### F.6 `web/tests/test_views.py`

`web/tests/test_views.py`

```python
import uuid

from django.core.cache import cache
from django.test import Client, TestCase, override_settings
from django.urls import URLPattern, reverse

from auditoria.models import AuditEvent
from cuentas.models import AccountStatus, Role, User
from ia.models import AiStatus, AiTask
from monitoreo.models import Incident
from notificaciones.models import NotificationRecipient
from revision import services as rv
from revision.models import Case, ReviewStatus
from web import messages as M
from web import urls as web_urls
from web.permissions import PERMISOS
from web.tests import factories as F

PUBLIC = {"login", "logout", "cambiar_contrasena"}
HX = {"HTTP_HX_REQUEST": "true"}


def login(client, user):
    client.force_login(user)
    return client


class Ingreso(TestCase):
    def setUp(self):
        cache.clear()
        self.w = F.world()

    def post(self, email, password=F.PASSWORD):
        return self.client.post(reverse("web:login"), {"email": email, "password": password})

    def test_credenciales_incorrectas_mensaje_generico(self):
        for email in ("esp@x.pe", "noexiste@x.pe"):
            r = self.post(email, "mala")
            self.assertContains(r, M.LOGIN_INVALIDO)

    def test_estado_de_cuenta_solo_con_contrasena_correcta(self):
        F.user("pend@x.pe", status=AccountStatus.PENDIENTE_APROBACION)
        self.assertContains(self.post("pend@x.pe", "mala"), M.LOGIN_INVALIDO)
        self.assertContains(self.post("pend@x.pe"), M.CUENTA_PENDIENTE)

    def test_operador_no_entra_a_la_web(self):
        self.assertContains(self.post("op@x.pe"), M.SIN_ACCESO_WEB)

    def test_bloqueo_tras_intentos_fallidos(self):
        for _ in range(5):
            self.post("esp@x.pe", "mala")
        self.assertContains(self.post("esp@x.pe"), "Demasiados intentos")

    def test_ingreso_y_redireccion(self):
        r = self.post("esp@x.pe")
        self.assertRedirects(r, reverse("web:dashboard"))
        self.assertTrue(AuditEvent.objects.filter(action="INGRESO_WEB").exists())

    def test_contrasena_temporal_obliga_a_cambiarla(self):
        self.w.sup.must_change_password = True
        self.w.sup.save()
        self.assertRedirects(self.post("sup@x.pe"), reverse("web:cambiar_contrasena"))
        r = self.client.get(reverse("web:bandeja"))
        self.assertRedirects(r, reverse("web:cambiar_contrasena"))
        r = self.client.post(reverse("web:cambiar_contrasena"), {
            "old_password": F.PASSWORD, "new_password1": "Otra-clave-larga-77", "new_password2": "Otra-clave-larga-77"})
        self.assertRedirects(r, reverse("web:dashboard"))
        self.w.sup.refresh_from_db()
        self.assertFalse(self.w.sup.must_change_password)

    def test_bloquear_cuenta_cierra_la_sesion_web(self):
        login(self.client, self.w.sup)
        self.assertEqual(self.client.get(reverse("web:bandeja")).status_code, 200)
        User.objects.filter(pk=self.w.sup.pk).update(status=AccountStatus.BLOQUEADO)
        r = self.client.get(reverse("web:bandeja"))
        self.assertEqual(r.status_code, 302)
        self.assertIn(reverse("web:login"), r["Location"])

    def test_next_no_permite_redireccion_externa(self):
        r = self.client.post(reverse("web:login") + "?next=https://malo.example.com/",
                             {"email": "esp@x.pe", "password": F.PASSWORD})
        self.assertRedirects(r, reverse("web:dashboard"))


class MatrizDePermisos(TestCase):
    """W-07: toda vista de la web (salvo ingreso/salida/contraseña) tiene @web_view y respeta la matriz 7.2."""

    def setUp(self):
        self.w = F.world()
        self.cap, self.task, self.case = F.analyzed(self.w)

    def test_todas_las_rutas_estan_protegidas(self):
        for p in web_urls.urlpatterns:
            assert isinstance(p, URLPattern)
            if p.name in PUBLIC:
                continue
            self.assertIn(getattr(p.callback, "web_permission", None), PERMISOS, f"{p.name} sin @web_view")

    def test_paginas_por_rol(self):
        pages = {
            "web:dashboard": [], "web:bandeja": [], "web:caso": [self.case.pk], "web:captura": [self.cap.pk],
            "web:mapa": [], "web:mapa_datos": [], "web:plano": [], "web:sesiones": [], "web:sesion": [self.w.session.pk],
            "web:reportes": [], "web:exportar_casos": [], "web:notificaciones": [], "web:ia": [],
            "web:usuarios": [], "web:dispositivos": [], "web:destinatarios": [], "web:auditoria": [],
        }
        for name, args in pages.items():
            perm = getattr(reverse_view(name), "web_permission")
            for user in (self.w.admin, self.w.esp, self.w.sup):
                c = login(Client(), user)
                r = c.get(reverse(name, args=args))
                expected = 200 if user.has_role(*PERMISOS[perm]) else 403
                self.assertEqual(r.status_code, expected, f"{name} {user.email}")

    def test_anonimo_va_al_login_y_htmx_recibe_redireccion_de_cliente(self):
        r = self.client.get(reverse("web:bandeja"))
        self.assertEqual(r.status_code, 302)
        r = self.client.get(reverse("web:bandeja"), **HX, HTTP_HX_CURRENT_URL="http://testserver/casos/?lote=SWG1")
        self.assertIn("/ingresar/?next=", r["HX-Redirect"])

    def test_decidir_solo_especialista(self):
        for user in (self.w.sup, self.w.admin):
            r = login(Client(), user).post(reverse("web:caso_decidir", args=[self.case.pk]),
                                           {"decision": "DESCARTADO"})
            self.assertEqual(r.status_code, 403)
        self.case.refresh_from_db()
        self.assertEqual(self.case.status, ReviewStatus.PENDIENTE_REVISION)


def reverse_view(name):
    from django.urls import resolve

    sample = {"web:caso": [uuid.uuid4()], "web:captura": [uuid.uuid4()], "web:sesion": [uuid.uuid4()]}
    return resolve(reverse(name, args=sample.get(name, []))).func


class BandejaYCaso(TestCase):
    def setUp(self):
        self.w = F.world()
        self.cases = [F.analyzed(self.w)[2] for _ in range(3)]
        rv.decide_case(self.cases[2].pk, self.w.esp, ReviewStatus.DESCARTADO, "")
        self.c = login(Client(), self.w.esp)

    def test_bandeja_muestra_pendientes_por_defecto(self):
        r = self.c.get(reverse("web:bandeja"))
        self.assertEqual(r.context["page"].paginator.count, 2)
        r = self.c.get(reverse("web:bandeja"), {"estado": ""})
        self.assertEqual(r.context["page"].paginator.count, 3)

    def test_htmx_devuelve_solo_el_fragmento_que_se_reemplaza_a_si_mismo(self):
        r = self.c.get(reverse("web:bandeja"), {"lote": "SWG1"}, **HX)
        html = r.content.decode()
        self.assertNotIn("<html", html)
        self.assertIn('id="bandeja"', html)
        self.assertIn('hx-get="/casos/?lote=SWG1"', html)
        self.assertIn("HX-Request", r["Vary"])

    def test_bandeja_sin_consultas_n_mas_1(self):
        # 6 consultas fijas: sesión, usuario, roles, conteo, página de casos (con select_related) y lotes del filtro
        with self.assertNumQueries(6):
            self.c.get(reverse("web:bandeja"), {"estado": ""})
        for _ in range(5):
            F.analyzed(self.w)
        with self.assertNumQueries(6):
            self.c.get(reverse("web:bandeja"), {"estado": ""})

    def _queries(self):
        from django.db import connection
        from django.test.utils import CaptureQueriesContext

        with CaptureQueriesContext(connection) as ctx:
            self.c.get(reverse("web:bandeja"), {"estado": ""})
        return len(ctx.captured_queries)

    def test_caso_con_url_firmada_y_cajas_en_coordenadas_del_analisis(self):
        case = self.cases[0]
        r = self.c.get(reverse("web:caso", args=[case.pk]))
        html = r.content.decode()
        self.assertIn("/image/authenticated/s--", html)
        self.assertIn("/t_revision/", html)
        self.assertNotIn("secreto-de-prueba", html)  # nunca el API secret
        self.assertIn('viewBox="0 0 3000 4000"', html)
        self.assertIn('<rect class="caja ', html)
        self.assertIn(M.IA_AVISO, html)
        self.assertIn("no-store", r["Cache-Control"])
        self.assertEqual(r["Referrer-Policy"], "strict-origin-when-cross-origin")

    def test_decision_htmx_ok_redirige_y_avisa(self):
        F.recipient()
        case = self.cases[0]
        r = self.c.post(reverse("web:caso_decidir", args=[case.pk]),
                        {"decision": "CONFIRMADO_POR_ESPECIALISTA", "observation": "Colonias"}, **HX)
        self.assertEqual(r.status_code, 200)
        self.assertEqual(r["HX-Redirect"], reverse("web:caso", args=[case.pk]))
        case.refresh_from_db()
        self.assertEqual(case.status, ReviewStatus.CONFIRMADO_POR_ESPECIALISTA)

    def test_guardar_y_siguiente(self):
        r = self.c.post(reverse("web:caso_decidir", args=[self.cases[0].pk]),
                        {"decision": "DESCARTADO", "siguiente": "1"}, **HX)
        self.assertEqual(r["HX-Redirect"], reverse("web:caso", args=[self.cases[1].pk]))

    def test_decision_invalida_422_con_errores_en_el_fragmento(self):
        r = self.c.post(reverse("web:caso_decidir", args=[self.cases[0].pk]),
                        {"decision": "CONFIRMADO_POR_ESPECIALISTA", "observation": ""}, **HX)
        self.assertEqual(r.status_code, 422)
        self.assertIn('id="panel-decision"', r.content.decode())
        self.assertContains(r, "Registra la observación fitosanitaria.", status_code=422)

    def test_caso_ya_decidido_409(self):
        rv.decide_case(self.cases[0].pk, self.w.esp2, ReviewStatus.DESCARTADO, "")
        r = self.c.post(reverse("web:caso_decidir", args=[self.cases[0].pk]),
                        {"decision": "CONFIRMADO_POR_ESPECIALISTA", "observation": "x"}, **HX)
        self.assertEqual(r.status_code, 409)
        self.assertContains(r, "ya fue decidido por Esp2", status_code=409)

    def test_csrf_obligatorio(self):
        c = Client(enforce_csrf_checks=True)
        c.force_login(self.w.esp)
        r = c.post(reverse("web:caso_decidir", args=[self.cases[0].pk]), {"decision": "DESCARTADO"})
        self.assertEqual(r.status_code, 403)

    def test_supervisor_ve_el_caso_sin_formulario(self):
        r = login(Client(), self.w.sup).get(reverse("web:caso", args=[self.cases[0].pk]))
        self.assertNotContains(r, 'id="form-decision"')
        self.assertContains(r, "solo el especialista fitosanitario puede decidir")


class MapaPlanoPanelExportacion(TestCase):
    def setUp(self):
        self.w = F.world()
        self.gps = F.analyzed(self.w)[2]
        self.aprox = F.analyzed(self.w, gps=False)[2]
        self.nada = F.analyzed(self.w, gps=False, marker=False)[2]
        self.c = login(Client(), self.w.sup)

    def test_geojson(self):
        data = self.c.get(reverse("web:mapa_datos")).json()
        self.assertEqual(data["type"], "FeatureCollection")
        props = {f["properties"]["id"]: f for f in data["features"]}
        self.assertEqual(set(props), {str(self.gps.pk), str(self.aprox.pk)})
        self.assertEqual(props[str(self.gps.pk)]["geometry"]["coordinates"], [-75.7302, -14.0601])  # [lon, lat]
        self.assertEqual(props[str(self.aprox.pk)]["properties"]["locationSource"], "MARCADOR")
        self.assertEqual(data["meta"]["sinUbicacion"], 1)
        data = self.c.get(reverse("web:mapa_datos"), {"estado": "DESCARTADO"}).json()
        self.assertEqual(data["features"], [])

    def test_plano_cuenta_casos_y_cobertura(self):
        F.make_pass(self.w, self.w.row, "LATERAL_B", status="INCOMPLETE")
        r = self.c.get(reverse("web:plano"), {"lote": "SWG1"})
        p = r.context["p"]
        fila = next(x for x in p["rows"] if x["row"].number == 5)
        self.assertEqual(fila["counts"], {"PENDIENTE_REVISION": 3})
        self.assertEqual(fila["segments"][0]["counts"], {"PENDIENTE_REVISION": 2})  # uno quedó sin segmento
        self.assertFalse(fila["covered"])  # LATERAL_B INCOMPLETE sin incidencia no cubre (RN-13)
        Incident.objects.create(incident_id=uuid.uuid4(), session=self.w.session,
                                monitoring_pass=self.w.session.passes.get(lateral_code="LATERAL_B"),
                                type="OPERADOR", severity="AVISO", detail="Riego", occurred_at=self.w.session.started_at,
                                created_by="OPERADOR")
        p = self.c.get(reverse("web:plano"), {"lote": "SWG1"}).context["p"]
        self.assertTrue(next(x for x in p["rows"] if x["row"].number == 5)["covered"])
        self.assertEqual(p["covered"], 1)

    def test_panel(self):
        k = self.c.get(reverse("web:dashboard")).context["k"]
        self.assertEqual((k["pendientes"], k["capturas"], k["casos"]["PENDIENTE_REVISION"]), (3, 3, 3))
        self.assertEqual(k["ia"]["INDICIO_SUGERIDO_POR_IA"], 3)

    def test_csv_utf8_con_bom_y_sin_inyeccion_de_formulas(self):
        self.w.marker.code = "=HYPERLINK(\"http://x\")"
        self.w.marker.save()
        r = self.c.get(reverse("web:exportar_casos"))
        body = b"".join(r.streaming_content).decode("utf-8")
        self.assertTrue(body.startswith("﻿caso,estado"))
        self.assertIn("'=HYPERLINK", body)
        self.assertEqual(len(body.strip().splitlines()), 4)
        self.assertTrue(AuditEvent.objects.filter(action="EXPORTACION_CASOS").exists())


class Administracion(TestCase):
    def setUp(self):
        self.w = F.world()
        self.c = login(Client(), self.w.admin)

    def test_aprobar_cuenta_con_roles(self):
        nuevo = F.user("nuevo@x.pe", status=AccountStatus.PENDIENTE_APROBACION)
        r = self.c.post(reverse("web:usuario_accion", args=[nuevo.pk, "aprobar"]), {"roles": [Role.SUPERVISOR]})
        self.assertRedirects(r, reverse("web:usuarios"))
        nuevo = User.objects.get(pk=nuevo.pk)
        self.assertEqual((nuevo.status, set(nuevo.roles), nuevo.approved_by), (AccountStatus.ACTIVO, {Role.SUPERVISOR}, self.w.admin))

    def test_contrasena_temporal_revoca_la_app(self):
        from rest_framework_simplejwt.token_blacklist.models import BlacklistedToken
        from rest_framework_simplejwt.tokens import RefreshToken

        RefreshToken.for_user(self.w.op)  # sesión abierta en un celular
        r = self.c.post(reverse("web:usuario_accion", args=[self.w.op.pk, "contrasena-temporal"]), follow=True)
        self.assertContains(r, "Contraseña temporal de")
        self.w.op.refresh_from_db()
        self.assertTrue(self.w.op.must_change_password)
        self.assertEqual(BlacklistedToken.objects.filter(token__user=self.w.op).count(), 1)

    def test_contrasena_temporal_cumple_la_politica(self):
        from django.contrib.auth.password_validation import validate_password

        from cuentas import services as cuentas

        for _ in range(20):
            validate_password(cuentas.set_temporary_password(self.w.op.pk, self.w.admin), self.w.op)

    def test_politica_igual_a_la_app(self):
        from django.contrib.auth.password_validation import validate_password
        from django.core.exceptions import ValidationError

        for mala in ("solo-letras-largas", "12345678", " Clave123", "corta1"):
            with self.assertRaises(ValidationError, msg=mala):
                validate_password(mala)
        validate_password("Vid-Ica-2026")

    def test_cambiar_roles_y_atender_pedido_por_correo(self):
        from cuentas.models import PasswordResetRequest

        r = self.c.post(reverse("web:usuario_accion", args=[self.w.sup.pk, "roles"]),
                        {"roles": [Role.SUPERVISOR, Role.ESPECIALISTA_FITOSANITARIO]})
        self.assertRedirects(r, reverse("web:usuarios"))
        self.assertEqual(set(User.objects.get(pk=self.w.sup.pk).roles), {Role.SUPERVISOR, Role.ESPECIALISTA_FITOSANITARIO})
        self.assertTrue(AuditEvent.objects.filter(action="ROLES_CAMBIADOS").exists())
        pedido = PasswordResetRequest.objects.create(email="SUP@x.pe")  # la API puede guardar solo el correo
        self.c.post(reverse("web:usuario_accion", args=[self.w.sup.pk, "contrasena-temporal"]))
        pedido.refresh_from_db()
        self.assertEqual(pedido.status, PasswordResetRequest.Status.ATENDIDA)

    def test_no_puede_bloquearse_a_si_mismo(self):
        r = self.c.post(reverse("web:usuario_accion", args=[self.w.admin.pk, "bloquear"]), follow=True)
        self.assertContains(r, "No puedes cambiar el estado de tu propia cuenta")

    def test_destinatario_con_consentimiento(self):
        r = self.c.post(reverse("web:destinatarios"), {"full_name": "Jefe", "phone_e164": "+51987654321",
                                                       "role_label": "JEFE_FUNDO", "lots": ["SWG1"], "active": "on",
                                                       "opt_in": "on"})
        self.assertRedirects(r, reverse("web:destinatarios"))
        dest = NotificationRecipient.objects.get()
        self.assertIsNotNone(dest.opt_in_at)
        r = self.c.post(reverse("web:destinatarios"), {"full_name": "Malo", "phone_e164": "[teléfono omitido]",
                                                       "role_label": "OTRO"})
        self.assertContains(r, "formato internacional")

    def test_reencolar_error_de_ia(self):
        cap = F.capture(self.w)
        from ia import services as ia

        task = ia.enqueue_analysis(cap)
        AiTask.objects.filter(pk=task.pk).update(status=AiStatus.ERROR_DE_ANALISIS)
        self.assertEqual(login(Client(), self.w.esp).post(reverse("web:ia_reencolar", args=[task.pk])).status_code, 403)
        self.c.post(reverse("web:ia_reencolar", args=[task.pk]))
        task.refresh_from_db()
        self.assertEqual(task.status, AiStatus.PENDIENTE_DE_ANALISIS)

    def test_revocar_celular(self):
        self.c.post(reverse("web:dispositivo_revocar", args=[self.w.device.pk]))
        self.w.device.refresh_from_db()
        self.assertIsNotNone(self.w.device.revoked_at)
```

---

## Anexo G. Administración, datos de demostración y operación

Todas las apps siguen el mismo `apps.py` (se muestra el de `revision`); `auditoria/apps.py` está en B.9.

### G.1 Django Admin — `*/admin.py`

`auditoria/admin.py`

```python
from django.contrib import admin

from auditoria.models import AuditEvent


class ReadOnlyAdmin(admin.ModelAdmin):
    """Evidencia y trazabilidad: en Django Admin se consultan, nunca se crean, editan ni borran (RN-W08)."""

    def has_add_permission(self, request):
        return False

    def has_change_permission(self, request, obj=None):
        return False

    def has_delete_permission(self, request, obj=None):
        return False


@admin.register(AuditEvent)
class AuditEventAdmin(ReadOnlyAdmin):
    list_display = ("timestamp", "action", "entity_type", "entity_id", "user")
    list_filter = ("action", "entity_type")
    search_fields = ("entity_id", "action")
```

`cuentas/admin.py`

```python
from django.contrib import admin

from auditoria.admin import ReadOnlyAdmin
from cuentas.models import Device, PasswordResetRequest, User, UserRole


class UserRoleInline(admin.TabularInline):
    """Solo lectura: los roles se cambian en la web (WEB-13) para pasar por cuentas.services y la auditoría."""

    model = UserRole
    extra = 0
    can_delete = False
    readonly_fields = ("role", "assigned_at")

    def has_add_permission(self, request, obj=None):
        return False


@admin.register(User)
class UserAdmin(admin.ModelAdmin):
    # Aprobar, rechazar, bloquear, roles y contraseña temporal se hacen en la web (WEB-13): cuentas.services + auditoría.
    # Aquí solo se editan datos de contacto y, por un superusuario, is_staff / is_superuser (excepción a W-15:
    # Django lo registra en django_admin_log).
    list_display = ("full_name", "email", "status", "must_change_password", "is_staff", "created_at")
    list_filter = ("status", "is_staff", "user_roles__role")
    search_fields = ("full_name", "email", "employee_code")
    readonly_fields = ("password", "status", "must_change_password", "approved_by", "approved_at", "created_at",
                       "last_login", "accepted_privacy_notice_at")
    exclude = ("groups", "user_permissions")
    inlines = [UserRoleInline]

    def has_delete_permission(self, request, obj=None):
        return False  # las cuentas se rechazan o bloquean, no se borran (trazabilidad)


@admin.register(Device)
class DeviceAdmin(ReadOnlyAdmin):  # revocar: WEB-14
    list_display = ("device_id", "model", "platform", "app_version", "user", "last_seen_at", "revoked_at")


@admin.register(PasswordResetRequest)
class PasswordResetRequestAdmin(ReadOnlyAdmin):  # se atienden con «Contraseña temporal» en WEB-13
    list_display = ("email", "user", "requested_at", "status", "handled_by", "handled_at")
```

`campo/admin.py`

```python
from django.contrib import admin

from campo.models import FieldLot, FieldRow, FieldSegment, Marker, QualityProfile


@admin.register(FieldLot)
class FieldLotAdmin(admin.ModelAdmin):
    list_display = ("code", "name", "active")


@admin.register(FieldRow)
class FieldRowAdmin(admin.ModelAdmin):
    list_display = ("id", "lot", "number", "plant_count", "active")
    list_filter = ("lot",)


@admin.register(FieldSegment)
class FieldSegmentAdmin(admin.ModelAdmin):
    list_display = ("code", "row", "start_plant", "end_plant", "is_pilot")
    list_filter = ("is_pilot", "row__lot")


@admin.register(Marker)
class MarkerAdmin(admin.ModelAdmin):
    list_display = ("code", "row", "segment", "position", "lat", "lon")
    list_filter = ("row__lot",)


admin.site.register(QualityProfile)
```

`ia/admin.py`

```python
from django.contrib import admin

from auditoria.admin import ReadOnlyAdmin
from ia.models import AiTask, Detection, ModelConfig


@admin.register(ModelConfig)
class ModelConfigAdmin(admin.ModelAdmin):
    list_display = ("name", "version", "active", "conf_threshold", "created_at")


@admin.register(AiTask)
class AiTaskAdmin(ReadOnlyAdmin):
    list_display = ("task_id", "status", "attempts", "model_version", "processing_ms", "requested_at")
    list_filter = ("status",)


@admin.register(Detection)
class DetectionAdmin(ReadOnlyAdmin):
    list_display = ("task", "class_name", "confidence", "review_status")
```

`monitoreo/admin.py`

```python
from django.contrib import admin

from auditoria.admin import ReadOnlyAdmin
from monitoreo.models import CaptureSequence, Incident, MonitoringPass, MonitoringSession

for model in (MonitoringSession, MonitoringPass, CaptureSequence, Incident):
    admin.site.register(model, ReadOnlyAdmin)
```

`evidencias/admin.py`

```python
from django.contrib import admin

from auditoria.admin import ReadOnlyAdmin
from evidencias.models import Capture

admin.site.register(Capture, ReadOnlyAdmin)
```

`revision/admin.py`

```python
from django.contrib import admin

from auditoria.admin import ReadOnlyAdmin
from revision.models import Case, HumanReview

admin.site.register(Case, ReadOnlyAdmin)
admin.site.register(HumanReview, ReadOnlyAdmin)
```

`notificaciones/admin.py`

```python
from django.contrib import admin

from auditoria.admin import ReadOnlyAdmin
from notificaciones.models import Notification

admin.site.register(Notification, ReadOnlyAdmin)
```

### G.2 Datos de demostración — `web/demo.py` y `web/management/commands/sembrar_demo.py`

`web/demo.py`

```python
# web/demo.py — Datos de demostración para desarrollar la web sin la app (T-W02). NUNCA en el entorno piloto.
# Catálogos: lotes del piloto del Anexo D del maestro móvil. Segmentos, marcadores y coordenadas son FICTICIOS (Q-01).
import hashlib
import random
import uuid
from datetime import timedelta

from django.db import transaction
from django.utils import timezone

from campo.models import FieldLot, FieldRow, FieldSegment, Marker, QualityProfile
from cuentas.models import AccountStatus, Device, Role, User
from evidencias.models import Capture, QualityResult, QualityStatus
from ia import services as ia
from ia.models import ModelConfig
from monitoreo.models import CaptureSequence, MonitoringPass, MonitoringSession, SessionDevice

PILOT_PLANTS_PER_ROW = {
    "SWG1": [383, 384, 381, 384, 385, 386, 384, 385, 386, 384, 388, 388, 391, 382, 275, 275, 274, 273, 273, 273, 272,
             270, 268, 270, 269, 269, 268, 268, 267, 266, 265],
    "SWG2": [251, 260, 267, 273, 279, 286, 292, 299, 307, 313, 319, 326, 332, 339, 347, 351],
    "SWG5": [11, 19, 25, 33, 40, 47, 54, 62, 68, 75, 84, 91, 97, 104, 111, 118, 126, 132, 138, 146, 154, 161, 168,
             174, 181, 188, 194, 200, 207, 213, 220, 223, 233, 239, 246, 250],
}
PILOT_LOTS = [("SWG1", "SWG 1", "Lote 1 (Piscina)"), ("SWG2", "SWG 2", "Lote 2 (Maíz)"),
              ("SWG5", "SWG 5", "Lote 5 (Triángulo)")]
DEMO_ORIGIN = {"SWG1": (-14.0600, -75.7300), "SWG2": (-14.0640, -75.7300), "SWG5": (-14.0680, -75.7300)}  # FICTICIAS
ROW_SPACING_DEG, PLANT_SPACING_DEG = 0.000027, 0.000013  # ~3 m entre hileras, ~1,4 m entre plantas (ficticio)


def _plant_point(lot_id, row_number, plant):
    lat0, lon0 = DEMO_ORIGIN[lot_id]
    return lat0 - row_number * ROW_SPACING_DEG, lon0 + plant * PLANT_SPACING_DEG


@transaction.atomic
def crear_catalogos_piloto():
    for lot_id, code, name in PILOT_LOTS:
        rows = PILOT_PLANTS_PER_ROW[lot_id]
        lat0, lon0 = DEMO_ORIGIN[lot_id]
        lat1, lon1 = _plant_point(lot_id, len(rows) + 1, max(rows) + 1)
        geometry = {"type": "Polygon", "coordinates": [[[lon0, lat0], [lon1, lat0], [lon1, lat1], [lon0, lat1], [lon0, lat0]]]}
        lot, _ = FieldLot.objects.update_or_create(id=lot_id, defaults={"code": code, "name": name, "geometry": geometry})
        for i, plants in enumerate(rows, 1):
            FieldRow.objects.update_or_create(id=f"{lot_id}-H{i:02d}",
                                              defaults={"lot": lot, "number": i, "plant_count": plants})
        # 4 segmentos piloto de 25 plantas por lote (FICTICIOS hasta Q-01)
        candidates = [n for n, p in enumerate(rows, 1) if p >= 60]
        for k, row_number in enumerate(candidates[:: max(1, len(candidates) // 4)][:4], 1):
            row = FieldRow.objects.get(id=f"{lot_id}-H{row_number:02d}")
            start = row.plant_count // 2 - 12
            seg, _ = FieldSegment.objects.update_or_create(
                id=f"{lot_id}-S{k}", defaults={"row": row, "code": f"{code}-S{k}", "start_plant": start,
                                               "end_plant": start + 24, "is_pilot": True})
            lat, lon = _plant_point(lot_id, row_number, start)
            Marker.objects.update_or_create(
                id=f"{lot_id}-M{k}", defaults={"row": row, "segment": seg, "code": f"M{k}-{lot_id}",
                                               "position": "INICIO", "lat": lat, "lon": lon})
    QualityProfile.objects.get_or_create(version="Q0", defaults={"params": {}, "published_at": timezone.now()})
    ModelConfig.objects.get_or_create(version="demo-0", defaults={
        "name": "detector-demo", "weights_uri": "demo://sin-pesos", "classes": ["chanchito_blanco"], "active": True})


def crear_usuarios_demo(password):
    gente = [("admin@demo.pe", "Ana Administradora", [Role.ADMINISTRADOR], True),
             ("especialista@demo.pe", "Eduardo Especialista", [Role.ESPECIALISTA_FITOSANITARIO], False),
             ("supervisor@demo.pe", "Sofía Supervisora", [Role.SUPERVISOR], False),
             ("operador@demo.pe", "Óscar Operador", [Role.OPERADOR_CAMPO], False)]
    out = {}
    for email, name, roles, staff in gente:
        user = User.objects.filter(email=email).first()
        if user is None:
            user = User.objects.create_user(email, password, roles=roles, full_name=name, is_staff=staff,
                                            status=AccountStatus.ACTIVO)
        out[email.split("@")[0]] = user
    User.objects.get_or_create(email="pendiente@demo.pe", defaults={"full_name": "Pedro Pendiente"})
    return out


@transaction.atomic
def crear_evidencia_demo(operador, secuencias_por_pasada=12, semilla=7):
    """Una sesión por lote, pasadas A y B en las hileras con segmento, fotos «subidas» y análisis simulado."""
    rnd = random.Random(semilla)
    ahora = timezone.now()
    devices = {}
    for role in ("CONTROLADOR", "CAMERA_1", "CAMERA_2"):
        devices[role], _ = Device.objects.get_or_create(
            device_id=uuid.uuid5(uuid.NAMESPACE_URL, f"demo-{role}"),
            defaults={"platform": "android", "model": f"Demo {role}", "app_version": "0.3.0", "user": operador})
    for d, (lot_id, _, _) in enumerate(PILOT_LOTS):
        inicio = ahora - timedelta(days=d + 1, hours=3)
        s = MonitoringSession.objects.create(
            session_id=uuid.uuid4(), operator=operador, controller_device=devices["CONTROLADOR"], status="CLOSED",
            mode="MANUAL", started_at=inicio, ended_at=inicio + timedelta(hours=2), app_version="0.3.0",
            config_version="CFG-4", quality_profile_version="Q0", short_test_passed_at=inicio)
        for role in ("CAMERA_1", "CAMERA_2"):
            SessionDevice.objects.create(session=s, role=role, device=devices[role], user=operador, platform="android",
                                         model=devices[role].model, paired_at=inicio)
        t = inicio
        for seg in FieldSegment.objects.filter(row__lot_id=lot_id).select_related("row"):
            for lateral in ("LATERAL_A", "LATERAL_B"):
                p = MonitoringPass.objects.create(
                    pass_id=uuid.uuid4(), session=s, lot_id=lot_id, row=seg.row, lateral_code=lateral, pass_order=1,
                    direction="ASCENDENTE" if lateral == "LATERAL_A" else "DESCENDENTE", status="COMPLETED",
                    started_at=t, ended_at=t + timedelta(minutes=10), sequences_total=secuencias_por_pasada,
                    sequences_complete=secuencias_por_pasada)
                for n in range(1, secuencias_por_pasada + 1):
                    t += timedelta(seconds=20)
                    planta = seg.start_plant + (n * 2) % 25
                    lat, lon = _plant_point(lot_id, seg.row.number, planta)
                    ids = {r: uuid.uuid4() for r in ("CAMERA_1", "CAMERA_2")}
                    seq = CaptureSequence.objects.create(
                        sequence_id=uuid.uuid4(), monitoring_pass=p, session=s, sequence_number=n, mode="MANUAL",
                        status="COMPLETE", segment=seg, marker=seg.markers.first(),
                        lat=lat if n % 5 else None, lon=lon if n % 5 else None,  # 1 de cada 5 sin GPS
                        gps_accuracy_m=rnd.choice([3.0, 5.0, 8.0]) if n % 5 else None, issued_at=t,
                        expected_capture_ids={k: str(v) for k, v in ids.items()},
                        slot_outcomes={"CAMERA_1": "OK_RECIBIDA", "CAMERA_2": "OK_RECIBIDA"})
                    for role, cid in ids.items():
                        q = QualityStatus.REPETIR_NITIDEZ if rnd.random() < 0.06 else QualityStatus.UTILIZABLE
                        cap = Capture.objects.create(
                            capture_id=cid, sequence=seq, session=s, monitoring_pass=p, lateral_code=lateral,
                            device=devices[role], camera_role=role, camera_user=operador, operator_user=operador,
                            captured_at=t, width=3000, height=4000, size_bytes=3_900_000,
                            md5=hashlib.md5(str(cid).encode()).hexdigest(), quality_status=q, app_version="0.3.0",
                            cloudinary_public_id=f"riachuelo/dev/{s.pk}/{p.pk}/{cid}", cloudinary_version=1700000000,
                            cloudinary_bytes=3_900_000, cloudinary_format="jpg")
                        QualityResult.objects.create(capture=cap, status=q, profile_version="Q0",
                                                     reasons=[] if q == QualityStatus.UTILIZABLE else ["NITIDEZ_BAJA"])
                        task = ia.enqueue_analysis(cap)
                        if task is None:
                            continue
                        task = ia.claim_next_task("demo")
                        boxes = []
                        if rnd.random() < 0.15:
                            for _ in range(rnd.randint(1, 3)):
                                x, y = rnd.uniform(200, 2500), rnd.uniform(200, 3500)
                                boxes.append({"class_name": "chanchito_blanco", "confidence": round(rnd.uniform(0.3, 0.95), 3),
                                              "x_min": x, "y_min": y, "x_max": x + rnd.uniform(60, 300), "y_max": y + rnd.uniform(60, 300)})
                        ia.save_analysis_result(task, boxes, 3000, 4000, rnd.randint(80, 400), "demo-0", {"demo": True})
                t += timedelta(minutes=2)
```

`web/management/commands/sembrar_demo.py`

```python
from django.conf import settings
from django.core.management.base import BaseCommand, CommandError

from web import demo


class Command(BaseCommand):
    help = "Carga catálogos del piloto y, opcionalmente, usuarios y evidencia de demostración (solo dev)."

    def add_arguments(self, parser):
        parser.add_argument("--password", default="Demo2026!", help="Contraseña de las cuentas demo")
        parser.add_argument("--solo-catalogos", action="store_true")

    def handle(self, *args, **opts):
        if settings.APP_ENV == "piloto":
            raise CommandError("sembrar_demo está prohibido en el entorno piloto (no mezclar datos de prueba).")
        demo.crear_catalogos_piloto()
        self.stdout.write("Catálogos del piloto cargados (segmentos, marcadores y coordenadas FICTICIOS).")
        if opts["solo_catalogos"]:
            return
        users = demo.crear_usuarios_demo(opts["password"])
        demo.crear_evidencia_demo(users["operador"])
        self.stdout.write(self.style.SUCCESS("Usuarios y evidencia de demostración cargados."))
```

### G.3 Worker — `ia/management/commands/worker_ia.py`

`ia/management/commands/worker_ia.py`

```python
# ia/management/commands/worker_ia.py — Proceso worker: `python manage.py worker_ia` (28.8 del maestro móvil).
# Esqueleto de referencia: el detector real (ia/inference/detector.py) se integra en la tarea T-W11.
import logging
import os
import socket
import time

from django.core.management.base import BaseCommand
from django.db import close_old_connections

from ia import services as ia
from notificaciones import services as notif
from notificaciones.whatsapp import get_client

log = logging.getLogger("worker_ia")


class Command(BaseCommand):
    help = "Toma tareas de ai_tasks y avisos pendientes; nunca atiende HTTP."

    def add_arguments(self, parser):
        parser.add_argument("--once", action="store_true", help="Procesa lo pendiente y termina (pruebas)")
        parser.add_argument("--sleep", type=float, default=3.0)

    def handle(self, *args, **opts):
        worker_id = f"{socket.gethostname()}:{os.getpid()}"
        detector = None  # cargar UNA vez: from ia.inference.detector import Detector; Detector.load(settings.MODEL_PATH)
        client = get_client()
        while True:
            close_old_connections()  # conexiones caídas o vencidas (CONN_MAX_AGE) entre vueltas
            trabajo = 0
            task = ia.claim_next_task(worker_id) if detector is not None else None
            if task is not None:
                trabajo += 1
                try:
                    r = detector.analyze(task)  # descarga el original firmado, tiling, NMS → cajas en píxeles
                    ia.save_analysis_result(task, r.boxes, r.width, r.height, r.ms, r.model_version, r.raw)
                except Exception as exc:  # noqa: BLE001 — cualquier fallo se registra y reintenta
                    log.exception("Fallo de análisis %s", task.pk)
                    ia.mark_analysis_failed(task, repr(exc))
            trabajo += notif.process_pending_notifications(client, worker_id)
            if opts["once"]:
                return
            if not trabajo:
                time.sleep(opts["sleep"])
```

### G.4 `.env.example`, `requirements.txt`, `README.md`, `manage.py` y `apps.py`

`.env.example`

```bash
# Copiar a .env (nunca subir .env a Git). Valores de ejemplo para desarrollo.
DJANGO_SECRET_KEY=cambia-esto-por-una-clave-larga-y-aleatoria
DJANGO_DEBUG=true
APP_ENV=dev
ALLOWED_HOSTS=localhost,127.0.0.1
CSRF_TRUSTED_ORIGINS=http://localhost:8000
PUBLIC_BASE_URL=http://localhost:8000
# Supabase (conexión directa o session pooler, puerto 5432) — o PostgreSQL local en desarrollo
DATABASE_URL=[DATABASE_URL omitida]
DB_SSLMODE=disable
CLOUDINARY_URL=[CLOUDINARY_URL omitida]
CLOUDINARY_ENV_PREFIX=dev
WHATSAPP_CLIENT=notificaciones.whatsapp.ConsoleClient
WHATSAPP_TOKEN=
WHATSAPP_PHONE_ID=
WHATSAPP_GRAPH_VERSION=
STATICFILES_BACKEND=django.contrib.staticfiles.storage.StaticFilesStorage
# Piloto (HTTPS): SECURE_SSL_REDIRECT=true y SECURE_HSTS_SECONDS=31536000 (por defecto 3600 fuera de DEBUG)
SECURE_HSTS_SECONDS=0
```

`requirements.txt`

```text
# Versiones comprobadas el 03–04/10/2026 (sección 5.2 de docs/MAESTRO_WEB.md). Django 5.2 LTS: no subir a 6.x
# mientras djangorestframework-simplejwt no declare soporte.
Django==5.2.17
djangorestframework==3.18.1
djangorestframework-simplejwt==5.5.1
drf-spectacular==0.30.0
django-htmx==1.29.0
whitenoise==6.12.0
cloudinary==1.46.2
psycopg[binary]==3.3.6
gunicorn==26.2.0
django-environ==0.14.0
requests==2.34.2
```

`README.md`

```text
# riachuelo-platform — código de referencia de la web (Maestro Web v1.0)

Plataforma Django (API `/api/v1` para la app, web de revisión y worker de IA). Este paquete trae la **web** y el
modelo de datos completo; la API de la app se construye según `docs/MAESTRO_APP_MOVIL.md` §15.

    python3.12 -m venv .venv && . .venv/bin/activate
    pip install -r requirements.txt
    cp .env.example .env            # completar DATABASE_URL, DJANGO_SECRET_KEY, CLOUDINARY_URL
    python manage.py migrate        # crea tablas, tabla de caché y aplica RLS/REVOKE (Supabase)
    python manage.py sembrar_demo   # SOLO en dev: catálogos del piloto + usuarios y casos de demostración
    python manage.py runserver      # http://localhost:8000  (especialista@demo.pe / Demo2026!)
    python manage.py worker_ia      # en otra terminal: avisos (y análisis cuando se integre el detector)
    python manage.py test           # 66 pruebas (PostgreSQL); en SQLite 3 se omiten

Lee `docs/MAESTRO_WEB.md` antes de cada tarea (sección 1: reglas W-01 a W-24).
```

`manage.py`

```python
#!/usr/bin/env python
import os
import sys

if __name__ == "__main__":
    os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings")
    from django.core.management import execute_from_command_line

    execute_from_command_line(sys.argv)
```

`revision/apps.py`

```python
from django.apps import AppConfig


class RevisionConfig(AppConfig):
    name = "revision"
    default_auto_field = "django.db.models.BigAutoField"
```

---

## Anexo H. Medición de rendimiento

### H.1 Datos sintéticos — `docs/perf/datos_sinteticos.sql`

`docs/perf/datos_sinteticos.sql`

```sql
-- docs/perf/datos_sinteticos.sql — Datos sintéticos para medir rendimiento (sección 18). SOLO en una COPIA de dev:
--   createdb -T riachuelo riachuelo_perf   (después de `python manage.py sembrar_demo` en riachuelo)
--   psql -d riachuelo_perf -f docs/perf/datos_sinteticos.sql
-- Genera 100 000 capturas (1 de cada 15 rechazada por calidad), sus análisis (≈ 20 % con indicio) y un caso por indicio.
WITH ref AS (SELECT c.sequence_id, c.session_id, c.pass_id, c.device_id, c.camera_user_id, c.operator_user_id FROM captures c LIMIT 1)
INSERT INTO captures (capture_id, sequence_id, session_id, pass_id, lateral_code, device_id, camera_role, camera_user_id, operator_user_id,
  captured_at, width, height, size_bytes, md5, quality_status, retake_context, app_version, cloudinary_public_id, cloudinary_version,
  cloudinary_bytes, cloudinary_format, confirmed_at, replaces_capture_id, cloudinary_width, cloudinary_height)
SELECT gen_random_uuid(), ref.sequence_id, ref.session_id, ref.pass_id, 'LATERAL_A', ref.device_id, 'CAMERA_1', ref.camera_user_id, ref.operator_user_id,
  now() - (g % 60) * interval '1 day' - (g % 600) * interval '1 minute', 3000, 4000, 3900000, md5(g::text),
  CASE WHEN g % 15 = 0 THEN 'REPETIR_NITIDEZ' ELSE 'UTILIZABLE' END, NULL, '0.3.0', 'perf/' || g, 1, 3900000, 'jpg', now(), NULL, NULL, NULL
FROM generate_series(1, 100000) g, ref;

INSERT INTO ai_tasks (task_id, capture_id, model_config_id, status, requested_at, available_at, attempts, processing_ms, model_version, locked_by, error_message)
SELECT gen_random_uuid(), c.capture_id, (SELECT id FROM model_configs WHERE active),
  CASE WHEN random() < 0.2 THEN 'INDICIO_SUGERIDO_POR_IA' ELSE 'SIN_INDICIOS_IA' END,
  c.captured_at, c.captured_at, 1, 150 + (random() * 200)::int, 'demo-0', '', ''
FROM captures c WHERE c.cloudinary_public_id LIKE 'perf/%' AND c.quality_status = 'UTILIZABLE';

CREATE TEMP TABLE filas AS SELECT row_number() OVER () - 1 AS i, id, lot_id FROM field_rows;
INSERT INTO review_cases (id, capture_id, ai_task_id, origin, status, notification_status, detections_count, max_confidence,
  session_id, pass_id, sequence_id, lot_id, row_id, lateral_code, segment_id, marker_id, lat, lon, gps_accuracy_m,
  location_source, captured_at, opened_at, decided_at, decided_by_id)
SELECT gen_random_uuid(), t.capture_id, t.task_id, 'IA',
  (ARRAY['PENDIENTE_REVISION','CONFIRMADO_POR_ESPECIALISTA','DESCARTADO','EVIDENCIA_INSUFICIENTE'])[1 + (abs(hashtext(t.task_id::text)) % 4)],
  'NO_APLICA', 1, random(), c.session_id, c.pass_id, c.sequence_id, f.lot_id, f.id, 'LATERAL_A', NULL, NULL,
  -14.06 - random() * 0.01, -75.73 + random() * 0.01, 5, 'GPS', c.captured_at, c.captured_at + interval '1 hour', NULL, NULL
FROM ai_tasks t JOIN captures c ON c.capture_id = t.capture_id
JOIN filas f ON f.i = abs(hashtext(t.task_id::text)) % (SELECT count(*) FROM filas)
WHERE t.status = 'INDICIO_SUGERIDO_POR_IA' AND c.cloudinary_public_id LIKE 'perf/%';

UPDATE review_cases SET decided_at = opened_at + interval '3 hours',
  decided_by_id = (SELECT id FROM users WHERE email = 'especialista@demo.pe')
WHERE status <> 'PENDIENTE_REVISION' AND decided_at IS NULL;
ANALYZE;
SELECT (SELECT count(*) FROM captures) AS capturas, (SELECT count(*) FROM ai_tasks) AS tareas,
       (SELECT count(*) FROM review_cases) AS casos;
```

### H.2 Comando — `web/management/commands/medir_rendimiento.py`

`web/management/commands/medir_rendimiento.py`

```python
# web/management/commands/medir_rendimiento.py — Mide el tiempo de servidor de las pantallas principales (sección 18).
# Uso: DATABASE_URL=<base con datos (Anexo H.1)> python manage.py medir_rendimiento --email especialista@demo.pe
import statistics
import time

from django.conf import settings
from django.core.management.base import BaseCommand
from django.db import connection
from django.test import Client
from django.test.utils import CaptureQueriesContext

from cuentas.models import User
from revision.models import Case, ReviewStatus


class Command(BaseCommand):
    help = "Mide mediana y p95 de las pantallas de la web con los datos de la base configurada."

    def add_arguments(self, parser):
        parser.add_argument("--email", default="especialista@demo.pe")
        parser.add_argument("--repeticiones", type=int, default=12)

    def handle(self, *args, **opts):
        if "testserver" not in settings.ALLOWED_HOSTS:
            settings.ALLOWED_HOSTS.append("testserver")
        client = Client()
        client.force_login(User.objects.get(email=opts["email"]))
        case = Case.objects.filter(status=ReviewStatus.PENDIENTE_REVISION).first()
        lote = case.lot_id if case else ""
        urls = {
            "bandeja (pendientes, pág. 1)": "/casos/",
            "bandeja (todos, confianza, pág. 50)": "/casos/?estado=&orden=confianza&pagina=50",
            "caso": f"/casos/{case.pk}/" if case else None,
            "mapa/datos (sin filtros)": "/mapa/datos/",
            "mapa/datos (un lote, confirmados)": f"/mapa/datos/?lote={lote}&estado=CONFIRMADO_POR_ESPECIALISTA",
            "plano (un lote)": f"/plano/?lote={lote}",
            "panel (30 días)": "/",
        }
        self.stdout.write(f"{'pantalla':40} {'mediana ms':>10} {'p95 ms':>8} {'consultas':>9} {'KB':>6}")
        for name, url in urls.items():
            if url is None:
                continue
            times, n_queries, size = [], 0, 0
            for i in range(opts["repeticiones"]):
                with CaptureQueriesContext(connection) as q:
                    t0 = time.perf_counter()
                    r = client.get(url, HTTP_ACCEPT_ENCODING="gzip")
                    dt = (time.perf_counter() - t0) * 1000
                if r.status_code != 200:
                    raise SystemExit(f"{url} respondió {r.status_code}")
                if i >= 2:  # las dos primeras calientan conexiones y cachés
                    times.append(dt)
                n_queries, size = len(q.captured_queries), len(r.content)
            times.sort()
            p95 = times[max(0, int(len(times) * 0.95) - 1)]
            self.stdout.write(f"{name:40} {statistics.median(times):10.0f} {p95:8.0f} {n_queries:9d} {size / 1024:6.0f}")
```
