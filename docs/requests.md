# Requests

En muchas aplicaciones web es normal conectarse a varios servicios de terceros mediante el uso de APIs. Al utilizar estas APIs puedes acceder a datos como información meteorológica, resultados deportivos, listas de películas, tweets, resultados de motores de búsqueda e imágenes entre otros ejemplos. 

También se pueden utilizar las APIs para añadir funcionalidad a una aplicación. Ejemplos de ello son los pagos, la programación, los correos electrónicos, las traducciones, los mapas y las transferencias de archivos. Si tuvieramos que crear cualquiera de ellas por nuestra cuenta, nos llevaría mucho tiempo, pero con las APIs, podemos tardar sólo unos minutos en conectarnos a una y acceder a sus funciones y datos.

En esta sección hablaremos sobre la biblioteca Python Requests, que permite enviar peticiones HTTP en Python.

Y dado que el uso de una API consiste en enviar peticiones HTTP y recibir respuestas, Requests permite utilizar las API en Python.

## Entendiendo las peticiones HTTP

Las peticiones HTTP son la forma de funcionamiento de la web. Cada vez que navegamos por una página web, el navegador realiza múltiples peticiones al servidor web. El servidor responde con todos los datos necesarios para renderizar la página, y el navegador la renderiza para que podamos verla.

El proceso genérico es el siguiente:

+ Un cliente (como un navegador o un script de Python usando Requests) envia algunos datos a una URL.
+ El servidor del sitio web asociado a esa URL leerá los datos, decidirá qué hacer con ellos y devolverá una respuesta al cliente.
+ Finalmente, el cliente puede decidir qué hacer con los datos de la respuesta.

Parte de los datos que el cliente envía en una solicitud es el método de solicitud. Algunos métodos de solicitud comunes son `GET`, `POST` y `PUT`. Las peticiones GET son normalmente para leer datos sólo sin hacer un cambio en algo, mientras que las peticiones `POST` y `PUT` son generalmente para modificar datos en el servidor. 

Cuando se envía una solicitud desde un script de Python o dentro de una aplicación web, el desarrollador puede decidir qué se envía en cada solicitud y qué se hace con la respuesta.

## Instalando requests

Podemos o bien instalar de forma global la biblioteca:

```sh
pipenv install requests
```

O bien, utilizar un entorno virtual a tal efecto. 


Creamos una carpeta para hacer la prueba:

```sh
mkdir prueba_r
```

Dentro de la carpeta, ejecutamos un nuevo entorno virtual:

```sh
pipenv shell
```

Y, una vez dentro, instalamos la biblioteca:

```sh
pipenv install requests
```

## Probando requests

Si entramos en el entorno interactivo de Python:

```
python3
```

Podemos empezar a jugar con la biblioteca. El primer paso será importar requests para poder utilizarlo realizando nuestra primera petición `GET` a una URL e imprimiendo la respuesta (o mejor dicho, su código HTTP):

```python
>>> import requests
>>> r = requests.get('https://raul-profesor.github.io')
>>> print(r)
<Response [200]>
>>> 
```

Por mera curiosidad, vamos a ver qué tipo de dato es `r`:

```python
>>> print(type(r))
<class 'requests.models.Response'>
```

Lo que nos dice es que es una instancia de la clase `Response`. El objeto `Response` contiene el resultado de la petición HTTP.

!!!note "Nota"
    Para los ejemplos utilizaremos el sitio web [httpbin.org] (http://httpbin.org/). La herramienta httpbin es un servicio gratuito y sencillo de petición y respuesta HTTP que proporciona un conjunto de endpoints URL. Utilizamos estos endpoints para probar varias formas de trabajar con operaciones HTTP. Utilizaremos la herramienta httpbin porque nos ayuda a centrarnos en el aprendizaje de la biblioteca Python Requests sin necesidad de configurar un servidor web real o utilizar un servicio web en línea.

### Peticiones GET

Utilizaremos las peticiones `GET` para solicitar datos de un servidor web concreto. Probemos:

```python
url = 'http://httpbin.org/json'
r = requests.get(url)
print('Response Code:', r.status_code)
print('Response Headers:\n', r.headers)
print('Response Content:\n',r.text)
```

Ejecutando el código de arriba obtenemos un códido de estado 200, el cual indica que la URL es alcanzable o accesible. También devuelve los datos de las cabeceras de la página, seguido del contenido de la página.

La propiedad `headers` devuelve un diccionario especial hecho sólo para cabeceras HTTP, de tal forma que podamos acceder a cada elemento simplemente usando su key o clave:

```python
print(r.headers['Content-Type'])
```

Ejecutando el comando de arriba, nos devolverá que el tipo de contenido de la página es `application/json`.

En la última línea del código, podemos utilizar lapropiedad `content` que devuelve el contenido de la página como una serie de bytes, pero preferiremos usar la propiedad `text` que imprime por pantalla dicho contenido como texto decodificado en formato Unicode.


#### Usando parámetros GET

Usamos los parámetros `GET` para pasar información en formato de parejas *clave-valor* hacia un servidor web a través de una URL. El método get nos permite pasar un diccionario de claves-valores usando el argumento `params`. Intentemoslo:

```python
url = 'http://httpbin.org/get'
payload = {
    'website':'dataquest.io',
    'courses':['Python','SQL']
    }
r = requests.get(url, params=payload)
print('Response Content:\n',r.text)

Run the code above. You’ll see the following output:

Response Content:
 {
  "args": {
    "courses": [
      "Python",
      "SQL"
    ],
    "website": "dataquest.io"
  },
  "headers": {
    "Accept": "*/*",
    "Accept-Encoding": "gzip, deflate",
    "Host": "httpbin.org",
    "User-Agent": "python-requests/2.27.1",
    "X-Amzn-Trace-Id": "Root=1-61e7e066-5d0cacfb49c3c1c3465bbfb2"
  },
  "origin": "121.122.65.155",
  "url": "http://httpbin.org/get?website=dataquest.io&courses=Python&courses=SQL"
}
```

El contenido de la respuesta está en formato JSON y el par clave-valor que le pasamos a través del argumento `params`  aparece en la sección `args` de la respuesta. También la sección `url` contiene la URL codificada junto con los parámetros que se le han pasado al servidor.

### Peticiones POST

Usamos la petición POST para enviar datos recogidos de un formulario web a un servidor web. Para llevar a cabo esto con la biblioteca requests, primero necesitamos crear un diccionario de datos y asignarlo al argumento `data` del método post:

```python
url = 'http://httpbin.org/post'
payload = {
    'website':'dataquest.io',
    'courses':['Python','SQL']
    }
r = requests.post(url, data=payload)
print('Response Content:\n',r.text)

Running the code above returns the following response:

Response Content:
 {
  "args": {},
  "data": "",
  "files": {},
  "form": {
    "courses": [
      "Python",
      "SQL"
    ],
    "website": "dataquest.io"
  },
  "headers": {
    "Accept": "*/*",
    "Accept-Encoding": "gzip, deflate",
    "Content-Length": "47",
    "Content-Type": "application/x-www-form-urlencoded",
    "Host": "httpbin.org",
    "User-Agent": "python-requests/2.27.1",
    "X-Amzn-Trace-Id": "Root=1-61e7ec9f-6333082d7f0b73d317acc1f6"
  },
  "json": null,
  "origin": "121.122.65.155",
  "url": "http://httpbin.org/post"
}
```

Esta vez los datos enviados a través del método post aparecen en la sección `form` de la respuesta, en lugar de en `args` porque en vez de enviar los datos como parámetros dentro de la URL, los enviamos como datos codificados de un formulario.

### Manejando excepciones

Cuando nos comunicamos con un servidor remoto, pueden ocurrir excepciones. Por ejemplo, el servidor puede ser inalcanzable, la URL no existir en el servidor, o el servidor no responder en un marco de tiempo adecuado. Veamos como detectar y resolver errores HTTP usando la clase `HTTPError` de la biblioteca requests:

```python
import requests
from requests import HTTPError

url = "http://httpbin.org/status/404"
try:
    r = requests.get(url)
    r.raise_for_status()
    print('Response Code:', r.status_code)
except HTTPError as ex:
    print(ex)
```

En el código de arriba, importamos la clase `HTTPError` para capturar y resolver las excepciones HTTP. Tras ello, realizamos una petición a un endpoint en `httpbin.org`, el cual genera un código de estado 404. El método `raise_for_status()` genera una excepción siempre que la respuesta HTTP contenga un códio de error (un error de cliente del tipo 4XX o una respuesta de error del servidor del tipo 5XX). 

La biblioteca requests no genera una excepción automáticamente una vez ocurre un error. Es por ello que necesitamos usaro el método `raise_for_status()` para identificar si un código de estado de error ha ocurrido o no. Finalmente, el manejador de excepciones bloquea la captura del error y lo imprime tal que así:

```python
404 Client Error: NOT FOUND for url: http://httpbin.org/status/404
```---
**NOTE**

If you try to access the <code>http://httpbin.org/status/200</code> endpoint, the code above outputs <code>Response Code: 200</code> because the status code is not in the range of error status codes. The <code>raise_for_status()</code> method will return <code>None</code>, which won't trigger the exception handler.
- - - -
In addition to the exception that we just discussed, let’s see how we can resolve server timeouts. It's crucial because we need to ensure our application doesn’t hang indefinitely. In the Requests library, if a request times out, a *Timeout*  exception will occur. To specify the timeout of a request to a number of seconds, we can use the <code>timeout</code> argument:
```py
import requests
from requests import Timeout
url = "http://httpbin.org/delay/10"

try:
    r = requests.get(url, timeout=3)
    print('Response Code:', r.status_code)
except Timeout as ex:
    print(ex)

Let’s first discuss the code. We import the Timeout class to resolve the timeout exceptions and provide a URL that refers to an endpoint with a 10-second delay to test our code. Then, we set the timeout argument to 3 seconds, which means the get request will throw an exception if the server doesn’t respond in 3 seconds. Finally, the exception handler catches the timeout error, if any, and prints out it. So running the code above outputs the following error message because the server won’t respond in the given time.

HTTPConnectionPool(host='httpbin.org', port=80): Read timed out. (read timeout=3)
```## Authentication
The Requests library supports various web authentications, such as basic, digest, the two versions of OAuth, etc. We can use these authentication methods when we want to work with any data sources that require us to be logged in.

We will implement the basic authentication using HTTPBin service in the following example. The endpoint for Basic Auth is /basic-auth/{*user*}/{*password*}. For example, if you use the following endpoint . . .

http://httpbin.org/basic-auth/data/quest

. . . you can authenticate using the username *'data'* and the password *'quest'* by assigning them as a tuple to the <code>auth</code> argument. Once you authenticate successfully, it responds with JSON data <code>{ "authenticated": true, "user": "data"}</code>.
```py
import requests
r = requests.get('http://httpbin.org/basic-auth/data/quest', auth=('data', 'quest'))
print('Response Code:', r.status_code)
print('Response Content:\n', r.text)

Run the code above. It outputs as follows:

Response Code: 200
Response Content:
 {
  "authenticated": true,
  "user": "data"
}

If we make either the username or password incorrect, it outputs as follows:

Response Code: 401
Response Content:

Conclusion

We learned about one of the most powerful and downloaded Python libraries. We discussed the basics of HTTP and different aspects of the Python Requests library. We also worked with GET and POST requests and learned how to resolve the exceptions of the requests. Finally, we tried to authenticate through the basic authentication method in a web service.

## Referencias 

[Documentación de biblioteca requests](https://requests.readthedocs.io/en/latest/user/quickstart/)

[Tutorial: An Introduction to Python Requests Library](https://www.dataquest.io/blog/tutorial-an-introduction-to-python-requests-library/)