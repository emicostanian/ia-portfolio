---
title: "Práctico 13 — Fine-tuning de Transformers para Clasificación Ofensiva (ES)"
date: 2025-12-02
---

# Práctico 13 — Fine-tuning de Transformers para Clasificación Ofensiva (ES)

**Notebook:** `Practico_13_Finetuning_Transformers.ipynb`

## Parte 1 — Dataset y exploración

- Dataset principal: `zeroshot/twitter-financial-news-sentiment` (Hugging Face), ~12k ejemplos, 3 clases (`0=Bearish`, `1=Bullish`, `2=Neutral`).  
- Carga con `load_dataset(...)` y normalización a un `DataFrame` con columnas estándar:
  - `text`: contenido del tweet.
  - `label`: entero 0/1/2 (mapeo desde strings si es necesario).  
- Limpieza básica:
  - `dropna()` y reseteo de índices.
- EDA inicial:
  - Cálculo de longitud en tokens por tweet y visualización de su distribución (histograma).
  - Revisión de `value_counts()` de las etiquetas para detectar desbalance.  
- EDA adicional:
  - Distribución de clases en gráfico de barras.
  - Top n-grams por clase usando `CountVectorizer` (p. ej., n-grams (1,2), topk reducido).
  - WordCloud por clase para inspeccionar términos frecuentes.
- EDA avanzada:
  - Representación TF-IDF + PCA/UMAP para visualizar separabilidad entre clases.
  - Entrenamiento exploratorio de Word2Vec para ver vecinos semánticos de palabras de interés.

## Parte 2 — Baseline clásico (TF-IDF + Logistic Regression)

- División train/test con `train_test_split`:
  - `test_size` típico (p. ej., 0.2).
  - `stratify=df["label"]` para preservar la distribución de clases.
- Pipeline baseline:
  - `TfidfVectorizer` con `max_features` limitado y `ngram_range` (1,2) o (1,3).
  - `LogisticRegression` como clasificador; ajuste de `max_iter` y posible `class_weight="balanced"` si no converge o hay desbalance fuerte.
- Entrenamiento:
  - `pipe.fit(X_train, y_train)` con datos de texto crudo.
- Evaluación:
  - `classification_report` (accuracy, precision, recall, F1 por clase).
  - Matriz de confusión en heatmap.
- Observaciones:
  - Identificación de clases donde más falla el baseline (p. ej., confusión entre Neutral y una de las polaridades).
  - Relación entre n-grams elegidos y capacidad de capturar patrones léxicos ofensivos/financieros.

## Parte 3 — Fine-tuning con Transformers

- Preparación de splits para Transformers:
  - Nuevo `train_test_split` con estratificación, generando `train_df` y `test_df`.
  - Conversión a `Dataset` de Hugging Face y renombrado de columna `label` → `labels`.
  - Conversión opcional a `ClassLabel(num_classes=3)`.
- Selección de checkpoints:
  - Lista de candidatos, por ejemplo:
    - `ProsusAI/finbert` (orientado a finanzas).
    - Otros modelos genéricos como alternativas.
  - Función que intenta cargar el primer checkpoint disponible con `AutoTokenizer` y `AutoModelForSequenceClassification(num_labels=3)`.
- Tokenización:
  - Uso del tokenizer BPE del checkpoint, con truncation y padding.
  - Visualización rápida de tokens para frases de ejemplo.
- Entrenamiento (fine-tuning):
  - Definición de `TrainingArguments` con hiperparámetros típicos:
    - `learning_rate` en el orden de `2e-5`.
    - `per_device_train_batch_size` moderado (según GPU).
    - `num_train_epochs` en el rango 3–5.
    - `weight_decay` para regularización ligera.
    - `eval_strategy="epoch"` y `load_best_model_at_end=True`.
  - Función `compute_metrics` con `accuracy` y `f1` (average `"macro"` para multiclase).
  - Entrenamiento con `Trainer`, pasando datasets tokenizados, tokenizer y métrica.

## Parte 4 — Evaluación y comparación

- Historial de entrenamiento:
  - Extracción de `eval_accuracy` y `eval_f1` del `trainer.state.log_history`.
  - Gráfico de curvas de validación por época (accuracy y F1).  
- Evaluación final del modelo Transformer:
  - `trainer.evaluate()` sobre el split de test.
  - Predicciones con `trainer.predict(...)` y cálculo de `accuracy` y `macro-F1` en el conjunto de test.  
- Comparación Baseline vs Transformer:
  - Cálculo de accuracy y `macro-F1` para el baseline TF-IDF+LR.
  - Tabla/dict con métricas de ambos métodos para ver mejora relativa.
- Observaciones:
  - El Transformer suele mejorar `macro-F1` especialmente en clases minoritarias.
  - Costo mayor en tiempo de entrenamiento y VRAM frente al baseline clásico.

## Conceptos clave

- **BoW / TF-IDF vs Transformers:**  
  - BoW/TF-IDF captura patrones de palabras y n-grams, pero no contexto profundo.  
  - Transformers aprovechan embeddings contextuales y suelen mejorar en tareas semánticas complejas.
- **Desbalance de clases:**  
  - Justifica el uso de `macro-F1` como métrica principal.  
  - Puede requerir `class_weight` u otras técnicas en modelos clásicos.
- **Tokenización y truncation:**  
  - La distribución de longitudes guía la elección de `max_length` y la estrategia de truncamiento.
- **Fine-tuning:**  
  - Ajustar pocos hiperparámetros (LR, epochs, batch size) puede impactar mucho en performance.  
  - `num_labels` y esquema de métricas deben ser coherentes con el problema.
- **Visualización:**  
  - PCA/UMAP de TF-IDF y curvas de entrenamiento ayudan a entender separabilidad y comportamiento del modelo.

## Reflexión

- ¿Qué tanto mejora el modelo Transformer respecto al baseline clásico en términos de `macro-F1` y errores por clase?  
- ¿En qué escenarios seguiría siendo razonable usar TF-IDF+LR (simplicidad, costo, interpretabilidad)?  
- ¿Cómo influye el desbalance de clases en las decisiones de métrica, modelo y recolección de datos?  
- ¿Qué ajustes de fine-tuning (más datos, más epochs, otros checkpoints) podrían mejorar aún más el rendimiento?  
- ¿Qué rol jugarían técnicas adicionales (data cleaning, re-etiquetado, RAG, traducción a ES) en un sistema de producción?

## Referencias

- Hugging Face Datasets: carga y normalización de datasets de texto.  
- Transformers (Hugging Face): `AutoTokenizer`, `AutoModelForSequenceClassification`, `Trainer`.  
- scikit-learn: TF-IDF, LogisticRegression, métricas de clasificación y visualización de matriz de confusión.  
- evaluate / métricas: accuracy y F1 (macro/binary).  
- UMAP, PCA: proyecciones para visualizar separabilidad.  
- Documentación de FinBERT y otros checkpoints de clasificación de texto.
