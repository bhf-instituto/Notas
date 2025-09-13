##### `async` **siempre devuelve una promesa**.
En **JavaScript**, toda función declarada con `async` **siempre devuelve una promesa**.
 
* Si dentro de la función devuelves un valor "normal" (string, número, objeto, etc.), automáticamente queda envuelto en una promesa resuelta.
- Si devuelves explícitamente una promesa, la función simplemente la devuelve tal cual.
- Si dentro ocurre un error o se hace `throw`, la promesa devuelta será rechazada.
###### Ejemplo:
```js
async function ejemplo() {
  return 42;
}
ejemplo().then(console.log); // 42
```
Aunque retornes un número, realmente lo que devuelve es una promesa que se resuelve con ese número.

Con error:
```js
async function ejemploError() {
  throw new Error("Ocurrió un problema");
}

ejemploError().catch(console.error); 
// Promise rechazada con Error: "Ocurrió un problema"

```

##### Uso de `try / catch
Siempre que pueda voy a intentar de separar los `try / catch` para poder ser mas especifico al momento de detectar el error, en otras palabras no debería envolver toda mi función en un `try / catch` . 

##### Las dependencias pueden tener sus propias dependencias
Hay que tener cuidado con eso, ya que se puede crear una cadena de dependencias muy grande. Y el problema no es solo el peso, si no que también las incompatibilidades. Puede haber dos dependencias que dependan de otra tercera pero en diferente versión de la misma.
![[GJyer-BWQAAxmzB.jpg]]

##### Un servidor solo puede hacer dos cosas
Recibir una petición o devolver una respuesta

##### Activar watcher en NodeJS
```powershell
node --watch ./script.js
```