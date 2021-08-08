# �rboles de decisi�n {#trees}




Los *�rboles de decisi�n* son uno de los m�todos m�s simples y f�ciles de interpretar para realizar predicciones en problemas de clasificaci�n y de regresi�n. 
Se desarrollan a partir de los a�os 70 del siglo pasado como una alternativa vers�til a los m�todos cl�sicos de la estad�stica, fuertemente basados en las hip�tesis de linealidad y de normalidad, y enseguida se convierten en una t�cnica b�sica del aprendizaje autom�tico. 
Aunque su calidad predictiva es mediocre (especialmente en el caso de regresi�n), constituyen la base de otros m�todos altamente competitivos (bagging, bosques aleatorios, boosting) en los que se combinan m�ltiples �rboles para mejorar la predicci�n, pagando el precio, eso s�, de hacer m�s dif�cil la interpretaci�n del modelo resultante.

La idea de este m�todo consiste en la segmentaci�n (partici�n) del *espacio predictor* (es decir, del conjunto de posibles valores de las variables predictoras) en regiones tan simples que el proceso se pueda representar mediante un �rbol binario. 
Se parte de un nodo inicial que representa a toda la muestra (se utiliza la muestra de entrenamiento), del que salen dos ramas que dividen la muestra en dos subconjuntos, cada uno representado por un nuevo nodo. 
Este proceso se repite un n�mero finito de veces hasta obtener las hojas del �rbol, es decir, los nodos terminales, que son los que se utilizan para realizar la predicci�n.
Una vez construido el �rbol, la predicci�n se realizar� en cada nodo terminal utilizando, t�picamente, la media en un problema de regresi�n y la moda en un problema de clasificaci�n. 

<img src="33-arboles_files/figure-html/unnamed-chunk-2-1.png" width="80%" style="display: block; margin: auto;" />

<!-- 
Pendiente:
Incluir figuras en dos columnas? 
fig.cap="Izquierda: ejemplo de un �rbol obtenido al realizar una partici�n binaria recursiva de un espacio bidimensional. Derecha: superficie de predicci�n correspondiente."
-->


Al final de este proceso iterativo el espacio predictor se ha particionado en regiones de forma rectangular en la que la predicci�n de la respuesta es constante. 
Si la relaci�n entre las variables predictoras y la variable respuesta no se puede describir adecuadamente mediante rect�ngulos, la calidad predictiva del �rbol ser� limitada. 
Como vemos, la simplicidad del modelo es su principal argumento, pero tambi�n su tal�n de Aquiles.

<img src="33-arboles_files/figure-html/unnamed-chunk-3-1.png" width="80%" style="display: block; margin: auto;" />

Como se ha dicho antes, cada nodo padre se divide, a trav�s de dos ramas, en dos nodos hijos. 
Esto se hace seleccionando una variable predictora y dando respuesta a una pregunta dicot�mica sobre ella.
Por ejemplo, �es el sueldo anual menor que 30000 euros?, o �es el g�nero igual a *mujer*? 
Lo que se persigue con esta partici�n recursiva es que los nodos terminales sean homog�neos respecto a la variable respuesta $Y$. 

Por ejemplo, en un problema de clasificaci�n, la homogeneidad de los nodos terminales significar�a que en cada uno de ellos s�lo hay elementos de una clase (categor�a), y dir�amos que los nodos son *puros*. 
En la pr�ctica, esto siempre se puede conseguir construyendo �rboles suficientemente profundos, con muchas hojas. 
Pero esta soluci�n no es interesante, ya que va a dar lugar a un modelo excesivamente complejo y por tanto sobreajustado y de dif�cil interpretaci�n. 
Ser� necesario encontrar un equilibrio entre la complejidad del �rbol y la pureza de los nodos terminales.


En resumen:

- M�todos simples y f�cilmente interpretables.

- Se representan mediante �rboles binarios.

- T�cnica cl�sica de apendizaje autom�tico (computaci�n).

- V�lidos para regresi�n y para clasificaci�n.

- V�lidos para predictores num�ricos y categ�ricos.

La metodolog�a CART (Classification and Regresion Trees, Breiman *et al.*, 1984) es la m�s popular para la construcci�n de �rboles de decisi�n y es la que se va a explicar con algo de detalle en las siguientes secciones. 

En primer lugar se tratar�n los *�rboles de regresi�n* (�rboles de decisi�n en un problema de regresi�n, en el que la variable respuesta $Y$ es num�rica) y despu�s veremos los *arboles de clasificaci�n* (respuesta categ�rica) que son los m�s utilizados en la pr�ctica (los primeros se suelen emplear �nicamente como m�todos descriptivos o como base de m�todos m�s complejos).
Las variables predictoras $\mathbf{X}=(X_1, X_2, \ldots, X_p)$ pueden ser tanto num�ricas como categ�ricas.
Adem�s, con la metodolog�a CART, las variables explicativas podr�an contener datos faltantes.
Se pueden establecer "particiones sustitutas" (*surrogate splits*), de forma que cuando falta un valor en una variable que determina una divisi�n, se usa una variable alternativa que produce una partici�n similar. 


## �rboles de regresi�n CART

Como ya se coment�, la construcci�n del modelo se hace a partir de la muestra de entrenamiento, y 
consiste en la partici�n del espacio predictor en $J$ regiones 
$R_1, R_2, \ldots, R_J$, para cada una de las cuales se va a calcular una constante: 
la media de la variable respuesta $Y$ para las observaciones de entranamiento que 
caen en la regi�n. Estas constantes son las que se van a utilizar para 
la predicci�n de nuevas observaciones; para ello solo hay que comprobar cu�l es 
la regi�n que le corresponde.

La cuesti�n clave es c�mo se elige la partici�n del espacio predictor, para lo 
que vamos a utilizar como criterio de error el RSS (suma de los residuos al cuadrado). 
Como hemos dicho, vamos a modelizar la respuesta en cada regi�n como una constante, 
por tanto en la regi�n $R_j$ nos interesa el 
$min_{c_j} \sum_{i\in R_j} (y_i - c_j)^2$, que se alcanza en la media de las 
respuestas $y_i$ (de la muestra de entrenamiento) en la regi�n $R_j$, 
a la que llamaremos $\widehat y_{R_j}$.
Por tanto, se deben seleccionar las regiones $R_1, R_2, \ldots, R_J$ que minimicen 

$$RSS = \sum_{j=1}^{J} \sum_{i\in R_j} (y_i - \widehat y_{R_j})^2$$ 
(Obs�rvese el abuso de notaci�n $i\in R_j$, que significa las observaciones 
$i\in N$ que verifican $x_i \in R_j$).

Pero este problema es, en la pr�ctica, intratable y vamos a tener que simplificarlo. 
El m�todo CART busca un compromiso 
entre rendimiento, por una parte, y sencillez e interpretabilidad, por otra, y por ello 
en lugar de hacer una b�squeda por todas las particiones posibles sigue un proceso 
iterativo (recursivo) en el que va realizando cortes binarios. En la primera iteraci�n 
se trabaja con todos los datos:

- Una variable explicativa $X_j$ y un punto de corte $s$ definen dos hiperplanos
$R_1 = \{ X \mid X_j \le s \}$ y $R_2 = \{ X \mid X_j > s \}$.

- Se seleccionan los valores de $j$ y $s$ que minimizen 

$$ \sum_{i\in R_1} (y_i - \widehat y_{R_1})^2 + \sum_{i\in R_2} (y_i - \widehat y_{R_2})^2$$

A diferencia del problema original, este se soluciona de forma muy r�pida. A continuaci�n 
se repite el proceso en cada una de las dos regiones $R_1$ y $R_2$, y as� sucesivamente 
hasta alcanzar un criterio de parada.

Fij�monos en que este m�todo hace dos concesiones importantes: no solo restringe la forma 
que pueden adoptar las particiones, sino que adem�s sigue un criterio de error *greedy*: 
en cada iteraci�n busca minimizar el RSS de las dos regiones resultantes, sin preocuparse 
del error que se va a cometer en iteraciones sucesivas. Y fij�monos tambi�n en que este 
proceso se puede representar en forma de �rbol binario (en el sentido de que de cada nodo 
salen dos ramas, o ninguna cuando se llega al final), de ah� la terminolog�a de *hacer 
crecer* el �rbol.

�Y cu�ndo paramos? Se puede parar cuando se alcance una profundidad m�xima, aunque lo 
m�s habitual es, para dividir un nodo (es decir, una regi�n), exigirle un n�mero m�nimo 
de observaciones.

- Si el �rbol resultante es demasiado grande, va a ser un modelo demasiado complejo, 
por tanto va a ser dif�cil de interpretar y, sobre todo, 
va a provocar un sobreajuste de los datos. Cuando se eval�e el rendimiento utilizando 
la muestra de validaci�n, los resultados van a ser malos. Dicho de otra manera, tendremos un 
modelo con poco sesgo pero con mucha varianza y en consecuencia inestable (peque�os 
cambios en los datos dar�n lugar a modelos muy distintos). M�s adelante veremos que esto 
justifica la utilizaci�n del *bagging* como t�cnica para reducir la varianza.

- Si el �rbol es demasiado peque�o, va a tener menos varianza (menos inestable) a costa 
de m�s sesgo. M�s adelante veremos que esto justifica la utilizaci�n del *boosting*. Los 
�rboles peque�os son m�s f�ciles de interpretar ya que permiten identificar las variables 
explicativas que m�s influyen en la predicci�n.

Sin entrar por ahora en m�todos combinados (m�todos *ensemble*, tipo *bagging* o *boosting*), 
vamos a explicar c�mo encontrar un equilibrio entre sesgo y varianza. Lo que se hace es 
construir un �rbol grande para a continuaci�n empezar a *podarlo*. Podar un �rbol significa 
colapsar cualquier cantidad de sus nodos internos (no terminales), dando lugar a otro �rbol m�s 
peque�o al que llamaremos *sub�rbol* del �rbol original. Sabemos que el �rbol completo es 
el que va a tener menor error si utilizamos la muestra de entrenamiento, pero lo que 
realmente nos interesa es encontrar el sub�rbol con un menor error al utilizar la muestra 
de validaci�n. Lamentablemente, no es una buena estrategia el evaluar todos los sub�rboles: 
simplemente, hay demasiados. Lo que se hace es, mediante un 
hiperpar�metro (*tuning parameter* o par�metro de ajuste) controlar el tama�o del �rbol, 
es decir, la complejidad del modelo, seleccionando el sub�rbol *optimo* (para los datos 
de los que disponemos, claro). Veamos la idea.

Dado un sub�rbol $T$ con $R_1, R_2, \ldots, R_t$ nodos terminales, consideramos como 
medida del error el RSS m�s una penalizaci�n que depende de un hiperpar�metro 
no negativo $\alpha \ge 0$

\begin{equation} 
RSS_{\alpha} = \sum_{j=1}^t \sum_{i\in R_j} (y_i - \widehat y_{R_j})^2 + \alpha t
(\#eq:rss-alpha)
\end{equation} 

Para cada valor del par�metro $\alpha$ existe un �nico sub�rbol *m�s peque�o* 
que minimiza este error (obs�rvese que aunque hay un continuo de valores 
distinos de $\alpha$, s�lo hay una cantidad finita de sub�rboles). 
Evidentemente, cuando $\alpha = 0$, ese sub�rbol ser� el �rbol completo, algo que 
no nos interesa. Pero a medida que se incrementa $\alpha$ se penalizan los sub�rboles 
con muchos nodos terminales, dando lugar a una soluci�n m�s peque�a. 
Encontrarla puede parecer muy costoso computacionalmente, pero lo 
cierto es que no lo es. El algoritmo consistente en ir colapsando nodos de forma 
sucesiva, de cada vez el nodo que produzca el menor incremento en el RSS (corregido por 
un factor que depende del tama�o), da 
lugar a una sucesi�n finita de sub�rboles que contiene, para todo $\alpha$, la 
soluci�n.

Para finalizar, s�lo resta seleccionar un valor de $\alpha$. 
Para ello, como se coment� en la Secci�n \@ref(entrenamiento-test), se podr�a dividir la muestra en tres subconjuntos: datos de entrenamiento, de validaci�n y de test. 
Para cada valor del par�metro de complejidad $\alpha$ hemos utilizado la muestra de entrenamiento para obtener un �rbol 
(en la jerga, para cada valor del hiperpar�metro $\alpha$ se entrena un modelo). 
Se emplea la muestra independiente de validaci�n para seleccionar el valor de $\alpha$ (y por tanto el �rbol) con el que nos quedamos. 
Y por �ltimo emplearemos la muestra de test (independiente de las otras dos) para evaluar el rendimiento del �rbol seleccionado. 
No obstante, lo m�s habitual para seleccionar el valor del hiperpar�metro $\alpha$ es emplear validaci�n cruzada (o otro tipo de remuestreo) en la muestra de entrenamiento en lugar de considerar una muestra adicional de validaci�n.

Hay dos opciones muy utilizadas en la pr�ctica para seleccionar el valor de $\alpha$: 
se puede utilizar directamente el valor que minimice el error; o se puede forzar 
que el modelo sea un poco m�s sencillo con la regla *one-standard-error*, que selecciona 
el �rbol m�s peque�o que est� a una distancia de un error est�ndar del �rbol obtenido 
mediante la opci�n anterior.


Tambi�n es habitual escribir la Ecuaci�n \@ref(eq:rss-alpha) reescalando el par�metro de complejidad como $\tilde \alpha = \alpha / RSS_0$, siendo $RSS_0 = \sum_{i=1}^{n} (y_i - \bar y)^2$ la variabilidad total (la suma de cuadrados residual del �rbol sin divisiones):
$$RSS_{\tilde \alpha}=RSS + \tilde \alpha RSS_0 t$$

De esta forma se podr�a interpretar el hiperpar�metro $\tilde \alpha$ como una penalizaci�n en la proporci�n de variabilidad explicada, ya que dividiendo la expresi�n anterior por $RSS_0$ obtendr�amos:
$$R^2_{\tilde \alpha}=R^2+ \tilde \alpha  t$$


## �rboles de clasificaci�n CART

En un problema de clasificaci�n la variable respuesta puede tomar los valores 
$1, 2, \ldots, K$, etiquetas que identifican las $K$ categor�as del problema. 
Una vez construido el �rbol, se comprueba cu�l es la categor�a modal de cada 
regi�n: considerando la muestra de entrenamiento, la categor�a m�s frecuente. 
Dada una observaci�n, se predice que pertenece a la categor�a modal de la 
regi�n a la que pertenece.

El resto del proceso es id�ntico al de los �rboles de regresi�n ya explicado, 
con una �nica salvedad: no podemos utilizar RSS como medida del error. Es 
necesario buscar una medida del error adaptada a este contexto. 
Fijada una regi�n, vamos a denotar por 
$\widehat p_{k}$, con $k = 1, 2, \ldots, K$, a la proporci�n de observaciones 
(de la muestra de entrenamiento) en la regi�n que pertenecen a la categor�a $k$. 
Se utilizan tres medidas distintas del error en la regi�n:

- Proporci�n de errores de clasificaci�n:
    $$1 - max_{k} (\widehat p_{k})$$

- �ndice de Gini:
    $$\sum_{k=1}^K \widehat p_{k} (1 - \widehat p_{k})$$

- Entrop�a^[La entrop�a es un concepto b�sico de la teor�a de la informaci�n (Shannon, 1948) y se mide en *bits* (cuando en la definici�n se utilizan $log_2$).] (*cross-entropy*):
    $$- \sum_{k=1}^K \widehat p_{k} \text{log}(\widehat p_{k})$$

Aunque la proporci�n de errores de clasificaci�n es la medida del error m�s intuitiva, en la pr�ctica s�lo se utiliza para la fase de poda. Fij�monos que en el c�lculo de esta medida s�lo interviene $max_{k} (\widehat p_{k})$, mientras que en las medidas alternativas intervienen las proporciones $\widehat p_{k}$ de todas las categor�as. Para la fase de crecimiento se utilizan indistintamente el �ndice de Gini o la entrop�a. Cuando nos interesa el error no en una �nica regi�n sino en varias (al romper un nodo en dos, o al considerar todos los nodos terminales), se suman los errores de cada regi�n previa ponderaci�n por el n�mero de observaciones que hay en cada una de ellas.

En la introducci�n de este tema se coment� que los �rboles de decisi�n admiten tanto variables predictoras num�ricas como categ�ricas, y esto es cierto tanto para �rboles de regresi�n como para �rboles de clasificaci�n. Veamos brevemente como se tratar�an los predictores categ�ricos a la hora de incorporarlos al �rbol. El problema radica en qu� se entiende por hacer un corte si las categor�as del predictor no est�n ordenadas. Hay dos soluciones b�sicas:

- Definir variables predictoras *dummy*. Se trata de variables indicadoras, una por cada una de las categor�as que tiene el predictor. Este criterio de *uno contra todos* tiene la ventaja de que estas variables son f�cilmente interpretables, pero tiene el inconveniente de que puede aumentar mucho el n�mero de variables predictoras.

- Ordenar las categor�as de la variable predictora. Lo ideal ser�a considerar todas las ordenaciones posibles, pero eso es desde luego poco pr�ctico: el incremento es factorial. El truco consiste en utilizar un �nico �rden basado en alg�n criterio *greedy*. Por ejemplo, si la variable respuesta $Y$ tambi�n es categ�rica, se puede seleccionar una de sus categor�as que resulte especialmente interesante y ordenar las categor�as del predictor seg�n su proporci�n en la categor�a de $Y$. Este enfoque no a�ade complejidad al modelo, pero puede dar lugar a resultados de dif�cil interpretaci�n.

## CART con el paquete `rpart`

La metodolog�a CART est� implementada en el paquete [`rpart`](https://CRAN.R-project.org/package=rpart) 
(Recursive PARTitioning)^[El paquete [`tree`](https://CRAN.R-project.org/package=tree) es una traducci�n del original en S.]. 
La funci�n principal es `rpart()` y habitualmente se emplea de la forma:

`rpart(formula, data, method, parms, control, ...)`  

* `formula`: permite especificar la respuesta y las variables predictoras de la forma habitual, 
  se suele establecer de la forma `respuesta ~ .` para incluir todas las posibles variables explicativas.
  
* `data`: `data.frame` (opcional; donde se evaluar� la f�rmula) con la muestra de entrenamiento.

* `method`: m�todo empleado para realizar las particiones, puede ser `"anova"` (regresi�n), `"class"` (clasificaci�n), 
  `"poisson"` (regresi�n de Poisson) o `"exp"` (supervivencia), o alternativamente una lista de funciones (con componentes 
  `init`, `split`, `eval`; ver la vignette [*User Written Split Functions*](https://cran.r-project.org/web/packages/rpart/vignettes/usercode.pdf)). 
  Por defecto se selecciona a partir de la variable respuesta en `formula`, 
  por ejemplo si es un factor (lo recomendado en clasificaci�n) emplea `method = "class"`.

* `parms`: lista de par�metros opcionales para la partici�n en el caso de clasificaci�n 
  (o regresi�n de Poisson). Puede contener los componentes `prior` (vector de probabilidades previas; 
  por defecto las frecuencias observadas), `loss` (matriz de p�rdidas; con ceros en la diagonal y por defecto 1 en el resto) 
  y `split` (criterio de error; por defecto `"gini"` o alternativamente `"information"`).
  
* `control`: lista de opciones que controlan el algoritmo de partici�n, por defecto se seleccionan mediante la funci�n `rpart.control`, 
  aunque tambi�n se pueden establecer en la llamada a la funci�n principal, y los principales par�metros son:
  
    `rpart.control(minsplit = 20, minbucket = round(minsplit/3), cp = 0.01, xval = 10, maxdepth = 30, ...)`
  
    - `cp` es el par�metro de complejidad $\tilde \alpha$ para la poda del �rbol, de forma que un valor de 1 se corresponde con un �rbol sin divisiones y un valor de 0 con un �rbol de profundidad m�xima. 
      Adicionalmente, para reducir el tiempo de computaci�n, el algoritmo empleado no realiza una partici�n si la proporci�n de reducci�n del error es inferior a este valor (valores m�s grandes simplifican el modelo y reducen el tiempo de computaci�n).
      
    - `maxdepth` es la profundidad m�xima del �rbol (la profundidad de la ra�z ser�a 0).
    
    - `minsplit` y `minbucket` son, respectivamente, los n�meros m�nimos de observaciones en un nodo intermedio para particionarlo 
      y en un nodo terminal.
    
    - `xval` es el n�mero de grupos (folds) para validaci�n cruzada.

Para m�s detalles consultar la documentaci�n de esta funci�n o la vignette [*Introduction to Rpart*](https://cran.r-project.org/web/packages/rpart/vignettes/longintro.pdf).

### Ejemplo: regresi�n

Emplearemos el conjunto de datos *winequality.RData* (ver Cortez et al., 2009), que contiene informaci�n fisico-qu�mica 
(`fixed.acidity`, `volatile.acidity`, `citric.acid`, `residual.sugar`, `chlorides`, `free.sulfur.dioxide`, 
`total.sulfur.dioxide`, `density`, `pH`, `sulphates` y `alcohol`) y sensorial (`quality`) 
de una muestra de 1250 vinos portugueses de la variedad *Vinho Verde*.
Como respuesta consideraremos la variable `quality`, mediana de al menos 3 evaluaciones de la calidad del vino 
realizadas por expertos, que los evaluaron entre 0 (muy malo) y 10 (muy excelente).




























































