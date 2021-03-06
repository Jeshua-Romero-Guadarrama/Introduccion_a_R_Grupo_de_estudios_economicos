# Tipos de datos
En R los datos pueden ser de diferentes tipos. Cada tipo tiene características particulares que lo distinguen de los demás. Entre otras cosas algunas operaciones sólo pueden realizarse con tipos de datos específicos

En este capítulo revisaremos los tipos de datos más comunes en R y sus propiedades, así como la coerción entre tipos de dato.

## Datos más comunes
Los tipos de datos de uso más común en R son los siguientes.

Tipo            | Ejemplo | Nombre en inglés
-----           | -----   |----
Entero          | 1       | integer
Numérico        | 1.3     | numeric
Cadena de texto | "uno"   | character
Factor          | uno     | factor
Lógico          | TRUE    | logical
Perdido         | `NA`    | NA
Vacío           | `NULL`  | null

Además de estos tipos, en R también contamos con datos complejos numéricos complejos (con una parte real y una imaginaria), **raw** (bytes), fechas y *raster*, entre otros. Estos tipos tiene aplicaciones muy específicas, por ejemplo, los datos de tipo fecha son ampliamente usados en economía, para análisis de series de tiempo.

Revisemos las principales características de estos tipos de dato.

## Entero y numérico
Como su nombre lo indica, los datos enteros representan números enteros, sin una parte decimal o fraccionaria, que pueden ser usados en operaciones matemáticas. 

Por su parte, como su nombre lo indica, los datos numéricos representan números, la diferencia de estos con los datos enteros es que tiene una parte decimal o fraccionaria.

Los datos numéricos también son llamados *doble* o *float* (flotantes). Este nombre se debe a que, en realidad, son números de doble precisión, pues tienen una parte entera y una fraccionaria decimal, y son llamados *float* debido a que se usa un punto flotante para su representación computacional. 

Para fines prácticos, estos términos son sinónimos. En este libro, siempre que hablemos de datos numéricos, nos referimos a este tipo. 

## Cadena de texto
El tipo *character* representa texto y es fácil reconocerlo porque un dato siempre esta rodeado de comillas, simples o dobles. De manera convencional, nos referimos a este tipo de datos como cadenas de texto, es decir, secuencias de caracteres.

Este es el tipo de datos más flexible de R, pues una cadena de texto puede contener letras, números, espacios, signos de puntuación y símbolos especiales.

## Factor
Un factor es un tipo de datos específico a R. Puede ser descrito como un dato numérico representado por una etiqueta. 

Supongamos que tenemos un conjunto de datos que representan el sexo de personas encuestadas por teléfono, pero estos se encuentran capturados con los números 1 y 2. El número 1 corresponde a **femenino** y  el 2 a **masculino**. 

En R, podemos indicar que se nos muestre, en la consola y para otros análisis, los 1 como `femenino` y los 2 como `masculino`. Aunque para nuestra computadora, `femenino` tiene un valor de 1, pero a nosotros se nos muestra la palabra `femenino`. De esta manera reducimos el espacio de almacenamiento necesario para nuestros datos.

Este comportamiento es similar a lo que ocurre con paquetes estadísticos comerciales como *SPSS Statistics*, en los que podemos asignar etiquetas a los datos, dependiendo de su valor. La diferencia se encuentra en que R trata a los factores de manera diferente a un dato numérico.

Por último, cada una de las etiquetas o valores que puedes asumir un factor se conoce como **nivel**. En nuestro ejemplo con `femenino` y `masculino`, tendríamos dos niveles.

## Lógico
Los datos de tipo lógico sólo tienen dos valores posibles: verdadero (`TRUE`) y falso (`FALSE`). Representan si una condición o estado se cumple, es verdadero, o no, es falso.

Este tipo de dato es, generalmente, el resultado de operaciones relacionales y lógicas, son esenciales para trabajar con **álgebra Booleana**, lo cual revisaremos en el (capítulo 5)(#-operadores).

Como este tipo de dato sólo admite dos valores específicos, es el más restrictivo de R.

## `NA` y `NULL`
En R, usamos `NA` para representar datos perdidos, mientras que `NULL` representa la ausencia de datos.

La diferencia entre las dos es que un dato `NULL` aparece sólo cuando R intenta recuperar un dato y no encuentra nada, mientras que `NA` es usado para representar de modo explícito datos perdidos, omitidos o que por alguna razón son faltantes.

Por ejemplo, si tratamos de recuperar la edad de una persona encuestada que no existe, obtendríamos un `NULL`, pues no hay ningún dato que corresponda con ello. En cambio, si tratamos de recuperar su estado civil, y la persona encuestada no contestó esta pregunta, obtendríamos un `NA`.

`NA` además puede aparecer como resultado de una operación realizada, pero no tuvo éxito en su ejecución.

## Coerción
En R, los datos pueden ser coercionados, es decir, forzados, para transformarlos de un tipo a otro.

La coerción es muy importante. Cuando pedimos a R ejecutar una operación, intentará coercionar de manera **implícita**, sin avisarnos, los datos de su tipo original al tipo correcto que la permita realizar. Habrá ocasiones en las que R tenga éxito y la operación ocurra sin problemas, y otras en las que falle y obtengamos un error.

Lo anterior ocurre porque no todos los tipos de datos pueden ser transformados a los demás, para ello se sigue una regla general.

**La coerción de tipos se realiza de los tipos de datos más restrictivos a los más flexibles.**

Las coerciones ocurren en el siguiente orden.

`lógico -> entero -> numérico -> cadena de texto`
(`logical -> integer -> numeric -> character`)

Las coerciones no pueden ocurrir en orden inverso. Podemos coercionar un dato de tipo entero a uno numérico, pero uno de cadena de texto a numérico.

Como los datos de tipo lógico sólo admiten dos valores (`TRUE` y `FALSE`), estos son los más restrictivos; mientras que los datos de cadena de texto, al admitir cualquier cantidad y combinación de caracteres, son los más flexibles. 

Los factores son un caso particular para la coerción. Dado que son valores numéricos con etiquetas, pueden ser coercionados a tipo numérico y cadena de texto; y los datos numéricos y cadena de texto pueden ser coercionados a factor. Sin embargo, al coercionar un factor tipo numérico, perdemos sus niveles.

### Coerción explícita con la familia `as()`
También podemos hacer coerciones **explícitas** usando la familia de funciones `as()`.

Función           | Tipo al que hace coerción
-----             | -----
`as.integer()`    | Entero
`as.numeric()`    | Numérico
`as.character()`  | Cadena de texto
`as.factor()`     | Factor
`as.logical()`    | Lógico
`as.null()`       | `NULL`

Todas estas funciones aceptan como argumento datos o vectores (veremos qué es un vector en el [capítulo 6](#estructuras-de-datos)). 

Cuando estas funciones tienen éxito en la coerción, nos devuelven datos del tipo pedido. Si fallan, obtenemos `NA` como resultado.

Por ejemplo, intentemos convertir el número 5 a una cadena de texto. Para ello usamos la función `as.character()`.

```r
as.character(5)
```

```
## [1] "5"
```

Esta es una coerción válida, así que tenemos éxito. Pero, si intentamos convertir la palabra "cinco" a un dato numérico, obtendremos una advertencia y `NA`.

```r
as.numeric("cinco")
```

```
## Warning: NAs introducidos por coerción
```

```
## [1] NA
```

Comprobemos el comportamiento especial de los factores. 

Podemos coercionar al número 5 y la palabra "cinco" en un factor. 


```r
as.factor(5)
```

```
## [1] 5
## Levels: 5
```

```r
as.factor("cinco")
```

```
## [1] cinco
## Levels: cinco
```

Asignamos la palabra "cinco" como factor al objeto `factor_cinco`.

```r
factor_cinco <- as.factor("cinco")

#Resultado
factor_cinco
```

```
## [1] cinco
## Levels: cinco
```

Ahora podemos coercionar `factor_cinco` a cadena de texto y a numérico.

```r
# Cadena de texto
as.character(factor_cinco)
```

```
## [1] "cinco"
```

```r
# Numérico
as.numeric(factor_cinco)
```

```
## [1] 1
```

Si coercionamos un dato de tipo lógico a numérico, `TRUE` siempre devolverá 1 y `FALSE` dará como resultado 0.

```r
as.numeric(TRUE)
```

```
## [1] 1
```

```r
as.numeric(FALSE)
```

```
## [1] 0
```

Por último, la función `as.null()` siempre devuelve `NULL`, sin importar el tipo de dato que demos como argumento.


```r
# Lógico
as.null(FALSE)
```

```
## NULL
```

```r
# Numérico
as.null(457)
```

```
## NULL
```

```r
# Cadena de texto
as.null("palabra")
```

```
## NULL
```

## Verificar el tipo de un dato
En ocasiones, tenemos datos pero no sabemos de simple vistazo de qué tipo son. Para esto casos, podemos usar la función `class()` para determinar el tipo de un dato. Esto es de utilidad para asegurarnos que las operaciones que deseamos realizar tendrán los datos apropiados para llevarse a cabo con éxito.

`class()` recibe como argumento un dato o vector y devuelve el nombre del tipo al que pertenece, en inglés.

Por ejemplo, verificamos el tipo de datos que son 3, "3" y `TRUE`.

```r
class(3)
```

```
## [1] "numeric"
```

```r
class("3")
```

```
## [1] "character"
```

```r
class(TRUE)
```

```
## [1] "logical"
```

### Verificación con la familia de funciones `is()`
También podemos verificar si un dato es de un tipo específico con la familia de funciones `is()`. 

Función           | Tipo que verifican
-----             | -----
`is.integer()`    | Entero
`is.numeric()`    | Numérico
`is.character()`  | Cadena de texto
`is.factor()`     | Factor
`is.logical()`    | Lógico
`is.na()`         | `NA`
`is.null()`       | `NULL`

Estas funciones toman como argumento un dato, si este es del tipo que estamos verificando, nos devolverán `TRUE` y en caso contrario devolverán `FALSE`.

Por ejemplo, verificamos que 5 sea numérico.

```r
is.numeric(5)
```

```
## [1] TRUE
```
Obtenemos `TRUE`, pues es verdadero que este es un dato numérico.

Verificamos que `5` sea de tipo cadena de texto.

```r
is.character(5)
```

```
## [1] FALSE
```
El resultado es `FALSE`, por lo tanto este no es un dato de cadena de texto.

Conociendo el tipo de datos con los que estamos trabajando, nos aseguramos de que obtendremos los resultados esperados para las operaciones que estemos realizando.
