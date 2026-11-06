# 🎬 Cine Crazy - Sistema de Reservas de Películas

Sistema interactivo de gestión de cartelera y reserva de boletos de cine por consola (CLI), desarrollado en **Python** utilizando **SQLAlchemy** como ORM y **PostgreSQL** como motor de base de datos.

---

## ✨ Características Principales

- **Autenticación y Seguridad:** Inicio de sesión con contraseñas protegidas mediante hashing (`Werkzeug Security`).
- **Control de Acceso por Roles:**
  - **Cliente (`usuario`):**
    - Visualización de la cartelera completa, sinopsis, géneros, salas, horarios y disponibilidad de asientos.
    - Reserva de entradas con validación de cupos disponibles y descuento automático de asientos en tiempo real.
    - Consulta del historial de reservas propias.
  - **Administrador (`admin`):**
    - Todas las funciones de cliente.
    - Registro de nuevas películas en la cartelera.
    - Programación de nuevas funciones (horarios, salas y capacidad de asientos).
    - Eliminación de películas (con borrado en cascada de sus horarios y reservas asociadas).
- **Inicialización Automática (Seed Data):** Al ejecutar el sistema por primera vez, se crean automáticamente las tablas en la base de datos junto con usuarios de prueba y una cartelera inicial (*The Batman*, *Spider-Man: No Way Home*, *Interestelar*).

---

## 🛠️ Tecnologías Utilizadas

- **Lenguaje:** Python 3.10+
- **Base de Datos:** PostgreSQL
- **ORM:** SQLAlchemy
- **Seguridad / Hashing:** Werkzeug (`generate_password_hash`, `check_password_hash`)

---

## 📂 Estructura del Proyecto

```text
cine/
├── main.py          # Punto de entrada, menú interactivo en consola y carga de datos semilla
├── database.py      # Configuración de la conexión a PostgreSQL y sesión de SQLAlchemy
├── models.py        # Modelos ORM (Usuario, Pelicula, Horario, Reserva) y relaciones
├── crud.py          # Operaciones CRUD y lógica de negocio (reservas, autenticación, cartelera)
├── tablas.sql       # Script SQL de referencia del diseño relacional
└── README.md        # Documentación del proyecto
```

---

## ⚙️ Requisitos Previos e Instalación

### 1. Requisitos
- **Python 3.x** instalado.
- **PostgreSQL** instalado y en ejecución local (puerto `5432`).

### 2. Configurar la Base de Datos
Crea una base de datos vacía en PostgreSQL llamada `cine_crazy`:

```sql
CREATE DATABASE cine_crazy;
```

> **Nota:** La cadena de conexión está configurada en `database.py`:
> ```python
> DATABASE_URL = "postgresql://postgres:1234@localhost:5432/cine_crazy"
> ```
> Si tu usuario o contraseña de PostgreSQL son distintos, actualiza esa línea en `database.py`.

### 3. Instalar Dependencias
Se recomienda crear un entorno virtual e instalar las librerías necesarias:

```bash
# Crear entorno virtual (opcional)
python -m venv venv

# Activar entorno virtual en Windows
venv\Scripts\activate

# Instalar dependencias
pip install sqlalchemy psycopg2-binary werkzeug
```

---

## 🚀 Ejecución y Uso

Para iniciar el sistema, ejecuta el archivo principal:

```bash
python main.py
```

Al iniciar por primera vez, el sistema generará las tablas automáticamente y cargará dos cuentas por defecto para que puedas probar todos los flujos de inmediato:

### 🔑 Credenciales de Prueba

| Rol | Usuario | Contraseña | Permisos |
| :--- | :--- | :--- | :--- |
| **Administrador** | `admin` | `1234` | Ver cartelera, reservar, agregar/eliminar películas y crear horarios |
| **Cliente** | `usuario` | `1234` | Ver cartelera, realizar reservas y consultar sus boletos reservados |

---

## 🗄️ Modelo de Datos (ORM)

El sistema se compone de 4 entidades principales definidas en `models.py`:

1. **`Usuario` (`usuarios`):** Almacena `id`, `nombre_usuario`, `correo`, `contrasena_hash`, `rol` (`admin` / `usuario`) y estado `activo`.
2. **`Pelicula` (`peliculas`):** Almacena `id`, `titulo`, `descripcion`, `genero` e `imagen_url`. Tiene relación uno a muchos con `Horario`.
3. **`Horario` (`horarios`):** Representa una función específica con `pelicula_id`, `fecha_hora`, `sala` y `asientos_disponibles`.
4. **`Reserva` (`reservas`):** Registra las entradas adquiridas vinculando `usuario_id`, `horario_id`, `cantidad_asientos` y `fecha_reserva`.

