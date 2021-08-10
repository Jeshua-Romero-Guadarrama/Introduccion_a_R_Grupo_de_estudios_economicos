# Conceptos básicos
Para trabajar con R es necesario conocer un poco del vocabulario usado en en este lenguaje de programación. Los siguientes son conceptos básicos que usaremos a lo largo de todo el libro.

## La consola de R
Lo primero que nos encontramos al ejecutar R es una pantalla que nos muestra la versión de este lenguaje que estamos ejecutando y un *prompt*:

`>_                                               ` 

Esta es la consola de R y corresponde al **entorno computacional** de este lenguaje. Es aquí donde nuestro **código** es interpretado.

Podemos escribir código directamente en la consola y R nos dará el resultado de lo pidamos allí mismo. Esta es la razón por la que se dice que R permite el uso interactivo, pues no es necesario compilar nuestro código para ver sus resultados.

Si estás usando RStudio, te encontrarás la consola de R en uno de los paneles de este programa.


## Ejecutar, llamar, correr y devolver
Cuando hablamos de ejecutar, llamar o correr nos referimos a pedir que R realice algo, en otras palabras, estamos dando una instrucción o una *entrada*. 

Cuando decimos que R nos devuelve algo, es que ha realizado algo que le hemos pedido, es decir, nos está dando una *salida*.

Por ejemplo, si escribimos los siguiente en la consola lo siguiente y damos *Enter*, estamos pidiendo que se ejecute esta operación:
`> 1 + 1`

Y nos será devuelto su resultado:
`[1] 2`

## Objetos
En R, todo es un objeto. Todos los datos y estructuras de datos son objetos. Además, todos los objetos tienen un nombre para identificarlos.

La explicación de esto es un tanto compleja y se sale del alcance de este libro. Se relaciona con el paradigma de **programación orientada a objetos** y ese es todo un tema en sí mismo.

Lo importante es que recuerdes que al hablar de un objeto, estamos hablando de cualquier cosa que existe en R y que tiene un nombre.

## Constantes y variables
De manera análoga al uso de estos términos en lenguaje matemático, una constante es un objeto cuyo valor no podemos cambiar, en contraste, una variable es un objeto que puede cambiar de valor.

Por ejemplo, en la siguiente expresión, **$\pi$** y **2** son constantes, mientras que **a** y **r** son variables.

$a = \pi r ^ 2$

Las constantes y variables en R tienen nombres que nos permiten hacer referencia a ellas en operaciones.

Las constantes ya están establecidas por R, mientras que nosotros podemos crear variables, asignándoles valores a nombres.

En R usamos `<-` para hacer asignaciones. De este modo, podemos asignar el valor **3** a la variable **radio**

```r
radio <- 3
```

Hablaremos sobre asignaciones más adelante, en el capítulo de [operadores](##operadores-de-asignacion).

Es recomendable que al crear una variable usemos **nombres claros, no ambiguos y descriptivos**. Esto previene confusión y hace que nuestro código sea más fácil de comprender por otras personas o por nosotros mismos en el futuro.

Los nombres de las variables pueden incluir letras, números, puntos y guiones bajos. Deben empezar siempre con una letra o un punto y si empiezan con un punto, a este no le puede seguir un número.

Finalmente, cuando te encuentres con un renglón de código que inicia con un gato (hashtag), esto representa un comentario, es código que no se ejecutará, sólo se mostrará.

```r
# Este es un comentario
```

## Funciones (introducción básica)
Una función es **una serie de operaciones a la que les hemos asignados un nombre**. Las funciones aceptan **argumentos**, es decir, especificaciones sobre cómo deben funcionar.

Cuando llamamos una función, se realizan las operaciones que contiene, usando los argumentos que hemos establecido.

En R reconocemos a una función usando la notación: `nombre_de_la_función()`. Por ejemplo:

* `mean()`
* `quantile()`
* `summary()`
* `density()`
* `c()`

Al igual que con las variables, se recomienda que los nombres de las funciones sean claros, no ambiguos y descriptivos. Idealmente, el nombre de una función  describe lo que hace. De hecho, es probable que adivines qué hacen casi todas funciones de la lista de arriba a partir de su nombre.

Aunque estrictamente hablando una función es un objeto, para fines de explicación, en este libro nos referiremos a ambos como si fueran cosas diferentes.

Las funciones son un tema que revisamos más adelante. Por el momento, recuerda que una función realiza operaciones y nos pide argumentos para poder llevarlas a cabo.

## Documentación
Las funciones de R *base* y aquellas que forman parte de paquete tienen un archivo de documentación.

Este archivo describe qué hace la función, sus argumentos, detalles sobre las operaciones que realiza,los resultados que devuelve y ejemplos de uso.

Para obtener la documentación de una función, escribimos el `?` antes de su nombre y lo ejecutamos. También podemos usar la función `help()`, con el nombre de la función.

Los dos procedimientos siguientes son equivalentes.

```r
?mean()

help("mean")
```

Si usas RStudio, la documentación de la función se mostrará en uno de los paneles de este IDE. Si estas usando R directamente, se abrirá una ventana de tu navegador de Internet.

También podemos obtener la documentación de un paquete, si damos el argumento `package` a la función `help()`, con el nombre de un paquete.

Por ejemplo, la documentación del paquete **stats**, instalado por defecto en R *base*.

```r
help(package = "stats")
```


## Directorio de trabajo
El directorio o carpeta de trabajo es el lugar en nuestra computadora en el que se encuentran los archivos con los que estamos trabajando en R. Este es el lugar donde R buscara archivos para importarlos y al que serán exportados, a menos que indiquemos otra cosa.

Puedes encontrar cuál es tu directorio de trabajo con la función `getwd()`. Sólo tienes que escribir la función en la consola y ejecutarla.

```r
getwd()
```

```
## [1] "D:/# Libros/# Libros-GitHub/Introduccion_a_R_grupo_de_estudios_economicos"
```
Se mostrará en la consola la ruta del directorio que está usando R.

Puedes cambiar el directorio de trabajo usando la función `setwd()`, dando como argumento la ruta del directorio que quieres usar.

```r
setwd("C:\otro_directorio")
```

Por último, si deseas conocer el contenido de tu directorio de trabajo, puedes ejecutar. la función `list.files()`, sin argumentos, que devolverá una lista con el nombre de los archivos de tu directorio de trabajo. La función `list.dirs()`, también sin argumentos` te dará una lista de los directorios dentro del directorio de trabajo.


```r
# Ver archivos
list.files()

# Ver directorios
list.dirs()
```

### Sesión
Los objetos y funciones de R son almacenados en la memoria RAM de nuestra computadora.

Cuando ejecutamos R, ya sea directamente o a través de RStudio, estamos creando una instancia del entorno del entorno computacional de este lenguaje de programación. **cada instancia es una sesión**.

Todos los objetos y funciones creadas en una sesión, permanecen sólo en ella, no son compartidos entre sesiones, sin embargo una sesión puede tener el mismo directorio de trabajo que otra sesión.

Es posible tener más de una sesión de R activa en la misma computadora. Aunque ambas

Cuando cerramos R, también cerramos nuestra sesión. Se nos preguntará si deseamos guardar el contenido de nuestra sesión para poder volver a ella después. Esto se guarda en un archivo con extensión **.Rdata* en tu directorio de trabajo.

Para conocer los objetos y funciones que contiene nuestra sesión, usamos la función `ls()`, que nos devolverá una lista con los nombres de todo lo guardado en la sesión.

```r
ls()
```

```
## [1] "radio"
```

De manera más precisa, nuestra sesión es un **entorno** de trabajo y los objetos pertenecen a un entorno específico. 

Los entornos son un concepto importante al hablar de lenguajes de programación, pero también son un tema que sale del alcance de este libro. 

Con que recuerdes que **cada sesión de R tiene su propio entorno global**, eso será suficiente.

## Paquetes
R puede ser expandido con **paquetes**. Cada paquete es una colección de funciones diseñadas para atender una tarea específica. Por ejemplo, hay paquetes para trabajo visualización geoespacial, análisis psicométricos, minería de datos, interacción con servicios de Internet y muchas otras cosas más.

Estos paquetes se encuentran alojados en **CRAN**, así que pasan por un control riguroso antes de estar disponibles para su uso generalizado.

Podemos instalar paquetes usando la función `install.packages()`, dando como argumento el nombre del paquete que deseamos instalar, entre comillas.

Por ejemplo, para instalar el paquete **readr**, corremos lo siguiente.

```r
install.packages("readr")
```

Hecho esto, aparecerán algunos mensajes en la consola mostrando el avance de la instalación

Una vez concluida la instalación de un paquete, podrás usar sus funciones con la función `library()`. Sólo tienes que llamar esta función usando como argumento el nombre del paquete que quieres utilizar

```r
library(readr)
```

Cuando haces esto, R importa las funciones contenidas en el paquete al entorno de trabajo actual. 

Es importante que tengas en mente que debes hacer una llamada a  `library()` cada que inicies una sesión en R. Aunque hayas importado las funciones de un paquete con anterioridad, las sesiones de R se inician "limpias", sólo con los objetos y funciones de *base*.

Este comportamiento es para evitar problemas de compatibilidad y  para propiciar buenas prácticas de colaboración. 

Si importamos paquetes automáticamente y usamos sus funciones sin indicar de donde provienen, al compartir nuestro código con otras personas, estas no tendrán la información completa para entender qué estamos haciendo. R, al pedirnos que cada sesión indiquemos qué estamos importando, nos obliga a ser explícito con todo lo que estamos haciendo. Es un poco latoso, pero te acostumbras a ello.

En caso de escribir en `install.packages()` el nombre de un paquete no disponible en **CRAN**, se nos mostrará una advertencia y no se instalará nada.

```r
install.packages("un_paquete_falso")
```

Los paquetes que hemos importado en nuestra sesión actual aparecen al llamar `sessionInfo()`.

También podemos ver qué paquetes tenemos ya instalados ejecutando la función `installed.packages()` sin ningún argumento. Una instalación nueva de R tiene pocos paquetes instalados, pero esta lista puede crecer considerablemente con el tiempo.

## Scripts
Los *scripts* son documentos de texto con la extensión de archivo **.R**, por ejemplo `mi_script.R`.

Estos archivos son iguales a cualquier documentos de texto, pero R los puede leer y ejecutar el código que contienen.

Aunque R permite el uso interactivo, es recomendable que guardes tu código en un archivo .R, de esta manera puedes usarlo después y compartirlo con otras personas. En realidad, en proyectos complejos, es posible que sean necesarios múltiples *scripts* para distintos fines.

Podemos abrir y ejecutar *scripts* en R usando la función `source()`, dándole como argumento la ruta del archivo .R en nuestra computadora, entre comillas. 

Por ejemplo.

```r
source("C:/Mis scripts/mi_script.R")
```

Cuando usamos RStudio y abrimos un *script* con extensión .R, este programa nos abre un panel en el cual podemos ver su contenido. De este modo podemos ejecutar todo el código que contiene o sólo partes de él.
