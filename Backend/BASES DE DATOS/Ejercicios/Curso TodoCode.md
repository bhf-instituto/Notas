## Ejercicio JOINS (departamentos, empleados)

```MySql
CREATE TABLE departamentos(
	id_departamento INT PRIMARY KEY,
    nombre VARCHAR(50),
    edificio VARCHAR(50),
    piso INT
);

CREATE TABLE empleados (
	id_empleado INT AUTO_INCREMENT PRIMARY KEY,
	dni VARCHAR(15),
	nombre VARCHAR(50),
	apellido VARCHAR(50),
	id_departamento INT,
	FOREIGN KEY (id_departamento) REFERENCES departamentos(id_departamento)
);
```
