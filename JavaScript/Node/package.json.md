Es el archivo de configuración principal en un proyecto de **Node.js**.  
Funciona como el **"manual de instrucciones"** de tu proyecto: ahí queda registrado todo lo necesario para que vos (o cualquier otra persona) pueda instalar, ejecutar y entender tu aplicación.

```cmd
npm init
```
### 🔹 ¿Qué contiene?

#### 1. **Metadatos del proyecto**
- Nombre, versión, autor, descripción, licencia, etc.
```json
{
  "name": "mi-app",
  "version": "1.0.0",
  "description": "Un proyecto con Node.js",
  "author": "Mariano"
}
```
#### 2. **Dependencias**
- Librerías que tu proyecto necesita para funcionar. //*npm install paquete*
```json
"dependencies": {
  "express": "^4.18.2"
}
```
#### 3. **Dependencias de desarrollo**
- Solo necesarias en desarrollo (no en producción).
```json
"devDependencies": {
  "nodemon": "^3.0.1"
}
```
#### 4. **Scripts**
- Atajos para ejecutar comandos.     //*npm run dev*
```json
"scripts": {
  "start": "node app.js",
  "dev": "nodemon app.js"
}
```