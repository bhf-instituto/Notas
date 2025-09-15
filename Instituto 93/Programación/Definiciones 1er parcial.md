### Clase 
1. Una **clase** es una estructura de datos que combina estados (campos) y acciones (métodos y otros miembros de función) en una sola unidad. Una **clase** proporciona una definición para instancia de la **clase** también conocidas como objetos.
2. Una clase es una plantilla en la que nos basamos para crear un **objeto**, y un **objeto** es una instancia de la **clase** a la que pertenece. Existen dos tipos de objetos: los concretos (C#: Consola, JS: Window) y los abstractos (las que no se pueden instanciar con *new*).
	* `static`: toda clase que se usa sin necesidad de realizar una instanciación de la misma. Se utiliza como una unidad de organización para métodos no asociados a objetos particulares.  
	* `abstract`: son clases que no se pueden instanciar con *new*,  solo podes crear objetos de la clase que **hereden** de ella. Indica que lo que se modifica carece de implementación o tiene una implementación incompleta. indicar que una clase está diseñada como clase base de otras clases, no para crear instancias por sí misma. Los miembros marcados como abstractos deben implementarse con clases no abstractas derivadas de la clase abstracta.
		* Ej: en mi programa RPG_CONSOLE creé una clase abstracta *Character* donde están definidas las propiedades y métodos generales que tendrán todo los personajes  (Attack(), Damage(), IsAlive(), Name, Level, Attributes, etc), pero luego cada personaje tiene sus habilidades propias, entonces creo una clase no abstracta *Fighter* la cual **hereda** de la clase *Character* y además agrega propiedades y métodos propios.

### For
Los bucles for van asignando valores a una variable desde un valor inicial hasta un valor final, y cuando la variable contiene un valor que está fuera del intervalo el bucle termina. 

### While
Un bucle while se repetirá **mientras**  una condición determinada se cumpla, o sea devuelva true. 

### Try - Catch - Finally
Es una estructura de control de excepciones en c#, se utiliza para manejar errores (excepciones) que pueden ocurrir durante la ejecución de un programa, y permitir que el programa continúe de manera controlada, incluso si ocurre un error. 
1. **try**: En este bloque escribimos el código que puede generar una excepción. Si ocurre un error, el flujo de ejecución pasa al bloque catch. 
2. **catch**: SI ocurre un error en el bloque **try**, el código dentro del bloque catch se ejecuta. Aquí manejamos el error, mostrando un error o tomando alguna acción para tratar el problema.
3. **finally**: Este bloque es opcional y se ejecuta **siempre**, haya ocurrido una excepción o no. Es útil para liberar recursos o hacer tareas de limpieza, como cerrar archivos o liberar conexiones a base de datos, sin importar si hubo un error o no. 
**Algunas excepciones**:
* **DivideByZeroException: Si el divisor es 0, se lanza una excepción. 
* **FormatException**: Excepción de tipo de datos, cuando se ingresa un tipo de dato que no es el correcto. 

### Funciones 
* Son algoritmos separados del cuerpo principal del programa que realizan tareas específicas.
* Hacen que el programa se mas modular.
* Ayudan a los programadores a ser ordenados en su código y durante todo el desarrollo.
* Solo deben ser depuradas una vez ya que una vez funcionales no es necesario volver sobre ellas sino que simplemente se las usa.
* Ayudan al seguimiento y corrección de errores. 

#### Tipos: 
1. `void`: no retorna ningún valor.
```csharp
public static void Saludar()
{
	Console.WriteLine("Hola !");
}
```

2. Recibe parámetros de entrada: 
```csharp
public static Saludar(string nombre)
{
	Console.WriteLine("Hola " + nombre + " !" );
}
```

3. Retorna un valor: 
```csharp
public static string DevuelveSaludo()
{
	return "Hola !";
}
```



### Listas

En c# se las conoce como colecciones de objetos: Las colecciones de clases por ejemplo son un conjunto de clases diseñadas específicamente para agrupar objetos y llevar a cabo tareas con ellos. 

* Tienen una ventaja frente a los arrays y es que las listas son dinámicas.

```csharp
List<int> nombreLista = new List<int>();

// forma antigua
ArrayList nombre = new ArrayList();
```

Para agregar elementos al final de la lista `.Add(elemento)`:  
```csharp
List<int> numeros = new List<int>();

numeros.Add(9); // ← ←
numeros.Add(2);
```

Para insertar elementos  en una ubicación especifica `.Insert(posición, elemento)`:
```csharp
numeros.Insert(2, 123) // agrego en la 3er posición el número 123

// y para ordenar elementos 
numeros.Sort();
```

Para buscar el índice de un elemento`.IndexOf(elemento)`: 
```csharp
List<string> nombres = new List<string>();
nombres.Add("Mirko");
nombres.Add("Komir");
nombres.Add("Irkom");

Console.WriteLine(nombres.IndexOf("Irkom")); // devuelve 2
```

Una forma de cambiar el valor de un elemento dentro de una lista de el cual no conozco la posición :
```csharp 
List<string> colores = new List<string>();
// supongamos que tiene agregados muchos colores y quiero cambiar "Amarillo" por "Negro" pero no se en que posició nestá "Amarillo" entonces:

colores[colores.IndexOf("Amarillo")] = "Negro";

```

Si tenemos elementos repetidos y queremos obtener el último de ellos podemos utilizar `.LastIndexOf()`.

#### Propiedades: 
`.Capacity` nos dice cuantos elementos puede almacenar una colección sin tener que cambiar el tamaño.
`.Count` nos dice la cantidad de elementos que posee nuestra lista.
`.TrimExcess()` método que borra los elementos excedentes entre `Capacity` y `Count`, es decir los elementos que se exceden de la capacidad de la lista. 
`.Clear()` borra todos los elementos de una lista y vuelve el valor de `Count` a 0, pero no así el valor de `Capacity`, este valor se elimina con `.TrimExcess()`
