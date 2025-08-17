# Navidrome | Un servidor de música autoalojado

Este proyecto despliega [Navidrome](https://www.navidrome.org/), un servidor de música autoalojado, usando **Docker Compose**.  
La configuración permite definir las rutas de datos y música desde un archivo `.env`.

## 📦 Requisitos

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- Carpeta de música accesible desde el host
- Carpeta para almacenar datos de configuración de Navidrome

## 📂 Estructura de Archivos

```
.
├── compose.yml
├── .env
```

## ⚙️ Configuración

1. Crea un archivo `.env` en la misma carpeta que `compose.yml`:

```env
DATA_PATH=/mnt/m2/navidrome/data
MUSIC_PATH=/mnt/m2/navidrome/music
```

> **Nota:** Ajusta las rutas según tu sistema.

2. Revisa el archivo `compose.yml`:

```yaml
services:
  navidrome:
    container_name: navidrome
    image: deluan/navidrome:latest
    user: 1000:1000
    ports:
      - "4533:4533"
    environment:
      ND_LOGLEVEL: info
      ND_DEVACTIVITYPANEL: "false"
      ND_ENABLEINSIGHTSCOLLECTOR: "false"
      ND_ENABLEEXTERNALSERVICES: "false"
    restart: unless-stopped
    volumes:
      - "${DATA_PATH}:/data"
      - "${MUSIC_PATH}:/music:ro"
```

## 🚀 Uso

Levantar el contenedor:

```bash
docker compose up -d
```

Detenerlo:

```bash
docker compose down
```

Revisar logs:

```bash
docker compose logs -f
```

## 🌐 Acceso a Navidrome

Una vez iniciado, abre en tu navegador:

```
http://localhost:4533
```

Si corres Navidrome en otra máquina, sustituye `localhost` por la IP de tu servidor.

## 🛠 Variables de Entorno

En la sección `environment:` del `compose.yml` puedes configurar opciones adicionales de Navidrome, por ejemplo:

- **ND_LOGLEVEL** — Nivel de log (`info`, `debug`, `warn`, `error`)
- **ND_DEVACTIVITYPANEL** — Mostrar o no el panel de actividad en desarrollo (`true` o `false`)

La lista completa de variables disponibles está en la documentación oficial de Navidrome:  
[https://www.navidrome.org/docs/usage/configuration-options/](https://www.navidrome.org/docs/usage/configuration-options/)

---

