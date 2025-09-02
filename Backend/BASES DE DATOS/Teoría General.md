**Base de datos:**  es el servidor, el lugar físico donde se encuentran los datos. Es el nivel más bajo de abstracción que hay.
**Sistema Gestor de Base de Datos (DBMS):**  Eso es el sistema con el que accedemos a la **DB**.

### Abstracción de datos: 
Es una representación lógica de las situaciones y objetos del mundo real. 
### Modelo de datos:
Proporciona los medios para conseguir una abstracción. 

## Modelo entidad-relación
Es un modelo conceptual que se utiliza para diseñar/dibujar una base de datos antes de crearla. 
- Usa entidades, atributos, y relaciones entre ellas. 
### Entidad

Representa una “cosa”, "objeto", o "concepto" del mundo real con existencia independiente, es decir, se diferencia únicamente de otro objeto o cosa, incluso siendo del mismo tipo, o una misma entidad.

Algunos ejemplos:

- Una persona: se diferencia de cualquier otra persona, incluso siendo gemelos.
- Un automóvil: aunque sean de la misma marca, el mismo modelo, etc, tendrán atributos diferentes, por ejemplo, el número de chasis.
- Una casa: aunque sea exactamente igual a otra, aún se diferenciará en su dirección.

Una entidad puede ser un objeto con existencia física como: una persona, un animal, una casa, etc. (entidad concreta); o un objeto con existencia conceptual como: un puesto de trabajo, una asignatura de clases, un nombre, etc. (entidad abstracta).

Una entidad está descrita y se representa por sus características o atributos. Por ejemplo, la entidad _Persona_ tiene como características: Nombre, Apellido, Género, Estatura, Peso, Fecha de nacimiento.
### Atributos

Los atributos son las características que definen o identifican a una entidad. Éstas pueden ser muchas, y el diseñador sólo utiliza o implementa las que considere más relevantes.

En un conjunto de entidades del mismo tipo, cada entidad tiene **valores** específicos asignados para cada uno de sus atributos, de esta forma, es posible su identificación unívoca.

Ejemplos:

A la colección de entidades «alumnos», con el siguiente conjunto de atributos en común, (id, nombre, edad, semestre), pertenecen las entidades:

- (1, Sofia, 15 años, 2)
- (2, Josefa, 19 años, 1)
- (3, Carlos, 20 años, 2)

Cada una de las entidades pertenecientes a este conjunto se diferencia de las demás por el valor de sus atributos. Nótese que dos o más entidades diferentes pueden tener los mismos valores para algunos de sus atributos, pero nunca para todos.

En particular, los **atributos identificativos** son aquellos que permiten diferenciar a una instancia de la entidad de otra distinta. Por ejemplo, el atributo identificativo que distingue a un alumno de otro es su número de id.

Para cada atributo, existe un **dominio** del mismo que hace referencia al tipo de datos que será almacenado a restricciones en los valores que el atributo puede tomar cadenas de caracteres, números, sólo dos letras, sólo números mayores que cero, sólo números enteros...).

Cuando algún atributo correspondiente a una entidad no tiene un valor determinado, recibe el **valor nulo**, bien sea porque no se conoce, porque no existe o porque no se sabe nada al respecto del mismo.
### [[GIT CMD]]
Tienen una característica conocida como "cardinalidad", la cual indica el sentido y la cantidad de "relaciones" existentes entre una entidad y otra.

#### Correspondencia de cardinalidades

Dado un conjunto de relaciones en el que participan dos o más conjuntos de entidades, la cardinalidad de la correspondencia indica el número de entidades con las que puede estar relacionada una entidad dada.

Dado un conjunto de relaciones binarias y los conjuntos de entidades A y B, las cardinalidades pueden ser:

- **Uno a Uno:** (1:1) Un registro de una entidad A se relaciona con solo un registro en una entidad B. (ejemplo dos entidades, profesor y departamento, con llaves primarias, codigo_profesor y jefe_depto respectivamente, un profesor solo puede ser jefe de un departamento y un departamento solo puede tener un jefe).

- **Uno a Varios:** (1:N) Un registro en una entidad en A se relaciona con un B. Pero los registros de B se relacionan con uno o muchos registros en una entidad A. (ejemplo: dos entidades, vendedor y ventas, con llaves primarias, código_vendedor y venta, respectivamente, un vendedor puede tener muchas ventas pero una venta solo puede tener un vendedor).

- **Varios a Uno:** (N:1) Una entidad en A se relaciona con varios registros con una entidad en B. Pero una entidad en B se puede relacionar con un solo registro de la entidad A. (ejemplo empleado-centro de trabajo).

- **Varios a Varios:** (N:M) Una entidad en A se puede relacionar con 1 o con muchas entidades en B y viceversa (ejemplo asociaciones-ciudadanos, donde muchos ciudadanos pueden pertenecer a una misma asociación, y cada ciudadano puede pertenecer a muchas asociaciones distintas).


## Primary  Key
valor único que identifica a cada elemento. 

#### Cuando hay una relación **(1 : n)** la foreign key va siempre del lado de la **n** .


## JOINS
### INNER JOIN
Solo se muestran los registros que coinciden tanto en la tabla A como en la tabla B.


## NORMALIZACIÓN

- Eliminar redundancias de datos.
- Evitar anomalías en operaciones de inserción, actualización y eliminación.
- Garantizar la integridad de los datos.