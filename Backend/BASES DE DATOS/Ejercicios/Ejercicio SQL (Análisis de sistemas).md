```MySql

CREATE DATABASE Ejercicio; 

USE ejerciciosistemas;

create table tipoCapacidad(
	id INT AUTO_INCREMENT PRIMARY KEY,
    descripcion VARCHAR(10) NOT NULL
);

 create table articulo(
id INT AUTO_INCREMENT NOT NULL PRIMARY KEY, 
codBarra INT NOT NULL, 
nombre VARCHAR(50) NOT NULL, 
id_tipoCapacidad INT ,
cantidad INT NOT NULL DEFAULT 0,
precio INT NOT NULL,
FOREIGN KEY (id_tipoCapacidad) REFERENCES tipoCapacidad(id_tipoCapacidad)
)
```