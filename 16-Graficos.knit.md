Gráficos
========






`R` dispone de varias funciones que permiten la
realización de gráficos. Estas funciones se dividen en dos grandes
grupos

-   Gráficos de **alto nivel**: Crean un gráfico nuevo.

    -   `plot, hist, boxplot, ... `

-   Gráficos de **bajo nivel**: Permiten añadir elementos (líneas, puntos, ...) a un
    gráfico ya existente

    -   `points, lines, legend, text, ...`

    -   El parámetro `add=TRUE` convierte una función de nivel alto a bajo.


Dentro de las funciones gráficas de alto nivel destaca la función `plot`
que tiene muchas variantes y dependiendo del tipo de datos que se le
pasen como argumento actuará de modo distinto.


El comando plot
---------------

Si escribimos `plot(x, y)` donde `x` e `y` son vectores, entonces `R`
hará directamente el conocido como **gráfico de dispersión** que
representa en un sistema coordenado los pares de valores $(x,y)$.

Por ejemplo, utilizando el siguiente código

























