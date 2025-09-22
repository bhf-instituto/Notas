```js
import express from 'express';
import path, {dirname} from "node:path";
import { fileURLToPath } from 'node:url';

const app = express();
const PORT = process.env.PORT || 5000;

// Obtener la ruta del archivo de la URL del modulo actual
// Nos permite navegar atraves de el directorio de carpetas
// de nuestro proyecto ("/2. SQLite")

// Esto nos da acceso al nombre del archivo
const __filename = fileURLToPath(import.meta.url);
// Obtener el directorio del archivo
const __dirname = dirname(__filename);

// esto le dice a la solicitud GET donde está realmente la carpeta public
app.use(express.static(path.join(__dirname, "../public")))

app.get("/", (req, res)=> {
    // esto obviamente da error porque busca la carpeta public dentro de src
    // (que es dodne está este archivo, para configuararlo usasmos un middleware)
    res.sendFile(path.join(__dirname, "public", "index.html"))
})

app.listen(PORT, ()=>{
    console.log(`Servidor corriendo y escuchando en el puerto ${PORT}`);
})
```

## 1. `const __filename = fileURLToPath(import.meta.url);`

`const __dirname = dirname(__filename);`

En **CommonJS** (cuando usas `require`), Node te da gratis `__filename` y `__dirname`.  
En **ESM** (`import/export`), esas variables **no existen**, y hay que reconstruirlas.

- `import.meta.url`  
    → Devuelve la URL del archivo actual, ej:  
    `"file:///C:/Users/Mariano/proyecto/src/server.js"`
    
- `fileURLToPath(import.meta.url)`  
    → Convierte esa URL en una **ruta de archivo normal** del sistema operativo:  
    `"C:\\Users\\Mariano\\proyecto\\src\\server.js"` (Windows)  
    o `"/home/mariano/proyecto/src/server.js"` (Linux).
    
- `dirname(__filename)`  
    → Extrae **solo la carpeta** donde está ese archivo:  
    `"C:\\Users\\Mariano\\proyecto\\src"`  
    o `"/home/mariano/proyecto/src"`.
    

👉 Con esto ya tenés el equivalente a `__dirname` de CommonJS.

---

## 2. `app.use(express.static(path.join(__dirname, "../public")))`

Esto configura un **middleware**.

- `path.join(__dirname, "../public")` → sube una carpeta desde `/src` y entra a `/public`.  
    Queda algo así: `".../proyecto/public"`.
    
- `express.static(...)` crea un “servidor de archivos estáticos”.  
    Si alguien pide `/style.css`, Express va a buscar automáticamente `.../public/style.css`.  
    Si alguien pide `/images/logo.png`, busca en `.../public/images/logo.png`.  
    Incluso, si alguien pide `/` y existe un `index.html` dentro de `public`, **lo sirve solo** (a menos que lo sobrescribas con otra ruta).
    

👉 Básicamente: le estás diciendo a Express _“cualquier archivo que esté en la carpeta `public` se puede servir directamente”_.

---

## 3. `app.get("/", (req, res)=> { res.sendFile(path.join(__dirname, "public", "index.html")) })`

Esto define una **ruta manual**.

- Cuando alguien entra a `/` (la raíz de tu servidor), se ejecuta esa función.
    
- `res.sendFile(...)` envía un archivo concreto.  
    En este caso, `path.join(__dirname, "public", "index.html")`.
    

👉 Entonces aunque `express.static` ya podría servir `index.html` solo, acá vos estás diciendo explícitamente:  
_"Cuando pidan `/`, quiero devolver este archivo específico."_

---

## Resumen con una analogía

- `__dirname` → saber dónde estoy parado en mi proyecto.
    
- `express.static(...)` → decirle al servidor _“cualquier cosa que esté en esta carpeta es accesible”_.
    
- `app.get("/", ...)` → decir _“cuando entren a la página principal `/`, devolvé este archivo concreto”_.