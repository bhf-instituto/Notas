

## Creación de tablas:
### v1
```mysql

CREATE TABLE users (
    id_user INT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(64) NOT NULL,
    password_hash VARCHAR(64) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE KEY uk_users_email (email)
);

CREATE TABLE expenses (
    id_expense INT AUTO_INCREMENT PRIMARY KEY,
    amount INT UNSIGNED NOT NULL,
    type ENUM('FIJO', 'VARIABLE') NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    user_id INT NOT NULL,
    collection_id INT NOT NULL,
    CONSTRAINT fk_expenses_user
        FOREIGN KEY (user_id) REFERENCES users(id_user),
    CONSTRAINT fk_expenses_collection
        FOREIGN KEY (collection_id) REFERENCES collections(id_collection)
);
ALTER TABLE expenses
ADD COLUMN description VARCHAR(255) NULL AFTER amount;


CREATE TABLE collections (
    id_collection INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(48) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    creator_user_id INT NOT NULL,
    KEY idx_collections_creator_user (creator_user_id),
    CONSTRAINT fk_collections_creator_user
        FOREIGN KEY (creator_user_id) REFERENCES users(id_user)
);


CREATE TABLE collection_users (
    collection_id INT NOT NULL,
    user_id INT NOT NULL,
    joined_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (collection_id, user_id),
    CONSTRAINT fk_collection_users_collection
        FOREIGN KEY (collection_id) REFERENCES collections(id_collection),
    CONSTRAINT fk_collection_users_user
        FOREIGN KEY (user_id) REFERENCES users(id_user)
);

```


### v2
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
## INSERTS para testear

```mysql 

# USERS
INSERT INTO users (email, password_hash) VALUES
('ana@mail.com',   'hash_ana_123'),
('bruno@mail.com', 'hash_bruno_456'),
('carla@mail.com', 'hash_carla_789');

# COLLECTIONS
INSERT INTO collections (name, creator_user_id) VALUES
('Gastos Personales', 1),
('Emprendimiento', 2);

# COLLECTION_GROUPS
INSERT INTO collection_users (collection_id, user_id) VALUES
-- Gastos personales
(1, 1),
(1, 2),

-- Emprendimiento
(2, 1),
(2, 2),
(2, 3);

# EXPENSES
INSERT INTO expenses (amount, type, user_id, collection_id) VALUES
-- Collection 1 (Gastos Personales)
(1200, 'FIJO',     1, 1),
(3500, 'VARIABLE', 1, 1),
(800,  'VARIABLE', 2, 1),
(2000, 'FIJO',     2, 1),
(1500, 'VARIABLE', 1, 1),
(900,  'VARIABLE', 2, 1),
(2700, 'FIJO',     1, 1),
(1100, 'VARIABLE', 2, 1),

-- Collection 2 (Emprendimiento)
(5000, 'FIJO',     1, 2),
(1800, 'VARIABLE', 2, 2),
(2200, 'VARIABLE', 3, 2),
(7500, 'FIJO',     2, 2),
(1300, 'VARIABLE', 1, 2),
(4100, 'FIJO',     3, 2),
(950,  'VARIABLE', 2, 2),
(6200, 'FIJO',     1, 2),
(2750, 'VARIABLE', 3, 2),
(3300, 'VARIABLE', 2, 2),
(8900, 'FIJO',     3, 2),
(1600, 'VARIABLE', 1, 2);
```


## Consultas básicas

```mysql 
## ver que usuarios pertenecen a un grupo
SELECT u.id_user, u.email 
FROM collection_users cu
JOIN users u ON u.id_user = cu.user_id
WHERE cu.collection_id = 1
```

### Modificacion de la tabla refresh_token y ON DELETE CASCADE

```mysql
## la original está arriba ↑↑↑

## modificada por chatGPT ↓↓↓
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

### 🔥 Detalle importante

`ON DELETE CASCADE`  
➡️ Si borrás el usuario, se limpian sus refresh tokens automáticamente.

```

### Resetear una tabla con relaciones

```mysql
SET FOREIGN_KEY_CHECKS = 0; 
DELETE FROM users ;
ALTER TABLE users AUTO_INCREMENT = 1;
SET FOREIGN_KEY_CHECKS = 1;
```


### Obtener resultados de una Query INSERT

```js
const [result] = await dbConnection.query(
  'INSERT INTO users (email, password_hash) VALUES (?, ?)',
  [normEmail, hashedPassword]
);

const userId = result.insertId;
```