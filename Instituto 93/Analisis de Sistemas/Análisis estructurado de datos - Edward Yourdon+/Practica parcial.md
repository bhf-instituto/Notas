#### DFD 
Un DFD es una herramienta que permite representar un sistema como una red de procesos interconectados entre si por flujos y almacenes de datos. 
Es útil para sistemas donde sus funciones son mas importantes y complejas que los datos que este maneja.
No solo sirve para representar sistemas de información, si no que también se pueden representar negocios enteros.
Sus características principales son : 
- Debe ser legible, la notación es simple. Basta con verlo para entenderlo. 
- Debe caber en una hoja, esto evita la ofuscación de quien lo lee. 

El DFD no intenta explicar el funcionamiento interno del sistema, si no que solo muestra el movimiento de las datos entre los procesos. 
##### Proceso
El proceso muestra una parte del sistema que se encarga de transformar entradas en salidas. Se representa como una burbuja y su nombre describe que acción se esta realizando a modo de "frase-verbo", como "ANALIZAR DATO", "RECIBIR PEDIDO". No se usan nombres propios ya que el personal de la empresa podría cambiar, por ende no se permiten nomenclaturas tipo "JUAN RECIBE EL PEDIDO", si bien Juan puede ser la persona encargada de recibir los pedidos, no se debe utilizar nombres, porque en el futuro esta proceso podría estar realizado por otra persona. 

##### Flujo
El flujo representa el movimiento de bloques o paquetes de datos de una parte del sistema a otra, aunque no siempre representa información (bits) también podríamos tomar como ejemplo un DFD para representar una línea de ensamblado de un producto, en ese caso el flujo representa el movimiento de materiales, algo físico.  
Los flujos se representan como flechas, esto indica que tienen una dirección, implicando que pueden entrar o salir de un proceso. Además no solo son unilaterales, si no que tambien existen flujos bi laterales como por ejemplo 

(DETERMINAR ESTADO DEL PEDIDO) → responder con el estado del producto→
							  ← preguntar por el estado del producto ←

Otra categorización para los flujos son los flujos convergentes y divergentes. No se si explicarlo mucho, pero lso convergentes es cuando varios bloques de informacion se unene un un proceso como por ejemplo ingredientes (harina, agua, aceite) uniendose en el proceso PREPARAR PAN.
Los divergentes es el caso opuesto, supongamos que un cliente hace un pedido, el proceso es RECIBIR PEDIDO a el ingresa un paquete de datos con la informacion del pedido, y de el se desprenden varios flujos, uno para generar la documentacion del envio, otro para actualizar el stock y otro para generar el pedido propiamente dicho. 

##### Almacén
Un almacen no es solo una base de datos, si no que es cualquier forma física en la que se pueda guardar un dato. Ya sea un archivo, tarjetas agujereadas o hasta grabado en piedras. 

Algo importante a mencionar de los almacenes es su propósito, si es que son creados obligatoriamente por una petición del usuario, o si son mas bien creados por una necesidad técnica. 

Hablamos acá de los **Almacenes necesarios** los cuales sirven como base de datos diferidas en el tiempo, es decir que la carga de esos paquetes de datos y su posterior recuperación pueden (o no) realizarse en momentos diferentes. Supongamos que en nuestro sistema de pedidos de comida, quien recibe los pedidos solo los acepta y los almacena en orden para que luego el encargado de la cocina los reciba y comience a cocinarlos. Esto proceso podría ser en el mismo momento, o podríamos tener una lista de espera antes de que el pedido sea recibido por la cocina. 

detalle del pedido →pedido→ (ingresar pedidos) →pedido→ | almacén pedidos | →pedido→(cocinar pedido).

La segunda distinción son los **Almacenes de implantación** y surgen a partir de una limitación técnica, cuando el lugar donde se alberga el sistema no es confiable entonces el almacen sirve para almacenar la información en caso de que algo falle. O cuando no tenemos los suficientes recursos suficientes para cubrir ambos procesos a la vez. También cuando se espera que un grupo de analistas trabaje sobre el sistema, pudiendo estar en lugares geográficos diferentes así el almacén sirve para hacer pruebas. O también sirve por si en el futuro el cliente quiere expandir el sistema.

Hay dos caracteristicas de los almacenes que si pertenecen a la parte procedural del sistema y es que los almacenes son pasivos, es decir que la información no se va a mover de ahi a menos que un proceso lo explicite. Y que los almacenes no cambian, en todo caso se hace una copia de un bloque o paquete de informacion pero el almacen original permanece inmutable, esto se llama una lectura no destructiva.

Los flujos que entran al almacen solo puden ser de lectura, actualizacion o eliminacion de registros. 

##### Terminador 
Los terminadores son las entidades externas con las cuales el sistema se comunica. Comunmente son una organizacion externa, o gubernamental, y están fuera del control del sistema que se está modelando. 



### DER
Es una notación gráfica que modela los datos de un sistema con un alto nivel de abstracción. Representa las relaciones que hay entre los almacenes de un DFD pero que en el no se explican. 
Modela los datos y sus relaciones, y facilita la comunicación entre el analista y el usuario. Ademas de complementar el DFD. 

#### Componentes 
##### Tipo de objeto (Entidad)

Caracteristicas principales: 
Las entidades o tipo de objeto representan un conjuto de cosas del mundo real,, las cuales deben  tener una forma de identificación única y se tienen que pode representar por atributos, es decir por caracteristicas, podía ser el caso de un CLIENTE, su identificador único podría ser su DNI y sus caracteristicas en comun podrian ser nombre, apellido. Es vital aclarar que los atributos son necesarios para todas las intancias de esa enditad, no pueden algunas tenerlas y otras no. 

Las entidades tienen un caracter esencial para el sistema, si no es esencial no deberia estar modelado. Por ejemplo tomemos un sistema de un restaurante, los clientes son entidades esenciales y los mozos -aunque útiles- no son esenciales, podrían no estar modelados. 

Además podrían representar conceptos no materiales, como podrían ser horarios, o estrategias. 


##### Relaciones
En un DER las relaciones conectan a las entidades. Son asociaciones entre las entidades participantes, y representan información que se debe almacenar, no algo que pueda ser derivado automaticamente. 