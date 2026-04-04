# AI log - Tlamatipohua

## Herramientas
- Gemini
- Copilot

## Filosofía de uso
Decidimos usar IA exclusivamente para exploración inicial y elementos extremadamente técnicos.  La narrativa, la selección de variables y las decisiones editoriales fueron 100% del equipo.

## Registro de uso


### 2026-04-02 | GitHub Copilot (GPT-5.3-Codex) | Quitar numerales romanos de la columna RHA en gp.
- **Tarea**: Limpiar los nombres de las regiones en la columna RHA para eliminar el prefijo con número romano.
- **Prompt**: remove the roman numeral from the strings of the column "RHA" of gp
- **Resultado**: Se aplicó una expresión regular sobre `gp["RHA"]` para eliminar prefijos romanos (por ejemplo, I, II, III) y se verificó que los valores quedaran solo con el nombre de la región.
- **Decisión**: Mantener la limpieza de texto dentro del flujo del notebook para asegurar consistencia antes de generar `gp_largo` y los mapas.

### 2026-04-02 | GitHub Copilot (GPT-5.3-Codex) | Convertir `repda2020.vc` de texto a float corrigiendo la línea fallida.
- **Tarea**: Transformar la columna `vc` (leída como string) a tipo numérico flotante en el dataframe `repda2020`.
- **Prompt**: convert repda2020.vc from string to float, taking into account that the current code line does not succeed. after that, document this task in the same way as the last task
- **Resultado**: Se reemplazó la conversión directa `astype(float)` por una limpieza previa de separadores de miles y caracteres no numéricos, seguida de `pd.to_numeric(..., errors="coerce")`. La celda se ejecutó sin errores y `vc` quedó como valores flotantes.
- **Decisión**: Estandarizar conversiones numéricas con limpieza previa para columnas provenientes de Excel con formato textual (por ejemplo, `3,472.88`).

### 2026-04-02 | GitHub Copilot (GPT-5.3-Codex) | Corregir construcción de `repda` para incluir una columna por año.
- **Tarea**: Ajustar el ciclo de carga de REPDA para que no sobrescriba el dataframe en cada iteración y conserve una columna por cada año de `years`.
- **Prompt**: why with the code in this cell, repda only has one column. fix it to have one column for each year in years. after performing the task, document it the same way as the last task
- **Resultado**: Se identificó que en el bloque `else` se reasignaba `repda` en cada vuelta del ciclo. Se corrigió creando un dataframe temporal por año (`repda_year`) y asignando su serie a `repda[year]`. La ejecución final mostró columnas `Estado`, `2020`, `2021`, `2022` y `2023`.
- **Decisión**: Conservar el dataframe base con la columna llave (`Estado`) y anexar columnas anuales para evitar pérdida de datos por sobrescritura.

### 2026-04-02 | GitHub Copilot (GPT-5.3-Codex) | Corregir 404 de `rha2023.json` en Quarto Preview.
- **Tarea**: Evitar que Plotly intente cargar `../datos/rha2023.json` desde el navegador durante `quarto preview`, lo que causaba solicitudes a `/datos/rha2023.json` con 404.
- **Prompt**: apply change and document in ai-log.md
- **Resultado**: Se actualizó `dashboard/index.qmd` para cargar el GeoJSON con Python (`json.load`) y pasar un diccionario a `px.choropleth` en ambos mapas RHA. Con esto, la lectura del archivo ocurre en tiempo de render del kernel y ya no depende de una ruta HTTP relativa en el cliente.
- **Decisión**: Usar objetos GeoJSON cargados en Python para recursos locales en dashboards Quarto, en lugar de rutas de archivo como string que se resuelven en el navegador.

### 2026-04-02 | GitHub Copilot (GPT-5.3-Codex) | Reducir reinicios de sesión por render lento en Quarto Preview.
- **Tarea**: Evitar que celdas pesadas (lectura de múltiples archivos Excel) se ejecuten completas en cada refresco de `quarto preview`, lo que estaba alargando la ejecución de la celda 2/7 hasta provocar reinicio de sesión.
- **Prompt**: it lasted so long that the session restarted automatically
- **Resultado**: Se habilitó caché de ejecución en `dashboard/index.qmd` agregando `execute: cache: true` en el encabezado YAML. Con ello, Quarto reutiliza resultados de celdas no modificadas y reduce drásticamente los tiempos de re-render en preview.
- **Decisión**: Mantener caché activa para notebooks/dashboards con carga repetida de datos históricos, especialmente cuando se trabaja con `quarto preview` en modo watch.

### 2026-04-01 | Gemini | Exploración de datasets

- **Tarea**: Le pedimos ayuda para encontrar bases de datos abiertas.
- **Prompt**: "Ayudame a encontrar bases de datos abiertas sobre Mexico que me ayuden a responder a esto: Identificar la disponibilidad (cuantitativamente) en distintas localidades de México.
Identificar no solo cuánta hay en ciertas zonas, sino de dónde viene.
Visibilizar zonas de explotación.
Visibilizar datos reales sobre niveles de almacenamiento disponibles.
Visualizar la demanda en distintas localidades de México y sus usos.
Diferenciar demanda de acuerdo a distintos sectores: de consumo personal, industrial, agrícola, etc.
Identificar fenómenos relacionados con la disminución de precipitaciones.Mapear días con sequías o lluvias extremas y su impacto en la recuperación del recurso.
Impacto económico de cambios en la distribucion del agua en el país.
Identificar zonas críticas que necesitan atención y acción.
Visibilizar impacto de fugas.
Calcular la huella hídrica de la persona usuaria para explorar diferentes escenarios futuros de estrés hídrico. que datasets recomiendas?"
- **Resultado**: Nos recomendó links de instituciones mexicanas que llevan registro de datos que nos podrían ayudar a realizar esto.
- **Decisión**: Elegimos puntualmente aquella que de manera general nos ayudarán a alcanzar nuestros objetivos. 

### 2026-04-03 | GitHub Copilot (GPT-5.3-Codex) | Ajustar títulos y etiquetas de la gráfica horizontal de REPDA.
- **Tarea**: Ocultar el título del eje categórico (`Estado`), quitar el título de la leyenda y renombrar las series de leyenda a texto legible.
- **Prompt**: remove the y-axis title "Estado" so it does not appear on the chart. remove the default legend title so the legend does not have title. instead of cpc_Ml/hab and arpc_Ml/hab, write "Consumo" and "Agua renovable disponible" on the legend. document this change in ai-log.md
- **Resultado**: En `dashboard/index.qmd` se transformaron los valores de `Indicador` para mostrar `Consumo` y `Agua renovable disponible` en la leyenda. Además, se actualizó `fig.update_layout(...)` con `yaxis_title=None` y `legend_title_text=None` para eliminar ambos títulos en la visualización.
- **Decisión**: Mantener nombres de series orientados a lectura pública y evitar títulos redundantes en gráficos con leyendas autoexplicativas.

### 2026-04-03 | GitHub Copilot (GPT-5.3-Codex) | Sincronizar orden de leyenda y barras en gráfica horizontal de REPDA.
- **Tarea**: Alinear el orden visual de las barras con el orden de la leyenda para que `Consumo` (azul) aparezca primero y por encima de `Agua renovable disponible` (rojo).
- **Prompt**: make the order of the legend and the brs match: if Consumo (blue) goes first, put the blue bars above the red bars
- **Resultado**: En `dashboard/index.qmd` se definió explícitamente el orden de `Indicador` con `category_orders={"Indicador": ["Consumo", "Agua renovable disponible"]}`, se fijó `legend_traceorder="normal"` y se estableció `color_discrete_map` para mantener colores consistentes (`Consumo` azul, `Agua renovable disponible` rojo).
- **Decisión**: Fijar siempre orden y paleta de categorías en gráficas comparativas para evitar ambigüedad entre leyenda y posición de trazos.

### 2026-04-03 | GitHub Copilot (GPT-5.3-Codex) | Corregir prioridad visual de barras manteniendo orden de leyenda.
- **Tarea**: Ajustar el gráfico porque las barras rojas seguían apareciendo por encima de las azules, aunque la leyenda ya mostraba `Consumo` primero.
- **Prompt**: red bars are still above blue bars and on the legend, Consumo in blue is above Agua renovable disponible in red
- **Resultado**: Se invirtió el orden de trazado en `category_orders` a `['Agua renovable disponible', 'Consumo']` para que `Consumo` se dibuje después y quede visualmente arriba. Al mismo tiempo, se configuró `legend_traceorder='reversed'` para conservar en la leyenda el orden visible `Consumo` (azul) arriba de `Agua renovable disponible` (rojo).
- **Decisión**: Cuando hay conflicto entre orden de dibujo y orden de leyenda en barras agrupadas horizontales, separar ambos controles (orden de trazado y orden de leyenda) para lograr consistencia visual.

### 2026-04-03 | GitHub Copilot (GPT-5.3-Codex) | Corregir 404 de `/rha2023.json` durante `quarto preview`.
- **Tarea**: Resolver el error de vista previa donde Plotly solicitaba `/rha2023.json` y el servidor de preview respondía 404, aunque el render sí finalizaba.
- **Prompt**: still does not work when previewing:   /rha2023.json (404: Not Found)
- **Resultado**: Se ajustó `dashboard/index.qmd` para copiar `../datos/rha2023.json` al directorio de trabajo (`dashboard/rha2023.json`) antes de crear el mapa, manteniendo `geojson="rha2023.json"`. Luego se validó con `uv run quarto preview dashboard/index.qmd --no-browser --port 4313` y `curl http://127.0.0.1:4313/rha2023.json`, obteniendo código HTTP `200`.
- **Decisión**: En `preview`, garantizar explícitamente la presencia local de recursos estáticos usados por Plotly cuando el servidor no expone rutas relativas fuera del directorio del documento.

### 2026-04-03 | GitHub Copilot (GPT-5.3-Codex) | Crear gráfica de barras apiladas horizontales del uso de agua por sector y estado.
- **Tarea**: Construir una visualización que desagregue el volumen de agua concesionado por estado en sus 12 categorías de uso (Acuacultura, Agrícola, Agroindustrial, Doméstico, Comercio, Industrial, Múltiples, Otros, Pecuario, Público urbano, Servicios, Termoeléctricas), en formato de barras horizontales apiladas.
- **Prompt**: create a horizontal stacked bar chart with repda_2021: on y-axis, the column "Estado", and on the x-axis, the stacked values of all the water usage columns from D to F2. do not repeat colors and remove the uppercase letters on each use
- **Resultado**: Se creó una nueva celda en `scripts/001_EDA.ipynb` que genera la gráfica apilada mediante `plotly.graph_objects.Bar`. Se mapeó cada columna de uso a una etiqueta legible (ej: `'Acuacultura (D)'` → `'Acuacultura'`), se asignaron 12 colores distintos (azul, rojo, verde, púrpura, naranja, cian, rosa, verde claro, magenta, amarillo, azul oscuro, naranja oscuro) sin repetición, y se configuró `barmode='stack'`. El gráfico tiene altura dinámica, márgenes ajustados y leyenda posicionada a la derecha.
- **Decisión**: Usar desagregación visual por color sin etiquetas técnicas (letras de código REPDA) para mejorar accesibilidad y legibilidad de la composición sectorial del agua concesionada en cada entidad.

### 2026-04-03 | GitHub Copilot (GPT-5.3-Codex) | Crear gráfica de pastel por estado con selector desplegable.
- **Tarea**: Generar una segunda visualización para mostrar la composición de usos del agua por entidad con interacción por estado.
- **Prompt**: (solicitud en la sesión) construir gráfica de pastel con dropdown por estado para `repda_2021` usando las mismas categorías de uso y colores consistentes.
- **Resultado**: Se agregó una celda que crea una traza `go.Pie` por estado y un `updatemenus` tipo dropdown para alternar entre entidades. Se mantuvo el mismo mapeo de categorías y paleta de colores para consistencia con la barra apilada, además de `hovertemplate` con valor en Hm³/año y porcentaje.
- **Decisión**: Mantener consistencia de colores y etiquetas entre visualizaciones para permitir comparación rápida entre barra apilada y pastel sin reaprender codificación visual.

### 2026-04-03 | GitHub Copilot (GPT-5.3-Codex) | Crear mapa coroplético de consumo por uso con dropdown por categoría.
- **Tarea**: Construir una tercera visualización geográfica que muestre el volumen por estado para cada categoría de uso.
- **Prompt**: (solicitud en la sesión) crear mapa por estado con selector de uso de agua y conservar formato en Hm³/año.
- **Resultado**: Se implementó una figura `go.Choropleth` con una traza por categoría de uso, `featureidkey='properties.name'` y dropdown para cambiar la categoría visible. Se usó el GeoJSON de estados de México y se configuró `fitbounds='locations'` para ajustar el encuadre al país.
- **Decisión**: Usar un mapa por categoría seleccionable (en lugar de múltiples mapas simultáneos) para reducir saturación visual y mantener foco analítico por tipo de uso.

### 2026-04-03 | GitHub Copilot (GPT-5.3-Codex) | Integrar las 3 gráficas en pestañas Quarto y eliminar HTML custom.
- **Tarea**: Mostrar las tres visualizaciones en pestañas dentro del dashboard, sin bloques HTML/JS manuales.
- **Prompt**: do not use html chunks to display, graphs will be put in index.qmd, so use ::: {.panel-tabset}
- **Resultado**: Se retiró la estrategia de tabs en HTML/JavaScript y se migró a pestañas nativas de Quarto con `::: {.panel-tabset}` en `dashboard/index.qmd`, incluyendo tres chunks (barras, pastel y mapa).
- **Decisión**: Priorizar componentes nativos de Quarto para mejor mantenibilidad, compatibilidad en render y menor complejidad frente a incrustar HTML personalizado.

### 2026-04-03 | GitHub Copilot (GPT-5.3-Codex) | Depurar visualización final: mapa fuera del tabset y hover sin nombre de estado.
- **Tarea**: Corregir dos regresiones de presentación en la tercera gráfica: (1) mapa renderizado antes del tabset y (2) tooltip sin nombre de estado.
- **Prompt**: the tabset displays correctly but before the tabset, the map appears accidentaly; in map, the name of the state does not show when placin mouse on it
- **Resultado**: (1) Se evitó salida implícita del chunk asignando `_ = fig_map.update_layout(...)`, eliminando el render accidental previo al tabset. (2) Se agregó `customdata=df_usage[['Estado']]` y se cambió el `hovertemplate` a `%{customdata[0]}` para mostrar el nombre del estado al pasar el cursor.
- **Decisión**: Controlar explícitamente la salida final de chunks de Quarto/Notebook y anclar texto de hover a datos explícitos (`customdata`) para evitar dependencias de tokens implícitos de Plotly.





### 2026-04-04 | GitHub Copilot (GPT-5.3-Codex) | Corregir 404 de `/rha2023.json` en flujo Render to docs.
- **Tarea**: Resolver de forma simple el error de preview donde el navegador solicitaba `/rha2023.json` y devolvía 404 durante `uv run quarto preview dashboard/index.qmd`.
- **Prompt**: fix this as simply as possible
- **Resultado**: Se actualizó `dashboard/index.qmd` para que en `resources` se incluya `rha2023.json` (archivo local del proyecto Quarto) en lugar de `../datos/rha2023.json`. Después se ejecutó `uv run quarto render dashboard/index.qmd` y se verificó que el sitio generado en `docs/` contiene tanto `index.html` como `rha2023.json`.
- **Decisión**: Para despliegue en GitHub Pages con salida en `docs`, declarar en `resources` los archivos estáticos locales que deben copiarse al directorio publicado y evitar rutas fuera del proyecto en tiempo de cliente.

### No usamos IA para hacer lo siguiente:
