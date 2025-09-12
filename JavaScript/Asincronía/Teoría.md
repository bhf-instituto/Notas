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
## Asíncrono con callback
* Forma antigua de manejar la asincronía, no se si está deprecada pero casi.
* Tiene problemas técnicos ya que si uno quiere encadenar  procesos asíncronos se genera lo que se llama *"callback hell"*
```js
const fs = require("node:fs");

fs.readFile("path1", "charset", (err, text) => {
	console.log("Primer texto leido", text);
})

const fs = require("node:fs");

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


## Asíncrono secuencial

![[Pasted image 20250912012408.png]]
Ejemplo con **async / await**
