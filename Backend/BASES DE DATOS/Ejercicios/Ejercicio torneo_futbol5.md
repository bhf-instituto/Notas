```MySql
```

```MySql
-- las creo primero sin las FOREIGN KEYS
CREATE TABLE categorias(
	id_categoria INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50),
    edad_min INT,
	edad_max INT,
    sexo ENUM("H", "M")
) ENGINE = InnoDB;

	CREATE TABLE participantes(
	id_participante INT AUTO_INCREMENT PRIMARY KEY,
    apellido VARCHAR(50),
    nombre VARCHAR(50),
    dni VARCHAR(15),
    edad INT,
    telefono INT,
    sexo ENUM("H", "M")
) ENGINE = InnoDB;

CREATE TABLE inscrripciones(
	num_inscripcion INT primary key,
    fecha_inscripcion DATE,
    abono BOOLEAN
);
```