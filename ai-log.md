# AI log - Tlamatipohua

## Herramientas

## Filosofía de uso

## Registro de uso

### Fecha | Herramienta | Tarea

- **Fecha | Herramienta | Tarea**: 2026-04-02 | GitHub Copilot (GPT-5.3-Codex) | Quitar numerales romanos de la columna RHA en gp.
- **Tarea**: Limpiar los nombres de las regiones en la columna RHA para eliminar el prefijo con número romano.
- **Prompt**: remove the roman numeral from the strings of the column "RHA" of gp
- **Resultado**: Se aplicó una expresión regular sobre `gp["RHA"]` para eliminar prefijos romanos (por ejemplo, I, II, III) y se verificó que los valores quedaran solo con el nombre de la región.
- **Decisión**: Mantener la limpieza de texto dentro del flujo del notebook para asegurar consistencia antes de generar `gp_largo` y los mapas.

- **Fecha | Herramienta | Tarea**: 2026-04-02 | GitHub Copilot (GPT-5.3-Codex) | Convertir `repda2020.vc` de texto a float corrigiendo la línea fallida.
- **Tarea**: Transformar la columna `vc` (leída como string) a tipo numérico flotante en el dataframe `repda2020`.
- **Prompt**: convert repda2020.vc from string to float, taking into account that the current code line does not succeed. after that, document this task in the same way as the last task
- **Resultado**: Se reemplazó la conversión directa `astype(float)` por una limpieza previa de separadores de miles y caracteres no numéricos, seguida de `pd.to_numeric(..., errors="coerce")`. La celda se ejecutó sin errores y `vc` quedó como valores flotantes.
- **Decisión**: Estandarizar conversiones numéricas con limpieza previa para columnas provenientes de Excel con formato textual (por ejemplo, `3,472.88`).

- **Fecha | Herramienta | Tarea**: 2026-04-02 | GitHub Copilot (GPT-5.3-Codex) | Corregir construcción de `repda` para incluir una columna por año.
- **Tarea**: Ajustar el ciclo de carga de REPDA para que no sobrescriba el dataframe en cada iteración y conserve una columna por cada año de `years`.
- **Prompt**: why with the code in this cell, repda only has one column. fix it to have one column for each year in years. after performing the task, document it the same way as the last task
- **Resultado**: Se identificó que en el bloque `else` se reasignaba `repda` en cada vuelta del ciclo. Se corrigió creando un dataframe temporal por año (`repda_year`) y asignando su serie a `repda[year]`. La ejecución final mostró columnas `Estado`, `2020`, `2021`, `2022` y `2023`.
- **Decisión**: Conservar el dataframe base con la columna llave (`Estado`) y anexar columnas anuales para evitar pérdida de datos por sobrescritura.

- **Fecha | Herramienta | Tarea**: 2026-04-02 | GitHub Copilot (GPT-5.3-Codex) | Corregir 404 de `rha2023.json` en Quarto Preview.
- **Tarea**: Evitar que Plotly intente cargar `../datos/rha2023.json` desde el navegador durante `quarto preview`, lo que causaba solicitudes a `/datos/rha2023.json` con 404.
- **Prompt**: apply change and document in ai-log.md
- **Resultado**: Se actualizó `dashboard/index.qmd` para cargar el GeoJSON con Python (`json.load`) y pasar un diccionario a `px.choropleth` en ambos mapas RHA. Con esto, la lectura del archivo ocurre en tiempo de render del kernel y ya no depende de una ruta HTTP relativa en el cliente.
- **Decisión**: Usar objetos GeoJSON cargados en Python para recursos locales en dashboards Quarto, en lugar de rutas de archivo como string que se resuelven en el navegador.

- **Fecha | Herramienta | Tarea**: 2026-04-02 | GitHub Copilot (GPT-5.3-Codex) | Reducir reinicios de sesión por render lento en Quarto Preview.
- **Tarea**: Evitar que celdas pesadas (lectura de múltiples archivos Excel) se ejecuten completas en cada refresco de `quarto preview`, lo que estaba alargando la ejecución de la celda 2/7 hasta provocar reinicio de sesión.
- **Prompt**: it lasted so long that the session restarted automatically
- **Resultado**: Se habilitó caché de ejecución en `dashboard/index.qmd` agregando `execute: cache: true` en el encabezado YAML. Con ello, Quarto reutiliza resultados de celdas no modificadas y reduce drásticamente los tiempos de re-render en preview.
- **Decisión**: Mantener caché activa para notebooks/dashboards con carga repetida de datos históricos, especialmente cuando se trabaja con `quarto preview` en modo watch.

- **Fecha | Herramienta | Tarea**: 2026-04-02 | GitHub Copilot (GPT-5.3-Codex) | Convertir columnas de `repda_2021` a tipo float.
- **Tarea**: Asegurar que las columnas numéricas de `repda_2021` (excepto `Estado`) queden como flotantes al cargar el archivo Excel.
- **Prompt**: convert columns from repda_2021 to float
- **Resultado**: Se actualizó la celda de carga para identificar columnas numéricas (`numeric_cols`), limpiar comas y caracteres no numéricos, y convertir con `pd.to_numeric(..., errors="coerce").astype(float)`. La celda se ejecutó correctamente y las columnas objetivo quedaron en formato float.
- **Decisión**: Reutilizar este patrón de limpieza + conversión para dataframes provenientes de Excel cuando puedan contener separadores de miles u otros símbolos.

- **Fecha | Herramienta | Tarea**: 2026-04-02 | GitHub Copilot (GPT-5.3-Codex) | Mostrar todas las etiquetas de `Estado` en la gráfica de barras horizontal.
- **Tarea**: Ajustar el eje categórico de la gráfica de `repda_2021` para que se visualicen todos los estados sin omitir etiquetas.
- **Prompt**: not all the "Estados" are shown. adjust x-axis (y-axis in horizontañ) so all the labels from "Estado" are displayed
- **Resultado**: Se actualizó la celda de visualización para forzar ticks categóricos completos (`tickmode="array"`, `tickvals`, `ticktext`), aumentar altura dinámica de la figura y ampliar márgenes del eje de etiquetas. Tras ejecutar la celda, se mostraron todas las etiquetas de `Estado`.
- **Decisión**: Para gráficas horizontales con muchas categorías, fijar ticks explícitos y escalar altura/márgenes para evitar ocultamiento o recorte de etiquetas.

### No usamos IA para hacer lo siguiente: