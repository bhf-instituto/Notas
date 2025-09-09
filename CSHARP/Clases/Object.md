La clase `object` en C# es **la raíz de la jerarquía de tipos**, es decir, **todas las clases derivan de ella**, y por lo tanto todos los objetos comparten ciertos comportamientos comunes que `object` define.

---

## 1️⃣ Qué es `object`

- Está definida en el **namespace `System`**.
- Todos los tipos, ya sean clases (`class`) o estructuras (`struct`), derivan directa o indirectamente de `object`.
- Proporciona métodos básicos que **todos los objetos pueden usar**.

---

## 2️⃣ Métodos más importantes de `object`

| Método               | Qué hace                                                                                                                               |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| [[ToString()]]       | Devuelve una representación en cadena del objeto. Por defecto devuelve el nombre de la clase, pero se puede sobrescribir (`override`). |
| `Equals(object obj)` | Compara si el objeto actual es igual a otro. Por defecto compara referencias, pero se puede sobrescribir para comparar valores.        |
| `GetHashCode()`      | Devuelve un número entero que representa el hash del objeto, útil para colecciones como `Dictionary`.                                  |
| `GetType()`          | Devuelve un objeto `Type` que describe la clase del objeto actual.                                                                     |
| `MemberwiseClone()`  | Crea una copia superficial del objeto (shallow copy). Es `protected`, solo accesible desde la clase o derivadas.                       |


