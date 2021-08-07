Manipulación de datos con R
===========================




La mayoría de los estudios estadísticos
requieren disponer de un conjunto de datos. 

Lectura, importación y exportación de datos
-------------------------------------------

Además de la introducción directa, R es capaz de
importar datos externos en múltiples formatos:

-   bases de datos disponibles en librerías de R

-   archivos de texto en formato ASCII

-   archivos en otros formatos: Excel, SPSS, ...

-   bases de datos relacionales: MySQL, Oracle, ...

-   formatos web: HTML, XML, JSON, ...

-   ....

### Formato de datos de R

El formato de archivo en el que habitualmente se almacena objetos (datos)
R es binario y está comprimido (en formato `"gzip"` por defecto).
Para cargar un fichero de datos se emplea normalmente [`load()`](https://www.rdocumentation.org/packages/base/versions/3.6.1/topics/load):

```r
res <- load("datos/empleados.RData")
res
```

```
## [1] "empleados"
```

```r
ls()
```

```
##  [1] "citefig"   "citefig2"  "citepkg"   "citepkgs"  "empleados" "fig.path" 
##  [7] "GCtorture" "inline"    "inline2"   "is_html"   "is_latex"  "latexfig" 
## [13] "res"
```
y para guardar [`save()`](https://www.rdocumentation.org/packages/base/versions/3.6.1/topics/save):

```r
# Guardar
save(empleados, file = "datos/empleados_new.RData")
```

El objeto empleado normalmente en R para almacenar datos en memoria 
es el [`data.frame`](https://www.rdocumentation.org/packages/base/versions/3.6.1/topics/data.frame).


### Acceso a datos en paquetes

R dispone de múltiples conjuntos de datos en distintos paquetes, especialmente en el paquete `datasets` 
que se carga por defecto al abrir R. 
Con el comando `data()` podemos obtener un listado de las bases de datos disponibles.

Para cargar una base de datos concreta se utiliza el comando
`data(nombre)` (aunque en algunos casos se cargan automáticamente al emplearlos). 
Por ejemplo, `data(cars)` carga la base de datos llamada `cars` en el entorno de trabajo (`".GlobalEnv"`)
y `?cars` muestra la ayuda correspondiente con la descripición de la base de datos.


### Lectura de archivos de texto {#cap4-texto}

En R para leer archivos de texto se suele utilizar la función `read.table()`.
Supóngase, por ejemplo, que en el directorio actual está el fichero
*empleados.txt*. La lectura de este fichero vendría dada por el código:


```r
# Session > Set Working Directory > To Source...?
datos <- read.table(file = "datos/empleados.txt", header = TRUE)
# head(datos)
str(datos)
```

```
## 'data.frame':	474 obs. of  10 variables:
##  $ id      : int  1 2 3 4 5 6 7 8 9 10 ...
##  $ sexo    : chr  "Hombre" "Hombre" "Mujer" "Mujer" ...
##  $ fechnac : chr  "2/3/1952" "5/23/1958" "7/26/1929" "4/15/1947" ...
##  $ educ    : int  15 16 12 8 15 15 15 12 15 12 ...
##  $ catlab  : chr  "Directivo" "Administrativo" "Administrativo" "Administrativo" ...
##  $ salario : num  57000 40200 21450 21900 45000 ...
##  $ salini  : int  27000 18750 12000 13200 21000 13500 18750 9750 12750 13500 ...
##  $ tiempemp: int  98 98 98 98 98 98 98 98 98 98 ...
##  $ expprev : int  144 36 381 190 138 67 114 0 115 244 ...
##  $ minoria : chr  "No" "No" "No" "No" ...
```
Si el fichero estuviese en el directorio *c:\\datos* bastaría con especificar
`file = "c:/datos/empleados.txt"`.
Nótese también que para la lectura del fichero anterior se ha
establecido el argumento `header=TRUE` para indicar que la primera línea del
fichero contiene los nombres de las variables.

Los argumentos utilizados habitualmente para esta función son:

-   `header`: indica si el fichero tiene cabecera (`header=TRUE`) o no
    (`header=FALSE`). Por defecto toma el valor `header=FALSE`.

-   `sep`: carácter separador de columnas que por defecto es un espacio
    en blanco (`sep=""`). Otras opciones serían: `sep=","` si el separador es
    un ";", `sep="*"` si el separador es un "\*", etc.

-   `dec`: carácter utilizado en el fichero para los números decimales.
    Por defecto se establece `dec = "."`. Si los decimales vienen dados
    por "," se utiliza `dec = ","`

Resumiendo, los (principales) argumentos por defecto de la función
`read.table` son los que se muestran en la siguiente línea:


```r
read.table(file, header = FALSE, sep = "", dec = ".")  
```

Para más detalles sobre esta función véase
`help(read.table)`.

Estan disponibles otras funciones con valores por defecto de los parámetros 
adecuados para otras situaciones. Por ejemplo, para ficheros separados por tabuladores 
se puede utilizar `read.delim()` o `read.delim2()`:

```r
read.delim(file, header = TRUE, sep = "\t", dec = ".")
read.delim2(file, header = TRUE, sep = "\t", dec = ",")
```

### Alternativa `tidyverse`

Para leer archivos de texto en distintos formatos también se puede emplear el paquete [`readr`](https://readr.tidyverse.org) 
(colección [`tidyverse`](https://www.tidyverse.org/)), para lo que se recomienda
consultar el [Capítulo 11](https://r4ds.had.co.nz/data-import.html) del libro [R for Data Science](http://r4ds.had.co.nz).


### Importación desde SPSS

El programa R permite
lectura de ficheros de datos en formato SPSS (extensión *.sav*) sin
necesidad de tener instalado dicho programa en el ordenador. Para ello
se necesita:

-   cargar la librería `foreign`

-   utilizar la función `read.spss`

Por ejemplo:


```r
library(foreign)
datos <- read.spss(file = "datos/Employee data.sav", to.data.frame = TRUE)
# head(datos)
str(datos)
```

```
## 'data.frame':	474 obs. of  10 variables:
##  $ id      : num  1 2 3 4 5 6 7 8 9 10 ...
##  $ sexo    : Factor w/ 2 levels "Hombre","Mujer": 1 1 2 2 1 1 1 2 2 2 ...
##  $ fechnac : num  1.17e+10 1.19e+10 1.09e+10 1.15e+10 1.17e+10 ...
##  $ educ    : Factor w/ 10 levels "8","12","14",..: 4 5 2 1 4 4 4 2 4 2 ...
##  $ catlab  : Factor w/ 3 levels "Administrativo",..: 3 1 1 1 1 1 1 1 1 1 ...
##  $ salario : Factor w/ 221 levels "15750","15900",..: 179 137 28 31 150 101 121 31 71 45 ...
##  $ salini  : Factor w/ 90 levels "9000","9750",..: 60 42 13 21 48 23 42 2 18 23 ...
##  $ tiempemp: Factor w/ 36 levels "63","64","65",..: 36 36 36 36 36 36 36 36 36 36 ...
##  $ expprev : Factor w/ 208 levels "Ausente","10",..: 38 131 139 64 34 181 13 1 14 91 ...
##  $ minoria : Factor w/ 2 levels "No","Sí": 1 1 1 1 1 1 1 1 1 1 ...
##  - attr(*, "variable.labels")= Named chr [1:10] "Código de empleado" "Sexo" "Fecha de nacimiento" "Nivel educativo" ...
##   ..- attr(*, "names")= chr [1:10] "id" "sexo" "fechnac" "educ" ...
##  - attr(*, "codepage")= int 1252
```

**Nota**: Si hay fechas, puede ser recomendable emplear la función `spss.get()` del paquete `Hmisc`.


### Importación desde Excel

Se pueden leer fichero de
Excel (con extensión *.xlsx*) utilizando por ejemplo los paquetes [`openxlsx`](https://cran.r-project.org/web/packages/openxlsx/index.html), [`readxl`](https://readxl.tidyverse.org) (colección [`tidyverse`](https://www.tidyverse.org/)), `XLConnect` o 
[`RODBC`](https://cran.r-project.org/web/packages/RODBC/index.html) (este paquete se empleará más adelante para acceder a bases de datos),
entre otros.

Sin embargo, un procedimiento sencillo consiste en  exportar los datos desde Excel a un archivo
de texto separado por tabuladores (extensión *.csv*).
Por ejemplo, supongamos que queremos leer el fichero *coches.xls*:

-   Desde Excel se selecciona el menú
    `Archivo -> Guardar como -> Guardar como` y en `Tipo` se escoge la opción de
    archivo CSV. De esta forma se guardarán los datos en el archivo
    *coches.csv*.

-   El fichero *coches.csv* es un fichero de texto plano (se puede
    editar con Notepad), con cabecera, las columnas separadas por ";", y
    siendo "," el carácter decimal.

-   Por lo tanto, la lectura de este fichero se puede hacer con:

    
    ```r
    datos <- read.table("coches.csv", header = TRUE, sep = ";", dec = ",")
    ```

Otra posibilidad es utilizar la función `read.csv2`, que es
una adaptación de la función general `read.table` con las siguientes
opciones:

```r
read.csv2(file, header = TRUE, sep = ";", dec = ",")
```

Por lo tanto, la lectura del fichero *coches.csv* se puede hacer de modo
más directo con:

```r
datos <- read.csv2("coches.csv")
```

### Exportación de datos

Puede ser de interés la
exportación de datos para que puedan leídos con otros programas. Para
ello, se puede emplear la función `write.table()`. Esta función es
similar, pero funcionando en sentido inverso, a `read.table()` 
(Sección \@ref(cap4-texto)).

Veamos un ejemplo:


```r
tipo <- c("A", "B", "C")
longitud <- c(120.34, 99.45, 115.67)
datos <- data.frame(tipo, longitud)
datos
```

```
##   tipo longitud
## 1    A   120.34
## 2    B    99.45
## 3    C   115.67
```
Para guardar el data.frame `datos` en un fichero de texto se
puede utilizar:

```r
write.table(datos, file = "datos.txt")
```
Otra posibilidad es utilizar la función:

```r
write.csv2(datos, file = "datos.csv")
```
que dará lugar al fichero *datos.csv* importable directamente desde Excel.


Manipulación de datos
---------------------

Una vez cargada una (o varias) bases
de datos hay una series de operaciones que serán de interés para el
tratamiento de datos: 

-   Operaciones con variables: 
    - crear
    - recodificar (e.g. categorizar)
    - ...

-   Operaciones con casos:
    - ordenar
    - filtrar
    - ...


A continuación se tratan algunas operaciones *básicas*.

### Operaciones con variables

#### Creación (y eliminación) de variables

Consideremos de nuevo la
base de datos `cars` incluida en el paquete `datasets`:

```r
data(cars)
# str(cars)
head(cars)
```

```
##      Price Mileage Cylinder Doors Cruise Sound Leather Buick Cadillac Chevy
## 1 22661.05   20105        6     4      1     0       0     1        0     0
## 2 21725.01   13457        6     2      1     1       0     0        0     1
## 3 29142.71   31655        4     2      1     1       1     0        0     0
## 4 30731.94   22479        4     2      1     0       0     0        0     0
## 5 33358.77   17590        4     2      1     1       1     0        0     0
## 6 30315.17   23635        4     2      1     0       0     0        0     0
##   Pontiac Saab Saturn convertible coupe hatchback sedan wagon
## 1       0    0      0           0     0         0     1     0
## 2       0    0      0           0     1         0     0     0
## 3       0    1      0           1     0         0     0     0
## 4       0    1      0           1     0         0     0     0
## 5       0    1      0           1     0         0     0     0
## 6       0    1      0           1     0         0     0     0
```

Utilizando el comando `help(cars)`
se obtiene que `cars` es un data.frame con 50 observaciones y dos
variables:

-   `speed`: Velocidad (millas por hora)

-   `dist`: tiempo hasta detenerse (pies)

Recordemos que, para acceder a la variable `speed` se puede
hacer directamente con su nombre o bien utilizando notación
"matricial".

```r
cars$speed
```

```
## NULL
```

```r
cars[, 1]  # Equivalente
```

```
##   [1] 22661.05 21725.01 29142.71 30731.94 33358.77 30315.17 33381.82 30251.02
##   [9] 30166.85 27060.14 26841.08 25790.51 25148.38 24173.53 24852.50 27825.95
##  [17] 26698.08 28185.78 27241.44 30800.66 28416.46 26653.24 27610.86 27788.81
##  [25] 29986.79 29197.79 29908.18 29481.53 26792.30 29321.08 37383.50 34393.00
##  [33] 38324.81 35304.49 28777.96 33248.34 35580.33 30274.71 30122.43 32197.34
##  [41] 27109.41 30353.59 27256.49 28291.76 24912.08 33183.33 28502.96 35033.22
##  [49] 29844.20 30075.99 28054.98 28432.82 32746.13 31002.73 28829.03 31849.31
##  [57] 31554.41 28678.08 29961.25 30575.25 29914.38 17325.27 16391.93 15505.29
##  [65] 15297.84 16078.67 16551.22 14546.88 37288.94 46732.61 47065.21 41371.38
##  [73] 39547.59 42773.03 36970.90 11203.15 13041.87 12207.87 11726.00 13007.98
##  [81] 11149.62 10788.97 12570.14 14023.94 12706.91 11961.62 12810.91 12845.17
##  [89] 12897.93 39713.67 39875.85 31186.74 39365.88 34297.31 38990.61 35261.44
##  [97] 27370.96 31024.87 28502.31 32219.59 29595.79 29664.70 27548.63 12464.07
## [105] 13160.12 12832.46 12258.86 12465.51 12828.03 11903.10 17750.88 18145.13
## [113] 17772.97 18348.90 17394.02 20099.26 20698.08 19924.16 17808.20 19774.25
## [121] 20538.09 19344.17 18543.43 20512.09 23785.92 22358.88 21895.76 22926.09
## [129] 21058.14 19556.90 21183.12 21341.26 20986.02 21683.03 21799.17 23447.69
## [137] 23016.01 23547.24 38600.24 36154.30 36077.80 40335.74 35338.65 39307.01
## [145] 39801.55 32537.19 37510.25 32954.14 36245.16 40856.39 41378.05 37215.17
## [153] 17202.83 17675.84 15253.87 14703.14 15059.13 16792.68 17141.94 19204.81
## [161] 18529.34 20627.66 18158.08 19540.24 21875.10 21903.32 15788.10 17162.87
## [169] 16569.14 16391.17 15457.17 16283.96 18254.92 15802.65 14396.27 14869.28
## [177] 15568.97 16353.10 15730.05 15977.91 19646.72 20173.91 22100.39 18701.22
## [185] 19448.23 21281.88 21230.98 23578.16 22231.56 22189.12 22525.27 23449.31
## [193] 21403.76 21200.69 68566.19 70755.47 52001.99 60567.55 63913.12 69133.73
## [201] 66374.31 17322.08 16507.07 16425.17 15118.89 17978.36 13585.64 15731.13
## [209] 21908.37 20952.22 19425.85 19191.99 21956.34 19981.13 21646.12 32075.98
## [217] 27666.23 34819.30 33540.54 31969.07 35622.14 32737.08 23249.84 29246.24
## [225] 24801.62 22244.88 26775.03 25996.81 25299.97 31156.60 29114.54 30443.88
## [233] 24903.48 31084.94 33005.78 31153.01 23329.21 23274.48 27280.98 25959.12
## [241] 12630.78 12425.39 12549.89 12274.96 13446.21 12319.70 12234.89 12684.99
## [249] 12553.07 14185.02 13019.07 15747.80 13699.04 13530.07 19177.41 16357.99
## [257] 15797.20 17515.40 18040.14 16345.94 18800.09 10386.04 11017.17 12163.82
## [265] 11096.86 10777.05 12146.19 11472.02 16825.19 14914.20 17801.23 16543.98
## [273] 16744.03 16143.96 17891.63 12409.95 12314.59 12649.11 10491.08 11318.01
## [281] 11700.11 11215.02 14894.98 13167.70 13230.92 12573.90 11328.96 12383.40
## [289] 14678.11 13545.03 14304.74 14847.04 14593.85 13744.85 13688.95 12741.19
## [297] 19075.68 17839.80 21757.05 17789.35 18527.21 17294.18 18912.98 16472.90
## [305] 16300.47 15138.40 17158.92 15623.20 19164.61 16993.78 12230.10 12507.49
## [313] 13141.05 15053.93 13594.09 13464.80 14642.32 14255.75 13308.83 14145.88
## [321] 12944.94 12846.06 14266.91 13688.00 20017.97 20839.15 17586.93 20676.17
## [329] 22384.12 21745.03 11179.95 11031.13 10921.95 11343.05 11013.87 11070.06
## [337] 11391.21 18273.01 19471.97 17663.22 18009.85 16988.30 17553.75 18311.76
## [345] 11918.46  9919.05 10805.13 11302.90 10770.11 10872.01 12408.81 14401.91
## [353] 15163.17 14508.75 16116.84 14897.04 15084.82 14418.17 16403.25 17316.10
## [361] 17119.46 16860.87 16536.74 19446.88 16295.21 16860.09 18324.83 18974.92
## [369] 16027.29 16267.09 19581.23 18169.38 14881.96 13719.24 14077.97 14411.86
## [377] 13991.04 15033.15 14771.00 13518.24 13825.15 16256.24 13869.15 16916.87
## [385] 13811.16 11539.05 18566.07 18856.02 19682.04 20318.89 17844.73 18678.41
## [393] 17768.06 13961.11 15077.18 13034.07 15604.15 15979.01 14841.92 16379.85
## [401] 15709.05 15048.04 15295.02 16339.17 17542.04 16218.85 17314.10 21831.82
## [409] 23420.71 25098.63 23077.57 22435.20 25589.98 46747.67 51154.05 42677.60
## [417] 43892.47 44300.64 40619.07 44084.91 30792.15 29275.21 30392.75 30957.08
## [425] 30781.52 33417.97 28040.13 31181.72 33220.03 32501.25 32509.48 31059.18
## [433] 31132.21 35715.77 36633.63 38297.46 42741.52 40966.61 32038.34 37192.90
## [441] 34974.38 44205.88 42377.96 48310.33 41516.43 41671.58 41053.48 38208.50
## [449] 13072.84 11539.05 11699.03 11574.17 12243.06 11671.86 14061.12 15553.21
## [457] 11581.91 13161.94 14077.97 15047.00 12981.95 14220.01 13159.82 12678.85
## [465] 13994.91 14194.82 14696.03 14582.77 12495.97 20021.20 15554.28 16644.09
## [473] 18835.19 17154.58 15951.81 16508.59 16646.77 17218.69 17089.92 17463.05
## [481] 15680.86 16752.51 15664.62 18620.87 20452.67 20677.59 21383.07 19294.79
## [489] 18042.22 16216.98 15724.25 19822.12 15756.15 15979.01 16256.24 16041.69
## [497] 19567.26 15110.19 12036.22 12099.01 12284.29 14203.00 14116.92 12791.75
## [505] 14739.07 12965.22 12412.52 10563.07 12333.60 12119.09 11521.53 12105.98
## [513] 12162.14 13122.91 13494.29 13174.07 13216.91 12594.18 11679.92 20830.99
## [521] 21607.77 22004.93 19116.13 23197.44 20109.90 23102.02 26781.81 21536.74
## [529] 26190.27 26060.34 23159.54 26302.07 23348.02 16706.67 16927.78 14909.05
## [537] 16379.10 15622.12 17418.07 15821.95 23573.82 21020.84 23527.73 20047.95
## [545] 22470.36 20619.11 21525.34 27714.05 25948.96 22064.29 23151.55 20294.58
## [553] 22894.44 24809.04 10354.04 10287.98 10897.08  9789.04 10106.02  9506.05
## [561]  9220.83  8769.00  8870.95 10315.02  9654.06  9041.91 10971.10  8638.93
## [569] 29612.15 33586.91 37088.56 24405.07 27703.20 25618.28 23733.40 28204.60
## [577] 27284.75 30959.93 28328.27 26012.37 33984.43 38167.17 36338.75 25267.37
## [585] 32053.10 26789.83 30271.92 26955.04 32649.76 16106.83 15174.35 16033.93
## [593] 38824.87 44749.69 39691.73 13135.91 12045.92 39092.19 34739.21 35575.42
## [601] 13106.90 12487.05 11539.85 12469.53 27425.84 25527.01 12830.10 12209.56
## [609] 32422.76 12878.05 19027.86 17944.86 17645.75 21335.85 17968.84 19105.13
## [617] 21460.01 21273.06 20406.10 22230.03 20902.10 22625.07 35866.58 38445.90
## [625] 34685.66 42820.33 36332.89 41419.04 15595.88 17360.81 15594.81 17095.04
## [633] 14963.05 16997.69 22104.97 22736.83 22311.05 15086.90 15589.78 17803.28
## [641] 21300.02 19423.17 19956.76 25452.47 21765.07 21982.65 55639.09 65281.48
## [649] 57154.44 18490.98 16175.96 18173.98 21562.05 19641.74 21575.46 34355.00
## [657] 33287.41 31970.54 24896.60 24063.01 26337.83 25845.21 30661.26 30322.15
## [665] 15635.80 17685.20 14584.45 15503.51 13681.70 18910.80 12327.64 14619.08
## [673] 13310.06  9928.19 11137.05 11045.11 18950.91 16723.99 18957.89 11555.27
## [681] 18083.40 11080.52 13471.01 14198.09 19409.75 19528.10 15000.99  9954.05
## [689] 18800.96 15128.99 14997.88 15233.16 17458.22 10144.95 14397.93 12733.86
## [697] 13678.86 14275.13 14222.31 13762.90 20537.14 21233.91 18876.87 11115.01
## [705] 11247.86 11394.89 17115.12 18004.87 16803.12 17312.91 11169.92 16428.58
## [713] 14191.88 10921.95 14175.88 11615.02 16341.80 16713.98 17173.94 17986.22
## [721] 16456.97 13600.03 15395.01 15277.07 13032.87 14702.80 15639.04 15327.10
## [729] 20127.04 15846.01 13162.85 18063.00 19751.04 23493.08 21878.12 21698.01
## [737] 16336.91 14862.09 15230.00 49248.16 35129.34 44130.62 31431.13 48365.98
## [745] 43374.05 36210.12 39072.39 35651.68 30646.44 38795.38 35165.76 35895.50
## [753] 45061.95 28817.08 14072.14 13436.00 17162.48 10546.78 13540.04 12379.13
## [761] 14429.79 13830.25 12257.16 18727.51 15623.92 16507.07 11464.63 16805.06
## [769] 15832.52 16853.11 16516.96 15792.83 18548.98 17023.94 15967.25 12293.06
## [777] 11504.82 11873.53 14568.00 13998.13 13258.37 11413.53 15194.98 19689.74
## [785] 22460.53 19338.38 26831.19 25508.21 23406.69 17214.33 14853.20 14398.92
## [793] 20221.81 20382.15 22113.63 25097.47 22120.76 23345.33 11167.86 10813.34
## [801]  9720.98  9482.22  9563.79  9665.85
```
Supongamos ahora que queremos transformar la variable original `speed`
(millas por hora) en una nueva variable `velocidad` (kilómetros por
hora) y añadir esta nueva variable al data.frame `cars`.
La transformación que permite pasar millas a kilómetros es
`kilómetros=millas/0.62137` que en R se hace directamente con:


```r
cars$speed/0.62137
```

 Finalmente, incluimos la nueva variable que llamaremos
`velocidad` en `cars`:


```r
cars[, c("velocidad")] <- cars[, 1]/0.62137
head(cars)
```

```
##      Price Mileage Cylinder Doors Cruise Sound Leather Buick Cadillac Chevy
## 1 22661.05   20105        6     4      1     0       0     1        0     0
## 2 21725.01   13457        6     2      1     1       0     0        0     1
## 3 29142.71   31655        4     2      1     1       1     0        0     0
## 4 30731.94   22479        4     2      1     0       0     0        0     0
## 5 33358.77   17590        4     2      1     1       1     0        0     0
## 6 30315.17   23635        4     2      1     0       0     0        0     0
##   Pontiac Saab Saturn convertible coupe hatchback sedan wagon velocidad
## 1       0    0      0           0     0         0     1     0  36469.49
## 2       0    0      0           0     1         0     0     0  34963.08
## 3       0    1      0           1     0         0     0     0  46900.74
## 4       0    1      0           1     0         0     0     0  49458.36
## 5       0    1      0           1     0         0     0     0  53685.84
## 6       0    1      0           1     0         0     0     0  48787.63
```

También transformaremos la variable `dist` (en pies) en una nueva
variable `distancia` (en metros). Ahora la transformación deseada es
`metros=pies/3.2808`:


```r
cars[, c("distancia")] <- cars[, 2]/3.2808
head(cars)
```

```
##      Price Mileage Cylinder Doors Cruise Sound Leather Buick Cadillac Chevy
## 1 22661.05   20105        6     4      1     0       0     1        0     0
## 2 21725.01   13457        6     2      1     1       0     0        0     1
## 3 29142.71   31655        4     2      1     1       1     0        0     0
## 4 30731.94   22479        4     2      1     0       0     0        0     0
## 5 33358.77   17590        4     2      1     1       1     0        0     0
## 6 30315.17   23635        4     2      1     0       0     0        0     0
##   Pontiac Saab Saturn convertible coupe hatchback sedan wagon velocidad
## 1       0    0      0           0     0         0     1     0  36469.49
## 2       0    0      0           0     1         0     0     0  34963.08
## 3       0    1      0           1     0         0     0     0  46900.74
## 4       0    1      0           1     0         0     0     0  49458.36
## 5       0    1      0           1     0         0     0     0  53685.84
## 6       0    1      0           1     0         0     0     0  48787.63
##   distancia
## 1  6128.079
## 2  4101.743
## 3  9648.561
## 4  6851.683
## 5  5361.497
## 6  7204.036
```

 Ahora, eliminaremos las variables originales `speed` y
`dist`, y guardaremos el data.frame resultante con el nombre `coches`.
En primer lugar, veamos varias formas de acceder a las variables de
interés:

```r
cars[, c(3, 4)]
cars[, c("velocidad", "distancia")]
cars[, -c(1, 2)]
```

Utilizando alguna de las opciones anteriores se obtiene el `data.frame`
deseado:

```r
coches <- cars[, c("velocidad", "distancia")]
# head(coches)
str(coches)
```

```
## 'data.frame':	804 obs. of  2 variables:
##  $ velocidad: num  36469 34963 46901 49458 53686 ...
##  $ distancia: num  6128 4102 9649 6852 5361 ...
```

Finalmente los datos anteriores podrían ser guardados en un fichero
exportable a Excel con el siguiente comando:

```r
write.csv2(coches, file = "coches.csv")
```

### Operaciones con casos

#### Ordenación

Continuemos con el data.frame `cars`. 
Se puede comprobar que los datos disponibles están ordenados por
los valores de `speed`. A continuación haremos la ordenación utilizando
los valores de `dist`. Para ello utilizaremos el conocido como vector de
índices de ordenación.
Este vector establece el orden en que tienen que ser elegidos los
elementos para obtener la ordenación deseada. 
Veamos un ejemplo sencillo:

```r
x <- c(2.5, 4.3, 1.2, 3.1, 5.0) # valores originales
ii <- order(x)
ii    # vector de ordenación
```

```
## [1] 3 1 4 2 5
```

```r
x[ii] # valores ordenados
```

```
## [1] 1.2 2.5 3.1 4.3 5.0
```
En el caso de vectores, el procedimiento anterior se podría
hacer directamente con: 

```r
sort(x)
```

Sin embargo, para ordenar data.frames será necesario la utilización del
vector de índices de ordenación. A continuación, los datos de `cars`
ordenados por `dist`:

```r
ii <- order(cars$dist) # Vector de índices de ordenación
cars2 <- cars[ii, ]    # Datos ordenados por dist
head(cars2)
```

```
##        Price Mileage Cylinder Doors Cruise Sound Leather Buick Cadillac Chevy
## 800 10813.34     266        4     4      1     0       1     0        0     1
## 196 70755.47     583        8     2      1     1       1     0        1     0
## 549 25948.96     636        6     4      1     0       0     0        0     1
## 444 48310.33     788        8     4      1     0       1     0        1     0
## 355 16116.84     865        4     4      1     1       1     0        0     1
## 630 17360.81     881        6     2      0     1       1     0        0     0
##     Pontiac Saab Saturn convertible coupe hatchback sedan wagon velocidad
## 800       0    0      0           0     0         1     0     0  17402.42
## 196       0    0      0           1     0         0     0     0 113870.11
## 549       0    0      0           0     0         0     1     0  41760.88
## 444       0    0      0           0     0         0     1     0  77748.09
## 355       0    0      0           0     0         0     1     0  25937.59
## 630       1    0      0           0     1         0     0     0  27939.57
##     distancia
## 800  81.07779
## 196 177.70056
## 549 193.85516
## 444 240.18532
## 355 263.65521
## 630 268.53207
```

#### Filtrado

El filtrado de datos consiste en
elegir una submuestra que cumpla determinadas condiciones. Para ello se
puede utilizar la función [`subset()` ](https://www.rdocumentation.org/packages/base/versions/3.6.1/topics/subset) 
(que además permite seleccionar variables).

A continuación se muestran un par de ejemplos:

```r
subset(cars, cars$dist > 85) # datos con dis>85
```

```
##        Price Mileage Cylinder Doors Cruise Sound Leather Buick Cadillac Chevy
## 1   22661.05   20105        6     4      1     0       0     1        0     0
## 2   21725.01   13457        6     2      1     1       0     0        0     1
## 3   29142.71   31655        4     2      1     1       1     0        0     0
## 4   30731.94   22479        4     2      1     0       0     0        0     0
## 5   33358.77   17590        4     2      1     1       1     0        0     0
## 6   30315.17   23635        4     2      1     0       0     0        0     0
## 7   33381.82   17381        4     2      1     1       1     0        0     0
## 8   30251.02   27558        4     2      1     0       1     0        0     0
## 9   30166.85   25049        4     2      1     0       0     0        0     0
## 10  27060.14   17319        4     4      1     0       1     0        0     0
## 11  26841.08   10003        4     4      1     1       0     0        0     0
## 12  25790.51   21160        4     4      1     1       1     0        0     0
## 13  25148.38   22272        4     4      1     1       1     0        0     0
## 14  24173.53   27015        4     4      1     0       0     0        0     0
## 15  24852.50   22814        4     4      1     1       1     0        0     0
## 16  27825.95   10014        4     4      1     0       1     0        0     0
## 17  26698.08   23055        4     4      1     1       1     0        0     0
## 18  28185.78   19854        4     4      1     1       1     0        0     0
## 19  27241.44   23204        4     4      1     1       1     0        0     0
## 20  30800.66    8017        4     4      1     0       1     0        0     0
## 21  28416.46   14613        4     4      1     0       1     0        0     0
## 22  26653.24   22590        4     4      1     1       1     0        0     0
## 23  27610.86   22881        4     4      1     1       1     0        0     0
## 24  27788.81   26786        4     4      1     0       1     0        0     0
## 25  29986.79   18464        4     4      1     0       1     0        0     0
## 26  29197.79   20907        4     4      1     0       1     0        0     0
## 27  29908.18   19830        4     4      1     1       1     0        0     0
## 28  29481.53   21822        4     4      1     1       1     0        0     0
## 29  26792.30   25357        4     4      1     0       1     0        0     0
## 30  29321.08   21545        4     4      1     0       1     0        0     0
## 31  37383.50   16088        4     2      1     0       1     0        0     0
## 32  34393.00   24031        4     2      1     0       1     0        0     0
## 33  38324.81   12090        4     2      1     1       1     0        0     0
## 34  35304.49   21293        4     2      1     0       1     0        0     0
## 35  28777.96   48991        4     2      1     1       1     0        0     0
## 36  33248.34   27051        4     2      1     0       1     0        0     0
## 37  35580.33   21167        4     2      1     1       0     0        0     0
## 38  30274.71   10800        4     4      1     1       0     0        0     0
## 39  30122.43   14568        4     4      1     1       1     0        0     0
## 40  32197.34    3867        4     4      1     1       0     0        0     0
## 41  27109.41   22598        4     4      1     0       1     0        0     0
## 42  30353.59   11273        4     4      1     1       0     0        0     0
## 43  27256.49   26400        4     4      1     1       0     0        0     0
## 44  28291.76   22328        4     4      1     1       0     0        0     0
## 45  24912.08   38717        4     4      1     0       1     0        0     0
## 46  33183.33    9795        4     4      1     1       1     0        0     0
## 47  28502.96   28598        4     4      1     1       1     0        0     0
## 48  35033.22    1676        4     4      1     1       1     0        0     0
## 49  29844.20   23143        4     4      1     1       1     0        0     0
## 50  30075.99   22052        4     4      1     1       1     0        0     0
## 51  28054.98   26276        4     4      1     1       1     0        0     0
## 52  28432.82   25247        4     4      1     1       1     0        0     0
## 53  32746.13    7924        4     4      1     0       1     0        0     0
## 54  31002.73   15087        4     4      1     1       1     0        0     0
## 55  28829.03   26503        4     4      1     0       1     0        0     0
## 56  31849.31   16956        4     4      1     0       1     0        0     0
## 57  31554.41   20103        4     4      1     1       1     0        0     0
## 58  28678.08   25380        4     4      1     0       1     0        0     0
## 59  29961.25   20015        4     4      1     1       1     0        0     0
## 60  30575.25   22298        4     4      1     1       1     0        0     0
## 61  29914.38   22105        4     4      1     0       1     0        0     0
## 62  17325.27   19894        4     4      1     0       0     0        0     0
## 63  16391.93   18096        4     4      1     1       0     0        0     0
## 64  15505.29   24239        4     4      1     0       0     0        0     0
## 65  15297.84   23062        4     4      0     0       0     0        0     0
## 66  16078.67   22779        4     4      0     0       1     0        0     0
## 67  16551.22   19531        4     4      1     1       1     0        0     0
## 68  14546.88   33374        4     4      0     1       1     0        0     0
## 69  37288.94   32039        8     2      1     1       1     0        0     1
## 70  46732.61    3625        8     2      1     1       1     0        0     1
## 71  47065.21    5239        8     2      1     1       1     0        0     1
## 72  41371.38   20000        8     2      1     0       1     0        0     1
## 73  39547.59   23826        8     2      1     1       1     0        0     1
## 74  42773.03   14546        8     2      1     1       1     0        0     1
## 75  36970.90   30502        8     2      1     1       1     0        0     1
## 76  11203.15   27364        4     2      1     1       1     0        0     1
## 77  13041.87   13607        4     2      0     1       1     0        0     1
## 78  12207.87   23512        4     2      1     1       1     0        0     1
## 79  11726.00   23103        4     2      0     1       1     0        0     1
## 80  13007.98    7372        4     2      1     1       1     0        0     1
## 81  11149.62   34447        4     2      1     1       1     0        0     1
## 82  10788.97   31436        4     2      1     1       1     0        0     1
## 83  12570.14   22479        4     2      0     1       1     0        0     1
## 84  14023.94   13776        4     2      0     1       1     0        0     1
## 85  12706.91   27521        4     2      1     1       1     0        0     1
## 86  11961.62   27394        4     2      1     1       1     0        0     1
## 87  12810.91   19461        4     2      1     1       1     0        0     1
## 88  12845.17   22382        4     2      1     1       1     0        0     1
## 89  12897.93   23200        4     2      1     1       1     0        0     1
## 90  39713.67    8967        8     2      1     1       1     0        0     1
## 91  39875.85    7054        8     2      1     0       1     0        0     1
## 92  31186.74   34191        8     2      1     0       1     0        0     1
## 93  39365.88   11619        8     2      1     1       1     0        0     1
## 94  34297.31   24259        8     2      1     0       1     0        0     1
## 95  38990.61    9410        8     2      1     1       1     0        0     1
## 96  35261.44   21350        8     2      1     1       1     0        0     1
## 97  27370.96   24960        8     2      1     1       1     0        0     0
## 98  31024.87   13678        8     2      1     1       1     0        0     0
## 99  28502.31   27199        8     2      1     1       1     0        0     0
## 100 32219.59   10915        8     2      1     1       1     0        0     0
## 101 29595.79   16193        8     2      1     0       1     0        0     0
## 102 29664.70   21418        8     2      1     0       1     0        0     0
## 103 27548.63   26126        8     2      1     1       1     0        0     0
## 104 12464.07   21891        4     2      1     1       1     0        0     0
## 105 13160.12   13145        4     2      0     1       1     0        0     0
## 106 12832.46   20618        4     2      1     1       1     0        0     0
## 107 12258.86   24318        4     2      1     1       1     0        0     0
## 108 12465.51   23931        4     2      0     1       1     0        0     0
## 109 12828.03   19081        4     2      1     1       1     0        0     0
## 110 11903.10   25285        4     2      0     1       1     0        0     0
## 111 17750.88   25040        6     4      1     1       0     1        0     0
## 112 18145.13   18339        6     4      1     1       0     1        0     0
## 113 17772.97   25052        6     4      1     1       0     1        0     0
## 114 18348.90   23852        6     4      1     1       0     1        0     0
## 115 17394.02   25464        6     4      1     1       0     1        0     0
## 116 20099.26   10036        6     4      1     1       1     1        0     0
## 117 20698.08    2992        6     4      1     0       1     1        0     0
## 118 19924.16   19800        6     4      1     1       1     1        0     0
## 119 17808.20   32896        6     4      1     1       0     1        0     0
## 120 19774.25   23359        6     4      1     1       1     1        0     0
## 121 20538.09   15066        6     4      1     1       0     1        0     0
## 122 19344.17   23765        6     4      1     1       0     1        0     0
## 123 18543.43   26034        6     4      1     1       1     1        0     0
## 124 20512.09   16633        6     4      1     1       0     1        0     0
## 125 23785.92   10577        6     4      1     1       1     1        0     0
## 126 22358.88    8970        6     4      1     1       0     1        0     0
## 127 21895.76   16508        6     4      1     0       1     1        0     0
## 128 22926.09   14363        6     4      1     1       1     1        0     0
## 129 21058.14   24469        6     4      1     1       1     1        0     0
## 130 19556.90   25245        6     4      1     0       0     1        0     0
## 131 21183.12   21394        6     4      1     0       0     1        0     0
## 132 21341.26   25212        6     4      1     1       1     1        0     0
## 133 20986.02   27096        6     4      1     0       0     1        0     0
## 134 21683.03   26779        6     4      1     1       0     1        0     0
## 135 21799.17   24439        6     4      1     0       0     1        0     0
## 136 23447.69   15755        6     4      1     1       0     1        0     0
## 137 23016.01   18147        6     4      1     1       1     1        0     0
## 138 23547.24   16235        6     4      1     1       0     1        0     0
## 139 38600.24   17138        8     4      1     0       1     0        1     0
## 140 36154.30   25339        8     4      1     1       1     0        1     0
## 141 36077.80   21966        8     4      1     0       1     0        1     0
## 142 40335.74   14743        8     4      1     0       1     0        1     0
## 143 35338.65   25163        8     4      1     0       1     0        1     0
## 144 39307.01   16041        8     4      1     0       1     0        1     0
## 145 39801.55   14095        8     4      1     0       1     0        1     0
## 146 32537.19   41829        8     4      1     1       1     0        1     0
## 147 37510.25   21593        8     4      1     0       1     0        1     0
## 148 32954.14   36074        8     4      1     0       1     0        1     0
## 149 36245.16   26250        8     4      1     1       1     0        1     0
## 150 40856.39   12791        8     4      1     1       1     0        1     0
## 151 41378.05    8125        8     4      1     0       1     0        1     0
## 152 37215.17   22211        8     4      1     0       1     0        1     0
## 153 17202.83    9380        6     2      0     1       1     0        0     0
## 154 17675.84    5131        6     2      0     1       1     0        0     0
## 155 15253.87   20917        6     2      1     1       1     0        0     0
## 156 14703.14   23335        6     2      0     1       1     0        0     0
## 157 15059.13   22641        6     2      1     1       1     0        0     0
## 158 16792.68   12071        6     2      1     1       1     0        0     0
## 159 17141.94    6761        6     2      1     1       1     0        0     0
## 160 19204.81   26477        6     4      1     0       1     0        0     0
## 161 18529.34   30063        6     4      1     0       1     0        0     0
## 162 20627.66   20770        6     4      1     1       1     0        0     0
## 163 18158.08   28354        6     4      1     0       0     0        0     0
## 164 19540.24   22628        6     4      1     1       0     0        0     0
## 165 21875.10   12313        6     4      1     0       1     0        0     0
## 166 21903.32    4537        6     4      1     1       0     0        0     0
## 167 15788.10   25295        6     4      1     1       0     0        0     0
## 168 17162.87   20829        6     4      1     0       0     0        0     0
## 169 16569.14   25777        6     4      1     0       0     0        0     0
## 170 16391.17   21304        6     4      1     1       0     0        0     0
## 171 15457.17   29925        6     4      1     1       0     0        0     0
## 172 16283.96   26511        6     4      1     0       1     0        0     0
## 173 18254.92   16554        6     4      1     1       1     0        0     0
## 174 15802.65   21461        4     4      0     0       0     0        0     0
## 175 14396.27   31424        4     4      0     0       0     0        0     0
## 176 14869.28   31791        4     4      1     1       1     0        0     0
## 177 15568.97   18206        4     4      0     0       0     0        0     0
## 178 16353.10   16078        4     4      0     1       0     0        0     0
## 179 15730.05   21391        4     4      0     1       1     0        0     0
## 180 15977.91   17053        4     4      0     0       1     0        0     0
## 181 19646.72   21132        6     4      1     1       1     0        0     0
## 182 20173.91   21211        6     4      1     0       1     0        0     0
## 183 22100.39   12314        6     4      1     1       0     0        0     0
## 184 18701.22   24992        6     4      1     0       0     0        0     0
## 185 19448.23   27721        6     4      1     0       1     0        0     0
## 186 21281.88   17417        6     4      1     1       1     0        0     0
## 187 21230.98   11229        6     4      1     1       0     0        0     0
## 188 23578.16   19148        8     4      1     0       1     0        0     0
## 189 22231.56   21929        8     4      1     1       1     0        0     0
## 190 22189.12   25651        8     4      1     0       1     0        0     0
## 191 22525.27   19521        8     4      1     1       1     0        0     0
## 192 23449.31   17273        8     4      1     0       1     0        0     0
## 193 21403.76   27168        8     4      1     0       1     0        0     0
## 194 21200.69   31197        8     4      1     1       1     0        0     0
## 195 68566.19    6420        8     2      1     1       1     0        1     0
## 196 70755.47     583        8     2      1     1       1     0        1     0
## 197 52001.99   42691        8     2      1     1       1     0        1     0
## 198 60567.55   23193        8     2      1     1       1     0        1     0
## 199 63913.12   18200        8     2      1     1       1     0        1     0
## 200 69133.73    7892        8     2      1     1       1     0        1     0
## 201 66374.31   12021        8     2      1     1       1     0        1     0
## 202 17322.08   10102        6     4      1     0       1     0        0     0
## 203 16507.07   16229        6     4      1     0       0     0        0     0
## 204 16425.17   14242        6     4      1     0       0     0        0     0
## 205 15118.89   25979        6     4      1     1       0     0        0     0
## 206 17978.36   10986        6     4      1     0       0     0        0     0
## 207 13585.64   35662        6     4      1     0       0     0        0     0
## 208 15731.13   20484        6     4      1     1       0     0        0     0
## 209 21908.37   17353        6     4      1     0       0     1        0     0
## 210 20952.22   20158        6     4      1     1       0     1        0     0
## 211 19425.85   27839        6     4      1     1       0     1        0     0
## 212 19191.99   29187        6     4      1     1       1     1        0     0
## 213 21956.34   17787        6     4      1     1       0     1        0     0
## 214 19981.13   24323        6     4      1     1       0     1        0     0
## 215 21646.12   19562        6     4      1     1       1     1        0     0
## 216 32075.98   23553        4     2      1     1       0     0        0     0
## 217 27666.23   35157        4     2      1     0       0     0        0     0
## 218 34819.30   12251        4     2      1     0       0     0        0     0
## 219 33540.54   20925        4     2      1     0       1     0        0     0
## 220 31969.07   24559        4     2      1     0       0     0        0     0
## 221 35622.14   10340        4     2      1     1       0     0        0     0
## 222 32737.08   19112        4     2      1     1       1     0        0     0
## 223 23249.84   27686        4     4      1     0       0     0        0     0
## 224 29246.24    3907        4     4      1     0       1     0        0     0
## 225 24801.62   26345        4     4      1     1       1     0        0     0
## 226 22244.88   50387        4     4      1     0       1     0        0     0
## 227 26775.03   16688        4     4      1     0       0     0        0     0
## 228 25996.81   21433        4     4      1     1       1     0        0     0
## 229 25299.97   19569        4     4      1     1       0     0        0     0
## 230 31156.60   18805        4     4      1     1       1     0        0     0
## 231 29114.54   21960        4     4      1     0       1     0        0     0
## 232 30443.88   15050        4     4      1     0       1     0        0     0
## 233 24903.48   40719        4     4      1     1       1     0        0     0
## 234 31084.94   18187        4     4      1     1       1     0        0     0
## 235 33005.78    6409        4     4      1     1       1     0        0     0
## 236 31153.01   17317        4     4      1     0       1     0        0     0
## 237 23329.21   25218        4     4      1     0       1     0        0     0
## 238 23274.48   21616        4     4      1     1       0     0        0     0
## 239 27280.98    4836        4     4      1     1       0     0        0     0
## 240 25959.12   17431        4     4      1     0       1     0        0     0
## 241 12630.78   22571        4     2      1     1       1     0        0     1
## 242 12425.39   22771        4     2      0     1       1     0        0     1
## 243 12549.89   25816        4     2      0     1       1     0        0     1
## 244 12274.96   19612        4     2      1     1       1     0        0     1
## 245 13446.21   17741        4     2      0     1       1     0        0     1
## 246 12319.70   24568        4     2      1     1       1     0        0     1
## 247 12234.89   30297        4     2      1     1       1     0        0     1
## 248 12684.99   29891        4     2      1     1       1     0        0     1
## 249 12553.07   32844        4     2      1     1       1     0        0     1
## 250 14185.02   20374        4     2      0     1       1     0        0     1
## 251 13019.07   27942        4     2      1     1       1     0        0     1
## 252 15747.80    6048        4     2      0     1       1     0        0     1
## 253 13699.04   25845        4     2      1     1       1     0        0     1
## 254 13530.07   27249        4     2      1     1       1     0        0     1
## 255 19177.41   10414        6     2      1     1       0     0        0     1
## 256 16357.99   23491        6     2      1     1       0     0        0     1
## 257 15797.20   26700        6     2      1     1       0     0        0     1
## 258 17515.40   18602        6     2      1     0       0     0        0     1
## 259 18040.14   11647        6     2      1     0       1     0        0     1
## 260 16345.94   25931        6     2      1     1       0     0        0     1
## 261 18800.09    5827        6     2      1     0       0     0        0     1
## 262 10386.04   22225        4     4      0     0       0     0        0     1
## 263 11017.17   20100        4     4      0     1       0     0        0     1
## 264 12163.82   12101        4     4      0     0       1     0        0     1
## 265 11096.86   20334        4     4      1     0       0     0        0     1
## 266 10777.05   27906        4     4      0     0       0     0        0     1
## 267 12146.19   10011        4     4      0     0       1     0        0     1
## 268 11472.02   19699        4     4      0     0       1     0        0     1
## 269 16825.19   23460        6     4      0     1       1     0        0     1
## 270 14914.20   33906        6     4      0     1       1     0        0     1
## 271 17801.23   19386        6     4      1     1       1     0        0     1
## 272 16543.98   24583        6     4      0     1       1     0        0     1
## 273 16744.03   21829        6     4      0     1       1     0        0     1
## 274 16143.96   26532        6     4      1     1       1     0        0     1
## 275 17891.63   17020        6     4      0     1       1     0        0     1
## 276 12409.95   11981        4     4      1     1       1     0        0     1
## 277 12314.59    4142        4     4      0     1       0     0        0     1
## 278 12649.11    3629        4     4      0     1       0     0        0     1
## 279 10491.08   30948        4     4      0     1       0     0        0     1
## 280 11318.01   11156        4     4      0     1       1     0        0     1
## 281 11700.11   15253        4     4      1     0       0     0        0     1
## 282 11215.02   19945        4     4      0     0       0     0        0     1
## 283 14894.98    2464        4     4      0     1       1     0        0     1
## 284 13167.70   14630        4     4      0     1       1     0        0     1
## 285 13230.92   25862        4     4      0     1       1     0        0     1
## 286 12573.90   25048        4     4      1     1       1     0        0     1
## 287 11328.96   39946        4     4      1     1       1     0        0     1
## 288 12383.40   25069        4     4      1     1       1     0        0     1
## 289 14678.11   11488        4     4      1     1       1     0        0     1
## 290 13545.03   27431        4     4      0     1       1     0        0     1
## 291 14304.74   21128        4     4      0     1       1     0        0     1
## 292 14847.04   12980        4     4      0     1       1     0        0     1
## 293 14593.85   18790        4     4      0     1       1     0        0     1
## 294 13744.85   23748        4     4      0     1       1     0        0     1
## 295 13688.95   21611        4     4      1     1       1     0        0     1
## 296 12741.19   34815        4     4      0     1       1     0        0     1
## 297 19075.68   18198        6     4      1     0       1     0        0     1
## 298 17839.80   25453        6     4      1     0       1     0        0     1
## 299 21757.05    1853        6     4      1     0       0     0        0     1
## 300 17789.35   26980        6     4      1     1       0     0        0     1
## 301 18527.21   19874        6     4      1     1       0     0        0     1
## 302 17294.18   29368        6     4      1     1       0     0        0     1
## 303 18912.98   21512        6     4      1     1       0     0        0     1
## 304 16472.90   21675        6     4      1     1       1     0        0     1
## 305 16300.47   25697        6     4      1     1       1     0        0     1
## 306 15138.40   32462        6     4      0     1       1     0        0     1
## 307 17158.92   21417        6     4      0     1       1     0        0     1
## 308 15623.20   27476        6     4      0     1       1     0        0     1
## 309 19164.61    1480        6     4      1     1       1     0        0     1
## 310 16993.78   23621        6     4      0     1       1     0        0     1
## 311 12230.10   28408        4     2      0     1       1     0        0     1
## 312 12507.49   19715        4     2      1     1       1     0        0     1
## 313 13141.05   19898        4     2      1     1       1     0        0     1
## 314 15053.93    4652        4     2      1     1       1     0        0     1
## 315 13594.09   20682        4     2      1     1       1     0        0     1
## 316 13464.80   14231        4     2      1     1       1     0        0     1
## 317 14642.32    6224        4     2      0     1       1     0        0     1
## 318 14255.75   16958        4     4      0     1       1     0        0     1
## 319 13308.83   20043        4     4      0     1       1     0        0     1
## 320 14145.88   20512        4     4      1     1       1     0        0     1
## 321 12944.94   21684        4     4      0     1       1     0        0     1
## 322 12846.06   27560        4     4      1     1       1     0        0     1
## 323 14266.91    8615        4     4      0     1       1     0        0     1
## 324 13688.00   18766        4     4      1     1       1     0        0     1
## 325 20017.97   22729        6     2      1     0       0     0        0     1
## 326 20839.15   22152        6     2      1     1       1     0        0     1
## 327 17586.93   39049        6     2      1     0       1     0        0     1
## 328 20676.17   18021        6     2      1     0       1     0        0     1
## 329 22384.12   14788        6     2      1     1       1     0        0     1
## 330 21745.03    7065        6     2      1     1       0     0        0     1
## 331 11179.95   23121        4     4      0     1       1     0        0     1
## 332 11031.13   20156        4     4      0     1       1     0        0     1
## 333 10921.95   23119        4     4      0     1       1     0        0     1
## 334 11343.05   20186        4     4      1     1       1     0        0     1
## 335 11013.87   25746        4     4      1     1       1     0        0     1
## 336 11070.06   25476        4     4      0     1       1     0        0     1
## 337 11391.21   21421        4     4      0     1       1     0        0     1
## 338 18273.01   16335        6     4      0     1       1     0        0     1
## 339 19471.97    6608        6     4      1     1       1     0        0     1
## 340 17663.22   19490        6     4      1     1       1     0        0     1
## 341 18009.85   15190        6     4      0     1       1     0        0     1
## 342 16988.30   24905        6     4      1     1       1     0        0     1
## 343 17553.75   18451        6     4      0     1       1     0        0     1
## 344 18311.76   17441        6     4      0     1       1     0        0     1
## 345 11918.46    7278        4     4      0     0       0     0        0     1
## 346  9919.05   34621        4     4      0     1       0     0        0     1
## 347 10805.13   21013        4     4      1     1       1     0        0     1
## 348 11302.90   14627        4     4      0     1       0     0        0     1
## 349 10770.11   25065        4     4      0     1       0     0        0     1
## 350 10872.01   25869        4     4      0     0       0     0        0     1
## 351 12408.81   10213        4     4      0     0       1     0        0     1
## 352 14401.91   21527        4     4      0     1       1     0        0     1
## 353 15163.17   17158        4     4      1     1       1     0        0     1
## 354 14508.75   18910        4     4      0     1       1     0        0     1
## 355 16116.84     865        4     4      1     1       1     0        0     1
## 356 14897.04   18210        4     4      1     1       1     0        0     1
## 357 15084.82   14824        4     4      1     1       1     0        0     1
## 358 14418.17   19818        4     4      0     1       1     0        0     1
## 359 16403.25   23133        6     4      1     1       1     0        0     1
## 360 17316.10   19593        6     4      1     1       1     0        0     1
## 361 17119.46   18277        6     4      1     1       1     0        0     1
## 362 16860.87   19883        6     4      1     1       1     0        0     1
## 363 16536.74   24218        6     4      1     1       1     0        0     1
## 364 19446.88     932        6     4      0     1       1     0        0     1
## 365 16295.21   28239        6     4      0     1       1     0        0     1
## 366 16860.09   22841        6     4      1     1       1     0        0     1
## 367 18324.83    7397        6     4      1     1       1     0        0     1
## 368 18974.92    5632        6     4      0     1       1     0        0     1
## 369 16027.29   22889        6     4      0     1       1     0        0     1
## 370 16267.09   21452        6     4      0     1       1     0        0     1
## 371 19581.23    7645        6     4      0     1       1     0        0     1
## 372 18169.38   14754        6     4      0     1       1     0        0     1
## 373 14881.96   14376        4     2      0     0       0     0        0     0
## 374 13719.24   23645        4     2      0     0       0     0        0     0
## 375 14077.97   26788        4     2      0     0       1     0        0     0
## 376 14411.86   26986        4     2      1     0       1     0        0     0
## 377 13991.04   21020        4     2      0     1       0     0        0     0
## 378 15033.15   19455        4     2      0     0       1     0        0     0
## 379 14771.00   22255        4     2      1     0       1     0        0     0
## 380 13518.24   25981        4     2      1     1       0     0        0     0
## 381 13825.15   26034        4     2      0     0       1     0        0     0
## 382 16256.24   10555        4     2      0     0       1     0        0     0
## 383 13869.15   24349        4     2      1     1       0     0        0     0
## 384 16916.87    2879        4     2      1     0       1     0        0     0
## 385 13811.16   26236        4     2      0     1       1     0        0     0
## 386 11539.05   38958        4     2      0     0       0     0        0     0
## 387 18566.07   24747        6     4      1     1       1     0        0     0
## 388 18856.02   22423        6     4      1     1       0     0        0     0
## 389 19682.04   11554        6     4      1     0       1     0        0     0
## 390 20318.89   17583        6     4      1     1       0     0        0     0
## 391 17844.73   21121        6     4      1     1       0     0        0     0
## 392 18678.41   16496        6     4      1     1       0     0        0     0
## 393 17768.06   28385        6     4      1     1       1     0        0     0
## 394 13961.11   19602        4     4      0     1       1     0        0     0
## 395 15077.18   13262        4     4      0     1       1     0        0     0
## 396 13034.07   23976        4     4      0     1       1     0        0     0
## 397 15604.15    5788        4     4      1     1       1     0        0     0
## 398 15979.01    3946        4     4      1     1       1     0        0     0
## 399 14841.92   12420        4     4      0     1       1     0        0     0
## 400 16379.85    4188        4     4      1     1       1     0        0     0
## 401 15709.05   22236        6     4      1     1       0     1        0     0
## 402 15048.04   22964        6     4      1     1       0     1        0     0
## 403 15295.02   27325        6     4      1     1       1     1        0     0
## 404 16339.17   19832        6     4      1     0       1     1        0     0
## 405 17542.04    9135        6     4      1     1       0     1        0     0
## 406 16218.85   13196        6     4      1     1       0     1        0     0
## 407 17314.10    8221        6     4      1     1       1     1        0     0
## 408 21831.82   25564        6     4      1     1       1     1        0     0
## 409 23420.71   18910        6     4      1     0       1     1        0     0
## 410 25098.63   10014        6     4      1     1       0     1        0     0
## 411 23077.57   23798        6     4      1     0       1     1        0     0
## 412 22435.20   22287        6     4      1     1       1     1        0     0
## 413 25589.98    2308        6     4      1     1       0     1        0     0
## 414 46747.67   15343        8     4      1     1       1     0        1     0
## 415 51154.05    2202        8     4      1     1       1     0        1     0
## 416 42677.60   24052        8     4      1     0       1     0        1     0
## 417 43892.47   23371        8     4      1     0       1     0        1     0
## 418 44300.64   23751        8     4      1     0       1     0        1     0
## 419 40619.07   30082        8     4      1     1       1     0        1     0
## 420 44084.91   21367        8     4      1     1       1     0        1     0
## 421 30792.15   17870        6     4      1     1       1     0        1     0
## 422 29275.21   21056        6     4      1     1       1     0        1     0
## 423 30392.75   18449        6     4      1     1       1     0        1     0
## 424 30957.08   10625        6     4      1     1       1     0        1     0
## 425 30781.52   14937        6     4      1     1       1     0        1     0
## 426 33417.97    6598        6     4      1     1       1     0        1     0
## 427 28040.13   27484        6     4      1     1       1     0        1     0
## 428 31181.72   26222        8     4      1     0       1     0        1     0
## 429 33220.03   18661        8     4      1     0       1     0        1     0
## 430 32501.25   17508        8     4      1     0       1     0        1     0
## 431 32509.48   20910        8     4      1     0       1     0        1     0
## 432 31059.18   27544        8     4      1     1       1     0        1     0
## 433 31132.21   23124        8     4      1     1       1     0        1     0
## 434 35715.77    6447        8     4      1     0       1     0        1     0
## 435 36633.63   22042        6     4      1     1       1     0        1     0
## 436 38297.46   16754        6     4      1     0       1     0        1     0
## 437 42741.52    2846        6     4      1     0       1     0        1     0
## 438 40966.61    7476        6     4      1     1       1     0        1     0
## 439 32038.34   35326        6     4      1     1       1     0        1     0
## 440 37192.90   19100        6     4      1     0       1     0        1     0
## 441 34974.38   25796        6     4      1     1       1     0        1     0
## 442 44205.88   15104        8     4      1     0       1     0        1     0
## 443 42377.96   18581        8     4      1     0       1     0        1     0
## 444 48310.33     788        8     4      1     0       1     0        1     0
## 445 41516.43   23861        8     4      1     1       1     0        1     0
## 446 41671.58   20575        8     4      1     0       1     0        1     0
## 447 41053.48   25717        8     4      1     1       1     0        1     0
## 448 38208.50   31303        8     4      1     1       1     0        1     0
## 449 13072.84   14311        4     4      0     1       1     0        0     1
## 450 11539.05   24163        4     4      0     1       1     0        0     1
## 451 11699.03   19816        4     4      1     1       1     0        0     1
## 452 11574.17   21525        4     4      0     1       1     0        0     1
## 453 12243.06   25014        4     4      1     1       1     0        0     1
## 454 11671.86   25727        4     4      1     1       1     0        0     1
## 455 14061.12    4922        4     4      0     1       1     0        0     1
## 456 15553.21    7695        4     4      0     1       1     0        0     1
## 457 11581.91   36566        4     4      0     1       1     0        0     1
## 458 13161.94   21145        4     4      1     1       1     0        0     1
## 459 14077.97   17445        4     4      0     1       1     0        0     1
## 460 15047.00   12305        4     4      1     1       1     0        0     1
## 461 12981.95   20309        4     4      1     1       1     0        0     1
## 462 14220.01   23069        4     4      1     1       1     0        0     1
## 463 13159.82   22740        4     4      1     1       1     0        0     1
## 464 12678.85   28683        4     4      0     1       1     0        0     1
## 465 13994.91   17270        4     4      0     1       1     0        0     1
## 466 14194.82    9561        4     4      0     1       1     0        0     1
## 467 14696.03    6709        4     4      1     1       1     0        0     1
## 468 14582.77    7115        4     4      1     1       1     0        0     1
## 469 12495.97   26204        4     4      0     1       1     0        0     1
## 470 20021.20    1787        6     4      1     0       0     0        0     1
## 471 15554.28   33357        6     4      1     1       1     0        0     1
## 472 16644.09   22383        6     4      1     1       1     0        0     1
## 473 18835.19    8211        6     4      1     0       1     0        0     1
## 474 17154.58   21567        6     4      1     0       1     0        0     1
## 475 15951.81   26070        6     4      1     1       1     0        0     1
## 476 16508.59   27460        6     4      1     1       1     0        0     1
## 477 16646.77   20154        6     4      0     1       1     0        0     1
## 478 17218.69   14579        6     4      1     1       1     0        0     1
## 479 17089.92    8732        6     4      0     1       1     0        0     1
## 480 17463.05   11393        6     4      1     1       1     0        0     1
## 481 15680.86   25956        6     4      0     1       1     0        0     1
## 482 16752.51   18562        6     4      1     1       1     0        0     1
## 483 15664.62   25787        6     4      1     1       1     0        0     1
## 484 18620.87   25516        6     4      1     0       1     0        0     0
## 485 20452.67   10338        6     4      1     0       0     0        0     0
## 486 20677.59   11204        6     4      1     1       0     0        0     0
## 487 21383.07    7287        6     4      1     1       1     0        0     0
## 488 19294.79   19539        6     4      1     0       0     0        0     0
## 489 18042.22   21702        6     4      1     0       0     0        0     0
## 490 16216.98   35624        6     4      1     1       0     0        0     0
## 491 15724.25   23989        6     4      1     0       0     0        0     0
## 492 19822.12    1592        6     4      1     1       0     0        0     0
## 493 15756.15   29325        6     4      1     1       0     0        0     0
## 494 15979.01   21974        6     4      1     0       0     0        0     0
## 495 16256.24   22637        6     4      1     0       0     0        0     0
## 496 16041.69   27800        6     4      1     1       0     0        0     0
## 497 19567.26    2189        6     4      1     1       1     0        0     0
## 498 15110.19    2392        4     4      1     1       0     0        0     0
## 499 12036.22   19853        4     4      0     1       1     0        0     0
## 500 12099.01   22758        4     4      1     1       0     0        0     0
## 501 12284.29   24740        4     4      1     1       0     0        0     0
## 502 14203.00   11836        4     4      0     0       0     0        0     0
## 503 14116.92   12878        4     4      1     0       0     0        0     0
## 504 12791.75   16163        4     4      0     0       1     0        0     0
## 505 14739.07    1737        4     4      0     0       1     0        0     0
## 506 12965.22   29707        4     4      0     1       1     0        0     0
## 507 12412.52   24664        4     4      0     1       1     0        0     0
## 508 10563.07   32458        4     4      0     0       0     0        0     0
## 509 12333.60   21877        4     4      0     0       0     0        0     0
## 510 12119.09   22826        4     4      0     0       1     0        0     0
## 511 11521.53   34998        4     4      0     1       1     0        0     0
## 512 12105.98   28298        4     4      0     1       0     0        0     0
## 513 12162.14   21770        4     4      0     1       0     0        0     0
## 514 13122.91   19101        4     4      0     1       1     0        0     0
## 515 13494.29   19500        4     4      1     0       1     0        0     0
## 516 13174.07   13318        4     4      0     0       1     0        0     0
## 517 13216.91   24069        4     4      0     1       0     0        0     0
## 518 12594.18   26328        4     4      0     0       1     0        0     0
## 519 11679.92   23388        4     4      0     0       0     0        0     0
## 520 20830.99   19419        6     4      1     1       0     0        0     0
## 521 21607.77   11069        6     4      1     0       1     0        0     0
## 522 22004.93   15516        6     4      1     1       1     0        0     0
## 523 19116.13   26252        6     4      1     0       1     0        0     0
## 524 23197.44    2295        6     4      1     1       1     0        0     0
## 525 20109.90   22891        6     4      1     1       1     0        0     0
## 526 23102.02    5653        6     4      1     0       0     0        0     0
## 527 26781.81   12052        6     4      1     1       0     1        0     0
## 528 21536.74   37128        6     4      1     1       1     1        0     0
## 529 26190.27   17335        6     4      1     0       1     1        0     0
## 530 26060.34    9795        6     4      1     1       0     1        0     0
## 531 23159.54   25869        6     4      1     0       0     1        0     0
## 532 26302.07   13050        6     4      1     1       0     1        0     0
## 533 23348.02   24027        6     4      1     0       1     1        0     0
## 534 16706.67    9150        4     4      0     0       0     0        0     0
## 535 16927.78    2973        4     4      0     0       0     0        0     0
## 536 14909.05   23323        4     4      0     1       1     0        0     0
## 537 16379.10    8754        4     4      0     1       0     0        0     0
## 538 15622.12   23217        4     4      1     1       1     0        0     0
## 539 17418.07    4463        4     4      1     0       1     0        0     0
## 540 15821.95   14304        4     4      0     0       1     0        0     0
## 541 23573.82   12466        6     2      1     1       0     0        0     1
## 542 21020.84   25550        6     2      1     1       1     0        0     1
## 543 23527.73   14948        6     2      1     1       0     0        0     1
## 544 20047.95   24665        6     2      1     1       0     0        0     1
## 545 22470.36   22626        6     2      1     1       0     0        0     1
## 546 20619.11   24067        6     2      1     0       0     0        0     1
## 547 21525.34   25020        6     2      1     1       1     0        0     1
## 548 27714.05    5379        6     4      1     1       0     0        0     1
## 549 25948.96     636        6     4      1     0       0     0        0     1
## 550 22064.29   27384        6     4      1     1       1     0        0     1
## 551 23151.55   27940        6     4      1     0       0     0        0     1
## 552 20294.58   33892        6     4      1     0       0     0        0     1
## 553 22894.44   26272        6     4      1     1       1     0        0     1
## 554 24809.04   16111        6     4      1     0       0     0        0     1
## 555 10354.04   14521        4     4      0     1       0     0        0     1
## 556 10287.98   16521        4     4      1     1       1     0        0     1
## 557 10897.08    6699        4     4      0     1       1     0        0     1
## 558  9789.04   22986        4     4      0     1       1     0        0     1
## 559 10106.02   14200        4     4      1     0       0     0        0     1
## 560  9506.05   22169        4     4      0     0       1     0        0     1
## 561  9220.83   29992        4     4      1     0       1     0        0     1
## 562  8769.00   35299        4     4      0     0       0     0        0     1
## 563  8870.95   32914        4     4      1     1       0     0        0     1
## 564 10315.02   14438        4     4      0     0       1     0        0     1
## 565  9654.06   19183        4     4      0     0       0     0        0     1
## 566  9041.91   26191        4     4      0     0       1     0        0     1
## 567 10971.10    7091        4     4      1     0       0     0        0     1
## 568  8638.93   25216        4     4      0     0       0     0        0     1
## 569 29612.15   32477        4     2      1     1       1     0        0     0
## 570 33586.91   18930        4     2      1     0       1     0        0     0
## 571 37088.56    3828        4     2      1     1       1     0        0     0
## 572 24405.07   31344        4     4      1     1       1     0        0     0
## 573 27703.20   24738        4     4      1     1       1     0        0     0
## 574 25618.28   20208        4     4      1     0       1     0        0     0
## 575 23733.40   27600        4     4      1     1       0     0        0     0
## 576 28204.60   22021        4     4      1     1       1     0        0     0
## 577 27284.75   14281        4     4      1     1       1     0        0     0
## 578 30959.93   17673        4     4      1     1       1     0        0     0
## 579 28328.27   20685        4     4      1     0       1     0        0     0
## 580 26012.37   34269        4     4      1     1       1     0        0     0
## 581 33984.43   25420        4     2      1     0       0     0        0     0
## 582 38167.17   13162        4     2      1     0       1     0        0     0
## 583 36338.75   18195        4     2      1     1       0     0        0     0
## 584 25267.37   34175        4     4      1     1       0     0        0     0
## 585 32053.10    5144        4     4      1     1       0     0        0     0
## 586 26789.83   22189        4     4      1     0       0     0        0     0
## 587 30271.92   23426        4     4      1     1       1     0        0     0
## 588 26955.04   31773        4     4      1     0       1     0        0     0
## 589 32649.76   16975        4     4      1     1       1     0        0     0
## 590 16106.83   19465        4     4      1     0       0     0        0     0
## 591 15174.35   27887        4     4      0     0       0     0        0     0
## 592 16033.93   18391        4     4      0     1       1     0        0     0
## 593 38824.87   25960        8     2      1     0       1     0        0     1
## 594 44749.69   12115        8     2      1     0       1     0        0     1
## 595 39691.73   25169        8     2      1     1       1     0        0     1
## 596 13135.91   21796        4     2      1     1       1     0        0     1
## 597 12045.92   19136        4     2      0     1       1     0        0     1
## 598 39092.19   10717        8     2      1     1       1     0        0     1
## 599 34739.21   25747        8     2      1     0       1     0        0     1
## 600 35575.42   22740        8     2      1     0       1     0        0     1
## 601 13106.90   21910        4     2      0     1       1     0        0     1
## 602 12487.05   28492        4     2      0     1       1     0        0     1
## 603 11539.85   22405        4     2      0     1       1     0        0     1
## 604 12469.53   19712        4     2      0     1       1     0        0     1
## 605 27425.84   23886        8     2      1     0       1     0        0     0
## 606 25527.01   36480        8     2      1     1       1     0        0     0
## 607 12830.10   17830        4     2      1     1       1     0        0     0
## 608 12209.56   26097        4     2      0     1       1     0        0     0
## 609 32422.76    9185        8     2      1     1       1     0        0     0
## 610 12878.05   19225        4     2      1     1       1     0        0     0
## 611 19027.86   21797        6     4      1     0       1     1        0     0
## 612 17944.86   19592        6     4      1     0       0     1        0     0
## 613 17645.75   27830        6     4      1     1       1     1        0     0
## 614 21335.85   10237        6     4      1     0       0     1        0     0
## 615 17968.84   34665        6     4      1     1       1     1        0     0
## 616 19105.13   24008        6     4      1     0       0     1        0     0
## 617 21460.01   19467        6     4      1     0       1     1        0     0
## 618 21273.06   18908        6     4      1     0       0     1        0     0
## 619 20406.10   22596        6     4      1     0       0     1        0     0
## 620 22230.03   22102        6     4      1     0       1     1        0     0
## 621 20902.10   29649        6     4      1     1       1     1        0     0
## 622 22625.07   23612        6     4      1     0       1     1        0     0
## 623 35866.58   24415        8     4      1     1       1     0        1     0
## 624 38445.90   18661        8     4      1     0       1     0        1     0
## 625 34685.66   25421        8     4      1     0       1     0        1     0
## 626 42820.33    5499        8     4      1     0       1     0        1     0
## 627 36332.89   25153        8     4      1     0       1     0        1     0
## 628 41419.04   14452        8     4      1     0       1     0        1     0
## 629 15595.88   18315        6     2      0     1       1     0        0     0
## 630 17360.81     881        6     2      0     1       1     0        0     0
## 631 15594.81   22414        6     2      0     1       1     0        0     0
## 632 17095.04   18720        6     4      1     0       0     0        0     0
## 633 14963.05   31471        6     4      1     1       0     0        0     0
## 634 16997.69   25830        6     4      1     0       1     0        0     0
## 635 22104.97    9049        6     4      1     1       1     0        0     0
## 636 22736.83    5690        6     4      1     1       1     0        0     0
## 637 22311.05   11221        6     4      1     1       1     0        0     0
## 638 15086.90   27438        4     4      0     0       1     0        0     0
## 639 15589.78   21307        4     4      0     0       0     0        0     0
## 640 17803.28   12303        4     4      1     1       1     0        0     0
## 641 21300.02   12772        6     4      1     1       0     0        0     0
## 642 19423.17   25557        6     4      1     0       1     0        0     0
## 643 19956.76   26028        6     4      1     1       0     0        0     0
## 644 25452.47   11892        8     4      1     0       1     0        0     0
## 645 21765.07   25794        8     4      1     0       1     0        0     0
## 646 21982.65   20472        8     4      1     1       1     0        0     0
## 647 55639.09   31805        8     2      1     0       1     0        1     0
## 648 65281.48   15600        8     2      1     1       1     0        1     0
## 649 57154.44   29260        8     2      1     1       1     0        1     0
## 650 18490.98    7755        6     4      1     1       1     0        0     0
## 651 16175.96   19095        6     4      1     1       0     0        0     0
## 652 18173.98    5826        6     4      1     1       1     0        0     0
## 653 21562.05   23767        6     4      1     0       1     1        0     0
## 654 19641.74   31324        6     4      1     1       1     1        0     0
## 655 21575.46   20137        6     4      1     1       0     1        0     0
## 656 34355.00   17711        4     2      1     1       0     0        0     0
## 657 33287.41   21661        4     2      1     0       1     0        0     0
## 658 31970.54   21208        4     2      1     1       0     0        0     0
## 659 24896.60   21266        4     4      1     1       0     0        0     0
## 660 24063.01   27674        4     4      1     1       1     0        0     0
## 661 26337.83   16068        4     4      1     0       0     0        0     0
## 662 25845.21   36557        4     4      1     1       1     0        0     0
## 663 30661.26   14278        4     4      1     0       1     0        0     0
## 664 30322.15   16225        4     4      1     1       1     0        0     0
## 665 15635.80    1169        4     2      0     1       1     0        0     1
## 666 17685.20   15898        6     2      1     0       0     0        0     1
## 667 14584.45    1160        4     2      1     1       1     0        0     1
## 668 15503.51   33345        6     2      1     0       0     0        0     1
## 669 13681.70   10210        4     2      1     1       1     0        0     1
## 670 18910.80    8345        6     2      1     0       1     0        0     1
## 671 12327.64   19347        4     2      0     1       1     0        0     1
## 672 14619.08   15768        4     2      1     1       1     0        0     1
## 673 13310.06   26143        4     2      1     1       1     0        0     1
## 674  9928.19   29680        4     4      0     0       1     0        0     1
## 675 11137.05   22484        4     4      0     1       1     0        0     1
## 676 11045.11   24568        4     4      1     0       1     0        0     1
## 677 18950.91    8687        6     4      0     1       1     0        0     1
## 678 16723.99   19740        6     4      1     1       1     0        0     1
## 679 18957.89    5936        6     4      0     1       1     0        0     1
## 680 11555.27   13404        4     4      1     1       0     0        0     1
## 681 18083.40   29420        6     4      1     1       1     0        0     1
## 682 11080.52   36855        4     4      0     1       1     0        0     1
## 683 13471.01   18910        4     4      1     1       1     0        0     1
## 684 14198.09   11322        4     4      1     1       1     0        0     1
## 685 19409.75   18795        6     4      1     1       1     0        0     1
## 686 19528.10   14115        6     4      1     1       0     0        0     1
## 687 15000.99   14504        4     4      0     1       1     0        0     1
## 688  9954.05   37345        4     4      0     1       1     0        0     1
## 689 18800.96    7961        6     4      0     1       1     0        0     1
## 690 15128.99   13828        4     4      1     1       1     0        0     1
## 691 14997.88    8880        4     4      0     1       1     0        0     1
## 692 15233.16   32535        6     4      0     1       1     0        0     1
## 693 17458.22   15144        6     4      1     1       1     0        0     1
## 694 10144.95   23963        4     4      1     1       0     0        0     1
## 695 14397.93    5189        4     2      0     1       1     0        0     1
## 696 12733.86   21386        4     2      1     1       1     0        0     1
## 697 13678.86   17971        4     2      1     1       1     0        0     1
## 698 14275.13   18533        4     4      1     1       1     0        0     1
## 699 14222.31    8427        4     4      0     1       1     0        0     1
## 700 13762.90   18040        4     4      1     1       1     0        0     1
## 701 20537.14   16950        6     2      1     0       0     0        0     1
## 702 21233.91   17337        6     2      1     1       1     0        0     1
## 703 18876.87   27218        6     2      1     1       0     0        0     1
## 704 11115.01   30056        4     4      1     1       1     0        0     1
## 705 11247.86   21427        4     4      1     1       1     0        0     1
## 706 11394.89   25107        4     4      0     1       1     0        0     1
## 707 17115.12   24461        6     4      0     1       1     0        0     1
## 708 18004.87   18771        6     4      0     1       1     0        0     1
## 709 16803.12   25874        6     4      1     1       1     0        0     1
## 710 17312.91   21420        6     4      1     1       1     0        0     1
## 711 11169.92   22380        4     4      0     1       0     0        0     1
## 712 16428.58    9882        4     4      0     1       1     0        0     1
## 713 14191.88   21181        4     4      1     1       1     0        0     1
## 714 10921.95   27776        4     4      1     0       0     0        0     1
## 715 14175.88   21627        4     4      0     1       1     0        0     1
## 716 11615.02   19014        4     4      0     1       1     0        0     1
## 717 16341.80   25394        6     4      0     1       1     0        0     1
## 718 16713.98   26328        6     4      1     1       1     0        0     1
## 719 17173.94   18721        6     4      0     1       1     0        0     1
## 720 17986.22   17488        6     4      0     1       1     0        0     1
## 721 16456.97   20200        6     4      0     1       1     0        0     1
## 722 13600.03   27231        4     2      0     1       0     0        0     0
## 723 15395.01   12920        4     2      1     0       0     0        0     0
## 724 15277.07   16951        4     2      0     1       0     0        0     0
## 725 13032.87   30108        4     2      1     1       1     0        0     0
## 726 14702.80   17656        4     2      1     1       0     0        0     0
## 727 15639.04    8507        4     2      1     0       0     0        0     0
## 728 15327.10    4318        4     4      0     1       1     0        0     0
## 729 20127.04   18419        6     4      1     1       1     0        0     0
## 730 15846.01    5350        4     4      0     1       1     0        0     0
## 731 13162.85   24542        4     4      0     1       1     0        0     0
## 732 18063.00   27574        6     4      1     0       0     0        0     0
## 733 19751.04   20510        6     4      1     0       1     0        0     0
## 734 23493.08   20453        6     4      1     1       0     1        0     0
## 735 21878.12   23237        6     4      1     1       0     1        0     0
## 736 21698.01   25489        6     4      1     1       1     1        0     0
## 737 16336.91   16342        6     4      1     0       0     1        0     0
## 738 14862.09   24021        6     4      1     0       1     1        0     0
## 739 15230.00   22576        6     4      1     1       0     1        0     0
## 740 49248.16    6685        8     4      1     0       1     0        1     0
## 741 35129.34   11975        8     4      1     1       1     0        1     0
## 742 44130.62   21341        8     4      1     0       1     0        1     0
## 743 31431.13   11013        6     4      1     1       1     0        1     0
## 744 48365.98    2616        8     4      1     1       1     0        1     0
## 745 43374.05   25199        8     4      1     1       1     0        1     0
## 746 36210.12   21778        6     4      1     0       1     0        1     0
## 747 39072.39   31587        8     4      1     0       1     0        1     0
## 748 35651.68   10555        8     4      1     1       1     0        1     0
## 749 30646.44   17094        6     4      1     1       1     0        1     0
## 750 38795.38   13973        6     4      1     1       1     0        1     0
## 751 35165.76   13449        8     4      1     1       1     0        1     0
## 752 35895.50   23056        6     4      1     1       1     0        1     0
## 753 45061.95   13829        8     4      1     1       1     0        1     0
## 754 28817.08   21039        6     4      1     0       1     0        1     0
## 755 14072.14   15233        4     4      0     1       1     0        0     1
## 756 13436.00   20530        4     4      0     1       1     0        0     1
## 757 17162.48   15903        6     4      0     1       1     0        0     1
## 758 10546.78   38866        4     4      1     1       1     0        0     1
## 759 13540.04   17343        4     4      0     1       1     0        0     1
## 760 12379.13   31199        4     4      1     1       1     0        0     1
## 761 14429.79    6114        4     4      1     1       1     0        0     1
## 762 13830.25   17594        4     4      0     1       1     0        0     1
## 763 12257.16   21492        4     4      1     1       1     0        0     1
## 764 18727.51   14054        6     4      1     1       1     0        0     1
## 765 15623.92   21272        6     4      1     1       1     0        0     1
## 766 16507.07   17451        6     4      0     1       1     0        0     1
## 767 11464.63   29410        4     4      1     1       1     0        0     1
## 768 16805.06   19498        6     4      1     0       0     0        0     1
## 769 15832.52   31202        6     4      1     1       1     0        0     1
## 770 16853.11   17959        6     4      1     0       0     0        0     0
## 771 16516.96   20751        6     4      1     1       1     0        0     0
## 772 15792.83   41566        6     4      1     1       1     0        0     0
## 773 18548.98   20870        6     4      1     0       0     0        0     0
## 774 17023.94   30404        6     4      1     0       0     0        0     0
## 775 15967.25   25598        6     4      1     0       0     0        0     0
## 776 12293.06   17139        4     4      1     0       1     0        0     0
## 777 11504.82   33962        4     4      0     0       0     0        0     0
## 778 11873.53   28398        4     4      0     0       0     0        0     0
## 779 14568.00   18511        4     4      0     1       1     0        0     0
## 780 13998.13   18257        4     4      0     1       1     0        0     0
## 781 13258.37   14938        4     4      1     1       1     0        0     0
## 782 11413.53   32619        4     4      1     0       1     0        0     0
## 783 15194.98   12412        4     4      0     0       1     0        0     0
## 784 19689.74   27077        6     4      1     1       0     0        0     0
## 785 22460.53    8928        6     4      1     0       1     0        0     0
## 786 19338.38   27966        6     4      1     0       1     0        0     0
## 787 26831.19    4695        6     4      1     1       0     1        0     0
## 788 25508.21   17480        6     4      1     1       0     1        0     0
## 789 23406.69   25387        6     4      1     0       1     1        0     0
## 790 17214.33   12610        4     4      1     1       1     0        0     0
## 791 14853.20   24270        4     4      0     0       0     0        0     0
## 792 14398.92   21688        4     4      0     0       0     0        0     0
## 793 20221.81   26223        6     2      1     1       1     0        0     1
## 794 20382.15   25240        6     2      1     1       1     0        0     1
## 795 22113.63   21992        6     2      1     1       0     0        0     1
## 796 25097.47   14461        6     4      1     1       1     0        0     1
## 797 22120.76   28242        6     4      1     1       1     0        0     1
## 798 23345.33   22964        6     4      1     1       1     0        0     1
## 799 11167.86    4716        4     4      1     1       0     0        0     1
## 801  9720.98   20836        4     4      1     1       0     0        0     1
## 802  9482.22   24842        4     4      1     0       0     0        0     1
## 803  9563.79   19273        4     4      1     1       0     0        0     1
## 804  9665.85   19565        4     4      0     1       1     0        0     1
##     Pontiac Saab Saturn convertible coupe hatchback sedan wagon velocidad
## 1         0    0      0           0     0         0     1     0  36469.49
## 2         0    0      0           0     1         0     0     0  34963.08
## 3         0    1      0           1     0         0     0     0  46900.74
## 4         0    1      0           1     0         0     0     0  49458.36
## 5         0    1      0           1     0         0     0     0  53685.84
## 6         0    1      0           1     0         0     0     0  48787.63
## 7         0    1      0           1     0         0     0     0  53722.93
## 8         0    1      0           1     0         0     0     0  48684.39
## 9         0    1      0           1     0         0     0     0  48548.93
## 10        0    1      0           0     0         0     1     0  43549.16
## 11        0    1      0           0     0         0     1     0  43196.61
## 12        0    1      0           0     0         0     1     0  41505.88
## 13        0    1      0           0     0         0     1     0  40472.47
## 14        0    1      0           0     0         0     1     0  38903.60
## 15        0    1      0           0     0         0     1     0  39996.30
## 16        0    1      0           0     0         0     1     0  44781.61
## 17        0    1      0           0     0         0     1     0  42966.48
## 18        0    1      0           0     0         0     1     0  45360.70
## 19        0    1      0           0     0         0     1     0  43840.93
## 20        0    1      0           0     0         0     1     0  49568.95
## 21        0    1      0           0     0         0     1     0  45731.95
## 22        0    1      0           0     0         0     1     0  42894.31
## 23        0    1      0           0     0         0     1     0  44435.46
## 24        0    1      0           0     0         0     0     1  44721.84
## 25        0    1      0           0     0         0     0     1  48259.15
## 26        0    1      0           0     0         0     0     1  46989.38
## 27        0    1      0           0     0         0     0     1  48132.64
## 28        0    1      0           0     0         0     0     1  47446.01
## 29        0    1      0           0     0         0     0     1  43118.11
## 30        0    1      0           0     0         0     0     1  47187.79
## 31        0    1      0           1     0         0     0     0  60163.03
## 32        0    1      0           1     0         0     0     0  55350.27
## 33        0    1      0           1     0         0     0     0  61677.92
## 34        0    1      0           1     0         0     0     0  56817.18
## 35        0    1      0           1     0         0     0     0  46313.73
## 36        0    1      0           1     0         0     0     0  53508.12
## 37        0    1      0           1     0         0     0     0  57261.10
## 38        0    1      0           0     0         0     1     0  48722.52
## 39        0    1      0           0     0         0     1     0  48477.45
## 40        0    1      0           0     0         0     1     0  51816.70
## 41        0    1      0           0     0         0     1     0  43628.45
## 42        0    1      0           0     0         0     1     0  48849.46
## 43        0    1      0           0     0         0     1     0  43865.15
## 44        0    1      0           0     0         0     1     0  45531.26
## 45        0    1      0           0     0         0     1     0  40092.18
## 46        0    1      0           0     0         0     1     0  53403.50
## 47        0    1      0           0     0         0     1     0  45871.16
## 48        0    1      0           0     0         0     1     0  56380.61
## 49        0    1      0           0     0         0     1     0  48029.68
## 50        0    1      0           0     0         0     1     0  48402.71
## 51        0    1      0           0     0         0     1     0  45150.20
## 52        0    1      0           0     0         0     1     0  45758.28
## 53        0    1      0           0     0         0     1     0  52699.89
## 54        0    1      0           0     0         0     1     0  49894.15
## 55        0    1      0           0     0         0     0     1  46395.92
## 56        0    1      0           0     0         0     0     1  51256.59
## 57        0    1      0           0     0         0     0     1  50782.00
## 58        0    1      0           0     0         0     0     1  46152.98
## 59        0    1      0           0     0         0     0     1  48218.05
## 60        0    1      0           0     0         0     0     1  49206.19
## 61        0    1      0           0     0         0     0     1  48142.62
## 62        1    0      0           0     0         0     0     1  27882.37
## 63        1    0      0           0     0         0     0     1  26380.30
## 64        1    0      0           0     0         0     0     1  24953.39
## 65        1    0      0           0     0         0     0     1  24619.53
## 66        1    0      0           0     0         0     0     1  25876.16
## 67        1    0      0           0     0         0     0     1  26636.66
## 68        1    0      0           0     0         0     0     1  23410.98
## 69        0    0      0           1     0         0     0     0  60010.85
## 70        0    0      0           1     0         0     0     0  75208.99
## 71        0    0      0           1     0         0     0     0  75744.26
## 72        0    0      0           1     0         0     0     0  66580.91
## 73        0    0      0           1     0         0     0     0  63645.80
## 74        0    0      0           1     0         0     0     0  68836.65
## 75        0    0      0           1     0         0     0     0  59499.01
## 76        0    0      0           0     1         0     0     0  18029.76
## 77        0    0      0           0     1         0     0     0  20988.90
## 78        0    0      0           0     1         0     0     0  19646.70
## 79        0    0      0           0     1         0     0     0  18871.20
## 80        0    0      0           0     1         0     0     0  20934.35
## 81        0    0      0           0     1         0     0     0  17943.61
## 82        0    0      0           0     1         0     0     0  17363.20
## 83        0    0      0           0     1         0     0     0  20229.72
## 84        0    0      0           0     1         0     0     0  22569.39
## 85        0    0      0           0     1         0     0     0  20449.83
## 86        0    0      0           0     1         0     0     0  19250.40
## 87        0    0      0           0     1         0     0     0  20617.20
## 88        0    0      0           0     1         0     0     0  20672.34
## 89        0    0      0           0     1         0     0     0  20757.25
## 90        0    0      0           0     1         0     0     0  63913.08
## 91        0    0      0           0     1         0     0     0  64174.08
## 92        0    0      0           0     1         0     0     0  50190.29
## 93        0    0      0           0     1         0     0     0  63353.36
## 94        0    0      0           0     1         0     0     0  55196.28
## 95        0    0      0           0     1         0     0     0  62749.42
## 96        0    0      0           0     1         0     0     0  56747.90
## 97        1    0      0           0     1         0     0     0  44049.37
## 98        1    0      0           0     1         0     0     0  49929.78
## 99        1    0      0           0     1         0     0     0  45870.11
## 100       1    0      0           0     1         0     0     0  51852.50
## 101       1    0      0           0     1         0     0     0  47629.90
## 102       1    0      0           0     1         0     0     0  47740.80
## 103       1    0      0           0     1         0     0     0  44335.31
## 104       1    0      0           0     1         0     0     0  20059.01
## 105       1    0      0           0     1         0     0     0  21179.20
## 106       1    0      0           0     1         0     0     0  20651.88
## 107       1    0      0           0     1         0     0     0  19728.76
## 108       1    0      0           0     1         0     0     0  20061.33
## 109       1    0      0           0     1         0     0     0  20644.75
## 110       1    0      0           0     1         0     0     0  19156.22
## 111       0    0      0           0     0         0     1     0  28567.33
## 112       0    0      0           0     0         0     1     0  29201.81
## 113       0    0      0           0     0         0     1     0  28602.88
## 114       0    0      0           0     0         0     1     0  29529.75
## 115       0    0      0           0     0         0     1     0  27993.02
## 116       0    0      0           0     0         0     1     0  32346.69
## 117       0    0      0           0     0         0     1     0  33310.39
## 118       0    0      0           0     0         0     1     0  32064.89
## 119       0    0      0           0     0         0     1     0  28659.57
## 120       0    0      0           0     0         0     1     0  31823.63
## 121       0    0      0           0     0         0     1     0  33052.92
## 122       0    0      0           0     0         0     1     0  31131.48
## 123       0    0      0           0     0         0     1     0  29842.82
## 124       0    0      0           0     0         0     1     0  33011.07
## 125       0    0      0           0     0         0     1     0  38279.80
## 126       0    0      0           0     0         0     1     0  35983.20
## 127       0    0      0           0     0         0     1     0  35237.88
## 128       0    0      0           0     0         0     1     0  36896.04
## 129       0    0      0           0     0         0     1     0  33889.86
## 130       0    0      0           0     0         0     1     0  31473.84
## 131       0    0      0           0     0         0     1     0  34090.99
## 132       0    0      0           0     0         0     1     0  34345.49
## 133       0    0      0           0     0         0     1     0  33773.79
## 134       0    0      0           0     0         0     1     0  34895.52
## 135       0    0      0           0     0         0     1     0  35082.43
## 136       0    0      0           0     0         0     1     0  37735.47
## 137       0    0      0           0     0         0     1     0  37040.75
## 138       0    0      0           0     0         0     1     0  37895.68
## 139       0    0      0           0     0         0     1     0  62121.18
## 140       0    0      0           0     0         0     1     0  58184.82
## 141       0    0      0           0     0         0     1     0  58061.70
## 142       0    0      0           0     0         0     1     0  64914.21
## 143       0    0      0           0     0         0     1     0  56872.15
## 144       0    0      0           0     0         0     1     0  63258.62
## 145       0    0      0           0     0         0     1     0  64054.51
## 146       0    0      0           0     0         0     1     0  52363.63
## 147       0    0      0           0     0         0     1     0  60367.01
## 148       0    0      0           0     0         0     1     0  53034.65
## 149       0    0      0           0     0         0     1     0  58331.04
## 150       0    0      0           0     0         0     1     0  65752.11
## 151       0    0      0           0     0         0     1     0  66591.64
## 152       0    0      0           0     0         0     1     0  59892.13
## 153       1    0      0           0     1         0     0     0  27685.32
## 154       1    0      0           0     1         0     0     0  28446.56
## 155       1    0      0           0     1         0     0     0  24548.77
## 156       1    0      0           0     1         0     0     0  23662.46
## 157       1    0      0           0     1         0     0     0  24235.37
## 158       1    0      0           0     1         0     0     0  27025.25
## 159       1    0      0           0     1         0     0     0  27587.33
## 160       1    0      0           0     0         0     1     0  30907.21
## 161       1    0      0           0     0         0     1     0  29820.14
## 162       1    0      0           0     0         0     1     0  33197.06
## 163       1    0      0           0     0         0     1     0  29222.65
## 164       1    0      0           0     0         0     1     0  31447.03
## 165       1    0      0           0     0         0     1     0  35204.63
## 166       1    0      0           0     0         0     1     0  35250.04
## 167       1    0      0           0     0         0     1     0  25408.53
## 168       1    0      0           0     0         0     1     0  27621.01
## 169       1    0      0           0     0         0     1     0  26665.50
## 170       1    0      0           0     0         0     1     0  26379.08
## 171       1    0      0           0     0         0     1     0  24875.95
## 172       1    0      0           0     0         0     1     0  26206.54
## 173       1    0      0           0     0         0     1     0  29378.50
## 174       1    0      0           0     0         0     0     1  25431.95
## 175       1    0      0           0     0         0     0     1  23168.60
## 176       1    0      0           0     0         0     0     1  23929.83
## 177       1    0      0           0     0         0     0     1  25055.88
## 178       1    0      0           0     0         0     0     1  26317.81
## 179       1    0      0           0     0         0     0     1  25315.11
## 180       1    0      0           0     0         0     0     1  25714.00
## 181       1    0      0           0     0         0     1     0  31618.39
## 182       1    0      0           0     0         0     1     0  32466.82
## 183       1    0      0           0     0         0     1     0  35567.20
## 184       1    0      0           0     0         0     1     0  30096.75
## 185       1    0      0           0     0         0     1     0  31298.95
## 186       1    0      0           0     0         0     1     0  34249.93
## 187       1    0      0           0     0         0     1     0  34168.02
## 188       1    0      0           0     0         0     1     0  37945.44
## 189       1    0      0           0     0         0     1     0  35778.30
## 190       1    0      0           0     0         0     1     0  35710.00
## 191       1    0      0           0     0         0     1     0  36250.98
## 192       1    0      0           0     0         0     1     0  37738.08
## 193       1    0      0           0     0         0     1     0  34446.08
## 194       1    0      0           0     0         0     1     0  34119.27
## 195       0    0      0           1     0         0     0     0 110346.80
## 196       0    0      0           1     0         0     0     0 113870.11
## 197       0    0      0           1     0         0     0     0  83689.25
## 198       0    0      0           1     0         0     0     0  97474.21
## 199       0    0      0           1     0         0     0     0 102858.39
## 200       0    0      0           1     0         0     0     0 111260.17
## 201       0    0      0           1     0         0     0     0 106819.30
## 202       0    0      1           0     0         0     1     0  27877.24
## 203       0    0      1           0     0         0     1     0  26565.61
## 204       0    0      1           0     0         0     1     0  26433.80
## 205       0    0      1           0     0         0     1     0  24331.54
## 206       0    0      1           0     0         0     1     0  28933.42
## 207       0    0      1           0     0         0     1     0  21864.01
## 208       0    0      1           0     0         0     1     0  25316.85
## 209       0    0      0           0     0         0     1     0  35258.17
## 210       0    0      0           0     0         0     1     0  33719.39
## 211       0    0      0           0     0         0     1     0  31262.94
## 212       0    0      0           0     0         0     1     0  30886.57
## 213       0    0      0           0     0         0     1     0  35335.37
## 214       0    0      0           0     0         0     1     0  32156.57
## 215       0    0      0           0     0         0     1     0  34836.12
## 216       0    1      0           1     0         0     0     0  51621.39
## 217       0    1      0           1     0         0     0     0  44524.57
## 218       0    1      0           1     0         0     0     0  56036.34
## 219       0    1      0           1     0         0     0     0  53978.37
## 220       0    1      0           1     0         0     0     0  51449.33
## 221       0    1      0           1     0         0     0     0  57328.39
## 222       0    1      0           1     0         0     0     0  52685.32
## 223       0    1      0           0     0         0     1     0  37417.06
## 224       0    1      0           0     0         0     1     0  47067.35
## 225       0    1      0           0     0         0     1     0  39914.41
## 226       0    1      0           0     0         0     1     0  35799.73
## 227       0    1      0           0     0         0     1     0  43090.32
## 228       0    1      0           0     0         0     1     0  41837.89
## 229       0    1      0           0     0         0     1     0  40716.43
## 230       0    1      0           0     0         0     0     1  50141.78
## 231       0    1      0           0     0         0     0     1  46855.40
## 232       0    1      0           0     0         0     0     1  48994.77
## 233       0    1      0           0     0         0     0     1  40078.34
## 234       0    1      0           0     0         0     0     1  50026.46
## 235       0    1      0           0     0         0     0     1  53117.76
## 236       0    1      0           0     0         0     0     1  50136.01
## 237       0    1      0           0     0         0     0     1  37544.80
## 238       0    1      0           0     0         0     0     1  37456.72
## 239       0    1      0           0     0         0     0     1  43904.57
## 240       0    1      0           0     0         0     0     1  41777.23
## 241       0    0      0           0     1         0     0     0  20327.31
## 242       0    0      0           0     1         0     0     0  19996.77
## 243       0    0      0           0     1         0     0     0  20197.13
## 244       0    0      0           0     1         0     0     0  19754.67
## 245       0    0      0           0     1         0     0     0  21639.62
## 246       0    0      0           0     1         0     0     0  19826.67
## 247       0    0      0           0     1         0     0     0  19690.18
## 248       0    0      0           0     1         0     0     0  20414.55
## 249       0    0      0           0     1         0     0     0  20202.25
## 250       0    0      0           0     1         0     0     0  22828.62
## 251       0    0      0           0     1         0     0     0  20952.20
## 252       0    0      0           0     1         0     0     0  25343.68
## 253       0    0      0           0     1         0     0     0  22046.51
## 254       0    0      0           0     1         0     0     0  21774.58
## 255       0    0      0           0     1         0     0     0  30863.11
## 256       0    0      0           0     1         0     0     0  26325.68
## 257       0    0      0           0     1         0     0     0  25423.18
## 258       0    0      0           0     1         0     0     0  28188.36
## 259       0    0      0           0     1         0     0     0  29032.85
## 260       0    0      0           0     1         0     0     0  26306.29
## 261       0    0      0           0     1         0     0     0  30255.87
## 262       0    0      0           0     0         1     0     0  16714.74
## 263       0    0      0           0     0         1     0     0  17730.45
## 264       0    0      0           0     0         1     0     0  19575.81
## 265       0    0      0           0     0         1     0     0  17858.70
## 266       0    0      0           0     0         1     0     0  17344.01
## 267       0    0      0           0     0         1     0     0  19547.44
## 268       0    0      0           0     0         1     0     0  18462.46
## 269       0    0      0           0     0         1     0     0  27077.57
## 270       0    0      0           0     0         1     0     0  24002.12
## 271       0    0      0           0     0         1     0     0  28648.36
## 272       0    0      0           0     0         1     0     0  26625.01
## 273       0    0      0           0     0         1     0     0  26946.96
## 274       0    0      0           0     0         1     0     0  25981.24
## 275       0    0      0           0     0         1     0     0  28793.84
## 276       0    0      0           0     0         0     1     0  19971.92
## 277       0    0      0           0     0         0     1     0  19818.45
## 278       0    0      0           0     0         0     1     0  20356.81
## 279       0    0      0           0     0         0     1     0  16883.79
## 280       0    0      0           0     0         0     1     0  18214.61
## 281       0    0      0           0     0         0     1     0  18829.54
## 282       0    0      0           0     0         0     1     0  18048.86
## 283       0    0      0           0     0         0     1     0  23971.19
## 284       0    0      0           0     0         0     1     0  21191.40
## 285       0    0      0           0     0         0     1     0  21293.14
## 286       0    0      0           0     0         0     1     0  20235.77
## 287       0    0      0           0     0         0     1     0  18232.23
## 288       0    0      0           0     0         0     1     0  19929.19
## 289       0    0      0           0     0         0     1     0  23622.17
## 290       0    0      0           0     0         0     1     0  21798.65
## 291       0    0      0           0     0         0     1     0  23021.29
## 292       0    0      0           0     0         0     1     0  23894.04
## 293       0    0      0           0     0         0     1     0  23486.57
## 294       0    0      0           0     0         0     1     0  22120.23
## 295       0    0      0           0     0         0     1     0  22030.27
## 296       0    0      0           0     0         0     1     0  20505.00
## 297       0    0      0           0     0         0     1     0  30699.39
## 298       0    0      0           0     0         0     1     0  28710.43
## 299       0    0      0           0     0         0     1     0  35014.65
## 300       0    0      0           0     0         0     1     0  28629.24
## 301       0    0      0           0     0         0     1     0  29816.71
## 302       0    0      0           0     0         0     1     0  27832.34
## 303       0    0      0           0     0         0     1     0  30437.55
## 304       0    0      0           0     0         0     1     0  26510.61
## 305       0    0      0           0     0         0     1     0  26233.11
## 306       0    0      0           0     0         0     1     0  24362.94
## 307       0    0      0           0     0         0     1     0  27614.66
## 308       0    0      0           0     0         0     1     0  25143.15
## 309       0    0      0           0     0         0     1     0  30842.51
## 310       0    0      0           0     0         0     1     0  27348.89
## 311       0    0      0           0     1         0     0     0  19682.48
## 312       0    0      0           0     1         0     0     0  20128.89
## 313       0    0      0           0     1         0     0     0  21148.51
## 314       0    0      0           0     1         0     0     0  24227.00
## 315       0    0      0           0     1         0     0     0  21877.61
## 316       0    0      0           0     1         0     0     0  21669.54
## 317       0    0      0           0     1         0     0     0  23564.58
## 318       0    0      0           0     0         0     1     0  22942.45
## 319       0    0      0           0     0         0     1     0  21418.53
## 320       0    0      0           0     0         0     1     0  22765.63
## 321       0    0      0           0     0         0     1     0  20832.90
## 322       0    0      0           0     0         0     1     0  20673.77
## 323       0    0      0           0     0         0     1     0  22960.41
## 324       0    0      0           0     0         0     1     0  22028.74
## 325       0    0      0           0     1         0     0     0  32215.86
## 326       0    0      0           0     1         0     0     0  33537.43
## 327       0    0      0           0     1         0     0     0  28303.47
## 328       0    0      0           0     1         0     0     0  33275.13
## 329       0    0      0           0     1         0     0     0  36023.82
## 330       0    0      0           0     1         0     0     0  34995.30
## 331       0    0      0           0     0         1     0     0  17992.42
## 332       0    0      0           0     0         1     0     0  17752.92
## 333       0    0      0           0     0         1     0     0  17577.21
## 334       0    0      0           0     0         1     0     0  18254.90
## 335       0    0      0           0     0         1     0     0  17725.14
## 336       0    0      0           0     0         1     0     0  17815.57
## 337       0    0      0           0     0         1     0     0  18332.41
## 338       0    0      0           0     0         1     0     0  29407.62
## 339       0    0      0           0     0         1     0     0  31337.16
## 340       0    0      0           0     0         1     0     0  28426.25
## 341       0    0      0           0     0         1     0     0  28984.10
## 342       0    0      0           0     0         1     0     0  27340.07
## 343       0    0      0           0     0         1     0     0  28250.08
## 344       0    0      0           0     0         1     0     0  29469.98
## 345       0    0      0           0     0         0     1     0  19180.94
## 346       0    0      0           0     0         0     1     0  15963.19
## 347       0    0      0           0     0         0     1     0  17389.20
## 348       0    0      0           0     0         0     1     0  18190.29
## 349       0    0      0           0     0         0     1     0  17332.85
## 350       0    0      0           0     0         0     1     0  17496.84
## 351       0    0      0           0     0         0     1     0  19970.08
## 352       0    0      0           0     0         0     1     0  23177.67
## 353       0    0      0           0     0         0     1     0  24402.80
## 354       0    0      0           0     0         0     1     0  23349.61
## 355       0    0      0           0     0         0     1     0  25937.59
## 356       0    0      0           0     0         0     1     0  23974.51
## 357       0    0      0           0     0         0     1     0  24276.71
## 358       0    0      0           0     0         0     1     0  23203.84
## 359       0    0      0           0     0         0     1     0  26398.52
## 360       0    0      0           0     0         0     1     0  27867.62
## 361       0    0      0           0     0         0     1     0  27551.15
## 362       0    0      0           0     0         0     1     0  27134.99
## 363       0    0      0           0     0         0     1     0  26613.35
## 364       0    0      0           0     0         0     1     0  31296.78
## 365       0    0      0           0     0         0     1     0  26224.65
## 366       0    0      0           0     0         1     0     0  27133.74
## 367       0    0      0           0     0         1     0     0  29491.01
## 368       0    0      0           0     0         1     0     0  30537.23
## 369       0    0      0           0     0         1     0     0  25793.47
## 370       0    0      0           0     0         1     0     0  26179.39
## 371       0    0      0           0     0         1     0     0  31513.00
## 372       0    0      0           0     0         1     0     0  29240.84
## 373       0    0      1           0     1         0     0     0  23950.24
## 374       0    0      1           0     1         0     0     0  22079.02
## 375       0    0      1           0     1         0     0     0  22656.34
## 376       0    0      1           0     1         0     0     0  23193.68
## 377       0    0      1           0     1         0     0     0  22516.44
## 378       0    0      1           0     1         0     0     0  24193.56
## 379       0    0      1           0     1         0     0     0  23771.67
## 380       0    0      1           0     1         0     0     0  21755.54
## 381       0    0      1           0     1         0     0     0  22249.46
## 382       0    0      1           0     1         0     0     0  26161.93
## 383       0    0      1           0     1         0     0     0  22320.28
## 384       0    0      1           0     1         0     0     0  27225.12
## 385       0    0      1           0     1         0     0     0  22226.95
## 386       0    0      1           0     1         0     0     0  18570.34
## 387       1    0      0           0     0         0     1     0  29879.25
## 388       1    0      0           0     0         0     1     0  30345.88
## 389       1    0      0           0     0         0     1     0  31675.23
## 390       1    0      0           0     0         0     1     0  32700.15
## 391       1    0      0           0     0         0     1     0  28718.36
## 392       1    0      0           0     0         0     1     0  30060.04
## 393       1    0      0           0     0         0     1     0  28594.98
## 394       1    0      0           0     0         0     1     0  22468.27
## 395       1    0      0           0     0         0     1     0  24264.42
## 396       1    0      0           0     0         0     1     0  20976.34
## 397       1    0      0           0     0         0     1     0  25112.49
## 398       1    0      0           0     0         0     1     0  25715.77
## 399       1    0      0           0     0         0     1     0  23885.80
## 400       1    0      0           0     0         0     1     0  26360.86
## 401       0    0      0           0     0         0     1     0  25281.31
## 402       0    0      0           0     0         0     1     0  24217.52
## 403       0    0      0           0     0         0     1     0  24615.00
## 404       0    0      0           0     0         0     1     0  26295.40
## 405       0    0      0           0     0         0     1     0  28231.23
## 406       0    0      0           0     0         0     1     0  26101.76
## 407       0    0      0           0     0         0     1     0  27864.40
## 408       0    0      0           0     0         0     1     0  35134.98
## 409       0    0      0           0     0         0     1     0  37692.05
## 410       0    0      0           0     0         0     1     0  40392.41
## 411       0    0      0           0     0         0     1     0  37139.82
## 412       0    0      0           0     0         0     1     0  36106.02
## 413       0    0      0           0     0         0     1     0  41183.16
## 414       0    0      0           0     0         0     1     0  75233.23
## 415       0    0      0           0     0         0     1     0  82324.62
## 416       0    0      0           0     0         0     1     0  68683.07
## 417       0    0      0           0     0         0     1     0  70638.22
## 418       0    0      0           0     0         0     1     0  71295.11
## 419       0    0      0           0     0         0     1     0  65370.18
## 420       0    0      0           0     0         0     1     0  70947.92
## 421       0    0      0           0     0         0     1     0  49555.26
## 422       0    0      0           0     0         0     1     0  47113.97
## 423       0    0      0           0     0         0     1     0  48912.48
## 424       0    0      0           0     0         0     1     0  49820.69
## 425       0    0      0           0     0         0     1     0  49538.15
## 426       0    0      0           0     0         0     1     0  53781.11
## 427       0    0      0           0     0         0     1     0  45126.30
## 428       0    0      0           0     0         0     1     0  50182.21
## 429       0    0      0           0     0         0     1     0  53462.56
## 430       0    0      0           0     0         0     1     0  52305.79
## 431       0    0      0           0     0         0     1     0  52319.04
## 432       0    0      0           0     0         0     1     0  49985.00
## 433       0    0      0           0     0         0     1     0  50102.53
## 434       0    0      0           0     0         0     1     0  57479.07
## 435       0    0      0           0     0         0     1     0  58956.23
## 436       0    0      0           0     0         0     1     0  61633.91
## 437       0    0      0           0     0         0     1     0  68785.94
## 438       0    0      0           0     0         0     1     0  65929.49
## 439       0    0      0           0     0         0     1     0  51560.81
## 440       0    0      0           0     0         0     1     0  59856.29
## 441       0    0      0           0     0         0     1     0  56285.92
## 442       0    0      0           0     0         0     1     0  71142.60
## 443       0    0      0           0     0         0     1     0  68200.85
## 444       0    0      0           0     0         0     1     0  77748.09
## 445       0    0      0           0     0         0     1     0  66814.35
## 446       0    0      0           0     0         0     1     0  67064.04
## 447       0    0      0           0     0         0     1     0  66069.30
## 448       0    0      0           0     0         0     1     0  61490.74
## 449       0    0      0           0     0         0     1     0  21038.74
## 450       0    0      0           0     0         0     1     0  18570.34
## 451       0    0      0           0     0         0     1     0  18827.80
## 452       0    0      0           0     0         0     1     0  18626.86
## 453       0    0      0           0     0         0     1     0  19703.33
## 454       0    0      0           0     0         0     1     0  18784.07
## 455       0    0      0           0     0         0     1     0  22629.22
## 456       0    0      0           0     0         0     1     0  25030.51
## 457       0    0      0           0     0         0     1     0  18639.31
## 458       0    0      0           0     0         0     1     0  21182.13
## 459       0    0      0           0     0         0     1     0  22656.34
## 460       0    0      0           0     0         0     1     0  24215.85
## 461       0    0      0           0     0         0     1     0  20892.46
## 462       0    0      0           0     0         0     1     0  22884.93
## 463       0    0      0           0     0         0     1     0  21178.72
## 464       0    0      0           0     0         0     1     0  20404.67
## 465       0    0      0           0     0         0     1     0  22522.67
## 466       0    0      0           0     0         0     1     0  22844.39
## 467       0    0      0           0     0         0     1     0  23651.01
## 468       0    0      0           0     0         0     1     0  23468.74
## 469       0    0      0           0     0         0     1     0  20110.35
## 470       0    0      0           0     0         0     1     0  32221.06
## 471       0    0      0           0     0         0     1     0  25032.24
## 472       0    0      0           0     0         0     1     0  26786.12
## 473       0    0      0           0     0         0     1     0  30312.36
## 474       0    0      0           0     0         0     1     0  27607.67
## 475       0    0      0           0     0         0     1     0  25672.00
## 476       0    0      0           0     0         0     1     0  26568.05
## 477       0    0      0           0     0         0     1     0  26790.43
## 478       0    0      0           0     0         0     1     0  27710.85
## 479       0    0      0           0     0         0     1     0  27503.61
## 480       0    0      0           0     0         0     1     0  28104.11
## 481       0    0      0           0     0         0     1     0  25235.95
## 482       0    0      0           0     0         0     1     0  26960.60
## 483       0    0      0           0     0         0     1     0  25209.81
## 484       1    0      0           0     0         0     1     0  29967.44
## 485       1    0      0           0     0         0     1     0  32915.44
## 486       1    0      0           0     0         0     1     0  33277.42
## 487       1    0      0           0     0         0     1     0  34412.78
## 488       1    0      0           0     0         0     1     0  31052.01
## 489       1    0      0           0     0         0     1     0  29036.19
## 490       1    0      0           0     0         0     1     0  26098.75
## 491       1    0      0           0     0         0     1     0  25305.78
## 492       1    0      0           0     0         0     1     0  31900.67
## 493       1    0      0           0     0         0     1     0  25357.11
## 494       1    0      0           0     0         0     1     0  25715.77
## 495       1    0      0           0     0         0     1     0  26161.93
## 496       1    0      0           0     0         0     1     0  25816.65
## 497       1    0      0           0     0         0     1     0  31490.51
## 498       0    0      1           0     0         0     1     0  24317.54
## 499       0    0      1           0     0         0     1     0  19370.46
## 500       0    0      1           0     0         0     1     0  19471.51
## 501       0    0      1           0     0         0     1     0  19769.69
## 502       0    0      1           0     0         0     1     0  22857.56
## 503       0    0      1           0     0         0     1     0  22719.02
## 504       0    0      1           0     0         0     1     0  20586.37
## 505       0    0      1           0     0         0     1     0  23720.28
## 506       0    0      1           0     0         0     1     0  20865.54
## 507       0    0      1           0     0         0     1     0  19976.05
## 508       0    0      1           0     0         0     1     0  16999.65
## 509       0    0      1           0     0         0     1     0  19849.04
## 510       0    0      1           0     0         0     1     0  19503.82
## 511       0    0      1           0     0         0     1     0  18542.14
## 512       0    0      1           0     0         0     1     0  19482.72
## 513       0    0      1           0     0         0     1     0  19573.10
## 514       0    0      1           0     0         0     1     0  21119.32
## 515       0    0      1           0     0         0     1     0  21717.00
## 516       0    0      1           0     0         0     1     0  21201.65
## 517       0    0      1           0     0         0     1     0  21270.60
## 518       0    0      1           0     0         0     1     0  20268.41
## 519       0    0      1           0     0         0     1     0  18797.05
## 520       1    0      0           0     0         0     1     0  33524.29
## 521       1    0      0           0     0         0     1     0  34774.40
## 522       1    0      0           0     0         0     1     0  35413.57
## 523       1    0      0           0     0         0     1     0  30764.49
## 524       1    0      0           0     0         0     1     0  37332.73
## 525       1    0      0           0     0         0     1     0  32363.81
## 526       1    0      0           0     0         0     1     0  37179.17
## 527       0    0      0           0     0         0     1     0  43101.23
## 528       0    0      0           0     0         0     1     0  34660.09
## 529       0    0      0           0     0         0     1     0  42149.23
## 530       0    0      0           0     0         0     1     0  41940.13
## 531       0    0      0           0     0         0     1     0  37271.74
## 532       0    0      0           0     0         0     1     0  42329.16
## 533       0    0      0           0     0         0     1     0  37575.07
## 534       1    0      0           0     0         0     0     1  26886.83
## 535       1    0      0           0     0         0     0     1  27242.67
## 536       1    0      0           0     0         0     0     1  23993.84
## 537       1    0      0           0     0         0     0     1  26359.66
## 538       1    0      0           0     0         0     0     1  25141.41
## 539       1    0      0           0     0         0     0     1  28031.72
## 540       1    0      0           0     0         0     0     1  25463.01
## 541       0    0      0           0     1         0     0     0  37938.46
## 542       0    0      0           0     1         0     0     0  33829.83
## 543       0    0      0           0     1         0     0     0  37864.28
## 544       0    0      0           0     1         0     0     0  32264.11
## 545       0    0      0           0     1         0     0     0  36162.61
## 546       0    0      0           0     1         0     0     0  33183.30
## 547       0    0      0           0     1         0     0     0  34641.74
## 548       0    0      0           0     0         0     1     0  44601.53
## 549       0    0      0           0     0         0     1     0  41760.88
## 550       0    0      0           0     0         0     1     0  35509.10
## 551       0    0      0           0     0         0     1     0  37258.88
## 552       0    0      0           0     0         0     1     0  32661.02
## 553       0    0      0           0     0         0     1     0  36845.10
## 554       0    0      0           0     0         0     1     0  39926.36
## 555       0    0      0           0     0         1     0     0  16663.24
## 556       0    0      0           0     0         1     0     0  16556.93
## 557       0    0      0           0     0         1     0     0  17537.18
## 558       0    0      0           0     0         1     0     0  15753.96
## 559       0    0      0           0     0         1     0     0  16264.09
## 560       0    0      0           0     0         1     0     0  15298.53
## 561       0    0      0           0     0         1     0     0  14839.52
## 562       0    0      0           0     0         0     1     0  14112.36
## 563       0    0      0           0     0         0     1     0  14276.44
## 564       0    0      0           0     0         0     1     0  16600.45
## 565       0    0      0           0     0         0     1     0  15536.73
## 566       0    0      0           0     0         0     1     0  14551.57
## 567       0    0      0           0     0         0     1     0  17656.31
## 568       0    0      0           0     0         0     1     0  13903.04
## 569       0    1      0           1     0         0     0     0  47656.23
## 570       0    1      0           1     0         0     0     0  54053.00
## 571       0    1      0           1     0         0     0     0  59688.37
## 572       0    1      0           0     0         0     1     0  39276.23
## 573       0    1      0           0     0         0     1     0  44584.06
## 574       0    1      0           0     0         0     1     0  41228.70
## 575       0    1      0           0     0         0     1     0  38195.28
## 576       0    1      0           0     0         0     1     0  45390.99
## 577       0    1      0           0     0         0     1     0  43910.63
## 578       0    1      0           0     0         0     0     1  49825.27
## 579       0    1      0           0     0         0     0     1  45590.02
## 580       0    1      0           0     0         0     0     1  41862.93
## 581       0    1      0           1     0         0     0     0  54692.74
## 582       0    1      0           1     0         0     0     0  61424.22
## 583       0    1      0           1     0         0     0     0  58481.66
## 584       0    1      0           0     0         0     1     0  40663.97
## 585       0    1      0           0     0         0     1     0  51584.56
## 586       0    1      0           0     0         0     1     0  43114.13
## 587       0    1      0           0     0         0     0     1  48718.03
## 588       0    1      0           0     0         0     0     1  43380.02
## 589       0    1      0           0     0         0     0     1  52544.80
## 590       1    0      0           0     0         0     0     1  25921.48
## 591       1    0      0           0     0         0     0     1  24420.80
## 592       1    0      0           0     0         0     0     1  25804.16
## 593       0    0      0           1     0         0     0     0  62482.69
## 594       0    0      0           1     0         0     0     0  72017.78
## 595       0    0      0           1     0         0     0     0  63877.77
## 596       0    0      0           0     1         0     0     0  21140.24
## 597       0    0      0           0     1         0     0     0  19386.07
## 598       0    0      0           0     1         0     0     0  62912.90
## 599       0    0      0           0     1         0     0     0  55907.45
## 600       0    0      0           0     1         0     0     0  57253.20
## 601       0    0      0           0     1         0     0     0  21093.55
## 602       0    0      0           0     1         0     0     0  20096.00
## 603       0    0      0           0     1         0     0     0  18571.62
## 604       0    0      0           0     1         0     0     0  20067.80
## 605       1    0      0           0     1         0     0     0  44137.70
## 606       1    0      0           0     1         0     0     0  41081.82
## 607       1    0      0           0     1         0     0     0  20648.08
## 608       1    0      0           0     1         0     0     0  19649.42
## 609       1    0      0           0     1         0     0     0  52179.47
## 610       1    0      0           0     1         0     0     0  20725.25
## 611       0    0      0           0     0         0     1     0  30622.43
## 612       0    0      0           0     0         0     1     0  28879.51
## 613       0    0      0           0     0         0     1     0  28398.14
## 614       0    0      0           0     0         0     1     0  34336.79
## 615       0    0      0           0     0         0     1     0  28918.10
## 616       0    0      0           0     0         0     1     0  30746.79
## 617       0    0      0           0     0         0     1     0  34536.60
## 618       0    0      0           0     0         0     1     0  34235.74
## 619       0    0      0           0     0         0     1     0  32840.50
## 620       0    0      0           0     0         0     1     0  35775.83
## 621       0    0      0           0     0         0     1     0  33638.73
## 622       0    0      0           0     0         0     1     0  36411.59
## 623       0    0      0           0     0         0     1     0  57721.78
## 624       0    0      0           0     0         0     1     0  61872.80
## 625       0    0      0           0     0         0     1     0  55821.27
## 626       0    0      0           0     0         0     1     0  68912.77
## 627       0    0      0           0     0         0     1     0  58472.23
## 628       0    0      0           0     0         0     1     0  66657.61
## 629       1    0      0           0     1         0     0     0  25099.18
## 630       1    0      0           0     1         0     0     0  27939.57
## 631       1    0      0           0     1         0     0     0  25097.46
## 632       1    0      0           0     0         0     1     0  27511.85
## 633       1    0      0           0     0         0     1     0  24080.74
## 634       1    0      0           0     0         0     1     0  27355.18
## 635       1    0      0           0     0         0     1     0  35574.57
## 636       1    0      0           0     0         0     1     0  36591.45
## 637       1    0      0           0     0         0     1     0  35906.22
## 638       1    0      0           0     0         0     0     1  24280.06
## 639       1    0      0           0     0         0     0     1  25089.37
## 640       1    0      0           0     0         0     0     1  28651.66
## 641       1    0      0           0     0         0     1     0  34279.13
## 642       1    0      0           0     0         0     1     0  31258.62
## 643       1    0      0           0     0         0     1     0  32117.35
## 644       1    0      0           0     0         0     1     0  40961.86
## 645       1    0      0           0     0         0     1     0  35027.55
## 646       1    0      0           0     0         0     1     0  35377.71
## 647       0    0      0           1     0         0     0     0  89542.61
## 648       0    0      0           1     0         0     0     0 105060.56
## 649       0    0      0           1     0         0     0     0  91981.33
## 650       0    0      1           0     0         0     1     0  29758.40
## 651       0    0      1           0     0         0     1     0  26032.73
## 652       0    0      1           0     0         0     1     0  29248.24
## 653       0    0      0           0     0         0     1     0  34700.82
## 654       0    0      0           0     0         0     1     0  31610.38
## 655       0    0      0           0     0         0     1     0  34722.40
## 656       0    1      0           1     0         0     0     0  55289.12
## 657       0    1      0           1     0         0     0     0  53571.00
## 658       0    1      0           1     0         0     0     0  51451.70
## 659       0    1      0           0     0         0     1     0  40067.27
## 660       0    1      0           0     0         0     1     0  38725.74
## 661       0    1      0           0     0         0     1     0  42386.71
## 662       0    1      0           0     0         0     0     1  41593.91
## 663       0    1      0           0     0         0     0     1  49344.61
## 664       0    1      0           0     0         0     0     1  48798.86
## 665       0    0      0           0     1         0     0     0  25163.43
## 666       0    0      0           0     1         0     0     0  28461.63
## 667       0    0      0           0     1         0     0     0  23471.44
## 668       0    0      0           0     1         0     0     0  24950.53
## 669       0    0      0           0     1         0     0     0  22018.60
## 670       0    0      0           0     1         0     0     0  30434.04
## 671       0    0      0           0     1         0     0     0  19839.45
## 672       0    0      0           0     1         0     0     0  23527.17
## 673       0    0      0           0     1         0     0     0  21420.51
## 674       0    0      0           0     0         1     0     0  15977.90
## 675       0    0      0           0     0         1     0     0  17923.38
## 676       0    0      0           0     0         1     0     0  17775.42
## 677       0    0      0           0     0         1     0     0  30498.59
## 678       0    0      0           0     0         1     0     0  26914.70
## 679       0    0      0           0     0         1     0     0  30509.83
## 680       0    0      0           0     0         0     1     0  18596.44
## 681       0    0      0           0     0         0     1     0  29102.47
## 682       0    0      0           0     0         0     1     0  17832.40
## 683       0    0      0           0     0         0     1     0  21679.53
## 684       0    0      0           0     0         0     1     0  22849.65
## 685       0    0      0           0     0         0     1     0  31237.02
## 686       0    0      0           0     0         0     1     0  31427.49
## 687       0    0      0           0     0         0     1     0  24141.80
## 688       0    0      0           0     0         0     1     0  16019.52
## 689       0    0      0           0     0         0     1     0  30257.27
## 690       0    0      0           0     0         0     1     0  24347.80
## 691       0    0      0           0     0         0     1     0  24136.79
## 692       0    0      0           0     0         0     1     0  24515.44
## 693       0    0      0           0     0         0     1     0  28096.34
## 694       0    0      0           0     0         0     1     0  16326.75
## 695       0    0      0           0     1         0     0     0  23171.27
## 696       0    0      0           0     1         0     0     0  20493.20
## 697       0    0      0           0     1         0     0     0  22014.03
## 698       0    0      0           0     0         0     1     0  22973.64
## 699       0    0      0           0     0         0     1     0  22888.63
## 700       0    0      0           0     0         0     1     0  22149.28
## 701       0    0      0           0     1         0     0     0  33051.39
## 702       0    0      0           0     1         0     0     0  34172.73
## 703       0    0      0           0     1         0     0     0  30379.44
## 704       0    0      0           0     0         1     0     0  17887.91
## 705       0    0      0           0     0         1     0     0  18101.71
## 706       0    0      0           0     0         1     0     0  18338.33
## 707       0    0      0           0     0         1     0     0  27544.17
## 708       0    0      0           0     0         1     0     0  28976.09
## 709       0    0      0           0     0         1     0     0  27042.05
## 710       0    0      0           0     0         0     1     0  27862.48
## 711       0    0      0           0     0         0     1     0  17976.28
## 712       0    0      0           0     0         0     1     0  26439.29
## 713       0    0      0           0     0         0     1     0  22839.66
## 714       0    0      0           0     0         0     1     0  17577.21
## 715       0    0      0           0     0         0     1     0  22813.91
## 716       0    0      0           0     0         0     1     0  18692.60
## 717       0    0      0           0     0         0     1     0  26299.63
## 718       0    0      0           0     0         0     1     0  26898.60
## 719       0    0      0           0     0         1     0     0  27638.83
## 720       0    0      0           0     0         1     0     0  28946.07
## 721       0    0      0           0     0         1     0     0  26484.98
## 722       0    0      1           0     1         0     0     0  21887.17
## 723       0    0      1           0     1         0     0     0  24775.91
## 724       0    0      1           0     1         0     0     0  24586.11
## 725       0    0      1           0     1         0     0     0  20974.41
## 726       0    0      1           0     1         0     0     0  23661.91
## 727       0    0      1           0     1         0     0     0  25168.64
## 728       1    0      0           0     0         0     1     0  24666.62
## 729       1    0      0           0     0         0     1     0  32391.39
## 730       1    0      0           0     0         0     1     0  25501.73
## 731       1    0      0           0     0         0     1     0  21183.59
## 732       1    0      0           0     0         0     1     0  29069.64
## 733       1    0      0           0     0         0     1     0  31786.28
## 734       0    0      0           0     0         0     1     0  37808.52
## 735       0    0      0           0     0         0     1     0  35209.49
## 736       0    0      0           0     0         0     1     0  34919.63
## 737       0    0      0           0     0         0     1     0  26291.76
## 738       0    0      0           0     0         0     1     0  23918.26
## 739       0    0      0           0     0         0     1     0  24510.36
## 740       0    0      0           0     0         0     1     0  79257.38
## 741       0    0      0           0     0         0     1     0  56535.30
## 742       0    0      0           0     0         0     1     0  71021.48
## 743       0    0      0           0     0         0     1     0  50583.60
## 744       0    0      0           0     0         0     1     0  77837.65
## 745       0    0      0           0     0         0     1     0  69803.90
## 746       0    0      0           0     0         0     1     0  58274.65
## 747       0    0      0           0     0         0     1     0  62881.04
## 748       0    0      0           0     0         0     1     0  57375.93
## 749       0    0      0           0     0         0     1     0  49320.76
## 750       0    0      0           0     0         0     1     0  62435.23
## 751       0    0      0           0     0         0     1     0  56593.91
## 752       0    0      0           0     0         0     1     0  57768.32
## 753       0    0      0           0     0         0     1     0  72520.32
## 754       0    0      0           0     0         0     1     0  46376.68
## 755       0    0      0           0     0         0     1     0  22646.96
## 756       0    0      0           0     0         0     1     0  21623.19
## 757       0    0      0           0     0         0     1     0  27620.39
## 758       0    0      0           0     0         0     1     0  16973.43
## 759       0    0      0           0     0         0     1     0  21790.62
## 760       0    0      0           0     0         0     1     0  19922.32
## 761       0    0      0           0     0         0     1     0  23222.54
## 762       0    0      0           0     0         0     1     0  22257.67
## 763       0    0      0           0     0         0     1     0  19726.02
## 764       0    0      0           0     0         0     1     0  30139.06
## 765       0    0      0           0     0         0     1     0  25144.31
## 766       0    0      0           0     0         0     1     0  26565.61
## 767       0    0      0           0     0         0     1     0  18450.57
## 768       0    0      0           0     0         0     1     0  27045.17
## 769       0    0      0           0     0         0     1     0  25480.02
## 770       1    0      0           0     0         0     1     0  27122.50
## 771       1    0      0           0     0         0     1     0  26581.52
## 772       1    0      0           0     0         0     1     0  25416.14
## 773       1    0      0           0     0         0     1     0  29851.75
## 774       1    0      0           0     0         0     1     0  27397.43
## 775       1    0      0           0     0         0     1     0  25696.85
## 776       0    0      1           0     0         0     1     0  19783.80
## 777       0    0      1           0     0         0     1     0  18515.25
## 778       0    0      1           0     0         0     1     0  19108.63
## 779       0    0      1           0     0         0     1     0  23444.97
## 780       0    0      1           0     0         0     1     0  22527.85
## 781       0    0      1           0     0         0     1     0  21337.32
## 782       0    0      1           0     0         0     1     0  18368.33
## 783       0    0      1           0     0         0     1     0  24454.00
## 784       1    0      0           0     0         0     1     0  31687.63
## 785       1    0      0           0     0         0     1     0  36146.79
## 786       1    0      0           0     0         0     1     0  31122.17
## 787       0    0      0           0     0         0     1     0  43180.70
## 788       0    0      0           0     0         0     1     0  41051.56
## 789       0    0      0           0     0         0     1     0  37669.49
## 790       1    0      0           0     0         0     0     1  27703.83
## 791       1    0      0           0     0         0     0     1  23903.95
## 792       1    0      0           0     0         0     0     1  23172.86
## 793       0    0      0           0     1         0     0     0  32543.91
## 794       0    0      0           0     1         0     0     0  32801.95
## 795       0    0      0           0     1         0     0     0  35588.51
## 796       0    0      0           0     0         0     1     0  40390.54
## 797       0    0      0           0     0         0     1     0  35599.98
## 798       0    0      0           0     0         0     1     0  37570.74
## 799       0    0      0           0     0         1     0     0  17972.96
## 801       0    0      0           0     0         1     0     0  15644.43
## 802       0    0      0           0     0         0     1     0  15260.18
## 803       0    0      0           0     0         0     1     0  15391.46
## 804       0    0      0           0     0         0     1     0  15555.71
##      distancia
## 1    6128.0785
## 2    4101.7435
## 3    9648.5613
## 4    6851.6825
## 5    5361.4972
## 6    7204.0356
## 7    5297.7932
## 8    8399.7805
## 9    7635.0280
## 10   5278.8954
## 11   3048.9515
## 12   6449.6464
## 13   6788.5881
## 14   8234.2721
## 15   6953.7918
## 16   3052.3043
## 17   7027.2495
## 18   6051.5728
## 19   7072.6652
## 20   2443.6113
## 21   4454.0966
## 22   6885.5157
## 23   6974.2136
## 24   8164.4721
## 25   5627.8956
## 26   6372.5311
## 27   6044.2575
## 28   6651.4265
## 29   7728.9076
## 30   6566.9959
## 31   4903.6820
## 32   7324.7379
## 33   3685.0768
## 34   6490.1853
## 35  14932.6384
## 36   8245.2451
## 37   6451.7801
## 38   3291.8800
## 39   4440.3804
## 40   1178.6759
## 41   6887.9542
## 42   3436.0522
## 43   8046.8178
## 44   6805.6572
## 45  11801.0851
## 46   2985.5523
## 47   8716.7764
## 48    510.8510
## 49   7054.0722
## 50   6721.5313
## 51   8009.0222
## 52   7695.3792
## 53   2415.2646
## 54   4598.5735
## 55   8078.2126
## 56   5168.2516
## 57   6127.4689
## 58   7735.9181
## 59   6100.6462
## 60   6796.5130
## 61   6737.6859
## 62   6063.7649
## 63   5515.7279
## 64   7388.1370
## 65   7029.3831
## 66   6943.1236
## 67   5953.1212
## 68  10172.5189
## 69   9765.6059
## 70   1104.9134
## 71   1596.8666
## 72   6096.0741
## 73   7262.2531
## 74   4433.6747
## 75   9297.1227
## 76   8340.6486
## 77   4147.4640
## 78   7166.5447
## 79   7041.8800
## 80   2247.0129
## 81  10499.5733
## 82   9581.8093
## 83   6851.6825
## 84   4198.9759
## 85   8388.5028
## 86   8349.7927
## 87   5931.7849
## 88   6822.1166
## 89   7071.4460
## 90   2733.1748
## 91   2150.0853
## 92  10421.5435
## 93   3541.5143
## 94   7394.2331
## 95   2868.2029
## 96   6507.5591
## 97   7607.9005
## 98   4169.1051
## 99   8290.3560
## 100  3326.9325
## 101  4935.6864
## 102  6528.2858
## 103  7963.3016
## 104  6672.4579
## 105  4006.6447
## 106  6284.4428
## 107  7412.2165
## 108  7294.2575
## 109  5815.9595
## 110  7706.9617
## 111  7632.2848
## 112  5589.7952
## 113  7635.9425
## 114  7270.1780
## 115  7761.5216
## 116  3059.0100
## 117   911.9727
## 118  6035.1134
## 119 10026.8227
## 120  7119.9098
## 121  4592.1726
## 122  7243.6601
## 123  7935.2597
## 124  5069.8000
## 125  3223.9088
## 126  2734.0892
## 127  5031.6996
## 128  4377.8956
## 129  7458.2419
## 130  7694.7696
## 131  6520.9705
## 132  7684.7110
## 133  8258.9612
## 134  8162.3385
## 135  7449.0978
## 136  4802.1824
## 137  5531.2729
## 138  4948.4882
## 139  5223.7259
## 140  7723.4211
## 141  6695.3182
## 142  4493.7210
## 143  7669.7757
## 144  4889.3563
## 145  4296.2082
## 146 12749.6342
## 147  6581.6264
## 148 10995.4889
## 149  8001.0973
## 150  3898.7442
## 151  2476.5301
## 152  6769.9951
## 153  2859.0588
## 154  1563.9478
## 155  6375.5791
## 156  7112.5945
## 157  6901.0607
## 158  3679.2855
## 159  2060.7779
## 160  8070.2877
## 161  9163.3138
## 162  6330.7730
## 163  8642.4043
## 164  6897.0983
## 165  3753.0480
## 166  1382.8944
## 167  7710.0098
## 168  6348.7564
## 169  7856.9251
## 170  6493.5382
## 171  9121.2509
## 172  8080.6511
## 173  5045.7206
## 174  6541.3923
## 175  9578.1517
## 176  9690.0146
## 177  5549.2563
## 178  4900.6340
## 179  6520.0561
## 180  5197.8176
## 181  6441.1119
## 182  6465.1914
## 183  3753.3528
## 184  7617.6542
## 185  8449.4635
## 186  5308.7662
## 187  3422.6408
## 188  5836.3814
## 189  6684.0405
## 190  7818.5199
## 191  5950.0732
## 192  5264.8744
## 193  8280.9071
## 194  9508.9612
## 195  1956.8398
## 196   177.7006
## 197 13012.3750
## 198  7069.3124
## 199  5547.4275
## 200  2405.5109
## 201  3664.0454
## 202  3079.1270
## 203  4946.6594
## 204  4341.0144
## 205  7918.4955
## 206  3348.5735
## 207 10869.9098
## 208  6243.5991
## 209  5289.2587
## 210  6144.2331
## 211  8485.4304
## 212  8896.3058
## 213  5421.5435
## 214  7413.7406
## 215  5962.5701
## 216  7179.0417
## 217 10715.9839
## 218  3734.1502
## 219  6378.0176
## 220  7485.6742
## 221  3151.6703
## 222  5825.4084
## 223  8438.7954
## 224  1190.8681
## 225  8030.0536
## 226 15358.1444
## 227  5086.5643
## 228  6532.8578
## 229  5964.7037
## 230  5731.8337
## 231  6693.4894
## 232  4587.2958
## 233 12411.3021
## 234  5543.4650
## 235  1953.4870
## 236  5278.2858
## 237  7686.5399
## 238  6588.6369
## 239  1474.0307
## 240  5313.0334
## 241  6879.7245
## 242  6940.6852
## 243  7868.8125
## 244  5977.8103
## 245  5407.5226
## 246  7488.4175
## 247  9234.6379
## 248  9110.8876
## 249 10010.9729
## 250  6210.0707
## 251  8516.8252
## 252  1843.4528
## 253  7877.6518
## 254  8305.5962
## 255  3174.2258
## 256  7160.1439
## 257  8138.2590
## 258  5669.9585
## 259  3550.0488
## 260  7903.8649
## 261  1776.0912
## 262  6774.2624
## 263  6126.5545
## 264  3688.4297
## 265  6197.8786
## 266  8505.8522
## 267  3051.3899
## 268  6004.3282
## 269  7150.6950
## 270 10334.6745
## 271  5908.9247
## 272  7492.9895
## 273  6653.5601
## 274  8087.0519
## 275  5187.7591
## 276  3651.8532
## 277  1262.4970
## 278  1106.1327
## 279  9433.0651
## 280  3400.3901
## 281  4649.1709
## 282  6079.3099
## 283   751.0363
## 284  4459.2782
## 285  7882.8335
## 286  7634.7232
## 287 12175.6889
## 288  7641.1241
## 289  3501.5850
## 290  8361.0705
## 291  6439.8927
## 292  3956.3521
## 293  5727.2616
## 294  7238.4784
## 295  6587.1129
## 296 10611.7410
## 297  5546.8178
## 298  7758.1687
## 299   564.8013
## 300  8223.6040
## 301  6057.6689
## 302  8951.4752
## 303  6556.9373
## 304  6606.6203
## 305  7832.5408
## 306  9894.5379
## 307  6527.9810
## 308  8374.7866
## 309   451.1095
## 310  7199.7683
## 311  8658.8637
## 312  6009.2051
## 313  6064.9842
## 314  1417.9468
## 315  6303.9503
## 316  4337.6615
## 317  1897.0983
## 318  5168.8613
## 319  6109.1807
## 320  6252.1336
## 321  6609.3636
## 322  8400.3901
## 323  2625.8839
## 324  5719.9464
## 325  6927.8834
## 326  6752.0117
## 327 11902.2799
## 328  5492.8676
## 329  4507.4372
## 330  2153.4382
## 331  7047.3665
## 332  6143.6235
## 333  7046.7569
## 334  6152.7676
## 335  7847.4762
## 336  7765.1792
## 337  6529.2002
## 338  4978.9685
## 339  2014.1429
## 340  5940.6242
## 341  4629.9683
## 342  7591.1363
## 343  5623.9332
## 344  5316.0814
## 345  2218.3614
## 346 10552.6091
## 347  6404.8403
## 348  4458.3638
## 349  7639.9049
## 350  7884.9671
## 351  3112.9603
## 352  6561.5094
## 353  5229.8220
## 354  5763.8381
## 355   263.6552
## 356  5550.4755
## 357  4518.4101
## 358  6040.5999
## 359  7051.0241
## 360  5972.0190
## 361  5570.8973
## 362  6060.4121
## 363  7381.7362
## 364   284.0771
## 365  8607.3519
## 366  6962.0215
## 367  2254.6330
## 368  1716.6545
## 369  6976.6520
## 370  6538.6491
## 371  2330.2243
## 372  4497.0739
## 373  4381.8581
## 374  7207.0836
## 375  8165.0817
## 376  8225.4328
## 377  6406.9739
## 378  5929.9561
## 379  6783.4065
## 380  7919.1051
## 381  7935.2597
## 382  3217.2031
## 383  7421.6654
## 384   877.5299
## 385  7996.8300
## 386 11874.5428
## 387  7542.9773
## 388  6834.6135
## 389  3521.7020
## 390  5359.3636
## 391  6437.7591
## 392  5028.0419
## 393  8651.8532
## 394  5974.7623
## 395  4042.3068
## 396  7307.9737
## 397  1764.2039
## 398  1202.7554
## 399  3785.6620
## 400  1276.5179
## 401  6777.6152
## 402  6999.5123
## 403  8328.7613
## 404  6044.8671
## 405  2784.3819
## 406  4022.1897
## 407  2505.7913
## 408  7792.0020
## 409  5763.8381
## 410  3052.3043
## 411  7253.7186
## 412  6793.1602
## 413   703.4870
## 414  4676.6033
## 415   671.1778
## 416  7331.1387
## 417  7123.5674
## 418  7239.3928
## 419  9169.1051
## 420  6512.7408
## 421  5446.8422
## 422  6417.9468
## 423  5623.3236
## 424  3238.5394
## 425  4552.8530
## 426  2011.0949
## 427  8377.2251
## 428  7992.5628
## 429  5687.9420
## 430  5336.5033
## 431  6373.4455
## 432  8395.5133
## 433  7048.2809
## 434  1965.0695
## 435  6718.4833
## 436  5106.6813
## 437   867.4713
## 438  2278.7125
## 439 10767.4957
## 440  5821.7508
## 441  7862.7164
## 442  4603.7552
## 443  5663.5577
## 444   240.1853
## 445  7272.9212
## 446  6271.3363
## 447  7838.6369
## 448  9541.2704
## 449  4362.0458
## 450  7364.9720
## 451  6039.9902
## 452  6560.8998
## 453  7624.3599
## 454  7841.6850
## 455  1500.2438
## 456  2345.4645
## 457 11145.4523
## 458  6445.0744
## 459  5317.3007
## 460  3750.6096
## 461  6190.2585
## 462  7031.5167
## 463  6931.2363
## 464  8742.6847
## 465  5263.9600
## 466  2914.2282
## 467  2044.9281
## 468  2168.6784
## 469  7987.0763
## 470   544.6842
## 471 10167.3372
## 472  6822.4214
## 473  2502.7432
## 474  6573.7015
## 475  7946.2326
## 476  8369.9098
## 477  6143.0139
## 478  4443.7332
## 479  2661.5460
## 480  3472.6286
## 481  7911.4850
## 482  5657.7664
## 483  7859.9732
## 484  7777.3714
## 485  3151.0607
## 486  3415.0207
## 487  2221.1046
## 488  5955.5596
## 489  6614.8500
## 490 10858.3272
## 491  7311.9361
## 492   485.2475
## 493  8938.3687
## 494  6697.7566
## 495  6899.8415
## 496  8473.5430
## 497   667.2153
## 498   729.0905
## 499  6051.2680
## 500  6936.7228
## 501  7540.8437
## 502  3607.6567
## 503  3925.2621
## 504  4926.5423
## 505   529.4440
## 506  9054.8037
## 507  7517.6786
## 508  9893.3187
## 509  6668.1907
## 510  6957.4494
## 511 10667.5201
## 512  8625.3353
## 513  6635.5767
## 514  5822.0556
## 515  5943.6723
## 516  4059.3758
## 517  7336.3204
## 518  8024.8720
## 519  7128.7491
## 520  5918.9832
## 521  3373.8722
## 522  4729.3343
## 523  8001.7069
## 524   699.5245
## 525  6977.2616
## 526  1723.0554
## 527  3673.4943
## 528 11316.7520
## 529  5283.7723
## 530  2985.5523
## 531  7884.9671
## 532  3977.6884
## 533  7323.5187
## 534  2788.9539
## 535   906.1814
## 536  7108.9368
## 537  2668.2516
## 538  7076.6277
## 539  1360.3389
## 540  4359.9122
## 541  3799.6830
## 542  7787.7347
## 543  4556.2058
## 544  7517.9834
## 545  6896.4887
## 546  7335.7108
## 547  7626.1887
## 548  1639.5391
## 549   193.8552
## 550  8346.7447
## 551  8516.2156
## 552 10330.4072
## 553  8007.8030
## 554  4910.6925
## 555  4426.0546
## 556  5035.6620
## 557  2041.8800
## 558  7006.2180
## 559  4328.2126
## 560  6757.1934
## 561  9141.6728
## 562 10759.2660
## 563 10032.3092
## 564  4400.7559
## 565  5847.0495
## 566  7983.1139
## 567  2161.3631
## 568  7685.9303
## 569  9899.1100
## 570  5769.9342
## 571  1166.7886
## 572  9553.7674
## 573  7540.2341
## 574  6159.4733
## 575  8412.5823
## 576  6712.0824
## 577  4352.9017
## 578  5386.7959
## 579  6304.8647
## 580 10445.3182
## 581  7748.1102
## 582  4011.8264
## 583  5545.9034
## 584 10416.6667
## 585  1567.9103
## 586  6763.2894
## 587  7140.3316
## 588  9684.5282
## 589  5174.0429
## 590  5933.0041
## 591  8500.0610
## 592  5605.6450
## 593  7912.7042
## 594  3692.6969
## 595  7671.6045
## 596  6643.5016
## 597  5832.7237
## 598  3266.5813
## 599  7847.7810
## 600  6931.2363
## 601  6678.2492
## 602  8684.4672
## 603  6829.1270
## 604  6008.2907
## 605  7280.5413
## 606 11119.2392
## 607  5434.6501
## 608  7954.4623
## 609  2799.6220
## 610  5859.8513
## 611  6643.8064
## 612  5971.7142
## 613  8482.6871
## 614  3120.2755
## 615 10566.0205
## 616  7317.7274
## 617  5933.6138
## 618  5763.2285
## 619  6887.3446
## 620  6736.7715
## 621  9037.1251
## 622  7197.0251
## 623  7441.7825
## 624  5687.9420
## 625  7748.4150
## 626  1676.1156
## 627  7666.7276
## 628  4405.0232
## 629  5582.4799
## 630   268.5321
## 631  6831.8703
## 632  5705.9254
## 633  9592.4774
## 634  7873.0797
## 635  2758.1687
## 636  1734.3331
## 637  3420.2024
## 638  8363.2041
## 639  6494.4526
## 640  3750.0000
## 641  3892.9529
## 642  7789.8683
## 643  7933.4309
## 644  3624.7257
## 645  7862.1068
## 646  6239.9415
## 647  9694.2819
## 648  4754.9378
## 649  8918.5564
## 650  2363.7527
## 651  5820.2268
## 652  1775.7864
## 653  7244.2697
## 654  9547.6713
## 655  6137.8322
## 656  5398.3784
## 657  6602.3531
## 658  6464.2770
## 659  6481.9556
## 660  8435.1378
## 661  4897.5860
## 662 11142.7091
## 663  4351.9873
## 664  4945.4401
## 665   356.3155
## 666  4845.7693
## 667   353.5723
## 668 10163.6796
## 669  3112.0458
## 670  2543.5869
## 671  5897.0373
## 672  4806.1448
## 673  7968.4833
## 674  9046.5740
## 675  6853.2065
## 676  7488.4175
## 677  2647.8298
## 678  6016.8252
## 679  1809.3148
## 680  4085.5889
## 681  8967.3250
## 682 11233.5406
## 683  5763.8381
## 684  3450.9876
## 685  5728.7857
## 686  4302.3043
## 687  4420.8730
## 688 11382.8944
## 689  2426.5423
## 690  4214.8257
## 691  2706.6569
## 692  9916.7886
## 693  4615.9473
## 694  7304.0112
## 695  1581.6264
## 696  6518.5321
## 697  5477.6274
## 698  5648.9271
## 699  2568.5808
## 700  5498.6589
## 701  5166.4228
## 702  5284.3819
## 703  8296.1473
## 704  9161.1802
## 705  6531.0290
## 706  7652.7067
## 707  7455.8035
## 708  5721.4704
## 709  7886.4911
## 710  6528.8954
## 711  6821.5069
## 712  3012.0702
## 713  6456.0473
## 714  8466.2277
## 715  6591.9898
## 716  5795.5377
## 717  7740.1853
## 718  8024.8720
## 719  5706.2302
## 720  5330.4072
## 721  6157.0349
## 722  8300.1097
## 723  3938.0639
## 724  5166.7276
## 725  9177.0300
## 726  5381.6142
## 727  2592.9651
## 728  1316.1424
## 729  5614.1795
## 730  1630.6998
## 731  7480.4926
## 732  8404.6574
## 733  6251.5240
## 734  6234.1502
## 735  7082.7237
## 736  7769.1417
## 737  4981.1022
## 738  7321.6898
## 739  6881.2485
## 740  2037.6128
## 741  3650.0244
## 742  6504.8159
## 743  3356.8032
## 744   797.3665
## 745  7680.7486
## 746  6638.0151
## 747  9627.8347
## 748  3217.2031
## 749  5210.3146
## 750  4259.0222
## 751  4099.3050
## 752  7027.5543
## 753  4215.1305
## 754  6412.7652
## 755  4643.0749
## 756  6257.6201
## 757  4847.2933
## 758 11846.5009
## 759  5286.2107
## 760  9509.5708
## 761  1863.5699
## 762  5362.7164
## 763  6550.8413
## 764  4283.7113
## 765  6483.7844
## 766  5319.1295
## 767  8964.2770
## 768  5943.0627
## 769  9510.4852
## 770  5473.9698
## 771  6324.9817
## 772 12669.4709
## 773  6361.2534
## 774  9267.2519
## 775  7802.3653
## 776  5224.0307
## 777 10351.7435
## 778  8655.8157
## 779  5642.2214
## 780  5564.8013
## 781  4553.1578
## 782  9942.3921
## 783  3783.2236
## 784  8253.1700
## 785  2721.2875
## 786  8524.1405
## 787  1431.0534
## 788  5327.9688
## 789  7738.0517
## 790  3843.5747
## 791  7397.5860
## 792  6610.5828
## 793  7992.8676
## 794  7693.2455
## 795  6703.2431
## 796  4407.7664
## 797  8608.2663
## 798  6999.5123
## 799  1437.4543
## 801  6350.8900
## 802  7571.9337
## 803  5874.4818
## 804  5963.4845
```

```r
subset(cars, cars$speed > 10 & cars$speed < 15 & cars$dist > 45) # speed en (10,15) y dist>45
```

```
##  [1] Price       Mileage     Cylinder    Doors       Cruise      Sound      
##  [7] Leather     Buick       Cadillac    Chevy       Pontiac     Saab       
## [13] Saturn      convertible coupe       hatchback   sedan       wagon      
## [19] velocidad   distancia  
## <0 rows> (or 0-length row.names)
```

También se pueden hacer el filtrado empleando directamente los
correspondientes vectores de índices:


```r
ii <- cars$dist > 85
cars[ii, ]   # dis>85
```

```
##        Price Mileage Cylinder Doors Cruise Sound Leather Buick Cadillac Chevy
## 1   22661.05   20105        6     4      1     0       0     1        0     0
## 2   21725.01   13457        6     2      1     1       0     0        0     1
## 3   29142.71   31655        4     2      1     1       1     0        0     0
## 4   30731.94   22479        4     2      1     0       0     0        0     0
## 5   33358.77   17590        4     2      1     1       1     0        0     0
## 6   30315.17   23635        4     2      1     0       0     0        0     0
## 7   33381.82   17381        4     2      1     1       1     0        0     0
## 8   30251.02   27558        4     2      1     0       1     0        0     0
## 9   30166.85   25049        4     2      1     0       0     0        0     0
## 10  27060.14   17319        4     4      1     0       1     0        0     0
## 11  26841.08   10003        4     4      1     1       0     0        0     0
## 12  25790.51   21160        4     4      1     1       1     0        0     0
## 13  25148.38   22272        4     4      1     1       1     0        0     0
## 14  24173.53   27015        4     4      1     0       0     0        0     0
## 15  24852.50   22814        4     4      1     1       1     0        0     0
## 16  27825.95   10014        4     4      1     0       1     0        0     0
## 17  26698.08   23055        4     4      1     1       1     0        0     0
## 18  28185.78   19854        4     4      1     1       1     0        0     0
## 19  27241.44   23204        4     4      1     1       1     0        0     0
## 20  30800.66    8017        4     4      1     0       1     0        0     0
## 21  28416.46   14613        4     4      1     0       1     0        0     0
## 22  26653.24   22590        4     4      1     1       1     0        0     0
## 23  27610.86   22881        4     4      1     1       1     0        0     0
## 24  27788.81   26786        4     4      1     0       1     0        0     0
## 25  29986.79   18464        4     4      1     0       1     0        0     0
## 26  29197.79   20907        4     4      1     0       1     0        0     0
## 27  29908.18   19830        4     4      1     1       1     0        0     0
## 28  29481.53   21822        4     4      1     1       1     0        0     0
## 29  26792.30   25357        4     4      1     0       1     0        0     0
## 30  29321.08   21545        4     4      1     0       1     0        0     0
## 31  37383.50   16088        4     2      1     0       1     0        0     0
## 32  34393.00   24031        4     2      1     0       1     0        0     0
## 33  38324.81   12090        4     2      1     1       1     0        0     0
## 34  35304.49   21293        4     2      1     0       1     0        0     0
## 35  28777.96   48991        4     2      1     1       1     0        0     0
## 36  33248.34   27051        4     2      1     0       1     0        0     0
## 37  35580.33   21167        4     2      1     1       0     0        0     0
## 38  30274.71   10800        4     4      1     1       0     0        0     0
## 39  30122.43   14568        4     4      1     1       1     0        0     0
## 40  32197.34    3867        4     4      1     1       0     0        0     0
## 41  27109.41   22598        4     4      1     0       1     0        0     0
## 42  30353.59   11273        4     4      1     1       0     0        0     0
## 43  27256.49   26400        4     4      1     1       0     0        0     0
## 44  28291.76   22328        4     4      1     1       0     0        0     0
## 45  24912.08   38717        4     4      1     0       1     0        0     0
## 46  33183.33    9795        4     4      1     1       1     0        0     0
## 47  28502.96   28598        4     4      1     1       1     0        0     0
## 48  35033.22    1676        4     4      1     1       1     0        0     0
## 49  29844.20   23143        4     4      1     1       1     0        0     0
## 50  30075.99   22052        4     4      1     1       1     0        0     0
## 51  28054.98   26276        4     4      1     1       1     0        0     0
## 52  28432.82   25247        4     4      1     1       1     0        0     0
## 53  32746.13    7924        4     4      1     0       1     0        0     0
## 54  31002.73   15087        4     4      1     1       1     0        0     0
## 55  28829.03   26503        4     4      1     0       1     0        0     0
## 56  31849.31   16956        4     4      1     0       1     0        0     0
## 57  31554.41   20103        4     4      1     1       1     0        0     0
## 58  28678.08   25380        4     4      1     0       1     0        0     0
## 59  29961.25   20015        4     4      1     1       1     0        0     0
## 60  30575.25   22298        4     4      1     1       1     0        0     0
## 61  29914.38   22105        4     4      1     0       1     0        0     0
## 62  17325.27   19894        4     4      1     0       0     0        0     0
## 63  16391.93   18096        4     4      1     1       0     0        0     0
## 64  15505.29   24239        4     4      1     0       0     0        0     0
## 65  15297.84   23062        4     4      0     0       0     0        0     0
## 66  16078.67   22779        4     4      0     0       1     0        0     0
## 67  16551.22   19531        4     4      1     1       1     0        0     0
## 68  14546.88   33374        4     4      0     1       1     0        0     0
## 69  37288.94   32039        8     2      1     1       1     0        0     1
## 70  46732.61    3625        8     2      1     1       1     0        0     1
## 71  47065.21    5239        8     2      1     1       1     0        0     1
## 72  41371.38   20000        8     2      1     0       1     0        0     1
## 73  39547.59   23826        8     2      1     1       1     0        0     1
## 74  42773.03   14546        8     2      1     1       1     0        0     1
## 75  36970.90   30502        8     2      1     1       1     0        0     1
## 76  11203.15   27364        4     2      1     1       1     0        0     1
## 77  13041.87   13607        4     2      0     1       1     0        0     1
## 78  12207.87   23512        4     2      1     1       1     0        0     1
## 79  11726.00   23103        4     2      0     1       1     0        0     1
## 80  13007.98    7372        4     2      1     1       1     0        0     1
## 81  11149.62   34447        4     2      1     1       1     0        0     1
## 82  10788.97   31436        4     2      1     1       1     0        0     1
## 83  12570.14   22479        4     2      0     1       1     0        0     1
## 84  14023.94   13776        4     2      0     1       1     0        0     1
## 85  12706.91   27521        4     2      1     1       1     0        0     1
## 86  11961.62   27394        4     2      1     1       1     0        0     1
## 87  12810.91   19461        4     2      1     1       1     0        0     1
## 88  12845.17   22382        4     2      1     1       1     0        0     1
## 89  12897.93   23200        4     2      1     1       1     0        0     1
## 90  39713.67    8967        8     2      1     1       1     0        0     1
## 91  39875.85    7054        8     2      1     0       1     0        0     1
## 92  31186.74   34191        8     2      1     0       1     0        0     1
## 93  39365.88   11619        8     2      1     1       1     0        0     1
## 94  34297.31   24259        8     2      1     0       1     0        0     1
## 95  38990.61    9410        8     2      1     1       1     0        0     1
## 96  35261.44   21350        8     2      1     1       1     0        0     1
## 97  27370.96   24960        8     2      1     1       1     0        0     0
## 98  31024.87   13678        8     2      1     1       1     0        0     0
## 99  28502.31   27199        8     2      1     1       1     0        0     0
## 100 32219.59   10915        8     2      1     1       1     0        0     0
## 101 29595.79   16193        8     2      1     0       1     0        0     0
## 102 29664.70   21418        8     2      1     0       1     0        0     0
## 103 27548.63   26126        8     2      1     1       1     0        0     0
## 104 12464.07   21891        4     2      1     1       1     0        0     0
## 105 13160.12   13145        4     2      0     1       1     0        0     0
## 106 12832.46   20618        4     2      1     1       1     0        0     0
## 107 12258.86   24318        4     2      1     1       1     0        0     0
## 108 12465.51   23931        4     2      0     1       1     0        0     0
## 109 12828.03   19081        4     2      1     1       1     0        0     0
## 110 11903.10   25285        4     2      0     1       1     0        0     0
## 111 17750.88   25040        6     4      1     1       0     1        0     0
## 112 18145.13   18339        6     4      1     1       0     1        0     0
## 113 17772.97   25052        6     4      1     1       0     1        0     0
## 114 18348.90   23852        6     4      1     1       0     1        0     0
## 115 17394.02   25464        6     4      1     1       0     1        0     0
## 116 20099.26   10036        6     4      1     1       1     1        0     0
## 117 20698.08    2992        6     4      1     0       1     1        0     0
## 118 19924.16   19800        6     4      1     1       1     1        0     0
## 119 17808.20   32896        6     4      1     1       0     1        0     0
## 120 19774.25   23359        6     4      1     1       1     1        0     0
## 121 20538.09   15066        6     4      1     1       0     1        0     0
## 122 19344.17   23765        6     4      1     1       0     1        0     0
## 123 18543.43   26034        6     4      1     1       1     1        0     0
## 124 20512.09   16633        6     4      1     1       0     1        0     0
## 125 23785.92   10577        6     4      1     1       1     1        0     0
## 126 22358.88    8970        6     4      1     1       0     1        0     0
## 127 21895.76   16508        6     4      1     0       1     1        0     0
## 128 22926.09   14363        6     4      1     1       1     1        0     0
## 129 21058.14   24469        6     4      1     1       1     1        0     0
## 130 19556.90   25245        6     4      1     0       0     1        0     0
## 131 21183.12   21394        6     4      1     0       0     1        0     0
## 132 21341.26   25212        6     4      1     1       1     1        0     0
## 133 20986.02   27096        6     4      1     0       0     1        0     0
## 134 21683.03   26779        6     4      1     1       0     1        0     0
## 135 21799.17   24439        6     4      1     0       0     1        0     0
## 136 23447.69   15755        6     4      1     1       0     1        0     0
## 137 23016.01   18147        6     4      1     1       1     1        0     0
## 138 23547.24   16235        6     4      1     1       0     1        0     0
## 139 38600.24   17138        8     4      1     0       1     0        1     0
## 140 36154.30   25339        8     4      1     1       1     0        1     0
## 141 36077.80   21966        8     4      1     0       1     0        1     0
## 142 40335.74   14743        8     4      1     0       1     0        1     0
## 143 35338.65   25163        8     4      1     0       1     0        1     0
## 144 39307.01   16041        8     4      1     0       1     0        1     0
## 145 39801.55   14095        8     4      1     0       1     0        1     0
## 146 32537.19   41829        8     4      1     1       1     0        1     0
## 147 37510.25   21593        8     4      1     0       1     0        1     0
## 148 32954.14   36074        8     4      1     0       1     0        1     0
## 149 36245.16   26250        8     4      1     1       1     0        1     0
## 150 40856.39   12791        8     4      1     1       1     0        1     0
## 151 41378.05    8125        8     4      1     0       1     0        1     0
## 152 37215.17   22211        8     4      1     0       1     0        1     0
## 153 17202.83    9380        6     2      0     1       1     0        0     0
## 154 17675.84    5131        6     2      0     1       1     0        0     0
## 155 15253.87   20917        6     2      1     1       1     0        0     0
## 156 14703.14   23335        6     2      0     1       1     0        0     0
## 157 15059.13   22641        6     2      1     1       1     0        0     0
## 158 16792.68   12071        6     2      1     1       1     0        0     0
## 159 17141.94    6761        6     2      1     1       1     0        0     0
## 160 19204.81   26477        6     4      1     0       1     0        0     0
## 161 18529.34   30063        6     4      1     0       1     0        0     0
## 162 20627.66   20770        6     4      1     1       1     0        0     0
## 163 18158.08   28354        6     4      1     0       0     0        0     0
## 164 19540.24   22628        6     4      1     1       0     0        0     0
## 165 21875.10   12313        6     4      1     0       1     0        0     0
## 166 21903.32    4537        6     4      1     1       0     0        0     0
## 167 15788.10   25295        6     4      1     1       0     0        0     0
## 168 17162.87   20829        6     4      1     0       0     0        0     0
## 169 16569.14   25777        6     4      1     0       0     0        0     0
## 170 16391.17   21304        6     4      1     1       0     0        0     0
## 171 15457.17   29925        6     4      1     1       0     0        0     0
## 172 16283.96   26511        6     4      1     0       1     0        0     0
## 173 18254.92   16554        6     4      1     1       1     0        0     0
## 174 15802.65   21461        4     4      0     0       0     0        0     0
## 175 14396.27   31424        4     4      0     0       0     0        0     0
## 176 14869.28   31791        4     4      1     1       1     0        0     0
## 177 15568.97   18206        4     4      0     0       0     0        0     0
## 178 16353.10   16078        4     4      0     1       0     0        0     0
## 179 15730.05   21391        4     4      0     1       1     0        0     0
## 180 15977.91   17053        4     4      0     0       1     0        0     0
## 181 19646.72   21132        6     4      1     1       1     0        0     0
## 182 20173.91   21211        6     4      1     0       1     0        0     0
## 183 22100.39   12314        6     4      1     1       0     0        0     0
## 184 18701.22   24992        6     4      1     0       0     0        0     0
## 185 19448.23   27721        6     4      1     0       1     0        0     0
## 186 21281.88   17417        6     4      1     1       1     0        0     0
## 187 21230.98   11229        6     4      1     1       0     0        0     0
## 188 23578.16   19148        8     4      1     0       1     0        0     0
## 189 22231.56   21929        8     4      1     1       1     0        0     0
## 190 22189.12   25651        8     4      1     0       1     0        0     0
## 191 22525.27   19521        8     4      1     1       1     0        0     0
## 192 23449.31   17273        8     4      1     0       1     0        0     0
## 193 21403.76   27168        8     4      1     0       1     0        0     0
## 194 21200.69   31197        8     4      1     1       1     0        0     0
## 195 68566.19    6420        8     2      1     1       1     0        1     0
## 196 70755.47     583        8     2      1     1       1     0        1     0
## 197 52001.99   42691        8     2      1     1       1     0        1     0
## 198 60567.55   23193        8     2      1     1       1     0        1     0
## 199 63913.12   18200        8     2      1     1       1     0        1     0
## 200 69133.73    7892        8     2      1     1       1     0        1     0
## 201 66374.31   12021        8     2      1     1       1     0        1     0
## 202 17322.08   10102        6     4      1     0       1     0        0     0
## 203 16507.07   16229        6     4      1     0       0     0        0     0
## 204 16425.17   14242        6     4      1     0       0     0        0     0
## 205 15118.89   25979        6     4      1     1       0     0        0     0
## 206 17978.36   10986        6     4      1     0       0     0        0     0
## 207 13585.64   35662        6     4      1     0       0     0        0     0
## 208 15731.13   20484        6     4      1     1       0     0        0     0
## 209 21908.37   17353        6     4      1     0       0     1        0     0
## 210 20952.22   20158        6     4      1     1       0     1        0     0
## 211 19425.85   27839        6     4      1     1       0     1        0     0
## 212 19191.99   29187        6     4      1     1       1     1        0     0
## 213 21956.34   17787        6     4      1     1       0     1        0     0
## 214 19981.13   24323        6     4      1     1       0     1        0     0
## 215 21646.12   19562        6     4      1     1       1     1        0     0
## 216 32075.98   23553        4     2      1     1       0     0        0     0
## 217 27666.23   35157        4     2      1     0       0     0        0     0
## 218 34819.30   12251        4     2      1     0       0     0        0     0
## 219 33540.54   20925        4     2      1     0       1     0        0     0
## 220 31969.07   24559        4     2      1     0       0     0        0     0
## 221 35622.14   10340        4     2      1     1       0     0        0     0
## 222 32737.08   19112        4     2      1     1       1     0        0     0
## 223 23249.84   27686        4     4      1     0       0     0        0     0
## 224 29246.24    3907        4     4      1     0       1     0        0     0
## 225 24801.62   26345        4     4      1     1       1     0        0     0
## 226 22244.88   50387        4     4      1     0       1     0        0     0
## 227 26775.03   16688        4     4      1     0       0     0        0     0
## 228 25996.81   21433        4     4      1     1       1     0        0     0
## 229 25299.97   19569        4     4      1     1       0     0        0     0
## 230 31156.60   18805        4     4      1     1       1     0        0     0
## 231 29114.54   21960        4     4      1     0       1     0        0     0
## 232 30443.88   15050        4     4      1     0       1     0        0     0
## 233 24903.48   40719        4     4      1     1       1     0        0     0
## 234 31084.94   18187        4     4      1     1       1     0        0     0
## 235 33005.78    6409        4     4      1     1       1     0        0     0
## 236 31153.01   17317        4     4      1     0       1     0        0     0
## 237 23329.21   25218        4     4      1     0       1     0        0     0
## 238 23274.48   21616        4     4      1     1       0     0        0     0
## 239 27280.98    4836        4     4      1     1       0     0        0     0
## 240 25959.12   17431        4     4      1     0       1     0        0     0
## 241 12630.78   22571        4     2      1     1       1     0        0     1
## 242 12425.39   22771        4     2      0     1       1     0        0     1
## 243 12549.89   25816        4     2      0     1       1     0        0     1
## 244 12274.96   19612        4     2      1     1       1     0        0     1
## 245 13446.21   17741        4     2      0     1       1     0        0     1
## 246 12319.70   24568        4     2      1     1       1     0        0     1
## 247 12234.89   30297        4     2      1     1       1     0        0     1
## 248 12684.99   29891        4     2      1     1       1     0        0     1
## 249 12553.07   32844        4     2      1     1       1     0        0     1
## 250 14185.02   20374        4     2      0     1       1     0        0     1
## 251 13019.07   27942        4     2      1     1       1     0        0     1
## 252 15747.80    6048        4     2      0     1       1     0        0     1
## 253 13699.04   25845        4     2      1     1       1     0        0     1
## 254 13530.07   27249        4     2      1     1       1     0        0     1
## 255 19177.41   10414        6     2      1     1       0     0        0     1
## 256 16357.99   23491        6     2      1     1       0     0        0     1
## 257 15797.20   26700        6     2      1     1       0     0        0     1
## 258 17515.40   18602        6     2      1     0       0     0        0     1
## 259 18040.14   11647        6     2      1     0       1     0        0     1
## 260 16345.94   25931        6     2      1     1       0     0        0     1
## 261 18800.09    5827        6     2      1     0       0     0        0     1
## 262 10386.04   22225        4     4      0     0       0     0        0     1
## 263 11017.17   20100        4     4      0     1       0     0        0     1
## 264 12163.82   12101        4     4      0     0       1     0        0     1
## 265 11096.86   20334        4     4      1     0       0     0        0     1
## 266 10777.05   27906        4     4      0     0       0     0        0     1
## 267 12146.19   10011        4     4      0     0       1     0        0     1
## 268 11472.02   19699        4     4      0     0       1     0        0     1
## 269 16825.19   23460        6     4      0     1       1     0        0     1
## 270 14914.20   33906        6     4      0     1       1     0        0     1
## 271 17801.23   19386        6     4      1     1       1     0        0     1
## 272 16543.98   24583        6     4      0     1       1     0        0     1
## 273 16744.03   21829        6     4      0     1       1     0        0     1
## 274 16143.96   26532        6     4      1     1       1     0        0     1
## 275 17891.63   17020        6     4      0     1       1     0        0     1
## 276 12409.95   11981        4     4      1     1       1     0        0     1
## 277 12314.59    4142        4     4      0     1       0     0        0     1
## 278 12649.11    3629        4     4      0     1       0     0        0     1
## 279 10491.08   30948        4     4      0     1       0     0        0     1
## 280 11318.01   11156        4     4      0     1       1     0        0     1
## 281 11700.11   15253        4     4      1     0       0     0        0     1
## 282 11215.02   19945        4     4      0     0       0     0        0     1
## 283 14894.98    2464        4     4      0     1       1     0        0     1
## 284 13167.70   14630        4     4      0     1       1     0        0     1
## 285 13230.92   25862        4     4      0     1       1     0        0     1
## 286 12573.90   25048        4     4      1     1       1     0        0     1
## 287 11328.96   39946        4     4      1     1       1     0        0     1
## 288 12383.40   25069        4     4      1     1       1     0        0     1
## 289 14678.11   11488        4     4      1     1       1     0        0     1
## 290 13545.03   27431        4     4      0     1       1     0        0     1
## 291 14304.74   21128        4     4      0     1       1     0        0     1
## 292 14847.04   12980        4     4      0     1       1     0        0     1
## 293 14593.85   18790        4     4      0     1       1     0        0     1
## 294 13744.85   23748        4     4      0     1       1     0        0     1
## 295 13688.95   21611        4     4      1     1       1     0        0     1
## 296 12741.19   34815        4     4      0     1       1     0        0     1
## 297 19075.68   18198        6     4      1     0       1     0        0     1
## 298 17839.80   25453        6     4      1     0       1     0        0     1
## 299 21757.05    1853        6     4      1     0       0     0        0     1
## 300 17789.35   26980        6     4      1     1       0     0        0     1
## 301 18527.21   19874        6     4      1     1       0     0        0     1
## 302 17294.18   29368        6     4      1     1       0     0        0     1
## 303 18912.98   21512        6     4      1     1       0     0        0     1
## 304 16472.90   21675        6     4      1     1       1     0        0     1
## 305 16300.47   25697        6     4      1     1       1     0        0     1
## 306 15138.40   32462        6     4      0     1       1     0        0     1
## 307 17158.92   21417        6     4      0     1       1     0        0     1
## 308 15623.20   27476        6     4      0     1       1     0        0     1
## 309 19164.61    1480        6     4      1     1       1     0        0     1
## 310 16993.78   23621        6     4      0     1       1     0        0     1
## 311 12230.10   28408        4     2      0     1       1     0        0     1
## 312 12507.49   19715        4     2      1     1       1     0        0     1
## 313 13141.05   19898        4     2      1     1       1     0        0     1
## 314 15053.93    4652        4     2      1     1       1     0        0     1
## 315 13594.09   20682        4     2      1     1       1     0        0     1
## 316 13464.80   14231        4     2      1     1       1     0        0     1
## 317 14642.32    6224        4     2      0     1       1     0        0     1
## 318 14255.75   16958        4     4      0     1       1     0        0     1
## 319 13308.83   20043        4     4      0     1       1     0        0     1
## 320 14145.88   20512        4     4      1     1       1     0        0     1
## 321 12944.94   21684        4     4      0     1       1     0        0     1
## 322 12846.06   27560        4     4      1     1       1     0        0     1
## 323 14266.91    8615        4     4      0     1       1     0        0     1
## 324 13688.00   18766        4     4      1     1       1     0        0     1
## 325 20017.97   22729        6     2      1     0       0     0        0     1
## 326 20839.15   22152        6     2      1     1       1     0        0     1
## 327 17586.93   39049        6     2      1     0       1     0        0     1
## 328 20676.17   18021        6     2      1     0       1     0        0     1
## 329 22384.12   14788        6     2      1     1       1     0        0     1
## 330 21745.03    7065        6     2      1     1       0     0        0     1
## 331 11179.95   23121        4     4      0     1       1     0        0     1
## 332 11031.13   20156        4     4      0     1       1     0        0     1
## 333 10921.95   23119        4     4      0     1       1     0        0     1
## 334 11343.05   20186        4     4      1     1       1     0        0     1
## 335 11013.87   25746        4     4      1     1       1     0        0     1
## 336 11070.06   25476        4     4      0     1       1     0        0     1
## 337 11391.21   21421        4     4      0     1       1     0        0     1
## 338 18273.01   16335        6     4      0     1       1     0        0     1
## 339 19471.97    6608        6     4      1     1       1     0        0     1
## 340 17663.22   19490        6     4      1     1       1     0        0     1
## 341 18009.85   15190        6     4      0     1       1     0        0     1
## 342 16988.30   24905        6     4      1     1       1     0        0     1
## 343 17553.75   18451        6     4      0     1       1     0        0     1
## 344 18311.76   17441        6     4      0     1       1     0        0     1
## 345 11918.46    7278        4     4      0     0       0     0        0     1
## 346  9919.05   34621        4     4      0     1       0     0        0     1
## 347 10805.13   21013        4     4      1     1       1     0        0     1
## 348 11302.90   14627        4     4      0     1       0     0        0     1
## 349 10770.11   25065        4     4      0     1       0     0        0     1
## 350 10872.01   25869        4     4      0     0       0     0        0     1
## 351 12408.81   10213        4     4      0     0       1     0        0     1
## 352 14401.91   21527        4     4      0     1       1     0        0     1
## 353 15163.17   17158        4     4      1     1       1     0        0     1
## 354 14508.75   18910        4     4      0     1       1     0        0     1
## 355 16116.84     865        4     4      1     1       1     0        0     1
## 356 14897.04   18210        4     4      1     1       1     0        0     1
## 357 15084.82   14824        4     4      1     1       1     0        0     1
## 358 14418.17   19818        4     4      0     1       1     0        0     1
## 359 16403.25   23133        6     4      1     1       1     0        0     1
## 360 17316.10   19593        6     4      1     1       1     0        0     1
## 361 17119.46   18277        6     4      1     1       1     0        0     1
## 362 16860.87   19883        6     4      1     1       1     0        0     1
## 363 16536.74   24218        6     4      1     1       1     0        0     1
## 364 19446.88     932        6     4      0     1       1     0        0     1
## 365 16295.21   28239        6     4      0     1       1     0        0     1
## 366 16860.09   22841        6     4      1     1       1     0        0     1
## 367 18324.83    7397        6     4      1     1       1     0        0     1
## 368 18974.92    5632        6     4      0     1       1     0        0     1
## 369 16027.29   22889        6     4      0     1       1     0        0     1
## 370 16267.09   21452        6     4      0     1       1     0        0     1
## 371 19581.23    7645        6     4      0     1       1     0        0     1
## 372 18169.38   14754        6     4      0     1       1     0        0     1
## 373 14881.96   14376        4     2      0     0       0     0        0     0
## 374 13719.24   23645        4     2      0     0       0     0        0     0
## 375 14077.97   26788        4     2      0     0       1     0        0     0
## 376 14411.86   26986        4     2      1     0       1     0        0     0
## 377 13991.04   21020        4     2      0     1       0     0        0     0
## 378 15033.15   19455        4     2      0     0       1     0        0     0
## 379 14771.00   22255        4     2      1     0       1     0        0     0
## 380 13518.24   25981        4     2      1     1       0     0        0     0
## 381 13825.15   26034        4     2      0     0       1     0        0     0
## 382 16256.24   10555        4     2      0     0       1     0        0     0
## 383 13869.15   24349        4     2      1     1       0     0        0     0
## 384 16916.87    2879        4     2      1     0       1     0        0     0
## 385 13811.16   26236        4     2      0     1       1     0        0     0
## 386 11539.05   38958        4     2      0     0       0     0        0     0
## 387 18566.07   24747        6     4      1     1       1     0        0     0
## 388 18856.02   22423        6     4      1     1       0     0        0     0
## 389 19682.04   11554        6     4      1     0       1     0        0     0
## 390 20318.89   17583        6     4      1     1       0     0        0     0
## 391 17844.73   21121        6     4      1     1       0     0        0     0
## 392 18678.41   16496        6     4      1     1       0     0        0     0
## 393 17768.06   28385        6     4      1     1       1     0        0     0
## 394 13961.11   19602        4     4      0     1       1     0        0     0
## 395 15077.18   13262        4     4      0     1       1     0        0     0
## 396 13034.07   23976        4     4      0     1       1     0        0     0
## 397 15604.15    5788        4     4      1     1       1     0        0     0
## 398 15979.01    3946        4     4      1     1       1     0        0     0
## 399 14841.92   12420        4     4      0     1       1     0        0     0
## 400 16379.85    4188        4     4      1     1       1     0        0     0
## 401 15709.05   22236        6     4      1     1       0     1        0     0
## 402 15048.04   22964        6     4      1     1       0     1        0     0
## 403 15295.02   27325        6     4      1     1       1     1        0     0
## 404 16339.17   19832        6     4      1     0       1     1        0     0
## 405 17542.04    9135        6     4      1     1       0     1        0     0
## 406 16218.85   13196        6     4      1     1       0     1        0     0
## 407 17314.10    8221        6     4      1     1       1     1        0     0
## 408 21831.82   25564        6     4      1     1       1     1        0     0
## 409 23420.71   18910        6     4      1     0       1     1        0     0
## 410 25098.63   10014        6     4      1     1       0     1        0     0
## 411 23077.57   23798        6     4      1     0       1     1        0     0
## 412 22435.20   22287        6     4      1     1       1     1        0     0
## 413 25589.98    2308        6     4      1     1       0     1        0     0
## 414 46747.67   15343        8     4      1     1       1     0        1     0
## 415 51154.05    2202        8     4      1     1       1     0        1     0
## 416 42677.60   24052        8     4      1     0       1     0        1     0
## 417 43892.47   23371        8     4      1     0       1     0        1     0
## 418 44300.64   23751        8     4      1     0       1     0        1     0
## 419 40619.07   30082        8     4      1     1       1     0        1     0
## 420 44084.91   21367        8     4      1     1       1     0        1     0
## 421 30792.15   17870        6     4      1     1       1     0        1     0
## 422 29275.21   21056        6     4      1     1       1     0        1     0
## 423 30392.75   18449        6     4      1     1       1     0        1     0
## 424 30957.08   10625        6     4      1     1       1     0        1     0
## 425 30781.52   14937        6     4      1     1       1     0        1     0
## 426 33417.97    6598        6     4      1     1       1     0        1     0
## 427 28040.13   27484        6     4      1     1       1     0        1     0
## 428 31181.72   26222        8     4      1     0       1     0        1     0
## 429 33220.03   18661        8     4      1     0       1     0        1     0
## 430 32501.25   17508        8     4      1     0       1     0        1     0
## 431 32509.48   20910        8     4      1     0       1     0        1     0
## 432 31059.18   27544        8     4      1     1       1     0        1     0
## 433 31132.21   23124        8     4      1     1       1     0        1     0
## 434 35715.77    6447        8     4      1     0       1     0        1     0
## 435 36633.63   22042        6     4      1     1       1     0        1     0
## 436 38297.46   16754        6     4      1     0       1     0        1     0
## 437 42741.52    2846        6     4      1     0       1     0        1     0
## 438 40966.61    7476        6     4      1     1       1     0        1     0
## 439 32038.34   35326        6     4      1     1       1     0        1     0
## 440 37192.90   19100        6     4      1     0       1     0        1     0
## 441 34974.38   25796        6     4      1     1       1     0        1     0
## 442 44205.88   15104        8     4      1     0       1     0        1     0
## 443 42377.96   18581        8     4      1     0       1     0        1     0
## 444 48310.33     788        8     4      1     0       1     0        1     0
## 445 41516.43   23861        8     4      1     1       1     0        1     0
## 446 41671.58   20575        8     4      1     0       1     0        1     0
## 447 41053.48   25717        8     4      1     1       1     0        1     0
## 448 38208.50   31303        8     4      1     1       1     0        1     0
## 449 13072.84   14311        4     4      0     1       1     0        0     1
## 450 11539.05   24163        4     4      0     1       1     0        0     1
## 451 11699.03   19816        4     4      1     1       1     0        0     1
## 452 11574.17   21525        4     4      0     1       1     0        0     1
## 453 12243.06   25014        4     4      1     1       1     0        0     1
## 454 11671.86   25727        4     4      1     1       1     0        0     1
## 455 14061.12    4922        4     4      0     1       1     0        0     1
## 456 15553.21    7695        4     4      0     1       1     0        0     1
## 457 11581.91   36566        4     4      0     1       1     0        0     1
## 458 13161.94   21145        4     4      1     1       1     0        0     1
## 459 14077.97   17445        4     4      0     1       1     0        0     1
## 460 15047.00   12305        4     4      1     1       1     0        0     1
## 461 12981.95   20309        4     4      1     1       1     0        0     1
## 462 14220.01   23069        4     4      1     1       1     0        0     1
## 463 13159.82   22740        4     4      1     1       1     0        0     1
## 464 12678.85   28683        4     4      0     1       1     0        0     1
## 465 13994.91   17270        4     4      0     1       1     0        0     1
## 466 14194.82    9561        4     4      0     1       1     0        0     1
## 467 14696.03    6709        4     4      1     1       1     0        0     1
## 468 14582.77    7115        4     4      1     1       1     0        0     1
## 469 12495.97   26204        4     4      0     1       1     0        0     1
## 470 20021.20    1787        6     4      1     0       0     0        0     1
## 471 15554.28   33357        6     4      1     1       1     0        0     1
## 472 16644.09   22383        6     4      1     1       1     0        0     1
## 473 18835.19    8211        6     4      1     0       1     0        0     1
## 474 17154.58   21567        6     4      1     0       1     0        0     1
## 475 15951.81   26070        6     4      1     1       1     0        0     1
## 476 16508.59   27460        6     4      1     1       1     0        0     1
## 477 16646.77   20154        6     4      0     1       1     0        0     1
## 478 17218.69   14579        6     4      1     1       1     0        0     1
## 479 17089.92    8732        6     4      0     1       1     0        0     1
## 480 17463.05   11393        6     4      1     1       1     0        0     1
## 481 15680.86   25956        6     4      0     1       1     0        0     1
## 482 16752.51   18562        6     4      1     1       1     0        0     1
## 483 15664.62   25787        6     4      1     1       1     0        0     1
## 484 18620.87   25516        6     4      1     0       1     0        0     0
## 485 20452.67   10338        6     4      1     0       0     0        0     0
## 486 20677.59   11204        6     4      1     1       0     0        0     0
## 487 21383.07    7287        6     4      1     1       1     0        0     0
## 488 19294.79   19539        6     4      1     0       0     0        0     0
## 489 18042.22   21702        6     4      1     0       0     0        0     0
## 490 16216.98   35624        6     4      1     1       0     0        0     0
## 491 15724.25   23989        6     4      1     0       0     0        0     0
## 492 19822.12    1592        6     4      1     1       0     0        0     0
## 493 15756.15   29325        6     4      1     1       0     0        0     0
## 494 15979.01   21974        6     4      1     0       0     0        0     0
## 495 16256.24   22637        6     4      1     0       0     0        0     0
## 496 16041.69   27800        6     4      1     1       0     0        0     0
## 497 19567.26    2189        6     4      1     1       1     0        0     0
## 498 15110.19    2392        4     4      1     1       0     0        0     0
## 499 12036.22   19853        4     4      0     1       1     0        0     0
## 500 12099.01   22758        4     4      1     1       0     0        0     0
## 501 12284.29   24740        4     4      1     1       0     0        0     0
## 502 14203.00   11836        4     4      0     0       0     0        0     0
## 503 14116.92   12878        4     4      1     0       0     0        0     0
## 504 12791.75   16163        4     4      0     0       1     0        0     0
## 505 14739.07    1737        4     4      0     0       1     0        0     0
## 506 12965.22   29707        4     4      0     1       1     0        0     0
## 507 12412.52   24664        4     4      0     1       1     0        0     0
## 508 10563.07   32458        4     4      0     0       0     0        0     0
## 509 12333.60   21877        4     4      0     0       0     0        0     0
## 510 12119.09   22826        4     4      0     0       1     0        0     0
## 511 11521.53   34998        4     4      0     1       1     0        0     0
## 512 12105.98   28298        4     4      0     1       0     0        0     0
## 513 12162.14   21770        4     4      0     1       0     0        0     0
## 514 13122.91   19101        4     4      0     1       1     0        0     0
## 515 13494.29   19500        4     4      1     0       1     0        0     0
## 516 13174.07   13318        4     4      0     0       1     0        0     0
## 517 13216.91   24069        4     4      0     1       0     0        0     0
## 518 12594.18   26328        4     4      0     0       1     0        0     0
## 519 11679.92   23388        4     4      0     0       0     0        0     0
## 520 20830.99   19419        6     4      1     1       0     0        0     0
## 521 21607.77   11069        6     4      1     0       1     0        0     0
## 522 22004.93   15516        6     4      1     1       1     0        0     0
## 523 19116.13   26252        6     4      1     0       1     0        0     0
## 524 23197.44    2295        6     4      1     1       1     0        0     0
## 525 20109.90   22891        6     4      1     1       1     0        0     0
## 526 23102.02    5653        6     4      1     0       0     0        0     0
## 527 26781.81   12052        6     4      1     1       0     1        0     0
## 528 21536.74   37128        6     4      1     1       1     1        0     0
## 529 26190.27   17335        6     4      1     0       1     1        0     0
## 530 26060.34    9795        6     4      1     1       0     1        0     0
## 531 23159.54   25869        6     4      1     0       0     1        0     0
## 532 26302.07   13050        6     4      1     1       0     1        0     0
## 533 23348.02   24027        6     4      1     0       1     1        0     0
## 534 16706.67    9150        4     4      0     0       0     0        0     0
## 535 16927.78    2973        4     4      0     0       0     0        0     0
## 536 14909.05   23323        4     4      0     1       1     0        0     0
## 537 16379.10    8754        4     4      0     1       0     0        0     0
## 538 15622.12   23217        4     4      1     1       1     0        0     0
## 539 17418.07    4463        4     4      1     0       1     0        0     0
## 540 15821.95   14304        4     4      0     0       1     0        0     0
## 541 23573.82   12466        6     2      1     1       0     0        0     1
## 542 21020.84   25550        6     2      1     1       1     0        0     1
## 543 23527.73   14948        6     2      1     1       0     0        0     1
## 544 20047.95   24665        6     2      1     1       0     0        0     1
## 545 22470.36   22626        6     2      1     1       0     0        0     1
## 546 20619.11   24067        6     2      1     0       0     0        0     1
## 547 21525.34   25020        6     2      1     1       1     0        0     1
## 548 27714.05    5379        6     4      1     1       0     0        0     1
## 549 25948.96     636        6     4      1     0       0     0        0     1
## 550 22064.29   27384        6     4      1     1       1     0        0     1
## 551 23151.55   27940        6     4      1     0       0     0        0     1
## 552 20294.58   33892        6     4      1     0       0     0        0     1
## 553 22894.44   26272        6     4      1     1       1     0        0     1
## 554 24809.04   16111        6     4      1     0       0     0        0     1
## 555 10354.04   14521        4     4      0     1       0     0        0     1
## 556 10287.98   16521        4     4      1     1       1     0        0     1
## 557 10897.08    6699        4     4      0     1       1     0        0     1
## 558  9789.04   22986        4     4      0     1       1     0        0     1
## 559 10106.02   14200        4     4      1     0       0     0        0     1
## 560  9506.05   22169        4     4      0     0       1     0        0     1
## 561  9220.83   29992        4     4      1     0       1     0        0     1
## 562  8769.00   35299        4     4      0     0       0     0        0     1
## 563  8870.95   32914        4     4      1     1       0     0        0     1
## 564 10315.02   14438        4     4      0     0       1     0        0     1
## 565  9654.06   19183        4     4      0     0       0     0        0     1
## 566  9041.91   26191        4     4      0     0       1     0        0     1
## 567 10971.10    7091        4     4      1     0       0     0        0     1
## 568  8638.93   25216        4     4      0     0       0     0        0     1
## 569 29612.15   32477        4     2      1     1       1     0        0     0
## 570 33586.91   18930        4     2      1     0       1     0        0     0
## 571 37088.56    3828        4     2      1     1       1     0        0     0
## 572 24405.07   31344        4     4      1     1       1     0        0     0
## 573 27703.20   24738        4     4      1     1       1     0        0     0
## 574 25618.28   20208        4     4      1     0       1     0        0     0
## 575 23733.40   27600        4     4      1     1       0     0        0     0
## 576 28204.60   22021        4     4      1     1       1     0        0     0
## 577 27284.75   14281        4     4      1     1       1     0        0     0
## 578 30959.93   17673        4     4      1     1       1     0        0     0
## 579 28328.27   20685        4     4      1     0       1     0        0     0
## 580 26012.37   34269        4     4      1     1       1     0        0     0
## 581 33984.43   25420        4     2      1     0       0     0        0     0
## 582 38167.17   13162        4     2      1     0       1     0        0     0
## 583 36338.75   18195        4     2      1     1       0     0        0     0
## 584 25267.37   34175        4     4      1     1       0     0        0     0
## 585 32053.10    5144        4     4      1     1       0     0        0     0
## 586 26789.83   22189        4     4      1     0       0     0        0     0
## 587 30271.92   23426        4     4      1     1       1     0        0     0
## 588 26955.04   31773        4     4      1     0       1     0        0     0
## 589 32649.76   16975        4     4      1     1       1     0        0     0
## 590 16106.83   19465        4     4      1     0       0     0        0     0
## 591 15174.35   27887        4     4      0     0       0     0        0     0
## 592 16033.93   18391        4     4      0     1       1     0        0     0
## 593 38824.87   25960        8     2      1     0       1     0        0     1
## 594 44749.69   12115        8     2      1     0       1     0        0     1
## 595 39691.73   25169        8     2      1     1       1     0        0     1
## 596 13135.91   21796        4     2      1     1       1     0        0     1
## 597 12045.92   19136        4     2      0     1       1     0        0     1
## 598 39092.19   10717        8     2      1     1       1     0        0     1
## 599 34739.21   25747        8     2      1     0       1     0        0     1
## 600 35575.42   22740        8     2      1     0       1     0        0     1
## 601 13106.90   21910        4     2      0     1       1     0        0     1
## 602 12487.05   28492        4     2      0     1       1     0        0     1
## 603 11539.85   22405        4     2      0     1       1     0        0     1
## 604 12469.53   19712        4     2      0     1       1     0        0     1
## 605 27425.84   23886        8     2      1     0       1     0        0     0
## 606 25527.01   36480        8     2      1     1       1     0        0     0
## 607 12830.10   17830        4     2      1     1       1     0        0     0
## 608 12209.56   26097        4     2      0     1       1     0        0     0
## 609 32422.76    9185        8     2      1     1       1     0        0     0
## 610 12878.05   19225        4     2      1     1       1     0        0     0
## 611 19027.86   21797        6     4      1     0       1     1        0     0
## 612 17944.86   19592        6     4      1     0       0     1        0     0
## 613 17645.75   27830        6     4      1     1       1     1        0     0
## 614 21335.85   10237        6     4      1     0       0     1        0     0
## 615 17968.84   34665        6     4      1     1       1     1        0     0
## 616 19105.13   24008        6     4      1     0       0     1        0     0
## 617 21460.01   19467        6     4      1     0       1     1        0     0
## 618 21273.06   18908        6     4      1     0       0     1        0     0
## 619 20406.10   22596        6     4      1     0       0     1        0     0
## 620 22230.03   22102        6     4      1     0       1     1        0     0
## 621 20902.10   29649        6     4      1     1       1     1        0     0
## 622 22625.07   23612        6     4      1     0       1     1        0     0
## 623 35866.58   24415        8     4      1     1       1     0        1     0
## 624 38445.90   18661        8     4      1     0       1     0        1     0
## 625 34685.66   25421        8     4      1     0       1     0        1     0
## 626 42820.33    5499        8     4      1     0       1     0        1     0
## 627 36332.89   25153        8     4      1     0       1     0        1     0
## 628 41419.04   14452        8     4      1     0       1     0        1     0
## 629 15595.88   18315        6     2      0     1       1     0        0     0
## 630 17360.81     881        6     2      0     1       1     0        0     0
## 631 15594.81   22414        6     2      0     1       1     0        0     0
## 632 17095.04   18720        6     4      1     0       0     0        0     0
## 633 14963.05   31471        6     4      1     1       0     0        0     0
## 634 16997.69   25830        6     4      1     0       1     0        0     0
## 635 22104.97    9049        6     4      1     1       1     0        0     0
## 636 22736.83    5690        6     4      1     1       1     0        0     0
## 637 22311.05   11221        6     4      1     1       1     0        0     0
## 638 15086.90   27438        4     4      0     0       1     0        0     0
## 639 15589.78   21307        4     4      0     0       0     0        0     0
## 640 17803.28   12303        4     4      1     1       1     0        0     0
## 641 21300.02   12772        6     4      1     1       0     0        0     0
## 642 19423.17   25557        6     4      1     0       1     0        0     0
## 643 19956.76   26028        6     4      1     1       0     0        0     0
## 644 25452.47   11892        8     4      1     0       1     0        0     0
## 645 21765.07   25794        8     4      1     0       1     0        0     0
## 646 21982.65   20472        8     4      1     1       1     0        0     0
## 647 55639.09   31805        8     2      1     0       1     0        1     0
## 648 65281.48   15600        8     2      1     1       1     0        1     0
## 649 57154.44   29260        8     2      1     1       1     0        1     0
## 650 18490.98    7755        6     4      1     1       1     0        0     0
## 651 16175.96   19095        6     4      1     1       0     0        0     0
## 652 18173.98    5826        6     4      1     1       1     0        0     0
## 653 21562.05   23767        6     4      1     0       1     1        0     0
## 654 19641.74   31324        6     4      1     1       1     1        0     0
## 655 21575.46   20137        6     4      1     1       0     1        0     0
## 656 34355.00   17711        4     2      1     1       0     0        0     0
## 657 33287.41   21661        4     2      1     0       1     0        0     0
## 658 31970.54   21208        4     2      1     1       0     0        0     0
## 659 24896.60   21266        4     4      1     1       0     0        0     0
## 660 24063.01   27674        4     4      1     1       1     0        0     0
## 661 26337.83   16068        4     4      1     0       0     0        0     0
## 662 25845.21   36557        4     4      1     1       1     0        0     0
## 663 30661.26   14278        4     4      1     0       1     0        0     0
## 664 30322.15   16225        4     4      1     1       1     0        0     0
## 665 15635.80    1169        4     2      0     1       1     0        0     1
## 666 17685.20   15898        6     2      1     0       0     0        0     1
## 667 14584.45    1160        4     2      1     1       1     0        0     1
## 668 15503.51   33345        6     2      1     0       0     0        0     1
## 669 13681.70   10210        4     2      1     1       1     0        0     1
## 670 18910.80    8345        6     2      1     0       1     0        0     1
## 671 12327.64   19347        4     2      0     1       1     0        0     1
## 672 14619.08   15768        4     2      1     1       1     0        0     1
## 673 13310.06   26143        4     2      1     1       1     0        0     1
## 674  9928.19   29680        4     4      0     0       1     0        0     1
## 675 11137.05   22484        4     4      0     1       1     0        0     1
## 676 11045.11   24568        4     4      1     0       1     0        0     1
## 677 18950.91    8687        6     4      0     1       1     0        0     1
## 678 16723.99   19740        6     4      1     1       1     0        0     1
## 679 18957.89    5936        6     4      0     1       1     0        0     1
## 680 11555.27   13404        4     4      1     1       0     0        0     1
## 681 18083.40   29420        6     4      1     1       1     0        0     1
## 682 11080.52   36855        4     4      0     1       1     0        0     1
## 683 13471.01   18910        4     4      1     1       1     0        0     1
## 684 14198.09   11322        4     4      1     1       1     0        0     1
## 685 19409.75   18795        6     4      1     1       1     0        0     1
## 686 19528.10   14115        6     4      1     1       0     0        0     1
## 687 15000.99   14504        4     4      0     1       1     0        0     1
## 688  9954.05   37345        4     4      0     1       1     0        0     1
## 689 18800.96    7961        6     4      0     1       1     0        0     1
## 690 15128.99   13828        4     4      1     1       1     0        0     1
## 691 14997.88    8880        4     4      0     1       1     0        0     1
## 692 15233.16   32535        6     4      0     1       1     0        0     1
## 693 17458.22   15144        6     4      1     1       1     0        0     1
## 694 10144.95   23963        4     4      1     1       0     0        0     1
## 695 14397.93    5189        4     2      0     1       1     0        0     1
## 696 12733.86   21386        4     2      1     1       1     0        0     1
## 697 13678.86   17971        4     2      1     1       1     0        0     1
## 698 14275.13   18533        4     4      1     1       1     0        0     1
## 699 14222.31    8427        4     4      0     1       1     0        0     1
## 700 13762.90   18040        4     4      1     1       1     0        0     1
## 701 20537.14   16950        6     2      1     0       0     0        0     1
## 702 21233.91   17337        6     2      1     1       1     0        0     1
## 703 18876.87   27218        6     2      1     1       0     0        0     1
## 704 11115.01   30056        4     4      1     1       1     0        0     1
## 705 11247.86   21427        4     4      1     1       1     0        0     1
## 706 11394.89   25107        4     4      0     1       1     0        0     1
## 707 17115.12   24461        6     4      0     1       1     0        0     1
## 708 18004.87   18771        6     4      0     1       1     0        0     1
## 709 16803.12   25874        6     4      1     1       1     0        0     1
## 710 17312.91   21420        6     4      1     1       1     0        0     1
## 711 11169.92   22380        4     4      0     1       0     0        0     1
## 712 16428.58    9882        4     4      0     1       1     0        0     1
## 713 14191.88   21181        4     4      1     1       1     0        0     1
## 714 10921.95   27776        4     4      1     0       0     0        0     1
## 715 14175.88   21627        4     4      0     1       1     0        0     1
## 716 11615.02   19014        4     4      0     1       1     0        0     1
## 717 16341.80   25394        6     4      0     1       1     0        0     1
## 718 16713.98   26328        6     4      1     1       1     0        0     1
## 719 17173.94   18721        6     4      0     1       1     0        0     1
## 720 17986.22   17488        6     4      0     1       1     0        0     1
## 721 16456.97   20200        6     4      0     1       1     0        0     1
## 722 13600.03   27231        4     2      0     1       0     0        0     0
## 723 15395.01   12920        4     2      1     0       0     0        0     0
## 724 15277.07   16951        4     2      0     1       0     0        0     0
## 725 13032.87   30108        4     2      1     1       1     0        0     0
## 726 14702.80   17656        4     2      1     1       0     0        0     0
## 727 15639.04    8507        4     2      1     0       0     0        0     0
## 728 15327.10    4318        4     4      0     1       1     0        0     0
## 729 20127.04   18419        6     4      1     1       1     0        0     0
## 730 15846.01    5350        4     4      0     1       1     0        0     0
## 731 13162.85   24542        4     4      0     1       1     0        0     0
## 732 18063.00   27574        6     4      1     0       0     0        0     0
## 733 19751.04   20510        6     4      1     0       1     0        0     0
## 734 23493.08   20453        6     4      1     1       0     1        0     0
## 735 21878.12   23237        6     4      1     1       0     1        0     0
## 736 21698.01   25489        6     4      1     1       1     1        0     0
## 737 16336.91   16342        6     4      1     0       0     1        0     0
## 738 14862.09   24021        6     4      1     0       1     1        0     0
## 739 15230.00   22576        6     4      1     1       0     1        0     0
## 740 49248.16    6685        8     4      1     0       1     0        1     0
## 741 35129.34   11975        8     4      1     1       1     0        1     0
## 742 44130.62   21341        8     4      1     0       1     0        1     0
## 743 31431.13   11013        6     4      1     1       1     0        1     0
## 744 48365.98    2616        8     4      1     1       1     0        1     0
## 745 43374.05   25199        8     4      1     1       1     0        1     0
## 746 36210.12   21778        6     4      1     0       1     0        1     0
## 747 39072.39   31587        8     4      1     0       1     0        1     0
## 748 35651.68   10555        8     4      1     1       1     0        1     0
## 749 30646.44   17094        6     4      1     1       1     0        1     0
## 750 38795.38   13973        6     4      1     1       1     0        1     0
## 751 35165.76   13449        8     4      1     1       1     0        1     0
## 752 35895.50   23056        6     4      1     1       1     0        1     0
## 753 45061.95   13829        8     4      1     1       1     0        1     0
## 754 28817.08   21039        6     4      1     0       1     0        1     0
## 755 14072.14   15233        4     4      0     1       1     0        0     1
## 756 13436.00   20530        4     4      0     1       1     0        0     1
## 757 17162.48   15903        6     4      0     1       1     0        0     1
## 758 10546.78   38866        4     4      1     1       1     0        0     1
## 759 13540.04   17343        4     4      0     1       1     0        0     1
## 760 12379.13   31199        4     4      1     1       1     0        0     1
## 761 14429.79    6114        4     4      1     1       1     0        0     1
## 762 13830.25   17594        4     4      0     1       1     0        0     1
## 763 12257.16   21492        4     4      1     1       1     0        0     1
## 764 18727.51   14054        6     4      1     1       1     0        0     1
## 765 15623.92   21272        6     4      1     1       1     0        0     1
## 766 16507.07   17451        6     4      0     1       1     0        0     1
## 767 11464.63   29410        4     4      1     1       1     0        0     1
## 768 16805.06   19498        6     4      1     0       0     0        0     1
## 769 15832.52   31202        6     4      1     1       1     0        0     1
## 770 16853.11   17959        6     4      1     0       0     0        0     0
## 771 16516.96   20751        6     4      1     1       1     0        0     0
## 772 15792.83   41566        6     4      1     1       1     0        0     0
## 773 18548.98   20870        6     4      1     0       0     0        0     0
## 774 17023.94   30404        6     4      1     0       0     0        0     0
## 775 15967.25   25598        6     4      1     0       0     0        0     0
## 776 12293.06   17139        4     4      1     0       1     0        0     0
## 777 11504.82   33962        4     4      0     0       0     0        0     0
## 778 11873.53   28398        4     4      0     0       0     0        0     0
## 779 14568.00   18511        4     4      0     1       1     0        0     0
## 780 13998.13   18257        4     4      0     1       1     0        0     0
## 781 13258.37   14938        4     4      1     1       1     0        0     0
## 782 11413.53   32619        4     4      1     0       1     0        0     0
## 783 15194.98   12412        4     4      0     0       1     0        0     0
## 784 19689.74   27077        6     4      1     1       0     0        0     0
## 785 22460.53    8928        6     4      1     0       1     0        0     0
## 786 19338.38   27966        6     4      1     0       1     0        0     0
## 787 26831.19    4695        6     4      1     1       0     1        0     0
## 788 25508.21   17480        6     4      1     1       0     1        0     0
## 789 23406.69   25387        6     4      1     0       1     1        0     0
## 790 17214.33   12610        4     4      1     1       1     0        0     0
## 791 14853.20   24270        4     4      0     0       0     0        0     0
## 792 14398.92   21688        4     4      0     0       0     0        0     0
## 793 20221.81   26223        6     2      1     1       1     0        0     1
## 794 20382.15   25240        6     2      1     1       1     0        0     1
## 795 22113.63   21992        6     2      1     1       0     0        0     1
## 796 25097.47   14461        6     4      1     1       1     0        0     1
## 797 22120.76   28242        6     4      1     1       1     0        0     1
## 798 23345.33   22964        6     4      1     1       1     0        0     1
## 799 11167.86    4716        4     4      1     1       0     0        0     1
## 801  9720.98   20836        4     4      1     1       0     0        0     1
## 802  9482.22   24842        4     4      1     0       0     0        0     1
## 803  9563.79   19273        4     4      1     1       0     0        0     1
## 804  9665.85   19565        4     4      0     1       1     0        0     1
##     Pontiac Saab Saturn convertible coupe hatchback sedan wagon velocidad
## 1         0    0      0           0     0         0     1     0  36469.49
## 2         0    0      0           0     1         0     0     0  34963.08
## 3         0    1      0           1     0         0     0     0  46900.74
## 4         0    1      0           1     0         0     0     0  49458.36
## 5         0    1      0           1     0         0     0     0  53685.84
## 6         0    1      0           1     0         0     0     0  48787.63
## 7         0    1      0           1     0         0     0     0  53722.93
## 8         0    1      0           1     0         0     0     0  48684.39
## 9         0    1      0           1     0         0     0     0  48548.93
## 10        0    1      0           0     0         0     1     0  43549.16
## 11        0    1      0           0     0         0     1     0  43196.61
## 12        0    1      0           0     0         0     1     0  41505.88
## 13        0    1      0           0     0         0     1     0  40472.47
## 14        0    1      0           0     0         0     1     0  38903.60
## 15        0    1      0           0     0         0     1     0  39996.30
## 16        0    1      0           0     0         0     1     0  44781.61
## 17        0    1      0           0     0         0     1     0  42966.48
## 18        0    1      0           0     0         0     1     0  45360.70
## 19        0    1      0           0     0         0     1     0  43840.93
## 20        0    1      0           0     0         0     1     0  49568.95
## 21        0    1      0           0     0         0     1     0  45731.95
## 22        0    1      0           0     0         0     1     0  42894.31
## 23        0    1      0           0     0         0     1     0  44435.46
## 24        0    1      0           0     0         0     0     1  44721.84
## 25        0    1      0           0     0         0     0     1  48259.15
## 26        0    1      0           0     0         0     0     1  46989.38
## 27        0    1      0           0     0         0     0     1  48132.64
## 28        0    1      0           0     0         0     0     1  47446.01
## 29        0    1      0           0     0         0     0     1  43118.11
## 30        0    1      0           0     0         0     0     1  47187.79
## 31        0    1      0           1     0         0     0     0  60163.03
## 32        0    1      0           1     0         0     0     0  55350.27
## 33        0    1      0           1     0         0     0     0  61677.92
## 34        0    1      0           1     0         0     0     0  56817.18
## 35        0    1      0           1     0         0     0     0  46313.73
## 36        0    1      0           1     0         0     0     0  53508.12
## 37        0    1      0           1     0         0     0     0  57261.10
## 38        0    1      0           0     0         0     1     0  48722.52
## 39        0    1      0           0     0         0     1     0  48477.45
## 40        0    1      0           0     0         0     1     0  51816.70
## 41        0    1      0           0     0         0     1     0  43628.45
## 42        0    1      0           0     0         0     1     0  48849.46
## 43        0    1      0           0     0         0     1     0  43865.15
## 44        0    1      0           0     0         0     1     0  45531.26
## 45        0    1      0           0     0         0     1     0  40092.18
## 46        0    1      0           0     0         0     1     0  53403.50
## 47        0    1      0           0     0         0     1     0  45871.16
## 48        0    1      0           0     0         0     1     0  56380.61
## 49        0    1      0           0     0         0     1     0  48029.68
## 50        0    1      0           0     0         0     1     0  48402.71
## 51        0    1      0           0     0         0     1     0  45150.20
## 52        0    1      0           0     0         0     1     0  45758.28
## 53        0    1      0           0     0         0     1     0  52699.89
## 54        0    1      0           0     0         0     1     0  49894.15
## 55        0    1      0           0     0         0     0     1  46395.92
## 56        0    1      0           0     0         0     0     1  51256.59
## 57        0    1      0           0     0         0     0     1  50782.00
## 58        0    1      0           0     0         0     0     1  46152.98
## 59        0    1      0           0     0         0     0     1  48218.05
## 60        0    1      0           0     0         0     0     1  49206.19
## 61        0    1      0           0     0         0     0     1  48142.62
## 62        1    0      0           0     0         0     0     1  27882.37
## 63        1    0      0           0     0         0     0     1  26380.30
## 64        1    0      0           0     0         0     0     1  24953.39
## 65        1    0      0           0     0         0     0     1  24619.53
## 66        1    0      0           0     0         0     0     1  25876.16
## 67        1    0      0           0     0         0     0     1  26636.66
## 68        1    0      0           0     0         0     0     1  23410.98
## 69        0    0      0           1     0         0     0     0  60010.85
## 70        0    0      0           1     0         0     0     0  75208.99
## 71        0    0      0           1     0         0     0     0  75744.26
## 72        0    0      0           1     0         0     0     0  66580.91
## 73        0    0      0           1     0         0     0     0  63645.80
## 74        0    0      0           1     0         0     0     0  68836.65
## 75        0    0      0           1     0         0     0     0  59499.01
## 76        0    0      0           0     1         0     0     0  18029.76
## 77        0    0      0           0     1         0     0     0  20988.90
## 78        0    0      0           0     1         0     0     0  19646.70
## 79        0    0      0           0     1         0     0     0  18871.20
## 80        0    0      0           0     1         0     0     0  20934.35
## 81        0    0      0           0     1         0     0     0  17943.61
## 82        0    0      0           0     1         0     0     0  17363.20
## 83        0    0      0           0     1         0     0     0  20229.72
## 84        0    0      0           0     1         0     0     0  22569.39
## 85        0    0      0           0     1         0     0     0  20449.83
## 86        0    0      0           0     1         0     0     0  19250.40
## 87        0    0      0           0     1         0     0     0  20617.20
## 88        0    0      0           0     1         0     0     0  20672.34
## 89        0    0      0           0     1         0     0     0  20757.25
## 90        0    0      0           0     1         0     0     0  63913.08
## 91        0    0      0           0     1         0     0     0  64174.08
## 92        0    0      0           0     1         0     0     0  50190.29
## 93        0    0      0           0     1         0     0     0  63353.36
## 94        0    0      0           0     1         0     0     0  55196.28
## 95        0    0      0           0     1         0     0     0  62749.42
## 96        0    0      0           0     1         0     0     0  56747.90
## 97        1    0      0           0     1         0     0     0  44049.37
## 98        1    0      0           0     1         0     0     0  49929.78
## 99        1    0      0           0     1         0     0     0  45870.11
## 100       1    0      0           0     1         0     0     0  51852.50
## 101       1    0      0           0     1         0     0     0  47629.90
## 102       1    0      0           0     1         0     0     0  47740.80
## 103       1    0      0           0     1         0     0     0  44335.31
## 104       1    0      0           0     1         0     0     0  20059.01
## 105       1    0      0           0     1         0     0     0  21179.20
## 106       1    0      0           0     1         0     0     0  20651.88
## 107       1    0      0           0     1         0     0     0  19728.76
## 108       1    0      0           0     1         0     0     0  20061.33
## 109       1    0      0           0     1         0     0     0  20644.75
## 110       1    0      0           0     1         0     0     0  19156.22
## 111       0    0      0           0     0         0     1     0  28567.33
## 112       0    0      0           0     0         0     1     0  29201.81
## 113       0    0      0           0     0         0     1     0  28602.88
## 114       0    0      0           0     0         0     1     0  29529.75
## 115       0    0      0           0     0         0     1     0  27993.02
## 116       0    0      0           0     0         0     1     0  32346.69
## 117       0    0      0           0     0         0     1     0  33310.39
## 118       0    0      0           0     0         0     1     0  32064.89
## 119       0    0      0           0     0         0     1     0  28659.57
## 120       0    0      0           0     0         0     1     0  31823.63
## 121       0    0      0           0     0         0     1     0  33052.92
## 122       0    0      0           0     0         0     1     0  31131.48
## 123       0    0      0           0     0         0     1     0  29842.82
## 124       0    0      0           0     0         0     1     0  33011.07
## 125       0    0      0           0     0         0     1     0  38279.80
## 126       0    0      0           0     0         0     1     0  35983.20
## 127       0    0      0           0     0         0     1     0  35237.88
## 128       0    0      0           0     0         0     1     0  36896.04
## 129       0    0      0           0     0         0     1     0  33889.86
## 130       0    0      0           0     0         0     1     0  31473.84
## 131       0    0      0           0     0         0     1     0  34090.99
## 132       0    0      0           0     0         0     1     0  34345.49
## 133       0    0      0           0     0         0     1     0  33773.79
## 134       0    0      0           0     0         0     1     0  34895.52
## 135       0    0      0           0     0         0     1     0  35082.43
## 136       0    0      0           0     0         0     1     0  37735.47
## 137       0    0      0           0     0         0     1     0  37040.75
## 138       0    0      0           0     0         0     1     0  37895.68
## 139       0    0      0           0     0         0     1     0  62121.18
## 140       0    0      0           0     0         0     1     0  58184.82
## 141       0    0      0           0     0         0     1     0  58061.70
## 142       0    0      0           0     0         0     1     0  64914.21
## 143       0    0      0           0     0         0     1     0  56872.15
## 144       0    0      0           0     0         0     1     0  63258.62
## 145       0    0      0           0     0         0     1     0  64054.51
## 146       0    0      0           0     0         0     1     0  52363.63
## 147       0    0      0           0     0         0     1     0  60367.01
## 148       0    0      0           0     0         0     1     0  53034.65
## 149       0    0      0           0     0         0     1     0  58331.04
## 150       0    0      0           0     0         0     1     0  65752.11
## 151       0    0      0           0     0         0     1     0  66591.64
## 152       0    0      0           0     0         0     1     0  59892.13
## 153       1    0      0           0     1         0     0     0  27685.32
## 154       1    0      0           0     1         0     0     0  28446.56
## 155       1    0      0           0     1         0     0     0  24548.77
## 156       1    0      0           0     1         0     0     0  23662.46
## 157       1    0      0           0     1         0     0     0  24235.37
## 158       1    0      0           0     1         0     0     0  27025.25
## 159       1    0      0           0     1         0     0     0  27587.33
## 160       1    0      0           0     0         0     1     0  30907.21
## 161       1    0      0           0     0         0     1     0  29820.14
## 162       1    0      0           0     0         0     1     0  33197.06
## 163       1    0      0           0     0         0     1     0  29222.65
## 164       1    0      0           0     0         0     1     0  31447.03
## 165       1    0      0           0     0         0     1     0  35204.63
## 166       1    0      0           0     0         0     1     0  35250.04
## 167       1    0      0           0     0         0     1     0  25408.53
## 168       1    0      0           0     0         0     1     0  27621.01
## 169       1    0      0           0     0         0     1     0  26665.50
## 170       1    0      0           0     0         0     1     0  26379.08
## 171       1    0      0           0     0         0     1     0  24875.95
## 172       1    0      0           0     0         0     1     0  26206.54
## 173       1    0      0           0     0         0     1     0  29378.50
## 174       1    0      0           0     0         0     0     1  25431.95
## 175       1    0      0           0     0         0     0     1  23168.60
## 176       1    0      0           0     0         0     0     1  23929.83
## 177       1    0      0           0     0         0     0     1  25055.88
## 178       1    0      0           0     0         0     0     1  26317.81
## 179       1    0      0           0     0         0     0     1  25315.11
## 180       1    0      0           0     0         0     0     1  25714.00
## 181       1    0      0           0     0         0     1     0  31618.39
## 182       1    0      0           0     0         0     1     0  32466.82
## 183       1    0      0           0     0         0     1     0  35567.20
## 184       1    0      0           0     0         0     1     0  30096.75
## 185       1    0      0           0     0         0     1     0  31298.95
## 186       1    0      0           0     0         0     1     0  34249.93
## 187       1    0      0           0     0         0     1     0  34168.02
## 188       1    0      0           0     0         0     1     0  37945.44
## 189       1    0      0           0     0         0     1     0  35778.30
## 190       1    0      0           0     0         0     1     0  35710.00
## 191       1    0      0           0     0         0     1     0  36250.98
## 192       1    0      0           0     0         0     1     0  37738.08
## 193       1    0      0           0     0         0     1     0  34446.08
## 194       1    0      0           0     0         0     1     0  34119.27
## 195       0    0      0           1     0         0     0     0 110346.80
## 196       0    0      0           1     0         0     0     0 113870.11
## 197       0    0      0           1     0         0     0     0  83689.25
## 198       0    0      0           1     0         0     0     0  97474.21
## 199       0    0      0           1     0         0     0     0 102858.39
## 200       0    0      0           1     0         0     0     0 111260.17
## 201       0    0      0           1     0         0     0     0 106819.30
## 202       0    0      1           0     0         0     1     0  27877.24
## 203       0    0      1           0     0         0     1     0  26565.61
## 204       0    0      1           0     0         0     1     0  26433.80
## 205       0    0      1           0     0         0     1     0  24331.54
## 206       0    0      1           0     0         0     1     0  28933.42
## 207       0    0      1           0     0         0     1     0  21864.01
## 208       0    0      1           0     0         0     1     0  25316.85
## 209       0    0      0           0     0         0     1     0  35258.17
## 210       0    0      0           0     0         0     1     0  33719.39
## 211       0    0      0           0     0         0     1     0  31262.94
## 212       0    0      0           0     0         0     1     0  30886.57
## 213       0    0      0           0     0         0     1     0  35335.37
## 214       0    0      0           0     0         0     1     0  32156.57
## 215       0    0      0           0     0         0     1     0  34836.12
## 216       0    1      0           1     0         0     0     0  51621.39
## 217       0    1      0           1     0         0     0     0  44524.57
## 218       0    1      0           1     0         0     0     0  56036.34
## 219       0    1      0           1     0         0     0     0  53978.37
## 220       0    1      0           1     0         0     0     0  51449.33
## 221       0    1      0           1     0         0     0     0  57328.39
## 222       0    1      0           1     0         0     0     0  52685.32
## 223       0    1      0           0     0         0     1     0  37417.06
## 224       0    1      0           0     0         0     1     0  47067.35
## 225       0    1      0           0     0         0     1     0  39914.41
## 226       0    1      0           0     0         0     1     0  35799.73
## 227       0    1      0           0     0         0     1     0  43090.32
## 228       0    1      0           0     0         0     1     0  41837.89
## 229       0    1      0           0     0         0     1     0  40716.43
## 230       0    1      0           0     0         0     0     1  50141.78
## 231       0    1      0           0     0         0     0     1  46855.40
## 232       0    1      0           0     0         0     0     1  48994.77
## 233       0    1      0           0     0         0     0     1  40078.34
## 234       0    1      0           0     0         0     0     1  50026.46
## 235       0    1      0           0     0         0     0     1  53117.76
## 236       0    1      0           0     0         0     0     1  50136.01
## 237       0    1      0           0     0         0     0     1  37544.80
## 238       0    1      0           0     0         0     0     1  37456.72
## 239       0    1      0           0     0         0     0     1  43904.57
## 240       0    1      0           0     0         0     0     1  41777.23
## 241       0    0      0           0     1         0     0     0  20327.31
## 242       0    0      0           0     1         0     0     0  19996.77
## 243       0    0      0           0     1         0     0     0  20197.13
## 244       0    0      0           0     1         0     0     0  19754.67
## 245       0    0      0           0     1         0     0     0  21639.62
## 246       0    0      0           0     1         0     0     0  19826.67
## 247       0    0      0           0     1         0     0     0  19690.18
## 248       0    0      0           0     1         0     0     0  20414.55
## 249       0    0      0           0     1         0     0     0  20202.25
## 250       0    0      0           0     1         0     0     0  22828.62
## 251       0    0      0           0     1         0     0     0  20952.20
## 252       0    0      0           0     1         0     0     0  25343.68
## 253       0    0      0           0     1         0     0     0  22046.51
## 254       0    0      0           0     1         0     0     0  21774.58
## 255       0    0      0           0     1         0     0     0  30863.11
## 256       0    0      0           0     1         0     0     0  26325.68
## 257       0    0      0           0     1         0     0     0  25423.18
## 258       0    0      0           0     1         0     0     0  28188.36
## 259       0    0      0           0     1         0     0     0  29032.85
## 260       0    0      0           0     1         0     0     0  26306.29
## 261       0    0      0           0     1         0     0     0  30255.87
## 262       0    0      0           0     0         1     0     0  16714.74
## 263       0    0      0           0     0         1     0     0  17730.45
## 264       0    0      0           0     0         1     0     0  19575.81
## 265       0    0      0           0     0         1     0     0  17858.70
## 266       0    0      0           0     0         1     0     0  17344.01
## 267       0    0      0           0     0         1     0     0  19547.44
## 268       0    0      0           0     0         1     0     0  18462.46
## 269       0    0      0           0     0         1     0     0  27077.57
## 270       0    0      0           0     0         1     0     0  24002.12
## 271       0    0      0           0     0         1     0     0  28648.36
## 272       0    0      0           0     0         1     0     0  26625.01
## 273       0    0      0           0     0         1     0     0  26946.96
## 274       0    0      0           0     0         1     0     0  25981.24
## 275       0    0      0           0     0         1     0     0  28793.84
## 276       0    0      0           0     0         0     1     0  19971.92
## 277       0    0      0           0     0         0     1     0  19818.45
## 278       0    0      0           0     0         0     1     0  20356.81
## 279       0    0      0           0     0         0     1     0  16883.79
## 280       0    0      0           0     0         0     1     0  18214.61
## 281       0    0      0           0     0         0     1     0  18829.54
## 282       0    0      0           0     0         0     1     0  18048.86
## 283       0    0      0           0     0         0     1     0  23971.19
## 284       0    0      0           0     0         0     1     0  21191.40
## 285       0    0      0           0     0         0     1     0  21293.14
## 286       0    0      0           0     0         0     1     0  20235.77
## 287       0    0      0           0     0         0     1     0  18232.23
## 288       0    0      0           0     0         0     1     0  19929.19
## 289       0    0      0           0     0         0     1     0  23622.17
## 290       0    0      0           0     0         0     1     0  21798.65
## 291       0    0      0           0     0         0     1     0  23021.29
## 292       0    0      0           0     0         0     1     0  23894.04
## 293       0    0      0           0     0         0     1     0  23486.57
## 294       0    0      0           0     0         0     1     0  22120.23
## 295       0    0      0           0     0         0     1     0  22030.27
## 296       0    0      0           0     0         0     1     0  20505.00
## 297       0    0      0           0     0         0     1     0  30699.39
## 298       0    0      0           0     0         0     1     0  28710.43
## 299       0    0      0           0     0         0     1     0  35014.65
## 300       0    0      0           0     0         0     1     0  28629.24
## 301       0    0      0           0     0         0     1     0  29816.71
## 302       0    0      0           0     0         0     1     0  27832.34
## 303       0    0      0           0     0         0     1     0  30437.55
## 304       0    0      0           0     0         0     1     0  26510.61
## 305       0    0      0           0     0         0     1     0  26233.11
## 306       0    0      0           0     0         0     1     0  24362.94
## 307       0    0      0           0     0         0     1     0  27614.66
## 308       0    0      0           0     0         0     1     0  25143.15
## 309       0    0      0           0     0         0     1     0  30842.51
## 310       0    0      0           0     0         0     1     0  27348.89
## 311       0    0      0           0     1         0     0     0  19682.48
## 312       0    0      0           0     1         0     0     0  20128.89
## 313       0    0      0           0     1         0     0     0  21148.51
## 314       0    0      0           0     1         0     0     0  24227.00
## 315       0    0      0           0     1         0     0     0  21877.61
## 316       0    0      0           0     1         0     0     0  21669.54
## 317       0    0      0           0     1         0     0     0  23564.58
## 318       0    0      0           0     0         0     1     0  22942.45
## 319       0    0      0           0     0         0     1     0  21418.53
## 320       0    0      0           0     0         0     1     0  22765.63
## 321       0    0      0           0     0         0     1     0  20832.90
## 322       0    0      0           0     0         0     1     0  20673.77
## 323       0    0      0           0     0         0     1     0  22960.41
## 324       0    0      0           0     0         0     1     0  22028.74
## 325       0    0      0           0     1         0     0     0  32215.86
## 326       0    0      0           0     1         0     0     0  33537.43
## 327       0    0      0           0     1         0     0     0  28303.47
## 328       0    0      0           0     1         0     0     0  33275.13
## 329       0    0      0           0     1         0     0     0  36023.82
## 330       0    0      0           0     1         0     0     0  34995.30
## 331       0    0      0           0     0         1     0     0  17992.42
## 332       0    0      0           0     0         1     0     0  17752.92
## 333       0    0      0           0     0         1     0     0  17577.21
## 334       0    0      0           0     0         1     0     0  18254.90
## 335       0    0      0           0     0         1     0     0  17725.14
## 336       0    0      0           0     0         1     0     0  17815.57
## 337       0    0      0           0     0         1     0     0  18332.41
## 338       0    0      0           0     0         1     0     0  29407.62
## 339       0    0      0           0     0         1     0     0  31337.16
## 340       0    0      0           0     0         1     0     0  28426.25
## 341       0    0      0           0     0         1     0     0  28984.10
## 342       0    0      0           0     0         1     0     0  27340.07
## 343       0    0      0           0     0         1     0     0  28250.08
## 344       0    0      0           0     0         1     0     0  29469.98
## 345       0    0      0           0     0         0     1     0  19180.94
## 346       0    0      0           0     0         0     1     0  15963.19
## 347       0    0      0           0     0         0     1     0  17389.20
## 348       0    0      0           0     0         0     1     0  18190.29
## 349       0    0      0           0     0         0     1     0  17332.85
## 350       0    0      0           0     0         0     1     0  17496.84
## 351       0    0      0           0     0         0     1     0  19970.08
## 352       0    0      0           0     0         0     1     0  23177.67
## 353       0    0      0           0     0         0     1     0  24402.80
## 354       0    0      0           0     0         0     1     0  23349.61
## 355       0    0      0           0     0         0     1     0  25937.59
## 356       0    0      0           0     0         0     1     0  23974.51
## 357       0    0      0           0     0         0     1     0  24276.71
## 358       0    0      0           0     0         0     1     0  23203.84
## 359       0    0      0           0     0         0     1     0  26398.52
## 360       0    0      0           0     0         0     1     0  27867.62
## 361       0    0      0           0     0         0     1     0  27551.15
## 362       0    0      0           0     0         0     1     0  27134.99
## 363       0    0      0           0     0         0     1     0  26613.35
## 364       0    0      0           0     0         0     1     0  31296.78
## 365       0    0      0           0     0         0     1     0  26224.65
## 366       0    0      0           0     0         1     0     0  27133.74
## 367       0    0      0           0     0         1     0     0  29491.01
## 368       0    0      0           0     0         1     0     0  30537.23
## 369       0    0      0           0     0         1     0     0  25793.47
## 370       0    0      0           0     0         1     0     0  26179.39
## 371       0    0      0           0     0         1     0     0  31513.00
## 372       0    0      0           0     0         1     0     0  29240.84
## 373       0    0      1           0     1         0     0     0  23950.24
## 374       0    0      1           0     1         0     0     0  22079.02
## 375       0    0      1           0     1         0     0     0  22656.34
## 376       0    0      1           0     1         0     0     0  23193.68
## 377       0    0      1           0     1         0     0     0  22516.44
## 378       0    0      1           0     1         0     0     0  24193.56
## 379       0    0      1           0     1         0     0     0  23771.67
## 380       0    0      1           0     1         0     0     0  21755.54
## 381       0    0      1           0     1         0     0     0  22249.46
## 382       0    0      1           0     1         0     0     0  26161.93
## 383       0    0      1           0     1         0     0     0  22320.28
## 384       0    0      1           0     1         0     0     0  27225.12
## 385       0    0      1           0     1         0     0     0  22226.95
## 386       0    0      1           0     1         0     0     0  18570.34
## 387       1    0      0           0     0         0     1     0  29879.25
## 388       1    0      0           0     0         0     1     0  30345.88
## 389       1    0      0           0     0         0     1     0  31675.23
## 390       1    0      0           0     0         0     1     0  32700.15
## 391       1    0      0           0     0         0     1     0  28718.36
## 392       1    0      0           0     0         0     1     0  30060.04
## 393       1    0      0           0     0         0     1     0  28594.98
## 394       1    0      0           0     0         0     1     0  22468.27
## 395       1    0      0           0     0         0     1     0  24264.42
## 396       1    0      0           0     0         0     1     0  20976.34
## 397       1    0      0           0     0         0     1     0  25112.49
## 398       1    0      0           0     0         0     1     0  25715.77
## 399       1    0      0           0     0         0     1     0  23885.80
## 400       1    0      0           0     0         0     1     0  26360.86
## 401       0    0      0           0     0         0     1     0  25281.31
## 402       0    0      0           0     0         0     1     0  24217.52
## 403       0    0      0           0     0         0     1     0  24615.00
## 404       0    0      0           0     0         0     1     0  26295.40
## 405       0    0      0           0     0         0     1     0  28231.23
## 406       0    0      0           0     0         0     1     0  26101.76
## 407       0    0      0           0     0         0     1     0  27864.40
## 408       0    0      0           0     0         0     1     0  35134.98
## 409       0    0      0           0     0         0     1     0  37692.05
## 410       0    0      0           0     0         0     1     0  40392.41
## 411       0    0      0           0     0         0     1     0  37139.82
## 412       0    0      0           0     0         0     1     0  36106.02
## 413       0    0      0           0     0         0     1     0  41183.16
## 414       0    0      0           0     0         0     1     0  75233.23
## 415       0    0      0           0     0         0     1     0  82324.62
## 416       0    0      0           0     0         0     1     0  68683.07
## 417       0    0      0           0     0         0     1     0  70638.22
## 418       0    0      0           0     0         0     1     0  71295.11
## 419       0    0      0           0     0         0     1     0  65370.18
## 420       0    0      0           0     0         0     1     0  70947.92
## 421       0    0      0           0     0         0     1     0  49555.26
## 422       0    0      0           0     0         0     1     0  47113.97
## 423       0    0      0           0     0         0     1     0  48912.48
## 424       0    0      0           0     0         0     1     0  49820.69
## 425       0    0      0           0     0         0     1     0  49538.15
## 426       0    0      0           0     0         0     1     0  53781.11
## 427       0    0      0           0     0         0     1     0  45126.30
## 428       0    0      0           0     0         0     1     0  50182.21
## 429       0    0      0           0     0         0     1     0  53462.56
## 430       0    0      0           0     0         0     1     0  52305.79
## 431       0    0      0           0     0         0     1     0  52319.04
## 432       0    0      0           0     0         0     1     0  49985.00
## 433       0    0      0           0     0         0     1     0  50102.53
## 434       0    0      0           0     0         0     1     0  57479.07
## 435       0    0      0           0     0         0     1     0  58956.23
## 436       0    0      0           0     0         0     1     0  61633.91
## 437       0    0      0           0     0         0     1     0  68785.94
## 438       0    0      0           0     0         0     1     0  65929.49
## 439       0    0      0           0     0         0     1     0  51560.81
## 440       0    0      0           0     0         0     1     0  59856.29
## 441       0    0      0           0     0         0     1     0  56285.92
## 442       0    0      0           0     0         0     1     0  71142.60
## 443       0    0      0           0     0         0     1     0  68200.85
## 444       0    0      0           0     0         0     1     0  77748.09
## 445       0    0      0           0     0         0     1     0  66814.35
## 446       0    0      0           0     0         0     1     0  67064.04
## 447       0    0      0           0     0         0     1     0  66069.30
## 448       0    0      0           0     0         0     1     0  61490.74
## 449       0    0      0           0     0         0     1     0  21038.74
## 450       0    0      0           0     0         0     1     0  18570.34
## 451       0    0      0           0     0         0     1     0  18827.80
## 452       0    0      0           0     0         0     1     0  18626.86
## 453       0    0      0           0     0         0     1     0  19703.33
## 454       0    0      0           0     0         0     1     0  18784.07
## 455       0    0      0           0     0         0     1     0  22629.22
## 456       0    0      0           0     0         0     1     0  25030.51
## 457       0    0      0           0     0         0     1     0  18639.31
## 458       0    0      0           0     0         0     1     0  21182.13
## 459       0    0      0           0     0         0     1     0  22656.34
## 460       0    0      0           0     0         0     1     0  24215.85
## 461       0    0      0           0     0         0     1     0  20892.46
## 462       0    0      0           0     0         0     1     0  22884.93
## 463       0    0      0           0     0         0     1     0  21178.72
## 464       0    0      0           0     0         0     1     0  20404.67
## 465       0    0      0           0     0         0     1     0  22522.67
## 466       0    0      0           0     0         0     1     0  22844.39
## 467       0    0      0           0     0         0     1     0  23651.01
## 468       0    0      0           0     0         0     1     0  23468.74
## 469       0    0      0           0     0         0     1     0  20110.35
## 470       0    0      0           0     0         0     1     0  32221.06
## 471       0    0      0           0     0         0     1     0  25032.24
## 472       0    0      0           0     0         0     1     0  26786.12
## 473       0    0      0           0     0         0     1     0  30312.36
## 474       0    0      0           0     0         0     1     0  27607.67
## 475       0    0      0           0     0         0     1     0  25672.00
## 476       0    0      0           0     0         0     1     0  26568.05
## 477       0    0      0           0     0         0     1     0  26790.43
## 478       0    0      0           0     0         0     1     0  27710.85
## 479       0    0      0           0     0         0     1     0  27503.61
## 480       0    0      0           0     0         0     1     0  28104.11
## 481       0    0      0           0     0         0     1     0  25235.95
## 482       0    0      0           0     0         0     1     0  26960.60
## 483       0    0      0           0     0         0     1     0  25209.81
## 484       1    0      0           0     0         0     1     0  29967.44
## 485       1    0      0           0     0         0     1     0  32915.44
## 486       1    0      0           0     0         0     1     0  33277.42
## 487       1    0      0           0     0         0     1     0  34412.78
## 488       1    0      0           0     0         0     1     0  31052.01
## 489       1    0      0           0     0         0     1     0  29036.19
## 490       1    0      0           0     0         0     1     0  26098.75
## 491       1    0      0           0     0         0     1     0  25305.78
## 492       1    0      0           0     0         0     1     0  31900.67
## 493       1    0      0           0     0         0     1     0  25357.11
## 494       1    0      0           0     0         0     1     0  25715.77
## 495       1    0      0           0     0         0     1     0  26161.93
## 496       1    0      0           0     0         0     1     0  25816.65
## 497       1    0      0           0     0         0     1     0  31490.51
## 498       0    0      1           0     0         0     1     0  24317.54
## 499       0    0      1           0     0         0     1     0  19370.46
## 500       0    0      1           0     0         0     1     0  19471.51
## 501       0    0      1           0     0         0     1     0  19769.69
## 502       0    0      1           0     0         0     1     0  22857.56
## 503       0    0      1           0     0         0     1     0  22719.02
## 504       0    0      1           0     0         0     1     0  20586.37
## 505       0    0      1           0     0         0     1     0  23720.28
## 506       0    0      1           0     0         0     1     0  20865.54
## 507       0    0      1           0     0         0     1     0  19976.05
## 508       0    0      1           0     0         0     1     0  16999.65
## 509       0    0      1           0     0         0     1     0  19849.04
## 510       0    0      1           0     0         0     1     0  19503.82
## 511       0    0      1           0     0         0     1     0  18542.14
## 512       0    0      1           0     0         0     1     0  19482.72
## 513       0    0      1           0     0         0     1     0  19573.10
## 514       0    0      1           0     0         0     1     0  21119.32
## 515       0    0      1           0     0         0     1     0  21717.00
## 516       0    0      1           0     0         0     1     0  21201.65
## 517       0    0      1           0     0         0     1     0  21270.60
## 518       0    0      1           0     0         0     1     0  20268.41
## 519       0    0      1           0     0         0     1     0  18797.05
## 520       1    0      0           0     0         0     1     0  33524.29
## 521       1    0      0           0     0         0     1     0  34774.40
## 522       1    0      0           0     0         0     1     0  35413.57
## 523       1    0      0           0     0         0     1     0  30764.49
## 524       1    0      0           0     0         0     1     0  37332.73
## 525       1    0      0           0     0         0     1     0  32363.81
## 526       1    0      0           0     0         0     1     0  37179.17
## 527       0    0      0           0     0         0     1     0  43101.23
## 528       0    0      0           0     0         0     1     0  34660.09
## 529       0    0      0           0     0         0     1     0  42149.23
## 530       0    0      0           0     0         0     1     0  41940.13
## 531       0    0      0           0     0         0     1     0  37271.74
## 532       0    0      0           0     0         0     1     0  42329.16
## 533       0    0      0           0     0         0     1     0  37575.07
## 534       1    0      0           0     0         0     0     1  26886.83
## 535       1    0      0           0     0         0     0     1  27242.67
## 536       1    0      0           0     0         0     0     1  23993.84
## 537       1    0      0           0     0         0     0     1  26359.66
## 538       1    0      0           0     0         0     0     1  25141.41
## 539       1    0      0           0     0         0     0     1  28031.72
## 540       1    0      0           0     0         0     0     1  25463.01
## 541       0    0      0           0     1         0     0     0  37938.46
## 542       0    0      0           0     1         0     0     0  33829.83
## 543       0    0      0           0     1         0     0     0  37864.28
## 544       0    0      0           0     1         0     0     0  32264.11
## 545       0    0      0           0     1         0     0     0  36162.61
## 546       0    0      0           0     1         0     0     0  33183.30
## 547       0    0      0           0     1         0     0     0  34641.74
## 548       0    0      0           0     0         0     1     0  44601.53
## 549       0    0      0           0     0         0     1     0  41760.88
## 550       0    0      0           0     0         0     1     0  35509.10
## 551       0    0      0           0     0         0     1     0  37258.88
## 552       0    0      0           0     0         0     1     0  32661.02
## 553       0    0      0           0     0         0     1     0  36845.10
## 554       0    0      0           0     0         0     1     0  39926.36
## 555       0    0      0           0     0         1     0     0  16663.24
## 556       0    0      0           0     0         1     0     0  16556.93
## 557       0    0      0           0     0         1     0     0  17537.18
## 558       0    0      0           0     0         1     0     0  15753.96
## 559       0    0      0           0     0         1     0     0  16264.09
## 560       0    0      0           0     0         1     0     0  15298.53
## 561       0    0      0           0     0         1     0     0  14839.52
## 562       0    0      0           0     0         0     1     0  14112.36
## 563       0    0      0           0     0         0     1     0  14276.44
## 564       0    0      0           0     0         0     1     0  16600.45
## 565       0    0      0           0     0         0     1     0  15536.73
## 566       0    0      0           0     0         0     1     0  14551.57
## 567       0    0      0           0     0         0     1     0  17656.31
## 568       0    0      0           0     0         0     1     0  13903.04
## 569       0    1      0           1     0         0     0     0  47656.23
## 570       0    1      0           1     0         0     0     0  54053.00
## 571       0    1      0           1     0         0     0     0  59688.37
## 572       0    1      0           0     0         0     1     0  39276.23
## 573       0    1      0           0     0         0     1     0  44584.06
## 574       0    1      0           0     0         0     1     0  41228.70
## 575       0    1      0           0     0         0     1     0  38195.28
## 576       0    1      0           0     0         0     1     0  45390.99
## 577       0    1      0           0     0         0     1     0  43910.63
## 578       0    1      0           0     0         0     0     1  49825.27
## 579       0    1      0           0     0         0     0     1  45590.02
## 580       0    1      0           0     0         0     0     1  41862.93
## 581       0    1      0           1     0         0     0     0  54692.74
## 582       0    1      0           1     0         0     0     0  61424.22
## 583       0    1      0           1     0         0     0     0  58481.66
## 584       0    1      0           0     0         0     1     0  40663.97
## 585       0    1      0           0     0         0     1     0  51584.56
## 586       0    1      0           0     0         0     1     0  43114.13
## 587       0    1      0           0     0         0     0     1  48718.03
## 588       0    1      0           0     0         0     0     1  43380.02
## 589       0    1      0           0     0         0     0     1  52544.80
## 590       1    0      0           0     0         0     0     1  25921.48
## 591       1    0      0           0     0         0     0     1  24420.80
## 592       1    0      0           0     0         0     0     1  25804.16
## 593       0    0      0           1     0         0     0     0  62482.69
## 594       0    0      0           1     0         0     0     0  72017.78
## 595       0    0      0           1     0         0     0     0  63877.77
## 596       0    0      0           0     1         0     0     0  21140.24
## 597       0    0      0           0     1         0     0     0  19386.07
## 598       0    0      0           0     1         0     0     0  62912.90
## 599       0    0      0           0     1         0     0     0  55907.45
## 600       0    0      0           0     1         0     0     0  57253.20
## 601       0    0      0           0     1         0     0     0  21093.55
## 602       0    0      0           0     1         0     0     0  20096.00
## 603       0    0      0           0     1         0     0     0  18571.62
## 604       0    0      0           0     1         0     0     0  20067.80
## 605       1    0      0           0     1         0     0     0  44137.70
## 606       1    0      0           0     1         0     0     0  41081.82
## 607       1    0      0           0     1         0     0     0  20648.08
## 608       1    0      0           0     1         0     0     0  19649.42
## 609       1    0      0           0     1         0     0     0  52179.47
## 610       1    0      0           0     1         0     0     0  20725.25
## 611       0    0      0           0     0         0     1     0  30622.43
## 612       0    0      0           0     0         0     1     0  28879.51
## 613       0    0      0           0     0         0     1     0  28398.14
## 614       0    0      0           0     0         0     1     0  34336.79
## 615       0    0      0           0     0         0     1     0  28918.10
## 616       0    0      0           0     0         0     1     0  30746.79
## 617       0    0      0           0     0         0     1     0  34536.60
## 618       0    0      0           0     0         0     1     0  34235.74
## 619       0    0      0           0     0         0     1     0  32840.50
## 620       0    0      0           0     0         0     1     0  35775.83
## 621       0    0      0           0     0         0     1     0  33638.73
## 622       0    0      0           0     0         0     1     0  36411.59
## 623       0    0      0           0     0         0     1     0  57721.78
## 624       0    0      0           0     0         0     1     0  61872.80
## 625       0    0      0           0     0         0     1     0  55821.27
## 626       0    0      0           0     0         0     1     0  68912.77
## 627       0    0      0           0     0         0     1     0  58472.23
## 628       0    0      0           0     0         0     1     0  66657.61
## 629       1    0      0           0     1         0     0     0  25099.18
## 630       1    0      0           0     1         0     0     0  27939.57
## 631       1    0      0           0     1         0     0     0  25097.46
## 632       1    0      0           0     0         0     1     0  27511.85
## 633       1    0      0           0     0         0     1     0  24080.74
## 634       1    0      0           0     0         0     1     0  27355.18
## 635       1    0      0           0     0         0     1     0  35574.57
## 636       1    0      0           0     0         0     1     0  36591.45
## 637       1    0      0           0     0         0     1     0  35906.22
## 638       1    0      0           0     0         0     0     1  24280.06
## 639       1    0      0           0     0         0     0     1  25089.37
## 640       1    0      0           0     0         0     0     1  28651.66
## 641       1    0      0           0     0         0     1     0  34279.13
## 642       1    0      0           0     0         0     1     0  31258.62
## 643       1    0      0           0     0         0     1     0  32117.35
## 644       1    0      0           0     0         0     1     0  40961.86
## 645       1    0      0           0     0         0     1     0  35027.55
## 646       1    0      0           0     0         0     1     0  35377.71
## 647       0    0      0           1     0         0     0     0  89542.61
## 648       0    0      0           1     0         0     0     0 105060.56
## 649       0    0      0           1     0         0     0     0  91981.33
## 650       0    0      1           0     0         0     1     0  29758.40
## 651       0    0      1           0     0         0     1     0  26032.73
## 652       0    0      1           0     0         0     1     0  29248.24
## 653       0    0      0           0     0         0     1     0  34700.82
## 654       0    0      0           0     0         0     1     0  31610.38
## 655       0    0      0           0     0         0     1     0  34722.40
## 656       0    1      0           1     0         0     0     0  55289.12
## 657       0    1      0           1     0         0     0     0  53571.00
## 658       0    1      0           1     0         0     0     0  51451.70
## 659       0    1      0           0     0         0     1     0  40067.27
## 660       0    1      0           0     0         0     1     0  38725.74
## 661       0    1      0           0     0         0     1     0  42386.71
## 662       0    1      0           0     0         0     0     1  41593.91
## 663       0    1      0           0     0         0     0     1  49344.61
## 664       0    1      0           0     0         0     0     1  48798.86
## 665       0    0      0           0     1         0     0     0  25163.43
## 666       0    0      0           0     1         0     0     0  28461.63
## 667       0    0      0           0     1         0     0     0  23471.44
## 668       0    0      0           0     1         0     0     0  24950.53
## 669       0    0      0           0     1         0     0     0  22018.60
## 670       0    0      0           0     1         0     0     0  30434.04
## 671       0    0      0           0     1         0     0     0  19839.45
## 672       0    0      0           0     1         0     0     0  23527.17
## 673       0    0      0           0     1         0     0     0  21420.51
## 674       0    0      0           0     0         1     0     0  15977.90
## 675       0    0      0           0     0         1     0     0  17923.38
## 676       0    0      0           0     0         1     0     0  17775.42
## 677       0    0      0           0     0         1     0     0  30498.59
## 678       0    0      0           0     0         1     0     0  26914.70
## 679       0    0      0           0     0         1     0     0  30509.83
## 680       0    0      0           0     0         0     1     0  18596.44
## 681       0    0      0           0     0         0     1     0  29102.47
## 682       0    0      0           0     0         0     1     0  17832.40
## 683       0    0      0           0     0         0     1     0  21679.53
## 684       0    0      0           0     0         0     1     0  22849.65
## 685       0    0      0           0     0         0     1     0  31237.02
## 686       0    0      0           0     0         0     1     0  31427.49
## 687       0    0      0           0     0         0     1     0  24141.80
## 688       0    0      0           0     0         0     1     0  16019.52
## 689       0    0      0           0     0         0     1     0  30257.27
## 690       0    0      0           0     0         0     1     0  24347.80
## 691       0    0      0           0     0         0     1     0  24136.79
## 692       0    0      0           0     0         0     1     0  24515.44
## 693       0    0      0           0     0         0     1     0  28096.34
## 694       0    0      0           0     0         0     1     0  16326.75
## 695       0    0      0           0     1         0     0     0  23171.27
## 696       0    0      0           0     1         0     0     0  20493.20
## 697       0    0      0           0     1         0     0     0  22014.03
## 698       0    0      0           0     0         0     1     0  22973.64
## 699       0    0      0           0     0         0     1     0  22888.63
## 700       0    0      0           0     0         0     1     0  22149.28
## 701       0    0      0           0     1         0     0     0  33051.39
## 702       0    0      0           0     1         0     0     0  34172.73
## 703       0    0      0           0     1         0     0     0  30379.44
## 704       0    0      0           0     0         1     0     0  17887.91
## 705       0    0      0           0     0         1     0     0  18101.71
## 706       0    0      0           0     0         1     0     0  18338.33
## 707       0    0      0           0     0         1     0     0  27544.17
## 708       0    0      0           0     0         1     0     0  28976.09
## 709       0    0      0           0     0         1     0     0  27042.05
## 710       0    0      0           0     0         0     1     0  27862.48
## 711       0    0      0           0     0         0     1     0  17976.28
## 712       0    0      0           0     0         0     1     0  26439.29
## 713       0    0      0           0     0         0     1     0  22839.66
## 714       0    0      0           0     0         0     1     0  17577.21
## 715       0    0      0           0     0         0     1     0  22813.91
## 716       0    0      0           0     0         0     1     0  18692.60
## 717       0    0      0           0     0         0     1     0  26299.63
## 718       0    0      0           0     0         0     1     0  26898.60
## 719       0    0      0           0     0         1     0     0  27638.83
## 720       0    0      0           0     0         1     0     0  28946.07
## 721       0    0      0           0     0         1     0     0  26484.98
## 722       0    0      1           0     1         0     0     0  21887.17
## 723       0    0      1           0     1         0     0     0  24775.91
## 724       0    0      1           0     1         0     0     0  24586.11
## 725       0    0      1           0     1         0     0     0  20974.41
## 726       0    0      1           0     1         0     0     0  23661.91
## 727       0    0      1           0     1         0     0     0  25168.64
## 728       1    0      0           0     0         0     1     0  24666.62
## 729       1    0      0           0     0         0     1     0  32391.39
## 730       1    0      0           0     0         0     1     0  25501.73
## 731       1    0      0           0     0         0     1     0  21183.59
## 732       1    0      0           0     0         0     1     0  29069.64
## 733       1    0      0           0     0         0     1     0  31786.28
## 734       0    0      0           0     0         0     1     0  37808.52
## 735       0    0      0           0     0         0     1     0  35209.49
## 736       0    0      0           0     0         0     1     0  34919.63
## 737       0    0      0           0     0         0     1     0  26291.76
## 738       0    0      0           0     0         0     1     0  23918.26
## 739       0    0      0           0     0         0     1     0  24510.36
## 740       0    0      0           0     0         0     1     0  79257.38
## 741       0    0      0           0     0         0     1     0  56535.30
## 742       0    0      0           0     0         0     1     0  71021.48
## 743       0    0      0           0     0         0     1     0  50583.60
## 744       0    0      0           0     0         0     1     0  77837.65
## 745       0    0      0           0     0         0     1     0  69803.90
## 746       0    0      0           0     0         0     1     0  58274.65
## 747       0    0      0           0     0         0     1     0  62881.04
## 748       0    0      0           0     0         0     1     0  57375.93
## 749       0    0      0           0     0         0     1     0  49320.76
## 750       0    0      0           0     0         0     1     0  62435.23
## 751       0    0      0           0     0         0     1     0  56593.91
## 752       0    0      0           0     0         0     1     0  57768.32
## 753       0    0      0           0     0         0     1     0  72520.32
## 754       0    0      0           0     0         0     1     0  46376.68
## 755       0    0      0           0     0         0     1     0  22646.96
## 756       0    0      0           0     0         0     1     0  21623.19
## 757       0    0      0           0     0         0     1     0  27620.39
## 758       0    0      0           0     0         0     1     0  16973.43
## 759       0    0      0           0     0         0     1     0  21790.62
## 760       0    0      0           0     0         0     1     0  19922.32
## 761       0    0      0           0     0         0     1     0  23222.54
## 762       0    0      0           0     0         0     1     0  22257.67
## 763       0    0      0           0     0         0     1     0  19726.02
## 764       0    0      0           0     0         0     1     0  30139.06
## 765       0    0      0           0     0         0     1     0  25144.31
## 766       0    0      0           0     0         0     1     0  26565.61
## 767       0    0      0           0     0         0     1     0  18450.57
## 768       0    0      0           0     0         0     1     0  27045.17
## 769       0    0      0           0     0         0     1     0  25480.02
## 770       1    0      0           0     0         0     1     0  27122.50
## 771       1    0      0           0     0         0     1     0  26581.52
## 772       1    0      0           0     0         0     1     0  25416.14
## 773       1    0      0           0     0         0     1     0  29851.75
## 774       1    0      0           0     0         0     1     0  27397.43
## 775       1    0      0           0     0         0     1     0  25696.85
## 776       0    0      1           0     0         0     1     0  19783.80
## 777       0    0      1           0     0         0     1     0  18515.25
## 778       0    0      1           0     0         0     1     0  19108.63
## 779       0    0      1           0     0         0     1     0  23444.97
## 780       0    0      1           0     0         0     1     0  22527.85
## 781       0    0      1           0     0         0     1     0  21337.32
## 782       0    0      1           0     0         0     1     0  18368.33
## 783       0    0      1           0     0         0     1     0  24454.00
## 784       1    0      0           0     0         0     1     0  31687.63
## 785       1    0      0           0     0         0     1     0  36146.79
## 786       1    0      0           0     0         0     1     0  31122.17
## 787       0    0      0           0     0         0     1     0  43180.70
## 788       0    0      0           0     0         0     1     0  41051.56
## 789       0    0      0           0     0         0     1     0  37669.49
## 790       1    0      0           0     0         0     0     1  27703.83
## 791       1    0      0           0     0         0     0     1  23903.95
## 792       1    0      0           0     0         0     0     1  23172.86
## 793       0    0      0           0     1         0     0     0  32543.91
## 794       0    0      0           0     1         0     0     0  32801.95
## 795       0    0      0           0     1         0     0     0  35588.51
## 796       0    0      0           0     0         0     1     0  40390.54
## 797       0    0      0           0     0         0     1     0  35599.98
## 798       0    0      0           0     0         0     1     0  37570.74
## 799       0    0      0           0     0         1     0     0  17972.96
## 801       0    0      0           0     0         1     0     0  15644.43
## 802       0    0      0           0     0         0     1     0  15260.18
## 803       0    0      0           0     0         0     1     0  15391.46
## 804       0    0      0           0     0         0     1     0  15555.71
##      distancia
## 1    6128.0785
## 2    4101.7435
## 3    9648.5613
## 4    6851.6825
## 5    5361.4972
## 6    7204.0356
## 7    5297.7932
## 8    8399.7805
## 9    7635.0280
## 10   5278.8954
## 11   3048.9515
## 12   6449.6464
## 13   6788.5881
## 14   8234.2721
## 15   6953.7918
## 16   3052.3043
## 17   7027.2495
## 18   6051.5728
## 19   7072.6652
## 20   2443.6113
## 21   4454.0966
## 22   6885.5157
## 23   6974.2136
## 24   8164.4721
## 25   5627.8956
## 26   6372.5311
## 27   6044.2575
## 28   6651.4265
## 29   7728.9076
## 30   6566.9959
## 31   4903.6820
## 32   7324.7379
## 33   3685.0768
## 34   6490.1853
## 35  14932.6384
## 36   8245.2451
## 37   6451.7801
## 38   3291.8800
## 39   4440.3804
## 40   1178.6759
## 41   6887.9542
## 42   3436.0522
## 43   8046.8178
## 44   6805.6572
## 45  11801.0851
## 46   2985.5523
## 47   8716.7764
## 48    510.8510
## 49   7054.0722
## 50   6721.5313
## 51   8009.0222
## 52   7695.3792
## 53   2415.2646
## 54   4598.5735
## 55   8078.2126
## 56   5168.2516
## 57   6127.4689
## 58   7735.9181
## 59   6100.6462
## 60   6796.5130
## 61   6737.6859
## 62   6063.7649
## 63   5515.7279
## 64   7388.1370
## 65   7029.3831
## 66   6943.1236
## 67   5953.1212
## 68  10172.5189
## 69   9765.6059
## 70   1104.9134
## 71   1596.8666
## 72   6096.0741
## 73   7262.2531
## 74   4433.6747
## 75   9297.1227
## 76   8340.6486
## 77   4147.4640
## 78   7166.5447
## 79   7041.8800
## 80   2247.0129
## 81  10499.5733
## 82   9581.8093
## 83   6851.6825
## 84   4198.9759
## 85   8388.5028
## 86   8349.7927
## 87   5931.7849
## 88   6822.1166
## 89   7071.4460
## 90   2733.1748
## 91   2150.0853
## 92  10421.5435
## 93   3541.5143
## 94   7394.2331
## 95   2868.2029
## 96   6507.5591
## 97   7607.9005
## 98   4169.1051
## 99   8290.3560
## 100  3326.9325
## 101  4935.6864
## 102  6528.2858
## 103  7963.3016
## 104  6672.4579
## 105  4006.6447
## 106  6284.4428
## 107  7412.2165
## 108  7294.2575
## 109  5815.9595
## 110  7706.9617
## 111  7632.2848
## 112  5589.7952
## 113  7635.9425
## 114  7270.1780
## 115  7761.5216
## 116  3059.0100
## 117   911.9727
## 118  6035.1134
## 119 10026.8227
## 120  7119.9098
## 121  4592.1726
## 122  7243.6601
## 123  7935.2597
## 124  5069.8000
## 125  3223.9088
## 126  2734.0892
## 127  5031.6996
## 128  4377.8956
## 129  7458.2419
## 130  7694.7696
## 131  6520.9705
## 132  7684.7110
## 133  8258.9612
## 134  8162.3385
## 135  7449.0978
## 136  4802.1824
## 137  5531.2729
## 138  4948.4882
## 139  5223.7259
## 140  7723.4211
## 141  6695.3182
## 142  4493.7210
## 143  7669.7757
## 144  4889.3563
## 145  4296.2082
## 146 12749.6342
## 147  6581.6264
## 148 10995.4889
## 149  8001.0973
## 150  3898.7442
## 151  2476.5301
## 152  6769.9951
## 153  2859.0588
## 154  1563.9478
## 155  6375.5791
## 156  7112.5945
## 157  6901.0607
## 158  3679.2855
## 159  2060.7779
## 160  8070.2877
## 161  9163.3138
## 162  6330.7730
## 163  8642.4043
## 164  6897.0983
## 165  3753.0480
## 166  1382.8944
## 167  7710.0098
## 168  6348.7564
## 169  7856.9251
## 170  6493.5382
## 171  9121.2509
## 172  8080.6511
## 173  5045.7206
## 174  6541.3923
## 175  9578.1517
## 176  9690.0146
## 177  5549.2563
## 178  4900.6340
## 179  6520.0561
## 180  5197.8176
## 181  6441.1119
## 182  6465.1914
## 183  3753.3528
## 184  7617.6542
## 185  8449.4635
## 186  5308.7662
## 187  3422.6408
## 188  5836.3814
## 189  6684.0405
## 190  7818.5199
## 191  5950.0732
## 192  5264.8744
## 193  8280.9071
## 194  9508.9612
## 195  1956.8398
## 196   177.7006
## 197 13012.3750
## 198  7069.3124
## 199  5547.4275
## 200  2405.5109
## 201  3664.0454
## 202  3079.1270
## 203  4946.6594
## 204  4341.0144
## 205  7918.4955
## 206  3348.5735
## 207 10869.9098
## 208  6243.5991
## 209  5289.2587
## 210  6144.2331
## 211  8485.4304
## 212  8896.3058
## 213  5421.5435
## 214  7413.7406
## 215  5962.5701
## 216  7179.0417
## 217 10715.9839
## 218  3734.1502
## 219  6378.0176
## 220  7485.6742
## 221  3151.6703
## 222  5825.4084
## 223  8438.7954
## 224  1190.8681
## 225  8030.0536
## 226 15358.1444
## 227  5086.5643
## 228  6532.8578
## 229  5964.7037
## 230  5731.8337
## 231  6693.4894
## 232  4587.2958
## 233 12411.3021
## 234  5543.4650
## 235  1953.4870
## 236  5278.2858
## 237  7686.5399
## 238  6588.6369
## 239  1474.0307
## 240  5313.0334
## 241  6879.7245
## 242  6940.6852
## 243  7868.8125
## 244  5977.8103
## 245  5407.5226
## 246  7488.4175
## 247  9234.6379
## 248  9110.8876
## 249 10010.9729
## 250  6210.0707
## 251  8516.8252
## 252  1843.4528
## 253  7877.6518
## 254  8305.5962
## 255  3174.2258
## 256  7160.1439
## 257  8138.2590
## 258  5669.9585
## 259  3550.0488
## 260  7903.8649
## 261  1776.0912
## 262  6774.2624
## 263  6126.5545
## 264  3688.4297
## 265  6197.8786
## 266  8505.8522
## 267  3051.3899
## 268  6004.3282
## 269  7150.6950
## 270 10334.6745
## 271  5908.9247
## 272  7492.9895
## 273  6653.5601
## 274  8087.0519
## 275  5187.7591
## 276  3651.8532
## 277  1262.4970
## 278  1106.1327
## 279  9433.0651
## 280  3400.3901
## 281  4649.1709
## 282  6079.3099
## 283   751.0363
## 284  4459.2782
## 285  7882.8335
## 286  7634.7232
## 287 12175.6889
## 288  7641.1241
## 289  3501.5850
## 290  8361.0705
## 291  6439.8927
## 292  3956.3521
## 293  5727.2616
## 294  7238.4784
## 295  6587.1129
## 296 10611.7410
## 297  5546.8178
## 298  7758.1687
## 299   564.8013
## 300  8223.6040
## 301  6057.6689
## 302  8951.4752
## 303  6556.9373
## 304  6606.6203
## 305  7832.5408
## 306  9894.5379
## 307  6527.9810
## 308  8374.7866
## 309   451.1095
## 310  7199.7683
## 311  8658.8637
## 312  6009.2051
## 313  6064.9842
## 314  1417.9468
## 315  6303.9503
## 316  4337.6615
## 317  1897.0983
## 318  5168.8613
## 319  6109.1807
## 320  6252.1336
## 321  6609.3636
## 322  8400.3901
## 323  2625.8839
## 324  5719.9464
## 325  6927.8834
## 326  6752.0117
## 327 11902.2799
## 328  5492.8676
## 329  4507.4372
## 330  2153.4382
## 331  7047.3665
## 332  6143.6235
## 333  7046.7569
## 334  6152.7676
## 335  7847.4762
## 336  7765.1792
## 337  6529.2002
## 338  4978.9685
## 339  2014.1429
## 340  5940.6242
## 341  4629.9683
## 342  7591.1363
## 343  5623.9332
## 344  5316.0814
## 345  2218.3614
## 346 10552.6091
## 347  6404.8403
## 348  4458.3638
## 349  7639.9049
## 350  7884.9671
## 351  3112.9603
## 352  6561.5094
## 353  5229.8220
## 354  5763.8381
## 355   263.6552
## 356  5550.4755
## 357  4518.4101
## 358  6040.5999
## 359  7051.0241
## 360  5972.0190
## 361  5570.8973
## 362  6060.4121
## 363  7381.7362
## 364   284.0771
## 365  8607.3519
## 366  6962.0215
## 367  2254.6330
## 368  1716.6545
## 369  6976.6520
## 370  6538.6491
## 371  2330.2243
## 372  4497.0739
## 373  4381.8581
## 374  7207.0836
## 375  8165.0817
## 376  8225.4328
## 377  6406.9739
## 378  5929.9561
## 379  6783.4065
## 380  7919.1051
## 381  7935.2597
## 382  3217.2031
## 383  7421.6654
## 384   877.5299
## 385  7996.8300
## 386 11874.5428
## 387  7542.9773
## 388  6834.6135
## 389  3521.7020
## 390  5359.3636
## 391  6437.7591
## 392  5028.0419
## 393  8651.8532
## 394  5974.7623
## 395  4042.3068
## 396  7307.9737
## 397  1764.2039
## 398  1202.7554
## 399  3785.6620
## 400  1276.5179
## 401  6777.6152
## 402  6999.5123
## 403  8328.7613
## 404  6044.8671
## 405  2784.3819
## 406  4022.1897
## 407  2505.7913
## 408  7792.0020
## 409  5763.8381
## 410  3052.3043
## 411  7253.7186
## 412  6793.1602
## 413   703.4870
## 414  4676.6033
## 415   671.1778
## 416  7331.1387
## 417  7123.5674
## 418  7239.3928
## 419  9169.1051
## 420  6512.7408
## 421  5446.8422
## 422  6417.9468
## 423  5623.3236
## 424  3238.5394
## 425  4552.8530
## 426  2011.0949
## 427  8377.2251
## 428  7992.5628
## 429  5687.9420
## 430  5336.5033
## 431  6373.4455
## 432  8395.5133
## 433  7048.2809
## 434  1965.0695
## 435  6718.4833
## 436  5106.6813
## 437   867.4713
## 438  2278.7125
## 439 10767.4957
## 440  5821.7508
## 441  7862.7164
## 442  4603.7552
## 443  5663.5577
## 444   240.1853
## 445  7272.9212
## 446  6271.3363
## 447  7838.6369
## 448  9541.2704
## 449  4362.0458
## 450  7364.9720
## 451  6039.9902
## 452  6560.8998
## 453  7624.3599
## 454  7841.6850
## 455  1500.2438
## 456  2345.4645
## 457 11145.4523
## 458  6445.0744
## 459  5317.3007
## 460  3750.6096
## 461  6190.2585
## 462  7031.5167
## 463  6931.2363
## 464  8742.6847
## 465  5263.9600
## 466  2914.2282
## 467  2044.9281
## 468  2168.6784
## 469  7987.0763
## 470   544.6842
## 471 10167.3372
## 472  6822.4214
## 473  2502.7432
## 474  6573.7015
## 475  7946.2326
## 476  8369.9098
## 477  6143.0139
## 478  4443.7332
## 479  2661.5460
## 480  3472.6286
## 481  7911.4850
## 482  5657.7664
## 483  7859.9732
## 484  7777.3714
## 485  3151.0607
## 486  3415.0207
## 487  2221.1046
## 488  5955.5596
## 489  6614.8500
## 490 10858.3272
## 491  7311.9361
## 492   485.2475
## 493  8938.3687
## 494  6697.7566
## 495  6899.8415
## 496  8473.5430
## 497   667.2153
## 498   729.0905
## 499  6051.2680
## 500  6936.7228
## 501  7540.8437
## 502  3607.6567
## 503  3925.2621
## 504  4926.5423
## 505   529.4440
## 506  9054.8037
## 507  7517.6786
## 508  9893.3187
## 509  6668.1907
## 510  6957.4494
## 511 10667.5201
## 512  8625.3353
## 513  6635.5767
## 514  5822.0556
## 515  5943.6723
## 516  4059.3758
## 517  7336.3204
## 518  8024.8720
## 519  7128.7491
## 520  5918.9832
## 521  3373.8722
## 522  4729.3343
## 523  8001.7069
## 524   699.5245
## 525  6977.2616
## 526  1723.0554
## 527  3673.4943
## 528 11316.7520
## 529  5283.7723
## 530  2985.5523
## 531  7884.9671
## 532  3977.6884
## 533  7323.5187
## 534  2788.9539
## 535   906.1814
## 536  7108.9368
## 537  2668.2516
## 538  7076.6277
## 539  1360.3389
## 540  4359.9122
## 541  3799.6830
## 542  7787.7347
## 543  4556.2058
## 544  7517.9834
## 545  6896.4887
## 546  7335.7108
## 547  7626.1887
## 548  1639.5391
## 549   193.8552
## 550  8346.7447
## 551  8516.2156
## 552 10330.4072
## 553  8007.8030
## 554  4910.6925
## 555  4426.0546
## 556  5035.6620
## 557  2041.8800
## 558  7006.2180
## 559  4328.2126
## 560  6757.1934
## 561  9141.6728
## 562 10759.2660
## 563 10032.3092
## 564  4400.7559
## 565  5847.0495
## 566  7983.1139
## 567  2161.3631
## 568  7685.9303
## 569  9899.1100
## 570  5769.9342
## 571  1166.7886
## 572  9553.7674
## 573  7540.2341
## 574  6159.4733
## 575  8412.5823
## 576  6712.0824
## 577  4352.9017
## 578  5386.7959
## 579  6304.8647
## 580 10445.3182
## 581  7748.1102
## 582  4011.8264
## 583  5545.9034
## 584 10416.6667
## 585  1567.9103
## 586  6763.2894
## 587  7140.3316
## 588  9684.5282
## 589  5174.0429
## 590  5933.0041
## 591  8500.0610
## 592  5605.6450
## 593  7912.7042
## 594  3692.6969
## 595  7671.6045
## 596  6643.5016
## 597  5832.7237
## 598  3266.5813
## 599  7847.7810
## 600  6931.2363
## 601  6678.2492
## 602  8684.4672
## 603  6829.1270
## 604  6008.2907
## 605  7280.5413
## 606 11119.2392
## 607  5434.6501
## 608  7954.4623
## 609  2799.6220
## 610  5859.8513
## 611  6643.8064
## 612  5971.7142
## 613  8482.6871
## 614  3120.2755
## 615 10566.0205
## 616  7317.7274
## 617  5933.6138
## 618  5763.2285
## 619  6887.3446
## 620  6736.7715
## 621  9037.1251
## 622  7197.0251
## 623  7441.7825
## 624  5687.9420
## 625  7748.4150
## 626  1676.1156
## 627  7666.7276
## 628  4405.0232
## 629  5582.4799
## 630   268.5321
## 631  6831.8703
## 632  5705.9254
## 633  9592.4774
## 634  7873.0797
## 635  2758.1687
## 636  1734.3331
## 637  3420.2024
## 638  8363.2041
## 639  6494.4526
## 640  3750.0000
## 641  3892.9529
## 642  7789.8683
## 643  7933.4309
## 644  3624.7257
## 645  7862.1068
## 646  6239.9415
## 647  9694.2819
## 648  4754.9378
## 649  8918.5564
## 650  2363.7527
## 651  5820.2268
## 652  1775.7864
## 653  7244.2697
## 654  9547.6713
## 655  6137.8322
## 656  5398.3784
## 657  6602.3531
## 658  6464.2770
## 659  6481.9556
## 660  8435.1378
## 661  4897.5860
## 662 11142.7091
## 663  4351.9873
## 664  4945.4401
## 665   356.3155
## 666  4845.7693
## 667   353.5723
## 668 10163.6796
## 669  3112.0458
## 670  2543.5869
## 671  5897.0373
## 672  4806.1448
## 673  7968.4833
## 674  9046.5740
## 675  6853.2065
## 676  7488.4175
## 677  2647.8298
## 678  6016.8252
## 679  1809.3148
## 680  4085.5889
## 681  8967.3250
## 682 11233.5406
## 683  5763.8381
## 684  3450.9876
## 685  5728.7857
## 686  4302.3043
## 687  4420.8730
## 688 11382.8944
## 689  2426.5423
## 690  4214.8257
## 691  2706.6569
## 692  9916.7886
## 693  4615.9473
## 694  7304.0112
## 695  1581.6264
## 696  6518.5321
## 697  5477.6274
## 698  5648.9271
## 699  2568.5808
## 700  5498.6589
## 701  5166.4228
## 702  5284.3819
## 703  8296.1473
## 704  9161.1802
## 705  6531.0290
## 706  7652.7067
## 707  7455.8035
## 708  5721.4704
## 709  7886.4911
## 710  6528.8954
## 711  6821.5069
## 712  3012.0702
## 713  6456.0473
## 714  8466.2277
## 715  6591.9898
## 716  5795.5377
## 717  7740.1853
## 718  8024.8720
## 719  5706.2302
## 720  5330.4072
## 721  6157.0349
## 722  8300.1097
## 723  3938.0639
## 724  5166.7276
## 725  9177.0300
## 726  5381.6142
## 727  2592.9651
## 728  1316.1424
## 729  5614.1795
## 730  1630.6998
## 731  7480.4926
## 732  8404.6574
## 733  6251.5240
## 734  6234.1502
## 735  7082.7237
## 736  7769.1417
## 737  4981.1022
## 738  7321.6898
## 739  6881.2485
## 740  2037.6128
## 741  3650.0244
## 742  6504.8159
## 743  3356.8032
## 744   797.3665
## 745  7680.7486
## 746  6638.0151
## 747  9627.8347
## 748  3217.2031
## 749  5210.3146
## 750  4259.0222
## 751  4099.3050
## 752  7027.5543
## 753  4215.1305
## 754  6412.7652
## 755  4643.0749
## 756  6257.6201
## 757  4847.2933
## 758 11846.5009
## 759  5286.2107
## 760  9509.5708
## 761  1863.5699
## 762  5362.7164
## 763  6550.8413
## 764  4283.7113
## 765  6483.7844
## 766  5319.1295
## 767  8964.2770
## 768  5943.0627
## 769  9510.4852
## 770  5473.9698
## 771  6324.9817
## 772 12669.4709
## 773  6361.2534
## 774  9267.2519
## 775  7802.3653
## 776  5224.0307
## 777 10351.7435
## 778  8655.8157
## 779  5642.2214
## 780  5564.8013
## 781  4553.1578
## 782  9942.3921
## 783  3783.2236
## 784  8253.1700
## 785  2721.2875
## 786  8524.1405
## 787  1431.0534
## 788  5327.9688
## 789  7738.0517
## 790  3843.5747
## 791  7397.5860
## 792  6610.5828
## 793  7992.8676
## 794  7693.2455
## 795  6703.2431
## 796  4407.7664
## 797  8608.2663
## 798  6999.5123
## 799  1437.4543
## 801  6350.8900
## 802  7571.9337
## 803  5874.4818
## 804  5963.4845
```

```r
ii <- cars$speed > 10 & cars$speed < 15 & cars$dist > 45
cars[ii, ]  # speed en (10,15) y dist>45
```

```
##  [1] Price       Mileage     Cylinder    Doors       Cruise      Sound      
##  [7] Leather     Buick       Cadillac    Chevy       Pontiac     Saab       
## [13] Saturn      convertible coupe       hatchback   sedan       wagon      
## [19] velocidad   distancia  
## <0 rows> (or 0-length row.names)
```

En este caso puede ser de utilidad la función [`which()` ](https://www.rdocumentation.org/packages/base/versions/3.6.1/topics/which):


```r
it <- which(ii)
str(it)
```

```
##  int(0)
```

```r
cars[it, 1:2]
```

```
## [1] Price   Mileage
## <0 rows> (or 0-length row.names)
```

```r
# rownames(cars[it, 1:2])

id <- which(!ii)
str(cars[id, 1:2])
```

```
## 'data.frame':	0 obs. of  2 variables:
##  $ Price  : num 
##  $ Mileage: int
```

```r
# Se podría p.e. emplear cars[id, ] para predecir cars[it, ]$speed
# ?which.min
```
