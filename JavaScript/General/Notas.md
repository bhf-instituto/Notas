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
##### **Watcher con Nodemon**
 Es una herramienta para el desarrollo en NodeJS, sirve para reiniciar automáticamente la aplicación cada vez que se hagan cambios en los archivos del proyecto. Es un watcher de toda la vida. 
```
npm install -g nodemon

//Como dependencia de desarrollo
npm install --save-dev nodemon
```

Luego, en el [[package.json]] se crea el script para ejecutar el watcher.
```json
  "scripts": {
    "dev": "nodemon /src/index.js"
  }...
```

##### REST Client (extensión VSC)
La extensión **REST Client** para VS Code sirve para hacer **peticiones HTTP (REST, SOAP o GraphQL)** directamente desde tu editor, sin necesidad de usar herramientas externas como Postman o Insomnia.

###### 🔑 **¿Qué permite hacer?**
- Crear archivos `.http` o `.rest` en los que escribís tus solicitudes.
- Ejecutar esas solicitudes con un simple click o atajo (`Ctrl+Alt+R`).
- Ver la respuesta directamente en VSCode (status code, headers, body, etc.).
- Probar fácilmente tus endpoints de **APIs** durante el desarrollo.
- Manejar autenticación (Basic, Bearer Token, OAuth).
- Usar variables y entornos (por ejemplo, `{{baseUrl}}` para cambiar entre desarrollo y producción).
- Guardar historial de peticiones.

💡 **Ejemplo de uso** en un archivo `api.http`:
```http
### Obtener productos
GET https://fakestoreapi.com/products
Content-Type: application/json

###    ← separa las peticiones 

### Crear producto
POST https://fakestoreapi.com/products
Content-Type: application/json

{
  "title": "Mi producto",
  "price": 29.99,
  "description": "Un producto de prueba",
  "image": "https://example.com/img.png",
  "category": "electronics"
}
```
Luego solo ejecutás cada request y ves la respuesta al instante en una pestaña dentro de VSCode.