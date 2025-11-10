---
title: "Práctico 9 — Transfer Learning"
date: 2025-11-09
---

# Práctico 9 — Transfer Learning

**Notebook:** [09-Practica9.ipynb](09-Practica9.ipynb)

## Parte 1 — Dataset y preprocesamiento
- **Dataset:** CIFAR-10 (32×32×3). Loader: `keras.datasets.cifar10`.
- Normalización/escala y división en **train/valid/test** (según notebook).
- (Si corresponde) **Data augmentation** con capas `Random*`.
- Semillas fijadas y verificación de **GPU** (si aplica).

## Parte 2 — Arquitectura y entrenamiento
- **API:** Sequential.
- **Capas más relevantes:** Conv2D, MaxPooling2D, Flatten, Dense, Activation.
- **Compilación:** `optimizer=optimizers`, `loss=categorical_crossentropy`, `metrics=['accuracy']`.
- **Entrenamiento:** `epochs=10`, `batch_size=128`, validación: validation_data.
- **Callbacks:** EarlyStopping.

## Parte 3 — Evaluación y resultados
- Curvas de **loss/accuracy** (train/val) y **métricas de test**.
- (Opcional) **Matriz de confusión** y `classification_report` por clase.
- (Opcional) Conteo de **parámetros** y tiempos de entrenamiento.

## Parte 4 — Observaciones
- Comentarios sobre **sobreajuste/subajuste** y estabilidad.
- Qué **mejoras** funcionaron/mejorar (regularización, LR schedule, arquitectura).
- (Si aplica) Comparación con prácticos anteriores.

## Conceptos clave
- Relación entre el **modelo** y el **dataset**; por qué esta arquitectura.
- Impacto de **regularización/augmentation/callbacks** en el desempeño.

## Herramientas
- scikit-learn (métricas), Matplotlib.

## Referencias
- Documentación de **TensorFlow/Keras** y apuntes de clase.
