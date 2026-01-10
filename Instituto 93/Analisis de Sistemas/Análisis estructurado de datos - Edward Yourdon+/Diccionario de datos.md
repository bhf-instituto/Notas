El diccionario de datos es un listado organizado de todos los datos pertinentes al sistema, con definiciones precisas y rigurosas para que tanto el usuario como el analista tengan un entendimiento común de todas las entradas, salidas, componentes de almacenes y cálculos intermedios. 

El diccionario de datos define los datos haciendo lo siguiente:
- Describe el significado de los flujos y almacenes que se muestran en los DFD.
- Describe la composición de agregados de paquetes de datos que se mueven a lo largo de los flujos, es decir, paquetes complejos (por ejemplo el domicilio de un cliente), que pueden descomponerse en unidades más elementales (como ciudad, estado y código postal).
- Describen la composición de los paquetes de datos en los almacenes.
- Especifica los valores y unidades relevantes de piezas elementales de información en los flujos de datos y en los almacenes de datos.
- Describe los detalles de las relaciones entre almacenes que se enfatizan en un diagrama de entidad-relación.

En la mayoría de los sistemas reales con los que se trabaja, los paquetes, o elementos de datos, serán lo suficientemente complejos como para que se necesite describirlos en términos de otras cosas. Los elementos complejos de datos se definen en términos de elementos más sencillos, y los sencillos en términos de los valores y unidades legítimos que pueden asumir.

>Tomamos como ejemplo la del marciano que no entiende que es un nombre, nosotros le explicamos y el nos dice que su nombre es "3.98123791236". Le decimos que eso es un número no un nombre, pero el nos dice que está orgulloso con su nombre. Entonces le preguntamos el apellido, nos dice que no sabe que es y no lo entiende. Al explicárselo nos pregunta si nosotros podríamos llamarnos "12 43 55" Le decimos que nosotros solo aceptamos caracteres de la A a la Z para nombres. 

Todas estas preguntas no están lejos de las que deberían hacerse entre el analista y el usuario, preguntas por ejemplo: 

- ¿Debe tener todo mundo un nombre? Qué tal el personaje “Sr. T” de la popular serie de televisión “Los cuatro fantásticos”?
- ¿Qué pasa con los signos de puntuación en los apellidos de las personas, por ejemplo “D’Arcy”?
- ¿Se permiten los segundos nombres abreviados, por ejemplo, “Juan X Jasso?”
- ¿Existe una longitud mínima para el nombre de una persona? Por ejemplo, ¿es legal el nombre “X Y”?
- ¿Cómo debemos tratar los sufijos que a veces siguen al apellido? por ejemplo, se supone que el nombre “Juan Jasso Jr.” es legítimo, pero ¿Se considera el Jr. como parte del apellido, o en una categoría aparte? Y si está en una nueva categoría, ¿no debiéramos permitir también dígitos, como por ejemplo, Samuel Sosa 3°?

Ninguna de estas cuestiones tienen que ver con la forma en la que se almacenará la información en la computadora, simplemente  estamos tratando de determinar, como cuestión de *política de negocios*, lo que constituye un nombre válido. 

### Notación

| =   | está compuesto de                               |
| --- | ----------------------------------------------- |
| +   | y                                               |
| ( ) | optativo (puede estar presente o ausente)       |
| { } | iteración                                       |
| [ ] | seleccionar una de varias alternativas          |
| * * | comentario                                      |
| @   | identificador                                   |
| \|  | separa opciones alternativas en la construcción |
Por ejemplo, podríamos definir el **nombre** de un usuario así: 
> **nombre = `título de cortesía + nombre + (segundo nombre) + apellido`** 
> **título de cortesía =  `[SR. | Srita. | Sra. | Dr. | Profesor]`**
> **nombre = `{carácter válido}`**
> **segundo nombre = `{carácter válido}`**
> **apellido = `{carácter válido}`**
> **carácter válido = `[A-Z | a-z | 0-9 | ' | - | |]`**

### Definiciones

La definición de un dato se introduce con el símbolo “=”. En este contexto, el se lee: “se define como”, o “se compone de”, o simplemente “significa”. Por ello, la notación: 
> **A + B = C **

