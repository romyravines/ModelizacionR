# (PART) Machine Learning {-} 



# Introducci�n
Los modelos anal�ticos se dividen en dos grandes grupos: 

- **modelos supervisados**
- **modelos no supervisados**

## Aprendizaje supervisado

En el **aprendizaje supervisado** existen una serie de **caracter�sticas** *(o variables, variables independientes, variables predictoras, columnas, features)* que vamos a utilizar para decir algo de otra **variable objetivo** *(o target, variable respuesta, variable dependiente)*. Ese *decir algo* puede ser **predecir** el valor que puede tomar la variable objetivo en funci�n de las caracter�sticas disponibles o intentar **explicar** porqu� se comporta como se comporta.

Pongamos dos ejemplos. En una empresa el�ctrica quieren desarrollar un modelo que les permita **detectar el fraude en los contadores**. En este caso, tendremos disponibles una serie de caracter�sticas como informaci�n del contrato del cliente (fecha de alta, datos demogr�ficos), consumos, etc�tera. Tambi�n tendremos disponible la informaci�n obtenida de una campa�a de inspecciones que han realizado para una muestra de los clientes y lo que se busca es **extraer un patr�n de fraude** de tal forma que dadas las caracter�sticas de un cliente  concreto, el modelo sea capaz de devolver una probabilidad de que el cliente est� cometiendo fraude. Es decir, sea $X = (X_1, X_2, \ldots, X_n)$ el vector de las caracter�sticas e $Y$ el valor de la variable objetivo que, en este caso, toma valores $0$ y $1$, estamos buscando 

$$
P(Y = 1|X)
$$

En este caso, lo que interesa es **detectar el fraude con la mayor precisi�n posible**.

Otro ejemplo podr�a ser el de que se quiera desarrollar un modelo para **conceder o no un cr�dito** pero que seamos capaces de **saber porqu� el modelo acepta o deniega un cr�dito.**

En el **primer ejemplo**, buscaremos un **modelo muy preciso** aunque no seamos capaces entender porqu� da las predicciones que da y ser�a conveniente el uso de modelos propios del **Machine Learning**. En el **segundo ejemplo**, el inter�s es justo al contrario, queremos **entender el comportamiento** del modelo aunque esto suponga un detrimento de la capacidad predictiva y podr�amos utilizar **modelos m�s cl�sicos**.

Este tipo de aprendizaje est� conformado por t�cnicas en las que tanto las caracter�sticas como la variable objetivo pueden ser de diversa naturaleza (variables binarias, reales, categ�ricas, etc�tera).

### Una reflexi�n

La cr�tica principal que se hace a los algoritmos de Machine Learning es que es complicado entender la naturaleza de las predicciones. En los �ltimos a�os est�n surgiendo iniciativas que permiten darle una capa explicativa a este tipo de modelos. La iniciativa con m�s repercusi�n es [LIME](https://shiring.github.io/machine_learning/2017/04/23/lime).

Ahora bien, el �xito de los modelos de machine learning frente a otros m�s cl�sicos se basa en que son capaces de adaptarse y detectar patrones complejos. Pero en la realidad, **no siempre se tiene porqu� dar un patr�n complejo** y podr�a ser suficiente con un modelo cl�sico. 

> Es fundamental entender la estructura de los datos y probar distintos modelos para escoger el m�s �til para el problema.


## Aprendizaje no supervisado

En el **aprendizaje no supervisado**, la naturaleza del problema es distinta. En estos modelos **no existe una target** y el objetivo es tratar de descubrir una estructura con las caracter�sticas presentes. Por ejemplo, si yo quisiese conocer qu� tipolog�a de clientes existen en mi empresa, utilizar�a este tipo de t�cnicas para poder **segmentarlos**. 

�Cu�l es la dificultad? Al no haber una variable objetivo, es dif�cil saber si la agrupaci�n realizada es correcta o no, ni siquiera podemos definir en qu� grado es correcta. En los modelos supervisados, al conocer la variable objetivo, somos capaces de dar una tasa error que comete un modelo y poder as� elegir aqu�l modelo que me proporciona una mejor tasa (el que es capaz de detectar un mayor n�mero de los fraudes realizados, en el ejemplo de la empresa el�ctrica). Por este motivo, **las t�cnicas no supervisadas tienen un componente de subjetividad.**


# Aprendizaje no supervisado
 
Para la resoluci�n de los problemas suelen existir un gran n�mero de modelos disponibles que difieren entre ellos de las hip�tesis utilizadas para la construcci�n del modelo matem�tico. 

Para el caso del **aprendizaje no supervisado** discutiremos el comportamiento de las siguientes t�cnicas:

 - k-medias (*k-means*).
 - clustering jer�rquico.

## K-medias (*k-means*)

El algoritmo de clustering de las **k-medias** es uno de los algoritmos m�s utilizados por su simplicidad te�rica y por su eficiencia computacional. No obstante, tiene algunas limitaciones que es necesario tener en cuenta.

###  El modelo

El algoritmo trata de dividir el espacio en regiones (*clusters*) de tal forma que **toda observaci�n pertenezca a un y solo un cluster**. Es decir, cualquier observaci�n estar� contenida en uno de los clusters que se definan y no podr� pertenecer a ning�n otro.

La $k$ que aparece en el nombre de la t�cnica hace referencia a que el algoritmo **necesita que se especifique previamente el n�mero de clusters $k$ que se desean obtener**. La forma de proceder del algoritmo es la siguiente:

 1. Se definen $k$ puntos aleatorios en el espacio conocidos como **centroides** y que ser�n los representantes de cada cluster.
 1. Se calcula la distancia -eucl�dea- de cada punto a cada uno de los $k$ centroides y se asigna cada punto aquel cluster cuya distancia a su centroide sea m�nima.
 1. Se recalculan los $k$ centroides como **la media de las observaciones que componen cada cluster**.
 1. Se vuelve al paso 2.

El algoritmo termina cuando se satisface alg�n criterio de parada como, por ejemplo, que ning�n punto cambie de cluster de una iteraci�n a la siguiente.

El problema que estamos tratando de resolver mediante el algoritmo se puede ver como un **problema de optimizaci�n**. Matem�ticamente, dado un conjunto de $n$ observaciones, $x_1, \ldots, x_n$ queremos buscar aquellos clusters $S^* = {S_{1}, S_{2}, �, S_{k}}$ con $k \leq n$ que minimicen la expresi�n

$$
S^* = \underset{S}{argmin} \sum_{i=1}^{k}\sum_{x_{j}\in S_{j}} d(x_{j}, c_{i}),
$$
donde $c_i$ es el centroide de $S_i$.

Se suele usar la distancia euclidea, pero podemos buscar otras distancias que se adecuen mejor a los datos, como por ejemplo, la norma infinita o la distancia de Manhattan.

### Un ejemplo gr�fico

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



















































