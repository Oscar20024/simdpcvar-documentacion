# Glosario rápido

Glosario completo del informe: `01-documentacion-principal/00-glosario.md`. Glosario del sistema: capítulo «Glosario» de `02-documentacion-del-sistema-y-pruebas/…`.

| Término | Significado en este proyecto |
|---|---|
| Fundo | Terreno agrícola de Agrícola Riachuelo (La Tinguiña, Ica) |
| Lote | División del fundo (p. ej. SWG1 «Lote 1 (Piscina)», SWG2 «Lote 2 (Maíz)», SWG5 «Lote 5 (Triángulo)») |
| Hilera | Fila de plantas de vid dentro de un lote (código p. ej. `SWG1-H05`), con largo y número de plantas |
| Lateral (lado) | Cara de la hilera: **LATERAL_A** (ida) y **LATERAL_B** (vuelta) |
| Segmento | Tramo fijo de ≈20 m de una hilera; cada hilera tiene **10**. Equivale a la «zona» del informe |
| Marcador | Poste físico al inicio de cada segmento; sirve de referencia cuando el GPS es impreciso |
| Soporte tipo T | Poste con barra que lleva las dos cámaras a la misma altura apuntando a la hilera |
| Controlador | Celular que crea la sesión, ordena capturas, guarda la base local (SQLite) con el GPS y sincroniza |
| Cámara 1 / Cámara 2 | Celulares que toman las fotos y las envían al controlador por Wi-Fi local |
| Función del celular | Controlador, Cámara 1 o Cámara 2; se elige después de cada login (PANT-08) |
| Sesión (de monitoreo) | Una salida al campo con los 3 celulares vinculados |
| Vinculación | Las cámaras escanean el QR del controlador para unirse a la sesión |
| Prueba corta | Comprobación inicial: cada cámara toma, evalúa y envía una foto en ≤ 20 s |
| Pasada | Recorrido de un lateral de una hilera |
| Secuencia | Una orden de captura: una foto por cámara del mismo punto + GPS del controlador |
| Captura | Una foto (de una cámara) con su calidad y metadatos |
| Calidad técnica | Evaluación en el celular de exposición y nitidez (y quietud en automático) |
| Modo MANUAL / AUTOMÁTICO | Captura con botón / por intervalo (1–5 s) solo con el celular quieto |
| Sincronizar | Enviar sesiones, pasadas, secuencias, incidencias y fotos al servidor cuando hay internet |
| Ticket (de subida) | Permiso firmado por Django, de un solo uso, para subir una foto directo a Cloudinary |
| Worker de IA | Proceso del servidor que analiza fotos (`worker_ia`), envía avisos y vacía la papelera |
| Indicio | Lo que la IA sugiere (cajas con clase y confianza); no es diagnóstico |
| Caja (bounding box) | Rectángulo que marca dónde la IA cree ver una plaga, con su confianza (0–1) |
| Triage | Regla que clasifica cada foto: descartada por IA, pendiente de revisión o confirmada por IA |
| Caso | Unidad de revisión del especialista (una por lugar/foto), con estado e historial |
| Bandeja | Lista de casos de la web (se refresca cada 15 s; 6 consultas) |
| Plano de hileras | Vista esquemática del lote por hilera y segmento con los casos |
| Ubicar plaga | Función de la app que lista y muestra en mapa las alertas y abre Google Maps «Cómo llegar» |
| Destinatario | Persona o grupo de Telegram que recibe avisos de ciertos lotes |
| Papelera | Fotos eliminadas que se pueden restaurar durante 30 días |
| Limpieza | Borrado en bloque ordenado por el administrador; no se deshace |
| Chanchito blanco | *Planococcus ficus*, cochinilla de la vid; plaga principal del alcance |
| Melaza / fumagina | Secreción azucarada (gotas ámbar) y hongo negro que crece sobre ella; señal visible de la plaga |
| Piloto | Despliegue real en el fundo con el servidor de producción |
| ADR | Registro de decisión de arquitectura |
| SC | Solicitud de cambio (formato FGPR-410-06) |
| Maestro | Archivo maestro: especificación con la que se construyó la web o la app |
