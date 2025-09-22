`cross-env` es un paquete de Node.js que permite **definir variables de entorno de forma compatible entre diferentes sistemas operativos** (Windows, Linux, MacOS).  
Normalmente, la sintaxis para setear variables cambia según el sistema operativo, lo cual puede romper scripts en equipos distintos. `cross-env` resuelve este problema.

```powershell
npm install --save-dev cross-env
```
> Se recomienda instalar para desarrollo porque no se utiliza en producción

### Diferencias entre SOs

Bash:
```BASH
PORT=3000 node server.js
```

Powershell:
```powershell
$env:PORT=3000; node server.js
```

Si escribís uno de esos en `package.json`, solo va a funcionar en **un sistema**, no en ambos.  
Ahí entra `cross-env`.

### 🔹 Cómo funciona

Con `cross-env`, escribís tu script **una sola vez** y funciona en cualquier sistema:

```json
"scripts": {
  "dev": "cross-env PORT=3000 node server.js"
}
```
Ahora, da igual si lo corre un usuario en Windows, Linux o MacOS.

### 🔹 Beneficio principal

✅ Portabilidad: asegurás que todos los desarrolladores del proyecto puedan ejecutar los scripts sin importar el sistema operativo.  
✅ Simplicidad: escribís una sola vez, sin preocuparte por diferencias de sintaxis.

---

👉 En resumen: **cross-env es un “traductor universal” para variables de entorno en los scripts de Node.**