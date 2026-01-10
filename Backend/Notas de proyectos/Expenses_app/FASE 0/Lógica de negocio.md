
## Gasto: 

- Todo gasto:
    - pertenece a **un grupo**
    - pertenece a **un usuario**
    - tiene **una categoría**
    
- Solo los gastos **variables**:
    - pueden tener **0 o 1 proveedor**
    
- Los gastos fijos:
    - **no pueden** tener proveedor

Esto es limpio, entendible y fácil de validar.


Aunque **todos los gastos tienen tipo**, eso no implica que:

- el tipo de gasto tenga que ser una tabla compleja
- ni que las categorías “dependan” del tipo

La relación correcta es:

> **El gasto conoce su tipo**  
> **La categoría es válida para ese tipo**


Cuando alguien lea tu diseño, debería entender esto:

> “Un gasto es un evento económico individual, realizado por una persona, dentro de un grupo, clasificado por una categoría, tipificado como fijo o variable, y eventualmente asociado a un proveedor.”