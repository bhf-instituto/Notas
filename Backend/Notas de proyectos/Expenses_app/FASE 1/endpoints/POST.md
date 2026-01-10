## POST /sets/:setId/expenses

### Auth
- JWT obligatorio
- `req.user.id` ya viene del middleware de auth

### Body esperado
```json
{
  "expense_type": 1,
  "category_id": 3,
  "provider_id": 5,
  "amount": 12000,
  "description": "Compra de insumos",
  "expense_date": "2026-01-08"
}
```
`provider_id` puede ser `null` o directamente no venir.

### Reglas de negocio

1) El usuario pertenece al set
2) La categoría existe y pertenece al set
3) El tipo de gasto coincide con la categoría
4) Si hay proveedor:
    - pertenece al set
    - el gasto es VARIABLE
5) Insertar gasto

### Queries SQL

#### Validar pertenencia al set
```mysql
SELECT 1
FROM set_users
WHERE set_id = ? AND user_id = ?
LIMIT 1;
```

#### Obtener categoría
```mysql
SELECT expense_type
FROM categories
WHERE id = ? AND set_id = ?
LIMIT 1;
```

#### Validar proveedor (solo si viene)
```mysql
SELECT 1
FROM providers
WHERE id = ? AND set_id = ?
LIMIT 1;
```

#### Insert gasto
```mysql
INSERT INTO expenses (
    set_id,
    user_id,
    category_id,
    provider_id,
    expense_type,
    amount,
    description,
    expense_date
)
VALUES (?, ?, ?, ?, ?, ?, ?, ?);
```