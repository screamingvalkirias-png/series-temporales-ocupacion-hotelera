# Resumen de resultados

La serie analizada contiene **793 observaciones diarias**, desde
**2015-07-01** hasta **2017-08-31**. La variable
objetivo es la cantidad total de huéspedes asociados a reservas no canceladas
con fecha de llegada en cada día.

Se compararon cuatro estrategias: Naive, Seasonal Naive, SARIMA y XGBoost.
El mejor resultado según RMSE correspondió a **XGBoost**, con:

- RMSE: **41.880**
- MAE: **33.818**
- MAPE: **15.026%**
- sMAPE: **15.075%**
- MASE: **0.660**

En comparación con el baseline Seasonal Naive, el modelo ganador
redujo el RMSE en
**12.44%**.

El modelo SARIMA seleccionado utilizó `order=(0, 1, 1)` y
`seasonal_order=(1, 1, 1, 7)`. XGBoost fue ajustado mediante
validación temporal y utilizó los hiperparámetros
`{'learning_rate': 0.03, 'max_depth': 3, 'n_estimators': 200, 'subsample': 0.8}`. Su pronóstico se generó recursivamente para
evitar utilizar valores reales del conjunto de prueba en los rezagos.

## Interpretación

SARIMA obtuvo RMSE=42.117, MAE=33.481
y MAPE=14.929%. XGBoost obtuvo
RMSE=41.880, MAE=33.818 y
MAPE=15.026%. La comparación debe interpretarse junto con
los gráficos de predicción y el diagnóstico de residuales.

## Limitaciones

- La serie representa llegadas, no habitaciones ocupadas durante toda la estadía.
- El dataset incluye únicamente variables internas de reservas.
- No se incorporaron feriados, eventos, clima, promociones o precios externos.
- El período histórico 2015–2017 puede no representar condiciones turísticas actuales.
- El horizonte de prueba corresponde a un único bloque final de 60 días.

## Trabajo futuro

- incorporar variables exógenas;
- evaluar otros horizontes mediante validación walk-forward;
- probar ETS, Prophet, LightGBM, LSTM o N-BEATS;
- modelar por separado cada tipo de hotel;
- construir una variable de habitaciones ocupadas por noche;
- generar intervalos probabilísticos para modelos de machine learning.