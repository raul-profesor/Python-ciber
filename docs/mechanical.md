# Mechanical Soup

## Introducción

MechanicalSoup es un paquete de Python que, de forma automática, envía cookies, sigue redirecciones y también puede seguir hiperenlaces o formularios de un sitio web.

Fue creado por [M. Hickford](https://github.com/hickford/) debido a su fascinación por la biblioteca Mechanize. Mechanize fue un proyecto de John J. Lee que permitia navegar de forma programática en Python.

Algunas de las características de Mechanize eran:

+ `mechanize.browser`: que usa `urllib2.OpenerDirector` para abrir una URL de Internet
+ Completado fácil de formularios HTML
+ Revisión automática de *robots.txt*
+ Manejo automática de `HTTP-Equiv`
+ Métodos de navegador `.back()` y `.reload`

Desafortunadamente, Mechanize no es compatible con Python3 y, además, su desarrollo lleva años estancado.

En este contexto, Hickford emergió con una solución: **MechanicalSoup** que provee la misma API y que está construido sobre las bibliotecas `Requests` (para las sesiones HTTP) y `BeautifulSoup` (para datos de navegación). 

MechanicalSoup está diseñado para imitar el comportamiento de los humanos cuando interactúan con los navegadores web. Algunos de los posibles casos de uso incluyen:

+ Interactuar con sitios web que no proporcionan API
+ Hacer pruebas de un sitio web que se esté desarrollando
+ Navegar a través de una interfaz

## Instalación y comprobación

Lo primero que deberemos hacer, como siempre, es instalar MechanicalSoup. Podemos hacerlo en nuestro entorno virtual:

```python
pipenv shell
pipenv install mechanicalsoup
```

O, de forma general, en nuestra máquina:

```python
pipenv install mechanicalsoup
```
Comprobemos que no tenemos errores con nuestra nueva biblioteca. Para ello, en el entorno interactivo de Python3:

```python
import mechanicalsoup

browser = mechanicalsoup.StatefulBrowser()
url = "http://httpbin.org"

browser.open(url)
print(browser.get_url())
```

En el código de arriba:

1. Importamos la biblioteca
2. `mechanicalsoup.StatefulBrowser()`creará un objeto de navegador. 
3. `browser.open()` brirá la página deseado en segundo plano y devolverá un `response[200]` que es el valor devuelto por `open()`.
    MechanicalSoup usa la biblioteca *requests* para realizar las peticiones HTTP a los sitios.
4.  `browser.get_url()` recoge la URL del sitio y también usa *requests*

## Trabajar con MechanicalSoup

Siguiendo con el ejemplo anterior, podemos llevar a cabo más acciones, como por ejemplo seguir subdominios de la siguiente forma:

```python
browser.follow_link("forms")
browser.get_url()
```

Ahora podemos extraer el contenido de la página:

```python
browser.get_current_page()
```

que devolverá el código fuente de la página actual de la misma forma que lo hace la función `prettify` de beautifulsoup. Recordemos que MechanicalSoup utiliza BeautifulSoup para la extracción de datos.

Para encontrar cualquier etiqueta, podríamos buscarla así:

```python
browser.get_current_page().find_all('legend')
```
### Formularios

También podemos encontrar los formularios y hacer peticiones POST con el siguiente comando:

```python
browser.select_form('form[action="/post"]')
browser.get_current_form().print_summary()
```

+ Con `select_form()` utilizamos un selector CSS, seleccionamos el tag HTML **form** que tiene al atributo `action` y cuyo valor es `/post`. 
+ `print_summary` nos mostrará todos los campos disponibles del formulario


Para rellenar el formulario, podemos utilizar los siguientes comandos:

```python
browser["custname"] = "Mohit"
browser["custtel"] = "9081515151"
browser["custemail"] = "mohitmaithani@aol.com"
browser["comments"] = "please make pizza dough more soft"
browser["size"] = "large"
browser["topping"] = "mushroom"
```

Y, posteriormente, podemos ejecutar el navegador con:

```python
browser.launch_browser()
```

+ La sintaxis `browser["marcador"] = "texto"` se usa para rellenar un formulario
+ `browser.launch_browser()` mostrará el resultado en tiempo real

Y con esto damos por presentados los fundamentos más básicos de MechanicalSoup. Para una documentación en profundidad acerca de su funcionamiento y opciones, podemos consultar [aquí](https://mechanicalsoup.readthedocs.io/en/stable/mechanicalsoup.html).

## Haciendo scraping de imágenes de hackers


import mechanicalsoup
 
browser = mechanicalsoup.StatefulBrowser()
url = "https://www.google.com/imghp?hl=en"
 
browser.open(url)
 
#get HTML
browser.get_current_page()
 
#target the search input
browser.select_form()
browser.get_current_form().print_summary()
 
#search for a term
search_term = 'cat'
browser["q"] = search_term 
 
#submit/"click" search
browser.launch_browser()
response = browser.submit_selected()
 
print('new url:', browser.get_url())
print('response:\n', response.text[:500])

#open URL
new_url = browser.get_url()
browser.open(new_url)
 
#get HTML code
page = browser.get_current_page()
all_images = page.find_all('img')
 
#target the source attributes of image
image_source = []
for image in all_images:
    image = image.get('src')
    image_source.append(image)
 
image_source[5:25]

#save cleaned links in "image_source"
image_source = [image for image in image_source if image.startswith('https')]

print(image_source)


import os
 
path = os.getcwd()
path = os.path.join(path, search_term + "s")
 
#create the directory
os.mkdir(path)
#print path where cats images are going to save
path


##download wget by uncommenting below line
#pip install wget  

##download images
counter = 0
for image in image_source:
    save_as = os.path.join(path, search_term + str(counter) + '.jpg')
    wget.download(image, save_as)
    counter += 1

## Referencias

[MechanicalSoup tutorial - First contact, step by step](https://mechanicalsoup.readthedocs.io/en/stable/tutorial.html)