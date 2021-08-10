Gr�ficos
========






`R` dispone de varias funciones que permiten la
realizaci�n de gr�ficos. Estas funciones se dividen en dos grandes
grupos

-   Gr�ficos de **alto nivel**: Crean un gr�fico nuevo.

    -   `plot, hist, boxplot, ... `

-   Gr�ficos de **bajo nivel**: Permiten a�adir elementos (l�neas, puntos, ...) a un
    gr�fico ya existente

    -   `points, lines, legend, text, ...`

    -   El par�metro `add=TRUE` convierte una funci�n de nivel alto a bajo.


Dentro de las funciones gr�ficas de alto nivel destaca la funci�n `plot`
que tiene muchas variantes y dependiendo del tipo de datos que se le
pasen como argumento actuar� de modo distinto.


El comando plot
---------------

Si escribimos `plot(x, y)` donde `x` e `y` son vectores, entonces `R`
har� directamente el conocido como **gr�fico de dispersi�n** que
representa en un sistema coordenado los pares de valores $(x,y)$.

Por ejemplo, utilizando el siguiente c�digo

























