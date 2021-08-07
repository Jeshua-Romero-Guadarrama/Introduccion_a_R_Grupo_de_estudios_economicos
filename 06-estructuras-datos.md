# Estructuras de datos
Las estructuras de datos son objetos que contienen datos. Cuando trabajamos con R, lo que estamos haciendo es manipular estas estructuras.

Las estructuras tienen diferentes características. Entre ellas, las que distinguen a una estructura de otra son su número de **dimensiones** y si son **homogeneas** o **hereterogeneas**.

La siguiente tabla muestra las principales estructuras de control que te encontrarás en R.

Dimensiones| Homogéneas| Heterogéneas
----       |----       |----
1          | Vector    | Lista
2          | Matriz    | Data frame 
n          | Array     | 
*Adaptado de Wickham (2016).*

Veamos las características de cada una de ellas.

## Vectores
Un vector es la estructura de datos más sencilla en R. Un vector es una colección de uno o más datos del mismo tipo.

Todos los vectores tienen tres propiedades:

* **Tipo**. Un vector tiene el mismo tipo que los datos que contiene. Si tenemos un vector que contiene datos de tipo numérico, el vector será también de tipo numérico. Los vectores son **atómicos**, pues sólo pueden contener datos de un sólo tipo, no es posible mezclar datos de tipos diferentes dentro de ellos.
* **Largo**. Es el número de elementos que contiene un vector. El largo es la única **dimensión** que tiene esta estructura de datos.
* **Atributos**. Los vectores pueden tener metadatos de muchos tipos, los cuales describen características de los datos que contienen. Todos ellos son incluidos en esta propiedad. En este libro no se usarán vectores con metadatos, por ser una propiedad con usos van más allá del alcance de este libro.

Cuando una estructura únicamente puede contener datos de un sólo tipo, como es el caso de los vectores, decimos que es **homogénea**, pero no implica que necesariamente sea **atómica**. Regresaremos sobre esto al hablar de matrices y arrays.

Como los vectores son la estructura de datos más sencilla de R, datos simples como el número **3**, son en realidad vectores. En este caso, un vector de tipo numérico y largo igual a 1.

```r
3
```

```
## [1] 3
```

Verificamos que  el **3** es un vector con la función `is.vector()`.

```r
is.vector(3)
```

```
## [1] TRUE
```

Y usamos la función `length()` para conocer su largo.

```r
length(3)
```

```
## [1] 1
```

Lo mismo ocurre con los demás tipos de datos, por ejemplo, con cadenas de texto y datos lógicos.

```r
is.vector("tres")
```

```
## [1] TRUE
```

```r
is.vector(TRUE)
```

```
## [1] TRUE
```

### Creación de vectores
Creamos vectores usando la función `c()` (*combinar*). 

Llamamos esta función y le damos como argumento los elementos que deseamos combinar en un vector, separados por comas.

```r
# Vector numérico
c(1, 2, 3, 5, 8, 13)
```

```
## [1]  1  2  3  5  8 13
```

```r
# Vector de cadena de texto
c("arbol", "casa", "persona")
```

```
## [1] "arbol"   "casa"    "persona"
```

```r
# Vector lógico
c(TRUE, TRUE, FALSE, FALSE, TRUE)
```

```
## [1]  TRUE  TRUE FALSE FALSE  TRUE
```

Si deseamos agregar un elemento a un vector ya existente, podemos hacerlo combinando nuestro vector original con los elementos nuevos y asignando el resultado a nuestro vector original.

```r
mi_vector <- c(TRUE, FALSE, TRUE)

mi_vector <- c(mi_vector, FALSE)

mi_vector
```

```
## [1]  TRUE FALSE  TRUE FALSE
```

Naturalmente, podemos crear vectores que son combinación de vectores.

```r
mi_vector_1 <- c(1, 3, 5)
mi_vector_2 <- c(2, 4, 6)

mi_vector_3 <- c(mi_vector_1, mi_vector_2)

mi_vector_3
```

```
## [1] 1 3 5 2 4 6
```

Si intentamos combinar datos de diferentes tipos en un mismo vector, R realizará coerción automáticamente. El vector resultante será del tipo más flexible entre los datos que contenga, siguiendo las reglas de ***coerción***.

Creamos un vector numérico.

```r
mi_vector <- c(1, 2, 3)

class(mi_vector)
```

```
## [1] "numeric"
```

Si intentamos agregar un dato de tipo cadena de texto, nuestro vector ahora será de tipo cadena de texto.

```r
mi_vector_nuevo <- c(mi_vector, "a")

class(mi_vector_nuevo)
```

```
## [1] "character"
```

Como las cadenas de texto son el tipo de dato más flexible, siempre que creamos un vector que incluye un dato de este tipo, el resultado será un vector de texto.

```r
mi_vector_mezcla <- c(FALSE, 2, "tercero", 4.00)

class(mi_vector_mezcla)
```

```
## [1] "character"
```

Podemos crear vectores de secuencias numéricas usando `:`. De un lado de los dos puntos escribimos el número de inicio de la secuencia y del otro el final.

Por ejemplo, creamos una secuencia del 1 al 10.

```r
1:10
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10
```

También podemos crear una secuencia del 10 al 1.

```r
10:1
```

```
##  [1] 10  9  8  7  6  5  4  3  2  1
```

Las secuencias creadas con `:` son consecutivas con incrementos o decrementos de 1. Estas secuencias pueden empezar con cualquier número, incluso si este es negativo o tiene cifras decimales


```r
# Número negativo
-43:-30
```

```
##  [1] -43 -42 -41 -40 -39 -38 -37 -36 -35 -34 -33 -32 -31 -30
```

```r
# Número con cifras decimales
67.23:75
```

```
## [1] 67.23 68.23 69.23 70.23 71.23 72.23 73.23 74.23
```

Si nuestro número de inicio tiene cifras decimales, estas serán respetadas al hacer los incrementos o decrementos de uno en uno. En contraste, si es nuestro número de final el que tiene cifras decimales, este será redondeado.

```r
# Se conservan los decimales del inicio
-2.48:2
```

```
## [1] -2.48 -1.48 -0.48  0.52  1.52
```

```r
56.007:50
```

```
## [1] 56.007 55.007 54.007 53.007 52.007 51.007 50.007
```

```r
# Se redondean los decimales del final
166:170.05
```

```
## [1] 166 167 168 169 170
```

```r
968:960.928
```

```
## [1] 968 967 966 965 964 963 962 961
```

### Vectorización de operaciones
Existen algunas operaciones al aplicarlas a un vector, se aplican a cada uno de sus elementos. A este proceso le llamamos **vectorización**.

Las operaciones aritméticas y relacionales pueden vectorizarse. Si las aplicamos a un vector, la operación se realizará para cada uno de los elementos que contiene.

Por ejemplo, creamos un vector numérico.

```r
mi_vector <- c(2, 3, 6, 7, 8, 10, 11)
```

Si aplicamos operaciones aritméticas, obtenemos un vector con un resultado por cada elemento.

```r
# Operaciones aritméticas
mi_vector + 2
```

```
## [1]  4  5  8  9 10 12 13
```

```r
mi_vector * 2
```

```
## [1]  4  6 12 14 16 20 22
```

```r
mi_vector %% 2
```

```
## [1] 0 1 0 1 0 0 1
```

Al aplicar operaciones relacionales, obtenemos un vector de `TRUE`y `FALSE`, uno para cada elemento comparado.

```r
mi_vector > 7
```

```
## [1] FALSE FALSE FALSE FALSE  TRUE  TRUE  TRUE
```

```r
mi_vector < 7
```

```
## [1]  TRUE  TRUE  TRUE FALSE FALSE FALSE FALSE
```

```r
mi_vector == 7
```

```
## [1] FALSE FALSE FALSE  TRUE FALSE FALSE FALSE
```

Esta manera de aplicar una operación es muy eficiente. Comparada con otros procedimientos, requiere de menos tiempo de cómputo, lo cual a veces es considerable, en particular cuando trabajamos con un número grande de datos.

Aunque el nombre de este proceso es **vectorización**, también funciona, en ciertas circunstancias, para otras estructuras de datos.

## Matrices y arrays
Las matrices y arrays pueden ser descritas como **vectores multidimensionales**. Al igual que un vector, únicamente pueden contener datos de un sólo tipo, pero además de largo, tienen más dimensiones.

En un sentido estricto, las matrices son una caso especial de un array, que se distingue por tener **específicamente dos dimensiones**, un "largo"" y un "alto". Las matrices son, por lo tanto, una estructura con forma rectangular, con renglones y columnas.

Como las matrices son usadas de manera regular en matemáticas y estadística, es una estructura de datos de uso común en R común y en la que nos enfocaremos en este libro.

Los arrays, por su parte, pueden tener un número arbitrario de dimensiones. Pueden ser cubos, hipercubos y otras formas. Su uso no es muy común en R, aunque a veces es deseable contar con objetos n-dimensionales para manipular datos. Como los arrays tienen la restricción de que todos sus datos deben ser del mismo tipo, no importando en cuántas dimensiones se encuentren, esto limita sus usos prácticos. 

En general, es preferible usar listas en lugar de arrays, una estructura de datos que además tienen ciertas ventajas que veremos más adelante.

### Creación de matrices
Creamos matrices en R con la función `matrix()`. La función `matrix()` acepta dos argumentos, `nrow` y `ncol`. Con ellos especificamos el número de renglones y columnas que tendrá nuestra matriz.

```r
# Un vector numérico del uno al doce
1:12
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10 11 12
```

```r
# matrix() sin especificar renglones ni columnas
matrix(1:12)
```

```
##       [,1]
##  [1,]    1
##  [2,]    2
##  [3,]    3
##  [4,]    4
##  [5,]    5
##  [6,]    6
##  [7,]    7
##  [8,]    8
##  [9,]    9
## [10,]   10
## [11,]   11
## [12,]   12
```

```r
# Tres renglones y cuatro columnas
matrix(1:12, nrow = 3, ncol = 4)
```

```
##      [,1] [,2] [,3] [,4]
## [1,]    1    4    7   10
## [2,]    2    5    8   11
## [3,]    3    6    9   12
```

```r
# Cuatro columnas y tres columnas
matrix(1:12, nrow = 4, ncol = 3)
```

```
##      [,1] [,2] [,3]
## [1,]    1    5    9
## [2,]    2    6   10
## [3,]    3    7   11
## [4,]    4    8   12
```

```r
# Dos renglones y seis columnas
matrix(1:12, nrow = 4, ncol = 3)
```

```
##      [,1] [,2] [,3]
## [1,]    1    5    9
## [2,]    2    6   10
## [3,]    3    7   11
## [4,]    4    8   12
```

Los datos que intentemos agrupar en una matriz serán acomodados en orden, de arriba a abajo, y de izquierda a derecha, hasta formar un rectángulo.

Si multiplicamos el número de renglones por el número de columnas, obtendremos el número de celdas de la matriz. En los ejemplo anteriores, el número de celdas es igual al número de elementos que queremos acomodar, así que la operación ocurre sin problemas.

Cuando intentamos acomodar un número diferente de elementos y celdas, ocurren dos cosas diferentes. 

Si el número de elementos es mayor al número de celdas, se acomodarán todos los datos que sean posibles y los demás se omitirán. 

```r
matrix(1:12, nrow = 3, ncol = 3)
```

```
##      [,1] [,2] [,3]
## [1,]    1    4    7
## [2,]    2    5    8
## [3,]    3    6    9
```

Si, por el contrario, el número de celdas es mayor que el número de elementos, estos se **reciclaran**. En cuanto los elementos sean insuficientes para acomodarse en las celdas, R nos devolverá una advertencia y se empezaran a usar los elementos a partir del primero de ellos

```r
matrix(1:12, nrow = 5, ncol = 4)
```

```
## Warning in matrix(1:12, nrow = 5, ncol = 4): la longitud de los datos [12] no es
## un submúltiplo o múltiplo del número de filas [5] en la matriz
```

```
##      [,1] [,2] [,3] [,4]
## [1,]    1    6   11    4
## [2,]    2    7   12    5
## [3,]    3    8    1    6
## [4,]    4    9    2    7
## [5,]    5   10    3    8
```

Otro procedimiento para crear matrices es la unión vectores con las siguientes funciones:

* `cbind()` para unir vectores, usando cada uno como una columna.
* `rbind()` para unir vectores, usando cada uno como un renglón.

De este modo podemos crear cuatro vectores y unirlos para formar una matriz. Cada vector será un renglón en esta matriz.

Creamos cuatro vectores, cada uno de largo igual a cuatro.

```r
vector_1 <- 1:4
vector_2 <- 5:8
vector_3 <- 9:12
vector_4 <- 13:16
```

Usamos `rbind()` para crear un matriz, en la que cada vector será un renglón.

```r
matriz <- rbind(vector_1, vector_2, vector_3, vector_4)

# Resultado
matriz
```

```
##          [,1] [,2] [,3] [,4]
## vector_1    1    2    3    4
## vector_2    5    6    7    8
## vector_3    9   10   11   12
## vector_4   13   14   15   16
```

Si utilizamos `cbind()`, entonces cada vector será una columna.

```r
matriz <- cbind(vector_1, vector_2, vector_3, vector_4)

# Resultado
matriz
```

```
##      vector_1 vector_2 vector_3 vector_4
## [1,]        1        5        9       13
## [2,]        2        6       10       14
## [3,]        3        7       11       15
## [4,]        4        8       12       16
```

Al igual que con `matrix()`, los elementos de los vectores son reciclados para formar una estructura rectangular y se nos muestra un mensaje de advertencia.

```r
# Elementos de largo diferente
vector_1 <- 1:2
vector_2 <- 1:3
vector_3 <- 1:5

matriz <- cbind(vector_1, vector_2, vector_3)
```

```
## Warning in cbind(vector_1, vector_2, vector_3): number of rows of result is not
## a multiple of vector length (arg 1)
```

```r
# Resultado
matriz
```

```
##      vector_1 vector_2 vector_3
## [1,]        1        1        1
## [2,]        2        2        2
## [3,]        1        3        3
## [4,]        2        1        4
## [5,]        1        2        5
```

Finalmente, las matrices pueden contener `NA`s.

Creamos dos vectores con un `NA` en ellos.

```r
vector_1 <- c(NA, 1, 2)
vector_2 <- c(3,  4, NA)
```

Creamos una matriz con `rbind()`.

```r
matriz <- rbind(vector_1, vector_2)

# Resultados
matriz
```

```
##          [,1] [,2] [,3]
## vector_1   NA    1    2
## vector_2    3    4   NA
```

Como `NA` representa datos perdidos, puede estar presente en compañía de todo tipo de de datos.

### Propiedades de las matrices
No obstante que las matrices y arrays son estructuras que sólo pueden contener un tipo de datos, no son atómicas. Su clase es igual a **matriz (matrix)** o **array** según corresponda.

Verificamos esto usando la función `class()`.

```r
mi_matriz <- matrix(1:10)

class(mi_matriz)
```

```
## [1] "matrix" "array"
```

Las matrices y arrays pueden tener más de una dimensión.

Obtenemos el número de dimensiones de una matriz o array con la función `dim()`. Esta función nos devolverá varios números, cada uno de ellos indica la cantidad de elementos que tiene una dimensión.

```r
mi_matriz <- matrix(1:12, nrow = 4, ncol = 3)
dim(mi_matriz)
```

```
## [1] 4 3
```

Cabe señalar que si usamos `dim()` con un vector, obtenemos `NULL`. Esto ocurre con todos los objetos unidimensionales 

```r
mi_vector <- 1:12

dim(mi_vector)
```

```
## NULL
```

Finalmente, las operaciones aritméticas también son vectorizadas al aplicarlas a una matriz. La operación es aplicada a cada uno de los elementos de la matriz.

Creamos una matriz.

```r
mi_matriz <- matrix(1:9, nrow = 3, ncol = 3)

# Resultado
mi_matriz
```

```
##      [,1] [,2] [,3]
## [1,]    1    4    7
## [2,]    2    5    8
## [3,]    3    6    9
```

Intentemos sumar, multiplicar y elevar a la tercera potencia.

```r
# Suma
mi_matriz + 1
```

```
##      [,1] [,2] [,3]
## [1,]    2    5    8
## [2,]    3    6    9
## [3,]    4    7   10
```

```r
# Multiplicación
mi_matriz * 2
```

```
##      [,1] [,2] [,3]
## [1,]    2    8   14
## [2,]    4   10   16
## [3,]    6   12   18
```

```r
# Potenciación
mi_matriz ^ 3
```

```
##      [,1] [,2] [,3]
## [1,]    1   64  343
## [2,]    8  125  512
## [3,]   27  216  729
```

Si intentamos vectorizar una operación utilizando una matriz con `NA`s, esta se aplicará para los elementos válidos, devolviendo `NA` cuando corresponda.

Creamos una matriz con `NA`s.

```r
vector_1 <- c(NA, 2, 3)
vector_2 <- c(4, 5, NA)

matriz <- rbind(vector_1, vector_2)

# Resultado
matriz
```

```
##          [,1] [,2] [,3]
## vector_1   NA    2    3
## vector_2    4    5   NA
```

Intentamos dividir sus elementos entre dos.

```r
matriz / 2
```

```
##          [,1] [,2] [,3]
## vector_1   NA  1.0  1.5
## vector_2    2  2.5   NA
```

Finalmente, podemos usar la función `t()` para transponer una matriz, es decir, rotarla 90°.

Creamos una matriz con tres renglones y dos columnas.

```r
matriz <- matrix(1:6, nrow = 3)

# Resultado
matriz
```

```
##      [,1] [,2]
## [1,]    1    4
## [2,]    2    5
## [3,]    3    6
```

Usamos `t()` para transponer.

```r
matriz_t <- t(matriz)

# Resultado
matriz_t
```

```
##      [,1] [,2] [,3]
## [1,]    1    2    3
## [2,]    4    5    6
```

Obtenemos una matriz con dos renglones y dos columnas.

## Data frames
Los data frames son estructuras de datos de dos dimensiones (rectangulares) que pueden contener datos de diferentes tipos, por lo tanto, son heterogéneas. Esta estructura de datos es la más usada para realizar análisis de datos y seguro te resultará familiar si has trabajado con otros paquetes estadísticos.

Podemos entender a los data frames como una versión más flexible de una matriz. Mientras que en una matriz todas las celdas deben contener datos del mismo tipo, los renglones de un data frame admiten datos de distintos tipos, pero sus columnas conservan la restricción de contener datos de un sólo tipo.

En términos generales, los renglones en un data frame representan casos, individuos u observaciones, mientras que las columnas representan atributos, rasgos o variables. 
Por ejemplo, así lucen los primeros cinco renglones del objeto **iris**, el famoso conjunto de datos  *Iris de Ronald Fisher*, que está incluido en todas las instalaciones de R.

```
##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
## 1          5.1         3.5          1.4         0.2  setosa
## 2          4.9         3.0          1.4         0.2  setosa
## 3          4.7         3.2          1.3         0.2  setosa
## 4          4.6         3.1          1.5         0.2  setosa
## 5          5.0         3.6          1.4         0.2  setosa
```

Los primeros cinco renglones corresponden a cinco casos, en este caso flores. Las columnas son variables con los rasgos de cada flor: largo y ancho de sépalo, largo y ancho de pétalo, y especie.

Para crear un data frame usamos la función `data.frame()`. Esta función nos pedirá un número de vectores igual al número de columnas que deseemos. Todos los vectores que proporcionemos deben tener el mismo largo. 

Esto es muy importante: **Un data frame está compuesto por vectores**.

Más adelante se hará evidente porque esta característica de un data frame es sumamente importante y también, cómo podemos sacarle provecho.

Además, podemos asignar un nombre a cada vector, que se convertirá en el nombre de la columna. Como todos los nombres, es recomendable que este sea claro, no ambiguo y descriptivo.

```r
mi_df <- data.frame(
  "entero" = 1:4, 
  "factor" = c("a", "b", "c", "d"), 
  "numero" = c(1.2, 3.4, 4.5, 5.6),
  "cadena" = as.character(c("a", "b", "c", "d"))
)

mi_df
```

```
##   entero factor numero cadena
## 1      1      a    1.2      a
## 2      2      b    3.4      b
## 3      3      c    4.5      c
## 4      4      d    5.6      d
```

```r
# Podemos usar dim() en un data frame
dim(mi_df)
```

```
## [1] 4 4
```

```r
# El largo de un data frame es igual a su número de columnas
length(mi_df)
```

```
## [1] 4
```

```r
# names() nos permite ver los nombres de las columnas
names(mi_df)
```

```
## [1] "entero" "factor" "numero" "cadena"
```

```r
# La clase de un data frame es data.frame
class(data.frame) 
```

```
## [1] "function"
```

Si los vectores que usamos para construir el data frame no son del mismo largo, los datos **no se reciclaran**. Se nos devolverá un error.

```r
data.frame(
  "entero" = 1:3, 
  "factor" = c("a", "b", "c", "d"), 
  "numero" = c(1.2, 3.4, 4.5, 5.6),
  "cadena" = as.character(c("a", "b", "c", "d"))
)
```

```
## Error in data.frame(entero = 1:3, factor = c("a", "b", "c", "d"), numero = c(1.2, : arguments imply differing number of rows: 3, 4
```

También podemos coercionar esta matriz a un data frame.

Creamos una matriz.

```r
matriz <- matrix(1:12, ncol = 4)
```

Usamos `as.data.frame()` para coercionar una matriz a un data frame.

```r
df <- as.data.frame(matriz)
```

Verificamos el resultado

```r
class(df)
```

```
## [1] "data.frame"
```

```r
# Resultado
df
```

```
##   V1 V2 V3 V4
## 1  1  4  7 10
## 2  2  5  8 11
## 3  3  6  9 12
```

### Propiedades de un data frame
Al igual que con una matriz, si aplicamos una operación aritmética a un data frame, esta se vectorizará. 

Los resultados que obtendremos dependerán del tipo de datos de cada columna. R nos devolverá todas las advertencias que ocurran como resultado de las operaciones realizadas, por ejemplo, aquellas que hayan requerido una coerción.

```r
mi_df <- data.frame(
  "entero" = 1:4, 
  "factor" = c("a", "b", "c", "d"), 
  "numero" = c(1.2, 3.4, 4.5, 5.6),
  "cadena" = as.character(c("a", "b", "c", "d"))
)

# mi_df * 2
```

## Listas
Las listas, al igual que los vectores, son estructuras de datos unidimensionales, sólo tienen largo, pero a diferencia de los vectores cada uno de sus elementos puede ser de diferente tipo o incluso de diferente clase, por lo que son estructuras heterogéneas.

Podemos tener listas que contengan datos atómicos, vectores, matrices, arrays, data frames u otras listas. Esta última característica es la razón por la que una lista puede ser considerada un vector recursivo, pues es un objeto que puede contener objetos de su misma clase.

Para crear una lista usamos la función `list()`, que nos pedirá los elementos que deseamos incluir en nuestra lista. Para esta estructura, no importan las dimensiones o largo de los elementos que queramos incluir en ella. 

Al igual que con un data frame, tenemos la opción de poner nombre a cada elemento de una lista.

Por último, no es posible vectorizar operaciones aritméticas usando una lista, se nos devuelve un error como resultado.

```r
mi_vector <- 1:10
mi_matriz <- matrix(1:4, nrow = 2)
mi_df     <- data.frame("num" = 1:3, "let" = c("a", "b", "c"))

mi_lista <- list("un_vector" = mi_vector, "una_matriz" = mi_matriz, "un_df" = mi_df)

mi_lista
```

```
## $un_vector
##  [1]  1  2  3  4  5  6  7  8  9 10
## 
## $una_matriz
##      [,1] [,2]
## [1,]    1    3
## [2,]    2    4
## 
## $un_df
##   num let
## 1   1   a
## 2   2   b
## 3   3   c
```

Creamos una lista que contiene otras listas.

```r
lista_recursiva <- list("lista1" = mi_lista, "lista2" = mi_lista)

# Resultado
lista_recursiva
```

```
## $lista1
## $lista1$un_vector
##  [1]  1  2  3  4  5  6  7  8  9 10
## 
## $lista1$una_matriz
##      [,1] [,2]
## [1,]    1    3
## [2,]    2    4
## 
## $lista1$un_df
##   num let
## 1   1   a
## 2   2   b
## 3   3   c
## 
## 
## $lista2
## $lista2$un_vector
##  [1]  1  2  3  4  5  6  7  8  9 10
## 
## $lista2$una_matriz
##      [,1] [,2]
## [1,]    1    3
## [2,]    2    4
## 
## $lista2$un_df
##   num let
## 1   1   a
## 2   2   b
## 3   3   c
```

### Propiedades de una lista
Una lista es unidimensional, sólo tiene largo.

El largo de una lista es igual al número de elementos que contiene, sin importar de qué tipo o clase sean. Usamos la lista recursiva que creamos en la sección anterior para ilustrar esto.

```r
length(lista_recursiva)
```

```
## [1] 2
```

Dado que una lista siempre tiene una sola dimensión, la función `dim()` nos devuelve `NULL`.

```r
dim(lista_recursiva)
```

```
## NULL
```

Las listas tienen clase **list**, sin importar qué elementos contienen.

```r
class(lista_recursiva)
```

```
## [1] "list"
```

Finalmente, no es posible vectorizar operaciones aritméticas usando listas. Al intentarlo nos es devuelto un error.

```r
mi_lista / 2
```

```
## Error in mi_lista/2: argumento no-numérico para operador binario
```

Si deseamos aplicar una función a cada elemento de una lista, usamos `lapply()`, como veremos en el [capítulo 10](#apply).

## Coerción
Al igual que con los datos, cuando intentamos hacer operaciones con una estructura de datos, R intenta coercionarla al tipo apropiado para poder llevarlas a cabo con éxito.

También podemos usar alguna de las funciones de la familia `as()` coercionar de un tipo de estructura de datos. A continuación se presentan las más comunes.

Función           | Coerciona a | Coerciona exitosamente a
----              |----         | ----
`as.vector()`     | Vector      | Matrices
`as.matrix()`     | Matrices    | Vectores, Data frames
`as.data.frame()` | Data frame  | Vectores, Matrices              
`as.list()`       | Lista       | Vectores, Matrices, Data frames

Como podrás ver, las estructuras de datos más sencillas, (unidimensionales, homogéneas) pueden ser coercionadas a otras más complejas (multidimensionales, heterogéneas), pero la operación inversa casi nunca es posible.

Veamos algunos ejemplos.

Creamos un vector, una matriz, un data frame y una lista.

```r
mi_vector <- c("a", "b", "c")
mi_matriz <- matrix(1:4, nrow = 2)
mi_df <- data.frame("a" = 1:2, "b" = c("a", "b"))
mi_lista <- list("a" = mi_vector, "b" = mi_matriz, "c" = mi_df)
```

Intentemos coercionar a vector con `as.vector()`.

```r
as.vector(mi_matriz)
```

```
## [1] 1 2 3 4
```

```r
as.vector(mi_df)
```

```
##   a b
## 1 1 a
## 2 2 b
```

```r
as.vector(mi_lista)
```

```
## $a
## [1] "a" "b" "c"
## 
## $b
##      [,1] [,2]
## [1,]    1    3
## [2,]    2    4
## 
## $c
##   a b
## 1 1 a
## 2 2 b
```

La coerción que intentamos sólo tuvo éxito para una matriz. Para data frame y lista, nos devolvió los mismos objetos.

Nota que `as.vector()` **no devolvió un error o una advertencia a pesar de que no tuvo éxito** al coercionar, en este caso un data frame o una lista. Esto es importante, pues no puedes confiar que `as.vector()` tuvo éxito porque corrió sin errores, es necesaria una verificación adicional. Como R intenta coercionar automáticamente, esto puede producir resultados inesperados si no tenemos cuidado.

Intentemos coercionar a matriz con `as.matrix()`.

```r
as.matrix(mi_vector)
```

```
##      [,1]
## [1,] "a" 
## [2,] "b" 
## [3,] "c"
```

```r
as.matrix(mi_df)
```

```
##      a   b  
## [1,] "1" "a"
## [2,] "2" "b"
```

```r
as.matrix(mi_lista)
```

```
##   [,1]        
## a character,3 
## b integer,4   
## c data.frame,2
```
El vector fue coercionado a una matriz con una sola columna. 

Por su parte, al correr la función con un data frame, coercionamos también todos los datos que contiene, siguiendo las reglas de coerción de tipos de dato que vimos en el [capítulo 4](## coercion).

Al coercionar una lista a una matriz, efectivamente obtenemos un objeto de este tipo, sin embargo perdemos toda la información que contiene, por lo tanto, no podemos considerar que esta es una coerción exitosa. Del mismo modo que con `as.vector()`, no nos es mostrado ningún error ni advertencia.

Intentemos coercionar a matriz con `as.data.frame()`.

```r
as.data.frame(mi_vector)
```

```
##   mi_vector
## 1         a
## 2         b
## 3         c
```

```r
as.data.frame(mi_matriz)
```

```
##   V1 V2
## 1  1  3
## 2  2  4
```

```r
as.data.frame(mi_lista)
```

```
## Error in (function (..., row.names = NULL, check.rows = FALSE, check.names = TRUE, : arguments imply differing number of rows: 3, 2
```

Tuvimos éxito al coercionar vectores y matrices. 

El vector, al igual que cuando fue coercionado a matriz, devolvió como resultado un objeto con una sola columna, mientras que la matriz conservó sus renglones y columnas.

En este caso, al intentar la coerción de lista a data frame, obtenemos un error. Esta es la única situación en la que esto ocurre utilizando las funciones revisadas en esta sección.

Por último, intentemos coercionar a matriz con `as.list()`.

```r
as.list(mi_vector)
```

```
## [[1]]
## [1] "a"
## 
## [[2]]
## [1] "b"
## 
## [[3]]
## [1] "c"
```

```r
as.list(mi_matriz)
```

```
## [[1]]
## [1] 1
## 
## [[2]]
## [1] 2
## 
## [[3]]
## [1] 3
## 
## [[4]]
## [1] 4
```

```r
as.list(mi_df)
```

```
## $a
## [1] 1 2
## 
## $b
## [1] "a" "b"
```

Dado que las listas son el tipo de objeto más flexible de todos, hemos tenido éxito en todos los casos con nuestra coerción.

Nota que para los vectores y matrices, cada uno de los elementos es transformado en un elemento dentro de la lista resultante. Si tuviéramos una matriz con cuarenta y ocho celdas, obtendríamos una lista con ese mismo número de elementos.

En cambio, para un data frame, el resultado es una lista, en la que cada elemento contiene los datos de una columna del data frame original. Un data frame con diez columnas resultará en una lista de diez elementos.

Conocer cómo ocurre la coerción de estructuras de datos te ayudará a entender mejor algunos resultados devueltos por funciones de R, además de que te facilitará la manipulación y procesamiento de datos.
