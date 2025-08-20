# 🤖 Asistente de Calendario con n8n y Evolution API

Este proyecto despliega un entorno automatizado que conecta **Evolution API** (una API no oficial para WhatsApp) con **n8n** para crear eventos en **Google Calendar**.

Es una solución poderosa y auto-hospedada para convertir mensajes de WhatsApp (texto y audio) en eventos de calendario de forma automática, ideal para agendar citas, recordatorios o tareas directamente desde tus conversaciones.

![Diagrama del flujo de automatización](https://i.imgur.com/example.png)  <!-- Reemplaza esto con una URL a tu propio diagrama o imagen -->

---

## ✨ Características Principales

-   **Entorno Contenerizado:** Todo listo para desplegar con un solo comando gracias a Docker Compose.
-   **Componentes Incluidos:**
    -   🟢 **Evolution API:** Para recibir y enviar mensajes de WhatsApp.
    -   ⚙️ **n8n:** El motor de automatización que conecta los servicios.
    -   🐘 **PostgreSQL:** Base de datos persistente para n8n y la memoria del chat.
    -   ⚡ **Redis:** Para optimizar el rendimiento de n8n.
-   **Automatización Inteligente:** Usa un Agente de IA para entender peticiones y gestionar el calendario.
-   **Soporte Multimodal:** Capaz de procesar mensajes de texto y notas de voz.
-   **Escalable y Personalizable:** Puedes modificar fácilmente los flujos de n8n para añadir más lógica o integraciones.

---

## 📋 Prerrequisitos

-   [**Docker**](https://docs.docker.com/get-docker/)
-   [**Docker Compose**](https://docs.docker.com/compose/install/)

---

## 🚀 Guía de Inicio Rápido

1.  **Clona el repositorio:**
    ```bash
    git clone <URL_DEL_REPOSITORIO>
    cd nombre-del-directorio
    ```

2.  **Configura las variables de entorno:**
    Crea un archivo `.env` a partir del ejemplo proporcionado (`cp .env.example .env`) y ajusta las variables según tus necesidades.

3.  **Levanta los servicios:**
    ```bash
    docker compose up -d
    ```

4.  **Accede a las aplicaciones:**
    -   **n8n:** [http://localhost:5678](http://localhost:5678)
    -   **Evolution API:** [http://localhost:8080](http://localhost:8080)

---

## 🛠️ Configuración Post-Instalación

1.  **En n8n:**
    -   Importa el flujo de trabajo (`workflow.json`) proporcionado en este repositorio.
    -   Configura las credenciales necesarias (ver detalles en la sección del flujo de trabajo).

2.  **En Evolution API:**
    -   Crea tu instancia de WhatsApp.
    -   Copia la URL del **Webhook de Producción** del nodo `Webhook` en n8n y configúrala en tu instancia de Evolution API para que notifique a n8n cada vez que llegue un mensaje.

---

## 🔎 Análisis del Flujo de Trabajo (Workflow)

El corazón de este proyecto es el flujo de n8n. A continuación se detalla la función de cada nodo clave.

![Imagen del Workflow en n8n](URL_A_LA_IMAGEN_DE_TU_WORKFLOW) <!-- Reemplaza esto por una captura de tu workflow -->

### 1. **Webhook**
-   **Propósito:** Es el punto de entrada. Recibe los datos enviados por Evolution API cada vez que llega un mensaje de WhatsApp.
-   **Configuración:** La `Test URL` se usa para pruebas, pero la `Production URL` es la que debes pegar en la configuración de webhooks de Evolution API.

### 2. **Bifurcación (Nodo `If`)**
-   **Propósito:** Revisa el mensaje entrante para determinar si contiene texto o una nota de voz (`voice_message`).
-   **Lógica:**
    -   Si es **texto**, sigue la rama superior.
    -   Si es **audio**, sigue la rama inferior.

### 3. **Ruta de Audio**
-   **`Convert to File`:** Convierte la cadena de texto `Base64` del audio a un archivo binario. La configuración clave es el `MIME Type`, que debe ser `audio/ogg` para los audios de WhatsApp.
-   **`Transcribe a recording`:** (No mostrado en detalle) Este nodo toma el archivo de audio y lo convierte en texto. Aquí es donde se usaría un modelo de transcripción, como el propio de Gemini.
-   **`final_message_voice`:** Un nodo `Set` que formatea el texto transcrito para pasarlo al agente de IA.

### 4. **Ruta de Texto**
-   **`final_message_text`:** Un nodo `Set` que simplemente prepara el texto original del mensaje para el agente de IA.

### 5. **🤖 AI Agent (El Cerebro)**
Este es el nodo central que orquesta todo.

-   **Chat Model (`Google Gemini Chat Model`):**
    -   **Propósito:** Es el modelo de lenguaje que entiende las peticiones del usuario.
    -   **Credenciales:** Para configurar la credencial, sigue los pasos de la sección **"Integración con IA"** más abajo para obtener tu clave API de Gemini.
    -   **Modelo:** Se recomienda usar `gemini-1.5-pro` o superior para poder analizar audios.

-   **Chat Memory (`Postgres Chat Memory`):**
    -   **Propósito:** Permite al agente recordar el contexto de la conversación (los últimos 10 mensajes, según la configuración). Esto es crucial para conversaciones fluidas.
    -   **Credenciales:** Utiliza las credenciales de la base de datos PostgreSQL. Los valores (`usuario`, `contraseña`, `host`) deben coincidir con los definidos en tu archivo `docker-compose.yml` y `.env`. El `host` debe ser el nombre del servicio de Docker (ej. `postgres`).
    -   **Session ID:** Se usa para mantener conversaciones separadas por cada usuario de WhatsApp.

-   **Herramientas (Tools - Nodos de Google Calendar):**
    Son las acciones que el agente puede decidir ejecutar. Cada herramienta tiene una descripción en lenguaje natural que ayuda a la IA a decidir cuál usar.
    -   `agendar_cita`: **Operación `Create`**. Se activa cuando el usuario quiere crear un nuevo evento. La IA extrae la fecha, hora y descripción del mensaje.
    -   `reprogramar_cita`: **Operación `Update`**. Se usa para mover una cita existente a otra fecha u hora.
    -   `cancelar_cita`: **Operación `Delete`**. Se activa para eliminar un evento del calendario.
    -   `consultar_citas`: **Operación `Get Many`**. Permite al usuario preguntar por sus próximas citas.

---

## 🧠 Integración con IA (Recomendación: Google Gemini)

Para que el bot funcione, necesitas una API de Inteligencia Artificial. **Se recomienda encarecidamente el uso de Google Gemini**.

-   **✅ Nivel Gratuito Generoso:** Ofrece un amplio límite de uso sin costo, ideal para este proyecto.
-   **🔊 Capacidades Multimodales:** Las versiones más recientes pueden procesar tanto texto como audio.

### Puntos Clave:

1.  **Versión del Modelo:** Utiliza **Gemini 1.5 Pro** o superior. Es la versión que puede **analizar audios** de WhatsApp.
2.  **Consumo de Tokens:** El nivel gratuito tiene un límite. Ten en cuenta que **el análisis de audio consume significativamente más tokens** que el texto.

---

## ⚙️ Gestión del Entorno

-   **Para detener todos los servicios:**
    ```bash
    docker compose down
    ```
-   **Para detener y eliminar los volúmenes** (¡cuidado, esto borrará tus flujos y credenciales de n8n!):
    ```bash
    docker compose down -v
    ```

---
