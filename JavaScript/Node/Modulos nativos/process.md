El objeto `process` es un **objeto global** que proporciona información y control sobre el proceso **actual** de ejecución.

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