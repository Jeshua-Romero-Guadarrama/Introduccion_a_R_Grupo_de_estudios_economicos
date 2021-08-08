<p align="center"><img align="center" src="https://github.com/Jeshua-Romero-Guadarrama/R_grupo_de_estudios_economicos/blob/main/docs/images/R_grupo_de_estudios_economicos.png" width="30%" height="30%"></p>

# Introducción a R: Grupo de estudios económicos

<br/>
<br/>

## 📖 Sobre el curso

<p><img src="https://github.com/Jeshua-Romero-Guadarrama/Econoalgoritmia/blob/Econoalgoritmia/docs/images/logo.png" alt="logo" align="right" width="20%" height="20%"> 
Los estudiantes con poca experiencia en el análisis avanzado de estadísticas a menudo tienen dificultades para entender los beneficios de desarrollar habilidades de programación al momento de aplicar diversos métodos descriptivos e inferenciales. <i>Introducción a R: Grupo de estudios económicos</i> por Jeshua Romero Guadarrama (2021), ofrece una introducción interactiva a los aspectos esenciales de la programación por medio del lenguaje y software estadístico R, así como una guía para la aplicación de la teoría económica y econométrica en entornos específicos. En otras palabras, el objetivo es que los estudiantes se adentren al mundo de la economía aplicada mediante ejemplos empíricos presentados en la vida diaria y haciendo uso de las habilidades de programación recién adquiridas en la probabilidad y estadística. Dicho objetivo se encuentra respaldado por ejercicios de programación interactivos y la incorporación de visualizaciones dinámicas de conceptos fundamentales mediante la flexibilidad de JavaScript, a través de la biblioteca D3.js.</p>

El curso se puede consultar aquí: [Introducción a R: Grupo de estudios económicos](http://rgrupodeestudioseconomicos.jeshuanomics.com/)

<br/>
<br/>

## 📦 Paquetería necesaria

Para ejecutar los ejemplos mostrados en el libro será necesario tener instalados los siguientes paquetes:

[`lattice`](https://cran.r-project.org/web/packages/lattice/index.html), 
[`ggplot2`](https://cran.r-project.org/web/packages/ggplot2/index.html), 
[`foreign`](https://cran.r-project.org/web/packages/foreign/index.html), 
[`car`](https://cran.r-project.org/web/packages/car/index.html), 
[`leaps`](https://cran.r-project.org/web/packages/leaps/index.html), 
[`MASS`](https://cran.r-project.org/web/packages/MASS/index.html), 
[`RcmdrMisc`](https://cran.r-project.org/web/packages/RcmdrMisc/index.html), 
[`lmtest`](https://cran.r-project.org/web/packages/lmtest/index.html), 
[`glmnet`](https://cran.r-project.org/web/packages/glmnet/index.html), 
[`mgcv`](https://cran.r-project.org/web/packages/mgcv/index.html), 
[`rmarkdown`](https://cran.r-project.org/web/packages/rmarkdown/index.html), 
[`knitr`](https://cran.r-project.org/web/packages/knitr/index.html) y 
[`dplyr`](https://cran.r-project.org/web/packages/dplyr/index.html).

Por ejemplo, ejecutando el siguiente comando:
```{r eval=FALSE}
pkgs <- c("lattice", "ggplot2", "foreign", "car", "leaps", "MASS", "RcmdrMisc", 
          "lmtest", "glmnet", "mgcv", "rmarkdown", "knitr", "dplyr",
          "caret", "rattle", "car", "AppliedPredictiveModeling", "ISLR")

install.packages(setdiff(pkgs, installed.packages()[,"Package"]), dependencies = TRUE)
```

<br/>
<br/>

## ✍🏻 Referencia bibliográfica

Romero, G. J. (2021). *Introducción a R: Grupo de estudios económicos*. JeshuaNomics.

<br/>
<br/>

___

<p align="center"><a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://mirrors.creativecommons.org/presskit/buttons/88x31/svg/by-nc-sa.eu.svg"/></a></p>Esta obra está autorizada bajo la <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.