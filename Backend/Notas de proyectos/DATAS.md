
Se puede validar datos desde MYSQL así:
```mysql

## en este caso valido que la cantidad sea mayor a 0
CREATE TABLE expenses (
    id_expense INT AUTO_INCREMENT PRIMARY KEY,
    amount INT NOT NULL,
    CHECK (amount >= 0)
);

ALTER TABLE expenses
ADD CONSTRAINT chk_amount_non_negative
CHECK (amount >= 0);
```


Declarando un ENUM: 
```mysql
CREATE TABLE expenses (
    id_expense INT AUTO_INCREMENT PRIMARY KEY,
    amount DECIMAL(10,2) UNSIGNED NOT NULL,
    type ENUM('FIJO', 'VARIABLE') NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```