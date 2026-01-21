## Aclaración 
Uso **InnoDB**, claves foráneas explícitas y constraints donde MySQL lo permite de forma segura.

> Nota: MySQL no hace cumplir `CHECK` antes de 8.0.16 y aun hoy es limitado; por eso algunas reglas **deben reforzarse en backend**, pero dejo los `CHECK` igualmente documentados.

## init.sql

```mysql


SET FOREIGN_KEY_CHECKS = 0;

DROP TABLE IF EXISTS expenses;
DROP TABLE IF EXISTS providers;
DROP TABLE IF EXISTS categories;
DROP TABLE IF EXISTS sets_users;
DROP TABLE IF EXISTS sets;
DROP TABLE IF EXISTS users;

SET FOREIGN_KEY_CHECKS = 1;



CREATE TABLE users (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(150) NOT NULL,
    password_hash CHAR(60) NOT NULL,
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,

    UNIQUE KEY uq_users_email (email)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- =====================================================
-- SETS
-- =====================================================

CREATE TABLE sets (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- =====================================================
-- SET_USERS (pivot)
-- =====================================================

CREATE TABLE set_users (
    set_id INT UNSIGNED NOT NULL,
    user_id INT UNSIGNED NOT NULL,
    role TINYINT UNSIGNED NOT NULL DEFAULT 0,
    -- 0 = member, 1 = admin

    PRIMARY KEY (set_id, user_id),

    CONSTRAINT fk_set_users_set
        FOREIGN KEY (set_id) REFERENCES sets(id)
        ON DELETE CASCADE,

    CONSTRAINT fk_set_users_user
        FOREIGN KEY (user_id) REFERENCES users(id)
        ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- =====================================================
-- CATEGORIES
-- =====================================================

CREATE TABLE categories (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    set_id INT UNSIGNED NOT NULL,
    name VARCHAR(80) NOT NULL,
    expense_type TINYINT UNSIGNED NOT NULL,
    -- 0 = fijo, 1 = variable

    CONSTRAINT uq_category_set_type_name
        UNIQUE (set_id, expense_type, name),

    CONSTRAINT fk_categories_set
        FOREIGN KEY (set_id) REFERENCES sets(id)
        ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- =====================================================
-- PROVIDERS
-- =====================================================

CREATE TABLE providers (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    set_id INT UNSIGNED NOT NULL,
    name VARCHAR(100) NOT NULL,
    contact_name VARCHAR(100) NULL,
    phone VARCHAR(30) NULL,

    CONSTRAINT fk_providers_set
        FOREIGN KEY (set_id) REFERENCES sets(id)
        ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- =====================================================
-- EXPENSES (core)
-- =====================================================

CREATE TABLE expenses (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    set_id INT UNSIGNED NOT NULL,
    user_id INT UNSIGNED NOT NULL,
    category_id INT UNSIGNED NOT NULL,
    provider_id INT UNSIGNED NULL,

    expense_type TINYINT UNSIGNED NOT NULL,
    -- 0 = fijo, 1 = variable

    amount INT UNSIGNED NOT NULL,
    description VARCHAR(255) NULL,
    expense_date DATE NOT NULL,
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,

    CONSTRAINT fk_expenses_set
        FOREIGN KEY (set_id) REFERENCES sets(id),

    CONSTRAINT fk_expenses_user
        FOREIGN KEY (user_id) REFERENCES users(id),

    CONSTRAINT fk_expenses_category
        FOREIGN KEY (category_id) REFERENCES categories(id),

    CONSTRAINT fk_expenses_provider
        FOREIGN KEY (provider_id) REFERENCES providers(id)
        ON DELETE SET NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;


CREATE TABLE refresh_tokens (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    user_id INT UNSIGNED NOT NULL,
    token VARCHAR(500) NOT NULL,
    expires_at DATETIME NOT NULL,
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,

    CONSTRAINT fk_refresh_tokens_user
        FOREIGN KEY (user_id)
        REFERENCES users(id)
        ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- =====================================================
-- INDEXES (only where they add real value)
-- =====================================================

CREATE INDEX idx_expenses_set_date
    ON expenses (set_id, expense_date);

CREATE INDEX idx_expenses_category
    ON expenses (category_id);

CREATE INDEX idx_expenses_provider
    ON expenses (provider_id);

CREATE INDEX idx_providers_set
    ON providers (set_id);

```

## RESETEAR TABLAS 

```mysql
SET FOREIGN_KEY_CHECKS = 0;

TRUNCATE TABLE categories;
TRUNCATE TABLE expenses;                  
TRUNCATE TABLE providers;                
TRUNCATE TABLE refresh_tokens;          
TRUNCATE TABLE set_users;                
TRUNCATE TABLE sets;                    
TRUNCATE TABLE users;

SET FOREIGN_KEY_CHECKS = 1;
```