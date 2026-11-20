# CAPÍTULO XII. SEGURIDAD Y PRUEBAS

## 12.1. Controles de seguridad

Los controles de seguridad protegen los datos en tránsito, en reposo y durante el acceso a los componentes del sistema: aplicación móvil, API REST, web de revisión, base de datos, almacenamiento de fotos, worker de IA y servicio de notificaciones.

- **Aislamiento de la base de datos:** Supabase PostgreSQL se utiliza como base administrada y no como backend público de la app. La aplicación móvil y el navegador no acceden directamente a Supabase; toda operación de negocio pasa por Django o por el worker autorizado.

- **Seguridad en el servidor web:** Django aplica sesiones seguras, protección CSRF, cabeceras contra clickjacking, políticas de referencia y redireccionamiento HTTPS en producción o piloto.

- **Gestión de Secretos:** DJANGO_SECRET_KEY, DATABASE_URL, credenciales de Cloudinary, token del bot de Telegram y demás claves sensibles se almacenan exclusivamente como variables de entorno del servidor, nunca en el código fuente ni en clientes móviles.

- **Separación de responsabilidades**: La API atiende a la app móvil, la web atiende a especialistas y supervisores, y el worker procesa IA y notificaciones sin exponer endpoints públicos.

## 12.2. Autenticación

- **Autenticación web:** La plataforma utiliza el sistema de sesiones de Django con cookies seguras y protección CSRF para especialistas, supervisores y administradores.

- **Autenticación móvil:** La app utiliza JWT emitidos por Django mediante SimpleJWT. El token permite consumir la API REST /api/v1 y se asocia al usuario y dispositivo correspondiente.

- **Modo offline:** La app permite operación limitada sin Internet mediante credenciales locales protegidas y una ventana de validez definida. Los tokens y verificadores se resguardan con SecureStore.

## 12.3. Autorización

La autorización se aplica en el servidor para impedir accesos indebidos a funcionalidades restringidas. Las vistas web validan rol, permisos y estado activo de la cuenta; los

servicios de dominio vuelven a comprobar las reglas críticas antes de ejecutar decisiones sensibles, como confirmar un caso o enviar una notificación.

## 12.4. Roles y permisos

- **Administrador:** Gestiona cuentas, roles, dispositivos, catálogos, configuraciones, aprobaciones y supervisión técnica de la plataforma.

- **Especialista fitosanitario:** Revisa casos, observa evidencias fotográficas, analiza cajas sugeridas por la IA y registra confirmaciones, descartes o evidencia insuficiente.

- **Supervisor o jefe de fundo:** Consulta indicadores, mapas, reportes y casos confirmados. Recibe avisos por Telegram (SC-02) cuando corresponda según lote o zona.

- **Operador de campo:** Utiliza la app móvil durante recorridos de captura. No accede directamente a la web administrativa, Supabase, Cloudinary Admin ni servicios internos de IA.

## 12.5. Cifrado

- Cifrado en tránsito: Toda comunicación entre app móvil, navegador y plataforma Django se realiza mediante HTTPS/TLS en producción y piloto.

- Cifrado y protección en reposo: Las contraseñas se almacenan con hash seguro de Django; los tokens y verificadores móviles se resguardan con SecureStore; las fotos se almacenan en Cloudinary bajo entrega autenticada o acceso controlado mediante URLs firmadas.

- Protección de claves: La app no contiene claves de Supabase, API secret de Cloudinary ni el token del bot de Telegram. Django genera tickets firmados para operaciones permitidas.

## 12.6. Protección frente a ataques

- **Prevención de Inyecciones:** Se emplea el ORM de Django y consultas parametrizadas, evitando concatenación insegura de SQL.

- **Mitigación de CSRF y XSS:** Los formularios web y peticiones HTMX incorporan token CSRF; las plantillas de Django realizan escape automático de contenido dinámico.

- **Control de Fuerza Bruta:** Se limita el número de intentos fallidos de inicio de sesión y se registran eventos sospechosos para auditoría.

- **Control de acceso a fotos:** Las evidencias no se publican como URLs abiertas. La web solicita derivados o enlaces firmados generados por Django según permisos.

- **Auditoría:** Las acciones críticas, como inicio de sesión, decisión de casos, cambios de roles, revocación de dispositivos y envío de notificaciones, deben quedar registradas.

## 12.7. Plan de pruebas

El plan de aseguramiento de calidad cubre pruebas unitarias, funcionales, integración, seguridad, rendimiento y pruebas de campo. Debe validar el flujo completo desde la captura móvil hasta la decisión del especialista, incluyendo operación offline, sincronización, Cloudinary, ai_tasks, worker de IA, web de revisión, reportes y notificaciones.

- Pruebas móviles: Validan pantallas, navegación, permisos, cámara, GPS, almacenamiento local, validación de calidad, SecureStore, sincronización y reintentos.

- Pruebas de plataforma: Validan modelos Django, API REST, permisos, sesiones, roles, bandeja de casos, mapas, reportes, revisión de evidencia y servicios de notificación.

- Pruebas de IA: Verifican creación de ai_tasks, toma de tareas con SKIP LOCKED, descarga de imágenes desde Cloudinary, inferencia YOLO, registro de detecciones y apertura de casos.

- Pruebas de campo: Simulan baja conectividad, recorridos reales, variación de luz, repetición de capturas y transferencia posterior de datos.

## 12.8. Casos de prueba

Se definen fichas estructuradas que detallan las condiciones iniciales, pasos de ejecución y resultados esperados para validar cada componente:

- **Web:** Ingreso por roles, protección de vistas, bandeja de casos, decisión del especialista, mapas, reportes CSV, auditoría y acceso desde los enlaces de los avisos de Telegram.

- **Móvil:** Inicio de sesión, modo offline, control de sesiones, capturas por lado, validación de imagen, almacenamiento en SQLite, subida a Cloudinary y sincronización con Django.

- **IA:** Creación de tareas, procesamiento por worker, detección de indicios, manejo de errores, reintentos y generación de casos de revisión.

- **Notificaciones:** Creación de avisos solo después de una confirmación (del especialista o «Confirmado por IA», SC-03), envío por Telegram, manejo de errores y conservación de la decisión aunque falle el aviso.

## 12.9. Pruebas funcionales

Las pruebas funcionales verifican que el sistema cumpla los requerimientos del negocio: control de recorridos, captura bilateral, almacenamiento offline, sincronización, visualización cartográfica, revisión de evidencias, confirmación humana y emisión de avisos oficiales. También se comprueba que la IA no genere diagnósticos automáticos y que solo emita la alerta temprana «Confirmado por IA», marcada «falta la revisión del especialista», cuando se cumple la regla de triage aprobada (SC-03).

## 12.10. Pruebas no funcionales

- **Rendimiento:** Validación de tiempos de respuesta aceptables en API, bandeja de casos, mapas y reportes, incluso con volúmenes altos de capturas y casos.

- **Usabilidad:** Verificación de claridad en la app móvil para operadores de campo y facilidad de revisión en la web para el especialista.

- **Disponibilidad y resiliencia:** Comprobación de comportamiento ante pérdida de Internet, fallas temporales de Cloudinary, pausa de base de datos o caída temporal del servidor.

- **Compatibilidad:** Verificación del funcionamiento de la app móvil en los dispositivos definidos para el piloto y de la web en navegadores modernos.

- **Seguridad:** Confirmación de que no existan accesos directos no autorizados a Supabase, Cloudinary Admin, el bot de Telegram o endpoints internos del worker.

## 12.11. Pruebas de integración

Las pruebas de integración validan el flujo extremo a extremo: la app captura una imagen, almacena metadatos en SQLite, solicita un ticket de subida a Django, carga la imagen a Cloudinary, confirma la captura en la API, Django crea una tarea ai_tasks, el worker procesa la imagen con YOLO, se registra una detección, se abre un caso de revisión, el especialista decide en la web y, si confirma, se encola y envía la notificación por Telegram.

## 12.12. Pruebas de rendimiento

Las pruebas de rendimiento evalúan estabilidad y tiempos de respuesta de la plataforma bajo volúmenes representativos de sesiones, capturas, tareas de IA y casos de revisión. Se revisa el uso de índices en PostgreSQL/Supabase, paginación en bandejas, renderizado de mapas con Leaflet, generación de reportes y separación entre el proceso web y el worker para que el análisis de imágenes no bloquee la navegación.

## 12.13. Validación de requerimientos

La validación de requerimientos se realiza mediante una matriz de trazabilidad que conecta historias de usuario, requisitos funcionales, requisitos no funcionales, entregables, casos de prueba y resultados del piloto. Esta matriz debe demostrar que el sistema cumple con captura digital, operación offline, sincronización segura, revisión humana, trazabilidad de evidencia, soporte a decisiones fitosanitarias y reducción de dependencia del registro manual en Excel.
