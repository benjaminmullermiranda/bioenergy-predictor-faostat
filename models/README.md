# Carpeta de modelos

Esta carpeta incluye los artefactos mínimos para que la aplicación Streamlit pueda cargar el predictor sin reentrenar en cada despliegue:

- `bioenergy_model.joblib`: pipeline entrenado con `HistGradientBoostingRegressor`.
- `model_metadata.json`: métricas, columnas, categorías y valores por defecto para la interfaz.

Para regenerar el modelo desde cero:

```bash
python src/run_pipeline.py
```

Los datos pesados (`data/raw`, `data/database` y CSV procesados grandes) siguen ignorados para mantener el repositorio ligero.

