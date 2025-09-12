### **Nodemon**
 Es una herramienta para el desarrollo en NodeJS, sirve para reiniciar automáticamente la aplicación cada vez que se hagan cambios en los archivos del proyecto. Es un watcher de toda la vida. 
```
npm install -g nodemon

//Como dependencia de desarrollo
npm install --save-dev nodemon
```

Luego, en el [[package.json]] se crea el script para ejecutar el watcher.
```json
  "scripts": {
    "dev": "nodemon /src/index.js"
  }...
```
