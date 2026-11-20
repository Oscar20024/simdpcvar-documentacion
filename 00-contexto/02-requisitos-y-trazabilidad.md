# Requisitos, casos de uso, historias y solicitudes de cambio

Los requerimientos **oficiales** son los del informe (Tablas 27, 28 y 29 del capítulo 8 y Anexo 9). **No se reescriben.** Cuando el sistema construido difiere, la diferencia se justifica con una **solicitud de cambio (SC)**. La tabla «cómo lo cumple el sistema hoy» de cada requisito está en `02-documentacion-del-sistema-y-pruebas/documentacion-del-sistema-y-guia-de-pruebas.md` (capítulos «Requerimientos funcionales» y «No funcionales y reglas de negocio»). La trazabilidad completa está en el Anexo 11 (AP-11) y en el capítulo «Matriz de trazabilidad» del documento 02.

## Requerimientos funcionales (Tabla 27)

| Código | Requerimiento (resumen fiel) | Estado en el sistema | SC |
|---|---|---|---|
| RF01 | Capturar fotos bilaterales de cada planta con dispositivos móviles | Cumple con cambio: dos cámaras toman el mismo punto (doble evidencia); la planta se cubre por sus dos lados con las pasadas lateral A y lateral B | SC-04 |
| RF02 | Validar automáticamente la calidad de cada foto (nitidez, exposición, movimiento) | Cumple: evaluación en el celular en < 2 s; en automático solo dispara con el celular quieto | — |
| RF03 | Repetir la captura del lado cuya foto no fue válida | Cumple (aclarado): se repite solo la cámara que falló, máximo 2 veces | SC-05 |
| RF04 | Almacenar localmente las fotos sin conexión | Cumple: SQLite y almacenamiento privado del controlador | — |
| RF05 | Sincronizar automáticamente al restablecerse la conexión | Cumple: ordenado, idempotente y reanudable; automático con Wi-Fi sin sesión abierta | — |
| RF06 | Asociar cada foto a lote, hilera, zona, lado, marcador, secuencia y GPS | Cumple (aclarado): «zona» = segmento; GPS solo si hay lectura válida | SC-06, SC-09 |
| RF07 | Analizar las imágenes sincronizadas con IA y señalar posibles indicios | Cumple: YOLO en el servidor, cajas con clase y confianza, triage | — |
| RF08 | Que el especialista visualice los indicios en la plataforma | Cumple: bandeja, caso con foto, cajas, zoom, la otra cámara y mapa | — |
| RF09 | Que el especialista confirme o descarte | Cumple con cambio: Confirmado · Posible plaga · Descartado · Evidencia insuficiente; la IA puede confirmar sola como alerta temprana | SC-03 |
| RF10 | Notificación automática vía **WhatsApp** al jefe de fundo y supervisor tras la confirmación | Cumple con cambio: **Telegram** (no WhatsApp) con foto, lugar y botones | SC-02 |
| RF11 | Exigir la vinculación de los tres dispositivos antes de iniciar el recorrido | Cumple: sin las dos cámaras conectadas y la prueba corta aprobada no se inicia una pasada | — |

## Requerimientos no funcionales (Tabla 28)

| Código | Requerimiento | Cómo se atiende |
|---|---|---|
| RNF01 | Operar con servicios en la nube, sin servidores físicos en el fundo | VPS, Supabase, Cloudinary y Telegram; en el fundo solo celulares y router (SC-07 ajusta la Tabla 30) |
| RNF02 | Validación de calidad ≤ 2 s por foto | Medida en el celular; el modo prueba muestra tiempo y métricas (SLA-03) |
| RNF03 | Iniciar un recorrido en menos de 3 pasos | La sesión se crea con 2 toques; vinculación y prueba corta se aclaran como preparación (SC-06) |
| RNF04 | Soportar 50 000+ plantas con API < 1,5 s | Medido con 100 000 fotos sintéticas: bandeja 45–61 ms (6 consultas), caso 103 ms, mapa 268 ms (SLA-02) |
| RNF05 | Autenticación y control de roles | Login obligatorio, permiso verificado en el servidor, bloqueo por intentos, auditoría |
| RNF06 | Modo sin conexión durante la captura y sincronización automática | Igual a RF04/RF05; ingreso sin internet hasta 7 días |
| RNF07 | Custodia local en almacenamiento privado y seguro hasta sincronizar | Fotos en el almacenamiento privado de la app, metadatos en SQLite privada, tokens en SecureStore (aclarado en SC-06) |

## Reglas de negocio (Tabla 29)

| Código | Regla | Cómo se cumple |
|---|---|---|
| RN-01 | Solo el **especialista fitosanitario** autenticado confirma un hallazgo o registra la observación | Permiso en el servidor; la IA puede marcar «Confirmado por IA» como alerta temprana sin observación (SC-03) |
| RN-02 | Ningún hallazgo «pendiente» o «descartado» genera observación ni notificación | Solo los estados confirmados avisan |
| RN-03 | Fotos con metadatos obligatorios (lote, hilera, zona, lado, marcador, secuencia; GPS si existe) | La sincronización rechaza datos incompletos |
| RN-04 | Repetición por calidad solo del lado afectado | Se repite solo la cámara que falló (máx. 2) (SC-05) |
| RN-05 | Las fotos sin conexión no se eliminan hasta confirmar su sincronización | Las cámaras borran su copia solo cuando el controlador confirma |
| RN-06 | El análisis de IA es «indicio sugerido», no vinculante | La IA siempre sugiere; el especialista ratifica (SC-03) |

## Casos de uso (capítulo 8)

CU1 Capturar fotos bilaterales · CU2 Validar calidad de imagen · CU3 Sincronizar fotografías · CU4 Analizar imágenes con IA · CU5 Revisar indicios señalados · CU6 Confirmar o descartar hallazgo · CU7 Registrar observación · CU8 Recibir alertas de hallazgos confirmados · CU9 Consultar dashboard y mapa de hallazgos.

## Historias de usuario

- **Del informe (Anexo 52):** HU01 Prototipo físico de captura bilateral · HU02 Validación automática de calidad y almacenamiento offline · HU03 Visualización de hallazgos en mapa e IA · HU04 Notificaciones por mensajería oficial · HU05 Administración de la plataforma.
- **Complementarias (sistema actual, documento 02):** HU06 Sincronizar al volver la señal · HU07 Vincular los tres celulares · HU08 Repetir solo la cámara que falló · HU09 Catálogos del fundo · HU10 Decidir casos en la bandeja · HU11 Rescatar una foto descartada por la IA · HU12 Llegar hasta la plaga · HU13 Seguimiento del fundo · HU14 Auditoría · HU15 Papelera y limpieza de fotos · HU16 Destinatarios de Telegram · HU17 Aprobar cuentas de la app sin elegir rol.

## Solicitudes de cambio

Formato FGPR-410-06. Texto completo en `01-documentacion-principal/anexos/anexo-37-ap-39-solicitud-de-cambio.md`; registro en el Anexo 42 (AP-46).

| N.º | Cambio | Tipo | Afecta |
|---|---|---|---|
| SC-01 | Cambio de la arquitectura inicial (Spring Boot + Angular + RabbitMQ + PostgreSQL en VPS + Amazon S3 + microservicio de IA) a **Django + Supabase (PostgreSQL administrado) + Cloudinary**, con la IA integrada al backend | Actualización | Arquitectura, EDT 3.3 y 4.1 |
| SC-02 | Avisos por **Telegram** en lugar de WhatsApp | Acción correctiva · Actualización | RF10, CU8, HU04, Tabla 30 |
| SC-03 | Confirmación automática por la IA y estado «Posible plaga» | Actualización | RF09, RF10, RN-01, RN-02, RN-06 |
| SC-04 | Cobertura bilateral con laterales A y B | Actualización | RF01, CU1, HU01 |
| SC-05 | Límite de repeticiones por calidad | Acción preventiva | RF03, RN-04 |
| SC-06 | Aclaraciones de interpretación | Actualización (aclaración) | RF06, RNF03, RNF07 |
| SC-07 | Ajustes de la arquitectura tecnológica: la Tabla 30 conserva elementos que no se implementaron tras la SC-01 (Supabase Auth, PostGIS, microservicio FastAPI, «React Native P2P», iPhone) | Actualización | Tabla 30, RNF01, SC-01 |
| SC-08 | Aprobar cuentas de la app sin elegir rol | Reparación de defecto | RNF05, HU05, HU17 |
| SC-09 | Funciones agregadas al alcance: «Ubicar plaga» en la app (mapa satelital y «Cómo llegar»), papelera de fotos (30 días) y limpieza controlada, 10 segmentos fijos por hilera, ayuda en cada pantalla y manuales PDF, cabeceras de seguridad y auditoría con contexto | Actualización | RF06, RF10, CU8, CU9 |

Estado: SC-02 a SC-09 registradas el 14/11/2026, **pendientes de revisión del Comité de Control de Cambios** (jefe de fundo y Project Manager). Los textos descriptivos del informe se actualizaron citando la SC correspondiente; las tablas de requerimientos se dejaron intactas.

## Cómo se prueba

Casos de prueba de la web (CP-W…) y de la app (CP-A…) con pasos y resultado esperado: capítulos «Guía de pruebas» del documento 02. Inspección de calidad: Anexo 38. Casos de prueba del informe: capítulo 12 y Anexo 73.
