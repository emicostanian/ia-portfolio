---
title: "Práctico 6 — Clustering con K-Means (Mall Customers)"
date: 2025-09-17
---

# Práctico 6 — Clustering con K-Means (Mall Customers)

**Notebook:** [Practico 6](06-practico6.ipynb)

## Contexto
Segmentación de clientes del dataset **Mall Customers** para identificar grupos por **ingreso anual** y **spending score** (entre otras variables).

## Objetivos
- EDA rápida (distribuciones, relaciones, outliers).
- Comparar **escaladores** (MinMax, Standard, Robust) y su impacto en clustering.
- Aplicar **K-Means** y evaluar con **Silhouette Score**.

## EDA (resumen)
- Histogramas de `Age`, `Annual Income (k$)`, `Spending Score (1-100)`.
- Scatter plots entre variables (p. ej., `Age` vs `Income`, `Income` vs `Spending`).
- Detección de **outliers** (IQR).
- Correlaciones: no se observan relaciones lineales fuertes, pero sí **estructuras de grupo** en `Income`–`Spending`.

## Preprocesamiento
- Escalado probado con **MinMaxScaler**, **StandardScaler** y **RobustScaler**.
- Selección de variables relevantes (especialmente `Annual Income (k$)` y `Spending Score (1-100)`).

## Clustering
- Algoritmo: **K-Means** (`n_init=10`, `random_state=42`).
- Comparativa de **silhouette score** por tipo de escalado para elegir la mejor preparación de datos.
- (Opcional) Heurística del **codo** para orientar la elección de **K**.

## Hallazgos
- Aparecen grupos coherentes por nivel de ingreso y gasto.
- El **escalado** afecta la forma de los clusters y su **silhouette**.

## Reflexión
- Siguientes pasos: visualizar clusters con colores, perfilar cada segmento y evaluar alternativas (**DBSCAN**, **GMM**) si hay forma no esférica.

## Referencias
- scikit-learn: KMeans, silhouette_score, escaladores.
