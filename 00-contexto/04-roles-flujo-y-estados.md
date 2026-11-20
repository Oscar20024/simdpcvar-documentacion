# Roles, flujo de trabajo y estados

## Roles y permisos

Cada cuenta tiene uno o más roles y los permisos se suman. El servidor comprueba el permiso en cada acción (decorador `@web_view("permiso")` en la web; permisos DRF en la API).

| Función | Administrador | Especialista fitosanitario | Supervisor | Operador de campo |
|---|:-:|:-:|:-:|:-:|
| Web: panel, casos, mapa, plano, sesiones, reportes, avisos, ayuda | ✓ | ✓ | ✓ | |
| Web: decidir y corregir casos; abrir un caso a mano | | ✓ | | |
| Web: estado de la IA (reintentar análisis: solo administrador) | ✓ | ✓ | | |
| Web: catálogos y dibujo en el mapa | ✓ | | ✓ | |
| Web: usuarios, celulares, destinatarios, auditoría, papelera, limpieza | ✓ | | | |
| App: monitoreo (controlador y cámaras) y sincronización | ✓ | | | ✓ |
| App: «Ubicar plaga» | ✓ | ✓ | | ✓ |

- **Cuenta de la web** (especialista, supervisor y/o administrador): solo la crea el administrador en «Usuarios → Nueva cuenta → Plataforma web»; nace con contraseña temporal que se cambia al primer ingreso. No hay registro público.
- **Cuenta de la app** (operador de campo, rol único): la persona la pide desde la app («Crear cuenta») y el administrador la **aprueba sin elegir rol** (SC-08), o la crea con «Nueva cuenta → App móvil».
- El especialista entra a la app **solo** para «Ubicar plaga»; las rutas de campo le responden `403 ROLE_NOT_ALLOWED`.
- Cuentas de evaluación del piloto: `testeador@gmail.com` (web, con los tres roles) y `testeador2@gmail.com` (app, operador). Contraseña: [omitido] — pedirla al equipo.

Interesados del negocio (STK): Gerencia General (patrocinador), Jefe de fundo (recibe avisos, coordina la respuesta), Especialista fitosanitario (Product Owner y validador), Supervisor de zona (seguimiento y avisos de su lote), Operadores de monitoreo (captura). Matrices de poder/interés e influencia/impacto en capítulo 5 y anexos 3–5.

## Jornada de campo paso a paso

1. **Preparar.** El administrador mantiene lotes, hileras (10 segmentos de ≈20 m) y destinatarios de avisos. El controlador descarga los catálogos con internet («Catálogos → Actualizar»). Los 3 celulares en la misma Wi-Fi local (el router no debe aislar clientes).
2. **Ingresar y elegir función.** Cada celular inicia sesión (el primer ingreso necesita internet; luego hasta 7 días sin internet) y elige su función: Controlador, Cámara 1 o Cámara 2 (PANT-08, después de **cada** login).
3. **Sesión y vinculación.** El controlador crea la sesión (modo MANUAL o AUTOMÁTICO) y muestra un QR; cada cámara lo escanea. **Prueba corta:** cada cámara toma, evalúa y envía una foto; aprueba si todo llega en ≤ 20 s. Sin las dos cámaras conectadas y la prueba aprobada no se inicia una pasada (RF11).
4. **Pasada.** Lote → hilera → lateral (A ida, B vuelta) → segmento → marcador. Cada orden de captura es una **secuencia**: una foto por cámara del mismo punto + GPS del controlador. En automático solo dispara con el celular quieto. En cada marcador se cambia de segmento.
5. **Calidad.** Cada cámara evalúa nitidez y exposición (y movimiento) en ≤ 2 s. Si falla, se repite solo esa cámara (máx. 2 veces; luego queda registrada y se sigue).
6. **Cierre y sincronización.** Se cierra la pasada y la sesión. Con internet, el controlador envía todo en orden y sin duplicar; sube cada foto a Cloudinary con ticket; las cámaras borran su copia solo cuando el controlador confirma.
7. **Análisis de IA** en el servidor (triage, abajo). Una sola decisión por lugar, considerando las dos cámaras (ADR-W-009).
8. **Decisión del especialista** en la web: revisa foto, cajas, la otra cámara y el lugar; decide con observación; puede corregir con motivo (historial).
9. **Aviso y acción.** Si se confirma, Telegram avisa a los destinatarios del lote. En la app, «Ubicar plaga» guía a pie hasta la planta (Google Maps).
10. **Seguimiento.** Panel, mapa satelital, plano de hileras, sesiones, reportes CSV, auditoría.

## Triage de la IA (vigente)

| Lo que ve la IA (cajas con confianza ≥ umbral del modelo 0,40) | Resultado |
|---|---|
| Ninguna caja ≥ 0,40, o menos de 2 cajas ≥ 0,40 | **Descartada por la IA**: sin caso; queda en «Descartadas por la IA» y se puede rescatar a mano |
| 2 o más cajas ≥ 0,70 | **Confirmado por IA** + aviso marcado «falta la revisión del especialista» |
| 3 o más cajas ≥ 0,50 | **Confirmado por IA** + aviso marcado «falta la revisión del especialista» |
| Cualquier otro caso | **Pendiente de revisión**: decide el especialista (compromiso: ≤ 24 h de servicio) |

Los casos ya decididos por el especialista nunca cambian al reaplicar el triage (`aplicar_triage`).

## Estados de un caso (`review_cases`)

| Estado | Significado | ¿Avisa por Telegram? |
|---|---|:-:|
| `PENDIENTE_REVISION` | La IA vio un posible indicio; falta la decisión del especialista | No |
| `CONFIRMADO_POR_IA` | La IA lo confirmó sola (regla de arriba); falta ratificación | Sí (marcado) |
| `CONFIRMADO_POR_ESPECIALISTA` | El especialista confirmó la plaga o el daño | Sí |
| `POSIBLE_PLAGA` | No es seguro; alguien debe ir a mirar (se ve en «Ubicar plaga») | No |
| `DESCARTADO` | No era plaga (reflejo, tallo, hoja seca…) | No |
| `EVIDENCIA_INSUFICIENTE` | La foto no permite decidir; hay que volver a fotografiar | No |
| `DESCARTADO_POR_IA` | Estado de la foto/caso descartado automáticamente por el triage | No |

En la web los colores indican la etapa de revisión, nunca la gravedad, y siempre van con texto (W-12).

## Otros estados útiles

- **Cuenta:** `PENDIENTE_APROBACION` → activa; bloqueo por intentos; contraseña temporal (`mustChangePassword`).
- **Calidad de foto (app):** `UTILIZABLE`, `REPETIR_EXPOSICION`, `REPETIR_NITIDEZ`, `PENDIENTE_REVISION_TECNICA`. El servidor crea tarea de IA para `UTILIZABLE` y `PENDIENTE_REVISION_TECNICA`.
- **Captura en el servidor:** `SINCRONIZADO` al confirmarse la subida.
- **Sesión:** una sesión `CLOSED` no se reabre; la sesión queda `SYNCED` en la app cuando todo se envió.
- **Foto en la papelera:** se puede restaurar durante 30 días; «Borrar para siempre» elimina servidor, nube y celulares (ADR-W-012).

Diagramas de estado y de actividad: capítulo 8 y documento 02 («Estados de un caso»).
