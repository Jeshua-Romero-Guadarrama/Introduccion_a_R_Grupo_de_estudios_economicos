# Modelos lineales y extensiones {#modelos-lineales}




En los modelo lineales se supone que la funci�n de regresi�n es lineal^[Algunos predictores podr�an corresponderse con interacciones, $X_i = X_j X_k$, o transformaciones (e.g. $X_i = X_j^2$) de las variables explicativas originales. Tambi�n se podr�a haber transformado la respuesta.]:
$$E( Y | \mathbf{X} ) = \beta_{0}+\beta_{1}X_{1}+\beta_{2}X_{2}+\cdots+\beta_{p}X_{p}$$
Es decir, que el efecto de las variables explicativas sobre la respuesta es muy simple, proporcional a su valor, y por tanto la interpretaci�n de este tipo de modelos es (en principio) muy f�cil. 
El coeficiente $\beta_j$ representa el incremento medio de $Y$ al aumentar en una unidad el valor de $X_j$, manteniendo fijos el resto de las covariables.
En este contexto las variables predictoras se denominan habitualmente variables independientes, pero en la pr�ctica es de esperar que no haya independencia entre ellas, por lo que puede no ser muy razonable pensar que al variar una de ellas el resto va a permanecer constante.

El ajuste de este tipo de modelos en la pr�ctica se suele realizar empleando el m�todo de m�nimos cuadrados (ordinarios), asumiendo (impl�citamente o expl�citamente) que la distribuci�n condicional de la respuesta es normal, lo que se conoce como el modelo de regresi�n lineal m�ltiple (siguiente secci�n).

Los modelos lineales generalizados son una extensi�n de los modelos lineales para el caso de que la distribuci�n condicional de la variable respuesta no sea normal (por ejemplo discreta: Bernouilli, Binomial, Poisson...).
En los modelos lineales generalizados se introduce una funci�n invertible *g*, denominada funci�n enlace (o link):
$$g\left(E(Y | \mathbf{X} )\right) = \beta_{0}+\beta_{1}X_{1}+\beta_{2}X_{2}+\cdots+\beta_{p}X_{p}$$
y su ajuste en la pr�ctica se realiza empleando el m�todo de m�xima verosimilitud.

Ambos son modelos cl�sicos de la inferencia estad�stica y, aunque pueden ser demasiado simples en muchos casos, pueden resultar muy �tiles en otros por lo que tambi�n se emplean habitualmente en AE.
Adem�s, como veremos m�s adelante (en las secciones finales de este cap�tulo y en los siguientes), sirve como punto de partida para procedimientos m�s avanzados.
En este cap�tulo se tratar�n estos m�todos desde el punto de vista de AE (descrito en el Cap�tulo \@ref(intro-AE)), es decir, con el objetivo de predecir en lugar de realizar inferencias (y preferiblemente empleando un procedimiento autom�tico y capaz de manejar grandes vol�menes de datos).

En consecuencia, se supondr� que se dispone de unos conocimientos b�sicos de los m�todos cl�sicos de regresi�n lineal y regresi�n lineal generalizada. 
Para un tratamiento m�s completo de este tipo de m�todos se puede consultar Faraway (2014), que incluye su aplicaci�n en la pr�ctica con R (tambi�n el [Cap�tulo 8](https://rubenfcasal.github.io/intror/modelos-lineales.html) de Fern�ndez-Casal *et al*., 2019).
Adem�s por simplicidad, en las siguientes secciones nos centraremos principalmente en el caso de modelos lineales, pero los distintos procedimientos y comentarios se extienden de forma an�loga al caso de modelos generalizados (b�sicamente habr�a que sustituir la suma de cuadrados residual por el logaritmo negativo de la verosimilitud), que ser�n tratados en la �ltima secci�n. 


## Regresi�n lineal m�ltiple {#reg-multiple}

Como ya se coment�, el m�todo tradicional considera el siguiente modelo:
\begin{equation} 
  Y = \beta_{0}+\beta_{1}X_{1}+\beta_{2}X_{2}+\cdots+\beta_{p}X_{p} + \varepsilon,
  (\#eq:modelo-rlm)
\end{equation}
donde $\left(  \beta_{0},\beta_{1},\ldots,\beta_{p}\right)^t$ es un vector de par�metros (desconocidos) y $\varepsilon$ es un error aleatorio normal de media cero y varianza $\sigma^2$.

Por tanto las hip�tesis estructurales del modelo son:

- Linealidad

- Homocedasticidad (varianza constante del error)

- Normalidad (y homogeneidad: ausencia de valores at�picos y/o influyentes)

- Independencia de los errores

Hip�tesis adicional en regresi�n m�ltiple:

- Ninguna de las variables explicativas es combinaci�n lineal de las dem�s.

En el caso de regresi�n m�ltiple es de especial inter�s el fen�meno de la multicolinealidad (o colinearidad) relacionado con la �ltima de estas hip�tesis (que se tratar� en la Secci�n \@ref(multicolinealidad)).
Adem�s se da por hecho que el n�mero de observaciones disponible es como m�nimo el n�mero de par�metros, $n \geq p + 1$.


### Ajuste: funci�n `lm`

El procedimiento habitual para ajustar un modelo de regresi�n lineal a un conjunto de datos es emplear m�nimos cuadrados (ordinarios):

$$\mbox{min}_{\beta_{0},\beta_{1},\ldots,\beta_{p}}  \sum\limits_{i=1}^{n}\left(  y_{i} - \beta_0 - \beta_1 x_{1i} - \cdots - \beta_p x_{pi} \right)^{2}$$

En R podemos emplear la funci�n `lm`:


```r
ajuste <- lm(formula, data, subset, weights, na.action)
```

-   `formula`: f�rmula que especifica el modelo.

-   `data`: data.frame (opcional) con las variables de la formula.

-   `subset`: vector (opcional) que especifica un subconjunto de observaciones.

-   `weights`: vector (opcional) de pesos (m�nimos cuadrados ponderados, WLS).

-   `na.action`: opci�n para manejar los datos faltantes; por defecto `na.omit`.

Alternativamente se puede emplear la funci�n `biglm()` del paquete [`biglm`](https://CRAN.R-project.org/package=biglm) para ajustar modelos lineales a grandes conjuntos de datos (especialmente cuando el n�mero de observaciones es muy grande, incluyendo el caso de que los datos excedan la capacidad de memoria del equipo).
Tambi�n se podr�a utilizar la funci�n `rlm()` del paquete [`MASS`](https://CRAN.R-project.org/package=MASS) para ajustar modelos lineales empleando un m�todo robusto cuando hay datos at�picos.

<!-- 
Proxecci�ns demogr�ficas de Galicia 2011-2030. An�lise dos resultados. Documentos de Traballo. An�lise Econ�mica (IDEGA).  
-->

### Ejemplo

Como ejemplo consideraremos el conjunto de datos *hbat.RData* que contiene observaciones de clientes de la compa��a de distribuci�n industrial HBAT (Hair *et al.*, 1998).
Las variables se pueden clasificar en tres grupos: las 6 primeras (categ�ricas) son caracter�sticas del comprador, las variables de la 7 a la 19 (num�ricas) miden percepciones de HBAT por parte del comprador y las 5 �ltimas son posibles variables de inter�s (respuestas).


```r
load("datos/hbat.RData")
as.data.frame(attr(hbat, "variable.labels"))
```

```
##             attr(hbat, "variable.labels")
## empresa                           Empresa
## tcliente                  Tipo de cliente
## tindustr                   Tipo Industria
## tama�o               Tama�o de la empresa
## region                             Regi�n
## distrib           Sistema de distribuci�n
## calidadp              Calidad de producto
## web      Actividades comercio electr�nico
## soporte                   Soporte t�cnico
## quejas               Resoluci�n de quejas
## publi                          Publicidad
## producto               L�nea de productos
## imgfvent       Imagen de fuerza de ventas
## precio                   Nivel de precios
## garantia         Garant�a y reclamaciones
## nprod                    Nuevos productos
## facturac            Encargo y facturaci�n
## flexprec          Flexibilidad de precios
## velocida             Velocidad de entrega
## satisfac            Nivel de satisfacci�n
## precomen          Propensi�n a recomendar
## pcompra              Propensi�n a comprar
## fidelida      Porcentaje de compra a HBAT
## alianza  Considerar�a alianza estrat�gica
```

Consideraremos como respuesta la variable *fidelida* y, por comodidad, �nicamente las variables continuas correspondientes a las percepciones de HBAT como variables explicativas (para una introducci�n al tratamiento de variables predictoras categ�ricas ver por ejemplo la [Secci�n 8.5](https://rubenfcasal.github.io/intror/modelos-lineales.html#regresion-con-variables-categoricas) de Fern�ndez-Casal *et al*., 2019).

Como ya se coment�, se trata de un m�todo cl�sico de Estad�stica y el procedimiento habitual es emplear toda la informaci�n disponible para construir el modelo y posteriormente (asumiendo que es el verdadero) utilizar m�todos de inferencia para evaluar su precisi�n.
Sin embargo seguiremos el procedimiento habitual en AE y particionaremos los datos en una muestra de entrenamiento y en otra de test.


```r
df <- hbat[, c(7:19, 23)]  # Nota: realmente no copia el objeto...
set.seed(1)
nobs <- nrow(df)
itrain <- sample(nobs, 0.8 * nobs)
train <- df[itrain, ]
test <- df[-itrain, ]

# plot(train)
mcor <- cor(train)
corrplot::corrplot(mcor, method = "ellipse")
```

<img src="37-modelos_lineales_files/figure-html/unnamed-chunk-4-1.png" width="80%" style="display: block; margin: auto;" />

```r
print(mcor, digits = 1)
```

```
##          calidadp    web soporte quejas publi producto imgfvent precio garantia
## calidadp     1.00 -0.088   0.051   0.05 -0.05     0.51    -0.15  -0.43     0.09
## web         -0.09  1.000  -0.009   0.14  0.53     0.03     0.79   0.20     0.08
## soporte      0.05 -0.009   1.000   0.17  0.03     0.17     0.04  -0.11     0.84
## quejas       0.05  0.144   0.172   1.00  0.27     0.53     0.23  -0.06     0.19
## publi       -0.05  0.534   0.026   0.27  1.00     0.15     0.66   0.10     0.04
## producto     0.51  0.027   0.166   0.53  0.15     1.00     0.02  -0.48     0.23
## imgfvent    -0.15  0.787   0.038   0.23  0.66     0.02     1.00   0.20     0.14
## precio      -0.43  0.196  -0.109  -0.06  0.10    -0.48     0.20   1.00    -0.10
## garantia     0.09  0.079   0.841   0.19  0.04     0.23     0.14  -0.10     1.00
## nprod        0.17 -0.049   0.017   0.06  0.05     0.13     0.03  -0.14     0.09
## facturac     0.04  0.209   0.128   0.74  0.26     0.42     0.30  -0.05     0.20
## flexprec    -0.51  0.221  -0.005   0.44  0.27    -0.36     0.29   0.45    -0.03
## velocida     0.04  0.227   0.142   0.88  0.36     0.60     0.29  -0.07     0.18
## fidelida     0.55  0.219   0.070   0.61  0.27     0.67     0.21  -0.19     0.14
##          nprod facturac flexprec velocida fidelida
## calidadp  0.17     0.04   -0.509     0.04     0.55
## web      -0.05     0.21    0.221     0.23     0.22
## soporte   0.02     0.13   -0.005     0.14     0.07
## quejas    0.06     0.74    0.444     0.88     0.61
## publi     0.05     0.26    0.266     0.36     0.27
## producto  0.13     0.42   -0.364     0.60     0.67
## imgfvent  0.03     0.30    0.285     0.29     0.21
## precio   -0.14    -0.05    0.449    -0.07    -0.19
## garantia  0.09     0.20   -0.030     0.18     0.14
## nprod     1.00     0.10    0.015     0.12     0.14
## facturac  0.10     1.00    0.428     0.77     0.50
## flexprec  0.01     0.43    1.000     0.52     0.05
## velocida  0.12     0.77    0.515     1.00     0.68
## fidelida  0.14     0.50    0.055     0.68     1.00
```

<!-- Pendiente: gr�fico compacto de correlaciones -->

En este caso observamos que aparentemente hay una relaci�n (lineal) entre la respuesta y algunas de las variables explicativas (que en principio no parece adecuado suponer que son independientes).
Si consideramos un modelo de regresi�n lineal simple, el mejor ajuste se obtendr�a empleando `velocida` como variable explicativa:


```r
modelo <- lm(fidelida ~ velocida, train)
summary(modelo)
```

```
## 
## Call:
## lm(formula = fidelida ~ velocida, data = train)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -13.8349  -4.3107   0.3677   4.3413  12.3677 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  27.5486     2.6961   10.22   <2e-16 ***
## velocida      7.9736     0.6926   11.51   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 6.403 on 158 degrees of freedom
## Multiple R-squared:  0.4562,	Adjusted R-squared:  0.4528 
## F-statistic: 132.6 on 1 and 158 DF,  p-value: < 2.2e-16
```

```r
plot(fidelida ~ velocida, train)
abline(modelo)
```

<img src="37-modelos_lineales_files/figure-html/unnamed-chunk-5-1.png" width="80%" style="display: block; margin: auto;" />

Para calcular predicciones (estimaciones de la media condicionada), tambi�n intervalos de confianza o de predicci�n, se puede emplear la funci�n `predict()` (consultar la ayuda `help(predict.lm)` para ver todas las opciones disponibles).


```r
valores <- seq(1, 6, len = 100)
newdata <- data.frame(velocida = valores)
pred <- predict(modelo, newdata = newdata, interval = c("confidence"))
# head(pred)
plot(fidelida ~ velocida, train)
matlines(valores, pred, lty = c(1, 2, 2), col = 1)
pred2 <- predict(modelo, newdata = newdata, interval = c("prediction"))
matlines(valores, pred2[, -1], lty = 3, col = 1)
legend("topleft", c("Ajuste", "Int. confianza", "Int. predicci�n"), lty = c(1, 2, 3))
```

<img src="37-modelos_lineales_files/figure-html/unnamed-chunk-6-1.png" width="80%" style="display: block; margin: auto;" />

Para la extracci�n de informaci�n se pueden acceder a los componentes del modelo ajustado o emplear funciones  (gen�ricas; muchas de ellas v�lidas para otro tipo de modelos: rlm, glm...). 
Algunas de las m�s utilizadas son las siguientes:

Funci�n   |   Descripci�n
-------   |   ---------------------------------------------------
`fitted`  |   valores ajustados
`coef`    |   coeficientes estimados (y errores est�ndar)
`confint`   |   intervalos de confianza para los coeficientes
`residuals` |   residuos
`plot`    |   gr�ficos de diagn�stico
`termplot` |  gr�fico de efectos parciales
`anova`   |   calcula tablas de an�lisis de varianza (tambi�n permite comparar modelos)
`influence.measures` |   calcula medidas de diagn�stico ("dejando uno fuera"; LOOCV)
`update`  |   actualiza un modelo (p.e. eliminando o a�adiendo variables)

Ejemplos (no evaluados):


```r
modelo2 <- update(modelo, . ~ . + calidadp)
summary(modelo2)
confint(modelo2)
anova(modelo2)
anova(modelo, modelo2)
oldpar <- par(mfrow=c(1,2))
termplot(modelo2, partial.resid = TRUE)
par(oldpar)
```


## El problema de la multicolinelidad {#multicolinealidad}

Si alguna de las variables explicativas no aporta informaci�n relevante sobre la respuesta puede aparecer el problema de la multicolinealidad. 

En regresi�n m�ltiple se supone que ninguna de las variables explicativas es combinaci�n lineal de las dem�s.
Si una de las variables explicativas (variables independientes) es combinaci�n lineal de las otras, no se pueden determinar los par�metros de forma �nica (sistema singular).
Sin llegar a esta situaci�n extrema, cuando algunas variables explicativas est�n altamente correlacionadas entre s�, tendremos una situaci�n de alta multicolinealidad.
En este caso las estimaciones de los par�metros pueden verse seriamente afectadas:

-   Tendr�n varianzas muy altas (ser�n poco eficientes).

-   Habr� mucha dependencia entre ellas (al modificar ligeramente el
    modelo, a�adiendo o eliminando una variable o una observaci�n,
    se producir�n grandes cambios en las estimaciones de los efectos).
 
Consideraremos un ejemplo de regresi�n lineal bidimensional con datos simulados en el que las dos variables explicativas est�n altamente correlacionadas:


```r
set.seed(1)
n <- 50
rand.gen <- runif # rnorm
x1 <- rand.gen(n)
rho <- sqrt(0.99) # coeficiente de correlaci�n
x2 <- rho*x1 + sqrt(1 - rho^2)*rand.gen(n)
fit.x2 <- lm(x2 ~ x1)
# plot(x1, x2)
# summary(fit.x2)

# Rejilla x-y para predicciones:
x1.range <- range(x1)
x1.grid <- seq(x1.range[1], x1.range[2], length.out = 30)
x2.range <- range(x2)
x2.grid <- seq(x2.range[1], x2.range[2], length.out = 30)
xy <- expand.grid(x1 = x1.grid, x2 = x2.grid)

# Modelo te�rico:
model.teor <- function(x1, x2) x1
# model.teor <- function(x1, x2) x1 - 0.5*x2
y.grid <- matrix(mapply(model.teor, xy$x1, xy$x2), nrow = length(x1.grid))
y.mean <- mapply(model.teor, x1, x2)
```

Tendencia te�rica y valores de las variables explicativas: 


```r
library(plot3D)
ylim <- c(-2, 3) # range(y, y.pred)
scatter3D(z = y.mean, x = x1, y = x2, pch = 16, cex = 1, clim = ylim, zlim = ylim,
          theta = -40, phi = 20, ticktype = "detailed", 
          main = "Modelo te�rico y valores de las variables explicativas",
          xlab = "x1", ylab = "x2", zlab = "y", sub = sprintf("R2(x1,x2) = %.2f", summary(fit.x2)$r.squared),
          surf = list(x = x1.grid, y = x2.grid, z = y.grid, facets = NA))
scatter3D(z = rep(ylim[1], n), x = x1, y = x2, add = TRUE, colkey = FALSE, 
           pch = 16, cex = 1, col = "black")
x2.pred <- predict(fit.x2, newdata = data.frame(x1 = x1.range))
lines3D(z = rep(ylim[1], 2), x = x1.range, y = x2.pred, add = TRUE, colkey = FALSE, col = "black") 
```

<img src="37-modelos_lineales_files/figure-html/unnamed-chunk-9-1.png" width="80%" style="display: block; margin: auto;" />

Simulaci�n de la respuesta:



















































































































