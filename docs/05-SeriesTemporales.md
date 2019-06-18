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
