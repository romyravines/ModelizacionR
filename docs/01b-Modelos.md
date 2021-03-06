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

<img src="imgs/unsupervisedtechniques.png" width="80%" />

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


<img src="imgs/classificationtechniques.png" width="80%" />

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



<img src="imgs/regressiontechniques.png" width="80%" />
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



<img src="imgs/choosealgorithm.png" width="90%" />




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



  
 
 
