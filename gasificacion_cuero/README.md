# Gasificación de residuos de cuero con Machine Learning y XAI

Predicción de **H₂ (vol%)** y **CH₄ (vol%)** en procesos de gasificación de residuos de cuero usando modelos de _Machine Learning_ (ML) y técnicas de _Explainable AI_ (XAI).

Dataset base:  
Kaggle – Gasification Dataset  
[kaggle gasification-dataset](https://www.kaggle.com/datasets/miracnurciner/gasification-dataset)

Artículo científico asociado:  [Doi accesss](https://doi.org/10.1016/j.jenvman.2025.126521Received ) 
## 1. Contexto

La industria del cuero genera residuos sólidos complejos (leather scraps, lodos, residuos con cromo) con alto impacto ambiental. La gasificación permite transformar estos residuos en **syngas** (H₂, CH₄, CO), habilitando:

- Valorización energética de residuos industriales
- Reducción de impacto ambiental
- Integración en esquemas de economía circular

La composición del syngas depende de múltiples variables termodinámicas, cinéticas y operativas (T, tiempo de residencia, tipo de catalizador, etc.), lo que convierte el problema en un sistema altamente no lineal.

## 2. Dataset

- 3748 muestras experimentales  
- Datos generados bajo condiciones controladas de laboratorio
- Variables incluyen:
    - Tiempo: Duración total del proceso de gasificación desde el inicio hasta la finalización (min).
    - Temperatura: Temperatura ambiente o de alimentación (°C).
    - Temperatura de proceso: Temperatura del reactor (°C).
    - Tipo de agente: Tipo de gas reactivo que habilita la conversión térmica (p. ej., aire, oxígeno).
    - Caudal del agente: Caudal del gas agente (L/min).
    - Tipo de muestra: Tipo de materia prima/alimentación usada (p. ej., recortes de cuero, TWTS: lodos del tratamiento de residuos de curtiembre).
    - Tipo de catalizador: Tipo de catalizador empleado (p. ej., Al-Ni, polvo de mármol, ninguno).
    - Relación de catalizador: Proporción catalizador/alimentación (%).

- Variables objetivo:
    - Monóxido de carbono (CO): Porcentaje de CO en el gas de síntesis (vol%).
    - Dióxido de carbono (CO₂): Porcentaje de CO₂ en el gas de síntesis (vol%).
    - Metano (CH₄): Porcentaje de CH₄ en el gas de síntesis (vol%) - (Mayor importancia).
    - Oxígeno (O₂): Porcentaje de O₂ detectado en la salida (vol%).
    - Hidrógeno (H₂): Porcentaje de H₂ en el gas de síntesis (vol%) - (Mayor importancia).
    - Poder calorífico: Contenido energético del gas de síntesis (kcal/m³).

Este dataset es significativamente más grande que los datasets típicos en literatura (< 800 muestras).

## 3. Objetivo del Proyecto
1. Comparar múltiples modelos de regresión para predicción de H₂ y CH₄.
2. Identificar el modelo con mejor desempeño estadístico. 
3. Evaluar robustez mediante k-fold cross-validation.
4. Aplicar SHAP para interpretar contribuciones de variables.
5. Analizar relevancia físico-química de las variables dominantes.

## 4. Modelos Evaluados
- Random Forest (RF)
- Linear Regression (LR)
- Decision Tree (DT)
- Support Vector Regression (Linear y RBF)
- K-Nearest Neighbors (KNN)
- Gradient Boosting Regressor (GBR)
- XGBoost
- CatBoost
- LightGBM

### Métricas usadas
- R² (coeficiente de determinación)
- RMSE
- MAE
- MAPE
- EVS
- Test estadístico de Friedman (p < 0.001)

## 5. Resultados Clave
Según el artículo mstrado, el modelo con mejor desempeño es:
KNN
- H₂:
    - R² = 0.987
    - RMSE = 1.253
- CH₄:
    - R² = 0.979
    - RMSE = 0.920

El test de Friedman confirmó diferencias estadísticamente significativas entre modelos (p < 0.001).

Para efectos prácticos de este proyecto, vamos a verificar que tan cerca podemos llegar a este resultado

## 6. Explainable AI (XAI)

Se utilizó SHAP (Shapley Additive Explanations) para:
- Cuantificar importancia de variables
- Analizar contribuciones locales y globales
- Validar coherencia físico-química

Resultado principal:
- La temperatura es la variable dominante en la producción de H₂ y CH₄.
- El tiempo de residencia tiene influencia secundaria relevante.

La interpretación permite conectar el modelo estadístico con fundamentos termoquímicos.

## 7. Stack Tecnológico

| Herramienta                 | Descripción                                                                                          | ¿Para qué se usa en este proyecto?                                                                                       |
| --------------------------- | ---------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Python                      | Lenguaje de programación de alto nivel orientado a ciencia de datos e ingeniería.                    | Implementación completa del pipeline: preprocesamiento, modelado, validación cruzada y análisis explicable.              |
| NumPy                       | Librería para computación numérica basada en arreglos multidimensionales y operaciones vectorizadas. | Manipulación eficiente de matrices y operaciones matemáticas utilizadas por los modelos de ML.                           |
| Pandas                      | Librería para manipulación y análisis de datos estructurados.                                        | Carga del dataset, limpieza, transformación de variables y preparación de datos para entrenamiento.                      |
| Scikit-learn                | Librería de machine learning para clasificación, regresión, validación y métricas.                   | Implementación de modelos como KNN, RF, SVR, LR, DT y evaluación mediante R², RMSE, MAE, MAPE y k-fold cross-validation. |
| XGBoost                     | Implementación optimizada de Gradient Boosting basada en árboles.                                    | Comparación de desempeño frente a otros modelos en la predicción de H₂ y CH₄.                                            |
| LightGBM                    | Framework de Gradient Boosting eficiente y escalable.                                                | Evaluación de modelos boosting de alto rendimiento para mejorar precisión predictiva.                                    |
| CatBoost                    | Algoritmo de Gradient Boosting optimizado para variables categóricas.                                | Modelado robusto cuando existen variables categóricas relacionadas con tipo de catalizador o condiciones operativas.     |
| SHAP                        | Framework de Explainable AI basado en valores de Shapley.                                            | Interpretación global y local de la importancia de variables en la predicción de H₂ y CH₄.                               |
| Matplotlib / Seaborn/ bokeh | Librerías de visualización de datos en Python.                                                       | Visualización exploratoria (EDA), comparación de métricas y gráficos de importancia de variables (SHAP).                 |

## 9. Aporte Científico
- Dataset experimental amplio y público.
- Comparación sistemática de 9 modelos.
- Validación estadística formal.
- Integración de XAI en gasificación de residuos de cuero.
- Base reproducible para investigación futura en:
    - Biomass gasification
    - Waste-to-energy
    - ML aplicado a procesos termoquímicos
    - Optimización de producción de hidrógeno

## 10. Posibles Extensiones
- Optimización multiobjetivo (max H₂, min CH₄ no deseado)
- Integración con modelos cinéticos
- Implementación en entorno industrial
- Modelos híbridos físico-informados (Physics-Informed ML)
- Deployment como API (FastAPI + Docker)

## 11. Referencia

Cihan, P. et al. (2025).  
Prediction of hydrogen and methane yields from gasification of leather waste using machine learning and explainable AI.  
Journal of Environmental Management, 391, 126521.  
DOI: 10.1016/j.jenvman.2025.126521