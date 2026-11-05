# EduRAG Agent – Evaluación Parcial N°2

## 1. Descripción del proyecto

**EduRAG Agent** es la segunda etapa del proyecto EduRAG, desarrollado para la asignatura **Ingeniería de Soluciones con Inteligencia Artificial**.

El proyecto implementa un agente académico orientado a estudiantes de Duoc UC, capaz de consultar documentación institucional, resumir información, generar planes académicos y mantener contexto mediante memoria de corto y largo plazo.

La solución utiliza un sistema **RAG (Retrieval-Augmented Generation)** desarrollado en la primera evaluación y lo integra con **LangGraph** para incorporar herramientas, memoria, planificación y toma de decisiones.

---

## 2. Objetivo

Desarrollar un agente inteligente capaz de:

- Consultar información académica desde documentos institucionales.
- Recuperar contexto mediante búsqueda semántica e híbrida.
- Resumir información relevante.
- Generar planes académicos basados en información recuperada.
- Identificar automáticamente la intención del usuario.
- Seleccionar diferentes rutas de ejecución.
- Mantener memoria de corto plazo durante una conversación.
- Mantener memoria de largo plazo entre distintas conversaciones.
- Actualizar información persistente cuando el usuario entrega nuevos datos.

---

## 3. Caso de uso

EduRAG está orientado principalmente a consultas relacionadas con:

- Reglamento Académico de Duoc UC.
- Práctica Profesional.
- Proceso de Titulación.
- Malla curricular de Ingeniería en Informática, mención Desarrollo de Software.

Ejemplos de consultas:

```text
¿En qué nivel está la práctica profesional?
```

```text
Resume la información sobre la práctica profesional.
```

```text
¿Qué debo hacer para realizar mi práctica profesional?
```

```text
¿En qué nivel estoy actualmente?
```

---

## 4. Arquitectura general

El agente utiliza el siguiente flujo:

```text
Usuario
   |
   v
Actualización de memoria
   |
   v
Identificación de intención
   |
   +----------------+----------------+----------------+
   |                |                |
   v                v                v
Consulta         Resumen           Plan
   |                |                |
   v                v                v
RAG            RAG + LLM        RAG + LLM
   |                |                |
   +----------------+----------------+
                    |
                    v
               Respuesta
```

El flujo es orquestado mediante **LangGraph** utilizando un estado compartido y rutas condicionales.

---

## 5. Componentes principales

### Sistema RAG

El sistema recupera información desde documentos institucionales mediante:

1. Extracción de texto desde PDF.
2. División del contenido en chunks.
3. Generación de embeddings.
4. Comparación semántica.
5. Comparación por coincidencia de palabras.
6. Ranking híbrido de resultados.

La búsqueda utiliza aproximadamente:

```text
70% similitud semántica
30% coincidencia de palabras
```

---

## 6. Documentos utilizados

El sistema utiliza los siguientes documentos:

- Reglamento Académico de Duoc UC 2026.
- Malla Curricular de Ingeniería en Informática, mención Desarrollo de Software.
- Reglamento sobre Proceso de Titulación de Duoc UC.

Para facilitar la recuperación de información desde la malla curricular, se incorporó además información estructurada sobre la asignatura:

```text
PPY4616 – Práctica Profesional
Nivel: 8°
```

---

## 7. Herramientas del agente

EduRAG dispone de tres herramientas principales.

### `consultar_documentos`

Realiza búsquedas en la base documental utilizando el sistema RAG.

Ejemplo:

```text
¿En qué nivel está la práctica profesional?
```

---

### `generar_resumen`

Genera resúmenes utilizando únicamente la información recuperada desde los documentos.

Ejemplo:

```text
Resume la información sobre la práctica profesional.
```

---

### `crear_plan_academico`

Genera planes académicos paso a paso utilizando información recuperada desde los documentos.

Ejemplo:

```text
¿Qué debo hacer para realizar mi práctica profesional?
```

El prompt utilizado limita al modelo para evitar inventar procedimientos, fechas, oficinas o requisitos que no estén presentes en las fuentes.

---

## 8. Identificación de intención

Antes de ejecutar una herramienta, el agente analiza la solicitud del usuario y la clasifica en una de tres categorías:

```text
consulta
resumen
plan
```

Ejemplo:

```text
Usuario:
¿Qué debo hacer para realizar mi práctica profesional?

Intención detectada:
plan
```

LangGraph utiliza esta clasificación para seleccionar automáticamente la ruta correspondiente.

---

## 9. Memoria de corto plazo

La memoria de corto plazo se implementa mediante:

```python
MemorySaver
```

Esta memoria permite conservar el historial dentro de un mismo `thread_id`.

Ejemplo:

```text
Usuario:
Estoy en 8° nivel.

Usuario:
¿En qué nivel te dije que estoy?

Agente:
Me indicaste que estás en 8° nivel.
```

---

## 10. Memoria de largo plazo

La memoria de largo plazo se implementa mediante un archivo JSON almacenado en Google Drive.

Ejemplo de información almacenada:

```json
{
    "nombre": "Miguel",
    "carrera": "Ingeniería en Informática",
    "nivel": "7° nivel"
}
```

Esto permite recuperar información incluso utilizando un nuevo `thread_id`.

---

## 11. Actualización adaptativa de memoria

El agente puede detectar información nueva entregada por el usuario y actualizar la memoria persistente.

Ejemplo:

```text
Memoria anterior:
8° nivel

Usuario:
Ahora estoy en 7° nivel.

Memoria actualizada:
7° nivel
```

Posteriormente, incluso en una nueva conversación:

```text
Usuario:
¿En qué nivel estoy actualmente?

Agente:
Actualmente estás en 7° nivel.
```

---

## 12. Decisiones adaptativas

El agente modifica su comportamiento dependiendo de la solicitud.

| Solicitud | Ruta |
|---|---|
| ¿En qué nivel está la práctica profesional? | Consulta |
| Resume la información sobre la práctica profesional | Resumen |
| ¿Qué debo hacer para realizar mi práctica profesional? | Plan |
| ¿En qué nivel estoy actualmente? | Consulta + memoria |

Esto permite que el sistema no ejecute siempre el mismo procedimiento.

---

## 13. Tecnologías utilizadas

- Python
- Google Colab
- Groq API
- OpenAI-compatible API
- LangChain
- LangGraph
- Sentence Transformers
- FAISS
- PyPDF
- Scikit-learn
- Google Drive
- JSON

Modelo de lenguaje:

```text
openai/gpt-oss-120b
```

Modelo de embeddings:

```text
sentence-transformers/all-MiniLM-L6-v2
```

---

## 14. Estructura del repositorio

```text
ia-eva1/
│
├── EVA1/
│   ├── eva1_1.ipynb
│   ├── README_EVA1.md
│   ├── documentos/
│   └── evidencias/
│
├── EVA2/
│   ├── evaluacion2.ipynb
│   ├── README_EVA2.md
│   ├── documentos/
│   └── evidencias/
│
└── README.md
```

---

## 15. Ejecución del proyecto

### 1. Abrir el notebook

Abrir:

```text
EVA2/evaluacion2.ipynb
```

en Google Colab.

### 2. Configurar la API Key

Crear un secreto en Google Colab llamado:

```text
GROQ_API_KEY
```

No se debe escribir la API Key directamente dentro del código.

### 3. Ejecutar las celdas en orden

Ejecutar el notebook desde la primera celda.

### 4. Cargar los documentos

Cuando el notebook lo solicite, cargar:

```text
NUEVO-REGLAMENTO-ACADEMICO_20260525.pdf
Malla-Curricular-1446116-2024-Ingenieroa-en-Informatica-Especializacion-en-Desarrollo-de-Software-1.pdf
Reglamento_Titulacion_Duoc_searchable.pdf
```

### 5. Autorizar Google Drive

El notebook solicitará acceso a Google Drive para almacenar la memoria persistente.

### 6. Ejecutar las pruebas

Al final del notebook se incluyen pruebas de:

- Consulta académica.
- Resumen.
- Planificación.
- Memoria de corto plazo.
- Memoria de largo plazo.
- Decisiones adaptativas.
- Orquestación mediante LangGraph.

---

## 16. Ejemplo de funcionamiento

### Entrada

```text
¿Qué debo hacer para realizar mi práctica profesional?
```

### Intención detectada

```text
plan
```

### Flujo

```text
Usuario
→ actualización de memoria
→ identificación de intención
→ nodo de planificación
→ recuperación RAG
→ generación del plan
→ respuesta
```

---

## 17. Manejo de información no disponible

EduRAG está configurado para evitar inventar información.

Cuando los documentos no especifican un procedimiento concreto, el agente debe indicarlo explícitamente.

Ejemplo:

```text
El documento consultado no especifica este procedimiento.
```

Esto permite reducir respuestas no respaldadas por la documentación institucional.

---

## 18. Limitaciones

Actualmente:

- La base documental está limitada a los documentos incorporados al proyecto.
- La memoria de largo plazo almacena un conjunto reducido de datos del estudiante.
- El sistema depende de conexión a Internet para utilizar la API de Groq.
- La API puede presentar límites temporales de tokens o solicitudes.
- El agente no realiza trámites reales dentro de plataformas institucionales.
- La información no presente en los documentos no debe considerarse información oficial de Duoc UC.

---

## 19. Evidencias

El notebook incluye pruebas de:

- Recuperación de información mediante RAG.
- Clasificación automática de intención.
- Cambio de ruta según la solicitud.
- Generación de resúmenes.
- Planificación académica.
- Memoria conversacional.
- Persistencia de datos entre conversaciones.
- Actualización de memoria.
- Visualización del grafo de orquestación.

Las capturas principales se almacenan en:

```text
EVA2/evidencias/
```

---

## 20. Autores

Proyecto desarrollado para la asignatura:

**Ingeniería de Soluciones con Inteligencia Artificial – ISY0101**

Duoc UC.
