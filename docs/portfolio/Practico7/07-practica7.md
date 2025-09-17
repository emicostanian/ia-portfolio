---
title: "Práctico 7 — Perceptrón (AND/OR/NOT/XOR) y MLP para XOR"
date: 2025-09-17
---

# Práctico 7 — Perceptrón (AND/OR/NOT/XOR) y MLP para XOR

**Notebook:** [07-practico7.ipynb](07-practico7.ipynb)

## Parte 1 — Perceptrón y compuertas lógicas
- Implementación de un **perceptrón** con pesos y bias.
- Pruebas con **AND**, **OR** y **NOT** (linealmente separables).
- Visualización de los puntos y la **recta de decisión**.
- **XOR**: se muestra que **no** es separable linealmente → el perceptrón falla.

## Parte 2 — Resolver XOR con MLP
- Dataset XOR: `[[0,0],[0,1],[1,0],[1,1]]` con etiquetas `[0,1,1,0]`.
- Modelo: **MLPClassifier** (scikit-learn).
  - `hidden_layer_sizes=(4,)`
  - `activation='tanh'` (podría usarse `relu`/`logistic`)
  - `solver='adam'`, `random_state=42`, `max_iter=2000`
- Entrenamiento y verificación de predicciones correctas.
- (Incluye) Función para **dibujar la arquitectura** de la red (capas y neuronas).

## Conceptos clave
- Un perceptrón solo traza **una** frontera lineal.
- Un **MLP** compone **múltiples fronteras** (no lineales) y resuelve XOR.
- Elección de **función de activación** y **capas ocultas** afecta capacidad de representación.

## Reflexión
- Extender a problemas reales: datasets no lineales, tuning de hiperparámetros y regularización.
- Visualizar **mapas de decisión** para comprender qué aprendió el modelo.

## Referencias
- scikit-learn: MLPClassifier.
- Material de perceptrón y compuertas lógicas.
