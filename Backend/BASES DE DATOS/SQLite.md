## Crear tablas

```SQLite
CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT NOT NULL UNIQUE,
    password TEXT NOT NULL
);
```
### Tipos de datos en SQLite

SQLite es **dinámico y flexible**. Usa estos tipos principales:

- `INTEGER` → números enteros
- `REAL` → números decimales
- `TEXT` → cadenas de texto
- `BLOB` → datos binarios (imágenes, archivos, etc.)

> Nota: `VARCHAR(50)` funciona, pero internamente se trata como `TEXT`.  
> No se respeta el límite de caracteres.

## Insertar datos

```sqlite
INSERT INTO users (username, password)
VALUES ('mariano', '1234');

INSERT INTO users (username, password)
VALUES ('juan', 'abcd');
```

## Consultar datos

```sqlite
-- Todos los registros
SELECT * FROM users;

-- Filtrar por condición
SELECT * FROM users WHERE username = 'mariano';

-- Solo ciertas columnas
SELECT id, username FROM users;
```

## Actualizar y borrar

```sqlite
-- Cambiar la contraseña de un usuario
UPDATE users SET password = 'nueva_pass'
WHERE username = 'mariano';

-- Eliminar un usuario
DELETE FROM users WHERE username = 'juan';

```

## Restricciones importantes

- `NOT NULL` → no permite valores nulos
- `UNIQUE` → evita duplicados
- `PRIMARY KEY` → identificador único
- `AUTOINCREMENT` → incrementa el `id` automáticamente
Ej:
```sqlite
CREATE TABLE productos (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    nombre TEXT NOT NULL,
    precio REAL NOT NULL CHECK(precio > 0),
    stock INTEGER DEFAULT 0
)
```

## FOREIGN KEYs

SQLite soporta **llaves foráneas**, pero hay que activarlas:
```sqlite
PRAGMA foreign_keys = ON;
```
Ejemplo:
```sqlite
CREATE TABLE pedidos (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    user_id INTEGER NOT NULL,
    fecha TEXT NOT NULL,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```

## Consultas avanzadas (JOINs)

```sqlite
SELECT pedidos.id, users.username, pedidos.fecha
FROM pedidos
JOIN users ON pedidos.user_id = users.id;
```

## Utilizar SQLite con [[Node]]

```bash 
npm install sqlite sqlite3
```

```js
import { DatabaseSync } from "node:sqlite";

// Base en memoria
const db = new DatabaseSync(":memory:");

// Crear tabla
db.exec(`
  CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT NOT NULL UNIQUE,
    password TEXT NOT NULL
  )
`);

// Insertar
db.exec(`INSERT INTO users (username, password) VALUES ('mariano', '1234')`);

// Consultar
const rows = db.prepare("SELECT * FROM users").all();
console.log(rows);
```

## Ventajas y limitaciones de SQLite

✅ Ventajas:
- Super liviano (un archivo `.db`)
- No requiere servidor
- Fácil de integrar en apps pequeñas/medianas
- Compatible con la mayoría del SQL estándar
⚠️ Limitaciones:
- No recomendado para sistemas de alta concurrencia
- Manejo básico de usuarios y permisos
- No escala como MySQL/PostgreSQL


## Vistas (VIEW)
Una **vista** es una consulta guardada como si fuera una tabla virtual.  
Sirve para simplificar queries complejas o dar acceso limitado a los datos.

```sql
-- Crear vista de usuarios sin mostrar contraseña
CREATE VIEW user_list AS
SELECT id, username
FROM users;

-- Consultar la vista
SELECT * FROM user_list;

-- Eliminar vista
DROP VIEW user_list;
```

## Triggers 
Un **trigger** es un bloque de SQL que se ejecuta automáticamente cuando ocurre un evento (`INSERT`, `UPDATE`, `DELETE`).

```sql
-- Ejemplo: mantener un log cuando se inserta un usuario
CREATE TABLE user_log (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    user_id INTEGER,
    fecha TEXT
);

CREATE TRIGGER log_user_insert
AFTER INSERT ON users
BEGIN
    INSERT INTO user_log (user_id, fecha)
    VALUES (NEW.id, datetime('now'));
END;

-- Probar el trigger
INSERT INTO users (username, password) VALUES ('pepe', '1234');
SELECT * FROM user_log;

```
Explicación:
- `NEW.columna` → valores recién insertados
- `OLD.columna` → valores antes de ser borrados o actualizados

## Transacciones (TRANSACTION)

Una **transacción** agrupa varias operaciones en un solo bloque atómico:
- O se ejecutan **todas** correctamente.
- O no se ejecuta ninguna (rollback).
```sql
BEGIN TRANSACTION;

INSERT INTO users (username, password) VALUES ('ana', 'pass1');
INSERT INTO users (username, password) VALUES ('maria', 'pass2');

-- Confirmar cambios
COMMIT;

-- O deshacer cambios
ROLLBACK;
```
👉 Útil para operaciones críticas (ej: transferencias, pedidos, múltiples inserts).

## Consultas avanzadas (JOIN + agregaciones)

### LEFT JOIN
```SQL
SELECT users.username, pedidos.fecha
FROM users
LEFT JOIN pedidos ON users.id = pedidos.user_id;
```

### COUNT, AVG, SUM, GROUP BY
```SQL
-- Contar cuántos usuarios hay
SELECT COUNT(*) FROM users;

-- Promedio de stock en productos
SELECT AVG(stock) FROM productos;

-- Total de ventas por usuario
SELECT user_id, SUM(total) 
FROM pedidos
GROUP BY user_id;
```

### Subconsulta
```sql
-- Usuarios que tienen al menos un pedido
SELECT username
FROM users
WHERE id IN (SELECT user_id FROM pedidos);
```

### Seguridad básica (PRAGMA)
```sql
-- Activar llaves foráneas
PRAGMA foreign_keys = ON;

-- Ver integridad de la base de datos
PRAGMA integrity_check;

-- Definir modo WAL (mejora concurrencia de lecturas/escrituras)
PRAGMA journal_mode = WAL;
```

### Exportar e importar datos
```sql
sqlite3 mi_base.db .dump > respaldo.sql

sqlite3 nueva_base.db < respaldo.sql
```


## Manejo de JSON

```SQL
-- Crear tabla con columna JSON
CREATE TABLE usuarios_json (
  id INTEGER PRIMARY KEY,
  data JSON
);

-- Insertar JSON
INSERT INTO usuarios_json (data)
VALUES ('{ "nombre": "Mariano", "edad": 25, "roles": ["admin", "editor"] }');

-- Consultar un campo JSON
SELECT json_extract(data, '$.nombre') AS nombre
FROM usuarios_json;

-- Buscar dentro de arrays
SELECT * FROM usuarios_json
WHERE json_each.value = 'admin'
AND json_each.key = 'roles'
AND usuarios_json.data = json_each.json;

```
## Buenas prácticas
- ✅ Usa `INTEGER PRIMARY KEY AUTOINCREMENT` para IDs.
- ✅ Crea índices en columnas muy consultadas.
- ✅ Activa `PRAGMA foreign_keys = ON` para asegurar integridad referencial.
- ✅ Agrupa operaciones en transacciones para más velocidad y consistencia.
- ✅ Activa `PRAGMA foreign_keys = ON` siempre.  
- ✅ Usa `WAL` para concurrencia.  
- ✅ Usa `EXPLAIN QUERY PLAN` para optimizar.  
- ✅ Para búsquedas de texto → usa FTS.  
- ✅ Para datos semi-estructurados → usa JSON.
- ❌ No uses SQLite para sistemas de **altísima concurrencia** (miles de escrituras por segundo).
- ❌ Evita demasiados `UNIQUE` e índices innecesarios (ralentizan escrituras).
- ❌ No uses SQLite en sistemas con muchísimos usuarios concurrentes → mejor MySQL/PostgreSQL.
## Recursos: 
### Básicos:
[Documentación oficial SQLite](https://www.sqlite.org/docs.html)
[SQLite Tutorial Interactivo] (https://www.sqlitetutorial.net/)

### Avanzados:
[Características Avanzadas](https://www.sqlite.org/lang.html)
[Pragmas](https://www.sqlite.org/pragma.html)
[Triggers](https://www.sqlite.org/lang_createtrigger.html)