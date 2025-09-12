- Es un entorno de ejecución de JavaScript, sirve para ejecutar JS fuera del navegador. 
- Es multiplataforma.
- Es asíncrono con In/Out (Entrada/Salida de datos).
- Usa como motor el V8 de Chrome.

Está orientado a eventos, esto quiere decir que es un modelo de programación donde tiene un bucle donde va manejando las solicitudes que le van llegando cada vez que tiene un evento. En lugar de esperar bloqueos, lo que hace es ir ejecutando tareas mientras espera respuestas de otras tareas que fue dejando atrás, y todo esto en un solo 2, ya que [[Node]] es un entorno mono-hilo. Pero tiene un sistema que libera ese proceso para poder hacer cosas en asíncrono. 
Node se aprovecha de la asincronía y su sistema de eventos, para apartar procesos, resolver otros y luego retomar los apartados. Simula ser multi hilo pero no lo es. 

si nosotros ejecutamos : 
```powershell
PS C:\node 
```
Abrimos el [[REPL]] de NodeJS. 
___

### Variable global 
- En Node la variable global es **global** y en el navegador es **window**.
- La forma correcta de llamar a la variable global en todos los entornos es con **globalThis**.
- Uno puede acceder a ella desde cualquier punto de la ejecución.
- Contiene muchas propiedades, las cuales son muy utilizadas como **console**, **Promise**, module, etc. 
```javascript
console.log(globalThis)
```