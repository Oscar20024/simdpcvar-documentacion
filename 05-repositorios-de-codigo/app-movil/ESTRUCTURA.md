# Estructura del repositorio de la app móvil (Expo / React Native)

Repositorio **`riachuelo-mobile`** (GitHub `OskitarBB/agricola-riachuelo-mobile`). App **Expo SDK 57 / React Native / TypeScript estricto** llamada «Riachuelo Monitoreo». Una sola app cumple tres funciones según el celular: **Controlador**, **Cámara 1** o **Cámara 2** (se elige después de cada login, PANT-08), más «Ubicar plaga» para operador, administrador y especialista. Versión documentada: **app 0.6.1**.

Cómo está organizado: `app/` solo tiene pantallas (rutas de Expo Router) y `src/` toda la lógica por capas: `domain/` (reglas puras) ← `storage/` (SQLite) ← servicios (`controller/`, `camera/`, `sync/`, `auth/`, `pests/`) ← `device/` y `local-network/` (hardware y red) ← `api/` (única salida a la plataforma) ← `ui/` (componentes y textos). Parámetros solo en `src/config/defaults.ts`; textos solo en `src/ui/strings.ts` y `messages.ts`.

Flujo dentro del código en una jornada: `app/controller/new-session.tsx` → `controller/sessionService.ts` → `pairingService.ts` muestra el QR → las cámaras (`app/camera/index.tsx` → `camera/cameraAgent.ts`) se conectan por `local-network/` → `shortTest.ts` → `app/controller/pass.tsx` + `sequenceEngine.ts` ordenan capturas → cada cámara captura (`captureService.ts`), evalúa calidad (`device/qualityPipeline.ts`) y envía (`transferQueue.ts`) → el controlador consolida (`consolidationService.ts`) en SQLite → con internet `sync/syncService.ts` envía a `/api/v1` y sube fotos a Cloudinary con ticket.

## Árbol completo de archivos versionados

Generado con `git ls-files` (sin `node_modules/` ni librerías de terceros copiadas en `vendor/`).

```text
├── .claude/
│   └── settings.json
├── __tests__/
│   ├── auth.test.ts
│   ├── captureUploader.test.ts
│   ├── config.test.ts
│   ├── coverage.test.ts
│   ├── deletions.test.ts
│   ├── domain.test.ts
│   ├── httpUpload.test.ts
│   ├── pests.test.ts
│   ├── pictureSize.test.ts
│   ├── protocol.test.ts
│   ├── quality.test.ts
│   ├── retry.test.ts
│   ├── roleChange.test.ts
│   ├── segments.test.ts
│   ├── shortTest.test.ts
│   ├── syncPayloads.test.ts
│   ├── syncPlanner.test.ts
│   └── wsCodec.test.ts
├── app/
│   ├── (auth)/
│   │   ├── _layout.tsx
│   │   ├── forgot-password.tsx
│   │   ├── login.tsx
│   │   ├── pending.tsx
│   │   └── register.tsx
│   ├── (setup)/
│   │   ├── _layout.tsx
│   │   ├── permissions.tsx
│   │   └── role.tsx
│   ├── camera/
│   │   ├── _layout.tsx
│   │   ├── index.tsx
│   │   ├── live.tsx
│   │   └── test.tsx
│   ├── controller/
│   │   ├── _layout.tsx
│   │   ├── catalogs.tsx
│   │   ├── close-pass.tsx
│   │   ├── incidents.tsx
│   │   ├── index.tsx
│   │   ├── marker.tsx
│   │   ├── new-pass.tsx
│   │   ├── new-session.tsx
│   │   ├── pairing.tsx
│   │   ├── pass.tsx
│   │   ├── session-summary.tsx
│   │   ├── short-test.tsx
│   │   └── sync.tsx
│   ├── pests/
│   │   ├── [id].tsx
│   │   ├── _layout.tsx
│   │   └── index.tsx
│   ├── _layout.tsx
│   ├── change-password.tsx
│   ├── gallery.tsx
│   ├── index.tsx
│   └── settings.tsx
├── assets/
│   ├── images/
│   │   └── logo-riachuelo.png
│   ├── sample/
│   │   ├── muestra_borrosa.jpg
│   │   ├── muestra_oscura.jpg
│   │   └── muestra_util.jpg
│   ├── sounds/
│   │   ├── confirm.wav
│   │   ├── error.wav
│   │   ├── shutter.wav
│   │   ├── success.wav
│   │   ├── tap.wav
│   │   └── toggle.wav
│   ├── android-icon-background.png
│   ├── android-icon-foreground.png
│   ├── android-icon-monochrome.png
│   ├── favicon.png
│   ├── icon.png
│   └── splash-icon.png
├── Claude outputs/
│   ├── arquitectura_produccion.png
│   └── HANDOFF_CATALOGOS_WEB.md
├── docs/
│   ├── adr/
│   │   ├── 0001-sdk57-expo-router.md
│   │   ├── 0002-cfg2-modos-y-estabilidad.md
│   │   ├── 0003-red-local.md
│   │   ├── 0004-ciclos-y-cobertura.md
│   │   ├── 0005-funcion-despues-del-login.md
│   │   ├── 0006-fase4-sincronizacion.md
│   │   ├── 0007-catalogos-codigos-no-unicos.md
│   │   ├── 0008-cerrar-sesion-con-monitoreo-abierto.md
│   │   ├── 0009-ubicar-plaga.md
│   │   ├── 0010-limpieza-ordenada-por-el-administrador.md
│   │   ├── 0011-segmentos-fijos-por-hilera.md
│   │   └── 0012-limite-de-repeticiones.md
│   ├── evidencias/
│   │   ├── fase4_integracion.md
│   │   └── README.md
│   ├── img/
│   │   ├── arquitectura_produccion.html
│   │   └── arquitectura_produccion.png
│   ├── referencia/
│   │   ├── CONTEXTO_ERRORES_Y_CONFIGURACION.txt
│   │   └── MAESTRO_APP_MOVIL_v1.0.pdf
│   ├── CAMBIOS.md
│   └── Resumen_Tecnico_App_Riachuelo.pdf
├── src/
│   ├── api/
│   │   ├── mock/
│   │   │   ├── mockBackend.ts
│   │   │   ├── mockCatalog.ts
│   │   │   ├── mockCloudinary.ts
│   │   │   ├── mockPests.ts
│   │   │   └── pilotRows.ts
│   │   ├── authApi.ts
│   │   ├── bootstrapApi.ts
│   │   ├── cloudinaryResponse.ts
│   │   ├── cloudinaryUpload.ts
│   │   ├── deletionApi.ts
│   │   ├── dto.ts
│   │   ├── httpClient.ts
│   │   ├── index.ts
│   │   ├── pestApi.ts
│   │   ├── syncApi.ts
│   │   └── uploadApi.ts
│   ├── auth/
│   │   ├── authService.ts
│   │   ├── authStore.ts
│   │   ├── offlineAuth.ts
│   │   ├── tokenStore.ts
│   │   └── validation.ts
│   ├── camera/
│   │   ├── cameraAgent.ts
│   │   ├── cameraStore.ts
│   │   ├── captureService.ts
│   │   ├── retentionService.ts
│   │   ├── testModeService.ts
│   │   └── transferQueue.ts
│   ├── config/
│   │   ├── defaults.ts
│   │   ├── env.ts
│   │   └── index.ts
│   ├── controller/
│   │   ├── cameraMonitor.ts
│   │   ├── catalogService.ts
│   │   ├── consolidationService.ts
│   │   ├── controllerRuntime.ts
│   │   ├── controllerStore.ts
│   │   ├── coverageService.ts
│   │   ├── pairingService.ts
│   │   ├── passService.ts
│   │   ├── sequenceEngine.ts
│   │   ├── sessionService.ts
│   │   └── shortTest.ts
│   ├── device/
│   │   ├── batteryService.ts
│   │   ├── cameraService.ts
│   │   ├── deviceIdentity.ts
│   │   ├── deviceRole.ts
│   │   ├── galleryService.ts
│   │   ├── gpsService.ts
│   │   ├── keepAwake.ts
│   │   ├── networkMonitor.ts
│   │   ├── permissions.ts
│   │   ├── preferences.ts
│   │   ├── quality.ts
│   │   ├── qualityPipeline.ts
│   │   ├── stabilityDetector.ts
│   │   └── storageInfo.ts
│   ├── diagnostics/
│   │   ├── eventLog.ts
│   │   ├── exportDiagnostics.ts
│   │   └── incidents.ts
│   ├── domain/
│   │   ├── coverage.ts
│   │   ├── deletions.ts
│   │   ├── geo.ts
│   │   ├── ids.ts
│   │   ├── pests.ts
│   │   ├── pictureSize.ts
│   │   ├── rules.ts
│   │   ├── segments.ts
│   │   ├── sequence.ts
│   │   ├── stateMachines.ts
│   │   ├── syncQueue.ts
│   │   ├── time.ts
│   │   └── types.ts
│   ├── local-network/
│   │   ├── client/
│   │   │   ├── uploadFileSender.ts
│   │   │   └── wsControlClient.ts
│   │   ├── simulator/
│   │   │   ├── simulatedNetwork.ts
│   │   │   └── virtualCamera.ts
│   │   ├── tcp/
│   │   │   ├── tcpControlServer.ts
│   │   │   └── tcpFileReceiver.ts
│   │   ├── factory.ts
│   │   ├── httpUpload.ts
│   │   ├── transport.ts
│   │   └── wsCodec.ts
│   ├── pests/
│   │   ├── mapHtml.ts
│   │   ├── openMaps.ts
│   │   └── pestService.ts
│   ├── protocol/
│   │   ├── envelope.ts
│   │   ├── messages.ts
│   │   └── schemas.ts
│   ├── storage/
│   │   ├── migrations/
│   │   │   ├── 001_initial.ts
│   │   │   ├── 002_cobertura.ts
│   │   │   ├── 003_remote_uploads.ts
│   │   │   ├── 004_catalog_codes.ts
│   │   │   └── 005_row_length.ts
│   │   ├── repositories/
│   │   │   ├── appMetaRepo.ts
│   │   │   ├── cameraContextRepo.ts
│   │   │   ├── captureRepo.ts
│   │   │   ├── catalogRepo.ts
│   │   │   ├── coverageRepo.ts
│   │   │   ├── deletionRepo.ts
│   │   │   ├── eventLogRepo.ts
│   │   │   ├── incidentRepo.ts
│   │   │   ├── passRepo.ts
│   │   │   ├── processedMessagesRepo.ts
│   │   │   ├── remoteUploadRepo.ts
│   │   │   ├── sequenceRepo.ts
│   │   │   ├── sessionRepo.ts
│   │   │   ├── syncQueueRepo.ts
│   │   │   ├── transferQueueRepo.ts
│   │   │   └── usersRepo.ts
│   │   ├── db.ts
│   │   ├── files.ts
│   │   └── secureStore.ts
│   ├── sync/
│   │   ├── captureUploader.ts
│   │   ├── deletionService.ts
│   │   ├── retry.ts
│   │   ├── syncPayloads.ts
│   │   ├── syncPlanner.ts
│   │   ├── syncQueue.ts
│   │   ├── syncService.ts
│   │   └── syncStore.ts
│   ├── ui/
│   │   ├── components/
│   │   │   ├── AnimatedCounter.tsx
│   │   │   ├── AppButton.tsx
│   │   │   ├── AppHeader.tsx
│   │   │   ├── CameraCardView.tsx
│   │   │   ├── Card.tsx
│   │   │   ├── CheckList.tsx
│   │   │   ├── ChipGroup.tsx
│   │   │   ├── ConfirmDialog.tsx
│   │   │   ├── FadeIn.tsx
│   │   │   ├── InfoRow.tsx
│   │   │   ├── Logo.tsx
│   │   │   ├── OfflineBanner.tsx
│   │   │   ├── PestItem.tsx
│   │   │   ├── PestMap.tsx
│   │   │   ├── PickerSheet.tsx
│   │   │   ├── QrCodeView.tsx
│   │   │   ├── ScanFrame.tsx
│   │   │   ├── Screen.tsx
│   │   │   ├── SectionTitle.tsx
│   │   │   ├── Segmented.tsx
│   │   │   ├── SimulatorTools.tsx
│   │   │   ├── StabilityRing.tsx
│   │   │   ├── StatusPill.tsx
│   │   │   ├── TextField.tsx
│   │   │   └── ToastHost.tsx
│   │   ├── hooks/
│   │   │   ├── useCapturePictureSize.ts
│   │   │   └── useLogout.ts
│   │   ├── feedback.ts
│   │   ├── messages.ts
│   │   ├── strings.ts
│   │   ├── theme.ts
│   │   └── toast.ts
│   └── boot.ts
├── tools/
│   ├── servidor/
│   │   └── crear_segmentos_hilera_completa.py
│   └── verificacion/
│       ├── mocks/
│       │   ├── expo-application.ts
│       │   ├── expo-constants.ts
│       │   ├── expo-crypto.ts
│       │   ├── expo-device.ts
│       │   ├── expo-file-system.ts
│       │   ├── expo-keep-awake.ts
│       │   ├── expo-location.ts
│       │   ├── expo-network.ts
│       │   ├── expo-secure-store.ts
│       │   ├── expo-sqlite.ts
│       │   └── react-native.ts
│       ├── actualizacion.ts
│       ├── integracion.ts
│       ├── piloto.ts
│       ├── README.md
│       ├── redlocal.ts
│       ├── simulado.ts
│       └── tsconfig.json
├── .env.example
├── .gitignore
├── .prettierrc.json
├── AGENTS.md
├── app.json
├── CLAUDE.md
├── eas.json
├── eslint.config.js
├── package-lock.json
├── package.json
├── README.md
└── tsconfig.json
```

## Carpeta por carpeta

### `(raíz)`

Configuración del proyecto Expo: `app.json` (nombre «Riachuelo Monitoreo», paquete Android `pe.riachuelo.monitoreo`, permisos, plugins), `eas.json` (perfiles de compilación: `piloto` apunta al servidor real, `preview` usa el backend simulado, `development`), `package.json` (scripts `validate`, `build:apk:piloto`…), `tsconfig.json`, `eslint.config.js`, `.env.example` y las instrucciones para agentes (`AGENTS.md`/`CLAUDE.md`).

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `.env.example` |  |  |
| `.gitignore` |  |  |
| `.prettierrc.json` |  |  |
| `AGENTS.md` | 74 | Riachuelo Monitoreo — guía para agentes y desarrolladores |
| `CLAUDE.md` | 3 | CLAUDE.md |
| `README.md` | 234 | Riachuelo Monitoreo — App móvil (Fases 0 a 4 · v0.5.0) |
| `app.json` |  |  |
| `eas.json` |  |  |
| `eslint.config.js` | 13 | Configuración de ESLint (flat config) basada en eslint-config-expo. Se ejecuta con `npm run lint`. Las reglas adicionales refuerzan las reglas R-02/R-05 del maestro. |
| `package-lock.json` |  |  |
| `package.json` |  |  |
| `tsconfig.json` |  |  |

### `app/`

Pantallas (Expo Router: cada archivo es una ruta; `_layout.tsx` define navegadores). Códigos PANT-xx del Maestro App Móvil. `(auth)/` ingreso, registro, cuenta pendiente y recuperar contraseña; `(setup)/` elegir función del celular (PANT-08) y permisos; `controller/` panel del controlador, catálogos, nueva sesión, vinculación por QR, prueba corta, nueva pasada, pasada activa, marcador, cerrar pasada, resumen de sesión, sincronización e incidencias; `camera/` escanear QR, cámara en sesión y modo prueba; `pests/` «Ubicar plaga» (lista, mapa y detalle); además galería «Fotos guardadas», ajustes y cambio de contraseña.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `app/(auth)/_layout.tsx` | 12 | Grupo de pantallas SIN sesión de usuario: login, registro, solicitud enviada y recuperación de contraseña (PANT-02 a PANT-05). La guarda está en app/_layout.tsx (Stack.Protected). |
| `app/(auth)/forgot-password.tsx` | 87 | PANT-05 Recuperar contraseña vía administrador (maestro §7.7, RF-07). QUÉ HACE: pide el correo y envía la solicitud. SIEMPRE muestra el mismo mensaje neutro (no revela si el correo existe). El administrador asigna una contraseñ |
| `app/(auth)/login.tsx` | 182 | PANT-02 Iniciar sesión (maestro §7.4 y §7.5; RF-02 a RF-05, RF-10). QUÉ HACE: logo animado de Agrícola Riachuelo, correo y contraseña (con mostrar/ocultar), indicador "Con internet / Sin internet" y versión de la app. Un solo botón "Ingr |
| `app/(auth)/pending.tsx` | 48 | PANT-04 Solicitud enviada (RF-01). QUÉ HACE: confirma que la cuenta quedó pendiente de aprobación y permite volver al login. |
| `app/(auth)/register.tsx` | 150 | PANT-03 Crear cuenta (maestro §7.3, RF-01). QUÉ HACE: pide nombre completo, correo, celular (opcional), código de trabajador (opcional), contraseña, confirmación y aceptación del aviso de privacidad. Valida por campo (validation.ts) y |
| `app/(setup)/_layout.tsx` | 11 | Configuración del celular después del login: función (PANT-08) y permisos (PANT-07). |
| `app/(setup)/permissions.tsx` | 131 | PANT-07 Permisos de la función (maestro §9; RF-14). QUÉ HACE: lista los permisos que necesita la función elegida con su estado (Concedido / Denegado / Sin decidir): - Cámara (Cámara 1/2): fotos y lectura del QR. - Ubicación (Contr |
| `app/(setup)/role.tsx` | 255 | PANT-08 Función del dispositivo (maestro §7.9; RF-12, RF-13; RN-15; ADR 0005). QUÉ HACE: después de CADA inicio de sesión (y desde Ajustes › Cambiar función) el operador elige qué será este celular: CONTROLADOR, CÁMARA 1 o CÁMARA 2. Tres |
| `app/_layout.tsx` | 82 | Raíz de la navegación (Expo Router) con rutas protegidas (maestro §8.11, D-17). QUÉ HACE: - Arranca la app (src/boot.ts) y muestra PANT-01 (app/index.tsx) mientras tanto. - Define el Stack y las GUARDAS con Stack.Protected: (auth)/* solo SIN  |
| `app/camera/_layout.tsx` | 31 | Pantallas de las funciones CÁMARA 1 / CÁMARA 2 (PANT-30, PANT-31, PANT-32). QUÉ HACE: al entrar inicia el agente de cámara (recupera un contexto abierto y reconecta con el controlador, 8.10) y el detector de estabilidad. La guarda (fun |
| `app/camera/index.tsx` | 202 | PANT-30 Escanear QR (maestro §14.6 y §8.12; RF-28). QUÉ HACE: vista de cámara a pantalla completa con lector de QR y marco animado. Al leer un QR válido (type = RIACHUELO_PAIR) se vincula con el controlador usando la función de este celu |
| `app/camera/live.tsx` | 220 | PANT-31 Cámara en sesión (maestro §9; RF-29 a RF-31, RF-35, RF-36). QUÉ HACE: la vista de cámara queda SIEMPRE montada a pantalla completa (el controlador ordena cada foto). Una franja de estado GRANDE, con colores y texto (legible a 1 m) |
| `app/camera/test.tsx` | 217 | PANT-32 Modo prueba de cámara (RF-32, RF-33) con disparo AUTOMÁTICO por estabilidad. QUÉ HACE: - MANUAL: botón grande "Tomar foto de prueba" (sonido de obturador y destello). - AUTOMÁTICO: dispara cada capture.intervalMs PERO SOLO si el c |
| `app/change-password.tsx` | 125 | PANT-06 Cambiar contraseña (maestro §7.6; RF-06, RF-08; RN-04). QUÉ HACE: pide contraseña actual, nueva y confirmación (política: 8+ caracteres con letras y números). Si viene de una contraseña TEMPORAL es obligatorio: solo se puede c |
| `app/controller/_layout.tsx` | 22 | Pantallas del CONTROLADOR (PANT-10 a PANT-21). QUÉ HACE: al entrar inicia el runtime del controlador (recupera una sesión abierta tras un cierre o reinicio, 8.10) y define la navegación. "Cambiar marcador" (PANT-17) se abre como ho |
| `app/controller/catalogs.tsx` | 109 | PANT-11 Catálogos (RF-15). QUÉ HACE: muestra versión, fecha y conteos de lotes, hileras, segmentos y marcadores (con contadores animados). "Actualizar" requiere internet y reemplaza los catálogos de forma atómica. Plataforma Djang |
| `app/controller/close-pass.tsx` | 193 | PANT-18 Terminar pasada (maestro §8.7; RF-26, RF-47; RN-12, RN-13). QUÉ HACE: resumen de la pasada (duración, completas/parciales/incompletas, repeticiones, fotos por llegar, incidencias y resumen por segmento) y el estado final |
| `app/controller/incidents.tsx` | 137 | PANT-21 Incidencias (RF-43). QUÉ HACE: lista las incidencias de la sesión actual (o las últimas) con tipo, severidad y hora; las automáticas las genera el sistema (desconexión, calidad, batería, espacio, transferencia…). "Agregar |
| `app/controller/index.tsx` | 198 | PANT-10 Panel del controlador (RF-15, RF-42, RF-46). QUÉ HACE: estado de preparación (catálogos y su fecha, batería, espacio, GPS), la sesión abierta si existe y las sesiones por sincronizar. Acciones: Nueva sesión, Continuar sesión  |
| `app/controller/marker.tsx` | 186 | PANT-17 Cambiar marcador (hoja modal; maestro §8.6; RF-24; contexto §25.2). QUÉ HACE: lista los marcadores de la hilera agrupados por segmento; el actual aparece marcado. v0.6.0 (ADR 0011): arriba propone el SIGUIENTE SEGMENTO en el |
| `app/controller/new-pass.tsx` | 268 | PANT-15 Nueva pasada (maestro §8.4 y §8.7; RF-16, RF-20; RN-10, RN-12). QUÉ HACE: selectores encadenados SIN internet (lote → hilera → segmento → marcador de inicio), lateral (A primero). La dirección no se elige en pantalla. v0.6 |
| `app/controller/new-session.tsx` | 84 | PANT-12 Nueva sesión (RF-17; contexto §25.1 y §25.7). QUÉ HACE (pantalla simplificada): Modo de captura [ MANUAL ] [ AUTOMÁTICO ] ← sin "MIXTO" Intervalo [1 s] [2 s] [3 s] [5 s] [ CREAR SESIÓN ] Reglas: con MANUAL el intervalo  |
| `app/controller/pairing.tsx` | 154 | PANT-13 Vincular cámaras (maestro §14.2 y §14.6; RF-18). QUÉ HACE: muestra el QR GRANDE (IP, puertos, sesión y token), la IP con opción de escribirla a mano (Q-04, si no se detecta) y una tarjeta por cámara (estado, usuario, modelo |
| `app/controller/pass.tsx` | 285 | PANT-16 Pasada activa / monitor (maestro §8.4–8.6; RF-21 a RF-25, RF-38, RF-42). QUÉ HACE (contexto §25.8, "menos texto, más estado y acción"): - Cabecera de estado: lote · hilera · lateral, marcador y segmento actuales, GPS y precisi |
| `app/controller/session-summary.tsx` | 156 | PANT-19 Cerrar sesión de monitoreo (maestro §8.8; RF-27). QUÉ HACE: resumen por pasada, fotos que faltan llegar por cámara e incidencias. "Cerrar sesión" pasa a CLOSING (sin órdenes nuevas; una pasada abierta se cierra INCO |
| `app/controller/short-test.tsx` | 173 | PANT-14 Prueba corta (maestro §14.12; RF-19). QUÉ HACE: envía una orden de prueba a ambas cámaras y muestra por cámara: respuesta (CAPTURE_OK o QUALITY_ERROR), calidad con métricas, transferencia (foto recibida y verificada), ti |
| `app/controller/sync.tsx` | 266 | PANT-20 Sincronización (RF-39 a RF-41, RF-49 a RF-51; maestro v2.0 §8.9 y §15.5). QUÉ HACE: - Arriba: servidor, con/sin internet, avance de la ronda en curso (elemento, paso de la foto: permiso de subida, subida a la nube o confirmaci |
| `app/gallery.tsx` | 244 | Fotos guardadas en este celular (pedido del equipo). QUÉ HACE: cuadrícula animada con las fotos del almacenamiento privado (evidencia de sesión, prueba corta y modo prueba), con filtros. Al tocar una foto se abre el visor a pantalla completa: |
| `app/index.tsx` | 119 | PANT-01 Arranque y distribuidor de rutas (maestro §6.4). QUÉ HACE: muestra el logo animado con "Preparando…" mientras se migra la base de datos y se carga la sesión. Luego redirige: sin sesión → login; contraseña temporal → cambio; especialista |
| `app/pests/[id].tsx` | 166 | PANT-41 Detalle de una alerta de plaga (ADR 0009). QUÉ HACE: muestra la foto (miniatura firmada por la plataforma; se ve con internet), el estado de revisión, la clase sugerida o confirmada (la IA «sugiere indicios», no diagnostica), el lu |
| `app/pests/_layout.tsx` | 12 | «Ubicar plaga» (ADR 0009): mapa/lista de alertas y detalle de cada una. La guarda (usuario AUTENTICADO: operador, administrador o especialista) está en app/_layout.tsx. |
| `app/pests/index.tsx` | 239 | PANT-40 «Ubicar plaga» (ADR 0009; plataforma v1.3, ADR-W-007). QUÉ HACE: muestra las alertas de plaga que el encargado debe ir a ver —confirmadas por la IA o por el especialista, posibles plagas y casos en revisión— en un MAPA SATELITAL ( |
| `app/settings.tsx` | 169 | PANT-09 Ajustes (maestro §9; RF-08, RF-09, RF-13, RF-35, RF-45). QUÉ HACE: muestra usuario, modo de acceso y fecha límite sin internet, función, versiones (app, protocolo, perfil de calidad, configuración), entorno, servidor (Fase 4: platafo |

### `src/`

Toda la lógica (sin rutas). Cada archivo empieza con un comentario «QUÉ HACE».

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `src/boot.ts` | 98 | Arranque de la app (PANT-01, maestro §6.4). QUÉ HACE, en orden: 1) Abre SQLite y ejecuta migraciones (si falla, PANT-01 ofrece "Exportar diagnóstico"). 2) Inyecta el generador de UUID (expo-crypto) y carga o crea el deviceId. 3) Purga registros v |

### `src/api/`

Único punto de salida a la red: `httpClient.ts` (solo él arma URLs de `/api/v1`), `dto.ts` (tipos del contrato), `authApi`, `bootstrapApi` (catálogos), `syncApi`, `uploadApi` + `cloudinaryUpload.ts` (subida directa con ticket firmado; único archivo que habla con Cloudinary), `deletionApi`, `pestApi` (alertas para «Ubicar plaga») y `mock/` (backend simulado para desarrollo y el perfil `preview`).

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `src/api/authApi.ts` | 41 | Endpoints de autenticación de la plataforma Django /api/v1/auth/* (maestro §7 y §15.2). QUÉ HACE: una función por endpoint. No guarda nada: authService decide qué hacer con la respuesta. El backend simulado (src/api/mock/mockBackend.ts) im |
| `src/api/bootstrapApi.ts` | 16 | Descarga de catálogos y parámetros (GET /api/v1/mobile/bootstrap, RF-15). La plataforma Django arma esta respuesta desde Supabase (lotes, hileras, segmentos y marcadores administrados en la web, más el perfil de calidad publicado). Si |
| `src/api/cloudinaryResponse.ts` | 64 | Interpretación pura de la respuesta de Cloudinary (v2.0, sección 15.7). Sin red ni Expo: se prueba con Jest (Anexo E.8, __tests__/captureUploader.test.ts). La usa cloudinaryUpload.ts. Copia exacta del bloque 15.7.1 del maestro v |
| `src/api/cloudinaryUpload.ts` | 54 | Subida directa de una foto a Cloudinary con el ticket firmado por Django (v2.0, 15.7). Único archivo de la app que habla con Cloudinary. No calcula firmas ni conoce el API secret (R-18, R-21). Adaptación al SDK 57 (el maestro v2.0 |
| `src/api/deletionApi.ts` | 20 | Fotos y sesiones borradas por el administrador (GET /api/v1/mobile/deleted-captures). QUÉ HACE: pide a la plataforma Django (v1.3.1, limpieza de fotos) la lista de fotos y sesiones que el ADMINISTRADOR eliminó para siempre después de ` |
| `src/api/dto.ts` | 376 | Contrato con la plataforma Django /api/v1 (maestro v2.0 §15.3 y docs/INTEGRACION_APP.md). QUÉ HACE: tipos de petición y respuesta de cada endpoint. El cliente real (httpClient + authApi, bootstrapApi, syncApi, uploadApi) y el backend simulado  |
| `src/api/httpClient.ts` | 187 | Único cliente HTTP hacia la plataforma Django /api/v1 (maestro v2.0 §6.1 y §15.1). QUÉ HACE: - Arma la URL `${EXPO_PUBLIC_API_URL}/api/v1${path}`, agrega cabeceras (X-Device-Id siempre y, si aplica, Authorization: Bearer) y aplica tiemp |
| `src/api/index.ts` | 38 | Punto único para obtener las APIs (real o simulada). QUÉ HACE: según ENV.useMockApi devuelve el backend simulado (desarrollo y demostración) o el cliente real de la plataforma Django /api/v1 (piloto). Los servicios importan SIEMPRE desde aqu |
| `src/api/mock/mockBackend.ts` | 419 | Backend SIMULADO con el mismo contrato que la plataforma Django (maestro §15.6). QUÉ HACE: implementa AuthApi, BootstrapApi, SyncApi, UploadApi y PestApi en memoria, con latencia y tasa de fallos configurables. Se activa con EXPO_ |
| `src/api/mock/mockCatalog.ts` | 62 | Catálogo semilla del backend simulado (maestro §15.6). QUÉ HACE: arma una BootstrapResponse con los lotes e hileras REALES del piloto (Anexo D) y segmentos y marcadores de EJEMPLO para poder recorrer la app. Los códigos definitivo |
| `src/api/mock/mockCloudinary.ts` | 65 | Cloudinary SIMULADO para el backend simulado (maestro v2.0 §15.6). QUÉ HACE: reemplaza a uploadToCloudinary cuando ENV.useMockApi es verdadero. No sube nada: guarda publicId y bytes en memoria y responde con la forma de la Uplo |
| `src/api/mock/mockPests.ts` | 133 | Alertas de plaga de EJEMPLO para el backend simulado («Ubicar plaga», ADR 0009). QUÉ HACE: arma una PestReportsResponse alrededor del centro del fundo (14°01'40.1"S 75°41'57.2"W) con un caso de cada estado que llega a la app y conto |
| `src/api/mock/pilotRows.ts` | 36 | Hileras y plantas por hilera de los lotes del piloto, tomadas del archivo «MAPEO CHANCHITO MOLINOS actualizado» (columna «N° ptas» de cada hoja). Maestro, Anexo D. Datos para el backend simulado; los códigos y nombres deben validars |
| `src/api/pestApi.ts` | 18 | Alertas de plaga con ubicación (GET /api/v1/mobile/pest-reports, plataforma v1.3; ADR 0009). QUÉ HACE: pide a la plataforma Django los casos que el encargado debe ir a ver (confirmados por la IA o por el especialista, posibles plagas y en  |
| `src/api/syncApi.ts` | 62 | Sincronización de datos con la plataforma Django (maestro v2.0 §15.2 y §15.5). QUÉ HACE: una función por endpoint JSON, todas idempotentes por ID (repetir no duplica y responde lo mismo). Devuelven ApiCallResult (nunca lanzan) para que src |
| `src/api/uploadApi.ts` | 85 | Ticket de subida y confirmación de capturas en Django (maestro v2.0 §15.2 y §15.7). QUÉ HACE (todas las funciones devuelven ApiCallResult; nunca lanzan): - requestTicket(): POST /captures/{captureId}/upload-ticket → parámetros firmados p |

### `src/auth/`

Sesión del usuario: login con y sin internet (verificador PBKDF2, hasta 7 días), tokens en SecureStore (acceso 15 min, renovación 14 días con rotación), validaciones.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `src/auth/authService.ts` | 541 | Inicio de sesión con y sin internet, renovación, cierre y revocación. Maestro §7.4 (online), §7.5 (offline), §7.6 (contraseña temporal), §7.8 (cierre/revocación), §7.11 (reabrir). QUÉ HACE (resumen): - login(): intenta con internet; s |
| `src/auth/authStore.ts` | 53 | Estado de sesión de usuario y del celular para la interfaz (Zustand). QUÉ HACE: guarda en memoria lo que las pantallas necesitan leer rápido: estado de autenticación, usuario, modo (con/sin internet), función del celular, permisos listo |
| `src/auth/offlineAuth.ts` | 164 | Verificador local para iniciar sesión sin internet (RN-03, RN-20). Maestro Anexo C.1. QUÉ HACE: tras un login CON internet se deriva un verificador PBKDF2-HMAC-SHA256 de la contraseña (sal aleatoria de 16 bytes, N iteraciones, 32 byte |
| `src/auth/tokenStore.ts` | 80 | Tokens de acceso/renovación en SecureStore (maestro §7.4, §7.8, §12.3). QUÉ HACE: guarda, lee y borra el par de tokens y el modo de acceso. Nunca los escribe en logs. Fase 4: mantiene una copia en memoria del par de tokens (la sincroni |
| `src/auth/validation.ts` | 66 | Validaciones de formularios (la API vuelve a validar). Maestro Anexo C.2. QUÉ HACE: valida login, registro, recuperación y cambio de contraseña ANTES de llamar a la API, y devuelve un error por campo para mostrarlo debajo de cada caja  |

### `src/camera/`

Agente de los celulares Cámara 1/Cámara 2: recibe órdenes del controlador, captura, evalúa calidad, envía la foto por la red local con cola y reintentos, y borra su copia solo cuando el controlador confirma la sincronización (retención).

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `src/camera/cameraAgent.ts` | 490 | Lado CÁMARA del protocolo local (maestro §14.6–14.9, §8.10, §8.12). QUÉ HACE: - pairWithQr(): tras escanear el QR (PANT-30) conecta el WebSocket y envía PAIR_REQUEST con la función del celular (CAMERA_1/2), el usuario con sesión, ve |
| `src/camera/cameraStore.ts` | 61 | Estado EN VIVO de un celular CÁMARA para PANT-30/31 (Zustand). QUÉ HACE: cameraAgent publica aquí el estado del enlace con el controlador, el contexto de la pasada (lateral, marcador), la pausa, la última captura y los pendientes. P |
| `src/camera/captureService.ts` | 258 | Ejecución de CAPTURE_COMMAND en la CÁMARA (maestro §14.7, pasos 4 a 6). QUÉ HACE: 1) Valida: sesión (UNKNOWN_SESSION), pasada (WRONG_PASS), vencimiento con el reloj ESTIMADO del controlador (COMMAND_EXPIRED) y ocupación (BUSY). U |
| `src/camera/retentionService.ts` | 44 | Liberación de espacio según la política de retención (RN-09, D-23). QUÉ HACE: - CÁMARA (TRAS_SINCRONIZACION): al recibir PAIRED.syncedSessionIds, borra SOLO los archivos de sus fotos que el controlador ya confirmó (RECIBIDA_CON |
| `src/camera/testModeService.ts` | 52 | Modo prueba de cámara en un solo celular (PANT-32; RF-32, RF-33). QUÉ HACE: toma una foto real, la guarda en test/{fecha}/{uuid}.jpg, evalúa la CALIDAD con el perfil vigente (CONFIG.quality) y guarda una ficha .json al lado (cal |
| `src/camera/transferQueue.ts` | 211 | Cola de envío de fotos de la CÁMARA al controlador (maestro §14.8, RF-35, RF-37). QUÉ HACE: procesa transfer_queue de a UNA foto (transfer.maxConcurrentPerCamera = 1): - Orden: prueba corta (-1) → útiles (0) → rechazadas (1); dent |

### `src/controller/`

Cerebro del controlador: runtime, sesiones, pasadas, motor de secuencias, monitor de cámaras (latidos), vinculación por QR, prueba corta, consolidación de fotos recibidas, cobertura y catálogos.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `src/controller/cameraMonitor.ts` | 41 | Monitor de enlace y desfase de reloj de las cámaras (maestro §14.9, RF-42). QUÉ HACE (funciones puras usadas por controllerRuntime en cada heartbeat): - linkFromSilence(): sin respuesta > unstableAfterMs → INESTABLE; > lostAft |
| `src/controller/catalogService.ts` | 102 | Catálogos de campo en el CONTROLADOR (RF-15, RF-16; maestro §12.5). QUÉ HACE: - updateCatalogs(): con internet y sesión ONLINE descarga GET /api/v1/mobile/bootstrap de la plataforma Django y REEMPLAZA de forma atómica lotes,  |
| `src/controller/consolidationService.ts` | 185 | Recepción y consolidación de fotos en el CONTROLADOR (§14.7–14.8, §8.13). QUÉ HACE con cada foto que llega (por HTTP real o desde una cámara virtual): 1) Controles: sesión conocida (UNKNOWN_SESSION), celular registrado  |
| `src/controller/controllerRuntime.ts` | 1668 | Orquestador del CONTROLADOR (protocolo local v1, maestro §8 y §14). QUÉ HACE: es el "cerebro" del celular controlador. Une los servicios de datos (sesión, pasada, secuencias, consolidación) con la red local (real o simulad |
| `src/controller/controllerStore.ts` | 154 | Estado EN VIVO del controlador para las pantallas (Zustand). QUÉ HACE: el runtime del controlador (controllerRuntime.ts) publica aquí lo que las pantallas muestran: sesión y pasada vigentes, tarjetas de cada cámara (enlace,  |
| `src/controller/coverageService.ts` | 215 | Ciclo de monitoreo y avance de campo en el CONTROLADOR (CFG-3). QUÉ HACE: une SQLite (src/storage/repositories/coverageRepo.ts y passRepo.ts) con las reglas puras de src/domain/coverage.ts para que las pantallas solo muestre |
| `src/controller/pairingService.ts` | 96 | Emparejamiento por QR en el CONTROLADOR (maestro §14.2 y §14.6). QUÉ HACE: - newPairingToken(): 16 bytes aleatorios en base64url; en SQLite solo se guarda su SHA-256. - buildQr(): contenido del QR (tipo RIACHUELO_PAIR, IP, pu |
| `src/controller/passService.ts` | 150 | Pasadas por lateral (maestro §8.4, §8.6, §8.7; RN-10, RN-12, RN-13). QUÉ HACE (datos y reglas; los mensajes START_PASS/END_PASS los envía controllerRuntime): - createPass(): valida RN-12 (LATERAL_B exige LATERAL_A cerrada; nunca |
| `src/controller/sequenceEngine.ts` | 128 | Secuencias coordinadas (maestro §14.7, RF-21, RN-06, RN-07, RN-18). QUÉ HACE: - createSequence(): número correlativo, dos captureId (uno por cámara), contexto vigente de la pasada (lote, hilera, segmento, marcador), GPS con a |
| `src/controller/sessionService.ts` | 146 | Sesión de monitoreo en el CONTROLADOR (maestro §8.3, §8.8, §13 "Sesión"). QUÉ HACE (solo datos y reglas; los mensajes a las cámaras los envía controllerRuntime): - createDraft(): PANT-12 "Crear sesión" → DRAFT con versión de  |
| `src/controller/shortTest.ts` | 76 | Prueba corta antes de la primera pasada (contrato del maestro §14.12). QUÉ HACE: define el estado de la prueba en curso (app_meta.short_test_pending) y la decisión por cámara: - APROBADA: respondió CAPTURE_OK a tiempo y su foto ll |

### `src/device/`

Acceso al hardware: cámara, GPS (lectura cada 1 s; se usa si tiene ≤ 10 s), batería, red, estabilidad (acelerómetro/giroscopio para el modo automático), calidad de imagen (exposición y nitidez en ≤ 2 s), almacenamiento, galería, permisos e identidad del celular.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `src/device/batteryService.ts` | 24 | Nivel de batería (RF-46). QUÉ HACE: lee el nivel (0..100 %) para el monitor, los avisos y las reglas de inicio/pausa (RN-10). Devuelve null si el sistema no lo informa (simuladores): null no bloquea ninguna regla. |
| `src/device/cameraService.ts` | 76 | Toma de fotos con la cámara física (RF-29, RF-30, RF-32). QUÉ HACE: la pantalla que muestra <CameraView> (PANT-31 o PANT-32) registra su referencia aquí. takeToFile() dispara takePictureAsync({ quality, exif: false, shutterSound } |
| `src/device/deviceIdentity.ts` | 46 | Identidad del celular (RF-11). QUÉ HACE: genera UNA sola vez por instalación un deviceId (UUID) y lo guarda en SecureStore (device.id) y en app_meta.device_id. Reúne plataforma, modelo, sistema y versión de la app, datos que viaj |
| `src/device/deviceRole.ts` | 63 | Función del celular: CONTROLADOR, CÁMARA 1 o CÁMARA 2 (maestro §7.9, D-08). QUÉ HACE: guarda la función en app_meta.device_role (persiste entre reinicios) y valida el cambio con RN-15 (sin sesión de monitoreo abierta ni transferencia |
| `src/device/galleryService.ts` | 76 | Fotos guardadas en el celular (galería, pedido del equipo). QUÉ HACE: lista las fotos del almacenamiento privado (sesión, prueba corta y modo prueba) y arma la ficha de cada una (calidad, métricas, rol, estado de transferencia) d |
| `src/device/gpsService.ts` | 72 | GPS continuo del controlador (RF-38, RN-18). QUÉ HACE: con watchPositionAsync (precisión máxima) guarda la ÚLTIMA lectura. Cada secuencia toma latest(): la lectura solo se usa si su antigüedad es ≤ gps.maxAgeMs; si no, la secuencia q |
| `src/device/keepAwake.ts` | 28 | Pantalla siempre encendida durante la pasada (RF-29, R-19). QUÉ HACE: reexporta useKeepAwake con una etiqueta por pantalla. Se usa en PANT-16 (pasada activa), PANT-31 (cámara en sesión) y PANT-32 (modo prueba). La captura NUNCA ocurre |
| `src/device/networkMonitor.ts` | 78 | Vigila la conectividad (expo-network, D-19) y la IP local. QUÉ HACE: - Al cambiar la red, comprueba GET /health (authService.checkBackend) con una pequeña espera para no saturar; así se actualiza el indicador "Con internet / Sin  |
| `src/device/permissions.ts` | 85 | Consulta y solicitud de permisos según la función del celular (RF-14, PANT-07). QUÉ HACE: CÁMARA para Cámara 1/2 (fotos y lectura del QR); UBICACIÓN para el Controlador (GPS por secuencia). La "Red local" del iPhone no se puede cons |
| `src/device/preferences.ts` | 17 | Preferencias de interfaz del celular (sonidos y vibración). QUÉ HACE: guarda en app_meta (ui_sounds, ui_haptics) y aplica al instante en src/ui/feedback.ts. Son preferencias del celular (no del usuario) y no viajan al backend. |
| `src/device/quality.ts` | 126 | Métricas y decisión de calidad (funciones puras, perfil Q0). Maestro Anexo B.1. QUÉ HACE: decide si una foto es TÉCNICAMENTE utilizable (no decide plagas: RN-14). Flujo en la app: (1) copia reducida a analysisWidth → jpeg-js → luminance |
| `src/device/qualityPipeline.ts` | 118 | Evaluación de calidad sobre una foto real (maestro §16.2, Anexo B.2). QUÉ HACE: 1) Exposición: copia reducida (analysisWidth px) de TODA la foto → luminancia media y % de píxeles oscuros/saturados. Si falla → REPETIR_EXPOSICION  |
| `src/device/stabilityDetector.ts` | 143 | Detector de estabilidad de la cámara (acelerómetro + giroscopio). QUÉ HACE: decide si el celular está QUIETO para que la captura AUTOMÁTICA solo dispare con la cámara estable (pedido del equipo, CFG-2). Analiza una ventana des |
| `src/device/storageInfo.ts` | 18 | Espacio libre del celular (RF-46, RNF-13). QUÉ HACE: informa el espacio libre y su estado frente a los umbrales de configuración (aviso, inicio, pausa). |

### `src/diagnostics/`

Registro de eventos, incidencias y exportación del diagnóstico (JSON sin contraseñas ni tokens).

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `src/diagnostics/eventLog.ts` | 80 | Registro de eventos técnicos (maestro §20.1). QUÉ HACE: logEvent() guarda un evento en SQLite (event_log) SIN lanzar errores: el registro nunca debe romper el flujo de trabajo. Categorías: AUTH, DEVICE, CATALOG, SESSION, NET, CAPT |
| `src/diagnostics/exportDiagnostics.ts` | 89 | Exportar diagnóstico (maestro §20.2, RF-45). QUÉ HACE: arma diagnostics/diagnostico_{deviceId}_{fecha}.json con: versiones (app, protocolo, configuración, perfil de calidad), datos del equipo, función, resumen de sesiones |
| `src/diagnostics/incidents.ts` | 59 | Alta de incidencias automáticas (SISTEMA) y manuales (OPERADOR), RF-43. QUÉ HACE: crea la fila en `incidents` y deja rastro en event_log. Las pantallas usan addManualIncident; el controlador usa addSystemIncident ante desconexion |

### `src/domain/`

Reglas puras y tipos (sin efectos): tipos del dominio, máquinas de estado, secuencias, cobertura, segmentos fijos, geografía (distancia/rumbo), plagas, borrados, cola de sincronización, IDs y tiempo. Fáciles de probar con Jest.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `src/domain/coverage.ts` | 251 | Avance de campo por CICLO: hileras, lotes, lateral B y áreas no hechas (funciones puras). QUÉ HACE (pedido del equipo, CFG-3; ver docs/adr/0004-ciclos-y-cobertura.md): 1. BLOQUEO POR CICLO: un lateral "terminó correctamente" si tiene u |
| `src/domain/deletions.ts` | 46 | Limpieza de fotos ordenada por el administrador (plataforma v1.3.1; lógica pura). QUÉ HACE: - isDeletionCode(): 410 SESSION_DELETED / CAPTURE_DELETED: lo que el celular intentaba sincronizar ya no existe en el servidor porque el ADMIN |
| `src/domain/geo.ts` | 78 | Cálculos de ubicación para «Ubicar plaga» (ADR 0009). TypeScript puro, sin Expo ni React. QUÉ HACE: - distanceM(): distancia en metros entre dos puntos WGS84 (fórmula del semiverseno; suficiente para un fundo). - bearingDeg() y cardinal():  |
| `src/domain/ids.ts` | 51 | Generación de identificadores (R-13: IDs con Crypto.randomUUID()). QUÉ HACE: centraliza la creación de UUID y tokens aleatorios. El dominio recibe la función por inyección (setIdProvider) para que las pruebas unitarias no dependan de módulo |
| `src/domain/pests.ts` | 179 | Alertas de plaga para «Ubicar plaga» (ADR 0009). TypeScript puro, sin Expo ni React. QUÉ HACE: - Tipos de GET /api/v1/mobile/pest-reports (plataforma v1.3, ADR-W-007): casos con ubicación y capas del fundo. - Qué estados llegan a la app:  |
| `src/domain/pictureSize.ts` | 42 | Elección de la resolución de captura (maestro Q-15 y riesgo «fotos demasiado grandes»). QUÉ HACE: de la lista de getAvailablePictureSizesAsync() de expo-camera elige la MAYOR resolución que no supere capture.maxMegapixels, prefirien |
| `src/domain/rules.ts` | 234 | Reglas de negocio como funciones puras (RN-06, RN-10 a RN-13, RN-15, RN-21). QUÉ HACE: cada función recibe datos ya leídos (no toca SQLite ni la red) y devuelve una decisión. Así se prueban con Jest (__tests__/rules.test.ts) y los servici |
| `src/domain/segments.ts` | 141 | Segmentos FIJOS por hilera (plataforma v1.4, ADR-W-010; app ADR 0011). Funciones puras. QUÉ HACE: - La web define una cantidad fija de segmentos por hilera (10 por defecto, ≈20 m cada uno en una hilera de 200 m) y la envía en el bootst |
| `src/domain/sequence.ts` | 83 | Estado de una secuencia derivado del resultado de cada cámara (maestro §11.3). QUÉ HACE: funciones puras que el motor de secuencias (src/controller/sequenceEngine.ts) usa para recalcular el estado cada vez que llega una respuesta o una |
| `src/domain/stateMachines.ts` | 118 | Transiciones de estado permitidas (contrato v1, maestro §11.2 y §13). QUÉ HACE: cada mapa dice, para un estado, a qué estados se puede pasar. Los servicios llaman a assertTransition() antes de escribir un cambio de estado en SQLit |
| `src/domain/syncQueue.ts` | 17 | Tipos y orden de la cola de sincronización con la plataforma (maestro v2.0 §15.5). QUÉ HACE: define los tipos de elemento de sync_queue, sus estados y el order_key de cada tipo. Es puro (sin SQLite) para que el planificador (src/sync/ |
| `src/domain/time.ts` | 59 | Reloj del dominio (R-13: fechas ISO-8601 UTC con toISOString()). QUÉ HACE: todas las capas piden la hora a nowIso()/nowMs(). En pruebas se puede fijar un reloj falso con setClock() para verificar vencimientos (órdenes, bloqueo de login, ve |
| `src/domain/types.ts` | 393 | Contrato de tipos del dominio (versión 1 + ajustes de interfaz CFG-2). QUÉ HACE: define los tipos que comparten TODAS las capas (pantallas, servicios, protocolo, base de datos y API). Es TypeScript puro: no importa React, Expo ni otras ca |

### `src/local-network/`

Red local sin internet entre los 3 celulares: servidor TCP propio en el controlador (`react-native-tcp-socket`) con WebSocket de control (puerto 8765, latido cada 2 s, enlace perdido a los 6 s) y HTTP de fotos (puerto 8766, `POST /local/captures`, verificación md5); clientes de las cámaras y un simulador para Expo Go.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `src/local-network/client/uploadFileSender.ts` | 51 | Envío de una foto de la CÁMARA al controlador (opción A, §14.8). QUÉ HACE: File.upload() de expo-file-system envía el archivo de forma NATIVA (no pasa por la memoria de JavaScript) como cuerpo binario a http://<ho |
| `src/local-network/client/wsControlClient.ts` | 109 | Cliente de control de la CÁMARA (WebSocket nativo de RN). QUÉ HACE: conecta a ws://<host>:<controlPort>/control, valida cada mensaje recibido con zod y avisa los cambios de estado (CONECTANDO / CONECTADO / DESCONEC |
| `src/local-network/factory.ts` | 73 | Elige la red local REAL o el SIMULADOR (maestro §6.3, regla R-04). QUÉ HACE: - En Expo Go (isRunningInExpoGo() = true): servidor y receptor del controlador = SIMULADOR con dos cámaras virtuales. NUNCA se carga react-native-tcp-so |
| `src/local-network/httpUpload.ts` | 161 | CÓDIGO DE REFERENCIA PROBADO (maestro, Anexo A.2). No modificar sin actualizar __tests__/httpUpload.test.ts. src/local-network/httpUpload.ts — Receptor HTTP mínimo para la opción A de transferencia (POST /local/captures con el JPEG como cuerpo binario y metada |
| `src/local-network/simulator/simulatedNetwork.ts` | 210 | Red local SIMULADA en memoria (Expo Go, Fase 1). QUÉ HACE: implementa ControlServer y FileReceiver sin sockets. Dos cámaras virtuales (virtualCamera.ts) viven dentro del controlador y hablan EXACTAMENTE el mism |
| `src/local-network/simulator/virtualCamera.ts` | 395 | Cámara VIRTUAL para Expo Go (maestro §6.3, RF-48). QUÉ HACE: se comporta como el celular de una cámara real pero dentro del controlador: - "escanea" el QR y envía PAIR_REQUEST con el token; responde ACK a PAIRED,  |
| `src/local-network/tcp/tcpControlServer.ts` | 195 | Servidor de control REAL del controlador (maestro §14.11). SOLO DEVELOPMENT BUILD / APK (Fase 2). Nunca se importa en Expo Go: factory.ts lo carga con require() diferido y le pasa el módulo react-native-tcp-socket ya |
| `src/local-network/tcp/tcpFileReceiver.ts` | 241 | Receptor HTTP REAL de fotos en el controlador (opción A, §14.8). SOLO DEVELOPMENT BUILD / APK (Fase 2). Se carga con require() diferido desde factory.ts. QUÉ HACE: servidor TCP en <filePort> que entiende un POST /loca |
| `src/local-network/transport.ts` | 88 | Interfaces de transporte (contrato del maestro §6.3). QUÉ HACE: la app depende de ESTAS interfaces, nunca de la librería de red. Implementaciones: (1) Real: servidor TCP + WebSocket propio (react-native-tcp-socket) en el contro |
| `src/local-network/wsCodec.ts` | 315 | CÓDIGO DE REFERENCIA PROBADO (maestro, Anexo A). No modificar sin actualizar __tests__/wsCodec.test.ts. src/local-network/wsCodec.ts — Servidor WebSocket mínimo (RFC 6455) sobre un socket TCP crudo. Sin dependencias de plataforma: recibe bytes (Uint8Array) y e |

### `src/pests/`

«Ubicar plaga» (ADR 0009): servicio de alertas, HTML del mapa con Leaflet embebido (`vendor/leaflet.ts`, sin CDN) y teselas de Esri, y `openMaps.ts` que abre Google Maps «Cómo llegar» a pie por URL (sin clave de API).

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `src/pests/mapHtml.ts` | 230 | Página del mapa satelital de «Ubicar plaga» para el WebView (ADR 0009). QUÉ HACE: - buildMapHtml(): arma una página con Leaflet 1.9.4 EMBEBIDO (src/pests/vendor/leaflet.ts, sin CDN) y la vista satelital de Esri World Imagery con nombres  |
| `src/pests/openMaps.ts` | 29 | «Cómo llegar»: abre Google Maps con la ruta a pie hasta la alerta (ADR 0009). QUÉ HACE: usa la URL universal de Google Maps (src/domain/geo.ts): abre la app de Google Maps si está instalada y, si no, el navegador. No necesita clave ni S |
| `src/pests/pestService.ts` | 141 | «Ubicar plaga»: alertas con ubicación, copia sin internet y posición del celular (ADR 0009). QUÉ HACE: - refreshPests(): con sesión ONLINE pide GET /api/v1/mobile/pest-reports (últimos pests.windowDays días) y guarda la respuesta en  |

### `src/protocol/`

Mensajes del protocolo local entre controlador y cámaras (sobre, tipos y esquemas zod).

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `src/protocol/envelope.ts` | 64 | Creación y lectura de sobres del protocolo local. QUÉ HACE: - createEnvelope(): arma un mensaje con messageId nuevo, hora del emisor y versión de protocolo. - parseEnvelope(): convierte el texto recibido por WebSocket en un Envelope  |
| `src/protocol/messages.ts` | 293 | Protocolo local v1 (WebSocket, JSON). Contrato del maestro §14.5. QUÉ HACE: define el sobre (Envelope) y el contenido (payload) de cada mensaje que viaja entre el controlador y las cámaras por el Wi-Fi del campo. Lo usan por igual la |
| `src/protocol/schemas.ts` | 259 | Esquemas zod de cada mensaje del protocolo local (maestro §14.3: todo mensaje se valida con zod ANTES de procesarse). También valida el QR y la cabecera X-Capture-Meta. QUÉ HACE: si un mensaje no cumple su esquema, envelope.ts lo rech |

### `src/storage/`

Base SQLite local con migraciones `001` a `005` (nunca modificar una publicada; agregar la siguiente), repositorios por tabla y manejo de archivos en el almacenamiento privado de la app.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `src/storage/db.ts` | 88 | Apertura de SQLite y migraciones (maestro §12.2). QUÉ HACE: - openAppDb(): abre riachuelo.db, activa WAL y claves foráneas y ejecuta las migraciones pendientes (cada una en una transacción exclusiva; al terminar fija PRAGMA user_version). - |
| `src/storage/files.ts` | 201 | Rutas y operaciones de archivos de fotos (maestro §12.4). QUÉ HACE: - Define dónde vive cada tipo de foto dentro del almacenamiento PRIVADO de la app (Paths.document): captures/{sessionId}/{passId}/{captureId}.jpg fotos de evidencia (cám |
| `src/storage/migrations/001_initial.ts` | 322 | Esquema SQLite v1 (PRAGMA user_version = 1). Maestro §12.1. QUÉ HACE: crea todas las tablas de la app. El SQL se exporta como texto porque Metro no importa archivos .sql. Columnas en snake_case; los repositorios (src/sto |
| `src/storage/migrations/002_cobertura.ts` | 53 | Ciclos de monitoreo, lotes planificados y áreas no hechas (CFG-3). QUÉ HACE: - monitoring_cycles: rondas de monitoreo. Lo terminado se bloquea DENTRO del ciclo; un ciclo nuevo libera todo. - monitoring_sessions.cycle_i |
| `src/storage/migrations/003_remote_uploads.ts` | 34 | Resultado de la subida directa a Cloudinary (maestro v2.0 §12.1.1). QUÉ HACE: agrega la tabla remote_uploads y no modifica ninguna existente. Una fila por captura de evidencia del CONTROLADOR que entró en sync_que |
| `src/storage/migrations/004_catalog_codes.ts` | 49 | Catálogos compatibles con la plataforma Django (Fase 4, ADR 0007). QUÉ HACE: en la migración 001, cat_segments.code y cat_markers.code eran UNIQUE en TODO el catálogo. En la plataforma (campo/models.py) el código d |
| `src/storage/migrations/005_row_length.ts` | 10 | Largo de la hilera en metros (v0.6.0; plataforma v1.4, ADR-W-010). QUÉ HACE: agrega cat_rows.length_m (REAL, NULL si la web no lo tiene). Solo una columna nueva: los catálogos se reemplazan completos en cada «Actualiz |
| `src/storage/repositories/appMetaRepo.ts` | 64 | Tabla app_meta (clave/valor no secreto). QUÉ HACE: guarda datos de configuración del celular que no son secretos: device_id, device_role, catalog_version, bootstrap_at, offline_failed_attempts, offline_locked_until, sh |
| `src/storage/repositories/cameraContextRepo.ts` | 98 | Sesión a la que está unida una CÁMARA (tabla camera_context). QUÉ HACE: guarda los datos del QR (host y puertos) para reconectar, la configuración recibida en PAIRED y el último SESSION_CONTEXT. Un contexto está  |
| `src/storage/repositories/captureRepo.ts` | 332 | Capturas (fotos) y resultados de calidad. QUÉ HACE: tabla `captures` (en cámaras y controlador) y `quality_results`. - En la CÁMARA: cada foto tomada, su calidad y su estado de transferencia al controlador. - En el CON |
| `src/storage/repositories/catalogRepo.ts` | 219 | Catálogos de campo (lotes, hileras, segmentos, marcadores). QUÉ HACE: - replaceCatalogs(): reemplazo ATÓMICO de las 4 tablas (borrar e insertar en una transacción) y actualización de app_meta.catalog_version / bootstra |
| `src/storage/repositories/coverageRepo.ts` | 131 | Ciclos, lotes planificados y áreas no hechas (CFG-3, migración 002). QUÉ HACE: lee y escribe monitoring_cycles, session_planned_lots y uncovered_areas en el CONTROLADOR. Las reglas (qué está bloqueado, qué falta, quié |
| `src/storage/repositories/deletionRepo.ts` | 65 | Copia local de lo que el administrador borró (plataforma v1.3.1). QUÉ HACE: para fotos o sesiones eliminadas en el servidor: - devuelve las rutas de los archivos de esas fotos (los borra deletionService, fuera de la t |
| `src/storage/repositories/eventLogRepo.ts` | 72 | Tabla event_log (registro técnico de eventos, maestro §20.1). QUÉ HACE: inserta, lista y purga eventos. data_json lleva SOLO identificadores, códigos, tiempos y conteos (nunca contraseñas, tokens ni el verificador; re |
| `src/storage/repositories/incidentRepo.ts` | 78 | Incidencias automáticas y manuales (RF-43). Se envían a la plataforma Django en lotes de hasta 200 (POST /api/v1/sessions/{id}/incidents/batch) al sincronizar (src/sync/syncService.ts, elemento INCIDENT_BATCH). |
| `src/storage/repositories/passRepo.ts` | 308 | Pasadas, cambios de marcador y repeticiones (controlador). QUÉ HACE: lee y escribe monitoring_passes, marker_changes y retake_requests. Las filas de evidencia nunca se eliminan (maestro §12.5). |
| `src/storage/repositories/processedMessagesRepo.ts` | 32 | Mensajes del protocolo ya procesados (idempotencia). QUÉ HACE: el receptor guarda cada messageId procesado con la respuesta enviada. Ante un duplicado (reenvío con el mismo messageId) NO vuelve a procesar y r |
| `src/storage/repositories/remoteUploadRepo.ts` | 165 | Tabla remote_uploads (migración 003, maestro v2.0 §12.1.1). QUÉ HACE: guarda el resultado de la subida directa de cada foto a Cloudinary en el CONTROLADOR, para confirmar en Django sin volver a subir si la app se  |
| `src/storage/repositories/sequenceRepo.ts` | 204 | Secuencias de captura (controlador). QUÉ HACE: una secuencia = una orden coordinada a las dos cámaras con dos captureId (uno por cámara), el contexto vigente (lote, hilera, segmento, marcador) y el GPS de ese momento  |
| `src/storage/repositories/sessionRepo.ts` | 309 | Sesiones de monitoreo y celulares de cámara por sesión. QUÉ HACE: lee y escribe monitoring_sessions y session_devices (solo en el CONTROLADOR). La lógica de qué cambio de estado es válido NO está aquí: está en src/cont |
| `src/storage/repositories/syncQueueRepo.ts` | 367 | Cola del CONTROLADOR hacia la plataforma Django (sync_queue). QUÉ HACE: al cerrar una sesión se crean, en la MISMA transacción, sus elementos en orden (10 sesión, 20 pasada, 30 secuencias, 40 capturas, 50 incidencias |
| `src/storage/repositories/transferQueueRepo.ts` | 130 | Cola de la CÁMARA hacia el controlador (transfer_queue). QUÉ HACE: cada foto a enviar tiene una fila con prioridad (-1 prueba corta, 0 útil, 1 rechazada), intentos, fallos de checksum y la hora del próximo intent |
| `src/storage/repositories/usersRepo.ts` | 76 | Tabla users_cache (datos NO secretos del último login). QUÉ HACE: guarda el perfil del usuario que inició sesión con internet en este celular. Se usa para el login sin internet (RN-03), para la cabecera y para el PAIR_RE |
| `src/storage/secureStore.ts` | 51 | Acceso tipado a SecureStore (maestro §12.3). QUÉ HACE: guarda SOLO secretos o datos sensibles pequeños (≤ 2048 bytes por valor; claves con letras, números, ".", "-" y "_"). Tokens, verificador sin internet y token del QR van aquí;  |

### `src/sync/`

Sincronización del controlador con Django: `syncService.ts` (motor: SESSION → PASS → SEQUENCE_BATCH ≤ 200 → CAPTURE → INCIDENT_BATCH → SESSION_CLOSE), `syncPlanner.ts` (orden y dependencias, puro), `syncPayloads.ts` (datos del contrato, puro), `captureUploader.ts` (ticket → Cloudinary → confirmación), `retry.ts`, cola y borrados ordenados.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `src/sync/captureUploader.ts` | 159 | Sincroniza UNA captura en tres pasos: ticket, subida directa a Cloudinary y confirmación en Django (v2.0, sección 15.7). Lógica pura con dependencias inyectadas: syncService.ts le pasa la base de datos (remote_uploads, captures, s |
| `src/sync/deletionService.ts` | 115 | Borra en el celular lo que el administrador eliminó en la plataforma (v0.5.1). QUÉ HACE: - checkServerDeletions(): con sesión ONLINE pide GET /api/v1/mobile/deleted-captures desde el último cursor (app_meta deleted_cursor), hasta  |
| `src/sync/retry.ts` | 108 | Espera entre reintentos y clasificación de errores (maestro v2.0, Anexo C.3, código probado). QUÉ HACE: - nextDelayMs(): espera del intento N según una tabla (repite la última; respeta un tope). - classifySyncError(): qué hacer ante un fall |
| `src/sync/syncPayloads.ts` | 267 | Conversión de los datos locales del CONTROLADOR al contrato /api/v1 (funciones puras). QUÉ HACE: arma los cuerpos de cada petición de la sincronización (maestro v2.0 §15.3 y §15.5) a partir de los tipos del dominio, SIN tocar SQLite  |
| `src/sync/syncPlanner.ts` | 159 | Qué elementos de sync_queue se pueden enviar ahora (funciones puras, maestro §15.5). QUÉ HACE: recibe los elementos de la cola (con su estado) y decide, sin tocar SQLite ni la red: 1) pickReadyItems(): los elementos LISTOS, en orden:  |
| `src/sync/syncQueue.ts` | 65 | Lectura de la cola de sincronización para PANT-20 y el panel del controlador (Fase 4). QUÉ HACE: resume sync_queue por sesión (total, enviados, pendientes, errores con su código, último intento) y, para las fotos, cuántas ya están en la |
| `src/sync/syncService.ts` | 820 | Sincronización del CONTROLADOR con la plataforma Django /api/v1 (maestro v2.0 §8.9, §15.5, §15.7; tarea T-20 de la Fase 4). QUÉ HACE runSync(): 1. Una sola ronda a la vez (si ya hay una, responde SINCRONIZACION_EN_CURSO). 2. GET /heal |
| `src/sync/syncStore.ts` | 71 | Estado en memoria de la sincronización para PANT-20 y el panel del controlador (Zustand). QUÉ HACE: syncService.ts escribe aquí el avance de la ronda en curso (elemento actual, paso de la foto, bytes enviados a Cloudinary, conteos) y el |

### `src/ui/`

Interfaz: componentes reutilizables, hooks, tema, retroalimentación (sonidos/vibración) y **todos los textos** en `strings.ts` y `messages.ts` (no se escriben textos sueltos en las pantallas).

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `src/ui/components/AnimatedCounter.tsx` | 41 | Número que "salta" cuando cambia (contadores de secuencias). |
| `src/ui/components/AppButton.tsx` | 132 | Botón de la app con animación, sonido y vibración. QUÉ HACE: al presionar se "hunde" (escala con resorte), suena (tap/confirm/shutter según la variante) y vibra levemente. Altura mínima 56 dp (R-15). Muestra un indicador de ca |
| `src/ui/components/AppHeader.tsx` | 119 | Cabecera con degradado: título, usuario · acceso · función, y acciones. QUÉ HACE (maestro §7.9 y contexto §25.3): - Siempre muestra quién está (usuario), el modo de acceso (con/sin internet) y la función del celular. - En las  |
| `src/ui/components/CameraCardView.tsx` | 91 | Tarjeta de una cámara en el controlador (PANT-13 y PANT-16, RF-42). QUÉ HACE: muestra enlace (con pulso si conecta o está inestable), usuario y modelo, batería, espacio, fotos pendientes, desfase de reloj y el último resu |
| `src/ui/components/Card.tsx` | 51 | Tarjeta blanca con sombra y entrada animada. QUÉ HACE: agrupa datos de una misma cosa (cámara, sesión, catálogos). Con `delay` se escalonan las tarjetas al abrir la pantalla. `tone` pinta un borde lateral de color para estados impo |
| `src/ui/components/CheckList.tsx` | 114 | Lista de selección múltiple con casillas grandes (CFG-3: "Lotes a trabajar"). QUÉ HACE: cada fila (≥ 56 dp) muestra una casilla animada, el nombre, una pista (p. ej. "12/31 hileras completas") y una etiqueta de estado. Las fil |
| `src/ui/components/ChipGroup.tsx` | 80 | Grupo de chips seleccionables (p. ej. intervalo 1 s · 2 s · 3 s · 5 s). QUÉ HACE: el chip elegido se resalta con un pequeño "rebote". Con `disabled` todo el grupo queda con fondo blanco/transparente y atenuado, sin interacción |
| `src/ui/components/ConfirmDialog.tsx` | 86 | Diálogo de confirmación animado (uso imperativo: await confirm({...})). QUÉ HACE: reemplaza Alert.alert para que el estilo, el tamaño de botones (≥ 56 dp) y los sonidos sean coherentes en Android e iOS. Se monta una vez en |
| `src/ui/components/FadeIn.tsx` | 35 | Animación de entrada (aparece y sube suavemente). QUÉ HACE: envuelve cualquier bloque para que entre con opacidad y desplazamiento. `delay` permite escalonar tarjetas y botones y darle dinamismo a cada pantalla sin afectar el ren |
| `src/ui/components/InfoRow.tsx` | 34 | Fila "Etiqueta ........ Valor" (estado antes que texto, contexto §25.6). |
| `src/ui/components/Logo.tsx` | 46 | Logo de Agrícola Riachuelo con animación de entrada y "flotación" suave. QUÉ HACE: el logo aparece creciendo con un rebote y luego flota arriba/abajo lentamente (sensación viva en el login y el arranque). `size` controla el ancho;  |
| `src/ui/components/OfflineBanner.tsx` | 95 | Aviso de acceso sin internet y revalidación (maestro §7.5). QUÉ HACE: - Sesión OFFLINE: franja "Sin internet — acceso válido hasta <fecha>". - Si vuelve el internet (y no hay pasada ACTIVE): aviso no bloqueante REAUTENTICA |
| `src/ui/components/PestItem.tsx` | 108 | Tarjeta de una alerta de plaga («Ubicar plaga», ADR 0009). QUÉ HACE: - PestBadge: estado con punto del MISMO color que el marcador del mapa y texto (RNF-09: no solo color). - PestItem: estado, clase sugerida o confirmada, lugar |
| `src/ui/components/PestMap.tsx` | 119 | Mapa satelital de «Ubicar plaga» (WebView con Leaflet embebido; ADR 0009). QUÉ HACE: muestra la página de src/pests/mapHtml.ts y le envía los datos cada vez que cambian (contornos de lotes, hileras, puntos del fundo, alertas con |
| `src/ui/components/PickerSheet.tsx` | 127 | Selector en hoja inferior animada (lote, hilera, segmento, marcador). QUÉ HACE: muestra un campo con el valor elegido; al tocarlo sube una hoja con la lista (botones grandes, RF-16: todo se elige sin internet desde los catál |
| `src/ui/components/QrCodeView.tsx` | 57 | Dibuja el QR de emparejamiento (maestro §14.2) con react-native-svg. QUÉ HACE: genera la matriz con qrcode-generator (MIT, JavaScript puro: funciona en Expo Go) y la dibuja como un solo <Path>. Nivel de corrección M y margen  |
| `src/ui/components/ScanFrame.tsx` | 44 | Marco animado para escanear el QR (esquinas que "respiran" y línea que recorre). |
| `src/ui/components/Screen.tsx` | 56 | Contenedor base de cada pantalla (área segura, fondo y desplazamiento). QUÉ HACE: aplica márgenes seguros (notch), el fondo de la app y, si `scroll`, un ScrollView que se ajusta al teclado. `footer` queda FIJO abajo (acciones crí |
| `src/ui/components/SectionTitle.tsx` | 21 | Título corto de sección (menos texto, más estado: contexto §25.6). |
| `src/ui/components/Segmented.tsx` | 82 | Selector de opciones con indicador deslizante animado. QUÉ HACE: muestra 2 a 4 opciones (p. ej. MANUAL / AUTOMÁTICO, LATERAL A / B). El fondo resaltado se desliza con resorte hacia la opción elegida y suena "toggle". Con `disa |
| `src/ui/components/SimulatorTools.tsx` | 75 | Herramientas para inducir fallos con las cámaras VIRTUALES. QUÉ HACE: solo aparece cuando el controlador usa el simulador (Expo Go). Permite cortar el enlace de una cámara 10 s (pausa automática, reconexión y RESYNC: CP-2 |
| `src/ui/components/StabilityRing.tsx` | 69 | Indicador circular de estabilidad de la cámara. QUÉ HACE: un anillo que se llena según qué tan quieto está el celular (0 a 100 %) y cambia de color: verde = "Quieta" (puede disparar en automático), ámbar/rojo = "En movimie |
| `src/ui/components/StatusPill.tsx` | 60 | Etiqueta de estado con punto de color y TEXTO (RNF-09: no solo color). QUÉ HACE: muestra estados como CONECTADA, PERDIDA, UTILIZABLE… El punto "late" (pulso) cuando `pulse` es verdadero, para estados en curso (conectando, cap |
| `src/ui/components/TextField.tsx` | 97 | Caja de texto con etiqueta, error por campo y animación de foco. QUÉ HACE: al enfocar, la línea inferior se ilumina (verde) con una transición; si hay error se pinta en rojo y el campo "tiembla" una vez para llamar la atención |
| `src/ui/components/ToastHost.tsx` | 57 | Muestra los avisos de src/ui/toast.ts (entra desde arriba y se va solo). Se monta UNA vez en app/_layout.tsx. También escucha los avisos del controlador (controllerStore.notice). |
| `src/ui/feedback.ts` | 103 | Sonidos y vibración de los botones (pedido del equipo, CFG-2: ui.*). QUÉ HACE: precarga sonidos cortos (assets/sounds/*.wav) con expo-audio y los reproduce al tocar botones: tap (toque), confirm (acción principal), success (operación corre |
| `src/ui/hooks/useCapturePictureSize.ts` | 68 | Resolución de captura de <CameraView> (capture.maxMegapixels, Q-15). QUÉ HACE: la primera vez que la cámara avisa que está lista, pide los tamaños disponibles y fija `pictureSize` en la mayor resolución ≤ capture.maxMega |
| `src/ui/hooks/useLogout.ts` | 45 | Acción "Cerrar sesión" (PANT-09 y cabecera), maestro §7.8; RN-19 modificada (ADR 0008). QUÉ HACE: pide confirmación y cierra la sesión de usuario (borra tokens, conserva datos de campo y el verificador sin internet), detiene los ser |
| `src/ui/messages.ts` | 276 | Mensajes al usuario por código (español, Perú). Un código = un texto (maestro §19). REGLAS: nunca mostrar el texto de una excepción; el detalle técnico va a event_log; los mensajes no culpan al usuario y dicen qué hacer. Contexto §25.4/25. |
| `src/ui/strings.ts` | 639 | Textos de la interfaz (español, Perú). Regla R-12: TODOS los textos visibles aquí. Principio de diseño (contexto §25.6): "MENOS TEXTO, MÁS ESTADO Y ACCIÓN". Títulos cortos, etiquetas de estado y botones. Nada de explicaciones técnicas, fase |
| `src/ui/theme.ts` | 111 | Tema visual (colores, tipografía, espacios, sombras). Paleta inspirada en el logo de Agrícola Riachuelo (verde hoja, verde profundo, dorado del marco, azul del río). Reglas de campo (R-15, RNF-09): contraste alto, textos ≥ 16 pt, botones ≥ 48 |
| `src/ui/toast.ts` | 34 | Avisos breves en pantalla (toast) con sonido. QUÉ HACE: showToast(code \| texto) publica un aviso que <ToastHost/> muestra animado arriba de la pantalla. Los códigos se traducen con messageFor() (un código = un texto, maestro §19). |

### `src/config/`

`defaults.ts`: todos los parámetros (puertos, tiempos, umbrales, calidad JPEG 0,85, 12 MP máx., GPS, batería, espacio) con versión CFG y `checkConfigCoherence()`; `env.ts` lee la URL de la API y si se usa el backend simulado.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `src/config/defaults.ts` | 402 | Parámetros configurables de la app (versión CFG-11). QUÉ HACE: concentra TODOS los umbrales, tiempos, puertos e intervalos (regla R-08 del maestro: ningún número "suelto" en el código). Los servicios leen estos valores a través de src/ |
| `src/config/env.ts` | 36 | Lectura ÚNICA de las variables de entorno públicas (EXPO_PUBLIC_*). QUÉ HACE: Expo reemplaza `process.env.EXPO_PUBLIC_*` al compilar el bundle. Por eso se escriben con el nombre completo (no se pueden leer de forma dinámica). Ningún otro ar |
| `src/config/index.ts` | 70 | Configuración efectiva de la app (CONFIG) y versiones visibles. QUÉ HACE: parte de DEFAULT_CONFIG y aplica: 1) pairing.requireSameAppVersion = (ENV.appEnv === 'piloto') → regla RN-17. 2) (controlador) el perfil de calidad publicado por el |

### `__tests__/`

Pruebas Jest de las reglas puras y servicios (dominio, protocolo, calidad, segmentos, sincronización, plagas, borrados…). `npm run validate` = typecheck + lint + jest.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `__tests__/auth.test.ts` | 103 | @jest-environment node |
| `__tests__/captureUploader.test.ts` | 280 | Subida directa de una foto a Cloudinary (maestro v2.0 §15.7; Anexo E.8 en Jest). Dependencias falsas en memoria: no usan red, Expo ni Cloudinary. |
| `__tests__/config.test.ts` | 93 | Coherencia de la configuración CFG-8 (maestro §17: un cambio que rompa una regla no se acepta). |
| `__tests__/coverage.test.ts` | 208 | CFG-3: bloqueo por ciclo, lateral B desde el final, plan de la sesión y áreas no hechas con motivo (src/domain/coverage.ts). |
| `__tests__/deletions.test.ts` | 53 | Limpieza ordenada por el administrador (app 0.5.1, plataforma v1.3.1): códigos 410, forma de la respuesta y plan de borrado local. |
| `__tests__/domain.test.ts` | 237 | Pruebas del dominio: transiciones, estado de secuencia, repetición y reglas. (Equivalente en Jest de las pruebas de referencia del maestro, Anexo E.1, más las reglas de CFG-2.) |
| `__tests__/httpUpload.test.ts` | 112 | @jest-environment node |
| `__tests__/pests.test.ts` | 171 | @jest-environment node |
| `__tests__/pictureSize.test.ts` | 30 | Resolución de captura (Q-15): la mayor ≤ capture.maxMegapixels, preferiblemente 4:3. |
| `__tests__/protocol.test.ts` | 123 | Sobres del protocolo y esquemas zod (tarea T-01): aceptan ejemplos válidos y rechazan campos faltantes, tipos erróneos y v ≠ 1; el QR y la cabecera X-Capture-Meta también se validan. |
| `__tests__/quality.test.ts` | 59 | @jest-environment node |
| `__tests__/retry.test.ts` | 132 | Reintentos y clasificación de errores (maestro Anexo C.3; pruebas del Anexo E.7 en Jest). |
| `__tests__/roleChange.test.ts` | 77 | Elección de la función del celular después del login (ADR 0005). Prueba la decisión pura decideRoleChange (RN-15 en PANT-08) y la bandera roleChoicePending del store. |
| `__tests__/segments.test.ts` | 136 | v0.6.0 (ADR 0011): segmentos fijos por hilera (src/domain/segments.ts). Hilera estándar de la plataforma v1.4: 133 plantas (≈200 m), 10 segmentos, marcador al inicio de cada segmento («Hxx Sk inicio»: INICIO el primero, INTERMEDIO  |
| `__tests__/shortTest.test.ts` | 40 | Decisión de la prueba corta (§14.12) y motivo de la reprobación (PANT-14). |
| `__tests__/syncPayloads.test.ts` | 300 | Datos locales → contrato /api/v1 de la plataforma Django (Fase 4). Reglas del servidor real (api/v1/serializers.py): lateral de la pasada, motivos de calidad aceptados, referencias de incidencias que el servidor conoce, lotes d |
| `__tests__/syncPlanner.test.ts` | 174 | Orden y padres de la cola de sincronización (maestro v2.0 §15.5, Fase 4). |
| `__tests__/wsCodec.test.ts` | 108 | @jest-environment node |

### `tools/`

`tools/verificacion/`: pruebas con el código real de `src/` en Node (contrato contra Django local, red local real sobre TCP, simulado, migraciones con datos, prueba contra el piloto con `PERMITIR_PILOTO=1`). `tools/servidor/`: script histórico para crear segmentos en el servidor.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `tools/servidor/crear_segmentos_hilera_completa.py` | 23 | Se ejecuta en el VPS de la PLATAFORMA (no en la app), v0.4.5. Uso: cd /srv/riachuelo/deploy && docker compose exec -T web python manage.py shell < crear_segmentos_hilera_completa.py Crea, SOLO en las hileras  |
| `tools/verificacion/README.md` | 59 | Verificación de la Fase 4 (pruebas de contrato en Node) |
| `tools/verificacion/actualizacion.ts` | 70 | Un celular con la v0.2/v0.3 (solo migración 001 y datos) abre la v0.4.0. QUÉ HACE: arma una base con la migración 001, catálogos, una sesión y su cola; luego abre la base con el código real (src/storage/db.ts → migraciones |
| `tools/verificacion/integracion.ts` | 753 | Prueba de contrato de la app con la plataforma Django REAL (Fase 4). QUÉ HACE: corre en Node el código REAL de la app (src/: migraciones, repositorios, autenticación, catálogos y el motor de sincronización) contra una plataf |
| `tools/verificacion/mocks/expo-application.ts` | 3 | datos fijos del APK (solo pruebas de verificación). |
| `tools/verificacion/mocks/expo-constants.ts` | 3 | versión de la app para las pruebas de verificación. |
| `tools/verificacion/mocks/expo-crypto.ts` | 10 | expo-crypto con node:crypto (solo pruebas de verificación). |
| `tools/verificacion/mocks/expo-device.ts` | 4 | datos fijos del equipo (solo pruebas de verificación). |
| `tools/verificacion/mocks/expo-file-system.ts` | 80 | API nueva del SDK 57 (File, Directory, Paths) sobre el disco local; File.upload hace el multipart con fetch (como el nativo: los `parameters` y el archivo en `fieldName`). |
| `tools/verificacion/mocks/expo-keep-awake.ts` | 5 | registra las etiquetas activas (solo pruebas de verificación). |
| `tools/verificacion/mocks/expo-location.ts` | 11 | GPS falso para Node (pestService.watchMyPosition, ADR 0009). |
| `tools/verificacion/mocks/expo-network.ts` | 6 | estado de red controlable desde las pruebas (__state). |
| `tools/verificacion/mocks/expo-secure-store.ts` | 12 | SecureStore en memoria (solo pruebas de verificación). |
| `tools/verificacion/mocks/expo-sqlite.ts` | 66 | expo-sqlite sobre node:sqlite (Node 22.5+) para correr la app en Node. Misma API asíncrona que usa la app (runAsync, getFirstAsync, getAllAsync, execAsync, withExclusiveTransactionAsync, prepareAsync). SOLO para las pr |
| `tools/verificacion/mocks/react-native.ts` | 5 | lo mínimo de react-native que usan los servicios fuera de la UI. |
| `tools/verificacion/piloto.ts` | 413 | Prueba corta de la app contra el PILOTO real (Django + Cloudinary reales): CP-38 y CP-41. QUÉ HACE: corre en Node el código REAL de la app (como integracion.ts) con una cuenta de operador aprobada y crea UNA sesión de prueba pequ |
| `tools/verificacion/redlocal.ts` | 224 | Red local REAL entre controlador y cámara, en Node (maestro §14.2, §14.8, §14.11). QUÉ HACE: usa el módulo `net` de Node en lugar de react-native-tcp-socket (misma forma: createServer, listen, socket.on('data'), write, destroy) |
| `tools/verificacion/simulado.ts` | 229 | El motor de sincronización con el BACKEND SIMULADO de la app (modo demostración). QUÉ HACE: corre en Node el código real de src/ con EXPO_PUBLIC_USE_MOCK_API=1 (Django y Cloudinary simulados en memoria, src/api/mock) e inyecta  |
| `tools/verificacion/tsconfig.json` |  |  |

### `docs/`

`CAMBIOS.md` (bitácora de versiones de la app), `adr/` (0001 a 0012), `evidencias/` (pruebas de la Fase 4), `referencia/` (contexto de errores y configuración, Maestro App v1.0 histórico), `img/` y el resumen técnico en PDF. Copias en `05-repositorios-de-codigo/app-movil/` de este repo.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `docs/CAMBIOS.md` | 446 | Informe de avance — App móvil v0.6.1 (formato maestro §23.2) |
| `docs/Resumen_Tecnico_App_Riachuelo.pdf` |  |  |
| `docs/adr/0001-sdk57-expo-router.md` | 18 | ADR 0001 — Expo SDK 57, Expo Router y carpeta `app/` en la raíz |
| `docs/adr/0002-cfg2-modos-y-estabilidad.md` | 21 | ADR 0002 — CFG-2: solo MANUAL/AUTOMÁTICO, marcador manual y disparo con cámara estable |
| `docs/adr/0003-red-local.md` | 15 | ADR 0003 — Red local: TCP propio en el APK, simulador en Expo Go |
| `docs/adr/0004-ciclos-y-cobertura.md` | 37 | ADR 0004 — CFG-3: ciclo de monitoreo, bloqueo de lo terminado, lateral B desde el final y áreas no hechas |
| `docs/adr/0005-funcion-despues-del-login.md` | 35 | ADR 0005 — Elegir la función del celular después de cada login y RN-15 sin sincronización |
| `docs/adr/0006-fase4-sincronizacion.md` | 78 | ADR 0006 — Fase 4: sincronización con la plataforma Django y subida directa a Cloudinary |
| `docs/adr/0007-catalogos-codigos-no-unicos.md` | 24 | ADR 0007 — Catálogos con códigos repetidos entre hileras (migración 004) y registro de migraciones |
| `docs/adr/0008-cerrar-sesion-con-monitoreo-abierto.md` | 24 | ADR 0008 — Cerrar la sesión de usuario con una sesión de monitoreo abierta, y volver a elegir función al cerrar |
| `docs/adr/0009-ubicar-plaga.md` | 51 | ADR 0009 — «Ubicar plaga»: mapa satelital, «Cómo llegar» y el especialista en la app |
| `docs/adr/0010-limpieza-ordenada-por-el-administrador.md` | 24 | ADR 0010 — Limpieza ordenada por el administrador (app 0.5.1, CFG-9) |
| `docs/adr/0011-segmentos-fijos-por-hilera.md` | 34 | ADR 0011 — Segmentos fijos por hilera y sentido de la pasada (app 0.6.0, CFG-10) |
| `docs/adr/0012-limite-de-repeticiones.md` | 39 | ADR 0012 — Límite de repeticiones por cámara y bloqueo de la captura (app 0.6.1, CFG-11) |
| `docs/evidencias/README.md` | 6 | Evidencias de prueba |
| `docs/evidencias/fase4_integracion.md` | 116 | Evidencia — Fase 4: sincronización con la plataforma Django (v0.4.0) |
| `docs/img/arquitectura_produccion.html` | 420 |  |
| `docs/img/arquitectura_produccion.png` |  |  |
| `docs/referencia/CONTEXTO_ERRORES_Y_CONFIGURACION.txt` | 1344 |  |
| `docs/referencia/MAESTRO_APP_MOVIL_v1.0.pdf` |  |  |

### `assets/`

Íconos, splash, logo, sonidos y fotos de muestra (nítida, borrosa, oscura) para el modo prueba de calidad.

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `assets/android-icon-background.png` |  |  |
| `assets/android-icon-foreground.png` |  |  |
| `assets/android-icon-monochrome.png` |  |  |
| `assets/favicon.png` |  |  |
| `assets/icon.png` |  |  |
| `assets/images/logo-riachuelo.png` |  |  |
| `assets/sample/muestra_borrosa.jpg` |  |  |
| `assets/sample/muestra_oscura.jpg` |  |  |
| `assets/sample/muestra_util.jpg` |  |  |
| `assets/sounds/confirm.wav` |  |  |
| `assets/sounds/error.wav` |  |  |
| `assets/sounds/shutter.wav` |  |  |
| `assets/sounds/success.wav` |  |  |
| `assets/sounds/tap.wav` |  |  |
| `assets/sounds/toggle.wav` |  |  |
| `assets/splash-icon.png` |  |  |

### `.claude/`

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `.claude/settings.json` |  |  |

### `Claude outputs/`

| Archivo | Líneas | Qué contiene (tomado de su comentario de cabecera) |
|---|---:|---|
| `Claude outputs/HANDOFF_CATALOGOS_WEB.md` | 192 | Traspaso: gestión de catálogos del campo en la web (lotes, hileras, segmentos y marcadores) |
| `Claude outputs/arquitectura_produccion.png` |  |  |
