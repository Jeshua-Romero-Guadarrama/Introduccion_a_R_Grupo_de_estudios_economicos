---
title: "Introducción a R: Grupo de estudios económicos"
cover-image: "images/cover.png"
author: "Jeshua Romero Guadarrama, Kevin Fernández, Apocryfo, Jenn, Daniel, Tifany Jiménez, Ernesto, Ezequiel, Rich Conejo, Angiebaram, Jesmarth, Adolfo Robles, Isaac Flores, Abdeel, Roberto Daniel"
date: "`r Sys.Date()`"
site: bookdown::bookdown_site
output:
  bookdown::word_document2: default 
  bookdown::pdf_book:
    pandoc_args: ["+RTS", "-K64m", "-RTS", "--csl", "apa-old-doi-prefix.csl"]
    includes:
      in_header: preamble.tex
    citation_package: natbib
    keep_tex: yes
  bookdown::gitbook:
    config:
      toc:
        collapse: subsection
        scroll_highlight: yes
      fontsettings:
        theme: white
        family: serif
        size: 2
    split_by: section+number
    highlight: tango
    includes:
      in_header: [header_include.html]
always_allow_html: yes
documentclass: book
bibliography: [book.bib, packages.bib]
biblio-style: apalike
biblatexoptions:
  - sortcites
link-citations: yes
github-repo: "https://github.com/Jeshua-Romero-Guadarrama/R_grupo_de_estudios_economicos"
description: "Los estudiantes con poca experiencia en el análisis avanzado de estadísticas a menudo tienen dificultades para entender los beneficios de desarrollar habilidades de programación al momento de aplicar diversos métodos descriptivos e inferenciales. 'Análisis estadístico con R para principiantes' por Jeshua Romero Guadarrama (2021), ofrece una introducción interactiva a los aspectos esenciales de la programación por medio del lenguaje y software estadístico R, así como una guía para la aplicación de la teoría económica y econométrica en entornos específicos. En otras palabras, el objetivo es que los estudiantes se adentren al mundo de la economía aplicada mediante ejemplos empíricos presentados en la vida diaria y haciendo uso de las habilidades de programación recién adquiridas. Dicho objetivo se encuentra respaldado por ejercicios de programación interactivos y la incorporación de visualizaciones dinámicas de conceptos fundamentales mediante la flexibilidad de JavaScript, a través de la biblioteca D3.js."
url: 'https\://rgrupodeestudioseconomicos.jeshuanomics.com'
tags: [Academia, Modelos lineales, Econoalgoritmia, Estadística avanzada, Análisis causal, Programación R]
favicon: "images/logo.png"
---

# Prefacio {-}

```{r, echo = F}
options(knitr.duplicate.label = "allow")
```

```{r, child="_setup.Rmd"}
```

```{r, eval=my_output == "html", echo=FALSE, results='asis'}
cat('<hr style="background-color:#03193b;height:2px">')
```

<center><img style = 'width:60%;' src='images/R_grupo_de_estudios_economicos.png'></center>

```{r, eval=my_output == "html", echo=FALSE, results='asis'}
cat('<hr style="background-color:#03193b;height:2px">')
```

<center><img style = 'width:30%;' src='images/cover.jpg'></center>
<br><center><img style='float: center; width:50%' src='images/logo_claim_en_rgb.png'/></center>
<br><center><a href="https://www.jeshuanomics.com/" target="blank">Publicado por Jeshua Romero Guadarrama en colaboración con JeshuaNomics:</a></center>
<br><center><a href="https://github.com/JeshuaNomics" class="fa fa-github"><span class="label">  Git Hub</span></a>
<a href="https://www.facebook.com/JeshuaNomics/" class="fa fa-facebook"><span class="label">  Facebook</span></a>
<a href="https://twitter.com/JeshuaNomics" class="fa fa-twitter"><span class="label">  Twitter</span></a>
<a href="https://www.linkedin.com/in/jeshua-romero-guadarrama/" class="fa fa-linkedin"><span class="label">  Linkedin</span></a>
<a href="https://vk.com/jeshuanomics" class="fa fa-vk"><span class="label">  Vkontakte</span></a>
<a href="https://jeshuanomics.tumblr.com/" class="fa fa-tumblr"><span class="label">  Tumblr</span></a>
<a href="https://www.youtube.com/channel/UCY7f84mJGvMN7TF7XI4-Jgg?view_as=subscriber/" class="fa fa-youtube-play"><span class="label">  YouTube</span></a>
<a href="https://www.instagram.com/JeshuaNomics/" class="fa fa-instagram"><span class="label">  Instagram</span></a></center>

<br> Jeshua Romero Guadarrama es economista y actuario por la <a href="http://www.economia.unam.mx/">Universidad Nacional Autónoma de México</a>, quien ha construido el presente proyecto en colaboración con <a href="https://www.jeshuanomics.com">JeshuaNomics</a>, ubicado en la Ciudad de México, se puede contactar mediante el siguiente correo electrónico: jeshuanomics@gmail.com.
<br>
<br> `r sf <- lubridate::stamp_date('Última actualización el martes 21 del 05 de 2021'); sf(Sys.Date())`
<br>

```{r, eval=knitr::opts_knit$get("rmarkdown.pandoc.to") == "html", results='asis', echo=FALSE}
cat('<hr style="background-color:#03193b;height:2px">')
```

Los estudiantes con poca experiencia en el análisis estadístico de datos a menudo tienen dificultades para entender los beneficios de desarrollar habilidades de programación al momento de aplicar diversos métodos descriptivos e inferenciales. <i>"Introducción a R: Grupo de estudios económicos</i> por Jeshua Romero Guadarrama (2021), ofrece una introducción interactiva a los aspectos esenciales de la programación por medio del lenguaje y software estadístico R, así como una guía para la aplicación de la teoría en ciencia de datos en la solución de problemas específicos. En otras palabras, el objetivo es que los estudiantes se adentren al mundo de la estadística aplicada mediante ejemplos empíricos presentados en la vida diaria y haciendo uso de las habilidades de programación recién adquiridas. Dicho objetivo se encuentra respaldado por ejercicios de programación interactivos y la incorporación de visualizaciones dinámicas de conceptos fundamentales mediante la flexibilidad de JavaScript, a través de la biblioteca D3.js.

En los últimos años, el lenguaje de programación estadístico R se ha convertido en una parte integral del plan de estudios de las clases de estadística que se imparten en las universidades de todo el mundo. Regularmente una gran parte de los estudiantes no han estado expuestos a ningún lenguaje de programación antes y, por lo tanto, tienen dificultades para participar en el aprendizaje de R por sí mismos. Con poca experiencia en el análisis estadístico, es natural que los novicios tengan dificultades para comprender los beneficios de desarrollar habilidades en R, así como aprender y aplicar los conceptos básicos de la probabilidad y estadística. Estos incluyen particularmente la capacidad de realizar, documentar y comunicar estudios empíricos y tener las facilidades para programar estudios de simulación, lo cual es útil para, por ejemplo, comprender y validar teoremas que generalmente no se asimilan o entienden fácilmente con el estudio de las fórmulas. Al ser un economista aplicado y econometrista, me gustaría que mis colegas desarrollen capacidades de gran valor; en consecuencia, deseo compartir con las nuevas generaciones interesadas en la probabilidad y estadística mis conocimientos.

En lugar de confrontar a los estudiantes con ejercicios de codificación puros y literatura clásica complementaria, he pensado que sería mejor proporcionar material de aprendizaje interactivo que combine el código en R con el contenido de un curso introductorio de probabilidad y estadística que sirva de base para construir conocimientos mucho más avanzaados. El presente trabajo es un complemento empírico interactivo al estilo de un informe de investigación reproducible que permite a los estudiantes no solo aprender cómo los resultados de los estudios de casos se pueden replicar con R, sino que también fortalezcan su capacidad para utilizar las habilidades recién adquiridas en otras aplicaciones empíricas.

#### Las convenciones usadas en el presente curso {-}

+ El texto *en cursiva* indica nuevos términos, nombres, botones y similares.

+ El texto **en negrita** se usa generalmente en párrafos para referirse al código **R**. Esto incluye comandos, variables, funciones, tipos de datos, bases de datos y nombres de archivos.

+ <code>Texto de ancho constante sobre fondo gris</code> indica un código **R** que usted puede escribir literalmente. Puede aparecer en párrafos para una mejor distinción entre declaraciones de código ejecutables y no ejecutables, pero se encontrará principalmente en forma de grandes bloques de código **R**. Estos bloques se denominan fragmentos de código.

#### Reconocimiento {-}

A mi alma máter: Universidad Nacional Autónoma de México (Facultad de Economía y Facultad de Ciencias). Por brindarme valiosas oportunidades que coadyuvaron a mi formación.

```{r, eval=knitr::opts_knit$get("rmarkdown.pandoc.to") == "html", results='asis', echo=FALSE}
cat('<br>
![Creative Commons License](https://mirrors.creativecommons.org/presskit/buttons/88x31/svg/by-nc-sa.eu.svg)

Esta obra está autorizado bajo la [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-nc-sa/4.0/).')
```

## Contenido {-}

1 Introducción: ¿Qué es R y para qué es usado?
1.1 Un poco de historia
1.2 ¿Quién usa R?
2 Instalación
2.1 Windows
2.2 OSX
2.3 Linux
2.4 RStudio - un IDE para R
3 Conceptos básicos
3.1 La consola de R
3.2 Ejecutar, llamar, correr y devolver
3.3 Objetos
3.4 Constantes y variables
3.5 Funciones (introducción básica)
3.6 Documentación
3.7 Directorio de trabajo
3.8 Paquetes
3.9 Scripts
4 Tipos de datos
4.1 Datos más comunes
4.2 Entero y numérico
4.3 Cadena de texto
4.4 Factor
4.5 Lógico
4.6 NA y NULL
4.7 Coerción
4.8 Verificar el tipo de un dato
5 Operadores
5.1 Operadores aritméticos
5.2 Operadores relacionales
5.3 Operadores lógicos
5.4 Operadores de asignación
5.5 Orden de operaciones
6 Estructuras de datos
6.1 Vectores
6.2 Matrices y arrays
6.3 Data frames
6.4 Listas
6.5 Coerción
7 Subconjuntos
7.1 Índices
7.2 Nombres
7.3 Subconjuntos por índice y nombre
7.4 El signo de dolar $ y los corchetes dobles [[]]
7.5 Condicionales
8 Funciones
8.1 ¿Por qué necesitamos crear nuestrar propias funciones?
8.2 Funciones definidas por el usuario
8.3 Nuestra primera función
8.4 Definiendo la función crear_histograma()
9 Estructuras de control
9.1 if, else
9.2 for
9.3 while
9.4 break y next
9.5 repeat
10 La familia apply
10.0.1 Un recordatorio sobre vectorización
10.0.2 Las funciones de la familia apply
10.1 apply
10.2 lapply
11 Importar y exportar datos
11.1 Descargando datos
11.2 Tablas (datos rectangulares)
11.3 Archivos con una estructura desconocida
11.4 Exportar datos
11.5 Hojas de cálculo de Excel
11.6 Datos de paquetes estadísticos comerciales (SPSS, SAS y STATA)
12 Gráficas
12.1 Datos usados en el capítulo
12.2 La función plot()
12.3 Histogramas
12.4 Gráficas de barras
12.5 Leyendas
12.6 Diagramas de dispersión
12.7 Diagramas de caja
12.8 Gráficos de mosaico
12.9 Exportar gráficos
13 Conclusión
14 Introducción
14.1 El lenguaje y entorno estadístico R
14.2 Entorno de trabajo
14.3 Librerías
14.4 Una primera sesión
14.5 Objetos básicos
14.6 Área de trabajo
15 Estructuras de datos
15.1 Vectores
15.2 Matrices y arrays
15.3 Data frames
15.4 Listas
16 Gráficos
16.1 El comando plot
16.2 Funciones gráficas de bajo nivel
16.3 Ejemplos
16.4 Parámetros gráficos
16.5 Múltiples gráficos por ventana
16.6 Exportar gráficos
16.7 Otras librerías gráficas
17 Manipulación de datos con R
17.1 Lectura, importación y exportación de datos
17.2 Manipulación de datos
18 Análisis exploratorio de datos
18.1 Medidas resumen
18.2 Gráficos
19 Inferencia estadística
19.1 Normalidad
19.2 Contrastes
19.3 Regresión y correlación
19.4 Análisis de la varianza
20 Modelado de datos
20.1 Modelos de regresión
20.2 Fórmulas
20.3 Ejemplo: regresión lineal simple
21 Modelos lineales
21.1 Ejemplo
21.2 Ajuste: función lm
21.3 Predicción
21.4 Selección de variables explicativas
21.5 Regresión con variables categóricas
21.6 Interacciones
21.7 Diagnosis del modelo
21.8 Métodos de regularización
21.9 Alternativas
22 Modelos lineales generalizados
22.1 Ajuste: función glm
22.2 Regresión logística
22.3 Predicción
22.4 Selección de variables explicativas
22.5 Diagnosis del modelo
22.6 Alternativas
23 Regresión no paramétrica
23.1 Modelos aditivos
24 Programación
24.1 Funciones
24.2 Ejecución condicional
24.3 Bucles y vectorización
24.4 Aplicación: validación cruzada
25 Generación de informes
25.1 R Markdown
25.2 Spin
Referencias
Bibliografía complementaria
Apendices
A Enlaces
A.1 RStudio
B Instalación de R
B.1 Instalación de R en Windows
C Interfaces gráficas
C.1 RStudio
C.2 RCommander
D Manipulación de datos con dplyr
D.1 El paquete dplyr
D.2 Operaciones con variables (columnas)
D.3 Operaciones con casos (filas)
D.4 Resumir valores con summarise()
D.5 Agrupar casos con group_by()
D.6 Operador pipe %>% (tubería, redirección)
E Compañías que usan R
E.1 Microsoft
E.2 RStudio

## Índice de contenido {-}

Parte I: Introducción al análisis estadístico con **R**

1. Datos, estadísticas y decisiones
    - Las nociones estadísticas (y relacionadas) que solo debe conocer
        + Muestras y poblaciones
        + Variables: dependientes e independientes
        + Tipos de datos
        + Un poco de probabilidad
    - Estadística inferencial: probando hipótesis
        + Hipótesis nulas y alternativas
        + Dos tipos de error

2. **R**: Qué hace y cómo lo hace
    - Descargando **R** y **RStudio**
    - Una sesión con **R**
        + El directorio de trabajo
        + Así que comencemos, ya
        + Datos faltantes
    - Funciones **R**
    - Funciones definidas por el usuario
    - comentarios
    - **R** Estructuras
        + Vectores
        + Vectores numéricos
        + Matrices
        + Factores
        + Listas
        + Listas y estadísticas
        + Marcos de datos
    - Paquetes
    - Más paquetes
    - **R** Fórmulas
    - Leyendo y escribiendo
        + Hojas de cálculo
        + Archivos CSV
        + Archivos de texto

Parte II: Descripción de datos

3. Obtención de gráficos
    - Encontrar patrones
        + Graficar una distribución
        + Salto de bares
        + Rebanar el pastel
        + La trama de dispersión
        + De cajas y bigotes
    - Gráficos básicos **R**
        + Histogramas
        + Añadiendo características gráficas
        + Parcelas de barras
        + Gráficos circulares
        + Gráficos de puntos
        + Parcelas de barras revisitadas
        + Diagramas de dispersión
        + Diagramas de caja
    - Graduarse a ggplot2
        + Histogramas
        + Parcelas de barras
        + Gráficos de puntos
        + Parcelas de barras revisitadas
        + Diagramas de dispersión
        + Diagramas de caja
    - Terminando

4. Encontrar su centro
    - Medios: el atractivo de los promedios
    - El promedio en **R**: mean()
        + ¿Cuál es tu condición?
        + Eliminar $-signos con with()
        + Explorando los datos
        + Valores atípicos: el defecto de los promedios
        + Otros medios para un fin
    - Medianas: atrapadas en el medio
    - La mediana en **R**: median()
    - Estadísticas à la Mode
    - El modo en **R**

5. Desviarse del promedio
    - Medición de la variación
        + Desviaciones cuadradas promedio: varianza y cómo calcularla
        + Varianza de la muestra
        + Varianza en **R**
    - Regreso a las raíces: desviación estándar
        + Desviación estándar de la población
        + Desviación estándar de la muestra
    - Desviación estándar en **R**
    - Condiciones, condiciones, condiciones

6. Cumplimiento de estándares y posiciones
    - Atrapando algunas Z
        + Características de las puntuaciones z
        + Bonos versus Bambino
        + Puntajes de exámenes
    - Puntuaciones estándar en **R**
    - ¿Cuál es tu posición?
        + Clasificación en **R**
        + Puntuaciones empatadas
        + Nth más pequeño, Nth más grande
        + Percentiles
        + Rangos de porcentaje
    - Resumiendo

7. Resumiendo todo
    - ¿Cuántos?
    - Lo alto y lo bajo
    - Viviendo en los momentos
        + Un momento de enseñanza
        + Volver a descriptivos
        + Asimetría
        + Curtosis
    - Sintonización de la frecuencia
        + Variables nominales: table() et al
        + Variables numéricas: hist()
        + Variables numéricas: stem()
    - Resumiendo un marco de datos

8. ¿Qué es normal?
    - Golpear la curva
        + Profundizando
        + Parámetros de una distribución normal
    - Trabajar con distribuciones normales
        + Distribuciones en **R**
        + Función de densidad normal
        + Función de densidad acumulativa
        + Cuantiles de distribuciones normales
        + Muestreo aleatorio
    - Un miembro distinguido de la familia

Parte III: Sacar conclusiones a partir de los datos

9. El juego de la confianza: estimación
    - Comprensión de las distribuciones de muestreo
    - Una idea EXTREMADAMENTE importante: el teorema del límite central
        + (Aproximadamente) Simulando el teorema del límite central
        + Predicciones del teorema del límite central
    - Confianza: ¡tiene sus límites!
        + Encontrar límites de confianza para una media
    - Encajar en una t

10. Prueba de hipótesis de una muestra
    - Hipótesis, pruebas y errores
    - Pruebas de hipótesis y distribuciones muestrales
    - Coger algo de Z de nuevo
    - Prueba Z en **R**
    - t para uno
    - t Prueba en **R**
    - Trabajar con distribuciones t
    - Visualización de distribuciones t
        + Trazado de t en gráficos **R** base
        + Trazando t en ggplot2
        + Una cosa más sobre ggplot2
    - Probando una varianza
        + Pruebas en **R**
    - Trabajar con distribuciones de chi-cuadrado
    - Visualización de distribuciones de chi-cuadrado
        + Trazado de chi-cuadrado en gráficos **R** base
        + Trazar chi-cuadrado en ggplot2

11. Prueba de hipótesis de dos muestras
    - Hipótesis construidas para dos
    - Distribuciones de muestreo revisadas
        + Aplicación del teorema del límite central
        + Z una vez más
        + Prueba Z para dos muestras en **R**
    - t para dos
    - Como guisantes en una vaina: variaciones iguales
    - Prueba t en **R**
        + Trabajando con dos vectores
        + Trabajar con un marco de datos y una fórmula
        + Visualizando los resultados
        + Como p y q: varianzas desiguales
    - Un conjunto emparejado: prueba de hipótesis para muestras emparejadas
    - Prueba t de muestras pareadas en **R**
    - Prueba de dos variaciones
        + Prueba F en **R**
        + F junto con t
    - Trabajar con distribuciones F
    - Visualización de distribuciones F

12. Prueba de más de dos muestras
    - Probando más de dos
        + Un problema espinoso
        + Una solución
        + Relaciones significativas
    - ANOVA en **R**
        + Visualizando los resultados
        + Después del ANOVA
        + Contrastes en **R**
        + Comparaciones no planificadas
    - Otro tipo de hipótesis, otro tipo de prueba
        + Trabajo con ANOVA de medidas repetidas
        + ANOVA de medidas repetidas en **R**
        + Visualizando los resultados
    - Ponerse de moda
    - Análisis de tendencias en **R**

13. Pruebas más complicadas
    - Rompiendo las combinaciones
        + Interacciones
        + El análisis
    - ANOVA bidireccional en **R**
        + Visualización de los resultados bidireccionales
    - Dos tipos de variables. . . En seguida
        + ANOVA mixto en **R**
        + Visualización de los resultados de ANOVA mixtos
    - Después del análisis
    - Análisis multivariado de varianza
        + MANOVA en **R**
        + Visualización de los resultados de MANOVA
        + Después del análisis

14. Regresión: modelo lineal, múltiple y lineal general
    - La trama de la dispersión
    - Graficar líneas
    - Regresión: ¡Qué línea!
        + Uso de regresión para pronosticar
        + Variación alrededor de la línea de regresión
        + Prueba de hipótesis sobre regresión
    - Regresión lineal en **R**
        + Características del modelo lineal
        + Haciendo predicciones
        + Visualización del diagrama de dispersión y la línea de regresión
        + Graficando los residuales
    - Hacer malabares con muchas relaciones a la vez: regresión múltiple
        + Regresión múltiple en **R**
        + Haciendo predicciones
        + Visualización del diagrama de dispersión 3D y el plano de regresión
    - ANOVA: otra mirada
    - Análisis de covarianza: el componente final del GLM
        + Pero espera, hay más

15. Correlación: el auge y la caída de las relaciones
    - Parcelas de dispersión de nuevo
    - Comprensión de la correlación
    - Correlación y regresión
    - Prueba de hipótesis sobre la correlación
        + ¿Un coeficiente de correlación es mayor que cero?
        + ¿Se diferencian dos coeficientes de correlación?
    - Correlación en **R**
        + Calcular un coeficiente de correlación
        + Prueba de un coeficiente de correlación
        + Prueba de la diferencia entre dos coeficientes de correlación
        + Calcular una matriz de correlación
        + Visualización de matrices de correlación
    - Correlación múltiple
        + Correlación múltiple en **R**
        + Ajuste de R-cuadrado
    - Correlación parcial
    - Correlación parcial en **R**
    - Correlación semiparcial
    - Correlación semiparcial en **R**

16. Regresión curvilínea: cuando las relaciones se complican
    - ¿Qué es un logaritmo?
    - ¿Qué es e?
    - Regresión de potencia
    - Regresión exponencial
    - Regresión logarítmica
    - Regresión polinomial: un poder superior
    - ¿Qué modelo debería utilizar?

Parte IV: Trabajar con probabilidad

17. Introducción a la probabilidad
    - ¿Qué es la probabilidad?
        + Experimentos, ensayos, eventos y espacios de muestra
        + Espacios muestrales y probabilidad
    - Eventos compuestos
        + Unión e intersección
        + Intersección de nuevo
    - La probabilidad condicional
        + Trabajando con las probabilidades
        + La base de la prueba de hipótesis
    - Grandes espacios de muestra
        + Permutaciones
        + Combinaciones
    - **R** Funciones para contar reglas
    - Variables aleatorias: discretas y continuas
    - Distribuciones de probabilidad y funciones de densidad
    - La distribución binomial
    - El binomio binomial y el binomio negativo en **R**
        + Distribución binomial
        + Distribución binomial negativa
    - Prueba de hipótesis con la distribución binomial
    - Más sobre pruebas de hipótesis: **R** versus tradición

18. Introducción al modelado
    - Modelado de una distribución
        + Sumergirse en la distribución de Poisson
        + Modelado con la distribución de Poisson
        + Probando el ajuste del modelo
        + Un comentario sobre chisq.test()
        + Jugando a la pelota con un modelo
    - Una discusión simulada
        + Arriesgarse: el método Monte Carlo
        + Cargando los dados
        + Simulando el teorema del límite central

Parte V: La parte de diez

19. Diez consejos para emigrados de Excel
    - Definir un vector en **R** es como nombrar un rango en Excel
    - Operar en vectores es como operar en rangos con nombre
    - A veces, las funciones estadísticas funcionan de la misma manera
    - Y a veces no
    - Contraste: Excel y **R** funcionan con diferentes formatos de datos
    - Las funciones de distribución son (algo) similares
    - Un marco de datos es (algo) como un rango con nombre de varias columnas
    - La función sapply() es como arrastrar
    - Usar edit() es (casi) como editar una hoja de cálculo
    - Utilice el portapapeles para importar una tabla de Excel a **R**

20. Diez valiosos recursos **R** en línea
    - Sitios web para usuarios **R**
        + **R** - blogueros
        + Red de aplicaciones de Microsoft **R**
        + Rápido - **R**
        + **RStudio** Aprendizaje en línea
        + Desbordamiento de pila
    - Libros y documentación en línea
        + **R** manuales
        + Documentación **R**
        + **RDocumentación**
        + USTED PUEDE analizar
        + El diario **R**