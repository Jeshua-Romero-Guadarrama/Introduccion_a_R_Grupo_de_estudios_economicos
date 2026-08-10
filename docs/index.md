---
title: "Introducción a R: Grupo de estudios económicos"
cover-image: "images/cover.png"
author: "Jeshua Romero Guadarrama, Kevin Fernández, Apocryfo, Jenn, Daniel, Tifany Jiménez, Ernesto, Ezequiel, Rich Conejo, Angiebaram, Jesmarth, Adolfo Robles, Isaac Flores, Abdeel, Roberto Daniel"
date: "2021-08-09"
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












<hr style="background-color:#03193b;height:2px">

<center><img style = 'width:60%;' src='images/R_grupo_de_estudios_economicos.png'></center>

<hr style="background-color:#03193b;height:2px">

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
<br> Última actualización el lunes 09 del 08 de 2021
<br>

<hr style="background-color:#03193b;height:2px">

Los estudiantes con poca experiencia en el análisis estadístico de datos a menudo tienen dificultades para entender los beneficios de desarrollar habilidades de programación al momento de aplicar diversos métodos descriptivos e inferenciales. <i>"Introducción a R: Grupo de estudios económicos</i> por Jeshua Romero Guadarrama (2021), ofrece una introducción interactiva a los aspectos esenciales de la programación por medio del lenguaje y software estadístico R, así como una guía para la aplicación de la teoría en ciencia de datos en la solución de problemas específicos. En otras palabras, el objetivo es que los estudiantes se adentren al mundo de la estadística aplicada mediante ejemplos empíricos presentados en la vida diaria y haciendo uso de las habilidades de programación recién adquiridas. Dicho objetivo se encuentra respaldado por ejercicios de programación interactivos y la incorporación de visualizaciones dinámicas de conceptos fundamentales mediante la flexibilidad de JavaScript, a través de la biblioteca D3.js.

En los últimos años, el lenguaje de programación estadístico R se ha convertido en una parte integral del plan de estudios de las clases de estadística que se imparten en las universidades de todo el mundo. Regularmente una gran parte de los estudiantes no han estado expuestos a ningún lenguaje de programación antes y, por lo tanto, tienen dificultades para participar en el aprendizaje de R por sí mismos. Con poca experiencia en el análisis estadístico, es natural que los novicios tengan dificultades para comprender los beneficios de desarrollar habilidades en R, así como aprender y aplicar los conceptos básicos de la probabilidad y estadística. Estos incluyen particularmente la capacidad de realizar, documentar y comunicar estudios empíricos y tener las facilidades para programar estudios de simulación, lo cual es útil para, por ejemplo, comprender y validar teoremas que generalmente no se asimilan o entienden fácilmente con el estudio de las fórmulas. Al ser un economista aplicado y econometrista, me gustaría que mis colegas desarrollen capacidades de gran valor; en consecuencia, deseo compartir con las nuevas generaciones interesadas en la probabilidad y estadística mis conocimientos.

En lugar de confrontar a los estudiantes con ejercicios de codificación puros y literatura clásica complementaria, he pensado que sería mejor proporcionar material de aprendizaje interactivo que combine el código en R con el contenido de un curso introductorio de probabilidad y estadística que sirva de base para construir conocimientos mucho más avanzaados. El presente trabajo es un complemento empírico interactivo al estilo de un informe de investigación reproducible que permite a los estudiantes no solo aprender cómo los resultados de los estudios de casos se pueden replicar con R, sino que también fortalezcan su capacidad para utilizar las habilidades recién adquiridas en otras aplicaciones empíricas.

#### Las convenciones usadas en el presente curso {-}

+ El texto *en cursiva* indica nuevos términos, nombres, botones y similares.

+ El texto **en negrita** se usa generalmente en párrafos para referirse al código **R**. Esto incluye comandos, variables, funciones, tipos de datos, bases de datos y nombres de archivos.

+ <code>Texto de ancho constante sobre fondo gris</code> indica un código **R** que usted puede escribir literalmente. Puede aparecer en párrafos para una mejor distinción entre declaraciones de código ejecutables y no ejecutables, pero se encontrará principalmente en forma de grandes bloques de código **R**. Estos bloques se denominan fragmentos de código.

#### Reconocimiento {-}

A mi alma máter: Universidad Nacional Autónoma de México (Facultad de Economía y Facultad de Ciencias). Por brindarme valiosas oportunidades que coadyuvaron a mi formación.

<br>
![Creative Commons License](https://mirrors.creativecommons.org/presskit/buttons/88x31/svg/by-nc-sa.eu.svg)

Esta obra está autorizado bajo la [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-nc-sa/4.0/).

## Contenido {-}

Capítulo 1. Introducción: ¿Qué es R y para qué es usado?

Capítulo 2. Instalación

Capítulo 3. Conceptos básicos

Capítulo 4. Tipos de datos

Capítulo 5. Operadores

Capítulo 6. Estructuras de datos

Capítulo 7. Subconjuntos

Capítulo 8. Funciones

Capítulo 9. Estructuras de control

Capítulo 10. La familia apply

Capítulo 11. Importar y exportar datos

Capítulo 12. Gráficas

Capítulo 13. Conclusión

Capítulo 14. Introducción al aprendizaje estadístico 

Capítulo 15. Estructuras de datos

Capítulo 16. Gráficos

Capítulo 17. Manipulación de datos con R

Capítulo 18. Análisis exploratorio de datos

Capítulo 19. Inferencia estadística

Capítulo 20. Modelado de datos

Capítulo 21. Modelos lineales

Capítulo 22. Modelos lineales generalizados

Capítulo 23. Regresión no paramétrica

Capítulo 24. Programación

Capítulo 25. Generación de informes

Referencias

Bibliografía complementaria

Apendices

## Índice de contenido {-}

Capítulo 1. Introducción: ¿Qué es R y para qué es usado?

- Un poco de historia
- ¿Quién usa R?

Capítulo 2. Instalación

- Windows
- OSX
- Linux
- RStudio - un IDE para R

Capítulo 3. Conceptos básicos

- La consola de R
- Ejecutar, llamar, correr y devolver
- Objetos
- Constantes y variables
- Funciones (introducción básica)
- Documentación
- Directorio de trabajo
- Paquetes
- Scripts

Capítulo 4. Tipos de datos

- Datos más comunes
- Entero y numérico
- Cadena de texto
- Factor
- Lógico
- NA y NULL
- Coerción
- Verificar el tipo de un dato

Capítulo 5. Operadores

- Operadores aritméticos
- Operadores relacionales
- Operadores lógicos
- Operadores de asignación
- Orden de operaciones

Capítulo 6. Estructuras de datos

- Vectores
- Matrices y arrays
- Data frames
- Listas
- Coerción

Capítulo 7. Subconjuntos

- Índices
- Nombres
- Subconjuntos por índice y nombre
- El signo de dolar $ y los corchetes dobles [[]]
- Condicionales

Capítulo 8. Funciones

- ¿Por qué necesitamos crear nuestrar propias funciones?
- Funciones definidas por el usuario
- Nuestra primera función
- Definiendo la función crear_histograma()

Capítulo 9. Estructuras de control

- if, else
- for
- while
- break y next
- repeat

Capítulo 10. La familia apply

- Un recordatorio sobre vectorización
- Las funciones de la familia apply
- apply
- lapply

Capítulo 11. Importar y exportar datos

- Descargando datos
- Tablas (datos rectangulares)
- Archivos con una estructura desconocida
- Exportar datos
- Hojas de cálculo de Excel
- Datos de paquetes estadísticos comerciales (SPSS, SAS y STATA)

Capítulo 12. Gráficas

- Datos usados en el capítulo
- La función plot()
- Histogramas
- Gráficas de barras
- Leyendas
- Diagramas de dispersión
- Diagramas de caja
- Gráficos de mosaico
- Exportar gráficos

Capítulo 13. Conclusión

Capítulo 14. Introducción al aprendizaje estadístico 

- El lenguaje y entorno estadístico R
- Entorno de trabajo
- Librerías
- Una primera sesión
- Objetos básicos
- Área de trabajo

Capítulo 15. Estructuras de datos

- Vectores
- Matrices y arrays
- Data frames
- Listas

Capítulo 16. Gráficos

- El comando plot
- Funciones gráficas de bajo nivel
- Ejemplos
- Parámetros gráficos
- Múltiples gráficos por ventana
- Exportar gráficos
- Otras librerías gráficas

Capítulo 17. Manipulación de datos con R

- Lectura, importación y exportación de datos
- Manipulación de datos

Capítulo 18. Análisis exploratorio de datos

- Medidas resumen
- Gráficos

Capítulo 19. Inferencia estadística

- Normalidad
- Contrastes
- Regresión y correlación
- Análisis de la varianza

Capítulo 20. Modelado de datos

- Modelos de regresión
- Fórmulas
- Ejemplo: regresión lineal simple

Capítulo 21. Modelos lineales

- Ejemplo
- Ajuste: función lm
- Predicción
- Selección de variables explicativas
- Regresión con variables categóricas
- Interacciones
- Diagnosis del modelo
- Métodos de regularización
- Alternativas

Capítulo 22. Modelos lineales generalizados

- Ajuste: función glm
- Regresión logística
- Predicción
- Selección de variables explicativas
- Diagnosis del modelo
- Alternativas

Capítulo 23. Regresión no paramétrica

- Modelos aditivos

Capítulo 24. Programación

- Funciones
- Ejecución condicional
- Bucles y vectorización
- Aplicación: validación cruzada

Capítulo 25. Generación de informes

- R Markdown
- Spin

Referencias

Bibliografía complementaria

Apendices

- A Enlaces
- A.1 RStudio
- B Instalación de R
- B.1 Instalación de R en Windows
- C Interfaces gráficas
- C.1 RStudio
- C.2 RCommander
- D Manipulación de datos con dplyr
- D.1 El paquete dplyr
- D.2 Operaciones con variables (columnas)
- D.3 Operaciones con casos (filas)
- D.4 Resumir valores con summarise()
- D.5 Agrupar casos con group_by()
- D.6 Operador pipe %>% (tubería, redirección)
- E Compañías que usan R
- E.1 Microsoft
- E.2 RStudio
