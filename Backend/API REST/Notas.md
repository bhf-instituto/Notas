#### String de conexión
```csharp
private readonly string connectionString = "server=localhost;user=root;password=TU_PASSWORD;database=prueba_api";
```


#### ASP.NET Conexión por consola a DB
```
Scaffold-DbContext "Data Source=(localdb)\MSSQLLocalDB; Initial Catalog=DBCrud;Integrated Security=True;Trusted_Connection=True;TrustServerCertificate=True;" Microsoft.EntityFrameworkCore.SqlServer -OutPutDir Models
```

Models es la carpeta de destino, en la cual se guarda el contexto de la DB y el modelo que viene de la tabla `Empleado` de la DB`DBCrud`



# [[API]] (App Programming Interface)
![[Pasted image 20250730142127.png]]

Es un conjunto de reglas , y definiciones  que permite a una aplicación interactuar con otra.

Para esto se usa JSON (JS Object Notation) que es un formato que lo puede entender cualquier lenguaje de programación, es el nexo entre todos. Otro formato que se usa para lo mismo es **xml** 
(Extensible Markup Languaje)
Las API permiten y facilitan la comunicación entre diferentes sistemas de software , entre diferentes aplicaciones y lógicas.

## END POINTS

Son rutas especificas por las cuales la API puede ser accedida. https://pokeapi.co/api/v2/pokemon/pikachu  /pikachu es el endpoint

Los endpoints pueden tener varios usos como por ejemplo tener uno para mostrar todos los productos y otro para mostrar los productos en stock: 
- /productos                     
- /productos/stocked


# REST
### Representational State Transfer
Es un conjunto principios y restricciones que guían como se debe estructurar y comunicar una API.


- **Representational** = Significa que los recursos que pueden ser datos o funcionalidades  se representan en diferentes formatos como JSON, XML, HTML, etc. Siempre estamos manejando una representación del recurso, no el recurso en si. *Por ejemplo, si el cliente pide los datos de un producto mediante el método GET del protocolo HTTP a un servidor, este le devuelve una representación de esos datos en el formato de representación JSON, XML, etc. *

- **State Transfer** = Los estados son los datos en un **momento dado.** *Ej.: Un carrito de compras, los datos de un usuario, etc.* Lo que quiere decir es que se transfiere el estado actual en el momento que se hace la petición, está ligado a eso. 


Pueden ser STATELESS o STATEFUL, esto indica si la API tiene ,o no, un estado anterior y/o actual. 