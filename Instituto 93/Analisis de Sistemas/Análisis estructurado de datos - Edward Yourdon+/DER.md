## ¿Qué es un DER? 

### Edward Yourdon

**EL diagrama de entidad-relación (DER o E-R)**, una notación gráfica que modela los datos de un sistema con alto nivel de abstracción.

- Se diferencia del **DFD** (que modela funciones) y del **diagrama de transición de estados** (que modela comportamiento en el tiempo).
- Es útil porque los datos y sus relaciones pueden ser muy complejos, y a menudo los directivos o responsables de información se preocupan más por **qué datos existen, quién los tiene y quién accede a ellos** que por los procesos.
- El **grupo de administración de datos (AD)** se encarga de controlar la información esencial del negocio y debe coordinarse cuando se construye un sistema nuevo.
- El **grupo de administración de bases de datos (ABD)**, dentro del área de procesos, se encarga de implementar el modelo de datos en un sistema de base de datos concreto.
- Para los analistas, el DER resalta relaciones entre almacenes de datos que en un DFD no se aprecian.
- En sistemas orientados solo a información (como consultas o apoyo a decisiones), el DER puede ser más importante que el DFD.
- El capítulo luego profundiza en la **estructura, componentes y reglas** para construir un DER bien organizado, tomando como base las propuestas de Chen, Martin, Date y otros.

👉 En pocas palabras: el DER sirve para **modelar datos y sus relaciones**, facilita la comunicación con usuarios y administradores de datos, y complementa (o incluso sustituye en algunos casos) al DFD en el análisis de sistemas.

![[Pasted image 20250923145821.png]]

### Chat gpt xd

Un **DER (Diagrama de Entidad-Relación)** es una representación gráfica que muestra **los datos de un sistema y cómo se relacionan entre sí**.

- **Qué es**:  
    Usa símbolos (rectángulos, rombos, líneas, etc.) para representar **entidades** (como clientes, productos, empleados), sus **atributos** (nombre, precio, edad) y las **relaciones** entre ellas (un cliente hace un pedido, un producto pertenece a una categoría).
- **Para qué sirve**:
    - **Visualizar la estructura de los datos** de manera clara y comprensible.
    - **Comunicar** cómo están organizados los datos a usuarios, analistas y programadores.
    - **Diseñar bases de datos**, ya que del DER se puede pasar al modelo relacional (tablas, claves primarias y foráneas).
    - **Detectar necesidades de información** antes de implementar un sistema.

👉 En resumen: un DER es una herramienta para **planificar y entender los datos y sus relaciones** antes de crear la base de datos real.

## Componentes
### Tipos de objetos

![[Pasted image 20250923150154.png]]
En un diagrama entidad-relación, un **tipo de objeto** se representa con un rectángulo y hace referencia a un conjunto de cosas del mundo real (instancias) que cumplen ciertas características.

Primero, cada instancia debe poder **identificarse de forma única**, ya sea con un número de cuenta, apellido, DNI, etc. Si no existe un criterio para diferenciar unas de otras, ese objeto carece de sentido en el modelo.

Segundo, el tipo de objeto debe ser **esencial para el sistema**. Por ejemplo, en un sistema de pedidos los _clientes_ son fundamentales, pero los _mozos_ pueden ser útiles sin ser imprescindibles; por lo tanto, quizás no deban representarse como objetos dentro del modelo. Este criterio siempre debe validarse con los usuarios.

Tercero, cada instancia debe poder **describirse mediante datos** (atributos). En el caso de un cliente, estos podrían ser nombre, domicilio, número telefónico o límite de crédito. Estos atributos deben aplicarse a **cada** miembro del tipo de objeto, no a unos sí y otros no.

En la mayoría de los sistemas, los tipos de objetos corresponden a elementos materiales del mundo real, como clientes, productos o empleados. Sin embargo, también pueden representar conceptos no materiales, como horarios, planes o estrategias. Además, una misma entidad del mundo real puede aparecer como distintos tipos de objetos en un modelo, o incluso en el mismo modelo: por ejemplo, Juan Pérez puede ser simultáneamente _empleado_ y _cliente_.

Finalmente, se acostumbra nombrar los objetos con **sustantivos** (cliente, empleado, producto), lo cual es práctico porque existe una correspondencia entre los objetos de un DER y los almacenes en un DFD: si hay un objeto _CLIENTE_ en el DER, debería haber un almacén de _CLIENTES_ en el DFD.


---
### Relaciones

![[Pasted image 20250923145733.png]]

En un diagrama entidad-relación, las **relaciones** conectan los distintos **tipos de objetos** y se representan con un **rombo**. Una relación es un **conjunto de asociaciones** entre instancias de los objetos participantes. Por ejemplo, la relación _**COMPRAS**_ puede incluir que un cliente compre uno o varios artículos, que varios clientes compren el mismo artículo, o incluso que un cliente no compre nada.

Las relaciones reflejan **información que el sistema necesita recordar**, no algo que pueda derivarse automáticamente. Así, si un cliente compró un artículo específico, ese hecho debe almacenarse explícitamente, ya que no podría deducirse por otros medios.

Es posible que existan **múltiples relaciones entre los mismos objetos**. Un ejemplo es la relación entre _**PACIENTE – MÉDICO**_: puede haber una relación de _tratamiento_ y otra de _recibo_, independientes entre sí. También es común que haya **relaciones complejas con múltiples objetos**, como sucede en la compra-venta de una casa, donde intervienen _**cliente – vendedor – agente inmobiliario – abogado del cliente – abogado del vendedor**_. Estas relaciones deben leerse como un todo, y pueden describirse desde la perspectiva de cualquiera de los objetos participantes, todas siendo válidas.

Además, en algunos casos puede haber **relaciones entre instancias de un mismo objeto**. Por ejemplo, un _**CURSO**_ universitario puede estar relacionado con otro a través de la condición de _prerrequisito_.
![[Pasted image 20250923150842.png]]

![[Pasted image 20250923150929.png]]

---
#### Notación alternativa para relaciones

Aunque en el modelo básico de **DER** las relaciones se representan de forma **multidireccional** y sin **cardinalidad** explícita, algunos analistas prefieren notaciones que agregan estos detalles.

Una opción es la **notación de punto de ancla**, donde se define un **objeto primario** desde el cual se lee la relación, y se indica el número de instancias del otro objeto que pueden vincularse. Por ejemplo, un _**CLIENTE**_ puede estar asociado con cero o más _**ARTÍCULOS**_, pero cada instancia de compra corresponde solo a un cliente.

Otra alternativa utiliza **flechas** para señalar cardinalidad: una **flecha simple** indica relación de _uno a uno_, mientras que una **flecha doble** (en forma de “patas de gallo”) representa una relación de _uno a muchos_.

Aun así, se recomienda mantener el **DER** como un esquema **global y limpio**, evitando sobrecargarlo con detalles de cardinalidad o restricciones, ya que esos aspectos pueden documentarse en el **diccionario de datos**.

![[Pasted image 20250923151153.png]]

### Indicadores asociativos de tipo de objeto

Un **indicador asociativo de tipo de objeto** es una notación especial en el DER que funciona **como objeto y como relación al mismo tiempo**. Representa información que se desea **almacenar sobre una relación específica**.

Por ejemplo, la relación _**COMPRA**_ entre un _**CLIENTE**_ y un _**ARTÍCULO**_ normalmente solo asocia instancias de ambos objetos. Sin embargo, si queremos recordar datos como la _hora del día_ en que se realizó la compra o el _descuento aplicado_, esos datos no pertenecen ni al **CLIENTE** ni al **ARTÍCULO**, sino a la **relación misma**. Para reflejar esto, _**COMPRA**_ se representa dentro de un **rectángulo** conectado a un rombo sin nombre, indicando que:

- _**COMPRA**_ funciona como un **tipo de objeto**, permitiendo almacenar información específica de cada instancia de la compra.
- _**COMPRA**_ funciona como una **relación** que conecta a los objetos _**CLIENTE**_ y _**ARTÍCULO**_.

En este caso, la existencia de _**COMPRA**_ depende de que existan las instancias de _**CLIENTE**_ y _**ARTÍCULO**_, pero los objetos por sí mismos pueden existir independientemente. El rombo de la relación se deja sin nombre porque el **indicador asociativo de objeto** ya cumple esa función.

![[Pasted image 20250923151814.png]]
### Indicadores de subtipo/supertipo

Los **indicadores de subtipo/supertipo** permiten modelar un **tipo de objeto general** (supertipo) que tiene una o más **subcategorías** (subtipos), conectadas mediante una relación. Por ejemplo, _**EMPLEADO**_ como supertipo puede tener los subtipos _**EMPLEADO ASALARIADO**_ y _**EMPLEADO POR HORAS**_.

El **supertipo** se describe mediante atributos aplicables a todos los subtipos, como:

- **Nombre**
- **Años de servicio**
- **Domicilio particular**
- **Nombre del supervisor**

Cada **subtipo**, en cambio, se describe con atributos específicos:

- Para _**EMPLEADO ASALARIADO**_:
    - **Salario mensual**
    - **Porcentaje anual adicional**
    - **Aportación para coche de la empresa**
- Para _**EMPLEADO POR HORAS**_:
    - **Paga por hora**
    - **Cantidad por tiempo extra**
    - **Hora de comienzo**

Esta notación permite **diferenciar los tipos de objeto que comparten atributos generales** pero requieren información particular para cada subtipo, facilitando un modelo más preciso y organizado.
![[Pasted image 20250923153402.png]]

## Reglas para la construcción de un DER

El **primer DER** que se construya **no será definitivo**; al igual que los DFD y otras herramientas de modelado, los diagramas E-R deben **revisarse y mejorarse** varias veces. Las primeras versiones suelen ser **borradores**, y se refinan siguiendo reglas que permiten **añadir o eliminar tipos de objetos y relaciones**.

---
### Añadir tipos de objetos adicionales

El primer DER se suele crear a partir de **entrevistas iniciales con los usuarios** y del conocimiento del negocio. Luego, los datos del sistema se **asignan a los tipos de objetos** mediante alguna de estas vías:

1. Si el **DFD** ya existe, el **diccionario de datos** sirve como referencia para asignar datos a los objetos.
2. Si no hay DFD, se deben **entrevistar a los usuarios** para obtener una lista completa de datos y sus definiciones, y asignarlos al DER.
3. Si se trabaja con un **grupo de administración de datos**, probablemente ya exista un diccionario que facilite el proceso.

El proceso de asignación puede generar la creación de **nuevos tipos de objetos** cuando:

- Algunos datos solo aplican a ciertas instancias de un tipo de objeto.
- Algunos datos aplican a todas las instancias de dos objetos distintos, sugiriendo un **supertipo**.
- Algunos datos describen **relaciones entre objetos**, sugiriendo un **tipo asociativo de objeto**.

Por ejemplo, en un sistema de personal, el tipo de objeto _**EMPLEADO**_ puede tener subtipos _**EMPLEADAS**_ y _**EMPLEADOS-VARONES**_ si ciertos datos solo aplican a uno de ellos (como _número de embarazos_).

Del mismo modo, si dos tipos de objeto (por ejemplo, _**CLIENTE-DE-CONTADO**_ y _**CLIENTE-A-CREDITO**_) comparten datos como _nombre del cliente_ o _domicilio_, se puede crear un **supertipo** común para almacenar esos atributos.

![[Pasted image 20250923154248.png]]

---
Si durante la asignación de datos se encuentra que **un dato pertenece a una relación existente** entre dos objetos, se sustituye la relación simple por un **tipo asociativo de objeto**.

Por ejemplo, si en la relación _**COMPRA**_ entre _**CLIENTE**_ y _**ARTÍCULO**_ aparece un dato como _fecha-de-compra_, este dato se asigna al **tipo asociativo de objeto COMPRA**.  

![[Pasted image 20250923154419.png]]
De manera similar, un objeto que inicialmente parece normal puede transformarse en **tipo asociativo** si se descubre que algunos datos dependen de su interacción con otros objetos. Por ejemplo, _**PEDIDO**_ puede ser un tipo de objeto que representa la relación entre _**CLIENTE**_ y _**PRODUCTO**_, y también contener datos como _fecha-de-entrega_.

![[Pasted image 20250923154525.png]]

---
También se deben revisar **objetos que contienen información repetida**, como el caso de _**EMPLEADO**_ con datos sobre hijos (_nombre-del-hijo, edad-del-hijo, sexo-del-hijo_). Esto puede generar un **nuevo tipo de objeto HIJO**, relacionado con _**EMPLEADO**_ mediante una relación.  

![[Pasted image 20250923154801.png]]

Este proceso forma parte de la **normalización**, cuyo objetivo es producir **tipos de objetos claros y consistentes**, donde cada instancia tiene un **valor clave primario** y un conjunto de **atributos independientes** que describen la entidad.

### Eliminar tipos de objetos

Además de **crear objetos y relaciones adicionales**, los refinamientos del DER también pueden llevar a **eliminar tipos de objetos o relaciones** que sean **redundantes o innecesarios**. Las situaciones más comunes son:

1. **Tipos de objetos que consisten solo en un identificador**
2. **Tipos de objetos con una única instancia**
3. **Tipos asociativos de objetos flotantes**
4. **Relaciones derivadas**

---
#### 1. Objetos con solo un identificador

Si un tipo de objeto solo tiene un **identificador** como dato, se puede eliminar y trasladar ese identificador como atributo a un **objeto relacionado**.

**Ejemplo:** En un sistema de personal, si el objeto _**CÓNYUGE**_ solo tiene el nombre como información, se elimina el objeto y se agrega el atributo _nombre-del-cónyuge_ al objeto _**EMPLEADO**_.

- Esto aplica si hay **correspondencia uno a uno** entre instancias del objeto eliminado y el objeto relacionado.  ![[Pasted image 20250923155321.png]]
---
#### 2. Objetos de una única instancia

Si un objeto tiene **una sola instancia** y su única información es el **identificador**, puede eliminarse del DER.

**Ejemplo:** En un hospital, si el objeto _**MEDICAMENTO**_ solo representa _aspirina_, no necesita mostrarse en el diagrama.  

![[Pasted image 20250923155431.png]]

---

#### 3. Tipos asociativos de objetos flotantes

Cuando un objeto asociativo se conecta solo con un tipo de objeto tras eliminar otro objeto, se llama **tipo asociativo flotante**. Aunque poco común, es **permitido en un DER**.

**Ejemplo:** Si tras eliminar _**MEDICAMENTO**_, el tipo asociativo _**TRATAMIENTO**_ queda conectado únicamente con _**PACIENTE**_:  

![[Pasted image 20250923155710.png]]

---

#### 4. Relaciones derivadas

Las relaciones que **pueden calcularse o derivarse automáticamente** no deben aparecer en el DER, ya que este debe reflejar únicamente **datos almacenados**.

**Ejemplo:** La relación _**RENOVAR**_ entre _**CONDUCTOR**_ y _**LICENCIA**_ puede derivarse de la fecha de nacimiento o reglas internas. Por lo tanto, se elimina del diagrama.  

![[Pasted image 20250923155811.png]]


### 12.3 Extensiones al diccionario de datos para diagramas E-R

El **diccionario de datos** debe adaptarse para reflejar los objetos y relaciones del DER.

- Los **objetos del DER** corresponden a **almacenes de datos**.
- Cada tipo de objeto debe incluir los **atributos** y el **campo llave** que identifica de manera única a cada instancia.

**Ejemplo:**
```text
CLIENTES = {CLIENTE}
CLIENTE = @ nombre-del-cliente + domicilio + número-telefónico
```
- El signo `@` indica el **campo llave**.

Además, el diccionario debe definir **todas las relaciones** presentes en el DER, especificando:

1. Significado de la relación en el contexto de la aplicación.
2. Objetos que forman la relación.
3. Cardinalidad (uno a uno, uno a muchos, muchos a muchos).

Ejemplo:
```text
compras = *asociación de un cliente y uno o más artículos*
@ identidad-del-cliente + 1{@ identidad-del-artículo + cantidad-comprada}
```

## Resumen

- El **DER** es útil para sistemas con múltiples objetos y relaciones complejas, enfocándose en los **datos y sus relaciones**, sin considerar las funciones que los crean o usan.
- Aunque el DER se usa gráficamente para modelar relaciones entre datos, existen **varias herramientas alternativas** para el mismo propósito.
- No importa si se desarrolla primero el **DFD** o el **DER**; depende de la naturaleza del sistema, las preferencias del equipo y del usuario:
    - Sistemas ricos en funciones → prioridad DFD.
    - Sistemas ricos en relaciones de datos → prioridad DER.
    - Sistemas de tiempo real → prioridad en el modelo de transición de estados.
- Sistemas modernos y complejos requieren integrar **DFD, DER y modelos de comportamiento de tiempo real**, mientras que sistemas simples pueden enfocarse solo en la herramienta más relevante.