`bcryptjs` es una implementación **pura en JavaScript** del algoritmo bcrypt para Node.js.  
Se usa principalmente para **hashear contraseñas** y **compararlas de forma segura**, ya que bcrypt aplica un **salting** automático y un proceso de encriptado lento para dificultar ataques de fuerza bruta.

👉 Diferencia con `bcrypt`:

- `bcrypt` depende de bindings nativos de C++ → a veces da problemas al instalar en Windows o entornos restringidos.
- `bcryptjs` es **100% JS**, sin dependencias nativas → instalación más simple y multiplataforma.

### Hashear una contraseña 
[[Estructura de Datos Hash]]

```js
const password = "miSuperClave123";

// número de rondas de salt (coste). Entre 8 y 12 suele estar bien.
const saltRounds = 10;

const hashedPassword = bcrypt.hashSync(password, saltRounds);

console.log("Password original:", password);
console.log("Password hasheado:", hashedPassword);
```
👉 Observa que cada vez que lo ejecutes, el resultado es distinto porque bcrypt genera un **salt único**.

### Verificar una contraseña

```js
const loginPassword = "miSuperClave123";

const isMatch = bcrypt.compareSync(loginPassword, hashedPassword);

if (isMatch) {
    console.log("✅ Contraseña correcta");
} else {
    console.log("❌ Contraseña incorrecta");
}
```

### Version Asincrónica (Producción)

```js
const bcrypt = require("bcryptjs");

async function run() {
    const password = "miSuperClave123";
    const saltRounds = 10;

    // Hashear
    const hash = await bcrypt.hash(password, saltRounds);
    console.log("Hash generado:", hash);

    // Comparar
    const match = await bcrypt.compare("miSuperClave123", hash);
    console.log("¿Coincide?:", match);
}

run();
```

## 🔹 Buenas prácticas

1. Usa **hash + salt** siempre (bcrypt lo maneja internamente).
2. Ajusta `saltRounds` según tu entorno (más rondas = más seguro pero más lento).
3. Nunca guardes la contraseña en texto plano, solo el hash.
4. Para login → compara con `bcrypt.compare()` en lugar de intentar desencriptar (bcrypt no es reversible).

## 🔹 ¿Qué es un `salt`?

Un **salt** es una **cadena aleatoria** que se agrega a la contraseña **antes de hashearla**.  
Se guarda junto al hash en la base de datos y sirve para que cada hash sea **único**, incluso si dos usuarios tienen la misma contraseña.

Ejemplo:

- Contraseña: `123456`
- Sin salt → hash: `abc123...`
- Con salt → hash: `k9s!f1@abc123...`

Así, aunque dos usuarios usen `123456`, el resultado será diferente.

---
## 🔹 ¿Por qué se llama "salt"?

La palabra viene del inglés **"sal"** (de cocina 🧂).  
La idea es **"sazonar" la contraseña con un poco de aleatoriedad extra**, igual que cuando a la comida se le pone sal para darle un sabor distinto.

Es una metáfora muy usada en criptografía:

- La **contraseña** es la "comida básica".
- El **salt** es la "sal" que le da un toque único.
- El **hash final** es la "receta resultante".

---

## 🔹 ¿Por qué es tan importante el `salt`?

1. **Evita ataques con rainbow tables**  
    Un atacante no puede usar tablas precalculadas de hashes comunes (`123456`, `password`, etc.), porque cada hash depende del `salt` único.
2. **Protege contraseñas repetidas**  
    Si dos usuarios usan la misma contraseña, sus hashes serán diferentes gracias al `salt`.
3. **Agrega aleatoriedad**  
    Hace que cada hash generado sea único, incluso con la misma contraseña y el mismo algoritmo.

En `bcrypt`, el `salt` se genera automáticamente:
```js
const salt = bcrypt.genSaltSync(10);  // genera un salt con 10 rondas
const hash = bcrypt.hashSync("miClave", salt);
```
Incluso si llamas 100 veces a ese código con la misma clave, **cada hash será distinto**, porque cada vez se crea un `salt` nuevo.  
👉 Pero no te preocupes: el `salt` se guarda dentro del hash mismo, así que `bcrypt.compare()` sabe cómo verificarlo.