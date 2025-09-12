## 1️⃣ Qué es una estructura de datos “hash”

Una **estructura de datos hash** es una forma de **guardar y buscar información rápidamente** usando un número llamado **hash**.

- Un **hash** es un valor calculado a partir del objeto (por ejemplo, un número, una cadena o un objeto completo).
- Ese hash indica **dónde se debe guardar el objeto en la memoria** (o en la estructura).
- Gracias a esto, podemos **encontrar el objeto en tiempo casi constante**, sin recorrer todos los elementos.

### 2️⃣ Ejemplo sencillo

Imaginá que querés guardar nombres y sus edades:
```csharp
Dictionary<string, int> edades = new Dictionary<string, int>();
edades["Mirko"] = 30;
edades["Ana"] = 25;
```
- Cuando hacés `edades["Mirko"]`, el diccionario no recorre toda la lista.
- Calcula un **hash del nombre "Mariano"** y usa ese número para ir **directamente a la “casilla” correcta** donde está guardada la edad.

### 3️⃣ Ventajas de las estructuras hash

- **Búsqueda rápida:** Accedés casi inmediatamente usando la clave.
- **Inserción rápida:** Guardar nuevos elementos es muy eficiente.
- Ejemplos: [[Dictionary]], [[HashSet]] en C#, tablas hash en otros lenguajes.

---
### 4️⃣ Desventaja

- Puede haber **colisiones**: dos objetos diferentes pueden generar el mismo hash.
- La estructura debe manejar esas colisiones, normalmente con listas internas o encadenamiento.

---

💡 **Analogía:**  
Imaginá un conjunto de casilleros con números. Cada nombre tiene un “número secreto” (hash) que te dice **exactamente en qué casillero ponerlo o buscarlo**. Eso evita tener que abrir todos los casilleros uno por uno.