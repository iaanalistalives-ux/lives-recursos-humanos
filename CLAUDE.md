# CLAUDE.md — Recursos Humanos: plantilla de personal (lives-recursos-humanos)

Las reglas generales de trabajo están en el repo privado `lives-instrucciones` (CLAUDE.md). Este archivo solo describe esta herramienta.

## Qué hace
`personal/index.html` es un **dashboard de la plantilla** (maestro de personal). No es un formulario. Desde el 19/11/2026 **lee el padrón de la base** por la puerta única `rh_api` (acción `padron`); ya nadie sube el Excel en la pantalla. El destino final es que Sistemas conecte esa misma puerta a su sistema de personal.
- No hay altas y bajas por período, rotación, ausentismo ni cumpleaños. No hay período anterior ni metas.
- La puerta devuelve `{ok, columnas, filas, total}`: los 42 encabezados del reporte «LISTADO DE TRABAJADORES» del sistema, con sus mismos nombres (incluido «Feche Cese», tal como lo escribe el sistema), y una fila por trabajador en ese orden. Las fechas llegan como AAAA-MM-DD. Si falla, la pantalla muestra el error con «Reintentar».
- «Fecha Exp DNI» viene como texto dd/mm/aaaa en el reporte; en la base es fecha y la pantalla la vuelve a mostrar dd/mm/aaaa.
- Campos calculados en la pantalla: nombre completo, estado (Cesado si hay fecha de cese, si no Activo), edad en años cumplidos y antigüedad (de ingreso a cese, o a hoy si no hay cese).
- Filtros: búsqueda libre, rango de fecha de ingreso y selectores por columna (primero las prioritarias, luego cualquier columna con entre 2 y 40 valores distintos; las primeras 5 en la barra y el resto en "Más filtros"). Los campos sensibles no se ofrecen como filtro.
- Pestañas: Estructura de la empresa (por área, sucursal y categoría), Perfil de las personas (sexo, tramos de edad y de antigüedad) y Detalle por trabajador (tabla paginada y ordenable, con selector de columnas).
- KPIs, todos sobre los trabajadores filtrados y siempre grupo ÷ filtrados (nunca promedio de porcentajes): trabajadores en la vista, activos (con cesados), mujeres (con hombres), edad promedio (solo con fecha de nacimiento), antigüedad promedio (solo con fecha de ingreso) y con discapacidad.
- Los vacíos van a "Sin dato" en los desgloses. Si los filtros no dejan a nadie, sale un aviso y se ocultan KPIs, pestañas y gráficos.
- Sin barra de título, veredicto, insight, nota de fuente ni bloque de ayuda (estándar v2.6).

## Datos sensibles
- El padrón trae datos sensibles (documento de identidad, dirección, teléfonos, correo, datos bancarios, sueldo básico y similares). La pantalla los **oculta por defecto** en la tabla, y quedan **fuera del Excel exportado** salvo que se marque la casilla "Incluir datos sensibles" en el modal de exportación.
- Decisión de Christian (19/11/2026): a la base de pruebas se cargaron las 42 columnas, también las sensibles, porque el link solo lo tienen él y Sistemas. La puerta se lee sin usuario, como todo en la base de pruebas: quien conozca la puerta puede leer el padrón. En SQL Server, Sistemas exige usuario y permisos por rol.
- No hay datos de personas dentro del código. Los archivos del padrón no se suben a GitHub (regla general).

## Salidas
- Excel propio (hoja con las filas filtradas y las columnas visibles; no tiene el formato LIVES completo) y Excel LIVES del bloque común.
- PNG del tablero (`dashboard_personal_lives_<fecha>.png`).

## Base de datos (prefijo `rh_`)
- `rh_trabajadores`: una fila por trabajador, llave `codigo` (código del trabajador en el sistema), 42 columnas del reporte más `orden` y `cargado_el`. Cerrada: sin lectura ni escritura directa.
- `rh_api(p_accion, p)`: puerta única, solo lectura. Acción `padron`.
- Carga: la hace Claude desde el Excel del sistema que Christian deja en Descargas (primera carga: 8 trabajadores de Planilla De Obreros, registro `Rh01`). Nadie escribe desde la pantalla.
- Códigos: se guardan los del sistema tal como vienen (trabajador, categoría, cargo, centro de costo, horario) junto a sus nombres; no se inventó ninguno.

## Quién la usa
No aparece en el registro de entradas del portal de los últimos 60 días (al 21/10/2026).

## Quién puede usarla
No tiene permisos propios ni registra nada: quien abre el link ve el padrón.

## Pendientes
- La tabla de detalle inserta los valores sin escapar: si un dato trae HTML, se inyectaría en la página. Conviene escapar.
- El Excel propio no usa el estándar LIVES (sin logo ni banda, sin formatos numéricos, sin autofiltro).
- Falta el paquete para Sistemas (`Personal_para_Sistemas`) con el contrato de `rh_api`.
