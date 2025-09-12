- Siempre devuelve un objeto **Response**  primero.
- Para acceder al contenido real hay que usar métodos como `response.json()`, `response.text()`, etc. 
 ```js
 fetch('https://fakestoreapi.com/products')
  .then(response => response.json())
  .then(data => console.log(data));
 ```
1. `fetch` hace una petición HTTP **asincrónica** al servidor.
2. El primer `.then(response => response.json())` toma el **objeto Response** que devuelve `fetch` y lo convierte en JSON.
    - `response.json()` también devuelve una **promesa**, porque convertir la respuesta a JSON puede tomar tiempo.    
3. El segundo `.then(data => console.log(data))` recibe el **contenido real del JSON**, que en este caso es un **array de productos**.
Ej. de salida 
```js
[
  { id: 1, title: 'Fjallraven - Foldsack No. 1 Backpack', price: 109.95, ... },
  { id: 2, title: 'Mens Casual Premium Slim Fit T-Shirts', price: 22.3, ... },
  ...
]
```

### Fetch con Async Await
```js
async function getProducts(){
  try{
      const res = await fetch('https://fakestoreapi.com/products');
      if(!res.ok) throw new Error("Error de respuesta, not ok 404 (?");
      const data = await res.json();
      return data;
  }catch(err){
    console.log(err)
    return []; // fallback
  }
}

const products = await getProducts();
const mensClothing = products.filter(p => p.category == "men's clothing"); //filtro
```
**Fallback**: un valor seguro y predecible que le permite al resto de tu aplicación seguir funcionando sin errores graves.