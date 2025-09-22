### 🔹 ¿Qué es?

`nodemon` es una herramienta que **observa los archivos de tu proyecto Node.js** y **reinicia automáticamente el servidor cada vez que detecta cambios** en el código.

Es decir, evita que tengas que **detener y volver a correr `node server.js` cada vez que editás algo**.

```bash
npm install -D nodemon
```

```json
"scripts": {
  "dev": "nodemon server.js"
}
```

> Es un watcher de toda la vida