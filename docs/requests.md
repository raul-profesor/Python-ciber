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