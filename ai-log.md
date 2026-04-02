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

### No usamos IA para hacer lo siguiente: