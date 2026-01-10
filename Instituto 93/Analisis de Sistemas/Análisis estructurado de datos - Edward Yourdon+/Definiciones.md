## DFD
- Es una herramienta que permite visualizar un sistema como una red de procesos funcionales, conectados entre sí por "conductos” y “tanques de almacenamiento” de datos. 
- No sólo se pueden utilizar para modelar sistemas de sistemas de proceso de información, sino también como manera de modelar organizaciones enteras, es decir, como una herramienta para la planeación estratégica y de negocios.

**Características básicas:** 
- No requieren explicación, con solo verlo se debería entender.
- Debe caber en una página:
	- Evita frustración del cliente
	- Denota que no es complejo el sistema. 

Los DFD no aclaran muchos aspectos operativos del movimiento de datos: no indican si un proceso solicita los datos o si estos llegan por sí mismos, ni explican cuándo se envían los paquetes, en qué orden llegan las entradas o cómo se combinan para producir las salidas.  
Tampoco especifican si un proceso necesita una cierta cantidad de datos de cada flujo ni qué relación exacta existe entre entradas y salidas.

La razón es simple: **no lo sabemos**, porque esas cuestiones pertenecen al nivel de detalle procedimental.  
Ese tipo de dudas —secuencias, pedidos explícitos, sincronización, reglas exactas de combinación— se modelan con herramientas orientadas a procedimientos, no con un DFD.  

>El DFD **no intenta** describir ese tipo de comportamiento interno; solo muestra el flujo de datos entre procesos, sin entrar en la lógica operativa que los gobierna.
### Proceso
El proceso muestra una parte del sistema que transforma entradas en salidas; es decir, muestra cómo es que una o más entradas se transforman en salidas.
-  Un buen nombre de proceso generalmente consiste en una frase verbo objeto tal como **VALIDAR ENTRADA** o **CALCULAR IMPUESTO**.

### Flujo
El flujo se usa para describir el movimiento de bloques o paquetes de información de una parte del sistema a otra. Por ello, los flujos representan datos en movimiento, mientras que los almacenes representan datos en reposo.
El nombre del flujo representa el significado del paquete que se mueve a lo largo del flujo.

#### Flujo Divergente 
 Esto significa que se están mandando copias por duplicado de un paquete de datos a diferentes partes del sistema, o bien que un paquete complejo de datos se está dividiendo en varios paquetes individuales más, cada uno de los cuales se está mandando a diferentes partes del sistema.

#### Flujo Convergente
Significa que varios paquetes elementales de datos se están uniendo para formar agregados más complejos de paquetes de datos.

### Almacén
Cuando nos referimos a **Almacén** no solo estamos hablando de bases de datos, si no a cualquier forma física de almacenar datos. Ya sean cintas perforadas, discos duros, bases de datos, cajones, fichas, etc. 

*¿Existe en el sistema por causa de un requerimiento fundamental del usuario o por algún aspecto conveniente de la realización del sistema?*
#### Almacén necesario
El **almacén necesario** existe porque **responde a un requerimiento funcional esencial del usuario**, no por razones técnicas de implementación. Su presencia en el sistema es obligatoria para cumplir la lógica del negocio o la política operativa del usuario.

Yourdon lo caracteriza así:

1. **Su existencia proviene de una necesidad del usuario, no de la tecnología.**  
    El almacén existe debido a un requerimiento fundamental del negocio.  
    No depende de que la computadora tenga limitaciones ni de cómo se implemente el sistema.
    
2. **Su propósito es permitir la separación temporal entre procesos.**  
    En muchos sistemas, dos procesos deben ejecutarse en **momentos distintos por decisión del usuario**, por ejemplo:
    - El proceso _ENTRADA DE ÓRDENES_
    - El proceso _INVESTIGACIÓN DE ÓRDENES_
    
    Esta separación temporal hace indispensable un área de almacenamiento.
    
3. **Debe existir en alguna forma física, cualquiera que sea.**  
    El almacén necesario tiene que existir independientemente del soporte:
    
    - Disco
    - Cinta
    - Tarjetas
    - O incluso “inscrito en piedra”, como dice el texto
    
    Lo fundamental es que el sistema **requiere** esa persistencia de datos para funcionar según las reglas del usuario.
    
4. **Es estrictamente parte del modelo lógico.**  
    El almacén necesario forma parte del DFD porque el sistema, en su lógica esencial, **no puede operar sin él**.

![[Pasted image 20251107011603.png]]
#### Almacén de implantación
Un **almacén de implantación** es un almacén añadido al diseño **no porque el modelo lógico lo requiera**, sino porque la **tecnología o las condiciones prácticas de implementación lo hacen necesario**. El almacén no representa un requisito funcional, sino una **solución técnica**.

El diseñador puede introducir un almacén (por ejemplo, un almacén de ORDENES entre _ENTRA ORDEN_ y _PROCESA ORDEN_) por varias razones:

1. **Limitaciones de hardware o recursos del sistema.**  
    Si ambos procesos deben ejecutarse en la misma computadora pero no pueden correr simultáneamente (por falta de memoria u otros recursos), se introduce un archivo intermedio.  
    El almacén permite ejecutar los procesos en **momentos distintos**, impuesto por la tecnología  disponible.
    
2. **Baja confiabilidad de la plataforma.**  
    Si uno o ambos procesos se ejecutan en hardware poco confiable, se crea el almacén para disponer de un **respaldo**.  
    Si un proceso falla, los datos almacenados permiten reanudar el trabajo sin perder  información.
    
3. **Separación del trabajo entre diferentes programadores o equipos.**  
    Si los procesos serán desarrollados por distintos programadores o incluso por **equipos en diferentes ubicaciones**, el almacén sirve como punto común de prueba y depuración.  
    Si el sistema falla, ambos grupos pueden inspeccionar el almacén y detectar el origen del  problema.
    
4. **Anticipación de necesidades futuras del usuario.**  
    El diseñador puede pensar que el usuario querrá acceder al almacén más adelante, aunque eso **no forme parte de los requisitos actuales**.  
    El almacén se crea por previsión, aunque implique costos adicionales para el usuario por algo no solicitado.

#### Únicos detalles de tipo _procedimiento_ en los almacenes
1) Existe un detalle de tipo procedimiento del cual podemos estar seguros: **el almacén es pasivo**, y los datos no viajarán a lo largo del flujo a menos que el proceso lo solicite explícitamente. 
2) Existe otro detalle de tipo procedimiento que suponen, por convenio, los sistemas de proceso de datos: **el almacén no cambia cuando un paquete se mueve del almacén a lo largo del flujo**. Un programador pudiera referirse a esto como una lectura no destructiva o, en otras palabras, del almacén se recupera una copia del paquete y el almacén mantiene su condición original.
--- 
- Un flujo hacia un almacén habitualmente se describe como una escritura, una actualización o posiblemente una eliminación. En todos estos casos es evidente que el almacén cambió como resultado del flujo que ingresa. El proceso (o procesos) conectados con el otro extremo del flujo es el responsable de realizar el  cambio al almacén.

### Entidad Externa
Los **terminadores** representan **entidades externas** con las cuales el sistema intercambia información. Son actores que están fuera del control del sistema, pero interactúan con él mediante flujos de datos.

#### Qué pueden ser los terminadores

- Personas, grupos, departamentos u organizaciones externas.
- Departamentos internos de la misma empresa, siempre que estén **fuera del control** del sistema modelado.
- Otros sistemas computacionales.
- En algunos casos, incluso el **usuario**, si no forma parte operativa del sistema.

#### Cómo se identifican

El usuario puede ayudar a determinarlos al describir qué entradas y salidas necesita el sistema.  
Ejemplo:

- El sistema debe recibir formularios del departamento de contaduría.
- El sistema debe enviar reportes al comité de finanzas.  
    Estos departamentos son terminadores distintos porque son externos al sistema.

#### Tres principios clave sobre los terminadores

1. **Son externos al sistema.**  
    Los flujos que los conectan con procesos o almacenes representan la interfaz entre el sistema y su entorno.
2. **El analista no puede modificar su funcionamiento.**  
    Como son externos, el analista no puede cambiar cómo operan, sus procedimientos o su información interna.
3. **Las relaciones entre terminadores no se muestran en un DFD.**  
    Aunque los terminadores puedan relacionarse entre sí, esas relaciones no forman parte del sistema modelado.  
    Si una relación entre ellos es tan importante que debe incluirse, entonces dichas entidades **no** son externas; deben modelarse como procesos del sistema.

## DER
