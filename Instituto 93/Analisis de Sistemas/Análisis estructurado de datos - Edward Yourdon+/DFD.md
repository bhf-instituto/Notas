## Definición
Es una herramienta que permite visualizar un sistema como una red de procesos funcionales, conectados entre sí por "conductos” y “tanques de almacenamiento” de datos. 
Es muy usados sobre todo por sistemas operacionales en los cuales las funciones del sistema son de gran importancia y son más complejas que los datos que éste maneja.
Los DFD no sólo se pueden utilizar para modelar sistemas de sistemas de proceso de información, sino también como manera de modelar organizaciones enteras, es decir, como una herramienta para la planeación estratégica y de negocios.
![[Pasted image 20251107000501.png]]
- Prácticamente no requiere explicación; se puede simplemente mirar el diagrama y entenderlo. La notación es sencilla y clara y, en cierto sentido, intuitivamente obvia. Sí el usuario necesita una enciclopedia para poder leer y entender el modelo de su sistema, probablemente no se tomará la molestia de hacer ninguna de las dos cosas.
- El diagrama cabe fácilmente en una página. Esto significa dos cosas: 
	1) alguien puede mirarlo sin ofuscarse  
	2) el sistema que se está modelando no es muy complejo.
* El diagrama se dibujó con computadora. 

## Componentes 
### El Proceso
Los sinónimos comunes son burbuja, función o transformación. El proceso muestra una parte del sistema que transforma entradas en salidas; es decir, muestra cómo es que una o más entradas se transforman en salidas.
![[Pasted image 20251107002446.png]]Nótese que el proceso se nombra o describe con una sola palabra, frase u oración sencilla. En casi todos los DFD que se discutirán en este libro, el nombre del proceso describirá lo que hace.
Por ahora es suficiente decir que un buen nombre generalmente consiste en una frase verbo objeto tal como **VALIDAR ENTRADA** o **CALCULAR IMPUESTO**.
En algunos casos, el proceso contendrá el nombre de una persona o un grupo (por ejemplo, un departamento o una división de una organización), o de una computadora o un aparato mecánico. Es decir, el proceso a veces describe quién o qué lo está efectuando, más que describir el proceso mismo. 
### El Flujo

Un flujo se representa gráficamente por medio de una flecha que entra o sale de un proceso.
El flujo se usa para describir el movimiento de bloques o paquetes de información de una parte del sistema a otra. Por ello, los flujos representan datos en movimiento, mientras que los almacenes representan datos en reposo.

![[Pasted image 20251107002712.png]]
En la mayoría de los sistemas los flujos realmente representarán datos, es decir, bits, caracteres, mensajes, números de punto flotante y los diversos otros tipos de información con los que las computadoras pueden tratar, Pero los DFD también pueden utilizarse para modelar otros sistemas aparte de los automatizados y computarizados; pudiera escogerse, por ejemplo, usar un modelo de DFD para modelar una línea de ensamblado en la que no haya componentes computarizados. En tales casos, los paquetes o fragmentos mostrados por los flujos serán típicamente materiales físicos. 
Nótese que los flujos de las figuras 9.3 y 9.4 tienen nombre. El nombre representa el significado del paquete que se mueve a lo largo del flujo.
También se pudiera ver un solo flujo llamado VEGETALES en lugar de diversos flujos llamados PAPAS, COLES DE BRUSELAS y CHICHAROS. Como veremos, esto requerirá de alguna explicación en el [[Diccionario de datos]].

![[Pasted image 20251107003437.png]]Por ejemplo como se ve en la figura 9.5 El mismo fragmento de datos por ejemplo, (212-410-9955) tiene distinto significado cuando viaja a lo largo de un flujo llamado **NUMERO TELEFONICO** que cuando viaja a lo largo de uno llamado **NUMERO TELEFONICO VALIDO**. En el primer caso, significa un número telefónico que pudiera ser o no válido; en el segundo caso, significa un número telefónico que, dentro del contexto de este sistema, se sabe que es válido.
Nótese también que los flujos muestran la dirección: una cabeza de flecha en cualquier extremo (o posiblemente ambos) del flujo indica si los datos (o el material) se está moviendo hacia adentro o hacia afuera de un proceso (o ambas cosas). 
![[Pasted image 20251107004206.png]]Nótese también que los flujos muestran la dirección: una cabeza de flecha en cualquier extremo (o posiblemente ambos) del flujo indica si los datos (o el material) se está moviendo hacia adentro o hacia afuera de un proceso (o ambas cosas). El flujo que se muestra en la figura 9.6(a), por ejemplo, indica claramente que el número se está mandando hacia el proceso denominado VALIDAR NUMERO TELEFONICO.
![[Pasted image 20251107004624.png]]
El flujo de dos cabezas que se muestra en la figura 9.6(c) es un diálogo, es decir, un empacado conveniente de dos paquetes de datos (una pregunta y una respuesta) en el mismo flujo. En el caso de un diálogo, los paquetes en cada extremo de la flecha deben nombrarse, como se ilustra en la figura 9,6(c)
![[Pasted image 20251107004850.png]]

Los flujos de datos pueden divergir o converger en un DFD; conceptualmente esto es algo así como un río principal que se divide en varios más pequeños, o varios pequeños que se unen. Sin embargo, esto tiene un significado especial en un DFD típico en el cual hay paquetes de datos que se mueven a través del sistema: 
- En el caso de un **flujo divergente**, esto significa que se están mandando copias por duplicado de un paquete de datos a diferentes partes del sistema, o bien que un paquete complejo de datos se está dividiendo en varios paquetes individuales más, cada uno de los cuales se está mandando a diferentes partes del sistema, o que el ducto de flujo de datos lleva artículos con distintos valores (por ejemplo, vegetales cuyos valores pudieran ser “papa”, “zanahoria” o “choclo”) que están siendo separados. Por ejemplo las figuras 9.7(a) y 9.7(b).
- En el caso de un **flujo convergente**, de manera inversa,  significa que varios paquetes elementales de datos se están uniendo para formar agregados más complejos de paquetes de datos.

![[Pasted image 20251107005821.png]]
___
![[Pasted image 20251107010016.png]]
Los DFD no aclaran muchos aspectos operativos del movimiento de datos: no indican si un proceso solicita los datos o si estos llegan por sí mismos, ni explican cuándo se envían los paquetes, en qué orden llegan las entradas o cómo se combinan para producir las salidas.  
Tampoco especifican si un proceso necesita una cierta cantidad de datos de cada flujo ni qué relación exacta existe entre entradas y salidas.

La razón es simple: **no lo sabemos**, porque esas cuestiones pertenecen al nivel de detalle procedimental.  
Ese tipo de dudas —secuencias, pedidos explícitos, sincronización, reglas exactas de combinación— se modelan con herramientas orientadas a procedimientos, no con un DFD.  

>El DFD **no intenta** describir ese tipo de comportamiento interno; solo muestra el flujo de datos entre procesos, sin entrar en la lógica operativa que los gobierna.


### El almacén 

El almacén se utiliza para modelar una colección de paquetes de datos en reposo.
![[Pasted image 20251107011004.png]]
Cuando nos referimos a **Almacén** no solo estamos hablando de bases de datos, si no a cualquier forma física de almacenar datos. Ya sean cintas perforadas, discos duros, bases de datos, cajones, fichas, etc. 
Aparte de la forma física que toma el almacén, también existe la cuestión de su propósito: ¿Existe el sistema por causa de un requerimiento fundamental del usuario o por algún aspecto conveniente de la realización del sistema? En el primer caso, la base de datos existe como un área de almacenamiento diferida en el tiempo necesaria entre dos procesos que ocurren en momentos diferentes. Por ejemplo, la figura 9.10 muestra un fragmento de un sistema en el cual, como política del usuario (independientemente de la tecnología que se use para implantar el sistema), el proceso de entrada de órdenes puede operar en tiempos diferentes (o posiblemente en el mismo) que el proceso de Investigación de órdenes. El almacén de ORDENES debe existir en alguna forma, ya sea en disco, cinta, tarjetas o inscrito en piedra. Estamos hablando de un **ALMACÉN NECESARIO**.
![[Pasted image 20251107011603.png]]
La figura 9.11 (a) muestra un tipo distinto de almacén: el almacén de implantación. Podemos imaginar al diseñador del sistema interponiendo un almacén de ORDENES entre ENTRA ORDEN y PROCESA ORDEN porque:
- Se espera que ambos procesos se ejecuten en la misma computadora, pero no hay suficiente memoria (o algún otro recurso de hardware) para cubrir ambos al mismo tiempo. Así, el almacén de ORDENES se crea como archivo intermedio, pues la tecnología de implantación disponible ha forzado a que los procesos se ejecuten en tiempos distintos.
- Se espera que cualquiera de los procesos, o ambos, se ejecuten en una configuración de hardware que es poco confiable. Así, el almacén de ORDENES se crea como respaldo en caso de que cualquiera de los procesos se aborte.
- Se espera que diferentes programadores implanten los dos procesos (o, en un caso más extremo, que lo hagan diferentes grupos de programadores que trabajan en lugares geográficos distintos). Así, el almacén de ORDENES se crea para probar y corregir, de manera que si el sistema completo no trabaja ambos grupos puedan ver los contenidos del almacén y detectar el problema.
- El analista o el diseñador pensaron que el usuario pudiera algún día hacer accesos al almacén de ORDENES por alguna otra razón, aun cuando no haya expresado tal interés. En este caso, el almacén se crea anticipando necesidades futuras del usuario (y dado que costará algo implantar el sistema de esta manera, el usuario acabará pagando por algo que no se pidió).

Si fueran a excluirse los asuntos y modelar sólo los requerimientos esenciales del sistema, no existiría necesidad de un almacén de ORDENES; en lugar de eso se tendría un DFD como el que se muestra en la figura 9 .11(b).

![[Pasted image 20251107011945.png]]![[Pasted image 20251107012057.png]]
Muchos analistas no se molestan en etiquetar el flujo si una instancia completa del paquete fluye hacia o desde el almacén.
Normalmente se interpreta un flujo que procede de un sistema como una lectura o un acceso a la información del almacén. Esto significa específicamente que:
- Se recupera del almacén un solo paquete de datos; esto es, de hecho, el ejemplo más común de flujo desde un almacén. Ej : un almacén llamado **CLIENTES** donde cada paquete contiene nombre, apellido, y teléfono de clientes. Y un flujo típico del almacén podría implicar la recuperación de un paquete completo de información acerca de un cliente.
- Se ha recuperado más de un paquete del almacén.  Ej: el flujo podría recuperar paquetes de información acerca de todos los  clientes de una ciudad. 
- Se tiene una porción de un paquete del almacén. Ej: un flujo podría recuperar solo el teléfono de los clientes.
![[Pasted image 20251107012706.png]]
>Si la etiqueta del flujo es la misma que la del almacén significa que se recupera todo un paquete (o múltiples instancias de uno completo).

>Si la etiqueta del flujo es diferente del nombre del almacén, entonces se están recuperando uno o más componentes de uno o más paquetes.

Existe un detalle de tipo procedimiento del cual podemos estar seguros: **el almacén es pasivo**, y los datos no viajarán a lo largo del flujo a menos que el proceso lo solicite explícitamente. Existe otro detalle de tipo procedimiento que suponen, por convenio, los sistemas de proceso de datos: **el almacén no cambia cuando un paquete se mueve del almacén a lo largo del flujo**. Un programador pudiera referirse a esto como una lectura no destructiva o, en otras palabras, del almacén se recupera una copia del paquete y el almacén mantiene su condición original.

Un flujo hacia un almacén habitualmente se describe como una escritura, una actualización o posiblemente una eliminación. Específicamente, sólo puede significar que se tiene una de las situaciones siguientes:
- Se están guardando uno o más paquetes nuevos en el almacén. Dependiendo de la naturaleza de sistema, los paquetes nuevos pudieran anexarse (cargarse al final *.append()*) o colocarse en algún lugar entre los paquetes existentes.
- Uno o más paquetes se están borrando o retirando del almacén.
- Uno o más paquetes se están modificando o cambiando. Esto pudiera traer consigo un cambio de todo un paquete, o (más comúnmente), de sólo una porción, o de una porción de múltiples paquetes. 

En todos estos casos es evidente que el almacén cambió como resultado del flujo que ingresa. El proceso (o procesos) conectados con el otro extremo del flujo es el responsable de realizar el  cambio al almacén.

>los flujos conectados a un almacén sólo pueden transportar paquetes de información que el almacén sea capaz de guardar.


### El terminador (Entidad externa)

Los terminadores representan entidades externas con las cuales el sistema se comunica. Comúnmente, un terminador es una persona o un grupo, por ejemplo, una organización externa o una agencia gubernamental, o un grupo o departamento que esté dentro de la misma compañía u organización, pero fuera del control del sistema que se está modelando. En algunos casos, un terminador puede ser otro sistema, como algún otro sistema computacional con el cual se comunica éste. A veces el terminador es el usuario.

En otros casos, el usuario se considera parte del sistema y ayudará a identificar los terminadores relevantes; por ejemplo, le dirá “Tenemos que estar listos para recibir las formas tipo 321 del departamento de contaduría, y debemos enviar reportes financieros semanales al comité de finanzas”. En este último caso, es evidente que el departamento de contaduría y el comité de finanzas son terminadores separados con los cuales se comunica el sistema.

![[Pasted image 20251107013621.png]]
**Existen tres cosas importantes que debemos recordar acerca de los terminadores:** 

- Son *externos* al sistema que se está modelando; los flujos que conectan los terminadores a diversos procesos (o almacenes) en el sistema representan la interfaz entre él y el mundo externo.
- Como consecuencia, es evidente que ni el analista ni el diseñador del sistema están en posibilidades de cambiar los contenidos de un terminador o la manera en la que trabaja.
- Las relaciones que existan entre los terminadores **no** se muestran en el modelo de DFD. Pudieran existir de hecho diversas relaciones, pero, por definición, no son parte de el sistema que se está estudiando. De manera inversa, si existen relaciones entre los terminadores y si es esencial para el analista modelarlos para poder documentar los requerimientos del sistema, entonces, por definición, los terminadores son en realidad parte del sistema y debieran modelarse como procesos.

## Guía para la construcción de un DFD

**Reglas básicas:**

1) Escoger nombres con significado para los procesos, flujos, almacenes y terminadores.
2) Numerar el proceso.
3) Redibujarlo tantas veces como sea necesario. 
4) Evitar los DFD excesivamente complejos.
5) Asegurarse de que el DFD sea internamente consistente y que también lo sea con cualesquiera DFD relacionados con él.

#### Escoger nombres con significado para los procesos, flujos, almacenes y terminadores

Un buen sistema que se puede utilizar para nombrar procesos es usar un verbo y un objeto. Es decir, escoja un verbo activo (un verbo transitivo, uno que tenga objeto) y un objeto apropiado para formar una frase descriptiva para el proceso. 

Los siguientes son ejemplos de nombres de procesos:
- CALCULAR TRAYECTORIA DEL PROYECTIL.
- PRODUCIR INFORME DE INVENTARIO. 
- VALIDAR NUMERO TELEFONICO.
- ASIGNAR ESTUDIANTES A LA CLASE. 

Sin embargo, aun en los casos sencillos hay la tentación de utilizar nombres ambiguos como HACER, MANEJAR y PROCESAR. Cuando se utilizan verbos tan “elásticos” (verbos con significados para cubrir casi cualquier situación), a menudo significa que el analista no está seguro de cuál función se está llevando a cabo o que se han agrupado diversas funciones que en realidad no debieran agruparse.

Estos son algunos nombres de procesos poco adecuados:
- DATOS DE PROCESOS.
- HACER ALGO.
- FUNCIONES MISCELANEAS.

#### Numerar el proceso

Como una forma conveniente de referirse a los procesos en un DFD, muchos analistas numeran cada burbuja. 
La única cosa que se debe tener en mente es que el sistema de numeración implicará, para algunos lectores casuales de su DFD, una cierta secuencia de ejecución. 
Los números se convierten en base para la numeración jerárquica cuando se introduzcan los diagramas de flujo por niveles. 

#### Evitar los DFD demasiado complejos

Hay una regla principal que se debe tener en mente: no cree un DFD con demasiados procesos, flujos, almacenes y terminadores. En la mayoría de los casos, esto significa que no debiera haber más de media docena de procesos y almacenes, flujos y terminadores relacionados en un solo diagrama. Otra manera de decir esto es que el DFD debe caber cómodamente en una hoja normal.
Existe una excepción importante a esto, que discutiremos en el capítulo 18: un DFD especial conocido como [[Diagrama de Contexto]], que representa el sistema entero como un solo proceso y destaca las interfaces entre el sistema y los terminadores externos.

Comúnmente, los diagramas de contexto como el que se muestra en la figura 9.15 no se pueden simplificar, pues describen, con el más alto detalle, una realidad que es intrínsecamente compleja.
![[Pasted image 20251107015705.png]]
#### Asegúrese de que su DFD sea lógicamente consistente

Las principales regias de consistencia son:

- Evite sumideros infinitos, burbujas que tienen entradas pero no salidas. (Agujeros negros).
- Evite las burbujas de generación espontánea, que tienen salidas sin tener entradas, porque son sumamente sospechosas y generalmente incorrectas. (Un ejemplo factible de una burbuja que sólo tiene salidas es un generador de números aleatorios)
- Tenga cuidado con los flujos y procesos no etiquetados. Esto suele ser un indicio de falta de esmero, pero puede esconder un error aún más grave: a veces el analista no etiqueta un flujo o un proceso porque simplemente no se le ocurre algún nombre razonable. En el caso de un flujo no etiquetado, pudiera significar que diversos datos elementales no relacionados se agruparon arbitrariamente; en el caso de un proceso no etiquetado, pudiera significar que el analista estaba tan confundido que dibujó un diagrama de flujo disfrazado en lugar de un diagrama de flujo de datos.
![[Pasted image 20251107020300.png]]
- Tenga cuidado con los almacenes de “sólo lectura” o “sólo escritura”. Esta regla es análoga a la de los procesos de “únicamente entradas” o “únicamente salidas”. Un almacén típico debiera tener tanto entradas como salidas. (La única excepción a esta regla es el almacén externo, que sirve de interfaz entre el sistema y algún terminador externo).
![[Pasted image 20251107020433.png]]
### DFD por niveles

Organizar el DFD global en una serie de niveles de modo que cada uno proporcione  sucesivamente más detalles sobre una porción del nivel anterior.

El DFD del primer nivel consta sólo de una burbuja, que representa el sistema completo; los flujos de datos muestran las interfaces entre el sistema y los terminadores externos (junto con los almacenes externos que pudiera haber, como lo ilustra la Figura 9.19.

El DFD que sigue del diagrama de contexto se conoce como la figura 0. Representa la vista de más alto nivel de las principales funciones del sistema, al igual que sus principales interfaces.

Los números también sirven como una manera adecuada de relacionar una burbuja con el siguiente nivel del DFD que la describe más a fondo.
![[Pasted image 20251107020814.png]]
##### **1. ¿Cuántos niveles debe tener un DFD?**

- Cada DFD no debe tener **más de media docena de burbujas** y almacenes.
- Si un nivel bajo aún tiene muchas burbujas (ej.: 50), falta descomponer más.
- En el capítulo 11 se sugiere otra verificación:  
    Si no podés escribir una **especificación de proceso** de una burbuja en **una página aproximadamente**, es demasiado compleja → se debe dividir en DFD de menor nivel.
##### **2. ¿Cuántos niveles suelen tener los sistemas?**

- Sistema simple: **2 a 3 niveles**.
- Sistema mediano: **3 a 6 niveles**.
- Sistema grande: **5 a 8 niveles**.
- Desconfiar de quien diga que todos los sistemas siempre tienen 3 niveles.
- Las burbujas crecen **exponencialmente** con cada nivel.  
    Ejemplo si cada figura tiene 7 burbujas:
    - Nivel 3: 343
    - Nivel 5: 16.807
    - Nivel 9: 40.353.607

##### **3. ¿Todas las partes del sistema deben tener la misma cantidad de niveles?**

- **No.**  
    Algunas burbujas pueden necesitar más niveles que otras.
- Pero si una burbuja de alto nivel es muy simple (primitiva) y otra requiere 7 niveles, el modelo queda **desequilibrado** → probablemente mal organizado.  
    Puede requerir **reasignar o separar funciones** entre burbujas.
##### **4. ¿Cómo mostrar los niveles al usuario?**

- Diferentes usuarios necesitan diferentes niveles:
    - Ejecutivos → solo el diagrama de contexto o figura 0.
    - Usuarios operativos → solo la parte del sistema que les interesa.
- Para mostrar todo el sistema, lo ideal es hacerlo **de forma descendente**:  
    **Contexto → Figura 0 → niveles inferiores**.
- Es útil mostrar juntos:
    - La figura del nivel en que se está trabajando
    - Su figura “progenitora” para ver el contexto.
##### **5. ¿Cómo asegurar consistencia entre niveles?**
- Regla fundamental de balanceo:  
    Los flujos que **entran y salen de una burbuja** deben coincidir exactamente con los flujos que **entran y salen de la figura** que la detalla en el nivel inferior.
- Si esto no coincide, el DFD está **desbalanceado**.
##### **6. ¿Cómo se muestran los almacenes en diferentes niveles?**
- Se introduce **redundancia deliberadamente**.
- Regla:
    1. Un almacén debe aparecer en el nivel más alto donde **interfaz** con dos o más procesos.
    2. Luego debe aparecer **otra vez en cada nivel inferior** donde se detallen esos procesos.
- Los **almacenes locales** (usados solo dentro de una figura de bajo nivel) **no se muestran arriba**, porque se consideran implícitos en el proceso superior.
##### **7. ¿Cómo se hace realmente la partición en niveles?**

- Aunque los DFD se **presentan** de arriba hacia abajo, no necesariamente se **construyen** así.
- El enfoque top-down es atractivo, pero puede fallar en la práctica.
- Enfoque recomendado:
    1. Identificar los **acontecimientos externos** del sistema.
    2. A partir de ellos crear un **primer borrador del DFD**.
    3. Luego, según necesidad, **partir hacia arriba** (más niveles altos) o **hacia abajo** (más detalle).
- En resumen:  
    **La forma en que se presentan los niveles no necesariamente es la forma en que se desarrollan.**
    