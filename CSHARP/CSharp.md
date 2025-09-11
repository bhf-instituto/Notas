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

Osea que este if no solo chequea una condición sino que hace una conversión en la forma en la que se trata al objeto. 

1. **`item is Weapon`**  
    → Verifica en tiempo de ejecución si el objeto al que apunta la variable `item` **es realmente de tipo `Weapon`** (o derivado).
    - Si `item` fue creado como `new Weapon(...)`, esto da `true`.
    - Si `item` es otro tipo (ej: `Armor`, `Ring`, etc.), da `false`.
    
2. **`weapon` (después del `is`)**  
    → Si la comprobación es `true`, C# crea una **nueva variable local `weapon`** (del tipo `Weapon`) que **apunta al mismo objeto en memoria que `item`**.
    - No se hace una copia.
    - No se transforma el objeto.
    - Simplemente tenés una variable con un tipo más específico, lo que te habilita a acceder a propiedades como `WeaponStat`.


**HashSet** es un tipo de lista que no permite elementos duplicados. 
```csharp
 public HashSet<tipo> nombreLista { get; set; } = new HashSet<tipo>();
```

El método **`UnionWith`** agrega a tu `HashSet` todos los elementos de otra colección, pero **sin permitir duplicados**. *En otras palabras, hace la unión de conjuntos matemáticos. 
```csharp
var set = new HashSet<int> { 1, 2, 3 };

set.UnionWith(new[] { 3, 4, 5 });

foreach (var n in set)
    Console.WriteLine(n);

// Resultado :     
1
2
3
4
5
```


### Definir propiedad de solo lectura
```csharp
public int HitDieFaces { get; set; }
public int Level => HitDiceCount;
```
`Level` no guarda un valor propio, sino que **refleja siempre** el valor actual de `HitDiceCount`.