```mysql
CREATE DATABASE libreria;

CREATE TABLE editoriales(
	id_editorial INT AUTO_INCREMENT PRIMARY KEY,
	nombre VARCHAR(35),
	pais_origen VARCHAR(35)
)


CREATE TABLE libros(
	id_libro INT AUTO_INCREMENT PRIMARY KEY,
	titulo VARCHAR(50),
	precio INT,
	id_editorial INT,
	FOREIGN KEY (id_editorial) REFERENCES editoriales(id_editorial)
)


INSERT INTO editoriales (nombre, pais_origen) VALUES
('Penguin Random House', 'Estados Unidos'),
('Planeta', 'España'),
('HarperCollins', 'Reino Unido'),
('Alfaguara', 'España'),
('Simon & Schuster', 'Estados Unidos'),
('Anagrama', 'España');

INSERT INTO libros (titulo, precio, id_editorial) VALUES
('El viaje de los sueños', 1800, 2),
('Historias del abismo', 2200, 4),
('La sombra eterna', 1950, 1),
('Reflejos del alma', 2500, 3),
('Código infinito', 1700, 5),
('Versos de invierno', 2100, 6),
('Laberintos del tiempo', 2300, 1),
('La melodía oculta', 1850, 2),
('Caminos de fuego', 2750, 6),
('Secretos del bosque', 2600, 4),
('El último mensaje', 2000, 1),
('Noche en el faro', 1900, 5),
('Ritual de sombras', 2200, 3),
('El mapa olvidado', 1700, 2),
('Diario de un mago', 2400, 3),
('Piedras del destino', 2050, 6),
('Eco de silencios', 1800, 1),
('La torre del saber', 2500, 4),
('Círculo cerrado', 2100, 5),
('Travesía a lo desconocido', 2650, 2),
('Bruma del pasado', 1950, 6),
('Luz entre ruinas', 1850, 3),
('Fragmentos de hielo', 2000, 1),
('El legado secreto', 2350, 4),
('Tierras de cristal', 2250, 2),
('Pasaje sin retorno', 1900, 6),
('Voces del silencio', 1800, 5),
('Los guardianes', 2600, 3),
('La séptima llave', 2000, 1),
('Sombras del atardecer', 2500, 4);

SELECT titulo, precio 
FROM libros 
ORDER BY precio ASC


SELECT * 
FROM editoriales 
ORDER BY nombre ASC

SELECT *
FROM libros
WHERE precio >= 1900

SELECT titulo, precio
FROM libros
ORDER BY precio ASC
LIMIT 1


```