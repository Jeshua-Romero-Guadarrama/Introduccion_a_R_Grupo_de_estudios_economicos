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
##  [7] "inline"    "inline2"   "is_html"   "is_latex"  "latexfig"  "res"
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

```
## numeric(0)
```

 Finalmente, incluimos la nueva variable que llamaremos
`velocidad` en `cars`:


También transformaremos la variable `dist` (en pies) en una nueva
variable `distancia` (en metros). Ahora la transformación deseada es
`metros=pies/3.2808`:



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








