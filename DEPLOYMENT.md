# Guía de Despliegue en la Nube (Cloud Deployment Guide)
## Sistema de Gestión de Taller Automotriz

Esta guía explica cómo desplegar el sistema completo en la nube de forma **100% gratuita** o en un servidor VPS.

---

## Arquitectura del Sistema

* **Frontend:** Single Page Application (React 18 + Vite + TypeScript).
* **Backend:** API REST de alto rendimiento en Go 1.25 (Clean Architecture, stateless JWT, orden canónico de bloqueos en transacciones).
* **Base de Datos:** MySQL 8.0 (con tablas InnoDB, claves foráneas `RESTRICT` y restricciones de unicidad activas).

---

## Opción 1: Despliegue Gratuito en la Nube (Render + TiDB Cloud / Aiven) [Recomendado]

Esta opción no requiere tarjeta de crédito y se actualiza automáticamente con cada `git push`.

### Paso 1: Base de Datos MySQL Gratuita (TiDB Cloud Serverless o Aiven)
1. Crea una cuenta gratuita en [TiDB Cloud](https://tidbcloud.com/) o [Aiven](https://aiven.io/).
2. Crea un clúster Serverless gratuito (compatible 100% con MySQL 8.0).
3. Obtén la URL de conexión estándar:
   ```text
   mysql://<usuario>:<password>@<host>:<puerto>/<database>?ssl-mode=REQUIRED
   ```
4. Ejecuta el script de esquema inicial [`database/init/01_init.sql`](database/init/01_init.sql) desde el cliente web SQL de TiDB/Aiven para crear las tablas y los usuarios iniciales.

### Paso 2: Backend en Render (Web Service Gratuito)
1. Inicia sesión en [Render.com](https://render.com) con tu cuenta de GitHub.
2. Haz clic en **New +** $\rightarrow$ **Web Service**.
3. Conecta el repositorio: `Tomasberserk/sistema-taller-automotriz`.
4. Configuración:
   - **Name:** `taller-automotriz-backend`
   - **Language / Runtime:** `Docker`
   - **Dockerfile Path:** `backend/Dockerfile`
   - **Docker Context:** `backend`
   - **Plan:** Free
5. En la sección **Environment Variables**, agrega:
   - `DATABASE_URL`: *(Tu URL de conexión de MySQL del Paso 1)*
   - `TOKEN_SECRET`: *(Cadena secreta aleatoria de 32+ caracteres)*
   - `TOKEN_TTL_MINUTE`: `480`
   - `ALLOWED_ORIGIN`: `*`
   - `REQUEST_TIMEOUT_SECOND`: `15`
   - `DATABASE_TIMEOUT_SECOND`: `5`
6. Haz clic en **Create Web Service**. Una vez desplegado, copia la URL pública (ejemplo: `https://taller-automotriz-backend.onrender.com`).

### Paso 3: Frontend en Render o Vercel (Static Site Gratuito)
1. En Render: **New +** $\rightarrow$ **Static Site** (o importa el proyecto en [Vercel](https://vercel.com)).
2. Conecta el repositorio: `Tomasberserk/sistema-taller-automotriz`.
3. Configuración:
   - **Root Directory:** `frontend`
   - **Build Command:** `npm ci && npm run build`
   - **Publish Directory:** `dist`
4. En **Environment Variables**:
   - `VITE_API_URL`: `https://taller-automotriz-backend.onrender.com/api` *(la URL de tu backend con `/api`)*
5. En Render, bajo **Redirects/Rewrites**:
   - Source: `/*` $\rightarrow$ Destination: `/index.html` $\rightarrow$ Action: `Rewrite` *(para que funcione el enrutador React Router)*.
6. Haz clic en **Create Static Site**.

¡Listo! Tendrás tu aplicación en línea con HTTPS y CI/CD activado.

---

## Opción 2: Despliegue Rápido en Railway (Todo en Uno)

1. Crea una cuenta en [Railway.app](https://railway.app).
2. **New Project** $\rightarrow$ **Provision MySQL**.
3. En el mismo proyecto: **New** $\rightarrow$ **GitHub Repo** $\rightarrow$ selecciona este repositorio.
4. En la configuración del servicio:
   - Dockerfile path: `backend/Dockerfile`
   - Conecta la variable `MYSQL_URL` a la base de datos provisionada.
   - Genera `TOKEN_SECRET` y expón el puerto.
5. Agrega el frontend conectando el mismo repo con `frontend/Dockerfile`.

---

## Opción 3: Despliegue en VPS Linux (DigitalOcean / Hetzner / AWS EC2) con Docker Compose

Si tienes un servidor VPS con Ubuntu/Debian y Docker instalado:

1. Clona el repositorio en el servidor:
   ```bash
   git clone https://github.com/Tomasberserk/sistema-taller-automotriz.git
   cd sistema-taller-automotriz
   ```
2. Copia y configura las variables de entorno:
   ```bash
   cp .env.example .env
   nano .env # Ajusta contraseñas seguras y token secret
   ```
3. Inicia la aplicación con Docker Compose:
   ```bash
   docker compose up -d
   ```
4. La aplicación estará corriendo en el puerto `8080`. Puedes apuntar tu dominio y agregar SSL gratuito con Nginx o Caddy:
   ```bash
   # Ejemplo con Caddy (HTTPS automático):
   taller.tudominio.com {
       reverse_proxy localhost:8080
   }
   ```
