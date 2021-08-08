# Modelos lineales y extensiones {#modelos-lineales}




En los modelo lineales se supone que la función de regresión es lineal^[Algunos predictores podrían corresponderse con interacciones, $X_i = X_j X_k$, o transformaciones (e.g. $X_i = X_j^2$) de las variables explicativas originales. También se podría haber transformado la respuesta.]:
$$E( Y | \mathbf{X} ) = \beta_{0}+\beta_{1}X_{1}+\beta_{2}X_{2}+\cdots+\beta_{p}X_{p}$$
Es decir, que el efecto de las variables explicativas sobre la respuesta es muy simple, proporcional a su valor, y por tanto la interpretación de este tipo de modelos es (en principio) muy fácil. 
El coeficiente $\beta_j$ representa el incremento medio de $Y$ al aumentar en una unidad el valor de $X_j$, manteniendo fijos el resto de las covariables.
En este contexto las variables predictoras se denominan habitualmente variables independientes, pero en la práctica es de esperar que no haya independencia entre ellas, por lo que puede no ser muy razonable pensar que al variar una de ellas el resto va a permanecer constante.

El ajuste de este tipo de modelos en la práctica se suele realizar empleando el método de mínimos cuadrados (ordinarios), asumiendo (implícitamente o explícitamente) que la distribución condicional de la respuesta es normal, lo que se conoce como el modelo de regresión lineal múltiple (siguiente sección).

Los modelos lineales generalizados son una extensión de los modelos lineales para el caso de que la distribución condicional de la variable respuesta no sea normal (por ejemplo discreta: Bernouilli, Binomial, Poisson...).
En los modelos lineales generalizados se introduce una función invertible *g*, denominada función enlace (o link):
$$g\left(E(Y | \mathbf{X} )\right) = \beta_{0}+\beta_{1}X_{1}+\beta_{2}X_{2}+\cdots+\beta_{p}X_{p}$$
y su ajuste en la práctica se realiza empleando el método de máxima verosimilitud.

Ambos son modelos clásicos de la inferencia estadística y, aunque pueden ser demasiado simples en muchos casos, pueden resultar muy útiles en otros por lo que también se emplean habitualmente en AE.
Además, como veremos más adelante (en las secciones finales de este capítulo y en los siguientes), sirve como punto de partida para procedimientos más avanzados.
En este capítulo se tratarán estos métodos desde el punto de vista de AE (descrito en el Capítulo \@ref(intro-AE)), es decir, con el objetivo de predecir en lugar de realizar inferencias (y preferiblemente empleando un procedimiento automático y capaz de manejar grandes volúmenes de datos).

En consecuencia, se supondrá que se dispone de unos conocimientos básicos de los métodos clásicos de regresión lineal y regresión lineal generalizada. 
Para un tratamiento más completo de este tipo de métodos se puede consultar Faraway (2014), que incluye su aplicación en la práctica con R (también el [Capítulo 8](https://rubenfcasal.github.io/intror/modelos-lineales.html) de Fernández-Casal *et al*., 2019).
Además por simplicidad, en las siguientes secciones nos centraremos principalmente en el caso de modelos lineales, pero los distintos procedimientos y comentarios se extienden de forma análoga al caso de modelos generalizados (básicamente habría que sustituir la suma de cuadrados residual por el logaritmo negativo de la verosimilitud), que serán tratados en la última sección. 


## Regresión lineal múltiple {#reg-multiple}

Como ya se comentó, el método tradicional considera el siguiente modelo:
\begin{equation} 
  Y = \beta_{0}+\beta_{1}X_{1}+\beta_{2}X_{2}+\cdots+\beta_{p}X_{p} + \varepsilon,
  (\#eq:modelo-rlm)
\end{equation}
donde $\left(  \beta_{0},\beta_{1},\ldots,\beta_{p}\right)^t$ es un vector de parámetros (desconocidos) y $\varepsilon$ es un error aleatorio normal de media cero y varianza $\sigma^2$.

Por tanto las hipótesis estructurales del modelo son:

- Linealidad

- Homocedasticidad (varianza constante del error)

- Normalidad (y homogeneidad: ausencia de valores atípicos y/o influyentes)

- Independencia de los errores

Hipótesis adicional en regresión múltiple:

- Ninguna de las variables explicativas es combinación lineal de las demás.

En el caso de regresión múltiple es de especial interés el fenómeno de la multicolinealidad (o colinearidad) relacionado con la última de estas hipótesis (que se tratará en la Sección \@ref(multicolinealidad)).
Además se da por hecho que el número de observaciones disponible es como mínimo el número de parámetros, $n \geq p + 1$.


### Ajuste: función `lm`

El procedimiento habitual para ajustar un modelo de regresión lineal a un conjunto de datos es emplear mínimos cuadrados (ordinarios):

$$\mbox{min}_{\beta_{0},\beta_{1},\ldots,\beta_{p}}  \sum\limits_{i=1}^{n}\left(  y_{i} - \beta_0 - \beta_1 x_{1i} - \cdots - \beta_p x_{pi} \right)^{2}$$

En R podemos emplear la función `lm`:


```r
ajuste <- lm(formula, data, subset, weights, na.action)
```

-   `formula`: fórmula que especifica el modelo.

-   `data`: data.frame (opcional) con las variables de la formula.

-   `subset`: vector (opcional) que especifica un subconjunto de observaciones.

-   `weights`: vector (opcional) de pesos (mínimos cuadrados ponderados, WLS).

-   `na.action`: opción para manejar los datos faltantes; por defecto `na.omit`.

Alternativamente se puede emplear la función `biglm()` del paquete [`biglm`](https://CRAN.R-project.org/package=biglm) para ajustar modelos lineales a grandes conjuntos de datos (especialmente cuando el número de observaciones es muy grande, incluyendo el caso de que los datos excedan la capacidad de memoria del equipo).
También se podría utilizar la función `rlm()` del paquete [`MASS`](https://CRAN.R-project.org/package=MASS) para ajustar modelos lineales empleando un método robusto cuando hay datos atípicos.

<!-- 
Proxeccións demográficas de Galicia 2011-2030. Análise dos resultados. Documentos de Traballo. Análise Económica (IDEGA).  
-->

### Ejemplo

Como ejemplo consideraremos el conjunto de datos *hbat.RData* que contiene observaciones de clientes de la compañía de distribución industrial HBAT (Hair *et al.*, 1998).
Las variables se pueden clasificar en tres grupos: las 6 primeras (categóricas) son características del comprador, las variables de la 7 a la 19 (numéricas) miden percepciones de HBAT por parte del comprador y las 5 últimas son posibles variables de interés (respuestas).


```r
load("datos/hbat.RData")
as.data.frame(attr(hbat, "variable.labels"))
```

```
##             attr(hbat, "variable.labels")
## empresa                           Empresa
## tcliente                  Tipo de cliente
## tindustr                   Tipo Industria
## tamaño               Tamaño de la empresa
## region                             Región
## distrib           Sistema de distribución
## calidadp              Calidad de producto
## web      Actividades comercio electrónico
## soporte                   Soporte técnico
## quejas               Resolución de quejas
## publi                          Publicidad
## producto               Línea de productos
## imgfvent       Imagen de fuerza de ventas
## precio                   Nivel de precios
## garantia         Garantía y reclamaciones
## nprod                    Nuevos productos
## facturac            Encargo y facturación
## flexprec          Flexibilidad de precios
## velocida             Velocidad de entrega
## satisfac            Nivel de satisfacción
## precomen          Propensión a recomendar
## pcompra              Propensión a comprar
## fidelida      Porcentaje de compra a HBAT
## alianza  Consideraría alianza estratégica
```

Consideraremos como respuesta la variable *fidelida* y, por comodidad, únicamente las variables continuas correspondientes a las percepciones de HBAT como variables explicativas (para una introducción al tratamiento de variables predictoras categóricas ver por ejemplo la [Sección 8.5](https://rubenfcasal.github.io/intror/modelos-lineales.html#regresion-con-variables-categoricas) de Fernández-Casal *et al*., 2019).

Como ya se comentó, se trata de un método clásico de Estadística y el procedimiento habitual es emplear toda la información disponible para construir el modelo y posteriormente (asumiendo que es el verdadero) utilizar métodos de inferencia para evaluar su precisión.
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

<!-- Pendiente: gráfico compacto de correlaciones -->

En este caso observamos que aparentemente hay una relación (lineal) entre la respuesta y algunas de las variables explicativas (que en principio no parece adecuado suponer que son independientes).
Si consideramos un modelo de regresión lineal simple, el mejor ajuste se obtendría empleando `velocida` como variable explicativa:


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

Para calcular predicciones (estimaciones de la media condicionada), también intervalos de confianza o de predicción, se puede emplear la función `predict()` (consultar la ayuda `help(predict.lm)` para ver todas las opciones disponibles).


```r
valores <- seq(1, 6, len = 100)
newdata <- data.frame(velocida = valores)
pred <- predict(modelo, newdata = newdata, interval = c("confidence"))
# head(pred)
plot(fidelida ~ velocida, train)
matlines(valores, pred, lty = c(1, 2, 2), col = 1)
pred2 <- predict(modelo, newdata = newdata, interval = c("prediction"))
matlines(valores, pred2[, -1], lty = 3, col = 1)
legend("topleft", c("Ajuste", "Int. confianza", "Int. predicción"), lty = c(1, 2, 3))
```

<img src="37-modelos_lineales_files/figure-html/unnamed-chunk-6-1.png" width="80%" style="display: block; margin: auto;" />

Para la extracción de información se pueden acceder a los componentes del modelo ajustado o emplear funciones  (genéricas; muchas de ellas válidas para otro tipo de modelos: rlm, glm...). 
Algunas de las más utilizadas son las siguientes:

Función   |   Descripción
-------   |   ---------------------------------------------------
`fitted`  |   valores ajustados
`coef`    |   coeficientes estimados (y errores estándar)
`confint`   |   intervalos de confianza para los coeficientes
`residuals` |   residuos
`plot`    |   gráficos de diagnóstico
`termplot` |  gráfico de efectos parciales
`anova`   |   calcula tablas de análisis de varianza (también permite comparar modelos)
`influence.measures` |   calcula medidas de diagnóstico ("dejando uno fuera"; LOOCV)
`update`  |   actualiza un modelo (p.e. eliminando o añadiendo variables)

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

Si alguna de las variables explicativas no aporta información relevante sobre la respuesta puede aparecer el problema de la multicolinealidad. 

En regresión múltiple se supone que ninguna de las variables explicativas es combinación lineal de las demás.
Si una de las variables explicativas (variables independientes) es combinación lineal de las otras, no se pueden determinar los parámetros de forma única (sistema singular).
Sin llegar a esta situación extrema, cuando algunas variables explicativas estén altamente correlacionadas entre sí, tendremos una situación de alta multicolinealidad.
En este caso las estimaciones de los parámetros pueden verse seriamente afectadas:

-   Tendrán varianzas muy altas (serán poco eficientes).

-   Habrá mucha dependencia entre ellas (al modificar ligeramente el
    modelo, añadiendo o eliminando una variable o una observación,
    se producirán grandes cambios en las estimaciones de los efectos).
 
Consideraremos un ejemplo de regresión lineal bidimensional con datos simulados en el que las dos variables explicativas están altamente correlacionadas:


```r
set.seed(1)
n <- 50
rand.gen <- runif # rnorm
x1 <- rand.gen(n)
rho <- sqrt(0.99) # coeficiente de correlación
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

# Modelo teórico:
model.teor <- function(x1, x2) x1
# model.teor <- function(x1, x2) x1 - 0.5*x2
y.grid <- matrix(mapply(model.teor, xy$x1, xy$x2), nrow = length(x1.grid))
y.mean <- mapply(model.teor, x1, x2)
```

Tendencia teórica y valores de las variables explicativas: 


```r
library(plot3D)
ylim <- c(-2, 3) # range(y, y.pred)
scatter3D(z = y.mean, x = x1, y = x2, pch = 16, cex = 1, clim = ylim, zlim = ylim,
          theta = -40, phi = 20, ticktype = "detailed", 
          main = "Modelo teórico y valores de las variables explicativas",
          xlab = "x1", ylab = "x2", zlab = "y", sub = sprintf("R2(x1,x2) = %.2f", summary(fit.x2)$r.squared),
          surf = list(x = x1.grid, y = x2.grid, z = y.grid, facets = NA))
scatter3D(z = rep(ylim[1], n), x = x1, y = x2, add = TRUE, colkey = FALSE, 
           pch = 16, cex = 1, col = "black")
x2.pred <- predict(fit.x2, newdata = data.frame(x1 = x1.range))
lines3D(z = rep(ylim[1], 2), x = x1.range, y = x2.pred, add = TRUE, colkey = FALSE, col = "black") 
```

<img src="37-modelos_lineales_files/figure-html/unnamed-chunk-9-1.png" width="80%" style="display: block; margin: auto;" />

Simulación de la respuesta:



















































































































