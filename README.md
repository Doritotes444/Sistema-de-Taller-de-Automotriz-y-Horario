# 🚗 Automotive Technical Support Management System
> Sistema de Gestión y Soporte Técnico Automotriz para Talleres Mecánicos.

Plataforma web diseñada para digitalizar la recepción de vehículos, diagnósticos técnicos, asignación de mecánicos, seguimiento en vivo de órdenes de servicio, registro de intervenciones con repuestos y control de garantías, integrando una línea de tiempo clínica por vehículo.

---

## 🛠️ Stack Tecnológico

- **Frontend:** React 18 + TypeScript + Vite + Nginx (Reverse Proxy).
- **Backend:** Go 1.25 (Arquitectura limpia con capas de dominio, casos de uso, repositorios y transporte HTTP).
- **Base de Datos:** MySQL 8.4 (con scripts de migración DDL y datos semilla de arranque).
- **Orquestación:** Docker & Docker Compose.

---

## 🚀 Cómo Levantarlo en un Servidor Local (Docker)

### 1. Requisitos Previos
- Tener instalado **Docker** y **Docker Desktop** en tu computadora.
- Asegúrate de abrir **Docker Desktop** y verificar que el motor esté en estado **Running** (verde).

### 2. Iniciar el Sistema Completo
Abre tu terminal en la carpeta raíz del proyecto y ejecuta:

```bash
docker compose up --build -d
```

Este comando:
1. Iniciará el contenedor de **MySQL 8.4** y aplicará automáticamente todas las migraciones de base de datos y los datos semilla (`database/init/01_init.sql`).
2. Esperará a que MySQL esté saludable (*healthy*).
3. Compilará e iniciará el contenedor del **Backend en Go** en el puerto interno 8080.
4. Compilará el **Frontend en React** y lo servirá mediante **Nginx**, exponiendo la aplicación en tu máquina local.

### 3. Abrir la Aplicación en el Navegador
Una vez los contenedores estén corriendo, abre tu navegador web en:
👉 **[http://localhost:8080](http://localhost:8080)**

---

## 🔑 Credenciales de Acceso (Usuarios Precargados)

El sistema incluye perfiles preconfigurados para pruebas inmediatas:

| Rol | Usuario | Contraseña | Descripción |
| :--- | :--- | :--- | :--- |
| **Administrador** | `admin` | `Admin#Secure2026!` | Jefe de taller: Gestión total de clientes, vehículos, órdenes y reportes. |
| **Técnico 1** | `jperez` | `Tech#Perez2026!` | Juan Pérez (Especialidad: Motor y transmisión). |
| **Técnico 2** | `lramirez` | `Tech#Ramirez2026!` | Laura Ramírez (Especialidad: Frenos y suspensión). |

---

## 🛑 Comandos Útiles de Operación

- **Ver logs en tiempo real:**
  ```bash
  docker compose logs -f
  ```
- **Ver logs de un servicio específico (ej. backend o db):**
  ```bash
  docker compose logs -f backend
  ```
- **Detener el servidor local:**
  ```bash
  docker compose down
  ```
- **Reiniciar desde cero (borrar datos y recrear la base de datos limpia):**
  ```bash
  docker compose down -v
  docker compose up --build -d
  ```

---

## 🧪 Pruebas Automatizadas (Testing Local)

Si deseas correr las pruebas unitarias directamente en tu entorno de desarrollo:

### Backend (Go)
```bash
cd backend
go test ./...
```

### Frontend (React & TypeScript)
```bash
cd frontend
npm ci
npm test
```
