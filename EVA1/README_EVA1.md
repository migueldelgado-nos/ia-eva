# EduRAG - Asistente Académico Inteligente

## Descripción

EduRAG es un asistente académico desarrollado para estudiantes de Duoc UC.

El objetivo del proyecto es permitir que los estudiantes puedan realizar preguntas relacionadas con información académica y obtener respuestas basadas en documentos oficiales.

El sistema utiliza una arquitectura RAG (Retrieval-Augmented Generation), que permite recuperar información relevante desde documentos y utilizarla como contexto para generar una respuesta mediante un modelo de lenguaje.

## Caso utilizado

El proyecto utiliza como referencia a Duoc UC y trabaja principalmente con información relacionada con:

- Reglamento académico.
- Malla curricular de Ingeniería en Informática mención Desarrollo de Software.
- Práctica profesional.
- Proceso de titulación.
- Normativa externa relacionada con educación superior.

## Documentos utilizados

### Fuentes internas

- Reglamento Académico Duoc UC 2026.
- Malla Curricular de Ingeniería en Informática mención Desarrollo de Software.
- Reglamento sobre Proceso de Titulación de Duoc UC.

### Fuente externa

- Ley 21.790 de Chile, consultada desde la Biblioteca del Congreso Nacional.

## Funcionamiento del sistema

El funcionamiento general de EduRAG es el siguiente:

1. Se cargan los documentos académicos en formato PDF.
2. Se extrae el texto de los documentos.
3. El texto se divide en fragmentos o chunks.
4. Se generan embeddings de los fragmentos.
5. Los embeddings permiten realizar búsquedas por similitud.
6. El estudiante realiza una pregunta.
7. El sistema busca los fragmentos más relacionados con la pregunta.
8. Los fragmentos recuperados se utilizan como contexto para el modelo de lenguaje.
9. El modelo genera una respuesta utilizando únicamente la información recuperada.
10. El sistema muestra la respuesta y las fuentes utilizadas.

## Tecnologías utilizadas

- Python
- Google Colab
- Groq API
- OpenAI Python Client
- Sentence Transformers
- FAISS
- PyPDF
- Scikit-learn

## Modelo utilizado

Para generar las respuestas se utiliza el modelo:

`openai/gpt-oss-120b`

mediante la API de Groq.

Para generar los embeddings se utiliza:

`sentence-transformers/all-MiniLM-L6-v2`

## Búsqueda de información

El sistema utiliza una búsqueda híbrida que combina:

- Similitud semántica entre la pregunta y los fragmentos.
- Coincidencia de palabras relevantes.

El puntaje utilizado corresponde a:

- 70% similitud semántica.
- 30% coincidencia de palabras.

Esto permite recuperar información relacionada con el significado de la pregunta y no solamente buscar palabras exactas.

## Instalación

El proyecto fue desarrollado en Google Colab.

Primero se deben instalar las siguientes librerías:

```python
!pip install openai streamlit faiss-cpu pypdf sentence-transformers
```

## Configuración de Groq

La API Key de Groq debe guardarse en los secretos de Google Colab con el nombre:

`GROQ_API_KEY`

Luego se realiza la conexión con Groq:

```python
from google.colab import userdata
from openai import OpenAI

groq_api_key = userdata.get("GROQ_API_KEY")

client = OpenAI(
    api_key=groq_api_key,
    base_url="https://api.groq.com/openai/v1"
)

print("Cliente Groq inicializado")
```

## Ejecución del proyecto

Para ejecutar EduRAG se deben ejecutar las celdas del notebook en el siguiente orden:

1. Instalar librerías.
2. Importar las librerías y realizar la conexión con Groq.
3. Probar la conexión con el modelo.
4. Subir y leer los documentos PDF.
5. Revisar que el contenido de los PDF se haya extraído correctamente.
6. Dividir los documentos en chunks.
7. Agregar información estructurada de la malla curricular.
8. Agregar la fuente externa.
9. Inicializar el modelo de embeddings.
10. Generar los embeddings.
11. Crear el índice FAISS.
12. Crear la función de búsqueda.
13. Probar la recuperación de información.
14. Crear la función principal de EduRAG.
15. Realizar preguntas de prueba.
16. Evaluar la fidelidad de las respuestas.
17. Evaluar la relevancia de las respuestas.

## Ejemplo de uso

```python
responder_pregunta(
    "¿Qué pasa si repruebo una asignatura?"
)
```

Otro ejemplo:

```python
responder_pregunta(
    "¿En qué nivel está la práctica profesional?"
)
```

El sistema muestra la pregunta realizada, la respuesta generada y las fuentes utilizadas.

## Preguntas utilizadas para las pruebas

Durante las pruebas se utilizaron preguntas como:

- ¿Qué pasa si repruebo una asignatura?
- ¿Qué porcentaje de asistencia necesito para aprobar una asignatura?
- ¿Qué requisitos necesito para realizar la práctica profesional?
- ¿En qué nivel está la práctica profesional?
- ¿Qué necesito para titularme?
- ¿Quién fiscaliza el cumplimiento de esta normativa en educación superior?

También se realizaron preguntas cuya respuesta no estaba disponible en los documentos para comprobar que el sistema no generara información sin respaldo.

## Evaluación de las respuestas

Se implementaron dos funciones simples de evaluación.

### Fidelidad

La fidelidad permite revisar si la respuesta generada se encuentra respaldada por el contexto recuperado desde los documentos.

Se utiliza una escala del 1 al 10.

### Relevancia

La relevancia permite revisar qué tan bien la respuesta generada responde a la pregunta realizada.

También se utiliza una escala del 1 al 10.

## Arquitectura general

El flujo general utilizado por EduRAG es:

Documentos PDF  
↓  
Extracción de texto  
↓  
Chunking  
↓  
Embeddings  
↓  
Búsqueda híbrida  
↓  
Recuperación de chunks relevantes  
↓  
Creación del contexto  
↓  
Modelo de lenguaje mediante Groq  
↓  
Respuesta al estudiante  
↓  
Fuentes utilizadas

## Estructura del repositorio

```text
EduRAG/
├── EduRAG.ipynb
├── README.md
├── documentos/
│   ├── NUEVO-REGLAMENTO-ACADEMICO_20260525.pdf
│   ├── Malla-Curricular-1446116-2024-Ingenieroa-en-Informatica-Especializacion-en-Desarrollo-de-Software-1.pdf
│   └── Reglamento_Titulacion_Duoc_searchable.pdf
└── evidencias/
    ├── prueba_reprobacion.png
    ├── prueba_practica.png
    ├── prueba_malla.png
    └── prueba_fuente_externa.png
```

## Integrantes

- Miguel Delgado

## Asignatura

INGENIERIA DE SOLUCIONES CON INTELIGENCIA ARTIFICIAL_010D_OLS

## Uso de Inteligencia Artificial

Durante el desarrollo del proyecto se utilizaron herramientas de inteligencia artificial como apoyo para resolver dudas de programación, organizar el desarrollo del proyecto y mejorar la redacción de algunos contenidos.
