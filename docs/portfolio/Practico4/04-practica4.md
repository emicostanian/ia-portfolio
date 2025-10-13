---
title: "Práctico 4 — Regresión (Boston Housing) + Clasificación (Breast Cancer)"
date: 2025-09-17
---

# Práctico 4 — Regresión (Boston Housing) + Clasificación (Breast Cancer)

**Notebooks:** [Practico 4](04-practico4.ipynb)

## Parte A — Regresión: Boston Housing
### Objetivo
Predecir el precio medio de vivienda (target continuo) con **LinearRegression**.

### Flujo
1. Carga de datos (versión en CSV desde URL).
2. Selección de variables / split `train_test_split`.
3. Entrenamiento `LinearRegression().fit(...)`.
4. Predicción y evaluación.

### Métricas reportadas
- **MAE**, **MSE**, **RMSE**.
- **R²** (coeficiente de determinación).
- (Opcional en el notebook) **MAPE** para error porcentual.

### Comentarios
- Explorar correlaciones previas ayuda a detectar multicolinealidad.
- Estandarizar puede ser útil si luego se comparan con modelos penalizados.

---

## Parte B — Clasificación: Breast Cancer
### Objetivo
Clasificar tumores **benignos/malignos** con **LogisticRegression**.

### Flujo
1. Carga del dataset `load_breast_cancer()` (scikit-learn).
2. Split train/test.
3. Entrenamiento y predicción.
4. Reporte de métricas.

### Métricas reportadas
- **Matriz de confusión**.
- **Precision**, **Recall**, **F1-score** (classification report).

### Comentarios
- Buen ejercicio para comparar **precision vs recall** según el umbral.
- Probar estandarización y regularización para mejorar estabilidad.

## Reflexión
- Diferencias clave entre **regresión** y **clasificación** en flujo y métricas.
- Próximos pasos: comparar con **árboles** o **random forest** y validar con **cross-validation**.

## Referencias
- scikit-learn: LinearRegression, LogisticRegression y métricas.
