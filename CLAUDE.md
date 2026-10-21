# CLAUDE.md — Recursos Humanos: plantilla de personal (lives-recursos-humanos)

Las reglas generales de trabajo están en el repo privado `lives-instrucciones` (CLAUDE.md). Este archivo solo describe esta herramienta.

## Qué hace
`personal/index.html` es un **dashboard de la plantilla** (maestro de personal). No es un formulario y **no usa la base de datos**: el usuario sube el Excel del padrón exportado del sistema (.xlsx, .xls o .csv), todo se calcula en el navegador, no se sube a ningún servidor y al cerrar la pestaña se pierde.
- No hay altas y bajas por período, rotación, ausentismo ni cumpleaños. No hay período anterior ni metas.
- Lee la primera hoja y detecta sola la fila de encabezados (en las primeras 25 filas). Columnas que usa: fecha de ingreso, fecha de cese, fecha de nacimiento, sexo, área, sucursal, categoría, discapacidad y el nombre.
- Campos calculados: estado (Cesado si hay fecha de cese, si no Activo), edad en años cumplidos y antigüedad (de ingreso a cese, o a hoy si no hay cese).
- Filtros: búsqueda libre, rango de fecha de ingreso y selectores por columna (primero las prioritarias, luego cualquier columna con entre 2 y 40 valores distintos; las primeras 5 en la barra y el resto en "Más filtros"). Los campos sensibles no se ofrecen como filtro.
- Pestañas: Estructura de la empresa (por área, sucursal y categoría), Perfil de las personas (sexo, tramos de edad y de antigüedad) y Detalle por trabajador (tabla paginada y ordenable, con selector de columnas).
- KPIs, todos sobre los trabajadores filtrados y siempre grupo ÷ filtrados (nunca promedio de porcentajes): trabajadores en la vista, activos (con cesados), mujeres (con hombres), edad promedio (solo con fecha de nacimiento), antigüedad promedio (solo con fecha de ingreso) y con discapacidad. El insight suma las 3 áreas más grandes ÷ trabajadores con área.
- Los vacíos van a "Sin dato" en los desgloses.

## Datos sensibles
- El padrón trae datos sensibles (documento de identidad, dirección, teléfonos, correo, datos bancarios, sueldo básico y similares). La pantalla los **oculta por defecto** en la tabla, y quedan **fuera del Excel exportado** salvo que se marque la casilla "Incluir datos sensibles" en el modal de exportación.
- No hay datos de personas dentro del código; solo llegan con el archivo que carga quien usa la pantalla.
- Los archivos del padrón no se suben a GitHub (regla general).

## Salidas
- Excel propio (hoja con las filas filtradas y las columnas visibles; no tiene el formato LIVES completo) y Excel LIVES del bloque común.
- PNG del tablero (`dashboard_personal_lives_<fecha>.png`).

## Base de datos
Ninguna. No hay tablas, funciones ni Storage de esta herramienta.

## Quién la usa
No aparece en el registro de entradas del portal de los últimos 60 días (al 21/10/2026).

## Quién puede usarla
No tiene permisos propios: cualquiera que abra la página y tenga el Excel puede usarla.

## Pendientes vistos al escribir esto (21/10/2026)
- La tabla de detalle inserta los valores del Excel sin escapar: si un archivo trae HTML, se inyectaría en la página. Es contenido propio de quien la usa, pero conviene escapar.
- El Excel propio no usa el estándar LIVES (sin logo ni banda, sin formatos numéricos, sin autofiltro).
- Para el paso a SQL Server, el padrón debería salir directo del sistema de personal, con permisos por rol (por ser datos de personas) y sin pasar por Excel.
