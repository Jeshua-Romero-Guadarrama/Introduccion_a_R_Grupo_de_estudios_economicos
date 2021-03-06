# Introducci贸n a R: Grupo de estudios econ贸micos

<br/>
<br/>

<p align="center"><img align="center" src="https://github.com/Jeshua-Romero-Guadarrama/R_grupo_de_estudios_economicos/blob/main/docs/images/R_grupo_de_estudios_economicos.png" width="30%" height="30%"></p>

<br/>
<br/>

## 馃摉 Sobre el curso

<p><img src="https://github.com/Jeshua-Romero-Guadarrama/Econoalgoritmia/blob/main/images/logo.png" alt="logo" align="right" width="20%" height="20%"> 
Los estudiantes con poca experiencia en el an谩lisis avanzado de estad铆sticas a menudo tienen dificultades para entender los beneficios de desarrollar habilidades de programaci贸n al momento de aplicar diversos m茅todos descriptivos e inferenciales. <i>Introducci贸n a R: Grupo de estudios econ贸micos</i> por Jeshua Romero Guadarrama (2021), ofrece una introducci贸n interactiva a los aspectos esenciales de la programaci贸n por medio del lenguaje y software estad铆stico R, as铆 como una gu铆a para la aplicaci贸n de la teor铆a econ贸mica y econom茅trica en entornos espec铆ficos. En otras palabras, el objetivo es que los estudiantes se adentren al mundo de la econom铆a aplicada mediante ejemplos emp铆ricos presentados en la vida diaria y haciendo uso de las habilidades de programaci贸n reci茅n adquiridas en la probabilidad y estad铆stica. Dicho objetivo se encuentra respaldado por ejercicios de programaci贸n interactivos y la incorporaci贸n de visualizaciones din谩micas de conceptos fundamentales mediante la flexibilidad de JavaScript, a trav茅s de la biblioteca D3.js.</p>

El curso se puede consultar aqu铆: [Introducci贸n a R: Grupo de estudios econ贸micos](http://rgrupodeestudioseconomicos.jeshuanomics.com/)

<br/>
<br/>

## 鉁嶐煆? Referencia bibliogr谩fica

Romero, G. J. (2021). *Introducci贸n a R: Grupo de estudios econ贸micos*. JeshuaNomics.

<br/>
<br/>

## 馃摝 Paqueter铆a necesaria

Para ejecutar los ejemplos mostrados en el libro ser谩 necesario tener instalados los siguientes paquetes:

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

___

<p align="center"><a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://mirrors.creativecommons.org/presskit/buttons/88x31/svg/by-nc-sa.eu.svg"/></a></p>Esta obra est谩 autorizada bajo la <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.