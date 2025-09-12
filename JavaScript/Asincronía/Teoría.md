Tenemos 4 formas de ver el orden de los proceso: 

## Secuencial Síncrono
Típico, una cosa va después de la otra y el hilo de ejecución se mantiene ocupado hasta que termina toda la ejecución, ejemplo: 

```js
const fs  = require('node:fs');
// import fs from 'node:fs'
const text_a = fs.readFileSync("/path1", "charset")
const text_b = fs.readFileSync("/path2/", "charset")
console.log(text_a);
console.log(text_b);
```

![[Pasted image 20250912000251.png]]

___
## Asíncrono con callback
* Forma antigua de manejar la asincronía, no se si está deprecada pero casi.
* Tiene problemas técnicos ya que si uno quiere encadenar  procesos asíncronos se genera lo que se llama *"callback hell"*
```js
const fs = require("node:fs");

fs.readFile("path1", "charset", (err, text) => {
	console.log("Primer texto leido", text);
})

fs.readFile("path2", "charset", (err, text) => {
	console.log("Segundo texto leido", text);
})
```

Lo mismo pero con Promises.
```js
const fs = require('node:fs/promises');

console.log("Leyendo primer archivo...\n ");

fs.readFile("./os-info.js", "utf-8")
	.then( text => console.log("→ Primer archivo leido: \n", text))
console.log("Haciendo cosas raras mientras lee el primer archivo...\n")
console.log("Leyendo segundo archivo...\n ");

fs.readFile("./fs-stats.js", "utf-8")
	.then((text)=> {
console.log("→ Segundo archivo leido");
console.log(text)
})
console.log("Haciendo cosas raras para gente normal mientras leo archivo...\n")
```

![[Pasted image 20250912000836.png]]
*En vez de callbacks también pueden ser las promesas resueltas*
### Ejemplo de *callback hell*
```js 
loginUser(username, password, (err, user) => {
  if (err) return console.error(err);
  
  getUserProfile(user.id, (err, profile) => {
    if (err) return console.error(err);
    
    getUserPosts(user.id, (err, posts) => {
      if (err) return console.error(err);
      
      console.log("Perfil y posts:", profile, posts);
    });
  });
});
```

---
## Asíncrono secuencial
El o los procesos se llevan a cabo de manera asíncrona pero secuencial, es decir que si por ejemplo queremos leer dos ficheros:
- → leeremos el primero → se libera el hilo principal de ejecución → se resuelve la promesa (o el callback) → se libera el hilo → leemos el segundo fichero → se libera el hilo → se resuelve la segunda promesas ( o callback). 
Si bien es un proceso de lectura asincrónico, debido a su naturaleza secuencial actúa casi como si fuesen procesos sincrónicos.
- Es interesante esta forma de ejecutar procesos asíncronos ya que a veces  este proceso depende del siguiente.
```js
const { readFile } = require('node:fs/promises');

async function init() {
    console.log("Leyendo el primer archivo: ");
    const text_a = await readFile("./text.txt", "utf-8");
    console.log(text_a, "\n");
    console.log("Haciendo cosas...\n");

    console.log("Leyendo el segundo archivo: ");
    const text_b = await readFile("./text2.txt", "utf-8");
    console.log(text_b, "\n");
}
init();
```

![[Pasted image 20250912012408.png]]
## Asíncrono en paralelo
Es cuando llamamos a varios procesos a la vez y esperamos que terminen todos para dar una única resolución.
```js
// Esto está dentro de un archivo .mjs que si admite modulos. 

import { readFile } from "fs/promises";

Promise.all([
    readFile("./os-info.js", "utf-8"),
    readFile("./fs-stats.js", "utf-8")
]).then(([firstText, secondText]) => {
    console.log(firstText);
    console.log(secondText);
})
```

![[Pasted image 20250912112512.png]]
___
