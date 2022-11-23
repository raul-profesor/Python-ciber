Este ejercicio consistirá en programar un tipo de [keylogger](https://es.wikipedia.org/wiki/Keylogger) para Linux.

Se ofrecen dos vías para implementar este ejercicio, una sencilla y otra una versión *mejorada* para los más avezados.


!!!tip "Consejo"
    Os recomiendo crear un nuevo entorno virtual con `pipenv` y sobre él instalar los paquetes que sean necesarios.


## Versión sencilla

Esta versión lo único que hará será mostrar por pantalla continuamente una lista con lo que contiene nuestro portapapeles, añadiendo un nuevo elemento a esta lista cada vez que se copia algo a dicho portapapeles (`CTRL+C`).

Vamos a utilizar una biblioteca concreta, que deberéis instalar y sobre la que debéis informaros a propósito de su sencillo funcionamiento:

+ [Pyperclip](https://pypi.org/project/pyperclip/)

Los pasos a seguir serán:

1. Importar ambas bibliotecas
2. Crear una lista vacía, que será la que contendrá el contenido del portapapeles
4. Tras ello, debéis crear un bucle infinito:
   1. Si el contenido del portapapeles no está vacío (`''`) debemos guardar su valor en una variable
      1. Para asegurarnos de no guardar valores repetidos, si el valor del punto anterior no está en la lista, añadimos el valor al final de la lista
      2. Imprimimos la lista por pantall

Una mejora muy sencilla de este ejercicio sería guardar el resultado en un archivo `.txt`.


## Versión mejorada


