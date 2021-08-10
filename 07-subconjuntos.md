# Subconjuntos
En R, podemos obtener subconjuntos de nuestras estructuras de datos. Es decir, podemos extraer partes de una estructura de datos (nuestro conjunto).

Hacemos esto para seleccionar datos que tienen características específicas, por ejemplo, todos los valores mayores a cierto número o aquellos que coinciden exactamente con un valor de nuestro interés.

Para realizar esta operación haremos uso de índices, operadores lógicos y álgebra Booleana. Algunos procedimientos para obtener subconjuntos pueden usarse con cualquier estructura de datos, mientras que otras sólo funcionan con algunas de ellas.

En este capítulo revisaremos cómo extraer subconjuntos de vectores, matrices, data frames y listas, usando índices, nombres y condicionales.

## Índices
Usar índices para obtener subconjuntos es el procedimiento más universal en R, pues funciona para todas las estructuras de datos.

Un índice en R representa una **posición**. Cuando usamos índices le pedimos a R que extraiga de una estructura los datos que se encuentran en una o varias posiciones específicas dentro de ella.

A diferencia de la mayoría de los lenguajes de programación, los índices en R empiezan en **1**, no en **0**. El índice del primer elemento de una estructura de datos siempre es 1, el segundo 2, y así sucesivamente.

Un aspecto muy importante de este procedimiento es que, para data frames y listas, **cuando extraemos un subconjunto de un objeto usando corchetes, obtenemos como resultado un objeto de la misma clase que el objeto original**. Si extraemos un subconjunto de un data frame, obtenemos un vector; y si extraemos de una lista, obtenemos una lista.

El uso de índices tiene además otras características particulares para las distintas estructuras de datos, así que veremos este procedimiento para cada una de ellas.

### Vectores
Empecemos creando un vector que contiene los nombres de distintos niveles educativos. 

```r
nivel <- c("Preescolar", "Primaria", "Secundaria", "Educación Media Superior",
           "Educación Superior")

nivel
```

```
## [1] "Preescolar"               "Primaria"                
## [3] "Secundaria"               "Educación Media Superior"
## [5] "Educación Superior"
```

Este es un vector de largo igual a cinco.

```r
length(nivel)
```

```
## [1] 5
```

¿Cómo obtendríamos el tercer elemento de este vector usando índices? ¿O del primer al cuarto elemento? ¿O el segundo y quinto elemento?

Para obtener subconjuntos con índices escribimos corchetes `[]` después del nombre de un objeto.  Dentro de los corchetes escribimos el o los números que corresponden a la posición que nos interesa extraer del objeto.

Por ejemplo:

* `objeto[3]`
* `lista[4:5]` 
* `dataframe[c(2, 7), ]`

Entonces, para extraer el tercer elemento de nuestro vector `nivel` hacemos lo siguiente.

```r
nivel[3]
```

```
## [1] "Secundaria"
```

Para extraer del primer al cuarto elemento de un vector, usamos un vector con una secuencia numérica del 1 al 4 creada con `:`.

```r
nivel[1:4]
```

```
## [1] "Preescolar"               "Primaria"                
## [3] "Secundaria"               "Educación Media Superior"
```

Sin embargo, si intentamos extraer el segundo y quinto elemento del vector `nivel` corriendo lo siguiente, obtendremos un error.

```r
nivel[2, 5]
```

```
## Error in nivel[2, 5]: número incorreto de dimensiones
```

¿Porqué no ha funcionado lo anterior? 

El mensaje de error nos da una pista muy importante. Al usar una coma dentro de los corchetes estamos dando la instrucción a R de buscar los índices solicitados en más de una dimensión. El número antes de la coma será buscado en la primera dimensión del objeto, y el segundo número, en su segunda dimensión.

Entonces, al llamar `nivel[2, 5]`, lo que estamos pidiendo es que R extraiga el elemento que se encuentra en la posición **2** de la primera dimensión del vector, y el elemento en la posición **5** de su segunda dimensión.  Como los vectores son unidimensionales, es imposible cumplir esta instrucción y se produce un error.

Recuerda que en R, un número sencillo es también un vector, por lo tanto, cuando escribimos `vector[3]`, en realidad estamos dando como índice un vector que contiene al número 3.

Por lo tanto, si deseamos extraer elementos en posiciones no consecutivas, debemos usar vectores generados con `c()`. De este modo damos un vector, de más de un número de largo al corchete, pero todos se encuentran en una misma dimensión.

Aplicando lo anterior, si escribimos dentro de los corchetes `c(2, 5)`, entonces tendremos éxito al extraer el segundo y quinto elemento de `nivel`.

```r
nivel[c(2, 5)]
```

```
## [1] "Primaria"           "Educación Superior"
```

Para estructuras de dos dimensiones, como son matrices y data frames, **el primer vector de un índice, antes de una coma, es la posición en los renglones y el segundo es la posición las columnas**. 

Obtener subconjuntos por renglones y columnas es un tipo de operación muy común al trabajar con data frames y matrices.

### Data frames
Creamos un data frame llamado `mi_df`. 

```r
mi_df <- data.frame("nombre" = c("Armando", "Elsa", "Ignacio", "Olga"),
                    "edad" = c(20, 24, 22, 30),
                    "sexo" = c("H", "M", "M", "H"),
                    "grupo" = c(0, 1, 1, 0))

# Resultado
mi_df
```

```
##    nombre edad sexo grupo
## 1 Armando   20    H     0
## 2    Elsa   24    M     1
## 3 Ignacio   22    M     1
## 4    Olga   30    H     0
```

Usamos `dim()` para confirmar que nuestro objeto tiene dos dimensiones: tres renglones y tres columnas.

```r
dim(mi_df)
```

```
## [1] 4 4
```

Si usamos un índice con un sólo número, extraemos una columna completa, con todos sus renglones.

```r
mi_df[1]
```

```
##    nombre
## 1 Armando
## 2    Elsa
## 3 Ignacio
## 4    Olga
```

Si usamos un vector, sin comas, obtenemos varias columnas.

```r
mi_df[c(1, 3)]
```

```
##    nombre sexo
## 1 Armando    H
## 2    Elsa    M
## 3 Ignacio    M
## 4    Olga    H
```

Al usar comas, el vector antes de la coma nos devolverá un renglón completo.


```r
mi_df[3,]
```

```
##    nombre edad sexo grupo
## 3 Ignacio   22    M     1
```

Nota que si dejamos vació el espacio después de la coma, se nos devuelven todas las columnas del data frame.

Si el espacio que dejamos vacío es el que se encuentra después de la coma, obtenemos columnas. Esto es equivalente a usar un solo vector dentro de los corchetes.

```r
mi_df[ ,1]
```

```
## [1] "Armando" "Elsa"    "Ignacio" "Olga"
```

Al combinar índices para renglones y columnas, obtenemos los datos que se encuentran en una posición específica.

Por ejemplo, el dato en el tercer renglón y la tercer columna.

```r
mi_df[3, 3]
```

```
## [1] "M"
```

El segundo y tercer dato de la tercera columna.

```r
mi_df[2:3, 3]
```

```
## [1] "M" "M"
```

El cuarto renglón de la tercera y cuarta columna.

```r
mi_df[4, 3:4]
```

```
##   sexo grupo
## 4    H     0
```

También podemos usar vectores de más de un número. Por ejemplo, los datos en en el primer y segundo renglón, y en la segunda y cuarta columna.

```r
mi_df[1:2, c(2, 4)]
```

```
##   edad grupo
## 1   20     0
## 2   24     1
```

Por último, en todos los casos anteriores, hemos obtenido como resultado un data frame.


```r
sub_df <- mi_df[1:2, c(2, 4)]

class(sub_df)
```

```
## [1] "data.frame"
```

Si damos un índice inválido para las columnas, es decir, un número de columna que no exista, se nos devuelve un renglón.

Intentemos obtener la séptima columna de `mi_df`.

```r
mi_df[7]
```

```
## Error in `[.data.frame`(mi_df, 7): undefined columns selected
```

Sin embargo, para los renglones simplemente se nos devuelve `NA`.


```r
mi_df[7, ]
```

```
##    nombre edad sexo grupo
## NA   <NA>   NA <NA>    NA
```

### Matrices
El procedimiento anterior para data frames funciona de la misma manera para las matrices, con una excepción.

Si usamos como índice un sólo número, entonces obtendremos el valor que se encuentre en esa posición, contando celdas de arriba a abajo y de izquierda a derecha.

Creamos una matriz con 4 renglones y dos columnas.

```r
mi_matriz <- matrix(1:8, nrow = 4)

# Resultado
mi_matriz
```

```
##      [,1] [,2]
## [1,]    1    5
## [2,]    2    6
## [3,]    3    7
## [4,]    4    8
```

Si damos como índice el número 8, R no intentará devolvernos la octava columna de la matriz, sino la octava celda.

```r
mi_matriz[8]
```

```
## [1] 8
```

Fuera de este caso, los índices de renglones y columna tienen el mismo comportamiento que en un data frame.

```r
# Tercer renglón
mi_matriz[3, ]
```

```
## [1] 3 7
```

```r
# Segunda columna
mi_matriz[ ,2]
```

```
## [1] 5 6 7 8
```

```r
# Tercer renglón y segunda columna
mi_matriz[3, 2]
```

```
## [1] 7
```

Nota que en este caso obtenemos vectores al extraer un subconjunto.

### Arrays
Para objetos de tres o más dimensiones se siguen las mismas reglas que con las matrices, aunque ya no es tan fácil hablar de renglones y columnas. 

Creamos un array de cuatro dimensiones.

```r
mi_array <- array(data = 1:16, dim = c(2, 2, 2, 2))
```

Veamos nuestro resultado y comprobamos con `dim()` su número de dimensiones.

```r
mi_array
```

```
## , , 1, 1
## 
##      [,1] [,2]
## [1,]    1    3
## [2,]    2    4
## 
## , , 2, 1
## 
##      [,1] [,2]
## [1,]    5    7
## [2,]    6    8
## 
## , , 1, 2
## 
##      [,1] [,2]
## [1,]    9   11
## [2,]   10   12
## 
## , , 2, 2
## 
##      [,1] [,2]
## [1,]   13   15
## [2,]   14   16
```

```r
# Comprobamos el número de dimensiones de nuestro objeto
dim(mi_array)
```

```
## [1] 2 2 2 2
```

Intentemos extraer varios subconjuntos, sólo para ilustrar cómo funcionan los índices con arrays.

```r
mi_array[1, , , ]
```

```
## , , 1
## 
##      [,1] [,2]
## [1,]    1    5
## [2,]    3    7
## 
## , , 2
## 
##      [,1] [,2]
## [1,]    9   13
## [2,]   11   15
```

```r
mi_array[1, 2, , ]
```

```
##      [,1] [,2]
## [1,]    3   11
## [2,]    7   15
```

```r
mi_array[1, 2, 1, ]
```

```
## [1]  3 11
```

```r
mi_array[1, 2, 1, 2]
```

```
## [1] 11
```

Nota que como resultados obtenemos matrices, a menos que hagamos una extraigamos el contenido de una sola celda.

## Nombres
Un segundo método para extraer subconjuntos es usar los nombres de los elementos en una estructura de datos. Este forma de obtener subconjuntos es usada principalmente con data frames y listas. 

De manera similar a los índices, usamos corchetes cuadrados `[]` después del nombre de un objeto, pero en lugar de escribir un número, escribimos el nombre del elemento que deseamos extraer como una cadena de texto, es decir, entre comillas.

### Data frames
Los elementos de un data frame son sus columnas y cada una de ellas tiene un nombre, lo que estamos pidiendo a R con este método es que nos devuelva los elementos cuyo nombre coincida con el que hemos proporcionado

Para mostrar el uso de este método, utilizaremos el mismo data frame que en la sección anterior.

Si escribimos entre corchetes "nombre", obtendremos toda la columna **nombre**.

```r
mi_df["nombre"]
```

```
##    nombre
## 1 Armando
## 2    Elsa
## 3 Ignacio
## 4    Olga
```

Al escribir "grupo", nos es devuelta toda la columna con ese nombre.

```r
mi_df["grupo"]
```

```
##   grupo
## 1     0
## 2     1
## 3     1
## 4     0
```

De igual manera que con los índices, al escribir una coma dentro de los corchetes, estamos pidiendo con ello extraer elementos en más de una dimensión. Lo que está antes escrito antes de la coma corresponde a renglones, y lo que está después, a columnas.

Si ejecutamos lo siguiente, obtendremos `NA` en lugar de obtener las columnas **edad** y **sexo**.

```r
mi_df["edad", "sexo"]
```

```
## [1] NA
```

Lo anterior ocurre porque R intenta encontrar un renglón llamado "edad" y una columna llamada "sexo", al no encontrarlas, nos devuelve `NA`. Recuerda que aunque no es lo más común, los renglones de un data frame pueden tener nombres.

Al igual que con los índices, si damos el nombre de un renglón que existe, obtenemos `NA`. Es sólo al solicitar un nombre de columna no válido que se nos devuelve un error.

Pedimos un nombre de renglón inexistente y obtenemos `NA`.

```r
mi_df["localidad", ]
```

```
##    nombre edad sexo grupo
## NA   <NA>   NA <NA>    NA
```

Pero si pedimos un nombre inválido de columna, nos es devuelto un error.

```r
mi_df[, "localidad"]
```

```
## Error in `[.data.frame`(mi_df, , "localidad"): undefined columns selected
```

Para extraer más de una columna, escribimos un vector de texto entre los corchetes.
Por ejemplo

```r
mi_df[c("edad", "sexo")]
```

```
##   edad sexo
## 1   20    H
## 2   24    M
## 3   22    M
## 4   30    H
```

Además, las columnas son devueltas en el orden que las pedimos, lo cual es conveniente cuando estamos procesando y recodificando datos.

```r
mi_df[c("sexo", "edad")]
```

```
##   sexo edad
## 1    H   20
## 2    M   24
## 3    M   22
## 4    H   30
```

### Listas
Para una lista, el procedimiento es prácticamente el mismo que para un data frame, pero en lugar de obtener columnas, obtenemos los elementos contenidos en ella.

La primera diferencia con los data frame es que, dado que las listas son unidimensionales, si usamos una coma dentro de los corchetes, nos será devuelto un error.

Creamos una lista llamada `mi_lista`.

```r
mi_lista <- list("uno" = 1, "dos" = "2", "tres" = as.factor(3), 
                 "cuatro" = matrix(1:4, nrow = 2))
```

Intentamos obtener un subconjunto con una coma.

```r
mi_lista["uno", "dos"]
```

```
## Error in mi_lista["uno", "dos"]: número incorreto de dimensiones
```

Si pedimos un nombre que no existe en la lista, se nos devuelve `NULL` en lugar de un error.

```r
mi_lista["cinco"]
```

```
## $<NA>
## NULL
```

Para todo lo demás, los nombres tienen el mismo comportamiento que para los data frames.

Extremos un elemento de la lista.

```r
mi_lista["dos"]
```

```
## $dos
## [1] "2"
```

Extraemos más de un elemento de la lista.

```r
mi_lista[c("cuatro", "tres")]
```

```
## $cuatro
##      [,1] [,2]
## [1,]    1    3
## [2,]    2    4
## 
## $tres
## [1] 3
## Levels: 3
```

## Subconjuntos por índice y nombre
Al extraer subconjuntos podemos combinar índices con nombres dentro del mismo corchete para objetos multidimensionales, por ejemplo, usando un índice antes de la coma y un nombre después de ella. 

Esto nos da una gran flexibilidad para hacer subconjuntos con data frames y matrices. En particular, es útil al definir funciones y al trabajar con conjuntos de datos de los tenemos información incompleta.

Por ejemplo, extraemos el tercer y cuarto renglón de la columna **nombre** en nuestro data frame `mi_df`.

```r
mi_df[3:4, "nombre"]
```

```
## [1] "Ignacio" "Olga"
```

También podemos usar vectores dentro de los corchetes. 

Extraemos los renglones con los nombres "48" y "100" de las primera y cuarta columna de `iris`.

```r
iris[c("48", "100"), c(1, 4)]
```

```
##     Sepal.Length Petal.Width
## 48           4.6         0.2
## 100          5.7         1.3
```

## El signo de dolar `$` y los corchetes dobles `[[]]`
Otra manera en la que podemos extraer subconjuntos usando nombres, es con el signo de dólar `$`.

Para usar este método, escribir el signo `$` después del nombre de un objeto de la siguiente forma: `objeto$nombre`.

Este método permite extraer un sólo elemento a la vez, funciona para data frames y listas, y para el caso de los data frame, el elemento extraído siempre será una columna.

Por ejemplo, extraemos la columna **nombre** del data frame `mi_df`.

```r
mi_df$nombre
```

```
## [1] "Armando" "Elsa"    "Ignacio" "Olga"
```

También podemos escribir el nombre del elemento que deseamos entre comillas, esto es útil si el nombre tiene espacios.

```r
mi_df$"nombre"
```

```
## [1] "Armando" "Elsa"    "Ignacio" "Olga"
```

Si intentamos dar más de un nombre después del signo `$`, nos es devuelto un error.

```r
mi_df$c("nombre", "edad")
```

El resultado de las operaciones anteriores no es un data frame, sino un vector. 

```r
class(mi_df$nombre)
```

```
## [1] "character"
```

Esta es una característica distintiva de este método, **al usar el signo `$` para extraer un elemento de un data frame o una lista, obtenemos un objeto de la clase que ese elemento era originalmente**.

Recuerda que **un data frame está formado por vectores**. Como vimos en el [capítulo 6](##data-frames), una manera de generar data frames es combinar vectores. Estos vectores, aunque estén contenidos dentro de un data frame, conservan todas las características de un vector y es posible extraerlos como tales.

Cuando usamos el signo `$`, le pedimos a R que extraiga de un objeto un subconjunto con sus propiedades originales. Por esta razón, para los data frame, siempre son devueltos vectores, mientras que para las listas lo que obtenemos depende del tipo de objeto contenido en ellas.

Por ejemplo, si extraemos el elemento **uno** de `mi_lista` usando corchetes, obtenemos una lista.

```r
class(mi_lista["uno"])
```

```
## [1] "list"
```

Pero si usamos el signo `$`, el resultado es un vector numérico, pues esta es su clase original.

```r
class(mi_lista$uno)
```

```
## [1] "numeric"
```

Si intentamos  extraer el elemento **cuatro**, obtendremos una matriz.

```r
class(mi_lista$"cuatro")
```

```
## [1] "matrix" "array"
```

De manera similar, podemos extraer elementos de un objeto, con su clase original,  usando índices y corchetes dobles `[[]]`. 

La ventaja de usar corchetes dobles es no sólo podemos usar índices, sino que los podemos combinar con nombres, lo cual nos da acceso a una mayor flexibilidad para extraer subconjuntos y permite usarlos en estructuras de datos con elementos sin nombre.

Por ejemplo, para extraer la columna **edad** de `mi_df` con corchetes dobles, podemos usar su índice, 2, o su nombre.

```r
# Usando un índice
mi_df[[2]]
```

```
## [1] 20 24 22 30
```

```r
# Usando un nombre
mi_df[["edad"]]
```

```
## [1] 20 24 22 30
```

A diferencia de los corchetes sencillos, no podemos extraer más de una columna de un data frame usando corchetes dobles y un vector.

Si escribimos un vector numérico dentro de corchetes dobles, será interpretado como si cada número estuviera separado por una coma, indicando las dimensiones de las cuales se extraerán datos.

Por ejemplo, intentamos extraer las columnas uno y tres de `mi_df`.

```r
mi_df[[c(1, 3)]]
```

```
## [1] "Ignacio"
```

El resultado que obtenemos no es el esperado.

Lo que ocurre es que cuando escribimos lo siguiente.

`mi_df[[c(1, 3)]]`

R lo interpreta como:

`mi_df[[1, 3]]`

Es decir,  R extraerá el dato en el renglón uno y la columna tres, en lugar de las columnas uno y tres.

```r
mi_df[[1, 3]]
```

```
## [1] "H"
```

Por lo tanto si escribimos lo siguiente:

`mi_df[[1:3]]`

R lo interpretará como buscar el dato uno en la primera dimensión, el dato dos, en la segunda dimensión, y el dato tres en la tercera dimensión. Como un data frame solo tiene dos dimensiones, se nos devolverá un error

```r
mi_df[[1:3]]
```

```
## Error in .subset2(x, i, exact = exact): falló indexación recursiva en nivel 2
```

Lo mismo ocurre con vectores que contienen nombres de columnas.

```r
mi_df[[c("nombre", "edad")]]
```

```
## Error in .subset2(x, i, exact = exact): subíndice fuera de  los límites
```

Como las listas son unidimensionales, sólo podemos extraer un elemento a la vez usando corchetes dobles `[[]]`

```r
mi_lista[[1]]
```

```
## [1] 1
```

Si damos más de un índice o nombre, siempre obtendremos un error.

```r
# Más de un índice
mi_lista[[1:2]]
```

```
## Error in mi_lista[[1:2]]: subíndice fuera de  los límites
```

```r
# Más de un nombre
mi_lista[["uno", "dos"]]
```

```
## Error in mi_lista[["uno", "dos"]]: número incorrecto de subíndices
```

### Los data frames y listas son como cajas de manzanas
Para comprender mejor el comportamiento comportamiento del signo `$` y los corchetes dobles, imagina que los data frames y listas son cajas que contienen manzanas. Los data frame contienen las manzanas en bolsas, y estas bolsas serían vectores, en los cuales cada manzana sería un elemento. 

Por su parte, las listas pueden tener manzanas en diferentes presentaciones: bolsas, manzanas sueltas o incluso otras cajas de manzana.

Cuando usamos corchetes, estamos sacando manzanas de una caja, usando una caja más pequeña, otro data frame o lista. Así, con un data frame, obtenemos otra caja que contiene bolsas de manzanas, y con una lista obtenemos otra caja con manzanas en las presentaciones que se encuentren.

En contraste, al usar el signo `$` o corchetes dobles, estamos sacando bolsas de manzana directamente en un data frame, y en las listas estamos sacando las manzanas en su presentación original, sin que haya de por medio otra caja en ninguno de estos casos.

Dado lo anterior, podemos extraer subconjuntos de subconjuntos combinando diferentes tipos de corchetes. 

```r
# Subconjunto de un subconjunto: Data frame.
mi_df[[2]][3]
```

```
## [1] 22
```

```r
# Subconjunto de un subcojunto: Lista.
mi_lista[["cuatro"]][2]
```

```
## [1] 2
```

No te preocupes mucho si lo anterior te parece confuso, lo es. No profundizaremos sobre este tema específico de los subconjuntos en este libro, pero ten en cuenta que si te encuentras con código como el del ejemplo anterior, lo que está ocurriendo es la extracción de subconjuntos de subconjuntos.

## Condicionales
Las condicionales nos permiten obtener subconjuntos que para los que una o más condiciones son verdaderas (`TRUE`). 

Para este procedimiento usamos operadores lógicos y condicionales, como revisamos en el [capítulo 5](#operadores) y lo podemos aplicar a **data frames**.

Con este procedimiento, podemos extraer todos los datos de una encuesta que corresponden a mujeres, o a personas que viven en una entidad específica, o que tienen un ingreso superior a la media, o cualquier otra condición que se verificable con **álgebra Booleana**.

Realizamos la extracción de subconjuntos mediante operaciones relacionales y lógicas dentro de corchetes.

Esta operación tiene la siguiente estructura.


```r
objeto[condicion, columnas_devueltas]
```

En donde:

* `objeto`: es un data frame.
* `condición`: un subconjunto de `objeto`, que devuelva un columna como vector, al que se le aplica una o más operaciones lógicas o relacionales.
* `columnas_devueltas`: los índices o nombres de las columnas que deseamos sean devueltas como resultado.

Dentro del corchete escribimos, antes de una coma, el código para obtener un subconjunto que extraiga una columna, del data frame al que queremos extraer un subconjunto usando un subconjunto.

Este primer subconjunto debe ser extraído con alguno de los dos procedimientos que da como resultado un vector, ya sea el signo `$` o corchetes dobles `[[]]`.

Necesitamos extraer un vector, porque aplicaremos a todos los elementos de esa columna una operación relacional, usando vectorización, como lo vimos en el [capítulo 6](###vectorizacion-de-operaciones).

Todos los elementos para los que el resultado de esta operación sea **TRUE**, formarán parte de nuestro subconjunto usando condicionales. Como cada elemento de una columna de un data frame está ubicado en su propio renglón, podemos decir que nos serán devueltos los renglones para los que la condición sea verdadera.

Si dejamos el espacio para `columnas_devueltas` vacío, nuestro resultado será un data frame con las mismas columnas que el data frame original. Si damos un índice o un nombre, entonces obtendremos un data frame sólo con las columnas solicitadas. De este modo, podemos extraer renglones y columnas específicas.

Veamos esto en práctica, extrayendo subconjuntos de los datos `iris`.

### Usando condicionales
Intentaremos extraer todos los datos en `iris` en los que el largo del sépalo, columna **Petal.Width**, sea mayor que 7.5.

Primero, obtenemos el subconjunto de esta columna de `iris`. Usaremos el signo `$`

```r
iris$Sepal.Length
```

```
##   [1] 5.1 4.9 4.7 4.6 5.0 5.4 4.6 5.0 4.4 4.9 5.4 4.8 4.8 4.3 5.8 5.7 5.4 5.1
##  [19] 5.7 5.1 5.4 5.1 4.6 5.1 4.8 5.0 5.0 5.2 5.2 4.7 4.8 5.4 5.2 5.5 4.9 5.0
##  [37] 5.5 4.9 4.4 5.1 5.0 4.5 4.4 5.0 5.1 4.8 5.1 4.6 5.3 5.0 7.0 6.4 6.9 5.5
##  [55] 6.5 5.7 6.3 4.9 6.6 5.2 5.0 5.9 6.0 6.1 5.6 6.7 5.6 5.8 6.2 5.6 5.9 6.1
##  [73] 6.3 6.1 6.4 6.6 6.8 6.7 6.0 5.7 5.5 5.5 5.8 6.0 5.4 6.0 6.7 6.3 5.6 5.5
##  [91] 5.5 6.1 5.8 5.0 5.6 5.7 5.7 6.2 5.1 5.7 6.3 5.8 7.1 6.3 6.5 7.6 4.9 7.3
## [109] 6.7 7.2 6.5 6.4 6.8 5.7 5.8 6.4 6.5 7.7 7.7 6.0 6.9 5.6 7.7 6.3 6.7 7.2
## [127] 6.2 6.1 6.4 7.2 7.4 7.9 6.4 6.3 6.1 7.7 6.3 6.4 6.0 6.9 6.7 6.9 5.8 6.8
## [145] 6.7 6.7 6.3 6.5 6.2 5.9
```

Si aplicamos la operación relacional `> 7` a este subconjunto, obtenemos un vector, con `TRUE` o `FALSE` para cada elemento de `iris$Petal.Width`.

```r
iris$Sepal.Length > 7.5
```

```
##   [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [13] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [25] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [37] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [49] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [61] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [73] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [85] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [97] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE  TRUE FALSE FALSE
## [109] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE  TRUE  TRUE FALSE
## [121] FALSE FALSE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE  TRUE
## [133] FALSE FALSE FALSE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [145] FALSE FALSE FALSE FALSE FALSE FALSE
```

Escribimos esta operación dentro de los corchetes, antes de una coma. No escribimos nada después de la coma, para obtener un subconjunto con todas las columnas de `iris`.

```r
iris[iris$Sepal.Length > 7.5, ]
```

```
##     Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
## 106          7.6         3.0          6.6         2.1 virginica
## 118          7.7         3.8          6.7         2.2 virginica
## 119          7.7         2.6          6.9         2.3 virginica
## 123          7.7         2.8          6.7         2.0 virginica
## 132          7.9         3.8          6.4         2.0 virginica
## 136          7.7         3.0          6.1         2.3 virginica
```

Podemos pedir que se nos devuelvan sólo los datos de la columna **Species**, escribiendo el índice o nombre de esta columna después de la coma.

```r
# Usando índice
iris[iris$Sepal.Length > 7.5, 5]
```

```
## [1] virginica virginica virginica virginica virginica virginica
## Levels: setosa versicolor virginica
```

```r
#USando nombres
iris[iris$Sepal.Length > 7.5, "Species"]
```

```
## [1] virginica virginica virginica virginica virginica virginica
## Levels: setosa versicolor virginica
```

Nota que si pedimos una sola columna en nuestros resultado, el resultado será un vector en lugar de un data frame.

```r
class(iris[iris$Sepal.Length > 7.5, "Species"])
```

```
## [1] "factor"
```

Podemos realizar más de una operación relacional antes de la coma, usando operadores lógicos. 

Por ejemplo, extraemos todos los datos para los que el ancho del pétalo (**Sepal.Width**) sea menor que 3 y la especie (**Species**) sea "setosa".

```r
iris[iris$Sepal.Width < 3 & iris$Species == "setosa", ]
```

```
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species
## 9           4.4         2.9          1.4         0.2  setosa
## 42          4.5         2.3          1.3         0.3  setosa
```

Recuerda que puedes usar el operados `!` para negaciones. De este modo puedes extraer todos los datos que no son de la especie virginica o su largo sea menor a 4.7.

```r
iris[!(iris$Petal.Length < 4.7 | iris$Species == "virginica"), ]
```

```
##    Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
## 51          7.0         3.2          4.7         1.4 versicolor
## 53          6.9         3.1          4.9         1.5 versicolor
## 57          6.3         3.3          4.7         1.6 versicolor
## 64          6.1         2.9          4.7         1.4 versicolor
## 71          5.9         3.2          4.8         1.8 versicolor
## 73          6.3         2.5          4.9         1.5 versicolor
## 74          6.1         2.8          4.7         1.2 versicolor
## 77          6.8         2.8          4.8         1.4 versicolor
## 78          6.7         3.0          5.0         1.7 versicolor
## 84          6.0         2.7          5.1         1.6 versicolor
## 87          6.7         3.1          4.7         1.5 versicolor
```

En realidad podemos usar un vector lógico para extraer subconjuntos, sin necesidad de realizar una operación relacional.

Por ejemplo, para obtener los dato en el primer y cuarto renglón de `mi_df`.

```r
mi_df[c(TRUE, FALSE, FALSE, TRUE), ]
```

```
##    nombre edad sexo grupo
## 1 Armando   20    H     0
## 4    Olga   30    H     0
```

Si damos un vector lógico de largo menor que el número de renglones en un data frame, el vector es reciclado. 

Al utilizar vector `c(FALSE, TRUE)`, nos serán devueltos el segundo y cuarto renglón de `mi_df`.

```r
mi_df[c(FALSE, TRUE), ]
```

```
##   nombre edad sexo grupo
## 2   Elsa   24    M     1
## 4   Olga   30    H     0
```

Si proporcionamos una condición que no se cumple en ningún caso, es decir, devuelve un vector que consiste sólo de `FALSE`, el subconjunto que obtenemos es una data frame sin renglones.

```r
iris[iris$Species == "oceanica", ]
```

```
## [1] Sepal.Length Sepal.Width  Petal.Length Petal.Width  Species     
## <0 rows> (or 0-length row.names)
```

Finalmente, si no escribimos una coma dentro del corchete después de la condicional, obtendremos un data frame sin columnas, que para fines prácticos es un objeto sin utilidad.

```r
iris[iris$Petal.Width >= 5]
```

```
## data frame with 0 columns and 150 rows
```

### La función `subset()`
Una alternativa para usar condicionales, sin necesidad de corchetes, es la función `subset()`.

Esta función tiene los siguientes argumentos:

* `x`: Un objeto, generalmente un data frame.
* `subset`: Una condición, expresada como operaciones relacionales o condicionales, que se aplicarán a una columna de `x`.
* `select`: Un vector con los nombres de las columnas a conservar en el resultado. Si no asignamos un valor a este argumento, se nos devuelven todas las columnas de `x`.

Puedes leer la documentación completa llamando `?subset`.

Como podrás ver, `subset()` necesita como argumentos los mismos elementos que usamos para extraer subconjuntos con corchetes. La principal diferencia se encuentra en el argumento `subset`.

Al usar corchetes, necesitamos aplicar una operación relacional a un vector, extraído de nuestro objeto original, por ejemplo `iris[iris$Species == "setosa", ]`. Con `subset()`, basta proporcionar el nombre del elemento al que aplicaremos las operaciones relacionales y condicionales. Para este caso, escribimos `Species == "setosa"`. Nota que el nombre de la columna está escrito sin comillas.

Por ejemplo, para extraer de iris todos los datos en los que el largo del sépalo es mayor que 7.5, damos a `iris` como argumento `x` y `Sepal.Lenght` como argumento `subset`. Dejamos sin definir el argumento `select`, para obtener todas las columnas.

```r
subset(x = iris, subset = Sepal.Length > 7.5)
```

```
##     Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
## 106          7.6         3.0          6.6         2.1 virginica
## 118          7.7         3.8          6.7         2.2 virginica
## 119          7.7         2.6          6.9         2.3 virginica
## 123          7.7         2.8          6.7         2.0 virginica
## 132          7.9         3.8          6.4         2.0 virginica
## 136          7.7         3.0          6.1         2.3 virginica
```

Obtenemos el mismo resultado que al llamar `iris[iris.Sepal.Length > 7.5, ]`.

Damos además `c("Sepal.Length", "Species")` como argumento a `select` y con ello sólo nos serán devueltas las columnas con estos nombres.

```r
subset(x = iris, subset = Sepal.Length > 7.5, select = c("Sepal.Length", "Species"))
```

```
##     Sepal.Length   Species
## 106          7.6 virginica
## 118          7.7 virginica
## 119          7.7 virginica
## 123          7.7 virginica
## 132          7.9 virginica
## 136          7.7 virginica
```

Desde luego, esto es equivalente a llamar `iris[iris.Sepal.Length > 7.5, c("Sepal.Length", "Species")]`.

También podemos usar operaciones lógicas para el argumento `subset`. 

Por ejemplo, los datos para los que el largo del sépalo sea mayor que 7.5 y el ancho del pétalo sea igual a 3.


```r
subset(x = iris, subset = Sepal.Length > 7.5 & Sepal.Width == 3)
```

```
##     Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
## 106          7.6           3          6.6         2.1 virginica
## 136          7.7           3          6.1         2.3 virginica
```

Si damos como argumento a subset una comparación con un nombre de columna no válido, obtenemos un error.

```r
subset(x = iris, subset = Sepal.Weight > 7.5 )
```

```
## Error in eval(e, x, parent.frame()): objeto 'Sepal.Weight' no encontrado
```

Al igual que si usamos corchetes, si pedimos una condición que para ningún caso es verdadera, nuestro resultado es un data frame sin renglones.

```r
subset(x = iris, subset = Sepal.Length > 9 )
```

```
## [1] Sepal.Length Sepal.Width  Petal.Length Petal.Width  Species     
## <0 rows> (or 0-length row.names)
```

Si damos como argumento `select` el nombre de una columna inválida, también se nos devuelve un error.

```r
subset(x = iris, subset = Sepal.Length > 7, select = "Sepal.Weight")
```

```
## Error in `[.data.frame`(x, r, vars, drop = drop): undefined columns selected
```


Finalmente, también podemos dar un vector lógico como argumento `subset` para obtener subconjuntos. Por ejemplo, el segundo y cuarto renglón de `mi_df `.

```r
subset(x = mi_df, subset = c(TRUE, FALSE, FALSE, TRUE))
```

```
##    nombre edad sexo grupo
## 1 Armando   20    H     0
## 4    Olga   30    H     0
```

La función `subset()` casi siempre resulta en código más breve y es más fácil de interpretar por el usuario que los subconjuntos condicionales con corchetes. Sin embargo, la mayor parte de las veces, usar uno u otro procedimiento depende de tu preferencia personal y de las necesidades de los proyectos en los que colaboras.
