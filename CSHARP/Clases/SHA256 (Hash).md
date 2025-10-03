```c#
    public class User
    {
        public string Name { get; set; }
        public string HashedPass { get; private set; }


        public User (string name)
        {
            Name = name;
        }

        public void setHashedPass (string pass)
        {
            if(string.IsNullOrWhiteSpace(pass) || pass.Length < 4)
            {
                throw new ArgumentException("La contraseña debe tener al menos 4                                               caracteres");
            }


            SHA256 hashAlgoritm = SHA256.Create();
            byte[] passBytes = Encoding.UTF8.GetBytes(pass);
            byte[] hashPassBytes = hashAlgoritm.ComputeHash(passBytes);
            HashedPass = Convert.ToBase64String(hashPassBytes);
        }
    }
```
devuelve: 
```c#
-b 49
-b 50
-b 51
-b 52
-hb 3
-hb 172
-hb 103
-hb 66
-hb 22
-hb 243
-hb 225
-hb 92
-hb 118
-hb 30
-hb 225
-hb 165
-hb 226
-hb 85
-hb 240
-hb 103
-hb 149
-hb 54
-hb 35
-hb 200
-hb 179
-hb 136
-hb 180
-hb 69
-hb 158
-hb 19
-hb 249
-hb 120
-hb 215
-hb 200
-hb 70
-hb 244
```

```csharp
byte[] passBytes = Encoding.UTF8.GetBytes(pass);
```
convierte cada caracter en byte según la codificación UTF-8.

```csharp
byte[] hashPassBytes = hashAlgoritm.ComputeHash(passBytes);
```
el método ComputeHash recibe la contraseña en un arreglo de bytes y le aplica el algortmo SHA256 el cual devuelve un arreglo de 32bytes = 256bits. Cada uno de esteos bytes puede ser un numero entre 0 y 255.

```csharp
HashedPass = Convert.ToBase64String(hashPassBytes);
```
Convierte el arreglo de bytes (binario) 




















Hola profe, quisiera hacerle una consulta acerca del TP2. Yo voy a hacerlo completo para intentar promocionar la materia directamente.  
Mi consulta es respecto al inciso de listas en memoria.   
Quisiera saber si el manejo de información (tanto el renderizado en pantalla, como la carga, edicion y eliminacion de contactos) se puede hacer mediante una lista temporal, que, luego recorriendola me permita guardar esa informacion en el archivo de texto. Pensé que esto podría aplicar no solo a los procesos de lectura, sino a los de escritura, siendo la lista como una especie de agenda temporal que luego refleja la informacion de contactos que contiene en el archivo de texto, en diferentes puntos de la ejecuciion ( al iniciar el programa, luego de crear un contacto, elimnar o editar uno, aqui se registrarian los logs tambien).  

Es decir, yo hago una clase Contacto con sus propiedades(Apellido, nombre, etc),  y una lista List<Contacto> list_agenda = ...; para almacenarlos.  
  
Al  iniciar el programa, se lee el archivo de texto, se parsean correctamente los datos  de contactos que puede (o no) tener el archivo de texto. Convirtiendo cada linea del archivo en un objeto Contacto que se agrega a la lista.
Y cada vez que agrego, edito o elimino un contacto, primero hago esta modificación en la lista y luego sobreescribo el archivo de texto con los contactos que están dentro de la lista (respetando el formato que indicó).   

Me pareció mas practico trabajar con la lista de contactos, hacer todas las modificaciones ahi y luego reflejar esos cambios en el archivo de texto. 

No se si mi forma de encararlo  es correcta, porque en el TP usted dice que "Usar listas en memoria (ej. List<Contacto>) en lugar de leer el archivo en cada operación de lectura o búsqueda..." y yo en este caso estaría escribiendo en el archivo con la información que almacena la lista temporal. Es decir que para eliminar un contacto, primero lo elimino de la lista y luego se sobreescribiría todo el archivo de texto con la modificación ya hecha. 

Espero no ser desubicado preguntandole esto por acá, disculpe las molestias y muchas gracias profe ! 