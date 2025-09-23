`jsonwebtoken` es un paquete de Node.js que permite **crear** y **verificar** tokens JWT (JSON Web Tokens).

### 👉 **¿Qué es un JWT?**  
Un **JWT** es un string en formato `header.payload.signature` usado para **autenticación y autorización** en aplicaciones.  

Un **JWT (JSON Web Token)** es un estándar abierto (RFC 7519) para **transmitir información de forma segura entre dos partes como un objeto JSON**.  
Se utiliza principalmente en **autenticación y autorización**.

Un token JWT tiene tres partes:

1. **Header** (encabezado): dice el algoritmo de firma y el tipo de token.
2. **Payload** (carga útil): contiene los datos (ej. id de usuario, email, roles).
3. **Signature** (firma): asegura que el token no fue modificado.

Se genera con una **clave secreta** (o certificado privado) y se usa para:

- Identificar usuarios.
- Manejar sesiones sin guardar estado en el servidor (stateless).
- Proteger rutas o recursos en una API.

### 🛡️ ¿Para qué sirve un JWT?

Principalmente para **autenticación sin estado (stateless)**:

- El usuario inicia sesión con usuario/contraseña.
- El servidor genera un JWT y lo devuelve al cliente.
- El cliente guarda el token (ej: en localStorage o cookie).
- Cada vez que el cliente hace una petición, envía el JWT en los headers (`Authorization: Bearer <token>`).
- El servidor valida el token y, si es correcto, permite el acceso.

De esta forma:

- El servidor **no necesita guardar sesiones en memoria o base de datos**, solo valida el token.
- Es ideal para **APIs REST** y **microservicios**.

### ⏳ ¿Por qué tiene tiempo de expiración?

Por seguridad.

- Si un JWT fuera **infinito**, cualquiera que lo robe tendría acceso para siempre.
    
- Con un tiempo de expiración (ej. `15m`, `1h`, `7d`), el token caduca y ya no sirve.
    
- Esto **limita el daño en caso de robo**.
    

Generalmente se usan dos tipos:

- **Access Token** (vida corta, ej. 15 min – 1h).
    
- **Refresh Token** (vida más larga, ej. 7 días – 30 días). Se usa para pedir un nuevo access token sin volver a loguearse.
### Crear (firmar) un token

```js
const secretKey = "miClaveSuperSecreta"; // normalmente va en variables de entorno (.env)

const user = {
    id: 1,
    username: "mariano",
    role: "admin"
};

// Generar token con expiración
const token = jwt.sign(user, secretKey, { expiresIn: "1h" });

console.log("Token generado:", token);
```
👉 El `payload` (`user` en este caso) se guarda en el token, pero **cualquiera puede verlo** (JWT está codificado en Base64, no cifrado).  
Por eso nunca pongas contraseñas ni datos críticos en el token.

---
## 📌 .sign()

Sintaxis básica
```js
jwt.sign(payload, secretOrPrivateKey, [options, callback])
```

### 1. **`payload`**

Es la **información que querés meter dentro del token**.

- Puede ser un objeto, un string o incluso un buffer.
- Normalmente contiene datos del usuario, pero **nunca pongas información sensible** (como contraseñas).

Ejemplo típico:
```js
const payload = {
  id: user.id,
  username: user.username,
  role: user.role
};
```

---
### 2. **`secretOrPrivateKey`**

La **clave secreta** que usás para firmar el token.

- En proyectos simples: una string (`"miClaveSuperSecreta"`).
- En producción: mejor usar una **clave fuerte** en una variable de entorno.
- También puede ser una **clave privada RSA/EC** si usás algoritmos asimétricos.

Ejemplo:
```js
const secret = process.env.JWT_SECRET;
```

---
### 3. **`options`** _(opcional)_

Objeto con configuraciones para el token. Las más usadas son:

- **`expiresIn`** → tiempo de expiración (ej: `"1h"`, `"2d"`, `60` seg).
- **`algorithm`** → algoritmo de firmado (default: HS256).
- **`issuer`** → quién emitió el token.
- **`subject`** → a quién pertenece el token.
- **`audience`** → para quién está pensado el token.
- **`notBefore`** → desde cuándo es válido el token.

Ejemplo:
```js
const options = {
  expiresIn: "1h",
  issuer: "miApp"
};
```

---
### 4. **`callback`** _(opcional)_

Si no lo ponés, `sign` devuelve el token directamente.  
Si lo ponés, trabaja de forma asíncrona y devuelve error o token en el callback.

Ejemplo sin callback:
```js
const token = jwt.sign(payload, secret, options);
```

Ejemplo con callback:
```js
jwt.sign(payload, secret, options, (err, token) => {
  if (err) {
    console.error("Error al generar token:", err);
  } else {
    console.log("Token generado:", token);
  }
});
```

### Ejemplo completo en Express:
```js
import jwt from "jsonwebtoken";

app.post("/login", (req, res) => {
  const { username, password } = req.body;

  // Simulación de usuario encontrado
  const user = { id: 1, username: "mariano", role: "admin" };

  // Crear token
  const token = jwt.sign(
    { id: user.id, username: user.username, role: user.role }, 
    process.env.JWT_SECRET, 
    { expiresIn: "1h" }
  );

  res.json({ token });
});
```
### Verificar un token

```js
try {
    const decoded = jwt.verify(token, secretKey);
    console.log("Token válido:", decoded);
} catch (error) {
    console.log("Token inválido o expirado");
}
```

### Decodificar un token sin verificar (solo lectura)

```js
const decodedData = jwt.decode(token);
console.log("Payload del token:", decodedData);
```
⚠️ `jwt.decode()` **no valida la firma**, solo lee el contenido. Úsalo con cuidado.

### 🔹 Ejemplo  en Express (mini log)

```js
const express = require("express");
const jwt = require("jsonwebtoken");

const app = express();
app.use(express.json());

const secretKey = "miClaveSuperSecreta";

// Login falso para generar token
app.post("/login", (req, res) => {
    const { username, password } = req.body;

    if (username === "admin" && password === "1234") {
        const token = jwt.sign({ username, role: "admin" }, secretKey, { expiresIn: "1h" });
        return res.json({ token });
    }

    res.status(401).json({ error: "Credenciales inválidas" });
});

// Middleware para proteger rutas
function authMiddleware(req, res, next) {
    const authHeader = req.headers["authorization"];
    const token = authHeader && authHeader.split(" ")[1];

    if (!token) return res.sendStatus(403);

    try {
        const decoded = jwt.verify(token, secretKey);
        req.user = decoded;
        next();
    } catch (error) {
        return res.status(401).json({ error: "Token inválido o expirado" });
    }
}

// Ruta protegida
app.get("/dashboard", authMiddleware, (req, res) => {
    res.json({ message: `Bienvenido ${req.user.username}, rol: ${req.user.role}` });
});

app.listen(3000, () => console.log("Servidor en http://localhost:3000"));
```

## 🔹 Buenas prácticas

1. Guarda el `secretKey` en variables de entorno (no en el código).
2. Define un tiempo de expiración (`expiresIn`) para mayor seguridad.
3. Usa `https` en producción para evitar robo de tokens.
4. Almacena el token en el **header Authorization** (`Bearer <token>`), no en cookies sin protección.
5. Combínalo con `bcryptjs` para validar contraseñas antes de generar el token.
