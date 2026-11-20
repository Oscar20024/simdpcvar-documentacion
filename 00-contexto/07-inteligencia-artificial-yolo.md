# Inteligencia artificial: modelo YOLO, triage y entrenamiento

Estado al 21/10/2026, vigente en noviembre de 2026. Código: `ia/` y `revision/` del repositorio web; herramientas en `tools/ia/`. Decisiones: ADR-W-007 (alertas IA), ADR-W-008 (triage y limpieza), ADR-W-009 (decisión por lugar con dos cámaras).

## Decisiones del modelo v1

- **Modelo:** YOLO26n (Ultralytics 8.4+), exportado a **ONNX** con salida cruda (sin NMS; el NMS lo hace el worker).
- **Clases, en este orden:** `["chanchito_blanco", "melaza_fumagina"]`.
- **Tamaño:** `imgsz` 640, entrenado sobre recortes de 1280 px. En producción se usa **tiling** `{"tile": 1280, "overlap": 0.2}` sobre la foto completa de 12 MP.
- **Entrenamiento:** Google Colab con GPU T4; datos en Drive (`externas/` y `propias/`, zips exportados de Roboflow en formato YOLOv8). Notebook `tools/ia/entrenar_yolo_colab.ipynb`, recortes con `tools/ia/cortar_mosaicos.py` (`--repetir-propias N`, `--max-externas M`).
- **Regla de activación:** un modelo solo se activa si `tools/ia/verificar_onnx.py` detecta al menos el **85 %** de las fotos de prueba con plaga (y, para el v3, además pocas falsas alarmas en fotos sanas).
- Solo un modelo activo a la vez; se configura en `/gestion/` → Modelos de IA (clases, umbrales, mosaicos). Los pesos `.onnx` no se versionan: se copian al servidor (`deploy/modelos/`).

## Modelo activo: 2026.10-v2 (provisional, desde el 21/10/2026)

- `conf` 0,4 · `iou` 0,45 · `imgsz` 640 · tiling 1280 / 0,2.
- Datos: 32 fotos propias del fundo + ~910 externas.
- mAP50 en test propio (recortes): chanchito 0,126; melaza 0,065 (el modelo anterior «int» daba 0,109 y 0).
- `verificar_onnx` sobre fotos completas: 5/5 fotos con plaga detectadas (el «int» 3/5). Recall por caja de chanchito ≈ 11 % con 0,4. **No se midieron falsas alarmas en fotos sanas** (prueba de solo 5 fotos).
- Rollback: existe respaldo del modelo anterior en el servidor; volver atrás = copiarlo como `modelo.onnx`, reactivarlo en `/gestion/` y reiniciar el worker.

## Triage vigente

Ver `04-roles-flujo-y-estados.md`. Umbrales actuales: revisión 0,40 con mínimo 2 cajas; confirmación automática con 2+ cajas ≥ 0,70 o 3+ cajas ≥ 0,50. Comando para reevaluar:

```text
python manage.py aplicar_triage --revision 0.40 --auto 0.70 [--min-cajas-caso 2] [--min-cajas 2]
       [--varias 3 --umbral-varias 0.50 | --varias apagado] [--revisar-confirmados] [--simular]
```

`--revisar-confirmados` devuelve a «Pendiente de revisión» los «Confirmado por IA» sin decisión humana que ya no cumplen las reglas. Los casos decididos por el especialista nunca cambian.

Motivo del ajuste (21/10): en campo, una sola caja de 0,72 sobre un tallo y sobre un reflejo de la uva se confirmó sola; se pidió además que 3 o más cajas medianas confirmen.

## Historial

| Fecha | Hecho |
|---|---|
| 08/10/2026 | Modelo de integración **2026.10-int**, solo con datos de internet (Roboflow Mealybug), mAP50 ≈ 0,50 en val externo. Activo hasta el 21/10 |
| 20/10/2026 | Se borraron con la limpieza las 246 fotos de campo del servidor porque YOLO funcionaba mal. **Lección: no usar la limpieza antes de tener un modelo entrenado**; para silenciar la IA se desactiva el modelo |
| 20–21/10/2026 | Proyecto Roboflow privado «riachuelo-chanchito»: 32 fotos. Criterio: una caja chica por insecto; un grupo solo si están encimados; melaza y fumagina (gotas ámbar, hollín negro) como `melaza_fumagina`; fotos sin plaga como Null |
| 21/10/2026 | Activado **2026.10-v2** (provisional) y triage de tres franjas |

## Siguiente (plan del equipo)

1. Prueba de campo con el v2: el especialista revisa los pendientes; lo que la IA confirmó mal se marca «Descartado» (sirve como negativo).
2. Juntar 150–200 fotos del fundo, 100+ con chanchito de cerca (40–60 cm), en corteza y racimos, e incluir fotos sanas.
3. Subirlas a Roboflow (batch `campo_octubre_2`), nueva versión → zip a `propias/`.
4. Entrenar **2026.10-v3** (`REPETIR_PROPIAS = 5`, `MAX_EXTERNAS = 300`); activar solo si cumple 85 % y pocas falsas alarmas.
5. Después: `python manage.py exportar_dataset --modo revisados` con las decisiones del especialista → v4.

## KPI de la IA

El capítulo 14.8.3 define KPI de desempeño de la IA frente al especialista (concordancia, falsos positivos/negativos con muestra semanal de «Descartadas por la IA», etc.). Ver `01-documentacion-principal/capitulo-14-gestion-del-servicio.md`.
