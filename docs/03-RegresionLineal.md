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







