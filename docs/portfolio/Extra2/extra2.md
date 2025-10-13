---
title: "Extra 2 — Regularización L1 vs L2 (Wine) + Curvas"
date: 2025-10-13
---

# Extra 2 — Regularización L1 vs L2 (Wine) + Curvas de Validación/Aprendizaje

**Notebook:** [Extra 2](Extra2.ipynb)

## Contexto
Como trabajo complementario, analizo el **efecto de la regularización** (`l1` vs `l2`) en **Regresión Logística** sobre el dataset **Wine** (multiclase). Incluyo **validation curve** (barrido de `C`) y **learning curve** para diagnosticar **sesgo/varianza**, y una mirada a la **esparsidad** de coeficientes con `l1`.

## Objetivos
- Comparar **L1** (esparsidad) vs **L2** (suavizado) en *multiclase*.
- Visualizar **validation curve** (accuracy vs `C`) para seleccionar complejidad.
- Visualizar **learning curve** para evaluar si conviene más datos o ajustar el modelo.
- Reportar **classification report** y **matriz de confusión**.

## Datos y preparación
- Dataset: `load_wine()` (scikit-learn).
- Partición estratificada: `train_test_split(test_size=0.2, random_state=42)`.
- **Pipeline** con `StandardScaler`.
- **Solver**: `saga` para soportar `l1` con `multi_class="multinomial"`.

## Modelos
- **LogisticRegression (L1)** y **LogisticRegression (L2)**, ambos con `max_iter=5000`.
- Comparación básica a `C=1.0` y análisis de sensibilidad vía `validation_curve` (`C ∈ [1e-3, 1e2]`).

## Evaluación y visualizaciones
- **Classification report** por clase y métricas ponderadas.
- **Validation curve** para `l1` y `l2` (diagnóstico de *under/overfitting*).
- **Learning curve** para `l2` (impacto del tamaño del conjunto de entrenamiento).
- **Esparsidad con L1**: porcentaje de coeficientes en cero y **top features** por clase (|coef|).

## Hallazgos (resumen)
- Con **L1** es posible lograr **coeficientes 0** (mejor interpretabilidad) sin perder demasiada accuracy si `C` es adecuado.
- **L2** tiende a ser **más estable**; la curva de aprendizaje ayuda a decidir si falta regularización o más datos.
- Las **curvas** permiten elegir `C` evitando tanto *underfitting* (C muy chico) como *overfitting* (C muy grande).

## Reflexión
El ejercicio conecta decisiones de **regularización** con **capacidad del modelo** y **tamaño de datos**, apoyándose en gráficos para justificar la selección de hiperparámetros. Como **extensiones**: *cross-validation* estratificada más amplia, *learning curve* para L1, *feature selection* y comparación con modelos no lineales.

## Referencias
- scikit-learn: `LogisticRegression` (penalty L1/L2, solver `saga`), `Pipeline`, `validation_curve`, `learning_curve`.
