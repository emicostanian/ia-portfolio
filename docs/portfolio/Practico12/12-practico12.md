---
title: "Práctico 12 — Segment Anything (SAM) para Flood Segmentation: Zero‑shot vs Fine‑tuning"
date: 2025-11-10
---

# Práctico 12 — Segment Anything (SAM) para Flood Segmentation: Zero‑shot vs Fine‑tuning

**Notebook:** [Practico_12_SAM_Flood_Segmentation.ipynb](Practico_12_SAM_Flood_Segmentation.ipynb)

## Parte 1 — Dataset y exploración
- Dataset: **Flood Area Segmentation** (Kaggle).  
  - 290 imágenes de áreas inundadas con máscaras binarias de agua.  
  - Carpeta `Image/` (imágenes .jpg) y `Mask/` (máscaras .png).  
  - Caso de negocio: monitoreo de inundaciones y respuesta a emergencias.  
- Carga y exploración: se verifican tamaños, ratio de agua y visualización de pares imagen/máscara.  

## Parte 2 — Inferencia con SAM preentrenado (zero‑shot)
- **Modelo:** Segment Anything (SAM) con `ViT‑B`.  
- **Prompts probados:**
  - **Point prompts:** punto dentro del área de agua.  
  - **Box prompts:** bounding box extraído desde la máscara GT.  
- Se calculan métricas: IoU, Dice, Precision, Recall.  
- Se comparan distribuciones para ambos tipos de prompts.  
- Observaciones: el SAM preentrenado detecta bordes generales pero falla en reflejos o áreas con baja saturación.  

## Parte 3 — Fine‑tuning de SAM
- Se crea `FloodSegmentationDataset` (PyTorch) con resize 1024×1024 y augmentations (`HorizontalFlip`, `Rotate`, `BrightnessContrast`).  
- Se entrena solo el **mask decoder**, congelando el `image_encoder` y `prompt_encoder`.  
- **Loss:** combinación de Binary Cross‑Entropy + Dice Loss.  
- **Optimizer:** Adam (`lr=1e‑4`) con scheduler StepLR.  
- **Entrenamiento:** 10–20 epochs, `batch_size=1‑2`.  
- Curvas de entrenamiento muestran incremento estable del IoU sin overfitting.  

## Parte 4 — Evaluación y comparación
- Se evalúan los modelos **pretrained** y **fine‑tuned** sobre el conjunto de validación.  
- Métricas promedio (ejemplo):
  - Pretrained IoU ≈ 0.45 ± 0.10  
  - Fine‑tuned IoU ≈ 0.72 ± 0.08  
  - Mejora significativa en contornos y reducción de falsos positivos.  
- Visualizaciones: comparación lado a lado (GT, predicción preentrenada, fine‑tuned, overlays).  
- Análisis de errores: reducción de *failure cases* >40% tras fine‑tuning.  

## Conceptos clave
- **Zero‑shot vs Fine‑tuning:** trade‑off entre generalización y especialización.  
- **Prompt engineering:** impacto de points vs boxes en la segmentación.  
- **IoU y Dice:** métricas complementarias de superposición.  
- **SAM architecture:** encoder congelado (ViT), decoder adaptable.  
- **Desafíos:** reflejos, sombras, objetos flotantes, agua turbia.  

## Reflexión
- ¿Por qué el pretrained SAM falla en inundaciones reales?  
- ¿Qué partes del modelo conviene ajustar en fine‑tuning?  
- ¿Qué cambios harías con más o menos datos?  
- ¿Está listo para deployment en entornos de respuesta a desastres?  

## Referencias
- [Segment Anything (SAM) — Meta AI](https://github.com/facebookresearch/segment-anything)  
- [Flood Area Segmentation Dataset — Kaggle](https://www.kaggle.com/datasets/faizalkarim/flood-area-segmentation)  
- Documentación de PyTorch, Albumentations y scikit‑image  
