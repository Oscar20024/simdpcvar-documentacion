# Repositorios de código

El sistema vive en **dos repositorios** independientes que se comunican solo por la API `/api/v1`.

| Carpeta | Repositorio | Qué hay aquí |
|---|---|---|
| [`plataforma-web/`](plataforma-web/) | `agricola-riachuelo-platform` (Django) | [`ESTRUCTURA.md`](plataforma-web/ESTRUCTURA.md): árbol completo y explicación de cada carpeta y archivo · `README_del_repositorio.md` · `AGENTS_del_repositorio.md` · `docs/` (CONTEXTO_WEB, ARQUITECTURA_DATOS, INTEGRACION_APP, TELEGRAM, REPORTE_AVANCE, adr/) · `historial_git_reciente.txt` |
| [`app-movil/`](app-movil/) | `riachuelo-mobile` (Expo / React Native) | [`ESTRUCTURA.md`](app-movil/ESTRUCTURA.md) · `README_del_repositorio.md` · `AGENTS_del_repositorio.md` · `docs/` (CAMBIOS, CONTEXTO_ERRORES_Y_CONFIGURACION, fase4_integracion, adr/) · `historial_git_reciente.txt` |

Los `ESTRUCTURA.md` se generaron con `git ls-files` y el comentario de cabecera de cada archivo (en la app todos empiezan con «QUÉ HACE»), más una explicación escrita por carpeta. Las copias de documentación son del 19/11/2026; algunos textos de octubre quedaron desactualizados (ver `00-contexto/08-estado-versiones-y-pendientes.md`).

Dónde tocar según el cambio:

| Cambio | Web (Django) | App (Expo) |
|---|---|---|
| Regla de revisión o triage | `revision/services.py`, `ia/models.py` (umbrales) | — |
| Nuevo campo que viaja en la sincronización | `api/v1/serializers.py`, servicio del dominio, migración | `src/api/dto.ts`, `src/sync/syncPayloads.ts`, migración SQLite nueva |
| Pantalla web | `web/views.py` + `web/urls.py` + `web/templates/web/*.html` + `web/permissions.py` + JS en `web/static/web/*.js` | — |
| Pantalla de la app | — | `app/<ruta>.tsx` + servicio en `src/` + textos en `src/ui/strings.ts` |
| Parámetro (tiempos, puertos, umbrales) | `config/settings.py` / variables de entorno | `src/config/defaults.ts` |
| Avisos | `notificaciones/services.py`, `notificaciones/telegram.py` | — |
| Modelo de IA | `ia/inference/onnx.py`, `/gestion/` → Modelos de IA, `tools/ia/` | — |
