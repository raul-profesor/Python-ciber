# Enunciado del reto de esteganografía

Si sabes qué es lo que aparece en la imagen que ilustra este post muy posiblemente tengas un/a hijo/a de unos 27 años o seas dicho hijo/a. Yo todavía recuerdo cuando jugaba a capturar el mayor número posible de tipos o especies de pokémon, para ir completando la información de todos ellos en la Pokédex y que éstos fueran sumando habilidad y fortaleza, y evolucionando, mediante su enfrentamiento a otros pokémon (salvajes o pertenecientes a otros entrenadores), y así convertirnos en verdaderos Maestros Pokémon. Cada uno teníamos nuestro pokémon preferido y el mio está atrapado en la pokéball que se ve en el archivo de imagen asociado al reto. Para abrirla deberás usar el nombre de mi pokémon favorito. ¿Puedes abrirla y decirme qué hay dentro?".

![](img/pokeball.jpg)

Algunas pistas para guiarte:

+ Si necesitáis crear un diccionario, una lista con todos los nombres de los Pokemon puede encontrarse [aquí](https://es.wikipedia.org/wiki/Anexo:Pok%C3%A9mon)
+ Puedes utilizar Requests y BeautifulSoap para generar un archivo con los nombres a modo de diccionario
  + Como sugerencia, podéis buscar todos las etiquetas de tipo `a` y filtrar por el `href`. En el `href` podéis afinar usando una expresión regular que indique que busque todo lo que empiece por */wiki/*. Para ello necesitaréis la biblioteca [re](https://docs.python.org/3/library/re.html)
+ Para extraer y aislar el nombre de los Pokemon podéis obtener el contenido del `title` y, una vez más, usando expresiones regulares, filtrar todo aquello que empiece por **Pok**, por ejemplo.
+ Para crear, escribir y/o eliminar archivos de texto, podéis consultar [este recurso](https://www.freecodecamp.org/espanol/news/python-como-escribir-en-un-archivo-abrir-leer-escribir-y-otras-funciones-de-archivos-explicadas/)
+ Una vez obtenido el diccionario, necesitaréis [algún](https://ayudaleyprotecciondatos.es/2020/05/13/estaganografia/#Programas_para_esteganografia) programa de esteganografía para el ataque.
  + Para ejecutar el programa será necesario ejecutar el comando en el terminal adecuado, esto es posible el Python mediante la biblioteca [subprocess](https://docs.python.org/3/library/subprocess.html)