# Asistente Calendario n8n
Este proyecto es un bot automatizador que asigna tareas en Google Calendar utilizando la integración entre evolution-api y n8n. Está diseñado como una solución simple pero poderosa para gestionar y distribuir tareas de manera automática, facilitando la organización y el seguimiento de actividades en equipos de trabajo o proyectos personales.

# evolutionapi

Este proyecto contiene un entorno completo con Docker Compose para levantar **evolution-api**, **n8n**, **PostgreSQL** y **Redis**.

## ¿Cómo iniciar el entorno?

1. **Clona el repositorio:**
   ```bash
   git clone <URL_DEL_REPOSITORIO>
   cd evolutionapi
   ```

2. **Configura el archivo `.env`**  
   Asegúrate de tener el archivo `.env` en la raíz del proyecto. Puedes usar el que ya viene en el repositorio o ajustarlo según tus necesidades.

3. **Levanta los servicios:**
   ```bash
   docker compose up -d
   ```

4. **Accede a las aplicaciones:**
   - n8n: [http://localhost:5678](http://localhost:5678)
   - evolution-api: [http://localhost:8080](http://localhost:8080)

## Notas

- Todos los servicios se comunican usando los nombres definidos en `docker-compose.yml`.
- Es necesario tener **Docker** y **Docker Compose** instalados.
- Para detener los servicios:
  ```bash
  docker compose down
  ```

---
