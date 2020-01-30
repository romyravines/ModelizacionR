# Random Forests







El primer paso es crear una carpeta con nuestros modelos y resultados dentro de nuestro espacio de trabajo (proyecto).

Obtenemos el directorio de trabajo

```r
myWD <- getwd() 
```

elegimos un nombre para nuestra carpeta con resultados

```r
myWorkingFolderName <- 'ModelResults' 
```

Creamos la carpeta donde guardaremos nuestros resultados y ficheros

```r
dir.create( paste0(getwd(),"/",myWorkingFolderName))
```

```
## Warning in dir.create(paste0(getwd(), "/", myWorkingFolderName)): 'C:
## \Users\romy.rodriguez\Documents\INNOVA\Formacion\MiCurso\ModelizacionR
## \ModelResults' already exists
```

Cargamos librerías de trabajo

 - Funciones para entrenar modelos de ML

```r
if (!require(caret)) install.packages('caret')
```

```
## Loading required package: caret
```

```
## Loading required package: lattice
```

```
## Loading required package: ggplot2
```

```r
library(caret)        
```

 - Métricas de evaluación del poder de clasificación

```r
if (!require(ModelMetrics)) install.packages('ModelMetrics')
```

```
## Loading required package: ModelMetrics
```

```
## 
## Attaching package: 'ModelMetrics'
```

```
## The following objects are masked from 'package:caret':
## 
##     confusionMatrix, precision, recall, sensitivity, specificity
```

```r
library(ModelMetrics)
```

 - Para crear curvas ROC

```r
if (!require(ROCR)) install.packages('ROCR')
```

```
## Loading required package: ROCR
```

```
## Loading required package: gplots
```

```
## 
## Attaching package: 'gplots'
```

```
## The following object is masked from 'package:stats':
## 
##     lowess
```

```
## Loading required package: methods
```

```r
library(ROCR)
```

 - Cargamos la librería `insuranceData` que contiene los datos que utilizaremos

```r
if (!require(insuranceData)) install.packages('insuranceData')
```

```
## Loading required package: insuranceData
```

```r
library(insuranceData)
```

Para ver los contenidos de la librería ejecutamos:

```r
data(package='insuranceData')
```

Vemos que hay 10 datasets. Trabajaremos con el primero: `AutoBi` (Automobile Bodily Injury Claims)
Cargamos los datos de pérdidas en accidentes de coches:

```r
data("AutoBi")
```

Descripción de las variables del fichero de datos AutoBi: 
  
 - `Casenum`. Identificador de la reclamación (esta variable no se utiliza ya que no es una variable predictora)
 - `Attorney`.  Indica si el reclamante está representado por un abogado (1= Sí, 2 = No)
 - `Clmsex`.  Indicador del 'Sexo del reclamante' (1 = Hombre, 2 = Mujer)
 - `Marital`.  Indicador del 'Estado Civil del reclamante' (1 = Casado, 2 = Soltero, 3 = Viudo, 4 = divorciado/separado)
 - `Clminsur`.  Indica si el conductor del vehículo del reclamante estaba o no asegurado (1 = S?, 2 = No, 3 = No aplica)
 - `Seatbelt`.  Si el reclamante llevaba o no un cinturón de seguridad en el asiento infantil (1 = S?, 2 = No, 3 = No Aplica)
 - `Clmage`.  Edad del reclamante
 - `Loss (*)`.  La pérdida económica total del reclamante (en miles). Esta es la variable dependiente del conjunto de datos.
  

Revisamos el contenido de la tabla y el tipo de datos que contiene

```r
str(AutoBi)
```

```
## 'data.frame':	1340 obs. of  8 variables:
##  $ CASENUM : int  5 13 66 71 96 97 120 136 152 155 ...
##  $ ATTORNEY: int  1 2 2 1 2 1 1 1 2 2 ...
##  $ CLMSEX  : int  1 2 1 1 1 2 1 2 2 1 ...
##  $ MARITAL : int  NA 2 2 1 4 1 2 2 2 2 ...
##  $ CLMINSUR: int  2 1 2 2 2 2 2 2 2 2 ...
##  $ SEATBELT: int  1 1 1 2 1 1 1 1 1 1 ...
##  $ CLMAGE  : int  50 28 5 32 30 35 19 34 61 NA ...
##  $ LOSS    : num  34.94 10.892 0.33 11.037 0.138 ...
```
Estadísticas descriptivas básicas

```r
summary(AutoBi)
```

```
##     CASENUM         ATTORNEY         CLMSEX         MARITAL     
##  Min.   :    5   Min.   :1.000   Min.   :1.000   Min.   :1.000  
##  1st Qu.: 8579   1st Qu.:1.000   1st Qu.:1.000   1st Qu.:1.000  
##  Median :17453   Median :1.000   Median :2.000   Median :2.000  
##  Mean   :17213   Mean   :1.489   Mean   :1.559   Mean   :1.593  
##  3rd Qu.:25703   3rd Qu.:2.000   3rd Qu.:2.000   3rd Qu.:2.000  
##  Max.   :34253   Max.   :2.000   Max.   :2.000   Max.   :4.000  
##                                  NA's   :12      NA's   :16     
##     CLMINSUR        SEATBELT         CLMAGE           LOSS         
##  Min.   :1.000   Min.   :1.000   Min.   : 0.00   Min.   :   0.005  
##  1st Qu.:2.000   1st Qu.:1.000   1st Qu.:19.00   1st Qu.:   0.640  
##  Median :2.000   Median :1.000   Median :31.00   Median :   2.331  
##  Mean   :1.908   Mean   :1.017   Mean   :32.53   Mean   :   5.954  
##  3rd Qu.:2.000   3rd Qu.:1.000   3rd Qu.:43.00   3rd Qu.:   3.995  
##  Max.   :2.000   Max.   :2.000   Max.   :95.00   Max.   :1067.697  
##  NA's   :41      NA's   :48      NA's   :189
```
Para llamar directamente a las variables por sus nombres en la tabla AutoBi utilizamos el comando `attach`

```r
attach(AutoBi)
```
Para mirar la distribución de la variable target: LOSS (pérdida económica) 


```r
hist(LOSS, breaks=300 , probability = T)
lines(density(LOSS), col="red",main="Loss distribution")
```

<img src="09-RandomForests_files/figure-html4/unnamed-chunk-14-1.png" width="672" />


es una variable altamente asimétrica (con posibles outliers a la derecha)




Debido a la alta asimetría de la distribución de la variable target, utilizamos una medida robusta para segmentar los datos en dos clases: $1$ si las pérdidas son atípicamente altas o $0$ si no lo son.

```r
lsup <- median(LOSS) + 1.5*IQR(LOSS) # Criterio basado en estadisticos robustos
sum(LOSS>=lsup) # 153 datos de perdidas atipicamente altas
```

```
## [1] 153
```
  
Guardamos el gráfico del histograma para futuros análisis y/o reportes  

```r
  Path_to_graphics <- paste0(getwd(),"/","Graphics")
  dir.create(Path_to_graphics)
```

```
## Warning in dir.create(Path_to_graphics): 'C:\Users\romy.rodriguez\Documents
## \INNOVA\Formacion\MiCurso\ModelizacionR\Graphics' already exists
```

```r
  png(paste0(Path_to_graphics,"/histograma.png"))
  hist(LOSS[LOSS<lsup],breaks = 100,probability = T, xlab="loss (pérdida en miles $US)" , main="Datos de pérdida no severa")
  lines(density(LOSS[LOSS<lsup]),col="red")
  dev.off()
```

```
## png 
##   2
```



Creamos el dataset de trabajo.

Creamos un dataset de trabajo eliminando la variable CASENUM (id) y filtrando por la variable LOSS y el valor lsup= 72.22587 (miles)

```r
df_autobi <- AutoBi[ , -match("CASENUM", colnames(AutoBi)) ] 
```

Fijamos los predictores categóricos como factores:

 * Representado por un abogado: $1$ = representado por letrado y $2$ = no representado

```r
  df_autobi$ATTORNEY <- ordered(df_autobi$ATTORNEY, levels = 1:2) 
```

 * Sexo del reclamante: $1$ = hombre y $2$ = mujer

```r
  df_autobi$CLMSEX   <- ordered(df_autobi$CLMSEX  , levels = 1:2)
```

 * Estado civil del reclamante: $1$ = casado, $2$ = soltero, $3$ = viudo y $4$ = divorciado / separado


```r
  df_autobi$MARITAL  <- ordered(df_autobi$MARITAL , levels = 1:4)
```

 *  $1$ = vehículo estaba asegurado y $2$= no lo estaba

```r
  df_autobi$CLMINSUR <- ordered(df_autobi$CLMINSUR, levels = 1:2) 
```

 * $1$ = llevaba cinturón abrochado y $2$ = no lo llevaba

```r
  df_autobi$SEATBELT <- ordered(df_autobi$SEATBELT, levels = 1:2)
```

 * $1$= representado por letrado y $2$= no representado


```r
df_autobi$Y        <- ifelse(df_autobi$LOSS>= lsup,1,0)
```

Proporción de atípicos 11.42%

```r
summary(df_autobi)
```

```
##  ATTORNEY  CLMSEX    MARITAL    CLMINSUR    SEATBELT        CLMAGE     
##  1:685    1   :586   1   :624   1   : 120   1   :1270   Min.   : 0.00  
##  2:655    2   :742   2   :650   2   :1179   2   :  22   1st Qu.:19.00  
##           NA's: 12   3   : 15   NA's:  41   NA's:  48   Median :31.00  
##                      4   : 35                           Mean   :32.53  
##                      NA's: 16                           3rd Qu.:43.00  
##                                                         Max.   :95.00  
##                                                         NA's   :189    
##       LOSS                Y         
##  Min.   :   0.005   Min.   :0.0000  
##  1st Qu.:   0.640   1st Qu.:0.0000  
##  Median :   2.331   Median :0.0000  
##  Mean   :   5.954   Mean   :0.1142  
##  3rd Qu.:   3.995   3rd Qu.:0.0000  
##  Max.   :1067.697   Max.   :1.0000  
## 
```


(Opcional) Ahondamos un poco más en la relación de la pérdida con los factores.
Falta utilizar aggregate para estudiar las correlaciones entre las variables.

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

Resumen

```r
  agg_loss_attorney
```

```
##                 ATTORNEY     LOSS
## REPRESENTED            1 9.863109
## NOT REPRESENTED        2 1.864745
```

```r
  agg_loss_clmsex
```

```
##        CLMSEX     LOSS
## MALE        1 5.652647
## FEMALE      2 6.213765
```

```r
  agg_loss_marital 
```

```
##          MARITAL     LOSS
## MARRIED        1 7.878564
## SINGLE         2 4.130960
## WIDOW          3 3.483867
## DIVORCED       4 6.857743
```

```r
  agg_loss_clminsur
```

```
##             CLMINSUR     LOSS
## INSURED            1 4.626200
## NOT INSURED        2 6.182775
```

```r
  agg_loss_seatbelt
```

```
##              SEATBELT     LOSS
## SEATBELT            1  5.85892
## NOT SEATBELT        2 19.47986
```
  

Resumen combinando los tres mayores factores de riesgo (propensión)

```r
agg_loss_many <- aggregate(LOSS, by = list(SEATBELT,ATTORNEY,MARITAL) , FUN= mean , na.rm=TRUE)
dimnames(agg_loss_many)[[2]] <- c("SEATBELT","ATTORNEY","MARITAL","LOSS")
```


Aleatorizamos los datos y separamos el set de datos en train y test:

```r
N=nrow(df_autobi)
```


Es importante fijar una semilla para los algoritmos de aleatorización internos de R

```r
set.seed(123456)
inTrain  <- createDataPartition(df_autobi$Y, times = 1, p = 0.7, list = TRUE)
dt_train <- df_autobi[inTrain[[1]],]  # 938 casos
dt_test  <- df_autobi[-inTrain[[1]],] # 402 casos
  
nrow(dt_train)
```

```
## [1] 938
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
##       LOSS                Y         
##  Min.   :  0.0050   Min.   :0.0000  
##  1st Qu.:  0.7123   1st Qu.:0.0000  
##  Median :  2.3645   Median :0.0000  
##  Mean   :  5.4656   Mean   :0.1141  
##  3rd Qu.:  4.0263   3rd Qu.:0.0000  
##  Max.   :273.6040   Max.   :1.0000  
## 
```

```r
nrow(dt_test)
```

```
## [1] 402
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
##       LOSS                 Y         
##  Min.   :   0.0050   Min.   :0.0000  
##  1st Qu.:   0.5175   1st Qu.:0.0000  
##  Median :   2.1645   Median :0.0000  
##  Mean   :   7.0917   Mean   :0.1144  
##  3rd Qu.:   3.7782   3rd Qu.:0.0000  
##  Max.   :1067.6970   Max.   :1.0000  
## 
```

Comprobamos si se han colado algún dato del train en el test

```r
  length(intersect(inTrain, setdiff(1:N,inTrain)))
```

```
## [1] 0
```
O coincidencias  -> Ok





## A.  Entrenamiento del modelo RF-clasificación


```r
if (!require(randomForest)) install.packages('randomForest')
```

```
## Loading required package: randomForest
```

```
## randomForest 4.6-12
```

```
## Type rfNews() to see new features/changes/bug fixes.
```

```
## 
## Attaching package: 'randomForest'
```

```
## The following object is masked from 'package:ggplot2':
## 
##     margin
```

```r
library(randomForest)
```

Creamos un objeto de clase 'formula' y se lo pasamos como argumento a la función `randomForest`

```r
set.seed(123456)
fmla.rf1 <- as.formula(paste0("Y"," ~",paste0(colnames(df_autobi[,-c(7,8)]),collapse = "+"),collapse = ""))
rf1 <- randomForest( fmla.rf1,
                       data =dt_train,
                       ntree = 5000, 
# El procedimiento es bastante rapido con una muestra tan pequena por lo que podemos utilizar ntree > = 2500
                       replace =TRUE,
                       mtry=4,
                       maxnodes =50,
                       importance = TRUE,
                       proximity =   TRUE,
                       keep.forest = TRUE,
                       na.action=na.omit)
```

```
## Warning in randomForest.default(m, y, ...): The response has five or fewer
## unique values. Are you sure you want to do regression?
```

No se ha hecho tuning en los parámetros básicos del modelo, se puede hacer con `ntree`, `mtry` y `maxnodes`

```r
summary(rf1)
```

```
##                 Length Class  Mode     
## call                11 -none- call     
## type                 1 -none- character
## predicted          759 -none- numeric  
## mse               5000 -none- numeric  
## rsq               5000 -none- numeric  
## oob.times          759 -none- numeric  
## importance          12 -none- numeric  
## importanceSD         6 -none- numeric  
## localImportance      0 -none- NULL     
## proximity       576081 -none- numeric  
## ntree                1 -none- numeric  
## mtry                 1 -none- numeric  
## forest              11 -none- list     
## coefs                0 -none- NULL     
## y                  759 -none- numeric  
## test                 0 -none- NULL     
## inbag                0 -none- NULL     
## terms                3 terms  call     
## na.action          179 omit   numeric
```

```r
str(rf1)
```

```
## List of 19
##  $ call           : language randomForest(formula = fmla.rf1, data = dt_train, ntree = 5000, replace = TRUE,      mtry = 4, maxnodes = 50, imp| __truncated__ ...
##  $ type           : chr "regression"
##  $ predicted      : atomic [1:759] 0.007273 0.000081 0.675016 0.006908 0.017108 ...
##   ..- attr(*, "na.action")=Class 'omit'  Named int [1:179] 1 9 19 25 27 40 43 46 50 51 ...
##   .. .. ..- attr(*, "names")= chr [1:179] "1" "10" "24" "30" ...
##  $ mse            : num [1:5000] 0.104 0.119 0.117 0.12 0.121 ...
##  $ rsq            : num [1:5000] 0.0107 -0.1241 -0.112 -0.1409 -0.1452 ...
##  $ oob.times      : int [1:759] 1836 1929 1846 1829 1860 1811 1808 1830 1811 1797 ...
##  $ importance     : num [1:6, 1:2] 0.016961 -0.002353 0.004298 0.000851 0.001241 ...
##   ..- attr(*, "dimnames")=List of 2
##   .. ..$ : chr [1:6] "ATTORNEY" "CLMSEX" "MARITAL" "CLMINSUR" ...
##   .. ..$ : chr [1:2] "%IncMSE" "IncNodePurity"
##  $ importanceSD   : Named num [1:6] 1.63e-04 1.05e-04 1.70e-04 8.66e-05 5.90e-05 ...
##   ..- attr(*, "names")= chr [1:6] "ATTORNEY" "CLMSEX" "MARITAL" "CLMINSUR" ...
##  $ localImportance: NULL
##  $ proximity      : num [1:759, 1:759] 1 0.192 0 0.427 0 ...
##   ..- attr(*, "dimnames")=List of 2
##   .. ..$ : chr [1:759] "2" "3" "4" "5" ...
##   .. ..$ : chr [1:759] "2" "3" "4" "5" ...
##  $ ntree          : num 5000
##  $ mtry           : num 4
##  $ forest         :List of 11
##   ..$ ndbigtree    : int [1:5000] 99 99 99 99 99 99 99 99 99 99 ...
##   ..$ nodestatus   : int [1:99, 1:5000] -3 -3 -3 -3 -3 -3 -3 -1 -1 -3 ...
##   ..$ leftDaughter : int [1:99, 1:5000] 2 4 6 8 10 12 14 0 0 16 ...
##   ..$ rightDaughter: int [1:99, 1:5000] 3 5 7 9 11 13 15 0 0 17 ...
##   ..$ nodepred     : num [1:99, 1:5000] 1.26e-01 1.50e-02 1.66e-01 1.53e-16 2.61e-02 ...
##   ..$ bestvar      : int [1:99, 1:5000] 6 6 3 6 6 1 4 0 0 1 ...
##   ..$ xbestsplit   : num [1:99, 1:5000] 20.5 15.5 3.5 3.5 17.5 1.5 1.5 0 0 1.5 ...
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
##  $ y              : atomic [1:759] 1 0 1 0 0 0 0 0 0 1 ...
##   ..- attr(*, "na.action")=Class 'omit'  Named int [1:179] 1 9 19 25 27 40 43 46 50 51 ...
##   .. .. ..- attr(*, "names")= chr [1:179] "1" "10" "24" "30" ...
##  $ test           : NULL
##  $ inbag          : NULL
##  $ terms          :Classes 'terms', 'formula'  language Y ~ ATTORNEY + CLMSEX + MARITAL + CLMINSUR + SEATBELT + CLMAGE
##   .. ..- attr(*, "variables")= language list(Y, ATTORNEY, CLMSEX, MARITAL, CLMINSUR, SEATBELT, CLMAGE)
##   .. ..- attr(*, "factors")= int [1:7, 1:6] 0 1 0 0 0 0 0 0 0 1 ...
##   .. .. ..- attr(*, "dimnames")=List of 2
##   .. .. .. ..$ : chr [1:7] "Y" "ATTORNEY" "CLMSEX" "MARITAL" ...
##   .. .. .. ..$ : chr [1:6] "ATTORNEY" "CLMSEX" "MARITAL" "CLMINSUR" ...
##   .. ..- attr(*, "term.labels")= chr [1:6] "ATTORNEY" "CLMSEX" "MARITAL" "CLMINSUR" ...
##   .. ..- attr(*, "order")= int [1:6] 1 1 1 1 1 1
##   .. ..- attr(*, "intercept")= num 0
##   .. ..- attr(*, "response")= int 1
##   .. ..- attr(*, ".Environment")=<environment: R_GlobalEnv> 
##   .. ..- attr(*, "predvars")= language list(Y, ATTORNEY, CLMSEX, MARITAL, CLMINSUR, SEATBELT, CLMAGE)
##   .. ..- attr(*, "dataClasses")= Named chr [1:7] "numeric" "ordered" "ordered" "ordered" ...
##   .. .. ..- attr(*, "names")= chr [1:7] "Y" "ATTORNEY" "CLMSEX" "MARITAL" ...
##  $ na.action      :Class 'omit'  Named int [1:179] 1 9 19 25 27 40 43 46 50 51 ...
##   .. ..- attr(*, "names")= chr [1:179] "1" "10" "24" "30" ...
##  - attr(*, "class")= chr [1:2] "randomForest.formula" "randomForest"
```

Gráfico de la importancia relativa de los predictores

```r
  varImpPlot(rf1,sort = T,main = "Variable Importance")
```

<img src="09-RandomForests_files/figure-html4/unnamed-chunk-34-1.png" width="672" />

Gráfico del Error vs número de árboles

```r
  plot(rf1, main="Error de clasificación vs núero de  árboles") 
```

<img src="09-RandomForests_files/figure-html4/unnamed-chunk-35-1.png" width="672" />

Como valor predicho se obtiene la probabilidad condicional: $P(Y=1|X_1 = ATTORNEY,\ldots,X_6=SEATBELT)$

Miramos el resultado en el train:

```r
  rf1.prediction <- as.data.frame(predict(rf1, newdata = dt_train))
  dt_train$pred_rf1 <- rf1.prediction$`predict(rf1, newdata = dt_train)` 
  head(dt_train)
```

```
##   ATTORNEY CLMSEX MARITAL CLMINSUR SEATBELT CLMAGE   LOSS Y     pred_rf1
## 1        1      1    <NA>        2        1     50 34.940 1           NA
## 2        2      2       2        1        1     28 10.892 1 3.739339e-01
## 3        2      1       2        2        1      5  0.330 0 6.700844e-05
## 4        1      1       1        2        2     32 11.037 1 8.044245e-01
## 5        2      1       4        2        1     30  0.138 0 4.058483e-03
## 7        1      1       2        2        1     19  3.538 0 1.521188e-02
```

```r
  tail(dt_train)
```

```
##      ATTORNEY CLMSEX MARITAL CLMINSUR SEATBELT CLMAGE  LOSS Y     pred_rf1
## 1332        1      2       2        2        1     13 1.951 0 0.0007952672
## 1333        2      2       2        2        1     19 1.110 0 0.0006900556
## 1334        2      1       2        2        1     49 0.100 0 0.0062276082
## 1335        2      2       2        2        1     26 0.161 0 0.0007159829
## 1338        2      2       1        2        1     39 0.099 0 0.0080326490
## 1340        2      2       2        2        1     30 0.688 0 0.0011077024
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
##  Min.   :  0.0050   Min.   :0.0000   Min.   :0.00007  
##  1st Qu.:  0.7123   1st Qu.:0.0000   1st Qu.:0.00483  
##  Median :  2.3645   Median :0.0000   Median :0.03644  
##  Mean   :  5.4656   Mean   :0.1141   Mean   :0.12016  
##  3rd Qu.:  4.0263   3rd Qu.:0.0000   3rd Qu.:0.20796  
##  Max.   :273.6040   Max.   :1.0000   Max.   :0.80442  
##                                      NA's   :179
```

```r
  plot(density(dt_train$pred_rf1[!is.na(dt_train$pred_rf1)]), col="red" , xlab="Predicciones" , main="Función de densidad estimada")
```

<img src="09-RandomForests_files/figure-html4/unnamed-chunk-36-1.png" width="672" />

Vemos que hay (claramente) dos concentraciones (clases) de probabilidades de pérdida, una concentranción en torno a la probabilidad de pérdida no severa ($Y=0$) y otra para la pérdida severa ($Y=1$).

Esto no lleva a determinar la elección del punto de corte óptimo para obtener una regla de clasificación, es decir, un criterio para $Y_predicted=1$ (pérdida severa), o bien, para $Y_predicted=0$ (pérdida no severa). Para ello utilizaremos el criterio de la distancia de Kolmogorov-Smirnov (KS).

Calculamos la medida de "performance" del mecanismo (modelo) de predicción 'rf1'

```r
  rf1.pred <- prediction(as.numeric(rf1$predicted),as.numeric(rf1$y)) 
```
con el train creamos un objeto de tipo 'prediction'

```r
  rf1.perf <- performance(rf1.pred,"tpr","fpr") 
```


### Criterio de la distancia de KS: 

```r
rf1.perf@alpha.values[[1]][rf1.perf@alpha.values[[1]]==Inf] <- round(max(rf1.perf@alpha.values[[1]][rf1.perf@alpha.values[[1]]!=Inf]),2)
```


```r
  KS.matrix= cbind(abs(rf1.perf@y.values[[1]]-rf1.perf@x.values[[1]]), rf1.perf@alpha.values[[1]])
```

La distancia KS se calcula como: KS = abs(rf1.perf@y.values[[1]]-rf1.perf@x.values[[1]])


```r
colnames(KS.matrix) <- c("KS-distance","cut-point")
head(KS.matrix)
```

```
##      KS-distance cut-point
## [1,] 0.000000000 0.7800000
## [2,] 0.001497006 0.7826242
## [3,] 0.002994012 0.7452204
## [4,] 0.007994999 0.6750160
## [5,] 0.006497993 0.6717595
## [6,] 0.005000987 0.6529603
```

```r
ind.ks  <- sort( KS.matrix[,1] , index.return=TRUE )$ix[nrow(KS.matrix)] 
```

El punto de corte óptimo de KS

```r
  rf1.KScutoff <- KS.matrix[ind.ks,2] # := f(rf1.KS1)
# 0.04 - 0.05 
```


### Gráfico de la Curva ROC y su métrica asociada

Área bajo la curva ROC (AUC), para evaluar el poder de clasificación

Tenemos dos maneras de calcular el área bajo la curva ROC:

Cálculo de AUC mediante la función 'performance'

```r
rf1.auc1 <- performance(rf1.pred,"auc")@y.values[[1]]
```
0.737 - 0.738

Finalmente calculamos la curva ROC junto con la métrica AUC 

```r
#win.graph()
plot( rf1.perf , col='red'  , lwd=2, type="l", xlab="Tasa de falsos positivos" , ylab="Tasa de verdaderos positivos", main="Curva ROC modelo Random Forest")
abline( 0 , 1  , col="blue" , lwd=2, lty=2)
abline( 0 , 0 , 1 , col="gray40"   , lty=3)
legend( 0.4, 0.15 , c(paste0("AUC (Random Forest)=",round(rf1.auc1,4)),"AUC (clasificaci?n al azar)=0.50"),lty=c(1,2), lwd=c(2,2) ,col=c("red","blue"), bty="n")
```

<img src="09-RandomForests_files/figure-html4/unnamed-chunk-44-1.png" width="672" />

Para realizar el mismo gráfico de la curva ROC utilizando la librería `ggplot2`

```r
library("ggplot2")
```

Generamos los datos en un data.frame

```r
  df.perf <- data.frame(x=rf1.perf@x.values[[1]],y=rf1.perf@y.values[[1]])
```

Costrucción del objeto gráfico con ggplot2

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
```

Generamos el gráfico que hemos almacenado en la variable 'p':

```r
p
```

<img src="09-RandomForests_files/figure-html4/unnamed-chunk-48-1.png" width="672" />


Calculamos la predicción en el test y evaluamos el poder de clasificación del modelo

```r
rf1.pred_test     <- as.data.frame(predict( rf1, newdata = dt_test))
dt_test$pred_rf1  <- rf1.pred_test$`predict(rf1, newdata = dt_test)` 
```


```r
head(dt_test)
```

```
##    ATTORNEY CLMSEX MARITAL CLMINSUR SEATBELT CLMAGE   LOSS Y    pred_rf1
## 6         1      2       1        2        1     35  0.309 0 0.248381186
## 12        1      1       1        2        1     42 29.620 1 0.220724924
## 18        1      1       1        2        1     58  0.758 0 0.199511204
## 21        1      1       2     <NA>        1     37  3.200 0          NA
## 22        2      2       1        2        1     39  0.230 0 0.008032649
## 34        1      2       2        2        1     35  2.673 0 0.303488003
```


```r
tail(dt_test)
```

```
##      ATTORNEY CLMSEX MARITAL CLMINSUR SEATBELT CLMAGE  LOSS Y    pred_rf1
## 1326        1      2       2        2        1     19 3.269 0 0.001133968
## 1328        1      2       3        2        1     71 0.505 0 0.625923580
## 1330        2      2       2        2        1     33 1.535 0 0.001283937
## 1336        2      1       2        2        1     NA 0.576 0          NA
## 1337        1      2       1        2        1     46 3.705 0 0.431751239
## 1339        1      2       2        1        1     18 3.277 0 0.003433201
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
##  Min.   :   0.0050   Min.   :0.0000   Min.   :0.00007  
##  1st Qu.:   0.5175   1st Qu.:0.0000   1st Qu.:0.00615  
##  Median :   2.1645   Median :0.0000   Median :0.03819  
##  Mean   :   7.0917   Mean   :0.1144   Mean   :0.12815  
##  3rd Qu.:   3.7782   3rd Qu.:0.0000   3rd Qu.:0.22274  
##  Max.   :1067.6970   Max.   :1.0000   Max.   :0.78703  
##                                       NA's   :70
```


Con el test creamos un objeto de tipo 'prediction'

```r
dt_test.pred  <- prediction(as.numeric(rf1.pred_test$`predict(rf1, newdata = dt_test)`),dt_test$Y) 
dt_test.perf  <- performance(dt_test.pred,"tpr","fpr") 
```


Evaluación del poder de clasificación del modelo RF1 vía curva ROC

```r
rf1.test.auc <- performance(dt_test.pred ,"auc")@y.values[[1]]
```

Gráfico de la curva ROC para el test 

```r
#win.graph()
plot( dt_test.perf , col='red' , lwd=2, type="l" , main="Curva ROC modelo RF - test",xlab="Tasa de falsos positivos", ylab="Tasa de verdaderos positivos")
abline( 0 , 1  , col="blue" , lwd=2, lty=2)
abline( 0 , 0 , 1 , col="gray40"   , lty=3)
legend( 0.4, 0.2 , c(paste0("AUC (Random Forest)=",round(rf1.test.auc,4)),"AUC (Coin toss)=0.50") ,lty=c(1,2), lwd=c(2,2) ,col=c("red","blue"), bty="n")
```

<img src="09-RandomForests_files/figure-html4/unnamed-chunk-55-1.png" width="672" />



### Métrica de error del clasificador RF:

 - Error tipo I ($\alpha$): 22.50%, indica el error que se comete clasificando una pérdida 'severa' como 'no severa'
 - Error tipo II ($\beta$): 43.15%, indica el error que se comete clasificando una pérdida 'no severa' como 'severa'
 - % mala clasificación ($%mc$) : 40.66%, indica el % de veces que el modelo clasifica incorrectamente las pérdidas 
 - Accuracy = $100 - %$: 59.34%, indica el % de veces que el modelo acierta clasificando las pérdidas
 - Area bajo la curva ROC $AUC$: 0.6988, medida global del poder de clasificación del modelo
 - Finalmente calculamos la curva ROC junto con la métrica AUC

Una función útil para obtener rápidamente el análisis de un clasificador binario es la siguiente:

```r
library(binaryLogic)
metricBinaryClass( fitted.model = rf1 , dataset= dt_test , cutpoint=rf1.KScutoff , roc.graph=TRUE)
```

```
## $ClassError.tI
## [1] 22.5
## 
## $ClassError.tII
## [1] 44.52
## 
## $Accuracy
## [1] 58.13
## 
## $Sensitivity
## [1] 77.5
## 
## $Specificity
## [1] 55.48
## 
## $auc
## [1] 0.698887
## 
## $Fisher.F1
## [1] 30.8458
```

```r
metricBinaryClass(fitted.model = rf1 , dataset= dt_test , cutpoint=rf1.KScutoff , roc.graph=TRUE)
```

```
## $ClassError.tI
## [1] 22.5
## 
## $ClassError.tII
## [1] 44.52
## 
## $Accuracy
## [1] 58.13
## 
## $Sensitivity
## [1] 77.5
## 
## $Specificity
## [1] 55.48
## 
## $auc
## [1] 0.698887
## 
## $Fisher.F1
## [1] 30.8458
```




### Ejercicio propuesto:

 ¿Qué suecede con los errores de clasificación si cambiamos el punto de corte (**cutpoint**)? 

 1. Tomar una serie de valores distintos para el parámetro cutpoint y graficar las distintas curvas ROC resultantes
 1. Graficar la superficie formada por los errores tipo I y tipo II versus el punto de corte ¿Qué valor minimiza ambos errores?   





## B.  Entrenamiento del modelo RF-regresión


```r
fmla.rf2 <- as.formula(paste0('LOSS','~',paste0(colnames(df_autobi[,-c(7,8)]),collapse = "+"),collapse = ''))
set.seed(112233)

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
##  $ predicted      : atomic [1:759] 2.481 0.673 37.787 1.805 4.297 ...
##   ..- attr(*, "na.action")=Class 'omit'  Named int [1:179] 1 9 19 25 27 40 43 46 50 51 ...
##   .. .. ..- attr(*, "names")= chr [1:179] "1" "10" "24" "30" ...
##  $ mse            : num [1:5000] 543 499 466 463 501 ...
##  $ rsq            : num [1:5000] -0.636 -0.504 -0.403 -0.395 -0.511 ...
##  $ oob.times      : int [1:759] 1830 1844 1839 1796 1845 1867 1829 1890 1824 1844 ...
##  $ importance     : num [1:6, 1:2] 18.05 2.03 -3.02 4.07 6.17 ...
##   ..- attr(*, "dimnames")=List of 2
##   .. ..$ : chr [1:6] "ATTORNEY" "CLMSEX" "MARITAL" "CLMINSUR" ...
##   .. ..$ : chr [1:2] "%IncMSE" "IncNodePurity"
##  $ importanceSD   : Named num [1:6] 1.161 1.077 1.206 0.401 0.755 ...
##   ..- attr(*, "names")= chr [1:6] "ATTORNEY" "CLMSEX" "MARITAL" "CLMINSUR" ...
##  $ localImportance: NULL
##  $ proximity      : NULL
##  $ ntree          : num 5000
##  $ mtry           : num 4
##  $ forest         :List of 11
##   ..$ ndbigtree    : int [1:5000] 99 99 99 99 99 99 99 99 99 99 ...
##   ..$ nodestatus   : int [1:99, 1:5000] -3 -3 -3 -3 -3 -3 -3 -3 -1 -3 ...
##   ..$ leftDaughter : int [1:99, 1:5000] 2 4 6 8 10 12 14 16 0 18 ...
##   ..$ rightDaughter: int [1:99, 1:5000] 3 5 7 9 11 13 15 17 0 19 ...
##   ..$ nodepred     : num [1:99, 1:5000] 4.96 2.43 6.21 3.78 1.25 ...
##   ..$ bestvar      : int [1:99, 1:5000] 6 1 1 5 6 5 6 6 0 6 ...
##   ..$ xbestsplit   : num [1:99, 1:5000] 24.5 1.5 1.5 1.5 20.5 1.5 52.5 15.5 0 13.5 ...
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
##  $ y              : atomic [1:759] 10.892 0.33 11.037 0.138 3.538 ...
##   ..- attr(*, "na.action")=Class 'omit'  Named int [1:179] 1 9 19 25 27 40 43 46 50 51 ...
##   .. .. ..- attr(*, "names")= chr [1:179] "1" "10" "24" "30" ...
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
##  $ na.action      :Class 'omit'  Named int [1:179] 1 9 19 25 27 40 43 46 50 51 ...
##   .. ..- attr(*, "names")= chr [1:179] "1" "10" "24" "30" ...
##  - attr(*, "class")= chr [1:2] "randomForest.formula" "randomForest"
```


```r
varImpPlot(rf2,sort = T,main="Variable Importance")
```

<img src="09-RandomForests_files/figure-html4/unnamed-chunk-59-1.png" width="672" />


```r
rf2.prediction <- as.data.frame(predict(rf2, newdata = dt_test))
dt_test$pred_rf2 <- rf2.prediction[[1]] 
```


```r
head(dt_test)
```

```
##    ATTORNEY CLMSEX MARITAL CLMINSUR SEATBELT CLMAGE   LOSS Y    pred_rf1
## 6         1      2       1        2        1     35  0.309 0 0.248381186
## 12        1      1       1        2        1     42 29.620 1 0.220724924
## 18        1      1       1        2        1     58  0.758 0 0.199511204
## 21        1      1       2     <NA>        1     37  3.200 0          NA
## 22        2      2       1        2        1     39  0.230 0 0.008032649
## 34        1      2       2        2        1     35  2.673 0 0.303488003
##    pred_rf2
## 6  7.914495
## 12 8.488978
## 18 9.846624
## 21       NA
## 22 2.412385
## 34 8.022277
```


```r
tail(dt_test)
```

```
##      ATTORNEY CLMSEX MARITAL CLMINSUR SEATBELT CLMAGE  LOSS Y    pred_rf1
## 1326        1      2       2        2        1     19 3.269 0 0.001133968
## 1328        1      2       3        2        1     71 0.505 0 0.625923580
## 1330        2      2       2        2        1     33 1.535 0 0.001283937
## 1336        2      1       2        2        1     NA 0.576 0          NA
## 1337        1      2       1        2        1     46 3.705 0 0.431751239
## 1339        1      2       2        1        1     18 3.277 0 0.003433201
##       pred_rf2
## 1326  4.269137
## 1328 10.602377
## 1330  2.182710
## 1336        NA
## 1337 36.665696
## 1339  3.978978
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
##       LOSS                 Y             pred_rf1          pred_rf2      
##  Min.   :   0.0050   Min.   :0.0000   Min.   :0.00007   Min.   : 0.3916  
##  1st Qu.:   0.5175   1st Qu.:0.0000   1st Qu.:0.00615   1st Qu.: 2.0644  
##  Median :   2.1645   Median :0.0000   Median :0.03819   Median : 3.4303  
##  Mean   :   7.0917   Mean   :0.1144   Mean   :0.12815   Mean   : 6.3906  
##  3rd Qu.:   3.7782   3rd Qu.:0.0000   3rd Qu.:0.22274   3rd Qu.: 7.8241  
##  Max.   :1067.6970   Max.   :1.0000   Max.   :0.78703   Max.   :57.5121  
##                                       NA's   :70        NA's   :70
```


Graficamos la distribución de los valores estimados en el train

```r
plot(density(dt_test$pred_rf2[!is.na(dt_test$pred_rf2) & dt_test$pred_rf2 < 30]), ylim= c(0,.25) , col="red" , main="")
lines(density(dt_test$LOSS[dt_test$LOSS<30]),col="blue",lty=1)
```

<img src="09-RandomForests_files/figure-html4/unnamed-chunk-64-1.png" width="672" />


```r
modelchecktest1 <- as.data.frame( cbind(real=dt_test$LOSS , predicted=dt_test$pred_rf2) )
modelchecktest1[is.na(modelchecktest1)] <- 0

summary(modelchecktest1)
```

```
##       real             predicted     
##  Min.   :   0.0050   Min.   : 0.000  
##  1st Qu.:   0.5175   1st Qu.: 1.316  
##  Median :   2.1645   Median : 2.414  
##  Mean   :   7.0917   Mean   : 5.278  
##  3rd Qu.:   3.7782   3rd Qu.: 7.424  
##  Max.   :1067.6970   Max.   :57.512
```

Error de ajuste del modelo

```r
plot(modelchecktest1, xlim=c(0,100) , ylim=c(0,100) ,  pch="." , cex=1.5)
segments( 0, 0 , 100, 100 , col="red")
```

<img src="09-RandomForests_files/figure-html4/unnamed-chunk-66-1.png" width="672" />

```r
modelMetrics(real=modelchecktest1$real, pred=modelchecktest1$predicted )
```

```
## ------------------------------
```

```
##  Accuracy metrics (global):
```

```
## ------------------------------
```

```
## MAE(ref) = 8.9208
```

```
## MAE = 7.7751
```

```
## RMSE = 54.5763
```

```
## MAPE = 127.55
```

```
## MAPE(sim) = 68.64
```

```
## WMAPE = 109.64
```

```
## 
```

```
## 
```

```
## 
```

Commentario: El error de ajuste del modelo de regresión es demasiado alto: $RMSE= 54.57$ y el $MAPE=127.19%$
Con estos errores de predicción, es preferible utilizar a un modelo de clasificación en lugar de un     modelo de regresión.


# Gradient Boosting Models

## C.  Entrenamiento del modelo GBM-clasificación


```r
if (!require(gbm)) install.packages('gbm')
```

```
## Loading required package: gbm
```

```
## Loading required package: survival
```

```
## 
## Attaching package: 'survival'
```

```
## The following object is masked from 'package:caret':
## 
##     cluster
```

```
## Loading required package: splines
```

```
## Loading required package: parallel
```

```
## Loaded gbm 2.1.3
```

```r
library(gbm)
```

Creamos un objeto de clase 'formula' y se lo pasamos como argumento a la función `gbm`

```r
xVars <- colnames(AutoBi[ , -match(c("SEATBELT","CASENUM","LOSS"), colnames(AutoBi)) ])
targets <- c("LOSS","Y")

fmla.gbm1 <- as.formula(paste0(targets[2],"~",paste0(xVars,collapse = "+"),collapse = ""))

set.seed(999)

gbm1 <- gbm(fmla.gbm1,
            data = dt_train,
            distribution="bernoulli",
            n.trees = 5000,
            interaction.depth = 3,
            n.minobsinnode = 20,
            shrinkage = 0.001 )

summary(gbm1)
```

<img src="09-RandomForests_files/figure-html4/unnamed-chunk-68-1.png" width="672" />

```
##               var   rel.inf
## CLMAGE     CLMAGE 40.705984
## ATTORNEY ATTORNEY 26.230600
## MARITAL   MARITAL 23.253011
## CLMSEX     CLMSEX  5.331329
## CLMINSUR CLMINSUR  4.479076
```

Como valor estimado se obtiene la probabilidad condicional  $ P(Y=1|X_1=ATTORNEY,\ldots,X_6=SEATBELT)$

Miramos el resultado en el test:
  

```r
gbm1.prediction <- as.data.frame(predict.gbm(gbm1, newdata = dt_train , n.trees = 5000 , type="response"))

dt_train$pred_gbm1 <- gbm1.prediction[[1]]
```


Calculamos la medida de "performance" del mecanismo (modelo) de predicción 'gbm1'

```r
gbm1.pred <- prediction(dt_train$pred_gbm1,dt_train$Y) # con el train creamos un

gbm1.perf <- performance(gbm1.pred,"tpr","fpr") 
```

Calculamos el punto de corte

```r
gbm1.perf@alpha.values[[1]][gbm1.perf@alpha.values[[1]]==Inf] <- round(max(gbm1.perf@alpha.values[[1]][gbm1.perf@alpha.values[[1]]!=Inf]),2)

KS.matrix= cbind(abs(gbm1.perf@y.values[[1]]-gbm1.perf@x.values[[1]]), gbm1.perf@alpha.values[[1]])

colnames(KS.matrix) <- c("KS-distance","cut-point")
head(KS.matrix)
```

```
##      KS-distance cut-point
## [1,] 0.000000000 0.4900000
## [2,] 0.009345794 0.4940501
## [3,] 0.018691589 0.4661792
## [4,] 0.028037383 0.4474047
## [5,] 0.037383178 0.4411769
## [6,] 0.046728972 0.4358773
```

```r
ind.ks  <- sort( KS.matrix[,1] , index.return=TRUE )$ix[nrow(KS.matrix)]
```

El punto de corte óptimo de KS

```r
gbm1.KScutoff <- KS.matrix[ind.ks,2]
```

(Opcional) Area bajo la curva ROC

```r
gbm1.auc <- performance(gbm1.pred ,"auc")@y.values[[1]]
```

(opcional) Curva ROC GBM - train

```r
#win.graph()
plot( gbm1.perf , col='red' , lwd=2, type="l" , main="Curva ROC modelo GBM",xlab="Tasa de falsos positivos", ylab="Tasa de verdaderos positivos")
abline( 0 , 1 , col="blue" , lwd=2, lty=2)
abline( 0 , 0 , 1 , col="gray40"   , lty=3)
legend( 0.4  , 0.2 , c(paste0("AUC (Gradient Boosting)=",round(gbm1.auc,4)),"AUC (Coin toss)=0.50") ,lty=c(1,2), lwd=c(2,2) ,col=c("red","blue"), bty="n")
```

<img src="09-RandomForests_files/figure-html4/unnamed-chunk-74-1.png" width="672" />


Calculamos la predicción en el test y evaluamos el poder de clasificación del modelo

```r
gbm1.pred_test    <- as.data.frame(predict.gbm( gbm1, newdata = dt_test , n.trees = 5000 , type = "response"))
dt_test$pred_gbm1 <- gbm1.pred_test[[1]] 
```

Con el test creamos un objeto de tipo 'prediction'

```r
dt_gbm1.pred  <- prediction(dt_test$pred_gbm1,dt_test$Y) 
dt_gbm1.perf  <- performance(dt_gbm1.pred,"tpr","fpr") 
```

Evaluación del poder de clasificaci?n del modelo GBM1 vía curva ROC

```r
gbm1.auc_test <- performance( dt_gbm1.pred ,"auc")@y.values[[1]]
```

 - Error tipo I ($\alpha$):       10.87% indica el error que se comete clasificando una pérdida 'severa' como 'no severa'
 - Error tipo II ($\beta$):       39.89% indica el error que se comete clasificando una pérdida 'no severa' como 'severa'
 - % de mala clasificación:    35.81% nos da el % de veces que el modelo clasifica incorrectamente las pérdidas 
 - Accuracy = $100 - %mc$:        63.43% nos da el % de veces que el modelo acierta clasificando las pérdidas
 - Area bajo la curva ROC, $AUC$: 0.7583 nos da una medida global del poder de clasificación del modelo
 - Finalmente calculamos la curva ROC junto con la métrica AUC 

Evaluamos el poder de clasificación con la función `metricBinaryClass`:

```r
metricBinaryClass( fitted.model = gbm1 , dataset= dt_test , cutpoint=gbm1.KScutoff , roc.graph=TRUE)
```

```
## $ClassError.tI
## [1] 10.87
## 
## $ClassError.tII
## [1] 39.89
## 
## $Accuracy
## [1] 63.43
## 
## $Sensitivity
## [1] 89.13
## 
## $Specificity
## [1] 60.11
## 
## $auc
## [1] 0.7582743
## 
## $Fisher.F1
## [1] 35.8079
```



## D.  Entrenamiento del modelo GBM-regresión


```r
fmla.gbm2 <- as.formula(paste0(targets[1],"~",paste0(xVars,collapse = "+"),collapse = ""))

set.seed(1234)

gbm2 <- gbm(fmla.gbm2,
            data = dt_train,
            distribution="gaussian",
            n.trees = 5000,
            interaction.depth = 3,
            n.minobsinnode = 20,
            #keep.data = TRUE,
            #n.cores = 4,
            shrinkage = 0.001 )

summary(gbm2)
```

<img src="09-RandomForests_files/figure-html4/unnamed-chunk-79-1.png" width="672" />

```
##               var    rel.inf
## CLMAGE     CLMAGE 64.8214435
## ATTORNEY ATTORNEY 23.3523204
## CLMSEX     CLMSEX  6.3215371
## MARITAL   MARITAL  4.7362733
## CLMINSUR CLMINSUR  0.7684257
```


```r
str(gbm2)
```

```
## List of 27
##  $ initF            : num 5.47
##  $ fit              : num [1:938] 11.97 2.05 1.35 10.23 1.61 ...
##  $ train.error      : num [1:5000] 319 319 319 319 319 ...
##  $ valid.error      : num [1:5000] NaN NaN NaN NaN NaN NaN NaN NaN NaN NaN ...
##  $ oobag.improve    : num [1:5000] 0.0145 0.012 0.03 0.0255 0.0247 ...
##  $ trees            :List of 5000
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 45.5 0.0021 50.5 0.0278 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 7900 4925 0 6911 0 ...
##   .. ..$ : num [1:10] 469 240 163 50 20 30 50 27 229 469
##   .. ..$ : num [1:10] 0.000864 0.004873 0.002096 0.013409 0.027808 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 44 -0.000232 49.5 0.028103 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 5298 7330 0 7143 0 ...
##   .. ..$ : num [1:10] 469 237 149 57 20 37 57 31 232 469
##   .. ..$ : num [1:10] -0.000325 0.003 -0.000232 0.012877 0.028103 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 2 4 -1 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 0.5 45.5 0.00533 0.01206 ...
##   .. ..$ : int [1:10] 1 2 3 -1 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 6 4 -1 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 5 -1 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 3720 2928 1788 0 0 ...
##   .. ..$ : num [1:10] 469 239 104 63 29 12 133 2 230 469
##   .. ..$ : num [1:10] -8.76e-05 2.92e-03 7.46e-03 5.33e-03 1.21e-02 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00161 27.5 0.01629 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 7053 2517 0 3592 0 ...
##   .. ..$ : num [1:10] 469 234 53 151 29 122 151 30 235 469
##   .. ..$ : num [1:10] 0.000563 0.004449 -0.001607 0.006285 0.016288 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 22.5 -0.000873 39.5 0.004771 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 3299 1078 0 237 0 ...
##   .. ..$ : num [1:10] 469 234 69 143 70 73 143 22 235 469
##   .. ..$ : num [1:10] -0.000913 0.001745 -0.000873 0.003456 0.004771 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00115 39.5 0.00521 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 2849 943 0 777 0 ...
##   .. ..$ : num [1:10] 469 236 57 149 86 63 149 30 233 469
##   .. ..$ : num [1:10] -0.000706 0.001743 -0.001153 0.003258 0.005213 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00191 25.5 0.01062 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 3587 1657 0 1270 0 ...
##   .. ..$ : num [1:10] 469 240 57 157 23 134 157 26 229 469
##   .. ..$ : num [1:10] -0.000858 0.001844 -0.001908 0.003752 0.010618 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 36.5 -0.00105 49.5 0.0134 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 4957 6725 0 2773 0 ...
##   .. ..$ : num [1:10] 469 242 115 91 64 27 91 36 227 469
##   .. ..$ : num [1:10] -0.000127 0.003022 -0.001051 0.009813 0.013398 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00219 26.5 0.01675 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 6019 2933 0 2899 0 ...
##   .. ..$ : num [1:10] 469 238 52 161 21 140 161 25 231 469
##   .. ..$ : num [1:10] -0.000146 0.003384 -0.002187 0.005793 0.01675 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 24.5 -0.00182 29.5 0.0169 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 6574 3062 0 3460 0 ...
##   .. ..$ : num [1:10] 469 236 64 147 25 122 147 25 233 469
##   .. ..$ : num [1:10] 0.000358 0.004078 -0.00182 0.006184 0.016901 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 2 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00178 0.5 0.00216 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 3441 2088 0 1571 0 ...
##   .. ..$ : num [1:10] 469 241 59 154 87 66 1 28 228 469
##   .. ..$ : num [1:10] -0.000414 0.00217 -0.001777 0.004322 0.002157 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 23.5 -0.00141 28.5 0.01508 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 7256 3999 0 1637 0 ...
##   .. ..$ : num [1:10] 469 243 72 143 23 120 143 28 226 469
##   .. ..$ : num [1:10] 0.00104 0.00483 -0.00141 0.00735 0.01508 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 1 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00221 0.5 0.01001 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 4863 2877 0 2941 0 ...
##   .. ..$ : num [1:10] 469 240 56 165 80 84 1 19 229 469
##   .. ..$ : num [1:10] 0.000374 0.00369 -0.002208 0.005691 0.010006 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 25.5 -0.00151 45.5 0.00491 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 7366 3591 0 1414 0 ...
##   .. ..$ : num [1:10] 469 240 68 139 88 51 139 33 229 469
##   .. ..$ : num [1:10] 0.000652 0.004523 -0.001514 0.007339 0.004911 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 24.5 -0.00167 29.5 0.01367 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 4437 2093 0 2468 0 ...
##   .. ..$ : num [1:10] 469 235 71 139 26 113 139 25 234 469
##   .. ..$ : num [1:10] -0.00059 0.00248 -0.00167 0.00489 0.01367 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 25.5 -0.0019 49.5 0.011 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 7114 4988 0 2105 0 ...
##   .. ..$ : num [1:10] 469 221 78 118 86 32 118 25 248 469
##   .. ..$ : num [1:10] 0.000296 0.004422 -0.001896 0.00838 0.010957 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00253 26.5 0.01587 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 6340 3252 0 2387 0 ...
##   .. ..$ : num [1:10] 469 234 48 161 22 139 161 25 235 469
##   .. ..$ : num [1:10] 2.93e-05 3.71e-03 -2.53e-03 6.19e-03 1.59e-02 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 24.5 -0.000471 28.5 0.015072 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 5279 986 0 3005 0 ...
##   .. ..$ : num [1:10] 469 243 60 148 22 126 148 35 226 469
##   .. ..$ : num [1:10] -0.000205 0.003031 -0.000471 0.004289 0.015072 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 5.00e-01 3.55e+01 3.69e-04 4.95e+01 1.29e-02 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 6863 3955 0 2461 0 ...
##   .. ..$ : num [1:10] 469 243 119 98 59 39 98 26 226 469
##   .. ..$ : num [1:10] 0.000704 0.004393 0.000369 0.008873 0.012947 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00239 29.5 0.01112 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 6181 3050 0 1501 0 ...
##   .. ..$ : num [1:10] 469 235 62 148 35 113 148 25 234 469
##   .. ..$ : num [1:10] -7.18e-05 3.55e-03 -2.39e-03 5.40e-03 1.11e-02 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 44.5 0.00136 50.5 0.02481 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 6180 3686 0 7293 0 ...
##   .. ..$ : num [1:10] 469 238 153 53 22 31 53 32 231 469
##   .. ..$ : num [1:10] 0.000513 0.004089 0.001363 0.010884 0.024809 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.0015 27.5 0.00768 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 2605 737 0 981 0 ...
##   .. ..$ : num [1:10] 469 236 62 146 28 118 146 28 233 469
##   .. ..$ : num [1:10] -0.001365 0.000977 -0.001498 0.002354 0.007676 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 1 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00257 0.5 0.01423 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 9717 5060 0 3501 0 ...
##   .. ..$ : num [1:10] 469 230 55 147 63 82 2 28 239 469
##   .. ..$ : num [1:10] 0.00108 0.00576 -0.00257 0.00881 0.01423 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 4 -1 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 48.5 28.5 -0.00108 0.00235 ...
##   .. ..$ : int [1:10] 1 2 3 -1 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 6 4 -1 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 5 -1 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 3136 3340 485 0 0 ...
##   .. ..$ : num [1:10] 469 234 165 87 78 165 37 32 235 469
##   .. ..$ : num [1:10] -0.000687 0.001904 0.000541 -0.001083 0.002351 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.003 27.5 0.00866 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 2682 1364 0 1329 0 ...
##   .. ..$ : num [1:10] 469 233 49 160 31 129 160 24 236 469
##   .. ..$ : num [1:10] -0.00122 0.00119 -0.003 0.00278 0.00866 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 1 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 24.5 -0.00125 0.5 0.0123 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 5828 3937 0 2946 0 ...
##   .. ..$ : num [1:10] 469 229 65 132 63 68 1 32 240 469
##   .. ..$ : num [1:10] 0.00026 0.00389 -0.00125 0.00748 0.0123 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 2 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 19.5 -0.002443 0.5 -0.000531 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 2448 2049 0 761 0 ...
##   .. ..$ : num [1:10] 469 232 46 161 101 59 1 25 237 469
##   .. ..$ : num [1:10] -0.001076 0.001241 -0.002443 0.001122 -0.000531 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 19.5 -0.0034 26.5 0.0124 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 4740 2323 0 2284 0 ...
##   .. ..$ : num [1:10] 469 229 46 158 27 131 158 25 240 469
##   .. ..$ : num [1:10] -0.000457 0.002797 -0.003401 0.004044 0.012419 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00224 46.5 0.0085 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 4334 3446 0 2334 0 ...
##   .. ..$ : num [1:10] 469 240 57 151 100 51 151 32 229 469
##   .. ..$ : num [1:10] -0.000184 0.002785 -0.002242 0.005694 0.008501 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 1 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 26.5 -0.00172 0.5 0.0091 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 4774 2261 0 1515 0 ...
##   .. ..$ : num [1:10] 469 223 70 127 54 73 127 26 246 469
##   .. ..$ : num [1:10] -0.000366 0.002985 -0.001721 0.005081 0.009097 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 2 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 19.5 -0.00307 0.5 0.00207 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 4113 1878 0 1462 0 ...
##   .. ..$ : num [1:10] 469 224 42 162 98 63 1 20 245 469
##   .. ..$ : num [1:10] -0.000868 0.00217 -0.003068 0.003881 0.002071 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00251 45.5 0.00707 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 9162 6704 0 2249 0 ...
##   .. ..$ : num [1:10] 469 233 52 150 110 40 150 31 236 469
##   .. ..$ : num [1:10] 0.000991 0.005439 -0.002506 0.009404 0.007069 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 25.5 -0.00157 39.5 0.00762 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 4671 1901 0 1382 0 ...
##   .. ..$ : num [1:10] 469 240 67 147 70 77 147 26 229 469
##   .. ..$ : num [1:10] -0.000188 0.002895 -0.001572 0.004405 0.007621 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 5.00e-01 4.45e+01 7.67e-05 5.15e+01 2.27e-02 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 4479 4459 0 6005 0 ...
##   .. ..$ : num [1:10] 469 237 154 51 23 28 51 32 232 469
##   .. ..$ : num [1:10] -6.34e-05 2.99e-03 7.67e-05 1.07e-02 2.27e-02 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 24.5 -0.00115 50.5 0.01146 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 12508 4407 0 2265 0 ...
##   .. ..$ : num [1:10] 469 231 55 148 112 36 148 28 238 469
##   .. ..$ : num [1:10] 0.00139 0.00664 -0.00115 0.00924 0.01146 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00171 38.5 0.00453 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 6772 4302 0 1051 0 ...
##   .. ..$ : num [1:10] 469 242 56 151 74 77 151 35 227 469
##   .. ..$ : num [1:10] 0.000271 0.003951 -0.00171 0.007221 0.00453 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 28.5 -0.00174 39.5 0.00709 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 4184 2238 0 529 0 ...
##   .. ..$ : num [1:10] 469 231 84 118 45 73 118 29 238 469
##   .. ..$ : num [1:10] -0.000693 0.002339 -0.001738 0.004398 0.007094 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 2 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00262 0.5 0.00277 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 3121 1286 0 695 0 ...
##   .. ..$ : num [1:10] 469 241 56 153 100 52 1 32 228 469
##   .. ..$ : num [1:10] -0.00122 0.00124 -0.00262 0.00277 0.00277 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00279 26.5 0.01398 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 3222 1607 0 2974 0 ...
##   .. ..$ : num [1:10] 469 219 46 149 23 126 149 24 250 469
##   .. ..$ : num [1:10] -0.00111 0.0017 -0.00279 0.00352 0.01398 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00194 27.5 0.02064 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 8382 4599 0 3920 0 ...
##   .. ..$ : num [1:10] 469 232 54 153 22 131 153 25 237 469
##   .. ..$ : num [1:10] 0.000875 0.005148 -0.001937 0.008287 0.020639 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.000762 46.5 0.006156 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 6943 1982 0 672 0 ...
##   .. ..$ : num [1:10] 469 237 51 166 118 48 166 20 232 469
##   .. ..$ : num [1:10] 0.000279 0.004086 -0.000762 0.004873 0.006156 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00184 25.5 0.01085 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 2753 1208 0 1409 0 ...
##   .. ..$ : num [1:10] 469 242 60 156 21 135 156 26 227 469
##   .. ..$ : num [1:10] -0.000741 0.001605 -0.001844 0.003229 0.01085 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 24.5 -0.0013 29.5 0.0162 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 5927 2328 0 3352 0 ...
##   .. ..$ : num [1:10] 469 238 64 144 26 118 144 30 231 469
##   .. ..$ : num [1:10] 0.000301 0.003804 -0.001303 0.005915 0.016193 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 1 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00183 0.5 0.00948 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 6121 2674 0 1992 0 ...
##   .. ..$ : num [1:10] 469 249 55 167 74 92 1 27 220 469
##   .. ..$ : num [1:10] 0.000837 0.004251 -0.00183 0.0057 0.009478 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 23.5 -0.00168 49.5 0.00946 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 6900 4415 0 1804 0 ...
##   .. ..$ : num [1:10] 469 231 64 151 116 35 151 16 238 469
##   .. ..$ : num [1:10] 0.000697 0.004809 -0.001679 0.007559 0.009458 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 24.5 -0.00157 49.5 0.0114 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 9300 4591 0 3335 0 ...
##   .. ..$ : num [1:10] 469 240 63 144 108 36 144 33 229 469
##   .. ..$ : num [1:10] 0.00134 0.00568 -0.00157 0.00862 0.0114 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 24.5 -0.00182 45.5 0.00567 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 8558 4067 0 1561 0 ...
##   .. ..$ : num [1:10] 469 231 59 143 95 48 143 29 238 469
##   .. ..$ : num [1:10] 0.000917 0.005253 -0.001824 0.008022 0.005673 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 44.5 0.00122 50.5 0.02602 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 5505 4003 0 7407 0 ...
##   .. ..$ : num [1:10] 469 240 175 46 20 26 46 19 229 469
##   .. ..$ : num [1:10] -0.000079 0.003367 0.001216 0.011548 0.026016 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 45.5 0.00159 50.5 0.02638 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 7504 4850 0 6132 0 ...
##   .. ..$ : num [1:10] 469 230 157 50 20 30 50 23 239 469
##   .. ..$ : num [1:10] 0.000405 0.004482 0.001587 0.012813 0.026376 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 44.5 0.00146 50.5 0.01977 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 5981 2849 0 3758 0 ...
##   .. ..$ : num [1:10] 469 221 152 44 21 23 44 25 248 469
##   .. ..$ : num [1:10] 3.83e-05 3.82e-03 1.46e-03 1.01e-02 1.98e-02 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 23.5 -0.00212 29.5 0.01625 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 6405 3273 0 2848 0 ...
##   .. ..$ : num [1:10] 469 238 66 144 24 120 144 28 231 469
##   .. ..$ : num [1:10] 0.000197 0.003838 -0.002121 0.006307 0.01625 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00197 49.5 0.00906 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 8136 3737 0 1609 0 ...
##   .. ..$ : num [1:10] 469 250 56 166 131 35 166 28 219 469
##   .. ..$ : num [1:10] 0.00129 0.00518 -0.00197 0.00745 0.00906 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 1 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 19.5 -0.00199 0.5 0.00699 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 4726 1601 0 1119 0 ...
##   .. ..$ : num [1:10] 469 242 54 157 74 83 157 31 227 469
##   .. ..$ : num [1:10] -0.000267 0.002807 -0.00199 0.004164 0.006992 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00158 26.5 0.0197 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 5529 1767 0 5991 0 ...
##   .. ..$ : num [1:10] 469 238 44 164 24 140 164 30 231 469
##   .. ..$ : num [1:10] -5.26e-05 3.33e-03 -1.58e-03 5.11e-03 1.97e-02 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00199 49.5 0.00951 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 8382 4196 0 1925 0 ...
##   .. ..$ : num [1:10] 469 245 49 169 132 37 169 27 224 469
##   .. ..$ : num [1:10] 0.000944 0.004986 -0.001988 0.007725 0.009512 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.000801 27.5 0.01522 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 5188 2594 0 3272 0 ...
##   .. ..$ : num [1:10] 469 232 53 150 32 118 150 29 237 469
##   .. ..$ : num [1:10] 0.000417 0.003779 -0.000801 0.006251 0.01522 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 1 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00174 0.5 0.00621 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 5098 1840 0 581 0 ...
##   .. ..$ : num [1:10] 469 244 51 161 78 82 1 32 225 469
##   .. ..$ : num [1:10] 0.000221 0.003403 -0.001735 0.004363 0.006206 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 1 4 -1 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 0.5 20.5 -0.000393 0.011328 ...
##   .. ..$ : int [1:10] 1 2 3 -1 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 6 4 -1 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 5 -1 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 6347 1967 3348 0 0 ...
##   .. ..$ : num [1:10] 469 230 107 27 67 13 118 5 239 469
##   .. ..$ : num [1:10] 0.0004 0.004429 0.007961 -0.000393 0.011328 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 2 4 -1 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 0.5 39.5 0.005 -0.00034 ...
##   .. ..$ : int [1:10] 1 2 3 -1 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 6 4 -1 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 5 -1 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 1927 720 903 0 0 ...
##   .. ..$ : num [1:10] 469 240 121 45 64 12 117 2 229 469
##   .. ..$ : num [1:10] -0.001538 0.000364 0.001864 0.004999 -0.00034 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 45.5 0.0019 52.5 0.023 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 6371 2295 0 6939 0 ...
##   .. ..$ : num [1:10] 469 238 166 48 21 27 48 24 231 469
##   .. ..$ : num [1:10] 0.000272 0.003903 0.001904 0.009354 0.022987 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 5.00e-01 3.85e+01 7.21e-04 4.95e+01 1.64e-02 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 7752 4706 0 3177 0 ...
##   .. ..$ : num [1:10] 469 233 128 80 42 38 80 25 236 469
##   .. ..$ : num [1:10] 0.000551 0.004643 0.000721 0.010425 0.016419 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 2 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.000873 0.5 0.001113 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 2671 770 0 862 0 ...
##   .. ..$ : num [1:10] 469 245 53 164 103 59 2 28 224 469
##   .. ..$ : num [1:10] -0.000825 0.001416 -0.000873 0.002617 0.001113 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.000896 26.5 0.014155 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 5980 1908 0 2064 0 ...
##   .. ..$ : num [1:10] 469 239 57 157 24 133 157 25 230 469
##   .. ..$ : num [1:10] 0.000126 0.003628 -0.000896 0.005619 0.014155 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 1 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 25.5 -0.00112 0.5 0.00568 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 3806 1804 0 667 0 ...
##   .. ..$ : num [1:10] 469 235 76 135 62 73 135 24 234 469
##   .. ..$ : num [1:10] -0.000509 0.002334 -0.001122 0.003266 0.005678 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 24.5 -0.00142 29.5 0.01399 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 4471 1815 0 2557 0 ...
##   .. ..$ : num [1:10] 469 238 68 139 23 116 139 31 231 469
##   .. ..$ : num [1:10] -0.000158 0.002884 -0.001423 0.004354 0.013985 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 5.00e-01 3.55e+01 5.24e-04 4.95e+01 1.32e-02 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 5840 4095 0 3328 0 ...
##   .. ..$ : num [1:10] 469 232 111 90 62 28 90 31 237 469
##   .. ..$ : num [1:10] 0.000298 0.003865 0.000524 0.009142 0.013228 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 1 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00228 0.5 0.00852 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 4674 2474 0 1427 0 ...
##   .. ..$ : num [1:10] 469 235 50 152 69 81 2 33 234 469
##   .. ..$ : num [1:10] -0.000267 0.002914 -0.002284 0.005304 0.008521 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00187 26.5 0.01551 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 7871 3472 0 2850 0 ...
##   .. ..$ : num [1:10] 469 239 57 157 29 128 157 25 230 469
##   .. ..$ : num [1:10] 0.000786 0.004805 -0.001875 0.006556 0.015507 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 5.00e-01 4.45e+01 3.92e-04 4.95e+01 2.05e-02 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 5388 4875 0 3937 0 ...
##   .. ..$ : num [1:10] 469 238 157 51 24 27 51 30 231 469
##   .. ..$ : num [1:10] 0.000172 0.003512 0.000392 0.011231 0.02055 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 41.5 -0.000539 50.5 0.009554 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 3475 2528 0 1065 0 ...
##   .. ..$ : num [1:10] 469 227 138 60 30 30 60 29 242 469
##   .. ..$ : num [1:10] -0.000732 0.002079 -0.000539 0.005341 0.009554 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 25.5 -0.00148 44.5 0.00499 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 6017 4135 0 1416 0 ...
##   .. ..$ : num [1:10] 469 228 76 128 86 42 128 24 241 469
##   .. ..$ : num [1:10] -0.000125 0.003557 -0.00148 0.007314 0.004989 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 28.5 -0.000812 39.5 0.006457 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 2485 1281 0 554 0 ...
##   .. ..$ : num [1:10] 469 225 85 112 48 64 112 28 244 469
##   .. ..$ : num [1:10] -0.000904 0.001493 -0.000812 0.003889 0.006457 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 1 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 0.5 -0.000366 45.5 0.001928 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 4469 1922 0 3291 0 ...
##   .. ..$ : num [1:10] 469 244 113 129 90 21 18 2 225 469
##   .. ..$ : num [1:10] -0.00065 0.002072 -0.000366 0.004207 0.001928 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.002 26.5 0.0189 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 5853 3523 0 3943 0 ...
##   .. ..$ : num [1:10] 469 229 53 148 23 125 148 28 240 469
##   .. ..$ : num [1:10] 0.000382 0.003999 -0.002004 0.006863 0.018897 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00252 26.5 0.01704 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 5228 2836 0 4811 0 ...
##   .. ..$ : num [1:10] 469 231 50 158 26 132 158 23 238 469
##   .. ..$ : num [1:10] 0.000166 0.003555 -0.002517 0.004608 0.017041 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 24.5 -0.00177 50.5 0.00627 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 3484 2429 0 692 0 ...
##   .. ..$ : num [1:10] 469 233 68 133 109 24 133 32 236 469
##   .. ..$ : num [1:10] -0.000312 0.002431 -0.001767 0.005197 0.006268 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00138 27.5 0.02008 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 6832 4003 0 4887 0 ...
##   .. ..$ : num [1:10] 469 233 53 153 26 127 153 27 236 469
##   .. ..$ : num [1:10] 0.000753 0.004594 -0.001382 0.007587 0.020079 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00219 25.5 0.01141 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 3364 1359 0 1700 0 ...
##   .. ..$ : num [1:10] 469 238 55 161 23 138 161 22 231 469
##   .. ..$ : num [1:10] -0.00077 0.00187 -0.00219 0.00345 0.01141 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 1 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00112 0.5 0.00963 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 4814 2767 0 2209 0 ...
##   .. ..$ : num [1:10] 469 236 50 158 73 84 1 28 233 469
##   .. ..$ : num [1:10] 7.27e-05 3.28e-03 -1.12e-03 5.69e-03 9.63e-03 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 24.5 -0.00121 49.5 0.01287 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 11851 5654 0 3121 0 ...
##   .. ..$ : num [1:10] 469 226 68 134 97 37 134 24 243 469
##   .. ..$ : num [1:10] 0.00117 0.00638 -0.00121 0.00989 0.01287 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 2 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 26.5 -0.001349 0.5 0.000432 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 2667 1337 0 832 0 ...
##   .. ..$ : num [1:10] 469 229 71 129 85 44 129 29 240 469
##   .. ..$ : num [1:10] -0.000778 0.001663 -0.001349 0.002259 0.000432 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 1 4 -1 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 0.5 27.5 0.01095 0.00458 ...
##   .. ..$ : int [1:10] 1 2 3 -1 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 6 4 -1 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 5 -1 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 4014 1634 2372 0 0 ...
##   .. ..$ : num [1:10] 469 249 117 38 62 17 127 5 220 469
##   .. ..$ : num [1:10] 0.000493 0.003592 0.007004 0.010951 0.004585 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 25.5 -0.00161 29.5 0.01189 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 4077 2064 0 1530 0 ...
##   .. ..$ : num [1:10] 469 243 73 143 24 119 143 27 226 469
##   .. ..$ : num [1:10] -0.00002 0.00282 -0.00161 0.0046 0.01189 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00184 48.5 0.00592 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 5126 2071 0 623 0 ...
##   .. ..$ : num [1:10] 469 231 51 154 118 36 154 26 238 469
##   .. ..$ : num [1:10] 0.000254 0.00361 -0.001838 0.004806 0.005917 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00159 26.5 0.01423 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 4014 1539 0 3002 0 ...
##   .. ..$ : num [1:10] 469 231 54 150 25 125 150 27 238 469
##   .. ..$ : num [1:10] 5.09e-05 3.02e-03 -1.59e-03 4.23e-03 1.42e-02 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 44.5 0.00211 49.5 0.0208 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 6951 3086 0 3960 0 ...
##   .. ..$ : num [1:10] 469 231 147 52 23 29 52 32 238 469
##   .. ..$ : num [1:10] 0.000714 0.004621 0.002111 0.010999 0.020798 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 19.5 -0.00081 39.5 0.00481 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 3887 767 0 677 0 ...
##   .. ..$ : num [1:10] 469 243 47 164 82 82 164 32 226 469
##   .. ..$ : num [1:10] -0.000365 0.002411 -0.00081 0.002781 0.004812 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 24.5 -0.000711 48.5 0.008142 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 4873 2528 0 1678 0 ...
##   .. ..$ : num [1:10] 469 236 69 138 102 36 138 29 233 469
##   .. ..$ : num [1:10] 0.000112 0.003315 -0.000711 0.006071 0.008142 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00194 27.5 0.01144 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 5845 2240 0 1166 0 ...
##   .. ..$ : num [1:10] 469 234 53 149 24 125 149 32 235 469
##   .. ..$ : num [1:10] 0.000152 0.00369 -0.00194 0.005052 0.011436 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 1 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00181 0.5 0.00623 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 4477 1884 0 736 0 ...
##   .. ..$ : num [1:10] 469 232 57 152 74 78 152 23 237 469
##   .. ..$ : num [1:10] -0.000241 0.002882 -0.001807 0.00397 0.006229 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.0018 26.5 0.0141 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 4589 1916 0 2345 0 ...
##   .. ..$ : num [1:10] 469 219 46 144 24 120 144 29 250 469
##   .. ..$ : num [1:10] -0.000332 0.00301 -0.001795 0.005103 0.014126 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 25.5 -0.0015 39.5 0.00756 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 4207 2205 0 1284 0 ...
##   .. ..$ : num [1:10] 469 234 74 132 64 68 132 28 235 469
##   .. ..$ : num [1:10] -0.00017 0.00283 -0.0015 0.00434 0.00756 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00207 26.5 0.01923 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 3765 1796 0 5841 0 ...
##   .. ..$ : num [1:10] 469 240 52 162 22 140 162 26 229 469
##   .. ..$ : num [1:10] -0.000577 0.002191 -0.002068 0.00408 0.019227 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 45.5 0.00215 51.5 0.0245 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 8359 4833 0 5478 0 ...
##   .. ..$ : num [1:10] 469 225 153 50 23 27 50 22 244 469
##   .. ..$ : num [1:10] 0.000826 0.005222 0.002152 0.013157 0.024498 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 36.5 -0.000845 40.5 0.009234 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 2374 1487 0 1600 0 ...
##   .. ..$ : num [1:10] 469 230 119 86 24 62 86 25 239 469
##   .. ..$ : num [1:10] -0.001093 0.001201 -0.000845 0.002302 0.009234 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00203 46.5 0.00808 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 4934 3277 0 1289 0 ...
##   .. ..$ : num [1:10] 469 237 59 150 100 50 150 28 232 469
##   .. ..$ : num [1:10] -2.05e-05 3.19e-03 -2.03e-03 6.01e-03 8.08e-03 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 20.5 -0.00171 26.5 0.01493 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 3748 2231 0 3030 0 ...
##   .. ..$ : num [1:10] 469 239 63 144 25 119 144 32 230 469
##   .. ..$ : num [1:10] -0.000313 0.002461 -0.00171 0.004922 0.01493 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 45.5 0.00106 50.5 0.02232 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 5609 3575 0 5289 0 ...
##   .. ..$ : num [1:10] 469 237 159 49 21 28 49 29 232 469
##   .. ..$ : num [1:10] 0.000287 0.003708 0.001064 0.010325 0.022321 ...
##   ..$ :List of 8
##   .. ..$ : int [1:10] 0 4 -1 1 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 0.5 25.5 -0.00171 0.5 0.00663 ...
##   .. ..$ : int [1:10] 1 2 -1 4 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 8 3 -1 5 -1 -1 -1 -1 -1 -1
##   .. ..$ : int [1:10] 9 7 -1 6 -1 -1 -1 -1 -1 -1
##   .. ..$ : num [1:10] 3512 1862 0 915 0 ...
##   .. ..$ : num [1:10] 469 237 72 133 68 65 133 32 232 469
##   .. ..$ : num [1:10] -0.000227 0.00248 -0.001714 0.004061 0.006625 ...
##   .. [list output truncated]
##  $ c.splits         : list()
##  $ bag.fraction     : num 0.5
##  $ distribution     :List of 1
##   ..$ name: chr "gaussian"
##  $ interaction.depth: num 3
##  $ n.minobsinnode   : num 20
##  $ num.classes      : num 1
##  $ n.trees          : num 5000
##  $ nTrain           : num 938
##  $ train.fraction   : num 1
##  $ response.name    : chr "LOSS"
##  $ shrinkage        : num 0.001
##  $ var.levels       :List of 5
##   ..$ : chr [1:2] "1" "2"
##   ..$ : chr [1:2] "1" "2"
##   ..$ : chr [1:4] "1" "2" "3" "4"
##   ..$ : chr [1:2] "1" "2"
##   ..$ : Named num [1:11] 0 13 18 21.9 27 ...
##   .. ..- attr(*, "names")= chr [1:11] "0%" "10%" "20%" "30%" ...
##  $ var.monotone     : num [1:5] 0 0 0 0 0
##  $ var.names        : chr [1:5] "ATTORNEY" "CLMSEX" "MARITAL" "CLMINSUR" ...
##  $ var.type         : num [1:5] 0 0 0 0 0
##  $ verbose          : logi FALSE
##  $ data             :List of 6
##   ..$ y      : Named num [1:938] 34.94 10.892 0.33 11.037 0.138 ...
##   .. ..- attr(*, "names")= chr [1:938] "1" "2" "3" "4" ...
##   ..$ x      : num [1:4690] 0 1 1 0 1 0 0 1 1 0 ...
##   ..$ x.order: num [1:938, 1:5] 0 3 5 6 9 11 16 17 18 22 ...
##   .. ..- attr(*, "dimnames")=List of 2
##   .. .. ..$ : NULL
##   .. .. ..$ : chr [1:5] "ATTORNEY" "CLMSEX" "MARITAL" "CLMINSUR" ...
##   ..$ offset : logi NA
##   ..$ Misc   : logi NA
##   ..$ w      : num [1:938] 1 1 1 1 1 1 1 1 1 1 ...
##  $ Terms            :Classes 'terms', 'formula'  language LOSS ~ ATTORNEY + CLMSEX + MARITAL + CLMINSUR + CLMAGE
##   .. ..- attr(*, "variables")= language list(LOSS, ATTORNEY, CLMSEX, MARITAL, CLMINSUR, CLMAGE)
##   .. ..- attr(*, "factors")= int [1:6, 1:5] 0 1 0 0 0 0 0 0 1 0 ...
##   .. .. ..- attr(*, "dimnames")=List of 2
##   .. .. .. ..$ : chr [1:6] "LOSS" "ATTORNEY" "CLMSEX" "MARITAL" ...
##   .. .. .. ..$ : chr [1:5] "ATTORNEY" "CLMSEX" "MARITAL" "CLMINSUR" ...
##   .. ..- attr(*, "term.labels")= chr [1:5] "ATTORNEY" "CLMSEX" "MARITAL" "CLMINSUR" ...
##   .. ..- attr(*, "order")= int [1:5] 1 1 1 1 1
##   .. ..- attr(*, "intercept")= int 1
##   .. ..- attr(*, "response")= int 1
##   .. ..- attr(*, ".Environment")=<environment: R_GlobalEnv> 
##   .. ..- attr(*, "predvars")= language list(LOSS, ATTORNEY, CLMSEX, MARITAL, CLMINSUR, CLMAGE)
##   .. ..- attr(*, "dataClasses")= Named chr [1:6] "numeric" "ordered" "ordered" "ordered" ...
##   .. .. ..- attr(*, "names")= chr [1:6] "LOSS" "ATTORNEY" "CLMSEX" "MARITAL" ...
##  $ cv.folds         : num 0
##  $ call             : language gbm(formula = fmla.gbm2, distribution = "gaussian", data = dt_train,      n.trees = 5000, interaction.depth = 3, | __truncated__
##  $ m                : language model.frame(formula = fmla.gbm2, data = dt_train, drop.unused.levels = TRUE,      na.action = function (object, ...)  ...
##  - attr(*, "class")= chr "gbm"
```

Como valor estimado se obtiene la esperanza condicional de la pérdida: $E(LOSS|X_1=ATTORNEY,...,X_6=SEATBELT)$

Miramos el resultado en el test:

```r
gbm2.prediction <- as.data.frame(predict.gbm(gbm2, newdata = dt_test , n.trees = 5000 , type="response"))
dt_test$pred_gbm2 <- gbm2.prediction[[1]]
head(dt_test)
```

```
##    ATTORNEY CLMSEX MARITAL CLMINSUR SEATBELT CLMAGE   LOSS Y    pred_rf1
## 6         1      2       1        2        1     35  0.309 0 0.248381186
## 12        1      1       1        2        1     42 29.620 1 0.220724924
## 18        1      1       1        2        1     58  0.758 0 0.199511204
## 21        1      1       2     <NA>        1     37  3.200 0          NA
## 22        2      2       1        2        1     39  0.230 0 0.008032649
## 34        1      2       2        2        1     35  2.673 0 0.303488003
##    pred_rf2  pred_gbm1 pred_gbm2
## 6  7.914495 0.25987576  7.028186
## 12 8.488978 0.20385256 11.097263
## 18 9.846624 0.20979586  9.715652
## 21       NA 0.28165589 10.629112
## 22 2.412385 0.04259158  2.166960
## 34 8.022277 0.32816568  6.946712
```


```r
tail(dt_test)
```

```
##      ATTORNEY CLMSEX MARITAL CLMINSUR SEATBELT CLMAGE  LOSS Y    pred_rf1
## 1326        1      2       2        2        1     19 3.269 0 0.001133968
## 1328        1      2       3        2        1     71 0.505 0 0.625923580
## 1330        2      2       2        2        1     33 1.535 0 0.001283937
## 1336        2      1       2        2        1     NA 0.576 0          NA
## 1337        1      2       1        2        1     46 3.705 0 0.431751239
## 1339        1      2       2        1        1     18 3.277 0 0.003433201
##       pred_rf2  pred_gbm1 pred_gbm2
## 1326  4.269137 0.05474639  3.380333
## 1328 10.602377 0.34634974  6.020655
## 1330  2.182710 0.04427271  1.397027
## 1336        NA 0.01755464  1.350616
## 1337 36.665696 0.33197590 21.693446
## 1339  3.978978 0.06891223  2.555850
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
##       LOSS                 Y             pred_rf1          pred_rf2      
##  Min.   :   0.0050   Min.   :0.0000   Min.   :0.00007   Min.   : 0.3916  
##  1st Qu.:   0.5175   1st Qu.:0.0000   1st Qu.:0.00615   1st Qu.: 2.0644  
##  Median :   2.1645   Median :0.0000   Median :0.03819   Median : 3.4303  
##  Mean   :   7.0917   Mean   :0.1144   Mean   :0.12815   Mean   : 6.3906  
##  3rd Qu.:   3.7782   3rd Qu.:0.0000   3rd Qu.:0.22274   3rd Qu.: 7.8241  
##  Max.   :1067.6970   Max.   :1.0000   Max.   :0.78703   Max.   :57.5121  
##                                       NA's   :70        NA's   :70       
##    pred_gbm1         pred_gbm2      
##  Min.   :0.01755   Min.   : 0.5787  
##  1st Qu.:0.03389   1st Qu.: 1.8938  
##  Median :0.05442   Median : 3.4468  
##  Mean   :0.10986   Mean   : 5.5137  
##  3rd Qu.:0.18907   3rd Qu.: 8.6580  
##  Max.   :0.44740   Max.   :21.6934  
## 
```


Graficamos la distribución de los valores estimados en el train

```r
plot(density(dt_test$pred_gbm2[!is.na(dt_test$pred_gbm2) & dt_test$pred_gbm2 < 30]), ylim= c(0,.25) , xlab="Predicted", col="red" , main="")
lines(density(dt_test$LOSS[dt_test$LOSS<30]),col="blue",lty=1)
lines(density(dt_test$pred_rf2[!is.na(dt_test$pred_rf2) & dt_test$pred_rf2 < 30]),col="darkgreen",lty=1)
```

<img src="09-RandomForests_files/figure-html4/unnamed-chunk-84-1.png" width="672" />


```r
modelchecktest2 <- as.data.frame( cbind(real=dt_test$LOSS , predicted=dt_test$pred_gbm2) )
modelchecktest2[is.na(modelchecktest2)] <- 0
summary(modelchecktest2)
```

```
##       real             predicted      
##  Min.   :   0.0050   Min.   : 0.5787  
##  1st Qu.:   0.5175   1st Qu.: 1.8938  
##  Median :   2.1645   Median : 3.4468  
##  Mean   :   7.0917   Mean   : 5.5137  
##  3rd Qu.:   3.7782   3rd Qu.: 8.6580  
##  Max.   :1067.6970   Max.   :21.6934
```

Error de ajuste del modelo


```r
plot(modelchecktest2, xlim=c(0,100) , ylim=c(0,100) ,  pch="." , cex=1.5)
segments( 0, 0 , 100, 100 , col="red")
```

<img src="09-RandomForests_files/figure-html4/unnamed-chunk-86-1.png" width="672" />

```r
modelMetrics(real=modelchecktest2$real, pred=modelchecktest2$predicted )
```

```
## ------------------------------
```

```
##  Accuracy metrics (global):
```

```
## ------------------------------
```

```
## MAE(ref) = 8.9208
```

```
## MAE = 7.6492
```

```
## RMSE = 53.8215
```

```
## MAPE = 134.19
```

```
## MAPE(sim) = 70.74
```

```
## WMAPE = 107.86
```

```
## 
```

```
## 
```

```
## 
```


Comentario: Al igual que en el caso del modelo de regresión RF2, el error de ajuste del modelo GBM2 demasiado alto: $RMSE= 53.82k$ y el $MAPE=134.19%$. Con estos errores, es preferible ir a un modelo de clasificación en lugar de un modelo de regresión.


# Support Vector Machines


## Introducción

Tienen su origen en los trabajos sobre teoría de aprendizaje estadístico realizados por Vapnik en los años 90. Son algoritmos utilizados para tanto para problemas de clasificación como de regresión.

Se han utilizado con éxito en campos como: reconocimiento de imágenes, clasificación de textos, clasificación de proteínas, etc.
Los algoritmos SVM se encuentran dentro de la familia de clasificadores lineales. Han ganando gran reconocimiento debido a sus solidos fundamentos teóricos.

## Descripción

Dado un conjunto separable

$$S=\{ (\overrightarrow{x_1},y_1), ((\overrightarrow{x_2},y_2 ), \ldots,((\overrightarrow{x_l},y_l ) \}$$
se define hiperplano de separación como una función lineal capaz de separar dicho conjunto sin error







## SVM-clasificación: ejemplo


```r
if (!require(e1071)) install.packages('e1071')
```

```
## Loading required package: e1071
```

```r
library(e1071)
```

librería alternativa que también es muy utilizada

```r
if (!require(kernlab)) install.packages('kernlab')
```

```
## Loading required package: kernlab
```

```
## 
## Attaching package: 'kernlab'
```

```
## The following object is masked from 'package:ggplot2':
## 
##     alpha
```

```r
library(kernlab) 
```

Creamos la relación (fórmula) entre las variables


```r
fmla.svm <-  as.formula(paste0(targets[2],"~",paste0( c(xVars,"SEATBELT"),collapse = "+"),collapse = ""))
```

Modelo exploratorio (sin ajustar parámetros):
  
  ```r
  svm0 <- svm( fmla.svm , data=dt_train , type="C-classification", cost=10, na.action = na.omit)
  ```

Utilizamos la función `na.index` para recuperar las posiciones de todos los registros donde hay missings (NA's, NULL's, blanks)

```r
#ids_NAs <- na.index(dt_test) 
```

Es necesario eliminar los valores missings (NA) de la variable a modelizar (target). Las posiciones están almacenadas en la variable 'ids_NAs'

```r
#real_target <- data.frame( real = dt_test[ -ids_NAs , 'Y']) 
#predicted_target <- data.frame(predicted = predict(svm0, dt_test))
#pred.svm0 <- cbind( real_target , predicted_target )

#metricBinaryClass( fitted.model = NULL , dataset= pred.svm0 , cutpoint= NULL , roc.graph=FALSE)
```

Este modelo ofrece un pobrísimo poder de clasificación. Hay que buscar un mejor modelo vía 'tuning' de los parámetros. A continuación vemos cómo realizar esto:
  
Comparamos dos funciones kernel:  'Base radial' vs 'sigmoide' (no debemos utilizar el kernel 'lineal') 


```r
t.ini <- Sys.time()
tuneSVMclass <- tune(svm, fmla.svm,  data = dt_train, ranges = list( kernel='sigmoid', gamma= seq(1,3,by=.5) ,                                              cost = seq(5,100,by=5)) ,
                     tune.control = tune.control( random = TRUE, sampling = "bootstrap", sampling.aggregate = mean, 
                                                  nboot = 10, boot.size = 0.90, best.model = TRUE, performances = TRUE, error.fun = 'auc'))                    

t.end <- Sys.time()
print(t.end-t.ini)
```

```
## Time difference of 2.58892 mins
```

```r
print(tuneSVMclass)
```

```
## 
## Parameter tuning of 'svm':
## 
## - sampling method: 10-fold cross validation 
## 
## - best parameters:
##   kernel gamma cost
##  sigmoid     1    5
## 
## - best performance: 11062.07
```


El mejor modelo de clasificación vía SVM

```r
tunedSVM1 <- svm( fmla.svm , data=dt_train, type="C-classification", kernel='sigmoid' , probability = TRUE , gamma= 1, cost =10 , na.action = na.omit)
```

El mejor SVM se obtiene con un **kernel de base radial**

```r
predicted_target <- as.data.frame(predict( tunedSVM1 , dt_test))
#pred.svm1        <- as.data.frame(cbind( real_target ,  predicted_target ))
#colnames(pred.svm1) <- c('actual','predicted')

#table( pred.svm1 )
```

Curva ROC SVM


```r
#metricBinaryClass( fitted.model = NULL , dataset=  pred.svm1   , cutpoint= NULL , roc.graph=TRUE)  

#metricBinaryClass( fitted.model = tunedSVM1 , dataset=  dt_test  , cutpoint= NULL , roc.graph=TRUE)  
```


Evaluamos el poder de clasificación con la función 'metricBinaryClass':
  
  ```r
  #metricBinaryClass( fitted.model = NULL , dataset= pred.svm.model , cutpoint= NULL , roc.graph=FALSE)
  ```

 - Error tipo I ($\alpha$) : 79.07% nos dice el error que se comete clasificando una pérdida 'severa' como 'no severa'
 - Error tipo II ($\beta$) : 10.73% nos dice el error que se comete clasificando una p?rdida 'no severa' como 'severa'
 - % de mala clasificación : 19.58% nos da el % de veces que el modelo clasifica incorrectamente las pérdidas 
 - Accuracy = $100 -%mc$   : 80.42%
 - Area bajo la curva ROC, $AUC$ : 0.55102
 - Finalmente calculamos la curva ROC junto con la métrica AUC 



## F.  Entrenamiento del modelo SVM-regresión


Para entrenar el SVM - regresión vamos directamente a buscar el mojor modelo vía 'tuning' de los parámetros:

  - Usando una función Kernel de base radial

```r
#t.ini <- Sys.time()
#tuneSVMreg <- tune(svm, fmla.svm,  data = dt_train, ranges = list(epsilon = seq(0,1,by=0.1), gamma= seq(0.1,3,by=0.1) , cost = seq(5,100, by=5) ), tune.control= tune.control(random = TRUE, nrepeat = 10, repeat.aggregate = mean, sampling = "bootstrap", sampling.aggregate = mean, samling.dispersion = sd, cross = 10, fix = 2/3, nboot = 10, boot.size = 0.90, best.model = TRUE, performances = TRUE, error.fun = 'mse'))
#t.end <- Sys.time()
#print(t.end-t.ini)
```

El mejor modelo SVM

# Incompleto...



###                                    G.  comparativa de modelos de clasificaci?n

Realizamos una breve comparativa de los distintos m?todos de clasificaci?n de la p?rdidas:
  
- Error tipo I (alpha)       : 79.07% nos dice el error que se comete clasificando una p?rdida 'severa' como 'no severa'
- Error tipo II (beta)       : 10.73% nos dice el error que se comete clasificando una p?rdida 'no severa' como 'severa'
- % de mala clasificaci?n    : 19.58% nos da el % de veces que el modelo clasifica incorrectamente las p?rdidas 
- Accuracy = 100 -%mc        : 80.42%
  - Area bajo la curva ROC auc : 0.55102
- Finalmente calculamos la curva ROC junto con la m?trica AUC 


### Curvas ROC

```r
#win.graph()
plot( gbm1.perf , col='red' , lwd=2, type="l" , main="Curva ROC modelo GBM",xlab="Tasa de falsos positivos", ylab="Tasa de verdaderos positivos")
abline( 0 , 1 , col="blue" , lwd=2, lty=2)
abline( 0 , 0 , 1 , col="gray40"   , lty=3)
legend( 0.4  , 0.2 , c(paste0("AUC (Gradient Boosting)=",round(gbm1.auc,4)),"AUC (Coin toss)=0.50") ,lty=c(1,2), lwd=c(2,2) ,col=c("red","blue"), bty="n")
```

<img src="09-RandomForests_files/figure-html4/unnamed-chunk-99-1.png" width="672" />

### falta - Incompleto
