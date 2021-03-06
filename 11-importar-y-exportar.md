# Importar y exportar datos
Hasta ahora, hemos trabajado con datos ya existentes en R *base* o que hemos generado nosotros mismos, sin embargo, lo usual es que usemos datos almacenados en archivos fuera de R.

R puede importar datos de una amplia variedad de tipos de archivo con las funciones en *base* además de que esta capacidad es ampliada con el uso de paquetes específicos.

Cuando importamos un archivo, estamos guardando su contenido en nuestra sesión como un objeto. Dependiendo del procedimiento que usemos será el tipo de objeto creado.

De manera análoga, podemos exportar nuestros objetos de R a archivos en nuestra computadora.

## Descargando datos
Antes de empezar a importar datos, vale la pena señalar que  podemos descargar archivos de internet usando R con la función `download.file()`.

De esta manera tendremos acceso a una vasta diversidad de fuentes de datos. Entre otras, podrás descargar los archivos 

La función `download.file()` nos pide como argumento `url`, la dirección de internet del archivo que queremos descargar y `destfile` el nombre que tendrá el archivo en nuestra computadora. Ambos argumentos como cadenas de texto, es decir, entre comillas.

Por ejemplo, para descargar una copia del set *iris* disponible en el [*UCI Machine Learning Repository*](https://archive.ics.uci.edu) usamos la siguiente dirección como argumento `url`:

* https://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data

Y asignamos "iris.data" al argumento `dest`.

```r
download.file(
  url = "https://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data", 
  destfile = "iris.data"
  )
```

El resultado es un archivo llamado "iris.data" en nuestro directorio de trabajo. 

Este método funciona con prácticamente todo tipo de archivos, aunque en algunos casos será necesario agregar el argumento `method = "wb"`, por asegurar que el archivo obtenido funcione correctamente.

## Tablas (datos rectangulares)
Como vimos en el capítulo [7](#estructuras-de-datos), las estructura rectangular, en renglones y columnas, es común y conveniente para el análisis de datos. Nos referiremos a esta forma de organizar datos como **tabla**.

R cuenta con la función genérica `read.table()`, que puede leer cualquier tipo de archivo que contenga una tabla. 

La condición para que R interprete un archivo como una tabla es que tenga renglones y en cada renglón, los datos estén separados por comas, o algún otro carácter, indicando columnas. Es decir, algo que luzca de la siguiente manera.

>1, 20,  8, 5

>1, 31,  6, 5

>2, 18,  9, 5

>2, 25, 10, 5

Por supuesto, en lugar de comas podemos tener puntos y coma, dos puntos, tabuladores o cualquier otro signo de puntuación como **separador** de columnas.

La función `read.table()` acepta un número considerable de argumentos. Los más importantes son los siguientes.

* `file`: La ruta del archivo que importaremos, como cadena de texto. Si el archivo se encuentra en nuestro [directorio de trabajo](##directorio-de-trabajo), es suficiente dar el nombre del archivo, sin la ruta completa.
* `header`: Si nuestro archivo tiene encabezados, para ser interpretados como nombres de columna, definimos este argumento como `TRUE`.
* `sep`: El carácter que es usado como separador de columnas. Por defecto es ";".
* `col.names`: Un vector opcional, de tipo carácter, con los nombres de las columnas en la tabla.
* `stringsAsFactors`: Esta función convierte automáticamente los datos de texto a factores. Si este no es el comportamiento que deseamos, definimos este argumento como `FALSE`.

Puedes consultar todos los argumentos de esta función ejecutando `?read.table` en la consola.

Es importante señalar que el objeto obtenido al usar esta función es siempre un **data frame**.

Probemos con un archivo con extensión ".data", descargado desde el repositorio de [Github](http://www.github.com) de este libro. 

```r
download.file(
  url = "https://raw.githubusercontent.com/jboscomendoza/r-principiantes-bookdown/master/datos/breast-cancer-wis.data", 
  dest = "breast-cancer-wis.data"
)
```

Estos datos pertenecen a una base de diagnósticos de cáncer mamario de la Universidad de Wisconsin, usado para probar métodos de aprendizaje automático. Puedes encontrar la información completa sobre este conjunto de datos en el siguiente enlace:

* https://archive.ics.uci.edu/ml/datasets/Breast+Cancer+Wisconsin+%28Diagnostic%29

Nos damos cuenta de que hemos tenido éxito en la descarga si aparece un mensaje en la consola de R indicando los resultados de nuestra operación.

Usamos sin especificar ningún otro argumento.

```r
bcancer <- read.table(file = "datos/breast-cancer-wis.data")
```

Veamos los primeros renglones de nuestros datos usando la función `head()`

```r
head(bcancer)
```

```
##                               V1
## 1    1000025,5,1,1,1,2,1,3,1,1,2
## 2   1002945,5,4,4,5,7,10,3,2,1,2
## 3    1015425,3,1,1,1,2,2,3,1,1,2
## 4    1016277,6,8,8,1,3,4,3,7,1,2
## 5    1017023,4,1,1,3,2,1,3,1,1,2
## 6 1017122,8,10,10,8,7,10,9,7,1,4
```

Nuestros datos no lucen particularmente bien. Necesitamos ajustar algunos parámetros al importarlos.

No hay datos de encabezado, por lo que `header` será igual a `FALSE` y  el separador de columnas es una coma, así que el valor de `sep` será ",". No conocemos cuál es el nombre de las columnas, así que por el momento no proporcionaremos uno.


```r
bcancer <- read.table(file = "datos/breast-cancer-wis.data", header = FALSE, sep = ",")

# Resultado
head(bcancer)
```

```
##        V1 V2 V3 V4 V5 V6 V7 V8 V9 V10 V11
## 1 1000025  5  1  1  1  2  1  3  1   1   2
## 2 1002945  5  4  4  5  7 10  3  2   1   2
## 3 1015425  3  1  1  1  2  2  3  1   1   2
## 4 1016277  6  8  8  1  3  4  3  7   1   2
## 5 1017023  4  1  1  3  2  1  3  1   1   2
## 6 1017122  8 10 10  8  7 10  9  7   1   4
```

Luce mejor, pero los nombres de las columnas son poco descriptivos. Si no damos nombres de variables, cada columna tendrá como nombre "V" seguida de números del 1 adelante.

Para este ejemplo, contamos con un archivo de información, que describe el contenido de los datos que hemos importado.

* https://raw.githubusercontent.com/jboscomendoza/r-principiantes-bookdown/master/datos/breast-cancer-wis.names

Si descargas este archivo, puedes abrirlo usando el bloc o navegador de internet de tu computadora.

Guardaremos en un vector las abreviaturas de los nombres de columna descritos en el documento anterior.

```r
nombres <- c("id", "clump_t", "u_csize", "u_cshape", "m_adh", "spcs", "b_nuc", 
             "b_chr", "n_nuc", "mit", "class")
```

Ahora usaremos este vector como argumento `col.names` en `read.table()`, para importar nuestros datos con nombres de columna.

```r
bcancer <- read.table(file = "datos/breast-cancer-wis.data", header = FALSE, sep = ",",
                      col.names = nombres)

# Resultado
head(bcancer)
```

```
##        id clump_t u_csize u_cshape m_adh spcs b_nuc b_chr n_nuc mit class
## 1 1000025       5       1        1     1    2     1     3     1   1     2
## 2 1002945       5       4        4     5    7    10     3     2   1     2
## 3 1015425       3       1        1     1    2     2     3     1   1     2
## 4 1016277       6       8        8     1    3     4     3     7   1     2
## 5 1017023       4       1        1     3    2     1     3     1   1     2
## 6 1017122       8      10       10     8    7    10     9     7   1     4
```

Nuestros datos han sido importados correctamente. Además, el objeto resultante es un data frame, listo para que trabajemos con él.

```r
class(bcancer)
```

```
## [1] "data.frame"
```

### Archivos CSV
Un caso particular de las tablas, son los archivos separados por comas, con extensión **.csv**, por *Comma Separated Values*, sus siglas en inglés. Este es un tipo de archivo comúnmente usado para compartir datos, pues es compatible con una amplia variedad de sistemas diferentes además de que ocupa relativamente poco espacio de almacenamiento.

Este tipo de archivos también se pueden importar usando la función `read.table()`.

Probemos descargando los mismos datos que en el ejemplo anterior, pero almacenados en un archivo con extensión **.csv**.

```r
download.file(
  url = "https://raw.githubusercontent.com/jboscomendoza/r-principiantes-bookdown/master/datos/breast-cancer-wis.csv", 
  dest = "breast-cancer-wis.csv"
)
```

Podemos usar `read.table()` con los mismos argumentos que en el ejemplo anterior, con la excepción de que este archivo sí tiene encabezados de columna, por lo que cambiamos `header` de `FALSE` a `TRUE`.

```r
bcancer <- read.table(file = "datos/breast-cancer-wis.csv", header = TRUE, sep = ",",
                      col.names = nombres)

# Resultado
head(bcancer)
```

```
##        id clump_t u_csize u_cshape m_adh spcs b_nuc b_chr n_nuc mit class
## 1 1000025       5       1        1     1    2     1     3     1   1     2
## 2 1002945       5       4        4     5    7    10     3     2   1     2
## 3 1015425       3       1        1     1    2     2     3     1   1     2
## 4 1016277       6       8        8     1    3     4     3     7   1     2
## 5 1017023       4       1        1     3    2     1     3     1   1     2
## 6 1017122       8      10       10     8    7    10     9     7   1     4
```

Una ventaja de usar documentos con extensión **.csv** es la posibilidad de usar la función `read.csv()`. Esta es una es una versión de `read.table()`, optimizada para importar archivos **.csv**.

`read.csv()` acepta los mismos argumentos que `read.table()`, pero al usarla con un archivo **.csv**, en casi todo los casos, no hará falta especificar nada salvo la ruta del archivo.

```r
bcancer <- read.csv("datos/breast-cancer-wis.csv")

# Resultado
head(bcancer)
```

```
##        id clump_t u_csize u_cshape m_adh spcs b_nuc b_chr n_nuc mit class
## 1 1000025       5       1        1     1    2     1     3     1   1     2
## 2 1002945       5       4        4     5    7    10     3     2   1     2
## 3 1015425       3       1        1     1    2     2     3     1   1     2
## 4 1016277       6       8        8     1    3     4     3     7   1     2
## 5 1017023       4       1        1     3    2     1     3     1   1     2
## 6 1017122       8      10       10     8    7    10     9     7   1     4
```

`read.csv()` también devuelve un data frame como resultado

## Archivos con una estructura desconocida
Habrá ocasiones en las que no estamos seguros del contenido de los archivos que deseamos importar. En estos casos, podemos pedirle a R que intente abrir el archivo en cuestión, usando la función `file.show()`.

Por ejemplo, intentamos abrir el archivo con extensión **.csv** que importamos antes.

```r
file.show("datos/breast-cancer-wis.csv")
```

R intentará usar el programa que en nuestro equipo, por defecto, abre el tipo de archivo que le hemos indicado. Si no tenemos un programa configurado para abrir el tipo de archivo que deseamos, nuestro sistema operativo nos pedirá que elijamos uno.

Lo anterior puede ocurrir si intentas abrir el archivo con extensión **.data** que hemos importado en este capítulo.

```r
file.show("datos/breast-cancer-wis.data")
```

Podemos usar la función `readLines()` para leer un archivo línea por línea. Establecemos el argumento `n = 4` para obtener sólo los primeros cuatro renglones del documento.

```r
readLines("datos/breast-cancer-wis.data", n = 4)
```

```
## [1] "1000025,5,1,1,1,2,1,3,1,1,2"  "1002945,5,4,4,5,7,10,3,2,1,2"
## [3] "1015425,3,1,1,1,2,2,3,1,1,2"  "1016277,6,8,8,1,3,4,3,7,1,2"
```

La salida es una lista de vectores, uno por linea en el archivo. 

Observando la salida de `readLines()` podremos determinar si el archivo que nos interesa puede ser importado usando con los métodos que hemos revisado o necesitaremos de herramientas diferentes.

El documento "R Data Import/Export" (R Core Team, 2018) contiene una guía avanzada sobre el proceso de importar y exportar todo tipo de datos. Puedes consultarlo en el siguiente enlace:

* https://cran.r-project.org/doc/manuals/r-release/R-data.pdf

## Exportar datos
Un paso muy importante en el trabajo con R es exportar los datos que hemos generado, ya sea para que sean usados por otras personas o para almacenar información en nuestro disco duro en lugar de nuestro RAM. 

Dependiendo del tipo de estructura de dato en el que se encuentran contenidos nuestros datos son las opciones que tenemos para exportarlos.

### Data frames y matrices
Si nuestros datos se encuentran contenidos en una estructura de datos rectangular, podemos exportarlos con diferentes funciones.

De manera análoga a `read.table()`, la función `write.table()` nos permite exportar matrices o data frames, como archivos de texto con distintas extensiones.

Los argumentos más usados de `write.table()` son los siguientes.

* `x`:  El nombre del data frame o matriz a exportar.
* `file`: El nombre, extensión y ruta del archivo creado con esta función. Si sólo escribimos el nombre del archivo, este será creado en nuestro directorio de trabajo.
* `sep`: El carácter que se usará como separador de columnas.
* `row.names`: Si deseamos incluir el nombre de los renglones en nuestro objeto al exportarlo, establecemos este argumento como `TRUE`. En general, es recomendable fijarlo como `FALSE`, para conservar una estructura tabular más fácil de leer.
* `col.names`: Si deseamos que el archivo incluya los nombres de las columnas en nuestro objeto, establecemos este argumento como `TRUE`. Es recomendable fijarlo como `TRUE` para evitar la necesidad de almacenar los nombres de columna en documentos distintos.

Puedes consultar todos los argumentos de esta función ejecutando `?write.table`.

Probemos exportando el objeto `iris` a un documento de texto llamado **iris.txt** a nuestro directorio de trabajo, usando como separador la coma, con nombres de columnas y sin nombre de renglones. 

```r
write.table(x = iris, file = "iris.txt", sep = ",", 
            row.names = FALSE, col.names = TRUE)
```

Importemos el archivo que hemos creado usando `read.table()`.

```r
iris_txt <- read.table(file = "iris.txt", header = TRUE, sep = ",")

# Resultado
head(iris_txt)
```

```
##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
## 1          5.1         3.5          1.4         0.2  setosa
## 2          4.9         3.0          1.4         0.2  setosa
## 3          4.7         3.2          1.3         0.2  setosa
## 4          4.6         3.1          1.5         0.2  setosa
## 5          5.0         3.6          1.4         0.2  setosa
## 6          5.4         3.9          1.7         0.4  setosa
```

También podemos exportar datos a archivos con extensión **.csv** con la función `write.csv()`. 

Vamos a exportar `iris` como un documento **.csv**. En este caso, sólo especificamos que no deseamos guardar los nombres de los renglones con `row.names = FALSE`.

```r
write.csv(x = iris, file = "iris.csv", row.names = FALSE) 
```

Importamos el archivo creado.

```r
iris_csv <- read.csv("iris.csv")

# Resultado
head(iris_csv)
```

```
##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
## 1          5.1         3.5          1.4         0.2  setosa
## 2          4.9         3.0          1.4         0.2  setosa
## 3          4.7         3.2          1.3         0.2  setosa
## 4          4.6         3.1          1.5         0.2  setosa
## 5          5.0         3.6          1.4         0.2  setosa
## 6          5.4         3.9          1.7         0.4  setosa
```

### Listas
La manera más sencilla de exportar listas es guardarlas en archivos RDS. Este es un tipo de archivo nativo de R que puede almacenar cualquier objeto a un archivo en nuestro disco duro. 

Además, RDS comprime los datos que almacena, por lo que ocupa menos espacio en disco duro que otros tipos de archivos, aunque contengan la misma información.

Para exportar un objeto a un archivo RDS, usamos la función `saveRDS()` que siempre nos pide dos argumentos:

* `object`: El nombre del objeto a exportar.
* `file`: El nombre y ruta del archivo que crearemos. Los archivos deben tener la extensión **.rds**. Si no especificamos una ruta completa, el archivo será creado en nuestro directorio de trabajo.

Creamos una lista de ejemplo que contiene dos vectores y dos matrices

```r
mi_lista <- list("a" = c(TRUE, FALSE, TRUE),
     "b" = c("a", "b", "c"),
     "c" = matrix(1:4, ncol = 2),
     "d" = matrix(1:6, ncol = 3))


# Resultado
mi_lista
```

```
## $a
## [1]  TRUE FALSE  TRUE
## 
## $b
## [1] "a" "b" "c"
## 
## $c
##      [,1] [,2]
## [1,]    1    3
## [2,]    2    4
## 
## $d
##      [,1] [,2] [,3]
## [1,]    1    3    5
## [2,]    2    4    6
```

Aunque podemos intentar `write.table()` para exportar listas, por lo general obtendremos un error como resultado.

Tratamos de exportar la lista anterior como un archivo **.txt**.

```r
write.table(x = mi_lista, file = "mi_lista.txt")
```

```
## Error in (function (..., row.names = NULL, check.rows = FALSE, check.names = TRUE, : arguments imply differing number of rows: 3, 2
```

Usamos la función `saveRDS()` para exportar al archivo **mi_lista.rds**.

```r
saveRDS(object = mi_lista, file = "mi_lista.rds")
```

Si deseamos importar un archivo RDS a R, usamos la función `readRDS()`, indicando la ruta en la que se encuentra el archivo que deseamos.

Intentemos importar el archivo **mi_lista.rds**.

```r
mi_lista_importado <- readRDS(file = "mi_lista.rds")
```

Vamos el resultado.

```r
mi_lista_importado
```

```
## $a
## [1]  TRUE FALSE  TRUE
## 
## $b
## [1] "a" "b" "c"
## 
## $c
##      [,1] [,2]
## [1,]    1    3
## [2,]    2    4
## 
## $d
##      [,1] [,2] [,3]
## [1,]    1    3    5
## [2,]    2    4    6
```

```r
# El resultado es una lista, al igual que el objeto original
class(mi_lista)
```

```
## [1] "list"
```

Los objetos importados usando un archivo RDS conservan los tipos y clases que tenían originalmente, lo cual previene pérdida de información.

## Hojas de cálculo de Excel
Un formato usado con mucha frecuencia para almacenar archivos son las hojas de cálculo, en particular las generadas por el paquete [*Microsoft Excel*](https://products.office.com/es-mx/excel).

R *base* no tiene una función para importar archivos almacenados en archivos con extensión **.xsl** y **.xslx**, creados con *Excel*.

Para importar datos desde este tipo de archivos, necesitamos instalar el paquete **readxl**, que contiene funciones específicas para realizar esta tarea.

Usamos la función `installpackages()`, como lo vimos en el [capítulo 3](##paquetes)

```r
install.packages("readxl")
```

Ya instalado, cargamos el **readxl** a nuestra sesión de trabajo.

```r
library(readxl)
```

Usaremos, principalmente dos funciones de este paquete.

* `read_excel()`: Para importar archivos **.xls** y **xlsx**.
* `excel_sheets()`: Para obtener los nombres de las pestañas en una hoja de cálculo de *Excel*.

Para probar estas funciones, descargaremos una hoja de cálculo de prueba. Nota que hemos establecido el argumento `mode = "wb"` para asegurar que el archivo se descargue correctamente.

```r
download.file(
  url = "https://github.com/jboscomendoza/r-principiantes-bookdown/raw/master/datos/data_frames.xlsx", 
  destfile = "datos/data_frames.xlsx", 
  mode = "wb"
) 
```

Si intentamos leer las primeras cinco líneas de **data_frames.xlsx**, confirmamos que este es un archivo que no tiene forma rectangular, de tabla.

```r
readLines("datos/data_frames.xlsx", n = 5)
```

```
## Warning in readLines("datos/data_frames.xlsx", n = 5): line 1 appears to contain
## an embedded nul
```

```
## Warning in readLines("datos/data_frames.xlsx", n = 5): incomplete final line
## found on 'datos/data_frames.xlsx'
```

```
## [1] "PK\003\004\024"                                                                                                      
## [2] "\177ß‰YTU,B õ’(±çmöL\177¸jêd\t\001\215³¹èf\035‘\200-œ6v–‹¯É{ú$\022$eµª\235…\\¬\001Åpp\177×Ÿ¬=`ÂÕ\026sQ\021ùg)±¨ Q\2309\017–WJ\027"
```

En caso de que tengamos instalado *Excel* o algún otro programa compatible con archivos de hoja de cálculo, como *LibreOffice Calc* o *Number*, podemos pedir a R que abra este archivo con `file.show()`. De este modo podemos explorar su contenido.

```r
file.show("datos/data_frames.xlsx")
```

La función `excel_sheets()` nos devuelve el nombre de las pestañas como un vector.

```r
excel_sheets("datos/data_frames.xlsx")
```

```
## [1] "iris"  "trees"
```

Este archivo tiene dos pestañas, llamadas **iris** y **trees**. 

Intentaremos importar la pestaña **iris** con `read_excel()`. Esta función tiene los siguientes argumentos principales. 

* `path`: La ruta del archivo a importar. Si no especificamos una ruta completa, será buscado en nuestro directorio de trabajo.
* `sheet`: El nombre de la pestaña a importar. Si no especificamos este argumento, `read_excel()` intentará leer la primera pestaña de la hoja de cálculo.
* `range`: Cadena de texto con el rango de celdas a importar, escrito con el formato usado en *Excel*. Por ejemplo, "A1:B:10".
* `col_names`: Con este argumento indicamos si la pestaña que vamos a importar tiene encabezados para usar como nombres de columna. Por defecto su valor es `TRUE`. Si no tenemos encabezados, podemos dar un vector con nombres para asignar a las columnas.

Puedes consultar todos los argumentos de esta función ejecutando `?read_excel`.

Probemos `read_excel()`.

```r
iris_excel <- read_excel(path = "datos/data_frames.xlsx", sheet = "iris")
```

Nuestro resultado es un data frame.

```r
iris_excel
```

```
## # A tibble: 150 x 5
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species
##           <dbl>       <dbl>        <dbl>       <dbl> <chr>  
##  1          5.1         3.5          1.4         0.2 setosa 
##  2          4.9         3            1.4         0.2 setosa 
##  3          4.7         3.2          1.3         0.2 setosa 
##  4          4.6         3.1          1.5         0.2 setosa 
##  5          5           3.6          1.4         0.2 setosa 
##  6          5.4         3.9          1.7         0.4 setosa 
##  7          4.6         3.4          1.4         0.3 setosa 
##  8          5           3.4          1.5         0.2 setosa 
##  9          4.4         2.9          1.4         0.2 setosa 
## 10          4.9         3.1          1.5         0.1 setosa 
## # ... with 140 more rows
```

Si los datos en la hoja de cálculo tienen forma de tabla, `read_excel()` no tendrá problemas para importarlos. Cuando este no es el caso, usamos el argumento `range` para extraer sólo la información que nos interesa.


Intentamos importar la pestaña **trees**.

```r
trees_excel <- read_excel(path = "datos/data_frames.xlsx", sheet = "trees")
```

```
## New names:
## * `` -> ...2
## * `` -> ...3
## * `` -> ...4
## * `` -> ...5
## * `` -> ...6
```

```r
# Resultado
trees_excel
```

```
## # A tibble: 34 x 6
##    `Datos trees`  ...2  ...3  ...4 ...5  ...6                                   
##    <chr>         <dbl> <dbl> <dbl> <lgl> <chr>                                  
##  1 <NA>           NA      NA  NA   NA    <NA>                                   
##  2 <NA>            8.3    70  10.3 NA    <NA>                                   
##  3 <NA>            8.6    65  10.3 NA    <NA>                                   
##  4 <NA>            8.8    63  10.2 NA    Los nombres de las variables son: Girt~
##  5 <NA>           10.5    72  16.4 NA    <NA>                                   
##  6 <NA>           10.7    81  18.8 NA    <NA>                                   
##  7 <NA>           10.8    83  19.7 NA    <NA>                                   
##  8 <NA>           11      66  15.6 NA    <NA>                                   
##  9 <NA>           11      75  18.2 NA    <NA>                                   
## 10 <NA>           11.1    80  22.6 NA    <NA>                                   
## # ... with 24 more rows
```

Los resultados no lucen bien porque los datos en la pestaña no tienen forma de tabla. 

Ajustamos los argumentos de `read_excel()` para leer correctamente la información de la pestaña. Al explorar manualmente el archivo **data.frames.xlsx**, podemos localizar el rango en el que se encuentran los datos (de las celdas B3 a D33) y los nombres de las columnas (Girth, Height y Volume).

Probemos importar de nuevo con esta información.

```r
trees_excel <- read_excel(path = "datos/data_frames.xlsx", sheet = "trees", 
                          range = "B3:D33", 
                          col_names = c("Girth", "Height", "Volume"))

# Resultado
trees_excel
```

```
## # A tibble: 31 x 3
##    Girth Height Volume
##    <dbl>  <dbl>  <dbl>
##  1   8.3     70   10.3
##  2   8.6     65   10.3
##  3   8.8     63   10.2
##  4  10.5     72   16.4
##  5  10.7     81   18.8
##  6  10.8     83   19.7
##  7  11       66   15.6
##  8  11       75   18.2
##  9  11.1     80   22.6
## 10  11.2     75   19.9
## # ... with 21 more rows
```

Esta vez hemos tenido éxito y los datos importados son los correctos.

El paquete **readxl** tiene más funciones para trabajar con hojas de cálculo además de `read_excel()` y `excel_sheets()`, pero revisar cada una de ellas sale del alcance de este libro. Puedes conocer más sobre ellas en la documentación de **readxl**, llamando `help(package = "readxl")`.

## Datos de paquetes estadísticos comerciales (SPSS, SAS y STATA)
En ciertas disciplinas, el uso de determinados paquetes estadísticos comerciales es sumamente común. Si

Por ejemplo, en Psicología el paquete [*SPSS Statistics*](https://www.ibm.com/products/spss-statistics) de IBM es el paquete estadístico comercial más usado. Si eres psicólogo o psicóloga, o colaboras con psicólogos, es altamente probable que te encuentres con datos contenidos en archivos con extensión **.sav**, el tipo de archivo nativo de **SPSS Statistics**.

Por lo tanto, es conveniente ser capaces de importar y exportar datos almacenados en archivos compatibles con paquetes estadísticos comerciales, pues esto nos permitirá  usar datos ya existentes compatibles con ellos y colaborar con otras personas.

Para este fin, usamos el paquete **haven**.

```r
install.packages("haven")
```

Para usar las funciones de **haven**, lo cargamos a nuestra sesión de trabajo.

```r
library(haven)
```

Las siguientes funciones de **haven** son usadas para importar datos. Todas estas funciones nos piden como argumento `file` la ruta y nombre del archivo a importar, si no especificamos ruta, será buscado en nuestro directorio de trabajo.

* `read_spss()`: *SPSS Statistics*, archivos con extensión **sav**, **zsav** y **por**.
* `read_sav()`: *SPSS Statistics*, sólo archivos **sav**, **zsav**.
* `read_sas()`: *SAS*, archivos **sas7bdat**.
* `read_xpt`: *SAS*, archivos **xpt**.
* `read_stata()`: *Stata*, archivos **dta**.

Todas importan los datos como un data frame.

También podemos exportar nuestros data frames creados en R como archivos compatibles con estos programas con las siguientes funciones. Todas piden el argumento `file`, con la ruta y nombre del archivo a crear. Es muy importante que demos como nombre de archivo uno con la extensión correcta para cada paquete.

* write_sav(): *SPSS Statistics*, archivos **sav**, **zsav** o **por**.
* write_sas(): *SAS*, archivos **sas7bda**.
* write_xpt(): *SAS*, archivos **xpt**.
* write_dta(): *Stata*, archivos **dta**.

Como siempre, puedes leer sobre las demás funciones en el paquete **haven** en su documentación, llamando `help(package = "haven")`.

