# Operadores
Los operadores son los símbolos que le indican a R que debe realizar una tarea. Combinando datos y operadores es que logramos que R haga su trabajo.

Existen operadores específicos para cada tipo de tarea. Los tipos de operadores principales son los siguientes:

* Aritméticos
* Relacionales
* Lógicos
* De asignación

Familiarizarnos con los operadores nos permitirá manipular y transformar datos de distintos tipos. 

## Operadores aritméticos
Como su nombre lo indica, este tipo de operador es usado para operaciones aritméticas.

En R tenemos los siguientes operadores aritméticos:

Operador  | Operación       | Ejemplo | Resultado
----      |----             |----     |----
`+`       | Suma            | `5 + 3` | 8
`-`       | Resta           | `5 - 3` | 2
`*`       | Multiplicación  | `5 * 3` | 18
`/`       | División        | `5 /3`  | 1.666667
`^`       | Potencia        | `5 ^ 3` | 125
`%%`      | División entera | `5 %% 3`| 2

Es posible realizar operaciones aritméticas con datos de tipo **entero** y **numérico**. 

Si escribes una operación aritmética en la consola de R y das *Enter*, esta se realiza y se devuelve su resultado.

```r
15 * 3
```

```
## [1] 45
```

Cuando intentas realizar una operación aritmética con otro tipo de dato, R primero intentará coercionar ese dato a uno numérico. Si la coerción tiene éxito se realizará la operación normalmente, si falla, el resultado será un error.

Por ejemplo, `4 + "tres"` devuelve: `Error in 4 + "tres" : non-numeric argument for binary operator."`

```r
4 + "tres"
```

```
## Error in 4 + "tres": argumento no-numérico para operador binario
```
  
El mensaje *"non-numeric argument for binary operator"* aparece siempre que intentas realizar una operación aritmética con un argumento no numérico. Si te encuentras un un error que contiene este mensaje, es la primera pista para que identifiques donde ha ocurrido un problema.

Cualquier operación aritmética que intentemos con un dato `NA`, devolverá `NA` como resultado.

```r
NA - 66
```

```
## [1] NA
```

```r
21 * NA
```

```
## [1] NA
```

```r
NA ^ 13
```

```
## [1] NA
```

### La división entera
Entre los operadores aritméticos, el de división entera o **módulo** requiere una explicación adicional sobre su uso. La operación que realiza es una división de un número entre otro, pero en lugar de devolver el cociente, nos devuelve el residuo.

Por ejemplo, si hacemos una división entera de 4 entre 2, el resultado será 0. Esta es una división exacta y no tiene residuo.

```r
4 %% 2
```

```
## [1] 0
```

En cambio, si hacemos una división entera de 5 entre 2, el resultado será 1, pues este es el residuo de la operación.

```r
5 %% 2
```

```
## [1] 1
```

## Operadores relacionales
Los operadores lógicos son usados para hacer comparaciones y siempre devuelven como resultado `TRUE` o `FALSE` (verdadero o falso, respectivamente).

Operador| Comparación           | Ejemplo   | Resultado
----    |----                   |----       |----
`<`     | Menor que             | `5 < 3`   | `FALSE`
`<=` 	  | Menor o igual que     | `5 <= 3`  | `FALSE`
`>`     |	Mayor que             | `5 > 3`   | `TRUE`
`>=` 	  | Mayor o igual que     | `5 >= 3`  | `TRUE`
`==` 	  | Exactamente igual que | `5 == 3`  | `FALSE`
`!=` 	  | No es igual que       | `5 != 3`  | `TRUE`

Es posible comparar cualquier tipo de dato sin que resulte en un error.

Sin embargo, al usar los operadores `>`, `>=`, `<` y `<=` con cadenas de texto, estos tienen un comportamiento especial. 

Por ejemplo, `"casa" > "barco"` nos devuelve `TRUE`.

```r
"casa" > "barco"
```

```
## [1] TRUE
```

Este resultado se debe a que se ha hecho una comparación por orden alfabético. En este caso, la palabra "casa" tendría una posición posterior a "barco", pues empieza con "c" y esta letra tiene una posición posterior a la "b" en el alfabeto. Por lo tanto, es verdadero que sea "mayor".

Cuando intentamos comparar factores, siempre obtendremos como resultado `NA` y una advertencia acerca de que estos operadores no son significativos para datos de tipo factor.

```r
as.factor("casa") > "barco"
```

```
## Warning in Ops.factor(as.factor("casa"), "barco"): '>' not meaningful for
## factors
```

```
## [1] NA
```

## Operadores lógicos
Los operadores lógicos son usados para operaciones de **álgebra Booleana**, es decir, para describir relaciones lógicas, expresadas como verdadero (`TRUE`) o falso (`FALSO`).
  
Operador    | Comparación                   | Ejemplo         | Resultado
----        |----                           |----             |----
`x | y`     |	x Ó y es verdadero            | `TRUE | FALSE`  | `TRUE`
`x & y`     |	x Y y son verdaderos          | `TRUE & FALSE`  | `FALSE`
`!x`        |	x no es verdadero (negación)  | `!TRUE`         | `FALSE`
`isTRUE(x)` |	x es verdadero (afirmación)   | `isTRUE(TRUE)`  | `TRUE`

Los operadores `|` y `&` siguen estas reglas:

* `|` devuelve `TRUE` si alguno de los datos es `TRUE`
* `&` solo devuelve `TRUE` si ambos datos es `TRUE`
* `|` solo devuelve `FALSE` si ambos datos son `FALSE`
* `&` devuelve `FALSE` si alguno de los datos es `FALSE`

Estos operadores pueden ser usados con estos con datos de tipo **numérico**, **lógico** y **complejo**. Al igual que con los operadores relacionales, los operadores lógicos siempre devuelven `TRUE` o `FALSE`.

Para realizar operaciones lógicas, todos los valores numéricos y complejos distintos a `0` son coercionados a `TRUE`, mientras que `0` siempre es coercionado a `FALSE`. 

Por ejemplo, `5 | 0` resulta en `TRUE` y `5 & FALSE` resulta en `FALSE`. Podemos comprobar lo anterior con la función `isTRUE()`.

```r
5 | 0
```

```
## [1] TRUE
```

```r
5 & 0
```

```
## [1] FALSE
```

```r
isTRUE(0)
```

```
## [1] FALSE
```

```r
isTRUE(5)
```

```
## [1] FALSE
```

Estos operadores se pueden combinar para expresar relaciones complejas.

Por ejemplo, la negación `FALSE` Y `FALSE` dará como resultado `TRUE`.

```r
!(FALSE | FALSE)
```

```
## [1] TRUE
```

También podemos combinar operadores lógicos y relacionales, dado que esto últimos dan como resultado `TRUE` y `FALSE`.

```
## [1] TRUE
```

## Operadores de asignación
Este es probablemente el operador más importante de todos, pues nos permite asignar datos a variables.

Operador  | Operación   
----      |----         
`<-`      | Asigna un valor a una variable
`=`       | Asigna un valor a una variable

Aunque podemos usar el signo igual para una asignación, a lo largo de este libro utilizaremos `<-`, por ser característico de R y fácil de reconocer visualmente.

Después de realizar la operación de asignación, podemos usar el nombre de la variable para realizar operaciones con ella, como si fuera del tipo de datos que le hemos asignado. Si asignamos un valor a una variable a la que ya habíamos asignado datos, nuestra variable conserva el valor más reciente.

Además, esta operación nos permite "guardar" el resultado de operaciones, de modo que los podemos recuperar sin necesidad de realizar las operaciones otra vez. Basta con llamar el nombre de la variable en la consola

En este ejemplo, asignamos valores a las variables `estatura` y `peso`.

```r
estatura <- 1.73
peso <- 83
```
Llamamos a sus valores asignados

```r
estatura
```

```
## [1] 1.73
```

```r
peso
```

```
## [1] 83
```

Usamos los valores asignados para realizar operaciones.

```r
peso / estatura ^ 2
```

```
## [1] 27.7323
```

Cambiamos el valor de una variable a uno nuevo y realizamos operaciones

```r
peso <- 76

peso
```

```
## [1] 76
```

```r
peso / estatura ^ 2
```

```
## [1] 25.39343
```

```r
estatura <- 1.56
peso <- 48

peso / estatura ^ 2
```

```
## [1] 19.72387
```

Asignamos el resultado de una operación a una variable nueva.

```r
bmi <- peso / estatura ^ 2

bmi
```

```
## [1] 19.72387
```

Como podrás ver, es posible asignar a una variable valores de otra variable o el resultado de operaciones con otras variables.

```r
velocidad_inicial <- 110
velocidad_final <- 185

tiempo_inicial <- 0
tiempo_final <- 15

variacion_velocidad <- velocidad_final - velocidad_inicial
variacion_tiempo <- tiempo_final - tiempo_inicial

variacion_velocidad / variacion_tiempo
```

```
## [1] 5
```

## Orden de operaciones
En R, al igual que en matemáticas, las operaciones tienen un orden de evaluación definido. 

Cuanto tenemos varias operaciones ocurriendo al mismo tiempo, en realidad, algunas de ellas son realizadas antes que otras y el resultado de ellas dependerá de este orden.

El orden de operaciones incluye a las aritméticas, relacionales, lógicas y de asignación.

En la tabla siguiente se presenta el orden en que ocurren las operaciones que hemos revisado en este capítulo.

Orden | Operadores
---   |---
1     | `^`          
2     | `*` `/`
3     | `+` `-`
4     | `<` `>` `<=` `>=` `==` `!=`
5     | `!`
6     | `&`
7     | `|`
8     | `<-`

Si deseamos que una operación ocurra antes que otra, rompiendo este orden de evaluación, usamos paréntesis.





Podemos tener paréntesis anidados.



