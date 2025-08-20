# 🤖 Asistente de Calendario con n8n y Evolution API

Este proyecto despliega un entorno automatizado que conecta **Evolution API** (una API no oficial para WhatsApp) con **n8n** para crear eventos en **Google Calendar**.

Es una solución poderosa y auto-hospedada para convertir mensajes de WhatsApp en eventos de calendario de forma automática, ideal para agendar citas, recordatorios o tareas directamente desde tus conversaciones.

![Diagrama del flujo de automatización](https://i.imgur.com/example.png)  <!-- Reemplaza esto con una URL a tu propio diagrama o imagen -->

---

## ✨ Características Principales

-   **Entorno Contenerizado:** Todo listo para desplegar con un solo comando gracias a Docker Compose.
-   **Componentes Incluidos:**
    -   🟢 **Evolution API:** Para recibir y enviar mensajes de WhatsApp.
    -   ⚙️ **n8n:** El motor de automatización que conecta los servicios.
    -   🐘 **PostgreSQL:** Base de datos persistente para n8n.
    -   ⚡ **Redis:** Para optimizar el rendimiento de n8n.
-   **Automatización Inteligente:** Diseñado para parsear mensajes y agendar tareas sin intervención manual (usando IA).
-   **Escalable y Personalizable:** Puedes modificar fácilmente los flujos de n8n para añadir más lógica o integraciones.

---

## 📋 Prerrequisitos

Antes de comenzar, asegúrate de tener instalado lo siguiente en tu sistema:

-   [**Docker**](https://docs.docker.com/get-docker/)
-   [**Docker Compose**](https://docs.docker.com/compose/install/)

---

## 🚀 Guía de Inicio Rápido

Sigue estos pasos para poner en marcha todo el entorno:

1.  **Clona el repositorio:**
    ```bash
    git clone <URL_DEL_REPOSITORIO>
    cd nombre-del-directorio
    ```

2.  **Configura las variables de entorno:**
    Crea un archivo `.env` a partir del ejemplo proporcionado.
    ```bash
    # Copia el archivo de ejemplo (si tienes uno)
    cp .env.example .env
    ```
    Abre el archivo `.env` y ajusta las variables según tus necesidades.

3.  **Levanta los servicios con Docker Compose:**
    Este comando construirá las imágenes e iniciará todos los contenedores en segundo plano (`-d`).
    ```bash
    docker compose up -d
    ```

4.  **Verifica que todo esté funcionando:**
    ```bash
    docker compose logs -f
    ```

5.  **¡Accede a las aplicaciones!**
    -   **n8n:** Abre [http://localhost:5678](http://localhost:5678) en tu navegador.
    -   **Evolution API:** La API estará disponible en [http://localhost:8080](http://localhost:8080). Consulta la [documentación oficial de Evolution API](https://documentation.evolution-api.com/) para generar la instancia y escanear el código QR.

---

## 🛠️ Configuración Post-Instalación

1.  **En n8n:**
    -   Importa el flujo de trabajo (`workflow.json`) proporcionado en este repositorio.
    -   Configura las credenciales para **Google Calendar (OAuth2)**.
    -   Configura las credenciales para la **API de IA** que elijas (ver recomendación abajo).

2.  **En Evolution API:**
    -   Crea tu instancia de WhatsApp.
    -   Configura la URL del **Webhook** de n8n en tu instancia para que notifique a n8n cada vez que llegue un mensaje.

---

## 🧠 Integración con IA (Recomendación: Google Gemini)

Para interpretar el lenguaje natural de los mensajes (texto o audio) y extraer los detalles del evento (título, fecha, hora), necesitas conectar una API de Inteligencia Artificial en tu flujo de n8n.

**Se recomienda encarecidamente el uso de Google Gemini**, principalmente por estas razones:

-   **✅ Nivel Gratuito muy Generoso:** A la fecha, Gemini ofrece el nivel gratuito más amplio del mercado, lo que permite desarrollar y operar este proyecto sin costos iniciales.
-   **🔊 Capacidades Multimodales:** Las versiones más recientes pueden procesar tanto texto como audio.

### Puntos Clave a Considerar:

1.  **Versión del Modelo:** Para la funcionalidad completa, se recomienda utilizar **Gemini 1.5 Pro** o superior. Esta es la versión que puede **analizar audios**, permitiendo que tu bot agende eventos a partir de notas de voz. Versiones anteriores como Gemini 1.0 Pro solo procesan texto.

2.  **Consumo de Tokens:** Las APIs de IA miden el uso en *tokens* (fragmentos de texto). El nivel gratuito tiene un límite generoso de tokens por minuto/día. Ten en cuenta que **los modelos más avanzados y el análisis de audio consumen significativamente más tokens** que un simple análisis de texto. Ajusta tus flujos para un uso eficiente.

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
