# CRUD Clientes — Node.js + PostgreSQL

Proyecto desarrollado como parte del módulo de Full Stack JavaScript. Implementa un servidor web en Node.js que expone una API REST para gestionar una tabla de clientes en PostgreSQL, consumida por un frontend HTML/CSS/JS.

---

## Tecnologías utilizadas

- **Node.js** — entorno de ejecución del servidor
- **Express** — framework para construir el servidor web y las rutas REST
- **PostgreSQL** — base de datos relacional
- **pg** — librería cliente para conectar Node.js con PostgreSQL
- **dotenv** — manejo de variables de entorno
- **nodemon** — reinicio automático del servidor en desarrollo
- **Thunder Client** — pruebas de endpoints REST
- **HTML / CSS / JavaScript** — interfaz de usuario

---

## Estructura del proyecto

```
crud-clientes/
├── public/              → Frontend
│   ├── index.html       → Página principal con los 4 formularios
│   ├── style.css        → Estilos visuales
│   └── app.js           → Lógica frontend (fetch API)
├── src/                 → Backend
│   ├── db.js            → Configuración del pool de conexiones PostgreSQL
│   └── routes/
│       └── clientes.js  → Endpoints CRUD
├── .env                 → Variables de entorno (no incluido en Git)
├── .gitignore           → Archivos ignorados por Git
├── package.json         → Dependencias y scripts del proyecto
└── server.js            → Punto de entrada del servidor
```

---

## Requisitos previos

- Node.js instalado
- PostgreSQL instalado y en ejecución
- Base de datos `taller_clientes` creada en PostgreSQL

---

## Instalación y configuración

**1. Clonar el repositorio**

```bash
git clone <url-del-repositorio>
cd crud-clientes
```

**2. Instalar dependencias**

```bash
npm install
```

**3. Crear el archivo `.env`** en la raíz del proyecto con las credenciales de PostgreSQL:

```
DB_HOST=localhost
DB_PORT=5432
DB_NAME=taller_clientes
DB_USER=postgres
DB_PASSWORD=tu_contraseña
```

**4. Crear la tabla en PostgreSQL**

Ejecutar en pgAdmin 4 o en cualquier cliente SQL conectado a la base de datos `taller_clientes`:

```sql
CREATE TABLE IF NOT EXISTS clientes (
  rut    VARCHAR(20)  PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  edad   INT          NOT NULL
);
```

**5. Iniciar el servidor**

```bash
# Modo desarrollo (con reinicio automático)
npm run dev

# Modo producción
npm start
```

**6. Abrir en el navegador**

```
http://localhost:3000
```

---

## Endpoints disponibles

| Método | Ruta             | Descripción                      | Códigos posibles   |
| ------ | ---------------- | -------------------------------- | ------------------ |
| GET    | `/clientes`      | Retorna todos los clientes       | 200, 500           |
| POST   | `/clientes`      | Crea un nuevo cliente            | 201, 400, 409, 500 |
| PUT    | `/clientes/:rut` | Modifica el nombre de un cliente | 200, 400, 404, 500 |
| DELETE | `/clientes/:rut` | Elimina un cliente por rut       | 200, 404, 500      |

### Ejemplos de uso

**GET /clientes**

```
GET http://localhost:3000/clientes
```

**POST /clientes**

```json
POST http://localhost:3000/clientes
Content-Type: application/json

{
  "rut": "12345678-9",
  "nombre": "Ana Torres",
  "edad": 28
}
```

**PUT /clientes/:rut**

```json
PUT http://localhost:3000/clientes/12345678-9
Content-Type: application/json

{
  "nombre": "Ana González"
}
```

**DELETE /clientes/:rut**

```
DELETE http://localhost:3000/clientes/12345678-9
```

---

## Validaciones implementadas

- Campos obligatorios en POST (`rut`, `nombre`, `edad`)
- `edad` debe ser un número entero positivo
- Detección de `rut` duplicado → responde `409 Conflict`
- Detección de `rut` inexistente en PUT y DELETE → responde `404 Not Found`
- Campo `nombre` obligatorio en PUT → responde `400 Bad Request`

---

## Consultas parametrizadas

Todas las consultas SQL utilizan parámetros posicionales (`$1`, `$2`, `$3`) para prevenir ataques de inyección SQL:

```javascript
const { rows } = await pool.query(
  "SELECT rut, nombre, edad FROM clientes WHERE rut = $1",
  [rut],
);
```

## Evidencia de pruebas

## Los pantallazos de todas las pruebas realizadas se encuentran en la carpeta [`/pantallazos`](./pantallazos/).

## Autor

**Jorge Bosch** — Aprendiz Fullstack Javascript
