# PicoClaw Copier Template

Este repositorio contiene una plantilla de [Copier](https://copier.readthedocs.io/) para desplegar rápidamente instancias de **PicoClaw** utilizando Docker Compose. Está diseñado para facilitar la gestión de múltiples proyectos independientes en un mismo equipo.

## Requisitos previos

- [Docker](https://docs.docker.com/get-docker/) y [Docker Compose](https://docs.docker.com/compose/install/)
- [Copier](https://copier.readthedocs.io/en/stable/#installation) instalado:
  ```bash
  pip install copier
  ```

## Uso de la Plantilla

Para crear un nuevo proyecto basado en PicoClaw, ejecuta:

```bash
copier copy https://github.com/angelmoya/copier-picoclaw-docker-compose.git mi-nuevo-proyecto
```

### Variables de configuración
Durante la generación, se te solicitarán los siguientes datos:
- **`agent_project_name`**: Nombre identificativo del proyecto (usado en los nombres de contenedores).
- **`launcher_token`**: Token/Password para acceder al Launcher Web.
- **`launcher_port`**: Puerto para la interfaz Web (por defecto `18800`).
- **`launcher_port_secure`**: Puerto seguro (por defecto `18790`).

## Puesta en marcha

Una vez generada la carpeta del proyecto:

1. **Primer inicio (Setup):**
   Ejecuta el siguiente comando para generar la configuración inicial:
   ```bash
   docker compose --profile launcher up
   ```
   El contenedor se detendrá tras crear los archivos base en la carpeta `./data`.

2. **Configuración de Modelos:**
   Inicia el Launcher de nuevo en segundo plano:
   ```bash
   docker compose --profile launcher up -d
   ```
   Accede a `http://localhost:PUERTO` (el puerto que elegiste en `launcher_port`).
   
   Desde la WebUI:
   - Configura un **Provider** (ej. OpenAI, Anthropic, Ollama).
   - Configura un **Channel** (ej. Telegram, Discord).

   *Ejemplo de configuración manual en `data/config.json`:*
   ```json
   {
     "model_name": "Gemma 4",
     "model": "ollama/gemma4:31b-cloud",
     "api_base": "https://ollama.com/v1",
     "api_keys": ["TU_API_KEY"]
   }
   ```

3. **Activar el Gateway:**
   Para que el bot empiece a responder en los canales configurados:
   ```bash
   docker compose --profile gateway up -d
   ```

## Comandos Útiles

- **Ver logs:** `docker compose logs -f`
- **Detener servicios:** `docker compose --profile launcher down`
- **Actualizar PicoClaw:**
  ```bash
  docker compose pull
  docker compose --profile launcher up -d
  ```

---
Para más detalles sobre el funcionamiento de PicoClaw, consulta la [documentación oficial](https://docs.picoclaw.io).
# copier-picoclaw-docker-compose
