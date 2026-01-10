# MySQL
### -v1
```mysql
CREATE TABLE users (
    id_user INT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(64) NOT NULL,
    password_hash VARCHAR(64) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE KEY uk_users_email (email)
);

CREATE TABLE expense_groups (
    id_group INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(48) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    creator_user_id INT NOT NULL,
    KEY idx_groups_creator_user (creator_user_id),
    CONSTRAINT fk_groups_creator_user
        FOREIGN KEY (creator_user_id) REFERENCES users(id_user)
);

CREATE TABLE expense_group_users (
    group_id INT NOT NULL,
    user_id INT NOT NULL,
    joined_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (group_id, user_id),
    CONSTRAINT fk_group_users_group
        FOREIGN KEY (group_id) REFERENCES expense_groups(id_group)
        ON DELETE CASCADE,
    CONSTRAINT fk_group_users_user
        FOREIGN KEY (user_id) REFERENCES users(id_user)
        ON DELETE CASCADE
);

CREATE TABLE expenses (
    id_expense INT AUTO_INCREMENT PRIMARY KEY,
    amount DECIMAL(10,2) NOT NULL,
    type ENUM('FIJO', 'VARIABLE') NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    user_id INT NOT NULL,
    group_id INT NOT NULL,
    KEY idx_expenses_group (group_id),
    KEY idx_expenses_group_date (group_id, created_at),
    CONSTRAINT fk_expenses_user
        FOREIGN KEY (user_id) REFERENCES users(id_user),
    CONSTRAINT fk_expenses_group
        FOREIGN KEY (group_id) REFERENCES expense_groups(id_group)
        ON DELETE CASCADE
);

CREATE TABLE refresh_tokens (
	id INT NOT NULL AUTO_INCREMENT,
	user_id INT NOT NULL,
	token VARCHAR(500) NOT NULL,
	expires_at DATETIME NOT NULL,
	created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
	PRIMARY KEY (id),
	KEY idx_refresh_user (user_id),
	CONSTRAINT fk_refresh_user
	    FOREIGN KEY (user_id)
	    REFERENCES users(id_user)
	    ON DELETE CASCADE
);
```

## v2 
-  Agrego la columna `description` a `expenses`.
-  Agrego las tablas `tags` y `expense_tags` (n:n).

```mysql
CREATE TABLE users (
    id_user INT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(64) NOT NULL,
    password_hash VARCHAR(64) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE KEY uk_users_email (email)
);

CREATE TABLE expense_groups (
    id_group INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(48) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    creator_user_id INT NOT NULL,
    KEY idx_groups_creator_user (creator_user_id),
    CONSTRAINT fk_groups_creator_user
        FOREIGN KEY (creator_user_id) REFERENCES users(id_user)
);

CREATE TABLE expense_group_users (
    group_id INT NOT NULL,
    user_id INT NOT NULL,
    joined_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (group_id, user_id),
    CONSTRAINT fk_group_users_group
        FOREIGN KEY (group_id) REFERENCES expense_groups(id_group)
        ON DELETE CASCADE,
    CONSTRAINT fk_group_users_user
        FOREIGN KEY (user_id) REFERENCES users(id_user)
        ON DELETE CASCADE
);

CREATE TABLE expenses (
    id_expense INT AUTO_INCREMENT PRIMARY KEY,
    amount INT NOT NULL,
    description VARCHAR(128) NOT NULL,
    type ENUM('FIJO', 'VARIABLE') NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    user_id INT NOT NULL,
    group_id INT NOT NULL,
    KEY idx_expenses_group (group_id),
    KEY idx_expenses_group_date (group_id, created_at),
    CONSTRAINT fk_expenses_user
        FOREIGN KEY (user_id) REFERENCES users(id_user),
    CONSTRAINT fk_expenses_group
        FOREIGN KEY (group_id) REFERENCES expense_groups(id_group)
        ON DELETE CASCADE
);

CREATE TABLE tags (
    id_tag INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(32) NOT NULL,
    UNIQUE KEY uk_tags_name (name)
);

CREATE TABLE expense_tags (
    expense_id INT NOT NULL,
    tag_id INT NOT NULL,
    PRIMARY KEY (expense_id, tag_id),
    KEY idx_expense_tags_tag (tag_id),
    CONSTRAINT fk_expense_tags_expense
        FOREIGN KEY (expense_id)
        REFERENCES expenses(id_expense)
        ON DELETE CASCADE,
    CONSTRAINT fk_expense_tags_tag
        FOREIGN KEY (tag_id)
        REFERENCES tags(id_tag)
);

CREATE TABLE refresh_tokens (
    id INT NOT NULL AUTO_INCREMENT,
    user_id INT NOT NULL,
    token VARCHAR(500) NOT NULL,
    expires_at DATETIME NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (id),
    UNIQUE KEY uk_refresh_token (token),
    KEY idx_refresh_user (user_id),
    CONSTRAINT fk_refresh_user
        FOREIGN KEY (user_id)
        REFERENCES users(id_user)
        ON DELETE CASCADE
);

```
# Prompt:
## 1er prompt:
Vas a ser un experto en base de datos y vas a ayudarme a modelar una DB MySQL.

El backend es NODEJS y mysql corriendo en un linux ubuntu server. 

Tengo un sistema simple dedicado recolectar datos de gastos hechos por personas para su futuro análisis. Vamos a modelar y/o corregir la base de datos lo mas correcta y profesionalmente posible,  pero siempre teniendo en cuenta que la complejidad de la app es muy baja. En pocas palabras, profesional pero simple.

Flujo aproximado: el usuario se registra, se loguea, crea grupos, luego crea gastos dentro de esos grupos. Fin.

Algunas reglas:

- Los gastos están relacionados uno a uno con el usuario que lo creo, y con el grupo en el que fue cargado. O sea que cada gasto tiene asignado UN usuario y UN grupo. 
- Los gastos no son compartidos.
- Los gastos tendrán tags asociados (0 o varios).
- Los tags están designados por mi (hardcode para mantener integridad)
- El usuario que crea un grupo es el administrador del mismo.
- Los grupos tendrán un administrador y 0 o varios usuarios participantes.
- Los usuarios solamente pueden crear gastos si participan (admin o participante) en un grupo, y solo podrán cargar gastos en los grupos que pertenezcan.

Esta app no es que solo la tengo en mente, si no que ya había comenzado el backend y la base de datos pero el sistema de tags para gastos es nuevo. Asi que quiero rehacer la DB pero tomando el proyecto inicial como base. 

Todo el sistema de usuarios ya está funcionando, se guardan las contraseñas hasheadas, se deshashean cuando se verifican, se crean la access_token y refresh_token (se guarda en la db, tiene su tabla) y almacenan en las cookies, se verifican esas credenciales luego para acceder a endpoint, todo anda bien. Eso no quiero modificarlo. 

lo que quiero hacer mas que nada es crear el sistema de tags para los gastos. Te voy a adjuntar archivos clave del proyecto: 

- Server.js:
	...
- app.js:
	...
- connectionMySql.js :
	...
- package.json:
	...


- Middlewares:
   - checktoken.middleware.js:
	...
- checkgroupacces.middleware.js:
	...

y finalmente los controllers te los subo directamente, son auth, group, index, invite, refreshaccestoken 

- IMPORTANTE ! : 
Antes de continuar quiero que me digas si tenés alguna duda. Cualquier informacion que creas que me hace falta darte te la dré, quiero que entiendas todo antes de hacer algo.


## 2do prompt

Le paso las tablas (sin tags)

1.1. descripción libre y corta, hay que agregarlo
1.2 `type` (`FIJO` / `VARIABLE`) es un campo propio, no es un tag.
1.3 una misma moneda, el numero es entero, sin decimales.

2.1. Quiero una **tabla `tags`** con registros fijos insertados a mano
2.2. Los tags son globales 
2.3. Si
2.4. si, 4.

3.1. solo se lo considera admin por `creator_user_id`, usamos una sola fuente de la verdad.
3.2. no, solo esos. Admin/participante.

4.1. Borrado físico. Ya que la DB se backupeará frecuentemente. 
4.2 si puede.

5.1. Si
5.2. La app puede llegar a ser usada por 8 personas máximo, 4 es el número de usuarios mas acertado.