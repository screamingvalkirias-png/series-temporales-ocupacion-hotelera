# Forecasting de Ocupación Hotelera

Proyecto final de Series Temporales. Predicción de llegadas diarias de huéspedes como proxy de ocupación hotelera.

**Integrantes:** Macarena Rios, Andrea Cristaldo

## 1. Descripción del problema

La ocupación hotelera es una variable clave para la gestión de recursos (personal, inventario, pricing dinámico). Este proyecto busca predecir la cantidad de llegadas diarias de huéspedes a partir de datos históricos de reservas, utilizando técnicas de forecasting de series temporales.

## 2. Dataset

- **Fuente:** [Hotel Booking Demand](https://www.kaggle.com/datasets/jessemostipak/hotel-booking-demand) (Kaggle)
- **Observaciones originales:** ~119,000 reservas individuales (2015-2017)
- **Serie temporal construida:** llegadas diarias de huéspedes (reservas no canceladas agrupadas por `arrival_date`)
- **Tamaño de la serie:** ~790 observaciones diarias
- **Variables:** `fecha` (index), `llegadas` (variable a predecir)

El dataset procesado se encuentra en `data/datos.csv`.

## 3. Metodología

1. **Construcción de la serie:** se reconstruyó la fecha de llegada a partir de año/mes/día, y se filtraron las reservas canceladas (`is_canceled == 0`), ya que no representan ocupación real.
2. **Preprocesamiento:** se completaron fechas faltantes en el rango temporal (relleno con 0) para asegurar una serie continua diaria.
3. **División train/test:** se reservaron los últimos 60 días como conjunto de prueba, respetando el orden temporal (sin shuffle).
4. **Feature engineering (para XGBoost):** variables de calendario (día de semana, mes, día del mes), lags (1, 7 y 14 días) y media móvil de 7 días.

## 4. Modelos implementados

| Modelo | Categoría | Descripción |
|---|---|---|
| **SARIMA** | Estadístico | `order=(1,1,1)`, `seasonal_order=(1,1,1,7)` — captura tendencia y estacionalidad semanal |
| **XGBoost** | Machine Learning | Regresión sobre features de calendario, lags y media móvil |

## 5. Resultados y métricas

| Modelo | RMSE | MAE | MAPE |
|---|---|---|---|
| SARIMA  | *ver `results/metricas.csv`* | | |
| XGBoost | *ver `results/metricas.csv`* | | |

> Las métricas exactas se generan al correr el notebook y quedan guardadas en `results/metricas.csv`.

**Métricas utilizadas:**
- **RMSE:** penaliza más los errores grandes, útil para detectar picos mal predichos.
- **MAE:** error absoluto promedio, más interpretable en la escala original (huéspedes/día).
- **MAPE:** error porcentual, permite comparar el desempeño independientemente de la magnitud.

## 6. Visualizaciones

Todas las imágenes se generan en `results/`:

- `serie_original.png` — Serie temporal completa de llegadas diarias
- `predicciones_vs_reales.png` — Comparación de ambos modelos contra los valores reales
- `comparacion_modelos.png` — Barras comparativas de RMSE/MAE/MAPE
- `residuales.png` — Análisis de residuales de cada modelo

## 7. Conclusiones

- [Completar tras correr el notebook: ¿qué modelo tuvo mejor desempeño y por qué?]
- [¿Se observan patrones estacionales claros? ¿Semanal, mensual?]
- [¿Los residuales muestran algún patrón no capturado por los modelos?]
- [Limitaciones: la serie no incluye eventos externos (feriados, promociones, pandemia) que podrían explicar variaciones.]
- [Trabajo futuro: incorporar variables exógenas, probar Prophet o LSTM, ajustar hiperparámetros de SARIMA con grid search.]

## 8. Estructura del repositorio

```
proyecto_ts/
│
├── data/
│   └── datos.csv
│
├── notebooks/
│   ├── 01_preprocesamiento.ipynb
│   ├── 02_modelos.ipynb
│   └── 03_evaluacion.ipynb
│
├── results/
│   ├── metricas.csv
│   ├── serie_original.png
│   ├── predicciones_vs_reales.png
│   ├── comparacion_modelos.png
│   └── residuales.png
│
├── README.md
└── requirements.txt
```

## 9. Cómo reproducir

```bash
pip install -r requirements.txt
```

Luego correr los notebooks en orden (`01` → `02` → `03`), o el script único si se usó todo en un solo notebook de Colab.
