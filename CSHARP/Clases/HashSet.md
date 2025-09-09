## **HashSet<T\>**

Un **`HashSet`** es una **colección de elementos únicos** sin un valor asociado, también basada en hashing.
### a) Cómo funciona internamente
1. Cuando agregás un elemento, se calcula su **hash**.
2. Ese hash determina dónde se almacena el elemento.
3. Si ya existe un elemento con el mismo valor (según `Equals()`), no se agrega.
4. Las operaciones de **agregar, eliminar y buscar** son muy rápidas, en promedio O(1).

💡 El `HashSet` es ideal cuando **solo te importa la existencia de elementos** y no un valor asociado.

---

### b) Características clave

- Elementos únicos: no puede haber duplicados.
- Basado en hashing, por lo que la rapidez depende de `GetHashCode()` bien implementado.
- Permite operaciones de **conjuntos matemáticos**:
    - `UnionWith()` → unión de conjuntos.
    - `IntersectWith()` → intersección.
    - `ExceptWith()` → diferencia.

---
### c) Ejemplo 

```csharp
HashSet<string> frutas = new HashSet<string>();

// Agregar elementos
frutas.Add("Manzana");
frutas.Add("Banana");
frutas.Add("Manzana"); // No se agrega, ya existe

// Verificar existencia
if (frutas.Contains("Banana"))
{
    Console.WriteLine("Tenemos banana");
}

// Recorrer elementos
foreach (var fruta in frutas)
{
    Console.WriteLine(fruta);
}

// Operaciones de conjunto
HashSet<string> tropicales = new HashSet<string> { "Banana", "Piña" };
frutas.UnionWith(tropicales); // frutas ahora contiene Manzana, Banana, Piña

```