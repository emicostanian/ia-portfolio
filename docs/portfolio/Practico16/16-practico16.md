---
title: "Lab — Recorrido por los labs prácticos de Google Cloud (GSP282)"
date: 2025-12-05
---

# Lab — Recorrido por los labs prácticos de Google Cloud (GSP282)

**Lab:** Recorrido por los labs prácticos de Google Cloud (Google Cloud Skills Boost — GSP282)

## Parte 1 — Contexto, plataforma y objetivos

- Google Cloud es un conjunto de servicios en la nube sobre la infraestructura de Google:
  - Cómputo, almacenamiento, redes.
  - Análisis de datos, machine learning, APIs y servicios gestionados.
- **Google Cloud Skills Boost**:
  - Plataforma donde se accede a labs, cursos, rutas de aprendizaje e insignias.
  - Usa tecnología de **Qwiklabs** para crear entornos temporales de práctica.
- Objetivos del lab:
  - Entender la interfaz y el funcionamiento general de los labs de Google Cloud.
  - Acceder a la consola de Google Cloud con credenciales temporales del lab.
  - Explorar proyectos, el menú de navegación, IAM y la biblioteca de APIs.
  - Comprender sistema de créditos, tiempo, puntuación y seguimiento de actividades.

## Parte 2 — Funcionamiento de un lab y acceso a la consola

- Componentes básicos de un lab:
  - **Comenzar lab (Start lab)**:
    - Crea un entorno temporal de Google Cloud y credenciales de estudiante.
    - Inicia un cronómetro con tiempo limitado (ej.: 45 minutos).
  - **Crédito**:
    - Indica el “costo” del lab en créditos (este es gratuito).
  - **Tiempo**:
    - Cuando llega a `00:00:00`, se elimina el entorno y se pierden los recursos.
  - **Puntuación / Seguimiento de actividades (Score)**:
    - Algunos labs exigen completar pasos específicos y verificarlos con “Revisar mi progreso”.
- Panel **Detalles del lab**:
  - Botón **Abrir la consola de Google Cloud**.
  - Nombre de usuario y contraseña temporales (identidad IAM tipo `student-xx-xxxxxx@qwiklabs.net`).
  - **ID del proyecto** temporal creado para el lab.
- Acceso a la consola:
  - Uso exclusivo de la cuenta de estudiante (no la cuenta personal o corporativa).
  - Aceptación de condiciones de servicio y política de privacidad.
  - La sesión se cierra y el acceso se pierde cuando se finaliza o expira el lab.

## Parte 3 — Proyectos de Google Cloud, navegación e IAM

- Proyectos de Google Cloud:
  - Unidad organizativa de recursos (VMs, bases de datos, redes, etc.).
  - Tienen **nombre**, **número** e **ID de proyecto único**.
  - El proyecto del lab es **temporal** y se elimina al terminar; se crea uno nuevo en cada lab.
- Proyecto **Qwiklabs Resources**:
  - Proyecto compartido de solo lectura.
  - Contiene archivos, imágenes y datasets para distintos labs.
  - No se usa para ejecutar los pasos del lab, ni se puede modificar ni borrar.
- Menú de navegación:
  - Ofrece acceso rápido a los servicios principales de Google Cloud.
  - Permite explorar todas las categorías: Compute, Storage, Networking, Operations, etc.
- IAM (Identity and Access Management):
  - Página **IAM** muestra usuarios, roles y permisos a nivel de proyecto.
  - Credencial de estudiante suele tener rol básico **Editor**:
    - Puede crear, modificar y borrar recursos, pero no gestionar miembros ni facturación.
  - Demostración: otorgar un rol adicional (por ejemplo, **Visualizador / Viewer**) a otra principal.
  - Roles básicos:
    - **Viewer:** solo lectura.
    - **Editor:** lectura + modificación de recursos.
    - **Owner:** todo lo anterior + gestión de permisos y facturación.

## Parte 4 — APIs, servicios y cierre del lab

- APIs y servicios:
  - Más de 200 APIs cubren áreas como ML, gestión empresarial, datos, etc.
  - En proyectos del lab, muchas APIs vienen habilitadas por defecto.
  - En proyectos propios, el usuario debe habilitar las APIs necesarias.
  - Biblioteca de APIs:
    - Búsqueda y habilitación de APIs (ejemplo: **Dialogflow API**).
    - Acceso a documentación y paneles de uso (tráfico, errores, latencia).
- Flujo típico en el lab:
  - Habilitar una API (Dialogflow), comprobar que quedó “Enabled”.
  - Probar la API desde la consola (Try this API) y revisar documentación.
- Finalización del lab:
  - Hacer clic en **Finalizar lab** y luego en **Enviar** para confirmar.
  - Se elimina el proyecto temporal y todos los recursos creados.
  - Posibilidad de calificar el lab y dejar comentarios.
  - La sesión en la consola se cierra automáticamente tras el cierre del lab.

## Conceptos clave

- **Lab de Google Cloud:** entorno temporal y guiado para practicar con recursos reales de Google Cloud.  
- **Credenciales temporales:** identidad IAM de estudiante con permisos acotados y válidos solo durante el lab.  
- **Proyecto de Google Cloud:** contenedor de recursos, servicios y configuraciones de seguridad.  
- **Qwiklabs Resources:** proyecto compartido de solo lectura usado como repositorio de datos para labs.  
- **IAM y roles básicos:** gestionan quién puede ver, modificar o administrar recursos (Viewer, Editor, Owner).  
- **APIs y servicios:** deben estar habilitados para poder usar las funcionalidades correspondientes (ej. Dialogflow).

## Reflexión

- ¿Por qué es importante separar tu cuenta personal/corporativa de la cuenta de estudiante del lab?  
- ¿Qué ventajas trae usar proyectos temporales para practicar (seguridad, limpieza, control de costos)?  
- ¿Cómo ayuda el sistema de seguimiento de actividades a asegurar que realmente realizaste los pasos del lab?  
- ¿Qué APIs activarías primero al empezar un proyecto real (según el tipo de solución que quieras construir)?  
- ¿Cómo organizarías múltiples proyectos en una organización grande (por equipo, por entorno, por producto)?

## Referencias

- Google Cloud Skills Boost — Lab “Recorrido por los labs prácticos de Google Cloud” (GSP282).  
- Documentación de Google Cloud Console y IAM.  
- Documentación de proyectos de Google Cloud.  
- Biblioteca de APIs y servicios de Google Cloud.  
- Documentación de Dialogflow API y Explorador de APIs de Google.
