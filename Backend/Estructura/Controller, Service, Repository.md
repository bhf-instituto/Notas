## 1️⃣ Repository (Capa de datos)

**Rol:** interactuar directamente con la base de datos.
- Aquí no hay lógica de negocio, solo consultas a la DB.
- Normalmente trabaja con un ORM (Sequelize, TypeORM) o con consultas SQL crudas (mysql2, pg).

**Recibe:** datos mínimos necesarios para la consulta (IDs, filtros, objetos a insertar/actualizar).  
**Devuelve:** los datos tal como los obtiene de la DB (puede ser un array, un objeto o null).

**Ejemplo usando `mysql2`:**
```js
import db from '../config/connectionMySQL.js';

export const getUserById = async (id) => {
    const [rows] = await db.execute('SELECT * FROM users WHERE id = ?', [id]);
    return rows[0]; // devuelve un objeto usuario o undefined
};

export const createUser = async (userData) => {
    const { email, password } = userData;
    const [result] = await db.execute(
        'INSERT INTO users (email, password) VALUES (?, ?)',
        [email, password]
    );
    return result.insertId;
};

```

## 2️⃣ Service (Capa de lógica de negocio)

**Rol:** contiene la lógica de negocio de tu aplicación.
- Usa los **repositories** para leer/escribir datos.
- Aquí decides reglas, validaciones, transformaciones, cálculos, etc.
- Nunca debe manejar la petición HTTP directamente, solo los datos.

**Recibe:** datos de la capa de Controller (normalmente `req.body`, `req.params`, etc.) ya validados parcialmente.  
**Devuelve:** resultados listos para enviar al Controller, o errores controlados (throw o return error object).

**Ejemplo:**
```js
import * as userRepository from '../repositories/userRepository.js';
import bcrypt from 'bcryptjs';

export const registerUser = async (email, password) => {
    // Validación de negocio
    if (!email || !password) throw new Error('Email y password requeridos');

    // Normalizar email
    const normalizedEmail = email.toLowerCase();

    // Hashear password
    const hashedPassword = await bcrypt.hash(password, 10);

    // Guardar en DB
    const userId = await userRepository.createUser({
        email: normalizedEmail,
        password: hashedPassword
    });

    return { id: userId, email: normalizedEmail };
};

export const getUserProfile = async (id) => {
    const user = await userRepository.getUserById(id);
    if (!user) throw new Error('Usuario no encontrado');
    delete user.password; // nunca enviar password
    return user;
};

```

## 3️⃣ Controller (Capa de HTTP / API)

**Rol:** manejar la petición y respuesta HTTP.
- Llama a los **services**.
- Maneja códigos HTTP, headers, status, JSON responses, etc.
- Aquí ocurre la validación inicial de parámetros (puede usarse `express-validator`).

**Recibe:** `req` y `res` de Express.  
**Devuelve:** respuesta HTTP con `res.status().json()`.

**Ejemplo:**
```js
import * as userService from '../services/userService.js';

export const registerUserController = async (req, res) => {
    try {
        const { email, password } = req.body;
        const newUser = await userService.registerUser(email, password);
        return res.status(201).json({ ok: true, data: newUser });
    } catch (error) {
        return res.status(400).json({ ok: false, message: error.message });
    }
};

export const getUserProfileController = async (req, res) => {
    try {
        const userId = req.params.id;
        const user = await userService.getUserProfile(userId);
        return res.status(200).json({ ok: true, data: user });
    } catch (error) {
        return res.status(404).json({ ok: false, message: error.message });
    }
};

```

## 4️⃣ Flujo general

1. **Cliente** hace petición HTTP → `/users/:id`.
2. **Controller** recibe `req`, llama a `service` → maneja status y respuesta.
3. **Service** aplica la lógica de negocio → llama a `repository` para acceder a datos.
4. **Repository** consulta la base de datos → devuelve resultado al `service`.
5. **Service** procesa los datos → devuelve al `controller`.
6. **Controller** construye la respuesta HTTP → devuelve al cliente.

```lua
HTTP Request --> Controller --> Service --> Repository --> DB
                                     <--         <--
                 <--              <--           <--
HTTP Response <-- Controller <---- Service <---- Repository

```

### ✅ Buenas prácticas

- **Controller:** solo manejo HTTP y validaciones básicas.
- **Service:** lógica de negocio, cálculos, reglas, transformaciones.
- **Repository:** acceso a la base de datos, queries o ORM.
- **Errores:** siempre lanzar en Service, atraparlos en Controller para responder HTTP.
- Mantener **inmutabilidad** y no exponer passwords ni datos sensibles.