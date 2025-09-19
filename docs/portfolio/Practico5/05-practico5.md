---
title: "Práctico 5 — Reducción de dimensionalidad (PCA & t-SNE)"
date: 2025-09-17
---

# Práctico 5 — Reducción de dimensionalidad (PCA & t-SNE)

**Notebook:** [05-practica5.ipynb](05-practica5.ipynb)

## Contexto
Aplicamos técnicas de **reducción de dimensionalidad** para visualizar datos de alta dimensión y/o preparar features para modelos: **PCA** como método lineal y **t-SNE** como método no lineal orientado a visualización.

## Objetivos
- Estandarizar los datos y aplicar **PCA** (varianza explicada, componentes).
- Proyectar a **2D/3D** y comparar con **t-SNE**.
- Discutir qué estructura revela cada método (clusters, separabilidad).

## Datos y preparación
- Dataset: *(indicar dataset usado en el notebook)*.
- Preprocesado:
  - `StandardScaler` (o el que se use) antes de PCA/t-SNE.
  - Selección de variables numéricas.

## PCA
- Ajuste: `PCA(n_components=2 or 3, random_state=42)`.
- **Varianza explicada acumulada**: (anotar % observado).
- **Carga de componentes**: (variables con mayor peso).

## t-SNE
- Ajuste: `TSNE(n_components=2, perplexity=30, random_state=42)`.
- Comparación con PCA: ¿mejor separabilidad visual? ¿agrupa sub-estructuras?

## Visualizaciones
- **Scatter 2D** de PCA y t-SNE con color por clase/cluster.
- (Opcional) **3D** para PCA (`n_components=3`) si aporta.

## Hallazgos
- PCA: (resumen breve de patrones observados).
- t-SNE: (patrones/agrupamientos que surgen).
- Observación: t-SNE no preserva distancias globales; se usa para **insight visual**, no para features directos sin cuidado.

## Reflexión
- ¿Qué método te resultó más informativo para este dataset?
- Próximos pasos: **UMAP**, **HDBSCAN** / **KMeans** tras PCA, búsqueda de hiperparámetros (perplexity, learning rate) en t-SNE.

## Referencias
- scikit-learn: `PCA`, `TSNE`, `StandardScaler`.
- Artículos introductorios de PCA/t-SNE.
