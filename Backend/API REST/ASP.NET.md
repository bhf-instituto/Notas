
Esto te devuelve una lista con todas las plantillas de creación de proyecto ASP.NET  
```
dotnet new List
```

Proyecto ASP.NET vacío, para tener la configuración mínima.
``` 
dotnet new web
```

Tenemos que crear [[DTOs]] para poder transferir correctamente la información y las peticiones. Esto funciona bien con los `records` de C# ya que los [[DTOs]] sirven para llevar data en formato JSON de un punto a otro sin ser modificado en el proceso.

Las [[DTOs]] deben ser  registros, es decir **Record**. 

Un `record` en C# es como una clase, pero:

- ✅ Está pensado para **representar datos**.
- ✅ Tiene **igualdad estructural** (dos records con los mismos valores son "iguales").
- ✅ Soporta fácilmente la **inmutabilidad** (los valores no cambian una vez creados).          ←←←←
- ✅ Tiene soporte para **`with` expressions**, para copiar con cambios.

```c#
public record Persona(string Nombre, int Edad);
```


### Creando un Endpoint

#### Endpoint GET
```c#
app.MapGet("saludos", () => "holas"); 
```
Se **accede** con:
	`http://rutalocal:puerto/endpoint` 
un ejemplo es : 
	`http://localhost:5000/saludos`

Supongamos que tengo una lista creada de elementos, en este caso [[DTOs]] que representan juegos, y quiero que cuando el cliente haga una petición **GET** al Endpoint `http://localhost:5000/juegos` este le devuelva la lista de juegos.

Primero creamos el `record` para representar los DTO.
```c#
public record class GameDto(
    int Id,
    string Name,
    string Genre,
    decimal price,
    DateOnly ReleaseDate);
```

Luego creo la lista y el endpoint.
```c#
List<GameDto> games = [
		    new (
				    1, 
				    "Street Fighters II", 
				    "Fight", 
				    19.99M, 
				    new DateOnly(1992, 7, 15)),
		    new (
				    2, 
				    "The Legend of Zelda: Ocarina of Time", 
				    "Adventure", 
				    29.99M, 
				    new DateOnly(1998, 11, 21))
];

app.MapGet("juegos", ()=> games)
```
Ahora, cuando se haga un **GET** a `http://localhost:5000/saludos` este devolverá el JSON correspondiente a esa lista.

Si  ahora quiero acceder a uno solo de los juegos, debería crear un endpoint el cual acepte un número como "parámetro" o como "ruta" por ej.:  `http://localhost:5000/games/1


```csharp
	app.MapGet("games/{id}", (int id) => games.Find(game => game.Id == id))
```
Básicamente creo el endpoint, al cual le doy la ruta "games/{id}" ahí recibiré el id del ítem al que quiero obtener.  La obtención se maneja a través de una función que pide un id, luego se busca dentro de la `Lista<GameDto> games` con el método `.Find()` al cual se le dice que traiga al item cuyo Id es igual a la id que se le está pasando a la función.