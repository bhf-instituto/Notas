## 🔹 `filter()`

👉 **Qué hace:**  
Recorre un array y devuelve un **nuevo array** con **solo los elementos que cumplen una condición**.

👉 **Cómo funciona:**
- Recibe una función callback que debe devolver `true` o `false`.
- Si devuelve `true`, el elemento queda en el array resultante.
- Si devuelve `false`, se descarta.

👉 **Ejemplo:**
```js
const numeros = [1, 2, 3, 4, 5, 6];
// quiero solo los pares
const pares = numeros.filter(n => n % 2 === 0);
console.log(pares); // [2, 4, 6]
```

## 🔹 `map()`

👉 **Qué hace:**  
Recorre un array y devuelve un **nuevo array transformado**, aplicando una operación a cada elemento.

👉 **Cómo funciona:**
- Recibe una función callback.
- Esa función debe devolver el **nuevo valor** para cada elemento.

👉 **Ejemplo:**

```js
const numeros = [1, 2, 3, 4];
// quiero duplicar todos los números
const dobles = numeros.map(n => n * 2);
console.log(dobles); // [2, 4, 6, 8]
```


```js

```


```js

```
## 🔄 `filter()` + `.map()`

Muy común en la vida real:
```js
const cheapIds = products
  .filter(p => p.price < 16) // primero filtro
  .map(p => p.id);           // luego transformo en ids

console.log(cheapIds);
```

## 🔹 `reduce()`

👉 **Qué hace:**  
Recorre un array y lo **reduce a un único valor** (puede ser un número, un string, un objeto, otro array...).

👉 **Cómo funciona:**
- Recibe un **callback** y un **valor inicial**.
- El callback tiene esta forma:

```js
array.reduce((acumulador, elementoActual) => {
  // lógica
  return nuevoAcumulador;
}, valorInicial);
```
- **acumulador**: es el resultado que se va “acumulando” en cada iteración.
- **elementoActual**: es el elemento que está recorriendo en ese momento.
- **valorInicial**: con qué arranca el acumulador.
### 🛠 Ejemplos sencillos
#### 1) Sumar números
```js
const numeros = [1, 2, 3, 4, 5];
const suma = numeros.reduce((acc, n) => acc + n, 0);
console.log(suma); // 15
```
#### 2) Encontrar el valor máximo
```js
const numeros = [10, 50, 20, 90, 30];
const max = numeros.reduce((acc, n) => Math.max(acc, n), -Infinity);
console.log(max); // 90
```

#### 3) Calcular precio promedio
```js
const avgPrice = products.reduce((acc, p) => acc + p.price, 0) / products.length;
console.log("Precio promedio:", avgPrice);
```


## ⚡ Diferencia resumida

- `map`: transforma **cada elemento** → devuelve array del mismo tamaño.
- `filter`: selecciona **algunos elementos** → devuelve array con menos o igual tamaño.
- `reduce`: transforma **todo el array en un único valor** (puede ser número, objeto, string, lo que vos quieras).