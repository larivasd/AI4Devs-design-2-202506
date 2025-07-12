# Documento de Planificación Inicial para LTI: El ATS del Futuro

## 1. Generación de User Stories Clave

A continuación se presentan tres User Stories fundamentales para el arranque del producto, derivadas directamente de los casos de uso de gestión de ofertas y candidatos.

---
**ID:** US-001  
**Título:** Creación y publicación de ofertas de empleo  
**Como un:** Reclutador  
**Quiero:** Crear, guardar y publicar una nueva oferta de empleo con todos sus detalles (título, descripción, requisitos, etc.)  
**Para:** Atraer candidatos calificados y comenzar el proceso de selección de manera formal.  
**Criterios de Aceptación:**
* **Dado que** estoy en el panel de gestión de ofertas, **cuando** hago clic en "Crear Nueva Oferta", **entonces** se me presenta un formulario con campos para título, descripción, departamento, ubicación y requisitos.
* **Dado que** he llenado todos los campos obligatorios del formulario, **cuando** hago clic en "Guardar como Borrador", **entonces** la oferta se guarda en el sistema con un estado "Borrador" y no es visible públicamente.
* **Dado que** estoy editando una oferta en estado "Borrador", **cuando** hago clic en "Publicar", **entonces** la oferta cambia su estado a "Publicada", se vuelve visible en el portal de carreras y se genera una URL única para compartirla.

---
**ID:** US-002  
**Título:** Aplicación a una oferta de empleo  
**Como un:** Candidato  
**Quiero:** Enviar mi aplicación a una oferta de empleo publicada, adjuntando mi CV y completando mis datos básicos.  
**Para:** Poder ser considerado para la vacante que me interesa.  
**Criterios de Aceptación:**
* **Dado que** he encontrado una oferta de empleo de mi interés, **cuando** hago clic en el botón "Aplicar", **entonces** se me presenta un formulario de aplicación sencillo.
* **Dado que** estoy en el formulario de aplicación, **cuando** completo mi nombre, correo electrónico, teléfono y adjunto mi CV (en formato PDF o DOCX), **entonces** el botón "Enviar Aplicación" se habilita.
* **Dado que** he enviado mi aplicación, **cuando** el sistema la procesa exitosamente, **entonces** soy redirigido a una página de confirmación y recibo un correo electrónico automático agradeciendo mi postulación.

---
**ID:** US-003  
**Título:** Revisión de candidatos por oferta  
**Como un:** Reclutador o Manager de Contratación  
**Quiero:** Ver una lista de todos los candidatos que han aplicado a una oferta específica.  
**Para:** Poder revisar sus perfiles y decidir los siguientes pasos en el proceso.  
**Criterios de Aceptación:**
* **Dado que** estoy en la vista de una oferta "Publicada", **cuando** hago clic en la pestaña "Candidatos", **entonces** se muestra una lista con el nombre, fecha de aplicación y estado actual de cada candidato.
* **Dado que** veo la lista de candidatos, **cuando** hago clic en el nombre de un candidato, **entonces** puedo ver su perfil completo, incluyendo los datos que proporcionó y un enlace para descargar su CV.
* **Dado que** estoy visualizando el perfil de un candidato, **cuando** cambio su estado (ej. de "Nuevo" a "Preseleccionado"), **entonces** el cambio se guarda y se refleja inmediatamente en la lista principal de candidatos.

---

## 2. Desglose de User Story en Tickets de Trabajo

Se ha elegido la User Story de mayor prioridad, **US-001: Creación y publicación de ofertas de empleo**, para desglosarla en tickets técnicos.

---
**ID del Ticket:** T-001  
**Título:** Diseñar y migrar el esquema de la base de datos para "Ofertas"  
**Descripción:** Crear y ejecutar el script de migración para la tabla `job_offers` en la base de datos. La tabla debe incluir campos como `id`, `title`, `description`, `department`, `location`, `requirements`, `status` ('draft', 'published', 'archived'), `created_at`, `published_at`.  
**Tipo de Ticket:** Tarea Técnica  
**User Story Padre:** US-001  
**Criterios de Aceptación:**
* El script de migración se ejecuta sin errores en el entorno de desarrollo.
* La tabla `job_offers` existe en la base de datos con todos los campos y tipos de datos correctos.
* Se crea un script "down" para revertir la migración de forma segura.  

**Asignado a (Equipo Sugerido):** Backend / DevOps

---
**ID del Ticket:** T-002  
**Título:** Crear endpoint de API (POST/PUT) para guardar y actualizar ofertas  
**Descripción:** Desarrollar los endpoints de la API REST para manejar la creación (`POST /api/offers`) y actualización (`PUT /api/offers/{id}`) de las ofertas de empleo. Debe incluir lógica de validación de datos en el backend.  
**Tipo de Ticket:** Feature  
**User Story Padre:** US-001  
**Criterios de Aceptación:**
* Una solicitud `POST` a `/api/offers` con datos válidos crea un nuevo registro en la tabla `job_offers` y devuelve un código 201.
* Una solicitud `PUT` a `/api/offers/{id}` con datos válidos actualiza el registro correspondiente y devuelve un código 200.
* Enviar datos inválidos (ej. título vacío) a cualquiera de los endpoints devuelve un error de validación 422 con mensajes claros.  

**Asignado a (Equipo Sugerido):** Backend

---
**ID del Ticket:** T-003  
**Título:** Desarrollar la interfaz de usuario (UI) para el formulario de creación/edición de ofertas  
**Descripción:** Construir el componente de frontend (React/Vue/Angular) que renderiza el formulario para crear y editar ofertas. Debe incluir campos de texto, un editor de texto enriquecido para la descripción y botones para "Guardar como Borrador" y "Publicar".  
**Tipo de Ticket:** Feature  
**User Story Padre:** US-001  
**Criterios de Aceptación:**
* El formulario se muestra correctamente con todos los campos definidos.
* El estado del formulario se gestiona correctamente (carga, envío, errores).
* Los botones de acción llaman a los endpoints de la API correspondientes (creados en T-002) al ser presionados.  

**Asignado a (Equipo Sugerido):** Frontend

---
**ID del Ticket:** T-004  
**Título:** Plan de pruebas y ejecución para la gestión de ofertas  
**Descripción:** Crear y ejecutar un plan de pruebas integral para la funcionalidad completa de creación, guardado y publicación de ofertas. Esto incluye pruebas unitarias para el backend, pruebas de componentes para el frontend y pruebas E2E (End-to-End) del flujo completo.  
**Tipo de Ticket:** Tarea Técnica  
**User Story Padre:** US-001  
**Criterios de Aceptación:**
* La cobertura de pruebas unitarias para la lógica del backend es superior al 80%.
* El flujo E2E (abrir formulario -> llenar datos -> guardar borrador -> editar -> publicar) se ejecuta exitosamente en un entorno de pruebas automatizado.
* Se han verificado manualmente los casos de error y los flujos alternativos.  

**Asignado a (Equipo Sugerido):** QA / Fullstack

---

## 3. (Extra) Estimación de Esfuerzo de los Tickets

A continuación, se añade la estimación de esfuerzo a cada ticket utilizando Puntos de Historia (Story Points) basados en la secuencia de Fibonacci.

* **ID del Ticket: T-001** (Diseñar y migrar el esquema de la base de datos)
    * **Estimación (Story Points): 2**
    * **Justificación:** Tarea bien definida con baja complejidad. Requiere conocimiento de la estructura de la base de datos, pero la ejecución es rápida.

* **ID del Ticket: T-002** (Crear endpoint de API)
    * **Estimación (Story Points): 3**
    * **Justificación:** Es un trabajo de desarrollo estándar, pero implica lógica de negocio (validación, manejo de estados) que añade una complejidad moderada.

* **ID del Ticket: T-003** (Desarrollar la interfaz de usuario)
    * **Estimación (Story Points): 5**
    * **Justificación:** La creación de formularios robustos en el frontend suele ser compleja. Implica gestión del estado, validación en el cliente, interacción con un editor de texto enriquecido y comunicación asíncrona con la API, lo que representa un esfuerzo considerable.

* **ID del Ticket: T-004** (Plan de pruebas y ejecución)
    * **Estimación (Story Points): 3**
    * **Justificación:** Requiere la escritura de múltiples tipos de pruebas y la configuración del entorno de testing. Aunque no es desarrollo de una feature nueva, asegurar la calidad de una funcionalidad tan crítica requiere un esfuerzo significativo.

## 4. Creación, Priorización y Análisis del Product Backlog con el proceso de "Prompt Engineering"

#### Prompt #1 (Básico):

"Creación y Priorización del Product Backlog

Tarea: Arma un Product Backlog inicial. Incluye las User Stories que creaste en el paso anterior y al menos otras 2 que consideres esenciales para un Producto Mínimo Viable (MVP).
Metodología: Prioriza el backlog utilizando el método MoSCoW (Must-have, Should-have, Could-have, Won't-have).
Formato: Presenta el backlog como una tabla con las siguientes columnas: ID, User Story (Título), Prioridad (MoSCoW)."

**Resultado generado con el Prompt #1:**

# Backlog de Implementación de Módulo de Reclutamiento

Este es el Product Backlog inicial para el MVP de LTI, priorizado con la metodología MoSCoW para asegurar la entrega de valor fundamental en las primeras iteraciones.

| ID       | User Story (Título)                        | Prioridad (MoSCoW) |
| :------- | :----------------------------------------- | :----------------- |
| **US-004** | Inicio de sesión seguro para usuarios internos | **Must-have** |
| **US-001** | Creación y publicación de ofertas de empleo      | **Must-have** |
| **US-002** | Aplicación a una oferta de empleo              | **Must-have** |
| **US-003** | Revisión de candidatos por oferta              | **Must-have** |
| **US-005** | Añadir notas y calificaciones a candidatos     | **Should-have** |

* **US-004 (Must-have):** Sin autenticación, no hay sistema. Es un requisito fundacional.
* **US-001, US-002, US-003 (Must-have):** Juntas, estas tres historias conforman el ciclo de vida mínimo y esencial de un ATS: publicar una vacante, recibir candidatos y poder revisarlos. Sin esto, el producto no cumple su propósito principal.
* **US-005 (Should-have):** Aunque la colaboración es un diferenciador clave, la primera versión funcional podría existir sin ella. Es la siguiente funcionalidad más importante a desarrollar justo después de los "Must-have" para empezar a cumplir la promesa de valor de la colaboración.

---

#### Prompt #2 (Básico):

"Crea un backlog para LTI con las user stories anteriores , estima por cada item en el backlog (genera una tabla markdown):

Impacto en el usuario y valor del negocio.
Urgencia basada en tendencias del mercado y feedback de usuarios.
Complejidad y esfuerzo estimado de implementación.
Riesgos y dependencias entre tareas."

**Resultado generado con el Prompt #2:**

### 🎯 Lista de Historias de Usuario Priorizadas

**US-001: Creación y publicación de ofertas de empleo**
* **Impacto y Valor:** Alto. Funcionalidad fundamental para el reclutador. Permite iniciar el proceso de contratación y es la base del sistema.
* **Urgencia:** Muy Alta.
* **Complejidad y Esfuerzo:** Medio (5-8 puntos).
* **Riesgos y Dependencias:** Bajo riesgo. Depende del backend para la gestión de estados (borrador/publicado).

**US-002: Aplicación a una oferta de empleo**
* **Impacto y Valor:** Muy Alto. Crucial para la experiencia del candidato y la tasa de conversión.
* **Urgencia:** Muy Alta.
* **Complejidad y Esfuerzo:** Medio (5-8 puntos).
* **Riesgos y Dependencias:** Requiere integración con servicios de carga de archivos (CVs) y envío de correos de confirmación. Depende de la existencia de ofertas (US-001).

**US-003: Revisión de candidatos por oferta**
* **Impacto y Valor:** Alto. Esencial para la gestión y toma de decisiones del reclutador una vez que las aplicaciones llegan.
* **Urgencia:** Alta.
* **Complejidad y Esfuerzo:** Medio-Alto (8-13 puntos).
* **Riesgos y Dependencias:** Depende directamente de US-001 (ofertas) y US-002 (aplicaciones).

---

### 📊 Matriz de Estimación (Resumen)

| ID | Título | Urgencia | Esfuerzo Estimado |
| :--- | :--- | :--- | :--- |
| **US-001** | Creación y publicación de ofertas de empleo | Muy Alta | 5-8 Puntos |
| **US-002** | Aplicación a una oferta de empleo | Muy Alta | 5-8 Puntos |
| **US-003** | Revisión de candidatos por oferta | Alta | 8-13 Puntos |

---

### Análisis Detallado (Información Adicional)

**US-001 (Creación y publicación de ofertas):** La complejidad radica en el desarrollo de la UI/UX del formulario, la implementación de la lógica de estado (Borrador vs. Publicada) y la generación de la URL única.

**US-002 (Aplicación a una oferta):** Implica manejar la carga de archivos, la validación de datos del formulario y la integración con servicios de correo electrónico para las notificaciones automáticas.

**US-003 (Revisión de candidatos):** Requiere una interfaz de gestión robusta, incluyendo la visualización de listas, la vista de perfil completo, la descarga de CVs y la funcionalidad para actualizar el estado del candidato en el sistema.

#### Prompt #3 (Mejorado)

"Asunto: Solicitud de Creación de Backlog y Estimación para el Módulo de Reclutamiento (LTI)

Objetivo: Crear un backlog priorizado y estimado para la fase inicial del desarrollo del Módulo de Reclutamiento de LTI, basándose en las user stories provistas.

Contexto del Proyecto: El equipo de desarrollo de LTI está iniciando la implementación de un sistema de reclutamiento integrado. Las siguientes tres historias de usuario representan la funcionalidad crítica para la creación de ofertas de empleo (Reclutadores), la aplicación de candidatos (Candidatos) y la revisión de perfiles (Reclutadores/Managers de Contratación).

Instrucciones para el Análisis y Estimación:

Para cada una de las historias de usuario (US-001, US-002, US-003), realice una estimación y análisis detallado considerando los siguientes criterios:

Impacto en el Usuario y Valor del Negocio: Evalúe la importancia de la funcionalidad para los roles clave (Reclutador, Candidato, Manager de Contratación) y su contribución directa a los objetivos de negocio de LTI (e.g., eficiencia en la contratación, experiencia del candidato).
Urgencia: Determine la prioridad de implementación basándose en la necesidad operativa inmediata, las tendencias del mercado y el feedback potencial de los usuarios.
Complejidad y Esfuerzo Estimado: Proporcione una estimación del esfuerzo de implementación (por ejemplo, en Puntos de Historia o Tallas T-Shirt: S, M, L, XL), considerando la complejidad técnica (frontend, backend, integración, pruebas).
Riesgos y Dependencias: Identifique posibles riesgos técnicos o de negocio, y las dependencias entre las historias de usuario (ej. ¿Qué historia debe completarse antes que otra?).

Formato de Entrega:

Presente los resultados en una tabla Markdown clara y estructurada que incluya las siguientes columnas para cada item del backlog:
ID de la Historia de Usuario
Título
Rol Principal del Usuario
Impacto y Valor
Urgencia
Esfuerzo Estimado
Riesgos y Dependencias Clave
Información Adicional Relevante:
Roles Involucrados: Reclutador, Candidato, Manager de Contratación.
Estado Inicial del Sistema: Se asume un entorno de desarrollo con la infraestructura básica para manejo de usuarios y datos de ofertas.
Nota: Utilice la información de las user stories US-001, US-002 y US-003 proporcionadas anteriormente como base para este análisis."

**Resultado generado con el Prompt #3:**

# Backlog y Estimación para el Módulo de Reclutamiento (LTI)

---

## Análisis y Priorización del Backlog

Este backlog priorizado y estimado detalla las historias de usuario iniciales para el Módulo de Reclutamiento de LTI. La estimación de esfuerzo se presenta en **Puntos de Historia** (basados en la secuencia de Fibonacci estándar).

| ID de la Historia de Usuario | Título | Rol Principal del Usuario | Impacto y Valor | Urgencia | Esfuerzo Estimado | Riesgos y Dependencias Clave |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **US-001** | Creación y publicación de ofertas de empleo | Reclutador | **Alto**. Fundamental para iniciar el proceso de contratación y atraer talento. | **Muy Alta**. Es la base operativa del sistema para el reclutador. | **5 - 8 Puntos** | - Bajo riesgo.<br>- Dependencia: Backend para la gestión de estados (Borrador/Publicado) y generación de URL. |
| **US-002** | Aplicación a una oferta de empleo | Candidato | **Muy Alto**. Crucial para la experiencia del candidato y la conversión. Es el punto central de interacción externa. | **Muy Alta**. Permite la captura de talento y la consideración de candidatos. | **5 - 8 Puntos** | - Dependencia: US-001 (la oferta debe estar publicada).<br>- Riesgo: Integración con servicios de carga de archivos (CVs) y envío de correos electrónicos de confirmación. |
| **US-003** | Revisión de candidatos por oferta | Reclutador, Manager de Contratación | **Alto**. Esencial para la gestión eficiente del flujo de trabajo de contratación y la toma de decisiones. | **Alta**. Necesaria para el seguimiento inmediato una vez que las aplicaciones comienzan a llegar. | **8 - 13 Puntos** | - Dependencia: US-001 y US-002 (requiere ofertas publicadas y aplicaciones recibidas).<br>- Riesgo: Complejidad en la interfaz de gestión de estado. |

---

## Notas de Implementación

**Contexto de Roles:**
* **Reclutador:** Usuario interno responsable de gestionar ofertas y candidatos.
* **Candidato:** Usuario externo que aplica a las ofertas.
* **Manager de Contratación:** Usuario interno que revisa y aprueba candidatos.

**Estado del Sistema:**
Se asume que la infraestructura básica para autenticación de usuarios internos y el manejo inicial de datos está en su lugar para comenzar el desarrollo de estas funcionalidades.

#### Análisis y Conclusiones

# Análisis de Prompts para la Creación de Backlog

Este documento presenta un análisis comparativo de tres prompts utilizados para generar un Product Backlog y su estimación, y evalúa la efectividad de cada enfoque.

---

## Análisis de Prompts y Resultados

### Prompt #1 (Básico - MoSCoW)

**Análisis del Prompt:**
Este prompt es **sencillo y directo**. Define la tarea (crear backlog inicial y añadir 2 US esenciales para MVP), la metodología de priorización (MoSCoW) y el formato de salida. Carece de contexto y de instrucciones para una evaluación detallada más allá de la priorización.

**Análisis del Resultado:**
El resultado es **funcional y claro**. Cumple con la priorización MoSCoW y añade correctamente las dos US esenciales (`US-004`, `US-005`). La justificación para las prioridades es lógica.

### Prompt #2 (Básico - Estimación de Riesgos/Esfuerzo)

**Análisis del Prompt:**
Este prompt se enfoca en la **estimación técnica y de negocio**, solicitando un análisis de impacto, urgencia, complejidad, esfuerzo, riesgos y dependencias. Carece de estructura contextual (no especifica el rol, el proyecto ni el objetivo).

**Análisis del Resultado:**
El resultado proporciona un **análisis técnico y estratégico útil**. Las estimaciones de esfuerzo y la identificación de riesgos/dependencias son valiosas. Sin embargo, la presentación es algo fragmentada, lo que dificulta la visualización integral del backlog.

### Prompt #3 (Mejorado y Profesional)

**Análisis del Prompt:**
Este prompt es **altamente estructurado y profesional**. Define claramente el **Asunto**, **Objetivo** y **Contexto del Proyecto**. Proporciona instrucciones detalladas para el análisis y especifica explícitamente los **Roles** involucrados y el **Formato de Entrega** (una tabla completa).

**Análisis del Resultado:**
El resultado es **exhaustivo, coherente y altamente útil**. La tabla de Markdown es la más completa, incluyendo el rol principal del usuario y un análisis conciso de todos los criterios solicitados. El resultado está perfectamente alineado con los objetivos de un análisis de backlog profesional.

---

## Comparación y Reflexión sobre la Optimización

Los tres prompts generaron backlogs válidos, pero el **Prompt #3 (Mejorado)** demostró ser el más efectivo. Mientras el Prompt #1 se enfoca en la priorización rápida (MoSCoW) y el Prompt #2 en la estimación técnica, el **Prompt #3 integra ambos aspectos en un contexto definido**.

El resultado del **Prompt #3** es superior porque la **especificidad** en el prompt garantiza un análisis más profundo. Al definir el contexto, los roles y el formato de salida detallado, el prompt:

1.  **Alinea el Resultado con el Negocio:** Al especificar el contexto del proyecto (LTI) y los roles, el análisis de impacto y valor fue más relevante.
2.  **Mejora la Planificación:** La solicitud de estimación de esfuerzo y análisis de riesgos proporciona métricas cruciales para la planificación de sprints.
3.  **Optimiza el Formato de Salida:** La solicitud de una tabla estructurada y detallada facilitó la presentación de la información de forma clara y lista para su uso.

El **Prompt #3** no solo prioriza el backlog, sino que también entrega las herramientas esenciales para la planificación y ejecución, demostrando cómo la claridad y el detalle en las instrucciones resultan en un análisis de mayor calidad y utilidad.