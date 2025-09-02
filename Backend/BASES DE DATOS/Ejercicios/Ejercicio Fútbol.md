```MySql
-- Esta DB arranca no muy bien planificada y hecha.

CREATE DATABASE futbol;
USE futbol;

CREATE TABLE hinchas (
	id_hincha INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL,
    apellido VARCHAR(50) NOT NULL,
    equipo_futbol VARCHAR(50) NOT NULL
);

CREATE TABLE equipos(
	id_equipo INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
	nombre_equipo VARCHAR(65),
    cantidad_socios INT,
    localidad VARCHAR(65),
    categoria VARCHAR(65)
);

INSERT INTO equipos_primera (nombre_equipo, cantidad_socios, localidad, categoria) 
VALUES
("River Plate", 330000, "Buenos Aires", "Primera División"),
("Boca Juniors", 315000, "Buenos Aires", "Primera División"),
("Independiente", 120000, "Avellaneda", "Primera División"),
("Racing Club", 105000, "Avellaneda", "Primera División"),
("San Lorenzo", 95000, "Buenos Aires", "Primera División");

INSERT INTO hinchas (nombre, apellido, equipo_futbol) 
VALUES 
("Marcelo", "Tinelli", "San Lorenzo"),
("Juan", "Pérez", "Boca Juniors"),
("Lucía", "Gómez", "River Plate"),
("Carlos", "Fernández", "Independiente"),
("Martina", "López", "Racing Club"),
("Santiago", "Rodríguez", "River Plate"),
("Julieta", "Martínez", "Boca Juniors"),
("Agustín", "Díaz", "San Lorenzo"),
("Camila", "Acosta", "Huracán"),
("Federico", "Morales", "Rosario Central"),
("Paula", "Silva", "Newell's Old Boys"),
("Leandro", "Sosa", "Estudiantes de La Plata"),
("Florencia", "Ramos", "Gimnasia y Esgrima La Plata"),
("Bruno", "Castro", "Independiente"),
("Valentina", "Vega", "Racing Club"),
("Matías", "Mendoza", "Talleres"),
("Carla", "Ruiz", "Banfield"),
("Nicolás", "Ibarra", "Lanús"),
("Sofía", "Navarro", "Boca Juniors"),
("Tomás", "Giménez", "River Plate");


-- todos los hinchas de Independiente
SELECT COUNT(*)
FROM hinchas
where hinchas.equipo_futbol = "Independiente"
-- >> 2

-- todos los hinchas que sean de un equipo de primera
SELECT *
FROM hinchas, equipos_primera
where hinchas.equipo_futbol = equipos_primera.nombre_equipo

-- o la cuenta de lo mismo, cuantos hinchas hay de equpos de primera. 
SELECT COUNT(*)
FROM hinchas, equipos_primera
where hinchas.equipo_futbol = equipos_primera.nombre_equipo
-- >> 11

-- acá cambio el valor de la columna equipo_futbol a "Estrella del Sur" donde el equipo_futbol sea "Independiente". Es decir, cambio todos los "Estrella..." por "Independiente".
UPDATE hinchas
SET equipo_futbol = "Estrella del Sur"
WHERE equipo_futbol = "Independiente"

-- Si ahora hago esto otra vez, el resultado cambia, porque hay menos hinchas de equipos que esten dentro de la tabla equipos_primera
SELECT COUNT(*)
FROM hinchas, equipos_primera
WHERE hinchas.equipo_futbol = equipos_primera.nombre_equipo
-- >> 9


-- Acá pido solo los datos de los hinchas de equipos de primera
select * 
FROM hinchas
WHERE equipo_futbol IN 
		(SELECT nombre_equipo FROM equipos_primera)

-- Arego Estrella del Sur a la tabla de primera division
INSERT INTO equipos_primera(nombre_equipo, cantidad_socios, localidad, categoria) 
VALUES ("Estrella del Sur", 2500, "Alejandro Korn", "Primera División");
```
