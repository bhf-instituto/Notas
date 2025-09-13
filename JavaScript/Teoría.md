### 🔑 **Características de Node.js**

1. **Basado en JavaScript**
    - Permite usar **JavaScript en el servidor**, no solo en el navegador.
    - Esto facilita que un desarrollador web pueda usar un único lenguaje tanto en frontend como en backend.

2. **Asíncrono y no bloqueante (Non-blocking I/O)**
    - Node.js usa un modelo de ejecución **event-driven** (basado en eventos).
    - Cuando se hace una operación de entrada/salida (como leer un archivo o consultar una base de datos), Node no se queda esperando: continúa ejecutando otras tareas y usa callbacks o promesas para manejar la respuesta cuando esté lista.
    - Esto lo hace muy eficiente en aplicaciones que requieren muchas conexiones concurrentes.

3. **Single-threaded con Event Loop**
    - Node corre en un **solo hilo** (thread), pero gracias al **event loop** puede manejar miles de conexiones concurrentes.
    - En lugar de usar múltiples hilos como en otros lenguajes, Node delega tareas pesadas al sistema operativo y luego procesa el resultado cuando está listo.

4. **Alta escalabilidad**
    - Ideal para aplicaciones en **tiempo real** (chat, streaming, APIs).
    - Gracias a su arquitectura asíncrona, puede escalar horizontalmente de forma sencilla.

5. **Motor V8 de Google**
    - Node.js está construido sobre el **motor V8 de Chrome**, que compila JavaScript a código máquina.
    - Esto le da gran **velocidad de ejecución**.

6. **NPM (Node Package Manager)**
    - Node incluye **npm**, el gestor de paquetes más grande del mundo.
    - Permite instalar fácilmente librerías y frameworks creados por la comunidad.

7. **Multiplataforma**
    - Funciona en Windows, Linux y macOS.
    - Muy usado en servidores cloud y entornos de microservicios.

8. **Streaming de datos**
    - Node puede procesar archivos y datos como flujos (streams) en lugar de cargarlos completos en memoria, lo que ahorra recursos y acelera procesos (ej. subir un video a la nube mientras se procesa).

9. **JSON nativo**
    - Como usa JavaScript, la interacción con **JSON** (formato clave en APIs y bases de datos NoSQL como MongoDB) es directa y eficiente.


---

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
