

# Práctico 11 — YOLOv8 Fine-tuning & Tracking

**Colab:** _Script de clase (código en la consigna)_

## Parte 1 — Setup e inferencia básica
- Instalación de dependencias: `ultralytics`, `opencv-python`, `matplotlib`, `numpy`, `pandas`, `seaborn`, `gdown`, `kaggle`, `norfair`.
- Verificación de **GPU** con `torch.cuda.is_available()`.
- Carga de **YOLOv8n (COCO)**: `YOLO('yolov8n.pt')` y chequeo de clases genéricas relevantes para *grocery* (apple, banana, orange, etc.).
- **Inferencia base** sobre imagen de góndola (`grocery_aisle.jpg`) con `conf=0.25` para evidenciar limitaciones del modelo generalista.

## Parte 2 — Fine-tuning en Fruit Detection Dataset
### 2.1 Descargar y validar dataset (Kaggle)
- Descarga del dataset **Fruit Detection** (`lakshaytyagi01/fruit-detection`).
- Verificación/creación de `data.yaml` con estructura YOLO: 
  - `train/images` + `train/labels`  
  - `valid/images` + `valid/labels`  
  - `nc`, `names` = \\[apple, banana, grapes, orange, pineapple, watermelon\\].

### 2.2 Exploración del dataset
- Conteo de instancias por clase leyendo `.txt` de labels.
- **Distribución** (bar chart): clases más/menos frecuentes y promedio por clase.
- Discusión de posibles **desbalances** y su impacto en el entrenamiento.

### 2.3 Visualización de ejemplos anotados
- Render de **bounding boxes** desde labels (formato YOLO → coordenadas absolutas) para 4 imágenes de `train/`.

### 2.4 Fine-tuning rápido
- Corrección de rutas en `data_fixed.yaml` (relativas al root del dataset).
- **Hiperparámetros sugeridos (Colab):**  
  `EPOCHS=10`, `BATCH_SIZE=16`, `IMAGE_SIZE=640`, `FRACTION=0.25` (para acelerar).
- Entrenamiento con `YOLO('yolov8n.pt').train(...)` → guarda `runs/detect/fruit_finetuned/*` y gráficos en `results.png`.

### 2.5 Carga del mejor modelo
- Carga de `best.pt` desde `training_dir/weights/best.pt`.
- Comparación de **clases** entre **COCO (80)** y **frutas específicas**.

### 2.6 Métricas de evaluación
- `model.val()` → reporte de `mAP@0.5`, `mAP@0.5:0.95`, `Precision`, `Recall` y **mAP por clase**.

### 2.7 Comparación visual antes vs después
- Inferencia con **modelo base** vs **fine-tuned** sobre imágenes de `valid/` (3 muestras).  
- Visualización lado a lado y conteo de detecciones por imagen.

### 2.8 Análisis de errores (FP/FN)
- Cálculo de **TP/FP/FN** por *matching* con **IoU ≥ 0.5** (misma clase).  
- Derivación de **Precision/Recall/F1** y comparación **Δ** entre base y fine‑tuned.

## Parte 3 — Tracking con modelo fine‑tuned
### 3.1 Video de prueba
- Descarga de video de frutas (Google Drive) a `videos/fruits_tracking.mp4`; lectura de FPS, resolución, duración.

### 3.2 Configuración de Norfair
- `Tracker(distance_function=mean_euclidean, distance_threshold=100, hit_counter_max=30, initialization_delay=2)`.
- Notas: threshold depende del tamaño del frame/velocidad; `hit_counter_max` tolera oclusiones; `initialization_delay` reduce **false positives**.

### 3.3 Aplicar tracking
- Detección frame a frame con `model_finetuned(frame, conf=0.25)`.
- Conversión a `Detection(points=[[x1,y1],[x2,y2]], data={{class_id}})`.
- Render de **ID persistente** por track, clase detectada y contador por frame.  
- Export a `videos/grocery_tracked.mp4`.

### 3.5 Análisis del tracking
- Estadísticas: cantidad de tracks, **duración promedio/máxima/mínima**, timeline de IDs, detecciones por frame y **tracks por clase**.
- Métricas de calidad: tracks cortos (<1s) y largos (>3s).

## Conceptos clave
- **Generalista vs específico**: COCO detecta clases genéricas; el **fine-tuning** especializa el detector al dominio *grocery*.
- **mAP, Precision, Recall**: lectura correcta de métricas y de su relación con FP/FN.
- **IoU** para *matching* entre predicciones y *ground truths*.
- **Tracking multi‑objeto**: asociación entre frames (distancia euclidiana media) y efecto de parámetros (`distance_threshold`, `hit_counter_max`, `initialization_delay`).

## Reflexión
- ¿Cuánto **mejoró** el mAP y las **FP/FN** tras el fine‑tuning?  
- Trade‑offs: **velocidad vs. precisión** (modelo *nano* y `FRACTION=0.25`), **epochs vs. tiempo**, **batch size vs. memoria**.  
- ¿Qué **hiperparámetro** ajustarías primero si tuvieras más tiempo (epochs, image size, LR, augmentations)?  
- Para *deployment* en retail: requerimientos de **FPS**, optimizaciones (cuantización, TensorRT), robustez ante **oclusiones**/iluminación.

## Referencias
- Ultralytics **YOLOv8** Docs y Training Guide.
- **Fruit Detection** dataset (Kaggle).
- **Norfair** (tracking multi‑objeto).
- Papers: **SORT**, **DeepSORT**, **ByteTrack**.
"""

