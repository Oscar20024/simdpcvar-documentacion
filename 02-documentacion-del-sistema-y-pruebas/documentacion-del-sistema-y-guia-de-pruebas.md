
CAPÍTULO 1 · importante

# Antes de probar

**En pocas palabras:**

Este documento explica qué hace el sistema, cómo está construido, cómo cumple cada requerimiento y cómo comprobarlo. Las pruebas se hacen en el **sistema real**, con datos y fotos reales del fundo: algunas acciones tienen efectos reales. Usa las **cuentas de prueba** de esta página.

## 1. Cuentas de prueba

**Plataforma web** — perfil con los 3 roles de la web

**Dirección:** monitoreo.agricolariachuelo.org

**Usuario:** testeador@gmail.com

**Contraseña:** [omitido]

Roles activos: Administrador Especialista fitosanitario Supervisor

**App móvil (Android)** — perfil de operador de campo

**App:** Riachuelo Monitoreo · versión 0.6.1

**Usuario:** testeador2@gmail.com

**Contraseña:** [omitido]

Rol activo: Operador de campo

**La cuenta de la web reúne a los tres tipos de usuario.** **testeador@gmail.com** tiene activos los roles **Administrador**, **Especialista fitosanitario** y **Supervisor** a la vez. Los permisos se suman: con esa sola cuenta se pueden usar **todas las funciones de esos tres tipos de usuario** (decidir casos como especialista; ver panel, mapa, plano, sesiones y reportes como supervisor; y administrar cuentas, celulares, destinatarios, catálogos, papelera y auditoría como administrador). Por ser administrador también puede entrar a la app móvil.

**Reglas para las cuentas compartidas.** 

- **No cambies la contraseña** ni bloquees estas cuentas: las usan todos los evaluadores.
- Para las pruebas que necesitan **un solo rol** (CP-W05, CP-W14, CP-A03, CP-A19), **bloqueo por intentos** (CP-W03) o **contraseña temporal** (CP-W04), crea una cuenta nueva desde «Usuarios → Nueva cuenta» (CP-W23) y úsala.
- Para copiar usuario y contraseña, selecciona el texto del recuadro: es texto, no imagen.

## 2. Acciones con efectos reales

| Acción                  | Efecto                                         | Cómo probarla sin riesgo                                           |
|-------------------------|------------------------------------------------|--------------------------------------------------------------------|
| **Confirmar un caso**   | Envía avisos de Telegram reales.               | Confirma solo lo que sea plaga. Para Telegram usa «Enviar prueba». |
| **Borrar para siempre** | La foto se pierde (servidor, nube, celulares). | Usa «Eliminar esta foto» y luego «Restaurar».                      |
| **Limpieza de fotos**   | Borra en bloque; no se deshace.                | Solo pulsa «Calcular».                                             |
| **Eliminar sesión**     | Borra una salida al campo.                     | No la ejecutes sobre sesiones reales.                              |
| **Revocar un celular**  | Deja de sincronizar; no se deshace.            | No revoques celulares en uso.                                      |
| **Catálogos**           | La app ubica fotos con ellos.                  | Crea el lote «QA» y desactívalo al terminar.                       |

## 3. Cómo reportar un hallazgo

| Campo                   | Qué anotar                             |
|-------------------------|----------------------------------------|
| **Identificador**       | CP-W05, CP-A10… (o «nuevo»)            |
| **Fecha y hora**        | Para ubicarlo en la auditoría          |
| **Usuario y rol**       | Con qué cuenta                         |
| **Pasos**               | Qué hiciste, en orden                  |
| **Esperado / obtenido** | Qué debía pasar y qué pasó             |
| **Evidencia**           | Captura; en la web, la dirección (URL) |

**Todo queda registrado.** Cada acción queda en **Auditoría** con el usuario, la fecha y el cambio.

CAPÍTULO 2 · visión general

# Qué es el sistema

**En pocas palabras:**

SIMDPCVAR es un sistema de **monitoreo fitosanitario** para los viñedos de Agrícola Riachuelo S.A.C. (La Tinguiña, Ica): fotografía las hileras con celulares, una **IA sugiere** posibles plagas, un **especialista confirma** y el equipo recibe un **aviso** con el lugar exacto.

## 1. El problema (AS-IS)

- Entre 10 y 20 operadores revisan a ojo miles de plantas y anotan en papel o en un Excel grande.
- No queda evidencia: ni foto, ni lugar exacto, ni hora.
- El especialista valida después de consolidar el Excel; la respuesta llega tarde.
- No hay alertas y en el campo casi no hay internet.

## 2. La solución (TO-BE)

- Tres celulares Android: un **controlador** y dos **cámaras** en un soporte tipo T.
- Trabajo **sin internet** en el campo; envío al volver la señal.
- **Plataforma web** con IA, revisión del especialista, mapa, reportes y auditoría.
- **Avisos por Telegram** con foto, lugar y botón para llegar a pie.

## 3. Partes del sistema

| Parte                               | Para quién                                                              | Qué hace                                                                                                                                                                |
|-------------------------------------|-------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **App móvil** «Riachuelo Monitoreo» | Operador de campo, administrador; especialista solo para «Ubicar plaga» | Controlador y cámaras: sesión, vinculación por QR, captura, control de calidad, repetición, guardado local y sincronización. «Ubicar plaga» guía hasta la planta.       |
| **Plataforma web**                  | Administrador, especialista fitosanitario, supervisor                   | Panel, casos, decisión del especialista, mapa satelital, plano de hileras, sesiones, reportes CSV, avisos, catálogos, usuarios, celulares, papelera, auditoría y ayuda. |
| **API /api/v1**                     | La app                                                                  | Contrato entre la app y el servidor: ingreso, catálogos, sesiones, secuencias, fotos y alertas.                                                                         |
| **Worker de IA**                    | Automático                                                              | Analiza cada foto con el modelo YOLO, abre o descarta casos, envía los avisos y vacía la papelera vencida.                                                              |
| **Avisos**                          | Jefe de fundo, supervisor de zona                                       | Mensaje de Telegram con foto, lote, hilera, lado, segmento, marcador y botones «Ver caso» y «Cómo llegar».                                                              |

## 4. Alcance del piloto

- Plagas que busca la IA: **chanchito blanco** (cochinilla) y **melaza / fumagina**.
- Lotes del fundo con sus hileras divididas en **10 segmentos** de ≈20 m (segmento = «zona» del informe).
- Celulares **Android** (iPhone pendiente de validar).
- La IA **sugiere indicios**, no diagnostica: la palabra final es del especialista (ver SC-03).
- Fuera del alcance: plagas ocultas, aplicación de tratamientos y reemplazo del especialista.

CAPÍTULO 3 · proceso

# Flujo de negocio

**En pocas palabras:**

Cómo trabaja el fundo con el sistema: quién hace qué, en qué orden y qué pasa con cada foto desde la hilera hasta la acción en campo.

## 1. Actores

| Actor                          | Rol en el negocio                                                        | Dónde trabaja                                |
|--------------------------------|--------------------------------------------------------------------------|----------------------------------------------|
| **Operador de campo**          | Recorre las hileras con el equipo de 3 celulares y captura la evidencia. | App (controlador y cámaras)                  |
| **Especialista fitosanitario** | Revisa los indicios de la IA y decide cada caso con su observación.      | Web; en la app solo «Ubicar plaga»           |
| **Supervisor de zona**         | Sigue el avance, el mapa y los reportes; recibe avisos de su lote.       | Web y Telegram                               |
| **Jefe de fundo**              | Recibe los avisos de casos confirmados y coordina la respuesta.          | Telegram (destinatario) y app «Ubicar plaga» |
| **Administrador**              | Cuentas, celulares, catálogos, destinatarios, papelera y auditoría.      | Web y app                                    |
| **IA (sistema)**               | Analiza cada foto y clasifica: descarta, deja para revisión o confirma.  | Servidor (worker)                            |

## 2. Flujo TO-BE por responsables

Flujo de negocio del monitoreo. La línea fucsia es la confirmación automática por la IA (solicitud de cambio SC-03).

## 3. Paso a paso

| \#  | Etapa                       | Qué ocurre                                                                                                                                                                         | Responsable                | Requerimiento     |
|-----|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|-------------------|
| 1   | **Preparar**                | El administrador mantiene lotes, hileras (10 segmentos) y destinatarios. El controlador descarga los catálogos con internet; los 3 celulares se conectan a la misma red Wi-Fi.     | Administrador · operador   | RF06              |
| 2   | **Sesión y vinculación**    | El controlador crea la sesión y muestra un QR; las cámaras lo escanean. La prueba corta comprueba las dos cámaras.                                                                 | Operador                   | RF11, RNF03       |
| 3   | **Recorrido**               | Por cada hilera: lateral A (ida) y lateral B (vuelta). Cada orden de captura es una **secuencia** con la foto de las dos cámaras y el GPS. En cada marcador se cambia de segmento. | Operador                   | RF01, RF06        |
| 4   | **Calidad**                 | Cada cámara revisa nitidez, exposición y movimiento en ≤ 2 s. Si falla, se repite solo esa cámara (máx. 2 veces).                                                                  | App                        | RF02, RF03, RNF02 |
| 5   | **Cierre y sincronización** | Todo queda en el controlador. Con internet se envían datos y fotos, en orden y sin duplicar; las cámaras borran su copia solo cuando el controlador confirma.                      | Operador · app             | RF04, RF05, RN-05 |
| 6   | **Análisis de IA**          | El worker analiza cada foto y aplica el triage (tabla de abajo). Una sola decisión por lugar con las dos cámaras.                                                                  | Sistema                    | RF07              |
| 7   | **Decisión**                | El especialista revisa la foto, las cajas y el lugar, y decide con observación. Puede corregir con motivo.                                                                         | Especialista               | RF08, RF09, RN-01 |
| 8   | **Aviso y acción**          | Si se confirma, Telegram avisa a los destinatarios del lote. En la app, «Ubicar plaga» guía a pie hasta la planta.                                                                 | Sistema · jefe de fundo    | RF10, RN-02       |
| 9   | **Seguimiento**             | Panel, mapa, plano de hileras, sesiones, reportes CSV y auditoría.                                                                                                                 | Supervisor · jefe · admin. | CU9, RNF05        |

## 4. Estados de un caso

Ciclo de vida de una foto y de su caso. Solo los estados «confirmado» generan aviso (RN-02).

## 5. Reglas de la IA (triage)

| Lo que ve la IA en la foto (cajas con su confianza) | Resultado                                                              |
|-----------------------------------------------------|------------------------------------------------------------------------|
| Ninguna caja ≥ 0,40, o menos de 2 cajas ≥ 0,40      | Descartada por la IA (sin caso; se puede rescatar a mano)              |
| 2 o más cajas con confianza ≥ 0,70                  | Confirmado por IA + aviso marcado «falta la revisión del especialista» |
| 3 o más cajas con confianza ≥ 0,50                  | Confirmado por IA + aviso marcado «falta la revisión del especialista» |
| Cualquier otro caso                                 | Pendiente de revisión: decide el especialista                          |

## 6. AS-IS frente a TO-BE

| Aspecto      | Antes (AS-IS)                                  | Con el sistema (TO-BE)                                                         |
|--------------|------------------------------------------------|--------------------------------------------------------------------------------|
| Registro     | Visual, en papel o Excel, por 10–20 operadores | Foto de cada punto con lote, hilera, lado, segmento, marcador, secuencia y GPS |
| Evidencia    | No hay                                         | Foto original privada, cajas de la IA y decisión con historial                 |
| Validación   | Después de consolidar el Excel                 | Caso a caso en la web, apenas sincroniza el controlador                        |
| Alertas      | No existen                                     | Telegram con foto y «Cómo llegar» al confirmarse                               |
| Ubicación    | Aproximada por hilera                          | Segmento de ≈20 m y GPS; mapa satelital y plano de hileras                     |
| Trazabilidad | No hay                                         | Auditoría de solo inserción: quién hizo qué y cuándo                           |

CAPÍTULO 4 · arquitectura

# Infraestructura y ecosistema

**En pocas palabras:**

Cuatro mundos: el **campo** (3 celulares sin internet), el **servidor** (un VPS con la web y el worker en contenedores), los **servicios externos** (Supabase, Cloudinary, Telegram) y las **personas**. Todo pasa por Django: nadie más toca la base de datos.

Mapa del ecosistema del sistema en producción.

## 1. Principios de la arquitectura

- **Primero sin internet:** el campo nunca depende de la señal; el controlador es la base de datos de campo.
- **Un solo portero:** la app y la web solo hablan con Django; la app nunca habla con Supabase ni tiene claves de Cloudinary.
- **Las fotos no pasan por el servidor:** van directo del celular a Cloudinary con un ticket firmado de un solo uso.
- **Secretos solo en el servidor:** ninguna clave en la app, en el navegador, en el repositorio ni en este documento.

## 2. Quién habla con quién

| De → A                         | Cómo                                         | Qué viaja                                                         | Cuándo                     |
|--------------------------------|----------------------------------------------|-------------------------------------------------------------------|----------------------------|
| Controlador ↔ cámaras          | Wi-Fi local (WebSocket y HTTP), sin internet | Órdenes, latidos, fotos JPEG y confirmaciones                     | Durante la pasada          |
| Controlador → Django           | HTTPS a /api/v1 con token JWT                | Sesiones, pasadas, secuencias, incidencias, confirmación de fotos | Al sincronizar             |
| Controlador → Cloudinary       | HTTPS con ticket firmado por Django          | La foto original (≤ 10 MB)                                        | Al sincronizar             |
| Django ↔ Supabase              | PostgreSQL con conexión cifrada              | Todos los datos                                                   | Siempre                    |
| Django → Cloudinary            | API de Cloudinary                            | Firma de tickets y URLs temporales para ver fotos                 | Al subir y al mostrar      |
| Worker → Cloudinary / Telegram | URL firmada / Bot API (HTTPS)                | Descarga de la foto; mensaje con foto y botones                   | Por cada foto y cada aviso |
| Navegador → Django             | HTTPS a través de Traefik                    | Páginas de la web (HTMX refresca partes)                          | Cuando alguien entra       |

## 3. Servidor y contenedores

| Elemento                | Detalle                                                                                                                                                                       |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Servidor**            | VPS Linux (Hostinger KVM 1 · Ubuntu 24.04 · 1 vCPU · 3,8 GB RAM · 48 GB de disco).                                                                                            |
| **Dominio y HTTPS**     | monitoreo.agricolariachuelo.org con certificado de Let's Encrypt, servido por **Traefik** (proxy inverso).                                                                    |
| **Contenedor «web»**    | gunicorn + Django (2 procesos × 4 hilos; ≈700 MB y 0,8 CPU). Aplica las migraciones al arrancar; sirve la web, la API y los estáticos (WhiteNoise).                           |
| **Contenedor «worker»** | Comando worker de IA (YOLO en ONNX), avisos de Telegram y papelera (≈1,2 GB y 0,7 CPU). Sin puertos abiertos.                                                                 |
| **Endurecimiento**      | Usuario sin privilegios, sistema de archivos de solo lectura, sin capacidades extra, límites de memoria y CPU; secretos en un archivo del servidor con permisos restringidos. |

## 4. Servicios externos

| Servicio               | Para qué                                                                                                                       | Plan      | Límite a vigilar                   |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------|-----------|------------------------------------|
| **Supabase**           | Base de datos PostgreSQL (≈30 tablas creadas por las migraciones de Django). RLS activo sin políticas: solo Django se conecta. | Free      | 500 MB; se pausa si no se usa      |
| **Cloudinary**         | Fotos originales privadas (tipo authenticated) y miniaturas por URL firmada.                                                   | Free      | 25 créditos al mes; 10 MB por foto |
| **Telegram Bot API**   | Avisos a personas y grupos.                                                                                                    | Gratis    | Límites de envío holgados          |
| **GitHub**             | Repositorios de la plataforma y de la app.                                                                                     | Gratis    | —                                  |
| **Expo EAS**           | Compilar el APK de Android.                                                                                                    | Gratis    | Cupo mensual de compilaciones      |
| **Esri / Google Maps** | Imagen satelital del mapa y «Cómo llegar».                                                                                     | Sin clave | Solo teselas y enlaces públicos    |

## 5. Equipo de campo

| Elemento                                           | Uso                                                                  |
|----------------------------------------------------|----------------------------------------------------------------------|
| 3 celulares Android con la misma versión de la app | 1 controlador (con GPS encendido) y 2 cámaras.                       |
| Soporte tipo T (poste y barra)                     | Lleva las dos cámaras a la misma altura, apuntando a la hilera.      |
| Router o hotspot                                   | Red Wi-Fi local sin internet para que los 3 celulares se comuniquen. |
| Power banks y correas                              | Autonomía de la jornada y fijación segura de los equipos.            |

## 6. Despliegue: del código al servidor y al celular

- **Web:** antes de subir se corren las pruebas automáticas (307 en la v1.6.1); en el servidor un script descarga el código, reconstruye la imagen, aplica las migraciones y reinicia web y worker (≈1–2 min).
- **App:** *npm run validate* (tipos, lint y Jest), se sube la versión y se compila el APK con Expo EAS; se instala la misma versión en los tres celulares.
- **Entorno de desarrollo:** en la laptop la web corre con datos de demostración, Cloudinary e IA simulados, sin tocar el piloto.

**Qué no incluye este documento.** Direcciones IP, rutas del servidor, comandos con credenciales, variables de entorno, tokens, claves ni números de chat. Esa información vive solo en el servidor y la conoce el responsable técnico.

CAPÍTULO 5 · uso interno

# Herramientas y tecnologías del proyecto

**En pocas palabras:**

Lo que el equipo usa por dentro para construir, probar, desplegar y gestionar el sistema: lenguajes, frameworks, librerías, servicios y herramientas de trabajo.

## 1. Plataforma web, API y worker

| Capa                   | Tecnología                                                               | Uso en el proyecto                                                                                            |
|------------------------|--------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| Lenguaje               | **Python 3**                                                             | Backend, API, worker de IA y scripts.                                                                         |
| Framework              | **Django 5.2 LTS**                                                       | Modelos, migraciones, reglas de negocio, permisos, páginas y auditoría.                                       |
| API                    | **Django REST Framework** + **SimpleJWT** + **drf-spectacular**          | API /api/v1 para la app, tokens JWT (acceso 15 min, renovación 14 días con rotación) y documentación OpenAPI. |
| Interfaz web           | **Django Templates** + **HTMX** (django-htmx) + CSS propio, fuente Inter | Páginas generadas en el servidor; HTMX actualiza partes sin recargar. Sin JavaScript en línea (CSP).          |
| Mapas                  | **Leaflet 1.9.4** (copia local) + teselas Esri                           | Mapa satelital, dibujo de lotes e hileras, puntos de casos.                                                   |
| Servidor de aplicación | **gunicorn** + **WhiteNoise**                                            | Procesos de la web y archivos estáticos.                                                                      |
| Datos                  | **PostgreSQL** (Supabase) con **psycopg 3**; SQLite en desarrollo        | Base de datos del sistema.                                                                                    |
| Fotos                  | **Cloudinary** (SDK de Python) + **Pillow**                              | Tickets firmados, URLs temporales, miniaturas y foto del aviso.                                               |
| IA                     | **ONNX Runtime** + **NumPy**                                             | Ejecuta el modelo YOLO exportado a ONNX en CPU, con mosaicos de 1280 px (20 % de solape).                     |
| Configuración          | **django-environ**, **requests**                                         | Variables de entorno y llamadas a Telegram.                                                                   |

## 2. App móvil

| Capa              | Tecnología                                                            | Uso en el proyecto                                                                                      |
|-------------------|-----------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| Lenguaje y base   | **TypeScript** · **React Native** · **Expo SDK 57** · **Expo Router** | App única para controlador, cámaras y «Ubicar plaga».                                                   |
| Estado            | **Zustand**                                                           | Sesión, controlador y cámaras.                                                                          |
| Cámara y sensores | **expo-camera**, **expo-sensors**, **expo-image-manipulator**         | Captura de 12 MP, quietud del celular y JPEG 0,85.                                                      |
| Datos locales     | **expo-sqlite**, **expo-file-system**, **expo-secure-store**          | Base local con migraciones, fotos en el almacenamiento privado de la app y tokens en el almacén seguro. |
| Red y ubicación   | **react-native-tcp-socket**, **expo-network**, **expo-location**      | Servidor WebSocket/HTTP local del controlador, estado de la red y GPS.                                  |
| Mapa              | **react-native-webview** + Leaflet embebido                           | «Ubicar plaga» con imagen satelital y «Cómo llegar» (Google Maps).                                      |
| Compilación       | **Expo EAS Build**                                                    | APK de Android del piloto.                                                                              |

## 3. Inteligencia artificial

| Elemento               | Detalle                                                                                                   |
|------------------------|-----------------------------------------------------------------------------------------------------------|
| Modelo                 | **YOLO** (Ultralytics, YOLO26n) exportado a **ONNX**; versión 2026.10-v2 (provisional).                   |
| Datos de entrenamiento | Fotos propias del fundo + ≈910 imágenes externas (Roboflow); clases chanchito blanco y melaza / fumagina. |
| Entrenamiento          | **Google Colab** con GPU; verificación con un script propio (meta: ≥ 85 % de detección).                  |
| Configuración          | Clases, umbrales de triage y mosaicos se configuran por modelo; solo un modelo activo a la vez.           |

## 4. Herramientas de trabajo del equipo

| Para qué                      | Herramienta                                                                                                                                              |
|-------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Código y control de versiones | **VS Code**, **Git** y **GitHub** (un repositorio para la plataforma y otro para la app).                                                                |
| Gestión ágil                  | **Scrum** con tablero en **Jira** (épicas, historias y sprints).                                                                                         |
| Pruebas                       | Pruebas automáticas de Django (307 en la v1.6.1), **Jest**, ESLint y verificación de tipos en la app; herramientas de verificación de la sincronización. |
| Contenedores y despliegue     | **Docker** y **Docker Compose**, **Traefik**, scripts de instalación y actualización.                                                                    |
| Diagnóstico                   | Comando de diagnóstico de la plataforma, registros con identificador de petición (traceId) y página «Estado de la IA».                                   |
| Documentación                 | Archivos maestros de la web y de la app, decisiones de arquitectura (ADR), manuales en PDF y ayuda en cada pantalla.                                     |
| Asistencia de desarrollo      | Asistente de IA (Claude) para programación, revisión y documentación, siempre con aprobación del equipo.                                                 |

CAPÍTULO 6 · seguridad

# Roles y permisos

**En pocas palabras:**

Cada cuenta tiene uno o más roles y los permisos se suman. El servidor comprueba el permiso en cada acción: ocultar un botón no basta, y una dirección escrita a mano devuelve «sin acceso».

| Función                                                                | Admin. | Especialista | Supervisor | Operador |
|------------------------------------------------------------------------|--------|--------------|------------|----------|
| Web: panel, casos, mapa, plano, sesiones, reportes, avisos, ayuda      | ✓      | ✓            | ✓          |          |
| Web: decidir y corregir casos; abrir un caso a mano                    |        | ✓            |            |          |
| Web: estado de la IA                                                   | ✓      | ✓            |            |          |
| Web: catálogos y dibujo en el mapa                                     | ✓      |              | ✓          |          |
| Web: usuarios, celulares, destinatarios, auditoría, papelera, limpieza | ✓      |              |            |          |
| App: monitoreo (controlador y cámaras) y sincronización                | ✓      |              |            | ✓        |
| App: «Ubicar plaga»                                                    | ✓      | ✓            |            | ✓        |

## 1. Tipos de cuenta

| Tipo                 | Roles posibles                                                      | Cómo nace                                                                                                                                                                               |
|----------------------|---------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Cuenta de la web** | Especialista fitosanitario, Supervisor y/o Administrador (se suman) | Solo la crea el administrador: «Usuarios → Nueva cuenta → Plataforma web». No hay registro público en la web.                                                                           |
| **Cuenta de la app** | Solo Operador de campo (rol único)                                  | La persona la pide desde la app («Crear cuenta») y el administrador la **aprueba sin elegir rol**: queda como Operador de campo. También se puede crear con «Nueva cuenta → App móvil». |

**Corrección v1.6.1 (SC-08).** Antes, al aprobar una solicitud que llegaba desde la app, la web ofrecía elegir roles de la web. Ahora la aprobación no pide rol, la cuenta queda como «Operador de campo» y el editor de roles solo aparece en cuentas de la web con sus tres roles.

**La cuenta de evaluación de la web.** **testeador@gmail.com** tiene las columnas Administrador, Especialista y Supervisor a la vez, así que puede usar todas las funciones de la web. **testeador2@gmail.com** es una cuenta de la app (Operador de campo).

CAPÍTULO 7 · RF01–RF11

# Requerimientos funcionales

**En pocas palabras:**

Texto oficial de la Tabla 27 del informe (no se modifica) y, al lado, cómo lo cumple el sistema que está funcionando hoy, dónde verlo y qué prueba lo verifica. Si hay una diferencia, se justifica con una solicitud de cambio (capítulo 10).

Estado: Cumple igual al informe · Cumple · SC-xx cumple con un cambio que requiere aprobación · Cumple · aclarado solo aclara la interpretación

| Código | Requerimiento (Tabla 27)                                                                                                                                                                                         | Cómo lo cumple el sistema actual                                                                                                                                                                                            | Dónde verlo                                      | Pruebas                | Estado                                                    |
|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------|------------------------|-----------------------------------------------------------|
| RF01   | El sistema debe permitir capturar fotos bilaterales de cada planta por medio de dispositivos móviles                                                                                    | Dos cámaras en el soporte toman el mismo punto a la vez (doble evidencia); la planta se cubre por sus dos lados con las pasadas A (ida) y B (vuelta).                                                                       | App: pasada activa. Web: sesión → pasadas A y B. | CP-A09, CP-A16, CP-W18 | Cumple · SC-04             |
| RF02   | El sistema debe validar automáticamente la calidad de cada foto (nitidez, exposición y movimiento)                                                                                      | Cada cámara evalúa la foto en el celular en menos de 2 s; en automático solo dispara con el celular quieto.                                                                                                                 | App: modo prueba. Web: calidad en la foto.       | CP-A10, CP-A12         | Cumple                     |
| RF03   | El sistema debe permitir repetir la captura del lado de la planta cuya foto no fuese valida                                                                                             | Se repite solo la cámara que falló, como máximo 2 veces; no sale otra secuencia hasta resolverla. En el último intento la foto se acepta y se marca para revisión técnica.                                                  | App: botón dorado «REPETIR».                     | CP-A10, CP-A11         | Cumple · aclarado en SC-05 |
| RF04   | El sistema debe almacenar localmente las fotos cuando no haya conexión a internet                                                                                                       | Base local SQLite y almacenamiento privado del controlador; las cámaras transfieren por Wi-Fi local.                                                                                                                        | App en modo avión.                               | CP-A13                 | Cumple                     |
| RF05   | El sistema debe sincronizar automáticamente las fotografías hacia la plataforma cuando la conexión se restablezca                                                                       | Envío ordenado, idempotente y reanudable; automático con Wi-Fi cuando no hay sesión abierta, o con «Sincronizar ahora».                                                                                                     | App: Sincronizar. Web: Sesiones.                 | CP-A14                 | Cumple                     |
| RF06   | El sistema debe asociar cada foto a los datos de lote, hilera, zona, lado, marcador, secuencia y coordenada GPS                                                                         | Cada secuencia guarda lote, hilera, lado, segmento (= zona), marcador, número de secuencia y GPS; sin GPS se ubica por marcador y segmento.                                                                                 | Web: página de la foto y del caso.               | CP-A09, CP-W10, CP-W19 | Cumple · aclarado en SC-06 |
| RF07   | El sistema debe analizar las imágenes sincronizadas mediante un modelo de IA y señalar posibles indicios de plaga                                                                       | Modelo YOLO en el servidor; cajas con clase y confianza; triage para descartar, revisar o confirmar.                                                                                                                        | Web: Estado de la IA; caso.                      | CP-W09, CP-W15, CP-W22 | Cumple                     |
| RF08   | El sistema debe permitir al especialista fitosanitario visualizar los indicios en la plataforma                                                                                         | Bandeja y caso con la foto, las cajas alineadas, zoom, original, la otra cámara y el mapa.                                                                                                                                  | Web: Casos y Mapa.                               | CP-W07, CP-W09, CP-W16 | Cumple                     |
| RF09   | El sistema debe permitir al especialista confirmar o descartar los hallazgos                                                                                                            | Confirmado · Posible plaga · Descartado · Evidencia insuficiente, con observación; corrección con motivo e historial.                                                                                                       | Web: caso.                                       | CP-W11, CP-W13, CP-W14 | Cumple · SC-03             |
| RF10   | El sistema debe enviar una notificación automática vía WhatsApp al jefe de fundo y supervisor de zona inmediatamente después de que el especialista confirme un hallazgo fitosanitario. | Aviso automático por **Telegram** (no WhatsApp) con foto, lugar y botones a cada destinatario del lote, al confirmar el especialista; la IA con alta confianza también avisa, marcado «falta la revisión del especialista». | Web: Avisos y Destinatarios.                     | CP-W12, CP-W25         | Cumple · SC-02, SC-03      |
| RF11   | El sistema debe exigir la vinculación exitosa de los tres dispositivos (controlador, cámara izquierda, cámara derecha) antes de habilitar el inicio del recorrido                       | Sin las dos cámaras CONECTADAS y la prueba corta aprobada no se puede iniciar una pasada.                                                                                                                                   | App: Vincular y Prueba corta.                    | CP-A05, CP-A06, CP-A07 | Cumple                     |

**Resultado.** Los 11 requerimientos funcionales están implementados. Tres tienen diferencias justificadas (RF01, RF09, RF10) y dos solo aclaraciones (RF03, RF06).

CAPÍTULO 8 · RNF01–RNF07 · RN-01–RN-06

# No funcionales y reglas de negocio

**En pocas palabras:**

Texto oficial de las Tablas 28 y 29 y cómo los atiende el sistema actual.

| Código | Requerimiento (Tabla 28)                                                                                                                                                                                                                    | Cómo se cumple                                                                                                                                                                                               | Verificación                         | Estado                                                                     |
|--------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------|----------------------------------------------------------------------------|
| RNF01  | El sistema debe operar por medio de servicios en la nube, sin necesidad de infraestructura de servidores físicos locales en el fundo                                                                               | Servidor (VPS), base de datos, fotos e IA en la nube; en el fundo solo hay celulares y un router.                                                                                                            | Capítulo 4.                          | Cumple SC-07 |
| RNF02  | La validación de calidad debe realizarse en un tiempo máximo de 2 segundos por fotografía                                                                                                                          | Evaluación en el propio celular; el modo prueba muestra el tiempo y las métricas.                                                                                                                            | CP-A12.                              | Cumple                                      |
| RNF03  | La aplicación móvil debe tener una interfaz intuitiva que permita iniciar un recorrido en menos de 3 pasos para el personal de campo                                                                               | La sesión se crea con 2 toques (Nueva sesión → Crear sesión). La vinculación y la prueba corta son controles exigidos por RF11.                                                                              | CP-A05.                              | Cumple · aclarado en SC-06                  |
| RNF04  | El sistema debe soportar la consulta web y almacenamiento de datos de 50,000+ plantas manteniendo un tiempo de respuesta de la API menor a 1.5 segundos                                                            | Medido con 100 000 fotos sintéticas: bandeja 45–61 ms (siempre 6 consultas), caso 103 ms, mapa 268 ms con 5 000 puntos, plano 18 ms, panel 15 ms. Peticiones de más de 1,5 s quedan marcadas en el registro. | DevTools → Red.                      | Cumple                                      |
| RNF05  | El acceso a la plataforma debe ser protegido por autenticación y control de roles según el usuario                                                                                                                 | Inicio de sesión obligatorio en web y app; permiso comprobado en el servidor; bloqueo por intentos; auditoría.                                                                                               | CP-W01 a CP-W05.                     | Cumple                                      |
| RNF06  | La aplicación debe operar en el modo sin conexión durante la captura, sincronizándose de manera automática al tener señal                                                                                          | Ver RF04 y RF05; ingreso sin internet hasta 7 días después del último ingreso con internet.                                                                                                                  | CP-A02, CP-A13, CP-A14.              | Cumple                                      |
| RNF07  | Las imágenes y metadatos almacenados localmente deben custodiarse dentro del almacenamiento privado y seguro de la aplicación móvil (expo-secure-store o react-native-keychain) hasta completar su sincronización. | Fotos en el almacenamiento privado de la app (no en la galería) y metadatos en su base SQLite privada; tokens y credenciales en expo-secure-store. No se borran hasta confirmar la sincronización.           | Las fotos no aparecen en la galería. | Cumple · aclarado en SC-06                  |

## 1. Reglas de negocio (Tabla 29)

| Código | Regla | Cómo la cumple el sistema | Estado |
|---|---|---|---|
| RN-01 | Validación del especialista El sistema no dejara que el estado del hallazgo cambie a «confirmado» ni registrar alguna observación fitosanitaria si la acción no viene del usuario autenticado con el rol de «Especialista fitosanitario». | Permiso exclusivo «decidir» comprobado en el servidor (CP-W14). La IA no registra observaciones; su confirmación automática es un estado distinto (SC-03). | Cumple · SC-03 |
| RN-02 | Bloqueo de acciones sobre un hallazgo no confirmado Ningún hallazgo podrá pasarse al estado «observación fitosanitaria registrada» ni dar ninguna notificación si su estado está como «pendiente» o «descartado». | Solo los estados confirmados generan avisos; descartar anula los avisos pendientes. | Cumple |
| RN-03 | Metadatos obligatorios por fotografía Rechazo de fotos sin lote, hilera, zona, lado, marcador y secuencia; GPS cuando exista una lectura válida, si no, ubicación por marcador y sector. | La sincronización rechaza datos incompletos; sin GPS se ubica por marcador y segmento. | Cumple |
| RN-04 | Repetición por calidad de imagen Si la foto no supera la configuración de nitidez, exposición o movimiento, se marca solo el lado afectado como «pendiente de recaptura». | Se repite solo la cámara que falló (máximo 2 intentos). | Cumple · aclarado en SC-05 |
| RN-05 | Persistencia local sin conexión Las fotos tomadas sin conexión se guardan en el dispositivo y no se eliminan hasta confirmar su sincronización. | Las cámaras borran su copia solo cuando el controlador confirma; el controlador conserva todo hasta sincronizar. | Cumple |
| RN-06 | Carácter no vinculante del análisis de IA El resultado de la IA se registra como «indicio sugerido» y no podrá derivar en observación fitosanitaria ni alerta sin la confirmación del especialista. | La IA siempre «sugiere indicios». Con alta confianza confirma y avisa sin esperar al especialista, marcado «falta la revisión del especialista»; se puede apagar dejando vacío el umbral automático. | Cumple · SC-03 |

CAPÍTULO 9 · HU01–HU17

# Historias de usuario

**En pocas palabras:**

Las cinco historias del informe (Anexo 52) y doce historias complementarias que describen lo que el sistema actual ya hace. Las complementarias no cambian ningún requerimiento: lo detallan.

## 1. Historias del informe

**HU01 · Prototipo físico de captura bilateral** — Informe (Anexo 52)

Como **operador de monitoreo**, quiero portar un prototipo físico de captura bilateral fijado al brazo, con soporte tipo T para dos dispositivos móviles, para capturar ambos lados de las plantas sin sostener los equipos con las manos.

- Captura fotos bilaterales de cada planta con los celulares montados en el soporte.
- Exige la vinculación de los tres dispositivos antes de iniciar el recorrido (RF11).
- El soporte mantiene la alineación y la fijación sin interrumpir la caminata.

**Traza:** RF01, RF11 · CU1 · CP-A05, CP-A06, CP-A09 · SC-04

**HU02 · Validación automática de calidad y almacenamiento offline** — Informe (Anexo 52)

Como **operador**, quiero que la aplicación verifique automáticamente la nitidez y la iluminación de cada foto antes de cambiar de zona, para evitar registrar imágenes inservibles.

- Valida nitidez, exposición y movimiento antes de guardar la evidencia.
- Si una captura falla, indica la cámara que debe repetirse.
- Las fotos aprobadas quedan en almacenamiento privado y los metadatos en SQLite hasta sincronizar.

**Traza:** RF02, RF03, RF04 · CU2 · CP-A10 a CP-A13

**HU03 · Visualización de hallazgos en mapa e IA** — Informe (Anexo 52)

Como **especialista fitosanitario**, quiero ver los posibles indicios de la IA en un mapa del fundo, con la foto y las cajas de detección, para confirmar o descartar el caso con trazabilidad.

- La bandeja y el caso muestran la foto, las cajas alineadas y la ubicación operativa.
- Solo el especialista confirma, descarta, marca posible plaga o evidencia insuficiente.
- Cada decisión queda en el historial y en la auditoría.

**Traza:** RF07, RF08, RF09 · CU4–CU6 · CP-W07 a CP-W16

**HU04 · Notificaciones por mensajería oficial** — Informe (Anexo 52)

Como **supervisor o jefe de fundo**, quiero recibir notificaciones solo cuando un caso haya sido confirmado, para coordinar la respuesta operativa.

- El aviso sale solo con estado confirmado (RN-02).
- Lleva foto, lote, hilera, lado, segmento, marcador y botones «Ver caso» y «Cómo llegar».
- Canal vigente: Telegram (SC-02).

**Traza:** RF10 · CU8 · CP-W12, CP-W25 · SC-02, SC-03

**HU05 · Administración de la plataforma** — Informe (Anexo 52)

Como **administrador de la plataforma**, quiero gestionar usuarios, roles, dispositivos, catálogos y destinatarios de avisos, para mantener el sistema controlado durante el piloto.

- Se hace desde pantallas internas de la web (Django Admin queda solo para el responsable técnico).
- Cada cambio queda en la auditoría.
- Las cuentas de la app se aprueban sin elegir rol (SC-08).

**Traza:** RNF05 · CP-W23 a CP-W30 · SC-08

## 2. Historias complementarias (sistema actual)

**HU06 · Sincronizar al volver la señal** — Complementaria

Como **operador de campo**, quiero que todo lo capturado se envíe solo cuando haya internet, para no perder ni duplicar evidencia.

- El envío es ordenado y se reanuda donde quedó.
- Reintentar no duplica sesiones ni fotos.
- La web muestra la sesión con sus pasadas y fotos.

**Traza:** RF04, RF05, RNF06 · CP-A13, CP-A14

**HU07 · Vincular los tres celulares** — Complementaria

Como **operador de campo**, quiero vincular las dos cámaras con un QR y probarlas antes de salir, para no empezar un recorrido con un equipo fallando.

- Las cámaras pasan a CONECTADA al escanear el QR.
- Sin las dos cámaras y la prueba corta aprobada no se inicia la pasada.

**Traza:** RF11, RNF03 · CP-A05 a CP-A07

**HU08 · Repetir solo la cámara que falló** — Complementaria

Como **operador de campo**, quiero repetir únicamente la foto de la cámara rechazada, para no rehacer todo el punto.

- Botón dorado «REPETIR CÁMARA X · INTENTO a DE 2».
- En el último intento la foto se acepta y se marca para revisión.

**Traza:** RF03, RN-04 · CP-A10, CP-A11 · SC-05

**HU09 · Catálogos del fundo** — Complementaria

Como **administrador o supervisor**, quiero mantener lotes, hileras, segmentos y marcadores, y dibujarlos en el mapa, para que cada foto quede bien ubicada.

- Cada hilera se reparte en 10 segmentos con marcadores.
- Desactivar no borra; queda en la auditoría.

**Traza:** RF06 · CP-A04, CP-W27

**HU10 · Decidir casos en la bandeja** — Complementaria

Como **especialista fitosanitario**, quiero filtrar los casos, abrir uno y decidir con una observación, para atender primero lo más urgente.

- Filtros por estado, lote y fechas; 25 por página.
- Corregir exige motivo y conserva la decisión anterior.

**Traza:** RF08, RF09, RN-01 · CP-W07, CP-W11, CP-W13

**HU11 · Rescatar una foto descartada por la IA** — Complementaria

Como **especialista fitosanitario**, quiero abrir a mano un caso de una foto que la IA descartó, para corregir un falso negativo.

- «Descartadas por la IA» lista esas fotos.
- «Abrir caso para revisión» la pasa a la bandeja como caso manual.

**Traza:** RF07, RN-06 · CP-W15

**HU12 · Llegar hasta la plaga** — Complementaria

Como **jefe de fundo, operador o especialista**, quiero ver las alertas en un mapa y que la app me guíe a pie, para aplicar la acción en la planta correcta.

- Distancia y dirección desde mi posición.
- «Cómo llegar» abre Google Maps a pie.

**Traza:** RF10 (apoyo), CU8 · CP-A18, CP-A19 · SC-09

**HU13 · Seguimiento del fundo** — Complementaria

Como **supervisor o jefe de fundo**, quiero ver el panel, el mapa, el plano de hileras y descargar reportes, para conocer el estado fitosanitario sin ir al campo.

- Indicadores por periodo y tarjetas que abren la lista filtrada.
- Reporte CSV con registro en la auditoría.

**Traza:** CU9 · CP-W06, CP-W16, CP-W17, CP-W20

**HU14 · Auditoría** — Complementaria

Como **administrador**, quiero consultar quién hizo qué y cuándo, para rendir cuentas de cada decisión y cambio.

- Registros de solo inserción, agrupados por categoría.
- El ID de la entidad muestra toda su historia.

**Traza:** RNF05 · CP-W30

**HU15 · Papelera y limpieza de fotos** — Complementaria

Como **administrador**, quiero quitar fotos de prueba o equivocadas con posibilidad de restaurarlas, para cuidar el espacio sin perder evidencia por error.

- La foto va a la papelera 30 días antes de borrarse.
- Borrar en bloque exige escribir ELIMINAR.

**Traza:** RN-05 · CP-W28, CP-W29 · SC-09

**HU16 · Destinatarios de Telegram** — Complementaria

Como **administrador**, quiero registrar a cada persona o grupo que recibe avisos y por qué lotes, para que el aviso llegue a quien debe actuar.

- «Buscar chats» y «Usar este chat» llenan el formulario.
- «Enviar prueba» confirma la entrega.

**Traza:** RF10 · CP-W25, CP-W26 · SC-02

**HU17 · Aprobar cuentas de la app sin elegir rol** — Complementaria

Como **administrador**, quiero aprobar con un clic las cuentas que piden los operadores desde la app, para no asignar por error roles de la web a un operador.

- Al aprobar, la cuenta queda como Operador de campo.
- El editor de roles solo existe en cuentas de la web.

**Traza:** RNF05 · CP-W35, CP-W36, CP-A21 · SC-08

CAPÍTULO 10 · control de cambios

# Solicitudes de cambio

**En pocas palabras:**

Las diferencias entre el informe y el sistema que funciona hoy no se corrigen en los requerimientos: se justifican aquí con solicitudes de cambio (formato FGPR-410-06), que se agregarán a los anexos de la documentación principal. SC-01 (arquitectura Django + Supabase) ya está en el Anexo 37.

| N.º       | Cambio                                                     | Tipo                              | Requerimientos afectados        |
|-----------|------------------------------------------------------------|-----------------------------------|---------------------------------|
| **SC-02** | Avisos por Telegram en lugar de WhatsApp                   | Acción correctiva · Actualización | RF10, CU8, HU04, Tabla 30       |
| **SC-03** | Confirmación automática por la IA y estado «Posible plaga» | Actualización                     | RF09, RF10, RN-01, RN-02, RN-06 |
| **SC-04** | Cobertura bilateral con laterales A y B                    | Actualización                     | RF01, CU1, HU01                 |
| **SC-05** | Límite de repeticiones por calidad                         | Acción preventiva                 | RF03, RN-04                     |
| **SC-06** | Aclaraciones de interpretación (RF06, RNF03, RNF07)        | Actualización (aclaración)        | RF06, RNF03, RNF07              |
| **SC-07** | Ajustes de la arquitectura tecnológica (Tabla 30)          | Actualización                     | Tabla 30, RNF01, SC-01          |
| **SC-08** | Aprobar cuentas de la app sin elegir rol                   | Reparación de defecto             | RNF05, HU05, HU17               |
| **SC-09** | Funciones agregadas al alcance                             | Actualización                     | RF06, RF10, CU8, CU9            |

**Estado de aprobación.** Todas quedan **pendientes de revisión del Comité de Control de Cambios** (jefe de fundo y Project Manager). Solicitantes: equipo del proyecto SIMDPCVAR. Fecha de registro: 14/11/2026.

**SC-02 · Avisos por Telegram en lugar de WhatsApp** — Acción correctiva · Actualización

|                             |                                                                                                                                                                                                                                                                                                                                                                    |
|-----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Requerimientos afectados    | RF10, CU8, HU04, Tabla 30                                                                                                                                                                                                                                                                                                                                          |
| Problema o situación actual | RF10 pide el aviso «vía WhatsApp». WhatsApp Cloud API exige cuenta de empresa en Meta, número verificado y plantillas aprobadas. Por problemas de configuración esos trámites no avanzaron y los avisos no podían salir.                                                                                                                                           |
| Descripción del cambio      | Enviar los avisos por la Bot API de Telegram, a personas o a un grupo por lote. El aviso lleva foto con las cajas de la IA, lote, hilera, lado, segmento, marcador, fecha, confianza, observación y botones «Ver caso» y «Cómo llegar». El administrador registra los destinatarios en la web («Buscar chats», «Enviar prueba»). Se retiró el cliente de WhatsApp. |
| Razón del cambio            | Telegram se activa en minutos, es gratuito y no exige aprobaciones de terceros. Sin el cambio, RF10 no se podía cumplir en el piloto.                                                                                                                                                                                                                              |
| Efectos en el proyecto      | Corto plazo: RF10 operativo de inmediato. Largo plazo: el canal se puede volver a cambiar sin tocar la regla de negocio (solo cambia el envío).                                                                                                                                                                                                                    |
| Efectos externos            | Los destinatarios deben instalar Telegram y escribir al bot. Las fotos de casos confirmados quedan en sus chats.                                                                                                                                                                                                                                                   |

**SC-03 · Confirmación automática por la IA y estado «Posible plaga»** — Actualización

|                             |                                                                                                                                                                                                                                                                                                                                                     |
|-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Requerimientos afectados    | RF09, RF10, RN-01, RN-02, RN-06                                                                                                                                                                                                                                                                                                                     |
| Problema o situación actual | En el piloto casi todas las fotos terminaban como pendientes y el especialista no alcanzaba a revisarlas; además, cuando la IA estaba muy segura, el aviso esperaba al especialista. Faltaba una decisión para «no es seguro, hay que ir a mirar».                                                                                                  |
| Descripción del cambio      | Triage: se descarta sola la foto sin indicios; queda pendiente lo intermedio; con 2 cajas ≥ 0,70 o 3 cajas ≥ 0,50 el caso pasa a «Confirmado por IA» y avisa, marcado «falta la revisión del especialista». El especialista sigue decidiendo esos casos y puede corregirlos. Nueva decisión «Posible plaga» (sin aviso, visible en «Ubicar plaga»). |
| Razón del cambio            | Responder antes ante plagas evidentes y concentrar al especialista en los casos dudosos. La confirmación automática se apaga dejando vacío el umbral automático, con lo que el sistema vuelve a RN-06 al pie de la letra.                                                                                                                           |
| Efectos en el proyecto      | Corto plazo: menos casos pendientes y avisos más rápidos. Largo plazo: medir falsos positivos y ajustar umbrales por modelo.                                                                                                                                                                                                                        |
| Efectos externos            | Requiere aprobación del especialista y del jefe de fundo porque flexibiliza RN-01 y RN-06.                                                                                                                                                                                                                                                          |

**SC-04 · Cobertura bilateral con laterales A y B** — Actualización

|                             |                                                                                                                                                                                                                                 |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Requerimientos afectados    | RF01, CU1, HU01                                                                                                                                                                                                                 |
| Problema o situación actual | RF01 y la propuesta hablan de una cámara izquierda y una derecha que fotografían a la vez los dos lados de cada planta. En la vid en espaldera, desde el pasillo solo se ve una cara de la hilera.                              |
| Descripción del cambio      | Las dos cámaras van en la barra y fotografían el mismo punto de la misma cara (doble evidencia). Cada hilera se recorre dos veces: lateral A de ida y lateral B de vuelta. La IA decide una vez por lugar usando las dos fotos. |
| Razón del cambio            | Así cada planta queda fotografiada por sus dos lados y cada punto tiene dos fotos para validar. Sin el cambio una cara de la hilera quedaría sin evidencia.                                                                     |
| Efectos en el proyecto      | Corto plazo: dos pasadas por hilera. Largo plazo: más evidencia por punto para entrenar el modelo.                                                                                                                              |
| Efectos externos            | El soporte y la app se usan igual; cambia el procedimiento de recorrido (capacitación).                                                                                                                                         |

**SC-05 · Límite de repeticiones por calidad** — Acción preventiva

|                             |                                                                                                                                                                                                                  |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Requerimientos afectados    | RF03, RN-04                                                                                                                                                                                                      |
| Problema o situación actual | Cuando una cámara fallaba una y otra vez (borrosa, oscura o en movimiento), la repetición no terminaba nunca y bloqueaba el recorrido.                                                                           |
| Descripción del cambio      | Cada cámara se repite como máximo 2 veces. En el último intento la foto se acepta, se marca «pendiente de revisión técnica» y la IA la analiza. Si la cámara no responde, la secuencia queda parcial y se sigue. |
| Razón del cambio            | Evitar que una cámara con problemas detenga la jornada, sin dejar de exigir la repetición del lado afectado.                                                                                                     |
| Efectos en el proyecto      | Corto plazo: jornadas sin bloqueos. Largo plazo: las fotos marcadas sirven para revisar el equipo.                                                                                                               |
| Efectos externos            | Ninguno fuera del proyecto.                                                                                                                                                                                      |

**SC-06 · Aclaraciones de interpretación (RF06, RNF03, RNF07)** — Actualización (aclaración)

|                             |                                                                                                                                                                                                                                                                                                   |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Requerimientos afectados    | RF06, RNF03, RNF07                                                                                                                                                                                                                                                                                |
| Problema o situación actual | Tres textos admiten más de una lectura: «zona» (RF06), «menos de 3 pasos» (RNF03) y la mención a expo-secure-store para guardar imágenes (RNF07), que es un almacén de claves pequeñas, no de archivos.                                                                                           |
| Descripción del cambio      | RF06: «zona» = segmento de ≈20 m de la hilera. RNF03: iniciar el recorrido son 2 toques; la vinculación y la prueba corta son controles exigidos por RF11. RNF07: fotos en el almacenamiento privado de la app y metadatos en su base SQLite privada; tokens y credenciales en expo-secure-store. |
| Razón del cambio            | Dejar por escrito cómo se cumple cada requerimiento sin modificar su texto.                                                                                                                                                                                                                       |
| Efectos en el proyecto      | Sin efecto en el alcance; mejora la verificación.                                                                                                                                                                                                                                                 |
| Efectos externos            | Ninguno.                                                                                                                                                                                                                                                                                          |

**SC-07 · Ajustes de la arquitectura tecnológica (Tabla 30)** — Actualización

|                             |                                                                                                                                                                                                                                                                                                          |
|-----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Requerimientos afectados    | Tabla 30, RNF01, SC-01                                                                                                                                                                                                                                                                                   |
| Problema o situación actual | La Tabla 30 y algunos diagramas mencionan Supabase Auth, PostGIS, un servicio de IA con FastAPI, comunicación P2P genérica y celulares iPhone.                                                                                                                                                           |
| Descripción del cambio      | Sistema actual: autenticación de Django con tokens JWT propios; coordenadas en columnas normales con validación en la base; IA en un worker de Django con ONNX; red local WebSocket/HTTP del controlador; celulares Android; servidor VPS con Docker y Traefik; fotos en Cloudinary y datos en Supabase. |
| Razón del cambio            | Menos piezas y un solo lenguaje en el servidor; Android es el equipo disponible para el piloto.                                                                                                                                                                                                          |
| Efectos en el proyecto      | Corto plazo: despliegue simple en un solo servidor. Largo plazo: iPhone queda pendiente de validar.                                                                                                                                                                                                      |
| Efectos externos            | Ninguno.                                                                                                                                                                                                                                                                                                 |

**SC-08 · Aprobar cuentas de la app sin elegir rol** — Reparación de defecto

|                             |                                                                                                                                                                                                                              |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Requerimientos afectados    | RNF05, HU05, HU17                                                                                                                                                                                                            |
| Problema o situación actual | Al aprobar una cuenta pedida desde la app, la web ofrecía los cuatro roles, incluidos los de la web. Un operador podía quedar con un rol que no le corresponde.                                                              |
| Descripción del cambio      | La aprobación ya no pide rol: la cuenta queda como «Operador de campo». El editor de roles solo aparece en cuentas de la web y solo con Especialista, Supervisor y Administrador. Plataforma v1.6.1 con pruebas automáticas. |
| Razón del cambio            | Los roles de la web son solo para cuentas creadas por el administrador con «Nueva cuenta». Mantener separadas las cuentas de campo y de oficina protege la auditoría.                                                        |
| Efectos en el proyecto      | Corto plazo: se elimina el error de configuración. Largo plazo: ninguno.                                                                                                                                                     |
| Efectos externos            | Ninguno.                                                                                                                                                                                                                     |

**SC-09 · Funciones agregadas al alcance** — Actualización

|                             |                                                                                                                                                                                                                                         |
|-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Requerimientos afectados    | RF06, RF10, CU8, CU9                                                                                                                                                                                                                    |
| Problema o situación actual | Durante el piloto aparecieron necesidades no escritas como requerimiento: llegar a pie hasta la plaga, quitar fotos de prueba, ubicar mejor las fotos y ayudar a los usuarios.                                                          |
| Descripción del cambio      | «Ubicar plaga» en la app (mapa satelital y «Cómo llegar»); papelera de fotos (30 días) y limpieza controlada; 10 segmentos fijos por hilera; ayuda en cada pantalla y manuales en PDF; cabeceras de seguridad y auditoría con contexto. |
| Razón del cambio            | Facilitan la operación sin cambiar ningún requerimiento.                                                                                                                                                                                |
| Efectos en el proyecto      | Corto plazo: mejor uso en campo. Largo plazo: más funciones que mantener.                                                                                                                                                               |
| Efectos externos            | Ninguno.                                                                                                                                                                                                                                |

CAPÍTULO 11 · auditoría

# Seguridad que se puede comprobar

**En pocas palabras:**

Medidas visibles para un evaluador y cómo comprobarlas con las herramientas del navegador (F12 → Red → elegir una página → Encabezados).

| Medida                                    | Cómo comprobarla                                                           |
|-------------------------------------------|----------------------------------------------------------------------------|
| HTTPS obligatorio y HSTS (un año)         | http:// redirige a https://; encabezado `Strict-Transport-Security`.       |
| Content-Security-Policy                   | Scripts solo del mismo sitio, sin `unsafe-eval`; prohíbe incrustar la web. |
| Permissions-Policy                        | Cámara, micrófono, ubicación y sensores desactivados para la web.          |
| Protección contra incrustación            | `X-Frame-Options: DENY` y `frame-ancestors 'none'`.                        |
| Cookies seguras                           | Sesión y CSRF con `HttpOnly`, `Secure` y `SameSite=Lax`.                   |
| Protección CSRF                           | Todo formulario lleva un token; una petición sin token es rechazada.       |
| Bloqueo por intentos                      | 5 contraseñas incorrectas → 15 minutos de espera (CP-W03).                 |
| Mensajes que no revelan cuentas           | El mismo mensaje con correo inexistente o contraseña errónea (CP-W02).     |
| Permisos en el servidor                   | Una dirección escrita a mano sin permiso devuelve «sin acceso» (CP-W05).   |
| Tipos de cuenta separados                 | Una cuenta de la app no recibe roles de la web (CP-W35, CP-W36).           |
| Sesión limitada                           | Expira al cerrar el navegador o tras 8 horas.                              |
| Páginas sin caché                         | Las páginas con datos llevan `Cache-Control: no-store`.                    |
| Fotos privadas                            | Solo se ven con enlaces firmados que vencen.                               |
| Auditoría inalterable                     | Cada cambio importante queda registrado y no se puede editar ni borrar.    |
| Celulares revocables                      | Un celular perdido se revoca y deja de sincronizar.                        |
| Sin secretos en el navegador ni en la app | Las claves de los servicios viven solo en el servidor.                     |

**Herramientas sugeridas.** Se puede usar OWASP ZAP en modo pasivo o los informes de cabeceras de seguridad. No ejecutes escaneos activos ni pruebas de carga sobre el sistema real sin coordinarlo con el responsable técnico.

CAPÍTULO 12 · CP-W

# Guía de pruebas · Plataforma web

**En pocas palabras:**

Casos de prueba de la web. Usa la columna «OK» para marcar el resultado. Salvo que el caso diga otra cosa, ingresa con **testeador@gmail.com** / **[omitido]**. Los pasos detallados de cada pantalla están en el Manual de usuario de la plataforma web.

## 1. Acceso y roles

| ID         | Qué se prueba            | Pasos                                                                                       | Resultado esperado                                                                        | OK  |
|------------|--------------------------|---------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|-----|
| **CP-W01** | Ingreso correcto         | Abrir la web e ingresar con testeador@gmail.com.                                            | Se abre el Panel con el menú completo (incluye Administración); INGRESO_WEB en Auditoría. |     |
| **CP-W02** | Credenciales incorrectas | Ingresar con contraseña errónea; luego con un correo inexistente.                           | Mismo mensaje genérico en ambos casos.                                                    |     |
| **CP-W03** | Bloqueo por intentos     | Con una cuenta creada en CP-W23 (no con la compartida), 5 contraseñas incorrectas seguidas. | «Demasiados intentos…»; tras 15 min vuelve a permitir.                                    |     |
| **CP-W04** | Contraseña temporal      | Ingresar con la contraseña temporal de la cuenta creada en CP-W23.                          | Obliga a cambiarla antes de mostrar cualquier página.                                     |     |
| **CP-W05** | Permisos por rol         | Con una cuenta solo Supervisor (CP-W23), escribir a mano /administracion/usuarios/.         | Página «sin acceso»; el menú no muestra Administración.                                   |     |
| **CP-W33** | Cierre de sesión         | Salir, o cerrar el navegador y volver a abrir la web.                                       | Pide ingresar de nuevo.                                                                   |     |

## 2. Casos e IA

| ID         | Qué se prueba               | Pasos                                                                   | Resultado esperado                                                           | OK  |
|------------|-----------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------------|-----|
| **CP-W06** | Panel                       | Cambiar Desde/Hasta y Aplicar; tocar una tarjeta.                       | Indicadores del periodo; la tarjeta abre la lista filtrada.                  |     |
| **CP-W07** | Bandeja y filtros           | Filtrar por estado, lote y fechas; ordenar; «Limpiar».                  | Lista coherente con los filtros; 25 por página.                              |     |
| **CP-W09** | Foto y cajas de la IA       | Abrir un caso; zoom; ocultar/mostrar cajas; «Original».                 | Cajas alineadas a cualquier zoom; carga la foto completa.                    |     |
| **CP-W10** | Ubicación del caso          | Revisar «Ubicación operativa» y la otra cámara.                         | Lote, hilera, lado, segmento, marcador, GPS y fuente; enlace a la otra foto. |     |
| **CP-W11** | Decidir (especialista)      | Elegir una decisión, escribir observación, «Guardar decisión».          | Cambia el estado; queda en Historial y CASO_DECIDIDO en Auditoría.           |     |
| **CP-W12** | Aviso tras confirmar        | Confirmar un caso que sea plaga, en un lote con destinatario de prueba. | Aviso «Enviado» en Avisos; el mensaje llega con foto y botones.              |     |
| **CP-W13** | Corregir una decisión       | «Corregir la decisión» con motivo.                                      | Nueva decisión vigente; la anterior en Historial; CASO_CORREGIDO.            |     |
| **CP-W14** | Solo el especialista decide | Abrir un caso con una cuenta sin rol Especialista (CP-W23).             | No hay botones de decisión; mensaje «solo el especialista…».                 |     |
| **CP-W15** | Rescatar una descartada     | Casos → Descartadas por la IA → Ver foto → «Abrir caso para revisión».  | La foto pasa a la bandeja como caso manual.                                  |     |
| **CP-W22** | Estado de la IA             | Abrir Estado de la IA.                                                  | Modelo activo, conteos por estado y lista de errores.                        |     |

## 3. Mapa, sesiones y reportes

| ID         | Qué se prueba        | Pasos                                        | Resultado esperado                                                | OK  |
|------------|----------------------|----------------------------------------------|-------------------------------------------------------------------|-----|
| **CP-W16** | Mapa                 | Abrir Mapa; filtrar; tocar un punto.         | Puntos con color por estado; abre el caso.                        |     |
| **CP-W17** | Plano de hileras     | Elegir un lote y Aplicar.                    | Hileras proporcionales; segmentos con casos enlazados.            |     |
| **CP-W18** | Sesión               | Abrir una sesión sincronizada.               | Operador, cámaras, pasadas A/B, incidencias y fotos.              |     |
| **CP-W19** | Metadatos de la foto | Abrir una foto desde la sesión.              | Lote, hilera, lado, segmento, marcador, secuencia, GPS y calidad. |     |
| **CP-W20** | Exportar CSV         | Reportes → Descargar CSV.                    | Se descarga el archivo; EXPORTACION_CASOS en Auditoría.           |     |
| **CP-W21** | Avisos               | Abrir Avisos; filtrar por estado.            | Cada envío con estado, intentos, fecha y error.                   |     |
| **CP-W34** | Uso en celular       | Abrir la bandeja y un caso desde un celular. | Se lee sin desplazamiento horizontal; tablas como tarjetas.       |     |

## 4. Administración

| ID         | Qué se prueba                   | Pasos                                                                                          | Resultado esperado                                                                                                | OK  |
|------------|---------------------------------|------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|-----|
| **CP-W23** | Crear cuenta                    | Usuarios → Nueva cuenta → Plataforma web, solo «Supervisor» (correo de prueba propio).         | Contraseña temporal visible una sola vez; CUENTA_CREADA. Esta cuenta sirve para CP-W03 a CP-W05, CP-W14 y CP-A03. |     |
| **CP-W24** | Bloquear y desbloquear          | Bloquear la cuenta de CP-W23 e intentar ingresar; desbloquear.                                 | No puede entrar mientras está bloqueada.                                                                          |     |
| **CP-W25** | Destinatario de Telegram        | Abrir el bot, «Buscar chats», «Usar este chat», guardar, «Enviar prueba».                      | Llega el mensaje de prueba; DESTINATARIO_PRUEBA.                                                                  |     |
| **CP-W26** | Validaciones de destinatario    | Escribir un número de celular en el chat; guardar sin consentimiento.                          | Mensajes que explican qué corregir.                                                                               |     |
| **CP-W27** | Catálogos                       | Crear el lote «QA», agregar hileras y desactivarlo.                                            | Se crean segmentos y marcadores; desactivar no borra.                                                             |     |
| **CP-W28** | Papelera                        | «Eliminar esta foto» en un caso; buscarla en la bandeja; «Restaurar».                          | Desaparece de las listas y vuelve al restaurar.                                                                   |     |
| **CP-W29** | Limpieza (sin ejecutar)         | Limpieza → «Calcular».                                                                         | Muestra cuántas fotos se borrarían; no se confirma.                                                               |     |
| **CP-W30** | Auditoría                       | Filtrar por categoría; tocar el ID de un registro.                                             | Aparecen tus pruebas con usuario y antes → después.                                                               |     |
| **CP-W31** | Manuales protegidos             | Copiar el enlace de un PDF de Ayuda y abrirlo en una ventana privada.                          | Pide iniciar sesión.                                                                                              |     |
| **CP-W32** | Cabeceras de seguridad          | F12 → Red → cualquier página → Encabezados.                                                    | CSP, HSTS, X-Frame-Options, Referrer-Policy y Permissions-Policy presentes.                                       |     |
| **CP-W35** | Aprobar una solicitud de la app | Pedir una cuenta desde la app (CP-A21); en Usuarios → Solicitudes pendientes pulsar «Aprobar». | No se elige rol; la cuenta queda Activa como «Operador de campo».                                                 |     |
| **CP-W36** | Roles solo en cuentas de la web | En Todas las cuentas, revisar «Roles» en una cuenta de operador y en una de la web.            | La cuenta de operador no tiene «Roles»; la de la web solo ofrece Especialista, Supervisor y Administrador.        |     |

CAPÍTULO 13 · CP-A

# Guía de pruebas · App móvil

**En pocas palabras:**

Casos de prueba de la app. Necesitan 3 celulares Android con la misma versión, una red Wi-Fi local y, para algunas, una hilera real. Salvo que el caso diga otra cosa, ingresa con **testeador2@gmail.com** / **[omitido]** en cada celular.

## 1. Casos de prueba

| ID         | Qué se prueba             | Pasos                                                              | Resultado esperado                                                                 | OK  |
|------------|---------------------------|--------------------------------------------------------------------|------------------------------------------------------------------------------------|-----|
| **CP-A01** | Ingreso con internet      | Ingresar con testeador2@gmail.com.                                 | Pantalla «Función del celular».                                                    |     |
| **CP-A02** | Ingreso sin internet      | Tras un ingreso con internet, activar modo avión e ingresar.       | Entra (hasta 7 días); 5 fallos bloquean 15 min.                                    |     |
| **CP-A03** | Rol no permitido          | Ingresar con la cuenta solo Supervisor de CP-W23.                  | La app indica que ese rol no usa la app.                                           |     |
| **CP-A04** | Catálogos                 | Controlador → Catálogos → Actualizar.                              | Lotes, hileras, segmentos y marcadores descargados.                                |     |
| **CP-A05** | Sesión y vinculación      | Nueva sesión → Crear sesión; cada cámara escanea el QR.            | Las dos cámaras pasan a CONECTADA.                                                 |     |
| **CP-A06** | Exigir las 3 unidades     | Intentar continuar con una sola cámara conectada.                  | No permite iniciar el recorrido.                                                   |     |
| **CP-A07** | Prueba corta              | Ejecutar prueba.                                                   | Aprobada en menos de 20 s con las dos cámaras.                                     |     |
| **CP-A08** | Nueva pasada              | Elegir lote, hilera y lateral A.                                   | Propone segmento, marcador y sentido del recorrido.                                |     |
| **CP-A09** | Captura bilateral         | CAPTURAR varias veces.                                             | Secuencias «Completa» con foto de Cámara 1 y 2 y GPS.                              |     |
| **CP-A10** | Repetición por calidad    | Tapar el lente de una cámara y CAPTURAR.                           | Solo esa cámara se rechaza; botón dorado «REPETIR».                                |     |
| **CP-A11** | Límite de repeticiones    | Mantener el lente tapado y repetir dos veces.                      | En el intento 2 se acepta la foto y se sigue.                                      |     |
| **CP-A12** | Tiempo de calidad         | Cámara → Modo prueba → Tomar foto de prueba.                       | Resultado de calidad en ≤ 2 s, con métricas.                                       |     |
| **CP-A13** | Sin conexión              | Con modo avión en el controlador, hacer una pasada.                | Las fotos se guardan y cuentan como pendientes.                                    |     |
| **CP-A14** | Sincronización            | Recuperar internet → Sincronizar ahora; revisar la web.            | Sesión, pasadas y fotos visibles en la web.                                        |     |
| **CP-A15** | Cambiar marcador          | CAMBIAR MARCADOR → Confirmar.                                      | El segmento avanza (por ejemplo «2 de 10»).                                        |     |
| **CP-A16** | Lateral B                 | Terminar la pasada A → Crear LATERAL B.                            | Recorrido de vuelta propuesto.                                                     |     |
| **CP-A17** | Incidencia                | Incidencias → Agregar incidencia.                                  | Se guarda y llega a la web con la sesión.                                          |     |
| **CP-A18** | Ubicar plaga              | Abrir «Ubicar plaga» y una alerta → «Cómo llegar».                 | Distancia y dirección; abre Google Maps a pie.                                     |     |
| **CP-A19** | Especialista en la app    | Ingresar con una cuenta solo Especialista (créala como en CP-W23). | Entra directo a «Ubicar plaga»; no monitorea.                                      |     |
| **CP-A20** | Versión única             | Ajustes → App en los 3 celulares.                                  | Los tres muestran la misma versión.                                                |     |
| **CP-A21** | Crear cuenta desde la app | Ingreso → «Crear cuenta» con un correo de prueba propio.           | El formulario no pide rol; la cuenta queda pendiente y aparece en la web (CP-W35). |     |

**La cuenta de la web también entra a la app.** testeador@gmail.com tiene el rol Administrador, que en la app equivale a un operador: puede monitorear, sincronizar y usar «Ubicar plaga».

CAPÍTULO 14 · RF → HU → CP

# Matriz de trazabilidad

**En pocas palabras:**

Cada requerimiento funcional con su caso de uso, sus historias de usuario, las pruebas que lo verifican y la solicitud de cambio que lo afecta.

| RF    | Caso de uso | Historias        | Pruebas                                 | Solicitud de cambio |
|-------|-------------|------------------|-----------------------------------------|---------------------|
| RF01  | CU1         | HU01             | CP-A09, CP-A16, CP-W18                  | SC-04               |
| RF02  | CU2         | HU02, HU08       | CP-A10, CP-A12                          | —                   |
| RF03  | CU2         | HU02, HU08       | CP-A10, CP-A11                          | SC-05               |
| RF04  | CU3         | HU02, HU06       | CP-A13                                  | SC-06               |
| RF05  | CU3         | HU06             | CP-A14                                  | —                   |
| RF06  | CU1         | HU09             | CP-A04, CP-A09, CP-W10, CP-W19          | SC-06               |
| RF07  | CU4         | HU03, HU11       | CP-W09, CP-W15, CP-W22                  | SC-03               |
| RF08  | CU5         | HU03, HU10       | CP-W07, CP-W09, CP-W16                  | —                   |
| RF09  | CU6, CU7    | HU03, HU10       | CP-W11, CP-W13, CP-W14                  | SC-03               |
| RF10  | CU8         | HU04, HU12, HU16 | CP-W12, CP-W21, CP-W25, CP-A18          | SC-02, SC-03        |
| RF11  | CU1         | HU01, HU07       | CP-A05, CP-A06, CP-A07                  | —                   |
| RNF05 | —           | HU05, HU14, HU17 | CP-W01 a CP-W05, CP-W35, CP-W36, CP-A03 | SC-08               |
| CU9   | CU9         | HU13             | CP-W06, CP-W16, CP-W17, CP-W20          | SC-09               |

CAPÍTULO 15 · referencia

# Glosario

**En pocas palabras:**

Términos usados en el sistema y en estos documentos.

| Término                  | Significado                                                                                |
|--------------------------|--------------------------------------------------------------------------------------------|
| Caso                     | Foto que la IA (o el especialista) marcó para revisar; tiene estado y decisión.            |
| Controlador              | Celular que crea la sesión, ordena las capturas, guarda todo y sincroniza.                 |
| Destinatario             | Persona o grupo de Telegram que recibe los avisos de un lote.                              |
| Historia de usuario (HU) | Necesidad de un usuario escrita como «Como…, quiero…, para…», con criterios de aceptación. |
| Indicio                  | Lo que la IA sugiere ver en la foto (caja con clase y confianza). No es un diagnóstico.    |
| Lateral A / B            | Cada lado de la hilera; A se recorre de ida y B de vuelta.                                 |
| Marcador                 | Poste al inicio de cada segmento (y uno al final de la hilera).                            |
| Papelera                 | Lugar donde queda una foto eliminada 30 días antes de borrarse; se puede restaurar.        |
| Pasada                   | Recorrido de un lado de una hilera.                                                        |
| Prueba corta             | Comprobación de las dos cámaras antes de empezar.                                          |
| Segmento                 | Tramo de la hilera (10 por hilera, ≈20 m). Equivale a la «zona» del informe.               |
| Secuencia                | Una orden de captura: una foto por cámara en el mismo punto, con el GPS.                   |
| Sesión                   | Una salida al campo con los 3 celulares vinculados.                                        |
| Sincronizar              | Enviar sesiones y fotos al servidor cuando hay internet.                                   |
| Solicitud de cambio (SC) | Registro formal que justifica una diferencia con lo aprobado, para revisión del comité.    |
| Ticket firmado           | Permiso de un solo uso que da el servidor para subir una foto directo a la nube.           |
| Triage                   | Clasificación automática de la IA: descartar, revisar o confirmar.                         |
| Worker                   | Programa del servidor que analiza las fotos y envía los avisos.                            |
