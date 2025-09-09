## Función genérica

```csharp
public static void PrintStruct<T>(T obj) where T : struct
{
    var type = typeof(T);
    // Campos
    var fields = type.GetFields();
    foreach (var field in fields)
    {
        var value = field.GetValue(obj);
        Console.WriteLine($"{field.Name}: {value}");
    }
    // Propiedades
    var props = type.GetProperties();
    foreach (var prop in props)
    {
        var value = prop.GetValue(obj);
        Console.WriteLine($"{prop.Name}: {value}");
    }
}
```



```csharp
public static void PrintStruct<T>(T obj) where T : struct
```

- **public**: lo podés llamar desde cualquier parte.
- **static**: no necesitás instanciar la clase que lo contiene; llamás `TuClase.PrintStruct(...)`.
- ** <T\> **: es un **genérico**; `T` es un tipo que se decide cuando llamás al método (por ej. `D20Result`).
- **where T : struct**: **restricción** que obliga a que `T` sea un **value type** (struct, `int`, etc.). Así evitás pasar clases. Si quisieras aceptar _todo_, sacá la restricción.

**Dentro del método**
```csharp
var type = typeof(T);
```
- **typeof(T)** te da un objeto **Type** con la _metainformación_ del tipo (`T`). Es lo que usa la **reflection** para inspeccionar miembros. Alternativa equivalente y más flexible:
```csharp
var type = obj.GetType(); // sirve igual y también si sacás la restricción
```

```csharp
var fields = type.GetFields();
```
- **GetFields()** devuelve los **fields públicos** del tipo (instancia y, potencialmente, estáticos). Cada elemento es un **FieldInfo** (reflection).
```csharp
foreach (var field in fields)
{
    var value = field.GetValue(obj);
    Console.WriteLine($"{field.Name}: {value}");
}
```
- **field.GetValue(obj)** lee el valor del field en _esa instancia_ (`obj`).
- Como `obj` es un **struct** (value type), acá ocurre **boxing**: el struct se empaqueta como `object` para que reflection pueda leerlo. Es automático y normal en estos escenarios.
- **$"{...}"** es _interpolación de strings_.
```csharp 
var props = type.GetProperties();
foreach (var prop in props)
{
    var value = prop.GetValue(obj);
    Console.WriteLine($"{prop.Name}: {value}");
}
```
- **GetProperties()** devuelve las **propiedades públicas** (cada una es un **PropertyInfo**).
- **prop.GetValue(obj)** llama al _getter_ de la propiedad (si lo tiene y es público).
- Ojo que pueden existir **indexers** (propiedades con parámetros). Conviene filtrarlos.

