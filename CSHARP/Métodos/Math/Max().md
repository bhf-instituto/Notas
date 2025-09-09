```csharp
return Math.Max(total, 1);
```

- `Math.Max(a, b)` es una función de C# que **devuelve el valor mayor entre `a` y `b`**.
- En tu caso, estás comparando `total` con `1`.
Por lo tanto:

- Si `total` es mayor que `1`, la función devolverá `total`.
- Si `total` es menor que `1` (por ejemplo 0 o negativo), la función devolverá `1`.

Esta línea asegura que el valor retornado **nunca será menor que 1**.