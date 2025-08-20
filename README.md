🤖 Asistente de Calendario con n8n y Evolution API
Este proyecto despliega un entorno automatizado que conecta Evolution API (una API no oficial para WhatsApp) con n8n para crear eventos en Google Calendar.
Es una solución poderosa y auto-hospedada para convertir mensajes de WhatsApp en eventos de calendario de forma automática, ideal para agendar citas, recordatorios o tareas directamente desde tus conversaciones.
![alt text](URL_A_UN_DIAGRAMA_O_IMAGEN_EXPLICATIVA)
(Opcional: crear un diagrama simple mejora mucho la comprensión)
✨ Características Principales
Entorno Contenedorizado: Todo listo para desplegar con un solo comando gracias a Docker Compose.
Componentes Incluidos:
🟢 Evolution API: Para recibir y enviar mensajes de WhatsApp.
⚙️ n8n: El motor de automatización que conecta los servicios.
🐘 PostgreSQL: Base de datos persistente para n8n.
⚡ Redis: Para optimizar el rendimiento de n8n.
Automatización Inteligente: Diseñado para parsear mensajes y agendar tareas sin intervención manual.
Escalable y Personalizable: Puedes modificar fácilmente los flujos de n8n para añadir más lógica o integraciones.
📋 Prerrequisitos
Antes de comenzar, asegúrate de tener instalado lo siguiente en tu sistema:
Docker
Docker Compose
🚀 Guía de Inicio Rápido
Sigue estos pasos para poner en marcha todo el entorno:
Clona el repositorio:
code
Bash
git clone <URL_DEL_REPOSITORIO>
cd nombre-del-directorio
Configura las variables de entorno:
Crea un archivo .env a partir del ejemplo proporcionado.
code
Bash
# Copia el archivo de ejemplo (si tienes uno)
cp .env.example .env
Abre el archivo .env y ajusta las variables, especialmente las relacionadas con las credenciales de n8n y la configuración de Evolution API.
Levanta los servicios con Docker Compose:
Este comando construirá las imágenes (si es necesario) y iniciará todos los contenedores en segundo plano (-d).
code
Bash
docker compose up -d
Verifica que todo esté funcionando:
Puedes revisar los logs de los servicios para asegurarte de que no haya errores.
code
Bash
docker compose logs -f
¡Accede a las aplicaciones!
n8n: Abre http://localhost:5678 en tu navegador. Deberás crear una cuenta de administrador la primera vez.
Evolution API: La API estará disponible en http://localhost:8080. Consulta la documentación de Evolution API para saber cómo generar la instancia y escanear el código QR.
🛠️ Configuración Post-Instalación
Una vez que el entorno está en línea, necesitas configurar la conexión entre los servicios.
1. Configurar n8n
Importar el Flujo de Trabajo: Importa el archivo workflow.json (si lo incluyes en tu repo) en tu instancia de n8n.
Crear Credenciales: Dentro de n8n, deberás configurar las credenciales para:
Google Calendar (OAuth2): Sigue las instrucciones aquí para generar tus credenciales en Google Cloud Console. <-- (Puedes enlazar a la sección que te sugerí en la respuesta anterior).
Webhook: n8n generará una URL de webhook que deberás configurar en Evolution API.
2. Configurar Evolution API
Crear una Instancia: Usa la API de Evolution para crear una nueva instancia de WhatsApp.
Configurar el Webhook: Configura la URL del webhook de n8n en tu instancia de Evolution API para que notifique a n8n cada vez que llegue un nuevo mensaje.
⚙️ Gestión del Entorno
Para detener todos los servicios:
code
Bash
docker compose down
Para detener y eliminar los volúmenes (¡cuidado, esto borrará tus datos de n8n!):
code
Bash
docker compose down -v
