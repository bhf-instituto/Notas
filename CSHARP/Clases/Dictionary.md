## **Dictionary<TKey, TValue>**

Un **`Dictionary`** es una colección de pares **clave-valor**, donde cada clave es **única** y se usa para acceder rápidamente a su valor asociado.
### a) Cómo funciona internamente
1. Cuando agregás un elemento, el diccionario **calcula el hash** de la clave.
2. Ese hash indica **dónde almacenar el valor en memoria** (una “casilla” interna en un array).
3. Si hay otra clave con el mismo hash (colisión), el diccionario maneja la colisión internamente, normalmente usando **listas enlazadas** o **re-hashing**.
4. Cuando querés obtener el valor por clave, el diccionario:
    - Calcula el hash de la clave de búsqueda.
    - Va directo a la “casilla” correspondiente.
    - Busca la clave exacta (en caso de colisión) y devuelve el valor.

💡 Esto significa que **buscar, insertar o eliminar por clave es muy rápido**, generalmente en **O(1)** tiempo promedio, aunque en el peor caso puede ser O(n) si hay muchas colisiones.

---

### b) Características clave

- Claves únicas: no podés tener dos elementos con la misma clave.
- Valores no necesitan ser únicos.
- Tipos genéricos: `TKey` y `TValue` pueden ser cualquier tipo.
- Basado en hashing: depende de que `GetHashCode()` y `Equals()` estén bien implementados en el tipo de la clave.

---

### c) Ejemplo  
```csharp
Dictionary<string, int> edades = new Dictionary<string, int>();

// Agregar elementos
edades.Add("Mariano", 30);
edades["Ana"] = 25;  // Alternativa con indexador

// Acceder por clave
Console.WriteLine(edades["Mariano"]); // 30

// Verificar si existe una clave
if (edades.ContainsKey("Ana")) 
{
    Console.WriteLine("Ana está en el diccionario");
}

// Recorrer todos los pares
foreach (var kvp in edades)
{
    Console.WriteLine($"Clave: {kvp.Key}, Valor: {kvp.Value}");
}

```

🔹 Observación: si intentás agregar la misma clave dos veces, lanza una **excepción**.

## Manejar las excepciones al agregar
### 1️⃣ Verificar antes
La forma más “segura” es comprobar si la clave existe antes de agregarla:
```csharp
Dictionary<string, int> edades = new Dictionary<string, int>();

if (!edades.ContainsKey("Mirko"))
{
    edades.Add("Mirko", 30);
}
else
{
    Console.WriteLine("La clave 'Mirko' ya existe");
}
```
👉 Bueno cuando no querés sobrescribir valores existentes.

### 2️⃣ Sobrescribir valores usando el indexador
Si usás el **indexador** (`diccionario[key] = value`), entonces:
- Si la clave **no existe**, la agrega.
- Si la clave **ya existe**, **sobrescribe el valor anterior**.
```csharp
edades["Ana"] = 25; // Si existe, se actualiza; si no, se agrega
```
👉 Útil cuando querés que **la última asignación sea la válida**.

### 3️⃣Usar `TryAdd` (C# 7.0+)
- Devuelve `true` si pudo agregar.
- Devuelve `false` si la clave ya existía (y no lanza excepción).
```csharp 
if (edades.TryAdd("Juan", 40))
{
    Console.WriteLine("Juan agregado");
}
else
{
    Console.WriteLine("Juan ya existía");
}
```
👉 Ideal para evitar excepciones y escribir código más limpio.

### 4️⃣ Actualizar con `TryGetValue`
Si querés **actualizar si existe** o **agregar si no existe**, podés hacer:
```csharp
if (edades.TryGetValue("Ana", out int edad))
{
    edades["Ana"] = edad + 1; // Actualizo edad existente
}
else
{
    edades.Add("Ana", 25); // La agrego
}
```
👉 Bueno para escenarios de acumulación, contadores, etc.

### 5️⃣ Usar `GetOrAdd` con `ConcurrentDictionary`
En entornos multihilo (threads), se suele usar `ConcurrentDictionary`, que tiene un método muy práctico:
```csharp
using System.Collections.Concurrent;
var edadesConcurrentes = new ConcurrentDictionary<string, int>();
int edad = edadesConcurrentes.GetOrAdd("Mariano", 30);
```
👉 Si `"Mariano"` ya estaba, devuelve su valor actual.  
👉 Si no estaba, lo agrega con 30 y devuelve 30.

### ✅ **Resumen práctico:**

- `Add` → lanza excepción si ya existe (se usa cuando **debe ser nuevo sí o sí**).
- `ContainsKey + Add` → cuando querés validar antes de agregar.
- `diccionario[key] = value` → cuando querés **insertar o reemplazar**.
- `TryAdd` → cuando querés **agregar solo si no existe**, sin excepciones.
- `TryGetValue` → cuando querés leer y actualizar.
- `GetOrAdd` (ConcurrentDictionary) → en entornos multihilo.
