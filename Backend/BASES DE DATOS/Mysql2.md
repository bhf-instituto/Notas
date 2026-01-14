[[Transacciones]] :
* **NO se deben hacer transacciones directamente en un pool.** 
* Así que hay que llamar a ese pool y crear una conexión nueva. 

>Por eso `const connection = await pool.getConnection();`. 
   En este caso connection es una conexión nueva que parte del pool conn.

```js
//
export const createSet = async (setName, userId) => {
    const connection = await pool.getConnection();

    try {
        await connection.beginTransaction();

        const [result] = await connection.query(
            `INSERT INTO sets (name) VALUES (?)`,
            [setName]
        );

        const setId = result.insertId;

        await connection.query(
            `INSERT INTO set_user (set_id, user_id, role)
             VALUES (?, ?, 1)`,
            [setId, userId]
        );

        await connection.commit();

        return setId;
    } catch (error) {
        await connection.rollback(); // 👈 CLAVE
        throw error;                 // 👈 se propaga
    } finally {
        connection.release();
    }
};
```

A diferencia de otros repositories con queries simples, en este debemos manejar errores ya que las [Transacciones] deben abrirse y cerrarse. Si por ejemplo, se inicia una transacción, luego se hace una query, luego una segunda query y esta falla: la transacción quedará abierta.  

En el caso de ejemplo, al controlar el posible error hacemos un `rollback()`, es decir que deshacemos la transacción y sus potenciales cambios. Y finalmente agregamos un bloque `finally{}` a la estructura `try/catch` para que, **pase lo que pase**, se suelte esa conexión que se inició con el método `release()`.

Entonces: 
* Creamos la conexión `connection` partiendo del pool `conn.
* Comenzamos la transacción `beginTransaction()`.
* Realizamos las `queries`.
* Si todo sale bien hacemos `commit()` de esa transacción .
* Si hay error revertimos los cambios con `rollback()`
* Finalmente, **pase lo que pase**, soltamos la conexión con `release()`