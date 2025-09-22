El módulo nativo **`path`** de Node.js sirve para **trabajar con rutas de archivos y directorios** de forma segura y multiplataforma (Windows usa `\`, Linux/Mac usan `/`).

🔹 **¿Para qué se usa?**
- Unir directorios y nombres de archivo sin preocuparte por las barras.
- Resolver rutas absolutas y relativas.
- Obtener extensiones, nombres de archivos y directorios.
- Normalizar rutas (limpia `../` y cosas raras).

---
### Curso midudev
```js
const path = require('node:path');

// barra separadora de carpetas segun SO
console.log(path.sep); //devuelve en windows → / y en iOS → \

// unir rutas con path join
const filePath = path.join("content", "subfolder", "test.txt");
console.log(filePath); // devuelve → content\subfolder\test.txt

// obtener nombre de fichero con su extension
const base = path.basename("/tmp/mirko/archivo.txt");
console.log(base);

// obtener nombre de fichero sin extension
const fileName = path.basename("/tmp/mirko/archivo", ".txt");
console.log(fileName);

// obtener extension
let extension = path.extname("/tmp/mirko/archivo.txt");
console.log(extension);

// sirve porque capaz uno tiene que acceeder a la extension de un  archivo con un nombre asi :
extension = path.extname("/tmp/mirko/archivo.de.texto.txt");
console.log(extension);
```

📌 **Funciones más comunes de `path`:**
```js
const path = require("path");

// Ruta actual del archivo
console.log("dirname:", __dirname); // directorio actual
console.log("filename:", __filename); // archivo actual con ruta completa

// Unir directorios de forma segura
const ruta = path.join("carpeta", "subcarpeta", "archivo.txt");
console.log("join:", ruta); // "carpeta/subcarpeta/archivo.txt"

// Resolver ruta absoluta
const abs = path.resolve("archivo.txt");
console.log("resolve:", abs); // C:\... o /home/... dependiendo del SO

// Obtener partes de la ruta
const ejemplo = "/home/user/proyecto/index.js";
console.log("basename:", path.basename(ejemplo)); // "index.js"
console.log("dirname:", path.dirname(ejemplo));   // "/home/user/proyecto"
console.log("extname:", path.extname(ejemplo));   // ".js"

// Normalizar ruta
console.log("normalize:", path.normalize("/home/user/../proyecto//index.js"));
// Resultado: "/home/proyecto/index.js"

// Separadores según SO
console.log("sep:", path.sep);     // "\" en Windows, "/" en Linux/Mac
console.log("delimiter:", path.delimiter); // ";" en Windows, ":" en Linux/Mac
```

___

### 📌 **Ejemplo práctico (`fs_path_demo.js`)**

*Combinamos **`fs.promises`** con **`path`** para trabajar con rutas sin hardcodearlas:*
```js
const fs = require("fs").promises;
const path = require("path");

async function demoFSPath() {
  try {
    // 1. Creamos un directorio dentro del proyecto
    const dir = path.join(__dirname, "demo_carpeta");
    await fs.mkdir(dir, { recursive: true });
    console.log("Directorio creado:", dir);
    // 2. Creamos un archivo dentro de esa carpeta
    const archivo = path.join(dir, "archivo_demo.txt");
    await fs.writeFile(archivo, "Contenido inicial generado con fs + path");
    console.log("Archivo creado:", archivo);
    // 3. Leemos el archivo
    const contenido = await fs.readFile(archivo, "utf8");
    console.log("Contenido leído:", contenido);
    // 4. Mostramos información de la ruta del archivo
    console.log("basename:", path.basename(archivo)); // archivo_demo.txt
    console.log("dirname:", path.dirname(archivo));   // ruta completa de la carpeta
    console.log("extname:", path.extname(archivo));   // .txt
    // 5. Eliminamos archivo y carpeta
    await fs.unlink(archivo);
    await fs.rmdir(dir);
    console.log("Archivo y directorio eliminados.");
    
  } catch (err) {
    console.error("Error en demoFSPath:", err.message);
  }
}

demoFSPath();
```

Este ejemplo te muestra cómo:
- Crear rutas seguras con `path.join`.
- Evitar hardcodear `/` o `\`.
- Usar `basename`, `dirname` y `extname` para extraer partes de una ruta.
___

### 📌 **Ejemplo práctico 2 (`listar_txt.js`)**

*Listamos todos los archivos de una carpeta y usamos **`path.extname`** para filtrar únicamente los `.txt`.*

```js
const fs = require("fs").promises;
const path = require("path");

async function listarArchivosTxt() {
  const dir = path.join(__dirname, "mis_archivos");

  try {
    // 1. Crear carpeta si no existe
    await fs.mkdir(dir, { recursive: true });
    // 2. Crear algunos archivos de prueba
    const archivosPrueba = [
      "nota1.txt",
      "nota2.txt",
      "imagen.png",
      "documento.pdf",
      "readme.txt"
    ];
    
    for (const nombre of archivosPrueba) {
      const archivo = path.join(dir, nombre);
      await fs.writeFile(archivo, `Contenido de ${nombre}`);
    }
    console.log("Archivos de prueba creados en:", dir);
    // 3. Leer contenido de la carpeta
    const archivos = await fs.readdir(dir);
    console.log("Todos los archivos:", archivos);
    // 4. Filtrar solo los .txt
    const archivosTxt = archivos.filter((archivo) => path.extname(archivo) === ".txt");
    console.log("Archivos .txt encontrados:", archivosTxt);

  } catch (err) {
    console.error("Error en listarArchivosTxt:", err.message);
  }
}

listarArchivosTxt();
```
📌 ¿Qué hace?

1. Crea una carpeta `mis_archivos`.
2. Mete dentro algunos archivos de prueba con diferentes extensiones.
3. Lista todo con `fs.readdir()`.
4. Filtra solo los `.txt` usando `path.extname`