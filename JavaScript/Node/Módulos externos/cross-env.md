`cross-env` es un paquete de Node.js que permite **definir variables de entorno de forma compatible entre diferentes sistemas operativos** (Windows, Linux, MacOS).  
Normalmente, la sintaxis para setear variables cambia según el sistema operativo, lo cual puede romper scripts en equipos distintos. `cross-env` resuelve este problema.

Bash:
```BASH
PORT=3000 node server.js
```

Powershell:
```powershell
$env:PORT=3000; node server.js
```
