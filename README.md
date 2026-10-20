# API de La Cosecha

## ¿Qué problema resuelve?

Esta API gestiona el inventario y las ventas de una tienda llamada La Cosecha, gestionada por Doña Marta en Zipaquirá. El sistema permite:

- registrar y consultar categorías de productos,
- administrar el inventario de productos,
- importar inventario desde un archivo Excel,
- registrar ventas del día,
- diferenciar permisos entre administradora y vendedoras,
- iniciar sesión con token para proteger las operaciones.

La aplicación crea la base de datos al arrancar y crea un administrador inicial si no existe.

## Tecnologías

- Python
- FastAPI
- PostgreSQL
- psycopg
- OAuth2 con formularios de login
- JWT para autenticación
- CORS habilitado para orígenes configurados
- Importación de inventario desde archivos Excel (.xlsx)

## Cómo funciona la API

La API se inicia desde la app principal y expone varios módulos:

- autenticación y sesión del usuario,
- gestión de usuarios,
- gestión de categorías,
- gestión de productos e importación de inventario,
- registro y consulta de ventas.

La documentación OpenAPI se genera automáticamente con los tags y descripciones definidos en la aplicación.

## Endpoints

> Esta tabla refleja únicamente los endpoints presentes en la aplicación y no incluye rutas inventadas.

| Método | Ruta | Qué hace | Quién puede usarla |
|---|---|---|---|
| GET | `/` | Comprueba que la API está en línea. | Cualquier persona con acceso a la API. |
| POST | `/auth/login` | Inicia sesión con correo y contraseña y devuelve un token de acceso. | Cualquier usuario activo registrado. |
| GET | `/auth/yo` | Devuelve el usuario autenticado actual. | Usuario autenticado. |
| GET | `/categorias/` | Lista todas las categorías. | Usuario autenticado. |
| GET | `/categorias/{categoria_id}` | Consulta una categoría por su id. | Usuario autenticado. |
| POST | `/categorias/` | Crea una categoría nueva. | Administradora. |
| PUT | `/categorias/{categoria_id}` | Actualiza una categoría existente. | Administradora. |
| DELETE | `/categorias/{categoria_id}` | Elimina una categoría si no tiene productos asociados. | Administradora. |
| GET | `/productos/` | Lista los productos del inventario. | Usuario autenticado. |
| GET | `/productos/{producto_id}` | Consulta un producto por su id. | Usuario autenticado. |
| POST | `/productos/` | Crea un producto nuevo. | Administradora. |
| POST | `/productos/importar` | Importa el inventario desde un archivo Excel con formato validado. | Administradora. |
| PUT | `/productos/{producto_id}` | Actualiza un producto existente. | Administradora. |
| DELETE | `/productos/{producto_id}` | Elimina un producto si no tiene ventas asociadas. | Administradora. |
| GET | `/usuarios/` | Lista los usuarios del sistema. | Administradora. |
| POST | `/usuarios/` | Crea un usuario nuevo. | Administradora. |
| POST | `/ventas/` | Registra una venta a nombre del usuario autenticado. Revisa stock y calcula totales en el backend. | Usuario autenticado (vendedora o administradora). |
| GET | `/ventas/` | Muestra las ventas de un día. Si no se pasa fecha, usa el día actual. La administradora ve todas; la vendedora ve solo las suyas. | Usuario autenticado. |

## Roles y permisos

La API distingue dos tipos de uso principal:

- Administradora: puede crear, editar y eliminar categorías, productos y usuarios; puede ver todas las ventas del día y cargar inventario desde Excel.
- Vendedora: puede iniciar sesión, registrar ventas y consultar las ventas propias del día.

El token generado al hacer login identifica al usuario activo y se usa para autorizar las operaciones.

## Instalación en Windows con venv

1. Abre una terminal en la carpeta del proyecto.
2. Crea el entorno virtual:

```powershell
py -m venv .venv
```

3. Activa el entorno:

```powershell
.venv\Scripts\activate
```

4. Actualiza pip:

```powershell
python -m pip install --upgrade pip
```

5. Instala las dependencias:

```powershell
pip install -r requirements.txt
```

6. Crea tu archivo de entorno a partir del ejemplo:

```powershell
copy .env.example .env
```

7. Edita `.env` con tus valores reales.
8. Inicia la API:

```powershell
fastapi dev app/main.py
```

O bien:

```powershell
uvicorn app.main:app --reload
```

## Variables de entorno

El archivo `.env.example` contiene estas variables:

```env
DATABASE_URL=postgresql+psycopg://postgres:TU_CONTRASENA@localhost:5432/la_cosecha
SECRET_KEY=GENERA_UNA_CON_SECRETS_TOKEN_HEX_32
ADMIN_NOMBRE=Marta Ruiz
ADMIN_CORREO=marta@lacosecha.com
ADMIN_CONTRASENA=CAMBIA_ESTA_CONTRASENA
```

### Descripción

- `DATABASE_URL`: cadena de conexión a PostgreSQL.
- `SECRET_KEY`: clave secreta usada para firmar tokens.
- `ADMIN_NOMBRE`: nombre del administrador inicial.
- `ADMIN_CORREO`: correo del administrador inicial.
- `ADMIN_CONTRASENA`: contraseña del administrador inicial.

## Producción con Render y Neon

Para desplegar esta API en producción se puede usar una combinación de:

- Render para alojar la aplicación FastAPI.
- Neon para la base de datos PostgreSQL.

### Recomendación de configuración

1. Crea la base de datos en Neon y copia la URL de conexión.
2. En Render, agrega las mismas variables de entorno de `.env.example` como variables del servicio:
   - `DATABASE_URL`
   - `SECRET_KEY`
   - `ADMIN_NOMBRE`
   - `ADMIN_CORREO`
   - `ADMIN_CONTRASENA`
3. Asegúrate de que la `DATABASE_URL` apunte a tu base de datos en Neon y no a localhost.
4. Usa una `SECRET_KEY` real y segura para la firma de tokens.
5. Configura el servicio en Render para ejecutar la aplicación FastAPI con el comando apropiado para tu proyecto.

### Recomendación general

- Mantén la base de datos fuera del entorno local en producción.
- Usa credenciales reales y no los valores de ejemplo.
- Revisa que el administrador inicial exista en el despliegue para poder acceder a la API y gestionar el inventario.

## Notas finales

Esta API está diseñada para apoyar la operación de una tienda con inventario, ventas y control de roles. Su flujo principal gira alrededor de:

- autenticación con token,
- administración del inventario,
- registro de ventas con validación de stock,
- consultas por fecha y por usuario.

## Autor

© 2026 Mauricio Coneo · Ingeniero de Sistemas & Desarrollador Backend · [linkedin.com/in/ana-alvarado-instructora-full-stack](https://www.linkedin.com/in/mauricio-coneo-romero-a01b08418/)