# Boosting

## ¿Qué es boosting?

> - Es una familia de algoritmos de *machine learning* cuya idea es la de utilizar **métodos de aprendizaje débiles** (*weak learners*) para crear un método de aprendizaje fuerte con alto poder predictivo.
> - Es uno de los algoritmos de aprendizaje que **mayor impacto han tenido en los últimos 20 años.**
> - Robert E. Schapire y Yoav Freund recibieron el **premio Gödel en 2003** por su trabajo sobre boosting.
> - La mayoría de los **ganadores recientes en Kaggle**, utilizan boosting.
> - Buscadores como *Yahoo* utilizan **versiones propias de algoritmos de boosting**.


## Modelos ensamblados

Los **métodos de ensamblado de modelos** usan múltiples algoritmos de aprendizaje para obtener un mejor poder predictivo del que se podría obtener con cada uno de los modelos por separado.

Los más habituales:

- Bagging (**B**ootstrap **Agg**regat**ing**)
- Random Forest
- **Boosting**


## AdaBoost

### Idea intuitiva




<img src="C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/Intuicion_1(2).png" width="25%" />

<img src="C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/Intuicion_2(2).png" width="25%" />

<img src="C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/Intuicion_3(2).png" width="25%" />

<img src="C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/Intuicion_4(2).png" width="25%" />

<img src="C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/Intuicion_5(2).png" width="25%" />

<img src="C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/Intuicion_6(2).png" width="25%" />

<img src="C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/Intuicion_8(2).png" width="25%" />

<img src="C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/LibroBoosting.png" width="50%" />


### Nomenclatura


Sean $n$ observaciones $(x_1, y_1), ... , (x_n, y_n)$ donde $x_i \in \mathcal{X}^p$ , $y_i \in \{-1,+1\}$.

Sea $h_t$ un clasificador binario de forma que
$$h_t:\mathcal{X}^p\rightarrow \{-1,+1\}$$

Definimos las funciones 

 $$I(y_i \ne h_t(x_i)) = \begin{cases} 1, & \mbox{si } y_i \neq h_t(x_i) \\ 0, & \mbox{si } y_i = h_t(x_i)  \end{cases}$$

$$H_t(x_i) = sign\left(\sum_{t=1}^T \alpha_t h_t(x_i)\right) = \begin{cases} 1, & \mbox{si } \sum_{t=1}^T \alpha_t h_t(x_i) > 0 \\ -1, & \mbox{si } \sum_{t=1}^T \alpha_t h_t(x_i) < 0  \end{cases}$$



### AdaBoost. Algoritmo

> * Inicializar: $\omega_1^{(i)}=\frac{1}{n} \text{ con } i = 1, \ldots , n$
> * Repetir $t = 1, \ldots, T$:
    + Seleccionar un clasificador $h_t(x_i)$ para los datos de entrenamiento usando $\omega_t^{(i)}$ 
    + Calcular $\alpha_t$
    + Calcular  $\omega_{t+1}^{(i)} \text{ para } i = 1, \ldots, n$

> * Modelo final:
  $$H(x) = sign\left(\sum_{t=1}^T\alpha_th_t(x)\right)$$
  
  

## Funciones de pérdida 

Los algoritmos de aprendizaje persiguen minimizar una **función de pérdida** (*loss function*) cumpliendo unas hipótesis de partida.

Como ejemplo, una **regresión lineal simple** busca la recta que mejor se ajuste a una nube de puntos. 

 **¿En qué sentido?**


### Funciones de pérdida: Regresión lineal

<img src="08-Boosting_files/figure-html4/unnamed-chunk-9-1.png" width="672" />


<img src="08-Boosting_files/figure-html4/unnamed-chunk-10-1.png" width="672" />

<img src="08-Boosting_files/figure-html4/unnamed-chunk-11-1.png" width="672" />

Una opción es utilizar el valor absoluto:

$$L(y,f(x)) = |y - f(x)| $$

<div class="columns-2"> 
<img src="08-Boosting_files/figure-html4/unnamed-chunk-12-1.png" width="288" />
  
>  * Incrementa el error de forma lineal.
  
>  * **No es derivable en todo punto**

>  *  [Least Absolute Deviations](https://en.wikipedia.org/wiki/Least_absolute_deviations) 

</div>


Tradicionalmente se elige una pérdida cuadrática:

$$L(y,f(x)) = (y - f(x))^2 $$

<div class="columns-2"> 
<img src="08-Boosting_files/figure-html4/unnamed-chunk-13-1.png" width="288" />
  
>  * Incrementa el error de forma cuadrática. Observaciones atípicas (*outliers*) contribuyen mucho al error.
  
>  * **Es derivable en todo punto**.
</div>


### Funciones de pérdida: Clasificación.

 * Clasificación errónea: $L(y, f(x)) = I(-yf(x) > 0)$

<img src="08-Boosting_files/figure-html4/unnamed-chunk-14-1.png" width="288" />




### Funciones de pérdida para clasificación.

 * Clasificación errónea: $L(y, f(x)) = I(-yf(x) > 0)$
 * Pérdida exponencial: $L(y, f(x)) = exp\{-y\cdot f(x) \}$


<img src="08-Boosting_files/figure-html4/unnamed-chunk-15-1.png" width="288" />


### Pérdida exponencial

Sea
$$E = \sum_{i=1}^n exp\{ -y_iH_t(x_i) \}$$  

Tal y como hemos construido el algoritmo, se tiene que
$$H_t(x_i)=\sum_{j=1}^t\alpha_jh_j(x_i)$$

$$= \alpha_th_t(x_i) + \sum_{j=1}^{t-1}\alpha_jh_j(x_i) $$

$$= \alpha_th_t(x_i) + H_{t-1}(x_i)$$

$$H_t(x_i)= \alpha_th_t(x_i) + H_{t-1}(x_i)$$

luego 
$$E = \sum_{i=1}^n exp\{ -y_iH_t(x_i) \}  $$

$$=\sum_{i=1}^n exp\{- y_i H_{t-1}(x_i) - y_i\alpha_th_t(x_i) \} $$

$$= \sum_{i=1}^n \omega_t^{(i)} exp\{ - y_i\alpha_th_t(x_i) \}$$  
siendo $\omega_t^{(i)} = exp\{- y_i H_{t-1}(x_i) \}$


$$E =  \sum_{i=1}^n \omega_t^{(i)} exp\{ - y_i\alpha_th_t(x_i) \}$$  
  
Sean:

+ **$\mathcal{B}_n$** el conjunto de puntos **bien clasificados**.
+ **$\mathcal{M}_n$** el conjunto de puntos **mal clasificados**.

podemos escribir

$$E = e^{-\alpha_t}\sum_{i\in\mathcal{B}_n} \omega_t^{(i)} + e^{\alpha_t} \sum_{i\in\mathcal{M}_n} \omega_t^{(i)}$$

$$= (e^{\alpha_t}-e^{-\alpha_t})\sum_{i= 1}^n\omega_t^{(i)}I(h_t(x_i) \neq y_i) + e^{-\alpha_t}\sum_{i= 1}^n\omega_t^{(i)}$$




$$E = (e^{\alpha_t}-e^{-\alpha_t})\sum_{i= 1}^n\omega_t^{(i)}I(h_t(x_i) \neq y_i) + e^{-\alpha_t}\sum_{i= 1}^n\omega_t^{(i)}$$  

* Si minimizamos respecto a $h_t(x_i)$, el sumando de la derecha es constante, con lo que es equivalente a 

$$\hat{h}_t =  \underset{h_t}{\arg\min} \sum_{i= 1}^n\omega_t^{(i)}I(h_t(x_i) \neq y_i)$$

$$E = (e^{\alpha_t}-e^{-\alpha_t})\sum_{i= 1}^n\omega_t^{(i)}I(h_t(x_i) \neq y_i) + e^{-\alpha_t}\sum_{i= 1}^n\omega_t^{(i)}$$  

* Minimizando respecto a $\alpha_t$:

$$\frac{\partial E}{\partial \alpha_t} = (e^{\alpha_t} + e^{-\alpha_t})\sum_{i= 1}^n\omega_t^{(i)}I(h_t(x_i) \neq y_i) - e^{-\alpha_t}\sum_{i= 1}^n\omega_t^{(i)} = 0 $$

$$\Leftrightarrow \frac{e^{\alpha_t} + e^{-\alpha_t}}{e^{-\alpha_t}} = \frac{\sum_{i= 1}^n\omega_t^{(i)}}{\sum_{i= 1}^n\omega_t^{(i)}I(h_t(x_i) \neq y_i)}$$

$$\Leftrightarrow 1 + e^{2\alpha_t} = \frac{\sum_{i= 1}^n\omega_t^{(i)}}{\sum_{i= 1}^n\omega_t^{(i)}I(h_t(x_i) \neq y_i)}$$

$$\Rightarrow 1 + e^{2\alpha_t} = \frac{\sum_{i= 1}^n\omega_t^{(i)}}{\sum_{i= 1}^n\omega_t^{(i)}I(h_t(x_i) \neq y_i)}$$

Por comodidad, llamemos $err_t = \frac{\sum_{i= 1}^n\omega_t^{(i)}I(h_t(x_i) \neq y_i)}{\sum_{i= 1}^n\omega_t^{(i)}}$. Tenemos que

$$e^{2\alpha_t} = \frac{1-err_t}{err_t}$$

> **$$\alpha_t = \frac{1}{2}\ln \left(\frac{1-err_t}{err_t}\right)$$**


Ya tenemos dos de los elementos del algoritmo:

 **$$\hat{h}_t(x_i) =  \underset{h_t(x_i)}{\arg\min} \sum_{i= 1}^n\omega_t^{(i)}I(h_t(x_i) \neq y_i)$$**


 **$$\alpha_t = \frac{1}{2}\ln \left(\frac{1-err_t}{err_t}\right)$$**

Nos falta deducir cómo se actualizan los pesos **$\omega_t^{(i)}$** en cada iteración $t$.

Hace un momento hemos llegado a que 
$$ \omega_t^{(i)} = exp\{- y_i H_{t-1}(x_i) \}$$

Podemos escribir
$$ \omega_{\bf{t+1}}^{(i)} = exp\{- y_i H_{t}(x_i) \} = exp\{- y_i (H_{t-1}(x_i) + \alpha_t h_t) \} $$
$$= exp\{- y_i (H_{t-1}(x_i))\} exp\{-y_i\alpha_t h_t \} $$

> $$\omega_{\bf{t+1}}^{(i)} = \omega_t^{(i)}  e^{-y_i\alpha_t h_t }$$


## Algoritmo AdaBoost Revisitado

Ya somos capaces de escribir el algoritmo AdaBoost al completo 

* Inicializar: $\omega_1^{(i)}=\frac{1}{m} \text{ con } i = 1, \ldots , m$
* Repetir $t = 1, \ldots, T$:
    + Seleccionar $h_t$ tal que minimice el error: 
    $$err_t = \frac{\sum_{i=1}^n [\omega_t^{(i)} \cdot I(y_i \neq h_t(x_i))]}{\sum_{i=1}^n \omega_t^{(i)}}$$
    + Calcular  **$$\alpha_t = \frac{1}{2}\ln \left(\frac{1-err_t}{err_t}\right)$$**
    + Actualizar los pesos, 
      $$\omega_{\bf{t+1}}^{(i)} = \omega_t^{(i)}  e^{-y_i\alpha_t h_t }$$

***

* Modelo final:

$$H(x) = sign\left(\sum_{t=1}^T\alpha_th_t(x)\right)$$

### AdaBoost en acción

<iframe width="560" height="315" src="https://www.youtube.com/embed/k4G2VCuOMMg" frameborder="0" allowfullscreen></iframe>


### AdaBoost. Propiedades

* El error de entrenamiento se reduce hasta 0.

* El error de generalización (error en test) continúa reduciéndose incluso después de que el error en entrenamiento es 0. (No sobreajusta).



## Gradient boosting

El algoritmo AdaBoost se ha desarrollado para problemas de **clasificación binaria** utilizando la **función de pérdida exponencial**.

Para poder aplicar la idea de boosting a otro tipo de problemas, surge el algoritmo de **gradient boosting**. 


### Algoritmo

En resumen

* Inicializar: $H_0 \equiv 0$
* Repetir $t = 1, \ldots, T$:
    + Seleccionar $h_t$ y $\alpha_t$ tal que 
      $$(h_t, \alpha_t) = \underset{(h_t(x_i), \alpha_t)}{\arg\min} \sum_{i= 1}^n L(y_i, f(x_i))$$
    + Actualizar
      $$H_t = H_{t-1} + \alpha_t h_t$$
* Modelo final:
  $$H(x) = sign\left(\sum_{t=1}^T\alpha_th_t(x)\right)$$


## Optimización numérica

Sea una función

$$L(H) = \sum_{i=1}^nL(y_i, H(x_i))$$

El objetivo es minimizar $L(H)$ con respecto a $H$. Esto puede verse como un problema de optimización numérica  

$$\hat{H} = \underset{H}{\arg\min}L(H)$$

***

Los métodos de optimización numérica tratan de resolver el problema como una suma de componentes

$$H_M = \sum_{m=0}^M h_m,\mbox{ }h_m\in\mathbb{R}^n$$

donde $H_0$ es un punto arbitrario inicial.

Los distintos métodos difieren en cómo calcular cada incremento $h_m$.

### Descenso del gradiente (gradient descent)

Uno de los métodos de optimización numérica más utilizados es el **descenso del gradiente** (*gradient descent*). 

Dado un valor inicial $x_0$, podemos cambiar su valor en muchas direcciones. Para averiguar cuál es la mejor dirección para minimizar $L$, podemos escoger el **gradiente** $\nabla L$.

Intuitivamente, el gradiente da la pendiente de la curva en $x$ y su dirección indicará hacia donde crece la función. Por tanto, cambiamos $x$ en la dirección opuesta del gradiente

$$x_{k+1} = x_k - \lambda \nabla L(x_k)$$

***

[Visualmente](http://www.onmyphd.com/?p=gradient.descent)

El algoritmo gradient boosting queda de la siguiente forma

> * Inicializar: $H_0 \equiv 0$
> * Repetir para $t = 1, \ldots, T$:
    + Calcular para $i = 1, \ldots, n$
          $$r_{it}=-\left[ \frac{\partial L(y_i, H(x_i))}{\partial H(x_i)}\right]_{H=H_{t-1}}$$
    + Ajustar $h_t$ a $r_{it}$
    + Elegir $\alpha_t >0$ tal que
      $$\alpha_t = \underset{\alpha_t}{\arg\min} \sum_{i= 1}^n L(x_i, H_{t-1}(x_i)+\alpha_th_t(x_i))$$
  

### ¡A jugar!

Tenemos los puntos $(x_1, y_1), \ldots , (x_n, y_n)$ y nuestro objetivo es ajustar un modelo $H(x)$ para minimizar la pérdida cuadrática.

Suponemos que nos dan un modelo $H$ que ya se ha ajustado a los datos. Comprobamos el modelo y vemos que es un modelo bueno pero no perfecto. Hay algunos errores: $H(x_1) = 0.8$ mientras que $y_1 = 0.9$ y $H(x_1) = 1.4$ mientras que $y_1 = 1.3$.
¿Cómo podemos mejorar este modelo?

Reglas del juego:

* No podemos tocar nada del modelo $H$.

* Podemos añadir un modelo adicional $h$ a $H$ de forma que la nueva predicción sea $H(x) + h(x)$.


Queremos mejorar el modelo de forma que

$$H(x_1) + h(x_1) = y_1$$
$$H(x_2) + h(x_2) = y_2$$
$$\ldots$$
$$H(x_n) + h(x_n) = y_n$$




O, equivalentemente,

$$h(x_1) = y_1 - H(x_1)$$
$$h(x_2) = y_2 - H(x_2)$$
$$\ldots$$
$$h(x_n) = y_n - H(x_n)$$

¿Podemos ajustar esto?

De forma perfecta, no. Pero podemos encontrar un modelo que lo ajuste de **forma aproximada.**

¿Cómo?

***
 
Ajustando un modelo sobre los puntos 

$$(x_1, y_1 - H(x_1))$$
$$(x_2, y_2 - H(x_2))$$
$$\ldots$$
$$(x_n, y_n - H(x_n))$$


Solemos referirnos a $y_i - H(x_i)$ como los **residuos**. Son partes que el modelo existente $H$ **no puede ajustar bien**.

Si el nuevo modelo $H + h$ no es suficientemente bueno, podemos repetir el mismo razonamiento.

¿En qué se parece esto al descenso del gradiente?


Si utilizamos la función de pérdida cuadrática $L(y, H(x)) = (y-H(x))^2/2$, tenemos que 

$$\frac{\partial J}{\partial H(x_i)} = \frac{\partial \sum_i L(y_i, H(x_i))}{\partial H(x_i)}$$
$$= \frac{\partial  L(y_i, H(x_i))}{\partial H(x_i)} = H(x_i) - y_i$$

Por lo que podemos interpretar los residuos como gradientes negativos

$$y_i - H(x_i) = -\frac{\partial J}{\partial H(x_i)}$$


## Funciones de pérdida más habituales


<img src="C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/LossFunctions.png" width="50%" />



## Parámetro de *shrinkage*

En el algoritmo de **Gradient Boosting** podemos añadir una tasa de aprendizaje (shrinkage) $\nu\in (0,1)$ de forma que

$$H_t = H_{t-1}(x_i)+\nu \alpha_th_t(x_i)$$

Empíricamente se ha visto que **menores valores de $\nu$ obtienen un mejor error en test pero requieren un mayor número de iteraciones $T$**.

La mejor estrategia suele ser la de elegir una tasa de apredizaje pequeña $\nu < 0.1$ y seleccionar $T$ por parada temprana (*early stopping*).

## Otros métodos

Se pueden incorporar ideas de otros métodos:

> * **Stochastic gradient boosting** aplica la idea de los modelos de tipo **bagging**.

> * **Random forest**.

Los algoritmos de boosting suelen considerarse algoritmos caja negra (*black box*) que, aunque tienen un alto poder predictivo, son de difícil interpretación. 

Han surgido formas de interpretación de las variables relevantes en el modelo.



