
```javascript

```

## Extensiones e importación/exportación
#### .js → por defecto utiliza CommonJS

En este caso, ambos archivos están en la misma carpeta.
```javascript
// sum.js
function sum (a,b) {
return a+b;
}
module.exports = sum;

// index.js
const sum = require("./sum"); →→ No hace falta la extensión 
console.log(sum(1,2));
```
#### .mjs → para utilizar ES Modules (Ecma Script Modules)
```js
// operaciones.mjs
export function sum (a,b) {
return a+b;
}
export function sub (a,b) {
return a+b;
}

// index.mjs
const { sum, sub } from './operaciones.msj'; →→ Hace falta la extensión 
console.log(sum(2,1));
console.log(sub(2,1));

```
