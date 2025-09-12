Una **Promise** en JavaScript es un objeto que representa el resultado de una operación asíncrona, la cual puede estar:
- **Pendiente (`pending`)** → cuando la operación todavía no terminó.
- **Resuelta (`fulfilled`)** → cuando la operación terminó bien y devuelve un valor.
- **Rechazada (`rejected`)** → cuando ocurre un error y devuelve una razón del fallo.

Sirven para manejar tareas que toman tiempo (como pedir datos a una API o leer un archivo) sin bloquear el resto del programa.

Se usan con los métodos:
- `.then()` → para manejar el resultado exitoso.
- `.catch()` → para manejar errores.
- `.finally()` → para ejecutar algo siempre, sin importar el resultado.

👉 En resumen: una Promise es como un "boleto" que promete darte un valor más adelante, cuando la operación termine.

```js
function sumaAsincronica(num_a, num_b){
	return new Promise((resolve, reject)=> {
		// chequeamos que el tipo de dato sea correcto
		if(typeof num_a != "number" || typeof num_b != "number"){
			reject(new Error("Error de tipo de dato"));
			return;
		} 
		setTimeout(()=> {
			resolve({
				num_a : num_a,
				num_b : num_b,
				result : num_a + num_b
			})
		},1000)
	})
}

sumaAsincronica(1,5)
	.then(resolve => console.log(resolve))
	.catch(err => console.error(err))
	// devuelve 6 leugo de 1 segundo.
sumaAsincronica(1,"5")
	.then(resolve => console.log(resolve))
	.catch(err => console.error(err))
	// devuelve inmediatamente el mensaje de error. 

// Si ejecuto ambas juntas primero devuelve el error de la segunda, y un segundo después devuelve 6.
```
En JavaScript, el **estado interno** de una Promise (`pending → fulfilled / rejected`) **no es accesible directamente**. Eso lo maneja el motor de JS y no hay una propiedad pública tipo `promise.state`. 
Pero si se puede simular poniendo logs del estado en los puntos clave, por ejemplo: 
```js
function sumaAsincronica(num_a, num_b) {
  console.log("→ Estado: pending"); // apenas se crea la promesa
  return new Promise((resolve, reject) => {
    if (typeof num_a != "number" || typeof num_b != "number") {
      console.log("→ Estado: rejected"); // justo antes de que se rechace
      reject(new Error("Error de tipo de dato"));
      return;
    }

    setTimeout(() => {
      console.log("→ Estado: fulfilled"); // justo antes de que se resuelva
      resolve({...
```

### Async/Await
Es *azúcar sintáctica* para las promesas, quita el uso de sintaxis como .then(), catch(), resolve y reject para que el código sea mas legible haciéndolo parecer sincrónico (aunque sea asincrónico no bloqueante)

```js
const promise = new Promise((resolve, reject) => {
    setTimeout(()=> {
        resolve("Succes");
        // reject(new Error("Failure"));
    }, 1000)
})

async function main(){
    try{
        console.log("intentado obtener la promesa resuelta")
        const resolve = await promise;
        console.log(resolve)
    } catch(err){
        console.error(err)
    }
}
main();
```