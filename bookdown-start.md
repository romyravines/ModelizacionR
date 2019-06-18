--- 
title: "Modelización en R"
author: "Aula Innova. Innova-tsn"
date: "Sabadell. Enero-Febrero, 2018"
site: bookdown::bookdown_site
output:
  bookdown::gitbook:
    lib_dir: assets
    split_by: section
    config:
     toc:
       collapse: section
       scroll_highlight: yes
     toolbar:
       position: fixed
     edit : null
     download: yes
     search: yes
     fontsettings:
       theme: white
       family: sans
       size: 2
     sharing: null
  bookdown::pdf_book:
    keep_tex: yes
    fig_caption: true
    latex_engine: xelatex
  bookdown::html_book:
  css: [style.css, font-awesome.min.css]  
documentclass: book
bibliography: [book.bib]
biblio-style: apalike
link-citations: yes
description: "Curso-taller preparado para profesionales analiticos."
---


# Presentación {-}




\includegraphics[width=0.75\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/sabadellcurso} 

**Modelización con R** es un manual práctico para la creación y utilización de modelos analíticos con el lenguaje R [@R-base]. En particular, esta versión del manual ha sido desarrollada para el equipo de _modelling_ del **Banc Sabadell** en Barcelona. 

El objetivo de este manual es presentar de manera resumida, los principales algoritmos del _analytics_, destacando cuándo deben ser utilizados y cómo evaluar su performance.

Cada capítulo corresponde a un tópico. Se pueden leer de manera individual. Todos los ejemplo están desarrollados con datos incluidos en las librerias de R y/o se encuentran disponibles en la web.

Para realizar las prácticas sugeridas en este manual, el lector requiere un portátil donde tenga instaladas las últimas versiones de R y RStudio. Además debe contar con una conexión a internet  y la posibilidad de instalar las librerías que se citan a lo largo del documento.

<!--chapter:end:index.Rmd-->


# (PART) MODELOS ANALÍTICOS {-} 




# Modelizar {#modelizacion}

<!-- ---------## Motivación---------------------------------------------- -->

Los sistemas analíticos  están presentes en todas las áreas de negocio del sector bancario.  A muy alto nivel, se puede contar con modelos predictivos en la gestión de clientes, gestión del riesgo y en el soporte a las operaciones. Así mismo, dichos sistemas pueden estar desagregados por segmento, producto, región geográfica, etc. 

Algunas de las aplicaciones de la modelización son:


<br>
<div class="info">
**Clientes**
</div>

 * Con modelos predictivos se puede mejorar la relación con los clientes, a través de: Anticipación de la Fuga (**Retención**), Incentivo del Uso de Productos (**Fidelización**), Adquisición de Nuevos Productos (**Venta Cruzada**), Anticipación de la Reclamación (**Satisfacción**), Promociones que le interesen (**Gestión de Campañas**), etc. 

<br>

<div class="info">
**Riesgo**
</div>

 * Con modelos analíticos se puede optimizar todo el ciclo de cobranza: Concesión de **créditos**, Anticipar **impagos**, Detectar **fraude**, Optimización de la **Cobranza**, etc.

<br>

<div class="info">
**Operaciones**
</div>

 * Con modelos predictivos se puede hacer más eficientes algunos procesos como: Gestión de **sucursales**, Optimización de **personal**, Gestión de **cajeros** automáticos, Inversión en **marketing**, Gestión del **call center**, etc.

<br>


Las técnicas/modelos analíticos utilizadas dependen del output o target. En general, se trabaja con **outputs binarios** (1-0), **outputs continuos** o, más recientemente, **datos no estructurados**.

<!-- ------------------------------------------------------- -->

## ¿Qué es Modelizar? {#modelizar}



<div class="info">
**Modelizar** es el proceso de crear, desarrollar y validar modelos que sirvan  para convertir los datos en información de negocio significativa que ayude a ejecutar las estrategias y mejorar el rendimiento.
</div>



 > _Data science_  es la disciplina que permite convertir los **datos** en **conocimiento** [@rdc2017].  **Modelizar** es una de las fases del _Data Science_.

<br>

Un esquema aceptado sobre las fases de un proyecto de _Data Science_ es^[Fuente: http://r4ds.had.co.nz/introduction.html]:

<img src='http://r4ds.had.co.nz/diagrams/data-science-model.png' alt="Modelling" style="float:width:90%;">



 > _Data_ y _Analytics_ van de la mano.  

<br>

Más generalmente, como lo expone @mar2015, en el context de _Big Data_, **apply analytics** (A), se encuentra en el corazón de la estrategia para crear  _SMART Business_ y aprovechar al máximo el valor de los datos.

<img src='http://www.meiyusheng.com/wp-content/uploads/2015/04/1.jpg' alt="SMART Model" style="float:width:60%;">

En resumen, **Modelizar** es el proceso de crear, desarrollar y validar modelos que sirvan  para convertir los datos en información de negocio significativa que ayude a ejecutar las estrategias y mejorar el rendimiento.

Los modelos más comunes son: modelos de **regresión**, modelos de **series temporales**, técnicas de **machine learning**, etc.


<!-- ------------------------------------------------------- -->
## ¿Qué es un  modelo analítico?


<div class="info">
Un **modelo** es resumen simple, de baja dimensión, de un conjunto de datos [@rdc2017].
</div>

<br>


Algunas de las afirmaciones tradicionalmente aceptadas sobre un modelo analítico son:

 * Un modelo es una representación simplificada de la realidad.
 * Un modelo es una forma matemática de describir la relación entre una variable de respuesta y un conjunto de variables independientes.
 * Un modelo se puede ver como: (a) Una teoría sobre cómo se generaron los datos y (b) Una forma útil de resumir los datos.
 * A un modelo no se le exige que sea _verdadero_, sino que sea _útil_, de acuerdo a los objetivos para los cuales fue creado.
 * Todos los modelos son errados, pero algunos son útiles.


De manera general, un modelo es una representación del mecanismo generador de los datos. Por ello, un modelo puede verse como un **resumen de la información disponible** y en consecuencia permite compactar los datos existentes. Los modelos se utilizan principalmente para entender dinámicas del mercado, prever el futuro, simular consecuencias ante cambios, evaluar acciones pasadas,  etc.


<div class="rmdcomment">
Los modelos son el elemento central de la creación de inteligencia corporativa.
Condesan y operacionalizan la información existente. Son almacenes y fábricas de conocimiento.
</div>

Cuando se quiere hacer uso de un modelo, se suele identificar:

 * _output_ : variable dependiente, variable respuesta, variable objetivo.
 * _input(s)_ : variable(s) independiente(s), predictor(es), o simplemente feature(s).
 

<!-- ------------------------------------------------------- -->




<!--chapter:end:01-Modelizar.Rmd-->

# Tipos de Problemas



<!-- ## Problemas Supervisados vs. Problemas No Supervisados -->

<!-- Machine learning uses two types of techniques: supervised -->
<!-- learning, which trains a model on known input and output data so -->
<!-- that it can predict future outputs, and unsupervised learning, which -->
<!-- finds hidden patterns or intrinsic structures in input data. -->


\includegraphics[width=0.9\linewidth]{imgs/mlalgorithms01} 

En bastante común que los algoritmos de _Machine Learning_ en aprendizaje supervisado y aprendizaje no supervisado. Esta misma clasificación se menciona en la sección \@ref(clasemodelos), las herramientas de _statistical learning_. Este tipo de clasificación responde al tipo de problema e información que disponemos del _output_, por ello, en este Manual generalizamos esta clasificación y la denominamos **Tipo de Problema Analítico** que debemos afrontar.



<br>

<div class="info">
**Problema / Aprendizaje Supervisado**
</div>



En el **aprendizaje supervisado**, cada dato, unidad analizada u observación está etiquetada o asociada con una categoría o valor de interés. Ejemplos:

 * Una imagen es etiquetada como un 'gato' o 'perro'. 
 * Un cliente es etiquetado como 'propenso' o 'no propenso' al uso del canal digital.
 * El precio de venta asociado a un coche usado, es una etiqueta de valor. 
 
El objetivo del aprendizaje supervisado es estudiar muchos ejemplos etiquetados y, luego, poder realizar predicciones sobre los datos futuros. Por ejemplo, identificar nuevas fotografías con el animal correcto, identificar clientes a clientes facilitar el uso de la banca online o asignar precios de venta precisos a otros coches usados. 

El aprendizaje supervisado usa técnicas de **clasificación** y **regresión** para desarrollar modelos predictivos.

  * Las técnicas de **clasificación** predicen **respuestas discretas** —por ejemplo, saber si un correo es genuino o _spam_, o si un tumor es benigno o maligno. Los modelos de clasificación categorizan los datos de entrada. Entre las aplicaciones típicas se incluyen imágenes médicas, reconocimiento de voz o puntaje crediticio. Cuando hay sólo dos opciones, se denomina clasificación de dos clases o binaria. Cundo hay más categorías, se denomina clasificación multiclase o multinomial.
  
    * En algunos casos la **detección de anomalías** se considera una técnica adicional de clasificación. En la detección de fraude, por ejemplo, los patrones de gasto de tarjeta de crédito muy poco habituales son sospechosos. Las posibles variaciones son tan numerosas y los ejemplos de formación son tan pocos, que no es posible saber de qué actividad fraudulenta se trata. El enfoque que toma la detección de anomalías es simplemente
  aprender qué puede considerarse como actividad normal (haciendo uso de las transacciones no fraudulentas del historial) e identificar todo lo que sea significativamente diferente^[Fuente:https://docs.microsoft.com/es-es/azure/machine-learning/studio/algorithm-choice].
  
  * Las técnicas de **reducción de dimensionalidad ** ayudan a disminuir la complejidad de los problemas debida al gran volumen de datos. Cuando mayor es el conjunto de datos, mayor la necesidad de reducir el número de variables (_features_) que se quieren analizar.
  
  * Las técnicas de **regresión** predicen **respuestas continuas** —por ejemplo, cambios en la temperatura o fluctuaciones en la demanda de energía. Las aplicaciones típicas pueden ser previsión del recurso eléctrico o trading algorítmico.
  


<br> 



<div class="info">
**Problema / Aprendizaje No Supervisado**
</div>



En el **aprendizaje no supervisado**, los datos no tienen etiquetas asociadas a ellos. En este caso, el objetivo  es organizar los datos de alguna manera o describir su estructura. Esto puede significar agrupar clientes en segmentos, o buscar diferentes maneras de examinar datos complejos para que parezcan más simples.

El aprendizaje no supervisado se utiliza en análisis exploratorio de datos para encontrar características ocultas y agrupar. Las aplicaciones del clustering incluyen análisis de secuencias genéticas, investigación de mercado y reconocimiento de objetos.



<!-- The aim of **supervised** machine learning is to build a model -->
<!-- that makes predictions based on evidence in the presence of -->
<!-- uncertainty. A supervised learning algorithm takes a known set of -->
<!-- input data and known responses to the data (output) and trains a -->
<!-- model to generate reasonable predictions for the response -->
<!-- to new data. -->
<!-- Supervised learning uses classification and regression techniques -->
<!-- to develop predictive models. -->

<!--  - Classification techniques predict discrete responses—for -->
<!-- example, whether an email is genuine or spam, or whether -->
<!-- a tumor is cancerous or benign. Classification models -->
<!-- classify input data into categories. Typical applications -->
<!-- include medical imaging, speech recognition, and -->
<!-- credit scoring. -->

<!--  - Regression techniques predict continuous responses— -->
<!-- for example, changes in temperature or fluctuations in -->
<!-- power demand. Typical applications include -->
<!-- electricity load forecasting and algorithmic trading. -->



<!-- El objetivo del machine learning **supervisado** es construir un modelo que haga predicciones basadas en la evidencia en un escenario de incertidumbre. Un algoritmo de aprendizaje supervisado toma un conjunto conocido de entrada y su respuesta para dicha entrada (salida) para entrenar el modelo y generar predicciones razonables de respuesta a nuevos conjuntos de entrada. -->


<!-- **Unsupervised learning** finds hidden patterns or intrinsic structures -->
<!-- in data. It is used to draw inferences from datasets consisting of -->
<!-- input data without labeled responses. -->
<!-- Clustering is the most common unsupervised learning -->
<!-- technique. It is used for exploratory data analysis to find hidden -->
<!-- patterns or groupings in data. -->
<!-- Applications for clustering include gene sequence analysis, -->
<!-- market research, and object recognition. -->


## Enfoques de Modelización {#clasemodelos}


<div class="info">
_Statistical Learning_ se refiere a un conjunto de herramientas para modelar y comprender conjuntos de datos complejos.
</div>

<br>

_Statistical Learning_ es un término presentado en @isl2014. Hace referencia a un área de reciente desarrollo en estadística, que se combina con desarrollos paralelos de ciencias de la computación (específicamente, _Machine Learning_). Se refiere a un ámplio conjunto de herramientas para **entender datos**. Estas herramientas pueden ser: **supervisadas** o **no supervisadas**. De manera muy genérica, en los problemas supervisados se busca estimar o prever un _output_ basado en uno o más _inputs_. En los problemas no supervisados, se cuenta con los _inputs_ pero con un _output_, por lo que se busca entender la estructura de los datos.

Otra forma de clasificar los métodos para modelizar se basa en su objetivo y forma de construcción. Cuando se prioriza la **interpretación del modelo**, buscando que expliquen las relaciones entre _output_ e _inputs_, se habla de **modelos estadísticos**. Cuando se prioriza la **precisión de la previsión** se habla de algoritmos de **machine learning**.



<div class="rmdcomment">
Tipos de modelos analíticos: *Modelos Estadísticos* y *Machine Learning*^[Fuente: http://www.kdnuggets.com/2016/11/machine-learning-vs-statistics.html]. Los primeros hacen uso de la probabilidad (inferencia), son explicativos y predictivos. Los segundos suelen ser 'cajas negras', se centran en la previsión y el trabajo con grandes volúmenes de datos^[Ver  https://www.quora.com/What-is-the-difference-between-statistics-and-machine-learning].
</div>

<br>

<img src='https://www.edvancer.in/wp-content/uploads/2016/01/ML-vs.-stats1.png' alt="Model" style="float:width:90%;">

<br>

El objetivo de los modelos o algoritmos de **Machine Learning** es enseñar a las computadoras a hacer lo que es natural para humanos y animales: **aprender de la experiencia**. Estos algoritmos utilizan métodos computacionales para "aprender" información directamente de los datos, sin depender de una ecuación predeterminada como modelo. Los algoritmos mejoran su rendimientode forma adaptativa  conforme aumenta la cantidad de muestras (datos) disponibles
para el aprendizaje.

<!-- **Machine learning** teaches computers to do what comes naturally to -->
<!-- humans and animals: learn from experience. Machine learning algorithms -->
<!-- use computational methods to “learn” information directly from data -->
<!-- without relying on a predetermined equation as a model. The algorithms -->
<!-- adaptively improve their performance as the number of samples available -->
<!-- for learning increases. -->


<!-- Machine learning uses two types of techniques: supervised -->
<!-- learning, which trains a model on known input and output data so -->
<!-- that it can predict future outputs, and unsupervised learning, which -->
<!-- finds hidden patterns or intrinsic structures in input data. -->


<!-- **Machine learning** requires no prior assumptions about the underlying relationships between the variables. You just have to throw in all the data you have, and the algorithm processes the data and discovers patterns, using which you can make predictions on the new data set. Machine learning treats an algorithm like a black box, as long it works. -->

<br>

<div class="info">
**Machine Learning**
</div>


El _Machine Learning_ no requiere hipótesis previas sobre las relaciones subyacentes entre las variables (o inputs). Sólo se deben ingresar todos los datos que se diponga, y el algoritmo procesa los datos y descubre patrones, con los cuales puede hacer predicciones sobre el nuevo conjunto de datos. El aprendizaje automático trata un algoritmo como una **black box** (caja negra), siempre que funcione. En otras palabras, su principal objetivo es la previsión.

<br>


<!-- In contrast, **statisticians** must understand how the data was collected, statistical properties of the estimator, the underlying distribution of the population they are studying and the kinds of properties you would expect if you did the experiment many times. You need to know precisely what you are doing and come up with parameters that will provide the predictive power.  -->

<div class="info">
**Modelos Estadísticos**
</div>

Por el contrario, los **estadísticos** deben comprender cómo se recopilaron los datos, las propiedades estadísticas de los estimadores, la distribución subyacente de la población que están estudiando y los tipos de propiedades que esperaría si hiciera el experimento muchas veces. Necesita saber exactamente lo que está haciendo y proponer parámetros que le proporcionen el poder predictivo.



<!--chapter:end:01a-Modelos.Rmd-->

# Catálogo de Técnicas





<div class="rmdcomment">
**¿Tus datos pueden ser etiquetados o categorizados?** 

Si tus datos pueden ser separados en clases o grupos específicos, usa algoritmos de clasificación.
</div>


<div class="rmdcomment">
**¿Estás trabajando con datos de un rango?** 

Si la naturaleza de tu respuesta es un número real - como la temperatura o el tiempo hasta que un cajero automático falle -, usa modelos o algoritmos de regresión.
</div>



<div class="rmdcomment">
**¿Aún no sabes como agrupar tus datos:**

 * Usa clúster jerárquico para encontrar posibles estructuras en los datos.
 * Usa la evaluación de clústers para encontrar el 'mejor' número de grupos.
</div>



## Técnicas de _Clustering_


\includegraphics[width=0.8\linewidth]{imgs/unsupervisedtechniques} 

<!-- most unsupervised learning techniques are -->
<!-- a form of cluster analysis. -->
<!-- In cluster analysis, data is partitioned into groups based on some -->
<!-- measure of similarity or shared characteristic. Clusters are formed -->
<!-- so that objects in the same cluster are very similar and objects in -->
<!-- different clusters are very distinct. -->
<!-- Clustering algorithms fall into two broad groups: -->
<!--  -  **Hard clustering**, where each data point belongs to only -->
<!-- one cluster -->
<!--  - **Soft clustering**, where each data point can belong to more -->
<!-- than one cluster -->
<!-- You can use hard or soft clustering techniques if you already know -->
<!-- the possible data groupings. -->

 > La mayoría de las técnicas de aprendizaje no supervisado son una forma de análisis por cluster.

<br>

En análisis por cluster, los datos son divididos en grupos de acuerdo con alguna métrica de similaridad o característica compartida. De esta forma los objetos o instancias en el mismo clúster son muy similares y los de distintos muy diferentes.

Los algoritmos de clustering se dividen en dos grandes grupos^[Fuente: https://es.mathworks.com/discovery/machine-learning.html]:

 - **Clustering rígido**, donde cada dato pertenece únicamente a un clúster.
 
 - **Clustering suave**, donde cada dato puede pertenecer a más de un clúster.
 


<!-- ```{block, type='FOO'} -->
<!-- **k-means**  -->

<!--   k-medias -->

<!-- - Partitions data into k number of mutually exclusive clusters. -->
<!-- How well a point fits into a cluster is determined by the -->
<!-- distance from that point to the cluster’s center.  -->
<!-- - When the number of clusters is known. For fast clustering of large data sets -->

<!-- More text. -->
<!-- ``` -->

<br>

<div class="info">
**k-means**
</div>


 - _¿Cómo trabaja?_ Particiona datos en k número de clusters mutuamente excluyentes. El como de bien un punto se ajuste a un clúster determinado viene dado por su distancia al centro de dicho clúster.
 
 - _¿Cuándo se usa?_ Cuando el número de clusters es conocido y cuando se requiere un clustering rápido  de grandes conjuntos de datos.
 
 - _¿Cuál es el resultado?_ Centroide de cada cluster.

<br>



<div class="info">
 **k-medoids**
</div>



 - _¿Cómo trabaja?_  Algoritmo similar a k-medias pero requiere de que los centroides sean puntos u observaciones de la muestra.
 
 - _¿Cuándo se usa?_ Cuando el número de clusters es conocido. Para clustering rápido de datos categóricos. Para escalar a grandes conjuntos de datos.
 
 - _¿Cuál es el resultado?_ Observación o individuo de la muestra que actúa de centroide o medoide de cada cluster.


<br>

<div class="info">
 **Hierarchical Clustering**
</div>



 - _¿Cómo trabaja?_ Produce conjuntos anidados de datos analizando similaridades entre pares de puntos y agrupando objetos en un arbol binario jerárquico. 
 
 - _¿Cuándo se usa?_  Cuando se desconoce el número de clusters a los que darán lugar los datos. Cuando se requiere de visualización para guiar la elección.
 
 - _¿Cuál es el resultado?_  Dendograma mostrando la relación jerárquica entre los clusters.

 
<br>

<div class="info">
 **Self-Organizing Map**
</div>
  

 - _¿Cómo trabaja?_   Red neuronal basada en clustering que transforma un conjunto de datos en un mapa 2D con preservación de topología.

 
 - _¿Cuándo se usa?_  Para observar datos de alta dimensionalidad en mapas 2D o 3D. Para deducir la dimensionalidad de los datos preservando su topología (forma).
 
 
 - _¿Cuál es el resultado?_ Representación en dimensión más baja (típicamente en 2D)
 


<br>

<div class="info">
 **Fuzzy c-Means**
</div>


 - _¿Cómo trabaja?_  Agrupamiento difuso. Agrupamiento basado en particiones en el que los datos pueden estar en más de un cluster.

 
 - _¿Cuándo se usa?_  Cuando el número de clusters es conocido. Para reconocimento de patrones. Cuando los clusters se sobreponen o se solopan.
 
 - _¿Cuál es el resultado?_  Centro de los clústers (similar a k-means) pero con difusión (_fuzziness_) de forma que las observaciones o individuos pueden pertenecer a más de 1 cluster.


<br>

<div class="info">
 **Gaussian Mixture Model**
</div>
  

 - _¿Cómo trabaja?_   Modelo gaussiando mixto. Agrupación basada en particiones en la que los datos provienen de diferentes distribuciones normales multivariantes con ciertas probabilidades.
 
 - _¿Cuándo se usa?_  Cuando un punto puede pertenecer a más de un clúster. Cuando los clusters diferentes tamaños y correlaciones entre ellos.
 
 - _¿Cuál es el resultado?_ Modelo de distribuciones gausianas que proporciona la probabilidad de que una observación o individuo pertenezca a un clúster.


 



## Técnicas de Clasificación



\includegraphics[width=0.8\linewidth]{imgs/classificationtechniques} 

<br>

<div class="info">
 **Regresión Logística**
</div>
  

 - _¿Cómo trabaja?_  Ajusta un modelo que puede predecir la probabilidad de que una respuesta binaria pertenezca a una clase u otra. Debido a su simplicidad, la regresión logística se utiliza comúnmente como punto de partida para los problemas de clasificación binaria.
 
 - _¿Cuándo se usa?_ Cuando los datos se pueden separar claramente por un solo límite lineal. Como una línea de base (_baseline_) para evaluar más complejos métodos de clasificación.
 
 
<br>

<div class="info">
 **k Vecinos Cercanos (kNN)**
</div>
  

 - _¿Cómo trabaja?_  kNN categoriza los objetos en función de las clases de su
vecinos más cercanos en el conjunto de datos. Las predicciones de kNN suponen
que los objetos cercanos entre sí son similares. Algunas de las métricas de distancia utilizadas para encontrar el vecino más cercano son: Euclides, _bloque de la ciudad_city block_, coseno y Chebychev.
 
 - _¿Cuándo se usa?_ Cuando se requiere un algoritmo simple para establecer
reglas de aprendizaje de referencia o base. Cuando el uso de memoria del modelo entrenado no es una preocupación.
Cuando la velocidad de predicción del modelo entrenado tampoco constituye una limitación.
 

<br>

<div class="info">
 **Support Vector Machines (SVM)**
</div>
  

 - _¿Cómo trabaja?_  Clasifica datos encontrando el límite de decisión lineal (hiperplano) que separa todos los puntos de datos de una clase de los de la otra clase. El mejor hiperplano para una SVM es aquel con el mayor margen entre las dos clases, cuando los datos son linealmente separables. Si los datos no son linealmente separables, se utiliza una función de pérdida para penalizar los puntos en el lado equivocado del hiperplano Los SVM a veces usan una transformación de núcleo para transformar los datos no separables linealmente  en dimensiones más altas donde un límite de decisión lineal puede ser encontrado.
 
 - _¿Cuándo se usa?_ Para datos que tienen exactamente dos clases.Para datos de alta dimensión, no linealmente separables.Cuando se necesita un clasificador que sea simple, fácil de interpretar y preciso.
 

<br>

<div class="info">
 **Redes Neuronales**
</div>
  

 - _¿Cómo trabaja?_  Inspirada en el cerebro humano, una red neuronal consiste enredes de neuronas altamente conectadas que relacionan las entradas a las salidas deseadas La red se entrena de forma iterativa, modificando las fortalezas de las conexiones para que las entradas se asignen a la respuesta correcta.
 
 - _¿Cuándo se usa?_ Para modelar sistemas altamente no lineales. Cuando los datos están disponibles de forma incremental y se desea actualiza constantemente el modelo. Cuando podría haber cambios inesperados en su
datos de entrada. Cuando la interpretabilidad del modelo no es una preocupación importante.



<br>

<div class="info">
 **Árboles de Decisión**
</div>
  

 - _¿Cómo trabaja?_  Un árbol de decisión permite predecir respuestas a datos siguiendo
las decisiones organizadas en un árbol, desde la raíz (inicio) hasta un nodo u hoja. Un árbol consiste en condiciones organizadas en forma de ramificaciones, donde el valor de un predictor se compara con un peso entrenado. Los número de ramas y los valores de los pesos se determinan
en el proceso de entrenamiento. Algunas acciones adicionales, como la poda, se pueden usar para simplificar el modelo.

 
 - _¿Cuándo se usa?_ Cuando se necesita un algoritmo fácil de interpretar y
rápido de ejecutar. Para minimizar el uso de memoria. Cuando la precisión predictiva alta no es un requisito.


<br>

<div class="info">
 **Bagging, Boosting**
</div>
  

 - _¿Cómo trabaja?_  Varios árboles de decisión "más débiles" son
combinados en un conjuto "más fuerte". Un árbol de decisión en bolsas (bagging) consta de árboles entrenados
de forma independiente en los datos que se remuestrean (_boostrapping_) a partir de los datos de entrada. _Boosting_ implica crear un modelo fuerte mediante la adición iterativa de modelos "débiles" y ajustando el peso de cada modelo débil para centrarse en ejemplos mal clasificados.


 
 - _¿Cuándo se usa?_ Cuando los predictores son categóricos (discretos) o se comportan
no lineal.
 
<br>

<div class="info">
 **Análisis Discriminante**
</div>
  

 - _¿Cómo trabaja?_  Clasifica los datos a partir de combinaciones lineales de los _inputs_. El análisis discriminante asume que las diferentes clases de datos se pueden generar a partir de distribuciones gaussianas. Entrenar o ajustar un modelo de análisis discriminante implica encontrar los parámetros para la distribución gaussiana de cada clase. 

 
 - _¿Cuándo se usa?_ Cuando necesitas un modelo simple que sea fácil de interpretar. Cuando el uso de la memoria durante el entrenamiento es una preocupación. Cuando necesitas un modelo que sea rápido para predecir.

## Técnicas de Regresión




\includegraphics[width=0.8\linewidth]{imgs/regressiontechniques} 
<br>

<div class="info">
 **Regresión Lineal**
</div>
  

 - _¿Cómo trabaja?_  La regresión lineal es una clase de modelo estadístico utilizado para
describir una variable de respuesta continua como una función lineal de una o más variables predictoras. Dado que los modelos de regresión lineal son simples de interpretar y fáciles de entrenar,
a menudo constituyen el primer modelo que se ajusta a un nuevo conjunto de datos.
 
 - _¿Cuándo se usa?_  Cuando se necesita un algoritmo  fácil de interpretar y rápido de ejecutar. Como línea de base para evaluar otros modelos de regresión más complejos.

 
 
<br>


<div class="info">
 **SVM Regression**
</div>
  

 - _¿Cómo trabaja?_  Los algoritmos de regresión SVM funcionan como los algoritmos de clasificación SVM, pero están modificados para poder predecir una respuesta continua. En lugar de encontrar un hiperplano que
separa los datos, los algoritmos de regresión SVM encuentran un modelo que se desvía (aleja) de los datos observados por un valor no mayor que una pequeña cantidad, con valores que son tan pequeños como
posible (para minimizar la sensibilidad al error).

 
 - _¿Cuándo se usa?_ Para datos de alta dimensión (donde habrá una gran
cantidad de variables predictoras)



<br>

<div class="info">
 **Generalized Linear Models**
</div>
  

 - _¿Cómo trabaja?_  Un modelo lineal generalizado es un caso especial de modelo no lineal. Implica ajustar un combinación lineal de los _inputs_ a una función no lineal (la función de enlace) de los _outputs_.

 
 - _¿Cuándo se usa?_ Cuando las variables de respuesta tienen un comportamiento de distribución no normal, como una variable de respuesta que se espera que sea siempre positiva.
 

<br>

<div class="info">
 **Regression Tree**
</div>
  

 - _¿Cómo trabaja?_  Los árboles de decisión para la regresión son similares a los árboles de decisión para
clasificación, pero se modifican para poder predecir respuestas continuas.
 
 - _¿Cuándo se usa?_ Cuando los predictores son categóricos (discretos) o se comportan
no lineal.

 
<br>

<div class="info">
 **Gaussian Process Regression Model**
</div>
  

 - _¿Cómo trabaja?_  Los modelos de regresión de procesos gaussianos (GPR) son modelos no paramétricos que se utilizan para predecir el valor de una variable de respuesta continua. Son ampliamente utilizados en
el campo del análisis espacial para la interpolación en presencia de incertidumbre. GPR también se conoce como _Kriging_.
 
 - _¿Cuándo se usa?_ Para la interpolación de datos espaciales.
 
 


## Técnicas de Reducción de Dimensión 

<div class="info">
 **Análisis de Componentes Principales (PCA)**
</div>
  

 - _¿Cómo trabaja?_  Realiza una transformación lineal en los datos de forma que la mayor varianza o información en el conjunto de datos de alta dimensión es capturada por las primeras (pocas) componentes principales. La primera componente capturará la mayor varianza, seguida por la segunda componente principal, y así sucesivamente.
 
<!--  performs a linear -->
<!-- transformation on the data so that most of the variance or -->
<!-- information in your high-dimensional dataset is captured by the -->
<!-- first few principal components. The first principal component -->
<!-- will capture the most variance, followed by the second principal -->
<!-- component, and so on. -->
 
 
<br>

<div class="info">
 **Análisis Factorial**
</div>
  

 - _¿Cómo trabaja?_  Identifia las correlaciones subyacentes entre las variables del conjunto de datos para proporcionar una representación en términos de un número pequeño de factores comunes latentes o no observables.
 
 
<!-- analysis—identifies underlying correlations between -->
<!-- variables in your dataset to provide a representation in terms of a -->
<!-- smaller number of unobserved latent, or common, factors. -->


## Otras 'Técnicas'

<div class="info">
 **Minería de Textos**
</div>

<br>


<div class="info">
 **Video/Image Analytics**
</div>

<br>

<div class="info">
 **Speech Analytics**
</div>

<br>

<div class="info">
 **Stream Analytics**
</div>

<br>



## ¿Qué técnica utilizar?




\includegraphics[width=0.9\linewidth]{imgs/choosealgorithm} 




<!-- Choosing the right algorithm can seem overwhelming—there -->
<!-- are dozens of supervised and unsupervised machine learning -->
<!-- algorithms, and each takes a different approach to learning. -->
<!-- There is no best method or one size fits all. Finding the right -->
<!-- algorithm is partly just trial and error—even highly experienced -->
<!-- data scientists can’t tell whether an algorithm will work without -->
<!-- trying it out. But algorithm selection also depends on the size and -->
<!-- type of data you’re working with, the insights you want to get from -->
<!-- the data, and how those insights will be used. -->


Elegir el algoritmo adecuado puede parecer abrumador—hay docenas de algoritmos de _statistical learning_, y cada uno tiene una aproximación diferente. No existe un único mejor método o uno que sirva para todos los casos. Buscar el adecuado es a veces una tarea de prueba y error—incluso los científicos de datos con más experiencia no pueden saber si un algoritmo funcionará sin haberlo probado. La selección del algoritmo también depende del tamaño y del tipo de los datos con el que se está trabajando, los resultados que se quieren obtener y como dichos resultados serán usados. 


El primer paso, y uno de los más importantes, es definir el objetivo del análisis que se va a realizar. Volviendo al esquema propuesto por @mar2015 (presentado en \@ref(modelizar)), _Start with Strategy_ implica que, antes de empezar con los datos, se empiece con la definición de los objetivos de negocio que se quieren alcanzar. Inmediatamente después se definen los datos y las métricas que estarán involucradas. 

En este momento se conoce la naturaleza del problema y de la variable objetivo, por lo tanto se conoce si estamos delante de una variable continua o binaria, si se requiere de un modelo explicativo, si _sólo_ se requiere una segmentación, etc. 

En consecuencia, una vez conocida la decisión que se requiere tomar y la variable objetivo que se analizará, se puede elegir el enfoque y modelo especíco que se va a utilizar.

<div class="rmdcomment">
Considera utilizar un _modelo estadístico_ si se debe priorizar el poder explicativo, se dispone de tiempo computacional y memoria para ajustar modelos relativamente complejos. 
</div>

Algunas situaciones donde este enfoque es útil son:

 - Proceso de concesión de créditos, con modelos supervisados por la entidad reguladora.
 - Asignación de presupuesto anual de marketing
 - Determinación de metas de venta por distribuidor, centro, etc.

<div class="rmdcomment">
Considera usar el _machine learning_ cuando se tenga una tarea compleja o un problema que involucre una gran cantidad de datos o variables, pero no exista fórmula o ecuación. 
</div>

Por ejemplo, _machine learning_ es una buena opción si se requieren manejar situaciones como:
 
  - Las reglas y ecuaciones de escritura a mano son muy complejas—como reconocimiento facial o de voz.
  - Las reglas de las tareas están cambiando constantemente—como en detección de fraude desde los registros de transacciones.
  - La naturaleza de los datos es cambiante y el programa necesita adaptarse—como en el trading automático, previsión de demanda de energía y predicción de tendencias en compras.


Algunas consideraciones al elegir un algoritmo son^[Fuente: https://docs.microsoft.com/es-es/azure/machine-learning/studio/algorithm-choice]:


 > Precisión

No siempre es necesario obtener la respuesta más precisa posible. A veces, una aproximación ya es útil, según para qué se la desee usar. Si es así, puede reducir el tiempo de procesamiento de forma considerable al usar métodos más aproximados. Otra ventaja de los métodos más aproximados es que tienden naturalmente a evitar el sobreajuste.

 > Tiempo (de entrenamiento)

La cantidad de minutos u horas necesarios para modelizar varía mucho según el algoritmo. A menudo, el tiempo  depende de la precisión (generalmente, uno determina al otro). Además, algunos algoritmos son más sensibles a la cantidad de datos que otros. Si el tiempo es limitado, esto puede determinar la elección del algoritmo, especialmente cuando el conjunto de datos es grande.


 > Cantidad de parámetros
 
Los parámetros son los _botones_ que el analista activa al configurar un algoritmo. Son números que afectan al comportamiento del algoritmo, como la tolerancia a errores o la cantidad de iteraciones, o bien opciones de variantes de comportamiento del algoritmo. El tiempo de entrenamiento y la precisión del algoritmo a veces pueden ser muy sensibles y requerir solo la configuración correcta. Normalmente, los algoritmos con muchos parámetrosla mayor cantidad de pruebas para encontrar una buena combinación. La ventaja es que tener muchos parámetros normalmente indica que un algoritmo tiene mayor flexibilidad. Se puede lograr una precisión muy alta, siempre y cuando se encuentre la combinación correcta de configuraciones de parámetros.

 > Cantidad de variables

Para ciertos tipos de datos, la cantidad de variables o características puede ser muy grande en comparación con la cantidad de datos. Este suele ser el caso de la genética o los datos textuales. Una gran cantidad de características puede trabar algunos algoritmos  y provocar que el tiempo de procesamiento sea demasiado largo.

 > Linealidad

Muchos algoritmos hacen uso de la linealidad. Los algoritmos de clasificación lineal suponen que las clases pueden estar separadas mediante una línea recta (o su análogo de mayores dimensiones). Entre ellos, se encuentran la regresión logística y las máquinas de vectores de soporte (svm). Los algoritmos de regresión lineal suponen que las tendencias de datos siguen una línea recta. Estas suposiciones no son incorrectas para algunos problemas, pero en otros disminuyen la precisión.

 > Casos especiales
 
Algunos algoritmos hacen determinadas suposiciones sobre la estructura de los datos o los resultados deseados. Si encuentra uno que se ajuste a sus necesidades, este puede ofrecerle resultados más útiles, predicciones más precisas o tiempos de procesamiento más cortos.




<!-- ## ¿Cuando deberías usar el machine learning? -->


<!-- Consider using machine learning when you have a complex task or -->
<!-- problem involving a large amount of data and lots of variables, but -->
<!-- no existing formula or equation. For example, machine learning is a -->
<!-- good option if you need to handle situations like these: -->

<!--  - Hand-written rules and equations are too complex—as in face recognition and speech recognition. -->

<!--  - The rules of a task are constantly changing—as in fraud detection from transaction records. -->

<!--  - The nature of the data keeps changing, and the program needs to adapt—as in automated trading, energy demand forecasting, and predicting shopping trends. -->



  
 
 

<!--chapter:end:01b-Modelos.Rmd-->

# Evaluación de modelos








\includegraphics[width=0.9\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/modellingcycle} 

El ciclo de vida de un modelo empieza con su propia definición, pasando por la extracción y tratamiento de los datos y la evaluación, tanto antes de ponerlo en producción, como en la monitorización de su calidad predictiva.


<!-- El ciclo de vida de un modelo en el entorno empresarial trae un concepto fundamental: **Gobernanza**. -->

<!-- Gobernanza es el conjunto de procedimientos y herramientas que minimizan errores y facilitan el -->
<!-- control de cada fase del ciclo, y la coexistencia pacífica de diferentes modelos en el ecosistema. -->

La diagnosis o evaluación es la clave para lograr un ecosistema de modelos que impacte en la organización. Hay muchas métricas para evaluar como de bien o de mal funciona un modelo o algoritmo. Para determinar cuáles usar en un problema particular, necesitamos formas sistemáticas de evaluar cómo funcionan los diferentes métodos y comparar uno con otro. La evaluación no es tan simple como podría parecer a primera vista.


<div class="rmdcomment">
La diagnosis de los modelos puede  realizarse desde dos perspectivas: 

  - **Negocio** y 
  - **Estadística**. 

Ambas pueden ser utilizadas para monitorizar la calidad de los modelos en producción. La frecuencia de análisis depende del tipo de modelo.
</div>


 * **Diagnosis de Negocio**: Se refiere a la utilización de métricas que indican si se cumplen las hipótesis sobre las cuales se ha construido el modelo, además de evaluar su calidad predictiva. Ejemplo de estas métricas son: R2, MAPE, AUC, LIFT, etc
 
 * **Diagnosis Estadística**: Se refiere a la discusión del significado de los resultados, teniendo en cuenta el sentido del negocio. Elementos susceptibles de esta interpretación son: parámetros, análisis decom, due-to, etc.



## Diagnosis de Negocio


\includegraphics[width=0.9\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/evaluanegocio} 

 * Los **parámetros** de los modelos estadisticos sirven para cuantificar el efecto de las palancas. Su interpretación depende de la propia especificación del modelo. Los principales tipos de parámetros son:  elasticidad, semi-elasticidad, _piecewise_, _yes/no_. Si el output es 0-1, la interpretación de los parámetros depende de la función enlace utilizada (_logit_ o _probit_).

 
 * Análisis de **Descomposición**. Mide el efecto de cada _input_ o _driver_ sobre el _output_ de un periodo

 * Análisis de **due-to**. Compara el efecto de los _inputs_ o _drivers_ en  el _output_ entre dos periodos



## Evaluación en Respuesta Binaria




\includegraphics[width=0.9\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/evaluabinario} 


No todos los problemas son iguales, con lo que no todos los problemas pueden usar las mismas métricas de evaluación. En esta sección veremos las métricas más usuales para los tipos de problemas que nos podemos encontrar. Si nos centramos en modelos supervisados, nos encontramos básicamente dos problemas distintos: clasificación y regresión.



### Clasificación

En los problemas de clasificación tenemos la variable objetivo que son las clases o etiquetas que debemos predecir y una serie de variables que son los predictores. Es decir, usando los predictores obtenemos una etiqueta. Nos podemos encontrar con problemas de clasificación binaria (dos clases) o múltiple (más de dos clases).

Para simplificar nos centraremos en la clasificación binaria, pero lo podemos trasladar a los problemas de clasificación múltiple. 


#### Confusion matrix

La confusion matrix o matriz de confusión muestra el número de predicciones correctas e incorrectas hechas por el modelo de clasificación en comparación con los resultados reales en los datos. La matriz de confusión es una matriz $n \times n$, dónde $n$ es el número de clases. La siguiente tabla muestra una matriz de confusión de $2x2$ para dos clases (positiva y negativa).



\includegraphics[width=0.8\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/confusionmatrix} 

* **Accuracy**: la proporción del número total de predicciones correctas.

$$ACC = \frac{TP+TN}{TP+TN+FP+FN}$$

* **Positive Predictive Value or Precision**: la proporción de casos positivos que fueron identificados correctamente.

$$PPV = \frac{TP}{TP+FP}$$

* **Negative Predictive Value**: la proporción de casos negativos que fueron identificados correctamente.

$$ 
NPV = \frac{TN}{TN+FN}
$$ 


* **Sensitivity or Recall**: la proporción de casos positivos reales que están correctamente identificados.

$$TPR = \frac{TP}{TP+FN}$$

* **Specificity**: la proporción de casos negativos reales que están correctamente identificados.

$$TNR = \frac{TN}{TN+FP}$$


#### Log-Loss

La log-loss o pérdida logarítmica entra en los detalles más finos de un clasificador. En particular, si la salida bruta del clasificador es una probabilidad numérica en lugar de una etiqueta de clase de $0$ o $1$, se puede usar la log-loss. La probabilidad se puede entender como un indicador de confianza. Si la etiqueta es $0$ pero el clasificador cree que pertenece a la clase $1$ con probabilidad de $0,51$. Aunque el clasificador estaría cometiendo un error de clasificación, el error se comente por poco, ya que la probabilidad está muy cerca del punto de corte de $0.5$. La log-loss es una medición de precisión que incorpora esta idea de confianza probabilística.

La log-loss para un clasificador binario es

$$LogLoss = - \frac{1}{n} \sum_{i=1}^{n} y_i \log p_i + (1-y_i) \log (1-p_i)$$

donde $n$ es el número de registros, $y_i$ es la etiqueta de la muestra $i$, y $p_i$ es la probabilidad del obtenida en el modelo.


#### Curvas ROC

Para los modelos de clasificación obtenidos a partir de una probabilidad se suelen usar las curvas ROC. Una curva ROC (acrónimo de Receiver Operating Characteristic, o Característica Operativa del Receptor) es una representación gráfica de la sensitivity (TPR) frente a la specificity (TNR) para un sistema clasificador binario según se varía el umbral de discriminación. 

La curva ROC se puede usar para generar estadísticos que resumen el rendimiento (o la efectividad, en su más amplio sentido) del clasificador. A continuación se proporcionan algunos:

 * El punto de inserción de la curva ROC con la línea convexa a la línea de discriminación.
 * El área entre la curva ROC y la línea de convexo-paralela discriminación.
 * El área bajo la curva ROC, llamada comúnmente AUC (area under curve).

El indicador más utilizado en muchos contextos es el área bajo la curva ROC o AUC. Este índice se puede interpretar como la probabilidad de que un clasificador ordenará o puntuará una instancia positiva elegida aleatoriamente más alta que una negativa.

En la figura abajo se muestran tres ejemplos de curvas ROC. La gráfica de la izquierda es la curva de un modelo perfecto, la del medio es la de un caso real con una $AUC = 0.8$ y la de la derecha es la gráfico de un modelo no informativo.



\includegraphics[width=0.8\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/curvasroc} 

#### Gráficos de ganancia y elevación (Gain and Lift Charts)

La ganancia o la elevación es una medida de la efectividad de un modelo de clasificación calculado como la relación entre los resultados obtenidos con y sin el modelo. Los gráficos de ganancia y elevación son ayudas visuales para evaluar el rendimiento de los modelos de clasificación. Sin embargo, en contraste con la matriz de confusión que evalúa los modelos en toda la población, los gráficos de ganancia o elevación evalúan el modelo en una porción de la población.

Para crear estos gráficos es necesario crear un ranking basado en la creabilidad de la predicción hecha por el modelo.

En la figura  tenemos un ejemplo de como obtener los puntos de las curvas de ganancia y elevación, y sus correspondientes gráficos. 


\includegraphics[width=0.8\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/gainlift} 

Igual que las curvas ROC, se busca el mayor AUC en las curvas de ganancia. Mientras que para los gráficos de elevación el modelo perfecto es el que la diferencia entre la línea azul y roja es nula. En otras palabras queremos una AUC mínima del gráfico de elevación.


### Medidas de desigualdad


#### El coeficiente de Gini

El coeficiente de Gini es una medida de la desigualdad ideada por el estadístico italiano Corrado Gini. El coeficiente de Gini es un número entre $0$ y $1$, en donde $0$ se corresponde con la perfecta igualdad y donde el valor $1$ se corresponde con la perfecta desigualdad.

El coeficiente de Gini se calcula como una proporción de las áreas en el diagrama de la curva de Lorenz. De forma resumida, la Curva de Lorenz es una gráfica de concentración acumulada de la distribución superpuesta a la curva de la distribución de frecuencias de los individuos, y su expresión en porcentajes es el índice de Gini. El coeficiente de Gini puede obtener mediante la siguiente fórmula:

$$G = \left| 1-\sum_{k=1}^{n-1} (X_{k+1}-X_k)(Y_{k+1}+Y_k) \right|$$

donde $X$ es la proporción acumulada de la variable población, $Y$ es la proporción acumulada de la variable a estudiar la desigualdad y $n$ es el número de la población.


#### Índice de entropía

El índice de entropía generalizado se ha propuesto como una medida de la desigualdad en una población. Se deriva de la teoría de la información como una medida de redundancia en los datos. En la teoría de la información, una medida de redundancia puede interpretarse como no aleatoriedad o compresión de datos; por lo tanto, esta interpretación también se aplica a este índice.

La fórmula de la entropía general para un valor real $\alpha$ es:

$$GE(\alpha) = \begin{cases}
       \frac{1}{n\alpha (\alpha -1)} \sum_{i=1}^{n} \left(\left( \frac{y_i}{\bar{y}} \right) ^{\alpha} -1\right), &\quad \alpha\neq0,1 ,\\
       \frac{1}{n} \sum_{i=1}^{n} \frac{y_i}{\bar{y}} \ln \frac{y_i}{\bar{y}} &\quad \alpha = 1 ,\\
       -\frac{1}{n} \sum_{i=1}^{n} \ln \frac{y_i}{\bar{y}} &\quad \alpha = 0 .\\
     \end{cases}$$

donde $n$ el número de muestras y $y$ es la medida de desigualdad.


## Evaluación en Respuesta Continúa


\includegraphics[width=0.9\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/evaluacontinuo} 


### Modelos de Regresión

En los problemas de regresión siempre tenemos una variable numérica dependiente que es la que queremos predecir y el resto son los predictores. Para evaluar los modelos de regresión tenemos varias métricas para evaluar el error cometido en al predicción:




* **RMSE** (root mean squared error) o error cuadrado medio: RMSE es la métrica más popular para medir la tasa de error de un modelo de regresión.

$$RMSE = \sqrt {\frac{\sum_{i=1}^{n} (\hat{y}_i - y_i)^2}{n}}$$

donde $n$ es el número de muestras, $\hat{y}_i$ el valor predicho de la variable objetivo y $y_i$ el valor real de la variable objetivo. 

* **MAE** (mean abosulte error) o error absoluto medio: 

$$MAE = \frac{\sum_{i=1}^{n} | \hat{y}_i - y_i |}{n}$$

donde $n$ es el número de muestras, $\hat{y}_i$ el valor predicho de la variable objetivo y $y_i$ el valor real de la variable objetivo.

* **RSE** (relative squared error) o error relativo cuadrado: 

$$RSE = \sqrt \frac{\sum_{i=1}^{n} (\hat{y}_i - y_i)^2}{\sum_{i=1}^{n} (\bar{y} - y_i)^2}$$

donde $n$ es el número de muestras, $\bar{y}$ es la media de la variable objetivo, $\hat{y}_i$ el valor predicho de la variable objetivo y $y_i$ el valor real de la variable objetivo.

* **RAE** (relative absolute error) o error relativo absoluto: 

$$RAE = \frac{\sum_{i=1}^{n} |\hat{y}_i - y_i|}{\sum_{i=1}^{n} |\bar{y} - y_i|}$$

donde $n$ es el número de muestras, $\bar{y}$ es la media de la variable objetivo, $\hat{y}_i$ el valor predicho de la variable objetivo y $y_i$ el valor real de la variable objetivo.

* **Coeficiente $R^2$**: $R^2$ resume el poder explicativo del modelo de regresión y se calcula a partir de los términos de las sumas de cuadrados. El coeficiente $R^2$ toma valores entre $0$ y $1$, si $R^2=1$ la regresión es perfecta.

$$R^2 = \frac {SSR}{SST} = 1 - \frac{SSE}{SST}, $$

donde $$SST = \sum_{i=1}^{n} (y - \bar{y})^2 ,$$

$$SSR = \sum_{i=1}^{n} (\hat{y} - \bar{\hat{y}})^2 ,$$ 

$$SSE = \sum_{i=1}^{n} (y-\hat{y})^2 .$$


### Modelos de Series temporales

Las series temporales son básicamente un problema de regresión. La diferencia es que hay una variable temporal y el objetivo es predecir el futuro dado un histórico. Por lo tanto, las métricas utilizadas son las mismas que las usadas para los problemas de regresión vistas en la sección anterior.

Otras métricas usadas frecuentemente para la evaluación de series temporales son:

<div class = info>
**MAPE**
</div>

_MAPE_ viene de _Mean Absolute Percentage Error_. Los errores porcentuales tienen la ventaja de ser independientes de la escala y, por lo tanto, se utilizan con frecuencia para comparar el rendimiento del pronóstico entre diferentes conjuntos de datos. MAPE es el más usual.

$$MAPE = \frac{1}{n} \sum_{i=1}^{n} \frac{100·|\hat{y_i}-y_i|}{y_i}$$

<div class = info>
**AIC**
</div>

_AIC_ viene de _Akaike information criterion_. Se define como 

$$AIC = 2k-2\ln (\hat{L})$$

Dado un conjunto de modelos candidatos para los datos, el modelo preferido es el que tiene el valor mínimo en el AIC. Por lo tanto AIC no sólo recompensa la bondad de ajuste, sino también incluye una penalidad, que es una función creciente del número de parámetros estimados.


<div class = info>
**BIC**
</div>

_BIC_**_ viene de _Bayesian Information Criterion_)_. Se define como

$$BIC = \ln (n) k - 2 \ln (\hat{L})$$

donde $\hat{L}$ es máximo de la función de verosimilitud, $n$ es el número de muestras, $k$ es el número de parámetros estimados por el modelo.

La fórmula del BIC es similar a la fórmula del AIC, pero con una penalización distinta que varia según el número de muestras de los datos.


## Evaluación en _Clustering_


El _Clustering_ es una forma de tratar los datos para los que no se conocen o no están definidos los grupos. Por tanto, tenemos que **conceptualizar** los grupos. Este hecho dificultad evaluar la calidad de los clasificación obtenida.


### Silueta

El coeficiente silueta proporciona una representación gráfica del grado de integración de un objeto en su cluster. El coeficiente silueta de un objeto $i$  se define como:

$$s_i=\dfrac{b_i -a_i}{max(b_i -a_i)}$$

donde $a_i$ denota la distancia media entre el objeto $i$  y todos los otros objetos de su cluster y $b_i$ denota la distancia media mínima entre $i$  y los objetos de otros clusters.
Los objetos con un coeficiente de silueta $s_i$ alto están bien integrados en su cluster; aquéllos con un si bajo tienden a estar entre clusters.




\includegraphics[width=0.9\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/silueta} 

## Métodos de re-muestreo 


\includegraphics[width=0.8\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/traintess} 


### Training & testing

Lo primero que debemos hacer para conseguir una buena evaluación es dividir los datos en dos subconjuntos. Uno para entrenar el modelo (training) y otro para evaluar el modelo (testing). El partición entre estos dos subconjuntos suele hacerse de forma aleatoria, aunque según el problema podemos usar otros criterios. Por ejemplo, si los datos que tenemos son una serie temporal, entonces podemos dividirlos a partir de un cierto tiempo. Es decir, coger como test los datos más recientes.

La razón de hacer esta división es usar los datos del subconjunto training para entrenar el modelo y luego evaluar los datos del testing. De esta manera simulamos correctamente una evaluación, ya que no podemos evaluar unos datos si hemos entrenado con ellos. Por lo tanto, los datos de testing no deben ser observados por el algoritmo.


### Cross validation

El procedimiento que se suele usar para evaluar un modelo es cross validation o validación cruzada. La idea básica de cross validation consiste en dividir los datos en $k$ subconjuntos. Cada subconjunto se predice mediante un modelo entrenado con el resto. De esta manera podemos hacer una evaluación sobre todos los datos y evitamos el problema de obtener una muestra sesgada si sólo lo hiciéramos una vez.  



<!-- ============================================ -->


## Práctica en R


<div class="info">
Evaluaremos la calidad predictiva de dos modelos:

 * Cuando la variable respuesta es binaria.
 * Cuando la variable respuesta es contínua.

</div>


### Preparación de los datos


 > Definimos el Entorno de Trabajo 


El primer paso es crear una carpeta con nuestros modelos y resultados dentro de nuestro espacio de trabajo (proyecto).

 - Obtenemos la ruta completa del directorio de trabajo^[Si queremos cambiar la ruta, podemos hacer 'myWd <- setwd("Ruta y Nombre de la carpeta")'.].

```r
myWD <- getwd() 
```

 - Elegimos un nombre para nuestra carpeta con resultados

```r
myWorkingFolderName <- 'ModelResults' 
```

 - Creamos la carpeta donde guardaremos nuestros resultados y ficheros

```r
dir.create( paste0(getwd(),"/",myWorkingFolderName))
```

 
 > Accedemos a los datos originales
 
 - Cargamos la librería `insuranceData` que contiene los datos que utilizaremos^[https://CRAN.R-project.org/package=insuranceData]

```r
if (!require(insuranceData)) install.packages('insuranceData')
library(insuranceData)
```

 - Para ver los contenidos de la librería `insuranceData` ejecutamos:

```r
data(package='insuranceData')
```

 - Vemos que hay 10 datasets. Trabajaremos con el primero: `AutoBi` (_Automobile Bodily Injury Claims_^[https://www.rdocumentation.org/packages/insuranceData/versions/1.0/topics/AutoBi]).
 
 - Cargamos el conjunto de datos seleccionado: **pérdidas en accidentes de coches**

```r
data("AutoBi")
```

- Descripción de las 8 variables del conjunto de datos (_tabla_) 'AutoBi': 
  
  1. `Casenum`. Identificador de la reclamación (esta variable no se utiliza en los modelos)
  1. `Attorney`.  Indica si el reclamante está representado por un **abogado** (1= Sí, 2 = No)
  1. `Clmsex`.  **Sexo** del reclamante (1 = Hombre, 2 = Mujer)
  1. `Marital`.  **Estado Civil** del reclamante (1 = Casado, 2 = Soltero, 3 = Viudo, 4 = divorciado/separado)
  1. `Clminsur`.  Indica si el conductor del vehículo del reclamante estaba o no **asegurado** (1 = Si, 2 = No, 3 = No aplica)
  1. `Seatbelt`.  Si el reclamante llevaba o no un **cinturón** de seguridad en el asiento infantil (1 = Si, 2 = No, 3 = No Aplica)
  1. `Clmage`.  **Edad** del reclamante.
  1. `Loss (*)`.  La **pérdida económica** total del reclamante (en miles). Esta es la variable objetivo o dependiente del conjunto de datos.
  
 - Revisamos el contenido de la tabla y el tipo de datos que contiene

```r
str(AutoBi)
```

```
FALSE 'data.frame':	1340 obs. of  8 variables:
FALSE  $ CASENUM : int  5 13 66 71 96 97 120 136 152 155 ...
FALSE  $ ATTORNEY: int  1 2 2 1 2 1 1 1 2 2 ...
FALSE  $ CLMSEX  : int  1 2 1 1 1 2 1 2 2 1 ...
FALSE  $ MARITAL : int  NA 2 2 1 4 1 2 2 2 2 ...
FALSE  $ CLMINSUR: int  2 1 2 2 2 2 2 2 2 2 ...
FALSE  $ SEATBELT: int  1 1 1 2 1 1 1 1 1 1 ...
FALSE  $ CLMAGE  : int  50 28 5 32 30 35 19 34 61 NA ...
FALSE  $ LOSS    : num  34.94 10.892 0.33 11.037 0.138 ...
```

 - Exploramos el contenido con estadísticos descriptivos básicos

```r
summary(AutoBi)
```

```
FALSE     CASENUM         ATTORNEY         CLMSEX         MARITAL     
FALSE  Min.   :    5   Min.   :1.000   Min.   :1.000   Min.   :1.000  
FALSE  1st Qu.: 8579   1st Qu.:1.000   1st Qu.:1.000   1st Qu.:1.000  
FALSE  Median :17453   Median :1.000   Median :2.000   Median :2.000  
FALSE  Mean   :17213   Mean   :1.489   Mean   :1.559   Mean   :1.593  
FALSE  3rd Qu.:25703   3rd Qu.:2.000   3rd Qu.:2.000   3rd Qu.:2.000  
FALSE  Max.   :34253   Max.   :2.000   Max.   :2.000   Max.   :4.000  
FALSE                                  NA's   :12      NA's   :16     
FALSE     CLMINSUR        SEATBELT         CLMAGE           LOSS         
FALSE  Min.   :1.000   Min.   :1.000   Min.   : 0.00   Min.   :   0.005  
FALSE  1st Qu.:2.000   1st Qu.:1.000   1st Qu.:19.00   1st Qu.:   0.640  
FALSE  Median :2.000   Median :1.000   Median :31.00   Median :   2.331  
FALSE  Mean   :1.908   Mean   :1.017   Mean   :32.53   Mean   :   5.954  
FALSE  3rd Qu.:2.000   3rd Qu.:1.000   3rd Qu.:43.00   3rd Qu.:   3.995  
FALSE  Max.   :2.000   Max.   :2.000   Max.   :95.00   Max.   :1067.697  
FALSE  NA's   :41      NA's   :48      NA's   :189
```

 - Para llamar directamente a las variables por sus nombres en la tabla AutoBi utilizamos el comando `attach`

```r
attach(AutoBi)
```

 > Exploramos la variable objetivo


<div class="info">
 LOSS es la **variable objetivo** una variable altamente asimétrica (con posibles outliers a la derecha o pérdida muy severa)^[A loss is the injury or damage sustained by the insured in consequence of the happening of one or more of the accidents or misfortunes against which the insurer, in consideration of the premium, has undertaken to indemnify the insured.]. 
</div>

 - Analizamos la variable target


```r
summary(LOSS)
```

```
FALSE     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
FALSE    0.005    0.640    2.331    5.954    3.995 1067.697
```

 - Analizamos la distribución de la variable target


```r
hist(LOSS, breaks=300 , probability = T)
lines(density(LOSS), col="red",main="Loss distribution")
```

![](02-Evaluation_files/figure-latex/unnamed-chunk-20-1.pdf)<!-- --> 

 - Utilizamos una medida _robusta_ (depende de la mediana y del IQR^[The interquartile range of an observation variable is the difference of its upper and lower quartiles. It is a measure of how far apart the middle portion of data spreads in value]) para segmentar los datos en dos clases: 
  * `1` si las pérdidas son atípicamente altas o 
  * `0` si no lo son.

```r
lsup <- median(LOSS) + 1.5*IQR(LOSS) # Criterio basado en estadisticos robustos
sum(LOSS>=lsup) # 153 datos de perdidas atipicamente altas
```

```
FALSE [1] 153
```

 - (Opcional) Guardamos el gráfico del histograma de las **pérdidas no severas**

```r
  Path_to_graphics <- paste0(getwd(),"/","Graphics")
  dir.create(Path_to_graphics)
  png(paste0(Path_to_graphics,"/histograma.png"))
  hist(LOSS[LOSS<lsup], breaks = 100, probability = T, xlab="loss (pérdida en miles US $)", main="Pérdida no severa")
  lines(density(LOSS[LOSS<lsup]),col="red")
  dev.off()
```

```
FALSE pdf 
FALSE   2
```


 > Creamos el _dataset_ de trabajo.

 - Creamos un dataset o tabla de trabajo eliminando la variable CASENUM (id) y filtrando por la variable LOSS y el valor lsup= 72.22587 (miles).

```r
df_autobi <- AutoBi[ , -match("CASENUM", colnames(AutoBi)) ] 
```

 - Fijamos los predictores categóricos como factores:

   * Representado por un abogado: '1' = representado por letrado y '2' = no representado

```r
  df_autobi$ATTORNEY <- ordered(df_autobi$ATTORNEY, levels = 1:2) 
```

   * Sexo: '1' = hombre y '2' = mujer

```r
  df_autobi$CLMSEX   <- ordered(df_autobi$CLMSEX  , levels = 1:2)
```

   * Estado civil: '1' = casado, '2' = soltero, '3' = viudo y '4' = divorciado / separado


```r
  df_autobi$MARITAL  <- ordered(df_autobi$MARITAL , levels = 1:4)
```

   * Vehículo asegurado:  '1' = vehículo estaba asegurado y '2'= no lo estaba

```r
  df_autobi$CLMINSUR <- ordered(df_autobi$CLMINSUR, levels = 1:2) 
```

   * Cinturón de seguridad: '1' = llevaba cinturón abrochado y '2' = no lo llevaba

```r
  df_autobi$SEATBELT <- ordered(df_autobi$SEATBELT, levels = 1:2)
```

   * Pérdida: '1'= pérdida severa y '2'= pérdida no severa


```r
df_autobi$Y        <- ifelse(df_autobi$LOSS>= lsup,1,0)
```

 - Exploramos el _dataset_ que acabamos de crear y verificamos la proporción de casos con pérdida severa (11.42%)

```r
summary(df_autobi)
```

```
FALSE  ATTORNEY  CLMSEX    MARITAL    CLMINSUR    SEATBELT        CLMAGE     
FALSE  1:685    1   :586   1   :624   1   : 120   1   :1270   Min.   : 0.00  
FALSE  2:655    2   :742   2   :650   2   :1179   2   :  22   1st Qu.:19.00  
FALSE           NA's: 12   3   : 15   NA's:  41   NA's:  48   Median :31.00  
FALSE                      4   : 35                           Mean   :32.53  
FALSE                      NA's: 16                           3rd Qu.:43.00  
FALSE                                                         Max.   :95.00  
FALSE                                                         NA's   :189    
FALSE       LOSS                Y         
FALSE  Min.   :   0.005   Min.   :0.0000  
FALSE  1st Qu.:   0.640   1st Qu.:0.0000  
FALSE  Median :   2.331   Median :0.0000  
FALSE  Mean   :   5.954   Mean   :0.1142  
FALSE  3rd Qu.:   3.995   3rd Qu.:0.0000  
FALSE  Max.   :1067.697   Max.   :1.0000  
FALSE 
```

 - Exploramos la relación de la pérdida con los factores.


```r
agg_loss_attorney <- aggregate(LOSS, by = list(ATTORNEY) , FUN= mean , na.rm=TRUE)
  dimnames(agg_loss_attorney)[[1]] <- c("REPRESENTED","NOT REPRESENTED") ; dimnames(agg_loss_attorney)[[2]] <- c("ATTORNEY","LOSS")
  
  agg_loss_clmsex   <- aggregate(LOSS, by = list(CLMSEX)  , FUN= mean , na.rm=TRUE)
  dimnames(agg_loss_clmsex)[[1]]   <- c("MALE","FEMALE")  ; dimnames(agg_loss_clmsex)[[2]] <- c("CLMSEX","LOSS")
  
  agg_loss_marital  <- aggregate(LOSS, by = list(MARITAL) , FUN= mean , na.rm=TRUE)
  dimnames(agg_loss_marital)[[1]]  <- c("MARRIED","SINGLE","WIDOW","DIVORCED") ; dimnames(agg_loss_marital)[[2]] <- c("MARITAL","LOSS")
  
  agg_loss_clminsur <- aggregate(LOSS, by = list(CLMINSUR) , FUN= mean , na.rm=TRUE)
  dimnames(agg_loss_clminsur)[[1]] <- c("INSURED","NOT INSURED") ; dimnames(agg_loss_clminsur)[[2]] <- c("CLMINSUR","LOSS")
  
  agg_loss_seatbelt <- aggregate(LOSS, by = list(SEATBELT) , FUN= mean , na.rm=TRUE)
  dimnames(agg_loss_seatbelt)[[1]] <- c("SEATBELT","NOT SEATBELT") ; dimnames(agg_loss_seatbelt)[[2]] <- c("SEATBELT","LOSS")
```


 > Creamos los sets _train_ y _test_

 - Aleatorizamos los datos y separamos el set de datos en _train_ y _test_:

```r
N=nrow(df_autobi)
```

- Es recomendable fijar una semilla (seed) para los algoritmos de aleatorización internos de R

```r
if (!require(caret)) install.packages('caret')
library(caret)  
set.seed(123456)
inTrain  <- createDataPartition(df_autobi$Y, times = 1, p = 0.7, list = TRUE)
dt_train <- df_autobi[inTrain[[1]],]  # 938 casos
dt_test  <- df_autobi[-inTrain[[1]],] # 402 casos
  
nrow(dt_train)
```

```
FALSE [1] 938
```

```r
summary(dt_train)
```

```
FALSE  ATTORNEY  CLMSEX    MARITAL    CLMINSUR   SEATBELT       CLMAGE     
FALSE  1:471    1   :406   1   :439   1   : 77   1   :885   Min.   : 0.00  
FALSE  2:467    2   :523   2   :455   2   :833   2   : 17   1st Qu.:20.00  
FALSE           NA's:  9   3   : 10   NA's: 28   NA's: 36   Median :32.00  
FALSE                      4   : 25                         Mean   :33.06  
FALSE                      NA's:  9                         3rd Qu.:43.00  
FALSE                                                       Max.   :95.00  
FALSE                                                       NA's   :134    
FALSE       LOSS                Y         
FALSE  Min.   :  0.0050   Min.   :0.0000  
FALSE  1st Qu.:  0.7123   1st Qu.:0.0000  
FALSE  Median :  2.3645   Median :0.0000  
FALSE  Mean   :  5.4656   Mean   :0.1141  
FALSE  3rd Qu.:  4.0263   3rd Qu.:0.0000  
FALSE  Max.   :273.6040   Max.   :1.0000  
FALSE 
```

```r
nrow(dt_test)
```

```
FALSE [1] 402
```

```r
summary(dt_test)
```

```
FALSE  ATTORNEY  CLMSEX    MARITAL    CLMINSUR   SEATBELT       CLMAGE     
FALSE  1:214    1   :180   1   :185   1   : 43   1   :385   Min.   : 0.00  
FALSE  2:188    2   :219   2   :195   2   :346   2   :  5   1st Qu.:19.00  
FALSE           NA's:  3   3   :  5   NA's: 13   NA's: 12   Median :29.00  
FALSE                      4   : 10                         Mean   :31.31  
FALSE                      NA's:  7                         3rd Qu.:42.00  
FALSE                                                       Max.   :78.00  
FALSE                                                       NA's   :55     
FALSE       LOSS                 Y         
FALSE  Min.   :   0.0050   Min.   :0.0000  
FALSE  1st Qu.:   0.5175   1st Qu.:0.0000  
FALSE  Median :   2.1645   Median :0.0000  
FALSE  Mean   :   7.0917   Mean   :0.1144  
FALSE  3rd Qu.:   3.7782   3rd Qu.:0.0000  
FALSE  Max.   :1067.6970   Max.   :1.0000  
FALSE 
```

Comprobamos si se que los conjuntos train y test se han formado correctamente

```r
  length(intersect(inTrain, setdiff(1:N,inTrain)))
```

```
FALSE [1] 0
```


### Clasificación
 

<div class="info">
Vamos a construir un modelo para identificar los casos con pérdidas severas.
</div>
 
 - El primer ejemplo lo hacemos con _Random Forest_

```r
if (!require(randomForest)) install.packages('randomForest')
library(randomForest)
```

 - Creamos un objeto de clase 'formula' y se lo pasamos como argumento a la función `randomForest`^[https://www.rdocumentation.org/packages/randomForest/versions/4.6-12/topics/randomForest]

```r
set.seed(123456)
fmla.rf1 <- as.formula(paste0("Y"," ~",paste0(colnames(df_autobi[,-c(7,8)]),collapse = "+"),collapse = ""))
rf1 <- randomForest( fmla.rf1,
                       data =dt_train,
                       ntree = 5000, # se ejecuta muy rapido, podemos utilizar ntree > = 2500
                       replace =TRUE,
                       mtry=4,
                       maxnodes =50,
                       importance = TRUE,
                       proximity =   TRUE,
                       keep.forest = TRUE,
                       na.action=na.omit)
```

 -  Exploramos el objeto con los resutados

```r
rf1
```

```
FALSE 
FALSE Call:
FALSE  randomForest(formula = fmla.rf1, data = dt_train, ntree = 5000,      replace = TRUE, mtry = 4, maxnodes = 50, importance = TRUE,      proximity = TRUE, keep.forest = TRUE, na.action = na.omit) 
FALSE                Type of random forest: regression
FALSE                      Number of trees: 5000
FALSE No. of variables tried at each split: 4
FALSE 
FALSE           Mean of squared residuals: 0.1009875
FALSE                     % Var explained: 4.3
```

```r
summary(rf1)
```

```
FALSE                 Length Class  Mode     
FALSE call                11 -none- call     
FALSE type                 1 -none- character
FALSE predicted          759 -none- numeric  
FALSE mse               5000 -none- numeric  
FALSE rsq               5000 -none- numeric  
FALSE oob.times          759 -none- numeric  
FALSE importance          12 -none- numeric  
FALSE importanceSD         6 -none- numeric  
FALSE localImportance      0 -none- NULL     
FALSE proximity       576081 -none- numeric  
FALSE ntree                1 -none- numeric  
FALSE mtry                 1 -none- numeric  
FALSE forest              11 -none- list     
FALSE coefs                0 -none- NULL     
FALSE y                  759 -none- numeric  
FALSE test                 0 -none- NULL     
FALSE inbag                0 -none- NULL     
FALSE terms                3 terms  call     
FALSE na.action          179 omit   numeric
```

```r
str(rf1)
```

```
FALSE List of 19
FALSE  $ call           : language randomForest(formula = fmla.rf1, data = dt_train, ntree = 5000, replace = TRUE,      mtry = 4, maxnodes = 50, imp| __truncated__ ...
FALSE  $ type           : chr "regression"
FALSE  $ predicted      : Named num [1:759] 0.00767 0.000161 0.629748 0.008208 0.022047 ...
FALSE   ..- attr(*, "names")= chr [1:759] "2" "3" "4" "5" ...
FALSE   ..- attr(*, "na.action")= 'omit' Named int [1:179] 1 9 19 25 27 40 43 46 50 51 ...
FALSE   .. ..- attr(*, "names")= chr [1:179] "1" "10" "24" "30" ...
FALSE  $ mse            : num [1:5000] 0.102 0.129 0.121 0.115 0.113 ...
FALSE  $ rsq            : num [1:5000] 0.0302 -0.2272 -0.1473 -0.0907 -0.071 ...
FALSE  $ oob.times      : int [1:759] 1804 1942 1844 1864 1855 1780 1837 1782 1847 1807 ...
FALSE  $ importance     : num [1:6, 1:2] 0.017697 -0.001602 0.004106 0.000933 0.0009 ...
FALSE   ..- attr(*, "dimnames")=List of 2
FALSE   .. ..$ : chr [1:6] "ATTORNEY" "CLMSEX" "MARITAL" "CLMINSUR" ...
FALSE   .. ..$ : chr [1:2] "%IncMSE" "IncNodePurity"
FALSE  $ importanceSD   : Named num [1:6] 1.50e-04 8.42e-05 1.54e-04 8.18e-05 5.65e-05 ...
FALSE   ..- attr(*, "names")= chr [1:6] "ATTORNEY" "CLMSEX" "MARITAL" "CLMINSUR" ...
FALSE  $ localImportance: NULL
FALSE  $ proximity      : num [1:759, 1:759] 1 0.179 0 0.318 0 ...
FALSE   ..- attr(*, "dimnames")=List of 2
FALSE   .. ..$ : chr [1:759] "2" "3" "4" "5" ...
FALSE   .. ..$ : chr [1:759] "2" "3" "4" "5" ...
FALSE  $ ntree          : num 5000
FALSE  $ mtry           : num 4
FALSE  $ forest         :List of 11
FALSE   ..$ ndbigtree    : int [1:5000] 99 99 99 99 99 99 99 99 99 99 ...
FALSE   ..$ nodestatus   : int [1:99, 1:5000] -3 -3 -3 -3 -3 -3 -3 -3 -3 -3 ...
FALSE   ..$ leftDaughter : int [1:99, 1:5000] 2 4 6 8 10 12 14 16 18 20 ...
FALSE   ..$ rightDaughter: int [1:99, 1:5000] 3 5 7 9 11 13 15 17 19 21 ...
FALSE   ..$ nodepred     : num [1:99, 1:5000] 0.1265 0.2198 0.0363 0.0115 0.2832 ...
FALSE   ..$ bestvar      : int [1:99, 1:5000] 1 6 4 6 5 6 6 6 6 3 ...
FALSE   ..$ xbestsplit   : num [1:99, 1:5000] 1.5 20.5 1.5 15.5 1.5 27.5 34.5 0 16.5 3.5 ...
FALSE   ..$ ncat         : Named int [1:6] 1 1 1 1 1 1
FALSE   .. ..- attr(*, "names")= chr [1:6] "ATTORNEY" "CLMSEX" "MARITAL" "CLMINSUR" ...
FALSE   ..$ nrnodes      : int 99
FALSE   ..$ ntree        : num 5000
FALSE   ..$ xlevels      :List of 6
FALSE   .. ..$ ATTORNEY: num 0
FALSE   .. ..$ CLMSEX  : num 0
FALSE   .. ..$ MARITAL : num 0
FALSE   .. ..$ CLMINSUR: num 0
FALSE   .. ..$ SEATBELT: num 0
FALSE   .. ..$ CLMAGE  : num 0
FALSE  $ coefs          : NULL
FALSE  $ y              : Named num [1:759] 1 0 1 0 0 0 0 0 0 1 ...
FALSE   ..- attr(*, "na.action")= 'omit' Named int [1:179] 1 9 19 25 27 40 43 46 50 51 ...
FALSE   .. ..- attr(*, "names")= chr [1:179] "1" "10" "24" "30" ...
FALSE   ..- attr(*, "names")= chr [1:759] "2" "3" "4" "5" ...
FALSE  $ test           : NULL
FALSE  $ inbag          : NULL
FALSE  $ terms          :Classes 'terms', 'formula'  language Y ~ ATTORNEY + CLMSEX + MARITAL + CLMINSUR + SEATBELT + CLMAGE
FALSE   .. ..- attr(*, "variables")= language list(Y, ATTORNEY, CLMSEX, MARITAL, CLMINSUR, SEATBELT, CLMAGE)
FALSE   .. ..- attr(*, "factors")= int [1:7, 1:6] 0 1 0 0 0 0 0 0 0 1 ...
FALSE   .. .. ..- attr(*, "dimnames")=List of 2
FALSE   .. .. .. ..$ : chr [1:7] "Y" "ATTORNEY" "CLMSEX" "MARITAL" ...
FALSE   .. .. .. ..$ : chr [1:6] "ATTORNEY" "CLMSEX" "MARITAL" "CLMINSUR" ...
FALSE   .. ..- attr(*, "term.labels")= chr [1:6] "ATTORNEY" "CLMSEX" "MARITAL" "CLMINSUR" ...
FALSE   .. ..- attr(*, "order")= int [1:6] 1 1 1 1 1 1
FALSE   .. ..- attr(*, "intercept")= num 0
FALSE   .. ..- attr(*, "response")= int 1
FALSE   .. ..- attr(*, ".Environment")=<environment: R_GlobalEnv> 
FALSE   .. ..- attr(*, "predvars")= language list(Y, ATTORNEY, CLMSEX, MARITAL, CLMINSUR, SEATBELT, CLMAGE)
FALSE   .. ..- attr(*, "dataClasses")= Named chr [1:7] "numeric" "ordered" "ordered" "ordered" ...
FALSE   .. .. ..- attr(*, "names")= chr [1:7] "Y" "ATTORNEY" "CLMSEX" "MARITAL" ...
FALSE  $ na.action      : 'omit' Named int [1:179] 1 9 19 25 27 40 43 46 50 51 ...
FALSE   ..- attr(*, "names")= chr [1:179] "1" "10" "24" "30" ...
FALSE  - attr(*, "class")= chr [1:2] "randomForest.formula" "randomForest"
```


 > Gráfico de la importancia relativa de los predictores


```r
  varImpPlot(rf1,sort = T,main = "Variable Importance")
```

![](02-Evaluation_files/figure-latex/unnamed-chunk-37-1.pdf)<!-- --> 

 > Gráfico del Error vs número de árboles


```r
  plot(rf1, main="Error de clasificación vs núero de  árboles") 
```

![](02-Evaluation_files/figure-latex/unnamed-chunk-38-1.pdf)<!-- --> 

  > Gráfico de la probabilidad condicional: $P(Y=1|X_1 = ATTORNEY,\ldots,X_6=SEATBELT)$



```r
  rf1.prediction <- as.data.frame(predict(rf1, newdata = dt_train))
  summary(rf1.prediction)
```

```
##  predict(rf1, newdata = dt_train)
##  Min.   :0.00005                 
##  1st Qu.:0.00599                 
##  Median :0.03651                 
##  Mean   :0.11984                 
##  3rd Qu.:0.21024                 
##  Max.   :0.77138                 
##  NA's   :179
```

```r
  dt_train$pred_rf1 <- rf1.prediction$`predict(rf1, newdata = dt_train)` 
  head(dt_train,3)
```

```
##   ATTORNEY CLMSEX MARITAL CLMINSUR SEATBELT CLMAGE   LOSS Y     pred_rf1
## 1        1      1    <NA>        2        1     50 34.940 1           NA
## 2        2      2       2        1        1     28 10.892 1 0.3807199932
## 3        2      1       2        2        1      5  0.330 0 0.0001324938
```

```r
  tail(dt_train,3)
```

```
##      ATTORNEY CLMSEX MARITAL CLMINSUR SEATBELT CLMAGE  LOSS Y    pred_rf1
## 1335        2      2       2        2        1     26 0.161 0 0.001142849
## 1338        2      2       1        2        1     39 0.099 0 0.012354460
## 1340        2      2       2        2        1     30 0.688 0 0.002329733
```

```r
  summary(dt_train)
```

```
##  ATTORNEY  CLMSEX    MARITAL    CLMINSUR   SEATBELT       CLMAGE     
##  1:471    1   :406   1   :439   1   : 77   1   :885   Min.   : 0.00  
##  2:467    2   :523   2   :455   2   :833   2   : 17   1st Qu.:20.00  
##           NA's:  9   3   : 10   NA's: 28   NA's: 36   Median :32.00  
##                      4   : 25                         Mean   :33.06  
##                      NA's:  9                         3rd Qu.:43.00  
##                                                       Max.   :95.00  
##                                                       NA's   :134    
##       LOSS                Y             pred_rf1      
##  Min.   :  0.0050   Min.   :0.0000   Min.   :0.00005  
##  1st Qu.:  0.7123   1st Qu.:0.0000   1st Qu.:0.00599  
##  Median :  2.3645   Median :0.0000   Median :0.03651  
##  Mean   :  5.4656   Mean   :0.1141   Mean   :0.11984  
##  3rd Qu.:  4.0263   3rd Qu.:0.0000   3rd Qu.:0.21024  
##  Max.   :273.6040   Max.   :1.0000   Max.   :0.77138  
##                                      NA's   :179
```

```r
  plot(density(dt_train$pred_rf1[!is.na(dt_train$pred_rf1)]), col="red" , xlab="Probabilidad" , main="Función de densidad estimada")
```

![](02-Evaluation_files/figure-latex/unnamed-chunk-39-1.pdf)<!-- --> 

 - Vemos que hay (claramente) dos concentraciones (clases) de probabilidades de pérdida, una concentración en torno a la probabilidad de pérdida no severa ($Y=0$) y otra para la pérdida severa ($Y=1$).

 - Esto no lleva a la elección del **punto de corte óptimo** para obtener una regla de clasificación, es decir, un criterio para $Y_{predicted}=1$ (pérdida severa), o bien, para $Y_{predicted}=0$ (pérdida no severa). Una alternativa es el criterio de la **Distancia de Kolmogorov-Smirnov** (KS).




 > Métricas de evaluación del poder de clasificación


```r
if (!require(ModelMetrics)) install.packages('ModelMetrics')
library(ModelMetrics)
if (!require(ROCR)) install.packages('ROCR')
library(ROCR)
if (!require(binaryLogic)) install.packages('binaryLogic')
library(binaryLogic)
```

 - Con el train creamos un objeto de tipo 'prediction'^[https://www.r-bloggers.com/a-small-introduction-to-the-rocr-package/]


```r
  rf1.pred <- prediction(as.numeric(rf1$predicted),as.numeric(rf1$y)) 
```

 - Calculamos la Curva de ROC con la función 'performance' sobre el objeto 'rf1'

```r
  rf1.perf <- performance(rf1.pred,"tpr","fpr") 
 ## "fpr" = False positive rate. P(Yhat = + | Y = -). Estimated as: FP/N.
 ## "tpr" = True positive rate. P(Yhat = + | Y = +). Estimated as: TP/P.
 plot(rf1.perf)
```

![](02-Evaluation_files/figure-latex/unnamed-chunk-42-1.pdf)<!-- --> 


 > Elección del punto de corte: Criterio de la distancia de KS

 - La distancia KS se calcula como: KS = abs(rf1.perf@y.values[[1]]-rf1.perf@x.values[[1]])


```r
rf1.perf@alpha.values[[1]][rf1.perf@alpha.values[[1]]==Inf] <- round(max(rf1.perf@alpha.values[[1]][rf1.perf@alpha.values[[1]]!=Inf]),2)
KS.matrix= cbind(abs(rf1.perf@y.values[[1]]-rf1.perf@x.values[[1]]), rf1.perf@alpha.values[[1]])
```

 - Resumiendo


```r
colnames(KS.matrix) <- c("KS-distance","cut-point")
head(KS.matrix)
```

```
##      KS-distance cut-point
## [1,] 0.000000000 0.7800000
## [2,] 0.001497006 0.7809184
## [3,] 0.002994012 0.7353170
## [4,] 0.004491018 0.6577091
## [5,] 0.005988024 0.6481896
## [6,] 0.005000987 0.6297476
```

```r
ind.ks  <- sort( KS.matrix[,1] , index.return=TRUE )$ix[nrow(KS.matrix)] 
```

 - El punto de corte óptimo de KS:

```r
  rf1.KScutoff <- KS.matrix[ind.ks,2] # := f(rf1.KS1)
  rf1.KScutoff
```

```
##  cut-point 
## 0.06415734
```

```r
# 0.04 - 0.05 
```


> Gráfico de la Curva ROC y su métrica: Área bajo la curva ROC (AUC)


 - Cálculo de AUC mediante la función 'performance'

```r
rf1.auc1 <- performance(rf1.pred,"auc")@y.values[[1]]
rf1.auc1
```

```
FALSE [1] 0.7424327
```

 -Cálculo de la curva ROC junto con la métrica AUC 

```r
#win.graph()
plot( rf1.perf , col='red'  , lwd=2, type="l", xlab="Tasa de falsos positivos" , ylab="Tasa de verdaderos positivos", main="Curva ROC con Random Forest")
abline( 0 , 1  , col="blue" , lwd=2, lty=2)
abline( 0 , 0 , 1 , col="gray40"   , lty=3)
legend( 0.4, 0.15 , c(paste0("AUC (Random Forest)=",round(rf1.auc1,4)),"AUC (clasificacion al azar)=0.50"),lty=c(1,2), lwd=c(2,2) ,col=c("red","blue"), bty="n")
```

![](02-Evaluation_files/figure-latex/unnamed-chunk-47-1.pdf)<!-- --> 

 - Se realizar el mismo gráfico de la curva ROC utilizando la librería `ggplot2`. Para ello guardamos los datos en un `data.frame`

```r
library("ggplot2")
df.perf <- data.frame(x=rf1.perf@x.values[[1]],y=rf1.perf@y.values[[1]])
```

 - Construcción del objeto gráfico con `ggplot2`

```r
#win.graph()
p <- ggplot(df.perf,aes(x=x,y=y)) + geom_path(size=1, colour="red")
p <- p + ggtitle("Curva ROC modelo Random Forest")
p <- p + theme_update(plot.title = element_text(hjust = 0.5))
p <- p + geom_segment(aes(x=0,y=0,xend=1,yend=1),colour="blue",linetype= 2)
p <- p + geom_text(aes(x=0.75 , y=0.3 , label=paste(sep ="","AUC (Random Forest) ) = ",round(rf1.auc1,4) )),colour="black",size=4)
p <- p + geom_text(aes(x=0.75 , y=0.25 , label=paste(sep ="","AUC (Coin toss) = ",round(0.50,4) )),colour="black",size=4)
p <- p + scale_x_continuous(name= "Tasa de falsos positivos")
p <- p + scale_y_continuous(name= "Tasa de verdaderos positivos")
p <- p + theme(
  plot.title   = element_text(size = 2),
  axis.text.x  = element_text(size = 10),
  axis.text.y  = element_text(size = 10),
  axis.title.x = element_text(size = 12,face = "italic"),
  axis.title.y = element_text(size = 12,face = "italic",angle=90),
  legend.title     = element_blank(), 
  panel.background = element_rect(fill = "grey"),
  panel.grid.minor = element_blank(), 
  panel.grid.major = element_line(colour='white'),
  plot.background  = element_blank()
)

p
```

![](02-Evaluation_files/figure-latex/unnamed-chunk-49-1.pdf)<!-- --> 

 > Métricas de evaluación del poder predictivo

 - Calculamos la predicción en el _test_ y evaluamos el poder de clasificación del modelo

```r
rf1.pred_test     <- as.data.frame(predict( rf1, newdata = dt_test))
dt_test$pred_rf1  <- rf1.pred_test$`predict(rf1, newdata = dt_test)` 
```


```r
head(dt_test,3)
```

```
##    ATTORNEY CLMSEX MARITAL CLMINSUR SEATBELT CLMAGE   LOSS Y  pred_rf1
## 6         1      2       1        2        1     35  0.309 0 0.2279703
## 12        1      1       1        2        1     42 29.620 1 0.2159590
## 18        1      1       1        2        1     58  0.758 0 0.2047155
```

```r
tail(dt_test,3)
```

```
##      ATTORNEY CLMSEX MARITAL CLMINSUR SEATBELT CLMAGE  LOSS Y    pred_rf1
## 1336        2      1       2        2        1     NA 0.576 0          NA
## 1337        1      2       1        2        1     46 3.705 0 0.349066396
## 1339        1      2       2        1        1     18 3.277 0 0.004032191
```

```r
summary(dt_test)
```

```
##  ATTORNEY  CLMSEX    MARITAL    CLMINSUR   SEATBELT       CLMAGE     
##  1:214    1   :180   1   :185   1   : 43   1   :385   Min.   : 0.00  
##  2:188    2   :219   2   :195   2   :346   2   :  5   1st Qu.:19.00  
##           NA's:  3   3   :  5   NA's: 13   NA's: 12   Median :29.00  
##                      4   : 10                         Mean   :31.31  
##                      NA's:  7                         3rd Qu.:42.00  
##                                                       Max.   :78.00  
##                                                       NA's   :55     
##       LOSS                 Y             pred_rf1      
##  Min.   :   0.0050   Min.   :0.0000   Min.   :0.00005  
##  1st Qu.:   0.5175   1st Qu.:0.0000   1st Qu.:0.00797  
##  Median :   2.1645   Median :0.0000   Median :0.03794  
##  Mean   :   7.0917   Mean   :0.1144   Mean   :0.12531  
##  3rd Qu.:   3.7782   3rd Qu.:0.0000   3rd Qu.:0.21666  
##  Max.   :1067.6970   Max.   :1.0000   Max.   :0.75036  
##                                       NA's   :70
```

 - Con el _test_ creamos un objeto de tipo 'prediction' y calculamos la curva ROC

```r
dt_test.pred  <- prediction(as.numeric(rf1.pred_test$`predict(rf1, newdata = dt_test)`),dt_test$Y) 
dt_test.perf  <- performance(dt_test.pred,"tpr","fpr") 
```

 - Evaluación del poder de clasificación del modelo RF1 vía curva ROC

```r
rf1.test.auc <- performance(dt_test.pred ,"auc")@y.values[[1]]
```

 - Gráfico de la curva ROC para el _test_ 

```r
#win.graph()
plot( dt_test.perf , col='red' , lwd=2, type="l" , main="Curva ROC modelo RF - test",xlab="Tasa de falsos positivos", ylab="Tasa de verdaderos positivos")
abline( 0 , 1  , col="blue" , lwd=2, lty=2)
abline( 0 , 0 , 1 , col="gray40"   , lty=3)
legend( 0.4, 0.2 , c(paste0("AUC (Random Forest)=",round(rf1.test.auc,4)),"AUC (Coin toss)=0.50") ,lty=c(1,2), lwd=c(2,2) ,col=c("red","blue"), bty="n")
```

![](02-Evaluation_files/figure-latex/unnamed-chunk-54-1.pdf)<!-- --> 



> Métrica de error del clasificador RF:

 - Error tipo I ($\alpha$): 22.50%, indica el error que se comete clasificando una pérdida 'severa' como 'no severa'
 - Error tipo II ($\beta$): 43.15%, indica el error que se comete clasificando una pérdida 'no severa' como 'severa'
 - % mala clasificación ($%mc$) : 40.66%, indica el % de veces que el modelo clasifica incorrectamente las pérdidas 
 - Accuracy = $100 - %$: 59.34%, indica el % de veces que el modelo acierta clasificando las pérdidas
 - Area bajo la curva ROC $AUC$: 0.6988, medida global del poder de clasificación del modelo
 - Finalmente calculamos la curva ROC junto con la métrica AUC


 > Resumiendo:

Una función útil para obtener rápidamente el análisis de un clasificador binario es la siguiente:



```r
metricBinaryClass = function( fitted.model , dataset , cutpoint=NULL , roc.graph=TRUE){
  
  # fitted.model : The Binary Classification model that is under evaluation. If provided, dataset contains all variables in the fitted model (target and predictors).
  # dataset      : If fitted.model is not provided, dataset should has only two columns, predictions and labels.
  # cuttpoint    : potimal cutoff or cutpoint to be used to split continuous predictions into two response categories of target variable
  # roc.graph    : If true, ROC curve graph for the model is shown 
  
  #install.packages("binaryLogic")
  library(binaryLogic)
  
  if( missing(fitted.model) | is.null(fitted.model) ){
    
    tabl  <- as.data.frame(dataset)
  } 
  
  else {
    if( class(fitted.model)[1] %in% c('glm','lm','randomForest.formula','randomForest') ){
      tabl.pred <- as.data.frame(predict( fitted.model, newdata = dataset ))
      tabl <- as.data.frame(cbind(tabl.pred[[1]], dataset[,'Y'] )) 
      tabl <- tabl[!is.na(tabl[[1]]),]
    }
    if( class(fitted.model)[1] %in% c("gbm") ){
      tabl.pred <- as.data.frame(predict.gbm( fitted.model , newdata = dataset , n.trees = 5000 , type="response" ))
      tabl <- as.data.frame(cbind(tabl.pred[[1]], dataset[,'Y'] )) 
      tabl <- tabl[!is.na(tabl[[1]]),]
    }
    if( class(fitted.model)[1] %in% c('svm.formula','svm') ){
      tabl.pred <- as.data.frame(predict( fitted.model, newdata = dataset ))
      ids_NAs <- na.index(dataset)
      tabl <- as.data.frame( cbind(tabl.pred[[1]], dataset[-ids_NAs,'Y']) ) 
      tabl <- tabl[!is.na(tabl[[1]]),]
    }
  }
  colnames(tabl) <- c('predicted','actual')
  
  # ROCR objects
  require(ROCR)
  obj.pred <- prediction(tabl$predicted,tabl$actual)
  obj.perf <- performance(obj.pred,"tpr","fpr")
  obj.auc  <- performance(obj.pred,"auc")@y.values[[1]]
  # For ROC curve:
  obj.perf@alpha.values[[1]][obj.perf@alpha.values[[1]]==Inf] <- max(obj.perf@alpha.values[[1]][obj.perf@alpha.values[[1]]!=Inf])
  # KS criteria
  KS.matrix= cbind(abs(obj.perf@y.values[[1]]-obj.perf@x.values[[1]]), obj.perf@alpha.values[[1]])
  # KS cutoff
  # colnames(KS.matrix) <- c("KS-distance","cut-point")
  ind.ks  <- sort( KS.matrix[,1] , index.return=TRUE )$ix[nrow(KS.matrix)] 
  
  if( missing(cutpoint) | is.null(cutpoint) ) cutpoint <- KS.matrix[ind.ks,2]
  
  if( !(is.binary(tabl)) ){
    
    # Make predictions objs.
    # Binary metrics 
    tp = sum( tabl$predicted  >  cutpoint & tabl$actual >  cutpoint)
    fp = sum( tabl$predicted  >  cutpoint & tabl$actual <= cutpoint)
    tn = sum( tabl$predicted  <= cutpoint & tabl$actual <= cutpoint)
    fn = sum( tabl$predicted  <= cutpoint & tabl$actual >  cutpoint) 
    pos = tp+fn
    neg = tn+fp
    acc=  100*(tp+tn)/(pos+neg)
    prec= 100*tp/(tp+fp)
    sens= 100*tp/(tp+fn) # = tpr = recall = 1 - error alpha
    spec= 100*tn/(tn+fp) # 1- error beta
    fpr = 100*fp/neg  # error beta (tipo II) = 1 - spec
    fnr = 100*fn/pos  # error alpha (tipo I) = 1- recall = 1- sens
  }
  
  if( is.binary(tabl) ){
    
    tp = sum( tabl$predicted  == 1 & tabl$actual == 1)
    fp = sum( tabl$predicted  == 1 & tabl$actual == 0)
    tn = sum( tabl$predicted  == 0 & tabl$actual == 0)
    fn = sum( tabl$predicted  == 0 & tabl$actual == 1) 
    pos = tp+fn
    neg = tn+fp
    acc=  100*(tp+tn)/(pos+neg)
    prec= 100*tp/(tp+fp)
    sens= 100*tp/(tp+fn) # = tpr = recall = 1 - error alpha
    spec= 100*tn/(tn+fp) # 1- error beta
    fpr = 100*fp/neg  # error beta (tipo II) = 1 - spec
    fnr = 100*fn/pos  # error alpha (tipo I) = 1- recall = 1- sens
  }  
  
  if(roc.graph==TRUE){
    win.graph()
    plot( obj.perf  , col='red' , lwd=2, type="l",xlab="Tasa de falsos positivos" , ylab="Tasa de verdaderos positivos", main="Curva ROC modelo clasificación")
    abline( 0.0 , 1.0 , col="blue", lwd=2, lty=2)
    abline( 0.0 , 0.0 , 1 , col="gray40" , lty=3)
    legend( 0.45, 0.2 , c(paste0("AUC (Model)=",round(obj.auc,4)),"AUC (Rolling dice)=0.50") ,lty=c(1,2), lwd=c(2,2) ,col=c("red","blue"), bty="n")
  }
  
  list(ClassError.tI=round(fnr,2), ClassError.tII=round(fpr,2), Accuracy=round(acc,2),Sensitivity = round(sens,2) , Specificity= round(spec,2), auc= obj.auc , Fisher.F1=round(2*prec*sens/(prec+sens),4) )
  
}
```



```r
 metricBinaryClass( fitted.model = rf1 , dataset= dt_test , cutpoint=rf1.KScutoff , roc.graph=TRUE)
```

```
## $ClassError.tI
## [1] 22.5
## 
## $ClassError.tII
## [1] 43.49
## 
## $Accuracy
## [1] 59.04
## 
## $Sensitivity
## [1] 77.5
## 
## $Specificity
## [1] 56.51
## 
## $auc
## [1] 0.7056507
## 
## $Fisher.F1
## [1] 31.3131
```

### Regresión

<div class="info">
Vamos a construir un modelo para prever las pérdidas.
</div>

 > Modelo con _Random Forest_ en _train_


```r
fmla.rf2 <- as.formula(paste0('LOSS','~',paste0(colnames(df_autobi[,-c(7,8)]),collapse = "+"),collapse = ''))
set.seed(112233) #recomendado

rf2 <- randomForest( fmla.rf2,
                     data =dt_train,
                     ntree = 5000,
                     replace =TRUE,
                     mtry=4,
                     maxnodes =50,
                     importance = TRUE,
                     na.action=na.omit)

summary(rf2)
```

```
##                 Length Class  Mode     
## call               9   -none- call     
## type               1   -none- character
## predicted        759   -none- numeric  
## mse             5000   -none- numeric  
## rsq             5000   -none- numeric  
## oob.times        759   -none- numeric  
## importance        12   -none- numeric  
## importanceSD       6   -none- numeric  
## localImportance    0   -none- NULL     
## proximity          0   -none- NULL     
## ntree              1   -none- numeric  
## mtry               1   -none- numeric  
## forest            11   -none- list     
## coefs              0   -none- NULL     
## y                759   -none- numeric  
## test               0   -none- NULL     
## inbag              0   -none- NULL     
## terms              3   terms  call     
## na.action        179   omit   numeric
```


```r
str(rf2)
```

```
## List of 19
##  $ call           : language randomForest(formula = fmla.rf2, data = dt_train, ntree = 5000, replace = TRUE,      mtry = 4, maxnodes = 50, imp| __truncated__
##  $ type           : chr "regression"
##  $ predicted      : Named num [1:759] 2.407 0.676 37.029 1.73 4.229 ...
##   ..- attr(*, "names")= chr [1:759] "2" "3" "4" "5" ...
##   ..- attr(*, "na.action")= 'omit' Named int [1:179] 1 9 19 25 27 40 43 46 50 51 ...
##   .. ..- attr(*, "names")= chr [1:179] "1" "10" "24" "30" ...
##  $ mse            : num [1:5000] 566 483 441 401 427 ...
##  $ rsq            : num [1:5000] -0.707 -0.457 -0.328 -0.208 -0.286 ...
##  $ oob.times      : int [1:759] 1820 1870 1853 1846 1856 1840 1820 1899 1880 1876 ...
##  $ importance     : num [1:6, 1:2] 17.313 0.136 -3.726 3.199 4.752 ...
##   ..- attr(*, "dimnames")=List of 2
##   .. ..$ : chr [1:6] "ATTORNEY" "CLMSEX" "MARITAL" "CLMINSUR" ...
##   .. ..$ : chr [1:2] "%IncMSE" "IncNodePurity"
##  $ importanceSD   : Named num [1:6] 1.215 1.018 1.116 0.38 0.743 ...
##   ..- attr(*, "names")= chr [1:6] "ATTORNEY" "CLMSEX" "MARITAL" "CLMINSUR" ...
##  $ localImportance: NULL
##  $ proximity      : NULL
##  $ ntree          : num 5000
##  $ mtry           : num 4
##  $ forest         :List of 11
##   ..$ ndbigtree    : int [1:5000] 99 99 99 99 99 99 99 99 99 99 ...
##   ..$ nodestatus   : int [1:99, 1:5000] -3 -3 -3 -3 -3 -3 -1 -3 -3 -3 ...
##   ..$ leftDaughter : int [1:99, 1:5000] 2 4 6 8 10 12 0 14 16 18 ...
##   ..$ rightDaughter: int [1:99, 1:5000] 3 5 7 9 11 13 0 15 17 19 ...
##   ..$ nodepred     : num [1:99, 1:5000] 4.96 4.6 23.76 7.19 1.9 ...
##   ..$ bestvar      : int [1:99, 1:5000] 5 1 6 3 6 3 0 6 2 3 ...
##   ..$ xbestsplit   : num [1:99, 1:5000] 1.5 1.5 37.5 3.5 20.5 1.5 0 25.5 1.5 1.5 ...
##   ..$ ncat         : Named int [1:6] 1 1 1 1 1 1
##   .. ..- attr(*, "names")= chr [1:6] "ATTORNEY" "CLMSEX" "MARITAL" "CLMINSUR" ...
##   ..$ nrnodes      : int 99
##   ..$ ntree        : num 5000
##   ..$ xlevels      :List of 6
##   .. ..$ ATTORNEY: num 0
##   .. ..$ CLMSEX  : num 0
##   .. ..$ MARITAL : num 0
##   .. ..$ CLMINSUR: num 0
##   .. ..$ SEATBELT: num 0
##   .. ..$ CLMAGE  : num 0
##  $ coefs          : NULL
##  $ y              : Named num [1:759] 10.892 0.33 11.037 0.138 3.538 ...
##   ..- attr(*, "na.action")= 'omit' Named int [1:179] 1 9 19 25 27 40 43 46 50 51 ...
##   .. ..- attr(*, "names")= chr [1:179] "1" "10" "24" "30" ...
##   ..- attr(*, "names")= chr [1:759] "2" "3" "4" "5" ...
##  $ test           : NULL
##  $ inbag          : NULL
##  $ terms          :Classes 'terms', 'formula'  language LOSS ~ ATTORNEY + CLMSEX + MARITAL + CLMINSUR + SEATBELT + CLMAGE
##   .. ..- attr(*, "variables")= language list(LOSS, ATTORNEY, CLMSEX, MARITAL, CLMINSUR, SEATBELT, CLMAGE)
##   .. ..- attr(*, "factors")= int [1:7, 1:6] 0 1 0 0 0 0 0 0 0 1 ...
##   .. .. ..- attr(*, "dimnames")=List of 2
##   .. .. .. ..$ : chr [1:7] "LOSS" "ATTORNEY" "CLMSEX" "MARITAL" ...
##   .. .. .. ..$ : chr [1:6] "ATTORNEY" "CLMSEX" "MARITAL" "CLMINSUR" ...
##   .. ..- attr(*, "term.labels")= chr [1:6] "ATTORNEY" "CLMSEX" "MARITAL" "CLMINSUR" ...
##   .. ..- attr(*, "order")= int [1:6] 1 1 1 1 1 1
##   .. ..- attr(*, "intercept")= num 0
##   .. ..- attr(*, "response")= int 1
##   .. ..- attr(*, ".Environment")=<environment: R_GlobalEnv> 
##   .. ..- attr(*, "predvars")= language list(LOSS, ATTORNEY, CLMSEX, MARITAL, CLMINSUR, SEATBELT, CLMAGE)
##   .. ..- attr(*, "dataClasses")= Named chr [1:7] "numeric" "ordered" "ordered" "ordered" ...
##   .. .. ..- attr(*, "names")= chr [1:7] "LOSS" "ATTORNEY" "CLMSEX" "MARITAL" ...
##  $ na.action      : 'omit' Named int [1:179] 1 9 19 25 27 40 43 46 50 51 ...
##   ..- attr(*, "names")= chr [1:179] "1" "10" "24" "30" ...
##  - attr(*, "class")= chr [1:2] "randomForest.formula" "randomForest"
```

 > Importancia Relativa de las Variables _Input_


```r
varImpPlot(rf2,sort = T,main="Variable Importance")
```

![](02-Evaluation_files/figure-latex/unnamed-chunk-59-1.pdf)<!-- --> 

 > Previsión en _test_


```r
rf2.prediction <- as.data.frame(predict(rf2, newdata = dt_test))
dt_test$pred_rf2 <- rf2.prediction[[1]] 
```


```r
head(dt_test, 3)
```

```
##    ATTORNEY CLMSEX MARITAL CLMINSUR SEATBELT CLMAGE   LOSS Y  pred_rf1
## 6         1      2       1        2        1     35  0.309 0 0.2279703
## 12        1      1       1        2        1     42 29.620 1 0.2159590
## 18        1      1       1        2        1     58  0.758 0 0.2047155
##    pred_rf2
## 6  7.914295
## 12 8.539666
## 18 9.864992
```

```r
tail(dt_test, 3)
```

```
##      ATTORNEY CLMSEX MARITAL CLMINSUR SEATBELT CLMAGE  LOSS Y    pred_rf1
## 1336        2      1       2        2        1     NA 0.576 0          NA
## 1337        1      2       1        2        1     46 3.705 0 0.349066396
## 1339        1      2       2        1        1     18 3.277 0 0.004032191
##       pred_rf2
## 1336        NA
## 1337 36.877666
## 1339  3.963606
```

```r
summary(dt_test, 3)
```

```
##  ATTORNEY  CLMSEX       MARITAL    CLMINSUR   SEATBELT       CLMAGE     
##  1:214    1   :180   2      :195   1   : 43   1   :385   Min.   : 0.00  
##  2:188    2   :219   (Other):200   2   :346   2   :  5   1st Qu.:19.00  
##           NA's:  3   NA's   :  7   NA's: 13   NA's: 12   Median :29.00  
##                                                          Mean   :31.31  
##                                                          3rd Qu.:42.00  
##                                                          Max.   :78.00  
##                                                          NA's   :55     
##       LOSS                 Y             pred_rf1          pred_rf2     
##  Min.   :   0.0050   Min.   :0.0000   Min.   :0.00005   Min.   : 0.377  
##  1st Qu.:   0.5175   1st Qu.:0.0000   1st Qu.:0.00797   1st Qu.: 2.048  
##  Median :   2.1645   Median :0.0000   Median :0.03794   Median : 3.293  
##  Mean   :   7.0917   Mean   :0.1144   Mean   :0.12531   Mean   : 6.375  
##  3rd Qu.:   3.7782   3rd Qu.:0.0000   3rd Qu.:0.21666   3rd Qu.: 7.881  
##  Max.   :1067.6970   Max.   :1.0000   Max.   :0.75036   Max.   :56.904  
##                                       NA's   :70        NA's   :70
```


 > Graficamos la distribución de los valores estimados en el _train_
 

```r
plot(density(dt_test$pred_rf2[!is.na(dt_test$pred_rf2) & dt_test$pred_rf2 < 30]), ylim= c(0,.25) , col="red" , main="")
lines(density(dt_test$LOSS[dt_test$LOSS<30]),col="blue",lty=1)
```

![](02-Evaluation_files/figure-latex/unnamed-chunk-62-1.pdf)<!-- --> 


```r
modelchecktest1 <- as.data.frame( cbind(real=dt_test$LOSS , predicted=dt_test$pred_rf2) )
modelchecktest1[is.na(modelchecktest1)] <- 0

summary(modelchecktest1)
```

```
##       real             predicted     
##  Min.   :   0.0050   Min.   : 0.000  
##  1st Qu.:   0.5175   1st Qu.: 1.310  
##  Median :   2.1645   Median : 2.422  
##  Mean   :   7.0917   Mean   : 5.265  
##  3rd Qu.:   3.7782   3rd Qu.: 7.469  
##  Max.   :1067.6970   Max.   :56.904
```

 > Error de ajuste del modelo


```r
plot(modelchecktest1, xlim=c(0,100) , ylim=c(0,100) ,  pch="." , cex=1.5)
segments( 0, 0 , 100, 100 , col="red")
```

![](02-Evaluation_files/figure-latex/unnamed-chunk-64-1.pdf)<!-- --> 

 > Resumiendo
 
Una función útil para medir el error:





```r
modelMetrics(real=modelchecktest1$real, pred=modelchecktest1$predicted )
```

```
##  Accuracy metrics (global):
```

```
## MAE(ref) = 8.9208
```

```
## MAE = 7.765
```

```
## RMSE = 54.5686
```

```
## MAPE = 127.01
```

```
## MAPE(sim) = 68.65
```

```
## WMAPE = 109.49
```
 

 * Commentario: El error de ajuste del modelo de regresión es demasiado alto: $RMSE= 54.57$ y el $MAPE=127.19%$
Con estos errores de predicción, es preferible utilizar a un modelo de clasificación en lugar de un     modelo de regresión.


<div class="rmdcomment">

 **Ejercicio sugerido**

 - Ajustar un Modelo de Regresión Logística para $Y$ y comparar los resultados con los proporcionados por el _Random Forest_

 - Ajustar un Modelo de Regresión Lineal para $LOSS$ y comparar los resultados con los proporcionados por el _Random Forest_


</div>
 

<!--chapter:end:02-Evaluation.Rmd-->

# (PART) Modelos Estadísticos {-} 



# Regresión Lineal



![](03-RegresionLineal_files/figure-latex/unnamed-chunk-2-1.pdf)<!-- --> 


![](03-RegresionLineal_files/figure-latex/unnamed-chunk-3-1.pdf)<!-- --> 




<img src='http://reliawiki.org/images/8/81/Doe4.4.png' alt="Linear Regression Model" style="float:width:90%;">



<img src='http://reliawiki.org/images/2/28/Doe4.3.png' alt="Linear Regression Model" style="float:width:90%;">













<br>

<div class="rmdcomment">

Nota sobre **Modelo Estadístico**

 - Los datos varian según las condiciones de su contexto experimental.  

 - La **variabilidad** en los datos, puede ser  expresada de manera simplificada a través de un modelo, conformado por una **ecuación** y una serie de **hipótesis** sobre las componentes de azar que subyacen el estudio.

 - La **ecuación** del modelo incluye siempre dos partes, una **determinísta** asociada con variaciones sistemáticas y que se reconoce que van a existir incluso antes de realizar el experimento  y otra que depende de **componentes aleatorias** que son imposible de controlar y usualmente inherentes a la variabilidad propia del fenómeno aleatorio en estudio.

    $$ \text{Y} = \text{Funcion Deterministica} + \text{Perturbacion Aleatoria} $$

 - También se puede decir que en un modelo estadístico hay siempre dos estructuras íntimamente relacionadas:

    - La **estructura de media** (que provee el valor esperado para la respuesta bajo las condiciones experimentales)
    - La **estructura de varianzas y covarianzas** (asociada a la o las componentes aleatorias del modelo).




## ¿Qué es un Modelo de Regresión?

<br>
<div class="info">
 El modelo de regresión se utiliza para representar la relación entre $Y$ y $X$: 
 $$Y = f(x)$$
</div>


  - $Y$ es una variable respuesta, explicada o dependiente: que depende de otras y que tratamos de explicar/predecir.
  - $X$, o mas bien $X_1, X_2, \ldots, X_K$ son variables explicativas o independientes que permiten explicar/predecir $Y$.



<br>
<div class="info">
 Una regresión es lineal, cuando la función $f(x)$ que relaciona $X$ e $Y$ es una **función lineal**.
</div>

Teniendo $K$ variables explicativas, la regresión lineal es:

$$ Y = \beta_0  + \beta_1 X_1 + \beta_2 X_2 + ... + \beta_K X_K + \epsilon; \epsilon \sim N(0,\sigma^2) $$

De manera que el valor de la predicción de la variable $Y$ será:

$$ \hat{Y}  = \beta_0  + \beta_1 X_1 + \beta X_2 + ... + \beta X_K $$

O lo que es lo mismo:

$$  \epsilon = Y - \hat{Y} $$
Así, $\epsilon$  será el error cometido en la previsión de $Y$ usando el mode.


<br>
<div class="info">
En función del número $K$ de variables explicativas que tengamos, la regresión lineal puede ser **Simple** o **Múltiple**.
</div>

  - **Simple** si hay una única variable independiente ($K = 1$).
  - **Múltiple** si hay varias variables independendientes ($K > 1$).


Para obtener la $\hat{Y}$ es necesario conocer $\beta_0, \ldots, \beta_K$, es decir, falta el proceso de **inferencia estadística**.




<br>
<div class="info">
 La **Inferencia Estadística** es el procedimiento que permiten elaborar conclusiones sobre parámetros poblacionales desconocidos. 
</div>



$$ \hat{Y}  = \hat{\beta}_0  + \hat{\beta}_1 X_1 + \hat{\beta} X_2 + ... + \hat{\beta} X_K $$


  - Conocer o estimar a un parámetro de la distribución de una variable es posible a través de un estadístico (estadístico muestral). 
  - Dado que el estadístico es obtenido a partir de una muestra y hay más de una muestra posible de ser elegida, el valor del estadístico dependerá de la muestra seleccionada. 
  - Como los valores de los estadísticos cambian de una muestra a otra. Interesa contar con una medida de estos cambios para cuantificar la medida del error en el que podría incurrirse al hacer una inferencia.



<br>
<div class="rmdcomment">

 Los parámetros de un modelo de regresión se pueden estimar con:
 
 - **Mínimos Cuadrados Ordinarios (OLS)**
 - **Máxima Verosimilitud (ML)**
 - **Inferencia Bayesiana**
 
</div>



### Ver también:

 - [Simple Linear Regression Analysis](http://reliawiki.org/index.php/Simple_Linear_Regression_Analysis)
 
  - [¿Cómo Validar Tu Modelo De Regresión?](https://www.maximaformacion.es/master-estadistica-aplicada-con-r/blog/item/como-validar-tu-modelo-de-regresion.html)


 - [¿Qué modelo de regresión debería elegir?]( https://www.maximaformacion.es/master-estadistica-aplicada-con-r/blog/item/que-modelo-de-regresion-deberia-elegir.html)
 

 - [¿Cómo seleccionar las variables adecuadas para tu modelo?](https://www.maximaformacion.es/master-estadistica-aplicada-con-r/blog/item/como-seleccionar-las-variables-adecuadas-para-tu-modelo.html)


-[Multiple Linear Regression Analysis](http://reliawiki.org/index.php/Multiple_Linear_Regression_Analysis)


## Práctica en R

### Regresión lineal simple



Para realizarla usaremos la base de datos `Boston` de la librería `MASS`. 

```r
library(MASS)
```

Veamos la descripción de `Boston` en la ayuda de R:


```r
?Boston
```

```
## starting httpd help server ... done
```

Observamos que es un data.frame con 506 observaciones y 14 variables. 

Podemos explorar un poco más los datos usando las funciones `head`, `tail` y `summary`.


```r
head(Boston,5)
```

```
##      crim zn indus chas   nox    rm  age    dis rad tax ptratio  black
## 1 0.00632 18  2.31    0 0.538 6.575 65.2 4.0900   1 296    15.3 396.90
## 2 0.02731  0  7.07    0 0.469 6.421 78.9 4.9671   2 242    17.8 396.90
## 3 0.02729  0  7.07    0 0.469 7.185 61.1 4.9671   2 242    17.8 392.83
## 4 0.03237  0  2.18    0 0.458 6.998 45.8 6.0622   3 222    18.7 394.63
## 5 0.06905  0  2.18    0 0.458 7.147 54.2 6.0622   3 222    18.7 396.90
##   lstat medv
## 1  4.98 24.0
## 2  9.14 21.6
## 3  4.03 34.7
## 4  2.94 33.4
## 5  5.33 36.2
```


```r
tail(Boston,5)
```

```
##        crim zn indus chas   nox    rm  age    dis rad tax ptratio  black
## 502 0.06263  0 11.93    0 0.573 6.593 69.1 2.4786   1 273      21 391.99
## 503 0.04527  0 11.93    0 0.573 6.120 76.7 2.2875   1 273      21 396.90
## 504 0.06076  0 11.93    0 0.573 6.976 91.0 2.1675   1 273      21 396.90
## 505 0.10959  0 11.93    0 0.573 6.794 89.3 2.3889   1 273      21 393.45
## 506 0.04741  0 11.93    0 0.573 6.030 80.8 2.5050   1 273      21 396.90
##     lstat medv
## 502  9.67 22.4
## 503  9.08 20.6
## 504  5.64 23.9
## 505  6.48 22.0
## 506  7.88 11.9
```


```r
summary(Boston)
```

```
##       crim                zn             indus            chas        
##  Min.   : 0.00632   Min.   :  0.00   Min.   : 0.46   Min.   :0.00000  
##  1st Qu.: 0.08204   1st Qu.:  0.00   1st Qu.: 5.19   1st Qu.:0.00000  
##  Median : 0.25651   Median :  0.00   Median : 9.69   Median :0.00000  
##  Mean   : 3.61352   Mean   : 11.36   Mean   :11.14   Mean   :0.06917  
##  3rd Qu.: 3.67708   3rd Qu.: 12.50   3rd Qu.:18.10   3rd Qu.:0.00000  
##  Max.   :88.97620   Max.   :100.00   Max.   :27.74   Max.   :1.00000  
##       nox               rm             age              dis        
##  Min.   :0.3850   Min.   :3.561   Min.   :  2.90   Min.   : 1.130  
##  1st Qu.:0.4490   1st Qu.:5.886   1st Qu.: 45.02   1st Qu.: 2.100  
##  Median :0.5380   Median :6.208   Median : 77.50   Median : 3.207  
##  Mean   :0.5547   Mean   :6.285   Mean   : 68.57   Mean   : 3.795  
##  3rd Qu.:0.6240   3rd Qu.:6.623   3rd Qu.: 94.08   3rd Qu.: 5.188  
##  Max.   :0.8710   Max.   :8.780   Max.   :100.00   Max.   :12.127  
##       rad              tax           ptratio          black       
##  Min.   : 1.000   Min.   :187.0   Min.   :12.60   Min.   :  0.32  
##  1st Qu.: 4.000   1st Qu.:279.0   1st Qu.:17.40   1st Qu.:375.38  
##  Median : 5.000   Median :330.0   Median :19.05   Median :391.44  
##  Mean   : 9.549   Mean   :408.2   Mean   :18.46   Mean   :356.67  
##  3rd Qu.:24.000   3rd Qu.:666.0   3rd Qu.:20.20   3rd Qu.:396.23  
##  Max.   :24.000   Max.   :711.0   Max.   :22.00   Max.   :396.90  
##      lstat            medv      
##  Min.   : 1.73   Min.   : 5.00  
##  1st Qu.: 6.95   1st Qu.:17.02  
##  Median :11.36   Median :21.20  
##  Mean   :12.65   Mean   :22.53  
##  3rd Qu.:16.95   3rd Qu.:25.00  
##  Max.   :37.97   Max.   :50.00
```

Antes de continuar, hacemos la división de `Boston` en `train`y `test`.

```r
id_train <- sample(1:nrow(Boston), size = 0.8*nrow(Boston))
train <- Boston[id_train, ]
test <- Boston[-id_train, ]
```

Ajustamos el modelo de regresión lineal simple para predecir la variable `medv` utilizando la variable `lstat` de nuestro conjunto de datos `Boston`. Para ello usaremos la función `lm`.

```r
reg_ls <- lm(medv~lstat, data = train)
reg_ls
```

```
## 
## Call:
## lm(formula = medv ~ lstat, data = train)
## 
## Coefficients:
## (Intercept)        lstat  
##     34.1715      -0.9111
```

Veamos los coeficientes de la regresión

```r
reg_ls$coefficients
```

```
## (Intercept)       lstat 
##  34.1715319  -0.9111111
```

Donde el término independiente es:

```r
reg_ls$coefficients[1]
```

```
## (Intercept) 
##    34.17153
```

y el coeficiente de la variable `lstat`es:

```r
reg_ls$coefficients[2]
```

```
##      lstat 
## -0.9111111
```

De manera que la recta de regresión lineal, siendo $y$ la variable `medv` y $x$ la variable `lstat`,  es:

```
## y =  34.17153 +  -0.9111111 x
```


Si queremos obtener los errores residuales de las observaciones correspondientes:

```r
residuales <- reg_ls$residuals
# Veamos los residuales de las 10 primeras observaciones
residuales[1:10]
```

```
##        324        503        350        377        167        133 
## -4.9750880 -5.2986434 -2.2050877  0.9026893 19.1995790 -1.0399768 
##        213        157        316        469 
##  2.8335785 -6.3661993 -7.4937546  1.4469118
```

Una vez obtenido el modelo de regresión lineal, para realizar la predicción sobre un nuevo conjunto de datos, utilizamos la función `predict`, de la siguiente manera:

```r
predic <- predict(reg_ls, newdata = test)
#Veamos la predicción de las 10 primeras observaciones
predic[1:10]
```

```
##       11       14       19       21       22       23       28       34 
## 15.53931 26.64575 23.52064 15.01998 21.57087 17.11553 18.42753 17.45264 
##       38       44 
## 26.18109 27.39287
```

Algunas representaciones gráficas de un modelo de regresión son:

 - Dispersión de los puntos y la recta de regresión lineal simple obtenida:

```r
regresion <- lm(medv~lstat, data = Boston)

plot(Boston$lstat, Boston$medv, xlab = "lstat", ylab = "medv")
abline(regresion, col='red', lwd=2)
a <- regresion$coefficients[[1]]
b <- regresion$coefficients[[2]] 
text(30,40,labels = paste('Y = ', round(b,2),'x +', round(a,2)), col='red')
```

![](03-RegresionLineal_files/figure-latex/unnamed-chunk-17-1.pdf)<!-- --> 

 - Análisis de residuos:

```r
par(mfrow=c(2,2))
plot(regresion)
```

![](03-RegresionLineal_files/figure-latex/unnamed-chunk-18-1.pdf)<!-- --> 



### Regresión lineal múltiple
Utilizamos lo mismo que hemos hecho para la regresión lineal simple, con la diferencia de que ahora hay más de una variable independiente.
Usamos la misma función, `lm`, y la sucesión de variables independientes estarán separadas con un `+`, es decir:

```r
reg_lm <- lm(medv~lstat + age, data = train)
reg_lm
```

```
## 
## Call:
## lm(formula = medv ~ lstat + age, data = train)
## 
## Coefficients:
## (Intercept)        lstat          age  
##    32.84634     -0.98562      0.03292
```
Veamos los coeficientes de la regresión

```r
reg_lm$coefficients
```

```
## (Intercept)       lstat         age 
## 32.84634095 -0.98562034  0.03292455
```

Donde el término independiente es:

```r
reg_lm$coefficients[1]
```

```
## (Intercept) 
##    32.84634
```

el coeficiente de la variable `lstat` es:

```r
reg_lm$coefficients[2]
```

```
##      lstat 
## -0.9856203
```
y el coeficiente de la variable `age` es:

```r
reg_lm$coefficients[3]
```

```
##        age 
## 0.03292455
```

De manera que la recta de regresión lineal, siendo $y$ la variable `medv`, $x1$ la variable `lstat` y $x2$ la variable `age`, será:

```
## y =  32.84634 +  -0.9856203 x1 + 0.03292455 x2
```

Veamos los gráficos de dispersión 2 a 2:

```r
pairs(Boston[,c('medv','lstat', 'age')],panel = panel.smooth)
```

![](03-RegresionLineal_files/figure-latex/]-1.pdf)<!-- --> 

Análogamente a como hemos hecho con la regresión lineal, podemos obtener los residuos y utilizar la función `predict` en un nuevo conjunto de datos.

Hay otras opciones de poner la variables independientes. Por ejemplo, si quisieramos usar todas las variables, como conjunto de variables independientes, bastaría con escribir:

```r
reg_lm2 <- lm(medv~., data = train)
reg_lm2
```

```
## 
## Call:
## lm(formula = medv ~ ., data = train)
## 
## Coefficients:
## (Intercept)         crim           zn        indus         chas  
##   43.865429    -0.106094     0.045046    -0.034204     3.122022  
##         nox           rm          age          dis          rad  
##  -19.830234     3.184818     0.003712    -1.695735     0.306205  
##         tax      ptratio        black        lstat  
##   -0.011510    -0.984545     0.007409    -0.534384
```

Por otro lado, si quisieramos usarlas todas excepto alguna, podemos escribir:

```r
reg_lm3 <- lm(medv~.-age, data = train)
reg_lm3
```

```
## 
## Call:
## lm(formula = medv ~ . - age, data = train)
## 
## Coefficients:
## (Intercept)         crim           zn        indus         chas  
##   43.767647    -0.105721     0.044539    -0.033702     3.130323  
##         nox           rm          dis          rad          tax  
##  -19.551080     3.205457    -1.711176     0.305140    -0.011495  
##     ptratio        black        lstat  
##   -0.981650     0.007475    -0.530236
```








<!--chapter:end:03-RegresionLineal.Rmd-->

# Regresión Logística



<img src='http://ravanhami.com/wp-content/uploads/2017/10/LogReg_1.png' alt="Logistic Regression Model" style="float:width:95%;">


<br>
<img src='https://cdn.livechatinc.com/website/uploads/2016/04/customer-churn@2x.jpg' alt=" " style="float:right;width:50%;">

Una aplicación muy conocida son los modelos de **churn**. Un modelo de churn es una herramienta que permite evaluar la **probabilidad de baja o fuga** de un cliente en función de sus características propias y del tipo de relación que tiene con la empresa.</p>






<br>
<div class ="info">
La variable que se analiza toma valor 1 ó 0. Para representar la relación entre esa **variable binaria** (output) y las variables explicativas (inputs), se utilizan modelos de tipo <font **logit** o **probit**.
</div>


## Modelos Lineales Generalizados {.tabset}

Los Modelos Lineales Generalizados son una extensión de los  modelos lineales
clásicos. Un modelo lineal se basa en un vector de observaciones  $\mathbf{Y}$ con $n$ componentes,
que son una realización de una variable aleatoria $\mathbf{Y}$ cuyas componentes están independientemente distribuidas con media  $\mu$. Un modelo lineal puede ser descrito
como:
$$\mathbf{Y} = \mathbf{\mu} + \mathbf{\epsilon}$$

La parte sistemática de un modelo es una especificación para $\mu$ en función de un número pequeño de parámetros, $\beta_1, \ldots, \beta_p.$ Esa especificación se hace de la siguiente manera:
$$
\mu_i = \sum_{j=1}^p  X_{ij}\beta_j;     i=1,\ldots,n.
$$

O en forma matricial,
$$
E(\mathbf{Y}) = \mathbf{\mu} = \mathbf{x} \mathbf{\beta}
$$

donde $\mathbf{X}$ es una matriz $n \times p$, con las covariables o regresoras del modelo. Para la parte aleatoria se supone independencia y varianza constante de los errores.

En un modelo lineal clásico, se tiene que:
$$
\mathbf{\epsilon} \sim N(0, \sigma^2 \mathbf{I})
$$

Por tanto un modelo lineal clásico puede ser resumido de la forma:
$$
\begin{align}
        \mathbf{Y} & \sim N(\mathbf{\mu}, \sigma^2 \mathbf{I}) \\
        E(\mathbf{Y}) & = \mathbf{X}\mathbf{\beta} \\
        Var(\mathbf{Y}) & =  \sigma^2\mathbf{I}
\end{align}
$$

La generalización de los modelos lineales incluye una especificación de tres aspectos principales:
 
 * Las componentes de $\mathbf{Y}$ tienen distribución normal con varianza constante y son independientes.
 * En la parte sistemática, las covariables, $x_1, x_2, ..., x_p$, producen un predictor
lineal $\eta$, dado por:

$$ \eta= \sum_{j=1}^p  X_{ij}\beta_j $$

 * La relación entre los componentes sistemáticos y aleatorios se hace a través de una función de manera que:

$$\mathbf{\mu}=\mathbf{\eta}$$


Los Modelos Lineales Generalizados o MLGs permiten dos extensiones. La primera extensión está en la función de enlace, que es la parte del modelo que determina la relación entre la media de la variable respuesta y las covariables. Esta función de enlace, ahora, podrá ser cualquier función monótona diferenciable y generalmente denotada por $g(\mu)$. La segunda extensión reside en la distribución especificada para la componente aleatoria. En los MLGs esta puede ser de la familia exponencial, de la cual la distribución normal forma parte.

Se supone que si $\mathbf{Y}$ tiene una distribución de la familia exponencial para unos específicos $a(\cdot), b(\cdot)$ y $c(\cdot)$ se asume la siguiente forma:

$$
f_Y ( \mathbf{Y} | \eta,\phi) = \exp \left\{ \dfrac{\mathbf{\eta} - b(\eta)}{a(\phi)} + c(\mathbf{Y},\phi) \right\}
$$

El parámetro $\phi$ es llamado parámetro de dispersión y, si es conocido, llamamos a su familia ``de familia exponencial lineal de parámetro canónico $\theta$''. Utilizando la ecuación anterior y algunas relaciones, se puede obtener expresiones para la media y la varianza de $\mathbf{Y}$  de la siguiente manera:

$$
\begin{align}
        E(\mathbf{Y}) & = b'(\eta) \\
        Var(\mathbf{Y}) & =  a(\phi)b''(\eta)
\end{align}
$$


## Modelo Logit

<div class="rmdcomment">

Cuando la variable respuesta es continua, se utilizan métodos de regresión lineal o de otro tipo; en cambio, cuando la **variable respuesta es cualitativa** se utilizan los llamados **Modelos de Regresión Logística**.
 
</div> 
 
 
El objetivo de los  modelos de regresión logística es encontrar el mejor ajuste para describir las relaciones entre las variables respuesta (dicotómica o cualitativa) y un grupo de variables explicativas. Esta diferencia (respecto a los modelos con variable respuesta cuantitativa) da lugar a distintos modelos paramétricos y a distintas hipótesis para estos modelos, pero, una vez salvada esta diferencia, los métodos empleados en Regresión Logística siguen los principios generales de los métodos de Regresión Lineal.

La primera razón por la cual un Modelo de Regresión Lineal no es adecuado para este tipo de datos es que la **variable respuesta sólo puede tomar 2 valores (0 y 1)**, de modo que si pretendiésemos elaborar una relación entre una variable explicativa y esta, tendríamos que condicionar la probabilidad de alguno de los valores de la variable respuesta a cada valor de la variable explicativa, es decir
$E(Y= 1 | X = x_1)$ y obtendríamos la **curva logística**: 


<br>



![](04-RegresionLogistica_files/figure-latex/unnamed-chunk-2-1.pdf)<!-- --> 


<br>    
       
La relación existente no es lineal, sino que puede asociarse con la función de distribución de cierta variable aleatoria. Al utilizar la distribución Logística, representaremos la media de $Y$, dado un valor $x$ de la variable $X$, por $\pi(X)=E(Y/x)$.

El modelo de Regresión Logística es:
$$
\pi(x) = \frac{e^{\beta_0+\beta_1x}}{1+e^{\beta_0+\beta_1x}}
$$
o de forma equivalente:
$$ \pi(x) = \frac{1}{1 + e^{-(\beta_0 + \beta_1 x)}} $$ 


Aplicando la transformación Logit:
$$
g(x)=\ln\Big[\frac{\pi(x)}{1-\pi(x)}\Big]= \beta_0+\beta_1x
$$

Hemos llegado a $g(x)$, que tiene las propiedades que se desea que tenga un Modelo de Regresión Lineal; es lineal en sus parámetros; puede ser continua y su rango está entre $-\infty$ y $\infty$ dependiendo del rango de X.

Como hemos dicho antes, también tenemos que tener clara la distribución de la parte aleatoria de nuestro modelo. En el Modelo Lineal Generalizado suponemos que un valor de la variable dependiente puede expresarse como $y=E(Y/x)+\epsilon.$   Donde $\epsilon\sim N(0,\sigma^2)$, con varianza constante para los distintos niveles de la variable independiente. Pero esto no ocurre así en el caso de una variable dicotómica.


Si expresamos nuestro modelo como $Y = \pi(x) + \epsilon,$   $\epsilon$ toma dos posibles valores:

 
 * Si $Y=1,$ con probabilidad $\pi(x)$, $\epsilon= 1-\pi(x)$, con probabilidad $\pi(x)$.
 * Si $Y=0, \epsilon = -\pi(x)$, con probabilidad $1-\pi(x)$.


Por tanto:
$$
E(\epsilon)= (1-\pi(x))\pi(x)-\pi(x)(1-\pi(x))=0
$$

$$
V(\epsilon) = (1-\pi(x))^2\pi(x)-\pi(x))^2(1-\pi(x))=\pi(x)(1-\pi(x))
$$

La distribución de la variable dependiente $Y$, dado un valor de $x$ de la variable $X$, sigue una distribución Binomial con probabilidad $\pi(x)$.



<br>
<div class="info">
Al no ser una relación lineal, no es posible interpretar directamente el valor de los parámetros estimados.
Para ello se utilizan los _'ODDS Ratios'_.
</div>


A través de un ratio de ODDS se puede calcular qué influencia genera en el target el incremento de una unidad en el valor de la variable explicativa.

- Si $\beta_i > 0$ el efecto de la variable explicativa $X_i$ sobre la respuesta $Y$ es de incremento: aumenta la probabilidad del target.
- Si $\beta_i < 0$ el efecto que produce la variable explicativa $X_i$ sobre la respuesta $Y$ es decremento: disminuye la probabilidad del target.


<img src='https://plot.ly/~Diksha_Gabha/3187/y-vs-x.png' alt="Logistic Regression Model" style="float:width:95%;">



## Modelo Probit


<img src='http://d2r5da613aq50s.cloudfront.net/wp-content/uploads/415059.image0.jpg' alt="Logit and Probit" style="float:width:85%;">

La relación existente entre $E(Y= 1/X=x_1)$ y $X$, que como dicho anteriormente no es linea, se asocia también con la curva de distribución normal.Este enfoque utiliza la inversa de la función de distribución normal para obtener una relación lineal entre $E(Y= 1/X=x_1)$ y $X$. Y una vez hayamos tenido los valores en forma de relación lineal del tipo $g(x) = B_0+B_1x_1$ volveremos a transformarlo en una curva que se asemeje a una distribución aleatoria de la siguiente manera:

$$
E(Y=1/X) = \int_{-\infty}^{B_0+B_1 x_1}\frac{1}{\sqrt{2\pi}}e^{\frac{-z^2}{2}}
$$

Así estimando los valores de $B_0$ y $B_1$, siguiendo el proceso anteriormente mencionado, obtendremos estimaciones de las probabilidades de un determinado valor de la variable respuesta $(Y)$ condicionada a unos determinados valores de las variables explicativas $(X_1,X_2,...X_n)$.








<!-- ## Modelos de regresión logística -->

<!-- Como ocurría en la regresión lineal, el objetivo es tratar de explicar/predecir el comportamiento de una variable $Y$ en función de otras variables $X_1, X_2, ... , X_K$. -->

<!-- La regresión logística se plantea de la siguiente manera: -->

<!-- $$ Y = \frac{1}{1 + e^{-\beta_0 - \beta_1 X_1 - ... -\beta_K X_K + \epsilon}} $$ -->

<!-- Los valores que devuelven se mueven en el rango $[0,1]$ -->

<!-- En general, permite establecer una relación de dependencia entre una variable categórica $Y$ (no necesariamente binaria) con un conjunto de variables independiente de cualquier tipo. -->

<!-- En función de la naturaleza de la variable dependiente se distinguen diferentes tipos de modelos: -->

<!-- - **Regresión Logística Binaria**: asociada a un target binario. Es la más utilizada y referenciada. -->
<!-- - **Regresión Logística Ordinal**: asociado a un target ordinal. -->
<!-- - **Regresión Logística Nominal**: asociada a un target nominal.  -->



## Práctica en R

Para este ejemplo cargamos la librería `ISLR` y utilizamos el conjunto de datos de `Smarket`.



Veamos información sobre los datos `Smarket`

```r
?Smarket
```

```
## starting httpd help server ... done
```

```r
head(Smarket)
```

```
##   Year   Lag1   Lag2   Lag3   Lag4   Lag5 Volume  Today Direction
## 1 2001  0.381 -0.192 -2.624 -1.055  5.010 1.1913  0.959        Up
## 2 2001  0.959  0.381 -0.192 -2.624 -1.055 1.2965  1.032        Up
## 3 2001  1.032  0.959  0.381 -0.192 -2.624 1.4112 -0.623      Down
## 4 2001 -0.623  1.032  0.959  0.381 -0.192 1.2760  0.614        Up
## 5 2001  0.614 -0.623  1.032  0.959  0.381 1.2057  0.213        Up
## 6 2001  0.213  0.614 -0.623  1.032  0.959 1.3491  1.392        Up
```

```r
summary(Smarket)
```

```
##       Year           Lag1                Lag2          
##  Min.   :2001   Min.   :-4.922000   Min.   :-4.922000  
##  1st Qu.:2002   1st Qu.:-0.639500   1st Qu.:-0.639500  
##  Median :2003   Median : 0.039000   Median : 0.039000  
##  Mean   :2003   Mean   : 0.003834   Mean   : 0.003919  
##  3rd Qu.:2004   3rd Qu.: 0.596750   3rd Qu.: 0.596750  
##  Max.   :2005   Max.   : 5.733000   Max.   : 5.733000  
##       Lag3                Lag4                Lag5         
##  Min.   :-4.922000   Min.   :-4.922000   Min.   :-4.92200  
##  1st Qu.:-0.640000   1st Qu.:-0.640000   1st Qu.:-0.64000  
##  Median : 0.038500   Median : 0.038500   Median : 0.03850  
##  Mean   : 0.001716   Mean   : 0.001636   Mean   : 0.00561  
##  3rd Qu.: 0.596750   3rd Qu.: 0.596750   3rd Qu.: 0.59700  
##  Max.   : 5.733000   Max.   : 5.733000   Max.   : 5.73300  
##      Volume           Today           Direction 
##  Min.   :0.3561   Min.   :-4.922000   Down:602  
##  1st Qu.:1.2574   1st Qu.:-0.639500   Up  :648  
##  Median :1.4229   Median : 0.038500             
##  Mean   :1.4783   Mean   : 0.003138             
##  3rd Qu.:1.6417   3rd Qu.: 0.596750             
##  Max.   :3.1525   Max.   : 5.733000
```



En este caso la variable `Y` que queremos predecir/explicar es la variable `Direction`, y las variables independientes son `Lag1`, `Lag2`, `Lag3`, `Lag4`, `Lag5` y `Volume`.

Veamos que valores toma la variable `Direction` 

```r
levels(Smarket$Direction)
```

```
## [1] "Down" "Up"
```

Vemos que es una variable binaria que toma valores `Down`o `Up`.
Antes de continuar pasamos esos valores a `0` o `1`, respectivamente.

```r
Smarket$Direction <- ifelse(Smarket$Direction == 'Up', 1, 0)
```

Para realizar la regresión logística en R utilizaremos la función `glm`.
Se puede observar en el código siguiente, que como nuestro Target es binario, el parámetro `family` lo debemos fijar a binomial.


```r
reg_logis <- glm(Direction~Lag1 + Lag2 + Lag3 + Lag4 + Lag5 + Volume,
                 data = Smarket,
                 family = binomial)
```

Veamos que hemos obtenido

```r
summary(reg_logis)
```

```
## 
## Call:
## glm(formula = Direction ~ Lag1 + Lag2 + Lag3 + Lag4 + Lag5 + 
##     Volume, family = binomial, data = Smarket)
## 
## Deviance Residuals: 
##    Min      1Q  Median      3Q     Max  
## -1.446  -1.203   1.065   1.145   1.326  
## 
## Coefficients:
##              Estimate Std. Error z value Pr(>|z|)
## (Intercept) -0.126000   0.240736  -0.523    0.601
## Lag1        -0.073074   0.050167  -1.457    0.145
## Lag2        -0.042301   0.050086  -0.845    0.398
## Lag3         0.011085   0.049939   0.222    0.824
## Lag4         0.009359   0.049974   0.187    0.851
## Lag5         0.010313   0.049511   0.208    0.835
## Volume       0.135441   0.158360   0.855    0.392
## 
## (Dispersion parameter for binomial family taken to be 1)
## 
##     Null deviance: 1731.2  on 1249  degrees of freedom
## Residual deviance: 1727.6  on 1243  degrees of freedom
## AIC: 1741.6
## 
## Number of Fisher Scoring iterations: 3
```

Los coeficientes de la regresión logística obtenida serán:

```r
coef(reg_logis)
```

```
##  (Intercept)         Lag1         Lag2         Lag3         Lag4 
## -0.126000257 -0.073073746 -0.042301344  0.011085108  0.009358938 
##         Lag5       Volume 
##  0.010313068  0.135440659
```

Como se hace en los otros modelos, la función `predict` la utilizaremos para predecir un nuevo conjunto de datos a partir de nuestro modelo de regresión logística ajustado.

Para un modelo binomial predeterminado, las predicciones serán de log-odds (probabilidades en la escala logit). Como vemos en el código a continuación, utilizamos el argumento `type = response` para guardar la predicción de las probabilidades.

```r
glm.probs <- predict(reg_logis,
                     type =  "response")
```

Lo que hacemos a continuación es dar a una observación el valor del target $1$ o $0$ en función a la probabilidad obtenida. El corte en la probabilidad en este caso lo ponemos en $0.5$, es decir, si la predicción que se ha obtenido de la probabilidad es menor que 0.5, le damos el valor $0$, y sino el valor $1$. El código que hace esto es de la siguiente manera:

```r
glm.pred <- rep(1, nrow(Smarket))
glm.pred[glm.probs < .5] <- 0
```

Ahora, obtenemos la **matriz de confusión**, en el que podemos comparar el valor de la predicción obtenida (filas) con el verdadero valor (columnas). De esta manera, lo que está en la diagonal principal será lo que se ha predecido correctamente.

```r
table(glm.pred, Smarket$Direction) 
```

```
##         
## glm.pred   0   1
##        0 145 141
##        1 457 507
```

observamos la media de los valores que se ha predecido bien:

```r
mean(glm.pred == Smarket$Direction)
```

```
## [1] 0.5216
```
y la media de los que se han predecido mal:

```r
mean(glm.pred != Smarket$Direction)
```

```
## [1] 0.4784
```

Podemos ver de manera gráfica como han sido clasificados por nuestro modelo (en función de la probabilidad obtenida) frente a su valor real.


```r
nuevo <- data.frame(glm.probs, glm.pred, Smarket$Direction)
names(nuevo)[1] <- "probs"
names(nuevo)[2] <- "pred"
names(nuevo)[3] <- "direction"

nuevo$direction <- ifelse(nuevo$direction == 1, 'Up', 'Down')
nuevo$pred <- ifelse(nuevo$pred == 1, 'Up', 'Down')
library(ggplot2)
ggplot(data = nuevo, 
       aes(x = pred, y = probs, col = direction)) + geom_point() +
       labs(x = 'Prediccion', y = 'Probabilidades') +  
       ggtitle('Prediccion vs Valor Real') +
       theme(legend.title=element_blank()) +
       scale_colour_manual(values=c("blue", "red"))
```

![](04-RegresionLogistica_files/figure-latex/unnamed-chunk-14-1.pdf)<!-- --> 


### Otros ejemplos


  - [How to perform a Logistic Regression in R](https://www.r-bloggers.com/how-to-perform-a-logistic-regression-in-r/)

  - [Logit Regression | R Data Analysis Examples](https://stats.idre.ucla.edu/r/dae/logit-regression/)

  - [Practical Guide to Logistic Regression Analysis in R](
https://www.hackerearth.com/practice/machine-learning/machine-learning-algorithms/logistic-regression-analysis-r/tutorial/)

  - [Customer Churn – Logistic Regression with R](http://www.treselle.com/blog/customer-churn-logistic-regression-with-r/ )




<!--chapter:end:04-RegresionLogistica.Rmd-->

# Series Temporales








<br>

 > Ejemplo A:

![](05-SeriesTemporales_files/figure-latex/unnamed-chunk-2-1.pdf)<!-- --> 


<br>

 > Ejemplo B:



![](05-SeriesTemporales_files/figure-latex/unnamed-chunk-3-1.pdf)<!-- --> 


<br>
<br>

<div class="info">

Una **serie temporal** es una secuencia de datos, medidos a intervalos de tiempo sucesivos regularmente espaciados.

</div>


**Ejemplos** de series temporales son:

 - tasa de cambio diario, 
 - tasa de desempleo mensual, 
 - PIB trimestral, 
 - volumen de lluvia diario, 
 - etc.

 
<br>

<div class="rmdcomment">

Una de las características más importantes de una serie temporal es que las observaciones vecinas son generalmente dependientes. Así, mientras que en los modelos de regresión, por ejemplo, el orden de las observaciones es irrelevante para el análisis, en las series temporales **el orden de los datos es crucial**.

</div>
 
 


## ¿Qué es una Serie Temporal?

<div class="info">
Una **serie temporal** (o simplemente una serie) es una secuencia de $N$ observaciones ordenadas y equidistantes cronológicamente sobre una característica o varias características de una unidad observable en diferentes momentos. </div>



 * Si la serie es sobre una característica se dice que es univariante o **escalar**. 
 * Si la serie es sobre dos o más características se dice que es multivariante o **vectorial**.


El estudio de las series temporales permite: 

 - **entender** mejor el mecanismo de generación de los datos, que puede no ser claro inicialmente en una investigación y/o 
 - hacer pronósticos sobre el futuro, es decir: **previsiones**. 
 
 
Las previsiones se utilizan en forma constante en diversos campos: economía, finanzas, marketing, medio ambiente, ingeniería, etc. En general, las previsiones proporcionan una guía para las decisiones que deben tomarse.

Algunos ejemplos de uso de las previsiones son:

 * En **Planeamiento y Control de Operaciones**. Las decisiones de producción de un artículo con base en los pronósticos de ventas. Es posible por ejemplo, detectar una disminución en la tendencia de ventas que conlleve a reducir la producción, o al contrario.
 
 * En Marketing. La decisión de invertir en publicidad puede depender de prever las ventas.
 
 * En **Economía**. Las decisiones del Banco de España, por ejemplo para el control de la inflación, requieren la previsión y el examen del comportamiento de ciertas variables macroeconómicas, como el PIB, la tasa de desempleo, el IPC, las tasas de inter?s a distintos plazos, activas y pasivas.
 
 * En **Turismo**. La previsión del de número de turistas mensuales para determinar la demanda hotelera.
 
 * En **Epidemiología** y **Medio Ambiente**. La vigilancia de los niveles de contaminantes en el aire tiene como herramienta fundamental las series de tiempo. Pero adicionalmente el efecto de estos niveles sobre la salud.


<br>


<div class="rmdcomment">

Todas las series temporales tienen características particulares. Asi por ejemplo, las series pueden:

 * evolucionar alrededor de un **nivel** constante o tienen **tendencias** crecientes o decrecientes, 
 * evolucionar alrededor de un nivel que cambia sin seguir aparentemente un patrón concreto - tienen tendencia estocástica - 
 * presentar reducciones (en invierno) y aumentos (en verano) sistemáticos en su nivel cada 12 meses - son **estacionales** -
 * presentar variabilidad constante alrededor de su nivel
 * presentar variabilidad condicional o alta **volatilidad**,
 * moverse conjuntamente con otras series - tendencia común -
 * etc.
</div> 


<br>

 > Ejemplo: Caudal Anual del Río Nilo. 1871–1970. Anual.



\includegraphics[width=0.9\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/ts_rionilo} 


<br>

 > Ejemplo: España: Viviendas Iniciadas. Ene-1989/Jun-2012. Mensual. Miles de Viviendas




\includegraphics[width=0.9\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/ts_viviendas} 


<br>

 > Ejemplo: Madrid: Temperatura Media en el Parque del Retiro. Ene-1989/Dic-2011. Mensual. 



\includegraphics[width=0.9\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/ts_temperatura} 


En las secciones siguiente se describen brevemente algunos conceptos necesarios para la modelación básica de series temporales.



## Herramientas de Análisis

### Autocorrelación (acf y pacf)

 > Los **correlogramas** permiten representar las funciones de autocorrelación simple (fas) y parcial (fap).


<br>
<div class="info">
El coeficiente de **correlación simple** (y así la fas) refleja la correlación entre la variable $Y$ y el valor retardado de la misma en $k$ instantes anteriores (_lags_).
</div>

![](05-SeriesTemporales_files/figure-latex/unnamed-chunk-7-1.pdf)<!-- --> 





<br>
<div class="info">
El **coeficiente de correlación parcial (y así la fap) calcula la correlación directa eliminando posibles dependencias asociadas a retardos intermedios.
</div>



![](05-SeriesTemporales_files/figure-latex/unnamed-chunk-8-1.pdf)<!-- --> 



<br>
<div class="rmdcomment">
Los correlogramas permiten representar las _acf_ y _pacf_ que solo tienen sentido dentro del ámbito de los procesos estacionarios porque asumen que **la correlación entre dos valores de la serie sólo depende de su distancia**, no del instante del tiempo al que van referidos.
</div>




![](05-SeriesTemporales_files/figure-latex/unnamed-chunk-9-1.pdf)<!-- --> 



### Operadores (del Tiempo)


 > Operador de **Retardo** Simple

El **operador de retardo simple** se define como
$$Bz_t=z_{t-1}$$


Si aplicamos el operador de retardo dos veces:
$$BBz_t=Bz_{t-1}=z_{t-2}$$
Del mismo modo, si aplicamos $n$ veces el operador de retardo, obtenemos:
$$ BB \ldots Bz_t=z_{t-n} $$
Definimos, por tanto
$$ B^n z_t=z_{t-n} $$

 > Operador de **Adelanto** simple
 
De modo análogo, definimos el **operador de adelanto simple**
$$
\begin{align}
    Fz_t&=z_{t+1}\\
    F^n z_t&=z_{t+n}
\end{align}
$$

El operador $F$ es el inverso del operador $B$ ya que:
$$
FBz_t=BFz_t=z_t
$$
Por tanto, $BF=FB=1,$ lo que implica que $F=B^{-1}$.

 > Polinomios en $B$

Sea el polinomio en el operador de retardo $B$:
$$
\phi_0 - \phi_1 B - \ldots - \phi_pB^p
$$
La operación de este polinomio se define como:
$$
(\phi_0 - \phi_1 B - \ldots - \phi_pB^p)z_t=\phi_0z_t+\phi_1z_{t-1}+\ldots+\phi_pz_{t-p}
$$
Llamamos **polinomio autorregresivo** de orden $p$ al polinomio de grado $p$
$$
1-\phi_1B-\dots-\phi_pB^p
$$
La razón de esta nomenclatura es que si tenemos una serie cuyo comportamiento puede expresarse como
$$
(1-\phi_1B-\dots-\phi_pB^p)z_t=e_t
$$
donde $e_t$ es un término de error, la anterior expresión puede escribirse como:
$$
    z_t=\phi_1 z_{t-1}+ \ldots + \phi_p z_{t-p} + e_t
$$

Es decir, como una regresión donde la serie $z_t$ es el *output* y los propios retardos $1,2,\ldots,p$ de la variable actúan como *inputs* o regresores construyendo una **autorregresión**.

En muchas ocasiones emplearemos las formas $\phi(B), \psi(B), \varphi(B)$ u otras semejantes para denotar polinomios en $B$. Notaremos más adelante que asociaremos ciertas formas de expresar polinomios en $B$ como $\phi(B)$ a clases de polinomios en $B$ que juegan cierto papel especial. Por ejemplo, reservaremos la expresi?n $\phi(B)$ a polinomios autorregresivos.

 > Operador **Diferencia**

El operador diferencia respecto al pasado, en lo sucesivo simplemente **operador diferencia**, se define como:
$$
\bigtriangledown z_t = z_t - z_{t-1},
$$
que puede expresarse como:
$$
\bigtriangledown z_t = z_t - z_{t-1},
$$
que puede expresarse como
$$
(1-B)z_t=\bigtriangledown z_t.
$$
Por lo tanto: $\bigtriangledown =1-B$.
El operador de \textbf{diferencia peri?dica}, usualmente **diferencia estacional**, se define como
$$
\bigtriangledown_s z_t=z_t-z_{t-s}=(1-B^s)z_t.
$$
Luego, $\bigtriangledown_s=(1-B^s).$

Debe observarse que cuando aplicamos el operador $B$ a una serie $S$ lo que hacemos en realidad es **adelantar** la serie un periodo. Análogamente, cuando aplicamos el operador $F$ a una serie $S$ **retrasamos** la serie un periodo.




## Alisado Exponencial


  <br>
  <div class="info">
  El alisado exponencial es una técnica aplicada a series de tiempo, para **suavizarlas** u obtener previsiones.
  </div>
 
 * Mientras que, con la media móvil, las observaciones pasadas se ponderan por igual, en el alisado exponencial se asignan ponderaciones exponencialmente decrecientes en el tiempo.
 
 * La fórmula utilizada es:
 
   $$ y_1 = x_0  $$
   $$ y_t = (1-\theta)x_{t-1}+\theta y_{t-1},  t > 1 $$

donde $\{x_t\}$ son las observaciones reales, $\{y_t\}$ son las estimaciones y  $\theta$ es el factor de alisamiento, $0 < \theta < 1$.
        
En otras palabras, con este método, la previsión para el periodo $t$ (valor esperado) como la suma ponderada de todas la observaciones anteriores, dando mayor importancia a las observaciones más recientes que a las más antiguas. Como puede verse en:
 
$$ y_t = (1-\theta) x_{t-1} +\theta y_{t-1} $$ 
$$ y_t = (1-\theta)x_{t-1}+(1-\theta)\theta x_{t-2}+(1-\theta) \theta^2 y_{t-2} $$
$$ y_t = (1-\theta)[x_{t-1}+\theta x_{t-2}+\theta x_{t-3}+\theta x_{t-4}+ ...] + \theta^{t-1} x_0 $$ 
Así, los pesos asignados a las observaciones previas pertenecen a una proporción de la progresión geométrica: $\{1, \theta, \theta^2, \theta^3, ..\}$.
            
 * Por otro lado, si la ecuación arriba se expresa como: 

$$
                y_t = x_{t-1} + \theta(y_{t-1} - x_{t-1}) ,  
$$
            
Se aprecia que $y_t$ está formada por la suma de la observación en el periodo anterior ($x_{t-1}$) más una proporción ($\theta$) del error cometido ($y_{t-1} - x_{t-1}$). Por lo tanto el valor de $\theta$ controla la rapidez con que la previsión se adapta a los cambios del nivel de la serie (estado).
            
 * Si $\theta$ es grande (próximo a 1), la previsión se adapta rápidamente a los cambios, por lo tanto se debe utilizar en series poco estables.
 * Si $\theta$ es pequeño (próximo a 0), se consigue eliminar el efecto de las fluctuaciones, por lo tanto se debe utilizar en series estables.
 * El valor de $\theta$ se puede optimizar minimizando la suma de cuadrados del error de previsión, es decir, resolviendo: $min(x_{t-1} - y_{t-1})^2$.
 * El alisado exponencial, técnicamente, es equivalente a un modelo *ARIMA (0,1,1)* sin constante. En otras palabras, se puede representar por:

$$\hat{y} = (1-\theta)(1 + \theta B + \theta^2 B^2 + \theta^3 B^3 + ...)x_{t-1}$$



donde $B$ es el operador retardo y $\theta$ es el parámetro de amortiguamiento. Esta representación no implica recargar el último término con un peso mayor a los valores más recientes.

Si existe un número finito de periodos observados, la ecuación anterior se reescribe como:
            
$$ \hat{y} = \alpha (1 + \theta B + \theta^2 B^2 + ... + \theta^p B^p)x_{t-1}$$
donde $p$ es el número de periodos disponibles y $\alpha <1 $ es un término que asegura que los coeficientes de la ecuación sumen la unidad. Eso permite que el peso relativo de cada uno de los datos del pasado se mantenga constante y, al mismo tiempo, el resultado siga siendo una media.
            
 * En la tabla abajo  se muestran los pesos que toman los términos, en el caso de contar con 6.

 |                   | I    | II      | III       | IV      | V   |
 | :-------------             |:----------:| ----------:| ----------:| ----------:| ----------:|
 | $\theta$                   | 0.70      | 0.65       | 0.60       | 0.55        | 0.50    |
 | $(1- \theta)$              | 0.30      | 0.35      | 0.40       | 0.45        | 0.50    |
 | $(1- \theta)\theta$       | 0.21      | 0.23      |    0.24      |    0.25   |    0.25    |
 | $(1- \theta)\theta^2$     | 0.15      | 0.15      |    0.14      |    0.14   |    0.13    |
 | $(1- \theta)\theta^3$     | 0.10      | 0.10      |    0.09      |    0.07   |    0.06    |
 | $(1- \theta)\theta^4$     | 0.07      | 0.06      |    0.05      |    0.04   |    0.03    |
 | $(1- \theta)\theta^5$     | 0.05      | 0.04      |    0.03      |    0.02   |    0.02    |
 
 
 
 
 
<!-- ------- -->

## ARIMA


 > Un proceso estocástico es un mecanismo generador de un número aleatorio de series. Una serie temporal es una realización particular de un proceso estocástico.
 



\includegraphics[width=0.9\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/ts_procesoEstocastico.png } 

El objetivo que se plantea es **inferir el proceso estocástico que ha generado el conjunto de observaciones** que definen la serie temporal.


Para caracterizar un proceso estocástico $F(y(t_1),\ldots,y(t_N))$, se requiere de la distribución conjunta de $F(y_t), \forall t$, las distribuciones marginales  $F(y_t,y_{t+1}), \forall t$, etc.

 - **Problema**. Como sólo se dispone de una observación por instante temporal, no es posible obtener dichas distribuciones.
 
 - **Solución**. Asumir qe las distribuiones son estables (estacionarias) en el tiempo para que las distribuciones asociadas a diferentes instantes sean comparables.
 
 > Un proceso es **estacionario** en sentido estricto si el comportamiento de una colección de variables aleatórias sólo depende de su posición relativa, no del instante $t$.
 
 
 <div class="rmdcomment">
 Dada una serie temporal, el objetivo es hacerla estacionaria para asumir esa estabilidad que permita hacer que todos los instantes sean comparables.
 </div>
 
 
Una vez que el proceso (serie) es estacionario, se busca algún tipo de modelo adecuado para su caracterización: **los procesos ARMA son modelizables mediante modelos ARMA**.


$$ARMA(p,q) = (1-\phi_1 B - \ldots - \phi_p B^p)X_t = (1- \theta_1 B - \ldots - \theta_q B^q)a_t$$

### Parte AR (Autorregresiva)

 > La parte autogresica del modelos muestra la dependencia del dato real con su propio pasado. Se trata de una regresión de la variable en sí misma (autoregresión).
 
 $$AR(p): X_t= \mu_t + \phi_1X_{t-1} + \ldots + \phi_1X_{t-p} + a_t$$
 



### Parte MA (Medias Móviles)

 > La parte de medias moviles muestra la dependencia del dato real con el pasado del proceso de error (media móvil  de la serie de los errores)

Los procesos $MA$ siempre son estacionarios.


$$MA(q): X_t= \mu - \theta_1 a_{t-1} - \ldots - \theta_q a_{t-q} + a_t$$

<br>
<div class="rmdcomment">
Se requiere identificar el proceso que buyace bajo los datos, lo cual consiste en **identicar los órdenes** $p$ y $q$ del modelo ARMA que generó la serie temporal.
</div>

Las herramientas para identificar esos procesos son las funciones de autocorrelación simple y parcial.

> Ejemplo AR(2): $Y_t = 0.6Y_{t-1}+0.2Y_{t-2}+A_t$


\includegraphics[width=0.95\linewidth]{05-SeriesTemporales_files/figure-latex/unnamed-chunk-11-1} 
\includegraphics[width=0.95\linewidth]{05-SeriesTemporales_files/figure-latex/unnamed-chunk-11-2} 
\includegraphics[width=0.95\linewidth]{05-SeriesTemporales_files/figure-latex/unnamed-chunk-11-3} 

> Ejemplo MA(2): $X_t=A_t-0.6A_{t-1}-0.2A_{t-2}$


\includegraphics[width=0.95\linewidth]{05-SeriesTemporales_files/figure-latex/unnamed-chunk-12-1} 
\includegraphics[width=0.95\linewidth]{05-SeriesTemporales_files/figure-latex/unnamed-chunk-12-2} 
\includegraphics[width=0.95\linewidth]{05-SeriesTemporales_files/figure-latex/unnamed-chunk-12-3} 

> Ejemplo ARIMA(1,1):$Y_t = -0.8 Y_{t-1} + A_t -0.8A_{t-1}$


\includegraphics[width=0.9\linewidth]{05-SeriesTemporales_files/figure-latex/unnamed-chunk-13-1} 
\includegraphics[width=0.9\linewidth]{05-SeriesTemporales_files/figure-latex/unnamed-chunk-13-2} 




## Funciones de Transferencia

Algunos ejemplos^[Fuente de los gráficos](https://www.xycoon.com/tf_identification.htm):

<br>



\includegraphics[width=0.9\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/tf_ide51} 


\includegraphics[width=0.9\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/tf_ide52} 



\includegraphics[width=0.9\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/tf_ide53} 




\includegraphics[width=0.9\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/tf_ide54} 




\includegraphics[width=0.9\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/tf_ide55} 







## Práctica en R

Un ejemplo sencillo sobre el manejo de series temporales que puede realizarse con algunos paquetes de R.

 > Datos

Los datos utilizados corresponden a las **ventas mensuales** para una tienda de souvenirs en un balneario de Queensland, Australia, de enero de 1987 a diciembre de 1993 (Ver [aquí](http://robjhyndman.com/tsdldata/data/fancy.dat). Datos originales de Wheelwright y Hyndman, 1998).


![](05-SeriesTemporales_files/figure-latex/unnamed-chunk-19-1.pdf)<!-- --> 



 > Librerías

A lo largo de esta práctica se utilizan las siguientes librerías: 

 * XTS: *eXtensible Time Series* y
 * HIGHCHARTER: *a R wrapper for Highcharts javascript libray and its modules. *. 

Los manual de usuario de ambos paquetes esta disponibles en la siguientes enlaces: [Manual de XTS](http://cran.r-project.org/web/packages/xts/xts.pdf) y [Manual de HIGHCHARTER](https://cran.r-project.org/web/packages/highcharter/highcharter.pdf)

Para instalar un paquete de R, se puede usar el comando: `install.packages("nombre del paquete")`. Por ejemplo, `install.packages("xts")`. De forma alternativa:


```r
# Get xts
if (!require("xts")) {install.packages("xts");    library(xts)}
# Get highcharter
if (!require("highcharter")) {install.packages("highcharter");    library(highcharter)}
# Get tseries
if (!require("tseries")) {install.packages("tseries");    library(tseries)}
```


 > Lectura y Visualización


<!-- Definimos la carpeta de trabajo y leemos el fichero de los datos con la función `read.zoo`. En este ejemplo, los datos están en el fichero `ClientesTotalesXTS.csv` que tiene formato `CSV`. Leer los datos de esta manera tiene como ventaja que las fechas son reconocidas como tal. Al usar una función como `read.csv` entienden las fechas como textos y no siempre son bien entendidas por las funciones que necesitan series temporales como input. -->

Obtenemos los datos en el site de la _Time Series Data Library_ (TSDL). (Los datos están [aquí](http://robjhyndman.com/tsdldata/data/fancy.dat). 


```r
datos = scan("http://robjhyndman.com/tsdldata/data/fancy.dat")
datos = log(datos) #transformación opcional

head(datos,5) #primeros 5 datos
```

```
FALSE [1] 7.41747 7.78219 7.95181 8.17394 8.23030
```

```r
tail(datos,5) #últimos 5 datos
```

```
FALSE [1] 10.2607 10.3257 10.3360 10.7501 11.5585
```

 >  Objeto XTS
 
La forma más conocida para la creación de un objeto de la clase serie temporal, es el uso de la función `ts`. En este ejemplo, creamos la serie temporal `sales.ts` a partir de `datos`.




Sin embargo, la manipulación de la serie es bastante más natural y amigable utilizando un objeto de la clase `xts`. En este ejemplo se crea el objeto `sales` a partir de `datos` y se hacen consultas básicas sobre su contenido (fechado, primer dato, últimas semanas, número de semanas en la muestra, etc.)



```r
sales = as.xts(sales.ts) #creación del objeto XTS
is.xts(sales) #debe devolver TRUE
```

```
FALSE [1] TRUE
```

```r
periodicity(sales) #fechado de los datos
```

```
FALSE Monthly periodicity from ene. 1987 to dic. 1993
```

```r
first(sales) #primer dato
```

```
FALSE              [,1]
FALSE ene. 1987 7.41747
```

```r
last(sales) #último dato
```

```
FALSE              [,1]
FALSE dic. 1993 11.5585
```

```r
first(sales, '7 months') #primeros 7 dias
```

```
FALSE              [,1]
FALSE ene. 1987 7.41747
FALSE feb. 1987 7.78219
FALSE mar. 1987 7.95181
FALSE abr. 1987 8.17394
FALSE may. 1987 8.23030
FALSE jun. 1987 8.22006
FALSE jul. 1987 8.37784
```

```r
last(sales, '2 quarters') #últimas dos semanas
```

```
FALSE              [,1]
FALSE jul. 1993 10.1718
FALSE ago. 1993 10.2607
FALSE sep. 1993 10.3257
FALSE oct. 1993 10.3360
FALSE nov. 1993 10.7501
FALSE dic. 1993 11.5585
```

```r
nmonths(sales) #número de meses en la muestra
```

```
FALSE [1] 84
```

```r
nquarters(sales) #número de trimestres en la muestra
```

```
FALSE [1] 28
```

```r
nyears(sales) #número de años en la muestra
```

```
FALSE [1] 7
```

> Selección de datos usando las fechas

Una funcionalidad interesante es la obtención de sub-muestras, utilizando la(s) fecha(s) como criterio(s) de selección.

```r
sales['1990-01-01/1990-05-01'] #todos los datos del 01 al 05 de Febrero de 1990
```

```
FALSE              [,1]
FALSE ene. 1990 8.68628
FALSE feb. 1990 8.66812
FALSE mar. 1990 9.42716
FALSE abr. 1990 8.75932
FALSE may. 1990 8.93710
```

```r
first(sales['1991'], '5 month') #primeros 5 meses desde Febrero de 1989
```

```
FALSE              [,1]
FALSE ene. 1991 8.48191
FALSE feb. 1991 8.77497
FALSE mar. 1991 9.17355
FALSE abr. 1991 9.08491
FALSE may. 1991 9.07365
```

```r
last(sales['1990'], '1 quarter') #datos último mes de 1990
```

```
FALSE               [,1]
FALSE oct. 1990  9.04508
FALSE nov. 1990  9.79337
FALSE dic. 1990 10.31276
```

```r
rbind(sales['1987-10/1988-03'],sales['1988-10/1989-03']) #todos los datos del 01 al 05 de Febrero de 2011 y 2012
```

```
FALSE               [,1]
FALSE oct. 1987  8.76772
FALSE nov. 1987  8.93598
FALSE dic. 1987  9.89122
FALSE ene. 1988  7.82397
FALSE feb. 1988  8.55608
FALSE mar. 1988  8.88532
FALSE oct. 1988  8.67165
FALSE nov. 1988  9.44146
FALSE dic. 1988 10.25912
FALSE ene. 1989  8.45893
FALSE feb. 1989  8.64868
FALSE mar. 1989  9.20609
```



 > Cambios de Fechado
 
El cambio de fechado o periodicidad es una operación muy útil durante el trabajo con series temporales. En este ejemplo, como la variable analizada corresponde a las ventas mensuales, se utiliza la función `sum` para obtener las ventas trimestrales y anuales.


```r
sales.qua=apply.quarterly(sales, sum) # datos trimestrales
 first(sales.qua, '3 quarters')
```

```
FALSE              [,1]
FALSE mar. 1987 23.1515
FALSE jun. 1987 24.6243
FALSE sep. 1987 25.0787
```

```r
sales.yea=apply.yearly(sales, sum) # datos anuales
 first(sales.yea, '3 years')
```

```
FALSE              [,1]
FALSE dic. 1987 100.449
FALSE dic. 1988 105.113
FALSE dic. 1989 108.676
```


 > Imputación de Datos Faltantes
 
La falta de algunos datos y/o la presencia de datos errones suele tratarse con procedimientos de imputación - para no perder histórico de la muestra disponible. El paquete `xts` posee funciones que permiten extender hacia adelante o hacia atrás, valores observados en la misma serie temporal.


```r
aux=sales['1990-01-01/1990-03-01']
sales['1990-01-01/1990-03-01']=NA
sales['1990-01-01/1990-03-01']
```

```
FALSE           [,1]
FALSE ene. 1990   NA
FALSE feb. 1990   NA
FALSE mar. 1990   NA
```

```r
(isna=which(is.na(sales))) #identifica las líneas con NA
```

```
FALSE [1] 37 38 39
```

```r
sales.na01=na.locf(sales) #repite el ultimo anterior a NA
 sales.na01[isna,]
```

```
FALSE              [,1]
FALSE ene. 1990 10.4359
FALSE feb. 1990 10.4359
FALSE mar. 1990 10.4359
```

```r
sales.na02=na.locf(sales, fromLast=TRUE) # repite el primero despues de NA
 sales.na02[isna,]
```

```
FALSE              [,1]
FALSE ene. 1990 8.75932
FALSE feb. 1990 8.75932
FALSE mar. 1990 8.75932
```

```r
sales.na03=na.locf(sales, na.rm=TRUE, fromLast=TRUE)
 sales.na03[isna,]
```

```
FALSE              [,1]
FALSE ene. 1990 8.75932
FALSE feb. 1990 8.75932
FALSE mar. 1990 8.75932
```

```r
sales[isna,]=aux
```

 > Estadísticos en diferentes fechados
 
El paquete `xts`permite trabajar con series de estadísticos en fechado agregado. Por ejemplo, el máximo del mes, el mínimo del trimestre, etc. El ingrediente indispensable es el vector que indica los puntos de quiebre de la serie. Este vector se obtiene con la función `endpoints`.


```r
aux.qq=endpoints(sales,"quarters") #indica los finales de trimestre
par(mfrow=c(1,3), cex.lab=0.8,cex.axis=0.8,las=2)
plot(period.sum(sales,aux.qq), main="Total") 
plot(period.min(sales,aux.qq), main="Mínimo")
plot(period.max(sales,aux.qq), main="Máximo")
```

![](05-SeriesTemporales_files/figure-latex/unnamed-chunk-27-1.pdf)<!-- --> 

```r
aux.yy=endpoints(sales,"years") #indica los finales de año
par(mfrow=c(1,3),cex.lab=0.8,cex.axis=0.8,las=2)
plot(period.sum(sales,aux.yy), main="Total")
plot(period.min(sales,aux.yy), main="Mínimo")
plot(period.max(sales,aux.yy), main="Máximo")
```

![](05-SeriesTemporales_files/figure-latex/unnamed-chunk-27-2.pdf)<!-- --> 

 > División del conjunto de datos usando fechas

Otra funcionalidad útil es `split`. Permite dividir el objeto original en sub-conjuntos, teniendo en cuenta un fechado y un horizonte. En este caso, se divide el objeto `sales` en sub-muestras de 4 meses cada una.

```r
sales.by.4months=split(sales, f="months",k=4) 
sales.by.4months
```

```
## [[1]]
##              [,1]
## ene. 1987 7.41747
## feb. 1987 7.78219
## mar. 1987 7.95181
## abr. 1987 8.17394
## 
## [[2]]
##              [,1]
## may. 1987 8.23030
## jun. 1987 8.22006
## jul. 1987 8.37784
## ago. 1987 8.17930
## 
## [[3]]
##              [,1]
## sep. 1987 8.52155
## oct. 1987 8.76772
## nov. 1987 8.93598
## dic. 1987 9.89122
## 
## [[4]]
##              [,1]
## ene. 1988 7.82397
## feb. 1988 8.55608
## mar. 1988 8.88532
## abr. 1988 8.47763
## 
## [[5]]
##              [,1]
## may. 1988 8.68286
## jun. 1988 8.50741
## jul. 1988 8.72893
## ago. 1988 8.46635
## 
## [[6]]
##               [,1]
## sep. 1988  8.61185
## oct. 1988  8.67165
## nov. 1988  9.44146
## dic. 1988 10.25912
## 
## [[7]]
##              [,1]
## ene. 1989 8.45893
## feb. 1989 8.64868
## mar. 1989 9.20609
## abr. 1989 8.57636
## 
## [[8]]
##              [,1]
## may. 1989 8.77839
## jun. 1989 8.79948
## jul. 1989 8.90240
## ago. 1989 9.00903
## 
## [[9]]
##               [,1]
## sep. 1989  9.05639
## oct. 1989  9.17890
## nov. 1989  9.62588
## dic. 1989 10.43591
## 
## [[10]]
##              [,1]
## ene. 1990 8.68628
## feb. 1990 8.66812
## mar. 1990 9.42716
## abr. 1990 8.75932
## 
## [[11]]
##              [,1]
## may. 1990 8.93710
## jun. 1990 8.88527
## jul. 1990 9.00224
## ago. 1990 8.98460
## 
## [[12]]
##               [,1]
## sep. 1990  8.99876
## oct. 1990  9.04508
## nov. 1990  9.79337
## dic. 1990 10.31276
## 
## [[13]]
##              [,1]
## ene. 1991 8.48191
## feb. 1991 8.77497
## mar. 1991 9.17355
## abr. 1991 9.08491
## 
## [[14]]
##              [,1]
## may. 1991 9.07365
## jun. 1991 9.23107
## jul. 1991 9.33048
## ago. 1991 9.43765
## 
## [[15]]
##               [,1]
## sep. 1991  9.36198
## oct. 1991  9.51833
## nov. 1991  9.99068
## dic. 1991 10.71577
## 
## [[16]]
##              [,1]
## ene. 1992 8.93788
## feb. 1992 9.19520
## mar. 1992 9.58592
## abr. 1992 9.35767
## 
## [[17]]
##              [,1]
## may. 1992 9.14126
## jun. 1992 9.47900
## jul. 1992 9.72512
## ago. 1992 9.89790
## 
## [[18]]
##              [,1]
## sep. 1992 10.0830
## oct. 1992 10.1422
## nov. 1992 10.4920
## dic. 1992 11.2988
## 
## [[19]]
##              [,1]
## ene. 1993 9.23437
## feb. 1993 9.32962
## mar. 1993 9.99090
## abr. 1993 9.76177
## 
## [[20]]
##               [,1]
## may. 1993  9.68021
## jun. 1993  9.83100
## jul. 1993 10.17180
## ago. 1993 10.26069
## 
## [[21]]
##              [,1]
## sep. 1993 10.3257
## oct. 1993 10.3360
## nov. 1993 10.7501
## dic. 1993 11.5585
```

```r
 #divide el conjunto de datos en partes de 4 meses cada una
summary(sales.by.4months) #indica el número de elemento que hay en cada parte
```

```
##       Length Class Mode   
##  [1,] 4      xts   numeric
##  [2,] 4      xts   numeric
##  [3,] 4      xts   numeric
##  [4,] 4      xts   numeric
##  [5,] 4      xts   numeric
##  [6,] 4      xts   numeric
##  [7,] 4      xts   numeric
##  [8,] 4      xts   numeric
##  [9,] 4      xts   numeric
## [10,] 4      xts   numeric
## [11,] 4      xts   numeric
## [12,] 4      xts   numeric
## [13,] 4      xts   numeric
## [14,] 4      xts   numeric
## [15,] 4      xts   numeric
## [16,] 4      xts   numeric
## [17,] 4      xts   numeric
## [18,] 4      xts   numeric
## [19,] 4      xts   numeric
## [20,] 4      xts   numeric
## [21,] 4      xts   numeric
```


 > Otras operaciones con los datos

Finalmente, se presenta un ejemplo de una serie obtenida a partir del uso de operaciones básicas de R como `diff` y `log`.

```r
sales.inc <- diff(log(sales), lag = 1) #Tasa de incremento mensual
sales.inc <- sales.inc[-1] #Eliminamos el primer dato por ser NA
par(mfrow=c(1,1),cex.lab=0.8,cex.axis=0.8,las=2)
plot(sales.inc, main = "Nuevas Ventas",
     col = "grey", xlab = "Date", ylab = "Variación", major.ticks='years',
     minor.ticks=FALSE)
```

![](05-SeriesTemporales_files/figure-latex/unnamed-chunk-29-1.pdf)<!-- --> 


 > Gráficos con xts

Los gráficos de objetos `xts` son bastante más visuales o legibles que los objetos `ts`. La principal diferencia está en el reconocimiento de las fechas y su visualización en el eje horizontal.


```r
par(mfrow=c(1,1),cex.lab=0.8,cex.axis=0.8,las=2)
plot(sales, main = "Ventas Mensuales",
     col = innCol[1],xlab = "Date", ylab = "Ventas", major.ticks='quarters',
     minor.ticks=FALSE)
```

![](05-SeriesTemporales_files/figure-latex/unnamed-chunk-30-1.pdf)<!-- --> 

 > Gráficos con  highcharter

Los gráficos generados con [highcharter](http://jkunst.com/highcharter/index.html) utilizan la biblioteca [Highcharts](http://www.highcharts.com/demo)


```r
highchart() %>% 
  hc_chart(type="line",zoomType="x")%>%
  hc_title(text = "Ventas Mensuales") %>%
  hc_subtitle(text = "Gráfico Tipo Línea") %>% 
  hc_legend(enabled = T) %>%
  hc_tooltip(valueDecimals= 2,shared=T, crosshairs=T) %>%
  hc_xAxis(type = 'datetime',
           tickInterval=10,
           labels = list(format = '{value:%m-%Y}',rotation=-90)) %>% 
  hc_add_series(data=sales.ts, 
                name = "Ventas",
                color = innCol[1],
                lineWidth= 1) %>%
  hc_credits(enabled = TRUE, # add credits
             text = "Elaborado por Innova-tsn",
             href = "https://www.innova-tsn.com") %>% 
  hc_exporting(enabled = TRUE)
```

![](05-SeriesTemporales_files/figure-latex/unnamed-chunk-31-1.pdf)<!-- --> 

<br>






```r
hc <- highchart(type="stock") %>%
  hc_title(text = "Ventas Mensuales") %>%
  hc_subtitle(text = "Gráfico tipo Stock") %>% 
  hc_legend(enabled = T) %>%
  hc_tooltip(valueDecimals= 0) %>%
  hc_add_series(data=sales.ts, 
                name = "Ventas",
                color = innCol[1]) %>%
  hc_credits(enabled = TRUE, # add credits
             text = "Elaborado por Innova-tsn",
             href = "https://www.innova-tsn.com")
hc
```

![](05-SeriesTemporales_files/figure-latex/unnamed-chunk-32-1.pdf)<!-- --> 


> Tendencia (anual)



```r
plot(sales.ts,main="Venta", type="l")
abline(lm(sales.ts ~ time(sales.ts)),col="#9d9fa0",lty=3)
```

![](05-SeriesTemporales_files/figure-latex/unnamed-chunk-33-1.pdf)<!-- --> 



> Tendencia (anual)



```r
plot(aggregate(sales.ts,FUN=mean),main="Venta media por Año", type="h")
```

![](05-SeriesTemporales_files/figure-latex/unnamed-chunk-34-1.pdf)<!-- --> 


> Estacionalidad


```r
boxplot(sales.ts~cycle(sales.ts), main="Ventas por Mes")
```

![](05-SeriesTemporales_files/figure-latex/unnamed-chunk-35-1.pdf)<!-- --> 





> Variacion anual (diferencia)


```r
sales.ts.diff = diff(sales.ts, differences = 12)
plot(sales.ts.diff)
```

![](05-SeriesTemporales_files/figure-latex/unnamed-chunk-36-1.pdf)<!-- --> 






> Test de Estacionariedad


```r
adf.test(diff(sales.ts,12), alternative="stationary", k=0)
```


	Augmented Dickey-Fuller Test

data:  diff(sales.ts, 12)
Dickey-Fuller = -4.705, Lag order = 0, p-value = 0.01
alternative hypothesis: stationary


> ACF


```r
acf(sales.ts.diff)
```

![](05-SeriesTemporales_files/figure-latex/unnamed-chunk-38-1.pdf)<!-- --> 

```r
pacf(sales.ts.diff)
```

![](05-SeriesTemporales_files/figure-latex/unnamed-chunk-38-2.pdf)<!-- --> 

> Ajuste Modelo ARIMA



```r
(fit <- arima(sales.ts, c(1, 1, 0),seasonal = list(order = c(0, 1, 1), period = 12)))
```


Call:
arima(x = sales.ts, order = c(1, 1, 0), seasonal = list(order = c(0, 1, 1), 
    period = 12))

Coefficients:
         ar1    sma1
      -0.502  -0.511
s.e.   0.101   0.154

sigma^2 estimated as 0.0311:  log likelihood = 20.49,  aic = -34.99

> Previsión



```r
pred <- predict(fit, n.ahead = 5*12)
ts.plot(sales.ts,pred$pred, log = "y", lty = c(1,3), main="Previsión 5 años")
```

![](05-SeriesTemporales_files/figure-latex/unnamed-chunk-40-1.pdf)<!-- --> 



### Ver también

 - [Using R for Time Series Analysis](http://a-little-book-of-r-for-time-series.readthedocs.io/en/latest/src/timeseries.html#selecting-a-candidate-arima-model)
 
 - [TSA: Start to Finish Examples](https://rpubs.com/ryankelly/ts6)
 
 - [A Complete Tutorial on Time Series Modeling in R](https://www.analyticsvidhya.com/blog/2015/12/complete-tutorial-time-series-modeling/)
 
 - [Business Science Demo Week](http://www.business-science.io/code-tools/2017/10/23/demo_week_tidyquant.html)
 
 -  [TimeKit: Time Series Forecast Applications Using Data Mining](http://www.business-science.io/code-tools/2017/05/02/timekit-0-2-0.html)


 - [Time Series Data Library](https://datamarket.com/data/list/?q=provider:tsdl)

## A manera de nota final

<br>

<img src='http://i.imgur.com/aoz1BJy.jpg' alt="Data Science Skills" style="float:width:90%;">

[Ver Fuente de la imagen](http://berkeleysciencereview.com/how-to-become-a-data-scientist-before-you-graduate/)

<!--chapter:end:05-SeriesTemporales.Rmd-->

# Aprendizaje Supervisado




Los modelos de la estadística tradicional (regresiones lineales, por ejemplo) suelen ser poco flexibles por su naturaleza paramétrica, es decir, estos modelos se construyen partiendo de u
nas hipótesis y que, si estas no se cumplen, el modelo falla estrepitosamente. 

Por ejemplo, una regresión lineal supone que la estructura de los datos sigue una tendencia lineal. 

![](07-SupervisedLearning_files/figure-latex/unnamed-chunk-1-1.pdf)<!-- --> 

Si la estructura de los datos no sigue la hipótesis de linealidad, el modelo lineal es inservible en este caso

![](07-SupervisedLearning_files/figure-latex/unnamed-chunk-2-1.pdf)<!-- --> 

Los métodos propios del machine learning intentan ser **métodos flexibles** que permitan adaptarse a estructuras sin imponer hipótesis rígidas. 

Una de las ideas más potentes y que más éxito ha tenido es la de los **modelos ensamblados** Estos modelos consisten en utilizar algún tipo de modelo que a priori pueda ser débil (como un árbol de decisión) y *ensamblar* distintos modelos con algún tipo de modificación en el que cada uno enfatice alguna característica.

Los modelos ensamblados (*ensemble learning*) que más uso tienen son:

 - Bagging (bootstrap aggregating)
 - Random Forest
 - Boosting

Estos tres modelos utilizan los árboles de decisión como algoritmo base, así que primero vamos a hablar sobre ellos.

## Árboles de decisión


\includegraphics[width=0.75\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/CartHastie} 

Los **árboles de decisión** particionan el espacio en un conjunto de rectángulos y ajustan un modelo simple (como una constante) en cada uno de ellos.


Formalmente, un árbol de decisión se puede expresar como

$$h(x; \Theta) = \sum_{j=1}^J  \gamma_J \cdot I(x\in R_j)$$

con parámetros $\Theta = \{ R_j, \gamma_j \}_1^J$. Los parámetros se buscan de forma que

$$\hat{\Theta} = \underset{\Theta}{\arg\min} \sum_{j=1}^J \sum_{x_i \in R_j} L(y_i, \gamma_j)$$

### En R

El siguiente ejemplo está sacado del libro [Introduction to statistical learning](http://www-bcf.usc.edu/~gareth/ISL/)


```r
library(tree)
library(ISLR)
datos <- Carseats
datos$High <- as.factor(ifelse(datos$Sales <= 8,
                     "No",
                     "Yes"))
names(datos)
```

```
##  [1] "Sales"       "CompPrice"   "Income"      "Advertising" "Population" 
##  [6] "Price"       "ShelveLoc"   "Age"         "Education"   "Urban"      
## [11] "US"          "High"
```

```r
tree.carseats <- tree(High ~ .-Sales, datos)
summary(tree.carseats)
```

```
## 
## Classification tree:
## tree(formula = High ~ . - Sales, data = datos)
## Variables actually used in tree construction:
## [1] "ShelveLoc"   "Price"       "Income"      "CompPrice"   "Population" 
## [6] "Advertising" "Age"         "US"         
## Number of terminal nodes:  27 
## Residual mean deviance:  0.4575 = 170.7 / 373 
## Misclassification error rate: 0.09 = 36 / 400
```

```r
plot(tree.carseats)
text(tree.carseats)
```

![](07-SupervisedLearning_files/figure-latex/unnamed-chunk-4-1.pdf)<!-- --> 

```r
tree.carseats
```

```
## node), split, n, deviance, yval, (yprob)
##       * denotes terminal node
## 
##   1) root 400 541.500 No ( 0.59000 0.41000 )  
##     2) ShelveLoc: Bad,Medium 315 390.600 No ( 0.68889 0.31111 )  
##       4) Price < 92.5 46  56.530 Yes ( 0.30435 0.69565 )  
##         8) Income < 57 10  12.220 No ( 0.70000 0.30000 )  
##          16) CompPrice < 110.5 5   0.000 No ( 1.00000 0.00000 ) *
##          17) CompPrice > 110.5 5   6.730 Yes ( 0.40000 0.60000 ) *
##         9) Income > 57 36  35.470 Yes ( 0.19444 0.80556 )  
##          18) Population < 207.5 16  21.170 Yes ( 0.37500 0.62500 ) *
##          19) Population > 207.5 20   7.941 Yes ( 0.05000 0.95000 ) *
##       5) Price > 92.5 269 299.800 No ( 0.75465 0.24535 )  
##        10) Advertising < 13.5 224 213.200 No ( 0.81696 0.18304 )  
##          20) CompPrice < 124.5 96  44.890 No ( 0.93750 0.06250 )  
##            40) Price < 106.5 38  33.150 No ( 0.84211 0.15789 )  
##              80) Population < 177 12  16.300 No ( 0.58333 0.41667 )  
##               160) Income < 60.5 6   0.000 No ( 1.00000 0.00000 ) *
##               161) Income > 60.5 6   5.407 Yes ( 0.16667 0.83333 ) *
##              81) Population > 177 26   8.477 No ( 0.96154 0.03846 ) *
##            41) Price > 106.5 58   0.000 No ( 1.00000 0.00000 ) *
##          21) CompPrice > 124.5 128 150.200 No ( 0.72656 0.27344 )  
##            42) Price < 122.5 51  70.680 Yes ( 0.49020 0.50980 )  
##              84) ShelveLoc: Bad 11   6.702 No ( 0.90909 0.09091 ) *
##              85) ShelveLoc: Medium 40  52.930 Yes ( 0.37500 0.62500 )  
##               170) Price < 109.5 16   7.481 Yes ( 0.06250 0.93750 ) *
##               171) Price > 109.5 24  32.600 No ( 0.58333 0.41667 )  
##                 342) Age < 49.5 13  16.050 Yes ( 0.30769 0.69231 ) *
##                 343) Age > 49.5 11   6.702 No ( 0.90909 0.09091 ) *
##            43) Price > 122.5 77  55.540 No ( 0.88312 0.11688 )  
##              86) CompPrice < 147.5 58  17.400 No ( 0.96552 0.03448 ) *
##              87) CompPrice > 147.5 19  25.010 No ( 0.63158 0.36842 )  
##               174) Price < 147 12  16.300 Yes ( 0.41667 0.58333 )  
##                 348) CompPrice < 152.5 7   5.742 Yes ( 0.14286 0.85714 ) *
##                 349) CompPrice > 152.5 5   5.004 No ( 0.80000 0.20000 ) *
##               175) Price > 147 7   0.000 No ( 1.00000 0.00000 ) *
##        11) Advertising > 13.5 45  61.830 Yes ( 0.44444 0.55556 )  
##          22) Age < 54.5 25  25.020 Yes ( 0.20000 0.80000 )  
##            44) CompPrice < 130.5 14  18.250 Yes ( 0.35714 0.64286 )  
##              88) Income < 100 9  12.370 No ( 0.55556 0.44444 ) *
##              89) Income > 100 5   0.000 Yes ( 0.00000 1.00000 ) *
##            45) CompPrice > 130.5 11   0.000 Yes ( 0.00000 1.00000 ) *
##          23) Age > 54.5 20  22.490 No ( 0.75000 0.25000 )  
##            46) CompPrice < 122.5 10   0.000 No ( 1.00000 0.00000 ) *
##            47) CompPrice > 122.5 10  13.860 No ( 0.50000 0.50000 )  
##              94) Price < 125 5   0.000 Yes ( 0.00000 1.00000 ) *
##              95) Price > 125 5   0.000 No ( 1.00000 0.00000 ) *
##     3) ShelveLoc: Good 85  90.330 Yes ( 0.22353 0.77647 )  
##       6) Price < 135 68  49.260 Yes ( 0.11765 0.88235 )  
##        12) US: No 17  22.070 Yes ( 0.35294 0.64706 )  
##          24) Price < 109 8   0.000 Yes ( 0.00000 1.00000 ) *
##          25) Price > 109 9  11.460 No ( 0.66667 0.33333 ) *
##        13) US: Yes 51  16.880 Yes ( 0.03922 0.96078 ) *
##       7) Price > 135 17  22.070 No ( 0.64706 0.35294 )  
##        14) Income < 46 6   0.000 No ( 1.00000 0.00000 ) *
##        15) Income > 46 11  15.160 Yes ( 0.45455 0.54545 ) *
```

Para puntuar nuevas observaciones, simplemente utilizamos la función `predict()` cuyos parámetros más importantes son:

 - `object`: el objeto resultante del ajuste del árbol.
 - `newdata`: el data.frame que contiene la información a puntuar.
 - `type`: el tipo de la predicción. Lo más recomendable es elegir `vector` que devuelve la probabilidad de cada clase.

### Ventajas y desventajas

Las **ventajas** son:

 - Fáciles de entender su funcionamiento.
 - Pueden ser interpretados gráficamente.
 - Pueden manejar variables cualitativas sin necesidad de crear variables *dummy*.

Las **desventajas** son:

 - No tienen un buen poder predictivo.
 - Poco robustos: un pequeño cambio en los datos suponen un gran cambio en el árbol estimado.


\includegraphics[width=0.75\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/arbol2} 

## Bagging y Random Forest

Uno de los problemas de los árboles de decisión es su alta varianza, es decir, un ligero cambio en los datos puede producir un gran cambio en la estructura del árbol. Para paliar este hecho, los **modelos de bagging** actúan de la siguiente forma:

 - Se obtienen **$n$ muestras bootstrap**.
 - Se ajusta un modelo para cada una de las muestras.
 - La predicción final será la media de las predicciones.

Las muestras bootstrap consisten en seleccionar **con reemplazamiento** muestras de las observaciones originales.

Esta idea se puede aplicar desde otro enfoque. Pueden existir variables que sean muy buenas predictoras y, aunque escojamos muestras bootstrap, puede que lso árboles siempre escojan estas variables haciendo que otras varaibles menos buenas no sean tenidas nunca en cuenta. Para ello, el **modelo random forest** actúa de la misma forma que el método de bagging pero **muestreando sobre las columnas en vez de las observaciones. Esto favorece que puedan intervenir variables que, a priori, no son tan buenas predictoras.

### En R




```
FALSE 
FALSE Call:
FALSE  randomForest(formula = medv ~ ., data = Boston, mtry = 13, importance = TRUE,      subset = train) 
FALSE                Type of random forest: regression
FALSE                      Number of trees: 500
FALSE No. of variables tried at each split: 13
FALSE 
FALSE           Mean of squared residuals: 10.80817
FALSE                     % Var explained: 86.91
```

```
FALSE           %IncMSE IncNodePurity
FALSE crim    12.042248    1083.56152
FALSE zn       2.144551      76.90258
FALSE indus    9.518353    1088.06694
FALSE chas     2.591068      77.14216
FALSE nox     12.346546    1002.78940
FALSE rm      32.409454    6134.55720
FALSE age     11.799314     530.13915
FALSE dis     15.362428    1309.52619
FALSE rad      3.468537      96.05578
FALSE tax      7.196484     361.87594
FALSE ptratio 10.103258    1018.55792
FALSE black    6.737108     384.53170
FALSE lstat   27.720132    7184.83340
```

![](07-SupervisedLearning_files/figure-latex/unnamed-chunk-6-1.pdf)<!-- --> 


## Boosting

Es una familia de algoritmos de *machine learning* cuya idea es la de utilizar **métodos de aprendizaje débiles** (*weak learners*) para crear un método de aprendizaje fuerte con alto poder predictivo. Es uno de los algoritmos de aprendizaje que **mayor impacto han tenido en los últimos 20 años.** Robert E. Schapire y Yoav Freund recibieron el **premio Gödel en 2003** por su trabajo sobre boosting. La mayoría de los **ganadores recientes en Kaggle**, utilizan boosting. Buscadores como *Yahoo* utilizan **versiones propias de algoritmos de boosting**.


\includegraphics[width=0.45\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/Intuicion_1(2)} 


\includegraphics[width=0.45\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/Intuicion_2(2)} 


\includegraphics[width=0.45\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/Intuicion_3(2)} 


\includegraphics[width=0.45\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/Intuicion_4(2)} 


\includegraphics[width=0.45\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/Intuicion_5(2)} 


\includegraphics[width=0.45\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/Intuicion_6(2)} 


\includegraphics[width=0.45\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/Intuicion_8(2)} 


\includegraphics[width=0.85\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/LibroBoosting} 


El algoritmo sería 

- Inicializar: $\omega_1^{(i)}=\frac{1}{n} \text{ con } i = 1, \ldots , n$
-Repetir $t = 1, \ldots, T$:
    + Seleccionar un clasificador $h_t(x_i)$ para los datos de entrenamiento usando $\omega_t^{(i)}$ 
    + Calcular $\alpha_t$
    + Calcular  $\omega_{t+1}^{(i)} \text{ para } i = 1, \ldots, n$

- Modelo final:
  $$H(x) = sign\left(\sum_{t=1}^T\alpha_th_t(x)\right)$$
  

Lo descrito anteriormente se denomina AdaBoost y se aplica solamente para problema de tipo binario. Para poder aplicarlo a cualquier tipo de problema, tenemos el gradient boosting, donde se utiliza el algoritmo del descenso del gradiente:

El algoritmo gradient boosting queda de la siguiente forma

 * Inicializar: $H_0 \equiv 0$
 * Repetir para $t = 1, \ldots, T$:
     + Calcular para $i = 1, \ldots, n$
          $$r_{it}=-\left[ \frac{\partial L(y_i, H(x_i))}{\partial H(x_i)}\right]_{H=H_{t-1}}$$
     + Ajustar $h_t$ a $r_{it}$
     + Elegir $\alpha_t >0$ tal que
      $$\alpha_t = \underset{\alpha_t}{\arg\min} \sum_{i= 1}^n L(x_i, H_{t-1}(x_i)+\alpha_th_t(x_i))$$
  
### Optimización de parámetros

Como con cualquier otro modelo, lleva asociados una serie de parámetros que necesitan ser ajustados para encontrar el mejor modelo. Dos formas básicas de hacerlo son

 * Hacer un barrido con un número elevado de combinaciones.
 * Hacer una búsqueda orientada de los metaparámetros óptimos.

**Paso 1. Modelo inicial.**

Hacer un primer lanzamiento dejando las opciones por defecto o elegir una combinación inicial.

Por ejemplo,

 *   profundidad = 5.
 *   submuestra = 0.8.
 *   **shrinkage = 0.1.**


**Paso 2. Profundidad del árbol**

Una vez que tenemos un modelo base, realizamos pruebas con distintas profundidades de árbol.

Una buena idea suele ser elegir profundidades entre 2 y 10.

**Paso 3. Submuestra (y muestra de columnas)**

El siguiente paso es elegir un porcentaje de observaciones que se utilizarán, aleatoriamente, en cada iteración.

Se suelen elegir valores 0.6, 0.7, 0.8, 0.9.

A veces, si el volumen de datos es muy grande, se pueden elegir submuestras por debajo de 0.5 sin que se vea afectado el poder predictivo.

En caso de que el *software* lo permita, se puede probar a la vez el muestreo sobre columnas (idea de *random forest*)

**Paso 4. Shrinkage**

Por último, nos ocupamos del *shrinkage*, probablemente el valor que más puede ayudar a tener un mejor modelo.

En función de la estructura de los datos, podemos hacer un barrido inicial de, por ejemplo, 0.1, 0.01 y 0.001. En este caso es importante cambiar el número de iteraciones a un valor alto y elegir el número óptimo de estas mediante parada temprana.


### En R


```r
library(xgboost)

dtrain <- xgb.DMatrix(agaricus.train$data, label = agaricus.train$label)
dtest <- xgb.DMatrix(agaricus.test$data, label = agaricus.test$label)
watchlist <- list(eval = dtest, train = dtrain)

xgb_grid = expand.grid(
  eta                 = c(0.1, 0.05, 0.01, 0.001),
  max_depth           = c(3, 6, 8, 10),
  colsample_bytree    = 1,
  subsample  = c(0.6, 0.75, 1)
)


modelos.xgb <- data.frame()
tiempo.ini <- Sys.time()
for (model in 1:nrow(xgb_grid)){
  set.seed(1234)
  boosting <- xgb.train(data                = dtrain,
                        nrounds             = 10,
                        objective           = "binary:logistic", 
                        booster             = "gbtree",
                        eval_metric         = "auc",
                        params              = as.list(xgb_grid[model,]),
                        watchlist           = watchlist,
                        early_stopping_rounds    = 3
  )
  
  modelos.xgb <- rbind(modelos.xgb, data.frame( xgb_grid[model,],
                                                iteracion = boosting$best_iteration,
                                                ntree = boosting$best_ntreelimit,
                                                error = boosting$best_score))
  
  print(paste('modelo', model, ' de ', nrow(xgb_grid)))
  print(max(modelos.xgb$error))
  print(paste('Tiempo ',Sys.time() - tiempo.ini))
}

pred <- predict(boosting, agaricus.test$data, ntreelimit = 8)
```

## Explicación de predicción

Una crítica que se hace a los modelos de Machine Learning es que están muy orientados hacia la predicción lo que provoca que sean modelos muy complejos convirtiéndose en *cajas negras* y de los que perdemos la *"explicatividad"* del modelo. Esto conlleva que los usuarios finales para los que se desarrollan los modelos puedan perder el interés por el ya que no entienden la naturaleza de las predicciones. Esto es especialmente relevante en entornos médicos: si se construye un modelo para determinar si se debe hacer una intervención, para que el equipo médico pueda confiar en el modelo necesita entender qué soporta la decisión de intervenir al margen de indicadores formales.

Para solventar estos problemas surge una iniciativa relativamente novedosa conocida como **LIME**. La referencia se puede consultar en el siguiente paper: [https://arxiv.org/pdf/1602.04938v1.pdf].

La idea intuitiva que hay detrás de esta técnica es que, aunque un modelo pueda ser altamente complejo y flexible, se supone que es localmente estable:


\includegraphics[width=0.65\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/lime} 

Por ejemplo, si entrenasemos una red neuronal para intenar describir los elementos de una fotografía, nos interesaría saber **qué es lo que soporta la decisión**


\includegraphics[width=0.85\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/lime2} 

### Un ejemplo en R


```r
library(lime)
library(MASS)
data(biopsy)

biopsy$ID <- NULL
biopsy <- na.omit(biopsy)
names(biopsy) <- c('clump thickness', 'uniformity of cell size', 
                   'uniformity of cell shape', 'marginal adhesion',
                   'single epithelial cell size', 'bare nuclei', 
                   'bland chromatin', 'normal nucleoli', 'mitoses',
                   'class')

set.seed(4)
test_set <- sample(seq_len(nrow(biopsy)), 100)
prediction <- biopsy$class
biopsy$class <- NULL
model <- lda(biopsy[-test_set, ], prediction[-test_set])

explainer <- lime(biopsy[-test_set,],
                  model)

explanation <- explain(biopsy[test_set[1:4], ], explainer, n_labels = 1, n_features = 4)


plot_features(explanation, ncol = 1)
```

![](07-SupervisedLearning_files/figure-latex/unnamed-chunk-18-1.pdf)<!-- --> 


<!--chapter:end:07-SupervisedLearning.Rmd-->

# (APPENDIX) ANEXO {-} 


# Sobre in-training 

**in-training** es una iniciativa del Aula Innova. Es la respuesta de Innova-tsn a la necesidad actual de formación especializada y personalizada de nuestros clientes ante un entorno en continua evolución y unos cambios tecnológicos vertiginosos.

El conocimiento y especialización que Innova-tsn ha adquirido a lo largo de los años, su continua apuesta por la innovación y la cualificación e inquietud académica de sus profesionales, permiten crear una oferta formativa flexible y totalmente personalizada que aúna conocimientos teóricos y experiencia práctica.

De este modo, se pretende alcanzar eficientemente el principal objetivo de Aula Innova: Dotar a los clientes de Innova-tsn de los conocimientos y competencias necesarias para afrontar eficiente y autónomamente sus propios retos de negocio.



\includegraphics[width=0.89\linewidth]{C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/InnovaTrainingLogo} 


 

# Colaboradores 




\begin{tabular}{ll}
\toprule
Alicia Morales & <alicia.morales@innova-tsn.com>\\
Alicia Muñoz & <alicia.munoz@innova-tsn.com>\\
Alvaro Díaz & <alvaro.munoz@innova-tsn.com>\\
Andrés Devia & <andres.devia@innova-tsn.com>\\
Daniel Vélez & <daniel.velez@innova-tsn.com>\\
\addlinespace
Diego Fernández & <diego.fernandez@innova-tsn.com>\\
Jaume Puigbó & <jaume.puigbo@innova-tsn.com>\\
Juan Luis Rivero & <juan.rivero@innova-tsn.com>\\
Pablo Hidalgo & <pablo.hidalgo@innova-tsn.com>\\
Romy Rodríguez & <romy.rodríguez@innova-tsn.com>\\
\bottomrule
\end{tabular}

<!--chapter:end:20-anexo.Rmd-->



<!--chapter:end:12-Referencias.Rmd-->

