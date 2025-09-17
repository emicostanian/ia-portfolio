---
title: "Práctica 1 — Titanic: EDA rápida"
date: 2025-09-17
---

# Práctica 1 — Titanic: EDA rápida

## Contexto
Exploración inicial del dataset **Titanic** (Kaggle). El objetivo es familiarizarse con el conjunto de datos, revisar calidad (faltantes, tipos), y obtener primeras hipótesis sobre supervivencia.

**Notebook completo:** [01-practica1.ipynb](01-practica1.ipynb)

## Objetivos
- Cargar el dataset y verificar su estructura.
- Detectar valores faltantes y campos problemáticos.
- Obtener métricas y visualizaciones iniciales sobre **supervivencia** por variables clave.

## Datos y preparación
- Fuente: `kaggle competitions download -c titanic`.
- Archivos usados: `train.csv` y `test.csv`.
- Librerías: `pandas`, `numpy`, `matplotlib`, `seaborn`.

## Estructura del dataset
- **Dimensiones (train):** `891 x 12`
- **Columnas:** `PassengerId, Survived, Pclass, Name, Sex, Age, SibSp, Parch, Ticket, Fare, Cabin, Embarked`.

### Calidad de datos (faltantes)
- `Cabin`: **687** faltantes  
- `Age`: **177** faltantes  
- `Embarked`: **2** faltantes  
- Resto de columnas: sin faltantes

> Implica que `Cabin` probablemente no sea utilizable sin un tratamiento fuerte (o derivar features simples como “tiene_cabina = sí/no”). `Age` requiere imputación.

## Métricas rápidas
- **Tasa de supervivencia general (train):** ~**38.4%**  
  (proporciones observadas: 0 = 61.6%, 1 = 38.4%)

## Hallazgos visuales (resumen)
Se generaron 4 vistas:
1. **Supervivencia por sexo:** se observa mayor supervivencia en **mujeres**.
2. **Supervivencia por clase (`Pclass`):** mayor en **1ª clase** que en 2ª y 3ª.
3. **Distribución de `Age` por supervivencia:** supervivencia varía a lo largo de la edad; faltantes relevantes.
4. **Heatmap de correlaciones** (numéricas): relaciones esperables entre `Pclass`, `Fare` y `Survived`.

> Estas señales motivan features como `is_female`, `is_first_class`, y manejo explícito de edad.

## Pasos realizados (pipeline)
1. **Setup** de entorno y carpetas (`data/`, `results/`).
2. **Descarga** vía Kaggle (usa `kaggle.json`) y lectura de `train/test`.
3. **Exploración**: `shape`, `head`, `info`, `describe`.
4. **Calidad**: conteo de NA y orden por cantidad.
5. **Métricas**: proporción de `Survived`.
6. **Gráficos**: countplot por `Sex`, barplot por `Pclass`, histograma `Age` vs `Survived`, heatmap de correlaciones.

## Reflexión
- El set es **clásico para clasificación binaria**; hay un fuerte **leak** social (sexo/clase) que ya da señales predictivas.
- **Riesgos**: `Age` faltante, `Cabin` casi vacío, alta cardinalidad en `Ticket`/`Name`.
- **Próximos pasos**: imputación de `Age`, ingeniería de variables (familia a bordo, títulos en `Name`, bins de tarifa/edad), baseline con regresión logística o árbol.

## Referencias
- Documentación de Pandas/Seaborn utilizada en el notebook.
- Competencia “Titanic — Machine Learning from Disaster” (Kaggle).
