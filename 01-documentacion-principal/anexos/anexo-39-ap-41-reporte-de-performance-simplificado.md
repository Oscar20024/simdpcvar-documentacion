# Anexo 39 (AP-41): Reporte de Performance Simplificado

<table>
<tr>
<th colspan="6">CONTROL DE VERSIONES</th>
</tr>
<tr>
<td>Versión</td>
<td>Hecha por</td>
<td>Revisada por</td>
<td>Aprobada por</td>
<td>Fecha</td>
<td>Motivo</td>
</tr>
<tr>
<td>1.0</td>
<td>Grace<br>Lucia Peña Reyes</td>
<td>Oscar Antonio Gutiérrez<br>Rojas</td>
<td>Jefe de Fundo (Agrícola Riachuelo S.A.C.)</td>
<td>03/10/2026</td>
<td>Emisión del Reporte de Performance Simplificado del Proyecto N° 01 al corte del Sprint 3.</td>
</tr>
</table>

### REPORTE DE PERFORMANCE DEL PROYECTO Nº01

##### - SIMPLIFICADO -

| Nombre del<br>PROYECTO | Siglas del<br>Proyecto | Periodo | cha de Corte |
|---|---|---|---|
| istema inteligente para el monitoreo y detección de plagas en cultivos de vid en Agrícola Riachuelo | SIMDPCVAR | 14/09/2026 al 03/10/2026 | 3 de octubre de 2026 |

<table>
<tr>
<th colspan="4">ESTADO ACTUAL DEL PROYECTO: CÓMO ESTÁ EL PROYECTO A LA FECHA DE CORTE DEL PERIODO.</th>
</tr>
<tr>
<td colspan="4">1.- Situación del Alcance</td>
</tr>
<tr>
<td>Indicador</td>
<td>Fórmula</td>
<td>Cálculo</td>
<td>Resultado</td>
</tr>
<tr>
<td>% AVANCE REAL</td>
<td>EV / BAC</td>
<td>S/ 3,966.67/S/ 5,950.00</td>
<td>66.67%</td>
</tr>
<tr>
<td>% AVANCE PLANIFICADO</td>
<td>PV / BAC</td>
<td>S/ 3,966.67/S/ 5,950.00</td>
<td>66.67%</td>
</tr>
<tr>
<td colspan="4">2.- Eficiencia del Cronograma</td>
</tr>
<tr>
<td>Indicador</td>
<td>Fórmula</td>
<td>Cálculo</td>
<td>Resultado</td>
</tr>
<tr>
<td>SV (Variación del<br>Cronograma)</td>
<td>EV – PV</td>
<td>S/ 3,966.67 − S/ 3,966.67</td>
<td>S/ 0.00 (A tiempo)</td>
</tr>
<tr>
<td>SPI (Índice de Desempeño del Cronograma)</td>
<td>EV / PV</td>
<td>S/ 3,966.67 / S/ 3,966.67</td>
<td>1.00 (Eficiencia óptima)</td>
</tr>
<tr>
<td colspan="4">3.- Eficiencia del Costo</td>
</tr>
<tr>
<td>Indicador</td>
<td>Fórmula</td>
<td>Cálculo</td>
<td>Resultado</td>
</tr>
<tr>
<td>CV (Variación del<br>Costo)</td>
<td>EV – AC</td>
<td>S/ 3,966.67 − S/ 3,510.00</td>
<td>+S/ 456.67 (Ahorro)</td>
</tr>
<tr>
<td>CPI (Índice de Desempeño del Costo)</td>
<td>EV / AC</td>
<td>S/ 3,966.67 / S/ 3,510.00</td>
<td>1.13 (Superávit favorable)</td>
</tr>
<tr>
<td colspan="4">4.- Cumplimiento de Objetivos de Calidad</td>
</tr>
<tr>
<td colspan="4">Tiempo de respuesta de API REST Django: Alcanzado 1.18 s (Meta: ≤ 1.5 s).<br>Precisión del modelo de visión artificial (YOLO): Alcanzado 87.5% (Meta: ≥ 85%). Validación local de nitidez en app móvil: Alcanzado 1.45 s (Meta: ≤ 2.0 s). Cumplimiento de la Regla Estricta de Arquitectura: 100% de las peticiones fluyen centralizadas por la API en Django con JWT; sin acceso directo de clientes a Supabase o Cloudinary.</td>
</tr>
<tr>
<td colspan="4">PROBLEMAS Y PENDIENTES: POR TRATAR.</td>
</tr>
<tr>
<td colspan="4">Pruebas de campo de resistencia de batería en dispositivos móviles durante recorridos continuos de más de en el viñedo.</td>
</tr>
</table>

<table>
<tr>
<th colspan="3">Validación del volumen máximo de fotos simultáneas subiendo a Cloudinary al retomar señal en la estación central.</th>
</tr>
<tr>
<td>Problema / Pendiente:<br>Programados para resolver.</td>
<td>Responsable</td>
<td>Fecha</td>
</tr>
<tr>
<td>Configuración de expiración de tokens<br>JWT a para jornadas de campo.</td>
<td>Oscar Manuel<br>Guerrero Figari</td>
<td>05/10/2026</td>
</tr>
<tr>
<td>Optimización de caché de miniaturas en<br>Cloudinary para el mapa interactivo Leaflet.</td>
<td>Oscar Manuel Guerrero Figari</td>
<td>06/10/2026</td>
</tr>
<tr>
<td>Pruebas de integración del webhook de disparo de alertas vía Telegram Bot API.</td>
<td>Grace Lucia Peña<br>Reyes</td>
<td>08/10/2026</td>
</tr>
<tr>
<td>Taller de capacitación práctica con operadores de monitoreo y entrega de guías físicas.</td>
<td>Grace LuciaPeñaReyes / Ma.<br>Fernanda Zúñiga</td>
<td>12/10/2026</td>
</tr>
<tr>
<td colspan="3">Otros COMENTARIOS U Observaciones</td>
</tr>
<tr>
<td colspan="3">La aprobación de la Solicitud de Cambio N° 01 (migración a la arquitectura unificada en Django + Supabase + Cloudinary) permitió corregir el problema de latencia previa y redujo los costos de infraestructura.</td>
</tr>
<tr>
<td colspan="3">Las reservas de contingencia (S/ 595.00) y de gestión (S/ 300.00) permanecen intactas a la fecha de corte.</td>
</tr>
<tr>
<td colspan="3">El proyecto se encuentra en condiciones óptimas para iniciar el Sprint 5 (Pruebas de<br>Sincronización y Alertas) el 26 de octubre de 2026, según el calendario del Acta de Constitución.</td>
</tr>
</table>
