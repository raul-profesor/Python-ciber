# Estructuras de control

## Condicionales
Las estructuras de control se utilizan para **ejecutar bloques de código en función de condiciones**.

### Sentencia IF - ELSE
Se evalúa la condición especificada en la sentencia `if` y en caso de cumplirse se ejecutará el bloque de código indentado (tabulado). En caso de que el resultado de la condición sea `False`, el bloque especificado no se ejecutará:

```python
numero = 5
if numero > 1:
    # Se ejecutará cuando la condición sea True
    print("Es mayor que uno")
```

Las condiciones pueden tener mayor complejidad:

```python
edad = 16
altura = 175
if (edad > 14 and altura > 160):
    print("Puede montarse en la montaña rusa")
```


Mediante la palabra reservada `else` es posible especificar un bloque de código que se ejecute en caso de que la condición no se cumpla:

```python
numero = 2
if numero > 10:
     # Se ejecutará cuando la condición sea True
    print("Es mayor que diez")
else:
    # Se ejecutará cuando la condición sera False
    print("Es menor o igual que diez")
```

También podemos comprobar más condiciones mediante la expresión `elif`. En este caso, se seguirán comprobando todas las condiciones `elif` hasta que una de ellas se cumpla. En caso contrario, se ejecutará el bloque de código dentro de `else` (si lo hubiera).

```python
numero = 5
if numero < 3:
    print("Es menor que 3")
elif numero < 6:
    print("El número está entre el 3 y el 5")
else:
    print("Es mayor o igual a 6")
```

Tal y como muestra el siguiente código de ejemplo, Python no tiene limitaciones para anidar distintos bloques de `IF`s.

```python
numero = 2
if numero >= 0:
    if numero == 0:
        print("El valor es 0")
    else:
        print("Es un número positivo")
else:
    print("Es un número negativo")
```

## Bucles
Los bucles permiten ejecutar un bloque de código tantas veces como queramos. 

### Sentencia WHILE

La sentencia `while` permite ejecutar un bloque de código mientras la expresión que definamos se cumpla (es decir, devuelva `True`). Python interpretará como `True` cualquier valor distinto a `0` o `None`.

```python
contador = 0
while(contador < 5):
    # Se ejecutará mientras la variable contador sea menor a 5.
    contador = contador+1
    print("Iteración número",contador)
print ("¡Fin!")
```

Para detener una ejecución de forma voluntaria se utiliza la sentencia `break`:
   
```python 
contador = 0
while(contador < 5):
    contador = contador+1
    print("Iteración número",contador)
    if contador == 3:
        break
print ("¡Fin!")
```

También es posible saltar únicamente la iteración actual mediante la sentencia `continue`:

```python
contador = 0
while(contador < 5):
    contador = contador+1
    if contador == 3:
        continue
    print("Iteración número {}".format(contador))
print ("¡Fin!")
```

La salida del programa anterior será la siguiente:
```
    Iteración número 1
    Iteración número 2
    Iteración número 4
    Iteración número 5
    Bucle while finalizado
```

Otros lenguajes de programación ofrecen otra estructura similar conocida como `DO - WHILE`. No es el caso de Python, por lo que habría que emular dicho comportamiento mediante nuestro código.

#### Bucle WHILE con ELSE

La expresión `else` puede utilizarse también tras un bloque `while`. De este forma podemos definir un bloque de código que se ejecutará una vez finalizado el bloque `while` (es decir, cuando la condición se evalúe `False` y no se haya finalizado mediante un `break`):

```python
count = 0
while(count < 5):
    count = count+1
    print("Iteración número {}".format(count))
else:
    print("Bucle while finalizado")
 ```   

### Sentencia FOR
A diferencia de otros lenguajes de programación, en Python la sentencia FOR itera únicamente por secuencias (listas, tuplas, cadenas de caracteres,...).

```python
alumnos = ["Ane", "Mikel", "Unai", "Lorea"]
for alumno in alumnos:
    print(alumno)
```

También es posible utilizarlo para recorrer un string:

```python
frase = "Aprendiendo Python"
for c in frase:
    print(c)
```

Para detener una ejecución se utiliza la sentencia `break`:

```python
numeros = [4,8,2,7,1,9,3,5]
total = 0

for n in numeros:
    total += n
    if total > 10
        break
```

Al igual que en otras estructuras de repetición, también es posible saltar únicamente la iteración actual mediante la sentencia `continue`:

```python
numeros = [4,8,2,7,1,9,3,5]
total = 0

# solo sumar los números impares
for num in numeros:
    if num % 2 == 0:
        print("Numero par, no lo sumamos")
        continue
    total += n
```

#### Bucle FOR con ELSE
Python permite definir un bloque de código que se ejecutará una vez finalice la iteración por todos los elementos de una lista. Es importante mencionar que no se ejecutará si se ha finalizado mediante `break`.

```python
alumnos = ["Ane", "Mikel", "Unai", "Lorea"]
for alumno in alumnos:
    print(alumno)
else:
    print("No quedan más alumnos.")
```

#### La función range()

La función `range([start,]  stop  [,  step])` devuelve una secuencia de números. Es por ello que se utiliza de forma frecuente para iterar:

```python
for i in range(3):
    print(i)
# 0
# 1
# 2
```

También podemos indicar el inicio, fin y step:

```python
print("Números del 5 al 10") 
for i in range(5,  10): 
    print(i,  end=', ')
# 5,  6,  7,  8,  9,

print("Números impares del 1 al 10")
for i in range(1,  10,  2):
    print(i,  end=', ')
# 1,  3,  5,  7,  9,
```

Para iterar por una lista utilizando el índice, basta con combinarlo con la función `len()`:

```python
    alumnos = ["Ane", "Mikel", "Unai", "Lorea"]
    for i in range(len(alumnos)):
    	print(alumnos[i])
```

## Captura de excepciones usando try y except

Imaginemos un script donde utilicemos las funciones `input` e `int` para leer y analizar un número entero introducido por el usuario. Resulta poco seguro realizar esta acción de esta forma:

```python
>>> velocidad = input(prompt)
¿Cual.... es la velocidad de vuelo de una golondrina sin carga?
¿Te refieres a una golondrina africana o a una europea?
>>> int(velocidad)
ValueError: invalid literal for int() with base 10:
>>>
```

Cuando estamos trabajando con el intérprete de Python, tras el error simplemente nos aparece de nuevo el prompt, así que pensamos “¡epa, me he equivocado!”, y continuamos con la siguiente sentencia.

Sin embargo, si se escribe ese código en un script de Python y se produce el error, el script se detendrá inmediatamente, y mostrará un “traceback”. No ejecutará la siguiente sentencia.

He aquí un programa de ejemplo para convertir una temperatura desde grados Fahrenheit a grados Celsius [^1]:

```python
ent = input('Introduzca la Temperatura Fahrenheit:')
fahr = float(ent)
cel = (fahr - 32.0) * 5.0 / 9.0
print(cel)
```
[:1] Código: https://es.py4e.com/code3/fahren.py

Si ejecutamos este código y le damos una entrada no válida, simplemente fallará con un mensaje de error bastante antipático:

```python
python fahren.py
Introduzca la Temperatura Fahrenheit:72
22.2222222222
python fahren.py
Introduzca la Temperatura Fahrenheit:fred
Traceback (most recent call last):
  File "fahren.py", line 2, in <module>
    fahr = float(ent)
ValueError: invalid literal for float(): fred
```

Existen estructuras de ejecución condicional dentro de Python para manejar este tipo de errores esperados e inesperados, llamadas `try/except`.

La idea de try y except es que si se sabe que cierta secuencia de instrucciones puede generar un problema, sea posible añadir ciertas sentencias para que sean ejecutadas en caso de error. Estas sentencias extras (el bloque except) serán ignoradas si no se produce ningún error.

Podéis pensar en la característica try y except de Python como una “póliza de seguros” en una secuencia de sentencias.

Se puede reescribir nuestro conversor de temperaturas de esta forma:

```python
ent = input('Introduzca la Temperatura Fahrenheit:')
try:
    fahr = float(ent)
    cel = (fahr - 32.0) * 5.0 / 9.0
    print(cel)
except:
    print('Por favor, introduzca un número')
```
[^1]:Código: https://es.py4e.com/code3/fahren2.py

Python comienza ejecutando la secuencia de sentencias del bloque try. Si todo va bien, se saltará todo el bloque except y terminará. Si ocurre una excepción dentro del bloque try, Python saltará fuera de ese bloque y ejecutará la secuencia de sentencias del bloque except.

```python
python fahren2.py
Introduzca la Temperatura Fahrenheit:72
22.2222222222
python fahren2.py
Introduzca la Temperatura Fahrenheit:fred
Por favor, introduzca un número
```

Gestionar una excepción con una sentencia try recibe el nombre de capturar una excepción. En este ejemplo, la clausula except muestra un mensaje de error. En general, capturar una excepción te da la oportunidad de corregir el problema, volverlo a intentar o, al menos, terminar el programa con elegancia.


!!!note "Nota"
    Un buen recurso para completar el conocimiento sobre las estructuras de control es [este](https://bedford-computing.co.uk/learning/wp-content/uploads/2015/10/No.Starch.Python.Oct_.2015.ISBN_.1593276036.pdf#page=108)

    Es especialmente interesante cuando explica los bucles y condicionales aplicados a listas, que veremos en el próximo capítulo.

## Coding time!

### Ejercicio 1
Crea un programa que solicite un número al usuario y devuelva el siguiente mensaje:

- Si es mayor que 0: "Es un número positivo."
- Si es igual a 0: "Es igual a cero.
- Si es menor que 0: "Es un número negativo."

Ejemplo 1:

```
Introduce un número: 5
Es un número positivo
```

Ejemplo 2:

```
Introduce un número: -3
Es un número negativo
```

### Ejercicio 2
Escribe un programa que solicite dos números enteros al usuario y muestre por pantalla la suma de todos los números enteros que hay entre los dos números (ambos números incluidos).

Ejemplo 1:

```
Introduce el número de inicio: 4
Introduce el número de fin: 8
El resultado es:  30
```

Ejemplo 2:

```
Introduce el número de inicio: 10
Introduce el número de fin: 15
El resultado es:  75
```

### Ejercicio 3
Mejora el programa anterior para que muestre por separado la suma de los números pares y los impares.

Ejemplo 1:

```
Introduce el número de inicio: 4
Introduce el número de fin: 8
Los pares suman 18 y los impares 12
```

Ejemplo 2:

```
Introduce el número de inicio: 10
Introduce el número de fin: 15
Los pares suman 36 y los impares 39
```

### Ejercicio 4
Escribe un programa que solicite al usuario un nombre de usuario y contraseña. El programa mostrará el mensaje "¡Bienvenido!" si el usuario introduce los siguientes datos:

- Nombre de usuario: root
- Contraseña: toor

Si los datos de acceso son incorrectos mostrará el mensaje "Acceso fallido" y el programa finalizará.

Ejemplo 1:

```
Introduce el nombre de usuario: root
Introduce la contraseña: toor
¡Bienvenido!
```

Ejemplo 2:

```
Introduce el nombre de usuario: root
Introduce la contraseña: 123456
Acceso fallido
```

### Ejercicio 5
Mejora el programa anterior para que solo permita 3 intentos. Cada vez vez que el usuario introduzca datos de acceso incorrectos el programa mostrará el mensaje: "Datos incorrectos. Le quedan X intentos.", siendo X el número de intentos restantes. Tras el tercer fallo el programa mostrará el mensaje "Has agotado todos tus intentos." y finalizará.

Ejemplo:

```
Introduce el nombre de usuario: root
Introduce la contraseña: 123456
Datos incorrectos. Le quedan 2 intentos.
Introduce el nombre de usuario: root
Introduce la contraseña: abcd
Datos incorrectos. Le quedan 1 intentos.
Introduce el nombre de usuario: root
Introduce la contraseña: 123abc
Datos incorrectos. Le quedan 0 intentos.

Has agotado todos tus intentos.
```

### Ejercicio 6
Crea un programa que reciba 5 números del usuario y muestre el mayor de todos por pantalla.

Ejemplo:

```
Introduce un número: 5
Introduce un número: -10
Introduce un número: 2
Introduce un número: 14
Introduce un número: 7
El número más alto es:  14
```

### Ejercicio 7
Mejora el programa anterior, de forma que el usuario pueda introducir tantos números como quiera. El programa solicitará números al usuario hasta que introduzca la palabra "fin". Entonces mostrará el mayor de todos por pantalla.

Ejemplo:

```
Introduce un número: 6
Introduce un número: 9
Introduce un número: 11
Introduce un número: 3
Introduce un número: 5
Introduce un número: fin
El número más alto es:  11
```
