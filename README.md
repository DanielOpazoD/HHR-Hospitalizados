📄 README.md
Analizador Estadístico de Hospitalizados — Hospital Hanga Roa
Versión: 1.0
Autor: Dr. Daniel Opazo — Servicio de Medicina Interna, Hanga Roa
Tecnologías: HTML, JavaScript, Pyodide (Python), Pandas, SheetJS, Plotly
🏥 Descripción general

Este proyecto implementa un analizador estadístico local, autónomo y ejecutable en navegador, diseñado para procesar la estadística diaria de pacientes hospitalizados del Hospital Hanga Roa mediante archivos Excel mensuales proporcionados por el Servicio de Hospitalizados.

El sistema permite:

Cargar un archivo Excel por mes (con una hoja por día).

Analizar hospitalizaciones, ingresos, altas, traslados y días de estadía (LOS).

Identificar pacientes UPC vs no UPC.

Evaluar la ocupación diaria y su desglose.

Generar gráficos dinámicos tipo dashboard.

Descargar tablas y estadísticas en formato CSV.

Administrar múltiples meses acumulados y visualizar cualquiera desde un selector.

Todo esto sin enviar datos a internet, ya que el procesamiento ocurre completamente en el navegador mediante Pyodide + Pandas.

🎯 Objetivos del proyecto
1. Automatizar la estadística hospitalaria mensual

Evitar el procesamiento manual de hojas Excel, que es propenso a errores, lento y no escalable.

2. Cuantificar indicadores clave

Ocupación diaria

Ocupación UPC

Altas por día

Traslados por día

Ingresos por día

LOS: distribución, promedio, y desglose por tipo de paciente

3. Permitir análisis multicapa

Soporta cargar noviembre, luego diciembre, luego enero, y visualizar el mes deseado en cualquier momento.

4. Proveer una herramienta liviana y portátil

Funciona offline

No necesita backend

No sube datos a servidores externos

Se puede correr en un computador sin instalar nada

5. Incrementar la calidad de la información hospitalaria

Consolidando datos, validando fechas y corrigiendo inconsistencias de manera automática.

🧠 Cómo funciona internamente

El analizador utiliza tres capas:

1️⃣ Capa de lectura (SheetJS)

Convierte cada hoja del Excel mensual a CSV:

Una hoja por día (ej: “01-11-2025”, “02-11-2025”).

Se normalizan cabeceras, espacios y columnas duplicadas.

Se detectan bloques:

Hospitalizados

ALTAS

TRASLADOS

2️⃣ Capa de procesamiento (Pyodide + Pandas)

Toda la lógica se ejecuta en Python dentro del navegador:

🔍 Procesamientos principales

Limpieza de datos

Detección de fecha por nombre de hoja

Unificación de columnas

Cálculo de:

Fecha de ingreso (primera aparición)

Fecha de egreso GOLD (última aparición)

Fecha de alta declarada

Fecha de traslado

Fecha de egreso final (regla jerárquica)

Días totales hospitalizados (LOS)

Ocupación diaria

Ocupación UPC

Altas / Traslados por día

✔ Manejo robusto de errores

Fechas inválidas → convertidas a NaT

RUTs corruptos → limpiados

Días sin altas/traslados → completados con 0

Hojas con espacios o formatos extraños → corregidas

3️⃣ Capa visual (Plotly + Tablas HTML)

Incluye gráficos:

Ocupación total vs UPC por día

Altas y traslados por día

Distribución LOS (histograma)

LOS según UPC vs No UPC

LOS según tipo de cama (UTI vs Media vs Otra)

Incluye también:

Tabla resumen de todos los pacientes del mes

Tabla de ocupación diaria

Botones de descarga CSV

Botón ZIP (opcional para expansiones futuras)

🧪 Cómo usar el analizador
✔ 1. Abrir el archivo index.html en un navegador moderno

Chrome recomendado.

✔ 2. Seleccionar un archivo Excel mensual

Debe tener:

Hojas con nombres tipo 01-11-2025, 2-11-2025, etc.

Una tabla principal desde fila 8 con columnas conocidas:

CAMA

TIPO CAMA

NOMBRE PACIENTE

RUT

EDAD

PATOLOGÍA

UPC

etc.

✔ 3. Hacer clic en “Agregar archivo mensual”

El log mostrará:

Hojas procesadas

Altas detectadas

Traslados detectados

Número de registros

✔ 4. Seleccionar el mes en el selector

Los gráficos y tablas se regeneran automáticamente.

✔ 5. Descargar CSV si lo deseas

Incluye:

Resumen por paciente

Ocupación diaria

Flujos diarios (altas/traslados)

LOS

UPC

⚠️ Precauciones y consideraciones importantes
1. Datos sensibles

El archivo Excel contiene información de pacientes.
El sistema:

Procesa todo localmente

Nunca sube datos a la nube

No hace requests hacia servidores externos
Aun así, no compartir archivos fuera del entorno clínico autorizado.

2. La estructura del Excel debe ser consistente

Aunque el programa es robusto, dependerá de:

Que la tabla principal mantenga sus columnas

Que “ALTAS” y “TRASLADOS” aparezcan con texto reconocible

Que no haya celdas fusionadas irregulares

Si el hospital cambia el formato del Excel, quizá se requiera ajustar el parser.

3. El procesamiento se hace en el navegador

Pyodide puede usar hasta 500–800 MB de RAM si se cargan muchos meses.

Recomendaciones:

Cargar uno o dos meses a la vez

Reiniciar el programa con el botón “Reiniciar datos” si se vuelve lento

Usar equipos con > 8 GB si se trabajará con muchos meses seguidos

4. No cerrar la pestaña mientras Pyodide procesa

Puede demorar 1–10 segundos por mes, según el tamaño.

5. RUTs deben ser uniformes

Idealmente sin puntos ni dígito verificador separado.

El sistema los limpia, pero entradas muy anómalas pueden perder correspondencia.

6. La fecha de egreso GOLD es la más confiable

La fecha de egreso declarada puede omitirse accidentalmente en la hoja.

La regla final es:

EGRESO FINAL =
    FECHA_EGRESO_DECLARADA
    → si existe
    SINO FECHA_TRASLADO
    → si existe
    SINO FECHA_EGRESO_GOLD

7. No usar archivos .xls antiguos

Solo .xlsx para evitar incompatibilidades.

🛠️ Funcionalidades planificadas (roadmap)

✔ Modularización completa (JS + Python en archivos separados)

✔ Uso de Web Workers para Pyodide (mayor fluidez)

✔ Dashboards avanzados tipo PowerBI

✔ Exportación ZIP con todos los CSV

✔ Modo oscuro

✔ Validación automática de consistencia (ingresos sin egresos, duplicados, etc.)

✔ Integración opcional con Google Drive (modo clínico interno)

🤝 Contribuir

Este proyecto es altamente ampliable.

Para colaborar:

Haz un fork del repositorio

Crea un branch con tus cambios

Envía un Pull Request

📬 Contacto

Dr. Daniel Opazo
Medicina Interna — Hospital Hanga Roa
