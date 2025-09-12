El módulo nativo **`os`** de Node.js provee utilidades para interactuar con el **sistema operativo** en el que corre tu aplicación.

Con él podés obtener información como:
- El **tipo de sistema operativo** (`os.type()`, `os.platform()`, `os.release()`).
- Datos del **CPU** y de los **núcleos** (`os.cpus()`).
- La **memoria total y libre** (`os.totalmem()`, `os.freemem()`).
- El **directorio del usuario actual** (`os.homedir()`).
- La **arquitectura** (`os.arch()`) y el **uptime** del sistema (`os.uptime()`).

```js
const os = require("node:os");

console.log("=== Información del Sistema Operativo ===");

// Tipo de sistema y versión
console.log("Tipo:", os.type());         // Ej: 'Windows_NT', 'Linux', 'Darwin'
console.log("Plataforma:", os.platform()); // Ej: 'win32', 'linux', 'darwin'
console.log("Release:", os.release());

// Arquitectura
console.log("Arquitectura:", os.arch()); // Ej: 'x64'

// CPUs
console.log("CPUs:", os.cpus().length, "núcleos");
console.log("Modelo de CPU:", os.cpus()[0].model);

// Memoria
console.log("Memoria total (MB):", (os.totalmem() / 1024 / 1024).toFixed(2));
console.log("Memoria libre (MB):", (os.freemem() / 1024 / 1024).toFixed(2));

// Usuario y directorios
console.log("Directorio del usuario:", os.homedir());
console.log("Directorio temporal:", os.tmpdir());

// Tiempo de actividad del sistema (en horas)
console.log("Uptime (horas):", (os.uptime() / 3600).toFixed(2));

```