# CLAUDE.md — Inspección Final de Costura (inspeccion-final-costura)

Las reglas generales de trabajo están en el repo privado `lives-instrucciones` (CLAUDE.md). Este archivo solo describe esta herramienta.

## Qué hace
- `index.html` — **formulario diario** de la inspectora de costura: fecha, inspectora, línea, estilo, carga, primeras, defectos por prenda, hora de inicio y fin, horas improductivas y observaciones. Tabla de registros con editar y eliminar (eliminar = queda ANULADO, con el usuario). Se pueden agregar defectos nuevos y el T. STD de un estilo. Botón "📊 Ver tablero" abre el tablero en una ventana; con `#tablero` abre directo en modo solo consulta.
- `dashboard.html` — el mismo **tablero**, solo de consulta (`SOLO_LECTURA`): pestañas Inspectoras, Defectos, Detalle, Tendencia y Reporte; período semanal, mensual, anual o por día; filtros de línea e inspectora; comparación con el período anterior. Excel LIVES (Resumen con numerador y denominador de cada indicador, por inspectora, por línea, defectos; y Detalle).
- Los dos permiten descargar una copia HTML del tablero o del reporte.

## Indicadores (suma primero, divide después)
- % Primeras = Σ primeras ÷ Σ carga. % Con defecto = 100 − % Primeras.
- % Segundas = prendas con defectos de la categoría segundas ÷ Σ carga.
- % Eficiencia = minutos producidos ÷ (horas asistidas × 60). Minutos producidos = (carga + re-inspección) × T. STD (lo calcula la base). Horas asistidas = turno − improductivas − 45 min de refrigerio por turno de 4 h o más; el turno se arma por fecha e inspectora.
- Primeras, defecto y segundas excluyen la prenda de SERVICIO; eficiencia la incluye. La re-inspección con carga 0 suma minutos pero no entra en los %.

## Base de datos (Supabase lives-datos)
- Todo pasa por la puerta única `ifc_api` (`p_accion`):
  - Leer: `catalogos` (inspectoras, líneas, estilos, defectos, categorías, T. STD) y `datos` (registros; desde el mes anterior en el formulario, completo en el tablero).
  - Registrar: `guardar`, `editar`, `eliminar`, `nuevoDefecto`, `guardarSTD`.
- Tablas: `ifc_registros`, `ifc_defectos`, `ifc_estilos`, `ifc_inspectoras`, `ifc_lineas`, `ifc_std`; historial de cambios en `ifc_bitacora`.
- No usa Storage.

## Quién la usa
Según las entradas por el portal de los últimos 30 días al 21/10/2026: 3 personas (Gerencia de Producción, supervisor de planta, supervisora de Mejoras), además de la inspectora que registra.

## Quién puede registrar
- Lo decide la base dentro de `ifc_api`: `ifc_puede_escribir_` para registrar; sin permiso, "Este usuario solo puede consultar". `dashboard.html` nunca registra.
- A Sistemas: `ifc_puede_escribir_` es la pieza que reemplazan con su seguridad.
