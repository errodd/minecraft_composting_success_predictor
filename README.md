# minecraft_composting_success_predictor

Proyecto de Aprendizaje Automatizado (Universidad de Carabobo, FACYT): predecir si un ítem introducido en un
compostador de Minecraft subirá su nivel.

## Notebooks

| Notebook | Contenido |
|---|---|
| `notebooks/01_eda.ipynb` | Análisis exploratorio de datos (Asignación 1) |
| `notebooks/02_data_preparation.ipynb` | Preparación de datos y feature engineering (Asignación 2, con las correcciones de la PR #2) |
| `notebooks/03_modeling_and_model_selection.ipynb` | Comparación de modelos, validación cruzada y optimización con Optuna (Asignación 3) |

Los notebooks se ejecutan a partir de los 16 CSV crudos de `data/`. El notebook 02 guarda en `artifacts/` el
manifiesto del split (`split_manifest.csv`) y sus metadatos (`data_preparation_metadata.json`); el notebook 03
reconstruye el split desde los datos crudos y se detiene si no coincide con esos archivos. Ambos están versionados,
así que el 03 puede ejecutarse directamente; si se cambia algo del 02, hay que volver a ejecutarlo antes del 03.

## Instalación del entorno

El proyecto usa [uv](https://docs.astral.sh/uv/) y Python 3.12 (fijado en `.python-version`).

1. Instalar uv (una sola vez):
   - Windows (PowerShell): `powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"`
   - macOS / Linux: `curl -LsSf https://astral.sh/uv/install.sh | sh`
2. Desde la raíz del repositorio, crear el entorno e instalar las dependencias exactas de `uv.lock`:
   ```
   uv sync
   ```
   Esto crea la carpeta `.venv/` con pandas, scikit-learn, Optuna, Jupyter y el resto de dependencias.
3. Abrir los notebooks:
   - **Jupyter:** `uv run jupyter lab` y abrir el notebook desde `notebooks/`.
   - **VS Code:** abrir el notebook y seleccionar como kernel el intérprete de `.venv`.

## Ejecución

Para reproducir los resultados, reiniciar el kernel y ejecutar todas las celdas
(*Kernel → Restart Kernel and Run All Cells*). También puede hacerse desde la terminal:

```
uv run jupyter nbconvert --to notebook --execute --inplace notebooks/02_data_preparation.ipynb
uv run jupyter nbconvert --to notebook --execute --inplace notebooks/03_modeling_and_model_selection.ipynb
```

El notebook 03 tarda unos 30–45 minutos en ejecutarse completo (la mayor parte es la búsqueda de hiperparámetros con
Optuna); el 02 tarda menos de un minuto.

Si aparece `ModuleNotFoundError` (por ejemplo, `No module named 'sklearn'`), el kernel no es el de `.venv`:
ejecutar `uv sync` y volver a seleccionar el kernel.
