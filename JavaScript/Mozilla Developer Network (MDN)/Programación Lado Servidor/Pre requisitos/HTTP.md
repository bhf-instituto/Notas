## 🔹 ¿Qué es HTTP?
HTTP significa **HyperText Transfer Protocol** (Protocolo de Transferencia de Hipertexto).  
Es un **protocolo de comunicación** que define cómo se transmiten datos en la web, especialmente entre un **cliente** (normalmente un navegador) y un **servidor web**.

👉 Se basa en un **modelo cliente-servidor**:

- El **cliente** hace una **solicitud** (request).
- El **servidor** procesa esa solicitud y envía una **respuesta** (response).

HTTP es **sin estado (stateless)**, lo que significa que cada solicitud es independiente y el servidor no recuerda información de solicitudes anteriores (aunque se puede mantener estado usando **cookies, sesiones o tokens**).

## 🔹 Cómo funciona la transferencia de datos con HTTP

1. **Cliente envía una solicitud HTTP (Request)**  
    Cuando escribes una URL en el navegador o haces clic en un enlace, el navegador construye un  mensaje HTTP que contiene:
    - **Método HTTP**: qué acción quieres hacer (GET, POST, PUT, DELETE, etc.).
    - **Ruta o recurso**: el archivo o endpoint que quieres (ej: `/index.html` o `/api/users`).
    - **Versión del protocolo**: HTTP/1.1, HTTP/2 o HTTP/3.
    - **Cabeceras (Headers)**: información adicional (ej: tipo de navegador, formatos aceptados, cookies, autenticación).
    - **Cuerpo (Body)** _(opcional)_: se envían datos, por ejemplo, en un formulario con método POST.
2. **Servidor procesa la solicitud**  
	El servidor web (ej: Apache, Nginx, o un backend hecho con Node.js y Express) recibe la petición, busca el recurso solicitado o ejecuta la lógica necesaria, y prepara una respuesta.
	```vbnet
		GET /index.html HTTP/1.1 
		Host: www.ejemplo.com 
		User-Agent: Mozilla/5.0
		Accept: text/html
	```
3. Servidor envía una respuesta HTTP (Response)
	El servidor devuelve un mensaje con:
	- **Versión del protocolo**.
	- **Código de estado (status code)**: indica el resultado (ej: `200 OK`, `404 Not Found`, `500 Internal Server Error`).
	- **Cabeceras de respuesta**: metadatos como tipo de contenido (`Content-Type`), longitud, caché, cookies, etc.
	- **Cuerpo (Body)**: el contenido solicitado (ej: HTML, JSON, imagen, etc.).

Ejemplo de una response:

```php 
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1256

<!DOCTYPE html>
<html>
<head><title>Ejemplo</title></head>
<body>
   <h1>Hola, mundo</h1>
</body>
</html>
```


## 🔹 Ciclo completo

1. Cliente (navegador) envía una request →
2. Servidor recibe, procesa y responde →
3. Cliente interpreta la respuesta y muestra la página/datos.

👉 Esto ocurre en **milisegundos** y se repite constantemente con cada recurso (HTML, CSS, JS, imágenes, fuentes, etc.).

---

## 🔹 Métodos HTTP más comunes

- **GET** → Solicitar datos (leer).
- **POST** → Enviar datos (crear).
- **PUT** → Actualizar un recurso existente.
- **DELETE** → Eliminar un recurso.

---

## 🔹 Ejemplo real

Cuando entras a `https://google.com`:

1. El navegador envía un **GET /** al servidor de Google.
2. El servidor responde con un HTML.
3. El navegador lee ese HTML y hace nuevas solicitudes (CSS, JS, imágenes).
4. Finalmente renderiza la página completa en pantalla.

## 📤 Request
### 2. ¿Qué trae `req.url`?

`req.url` es **la parte de la URL después del host y puerto**.
- Si el cliente pide:
```
http://localhost:3000/
```
→ `req.url` será "/".

```
http://localhost:3000/contact?name=juan
```
→ `req.url` será "/contact?name=juan".

### Ejemplo de funciones asincronas
En este caso estamos creando rutas manualmente, no es la mejor idea ya que cada ruta requeriría su propia porción de código. 
```js
const http = require("node:http");
const fs = require("node:fs");
const desiredPort = process.env.PORT || 3000;  

console.log(process.env.PORT);

const processRequest = (req, res) => {
    res.setHeader("Content-Type", "text/html; charset=utf-8");
    //si el cliente pide la carpeta raíz, devuelvo este html.
    if (req.url == "/") {
        res.end('<button style="color:red">Response</button><p>hola loco</p>');
    } else if (req.url == "/image.png") {
        fs.readFile("./image.png", (err, data) => {
            if (err) {
                res.statusCode = 500;
                res.end("<h1>ERROR 500</h1>")
            }
            else {
                res.setHeader("Content-Type", "image/png");
                // acá en realidad estoy devolviendo un buffer de información
                // en el header describo que tipo de dato es para que lo
                // interprete correctamente el navegador al recibir el buffer.
                res.end(data);
            }
        })
    }
    else {
        res.statusCode = 404;
        res.end('<button style="background-color:red">ERROR </button>')
    }
}
const server = http.createServer(processRequest);

server.listen(desiredPort, () => {
    console.log("server listening on port", desiredPort);
})
```
## 📥 Response
### Status Code
#### 🔹 **1xx – Informativos**

El servidor recibió la request y sigue procesándola.
- `100 Continue` → el cliente puede seguir mandando datos.
- (Poco usados en la práctica del día a día).
#### 🔹 **2xx – Éxito**

La request fue recibida, entendida y procesada correctamente.
- `200 OK` → todo salió bien, se devuelve la respuesta normal.
- `201 Created` → se creó un recurso nuevo (ej: al hacer un POST).
- `204 No Content` → todo bien, pero no hay cuerpo en la respuesta.
#### 🔹 **3xx – Redirecciones**

El cliente debe hacer otra acción para completar la request.
- `301 Moved Permanently` → la URL cambió para siempre.
- `302 Found` → redirección temporal.
- `304 Not Modified` → el recurso no cambió, el navegador puede usar la versión en caché.
#### 🔹 **4xx – Errores del cliente**

La request está mal hecha o el recurso no existe.
- `400 Bad Request` → la solicitud es inválida (mala sintaxis, parámetros, etc.).
- `401 Unauthorized` → falta autenticación.
- `403 Forbidden` → autenticado, pero no tiene permiso.
- `404 Not Found` → el recurso no existe.
#### 🔹 **5xx – Errores del servidor**

El servidor falló al procesar la request.
- `500 Internal Server Error` → error genérico en el servidor.
- `502 Bad Gateway` → un servidor intermedio recibió una mala respuesta.
- `503 Service Unavailable` → el servidor está caído o saturado.
- `504 Gateway Timeout` → el servidor intermedio no recibió respuesta a tiempo.