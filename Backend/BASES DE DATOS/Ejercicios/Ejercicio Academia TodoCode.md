```MySql
-- CREATE DATABASE academia;


CREATE TABLE idiomas (
	id_idioma INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50),
    nivel VARCHAR(15)
);

CREATE TABLE alumnos (
	id_alumno INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50),
    apellido VARCHAR(50),
    dni VARCHAR(15),
    id_idioma INT, 
    FOREIGN KEY (id_idioma) REFERENCES idiomas(id_idioma)
);



-- Me devuelve todos los alumnos que esten anotados en la materia 2, en este caso es Inglés y que es la materia que estoy buscando
SELECT * 
FROM alumnos
WHERE id_idioma = 2;

-- pero no es correcto ya que eventualmente los IDs pueden cambiar, y el ID 2 podría estar ahora asignado a Francés por ejemplo. De esta forma nos aseguramos que siempre sea Inglés. Hacemos una union, y unimos a los alumnos con los idiomas donde el nombre del idioma (tabla idiomas) es "Inglés".
SELECT *
FROM alumnos
JOIN idiomas ON alumnos.id_idioma = idiomas.id_idioma
WHERE idiomas.nombre = "Inglés"

-- y de esta forma lo hacemos impicito, sin declarar el JOIN ON
SELECT *
FROM alumnos, idiomas
WHERE alumnos.id_idioma = idiomas.id_idioma
AND idiomas.nombre = "Inglés";
```