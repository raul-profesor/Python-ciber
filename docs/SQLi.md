Vamos a escribir un script que detecte la forma más simple de inyección SQL a modo de prueba de concepto. Probará a añadir una comilla o dobles comillas en la URL y parsear la respuesta en busca de algún error de base de datos, lo cual nos permite deducir que puede haber una potencial vulnerabilidad SQLi.

Así las cosas, sigamos una serie de pasos para construir por partes nuestro script. Lo primero sería iniciar nuestro entorno virtual e instalar ahí las dependencias que necesitaremos si es que no las teníamos todavía:

```sh
pipenv shell
pipenv install requests bs4
```

Ahora, debemos importar los módulos que vamos a necesitar en nuestro script:

```py
import requests
from bs4 import BeautifulSoup as bs
from urllib.parse import urljoin
from pprint import pprint

s = requests.Session()
s.headers["User-Agent"] = "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.106 Safari/537.36"
```

+ `requests` para realizar las peticiones HTTP
+ `BeautifulSoup` para parsear el contenido de las páginas web
+ [`urllib.parse`](https://rico-schmidt.name/pymotw-3/urllib.parse/index.html) para dividir las URLs en fragmentos y [`urljoin`](https://docs.python.org/es/3/library/urllib.parse.html#urllib.parse.urljoin) permite construir URLs
+ [`pprint`](https://docs.python.org/es/3/library/pprint.html) para imprimir datos de forma "bonita"

También inicializamos la sesión de `requests` y establecemos el user-agent del "navegador".

Puesto que la inyección SQL se basa en las entradas o inputs del usuario, necesitaremos extraer primero los formularios de la web. 

```python
def get_all_forms(_______): # (1)
    soup = bs(s.get(_______).content, "_______")
    return soup.find_all("form")

def get_form_details(_______): # (2)    
    details = {}
       
    try: # (3)
        action = form.attrs.get("action").lower()
    except:
        action = None

    method = form.attrs.get("method").lower() # (4)

    inputs = [] # (5)
    for input_tag in _______.find_all("input"):
        input_type = input_tag.attrs.get("type", "text")
        input_name = input_tag.attrs.get("name")
        input_value = input_tag.attrs.get("value", "")
        inputs.append({"type": input_type, "name": input_name, "value": input_value})

    details["action"] = action # (6)
    details["method"] = method
    details["inputs"] = inputs
    return _______
```

1. Dada una 'url'. devuelve todos los formularios  que contiene el código HTML
2. Esta función extrae toda la posible información útil de un formulario HTML
3. Se extrae el atributo [form action](https://www.w3schools.com/tags/att_form_action.asp) de la URL objetivo
4. Obtenemos el método HTTP utilizado por el formulario (POST, GET ...)
5. Se obtienen todos los detalles de los datos de entrada, tales como el tipo y el nombre
6. Guardamos todos los resultados en un diccionario

`get_all_forms()` utiliza **BeautifulSoup** para extraer todas las etiquetas/tags del código HTML y devolerlas en forma de lista, mientras que la función `get_form_details()` recibe una etiqueta única del formulario como argumento y parsea la información útil del mismo.

A continuación definiremos una función que nos dirá si una página contiene errores SQL, lo cual nos será útil para comprobar si existe una vulnerabilidad del tipo SQL injection.


```python
def es_vulnerable(_______): # (1)
    errors = { 
        # MySQL
        "you have an error in your sql syntax;",
        "warning: mysql",
        # SQL Server
        "unclosed quotation mark after the character string",
        # Oracle
        "quoted string not properly terminated",
    }
    for error in errors: 
        # Si se encuentra algunos de estos errores, devuelve True
        if error in respuesta.content.decode().lower():
            return True
    # No se ha detectado error
    return False
```

1. Una simple función booleana que determina si una página es vulnerable a un SQL injection analizando su respuesta
   
Obviamente no podemos definir errores para todos los servidores de bases de datos. Para abarcar el máximo número de posibilidades de tipos de error, deberíamos hacer uso de las [expresiones regulares](https://www.adictosaltrabajo.com/2015/01/29/regexsam/).

```python
def scan_sql_injection(_______):
    # probar en la URL
    for c in "\"'":
        # añadir comillas o dobles comillas a la URL
        new_url = f"{url}{c}"
        print("[!] Probando", new_url)
        # "fabricamos" la petición HTTP
        res = s.get(_______)
        if es_vulnerable(res): #(1)
            print("[+] Vulnerabilidad SQL Injection detectada, link:", _______)
            return
    # probamos los fomularios HTML
    forms = get_all_forms(_______)
    print(f"[+] Detectados {len(forms)} formularios en {url}.")
    for form in _______:
        form_details = get_form_details(_______)
        for c in "\"'":
            # los datos del cuerpo de la petición que queremos enviar
            data = {}
            for input_tag in _______["inputs"]:
                if input_tag["value"] or input_tag["type"] == "hidden": # (2)
                    try:
                        data[input_tag["name"]] = input_tag["value"] + c
                    except:
                        pass
                elif input_tag["type"] != "submit":
                    #todos los tipos excepto *submit*, usando datos basura como 
                    #caractéres especiales
                    data[input_tag["name"]] = f"test{c}"
            url = urljoin(_______, form_details["action"]) # (3)
            if form_details["method"] == "post":
                res = s.post(_______, data=data)
            elif form_details["method"] == "get":
                res = s.get(_______, params=data)
            # comprobar si la página resultante es vulnerable
            if es_vulnerable(res):
                print("[+] Vulnerabilidad SQL Injection detectada, link", url)
                print("[+] Formulario:")
                pprint(form_details)
                break   
```

1. Se ha detectado un SQLi en la misma URL, no es necesario proceder más allá para extraer formularios y enviarlos
2. Cualquier *input* del formulario que tiene algún valor u oculto, sólo para usarlo en el cuerpo de la petición
3.  Se junta (join) la URL con el `action` (la URL de la petición del formulario)
   
Antes de extraer los formularios y enviarlos, la función de arriba comprueba primero si hay vulnerabilidad en la misma URL. Esto lo hace simplemente añadiendo una comilla `'` a la URL.

Tras ello se utiliza la biblioteca `requests` para realizar la petición y se compreba si el contenido de la respuesta contiene los errores que estamos buscando.

A continuación, "parseamos" los formularios y hacemos "submit" de cada uno de los que hemos encontrado añadiendo las comillas.

Así pues, el `main` de nuestro script queda así:

```python
if __name__ == "__main__":
    import sys
    url = sys.argv[1]
    _______ ()
```




# Referencias

[What is an SQLi?](https://portswigger.net/web-security/sql-injection)

[How to Build a SQL Injection Scanner in Python](https://www.thepythoncode.com/article/sql-injection-vulnerability-detector-in-python)

