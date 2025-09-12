Es una función ejecutable que se usa como argumento de otra función «B». De esta forma, al llamar a «B», esta ejecutará «A». Esta acción puede ser inmediata, lo que se denominará **callback sincrónico** o puede producirse en un punto posterior, lo que se denominaría **callback asincrónico**. 

En otras palabras, una función de callback es una función que se pasa a otra función como un argumento, que luego se invoca dentro de la función externa para completar algún tipo de rutina o acción.

Ejemplo:
``` js
function saludar(nombre) {
  alert("Hola " + nombre);
}

function procesarEntradaUsuario(callback) {
  var nombre = prompt("Por favor ingresa tu nombre.");
  callback(nombre);
}

procesarEntradaUsuario(saludar);
```

El ejemplo anterior es una callback sincrónica, ya que se ejecuta inmediatamente.

Sin embargo, tenga en cuenta que las callbacks a menudo se utilizan para continuar con la ejecución del código después de que se haya completado una operación a sincrónica — estas se denominan devoluciones de llamada asincrónicas.

```js 
function delayedMessage(mensaje, ms, callback){
    setTimeout(()=> {
        console.log(mensaje);
        callback()
    }, ms)
}

delayedMessage("Hola después de 2 segundos", 2000, () => {
    console.log("Mensaje mostrado");
});

// Espera 2 segundos
// Muestra el mensaje
// Ejecuta callback
```