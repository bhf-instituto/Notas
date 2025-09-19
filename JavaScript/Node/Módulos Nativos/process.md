`process` es un **objeto global** en Node.js que representa el **proceso actual de Node**. Desde él puedes acceder a información como:
- Argumentos pasados a Node (`process.argv`)
- Estado de salida (`process.exitCode`)
- Variables de entorno (`process.env`)

No necesitas `require()` para usarlo, está disponible automáticamente.
### *process.argv[]*
Nos devuelve un array con los **argumentos** que se le pasaron al proceso. 
```cmd
//cmd
node miScript.js arg1 arg2 arg3
```

```js
console.log(process.argv);

//devuelve: 
[
  'C:\\Program Files\\nodejs\\node.exe', // [0] → Ruta del ejecutable de Node
  'D:\\Proyectos\\miScript.js',  // [1] → Ruta del archivo que estás ejecutando
  'arg1',                                // [2] → Primer argumento que vos pasaste
  'arg2',                                // [3] → Segundo argumento
  'arg3'                                 // [4] → Tercer argumento
]
```

Esto se puede utilizar para por ejemplo al momento de ejecutar un script pasar un path como parámetro del proceso, y ese parámetro utilizarlo con el método `readdir`.

```js
// obtengo el path del argumento pasado por consola al proceso
// si no se le pasa el path usa el actual en su lugar "." 
const folder = process.argv[2] || ".";

// utilizo ese dato como argumento de fs.readdir()
fs.readdir(folder)
    .then(files => {
        files.forEach(file => {
            console.log("- ", file);
        })
    })
    .catch(err => {
        if(err){
            console.error("Error al leer el directorio: ", err);
            return;
        }
    })
```


### `process.exit()`
cuando usás `process.exit(código)`, lo que hacés es **finalizar inmediatamente la ejecución del proceso de Node** y devolver un _código de salida_ al sistema operativo.

🔹 El número que pasás (`código`) sirve para **indicar el motivo de la salida**:

- `process.exit(0)`  
    👉 Termina el proceso indicando que **todo salió bien**.  
    (Por convención, `0` significa "éxito" o "sin errores").
- `process.exit(1)`  
    👉 Termina el proceso indicando que ocurrió **un error**.  
    (Cualquier número distinto de `0` se interpreta como fallo, pero el `1` es el más común para "error genérico").

```js
if (someCondition) {
  console.log("Todo bien 👍");
  process.exit(0); // Éxito
} else {
  console.error("Algo salió mal ❌");
  process.exit(1); // Error
}
```
#### 🚨 Importante

- `process.exit()` **corta el programa inmediatamente**, no espera a que terminen operaciones pendientes (como promesas, timers o conexiones).
- Si querés que el programa termine "limpiamente", asegurate de cerrar archivos, conexiones, etc., antes de llamar a `process.exit()`.

## `process.env`

`process.env` es un **objeto plano** (tipo `{ [key: string]: string }`) que contiene todas las variables de entorno del sistema.
```
{
  PATH: 'C:\\Windows\\System32;C:\\Program Files\\nodejs\\',
  USERNAME: 'Juan',
  PORT: '1234',
  NODE_ENV: 'development',
  ...
}
```

Ejemplo:
```js
const port = process.env.PORT || 3000;
console.log(`El servidor va a correr en el puerto ${port}`);
```

**Ejecutamos en Windows PowerShell:**
```powershell
$env:PORT="1234"; node index.js
```