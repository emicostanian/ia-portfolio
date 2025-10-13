


# Práctico 8 — MLP en CIFAR-10 con Keras/TensorFlow

**Notebook:** [Practico_8.ipynb](08-Practico8.ipynb)

## Parte 1 — Dataset y preprocesamiento
- Carga de **CIFAR-10** desde `keras.datasets.cifar10`.
- Etiquetas a vector 1D y **nombres de clases**: `['airplane','automobile','bird','cat','deer','dog','frog','horse','ship','truck']`.
- **Normalización** de imágenes a **[-1, 1]**:
  - `(x.astype("float32")/255.0 - 0.5) * 2.0`
- **Split de validación** (10%) a partir del conjunto de entrenamiento.
- **Aplanado** de imágenes `32×32×3 → 3072` para usar un **MLP** (capas densas).
- Semillas fijadas (`random/NumPy/TF`) y chequeo de **GPU** disponible.

## Parte 2 — Visualización rápida
- Muestra una grilla de imágenes aleatorias (2×8) con su **clase** como título.
- Re-escala temporalmente a `[0,1]` para poder **plotear** con Matplotlib.

## Parte 3 — MLP (clasificador denso) y entrenamiento
- Arquitectura **Sequential**:
  - `Dense(32, activation='relu', input_shape=(3072,))`
  - `Dense(32, activation='relu')`
  - `Dense(10, activation='softmax')`
- **Compilación**:
  - `optimizer='adam'`
  - `loss='sparse_categorical_crossentropy'`
  - `metrics=['accuracy']`
- **Entrenamiento**:
  - `epochs=5`, `batch_size=32`
  - `validation_data=(x_test, y_test)` *(además se separa un 10% de validación del train)*
  - **Callback**: `TensorBoard(log_dir=run_dir, histogram_freq=1)`
- **Evaluación**:
  - Imprime `Training Accuracy`, `Test Accuracy` y **parámetros totales** (`model.count_params()`).

## Parte 4 — Seguimiento con TensorBoard
- Carpeta base `tb_logs/experimentYYYYMMDD-HHMMSS`.
- Celdas mágicas para **lanzar TensorBoard** dentro del notebook:
  - `%load_ext tensorboard`
  - `%tensorboard --logdir tb_logs`

## Conceptos clave
- Un **MLP** sobre píxeles aplanados aprende límites no lineales, pero **no explota la estructura espacial** como una CNN.
- **Normalizar** entradas y **fijar semillas** mejora estabilidad y reproducibilidad.
- **TensorBoard** facilita el seguimiento de **loss/accuracy**, histogramas de activaciones y pesos.

## Reflexión
- Extender con:
  - **Matriz de confusión** y `classification_report` por clase.
  - **Regularización** (Dropout/L2), **EarlyStopping** y más **épocas**.
  - Comparar contra una **CNN** sencilla (Conv2D/MaxPool) para aprovechar la estructura 2D.
  - Usar el *split* de validación interno (x_val, y_val) para *tuning* y reservar `x_test` solo para **evaluación final**.

## Referencias
- TensorFlow/Keras: `keras.datasets.cifar10`, `keras.Sequential`, `layers.Dense`.
- TensorBoard para seguimiento de entrenamiento.
"""
path = "/mnt/data/Practico_8.md"
with open(path, "w", encoding="utf-8") as f:
    f.write(md_content)

path
