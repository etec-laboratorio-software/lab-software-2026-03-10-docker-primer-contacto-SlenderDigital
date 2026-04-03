# YouTube Downloader

Aplicación web para descargar videos de YouTube en diferentes resoluciones. Desarrollada con FastAPI (Backend) y React + Vite (Frontend).

## 🛠 Tecnologías

### Backend
- Python 3.13
- FastAPI
- SQLModel (ORM)
- PyTubeFix (descarga de YouTube)
- FFmpeg (procesamiento de video)

### Frontend
- React 19
- Vite
- Tailwind CSS
- Axios
- Lucide React (iconos)

---

## 🐳 Opción A — Docker (recomendado)

La forma más sencilla de correr la app. Solo necesitas tener instalado **Docker Desktop** (incluye Docker Compose).

### Requisitos

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) — Windows, macOS o Linux

### 1. Clonar el repositorio

```bash
git clone https://github.com/etec-laboratorio-software/lab-software-2026-03-10-docker-primer-contacto-SlenderDigital
cd lab-software-2026-03-10-docker-primer-contacto-SlenderDigital
```

### 2. Construir y levantar los contenedores

```bash
docker compose up --build
```

La primera vez tarda ~2–3 minutos mientras descarga las imágenes base e instala dependencias.

### 3. Abrir la app

| Servicio | URL |
|---|---|
| Frontend (app) | http://localhost |
| Backend API docs (Swagger) | http://localhost:8001/docs |

### 4. Detener los contenedores

```bash
docker compose down
```

### Datos persistentes

Los videos descargados y la base de datos **sobreviven reinicios** porque se guardan en tu máquina local:

- `./storage/` → archivos `.mp4` descargados
- `./backend/database/sqlite/` → base de datos SQLite

### Despliegue en otro host o dominio

`VITE_API_URL` se compila dentro del bundle del frontend. Si desplegas en un servidor con una IP o dominio diferente, edita `docker-compose.yml` antes de construir:

```yaml
args:
  VITE_API_URL: https://tu-dominio.com/api
```

Luego reconstruye con `docker compose up --build`.
