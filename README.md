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

---

## 💻 Opción B — Instalación manual (sin Docker)

### Requisitos previos

- **Python** 3.8 o superior
- **pip** 20.0 o superior
- **Node.js** 18.0 o superior
- **npm** 9.0 o superior

```bash
# Verificar versiones
python --version
pip --version
node --version
npm --version
```

### 1. Clonar el repositorio

```bash
git clone https://github.com/etec-laboratorio-software/lab-software-2026-03-10-docker-primer-contacto-SlenderDigital
cd lab-software-2026-03-10-docker-primer-contacto-SlenderDigital
```

### 2. Configurar el Backend

#### Crear y activar entorno virtual

```bash
python -m venv .venv

# Windows
.venv\Scripts\activate

# macOS / Linux
source .venv/bin/activate
```

#### Instalar dependencias

```bash
pip install -r backend/requirements.txt
```

#### Nota sobre FFmpeg

`imageio-ffmpeg` descarga FFmpeg automáticamente. Si hay problemas, instálalo manualmente:

- **Windows** → [ffmpeg.org/download.html](https://ffmpeg.org/download.html)
- **macOS** → `brew install ffmpeg`
- **Arch Linux** → `sudo pacman -S ffmpeg`
- **Ubuntu/Debian** → `sudo apt install ffmpeg`

#### Crear carpeta de almacenamiento

```bash
mkdir storage
```

### 3. Configurar el Frontend

```bash
cd frontend
npm install
```

El archivo `frontend/.env` ya contiene la URL por defecto:

```env
VITE_API_URL=http://localhost:8001/api
```

### 4. Ejecutar la aplicación

Abre **dos terminales** desde la raíz del proyecto.

**Terminal 1 — Backend** (con el entorno virtual activado):

```bash
python backend/server.py
```

**Terminal 2 — Frontend:**

```bash
cd frontend
npm run dev
```

| Servicio | URL |
|---|---|
| Frontend | http://localhost:3000 |
| Backend API docs | http://localhost:8001/docs |

---

## 📁 Estructura del Proyecto

```
.
├── backend/
│   ├── database/
│   │   ├── models.py          # Modelos de base de datos
│   │   └── database.py        # Configuración de DB
│   ├── routers/
│   │   └── history.py         # Endpoints de historial
│   ├── youtube/
│   │   └── yt_logic.py        # Lógica de descarga
│   ├── config.py              # Configuración
│   ├── main.py                # Aplicación FastAPI
│   ├── server.py              # Punto de entrada
│   ├── utils.py               # Utilidades
│   └── requirements.txt       # Dependencias Python
├── frontend/
│   ├── src/
│   │   ├── components/        # Componentes React
│   │   │   ├── Header.jsx
│   │   │   ├── SearchBar.jsx
│   │   │   ├── VideoCard.jsx
│   │   │   └── History.jsx
│   │   ├── App.jsx            # Componente principal
│   │   ├── main.jsx           # Punto de entrada
│   │   └── index.css          # Estilos globales
│   ├── package.json
│   └── vite.config.js
├── storage/                   # Videos descargados (generado automáticamente)
├── Dockerfile.backend
├── Dockerfile.frontend
├── docker-compose.yml
├── nginx.conf
└── .dockerignore
```

---

## 🔌 Endpoints de la API

| Método | Endpoint | Descripción |
|---|---|---|
| `GET` | `/api/yt/video-info` | Obtener información del video |
| `GET` | `/api/yt/download_video` | Descargar video |
| `GET` | `/api/history/videos` | Obtener historial completo |
| `DELETE` | `/api/history/clear` | Limpiar todo el historial |
| `DELETE` | `/api/history/video/{id}` | Eliminar video específico |

Documentación interactiva completa disponible en `http://localhost:8001/docs`.

---

## 🎯 Cómo usar la app

1. **Buscar video** — pega la URL de YouTube en la barra de búsqueda
2. **Seleccionar resolución** — elige la calidad deseada (hasta 4K si está disponible)
3. **Descargar** — haz clic en "Descargar Video"
4. **Historial** — accede a tus búsquedas anteriores desde el botón "Historial"

---

## 🐛 Solución de Problemas

### Docker

**Los contenedores no levantan**
```bash
# Ver logs detallados
docker compose logs -f

# Solo el backend
docker compose logs -f backend

# Solo el frontend
docker compose logs -f frontend
```

**Conflicto de puertos** — si el 80 u 8001 ya están en uso, cambia el puerto en `docker-compose.yml`:
```yaml
ports:
  - "8080:80"   # cambia 80 por cualquier puerto libre
```

Luego reconstruye con `docker compose up --build`.

**Reconstruir desde cero** (útil si algo quedó en un estado inconsistente):
```bash
docker compose down --volumes
docker compose up --build
```

### Instalación manual

**El backend no inicia**
- Verifica que el entorno virtual esté activado
- Asegúrate de que el puerto 8001 no esté en uso
- Revisa que las dependencias estén instaladas: `pip list`

**El frontend no se conecta al backend**
- Confirma que el backend esté corriendo
- Verifica que `frontend/.env` apunte a `http://localhost:8001/api`
- Revisa la consola del navegador para errores CORS

**Error al descargar videos**
- Verifica tu conexión a internet
- Asegúrate de que la URL de YouTube sea válida y el video no sea privado
- Confirma que FFmpeg esté instalado: `ffmpeg -version`

**Error "Module not found"**

Backend:
```bash
pip install -r backend/requirements.txt --upgrade
```

Frontend:
```bash
cd frontend
rm -rf node_modules package-lock.json
npm install
```
