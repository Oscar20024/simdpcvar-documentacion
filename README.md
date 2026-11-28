# SIMDPCVAR · Documentación y contexto del proyecto

**Sistema inteligente para el monitoreo y detección de plagas en cultivos de vid en Agrícola Riachuelo S.A.C. (Ica, Perú).**
Proyecto del curso *Curso Integrador II: Sistemas* (Facultad de Ingeniería de Sistemas e Informática, Universidad Tecnológica del Perú, Ica, 2026).

Este repositorio **no contiene código ejecutable**. Reúne en Markdown toda la documentación del proyecto (el informe principal de 353 páginas, la documentación del sistema, los manuales, los archivos maestros de la web y de la app y la documentación técnica de los dos repositorios de código) más un conjunto de documentos de contexto escritos para que **otra IA o una persona nueva** entienda el proyecto sin haber participado en él.

> **Si eres un agente de IA:** empieza por [`AGENTS.md`](AGENTS.md). Ahí está el orden de lectura, las reglas que no se pueden romper y un índice «tema → archivo».

---

## El proyecto en 30 segundos

- **Problema.** En el fundo de Agrícola Riachuelo (La Tinguiña, Ica) entre 10 y 20 operadores revisan a ojo miles de plantas de vid buscando plagas (sobre todo el **chanchito blanco**, *Planococcus ficus*) y anotan en papel o en un Excel. No queda evidencia, la ubicación es aproximada, el especialista valida tarde y no hay alertas. En el campo casi no hay internet.
- **Solución.** Tres celulares Android en un **soporte tipo T** (un *controlador* y dos *cámaras*) recorren cada hilera por sus dos lados (lateral A y lateral B) y fotografían cada punto **sin internet**. Con señal, el controlador **sincroniza** con una **plataforma Django** en la nube; un **modelo YOLO** analiza cada foto y **sugiere indicios**; el **especialista fitosanitario** confirma o descarta en la web; al confirmarse, **Telegram** avisa al jefe de fundo y al supervisor con foto, lugar exacto y botón «Cómo llegar». La app tiene además «Ubicar plaga» para llegar a pie a la planta.
- **Componentes.** App móvil (Expo/React Native) · Plataforma web + API `/api/v1` (Django 5.2 + HTMX + Leaflet) · Worker de IA (YOLO26n en ONNX) · Supabase PostgreSQL · Cloudinary · Telegram Bot API · VPS Hostinger con Docker y Traefik.
- **En producción (piloto):** <https://monitoreo.agricolariachuelo.org> · salud: `/api/v1/health` → `{"status":"UP"}`.
- **Versiones documentadas (19/11/2026):** web **v1.6.2** en código (en el servidor, hasta v1.6 mientras no se despliegue) · app **0.6.1** · modelo IA **2026.10-v2** (provisional). Qué corre hoy y qué falta: [`00-contexto/08-estado-versiones-y-pendientes.md`](00-contexto/08-estado-versiones-y-pendientes.md).
- **Soporte:** portal de Jira Service Management <https://oscoscantonio.atlassian.net/servicedesk/customer/portal/1> (incidentes INC y solicitudes SOL con SLA).

## Mapa del repositorio

```text
simdpcvar-documentacion/
├── README.md                     ← estás aquí
├── AGENTS.md                     ← guía para agentes de IA (léela primero si eres una IA)
├── CLAUDE.md                     ← apunta a AGENTS.md
├── 00-contexto/                  ← documentos de contexto escritos para entender el proyecto rápido
│   ├── 01-proyecto-en-una-pagina.md
│   ├── 02-requisitos-y-trazabilidad.md
│   ├── 03-arquitectura-y-tecnologias.md
│   ├── 04-roles-flujo-y-estados.md
│   ├── 05-reglas-limites-y-restricciones.md
│   ├── 06-gestion-del-servicio-sla-kpi-jira.md
│   ├── 07-inteligencia-artificial-yolo.md
│   ├── 08-estado-versiones-y-pendientes.md
│   └── 09-glosario-rapido.md
├── 01-documentacion-principal/   ← el informe completo (353 págs.) partido en capítulos y anexos
│   ├── README.md                 ← índice detallado de capítulos y anexos
│   ├── 00-preliminares.md, 00-glosario.md
│   ├── capitulo-01-…md … capitulo-14-…md
│   ├── referencias.md
│   ├── anexos/                   ← 81 anexos (AP = proyecto PMBOK, AI = investigación, AS = Scrum, AT = técnicos)
│   └── media/                    ← figuras del informe
├── 02-documentacion-del-sistema-y-pruebas/
│   └── documentacion-del-sistema-y-guia-de-pruebas.md   ← qué hace el sistema, RF/RNF/RN, SC, casos de prueba
├── 03-manuales/
│   ├── manual-de-usuario-plataforma-web.md
│   ├── manual-de-usuario-app-movil.md
│   └── media/                    ← capturas de pantalla de los manuales
├── 06-figuras/                   ← cada imagen transcrita a texto (qué muestra + todo su texto)
│   ├── informe-principal/        ← 51 figuras del informe (diagramas, mockups, tablas en imagen, capturas)
│   ├── manual-web/               ← 32 capturas del manual de la plataforma web
│   └── manual-app/               ← 35 capturas del manual de la app móvil
├── 04-maestros/
│   ├── MAESTRO_WEB.md            ← especificación con la que se construyó la web (reglas W-01…W-24)
│   └── MAESTRO_APP_MOVIL.md      ← especificación de la app, la API /api/v1 (§15) y la arquitectura (§28)
└── 05-repositorios-de-codigo/
    ├── plataforma-web/           ← ESTRUCTURA.md (cada archivo explicado), README, AGENTS, docs/ y adr/ del repo Django
    └── app-movil/                ← ESTRUCTURA.md, README, AGENTS, CAMBIOS, contexto de errores y adr/ del repo Expo
```

## Orden de lectura recomendado

1. [`00-contexto/01-proyecto-en-una-pagina.md`](00-contexto/01-proyecto-en-una-pagina.md) — qué es, para quién, alcance y límites.
2. [`02-documentacion-del-sistema-y-pruebas/documentacion-del-sistema-y-guia-de-pruebas.md`](02-documentacion-del-sistema-y-pruebas/documentacion-del-sistema-y-guia-de-pruebas.md) — el resumen más fiel del sistema **tal como funciona hoy** (17 págs. originales).
3. [`00-contexto/03-arquitectura-y-tecnologias.md`](00-contexto/03-arquitectura-y-tecnologias.md) y [`04-roles-flujo-y-estados.md`](00-contexto/04-roles-flujo-y-estados.md).
4. Según la tarea: el capítulo del informe ([índice](01-documentacion-principal/README.md)), el maestro de la web o de la app, o la estructura de código en `05-repositorios-de-codigo/`.

## Fuentes y fidelidad

| Carpeta | De dónde sale | Cómo se convirtió |
|---|---|---|
| `01-documentacion-principal/` | `Documentacion_Actual_19-11-26_revisada.docx` (348 págs.) convertido con pandoc, y actualizado a mano con los cambios de la versión PDF de 353 págs. (`Documentacion_principal.pdf`, noviembre 2026) | pandoc a GFM, con la numeración automática de Word convertida en texto (para conservar «14.7.10», etc.), tablas simplificadas y partido por capítulo y anexo. Cambios de la versión de 353 págs.: Leaflet sobre teselas Esri y OpenStreetMap; prototipo descrito como «soporte» (ya no «vestible» ni «mochila»); cámaras A y B que fotografían la hilera; avisos por Telegram en RF10, acta, canales de soporte e instrumentos. |
| `02-documentacion-del-sistema-y-pruebas/` | Fuente HTML del PDF «Documentación del sistema y guía de pruebas» v1.6.1 | pandoc HTML → GFM. Los diagramas SVG no se incluyen. |
| `03-manuales/` | PDF «Manual de usuario · Plataforma web» (43 págs.) y «· App móvil» (35 págs.), v1.6.1 | Texto extraído del PDF con PyMuPDF (sin el texto viejo tapado por las correcciones) + capturas. Las tablas del PDF quedan como texto. |
| `06-figuras/` | Las imágenes de las carpetas 01 y 03 | Cada figura se miró y se transcribió a mano: qué muestra, todo su texto y, en el informe, si sigue vigente frente al sistema actual. En las capturas de los manuales se agrega el texto reconocido por OCR. El capítulo o manual enlaza cada transcripción justo debajo de la imagen. |
| `04-maestros/` | Archivos maestros en Markdown (MAESTRO_WEB v1.0, MAESTRO_APP_MOVIL v2.0) | Copia directa. |
| `05-repositorios-de-codigo/` | Repositorios `agricola-riachuelo-platform` (commit `97575dc`) y `riachuelo-mobile` (commit `7a95da6` + trabajo local) | `git ls-files` + comentario de cabecera de cada archivo; copias de su documentación. |

Cuando dos fuentes no coinciden, manda este orden: **código y documentación técnica de los repos (05) > documentación del sistema (02) > informe principal (01) > maestros (04)**. Los maestros son la especificación inicial; varias decisiones posteriores (ADR y solicitudes de cambio SC-01…SC-09) los modifican. Ver [`00-contexto/08-estado-versiones-y-pendientes.md`](00-contexto/08-estado-versiones-y-pendientes.md).

## Datos omitidos en esta versión pública

Este repositorio es público. Se reemplazaron por `[omitido]`: contraseñas (incluida la de las cuentas de prueba), teléfonos, correos personales, códigos de estudiante, direcciones domiciliarias, la IP del servidor e identificadores de servicios (Cloudinary, Expo, Supabase). Ningún secreto (`.env`, tokens, claves de API) se copió. Quien necesite esos datos debe pedirlos al responsable técnico del equipo.

## Equipo

Gutiérrez Rojas, Oscar Antonio (Project Manager) · Peña Reyes, Grace Lucia · Guerrero Figari, Oscar Manuel · Zuñiga Matos, María Fernanda. Docente: Pacheco Tamariz, Jean Pierre. Cliente: Agrícola Riachuelo S.A.C. (jefe de fundo y especialista fitosanitario como interesados clave).
