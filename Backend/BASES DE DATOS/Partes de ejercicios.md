
```MySql
```

```MySql
UPDATE
    productos
SET
    stock = 5
WHERE
    id = 3 OR id = 5 OR id = 15;

-- SELECT * FROM productos ;

SELECT
    nombre,
    precio
FROM
    productos
WHERE
    stock < 20
ORDER BY
    nombre ASC

-- traeme el nombre, id y la sumatoria total del stock de la agrupacion (por nombre). Pero solamente traeme a los que en su sumatoria de stock superen 100. Finalmente ordenalos de manera descendente.

SELECT nombre, id, SUM(stock)
FROM productos 
GROUP BY nombre
HAVING SUM(stock) > 100
ORDER BY id DESC
```

#### Generar número random
```MySql
SELECT FLOOR(50 + (RAND() * (300 - 50)));
FLOOR(500000 + (RAND() * (3000000 - 500000)));
-- De 50 a 300, entero, por eso el FLOOR() que redondea.
```
### lista personas
```SQL
INSERT INTO 
	empleados (dni, nombre, apellido, id_departamento) 
VALUES
("36457899", "Mirko", "Wiebe", 3),
("38246902", "Lucía", "Pérez", 7),
("39586741", "Julián", "Gómez", 2),
("37745126", "Valentina", "Martínez", 5),
("36829485", "Sofía", "Fernández", 1),
("35984102", "Mateo", "López", 4),
("38452013", "Agustina", "García", 6),
("37381496", "Tomás", "Sánchez", 9),
("36109573", "Martina", "Díaz", 8),
("39028461", "Benjamín", "Rodríguez", 10),
("36289140", "Emma", "Romero", 2),
("38670145", "Thiago", "Torres", 4),
("37892051", "Camila", "Silva", 6),
("36947258", "Lautaro", "Álvarez", 1),
("38741963", "Mía", "Flores", 3),
("37581294", "Francisco", "Ruiz", 5),
("39184725", "Isabella", "Acosta", 7),
("36572840", "Dylan", "Molina", 8),
("37059634", "Catalina", "Ortiz", 9),
("38921476", "Bautista", "Ramos", 10);
```

