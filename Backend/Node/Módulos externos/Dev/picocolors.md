`picocolors` es una librería de **colores para la consola** en Node.js, similar a `chalk` o `kleur`, pero con un enfoque en ser:

- **Ultra ligera** (menos de 100 líneas de código).
- **Rápida** (0 dependencias, mínima sobrecarga).
- **Compatible** con Node.js y navegadores.
- **Sencilla**: solo funciones puras, sin "mágica" detrás.

Sirve para **colorear texto** en la terminal, ideal para logs, CLIs o scripts.

---
### 🔹 Uso básico
```js
const pc = require("picocolors");

console.log(pc.green("✔ Operación exitosa"));
console.log(pc.red("✖ Error al procesar"));
console.log(pc.yellow("⚠ Advertencia"));
console.log(pc.blue("ℹ Información"));
```
👉 Cada función (`red`, `green`, `blue`, etc.) **devuelve un string coloreado** usando códigos ANSI.

--- 
## 🔹 Colores y estilos disponibles

- Colores básicos:  
    `pc.black`, `pc.red`, `pc.green`, `pc.yellow`,  
    `pc.blue`, `pc.magenta`, `pc.cyan`, `pc.white`.
- Colores brillantes:  
    `pc.gray`, `pc.bgBlack`, `pc.bgRed`, etc.
- Estilos:  
    `pc.bold`, `pc.dim`, `pc.italic`, `pc.underline`,  
    `pc.inverse`, `pc.hidden`, `pc.strikethrough`.

### 🔹 Combinación de estilos 
```js
console.log(pc.bold(pc.green("✔ Todo listo")));
console.log(pc.bgRed(pc.white(" ERROR CRÍTICO ")));
```