---
title: "Práctica 2 — Titanic: Clasificación con Regresión Logística"
date: 2025-09-17
---

# Práctica 2 — Titanic: Clasificación con Regresión Logística

**Notebook:** [Practico 2](02-practica2.ipynb)

## Contexto
Volvemos al dataset **Titanic** para construir un **modelo de clasificación binaria** con *LogisticRegression* (scikit-learn). El objetivo es predecir `Survived` a partir de variables demográficas y del viaje.

## Objetivos
- Preparar el dataset (selección de variables y partición train/test).
- Entrenar **Regresión Logística** y obtener **métricas de clasificación**.
- Analizar el **informe de clasificación** y la **matriz de confusión**.

## Datos y preparación
- Columnas típicas: `Pclass`, `Sex`, `Age`, `SibSp`, `Parch`, `Fare`, `Embarked` (entre otras).
- Tratamiento básico:
  - Conversión de categóricas a numéricas (p. ej., `Sex`, `Embarked`).
  - Imputación simple de faltantes (especialmente en `Age`).
  - `train_test_split(test_size=?, random_state=42)`.

## Entrenamiento
- Modelo: `LogisticRegression` (scikit-learn).
- Flujo:
  1. Selección de features.
  2. `fit(X_train, y_train)`.
  3. `predict(X_test)`.

## Evaluación
Se reportan:
- **Accuracy**.
- **Classification report** (precision, recall, f1).
- **Matriz de confusión** para interpretar aciertos y errores.

## Hallazgos (resumen)
- Las variables **sexo** y **clase** suelen aportar mucha señal.
- La imputación de `Age` impacta en la calidad del modelo.
- Útil explorar *features* derivados (p. ej., familia a bordo, binnings de tarifa/edad).

## Reflexión
- Próximos pasos: probar **regularización**, **balanceo** de clases si aplica, y **features** adicionales.
- Métricas adicionales: ROC-AUC y curva PR.

## Referencias
- Documentación de scikit-learn (LogisticRegression, métricas).
- Descripción de columnas del dataset Titanic.
