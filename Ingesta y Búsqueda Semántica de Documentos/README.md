# 🧠 Ingesta y Búsqueda Semántica de Documentos (DEV)

## 📘 Descripción general

Workflow de **n8n** para extraer texto de archivos en **Google Drive**, normalizar campos y persistir contenido y metadatos en **PostgreSQL** (tabla `documents`) y **embeddings** en **PGVector** para consultas semánticas.  
Incluye un subflujo de **evaluación** con **Google Sheets** y un **agente** que responde exclusivamente basado en la base vectorial.

---

## ⚙️ Arquitectura y funcionamiento

### 🧩 Ingesta  
- Búsqueda y descarga de nuevos ficheros desde **Google Drive**  
- Envío a un servicio de parsing (`unstructured`) para obtener texto y metadatos (`tipo`, `element_id`, `página`, etc.)

### 🧮 Transformación  
- Nodos **Set** y **Merge** reestructuran y renombran campos (`Document`, `Type`, `element_id`, `metadata.*`)  
- Filtro de títulos irrelevantes

### 🗄️ Persistencia  
- **PostgreSQL (`documents`)** → *Upsert* de contenido y metadatos  
- **PGVector** → Generación de embeddings con **OpenAI** y carga vía *Default Data Loader → Vector Store PGVector*

### 🤖 Consulta  
- Un **agente (LLM)** usa la vector store en PostgreSQL como herramienta de recuperación  
- Responde *sí/no* con cita, basado exclusivamente en la información vectorizada

### 🧾 Evaluación  
- Disparador y hojas de **Google Sheets** alimentan preguntas y registran outputs y métricas de *correctness*  
- Un **LLM as a Judge** puntúa las respuestas generadas

---

## 🔁 Flujo general (alto nivel)

Entrada (Drive)
↓
Procesamiento (unstructured + normalización)
↓
Integración (Postgres / PGVector)
↓
Respuesta (Agente con RAG)
↓
Registro (Sheets + Evaluación)


---

## 🧰 Tecnologías y principios

- **n8n**, **APIs REST**, **HTTP+JSON**  
- **Google Drive** y **Google Sheets** (OAuth)  
- **PostgreSQL + PGVector** (búsqueda semántica)  
- **OpenAI Embeddings** (`dim=1536`) y *LLM as a judge*  
- Control de estado por IDs, *upsert* idempotente, logs estructurados y modularidad  
- Autenticación segura: credenciales fuera del repo mediante **Credentials de n8n** / **variables de entorno**

---

## 📂 Contenido del repo

| Archivo | Descripción |
|----------|--------------|
| `workflow.json` | Workflow n8n de ingesta, vectorización, consulta y evaluación |

---

## 🔐 Seguridad y manejo de credenciales

Nunca versionar **tokens/secretos**.  
Usar **Credentials de n8n** y **variables de entorno**.

Este repo mantiene estructura y metadatos, pero redacta cualquier campo sensible en los JSON con:


Campos sensibles: `token`, `api_key`, `secret`, `authorization`, `cookie`, `client_secret`, `connection_string`, etc.

---

## 📈 Estado de desarrollo

| Componente | Estado |
|-------------|--------|
| Ingesta (Drive → Unstructured) | ✅ |
| Normalización y filtro | ✅ |
| PostgreSQL (tabla documents) | ✅ |
| PGVector + Embeddings | ✅ |
| Agente RAG (sí/no + cita) | ✅ |
| Evaluación (Sheets + Judge) | ✅ |

---

## 🧭 Roadmap

- Control de versiones de documentos y *re-embedding* selectivo  
- Monitorización de *drift* de embeddings y *batch reindexing*  
- Alertas de fallos (Drive/HTTP/DB) con reintentos exponenciales  

---

## 👥 Autores y mantenimiento

Equipo de **Data/Platform**.  
Mantenimiento compartido entre:  
- **Integraciones (n8n)**  
- **Data Engineering (PostgreSQL / PGVector)**