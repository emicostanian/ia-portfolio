---
title: "Práctico 14 — LLMs con LangChain (OpenAI): Prompting, Plantillas y Salida Estructurada"
date: 2025-12-02
---

# Práctico 14 — LLMs con LangChain (OpenAI): Prompting, Plantillas y Salida Estructurada

**Notebook:** `Practico_14_LLMs_LangChain.ipynb`

## Parte 1 — Setup y exploración inicial
- Se instalan las librerías principales: `langchain`, `langchain-openai`, `langsmith`, `langchain-core` y opcionales como `faiss-cpu`, `chromadb` y `langchain-text-splitters`.  
- Se configuran variables de entorno: `OPENAI_API_KEY` y `LANGSMITH_API_KEY`.  
- Se crea un modelo base con `ChatOpenAI(model="gpt-5-mini", temperature=0)` para obtener respuestas deterministas.  
- Se realiza una primera invocación simple (“Hello LLM”) para verificar correcto funcionamiento.  
- Observaciones: el modelo responde de forma estable con temperatura baja.

## Parte 2 — Control de parámetros y prompting con plantillas
- Se experimenta con `temperature` (0.0, 0.5, 0.9), `max_tokens` y `top_p` para observar efectos en creatividad, variación y concisión.  
- Se prueba un conjunto fijo de prompts para comparar estilo y calidad bajo diferentes configuraciones.  
- Se diseña un `ChatPromptTemplate` que separa instrucciones del contenido, permitiendo prompts reutilizables y más controlados.  
- Se conecta el template al modelo usando LCEL: `prompt | llm`.  
- Se agregan ejemplos mínimos (few-shot) para mejorar consistencia en tareas específicas como explicaciones cortas o clasificación.

## Parte 3 — Salida estructurada y observabilidad
- Se usa `with_structured_output(...)` para obtener respuestas JSON válidas y tipadas mediante modelos Pydantic (por ejemplo, títulos y bullets).  
- Se prueban tareas como resúmenes ejecutivos, traducciones deterministas y extracción de entidades usando esquemas fijos.  
- Se registra información de tokens y latencia habilitando `LANGSMITH_TRACING=true`.  
- Observaciones: la salida estructurada evita errores de parseo y facilita integrarla en pipelines; LangSmith ayuda a evaluar costos y performance.

## Parte 4 — Mini-pipelines y RAG básico
- Se implementan pequeñas cadenas como:  
  - Traductor determinista en JSON.  
  - Resumen con estructura fija (Intro / Hallazgos / Recomendación).  
  - Q&A usando solo el contexto provisto, con detección de insuficiencia.  
- Se aplica un RAG simple: textos locales → split → embeddings → FAISS → recuperación → LLM.  
- Se compara esta estrategia con pasar el contexto crudo, observando menor alucinación y mayor precisión.

## Conceptos clave
- **Temperature, max_tokens, top_p:** permiten regular creatividad, longitud y diversidad.  
- **ChatPromptTemplate:** mejora reutilización y control sobre las instrucciones.  
- **LCEL:** conecta componentes de forma declarativa.  
- **Structured Output:** asegura JSON válido mediante modelos tipados.  
- **Embeddings + Vector Stores:** base para RAG y recuperación semántica.  
- **LangSmith:** soporte para observabilidad y análisis de rendimiento.

## Reflexión
- ¿Qué combinación de parámetros produjo el mejor balance entre claridad y variedad?  
- ¿Cuándo few-shot mejoró la consistencia frente a zero-shot?  
- ¿Qué ventajas concretas aporta la salida estructurada al integrar LLMs en un sistema real?  
- ¿Dónde se observan límites de un RAG simple sin corpus grande?  
- ¿Qué aspectos del pipeline serían prioritarios al llevarlo a producción?

## Referencias
- Documentación oficial de LangChain y OpenAI.  
- ChatPromptTemplate y LCEL (API Python).  
- Structured Output — JSON y Pydantic.  
- LangSmith — tracing y métricas.  
- Text Splitters y Vector Stores (FAISS/Chroma).  
- Guía de parámetros de generación de OpenAI.
