# Diccionarios

Un diccionario es un conjunto de parejas **clave- valor** (key-value). Es decir, se accede a cada elemento a partir de su clave. Se definen de la siguiente manera:

```python
estudiante = {
	"nombre": "Martí Macià",
	"edad": 30,
	"nota_media": 7.25,
	"repetidor" : False
}
```

Las **claves tienen que ser únicas** y estar formadas por un **string o un número**. Para acceder al valor de una clave existen dos maneras distintas:

```python
# Acceder al valor de una clave
edad = estudiante["edad"] # devuelve el valor de 'edad'
nota_media = estudiante.get("nota_media") # devuelve el valor de 'nota_media'

# Insertar o actualizar un valor:
estudiante["edad"] = 25 # actualiza el valor de 'edad'
estudiante["suspensos"] = 3 # inserta una nueva pareja clave - valor

# insertar una pareja clave - valor o actualizar si ya existe:
estudiante.update({'aprobados':'8'})
```

Algunos de los métodos más utilizados son los siguientes:

| Método | Acción |
|--|--|
| `diccionario.keys()` | Devuelve todas las claves del diccionario |
| `diccionario.values()` | Devuelve todos los valores del diccionario |
| `diccionario.pop(clave[,<default>])` | Elimina la clave del diccionario y devuelve su valor asociado. Si no la encuentra y se indica un valor por defecto, devuelve el valor por defecto indicado. |
| `diccionario.clear()` | Vacía el diccionario |
| `clave in diccionario` | Devuelve True si el diccionario contiene la clave o False en caso contrario. |
| `valor in diccionario.values()` | Devuelve True si el diccionario contiene el valor o False en caso contrario. |

## Recorrer un diccionario
La forma más habitual de recorrer un diccionario es mediante la sentencia `for`. Al recorrer un diccionario, por defecto se iterará sobre sus claves:

```python
diccionario =  {'a':1,  'b':2,  'c':3}
for key in diccionario:
	print(key)

# Resultado: a b c
```
Es decir, el código anterior será equivalente al siguiente:
```python
diccionario =  {'a':1,  'b':2,  'c':3}
for key in diccionario.keys():
	print(key)

# Resultado: a b c
```
Por lo tanto, para iterar accediendo a los valores, realizaremos lo siguiente:

```python
diccionario =  {'a':1,  'b':2,  'c':3}
for key in diccionario:
	print(diccionario[key])

# Resultado: 1 2 3
```
Otro manera alternativa sería empleando la función `items()`, la cual devuelve el diccionario como tuplas de tipo (key,value):
```python
diccionario =  {'a':1,  'b':2,  'c':3}
for key, value in diccionario.items():
	print("El valor de %s is %d" % (key, value))

# Resultado:
# El valor de a is 1
# El valor de b is 2
# El valor de c is 3
```

## Borrar un elemento

Para borrar un elemento de un diccionario se utiliza la instrucción `del`.
```python
edades = {
   "Ana" : 22,
   "Jordi" : 27,
   "Aitor" : 15
}
del edades["Aitor"]
```
Otra alternativa también utilizada y mencionada anteriormente es la función `pop()`, el cual devuelve el valor del elemento eliminado:

```python
edades = {
   "Ana" : 22,
   "Jordi" : 27,
   "Aitor" : 15
}
edades.pop("Aitor")
```
Un diccionario nunca debería contener dos claves iguales. No obstante, en caso de contener una clave repetida, tanto `del` como `pop()` eliminarán todas las claves coincidentes.

## Diccionario como un conjunto de contadores

Supongamos que recibes una cadena y quieres contar cuántas veces aparece cada letra. Hay varias formas de hacerlo:

1. Puedes crear 26 variables, una por cada letra del alfabeto. Luego puedes recorrer la cadena, y para cada caracter, incrementar el contador correspondiente, probablemente utilizando varios condicionales.
   
2. Puedes crear una lista con 26 elementos. Después podrías convertir cada caracter en un número (usando la función interna `ord`), usar el número como
índice dentro de la lista, e incrementar el contador correspondiente.

3. Puedes crear un diccionario con caracteres como claves y contadores como los valores correspondientes. La primera vez que encuentres un caracter,
agregarías un elemento al diccionario. Después de eso incrementarías el valor del elemento existente.

Cada una de esas opciones realiza la misma operación computacional, pero cada una de ellas implementa esa operación de forma diferente.

Una **implementación** es una forma de llevar a cabo una operación computacional; algunas implementaciones son mejores que otras. Por ejemplo, una ventaja de la
implementación del diccionario es que no tenemos que saber con antelación qué letras aparecen en la cadena y solamente necesitamos espacio para las letras que
sí aparecen.

Aquí está un ejemplo de como sería ese código:

```python
palabra = 'brontosaurio'
d = dict()
for c in palabra:
	if c not in d:
		d[c] = 1
	else:
		d[c] = d[c] + 1
print(d)
```

Realmente estamos calculando un histograma, el cual es un término estadístico para un conjunto de contadores (o frecuencias).

El bucle for recorre la cadena. Cada vez que entramos al bucle, si el caracter c no está en el diccionario, creamos un nuevo elemento con la clave c y el valor inicial 1 (debido a que hemos visto esta letra solo una vez). Si c ya está previamente en el diccionario incrementamos d[c].
Aquí está la salida del programa:

```python
{'b': 1, 'r': 2, 'o': 3, 'n': 1, 't': 1, 's': 1, 'a': 1, 'u': 1, 'i': 1}
```

El histograma indica que las letras “a” y “b” aparecen solo una vez; “o” aparece dos, y así sucesivamente.

Los diccionarios tienen un método llamado `get` que toma una clave y un valor por defecto. Si la clave aparece en el diccionario, `get` regresa el valor correspondiente; sino, devuelve el valor por defecto. Por ejemplo:

```python
>>> cuentas = { 'chuck' : 1 , 'annie' : 42, 'jan': 100}
>>> print(cuentas.get('jan', 0))
100
>>> print(cuentas.get('tim', 0))
0
```
Podemos usar `get` para escribir nuestro bucle de histograma más conciso. Puesto que el método get automáticamente maneja el caso en que una clave no está en el diccionario, podemos reducir cuatro líneas a una y eliminar la sentencia `if`.

```python
palabra = 'brontosaurio'
d = dict()
for c in palabra:
	d[c] = d.get(c,0) + 1
print(d)
```

El uso del método get para simplificar este bucle contador termina siendo un “idioma” muy utilizado en Python.

Tomáos un momento para comparar el bucle utilizando la sentencia `if` y el operador `in` con el bucle utilizando el método `get`. Ambos
hacen exactamente lo mismo, pero uno es más breve.


!!!note "Nota"
	Para completar todo lo relativo a diccionarios y descubrir el concepto de *nesting* (diccionarios de listas, listas de diccionarios, diccionarios dentro de diccionarios...), podéis consultar [esta parte de este capítulo](https://bedford-computing.co.uk/learning/wp-content/uploads/2015/10/No.Starch.Python.Oct_.2015.ISBN_.1593276036.pdf#page=142).


## Coding time!

### Ejercicio 1
Crea un programa que recorra una lista y cree un diccionario que contenga el número de veces que aparece cada número en la lista.

- Ejemplo: `[12, 23, 5, 12, 92, 5,12, 5, 29, 92, 64,23]`

- Resultado: `{12: 3, 23: 2, 5: 3, 92: 2, 29: 1, 64: 1}`

### Ejercicio 2
Recorre un diccionario y crea una lista solo con los valores que contiene, sin añadir valores duplicados.

- Ejemplo: `{'Mikel': 3, 'Ana': 8, 'Amaia': 12, 'Unai': 5, 'Jon': 8, 'Ainhoa': 7, 'Maite': 5}`
  
- Resultado: `[3, 8, 12, 5, 7]`

### Ejercicio 3
Crea una programa de Login que compruebe el usuario y contraseña en el diccionario a continuación:

```python
usuarios = {  
      "mmacia": {  
          "nombre": "Martí",  
		  "apellido": "Macià",  
		  "password": "123123"  
	  },  
	  "fmuguruza": {  
	       "nombre": "Fermín",  
		  "apellido": "Muguruza",  
		  "password": "654321"  
	  },  
	  "cbiriukov": {  
	       "nombre": "Chechu",  
		  "apellido": "Biriukov",  
		  "password": "123456"  
	  }  
    }
```

El usuario tendrá un máximo de 3 intentos, y al acceder correctamente se mostrará el nombre y apellido del usuario.

### Ejercicio 4
Crea un programa que permita introducir a un profesor las notas de sus estudiantes (máximo 10 estudiantes). Los datos se deberán almacenar en un diccionario como el siguiente:

```python
estudiantes = {  
   "1": {  
	  "nombre": "Lorena",  
	  "nota": 8  
  },  
  "2": {  
      "nombre": "Marcel",  
	  "nota": "4.2"  
  },  
  "3": {  
      "nombre": "Julio",  
	  "nota": 6.5  
  }  
}
```
Una vez introducidos todos los datos, el programa mostrará una lista con los nombres de los estudiantes que han suspendido y otra con los que han aprobado. También calculará y mostrará la nota media de la clase.
