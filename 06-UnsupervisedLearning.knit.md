# (PART) Machine Learning {-} 



# Introducción
Los modelos analíticos se dividen en dos grandes grupos: 

- **modelos supervisados**
- **modelos no supervisados**

## Aprendizaje supervisado

En el **aprendizaje supervisado** existen una serie de **características** *(o variables, variables independientes, variables predictoras, columnas, features)* que vamos a utilizar para decir algo de otra **variable objetivo** *(o target, variable respuesta, variable dependiente)*. Ese *decir algo* puede ser **predecir** el valor que puede tomar la variable objetivo en función de las características disponibles o intentar **explicar** porqué se comporta como se comporta.

Pongamos dos ejemplos. En una empresa eléctrica quieren desarrollar un modelo que les permita **detectar el fraude en los contadores**. En este caso, tendremos disponibles una serie de características como información del contrato del cliente (fecha de alta, datos demográficos), consumos, etcétera. También tendremos disponible la información obtenida de una campaña de inspecciones que han realizado para una muestra de los clientes y lo que se busca es **extraer un patrón de fraude** de tal forma que dadas las características de un cliente  concreto, el modelo sea capaz de devolver una probabilidad de que el cliente esté cometiendo fraude. Es decir, sea $X = (X_1, X_2, \ldots, X_n)$ el vector de las características e $Y$ el valor de la variable objetivo que, en este caso, toma valores $0$ y $1$, estamos buscando 

$$
P(Y = 1|X)
$$

En este caso, lo que interesa es **detectar el fraude con la mayor precisión posible**.

Otro ejemplo podría ser el de que se quiera desarrollar un modelo para **conceder o no un crédito** pero que seamos capaces de **saber porqué el modelo acepta o deniega un crédito.**

En el **primer ejemplo**, buscaremos un **modelo muy preciso** aunque no seamos capaces entender porqué da las predicciones que da y sería conveniente el uso de modelos propios del **Machine Learning**. En el **segundo ejemplo**, el interés es justo al contrario, queremos **entender el comportamiento** del modelo aunque esto suponga un detrimento de la capacidad predictiva y podríamos utilizar **modelos más clásicos**.

Este tipo de aprendizaje está conformado por técnicas en las que tanto las características como la variable objetivo pueden ser de diversa naturaleza (variables binarias, reales, categóricas, etcétera).

### Una reflexión

La crítica principal que se hace a los algoritmos de Machine Learning es que es complicado entender la naturaleza de las predicciones. En los últimos años están surgiendo iniciativas que permiten darle una capa explicativa a este tipo de modelos. La iniciativa con más repercusión es [LIME](https://shiring.github.io/machine_learning/2017/04/23/lime).

Ahora bien, el éxito de los modelos de machine learning frente a otros más clásicos se basa en que son capaces de adaptarse y detectar patrones complejos. Pero en la realidad, **no siempre se tiene porqué dar un patrón complejo** y podría ser suficiente con un modelo clásico. 

> Es fundamental entender la estructura de los datos y probar distintos modelos para escoger el más útil para el problema.


## Aprendizaje no supervisado

En el **aprendizaje no supervisado**, la naturaleza del problema es distinta. En estos modelos **no existe una target** y el objetivo es tratar de descubrir una estructura con las características presentes. Por ejemplo, si yo quisiese conocer qué tipología de clientes existen en mi empresa, utilizaría este tipo de técnicas para poder **segmentarlos**. 

¿Cuál es la dificultad? Al no haber una variable objetivo, es difícil saber si la agrupación realizada es correcta o no, ni siquiera podemos definir en qué grado es correcta. En los modelos supervisados, al conocer la variable objetivo, somos capaces de dar una tasa error que comete un modelo y poder así elegir aquél modelo que me proporciona una mejor tasa (el que es capaz de detectar un mayor número de los fraudes realizados, en el ejemplo de la empresa eléctrica). Por este motivo, **las técnicas no supervisadas tienen un componente de subjetividad.**


# Aprendizaje no supervisado
 
Para la resolución de los problemas suelen existir un gran número de modelos disponibles que difieren entre ellos de las hipótesis utilizadas para la construcción del modelo matemático. 

Para el caso del **aprendizaje no supervisado** discutiremos el comportamiento de las siguientes técnicas:

 - k-medias (*k-means*).
 - clustering jerárquico.

## K-medias (*k-means*)

El algoritmo de clustering de las **k-medias** es uno de los algoritmos más utilizados por su simplicidad teórica y por su eficiencia computacional. No obstante, tiene algunas limitaciones que es necesario tener en cuenta.

###  El modelo

El algoritmo trata de dividir el espacio en regiones (*clusters*) de tal forma que **toda observación pertenezca a un y solo un cluster**. Es decir, cualquier observación estará contenida en uno de los clusters que se definan y no podrá pertenecer a ningún otro.

La $k$ que aparece en el nombre de la técnica hace referencia a que el algoritmo **necesita que se especifique previamente el número de clusters $k$ que se desean obtener**. La forma de proceder del algoritmo es la siguiente:

 1. Se definen $k$ puntos aleatorios en el espacio conocidos como **centroides** y que serán los representantes de cada cluster.
 1. Se calcula la distancia -euclídea- de cada punto a cada uno de los $k$ centroides y se asigna cada punto aquel cluster cuya distancia a su centroide sea mínima.
 1. Se recalculan los $k$ centroides como **la media de las observaciones que componen cada cluster**.
 1. Se vuelve al paso 2.

El algoritmo termina cuando se satisface algún criterio de parada como, por ejemplo, que ningún punto cambie de cluster de una iteración a la siguiente.

El problema que estamos tratando de resolver mediante el algoritmo se puede ver como un **problema de optimización**. Matemáticamente, dado un conjunto de $n$ observaciones, $x_1, \ldots, x_n$ queremos buscar aquellos clusters $S^* = {S_{1}, S_{2}, …, S_{k}}$ con $k \leq n$ que minimicen la expresión

$$
S^* = \underset{S}{argmin} \sum_{i=1}^{k}\sum_{x_{j}\in S_{j}} d(x_{j}, c_{i}),
$$
donde $c_i$ es el centroide de $S_i$.

Se suele usar la distancia euclidea, pero podemos buscar otras distancias que se adecuen mejor a los datos, como por ejemplo, la norma infinita o la distancia de Manhattan.

### Un ejemplo gráfico

Para comprender mejor el comportamiento del algoritmo, veamos su funcionamiento en un ejemplo de juguete:




```r
set.seed(1234)

Sigma <- matrix(c(2,0, 0, 2), 2, 2)

norm_multi1 <- mvrnorm(n = 100, c(-4, -4), Sigma)
norm_multi2 <- mvrnorm(n = 100, c(0,   4), Sigma)
norm_multi3 <- mvrnorm(n = 100, c(4,  -4), Sigma)

datos <- as.data.frame(rbind(norm_multi1, norm_multi2, norm_multi3))

names(datos) <- c('X', 'Y')
```



















































