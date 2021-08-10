# La familia apply
La familia de funciones `apply` es usada para aplicar una función a cada elemento de una estructura de datos. En particular, es usada para aplicar funciones en matrices, data frames, arrays y listas.

Con esta familia de funciones podemos automatizar tareas complejas usando poca líneas de código y es una de las características distintivas de R como lenguaje de programación.

La familia de funciones `apply` es una expresión de los rasgos del paradigma funcional de programación presentes en R. Sobre esto no profundizaremos demasiado, pero se refiere saber que en R las funciones son "ciudadanos de primera", con la misma importancia que los objetos, y por lo tanto, operamos en ellas. 

La familia de funciones apply no sólo recibe datos como argumentos, también recibe funciones.

### Un recordatorio sobre vectorización
Para entender más fácilmente el uso de la familia 0, recordemos la [vectorización de operaciones](###vectorización-de-operaciones).

Hay operaciones que, si las aplicamos a un vector, son aplicadas a todos sus elementos.

```r
mi_vector <- 1:10

mi_vector
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10
```

```r
mi_vector ^ 2
```

```
##  [1]   1   4   9  16  25  36  49  64  81 100
```

Lo anterior es, generalmente, preferible a escribir una operación para cada elemento o a usar un bucle **for**, como se describió en  el capítulo sobre [estructuras de control](#estructuras-de-control).

Como todo lo que ocurre en R es una función, podemos decir que **al vectorizar estamos aplicando una función a cada elemento de un vector**. La familia de funciones **apply** nos permite implementar esto en estructuras de datos distintas a los vectores.

### Las funciones de la familia apply
La familia apply esta formada por las siguientes funciones:

* `apply()`
* `eapply()`
* `lapply()`
* `mapply()`
* `rapply()`
* `sapply()`
* `tapply()`
* `vapply()`

Es una familia numerosa y esta variedad de funciones se debe a que varias de ellas tienen aplicaciones sumamente específicas.

Todas las funciones de esta familia tienen una característica en común: **reciben como argumentos a un objeto y al menos una función**. 

Hasta ahora, todas las funciones que hemos usado han recibido como argumentos estructuras de datos, sean vectores, data frames o de otro tipo. Las funciones de la familia apply tienen la particularidad que pueden recibir a otra función como un argumento. Lo anterior puede sonar confuso, pero es más bien intuitivo al verlo implementado.

Nosotros trabajaremos con las funciones más generales y de uso común de esta familia:

* `apply()`
* `lapply()`

Estas dos funciones nos permitirán solucionar casi todos los problemas a los que nos encontremos. Además, conociendo su uso, las demás funciones de la familia **apply** serán relativamente fáciles de entender.

## apply
`apply` aplica una función a todos los elementos de una **matriz**.

La estructura de esta función es la siguiente.

```r
apply(X, MARGIN, FUN)
```

`apply` tiene tres argumentos:

* `X`: Una matriz o un objeto que pueda coercionarse a una matriz, generalmente, un data frame.
* `MARGIN`: La dimensión (margen) que agrupará los elementos de la matriz `X`, para aplicarles una función. Son identificadas con números, **1** son renglones y **2** son columnas.
* `FUN`: La función que aplicaremos a la matriz `X` en su dimensión `MARGIN`.

### ¿Qué es X
`X` es una matriz o cualquier otro objeto que sea posible coercionar a una matriz. Esto es, principalmente, vectores y data frames. 

Recuerda que puedes coercionar objetos a matriz usando `as.matrix()` y puedes comprobar si un objeto es de esta clase con `is.matrix()`.

```r
# Creamos un data frame
mi_df <- data.frame(v1 = 1:3, v2 = 4:6)

mi_df
```

```
##   v1 v2
## 1  1  4
## 2  2  5
## 3  3  6
```

```r
# Coerción a matriz
mi_matriz <- as.matrix(mi_df)

# Verificamos que sea matriz
is.matrix(mi_matriz)
```

```
## [1] TRUE
```

```r
# Resultado
mi_matriz
```

```
##      v1 v2
## [1,]  1  4
## [2,]  2  5
## [3,]  3  6
```

Aunque también podemos coercionar listas y arrays a matrices, los resultados que obtenemos no siempre son apropiados para `apply()`, por lo que no es recomendable usar estos objetos como argumentos.

### ¿Qué es MARGIN?
Recuerda que las matrices y los data frames están formadas por vectores y que estas estructuras tienen dos dimensiones, ordenadas en renglones y columnas. Esto lo vimos en en [Matrices y arrays](##matrices-y-arrays) y [Data frames](##data-frames).

Para `MARGIN`:

* 1 es renglones.
* 2 es columnas.

Por ejemplo, podemos usar `apply()` para obtener la sumatoria de los elementos de una matriz, por renglón.

Creamos una matriz de cuatro renglones.

```r
matriz <- matrix(1:14, nrow = 4) 
```

```
## Warning in matrix(1:14, nrow = 4): la longitud de los datos [14] no es un
## submúltiplo o múltiplo del número de filas [4] en la matriz
```

Aplicamos `apply()`, dando la función `sum()` el argumento `FUN`, nota que sólo necesitamos el nombre de la función, sin paréntesis.

Por último, damos el argumento `MARGIN = 1`, para aplicar la función por renglón.

```r
apply(X = matriz, MARGIN = 1, FUN = sum)
```

```
## [1] 28 32 22 26
```

Esto es equivalente a hacer lo siguiente.

```r
sum(matriz[1, ])
```

```
## [1] 28
```

```r
sum(matriz[2, ])
```

```
## [1] 32
```

```r
sum(matriz[3, ])
```

```
## [1] 22
```

```r
sum(matriz[4, ])
```

```
## [1] 26
```

Y naturalmente, es equivalente a hacer lo siguiente.


**Estamos aplicando una función a cada elemento de nuestra matriz. Los elementos son los renglones. Cada renglón es un vector. Cada vector es usado como argumento de la función.**

Si cambiamos el argumento MARGIN de `MARGIN = 1` a `MARGIN = 2`, entonces la función se aplicará por columna.

```r
apply(X = matriz, MARGIN = 2, FUN = sum)
```

```
## [1] 10 26 42 30
```

En este caso, la función `sum()` ha sido aplicado a cada elementos de nuestra matriz, los elementos son las columnas, y cada columna es un vector.

### ¿Qué es FUN?
FUN es un argumento que nos pide el **nombre de una función que se se aplicarla a todos los elementos de nuestra matriz**.

El ejemplo de la sección anterior aplicamos las funciones `mean()` y `sum()` usando sus nombres, sin paréntesis, esto es, sin especificar argumentos.

Podemos dar como argumento cualquier nombre de función, siempre y cuando ésta acepte vectores como argumentos.

Probemos cambiando el argumento `FUN`. Usaremos la función `mean()` para obtener la media de cada renglón y de cada columna.

Aplicado a los renglones.

```r
apply(matriz, 1, mean)
```

```
## [1] 7.0 8.0 5.5 6.5
```

Aplicado a las columnas

```r
apply(matriz, 2, mean)
```

```
## [1]  2.5  6.5 10.5  7.5
```

Las siguientes llamadas a `sd()`, `max()` y `quantile()` se ejecutan sin necesidad de especificar argumentos.

```r
# Desviación estándar
apply(matriz, 1, FUN = sd)
```

```
## [1] 5.163978 5.163978 4.434712 4.434712
```

```r
# Máximo
apply(matriz, 1, FUN = max)
```

```
## [1] 13 14 11 12
```

```r
# Cuantiles
apply(matriz, 1, FUN = quantile)
```

```
##      [,1] [,2] [,3] [,4]
## 0%      1    2  1.0  2.0
## 25%     4    5  2.5  3.5
## 50%     7    8  5.0  6.0
## 75%    10   11  8.0  9.0
## 100%   13   14 11.0 12.0
```

### ¿Cómo sabe FUN cuáles son sus argumentos?
Recuerda que podemos llamar una función y proporcionar sus argumentos en orden, tal como fueron establecidos en su definición.

Por lo tanto, **el primer argumento que espera la función, será la `X` del `apply()`**.

Para ilustrar esto, usaremos la función `quantile()`. Llama `?quantile` en la consola para ver su documentación.

```r
?quantile
```

`quantile()` espera siempre un argumento `x`, que debe ser un vector numérico, además tener varios argumentos adicionales. 

* `probs` es un vector numérico con las probabilidades de las que queremos extraer cuantiles.
* `na.rm`, si le asignamos `TRUE` quitará de x los `NA` y `NaN` antes de realizar operaciones. 
* `names`, si le asignamos `TRUE`, hará que el objeto resultado de la función tenga nombres. 
* `type` espera un valor entre 1 y 9, para determinar el algoritmo usado para el cálculo de los cuantiles.

En orden, el primer argumento es `x`, el segundo `probs`, y así sucesivamente.

Cuando usamos `quantile()` en un `apply()`, el argumento `x` de la función será cada elemento de nuestra matriz. Es decir, los vectores como renglones o columnas de los que está constituida la matriz.

Esto funcionará siempre y cuando los argumentos sean apropiados para la función. Si proporcionamos un argumento inválido, la función no se ejecutará y **apply** fallará.

Por ejemplo, intentamos obtener cuantiles de las columnas de una matriz, en la que una de ellas es de tipo carácter.

Creamos una matriz.

```r
matriz2 <- matrix(c(1:2, "a", "b"), nrow = 2)

# Resultado
```

Aplicamos la función y obtenemos un error.

```r
apply(matriz2, 2, quantile)
```

```
## Error in (1 - h) * qs[i]: argumento no-numérico para operador binario
```

Por lo tanto, **apply sólo puede ser usado con funciones que esperan vectores como argumentos**.

### ¿Qué pasa si deseamos utilizar los demás argumentos de una función con apply?
En los casos en los que una función tiene recibe más de un argumento, asignamos los valores de estos del nombre de la función, separados por comas, usando sus propios nombres (a este procedimiento es al que se refiere el argumento `...` descrito en la documentación de `apply`).

Supongamos que deseamos encontrar los cuantiles de un vector, correspondientes a las probabilidades **.33** y **.66**. Esto es definido con el argumento `probs` de esta función. 

Para ello, usamos `quantile()` y después de haber escrito el nombre de la función, escribimos el nombre del argumento probs y los valores que deseamos para este.

```r
apply(X = matriz, MARGIN = 2, FUN = quantile, probs = c(.33, .66))
```

```
##     [,1] [,2]  [,3]  [,4]
## 33% 1.99 5.99  9.99  1.99
## 66% 2.98 6.98 10.98 12.78
```

Como podrás ver, hemos obtenido los resultados esperados.

Si además deseamos que el resultado aparezca sin nombres, entonces definimos el valor del argumento `names` de la misma manera.

```r
apply(matriz, 2, quantile, probs = c(.33, .66), names = FALSE)
```

```
##      [,1] [,2]  [,3]  [,4]
## [1,] 1.99 5.99  9.99  1.99
## [2,] 2.98 6.98 10.98 12.78
```

De este modo es posible aplicar funciones complejas que aceptan múltiples argumentos, con la ventaja que usamos pocas líneas de código.

### ¿Qué tipo de resultados devuelve apply?
En los ejemplos anteriores, el resultado de `apply()` en algunas ocasiones fue un vector y en otros fue una matriz. 

Si aplicamos `mean()`, obtenemos como resultado un vector.

```r
mat_media <- apply(matriz, 1, mean)

class(mat_media)
```

```
## [1] "numeric"
```

Pero si aplicamos `quantile()`, obtenemos una matriz.

```r
mat_cuant <- apply(matriz, 1, quantile)

class(mat_cuant)
```

```
## [1] "matrix" "array"
```

Este comportamiento se debe a que **`apply()` nos devolverá objetos del mismo tipo que la función aplicada devuelve**. Dependiendo de la función, será el tipo de objeto que obtengamos. 

Sin embargo, este comportamiento puede causarte algunos problemas. En primer lugar, anterior te obliga a conocer de antemano el tipo del resultado que obtendrás, lo cual no siempre es fácil de determinar, en particular si las funciones que estás utilizando son poco comunes o tienen comportamientos poco convencionales.

Cuando estás trabajando en proyectos en los que el resultado de una operación será usado en operaciones posteriores, corres el riesgo de que en alguna parte del proceso, un `apply()` te devuelva un resultado que te impida continuar adelante.

Con algo de práctica es más o menos sencillo identificar problemas posibles con los resultados de `apply()`, pero es algo que debes tener en cuenta, pues puede explicar por qué tu código no funciona como esperabas.

En este sentido, `lapply()` tiene la ventaja de que siempre devuelve una lista.

## lapply
`lapply()` es un caso especial de `apply()`, diseñado para **aplicar funciones a todos los elementos de una lista**. La **l** de su nombre se refiere, precisamente, a **lista**. 

`lapply()` intentará coercionar a una lista el objeto que demos como argumento y después aplicará una función a todos sus elementos. 

`lapply` siempre nos devolverá una lista como resultado. A diferencia de `apply`, sabemos que siempre obtendremos un objeto de tipo lista después de aplicar una función, sin importar cuál función sea.

Dado que en R todas las estructuras de datos pueden coercionarse a una lista, `lapply()` puede usarse en un número más amplio de casos que `apply()`, además de que esto nos permite utilizar funciones que aceptan argumentos distintos a vectores.

La estructura de esta función es:

```r
lapply(X, FUN)
```

En donde:

* `X` es una lista o un objeto coercionable a una lista.
* `FUN` es la función a aplicar.

Estos argumentos son idéntico a los de `apply()`, pero a diferencia aquí no especificamos `MARGIN`, pues las listas son estructuras con una unidimensionales, que sólo tienen largo.

### Usando lapply()
Probemos `lapply()` aplicando una función a un data frame. Usaremos el conjunto de datos `trees`, incluido por defecto en R *base*.

`trees` contiene datos sobre el grueso, alto y volumen de distinto árboles de cerezo negro. Cada una de estas variables está almacenada en una columna del data frame.

Veamos los primeros cinco renglones de `trees`.

```r
trees[1:5, ]
```

```
##   Girth Height Volume
## 1   8.3     70   10.3
## 2   8.6     65   10.3
## 3   8.8     63   10.2
## 4  10.5     72   16.4
## 5  10.7     81   18.8
```

Aplicamos la función `mean()`, usando su nombre.

```r
lapply(X = trees, FUN = mean)
```

```
## $Girth
## [1] 13.24839
## 
## $Height
## [1] 76
## 
## $Volume
## [1] 30.17097
```

Dado que un data frame está formado por columnas y cada columna es un vector atómico, cuando usamos `lapply()` , la función es aplicada a cada columna. `lapply()`, a diferencia de `apply()` no puede aplicarse a renglones.

En este ejemplo, obtuvimos la media de grueso (Girth), alto (Height) y volumen (Volume), como una lista.

Verificamos que la clase de nuestro resultado es una lista con `class()`.

```r
arboles <- lapply(X = trees, FUN = mean)

class(arboles)
```

```
## [1] "list"
```

Esto es muy conveniente, pues la recomendación para almacenar datos en un data frame es que cada columna represente una variable y cada renglón un caso (por ejemplo, el enfoque **tidy** de [Wickham (2014)](https://www.jstatsoft.org/article/view/v059i10/v59i10.pdf)). Por lo tanto,  con `lapply()` podemos manipular y transformar datos, por variable.

Al igual que con `apply()`, podemos definir argumentos adicionales a las funciones que usemos, usando sus nombres, después del nombre de la función.

```r
lapply(X = trees, FUN = quantile, probs = .8)
```

```
## $Girth
##  80% 
## 16.3 
## 
## $Height
## 80% 
##  81 
## 
## $Volume
##  80% 
## 42.6
```

Si usamos `lapply` con una matriz, la función se aplicará a cada **celda** de la matriz, no a cada columna.

Creamos una matriz.

```r
matriz <- matrix(1:9, ncol = 3)

# Resultado
matriz
```

```
##      [,1] [,2] [,3]
## [1,]    1    4    7
## [2,]    2    5    8
## [3,]    3    6    9
```

Llamamos a  `lapply()`.

```r
lapply(matriz, quantile, probs = .8)
```

```
## [[1]]
## 80% 
##   1 
## 
## [[2]]
## 80% 
##   2 
## 
## [[3]]
## 80% 
##   3 
## 
## [[4]]
## 80% 
##   4 
## 
## [[5]]
## 80% 
##   5 
## 
## [[6]]
## 80% 
##   6 
## 
## [[7]]
## 80% 
##   7 
## 
## [[8]]
## 80% 
##   8 
## 
## [[9]]
## 80% 
##   9
```

Para usar una matriz con `lapply()` y que la función se aplique a cada columna, primero la coercionamos a un data frame con la función `as.data.frame()`

```r
lapply(as.data.frame(matriz), quantile, probs = .8)
```

```
## $V1
## 80% 
## 2.6 
## 
## $V2
## 80% 
## 5.6 
## 
## $V3
## 80% 
## 8.6
```

Si deseamos aplicar una función a los renglones de una matriz, una manera de lograr es transponer la matriz con `t()` y después coercionar a un data frame.

```r
matriz_t <- t(matriz)

lapply(as.data.frame(matriz_t), quantile, probs = .8)
```

```
## $V1
## 80% 
## 5.8 
## 
## $V2
## 80% 
## 6.8 
## 
## $V3
## 80% 
## 7.8
```

Con vectores como argumento, `lapply()` aplicará la función a cada elementos del vector, de manera similar a una vectorización de operaciones. 

Por ejemplo, usamos `lapply()` para obtener la raíz cuadrada de un vector numérico del 1 al 4, con la función `sqrt()`.

```r
mi_vector <- 1:4

lapply(mi_vector, sqrt)
```

```
## [[1]]
## [1] 1
## 
## [[2]]
## [1] 1.414214
## 
## [[3]]
## [1] 1.732051
## 
## [[4]]
## [1] 2
```

### Usando lapply() en lugar de un bucle for
En muchos casos es posible reemplazar un bucle `for()` por un `lapply()`. 

De hecho, `lapply()` está haciendo lo mismo que un `for()`, está iterando una operación en todos los elementos de una estructura de datos.

Por lo tanto, el siguiente código con un `for()`...

```r
mi_vector <- 6:12
resultado <- NULL
posicion <- 1

for(numero in mi_vector) {
  resultado[posicion] <- sqrt(numero)
  posicion <- posicion + 1
}

resultado
```

```
## [1] 2.449490 2.645751 2.828427 3.000000 3.162278 3.316625 3.464102
```

... nos dará los mismos resultados que el siguiente código con `lapply()`.

```r
resultado <- NULL

resultado <- lapply(mi_vector, sqrt)

resultado
```

```
## [[1]]
## [1] 2.44949
## 
## [[2]]
## [1] 2.645751
## 
## [[3]]
## [1] 2.828427
## 
## [[4]]
## [1] 3
## 
## [[5]]
## [1] 3.162278
## 
## [[6]]
## [1] 3.316625
## 
## [[7]]
## [1] 3.464102
```

El código con `lapply()` es mucho más breve y más sencillo de entender, al menos para otros usuarios de R.

El inconveniente es que obtenemos una lista como resultado en lugar de un vector, pero eso es fácil de resolver usando la función `as.numeric()` para hacer coerción a tipo numérico.

```r
as.numeric(resultado)
```

```
## [1] 2.449490 2.645751 2.828427 3.000000 3.162278 3.316625 3.464102
```

El siguiente código es la manera en la que usamos `for()` si deseamos aplicar una función a todas sus columnas, tiene algunas partes que no hemos discutido, pero es sólo para ilustrar la diferencia simplemente usar `trees_max <- lapply(trees, max)`.

```r
trees_max <- NULL
i <- 1
columnas <- ncol(trees)

for(i in 1:columnas) {
  trees_max[i] <- max(trees[, i])
  i <- i +1
}

trees_max
```

```
## [1] 20.6 87.0 77.0
```

### Usando lapply con listas
Hasta hora hemos hablado de usar `lapply()` con objetos que pueden coercionarse a una lista, pero ¿qué pasa si usamos esta función con una lista que contiene a otros objetos?

Pues la función se aplicará a cada uno de ellos. Por lo tanto, así podemos utilizar funciones que acepten todo tipo de objetos como argumento. Incluso podemos aplicar funciones a listas recursivas, es decir, listas de listas.

Por ejemplo, obtendremos el coeficiente de correlación de cuatro data frames contenidos en una sola lista. Esto no es posible con `apply()`, porque sólo podemos usar funciones que aceptan vectores como argumentos, pero con `lapply()` no es ningún problema.

Empezaremos creando una lista de data frames. Para esto, usaremos las función `rnorm()`, que genera números al azar y `set.seed()`, para que obtengas los mismos resultados aquí mostrados.

`rnorm()` creara `n` números al azar (pseudoaleatorios, en realidad), sacados de una distribución normal con media 0 y desviación estándar 1. `set.seed()` es una función que "fija" los resultados de una generación de valores al azar. Cada que ejecutas `rnorm()` obtienes resultados diferentes, pero si das un número como argumento `seed` a `set.seed()`, siempre obtendrás los mismos números.

```r
# Fijamos seed
set.seed(seed = 2018)

# Creamos una lista con tres data frames dentro
tablas <- list(
  df1 = data.frame(a = rnorm(n = 5), b = rnorm(n = 5), c = rnorm(n = 5)),
  df2 = data.frame(d = rnorm(n = 5), e = rnorm(n = 5), f = rnorm(n = 5)),
  df3 = data.frame(g = rnorm(n = 5), h = rnorm(n = 5), i = rnorm(n = 5))
)

# Resultado
tablas
```

```
## $df1
##             a          b          c
## 1 -0.42298398 -0.2647112 -0.6430347
## 2 -1.54987816  2.0994707 -1.0300287
## 3 -0.06442932  0.8633512  0.7124813
## 4  0.27088135 -0.6105871 -0.4457721
## 5  1.73528367  0.6370556  0.2489796
## 
## $df2
##            d          e          f
## 1 -1.0741940  1.2638637 -0.2401222
## 2 -1.8272617  0.2501979 -1.0586618
## 3  0.0154919  0.2581954  0.4194091
## 4 -1.6843613  1.7855342 -0.2709566
## 5  0.2044675 -1.2197058 -0.6318248
## 
## $df3
##            g          h          i
## 1 -0.2284119 -0.4897908 -0.3594423
## 2  1.1786797  1.4105216 -1.2995363
## 3 -0.2662727 -1.0752636 -0.8698701
## 4  0.5281408  0.2923947  1.0543623
## 5 -1.7686592 -0.2066645 -0.1486396
```

Para obtener el coeficiente de correlación usaremos la función `cor()`. 

Esta función acepta como argumento una data frame o una matriz. Con este objeto, calculará el coeficiente de correlación **R de Pearson** existente entre cada una de sus columnas. Como resultado obtendremos una matriz de correlación.

Por ejemplo, este es el resultado de aplicar `cor()` a `iris`.

```r
cor(iris[1:4])
```

```
##              Sepal.Length Sepal.Width Petal.Length Petal.Width
## Sepal.Length    1.0000000  -0.1175698    0.8717538   0.8179411
## Sepal.Width    -0.1175698   1.0000000   -0.4284401  -0.3661259
## Petal.Length    0.8717538  -0.4284401    1.0000000   0.9628654
## Petal.Width     0.8179411  -0.3661259    0.9628654   1.0000000
```

Con `lapply` aplicaremos `cor()` a cada uno de los data frames contenidos en nuestra lista. El resultado será una lista de matrices de correlaciones.

Esto lo logramos con una línea de código.

```r
lapply(X = tablas, FUN = cor)
```

```
## $df1
##            a          b          c
## a  1.0000000 -0.4427336  0.6355358
## b -0.4427336  1.0000000 -0.1057007
## c  0.6355358 -0.1057007  1.0000000
## 
## $df2
##            d          e         f
## d  1.0000000 -0.6960942 0.4709283
## e -0.6960942  1.0000000 0.2624429
## f  0.4709283  0.2624429 1.0000000
## 
## $df3
##            g          h          i
## g  1.0000000  0.6228793 -0.1472657
## h  0.6228793  1.0000000 -0.1211321
## i -0.1472657 -0.1211321  1.0000000
```

De esta manera puedes manipular información de múltiples data frames, matrices o listas con muy pocas líneas de código y, en muchos casos, más rápidamente que con las alternativas existentes.

Finalmente, si asignamos los resultados de las última operación a un objeto, podemos usarlos y manipularlos de la misma manera que cualquier otra lista.


```r
correlaciones <- lapply(tablas, cor)

# Extraemos el primer elemento de la lista
correlaciones[[1]]
```

```
##            a          b          c
## a  1.0000000 -0.4427336  0.6355358
## b -0.4427336  1.0000000 -0.1057007
## c  0.6355358 -0.1057007  1.0000000
```
