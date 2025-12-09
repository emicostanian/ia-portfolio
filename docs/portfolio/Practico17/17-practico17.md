---
title: "Lab — Vertex AI Pipelines: Qwik Start (GSP965)"
date: 2025-12-05
---

# Lab — Vertex AI Pipelines: Qwik Start (GSP965)

**Lab:** Vertex AI Pipelines: Qwik Start (Google Cloud Skills Boost — GSP965)

## Parte 1 — Contexto y objetivos

- Vertex AI unifica en una sola API los modelos AutoML y los modelos personalizados, junto con productos de MLOps como **Vertex AI Pipelines**.  
- Las **pipelines** permiten:
  - Automatizar y reproducir flujos de ML (preprocesamiento, entrenamiento, tuning, evaluación, despliegue).  
  - Descomponer el flujo en pasos independientes (cada uno en su propio contenedor).  
  - Compartir, versionar y volver a ejecutar workflows de forma confiable.  
- Objetivos del lab:
  - Usar el **Kubeflow Pipelines SDK (KFP)** para construir pipelines escalables.  
  - Crear y ejecutar una pipeline introductoria de 3 pasos que arma una frase con un emoji.  
  - Crear y ejecutar una pipeline de ML que entrena, evalúa y despliega un modelo de clasificación tabular con AutoML.  
  - Usar componentes preconstruidos de `google_cloud_pipeline_components` para interactuar con Vertex AI.  
  - Programar ejecuciones de pipeline con **Cloud Scheduler** (se menciona en los objetivos del lab).

## Parte 2 — Setup en Vertex AI Workbench y entorno de pipelines

- Acceso al entorno:
  - Ingreso a la consola de Google Cloud con credenciales temporales del lab.  
  - Navegación a **Vertex AI → Workbench** y apertura de la instancia (JupyterLab).  
- En el notebook:
  - Creación de un notebook Python 3 y renombrado.  
  - Instalación de librerías:
    - `google-cloud-aiplatform`
    - `kfp`
    - `google-cloud-pipeline-components`
    - Ajuste de versiones para `shapely`, `pygeos`, `geopandas`.  
  - Reinicio del kernel tras la instalación y verificación de versiones de KFP y componentes.  
- Definición de configuración:
  - Obtención del `PROJECT_ID` desde `gcloud`.  
  - Definición del bucket: `BUCKET_NAME = "gs://" + PROJECT_ID + "-labconfig-bucket"`.  
  - Importación de módulos clave:
    - `kfp`, `kfp.v2.dsl`, `kfp.v2.compiler`, `AIPlatformClient`.  
    - `google.cloud.aiplatform` y `google_cloud_pipeline_components.aiplatform as gcc_aip`.  
  - Definición de constantes:
    - `REGION` (por ej. `us-central1` según la región del lab).  
    - `PIPELINE_ROOT = f"{BUCKET_NAME}/pipeline_root/"` como ruta de artefactos de la pipeline.

## Parte 3 — Pipeline introductoria de componentes personalizados (emoji pipeline)

- Componentes Python definidos con el decorador `@component`:
  - `product_name(text: str) -> str`  
    - Devuelve el nombre del producto recibido por parámetro.  
  - `emoji(text: str) -> NamedTuple("Outputs", [("emoji_text", str), ("emoji", str)])`  
    - Usa la librería `emoji` (instalada vía `packages_to_install`) para convertir el código de emoji (por ejemplo `"sparkles"`) en el carácter correspondiente (`✨`).  
    - Retorna tanto el texto original como el emoji.  
  - `build_sentence(product: str, emoji: str, emojitext: str) -> str`  
    - Construye la frase final del estilo: `"Vertex AI Pipelines is ✨"`, usando el emoji si existe, o el texto si no.  
- Definición de la pipeline con `@dsl.pipeline`:
  - `intro_pipeline(text="Vertex AI Pipelines", emoji_str="sparkles")`  
    - `product_task = product_name(text)`  
    - `emoji_task = emoji(emoji_str)`  
    - `build_sentence` consume:
      - `product_task.output`  
      - `emoji_task.outputs["emoji"]`  
      - `emoji_task.outputs["emoji_text"]`  
- Ejecución:
  - Compilación a JSON: `intro_pipeline_job.json` con `compiler.Compiler().compile(...)`.  
  - Creación de cliente: `AIPlatformClient(project_id=PROJECT_ID, region=REGION)`.  
  - Ejecución: `create_run_from_job_spec("intro_pipeline_job.json")`.  
  - Monitoreo del run en la consola de Vertex AI Pipelines:
    - Visualización del grafo de 3 pasos.  
    - Inspección del output final en el componente `build_sentence`.

## Parte 4 — Pipeline end-to-end de AutoML Tabular (Dry Beans)

- Caso de uso:
  - Dataset **Dry Beans** (UCI): clasificación de 7 tipos de porotos en base a características tabulares.  
  - Objetivo de la pipeline:
    - Crear dataset en Vertex AI.  
    - Entrenar modelo AutoML de clasificación tabular.  
    - Evaluar métricas y registrar gráficos (ROC, matriz de confusión).  
    - Condicionalmente desplegar el modelo según un umbral de calidad.  
- Componente personalizado de evaluación (`classif_model_eval_metrics`):
  - Base image: `gcr.io/deeplearning-platform-release/tf2-cpu.2-3:latest`.  
  - Dependencia: `google-cloud-aiplatform`.  
  - Entradas:
    - `project`, `location`, `api_endpoint`.  
    - `thresholds_dict_str` (por ejemplo `{"auRoc": 0.95}`).  
    - `model` (artefacto de tipo `Model`).  
  - Salidas:
    - `metrics` (objeto `Metrics`).  
    - `metricsc` (objeto `ClassificationMetrics` con ROC y matriz de confusión).  
    - `dep_decision` (`"true"` o `"false"`) según si el modelo supera el umbral.  
  - Comportamiento:
    - Obtiene evaluaciones del modelo (`list_model_evaluations`).  
    - Loguea métricas y construye ROC y matriz de confusión en `metricsc`.  
    - Compara métricas (como `auRoc`) con los umbrales para decidir despliegue.  
- Pipeline de AutoML (`automl-tab-beans-training-v2`):
  - Parámetros de entrada:
    - `bq_source`: ruta a la tabla de BigQuery con el dataset (`"bq://aju-dev-demos.beans.beans1"`).  
    - `display_name`, `project`, `gcp_region`, `api_endpoint`.  
    - `thresholds_dict_str` para el umbral de despliegue.  
  - Componentes preconstruidos de `google_cloud_pipeline_components`:
    - `TabularDatasetCreateOp`:
      - Crea un dataset tabular en Vertex AI a partir de la tabla de BigQuery.  
    - `AutoMLTabularTrainingJobRunOp`:
      - Entrenamiento AutoML de clasificación:
        - Columnas numéricas para las features (Area, Perimeter, etc.).  
        - Columna objetivo: `Class`.  
        - Tiempo de entrenamiento configurado con `budget_milli_node_hours`.  
      - Produce un modelo (`outputs["model"]`).  
    - `ModelDeployOp`:
      - Despliega el modelo entrenado a un endpoint con `machine_type="e2-standard-4"`.  
  - Lógica condicional:
    - `model_eval_task = classif_model_eval_metrics(...)` procesando el modelo y sus métricas.  
    - Bloque `dsl.Condition(model_eval_task.outputs["dep_decision"] == "true")`:
      - Solo si la decisión es `"true"`, ejecuta `ModelDeployOp`.  
      - Si no, la pipeline termina sin desplegar modelo.  
- Compilación y ejecución:
  - Compilación a `tab_classif_pipeline.json`.  
  - Ejecución con `create_run_from_job_spec(..., parameter_values={...})`.  
  - La etapa de AutoML entrenamiento puede demorar más de una hora; el lab considera objetivo cumplido al iniciar correctamente el job.  
- Inspección en la consola:
  - Visualización del grafo del pipeline completo (dataset → training → eval → deploy condicional).  
  - Expansión de artefactos para ver:
    - Dataset creado.  
    - Modelo AutoML.  
    - Endpoint desplegado (si aplica).  
    - Métricas con matriz de confusión y curva ROC (`metricsc`).  
  - Uso de **Lineage**:
    - Ver cómo la dataset, el modelo y el endpoint se relacionan a través de las ejecuciones.  
- Comparación de runs (opcional):
  - Uso de `aiplatform.get_pipeline_df(pipeline="automl-tab-beans-training-v2")` para obtener un DataFrame con metadatos de runs.  
  - Permite comparar métricas, tiempos y configuraciones entre ejecuciones.

## Conceptos clave

- **Vertex AI Pipelines:** orquestador de flujos de ML desacoplados en pasos contenedorizados y reproducibles.  
- **Kubeflow Pipelines SDK (KFP):** API principal para definir componentes (`@component`) y pipelines (`@dsl.pipeline`).  
- **PIPELINE_ROOT:** ruta en Cloud Storage donde se almacenan los artefactos (datasets, modelos, métricas).  
- **Componentes personalizados vs preconstruidos:**
  - Personalizados para lógica específica (e.g. evaluación y umbrales).  
  - Preconstruidos para operaciones estándar con Vertex AI (dataset, training, deploy).  
- **Condicionales en pipelines:** `dsl.Condition` para controlar flujos de despliegue según métricas.  
- **Artefactos y lineage tracking:** permiten entender qué ejecuciones crearon qué recursos y cómo se usan.  

## Reflexión

- ¿Qué ventajas concretas aporta usar una pipeline frente a scripts sueltos o notebooks monolíticos (reproducibilidad, colaboración, despliegue repetible)?  
- ¿Cómo ayuda separar el flujo en componentes independientes (mantenibilidad, debugging, reuso entre proyectos)?  
- ¿Qué criterios de negocio usarías para fijar umbrales de despliegue (ej. auROC mínimo, precisión por clase, costo de errores)?  
- ¿Cómo aprovecharías lineage y `get_pipeline_df` para auditar modelos, experimentos y versiones en un contexto real de MLOps?  
- ¿Qué partes del lab extenderías (por ejemplo, cambiar el dataset, los hiperparámetros de AutoML o el criterio de despliegue)?

## Referencias

- Documentación de Vertex AI Pipelines (Google Cloud).  
- SDK de Kubeflow Pipelines (KFP).  
- Librería `google_cloud_pipeline_components` (componentes preconstruidos de Vertex AI).  
- Documentación de Vertex AI AutoML Tabular y Vertex AI Prediction.  
- Google Cloud Skills Boost — Lab “Vertex AI Pipelines: Qwik Start” (GSP965).
