# Redis Docker Template

Este repositorio contiene una plantilla para desplegar rápidamente una instancia de **Redis** utilizando Docker y Docker Compose.

## 1. Configuración de Variables de Entorno

El despliegue está configurado para utilizar un archivo `.env` que definirá la configuración del contenedor y la base de datos. 

Primero, asegúrate de tener tu archivo `.env`. Puedes crearlo basándote en el archivo de ejemplo proporcionado:

```bash
cp .env.example .env
```

### Contenido de `.env`

El archivo debe contener las siguientes variables:

- `CONTAINER_NAME`: El nombre que tendrá tu contenedor de Redis. **Se recomienda**: reemplazar `proyecto` por el nombre de tu aplicación y conservar el sufijo `_redis` (ej. `miapp_redis`).
- `NETWORK_NAME`: La red externa a la que se conectará el contenedor. **Se recomienda**: reemplazar `proyecto` por el nombre de tu aplicación y conservar el sufijo `_network` (ej. `miapp_network`). *Asegúrate de que esta red ya esté creada o créala previamente con `docker network create miapp_network`.*

## 2. Generación de Contraseñas Seguras

Para mantener la seguridad de tu base de datos, se recomienda generar contraseñas fuertes y aleatorias para la variable `REDIS_PASSWORD`.

Puedes utilizar el siguiente comando en tu terminal para generar una cadena hexadecimal segura de 64 caracteres:

```bash
openssl rand -hex 32
```

Ejecuta el comando y asigna el resultado a la siguiente variable en tu archivo `.env`:

- `REDIS_PASSWORD`: Contraseña para proteger el acceso a Redis.

## 3. Despliegue

Una vez que tengas tu archivo `.env` configurado con las contraseñas generadas y los nombres apropiados para la red y el contenedor, estás listo para levantar el servicio.

1. **Asegúrate de que la red externa existe** (si no existe, el contenedor no se podrá iniciar):
   ```bash
   docker network create <TU_NETWORK_NAME>
   ```

2. **Levanta el contenedor de Redis** ejecutando:
   ```bash
   docker-compose up -d --build
   ```

Esto construirá la imagen desde el `Dockerfile` local (basado en la imagen `redis:7-alpine`) e iniciará el contenedor en segundo plano (`-d`). 

### Persistencia de Datos

Los datos de la base de datos se guardan de manera persistente en el volumen local. Puedes encontrar todos los datos físicos en el directorio:
`./volume/data/`

Esto asegura que aunque elimines el contenedor, la información de tu base de datos seguirá segura en ese directorio.
