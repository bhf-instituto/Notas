**Switch Simplificado**
```csharp
switch simplificado: 
string userInput = Console.ReadLine() switch
{
    "1" => "Opc 1",
    "2" => "Opc 2",
    _ => "error"
};
```

La clase principal es Item, dentro hay una clase hija que hereda todo lo de item llamada Weapon ( Weapon : Item). Entonces yo necesito crear una condición para distinguir a los tipo de items por su propia subclase (Weapon, Armor, Helmet, etc).

```csharp
if (item is Weapon weapon)
{
    SetMainStat(weapon);
}
```

1. **`item is Weapon`**  
    → Verifica en tiempo de ejecución si el objeto al que apunta la variable `item` **es realmente de tipo `Weapon`** (o derivado).
    - Si `item` fue creado como `new Weapon(...)`, esto da `true`.
    - Si `item` es otro tipo (ej: `Armor`, `Ring`, etc.), da `false`.
    
2. **`weapon` (después del `is`)**  
    → Si la comprobación es `true`, C# crea una **nueva variable local `weapon`** (del tipo `Weapon`) que **apunta al mismo objeto en memoria que `item`**.
    - No se hace una copia.
    - No se transforma el objeto.
    - Simplemente tenés una variable con un tipo más específico, lo que te habilita a acceder a propiedades como `WeaponStat`.



