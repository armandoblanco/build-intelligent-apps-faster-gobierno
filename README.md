# 🤖 Workshop: Apps Inteligentes con GitHub Copilot y Azure OpenAI

## Contoso Gobierno — Asistente Virtual de Trámites Ciudadanos

![GitHub Copilot](https://img.shields.io/badge/GitHub%20Copilot-Enabled-green)
![Python](https://img.shields.io/badge/Python-3.10+-blue)
![Flask](https://img.shields.io/badge/Flask-3.x-lightgrey)
![Azure OpenAI](https://img.shields.io/badge/Azure%20OpenAI-GPT--4o-purple)
![Duración](https://img.shields.io/badge/Duración-2%20horas-red)

---

## 📋 Tabla de Contenidos

- [Introducción](#-introducción)
- [Conceptos Clave](#-conceptos-clave)
- [Pre-requisitos](#️-pre-requisitos)
- [Agenda del Workshop](#-agenda-del-workshop)
- [Ejercicio 1: API REST + Asistente IA](#-ejercicio-1-api-rest--asistente-ia-con-copilot-35-40-min)
- [Ejercicio 2: Frontend con Chat IA](#-ejercicio-2-frontend-con-chat-ia-25-30-min)
- [Ejercicio 3: Tests y Refactoring](#-ejercicio-3-tests-y-refactoring-20-25-min)
- [Referencia Rápida](#-referencia-rápida)
- [Recursos Adicionales](#-recursos-adicionales)

---

## 🎯 Introducción

Este workshop práctico de **2 horas** te guiará en la construcción de un **Asistente Virtual Inteligente para Contoso Gobierno** — una aplicación monolítica en Python que combina una API REST con un agente de IA capaz de responder consultas ciudadanas en lenguaje natural.

Construirás desde cero:

- ✅ Una **API REST monolítica** en Python (Flask) con GitHub Copilot
- ✅ Un **asistente inteligente** usando Azure OpenAI (GPT-4o) que responde preguntas sobre trámites
- ✅ Un **frontend HTML** con interfaz de chat integrada (sin frameworks)
- ✅ **Pruebas unitarias** generadas con asistencia de IA

> 💡 Este workshop es el **siguiente paso natural** después del workshop básico de GitHub Copilot. Aquí asumimos que ya conoces los modos de Copilot (Ask, Agent, Plan) y los comandos `/tests`, `/explain`, `/fix`. Si no, consulta primero el workshop introductorio.

### Estándares del Proyecto

| Aspecto | Estándar |
|---------|----------|
| Tipo | Aplicación monolítica (API + Frontend + IA) |
| Tecnología | Python 3.10+ |
| Framework | Flask + Flask-RESTX |
| IA | Azure OpenAI (GPT-4o) via librería `openai` |
| Documentación API | Swagger UI (integrada) |
| Idioma | Español (código, comentarios, documentación) |
| Base de datos | En memoria (diccionarios Python) |
| Frontend | HTML + JavaScript vanilla + Bootstrap 5 (sin frameworks) |
| Pruebas | pytest |

### Escenario: Contoso Gobierno — Asistente de Trámites 🏛️

**Contoso Gobierno** necesita un **asistente virtual inteligente** que ayude a los ciudadanos con:

- 📋 **Información sobre trámites** (registro civil, licencias, permisos)
- 📝 **Requisitos y documentos** necesarios para cada trámite
- 🕐 **Estado de solicitudes** y tiempos estimados
- 💡 **Orientación general** sobre procesos gubernamentales

El asistente usa **Azure OpenAI** para entender preguntas en lenguaje natural y responder con información contextualizada sobre los trámites disponibles en el sistema.

### Arquitectura de la Solución

```
┌─────────────────────────────────────────────────────────────┐
│                     NAVEGADOR WEB                           │
│                                                             │
│  ┌─────────────────────┐    ┌───────────────────────────┐   │
│  │   Frontend HTML     │    │      Swagger UI            │   │
│  │                     │    │   (auto-generado por       │   │
│  │  Chat IA integrado  │    │    Flask-RESTX)            │   │
│  │  Bootstrap 5 (CDN)  │    │                           │   │
│  │  JavaScript vanilla │    │   /api/doc                │   │
│  │  fetch() → API      │    │                           │   │
│  │  /                  │    │                           │   │
│  └────────┬────────────┘    └─────────┬─────────────────┘   │
│           │                           │                     │
└───────────┼───────────────────────────┼─────────────────────┘
            │  HTTP (mismo origen)       │
            ▼                           ▼
┌─────────────────────────────────────────────────────────────┐
│              SERVIDOR FLASK MONOLÍTICO                       │
│              (app.py — todo en uno)                          │
│                                                             │
│  ┌──────────────┐   ┌──────────────────────────────────┐    │
│  │ render_       │   │        Flask-RESTX API           │    │
│  │ template()   │   │                                  │    │
│  │              │   │  /api/tramites     → CRUD        │    │
│  │ index.html   │   │  /api/ciudadanos   → CRUD        │    │
│  │              │   │  /api/asistente    → Azure OpenAI │    │
│  └──────────────┘   └──────────┬───────────────────────┘    │
│                                │                            │
│                    ┌───────────┴───────────┐                │
│                    ▼                       ▼                │
│     ┌──────────────────────┐  ┌────────────────────────┐    │
│     │   Modelos (Python)   │  │  Servicio IA           │    │
│     │                      │  │                        │    │
│     │  modelos/tramite.py  │  │  servicios/            │    │
│     │  modelos/ciudadano.py│  │   asistente_ia.py      │    │
│     │                      │  │                        │    │
│     │  Datos en memoria    │  │  Azure OpenAI (GPT-4o) │    │
│     └──────────────────────┘  └────────────────────────┘    │
│                                                             │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  pytest  →  tests/test_api.py                        │   │
│  │             tests/test_asistente.py                   │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
                                │
                                ▼
                  ┌──────────────────────────┐
                  │     Azure OpenAI         │
                  │     (GPT-4o)             │
                  │                          │
                  │  Endpoint + API Key      │
                  │  (proveído por el        │
                  │   instructor)            │
                  └──────────────────────────┘
```

**Flujo del asistente:**
1. El ciudadano escribe una pregunta en el chat del frontend
2. JavaScript envía la pregunta via `fetch()` a `POST /api/asistente`
3. Flask recibe la pregunta y la envía a Azure OpenAI junto con el contexto de los trámites disponibles
4. Azure OpenAI genera una respuesta en lenguaje natural
5. La respuesta se muestra en el chat del frontend

**¿Qué lo hace "inteligente"?** El endpoint `/api/asistente` no solo pasa la pregunta a Azure OpenAI — también inyecta como contexto los trámites reales del sistema (los mismos que están en la API), para que las respuestas sean relevantes y actualizadas.

---

## 🧠 Conceptos Clave

### Azure OpenAI en 30 segundos

Azure OpenAI te permite usar modelos como GPT-4o desde tu código Python con unas pocas líneas:

```python
from openai import AzureOpenAI

cliente = AzureOpenAI(
    azure_endpoint="https://tu-recurso.openai.azure.com/",
    api_key="tu-api-key",
    api_version="2024-12-01-preview"
)

respuesta = cliente.chat.completions.create(
    model="gpt-4o",
    messages=[
        {"role": "system", "content": "Eres un asistente de trámites gubernamentales."},
        {"role": "user", "content": "¿Qué necesito para sacar mi licencia de conducir?"}
    ]
)

print(respuesta.choices[0].message.content)
```

> 💡 El **system prompt** es donde defines la personalidad y el contexto del asistente. Aquí es donde inyectaremos la información de los trámites del sistema.

### ¿Qué es un System Prompt?

El system prompt es una instrucción que le das al modelo antes de que el usuario haga su pregunta. Define el comportamiento, tono y conocimiento del asistente.

**❌ System prompt débil:**
```
Eres un asistente.
```

**✅ System prompt efectivo:**
```
Eres el asistente virtual oficial de Contoso Gobierno. Tu rol es ayudar a ciudadanos
con información sobre trámites gubernamentales. Responde siempre en español, sé amable
y conciso. Si no tienes información sobre un trámite específico, sugiere al ciudadano
acudir a la ventanilla de atención. Estos son los trámites disponibles en el sistema:
{contexto_dinamico}
```

### Recordatorio: Modos de Copilot

| Modo | Cuándo usarlo |
|------|---------------|
| Ask 💬 | Explorar, entender, planificar (no modifica archivos) |
| Agent 🤖 | Implementar, crear archivos, hacer cambios |
| Plan 📋 | Tareas complejas multi-archivo (con aprobación) |

> ⚠️ La interfaz de los modos puede variar según tu versión de VS Code. Consulta con el instructor si ves algo diferente.

---

## 🛠️ Pre-requisitos

### Software Necesario

```bash
python3 --version   # Debe ser 3.10 o superior
code --version      # Visual Studio Code
pip --version       # Gestor de paquetes de Python
```

### Extensiones de VS Code

1. **GitHub Copilot** — Extensión principal
2. **GitHub Copilot Chat** — Chat integrado
3. **Python** (Microsoft) — Soporte para Python

### Cuenta de GitHub

- Necesitas una cuenta con acceso a **GitHub Copilot**

### Azure OpenAI

> 🔑 **IMPORTANTE — Lee esto antes de empezar:**
>
> Para el ejercicio del asistente inteligente necesitas acceso a Azure OpenAI (un **Endpoint** y un **API Key**).
>
> **Opción A — El instructor te provee las credenciales:**
> Si no tienes cuenta de Azure o no tienes un recurso de Azure OpenAI desplegado, **no te preocupes**. El instructor compartirá las credenciales necesarias al inicio del Ejercicio 1. Solo necesitas copiar el endpoint y la API key que se te indiquen.
>
> **Opción B — Usas tus propias credenciales:**
> Si ya tienes acceso a Azure OpenAI con un modelo desplegado (por ejemplo, `gpt-4o` o `gpt-4o-mini`), puedes usar tus propias credenciales. Necesitarás: el endpoint de tu recurso, tu API key y el nombre del deployment del modelo.

---

## 📅 Agenda del Workshop

| Hora | Bloque | Actividad | Modo Copilot |
|------|--------|-----------|--------------|
| 0:00 - 0:15 | Bienvenida | Setup, configuración y repaso de conceptos | - |
| 0:15 - 0:55 | Ejercicio 1 | API REST + Asistente IA con Azure OpenAI | Ask → Agent |
| 0:55 - 1:00 | ☕ Break | Descanso y Q&A rápido | - |
| 1:00 - 1:30 | Ejercicio 2 | Frontend HTML con chat IA integrado | Agent |
| 1:30 - 1:50 | Ejercicio 3 | Pruebas unitarias y refactoring | Agent + /tests |
| 1:50 - 2:00 | Cierre | Recap, tips avanzados y recursos | - |

> ⏱️ Los tiempos son aproximados. **Nunca excedas 2 horas.**

> 🎓 **Nota para el instructor:** Si el setup toma más de 15 minutos, compensa reduciendo el Ejercicio 3 a solo generar tests (Pasos 3.1–3.2). Lo más importante es que los participantes completen los Ejercicios 1 y 2 con el asistente IA funcionando.

---

## 🔬 Ejercicio 1: API REST + Asistente IA con Copilot (35-40 min)

### Objetivos

- ✅ Crear una API REST con Flask-RESTX y Swagger
- ✅ Integrar Azure OpenAI como asistente inteligente
- ✅ Crear un endpoint que combine datos del sistema con IA generativa
- ✅ Experimentar con system prompts y contexto dinámico

### Paso 1.1: Explorar la arquitectura con Modo Ask 🔍

🤖 **PROMPT en Copilot Chat (Modo Ask 💬):**

```
Soy desarrollador en Contoso Gobierno. Necesito construir una aplicación monolítica en Python con Flask que combine:

1. Una API REST para gestionar trámites ciudadanos (CRUD básico)
2. Un endpoint de asistente inteligente que use Azure OpenAI para responder preguntas de ciudadanos sobre trámites

El asistente debe recibir una pregunta en lenguaje natural y responder usando como contexto los trámites reales que existen en el sistema.

¿Cómo organizarías esta aplicación en un solo proyecto Flask? ¿Qué patrón usarías para inyectar el contexto de los trámites en el system prompt de Azure OpenAI?
```

> 🌟 **Momento wow:** Copilot entiende el patrón RAG simplificado (Retrieval Augmented Generation) y sugiere cómo inyectar datos reales como contexto del modelo.

---

### Paso 1.2: Crear instrucciones de Copilot y estructura del proyecto

🤖 **PROMPT en Modo Agent 🤖:**

```
Crea la estructura inicial del proyecto para el asistente inteligente de Contoso Gobierno.

Necesito:
1. Un archivo .github/copilot-instructions.md con las instrucciones del proyecto:
   - Todo en español (código, comentarios, documentación)
   - Python 3.10+, Flask, Flask-RESTX, PEP 8
   - Contexto: sistema de trámites ciudadanos con asistente IA usando Azure OpenAI

2. La estructura del proyecto:
   - Un archivo principal para Flask (app.py)
   - Una carpeta modelos/ para los modelos de datos (con __init__.py)
   - Una carpeta servicios/ para la lógica de negocio y la integración con Azure OpenAI
   - Carpetas templates/, static/ y tests/ (con __init__.py)
   - Un archivo de dependencias con flask, flask-restx, openai, python-dotenv y pytest
   - Un archivo .env.example con las variables AZURE_OPENAI_ENDPOINT, AZURE_OPENAI_API_KEY, AZURE_OPENAI_MODEL

3. Instala las dependencias después de crear la estructura
```

📝 **Alternativa manual** (si el agente no ejecuta):
```bash
mkdir -p contoso-gobierno-ia/modelos contoso-gobierno-ia/servicios
mkdir -p contoso-gobierno-ia/templates contoso-gobierno-ia/static contoso-gobierno-ia/tests
touch contoso-gobierno-ia/app.py contoso-gobierno-ia/requirements.txt
touch contoso-gobierno-ia/modelos/__init__.py contoso-gobierno-ia/tests/__init__.py
touch contoso-gobierno-ia/servicios/__init__.py
touch contoso-gobierno-ia/.env.example

echo -e "flask\nflask-restx\nopenai\npython-dotenv\npytest" > contoso-gobierno-ia/requirements.txt
echo -e "AZURE_OPENAI_ENDPOINT=https://tu-recurso.openai.azure.com/\nAZURE_OPENAI_API_KEY=tu-api-key\nAZURE_OPENAI_MODEL=gpt-4o" > contoso-gobierno-ia/.env.example

cd contoso-gobierno-ia
pip install -r requirements.txt
```

---

### Paso 1.3: Configurar las credenciales de Azure OpenAI

> 🔑 **Momento de las credenciales:** El instructor compartirá el **Endpoint** y **API Key** de Azure OpenAI. Si tienes tus propias credenciales, úsalas.

📍 **Instrucciones:**
1. Copia el archivo `.env.example` a `.env`
2. Reemplaza los valores con las credenciales proporcionadas:

```bash
cp .env.example .env
```

Edita `.env` con los valores reales:
```
AZURE_OPENAI_ENDPOINT=https://recurso-compartido.openai.azure.com/
AZURE_OPENAI_API_KEY=abc123...
AZURE_OPENAI_MODEL=gpt-4o
```

> ⚠️ **NUNCA subas el archivo `.env` a Git.** Verifica que `.gitignore` incluya `.env`. Si no lo tiene, agrégalo.

---

### Paso 1.4: Crear el modelo de trámites

🤖 **PROMPT en Modo Agent 🤖:**

```
Crea el archivo contoso-gobierno-ia/modelos/tramite.py con el modelo de datos para los trámites de Contoso Gobierno.

El modelo debe incluir:
- Un diccionario en memoria como base de datos simulada
- Datos de ejemplo precargados con 5-6 trámites reales de gobierno:
  * Acta de nacimiento, Licencia de conducir, Permiso de construcción,
    CURP, Pasaporte, Constancia de residencia
- Campos: id, nombre, categoria, descripcion, requisitos (lista), costo, tiempo_estimado
- Funciones para: obtener todos, obtener por id, buscar por nombre/categoría
- Cada función con comentarios en español
```

📝 **Observa:** Los datos precargados serán el **conocimiento base** que el asistente IA usará para responder preguntas. Entre más detallados sean los requisitos y descripciones, mejores serán las respuestas del asistente.

---

### Paso 1.5: Crear el servicio del asistente IA

> 💡 **Este es el paso clave del workshop.** Aquí conectamos Azure OpenAI con los datos del sistema.

🤖 **PROMPT en Modo Agent 🤖:**

```
Crea el archivo contoso-gobierno-ia/servicios/asistente_ia.py con el servicio de asistente inteligente.

Este servicio debe:
1. Cargar las credenciales de Azure OpenAI desde variables de entorno (usando python-dotenv)
2. Crear un cliente de AzureOpenAI con el endpoint, api_key y api_version
3. Tener una función "generar_respuesta(pregunta)" que:
   - Obtenga todos los trámites disponibles del modelo de datos
   - Construya un system prompt que incluya:
     * El rol del asistente (asistente virtual de Contoso Gobierno)
     * Instrucciones de comportamiento (responder en español, ser amable, ser conciso)
     * Los trámites disponibles con sus requisitos y costos como contexto
   - Envíe la pregunta del ciudadano junto con el system prompt a Azure OpenAI
   - Retorne la respuesta del modelo
4. Manejar errores si Azure OpenAI no está disponible o las credenciales son inválidas
```

> 🌟 **Momento wow:** Observa cómo Copilot implementa un patrón RAG simplificado: recupera datos reales del sistema (los trámites) y los inyecta como contexto en el prompt, haciendo que las respuestas de la IA sean específicas y actualizadas.

> 💡 **Tip:** Si el servicio generado usa credenciales hardcodeadas en lugar de variables de entorno, recuerda al agente: *"Usa python-dotenv para cargar las credenciales desde el archivo .env"*

---

### Paso 1.6: Crear la aplicación Flask monolítica

🤖 **PROMPT en Modo Agent 🤖:**

```
Crea contoso-gobierno-ia/app.py como la aplicación principal monolítica de Contoso Gobierno.

Esta aplicación debe integrar todo en un solo servidor Flask:

1. Flask-RESTX con Swagger UI. Título: "API de Contoso Gobierno — Asistente Inteligente", versión 1.0.
2. Namespace /api/tramites con endpoints CRUD para trámites (usando modelos/tramite.py)
3. Namespace /api/asistente con un endpoint POST que:
   - Recibe un JSON con el campo "pregunta"
   - Usa servicios/asistente_ia.py para generar la respuesta con Azure OpenAI
   - Retorna la respuesta del asistente en JSON
   - Incluye documentación Swagger
4. Una ruta "/" que sirva templates/index.html con render_template
5. Carga las variables de entorno con python-dotenv al inicio
```

---

### Paso 1.7: Ejecutar y probar el asistente

```bash
cd contoso-gobierno-ia
python app.py
```

**Abre en el navegador:** `http://127.0.0.1:5000/api/doc`

✅ **Verificar en Swagger:**
1. GET `/api/tramites` retorna los trámites de ejemplo
2. POST `/api/asistente` con el body:
```json
{
  "pregunta": "¿Qué necesito para tramitar mi licencia de conducir?"
}
```
3. La respuesta debe incluir los requisitos reales del trámite de licencia que están en tu modelo de datos

> 🌟 **Momento wow:** El asistente responde con información **específica** de los trámites de Contoso Gobierno, no con información genérica. Esto es porque inyectaste los datos reales en el system prompt. Prueba con otras preguntas:
> - *"¿Cuánto cuesta sacar un acta de nacimiento?"*
> - *"¿Cuáles son los trámites más rápidos?"*
> - *"Necesito renovar mi pasaporte, ¿qué documentos llevo?"*

---

### Paso 1.8: Mejorar el system prompt (⭐ desafío bonus)

> 📝 **Este paso es OPCIONAL** para quienes terminaron antes.

Experimenta mejorando el system prompt del asistente:

🤖 **PROMPT en Modo Agent 🤖:**

```
@workspace Mejora el system prompt en servicios/asistente_ia.py para que el asistente:
- Responda de forma más amigable y empática con los ciudadanos
- Cuando no conozca un trámite, sugiera acudir a la ventanilla de atención presencial
- Incluya el horario de atención: lunes a viernes de 8:00 a 15:00
- Formatee los requisitos como lista cuando sea necesario
```

---

### 🛠️ Troubleshooting Ejercicio 1

| Problema | Solución |
|----------|----------|
| `openai.AuthenticationError` | Verifica API Key en `.env` — pide las credenciales al instructor |
| `openai.NotFoundError` | Verifica el nombre del modelo/deployment en `.env` |
| `openai.APIConnectionError` | Verifica el endpoint en `.env` y tu conexión a internet |
| El asistente da respuestas genéricas | Revisa que el system prompt incluya los datos de los trámites |
| `ModuleNotFoundError: dotenv` | Ejecuta `pip install python-dotenv` |
| `.env` no se carga | Verifica que `load_dotenv()` se ejecute al inicio de `app.py` |

---

## 🔬 Ejercicio 2: Frontend con Chat IA (25-30 min)

> ⚠️ **PRERREQUISITO:** La API del Ejercicio 1 debe estar funcionando, incluyendo el endpoint `/api/asistente`.

### Objetivos

- ✅ Crear una interfaz de chat que consuma el asistente IA
- ✅ Mostrar un dashboard con los trámites disponibles
- ✅ Usar Copilot para generar JavaScript vanilla con fetch()
- ✅ Practicar el uso de `/explain` para entender código generado

### Paso 2.1: Crear la página HTML con chat integrado

🤖 **PROMPT en Modo Agent 🤖:**

```
Crea el archivo contoso-gobierno-ia/templates/index.html con la página principal de Contoso Gobierno.

Es una sola página HTML con JavaScript vanilla embebido (sin frameworks como React o Vue).

Requisitos:
1. Bootstrap 5 via CDN
2. Header con "Contoso Gobierno — Portal de Trámites Ciudadanos"
3. Dos secciones principales con tabs: "Trámites Disponibles" y "Asistente Virtual"
4. Sección Trámites:
   - Cards con los trámites disponibles (cargados desde GET /api/tramites)
   - Cada card muestra: nombre, categoría, costo y tiempo estimado
   - Al hacer clic en un trámite, muestra sus requisitos en un modal
5. Sección Asistente Virtual (interfaz de chat):
   - Un área de mensajes estilo chat (burbujas de conversación)
   - Mensajes del ciudadano alineados a la derecha (color azul)
   - Respuestas del asistente alineadas a la izquierda (color gris)
   - Un input de texto con botón "Enviar" que haga POST a /api/asistente
   - Un indicador de "escribiendo..." mientras espera la respuesta
   - Un mensaje de bienvenida del asistente al cargar la página
6. Footer con "© Contoso Gobierno"

Colores institucionales: azul oscuro #0d47a1, blanco, gris claro.
El JavaScript usa fetch() para consumir la API.
Incluye manejo de errores (si la API no responde, mostrar mensaje amigable).
```

> 📝 **Nota:** La interfaz de chat es el elemento más impactante visualmente. Copilot debería generar algo que se vea profesional con Bootstrap. Si las burbujas no se ven bien, itera con: *"Mejora el estilo de las burbujas del chat para que se vean como WhatsApp o Teams"*.

---

### Paso 2.2: Verificar la ruta del frontend

🤖 **PROMPT en Modo Ask 💬:**

```
@workspace ¿El archivo app.py ya tiene configurada una ruta "/" que sirva templates/index.html?
```

Si la respuesta es **no**:

🤖 **PROMPT en Modo Agent 🤖:**

```
@workspace Agrega la ruta "/" en app.py para servir templates/index.html, sin modificar la API existente.
```

---

### Paso 2.3: Ejecutar y probar el chat

```bash
cd contoso-gobierno-ia
python app.py
```

**Abre:** `http://127.0.0.1:5000/`

✅ **Verificar:**
- Los trámites se cargan y muestran como cards
- Al hacer clic en un trámite, se muestran sus requisitos
- El chat muestra el mensaje de bienvenida del asistente
- Puedes escribir una pregunta y recibir una respuesta del asistente IA
- Las burbujas del chat se ven correctamente (ciudadano a la derecha, asistente a la izquierda)
- El indicador de "escribiendo..." aparece mientras se espera la respuesta

> 🌟 **Momento wow:** Tienes un asistente virtual funcional que responde preguntas sobre trámites reales del sistema, con una interfaz de chat profesional, ¡todo construido con GitHub Copilot en menos de una hora!

---

### Paso 2.4: Usar /explain para entender la integración

📍 **Instrucciones:**
1. Selecciona la función del JavaScript que envía mensajes al asistente (la que hace `fetch('/api/asistente', ...)`)
2. En Copilot Chat:

```
/explain ¿Cómo funciona esta integración entre el frontend y Azure OpenAI? 
¿Qué pasa si la API tarda mucho en responder?
```

---

### 🛠️ Troubleshooting Ejercicio 2

| Problema | Solución |
|----------|----------|
| Chat no responde | Verifica que la API esté corriendo y el endpoint `/api/asistente` funcione en Swagger |
| "Error al contactar al asistente" | Revisa la consola del navegador (F12) y verifica las credenciales de Azure OpenAI |
| Respuesta tarda mucho | Normal — Azure OpenAI puede tardar 3-10 segundos. El indicador de "escribiendo..." debería ser visible |
| Bootstrap no carga | Verifica tu conexión a internet (se usa CDN) |
| Trámites no se muestran | Verifica que GET `/api/tramites` funcione en Swagger |

---

## 🔬 Ejercicio 3: Tests y Refactoring (20-25 min)

> ⚠️ **PRERREQUISITO:** Ejercicio 1 completado con la API funcionando.

> 🎓 **Nota para el instructor:** Si el tiempo es limitado, prioriza solo los Pasos 3.1–3.2. El refactoring (3.3) es bonus.

### Objetivos

- ✅ Generar pruebas para la API y el servicio de IA
- ✅ Aprender a mockear servicios externos (Azure OpenAI) en tests
- ✅ Practicar refactoring asistido

### Paso 3.1: Generar pruebas de la API

🤖 **PROMPT en Modo Agent 🤖:**

```
@workspace Crea pruebas de integración para la API de Contoso Gobierno en contoso-gobierno-ia/tests/test_api.py

Usando el test client de Flask, verifica que los endpoints principales funcionan:
- Listar, consultar y buscar trámites
- Que los endpoints respondan con los códigos HTTP correctos
- Que al consultar un trámite que no existe se obtenga un error apropiado

Usa pytest con nombres descriptivos en español.
Cada test debe ser independiente.
```

---

### Paso 3.2: Generar pruebas del asistente IA con mock

> 💡 **CONCEPTO IMPORTANTE:** No queremos que los tests llamen a Azure OpenAI real (costaría dinero y dependería de internet). Usamos **mocks** para simular las respuestas.

🤖 **PROMPT en Modo Agent 🤖:**

```
@workspace Crea pruebas para el servicio de asistente IA en contoso-gobierno-ia/tests/test_asistente.py

Las pruebas deben:
- Usar unittest.mock para simular las respuestas de Azure OpenAI (no llamar a la API real)
- Verificar que el system prompt incluye los trámites del sistema como contexto
- Verificar que la función generar_respuesta retorna una respuesta coherente
- Verificar que los errores de conexión se manejan correctamente
- Usar pytest con nombres descriptivos en español
```

> 🌟 **Momento wow:** Copilot entiende que necesita mockear el cliente de Azure OpenAI y genera tests que verifican tanto la lógica de negocio como el manejo de errores, sin gastar tokens reales de la API.

**Ejecutar las pruebas:**
```bash
cd contoso-gobierno-ia
python -m pytest tests/ -v
```

---

### Paso 3.3: Refactoring del system prompt *(si hay tiempo)*

🤖 **PROMPT en Copilot Chat:**

```
/explain Analiza el system prompt en servicios/asistente_ia.py:
1. ¿Es demasiado largo o demasiado corto?
2. ¿Podría causar alucinaciones del modelo?
3. ¿Hay instrucciones que limiten respuestas fuera de tema?
4. ¿Cómo mejorarías la inyección de contexto de los trámites?
```

Después:

```
/fix Aplica las mejoras sugeridas al system prompt
```

---

### 🛠️ Troubleshooting Ejercicio 3

| Problema | Solución |
|----------|----------|
| Tests fallan con errores de importación | Verifica que `__init__.py` exista en `modelos/`, `servicios/` y `tests/` |
| Mock no funciona | Verifica que el path del mock coincida con cómo se importa el cliente |
| Tests del asistente llaman a Azure OpenAI real | Asegúrate de que el mock esté correctamente aplicado con `@patch` |

---

## 📖 Referencia Rápida

### Comandos de Copilot Chat

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| `@workspace` | Contexto del proyecto entero | `@workspace ¿cómo se conecta la API con Azure OpenAI?` |
| `/tests` | Generar pruebas unitarias | Selecciona código → `/tests` |
| `/doc` | Generar documentación | Selecciona función → `/doc` |
| `/fix` | Proponer corrección | Selecciona código con error → `/fix` |
| `/explain` | Explicar código | Selecciona código → `/explain` |

### Atajos de Teclado

| Atajo | Acción |
|-------|--------|
| `Ctrl+I` / `Cmd+I` | Abrir Copilot inline en el editor |
| `Ctrl+Shift+I` / `Cmd+Shift+I` | Abrir panel de Copilot Chat |
| `Tab` | Aceptar sugerencia de Copilot |
| `Esc` | Rechazar sugerencia |

### Variables de Azure OpenAI

| Variable | Descripción | Ejemplo |
|----------|-------------|---------|
| `AZURE_OPENAI_ENDPOINT` | URL del recurso de Azure OpenAI | `https://mi-recurso.openai.azure.com/` |
| `AZURE_OPENAI_API_KEY` | Clave de acceso a la API | `abc123...` |
| `AZURE_OPENAI_MODEL` | Nombre del deployment del modelo | `gpt-4o` |

---

## 🆘 ¿Te quedaste atrás?

| Situación | Qué hacer |
|-----------|-----------|
| No tengo credenciales de Azure OpenAI | Pídelas al instructor — las compartirá durante el workshop |
| La API funciona pero el asistente no | Verifica las credenciales en `.env` y que el modelo esté desplegado |
| No terminé el Ejercicio 1 | Pide al instructor la solución de referencia |
| No terminé el Ejercicio 2 | El Ejercicio 3 solo necesita la API. Puedes hacer tests sin frontend |
| El asistente responde cosas raras | Revisa y mejora el system prompt — es la parte más importante |

> 🎓 **Para el instructor:** Se recomienda tener una rama `solucion` con el código completo y credenciales de ejemplo preconfiguradas para participantes que se queden atrás.

---

## ✅ Checklist Final del Workshop

### Configuración
- [ ] `.github/copilot-instructions.md` — Instrucciones de Copilot
- [ ] `.env` — Credenciales de Azure OpenAI configuradas
- [ ] `.gitignore` — Incluye `.env`

### Backend + IA
- [ ] `contoso-gobierno-ia/app.py` — Aplicación Flask monolítica
- [ ] `contoso-gobierno-ia/requirements.txt` — Dependencias
- [ ] `contoso-gobierno-ia/modelos/tramite.py` — Modelo de trámites con datos de ejemplo
- [ ] `contoso-gobierno-ia/servicios/asistente_ia.py` — Servicio de asistente con Azure OpenAI
- [ ] Endpoint `/api/tramites` funcional en Swagger
- [ ] Endpoint `/api/asistente` funcional y respondiendo con contexto real

### Frontend
- [ ] `contoso-gobierno-ia/templates/index.html` — Página con chat IA y catálogo de trámites
- [ ] Interfaz de chat funcional (enviar pregunta, recibir respuesta)
- [ ] Cards de trámites cargadas desde la API

### Pruebas
- [ ] `contoso-gobierno-ia/tests/test_api.py` — Pruebas de integración
- [ ] `contoso-gobierno-ia/tests/test_asistente.py` — Pruebas del asistente con mock
- [ ] Todas las pruebas pasan con `pytest`

---

## 📚 Recursos Adicionales

### Documentación Oficial

- [GitHub Copilot Docs](https://docs.github.com/en/copilot)
- [Azure OpenAI Documentation](https://learn.microsoft.com/en-us/azure/ai-services/openai/)
- [Azure OpenAI Python SDK](https://learn.microsoft.com/en-us/azure/ai-services/openai/quickstart?tabs=python)
- [Flask Documentation](https://flask.palletsprojects.com/)
- [Flask-RESTX Documentation](https://flask-restx.readthedocs.io/)
- [pytest Documentation](https://docs.pytest.org/)
- [python-dotenv](https://pypi.org/project/python-dotenv/)

### Siguiente Nivel

- **RAG completo:** Agregar Azure AI Search para buscar en documentos PDF de normativas gubernamentales
- **Historial de conversación:** Implementar multi-turn chat guardando el contexto de la conversación
- **Containerización:** Empaquetar la aplicación en Docker y desplegar en Azure Container Apps
- **CI/CD:** Automatizar el despliegue con GitHub Actions
- **Agentes Copilot:** Crear un agente en `.github/agents/` para especializar a Copilot en el dominio gubernamental

---

## 🙋 Preguntas Frecuentes

### ¿Necesito mi propia cuenta de Azure?

No. El instructor proporcionará las credenciales de Azure OpenAI. Si ya tienes acceso propio, puedes usarlo, pero no es requisito.

### ¿Qué modelo de Azure OpenAI se usa?

El workshop está diseñado para **GPT-4o** o **GPT-4o-mini**. Ambos funcionan bien para este escenario. GPT-4o-mini es más rápido y económico.

### ¿Por qué usamos datos en memoria en lugar de una base de datos?

Simplicidad. El objetivo es aprender Copilot + Azure OpenAI, no configurar bases de datos. Migrar a SQLAlchemy sería un siguiente paso natural.

### ¿Por qué el asistente a veces inventa información?

Esto se llama "alucinación". Para mitigarlo: incluye en el system prompt *"Si no tienes información sobre un trámite, indica claramente que no dispones de esa información y sugiere acudir a ventanilla."*

### ¿Cuánto cuesta usar Azure OpenAI?

Para este workshop, el costo es mínimo (centavos). Las credenciales compartidas por el instructor cubren el uso del taller. En producción, Azure OpenAI cobra por tokens consumidos.

### ¿Qué diferencia hay entre este workshop y el básico de Copilot?

El workshop básico se enfoca en las herramientas de Copilot (modos, comandos, prompts). Este workshop agrega una capa de **inteligencia artificial generativa** con Azure OpenAI, mostrando cómo construir aplicaciones que combinan APIs tradicionales con IA.

---

## 👥 Créditos

**Workshop desarrollado para:** Demostración de GitHub Copilot + Azure OpenAI

**Tecnologías:** GitHub Copilot, Python 3, Flask, Flask-RESTX, Azure OpenAI (GPT-4o), Swagger UI, HTML + JavaScript vanilla, Bootstrap 5 (CDN), pytest

**Duración:** 2 horas

**Escenario ficticio:** Contoso Gobierno (entidad gubernamental ficticia para fines educativos)

**Repositorio de referencia:** [armandoblanco/build-intelligent-apps-faster](https://github.com/armandoblanco/build-intelligent-apps-faster)

---

> 🎉 **¡Gracias por participar!** Ahora sabes cómo construir aplicaciones inteligentes que combinan APIs REST con IA generativa, todo acelerado por GitHub Copilot.
