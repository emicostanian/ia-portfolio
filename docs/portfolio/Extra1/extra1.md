---
title: "Extra 1 — KNN vs SVM en Breast Cancer"
date: 2025-10-13
---

# Extra 1 — KNN vs SVM en Breast Cancer (scikit-learn)

**Notebook:** [Extra 1](Extra1.ipynb)

## Contexto
Extiendo la práctica de **clasificación** usando el dataset **Breast Cancer** (scikit-learn) para comparar dos modelos populares: **KNN** y **SVM (RBF)**. Uso **pipelines** con **StandardScaler**, **GridSearchCV** para búsqueda de hiperparámetros y métricas estándar de evaluación.

## Objetivos
- Construir *pipelines* con **escalado + modelo**.
- Realizar **búsqueda de hiperparámetros** con validación cruzada (cv=5).
- Evaluar con **classification report**, **matriz de confusión** y **ROC-AUC**.
- Comparar rendimiento entre **KNN** y **SVM (RBF)**.

## Datos y preparación
- Dataset: `load_breast_cancer()` (no requiere archivos externos).
- Partición estratificada: `train_test_split(test_size=0.2, random_state=42)`.
- Preprocesamiento: **StandardScaler** dentro del *pipeline*.

## Modelos y búsqueda
- **KNN**  
  - *Grid*: `n_neighbors ∈ [3,5,7,9,11]`, `weights ∈ {uniform, distance}`, `p ∈ {1,2}`.  
  - Métrica de CV: `f1`.
- **SVM (RBF)**  
  - *Grid*: `C ∈ [0.1,1,3,10]`, `gamma ∈ {scale, 0.01, 0.1, 1}`.  
  - Métrica de CV: `f1`.

## Evaluación
- **Classification report** (precision, recall, f1 por clase y ponderadas).
- **Matriz de confusión**.
- **ROC-AUC** (usando probabilidades del mejor modelo por grid).
- **Tabla comparativa** con `f1_cv_best`, `precision/recall/f1` en test y `AUC`.

## Hallazgos (resumen)
- **SVM (RBF)** suele obtener mejores métricas globales en este dataset cuando está bien escalado y con `C/gamma` razonables.
- **KNN** puede acercarse si se elige correctamente `n_neighbors` y el esquema de pesos, pero es más sensible al ruido local y a la escala.
- El **escalado** es crítico para ambos modelos.

## Reflexión
Este extra refuerza el flujo visto en las prácticas de clasificación: *pipeline → búsqueda → evaluación sólida*. Posibles **próximos pasos**: calibración de probabilidades, otros kernels (poly), *feature selection* y validación con *stratified K-fold* más amplio.

## Referencias
- scikit-learn: `SVC`, `KNeighborsClassifier`, `Pipeline`, `GridSearchCV`, métricas.
