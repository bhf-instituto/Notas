##  *DDL (Data Definition Language)*
```MYSQL
CREATE
ALTER 
DROP
TRUNCATE
```
Para borrar todos los datos de una tabla rápidamente, y resetear su contador **AUTO_INCREMENT**
(No sirve si tiene **FOREIGN KEYS** ):
```mYsQL
TRUNCATE TABLE nombre_de_la_tabla;
```


Para resetear una tabla a su estado inicial:
```mysql
SET FOREIGN_KEY_CHECKS = 0;

TRUNCATE TABLE expenses;
TRUNCATE TABLE collection_users;
TRUNCATE TABLE collections;
TRUNCATE TABLE users;

SET FOREIGN_KEY_CHECKS = 1;

# - Borra **TODOS** los registros 
# - Resetea los `AUTO_INCREMENT`  
# - Mantiene:
#     - tablas  
#     - índices 
#     - constraints 
# - Queda exactamente como **recién creada**

```

```MySql
CREATE DATABASE mi_db;
USE mi_db;
DROP DATABASE mi_db;
DROP TABLE tabla;
```

Para crear una DB y asignarle el **CHARACTER SET**: 
```MySql
CREATE DATABASE joins
CHARACTER SET utf8
COLLATE utf8_spanish_ci;
```
- `CHARACTER SET utf8`: define que la base de datos usará la codificación UTF-8.
- `COLLATE utf8_spanish_ci`: define que se usará la colación "spanish case-insensitive". La **colación** (`collation`) no define los caracteres en sí, sino **cómo se comparan y ordenan**.  El sufijo `ci` significa **case insensitive** = **no distingue entre mayúsculas y minúsculas**.

```sql
SELECT 'hola' = 'HOLA'; -- devuelve TRUE con utf8_spanish_ci
```

Ejemplo:

Para crear una tabla:
```MySql
-- Primero creamos la tabla que no tiene FOREIGN KEY, para que no de error. Ya que crear una tabla con una FOREIGN KEY referenciada a una tabla que no existe (o que no tiene el atributo requerido) nos dará error.
CREATE TABLE tabla (
	id_tabla INT AUTO_INCREMENT PRIMARY KEY,
	nombre VARCHAR(35) NOT NULL,
	);

CREATE TABLE tabla2 (
	id_tabla2 INT AUTO_INCREMENT PRIMARY KEY,
	nombre VARCHAR(35) NOT NULL,
	sueldo DOUBLE DEFAULT 0,
	id_tabla INT,
	FOREIGN KEY (id_tabla) REFERENCES tabla(id_tabla)
) ENGINE=InnoDB;
-- ELEGIMOS EL MOTOR

PRIMARY KEY (comp, uesta)
-- La PK puede ser compuesta utilizando el valor de 
CREATE TABLE hamburguesa_ingredientes (
	id_hamburguesa INT,
    id_ingrediente INT,
	PRIMARY KEY (id_hamburguesa, id_ingrediente),
    FOREIGN KEY (id_hamburguesa) REFERENCES hamburguesas_base(id_hamburguesa),
	FOREIGN KEY (id_ingrediente) REFERENCES ingredientes(id_ingrediente)
);

## Declarando un ENUM: 
CREATE TABLE expenses (
    id_expense INT AUTO_INCREMENT PRIMARY KEY,
    amount DECIMAL(10,2) UNSIGNED NOT NULL,
    type ENUM('FIJO', 'VARIABLE') NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- DATETIME DEFAULT CURRENT_TIMESTAMP elegimos como valor por defecto el la hora actual. 
CREATE TABLE tabla(
    fecha DATETIME DEFAULT CURRENT_TIMESTAMP,
) ENGINE = INNODB;
```

Para renombrar una tabla:
```MySQL
RENAME TABLE nombre_actual TO nuevo_nombre;
```
Para re-setear el contador de **AUTO_INCREMENT** al valor deseado:
```MySql
ALTER TABLE productos AUTO_INCREMENT = 3;
```
Para renombrar una columna, ej.:
```MySql
ALTER TABLE productos CHANGE precio costo DOUBLE;
```
 Renombro la columna precio por costo, y además siempre hay que declarar el tipo de dato que aceptará la columna luego de renombrarla. 
 ```MySql
ALTER TABLE productos CHANGE nombre producto_nombre VARCHAR(50);
ALTER TABLE productos CHANGE producto_nombre producto_valor DOUBLE;
```

Para modificar solamente el tipo de dato de una columna, en este caso de DOUBLE a DECIMAL. Al cual hay que especificarle la cantidad de números antes y después de la coma **(Antes , Después)** Esto se llama precisión, precisión de la longitud de los valores.
```mYsQL
ALTER TABLE productos MODIFY costo DECIMAL (12,2);
```

### Resetear una tabla con relaciones
Borro todos los datos de las tablas, seteo (reseteo) los indices (index) a 0.

```mysql
SET FOREIGN_KEY_CHECKS = 0; 

DELETE FROM users ;
# Depende si las quier oborrar o solo truncar
TRUNCATE FROM users ;

ALTER TABLE users AUTO_INCREMENT = 1;
SET FOREIGN_KEY_CHECKS = 1;
```



Se puede validar datos :
```mysql

## en este caso valido que la cantidad sea mayor a 0
CREATE TABLE expenses (
    id_expense INT AUTO_INCREMENT PRIMARY KEY,
    amount INT NOT NULL,
    CHECK (amount >= 0)
);

## Tambien tenemos UNSIGNED que le quita el signo al numero, duplica el rango de positivos.
CREATE TABLE expenses (
    amount INT UNSIGNED NOT NULL,
);

ALTER TABLE expenses
ADD CONSTRAINT chk_amount_non_negative
CHECK (amount >= 0);
```

Al crear una restricción ( CONSTRAINT ) hay que explicitar el nombre de por ejemplo una relación, si no SQL pondrá un nombre. 

```mysql
CREATE TABLE expenses (
    id_expense INT AUTO_INCREMENT PRIMARY KEY,
    amount INT UNSIGNED NOT NULL,
    type ENUM('FIJO', 'VARIABLE') NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    user_id INT NOT NULL,
    group_id INT NOT NULL,
    
    CONSTRAINT fk_expense_user ← nombre 
        FOREIGN KEY (user_id) REFERENCES users(id_user),

    CONSTRAINT fk_expense_group
        FOREIGN KEY (group_id) REFERENCES groups(id_group)
);
```

Para borrar una columna: 
```MySql
ALTER TABLE productos DROP COLUMN descripcion;
```

Para agregar una columna:
```MySql
ALTER TABLE nombre_tabla
ADD COLUMN nombre_columna tipo_de_dato;
```

Para reiniciar una DB a su estado inicial: 
```mysql
SET FOREIGN_KEY_CHECKS = 0;


DELETE FROM tabla_1;
DELETE FROM tabla_2;
...

ALTER TABLE tabla_1 AUTO_INCREMENT = 1;
ALTER TABLE tabla_2 AUTO_INCREMENT = 1;

SET FOREIGN_KEY_CHECKS = 1;
```



### Pedir información de las DB, tablas
```MySql
SHOW DATABASES;
SHOW TABLES;
DESCRIBE nombre_tabla;
SHOW COLUMNS FROM nombre_tabla;

## pedir ver el codigo sql de una tabla por ejemplo: 
SHOW CREATE TABLE nombre_tabla;

## ver las claves foreaneas
SHOW CREATE TABLE nombre_tabla\G;


```


##  *DML (Data Manipulation Language)(CRUD)*
```MYSQL
SELECT
INSERT
UPDATE
DELETE
```

Agregar filas a las tablas, forma genérica: 
```MySql
INSERT INTO tabla (attr1, attr2, attr3, attr4) VALUES (valor1, valor, valor3,valor4)
```

Caso real: 
```MySql
INSERT INTO productos (nombre, precio, stock) 
VALUES ("fideos", 500.5, 100)
--  el id no se pone porque es automático AUTO_INCREMENT
```

El **INSERT INTO** es para agregar listas de cosas dentro de las tablas. 

```MySql
INSERT INTO productos (nombre, precio, stock) 
VALUES 
("fideos", 500.5, 100),
("arroz", 236.5, 200),
("leche", 1020 50),
("manteca", 685.5, 25);
-- se cierra con un ;
```

**UPDATE** para modificar valores. 
```MySql
UPDATE nombre_tabla
SET columna = valor_nuevo, columna2 = valor_nuevo2
WHERE condicion;

UPDATE productos 
SET precio = 1100,
WHERE id = 1
-- o 
WHERE nombre = "Arroz"
-- ... 
```
Muy importante el **WHERE** para no hacer cagada.

**DELETE**
```MySql
DELETE FROM nombre_tabla
WHERE condicion;

DELETE FROM productos
WHERE precio > 2500;

DELETE FROM productos
WHERE nombre = "Arroz";
```

**SELECT**
```MySql
SELECT * 
FROM tabla
WHERE condicion
ORDER BY columna ASC / DEC;

SELECT *
FROM productos 
WHERE id > 20
ORDER BY precio ASC

SELECT nombre
FROM productos 
WHERE stock < 20
ORDER BY stock ASC
```

##### LIKE y %  → Comodín o Wildcard
Esta query trae todos los productos que en su columna nombre tengan **exactamente** "aceite".
```Mysql
SELECT * 
FROM productos
WHERE nombre
LIKE "aceite"
```

Esta query trae todos los productos cuyo nombre comience(1) o termine(2) con "aceite"
```Mysql
(1)
SELECT * 
FROM productos
WHERE nombre
LIKE "aceite%"
(2)
...
LIKE "%aceite"
```

Esta query trae todos los productos cuyo nombre contenga "ace", este caso funciona como el método .Contains() de los strings en lenguajes de programación. 
```Mysql
SELECT * 
FROM productos
WHERE nombre
LIKE "%ace%"
```
#### Funciones de agregación
```MySql
-- El COUNT es god, te devuelve la cantidad de algo que le pidas
=> => COUNT(columna) <= <= 

SELECT
    COUNT(*)
AS              <= AS como. Le da un nick, como una variable.
	cant_prod
FROM
    productos
WHERE
    id > 15
```

```MySql
SELECT SUM(stock) FROM productos;
```
Devuelve la suma de todos los valores de stock de todos los productos, es decir que el total de productos que tengo en stock son 1350, mezclado entre todos. 

| SUM(stock) |
| ---------: |
|       1350 |
```MySql
SELECT
    SUM(stock)
FROM
    productos AS sum_stock_min100
WHERE
    stock >= 100
```

**MIN / MAX** 

```MySql
SELECT
    MAX(precio)
FROM
    productos
-- devuelve el valor del precio mas alto

SELECT
    MIN(stock)
FROM
    productos
-- devuelve el valor del stock mas pequeño

SELECT
    MIN(nombre)
FROM
    productos;
-- devuelve el primer nombre ALFABETICAMENTE ("Aceite")

SELECT
    MAX(nombre)
FROM
    productos;
-- devuelve el último nombre ALFABETICAMENTE ("Yerba")
```

**AVG (Average)**: Calcula el promedio. ( ∑n/n )

```MySql 
SELECT 
	AVG (columna)
FROM 
	tabla
WHERE 
	condicion
-- 

SELECT 
	AVG (precio)
FROM 
	productos
WHERE 
	precio >= 1000;
-- calcula el promedio de los precios mayores o iguales a 1000. 
```

**GROUP BY** : 
```mYsQL
SELECT 
	columna
FROM
	tabla
GROUP BY
	columna
-- 

SELECT nombre, AVG(precio)
FROM productos
GROUP BY nombre;
-- traeme todos los productos, y de los que estén repetidos traeme el promedio del precio. 
```

| nombre | AVG(precio) |                        comentarios                        |
| -----: | ----------: | :-------------------------------------------------------: |
|    Pan |     830.000 | <= promedio de precio de todos los prod. con nombre "Pan" |
| Aceite |    1500.000 |  <= Aceite hay uno solo así que devuelve  solo su precio  |
Aunque el **GROUP BY** se use para filtrar repetidos, este debe ser usado para cosas mucho mas potentes, como por ejemplo *devolver un promedio, una suma, el valor máximo o mínimo, etc.* 
Si lo que quiero es **SOLAMENTE** descartar los repetidos de forma rápida y ligera, tengo este modificador para la cláusula SELECT llamado ==DISTINCT==: 

```MySql
SELECT 
	DISTINCT nombre
FROM 
	productos
```


**HAVING**: Se usa para agregarle una condición al **GROUP BY**. Se podría traducir como *"Teniendo en cuenta que..."*. 
**HAVING** y **GROUP BY** van siempre juntos, ya que HAVING es una condición **para** GROUP BY.

```MySQL
SELECT 
	funcion_agregada
FROM 
	tabla
GROUP BY 
	columna
HAVING 
	condicion

-- EJ: 
SELECT nombre, SUM(stock)
FROM productos
GROUP BY nombre
HAVING SUM(stock) > 100;
```

Esta petición (Query) se puede traducir como : *“Selecciona el **nombre** de cada producto y la **suma total de su stock**, **agrupando por nombre**, **pero solo mostrame aquellos productos cuya suma de stock sea mayor a 100 unidades.” * 
 o
"Agrupame a cada uno de sus productos por su nombre, sumá el stock total que tienen todos, pero solo mostrame los que en su sumatoria superen 100"*

|      nombre       | SUM(stock) |                comentario                 |
| :---------------: | :--------: | :---------------------------------------: |
|      Azucar       |    122     | SUM => 90 + 32 (Dos 'Azúcar' en la tabla) |
| Galletitas Dulces |    101     |                                           |
|        Pan        |    360     |              SUM => 40 + 320              |
|     Sal final     |    120     |                                           |
También se puede agregar un **WHERE** pero afectaría solamente al **SELECT**, es previo al **HAVING**. 
```MySql
SELECT nombre, SUM(stock)
FROM productos
WHERE stock > 100
GROUP BY nombre
HAVING SUM(stock) > 100;
``` 

|            nombre | SUM(stock) |
| ----------------: | ---------: |
|            Azucar |        122 |
| Galletitas Dulces |        101 |
|               Pan |        360 |
|         Sal final |        120 |
**LIMIT**: Es para limitar la cantidad de resultados obtenidos, un limitador. Si tu tabla tiene muchos datos y vas a hacer una petición con muchas respuestas, y solamente querés los 2 primeros resultados metés un LIMIT 2 y ya 
```SQL
SELECT * FROM productos LIMIT 5;
```
**IS NULL / IS NOT NULL**
Te devuelve una lista con los elementos que en su columna *id_departamento*  tienen (o no) valor NULL.
```MySQL
SELECT * FROM productos
WHERE id_departamento IS NULL;

SELECT * FROM productos
WHERE id_departamento IS NOT NULL;
```
 **AUNQUE, en SQL NULL es la ausencia de valor, no un valor en si mismo por eso no se puede hacer una comparación como por ejemplo:** 
```Sql
SELECT * FROM productos
WHERE id_departamento = NULL;
-- Esto no es válido  ↑ ↑ ↑ ↑
```
### JOINS
#### INNER JOIN
Solo muestra los registros que coinciden tanto de la tabla A como de la tabla B.

##### **Forma explícita**
```MySQL
SELECT * 
FROM empleados 
INNER JOIN departamentos
ON empleados.id_departamento = departamentos.id_departamento
LIMIT 5
-- Hay más
```

##### **Forma implícita**
```MySQL
SELECT * 
FROM empleados, departamentos
WHERE empleados.id_departamento = departamentos.id_departamento
LIMIT 5
-- Hay más
```
Esto devuelve las dos tablas unidas, conectadas por el valor de **empleados.id_departamento y departamentos.id_departamento**. Esto, semánticamente, sería como decir : *Traéme ==la unión== de las dos tablas donde el id del departamento en el que está trabajando coincide con un departamento*. 

| [id_empleado](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=empleados&sql_query=SELECT+%2A+%0D%0AFROM+empleados+%0D%0AINNER+JOIN+departamentos%0D%0AON+empleados.id_departamento+%3D+departamentos.id_departamento++%0AORDER+BY+%60empleados%60.%60id_empleado%60+ASC&sql_signature=91052ba5e37893aa7d0ff33e6ed7fa8c550e2dcfaa3ac873ba1676f3343aa8a6&session_max_rows=25&is_browse_distinct=0) | [dni](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=empleados&sql_query=SELECT+%2A+%0D%0AFROM+empleados+%0D%0AINNER+JOIN+departamentos%0D%0AON+empleados.id_departamento+%3D+departamentos.id_departamento++%0AORDER+BY+%60empleados%60.%60dni%60+ASC&sql_signature=b39f0b980ed8e1f558c04e2c492554ec8bee2ea94bd8cb9ef73ad3b4325d9479&session_max_rows=25&is_browse_distinct=0) | [nombre](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=empleados&sql_query=SELECT+%2A+%0D%0AFROM+empleados+%0D%0AINNER+JOIN+departamentos%0D%0AON+empleados.id_departamento+%3D+departamentos.id_departamento++%0AORDER+BY+%60empleados%60.%60nombre%60+ASC&sql_signature=103e54349199c8aef4c20839990bf932b5afc4bf28823f22ddb2a40f7a06bd55&session_max_rows=25&is_browse_distinct=0) | [apellido](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=empleados&sql_query=SELECT+%2A+%0D%0AFROM+empleados+%0D%0AINNER+JOIN+departamentos%0D%0AON+empleados.id_departamento+%3D+departamentos.id_departamento++%0AORDER+BY+%60empleados%60.%60apellido%60+ASC&sql_signature=987e67f710704afbd1bf1e7cfda321602606bd31a4a79cfdb7d9774219db35ff&session_max_rows=25&is_browse_distinct=0) | ==[id_departamento](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=empleados&sql_query=SELECT+%2A+%0D%0AFROM+empleados+%0D%0AINNER+JOIN+departamentos%0D%0AON+empleados.id_departamento+%3D+departamentos.id_departamento++%0AORDER+BY+%60empleados%60.%60id_departamento%60+ASC&sql_signature=fca512ee207d33ff3cc68da21c9b22cceb820f761ea7434e0135ccf543f04b04&session_max_rows=25&is_browse_distinct=0)== | ==[id_departamento](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=empleados&sql_query=SELECT+%2A+%0D%0AFROM+empleados+%0D%0AINNER+JOIN+departamentos%0D%0AON+empleados.id_departamento+%3D+departamentos.id_departamento++%0AORDER+BY+%60departamentos%60.%60id_departamento%60+ASC&sql_signature=aabdf8aaa8ee3fa8b1cae3134d9b7113285fc623bd866fe63d7db0bdbc8742ba&session_max_rows=25&is_browse_distinct=0)== | [nombre](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=empleados&sql_query=SELECT+%2A+%0D%0AFROM+empleados+%0D%0AINNER+JOIN+departamentos%0D%0AON+empleados.id_departamento+%3D+departamentos.id_departamento++%0AORDER+BY+%60departamentos%60.%60nombre%60+ASC&sql_signature=963154cb5194b36c2df9536dfd3440835988bf773d4a4c362ac6159a8b9e6f29&session_max_rows=25&is_browse_distinct=0) | [edificio](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=empleados&sql_query=SELECT+%2A+%0D%0AFROM+empleados+%0D%0AINNER+JOIN+departamentos%0D%0AON+empleados.id_departamento+%3D+departamentos.id_departamento++%0AORDER+BY+%60departamentos%60.%60edificio%60+ASC&sql_signature=5b963677dae8c2e38d7a91a8f183c2dc941954117ac308012c52a96c7953e7c8&session_max_rows=25&is_browse_distinct=0) | [piso](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=empleados&sql_query=SELECT+%2A+%0D%0AFROM+empleados+%0D%0AINNER+JOIN+departamentos%0D%0AON+empleados.id_departamento+%3D+departamentos.id_departamento++%0AORDER+BY+%60departamentos%60.%60piso%60+ASC&sql_signature=9e1b54f2f2ecaddf627c1cdd6d3bf9308db2184ee9caebcc58438afa5ce37e98&session_max_rows=25&is_browse_distinct=0) |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 14                                                                                                                                                                                                                                                                                                                                                                                                                    | 36947258                                                                                                                                                                                                                                                                                                                                                                                              | Lautaro                                                                                                                                                                                                                                                                                                                                                                                                     | Álvarez                                                                                                                                                                                                                                                                                                                                                                                                         | ==[1](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=departamentos&pos=0&sql_signature=a26e1dafcdadfefc021e9458626a9a25acf9dac3508368034a2c71d05f897baa&sql_query=SELECT+%2A+FROM+%60joins%60.%60departamentos%60+WHERE+%60id_departamento%60+%3D+1 "Recursos Humanos")==                                                                                                                                   | ==[1](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=departamentos&pos=0&sql_signature=a26e1dafcdadfefc021e9458626a9a25acf9dac3508368034a2c71d05f897baa&sql_query=SELECT+%2A+FROM+%60joins%60.%60departamentos%60+WHERE+%60id_departamento%60+%3D+1 "Recursos Humanos")==                                                                                                                                       | Recursos Humanos                                                                                                                                                                                                                                                                                                                                                                                                | Edificio A                                                                                                                                                                                                                                                                                                                                                                                                          | 1                                                                                                                                                                                                                                                                                                                                                                                                           |
| 3                                                                                                                                                                                                                                                                                                                                                                                                                     | 39586741                                                                                                                                                                                                                                                                                                                                                                                              | Julián                                                                                                                                                                                                                                                                                                                                                                                                      | Gómez                                                                                                                                                                                                                                                                                                                                                                                                           | ==[2](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=departamentos&pos=0&sql_signature=59f67c6461933406a701bd3c902115c5e971c6d17b3085176545b23125374768&sql_query=SELECT+%2A+FROM+%60joins%60.%60departamentos%60+WHERE+%60id_departamento%60+%3D+2 "Marketing")==                                                                                                                                          | ==[2](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=departamentos&pos=0&sql_signature=59f67c6461933406a701bd3c902115c5e971c6d17b3085176545b23125374768&sql_query=SELECT+%2A+FROM+%60joins%60.%60departamentos%60+WHERE+%60id_departamento%60+%3D+2 "Marketing")==                                                                                                                                              | Marketing                                                                                                                                                                                                                                                                                                                                                                                                       | Edificio B                                                                                                                                                                                                                                                                                                                                                                                                          | 3                                                                                                                                                                                                                                                                                                                                                                                                           |
| 11                                                                                                                                                                                                                                                                                                                                                                                                                    | 36289140                                                                                                                                                                                                                                                                                                                                                                                              | Emma                                                                                                                                                                                                                                                                                                                                                                                                        | Romero                                                                                                                                                                                                                                                                                                                                                                                                          | ==[2](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=departamentos&pos=0&sql_signature=59f67c6461933406a701bd3c902115c5e971c6d17b3085176545b23125374768&sql_query=SELECT+%2A+FROM+%60joins%60.%60departamentos%60+WHERE+%60id_departamento%60+%3D+2 "Marketing")==                                                                                                                                          | ==[2](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=departamentos&pos=0&sql_signature=59f67c6461933406a701bd3c902115c5e971c6d17b3085176545b23125374768&sql_query=SELECT+%2A+FROM+%60joins%60.%60departamentos%60+WHERE+%60id_departamento%60+%3D+2 "Marketing")==                                                                                                                                              | Marketing                                                                                                                                                                                                                                                                                                                                                                                                       | Edificio B                                                                                                                                                                                                                                                                                                                                                                                                          | 3                                                                                                                                                                                                                                                                                                                                                                                                           |
| 1                                                                                                                                                                                                                                                                                                                                                                                                                     | 36457899                                                                                                                                                                                                                                                                                                                                                                                              | Mirko                                                                                                                                                                                                                                                                                                                                                                                                       | Wiebe                                                                                                                                                                                                                                                                                                                                                                                                           | ==[3](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=departamentos&pos=0&sql_signature=ed33b7a6ab844f2d8f27db2a83dffe7ac80ff72e4fc46504fe2462d07e500104&sql_query=SELECT+%2A+FROM+%60joins%60.%60departamentos%60+WHERE+%60id_departamento%60+%3D+3 "Finanzas")==                                                                                                                                           | ==[3](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=departamentos&pos=0&sql_signature=ed33b7a6ab844f2d8f27db2a83dffe7ac80ff72e4fc46504fe2462d07e500104&sql_query=SELECT+%2A+FROM+%60joins%60.%60departamentos%60+WHERE+%60id_departamento%60+%3D+3 "Finanzas")==                                                                                                                                               | Finanzas                                                                                                                                                                                                                                                                                                                                                                                                        | Edificio C                                                                                                                                                                                                                                                                                                                                                                                                          | 2                                                                                                                                                                                                                                                                                                                                                                                                           |
| 6                                                                                                                                                                                                                                                                                                                                                                                                                     | 35984102                                                                                                                                                                                                                                                                                                                                                                                              | Mateo                                                                                                                                                                                                                                                                                                                                                                                                       | López                                                                                                                                                                                                                                                                                                                                                                                                           | ==[4](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=departamentos&pos=0&sql_signature=2df40eff120cadbdd162e6bfde11bcb6ae3f3c9f7444d0ed4f27fac245289b3f&sql_query=SELECT+%2A+FROM+%60joins%60.%60departamentos%60+WHERE+%60id_departamento%60+%3D+4 "IT")==                                                                                                                                                 | ==[4](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=departamentos&pos=0&sql_signature=2df40eff120cadbdd162e6bfde11bcb6ae3f3c9f7444d0ed4f27fac245289b3f&sql_query=SELECT+%2A+FROM+%60joins%60.%60departamentos%60+WHERE+%60id_departamento%60+%3D+4 "IT")==                                                                                                                                                     | IT                                                                                                                                                                                                                                                                                                                                                                                                              | Edificio D                                                                                                                                                                                                                                                                                                                                                                                                          | 4                                                                                                                                                                                                                                                                                                                                                                                                           |


#### LEFT JOIN
Muestra el conjunto completo de los registros de la tabla A, con los registros ==que coincidan== de la tabla B (Si es que los hay).
```MYSQL
SELECT * 
FROM empleados
LEFT JOIN departamentos
ON empleados.id_departamento = departamentos.id_departamento
LIMIT 5
```

| [id_empleado](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=empleados&sql_query=SELECT+%2A+%0D%0AFROM+empleados+%0D%0AINNER+JOIN+departamentos%0D%0AON+empleados.id_departamento+%3D+departamentos.id_departamento++%0AORDER+BY+%60empleados%60.%60id_empleado%60+ASC&sql_signature=91052ba5e37893aa7d0ff33e6ed7fa8c550e2dcfaa3ac873ba1676f3343aa8a6&session_max_rows=25&is_browse_distinct=0) | [dni](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=empleados&sql_query=SELECT+%2A+%0D%0AFROM+empleados+%0D%0AINNER+JOIN+departamentos%0D%0AON+empleados.id_departamento+%3D+departamentos.id_departamento++%0AORDER+BY+%60empleados%60.%60dni%60+ASC&sql_signature=b39f0b980ed8e1f558c04e2c492554ec8bee2ea94bd8cb9ef73ad3b4325d9479&session_max_rows=25&is_browse_distinct=0) | [nombre](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=empleados&sql_query=SELECT+%2A+%0D%0AFROM+empleados+%0D%0AINNER+JOIN+departamentos%0D%0AON+empleados.id_departamento+%3D+departamentos.id_departamento++%0AORDER+BY+%60empleados%60.%60nombre%60+ASC&sql_signature=103e54349199c8aef4c20839990bf932b5afc4bf28823f22ddb2a40f7a06bd55&session_max_rows=25&is_browse_distinct=0) | [apellido](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=empleados&sql_query=SELECT+%2A+%0D%0AFROM+empleados+%0D%0AINNER+JOIN+departamentos%0D%0AON+empleados.id_departamento+%3D+departamentos.id_departamento++%0AORDER+BY+%60empleados%60.%60apellido%60+ASC&sql_signature=987e67f710704afbd1bf1e7cfda321602606bd31a4a79cfdb7d9774219db35ff&session_max_rows=25&is_browse_distinct=0) | ==[id_departamento](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=empleados&sql_query=SELECT+%2A+%0D%0AFROM+empleados+%0D%0AINNER+JOIN+departamentos%0D%0AON+empleados.id_departamento+%3D+departamentos.id_departamento++%0AORDER+BY+%60empleados%60.%60id_departamento%60+ASC&sql_signature=fca512ee207d33ff3cc68da21c9b22cceb820f761ea7434e0135ccf543f04b04&session_max_rows=25&is_browse_distinct=0)== | ==[id_departamento](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=empleados&sql_query=SELECT+%2A+%0D%0AFROM+empleados+%0D%0AINNER+JOIN+departamentos%0D%0AON+empleados.id_departamento+%3D+departamentos.id_departamento++%0AORDER+BY+%60departamentos%60.%60id_departamento%60+ASC&sql_signature=aabdf8aaa8ee3fa8b1cae3134d9b7113285fc623bd866fe63d7db0bdbc8742ba&session_max_rows=25&is_browse_distinct=0)== | [nombre](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=empleados&sql_query=SELECT+%2A+%0D%0AFROM+empleados+%0D%0AINNER+JOIN+departamentos%0D%0AON+empleados.id_departamento+%3D+departamentos.id_departamento++%0AORDER+BY+%60departamentos%60.%60nombre%60+ASC&sql_signature=963154cb5194b36c2df9536dfd3440835988bf773d4a4c362ac6159a8b9e6f29&session_max_rows=25&is_browse_distinct=0) | [edificio](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=empleados&sql_query=SELECT+%2A+%0D%0AFROM+empleados+%0D%0AINNER+JOIN+departamentos%0D%0AON+empleados.id_departamento+%3D+departamentos.id_departamento++%0AORDER+BY+%60departamentos%60.%60edificio%60+ASC&sql_signature=5b963677dae8c2e38d7a91a8f183c2dc941954117ac308012c52a96c7953e7c8&session_max_rows=25&is_browse_distinct=0) | [piso](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=empleados&sql_query=SELECT+%2A+%0D%0AFROM+empleados+%0D%0AINNER+JOIN+departamentos%0D%0AON+empleados.id_departamento+%3D+departamentos.id_departamento++%0AORDER+BY+%60departamentos%60.%60piso%60+ASC&sql_signature=9e1b54f2f2ecaddf627c1cdd6d3bf9308db2184ee9caebcc58438afa5ce37e98&session_max_rows=25&is_browse_distinct=0) |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 14                                                                                                                                                                                                                                                                                                                                                                                                                    | 36947258                                                                                                                                                                                                                                                                                                                                                                                              | Lautaro                                                                                                                                                                                                                                                                                                                                                                                                     | Álvarez                                                                                                                                                                                                                                                                                                                                                                                                         | ==[1](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=departamentos&pos=0&sql_signature=a26e1dafcdadfefc021e9458626a9a25acf9dac3508368034a2c71d05f897baa&sql_query=SELECT+%2A+FROM+%60joins%60.%60departamentos%60+WHERE+%60id_departamento%60+%3D+1 "Recursos Humanos")==                                                                                                                                   | ==[1](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=departamentos&pos=0&sql_signature=a26e1dafcdadfefc021e9458626a9a25acf9dac3508368034a2c71d05f897baa&sql_query=SELECT+%2A+FROM+%60joins%60.%60departamentos%60+WHERE+%60id_departamento%60+%3D+1 "Recursos Humanos")==                                                                                                                                       | Recursos Humanos                                                                                                                                                                                                                                                                                                                                                                                                | Edificio A                                                                                                                                                                                                                                                                                                                                                                                                          | 1                                                                                                                                                                                                                                                                                                                                                                                                           |
| 3                                                                                                                                                                                                                                                                                                                                                                                                                     | 39586741                                                                                                                                                                                                                                                                                                                                                                                              | Julián                                                                                                                                                                                                                                                                                                                                                                                                      | Gómez                                                                                                                                                                                                                                                                                                                                                                                                           | ==[2](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=departamentos&pos=0&sql_signature=59f67c6461933406a701bd3c902115c5e971c6d17b3085176545b23125374768&sql_query=SELECT+%2A+FROM+%60joins%60.%60departamentos%60+WHERE+%60id_departamento%60+%3D+2 "Marketing")==                                                                                                                                          | ==[2](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=departamentos&pos=0&sql_signature=59f67c6461933406a701bd3c902115c5e971c6d17b3085176545b23125374768&sql_query=SELECT+%2A+FROM+%60joins%60.%60departamentos%60+WHERE+%60id_departamento%60+%3D+2 "Marketing")==                                                                                                                                              | Marketing                                                                                                                                                                                                                                                                                                                                                                                                       | Edificio B                                                                                                                                                                                                                                                                                                                                                                                                          | 3                                                                                                                                                                                                                                                                                                                                                                                                           |
| 1                                                                                                                                                                                                                                                                                                                                                                                                                     | 36457899                                                                                                                                                                                                                                                                                                                                                                                              | Mirko                                                                                                                                                                                                                                                                                                                                                                                                       | Wiebe                                                                                                                                                                                                                                                                                                                                                                                                           | ==[3](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=departamentos&pos=0&sql_signature=ed33b7a6ab844f2d8f27db2a83dffe7ac80ff72e4fc46504fe2462d07e500104&sql_query=SELECT+%2A+FROM+%60joins%60.%60departamentos%60+WHERE+%60id_departamento%60+%3D+3 "Finanzas")==                                                                                                                                           | ==[3](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=departamentos&pos=0&sql_signature=ed33b7a6ab844f2d8f27db2a83dffe7ac80ff72e4fc46504fe2462d07e500104&sql_query=SELECT+%2A+FROM+%60joins%60.%60departamentos%60+WHERE+%60id_departamento%60+%3D+3 "Finanzas")==                                                                                                                                               | Finanzas                                                                                                                                                                                                                                                                                                                                                                                                        | Edificio C                                                                                                                                                                                                                                                                                                                                                                                                          | 2                                                                                                                                                                                                                                                                                                                                                                                                           |
| 6                                                                                                                                                                                                                                                                                                                                                                                                                     | 35984102                                                                                                                                                                                                                                                                                                                                                                                              | Mateo                                                                                                                                                                                                                                                                                                                                                                                                       | López                                                                                                                                                                                                                                                                                                                                                                                                           | ==[4](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=departamentos&pos=0&sql_signature=2df40eff120cadbdd162e6bfde11bcb6ae3f3c9f7444d0ed4f27fac245289b3f&sql_query=SELECT+%2A+FROM+%60joins%60.%60departamentos%60+WHERE+%60id_departamento%60+%3D+4 "IT")==                                                                                                                                                 | ==[4](http://localhost:9090/phpmyadmin/index.php?route=/sql&db=joins&table=departamentos&pos=0&sql_signature=2df40eff120cadbdd162e6bfde11bcb6ae3f3c9f7444d0ed4f27fac245289b3f&sql_query=SELECT+%2A+FROM+%60joins%60.%60departamentos%60+WHERE+%60id_departamento%60+%3D+4 "IT")==                                                                                                                                                     | IT                                                                                                                                                                                                                                                                                                                                                                                                              | Edificio D                                                                                                                                                                                                                                                                                                                                                                                                          | 4                                                                                                                                                                                                                                                                                                                                                                                                           |
| 5                                                                                                                                                                                                                                                                                                                                                                                                                     | 36829485                                                                                                                                                                                                                                                                                                                                                                                              | Sofía                                                                                                                                                                                                                                                                                                                                                                                                       | Fernández                                                                                                                                                                                                                                                                                                                                                                                                       | NULL                                                                                                                                                                                                                                                                                                                                                                                                                              | NULL                                                                                                                                                                                                                                                                                                                                                                                                                                  | NULL                                                                                                                                                                                                                                                                                                                                                                                                            | NULL                                                                                                                                                                                                                                                                                                                                                                                                                |                                                                                                                                                                                                                                                                                                                                                                                                             |
#### RIGHT JOIN
Es exactamente lo mismo que el LEFT JOIN, pero trayendo el conjunto completo de elementos de la tabla B y los que coincidan de la tabla A.

#### FULL JOIN
Es un bardo, mezcla todo con todo. Une todo lo de la tabla A, con todo lo de la tabla B. Es decir que hace un quilombo terrible.

## Sub Consultas

Tienen diferentes niveles, son como IFs anidados por así decirlo. 

```mYSQL
SELECT * 
FROM empleados
WHERE sueldo >= 
		(SELECT AVG(sueldo) FROM empleados)
		-- En este caso está usando un solo valor AVG(sueldo) para la comparación
```
	Acá lo que hago es primero calcular el promedio de los sueldos (AVG) luego, pido traer todos los empleados donde su sueldo sea mayor o igual a ese promedio.


Acá muestro la cantidad de empleados que hay por departamento, *la clave está al final en el **GROUP BY***.
```MySql
SELECT  id_departamento, COUNT(*) AS cantidad_empleados
FROM empleados
WHERE id_departamento IS NOT NULL
GROUP BY id_departamento
```


### Consulta abstracta
Le asignamos parámetros con @nombreColumna

```MySql
INSERT INTO productos (nombre, precio, stock) 
VALUES 
(@nombre, @precio, @stock);
```

# Usuarios y privilegios
## Creando usuarios y dándole privilegios
```mysql
## creando usuario
CREATE USER 'app_user'@'%' IDENTIFIED BY 'password_segura';

## dando privilegios
GRANT SELECT, INSERT, UPDATE, DELETE
ON mi_app_db.*
TO 'app_user'@'%';

FLUSH PRIVILEGES;
```

### Ver privilegios
```MYSQL
SHOW GRANTS FROM 'user'@'host';
```

### Revocar quitar privilegios
```mysql
## Puntuales
REVOKE INSERT, UPDATE ON midb.* FROM 'user'@'localhost';

## Todos
REVOKE ALL PRIVILEGES ON midb.* FROM 'user'@'localhost';
```
### Creo un archivo de configuracion para evitar escribir las ocntraseñas manualmente

- esto encripta la contraseña y al guarda en el servidor.

```bash
## verifico si existe mysql_config_editor:
mysql_config_editor print --all

## si quiero borrarlo:
mysql_config_editor remove --login-path=localroot

## si quiero crearlo: 
mysql_config_editor set \
--login-path=localroot \
--user=root \
--host=localhost \
--password

## Te pide el password y listo.

## Finalmente se accede al usuario MySql asi :
mysql --login-path=localroot # localroot : usuario cargado en mysql_config_editor

```

### Ver usuarios :

```mysql 
SELECT user, host
FROM mysql.user
ORDER BY user, host;

SELECT USER(), CURRENT_USER();
```


## Creando usaurio admin (prvileges, config_editor)

```mysql
# Creamos el usuario
CREATE USER 'admin'@'localhost'
IDENTIFIED WITH mysql_native_password
BY 'PasswordFuerteAqui!';

# Damos privilegios y aplicamos cambios
GRANT ALL PRIVILEGES ON *.* 
TO 'dbadmin'@'localhost'
WITH GRANT OPTION;
FLUSH PRIVILEGES;

# verificamos si se creo correctamente 
SELECT user, host, plugin
FROM mysql.user
WHERE user = 'admin';

# Hecho esto salimso de MySQL
EXIT
```
	
	↓↓ sigue en bash ↓↓
	
```bash

# probamos el login del nuevo usuario
mysql -u admin -p

# Configuramos el archivo `mysql_config_editor`
# Creamos login path

mysql_config_editor set \
--login-path=dbadmin \
--user=dbadmin \
--host=localhost \
--password

# La consola me pide la contraseña, se la damos y listo. Ahora verificamos.
mysql_config_editor print --all

# ACCEDEMOS :
mysql --login-path=admin

# Verificacion final dentro de mysql
SELECT USER(), CURRENT_USER();

```