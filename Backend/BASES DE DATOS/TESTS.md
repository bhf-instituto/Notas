# pedidos_simples
```mysql
CREATE TABLE clientes(
	id_cliente INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50),
    direccion VARCHAR(100)
) ENGINE = INNODB;

CREATE TABLE hamburguesas(
	id_hamburguesa INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(25),
    ingredientes VARCHAR(255),
    precio INT
) ENGINE = INNODB;

CREATE TABLE pedidos(
	id_pedido INT AUTO_INCREMENT PRIMARY KEY,
    fecha DATETIME DEFAULT CURRENT_TIMESTAMP,
    id_cliente INT,
    FOREIGN KEY (id_cliente) REFERENCES clientes(id_cliente)
) ENGINE = INNODB;

CREATE TABLE detalle_pedido(
	id_detalle INT AUTO_INCREMENT PRIMARY KEY,
    id_pedido INT,
    id_hamburguesa INT,
    cantidad INT,
    
    FOREIGN KEY (id_pedido) REFERENCES pedidos(id_pedido),
    FOREIGN KEY (id_hamburguesa) REFERENCES hamburguesas(id_hamburguesa)
) ENGINE = INNODB;	

INSERT INTO hamburguesas (nombre, ingredientes, precio) VALUES
('Clásica', 'Carne, queso, lechuga, tomate, mayonesa', 2500),
('Doble Queso', 'Carne, doble queso cheddar, pepinillos, ketchup', 3000),
('Bacon BBQ', 'Carne, queso, panceta, cebolla crispy, salsa BBQ', 3200),
('Veggie', 'Hamburguesa de garbanzos, lechuga, tomate, palta', 2800),
('Picante', 'Carne, queso, jalapeños, cebolla morada, salsa picante', 3100);


-- creo un pedido, al cual solo le tengo que asociar el cliente, porque la id y la hora son automaticas(AUTO_INCREMENT y DEFAULT respectivamente). 
INSERT INTO pedidos (id_cliente) VALUES (1)

-- llamo nombre, direccion e id de pedid, realizado por todos los clientes. 
SELECT nombre, direccion , id_pedido
FROM pedidos, clientes
WHERE pedidos.id_cliente = clientes.id_cliente

-- pido saber cuantos pedidos hizo cada cliente, si es que hizo alguno.
SELECT nombre, COUNT(*) AS cant_pedidos
FROM pedidos, clientes
WHERE pedidos.id_cliente = clientes.id_cliente
GROUP BY clientes.id_cliente


-- Al pedido 2 (previamente creado) le agrego 3 hamburguesas 'Clasica'
-- Seguido pido ver todo lo que haya en detalles_pedido dondo la id_pedido sea 2 (osea lo que acabo de agregar).

INSERT INTO detalle_pedido(id_pedido, id_hamburguesa, cantidad) VALUES (2,1,3)
SELECT * from detalle_pedido WHERE id_pedido  = 2

-- Supongamos que quiero saber cuantas hamburguesas hay de cada una en el pedido 2 sin hacer uniones, toda esa informacion la puedo sacar de detalle_pedido
SELECT id_hamburguesa, SUM(cantidad)
from detalle_pedido
WHERE id_pedido  = 2 
GROUP BY id_hamburguesa
-- La magia está en el GROUP BY, eso separa ambas hamburguesas. 

```

En una base de datos tengo que modelar tablas para hacer pedidos de hamburguesas, basicamente el cliente hará un pedido. Ese pedido puede tener muchas hamburguesas, y un precio total que sale del valor de la hamburguesa, su tamaño y sus extras.

La hamburguesa tendra id_hamburguesa, tamaño, ingredientes, extras, comentario.

hago un comentario: todas las hamburguesas tendran ingredientes implicitos y explicitos, ya que todas las hamburguesas tienen pan, al menos 1 medallon de carne y cheddar.

tamaño: si es simple ,doble, triple o cuadruple, esto esta relacionado con los ingredientes implicitos, ya que la hamburguesa al ser doble, tendrá pan, 2 medallones y 2 cheddar, es como que este atributo multiplica la cantidad de medallones y chedda, no asi de pan el cual siempre será 1.

ingredientes: estos son los ingredientes explicitos, los ingredientes que sean propios de cada hamburguesa, por ejemplo un BIG MAC ademas del pan, medallones de carne y cheddar (implicitos) tiene salsa especial Big Mac, lechuga, pepinillos, cebolla. Estos 4 últimos son los ingredientes explicitos.


En una base de datos tengo que modelar tablas para hacer pedidos de hamburguesas, basicamente el cliente hará un pedido. Ese pedido puede tener muchas hamburguesas, y las hamburguesas tendran una cierta construccion, y un precio total que partirá de la suma de todos los elementos.

Tengo una idea de como se va armar el pedido, te paso a explicar la forma:

El cliente podrá crear un pedido al cual le irá agregando hamburguesas, estas hamburguesas tendran cierta construccion. Ya que podran ser

La hamburguesa tendra id_hamburguesa, tamaño, ingredientes, extras, comentario.

// hago un comentario: todas las hamburguesas tendran ingredientes implicitos y explicitos, ya que todas las hamburguesas tienen pan, al menos 1 medallon de carne y cheddar.

- tamaño: si es simple ,doble, triple o cuadruple, esto esta relacionado con los ingredientes implicitos, ya que la hamburguesa al ser doble, tendrá pan, 2 medallones y 2 cheddar, es como que este atributo multiplica la cantidad de medallones y chedda, no asi de pan el cual siempre será 1.

- ingredientes: estos son los ingredientes explicitos, los ingredientes que sean propios de cada hamburguesa, por ejemplo un BIG MAC ademas del pan, medallones de carne y cheddar (implicitos) tiene salsa especial Big Mac, lechuga, pepinillos, cebolla. Estos 4 últimos son los ingredientes explicitos. Estos ingredientes deben poder quitarse tambien, ya que alguien podría querer un Big mac sin cebolla, pero con todo lo demás.

- extras : estos vendran de otra tabla, y es para agregarle mas de cierto ingrediente extra, sin importar si la hamburguesa lo tiene como ingrediente explicito. Por ejemplo, el BIG MAC se podria pedir con extra pepinillos, y/o extra cebolla.
  

Necesito modelar la base de datos de un sistema de pedidos de hamburguesas, quiero concentrarme en la parte en la que el usuario arma el pedido de hamburguesas.

Ya que las hamburguesas van a tener un armado por defecto y el cliente podrá modificarlo, para esto pensé en un concepto de ingredientes implicitos, explicitos, y extras.

Te explico el concepto (hamburguesas):

- TODAS las hamburguesas tienen ingredientes base (implicitos) los cuales pueden ser pan, medallon de carne, cheddar (el medallon de carne siempre va con dos rodajas de cheddar, eso se toma como una sola cosa, es decir que los medallones SIEMRPE tienen dos rodajas de cheddar).

- Las hamburguesas pueden ser simples, dobles, triples y cuadruples. Por defecto todas son dobles.

- Cada hamburguesa tendrá ingredientes propios (explicitos), los cuales se pueden quitar si el usuario lo desea. Por ejemplo, un Big Mc tiene salsa especial Big Mac, lechuga, pepinillos y cebolla. (el pan, el medallon de carne y el cheddar está implicito en todas las hamburguesas). Y el cliente podría pedirlo sin cebolla y/o sin pepinillo.

- A cada hamburguesa se le podrá agregar extras, los cuales estarán predefinidos en otra tabla (cebolla, bacon, pepinillos, etc). Por ejemplo, alguien podría pedir una Big Mc con extra pepinillo. No hay cantidad en los extras, es decir los extras se tienen o no se tienen, no es que el usuario puede elegir x2, x3, x4 extra pepinillo, solo hay una opcion: o tiene extra pepinillo o no.

- Se puede asignar una cantidad de cuantas de estas hamburguesas quiero en el pedido actual.

(pedido)

- los pedidos pueden contener muchas hamburguesas.

- un cliente puede hacer varios pedidos.

En este sistema el cliente selecciona la hamburguesa, la modifica, elije la cantidad, la agrega a un carrito el cual es el pedido. Y puede seguir agregando y modificando obviamente.

Com ote dije al principio quier oque nos concentremeos en la base de datos de la parte en la que un cliente arma un pedido.


# TEST hamburguesa-ingredientes

```MYSQL
CREATE TABLE ingredientes(
	id_ingrediente INT AUTO_INCREMENT PRIMARY KEY NOT NULL,
    nombre VARCHAR(50)
);

create table hamburguesas_base
(
	id_hamburguesa INT AUTO_INCREMENT PRIMARY KEY NOT NULL,
    nombre VARCHAR(25)
);

INSERT INTO hamburguesas_base (nombre) 
VALUES 
	("Sudamericana"),
	("Keseso")


insert into ingredientes(nombre) VALUES ("bacon"), ("lechuga"), ("tomate"), ("cebolla"), ("pepinillo"), ("salsa Thousand");


INSERT INTO hamburguesa_ingredientes(id_hamburguesa, id_ingrediente) VALUES (2,1), (2,2), (1,1), (1,2);


-- devuelve una sola columna llamada KESESO en la cual solo muestra la lista de ingredientes de esa hamburguesa, depende del nombre, lo cual está bueno ya que si la id_hamburguesa de la "keseso" cambia, la consulta devolverá lo mismo
SELECT i.nombre AS keseso
FROM hamburguesas_base hb
JOIN hamburguesa_ingredientes hi
ON hb.id_hamburguesa = hi.id_hamburguesa
JOIN ingredientes i 
ON hi.id_ingrediente = i.id_ingrediente
WHERE hb.nombre = "Keseso";


INSERT INTO hamburguesas_base (nombre) VALUES ("shaker"), ("ineludible");



-- Agregamos clientes, pedidos, y detalle de pedidos para registrar cuantas hamburguesas de cada tipo hay en cada pedido.


CREATE TABLE clientes (
    id_cliente INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    direccion VARCHAR(150)
);

CREATE TABLE pedidos (
    id_pedido INT AUTO_INCREMENT PRIMARY KEY,
    id_cliente INT,
    fecha DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (id_cliente) REFERENCES clientes(id_cliente)
);

CREATE TABLE detalle_pedido (
    id_detalle INT AUTO_INCREMENT PRIMARY KEY,
    id_pedido INT,
    id_hamburguesa INT,
    cantidad INT DEFAULT 1,
    FOREIGN KEY (id_pedido) REFERENCES pedidos(id_pedido),
    FOREIGN KEY (id_hamburguesa) REFERENCES hamburguesas_base(id_hamburguesa)
);

INSERT INTO clientes(nombre, direccion) 
VALUES 
	("Mirko Wiebe", "Brasil 76"),
    ("Erick Gonzales", "Juan Manuel de Rosas 355")

-- Creamos un pedido hecho por el cliente 1 (Mirko)
INSERT into pedidos (id_cliente ) VALUES (1)

-- le agrego hamburguesas al pedido. "Al pedido 1, le agrego 5 hamburguesas 2 ("keseso")"

INSERT INTO detalle_pedido(id_pedido, id_hamburguesa, cantidad) 
VALUES
	(1, 2, 5),
    (1, 1, 2);

-- acá pregunto cuales hamburguesas y cuantas de cada una hay en todos los pedidos. 
SELECT p.id_pedido, dp.cantidad, hb.nombre
FROM pedidos p
JOIN detalle_pedido dp
ON p.id_pedido = dp.id_pedido
JOIN hamburguesas_base hb
ON dp.id_hamburguesa = hb.id_hamburguesa


INSERT INTO pedidos (id_cliente) VALUES (2)

insert into detalle_pedido (id_pedido, id_hamburguesa, cantidad)
VALUES 
	(2, 1,1),
    (2, 1,4),
    (2, 2,2);
    

-- pido la cantidad total de hamburguesas en todos los pedidos entre 1 y 2 xd
SELECT sum(cantidad)
FROM detalle_pedido dp
WHERE id_pedido BETWEEN 1 AND 2



```

