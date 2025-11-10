---
title: "Práctico 10 — Transfer Learning"
date: 2025-11-09
---

# Práctico 10 — Transfer Learning

**Notebook:** [Practica10.ipynb](Practica10.ipynb)

## Parte 1 — Dataset y preprocesamiento
- **Dataset:** Dataset propio/externo (—). Loader: `(ver notebook)`.
- Normalización/escala y división en **train/valid/test** (según notebook).
- (Si corresponde) **Data augmentation** con capas `Random*`.
- Semillas fijadas y verificación de **GPU** (si aplica).

## Parte 2 — Arquitectura y entrenamiento
- **API:** Functional API.
- **Capas más relevantes:** GlobalAveragePooling2D, Dense, RandomFlip, RandomRotation, RandomZoom, RandomTranslation, RandomContrast, RandomBrightness.
- **Compilación:** `optimizer=adam`, `loss=predictions, sparse_categorical_crossentropy`, `metrics=['accuracy']`.
- **Entrenamiento:** `epochs=5`, `batch_size=32`, validación: validation_data.
- **Callbacks:** —.

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
- Matplotlib.

## Referencias
- Documentación de **TensorFlow/Keras** y apuntes de clase.
