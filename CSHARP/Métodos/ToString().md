## 1️⃣ Qué es `ToString()`

- `ToString()` es un **método de la clase base `Object`**.
- Como **todas las clases en C# heredan de `Object`**, todas las clases tienen `ToString()` disponible, incluso si no lo escribiste explícitamente.
- Su propósito es **devolver una representación en forma de cadena (string) del objeto**.
```csharp 
object obj = new object();
Console.WriteLine(obj.ToString()); // "System.Object"
```
💡 Nota: Por defecto, `ToString()` devuelve el **nombre completo de la clase** si no se sobrescribe.

---
## 2️⃣ Sobrescribir `ToString()`

Normalmente, queremos que `ToString()` nos devuelva **información útil** del objeto, no solo el nombre de la clase. Para eso, **sobrescribimos** (usando [[override]]) el método en nuestra clase:
```csharp
class Persona
{
    public string Nombre { get; set; }
    public int Edad { get; set; }

    public override string ToString()
    {
        return $"Nombre: {Nombre}, Edad: {Edad}";
    }
}
Persona p = new Persona { Nombre = "Mariano", Edad = 30 };
Console.WriteLine(p);  // "Nombre: Mariano, Edad: 30"
```

En un [[Struct]]:
```csharp
// ejemplo tirada del D20
    public struct D20Result
    {
        public int Value;
        public int Mod;
        public int Result;
        public bool IsCritHit;
        public bool IsCritFail;

        public override string ToString()
            =>
            "Roll: " + Value + "\n" +
            "Mod: " + Mod + "\n" +
            "Result: " + Result + "\n" +
            "Is CritHit ? : " + IsCritHit + "\n" +
            "Is CritFail ? : " + IsCritFail + "\n";
    }
```



Básicamente lo que pasa es que los Struct heredan de la clase Object, dentro de las cosas que hereda está el método `ToString()` el cual devuelve el nombre del tipo. Por ejemplo, si yo creo un Struct (o una clase) al cual le asigno MyStruct como nombre de tipo, y llamo al método `ToString()` este me devolverá "MyStruct". Yo no quiero eso,  lo que quiero es que el método `ToString()` devuelva un string compuesto por las key y values de ese Struct, para eso tengo que hacer un `override`.

Está bueno para hacer **debuggin** rápido.  