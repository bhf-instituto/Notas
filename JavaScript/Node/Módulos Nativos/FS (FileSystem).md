El módulo nativo **`fs`** (File System) de Node.js sirve para **trabajar con el sistema de archivos**: leer, escribir, copiar, borrar, renombrar y manejar directorios/archivos.

🔹 Cosas clave que podés hacer con `fs`:
- **Leer y escribir archivos** (sincrónica o asincrónicamente).
- **Abrir, cerrar, renombrar y eliminar archivos**.
- **Crear y borrar directorios**.
- **Verificar si un archivo existe** y obtener info con `fs.stat()`.
- Soporta **promesas** (`node:fs/promises`) y callbacks tradicionales.
### *Versión con callbacks*
```js
const fs = require("node:fs");

// Leer un archivo (asincrónico)
fs.readFile("ejemplo.txt", "utf8", (err, data) => {
  if (err) {
    console.error("Error al leer:", err);
    return;
  }
  console.log("Contenido del archivo:", data);
});

// Escribir en un archivo (sobrescribe si ya existe)
fs.writeFile("nuevo.txt", "Hola desde Node.js", (err) => {
  if (err) {
    console.error("Error al escribir:", err);
    return;
  }
  console.log("Archivo creado/escrito con éxito");
});

// Agregar contenido a un archivo
fs.appendFile("nuevo.txt", "\nLínea agregada", (err) => {
  if (err) throw err;
  console.log("Texto agregado");
});

// Obtener información de un archivo
fs.stat("nuevo.txt", (err, stats) => {
  if (err) throw err;
  console.log("Info del archivo:");
  console.log("Tamaño (bytes):", stats.size);
  console.log("¿Es archivo?:", stats.isFile());
  console.log("¿Es directorio?:", stats.isDirectory());
});
```

### *versión con Promises y Async/await*
```js
const fs = require("node:fs/promises");

async function main() {
  try {
    // Leer un archivo
    const data = await fs.readFile("ejemplo.txt", "utf8");
    console.log("Contenido del archivo:", data);

    // Escribir en un archivo (sobrescribe si ya existe)
    await fs.writeFile("nuevo.txt", "Hola desde Node.js con Promises");
    console.log("Archivo creado/escrito con éxito");

    // Agregar contenido
    await fs.appendFile("nuevo.txt", "\nLínea agregada con Promises");
    console.log("Texto agregado");

    // Obtener información de un archivo
    const stats = await fs.stat("nuevo.txt");
    console.log("Info del archivo:");
    console.log("Tamaño (bytes):", stats.size);
    console.log("¿Es archivo?:", stats.isFile());
    console.log("¿Es directorio?:", stats.isDirectory());
  } catch (err) {
    console.error("Ocurrió un error:", err.message);
  }
}

main();
```
___
### Gestor de archivos

```js
const fs = require("fs").promises;
const path = require("path");

// Función auxiliar para verificar si existe un archivo/directorio
async function existe(ruta) {
  try {
    await fs.access(ruta);
    return true;
  } catch {
    return false;
  }
}

async function gestorArchivosRobusto() {
  const dir = path.join(__dirname, "carpeta_test");
  const archivoOriginal = path.join(dir, "archivo.txt");
  const archivoMovido = path.join(__dirname, "archivo_movidito.txt");

  try {
    // 1. Crear directorio si no existe
    if (!(await existe(dir))) {
      await fs.mkdir(dir, { recursive: true });
      console.log("Directorio creado:", dir);
    } else {
      console.log("Directorio ya existía:", dir);
    }
    // 2. Crear y escribir en un archivo
    await fs.writeFile(archivoOriginal, "Este es un archivo de prueba.");
    console.log("Archivo creado:", archivoOriginal);
    // 3. Mover (renombrar) el archivo
    if (await existe(archivoOriginal)) {
      await fs.rename(archivoOriginal, archivoMovido);
      console.log("Archivo movido a:", archivoMovido);
    } else {
      console.log("El archivo original no existe, no se puede mover.");
    }
    // 4. Borrar el archivo movido
    if (await existe(archivoMovido)) {
      await fs.unlink(archivoMovido);
      console.log("Archivo borrado:", archivoMovido);
    } else {
      console.log("El archivo movido no existe, no se puede borrar.");
    }
    // 5. Eliminar el directorio
    if (await existe(dir)) {
      await fs.rmdir(dir);
      console.log("Directorio eliminado:", dir);
    } else {
      console.log("El directorio no existe, no se puede borrar.");
    }
    
  } catch (err) {
    console.error("Error en gestor de archivos:", err.message);
  }
}

gestorArchivosRobusto();

```

### Ejemplo de readdir con callback, promises y async await.
```js
	fs.readdir(".",(err, files) => {
	    if(files == null){              // manejo de error 
	        console.error(err);
	        return;
	    }
	    files.forEach(file => {
	        console.log("- ", file);
	    })
	})
	// con promesa
	fs.readdir(".")
		.then(response => {
	    response.forEach(res => console.log("- ", res))
	})
		.catch(err => console.error(err));
	// con async await
	async function readDir(){
	    const files = await fs.readdir(".");
	    files.forEach(file => console.log("- ", file))
	}
	readDir();
	
```