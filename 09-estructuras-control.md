# Estructuras de control
Como su nombre lo indica, las estructuras de control nos permiten controlar la manera en que se ejecuta nuestro código. 

Las estructuras de control establecen **condicionales** en nuestros código. Por ejemplo, qué condiciones deben cumplirse para realizar una operación o qué debe ocurrir para ejecutar una función.

Esto es de gran utilidad para determinar la lógica y el orden en que ocurren las operaciones, en especial al definir funciones.

Las estructuras de control más usadas en R son las siguientes.

| Estructura de control | Descripción
| ---                   |---
| `if`, `else`          | Si, de otro modo
| `for`                 | Para cada uno en
| `while`               | Mientras
| `break`               | Interrupción
| `next`                | Siguiente

## if, else
`if` (si) es usado cuando deseamos que una operación se ejecute únicamente cuando una condición se cumple. 

`else` (de otro modo) es usado para indicarle a R qué hacer en caso de la condición de un `if` no se cumpla. 

Un `if` es la manera de decirle a R: 

* **SI** esta condición es cierta, **ENTONCES** haz estas operaciones.

El modelo para un `if` es:

```r
if(Condición) {
  operaciones_si_la_condición_es_TRUE
}
```

Si la condición se cumple, es decir, es verdadera (`TRUE`), entonces se realizan las operaciones. En caso contrario, no ocurre nada y el código con las operaciones no es ejecutado.

Por ejemplo, le pedimos a R que nos muestre el texto "Verdadero" si la condición se cumple.

```r
# Se cumple la condición y se muestra "verdadero"
if(4 > 3) {
  "Verdadero"
}
```

```
## [1] "Verdadero"
```

```r
# No se cumple la condición y no pasa nada
if(4 > 5) {
  "Verdadero"
}
```

`else` complementa un `if`, pues indica qué ocurrirá cuando la condición no se cumple, es falsa (`FALSE`), en lugar de no hacer nada.

Un `if` con `else` es la manera de decirle a R:

* **SI** esta condición es es cierta, **ENTONCES** haz estas operaciones, **DE OTRO MODO** haz estas otras operaciones.

El modelo para un **if** con un **else** es:

```r
if(condición) {
  operaciones_si_la_condición_es_TRUE
} else {
  operaciones_si_la_condición_es_FALSE
}
```

Usando los ejemplos anteriores, podemos mostrar "Falso" si no se cumple la condición, en lugar de que no ocurra nada.


```r
# Se cumple la condición y se muestra "Verdadero"
if(4 > 3) {
  "Verdadero"
} else {
  "Falso"
}
```

```
## [1] "Verdadero"
```

```r
# No se cumple la condición y se muestra "Falso"
if(4 > 5) {
  "Verdadero"
} else {
  "Falso"
}
```

```
## [1] "Falso"
```

### Usando if y else
Para ilustrar el uso de `if` `else` definiremos una función que calcule el promedio de calificaciones de un estudiante y, dependiendo de la calificación calculada, nos devuelva un mensaje específico.

Empezamos definiendo una función para calcular promedio. En realidad, sólo es la aplicación de la función `mean()` ya existente en R *base*, pero la ampliaremos después.

```r
promedio <- function(calificaciones) {
  mean(calificaciones)
}

promedio(c(6, 7, 8, 9, 8))
```

```
## [1] 7.6
```

```r
promedio(c(5, 8, 5, 6, 5))
```

```
## [1] 5.8
```

Ahora deseamos que esta función nos muestre si un estudiante ha aprobado o no.

Si asumimos que un estudiante necesita obtener 6 o más de promedio para aprobar, podemos decir que:

* **SI** el promedio de un estudiante es igual o mayor a 6, **ENTONCES** mostrar "Aprobado", **DE OTRO MODO**, mostrar "Reprobado".

Aplicamos esta lógica con un `if`,  `else` en la función `promedio()`.

```r
promedio <- function(calificaciones) {
  media <- mean(calificaciones)
  
  if(media >= 6) {
    print("Aprobado")
  } else {
    print("Reprobado")
  }
}
```

Probemos nuestra función

```r
promedio(c(6, 7, 8, 9, 8))
```

```
## [1] "Aprobado"
```

```r
promedio(c(5, 8, 5, 6, 5))
```

```
## [1] "Reprobado"
```

Está funcionando, aunque los resultados podrían tener una mejor presentación.

Usaremos la función `paste0()` para pegar el promedio de calificaciones, como texto, con el resultado de "Aprobado" o "Reprobado". Esta función acepta como argumentos cadenas de texto y las pega (concatena) entre sí, devolviendo como resultado una nueva cadena.

Primero concatenaremos la palabra "Calificación: " a la media obtenida con la función `promedio()` y después el resultado de esta operación con la palabra "aprobado" o "reprobado", según corresponda.

```r
promedio <- function(calificaciones) {
  media <- mean(calificaciones)
  texto <- paste0("Calificación: ", media, ", ")
  
  if(media >= 6) {
    print(paste0(texto, "aprobado"))
  } else {
    print(paste0(texto, "reprobado"))
  }
}
```

Pongamos a prueba nuestra función.

```r
promedio(c(6, 7, 8, 9, 8))
```

```
## [1] "Calificación: 7.6, aprobado"
```

```r
promedio(c(5, 8, 5, 6, 5))
```

```
## [1] "Calificación: 5.8, reprobado"
```

Por supuesto, como lo vimos en el capítulo sobre [funciones](#funciones), podemos hacer aún más compleja a `promedio()`, pero esto es suficiente para conocer mejor las aplicaciones de `if` `else`.

### ifelse
La función `ifelse( )` nos permite vectorizar `if, else`. En lugar de escribir una línea de código para cada comparación, podemos usar una sola llamada a esta función, que se aplicará a todos los elementos de un vector.

Si intentamos usar `if` `else` con un vector, se nos mostrará una advertencia.

```r
if(1:10 < 3) {
  "Verdadero"
}
```

```
## Warning in if (1:10 < 3) {: la condición tiene longitud > 1 y sólo el primer
## elemento será usado
```

```
## [1] "Verdadero"
```
Este mensaje nos dice que sólo se usará el primer elemento del vector para evaluar su la condición es verdadera y lo demás será ignorado.

En cambio, con `ifelse` se nos devolverá un valor para cada elemento de un vector en el que la condición sea `TRUE`, además nos devolverá otro valor para los elementos en que la condición sea `FALSE`. 

Esta función tiene la siguiente forma.

```r
ifelse(vector, valor_si_TRUE, valor_si_FALSE)
```

Si intentamos el ejemplo anterior con `ifelse()`, se nos devolverá un resultado para cada elemento del vector, no sólo del primero de ellos.

```
##  [1] "Verdadero" "Verdadero" "Falso"     "Falso"     "Falso"     "Falso"    
##  [7] "Falso"     "Falso"     "Falso"     "Falso"
```
De este modo podemos usar `ifelse()` para saber si los números en un vector son pares o nones.

```r
num <- 1:8

ifelse(num %% 2 == 0, "Par", "Non")
```

```
## [1] "Non" "Par" "Non" "Par" "Non" "Par" "Non" "Par"
```

También tenemos la opción de crear condiciones más complejas usando operadores Booleanos.

Por ejemplo, pedimos sólo los números que son exactamente divisibles entre 2 y 3.

```r
num <- 1:20

ifelse(num %% 2 == 0 & num %% 3, "Divisible", "No divisible")
```

```
##  [1] "No divisible" "Divisible"    "No divisible" "Divisible"    "No divisible"
##  [6] "No divisible" "No divisible" "Divisible"    "No divisible" "Divisible"   
## [11] "No divisible" "No divisible" "No divisible" "Divisible"    "No divisible"
## [16] "Divisible"    "No divisible" "No divisible" "No divisible" "Divisible"
```

Desde luego, esto es particularmente útil para recodificar datos.

```r
num <- c(0, 1, 0, 0, 0, 1, 1)

num <- ifelse(num == 0, "Hombre", "Mujer")

num
```

```
## [1] "Hombre" "Mujer"  "Hombre" "Hombre" "Hombre" "Mujer"  "Mujer"
```


## for 
La estructura `for` nos permite ejecutar un bucle (*loop*), realizando una operación para cada elemento de un conjunto de datos.

Su estructura es la siguiente:

```r
for(elemento in objeto) {
  operacion_con_elemento
}
```

Con lo anterior le decimos a R:

* **PARA** cada elemento **EN** un objeto, haz la siguiente operación.

Al escribir un bucle `for` la parte que corresponde al **elemento** la podemos llamar como nosotros deseemos, pero la parte que corresponde al **objeto** debe ser el nombre de un objeto existente.

Los dos bucles siguientes son equivalentes, sólo cambia el nombre que le hemos puesto al **elemento**.

```r
objeto <- 1:10

for(elemento in objeto) {
  operacion_con_elemento
}

for(i in objeto) {
  operacion_con_elemento
}
```

Tradicionalmente se usa la letra **i** para denotar al elemento, pero nosotros usaremos nombres más descriptivos en este capítulo.


### Usando for
Vamos a obtener el cuadrado de cada uno de los elementos en un vector numérico del 1 al 6, que representa las caras de un dado.

```r
dado <- 1:6

for(cara in dado) {
  dado ^ 2 
}
```

Notarás que al ejecutar el código anterior parece que no ha ocurrido nada. En realidad, sí se han realizado las operaciones, pero R no ha devuelto sus resultados.

Las operaciones en un `for` se realizan pero sus resultados nunca son devueltos automáticamente, es necesario pedirlos de manera explícita.

A diferencia de otros lenguajes de programación en los que pedimos los resultados de un bucle con `return()`, en R este procedimiento sólo funciona con funciones.

Una solución para mostrar los resultados de un bucle `for` es usar la función `print()`.

```r
for(cara in dado) {
  print(cara ^ 2)
}
```

```
## [1] 1
## [1] 4
## [1] 9
## [1] 16
## [1] 25
## [1] 36
```

Comprobamos que la operación ha sido realizada a cada elemento de nuestro objeto. Sin embargo, usar `print()` sólo mostrará los resultados de las operaciones en la consola, no los asignará a un objeto.

Si deseamos asignar los resultados de un bucle `for` a un objeto, usamos [índices](##indices). 

Aprovechamos que el primer elemento en un bucle siempre es identificado con el número **1** y que continuará realizando operaciones hasta llegar al total de elementos que hemos especificado.

```r
for(numero in 1:10) {
  print(numero)
}
```

```
## [1] 1
## [1] 2
## [1] 3
## [1] 4
## [1] 5
## [1] 6
## [1] 7
## [1] 8
## [1] 9
## [1] 10
```

En nuestro ejemplo, pasamos por los valores de dado, cara por cara. La primera cara será igual a **1**, la segunda a **2**, y así sucesivamente hasta el **6**. 

Podemos usar estos valores para asignar cada valor resultante de nuestras operaciones a una posición específica en un vector, incluso si este está vacío.

Creamos un vector vacío, asignándole como `NULL` como valor.

```r
mi_vector <- NULL
```

Ejecutamos nuestro bucle.

```r
for(cara in dado) {
  mi_vector[cara] <- cara ^ 2
}
```

Aunque no fueron mostrados en la consola, los resultados han sido asignados al objeto `mi_vector`.

```r
mi_vector
```

```
## [1]  1  4  9 16 25 36
```

### for y vectorización
Notarás que el resultado que obtuvimos usando **for** es el mismo que si vectorizamos la operación.

```r
dado ^ 2
```

```
## [1]  1  4  9 16 25 36
```

Dado que en R contamos con vectorización de operaciones, que podemos usar las funciones de [la familia apply](#la-familia-apply) (discutido en siguiente capítulo) en objetos diferentes a vectores y que la manera de recuperar los resultados de un `for` es un tanto laboriosa, este tipo de bucle no es muy popular en R. 

En R generalmente hay opciones mejores, en cuanto a simplicidad y velocidad de cómputo, que un bucle `for`.

Sin embargo, es conveniente que conozcas esta estructura de control, pues hay ocasiones en la que es la mejor herramienta para algunos problemas específicos.

## while
Este es un tipo de bucle que ocurre **mientras** una condición es verdadera (`TRUE`). La operación se realiza hasta que se se llega a cumplir un criterio previamente establecido.

El modelo de **while** es:

```r
while(condicion) {
  operaciones
}
```

Con esto le decimos a R:

* **MIENTRAS** esta condición sea **VERDADERA**, haz estas operaciones.

La condición generalmente es expresada como el resultado de una o varias operaciones de comparación, pero también puede ser el resultado de una función.

### Usando while
Probemos sumar `+1` a un valor, mientras que este sea menor que 5. Al igual que con `for`, necesitamos la función `print()` para mostrar los resultados en la consola.

```r
umbral <- 5
valor <- 0

while(valor < umbral) {
  print("Todavía no.")
  valor <- valor + 1
}
```

```
## [1] "Todavía no."
## [1] "Todavía no."
## [1] "Todavía no."
## [1] "Todavía no."
## [1] "Todavía no."
```

¡Ten cuidado con crear bucles infinitos! Si ejecutas un `while` con una condición que nunca será `FALSE`, este nunca se detendrá.

Si corres lo siguiente, presiona la tecla **ESC** para detener la ejecución, de otro modo, correrá por siempre y puede llegar a congelar tu equipo.

```r
while(1 < 2) {
  print("Presiona ESC para detener")
}
```

El siguiente es un error común. Estamos sumando 1 a `i` con cada iteración del bucle, pero como no estamos asignando este nuevo valor a `i`, su valor se  mantiene igual, entonces la condición nunca se cumplirá y el bucle será infinito.

De nuevo, si corres lo siguiente, presiona la tecla **ESC** para detener la ejecución.

```r
i <- 0
while(i < 10) {
  i + 1
}
```

Un uso común de **while** es que realice operaciones que queremos detener cuando se cumple una condición, pero desconocemos cuándo ocurrirá esto.

Supongamos que, por alguna razón queremos sumar calificaciones, del 1 al 10 al azar, hasta llegar a un número que mayor o igual a 50. Además nos interesa saber cuántas calificaciones sumaron y cuál fue el resultado al momento de cumplir la condición.

Para obtener números al azar del 1 al 10, usamos la función `sample()`. Esta función va a tomar una muestra al azar de tamaño igual a 1 (argumento `size`) de un vector del 1 al 10 (argumento `x`) cada vez que se ejecute.

Por lo tanto, cada vez que corras el ejemplo siguiente obtendrás un resultado distinto, pero siempre llegarás a un valor mayor a 50.

Creamos dos objetos, `conteo` y `valor`. Les asignamos el valor 0.

```r
conteo <-  0
valor <- 0
```

Nuestro `while` hará dos cosas. 

Primero, tomará un número al azar del 1 al 10, y lo sumará a `valor`. Segundo, le sumará 1 a `conteo` cada que esto ocurra, de esta manera sabremos cuántas iteraciones ocurrieron para llegar a un valor que no sea menor a 50.

```r
while(valor < 50) {
  valor <- valor + sample(x = 1:10, size = 1)
  conteo <- conteo + 1
}
```

Aunque no son mostrados en la consola los resultados son asignados a los objetos `valor`y `conteo`

```r
valor
```

```
## [1] 55
```

```r
conteo
```

```
## [1] 8
```

Por último, si intentamos ejecutar un `while` para el que la condición nunca es igual a `TRUE`, este no realizará ninguna operación.

```r
conteo <- 0

while("dado" == "ficha") {
  conteo <- conteo + 1
}

conteo
```

```
## [1] 0
```

## break y next
`break` y `next`  son  **palabras reservadas** en R, no podemos asignarles nuevos valores y realizan una operación específica cuando aparecen en nuestro código.

`break` nos permite **interrumpir** un bucle, mientras que `next` nos deja avanzar a la **siguiente** iteración del bucle, "saltándose" la actual. Ambas funcionan para `for` y `while`.

### Usando break
Para interrumpir un bucle con `break`, necesitamos que se cumpla una condición. Cuando esto ocurre, el bucle se detiene, aunque existan elementos a los cuales aún podría aplicarse.

Interrumpimos un `for` cuando `i` es igual a 3, aunque aún queden 7 elementos en el objeto.

```r
for(i in 1:10) {
  if(i == 3) {
    break
  }
  print(i)
}
```

```
## [1] 1
## [1] 2
```

Interrumpimos un `while` antes de se cumpla la condición de que `numero` sea mayor a 5, en cuanto este tiene el valor de 15.

```r
numero <- 20

while(numero > 5) {
  if(numero == 15) {
    break
  }
  numero <- numero - 1
}

numero
```

```
## [1] 15
```

Como habrás notado, la  aplicación de `break` es muy similar a `while`, realizar una operación hasta que se cumple una condición, y ambos pueden usarse en conjunto.

### Usando next
Por su parte, usamos next para "saltarnos" una iteración en un bucle. Cuando la condición se cumple, esa iteración es omitida. 

```r
for(i in 1:4) {
  if(i == 3) {
    next
  }
  print(i)
}
```

```
## [1] 1
## [1] 2
## [1] 4
```

Estas dos estructuras de control nos dan un control fino sobre nuestro código. aunque los dos ejemplos de arriba son con **for**, también funcionan con **while** y **repeat**. 

En realidad, **break** es indispensable para **repeat**.

## repeat
Este es un bucle que se llevará a cabo el número de veces que especifiquemos, usando un `break` para detenerse. `repeat` asegura que las operaciones que contiene sean iteradas al menos en una ocasión.

La estructura de **repeat** es el siguiente:

```r
repeat {
  operaciones
  
  un_break_para_detener
}
```

Si no incluimos un `break`, el bucle se repetirá indefinidamente y sólo lo podremos detener pulsando la tecla ESC, así que hay que tener cuidado al usar esta estructura de control.

Por ejemplo, el siguiente `repeat` sumará `+1` a `valor` hasta que este sea igual a cinco, entonces se detendrá.

```r
valor <-  0
mi_vector <- NULL

repeat{
  valor <- valor + 1
  if(valor == 5) {
    break
  }
}

# Resultado
valor
```

```
## [1] 5
```

Este tipo de bucle es quizás el menos utilizado de todos, pues en R existen alternativas para obtener los mismos resultados de manera más sencilla y sin el riesgo de crear un bucle infinito. Sin embargo, puede ser la mejor alternativa para problemas específicos.
