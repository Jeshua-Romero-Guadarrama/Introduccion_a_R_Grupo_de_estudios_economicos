# Instalación
La manera de instalar R cambia dependiendo del sistema operativo utilices pero todas tienen en común el uso de **CRAN**.

[CRAN](https://cran.r-project.org/) es el *The Comprehensive R Archive Network*, una red en la que se archivan todas las versiones de R *base*, así como todos los paquetes para R que han pasado por un proceso de revisión riguroso, realizado por el *CRAN Team*, que se encarga de asegurar su correcto funcionamiento.

CRAN es una red porque existen copias de su contenido en diferentes servidores alrededor del mundo, los cuales se actualizan diariamente. De este modo, no importa de qué servidor de CRAN descargues R o algún paquete, lo que vas a obtener será la versión más reciente de ese recurso, que es igual a la disponible en todos los demás servidores.

Como veremos más adelante, cuando descargamos un paquete de R, lo estamos haciendo desde CRAN, a menos que indiquemos otra cosa.

El sitio oficial de CRAN, en el que encontrarás más información sobre este repositorio es el siguiente:

* https://cran.r-project.org/ 

## Windows
Para instalar R en *Windows*, la forma más simple es descargar la versión más reciente de R *base* desde el siguiente enlace de CRAN:

* https://cran.r-project.org/bin/windows/base/

El archivo que necesitamos tiene la extensión **.exe** (por ejemplo *R-3.5.1-win.exe*). Una vez descargado, lo ejecutamos como cualquier instalable.

Después de la instalación, estamos listos para usar R.

## OSX
Para instalar R en *OSX*, se sigue un procedimiento similar que en *Windows*. Necesitamos descargar los archivos binarios de R base desde CRAN y ejecutarlos.

* https://cran.r-project.org/bin/macosx/ 

Al concluir la instalación, podremos usar R, incluso llamándolo directamente desde la consola.

## Linux
En *Linux*, como suele ser el caso para casi todo, hay una manera fácil y una difícil de instalar R.

La manera fácil depende de la  presencia de R en los repositorios de la distribución de *Linux* que estés usando. Si R se encuentra en los repositorios de tu distribución, sólo es necesario usar el gestor de paquetes de tu preferencia para instalarlo, como cualquier otro *software*.

Si R no se encuentra en los repositorios, debes agregar una entrada a tu lista de fuentes de software. Esta entrada depende de tu distribución. 

También tienes la opción de puedes compilar R directamente desde archivos fuente.

Para todas las opciones anteriores, los detalles de instalación se se encuentran en el siguiente enlace:

* https://cran.r-project.org/bin/linux/ 

Si estás usando *Linux* no te debería ser difícil seguir las instrucciones presentadas.

## RStudio - un IDE para R
Aunque podemos usar R directamente, es recomendable instalar y usar un entorno integrado de desarrollo (*IDE*, por sus siglas en inglés). 

Podemos utilizar R ejecutando nuestro código directamente desde documentos de texto plano, pero esta es una manera poco efectiva de trabajar, especialmente en proyectos complejos.

Un IDE nos proporciona herramientas para escribir y revisar nuestro código, administrar los archivos que estamos usando, gestionar nuestro entorno de trabajo y algunas otras herramientas de productividad. Tareas que serían difíciles o tediosas de realizar de otro modo, son fáciles a través de un IDE.

Hay varias opciones de IDE para R,  y entre ellas mi preferido es [RStudio](https://www.rstudio.com/). Este entorno, además de incorporar las funciones esenciales de una IDE, es desarrollado por un equipo que ha contribuido de manera significativa para lograr que R sea lenguaje de programación más accesible, con un énfasis en la colaboración y la reproducción de los análisis.

Para instalar RStudio, es necesario con descargar y ejecutar alguno de los instaladores disponibles en su sitio oficial. Están disponibles versiones para *Windows*, *OSX* y *Linux*.

* https://www.rstudio.com/products/rstudio/download/ 

Si ya hemos instalado R en nuestro equipo, RStudio lo detectará automáticamente y podremos utilizarlo desde este entorno. Si no instalamos RStudio antes que R, no hay problema, cada vez que iniciamos este programa, verificará la instalación de R.
