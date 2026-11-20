# Reglas, límites y restricciones

## Reglas de dominio (no negociables)

- La IA **sugiere indicios**; no diagnostica ni reemplaza al especialista (RN-06, W-11). Esto vale también en los textos: nunca escribir «la IA diagnosticó».
- Solo el especialista confirma con observación; la IA solo puede emitir una alerta temprana «Confirmado por IA» (SC-03).
- Ningún caso pendiente o descartado genera aviso (RN-02).
- Ninguna foto se borra del celular antes de confirmar su sincronización (RN-05). Nada se borra de una sesión abierta.
- Las decisiones de revisión nunca se editan ni se borran: una corrección crea otra fila con motivo.
- La auditoría es de **solo inserción** (trigger en la base).
- Catálogos: **desactivar, nunca borrar**; los IDs no cambian (la sincronización sigue aceptando IDs desactivados).
- La web no crea, edita ni borra datos de campo (RN-W08).
- Los 3 celulares deben tener **la misma versión** de la app.

## Reglas de construcción de la web (Maestro Web, W-01 a W-24; las más usadas)

| Regla | Qué exige |
|---|---|
| W-01 | Sin CDN, sin npm, sin frameworks de JavaScript. HTMX y Leaflet copiados en `web/static/web/vendor/` |
| W-07 | Toda vista con `@web_view("permiso")` |
| W-10 | No guardar URLs de fotos; firmarlas al mostrar |
| W-11 | La IA «sugiere indicios», nunca diagnostica |
| W-12 | Los colores indican estado de revisión, nunca gravedad |
| W-16 | Números en atributos HTML y CSS sin localizar (filtros `pctnum` y `stringformat`), porque es-PE usa coma decimal |
| W-17 | Sin JavaScript en línea (`<script>` sin `src`, `onclick=`); todo en `app.js`, `caso.js` o `mapa.js` (lo verifica `web/tests/test_extras.py`) |
| W-21 | Dependencias nuevas justificadas en el reporte de avance |
| W-22 | `sembrar_demo` prohibido en el piloto |
| W-23 | Accesibilidad y diseño responsive |
| W-24 | Reporte de avance (sección 23.2 del maestro) en `docs/REPORTE_AVANCE.md` al terminar cada tarea |
| — | La bandeja hace **exactamente 6 consultas**: no agregar consultas en `base.html` ni en context processors |
| — | Textos para el usuario en `web/messages.py`; las tablas solo cambian con migraciones de Django |
| — | El servicio vuelve a comprobar las reglas críticas aunque la vista ya lo haga (RN-W01) |

Validación obligatoria: `python manage.py check`, `python manage.py makemigrations --check --dry-run`, `python manage.py test`.

## Reglas de construcción de la app (Maestro App Móvil v2.0 y `AGENTS.md` de la app)

- La app habla solo con Django por HTTPS; nunca con Supabase ni con secretos de Cloudinary (R-18, R-21). Solo `src/api/httpClient.ts` arma URLs de la API y solo `src/api/cloudinaryUpload.ts` habla con Cloudinary. Únicas otras salidas: teselas de Esri y el enlace de Google Maps de «Ubicar plaga».
- Contrato `/api/v1`: agregar campos o rutas sí; quitar o renombrar no (hay celulares con versiones anteriores en campo).
- Migraciones SQLite: nunca modificar una publicada; agregar la siguiente (hoy 001–005).
- Parámetros solo en `src/config/defaults.ts` (con versión CFG y `checkConfigCoherence()`); textos solo en `src/ui/strings.ts` y `messages.ts`; cada archivo empieza con un comentario «QUÉ HACE».
- Nunca registrar contraseñas, tokens ni el verificador sin internet (R-10).
- Módulos nativos que no existen en Expo Go se cargan con `require()` diferido (ver `src/local-network/factory.ts`).
- Instalar paquetes con `npx expo install`; no editar `android/` ni `ios/` a mano.
- Validación: `npm run validate` (typecheck + lint + jest) y `tools/verificacion` si se toca la sincronización o el contrato.

## Parámetros y límites técnicos

| Tema | Valor |
|---|---|
| Foto | Mayor resolución ≤ 12 MP (4:3 preferente), JPEG calidad 0,85, sin EXIF de GPS; ≤ 10 MB (límite de Cloudinary Free) |
| Calidad | ≤ 2 s por foto; repetición máx. 2 por cámara |
| Red local | Control 8765, fotos 8766; latido 2 s; enlace perdido a los 6 s; prueba corta ≤ 20 s |
| GPS | Lectura cada 1 s; válida si ≤ 10 s; aviso si precisión > 15 m; precisión real 5–30 m |
| Login sin internet | Hasta 7 días desde el último login con internet; 5 intentos y bloqueo de 15 min; verificador PBKDF2 |
| Tokens | Acceso 15 min; renovación 14 días con rotación |
| Sincronización | Lotes de ≤ 200 secuencias/incidencias; automática solo con Wi-Fi y sin sesión abierta |
| Batería / espacio | Aviso bajo 15 % de batería (no bloquea); espacio: aviso < 3 GB, no inicia < 2 GB, pausa < 1 GB |
| IA | Mosaicos de 1280 px con 20 % de solape, `imgsz` 640, `conf` 0,4, `iou` 0,45 |
| Rendimiento | API p95 ≤ 1,5 s (RNF04); medido: bandeja 45–61 ms, caso 103 ms, mapa 268 ms con 100 000 fotos sintéticas |
| Supabase Free | 500 MB; el proyecto se pausa si no se usa; ~1 KB por foto en la base |
| Cloudinary Free | 25 créditos al mes (~1 000 fotos ≈ 10 créditos); 10 MB por foto |
| VPS | 1 vCPU, 3,8 GB RAM, 48 GB; compartido con otro proyecto personal (no tocarlo) |

## Limitaciones declaradas en el informe (1.13)

- Resultados válidos solo para Agrícola Riachuelo, Ica, 2026; no se generalizan automáticamente.
- No detecta plagas fuera del alcance visual (bajo la corteza, dentro del racimo).
- La calidad depende de distancia, sombras, movimiento, hojas que tapan, fondo e iluminación.
- El GPS del celular no basta para una planta; se combina con lote, hilera, zona/segmento, lado y marcadores.
- La sincronización y el procesamiento central requieren buena conexión a internet fuera del campo.
- El modelo depende de la cantidad y diversidad de imágenes; habrá falsos positivos y negativos.
- Es un prototipo en piloto: no reemplaza al personal agronómico.

## Seguridad y privacidad

- Secretos solo en el servidor (`.env` del VPS con permisos 600); `.env.example` y `env.piloto.plantilla` sin valores.
- RLS sin políticas y REVOKE a `anon`/`authenticated` en Supabase; solo Django se conecta (session pooler).
- Fotos privadas en Cloudinary (`authenticated`), URLs firmadas y temporales.
- Web: CSP sin JavaScript en línea, cookie CSRF HttpOnly, HSTS, cabeceras de seguridad (`CabecerasSeguridadMiddleware`), bloqueo por intentos, auditoría con contexto.
- Celulares revocables desde la web; un celular revocado deja de sincronizar.
- Acciones con efectos reales en el piloto (documento 02): confirmar un caso envía Telegram real; «Borrar para siempre» y la limpieza no se deshacen; revocar un celular no se deshace. Lección del 20/10/2026: se borraron 246 fotos de campo con la limpieza; **no usar la limpieza antes de tener un modelo entrenado** (para silenciar la IA, se desactiva el modelo).
