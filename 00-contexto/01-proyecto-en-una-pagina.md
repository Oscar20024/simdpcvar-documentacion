# El proyecto en una página

## Identificación

| Campo | Valor |
|---|---|
| Nombre | Sistema inteligente para el monitoreo y detección de plagas en cultivos de vid en Agrícola Riachuelo |
| Siglas | **SIMDPCVAR** |
| Cliente | Agrícola Riachuelo S.A.C. — fundo de vid (uva de mesa para agroexportación) en La Tinguiña, Ica, Perú |
| Contexto académico | Curso *Curso Integrador II: Sistemas*, Facultad de Ingeniería de Sistemas e Informática, Universidad Tecnológica del Perú (Ica), 2026 |
| Tipo de trabajo | Proyecto de investigación aplicada **y** desarrollo de un prototipo en piloto real. El informe combina tesis (capítulos I–III), análisis empresarial (IV), gestión de proyecto PMBOK (V–VI), Scrum (VII), ingeniería de software (VIII–XIII) y gestión de servicios ITIL (XIV) |
| Equipo | Oscar Antonio Gutiérrez Rojas (Project Manager), Grace Lucia Peña Reyes, Oscar Manuel Guerrero Figari, María Fernanda Zuñiga Matos |
| Docente | Jean Pierre Pacheco Tamariz |
| Interesados clave | Gerencia General (STK-01, patrocinador), Jefe de fundo (STK-02), Especialista fitosanitario (STK-03, Product Owner y validador técnico), Supervisor de zona (STK-04), Operadores de monitoreo (STK-05) |
| Producción (piloto) | <https://monitoreo.agricolariachuelo.org> |

## Problema (AS-IS)

El monitoreo fitosanitario de la vid se hace **a ojo**: entre 10 y 20 operadores recorren las hileras y anotan en papel o en un Excel grande la presencia de plagas, sobre todo el **chanchito blanco** (*Planococcus ficus*), que succiona savia, deja melaza y fumagina y transmite virus del enrollamiento. Consecuencias: no queda evidencia (foto, lugar exacto, hora), el GPS del celular no distingue hileras cercanas, la consolidación del Excel es lenta, el especialista valida tarde, no hay alertas y la trazabilidad para auditorías (SENASA, GlobalG.A.P.) es pobre. En el campo casi no hay internet.

## Pregunta, objetivo e hipótesis

- **Problema general:** ¿en qué medida un sistema tecnológico de monitoreo fitosanitario mejora el desempeño del proceso de monitoreo en los cultivos de vid de Agrícola Riachuelo S.A.C., Ica, 2026?
- **Objetivo general:** determinar en qué medida la implementación del sistema mejora ese desempeño.
- **Objetivos/hipótesis específicos:** mejora la **eficiencia operativa**, la **efectividad del monitoreo** y la **trazabilidad y seguimiento** de los hallazgos.
- **Diseño:** preexperimental de un solo grupo, **O₁ – X – O₂** (pretest, intervención tecnológica, postest). Unidad de análisis: segmento de monitoreo de 25 plantas. Variables: VI = sistema tecnológico de monitoreo; VD = desempeño del proceso (3 dimensiones). Ver capítulos I–III y anexos 43–50.

## Solución (TO-BE)

1. **Campo, sin internet.** Tres celulares Android en un soporte tipo T: un **controlador** (crea la sesión, ordena las capturas, guarda todo en SQLite con el GPS) y dos **cámaras** que fotografían el mismo punto del mismo lateral (doble evidencia). Se comunican por una red Wi-Fi local sin internet (router o hotspot). Cada hilera se recorre por el **lateral A** (ida) y el **lateral B** (vuelta). Cada foto pasa un **control de calidad** en el celular (nitidez, exposición, movimiento) en ≤ 2 s y se repite solo la cámara que falló (máx. 2 veces).
2. **Sincronización.** Con internet, el controlador envía sesiones, pasadas, secuencias e incidencias a la **API Django `/api/v1`** y sube cada foto **directo a Cloudinary** con un ticket firmado. Las cámaras borran su copia solo cuando el controlador confirma.
3. **IA.** Un **worker** analiza cada foto con un modelo **YOLO26n (ONNX)** y aplica el **triage**: descarta, deja «Pendiente de revisión» o marca «Confirmado por IA» (alerta temprana). Una sola decisión por lugar con las dos cámaras.
4. **Decisión humana.** El **especialista fitosanitario** revisa en la **web** la foto, las cajas de la IA y el lugar, y decide: Confirmado · Posible plaga · Descartado · Evidencia insuficiente, con observación (puede corregir con motivo; queda historial).
5. **Aviso y acción.** Al confirmarse, **Telegram** avisa al jefe de fundo y al supervisor del lote con foto, lote, hilera, lado, segmento, marcador y botones «Ver caso» y «Cómo llegar». En la app, **«Ubicar plaga»** muestra las alertas en un mapa satelital y guía a pie con Google Maps.
6. **Seguimiento.** Panel, bandeja de casos, mapa satelital, plano de hileras, sesiones, reportes CSV, catálogos, usuarios, celulares, destinatarios, papelera, auditoría de solo inserción y ayuda en cada página.

## Alcance del piloto

- Plagas: **chanchito blanco** y **melaza / fumagina** (señales visibles).
- Lotes del fundo con sus hileras divididas en **10 segmentos** de ≈20 m; marcadores físicos al inicio de cada segmento.
- Celulares **Android** (iPhone sin validar).
- Servicio en la nube (sin servidores físicos en el fundo).

## Fuera del alcance / límites declarados

- Plagas ocultas (bajo la corteza, dentro del racimo) que no se ven en la foto.
- Aplicar tratamientos o recomendar dosis.
- Reemplazar al especialista: la IA no diagnostica.
- La ubicación GPS de un celular (5–30 m) no basta para una planta: por eso se combina con lote, hilera, lateral, segmento y marcador.
- Los resultados valen para el contexto del fundo en 2026; no se generalizan automáticamente.
- El desempeño del modelo depende de la cantidad y calidad de fotos de entrenamiento (hoy pocas fotos propias; modelo provisional).

## Dónde seguir

- Sistema tal como funciona hoy: `02-documentacion-del-sistema-y-pruebas/documentacion-del-sistema-y-guia-de-pruebas.md`.
- Arquitectura: `00-contexto/03-arquitectura-y-tecnologias.md`.
- Requerimientos y cambios: `00-contexto/02-requisitos-y-trazabilidad.md`.
