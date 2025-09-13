### 1) Leer directorio y obtener información
```js
// Importamos el módulo de fs (File System) en su versión con promesas
// Esto nos permite trabajar con archivos y directorios de manera asíncrona usando async/await
const fs = require('node:fs/promises');

// Importamos el módulo path para construir rutas de archivos y directorios
// Sirve para unir carpetas y archivos sin preocuparnos por el sistema operativo (Windows, Linux, etc.)
const path = require('node:path');

// Obtenemos el directorio pasado como argumento en la terminal
// process.argv[2] es el tercer argumento (el primero es 'node', el segundo el archivo actual)
// Si no se pasa ningún directorio, por defecto usamos "." (el directorio actual)
const folder = process.argv[2] ?? ".";

// Definimos la función asíncrona 'ls' que listará la información de los archivos del directorio
async function ls(folder) {
    let files;

    // Intentamos leer el contenido del directorio
    try {
        files = await fs.readdir(folder);
    }
    // Si no se puede leer el directorio, mostramos un error y cortamos la ejecución
    catch {
        console.error("No se pudo leer el directorio:", folder);
        process.exit(1);   // Exit code 1 = error
    }
    
    // Recorremos la lista de archivos y carpetas
    // Usamos 'map' para transformar cada archivo en una promesa con su información
    const filesPromises = files.map(async file => {
        // Construimos la ruta absoluta del archivo/carpeta
        const filePath = path.join(folder, file);

        let stats;
        // Obtenemos las estadísticas del archivo (tamaño, fecha de modificación,etc)
        try {
            stats = await fs.stat(filePath);
        }
        // Si no se puede acceder al archivo, mostramos un error y cortamos ejecución
        catch {
            console.error("No se pudo leer el archivo ", filePath);
            process.exit(1);
        }
        // Determinamos si es directorio o archivo normal
        const isDirectory = stats.isDirectory();
        const fileType = isDirectory ? "d" : "f";  // "d" para directorio, "f" para file (archivo)
        
        // Tamaño del archivo en bytes
        const fileSize = stats.size;

        // Fecha de última modificación en formato legible
        const fileModified = stats.mtime.toLocaleString();

        // Devolvemos un string formateado con la información
        // Usamos padEnd/padStart para alinear las columnas de manera prolija
        return `${fileType} ${file.padEnd(25)} ${fileSize.toString().padStart(30)} ${fileModified.padStart(10)}`
    });

    // Esperamos que todas las promesas de filesPromises se resuelvan
    const filesInfo = await Promise.all(filesPromises);

    // Mostramos en consola cada línea con la información del archivo/directorio
    filesInfo.forEach(fileInfo => console.log(fileInfo));
}

// Llamamos a la función con el directorio elegido (el argumento o el actual)
ls(folder);
```