# Introducción: ¿Qué es R y para qué es usado?

R es un lenguaje de programación y entorno computacional dedicado a la estadística. 

Decimos que es un lenguaje de programación porque nos permite dar instrucciones, usando código, a nuestros equipos de cómputo para que realicen tareas específicas (además de que es Turing Completo, pero profundizaremos en ello); para ello sólo necesitamos un intérprete para este código y es a esto a lo que llamamos un entorno computacional. 

Cuando instalamos R en nuestra computadora en realidad lo que estamos instalando es el entorno computacional, y para que podamos hacer algo en ese entorno necesitamos conocer la manera de escribir instrucciones que el software pueda interpretar y ejecutar. Eso es lo que aprenderemos a hacer en este curso.

R es diferente a otros lenguajes de programación que por lo general están diseñados para realizar muchas tareas diferentes; esto es porque fue creado con el único propósito de hacer estadística. Esta característica es la razón de que R sea un lenguaje de programación peculiar, que puede resultar absurdo en algunos sentidos para personas con experiencia en otros lenguajes, pero también es la razón por la que R es una herramienta muy poderosa para el trabajo en estadística, puesto que funciona de la manera que una persona especializada en esta disciplina desearía que lo hiciera.

Para entender mejor estas peculiaridades, nos conviene conocer un poco de los orígenes de este lenguaje de programación.

## Un poco de historia
R tiene sus orígenes en S, un lenguaje de programación creado en los Laboratorios Bell de Estados Unidos. Sí, los mismos laboratorios que inventaron el transistor, el láser, el sistema operativo Unix y algunas otras cosas más. 

Dado que S y sus estándares son propiedad de los Laboratorios Bell, lo cual restringe su uso, Ross Ihaka y Robert Gentleman, de la Universidad de Auckland en Nueva Zelanda, decidieron crear una implementación abierta y gratuita de S. Este trabajo, que culminaría en la creación de R inició en 1992, teniendo una versión inicial del lenguaje en 1995 y en el 2000 una versión final estable.

R hereda muchas características de S, por lo que puedes correr código de este lenguaje usando R sin mayor problema. Para lograr esto, en R frecuentemente existe más de una manera de realizar tareas comunes, una compatible con S y otra diseñada específicamente para R. Lo anterior tiene como resultado inconsistencias, sintaxis poco intuitiva y abundante frustración de cabeza para las personas que quieren aprender R. 

En el presente, el mantenimiento y desarrollo de R es realizado por el R Development Core Team, un equipo de especialistas en ciencias computacionales y estadística provenientes de diferentes instituciones y lugares alrededor del mundo. La versión de R mantenida por este equipo es conocida como “base” y como su nombre indica, es sobre aquella que se crean otras implementaciones de R así como los paquetes que expanden su funcionalidad.

Para lograr que R sea usado sin restricciones es distribuido de manera gratuita, a través de la Licencia Pública General de GNU, por lo que es software libre y de código abierto. Si lo deseas, puedes examinar y estudiar el código que hace que R funcione o puedes crear versiones propias de R que se ajusten a tus necesidades particulares. Esta licencia también te permite usar R para los fines que desees, sin limitaciones, no importando si personales, académicos o comerciales.

En la actualidad, el desarrollo de este lenguaje de programación se mantiene activa. La versión más reciente de R al momento de escribir este documento es la 3.4.2 “Short Summer” fue publicada en septiembre del 2017 y diariamente son publicados nuevos paquetes y sus respectivas actualizaciones.

## ¿Quién usa R?
R es un lenguaje relativamente joven pero que ha experimentado un crecimiento acelerado en su adopción durante los últimos 10 años. 

En septiembre de 2017, de acuerdo al TIOBE programming community index (2017), que es uno de los índices de más prestigio en el mundo en relación popularidad en el uso de lenguajes de programación, R era el lenguaje número 11 en popularidad, después de haber sido el lenguaje número 18 en el 2016. Esto es sobresaliente si consideramos que R es un lenguaje dedicado únicamente a la estadística, mientras que lenguajes como Python (número 5 en 2017) o Java (número 1) son lenguajes que pueden ser usados para todo tipo de tareas, desde crear sitios web hasta programar robots.

La adopción de R se debe en gran medida a que permite responder preguntas mediante el uso de datos de forma efectiva, y como es un lenguaje abierto y gratuito, se facilita compartir código, crear herramientas para solucionar problemas comunes y que todo tipo de personas interesadas en análisis estadísticos puedan participar y contribuir al desarrollo y uso de R, no sólo aquellas que tengan acceso a licencias de software cerrado. 

Incluso compañías e instituciones que no tendrían ninguna dificultad para financiar el costo de licencias de software cerrado utilizan R.

R, por citar un ejemplo, es usado por Facebook para analizar la manera en que sus usuarios interactúan con sus muros de publicaciones para así determinar qué contenido mostrarles. Esta es una tarea muy importante en Facebook, pues las interacciones de los usuarios con publicidad y contenido pagado son la principal fuente de ingreso de esta compañía.  Además de que su división de recursos humanos emplea esta herramienta para estudiar las interacciones entre sus trabajadores. 

Google usa R para analizar la efectividad las campañas de publicidad implementadas en sus servicios, por ejemplo, los anuncios pagados que te aparecen cuando “googleas” algo. Nuevamente, esta es la principal fuente de ingresos de esta compañía. R También es usado para hacer predicciones económicas y otras actividades. 

Microsoft adquirió y ahora desarrolla una versión propia de R llamada OpenR, que ha hecho disponible para uso general del público. OpenR es empleada para realizar todo tipo de análisis estadísticos, por ejemplo, para empatar a jugadores en la plataforma de videojuegos XBOX Live (así que puedes culpar a R cuando te tocan partidas contra jugadores mucho más hábiles que tú). 

Otras compañías que usan R de modo cotidiano son American Express, IBM, Ford, Citibank, HP y Roche, entre  muchas más (Bhalla, 2016; Level, 2017; Microsoft, 2014).

Lo anterior ilustra algunas de las aplicaciones específicas de este lenguaje y de manera general podemos decir que R es usado para procesar, analizar, modelar y comunicar datos. 

Aunque R está diseñado para análisis estadístico, con el paso del tiempo los usuarios de este lenguaje han creado extensiones a R, llamadas paquetes, que han ampliado su funcionalidad. En la actualidad es posible realizar en R minería de textos, procesamiento de imagen, visualizaciones interactivas de datos y  procesamiento de Big Data, entre muchas otras cosas.

Así que, empecemos a usar R.

**Referencias**

* Level (2017). How Big Companies Are Using R for Data Analysis. Recuperado en septiembre de 2017 de: http://www.northeastern.edu/levelblog/2017/05/31/big-companies-using-r-data-analysis/ 
* Microsoft (2014). Companies using R in 2014. Recuperado en septiembre de 2017 de: http://blog.revolutionanalytics.com/2014/05/companies-using-r-in-2014.html 
* Bhalla, D. (2016) Companies using R. Recuperado en septiembre de 2017 de: http://www.listendata.com/2016/12/companies-using-r.html 
* R FAQ. Recuperado en Septiembre de 2017 de: https://cran.r-project.org/doc/FAQ/R-FAQ.html#What-is-R_003f 
* TIOBE Index for September 2017. Recuperado en Septiembre de 2017 de: https://www.tiobe.com/tiobe-index/ 
* Adesanya, T. (2017). A Gentler Introduction to Programming. Recuperado en Septiembre de 2017 de: https://medium.freecodecamp.org/a-gentler-introduction-to-programming-707453a79ee8 
