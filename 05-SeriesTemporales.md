# Modelos de Series Temporales








## Series Temporales

### ¿Qué es una Serie Temporal?

Una **serie temporal** es una secuencia de datos, medidos a intervalos de tiempo sucesivos regularmente espaciados.

Ejemplos de series temporales son: tasas de interés mensuales, cantidad de lluvia anual, tasa de desempleo mensual, PIB anual, ondas cardíacas, etc.

<br>

<!--html_preserve--><div id="htmlwidget-ad4c49077223923ee9c9" style="width:100%;height:500px;" class="highchart html-widget"></div>
<script type="application/json" data-for="htmlwidget-ad4c49077223923ee9c9">{"x":{"hc_opts":{"title":{"text":null},"yAxis":{"title":{"text":null}},"credits":{"enabled":true,"text":"Elaborado por Innova-tsn","href":"http://www.innova-tsn.com"},"exporting":{"enabled":true},"plotOptions":{"series":{"turboThreshold":0},"treemap":{"layoutAlgorithm":"squarified"},"bubble":{"minSize":5,"maxSize":25}},"annotationsOptions":{"enabledButtons":false},"tooltip":{"delayForDisplay":10,"valueDecimals":2,"shared":true,"crosshairs":true},"chart":{"type":"line","zoomType":"x"},"legend":{"enabled":true},"xAxis":{"type":"datetime","tickInterval":100,"labels":{"format":"{value:%Y-%m-%d}","rotation":-90}},"series":[{"data":[[-757382400000,26.663],[-754704000000,23.598],[-752284800000,26.931],[-749606400000,24.74],[-747014400000,25.806],[-744336000000,24.364],[-741744000000,24.477],[-739065600000,23.901],[-736387200000,23.175],[-733795200000,23.227],[-731116800000,21.672],[-728524800000,21.87],[-725846400000,21.439],[-723168000000,21.089],[-720748800000,23.709],[-718070400000,21.669],[-715478400000,21.752],[-712800000000,20.761],[-710208000000,23.479],[-707529600000,23.824],[-704851200000,23.105],[-702259200000,23.11],[-699580800000,21.759],[-696988800000,22.073],[-694310400000,21.937],[-691632000000,20.035],[-689126400000,23.59],[-686448000000,21.672],[-683856000000,22.222],[-681177600000,22.123],[-678585600000,23.95],[-675907200000,23.504],[-673228800000,22.238],[-670636800000,23.142],[-667958400000,21.059],[-665366400000,21.573],[-662688000000,21.548],[-660009600000,20],[-657590400000,22.424],[-654912000000,20.615],[-652320000000,21.761],[-649641600000,22.874],[-647049600000,24.104],[-644371200000,23.748],[-641692800000,23.262],[-639100800000,22.907],[-636422400000,21.519],[-633830400000,22.025],[-631152000000,22.604],[-628473600000,20.894],[-626054400000,24.677],[-623376000000,23.673],[-620784000000,25.32],[-618105600000,23.583],[-615513600000,24.671],[-612835200000,24.454],[-610156800000,24.122],[-607564800000,24.252],[-604886400000,22.084],[-602294400000,22.991],[-599616000000,23.287],[-596937600000,23.049],[-594518400000,25.076],[-591840000000,24.037],[-589248000000,24.43],[-586569600000,24.667],[-583977600000,26.451],[-581299200000,25.618],[-578620800000,25.014],[-576028800000,25.11],[-573350400000,22.964],[-570758400000,23.981],[-568080000000,23.798],[-565401600000,22.27],[-562896000000,24.775],[-560217600000,22.646],[-557625600000,23.988],[-554947200000,24.737],[-552355200000,26.276],[-549676800000,25.816],[-546998400000,25.21],[-544406400000,25.199],[-541728000000,23.162],[-539136000000,24.707],[-536457600000,24.364],[-533779200000,22.644],[-531360000000,25.565],[-528681600000,24.062],[-526089600000,25.431],[-523411200000,24.635],[-520819200000,27.009],[-518140800000,26.606],[-515462400000,26.268],[-512870400000,26.462],[-510192000000,25.246],[-507600000000,25.18],[-504921600000,24.657],[-502243200000,23.304],[-499824000000,26.982],[-497145600000,26.199],[-494553600000,27.21],[-491875200000,26.122],[-489283200000,26.706],[-486604800000,26.878],[-483926400000,26.152],[-481334400000,26.379],[-478656000000,24.712],[-476064000000,25.688],[-473385600000,24.99],[-470707200000,24.239],[-468288000000,26.721],[-465609600000,23.475],[-463017600000,24.767],[-460339200000,26.219],[-457747200000,28.361],[-455068800000,28.599],[-452390400000,27.914],[-449798400000,27.784],[-447120000000,25.693],[-444528000000,26.881],[-441849600000,26.217],[-439171200000,24.218],[-436665600000,27.914],[-433987200000,26.975],[-431395200000,28.527],[-428716800000,27.139],[-426124800000,28.982],[-423446400000,28.169],[-420768000000,28.056],[-418176000000,29.136],[-415497600000,26.291],[-412905600000,26.987],[-410227200000,26.589],[-407548800000,24.848],[-405129600000,27.543],[-402451200000,26.896],[-399859200000,28.878],[-397180800000,27.39],[-394588800000,28.065],[-391910400000,28.141],[-389232000000,29.048],[-386640000000,28.484],[-383961600000,26.634],[-381369600000,27.735],[-378691200000,27.132],[-376012800000,24.924],[-373593600000,28.963],[-370915200000,26.589],[-368323200000,27.931],[-365644800000,28.009],[-363052800000,29.229],[-360374400000,28.759],[-357696000000,28.405],[-355104000000,27.945],[-352425600000,25.912],[-349833600000,26.619],[-347155200000,26.076],[-344476800000,25.286],[-342057600000,27.66],[-339379200000,25.951],[-336787200000,26.398],[-334108800000,25.565],[-331516800000,28.865],[-328838400000,30],[-326160000000,29.261],[-323568000000,29.012],[-320889600000,26.992],[-318297600000,27.897]],"name":"Ejemplo","color":"#32b4f7","lineWidth":1}]},"theme":{"chart":{"backgroundColor":"transparent"}},"conf_opts":{"global":{"Date":null,"VMLRadialGradientURL":"http =//code.highcharts.com/list(version)/gfx/vml-radial-gradient.png","canvasToolsURL":"http =//code.highcharts.com/list(version)/modules/canvas-tools.js","getTimezoneOffset":null,"timezoneOffset":0,"useUTC":true},"lang":{"contextButtonTitle":"Chart context menu","decimalPoint":".","downloadJPEG":"Download JPEG image","downloadPDF":"Download PDF document","downloadPNG":"Download PNG image","downloadSVG":"Download SVG vector image","drillUpText":"Back to {series.name}","invalidDate":null,"loading":"Loading...","months":["January","February","March","April","May","June","July","August","September","October","November","December"],"noData":"No data to display","numericSymbols":["k","M","G","T","P","E"],"printChart":"Print chart","resetZoom":"Reset zoom","resetZoomTitle":"Reset zoom level 1:1","shortMonths":["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"],"thousandsSep":" ","weekdays":["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]}},"type":"chart","fonts":[],"debug":false},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->




 > Una serie temporal (o simplemente una serie) es una secuencia de N observaciones ordenadas y equidistantes cronológicamente sobre una característica o varias características de una unidad observable en diferentes momentos. 
 

 * Si la serie es sobre una característica se dice que es univariante o escalar. 
 * Si la serie es sobre dos o más características se dice que es multivariante o vectorial.


El estudio de las series temporales permite: (1) entender mejor el mecanismo de generación de los datos, que puede no ser claro inicialmente en una investigaci?n y/o (2) hacer pronósticos sobre el futuro, es decir: previsiones. Las previsiones se utilizan en forma constante en diversos campos: econom?a, finanzas, marketing, medio ambiente, ingeniería, etc. En general, las previsiones proporcionan una guía para las decisiones que deben tomarse.

Algunos ejemplos de uso de las previsiones son:

 * En **Planeamiento y Control de Operaciones**. Las decisiones de producción de un artículo con base en los pronósticos de ventas. Es posible por ejemplo, detectar una disminución en la tendencia de ventas que conlleve a reducir la producción, o al contrario.
 
 * En Marketing. La decisión de invertir en publicidad puede depender de prever las ventas.
 
 * En **Economía**. Las decisiones del Banco de España, por ejemplo para el control de la inflación, requieren la previsión y el examen del comportamiento de ciertas variables macroeconómicas, como el PIB, la tasa de desempleo, el IPC, las tasas de inter?s a distintos plazos, activas y pasivas.
 
 * En **Turismo**. La previsión del de número de turistas mensuales para determinar la demanda hotelera.
 
 * En **Epidemiología** y **Medio Ambiente**. La vigilancia de los niveles de contaminantes en el aire tiene como herramienta fundamental las series de tiempo. Pero adicionalmente el efecto de estos niveles sobre la salud.

Todas las series temporales tienen características particulares. Asi por ejemplo, las series pueden:

 * evolucionar alrededor de un nivel constante o tienen tendencias crecientes o decrecientes, 
 * evolucionar alrededor de un nivel que cambia sin seguir aparentemente un patrón concreto - tienen tendencia estocástica - 
 * presentar reducciones (en invierno) y aumentos (en verano) sistemáticos en su nivel cada 12 meses - son estacionales -
 * presentar variabilidad constante alrededor de su nivel
 * presentar variabilidad condicional o alta volatilidad,
 * moverse conjuntamente con otras series - tendencia común -
 * etc.


En las secciones siguiente se describen brevemente algunos conceptos necesarios para la modelación básica de series temporales.

## Operadores 

### Operador de retardo simple

El **operador de retardo simple** se define como
$$Bz_t=z_{t-1}$$
Si aplicamos el operador de retardo dos veces:
$$BBz_t=Bz_{t-1}=z_{t-2}$$
Del mismo modo, si aplicamos $n$ veces el operador de retardo, obtenemos:
$$ BB \ldots Bz_t=z_{t-n} $$
Definimos, por tanto
$$ B^n z_t=z_{t-n} $$

### Operador de adelanto simple
De modo an?logo, definimos el **operador de adelanto simple**
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

### Polinomios en B

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

### Operador diferencia

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

### ¿Qué es el Alisado Exponencial?


 * Es una técnica aplicada a series de tiempo, para ``suavizarlas'' u obtener previsiones.
 * Mientras que, con la media móvil, las observaciones pasadas se ponderan por igual, en el alisado exponencial se asignan ponderaciones exponencialmente decrecientes en el tiempo.
 * La fórmula utilizada es:
 
   $$ y_1 = x_0  $$
   $$ y_t = (1-\theta)x_{t-1}+\theta y_{t-1},  t > 1 $$



donde $\{x_t\}$ son las observaciones reales, $\{y_t\}$ son las estimaciones y  $\theta$ es el factor de alisamiento, $0 < \theta < 1$.
        
 En otras palabras, con este método, la previsién para el periodo $t$ (valor esperado) como la suma ponderada de todas la observaciones anteriores, dando mayor importancia a las observaciones más recientes que a las más antiguas. Como puede verse en:
 
$$ y_t = (1-\theta) x_{t-1} +\theta y_{t-1} $$ 
$$ y_t = (1-\theta)x_{t-1}+(1-\theta)\theta x_{t-2}+(1-\theta) \theta^2 y_{t-2} $$
$$ y_t = (1-\theta)[x_{t-1}+\theta x_{t-2}+\theta x_{t-3}+\theta x_{t-4}+ ...] + \theta^{t-1} x_0 $$ 
Así, los pesos asignados a las observaciones previas pertenecen a una proporción de la progresión geométrica: $\{1, \theta, \theta^2, \theta^3, ..\}$.
            
 * Por otro lado, si la ecuación arriba se expresa como: 

$$
                y_t = x_{t-1} + \theta(y_{t-1} - x_{t-1}) ,  
$$
            
Se aprecia que $y_t$ está formada por la suma de la observaci?n en el periodo anterior ($x_{t-1}$) más una proporción ($\theta$) del error cometido ($y_{t-1} - x_{t-1}$). Por lo tanto el valor de $\theta$ controla la rapidez con que la previsión se adapta a los cambios del nivel de la serie (estado).
            
 * Si $\theta$ es grande (próximo a 1), la previsión se adapta rápidamente a los cambios, por lo tanto se debe utilizar en series poco estables.
 * Si $\theta$ es pequeño (próximo a 0), se consigue eliminar el efecto de las fluctuaciones, por lo tanto se debe utilizar en series estables.
 * El valor de $\theta$ se puede optimizar minimizando la suma de cuadrados del error de previsión, es decir, resolviendo: $min(x_{t-1} - y_{t-1})^2$.
 * El alisado exponencial, t?cnicamente, es equivalente a un modelo *ARIMA (0,1,1)* sin constante. En otras palabras, se puede representar por:
$$             \hat{y} = (1-\theta)(1 + \theta B + \theta^2 B^2 + \theta^3 B^3 + ...)x_{t-1}
$$
 donde $B$ es el operador retardo y $\theta$ es el parámetro de amortiguamiento. Esta representación no implica recargar el último término con un peso mayor a los valores más recientes.
            Si existe un n?mero finito de per?odos observados, la ecuaci?n anterior se reescribe como:
            
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
 

## Explorando Series Temporales con R

Un ejemplo sencillo sobre el manejo de series temporales que puede realizarse con algunos paquetes de R.

### Datos

Los datos utilizados corresponden al 
** Similarly, the file http://robjhyndman.com/tsdldata/data/fancy.dat contains monthly sales for a souvenir shop at a beach resort town in Queensland, Australia, for January 1987-December 1993 (original data from Wheelwright and Hyndman, 1998).**


<img src="05-SeriesTemporales_files/figure-html4/unnamed-chunk-3-1.png" width="672" /><!--html_preserve--><div id="htmlwidget-46c21faaa0baf43d9b20" style="width:100%;height:500px;" class="highchart html-widget"></div>
<script type="application/json" data-for="htmlwidget-46c21faaa0baf43d9b20">{"x":{"hc_opts":{"title":{"text":null},"yAxis":{"title":{"text":null}},"credits":{"enabled":true,"text":"Elaborado por Innova-tsn","href":"https://www.innova-tsn.com"},"exporting":{"enabled":true},"plotOptions":{"series":{"turboThreshold":0},"treemap":{"layoutAlgorithm":"squarified"},"bubble":{"minSize":5,"maxSize":25}},"annotationsOptions":{"enabledButtons":false},"tooltip":{"delayForDisplay":10,"valueDecimals":0,"shared":true,"crosshairs":true},"chart":{"type":"line","zoomType":"x"},"legend":{"enabled":true},"xAxis":{"type":"datetime","tickInterval":10,"labels":{"format":"{value:%m-%Y}","rotation":-90}},"series":[{"data":[[536457600000,7.41746628178892],[539136000000,7.78219431971372],[541555200000,7.95180929991046],[544233600000,8.17393921066467],[546825600000,8.23030014093807],[549504000000,8.22006396816164],[552096000000,8.37784146489105],[554774400000,8.17929513880233],[557452800000,8.52154769678127],[560044800000,8.76771530589945],[562723200000,8.93598247052666],[565315200000,9.89122315128614],[567993600000,7.82397000796815],[570672000000,8.55607538574002],[573177600000,8.88532188995553],[575856000000,8.47762665847418],[578448000000,8.68285677131405],[581126400000,8.5074135259989],[583718400000,8.72893114549063],[586396800000,8.46635242620086],[589075200000,8.61185406956077],[591667200000,8.67164668253439],[594345600000,9.44145844212576],[596937600000,10.2591221555078],[599616000000,8.45893252325831],[602294400000,8.64868275091755],[604713600000,9.20608934916864],[607392000000,8.57636357987714],[609984000000,8.77839216180762],[612662400000,8.79948073955071],[615254400000,8.90240389019008],[617932800000,9.00903414127095],[620611200000,9.05639283818004],[623203200000,9.1789013031408],[625881600000,9.62587725570802],[628473600000,10.4359086073296],[631152000000,8.68627752142817],[633830400000,8.66812383534513],[636249600000,9.42716399454557],[638928000000,8.75931864116395],[641520000000,8.9371028068499],[644198400000,8.88526791030584],[646790400000,9.0022356681753],[649468800000,8.98459970106459],[652147200000,8.99876218328263],[654739200000,9.04507650210367],[657417600000,9.79337465104933],[660009600000,10.3127590737202],[662688000000,8.48190585239446],[665366400000,8.77496693554935],[667785600000,9.17354878610287],[670464000000,9.08490979326442],[673056000000,9.07364626896591],[675734400000,9.23107197940138],[678326400000,9.33048062720624],[681004800000,9.43765282134659],[683683200000,9.36197846933872],[686275200000,9.51833156108381],[688953600000,9.99067895498687],[691545600000,10.7157655267851],[694224000000,8.93787920491441],[696902400000,9.19519526158966],[699408000000,9.58592342560243],[702086400000,9.35766753878483],[704678400000,9.14126463991353],[707356800000,9.4789993981795],[709948800000,9.72512494873588],[712627200000,9.89790248504199],[715305600000,10.083029416227],[717897600000,10.1421638438248],[720576000000,10.4919628691521],[723168000000,11.298762839144],[725846400000,9.2343732547976],[728524800000,9.32962272753498],[730944000000,9.99089568414157],[733622400000,9.76177017454231],[736214400000,9.68020586668178],[738892800000,9.8309991143828],[741484800000,10.1718013908294],[744163200000,10.2606905570263],[746841600000,10.3256593239152],[749433600000,10.3359622617392],[752112000000,10.7500933163369],[754704000000,11.5584786815873]],"name":"Número de Clientes","color":"#07077e","lineWidth":1}]},"theme":{"chart":{"backgroundColor":"transparent"}},"conf_opts":{"global":{"Date":null,"VMLRadialGradientURL":"http =//code.highcharts.com/list(version)/gfx/vml-radial-gradient.png","canvasToolsURL":"http =//code.highcharts.com/list(version)/modules/canvas-tools.js","getTimezoneOffset":null,"timezoneOffset":0,"useUTC":true},"lang":{"contextButtonTitle":"Chart context menu","decimalPoint":".","downloadJPEG":"Download JPEG image","downloadPDF":"Download PDF document","downloadPNG":"Download PNG image","downloadSVG":"Download SVG vector image","drillUpText":"Back to {series.name}","invalidDate":null,"loading":"Loading...","months":["January","February","March","April","May","June","July","August","September","October","November","December"],"noData":"No data to display","numericSymbols":["k","M","G","T","P","E"],"printChart":"Print chart","resetZoom":"Reset zoom","resetZoomTitle":"Reset zoom level 1:1","shortMonths":["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"],"thousandsSep":" ","weekdays":["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]}},"type":"chart","fonts":[],"debug":false},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->



### Librerías

A lo largo de este documento se utilizan dos paquetes de [R](https://cran.r-project.org/): 

 * XTS: *eXtensible Time Series* y
 * HIGHCHARTER: *a R wrapper for Highcharts javascript libray and its modules. *. 

Los manual de usuario de ambos paquetes esta disponibles en la siguientes enlaces: [Manual de XTS](http://cran.r-project.org/web/packages/xts/xts.pdf) y [Manual de HIGHCHARTER](https://cran.r-project.org/web/packages/highcharter/highcharter.pdf)

Para instalar un paquete de R, se puede usar el comando: `install.packages("nombre del paquete")`. Por ejemplo, `install.packages("xts")`. De forma alternativa:


```r
# Get xts
if (!require("xts")) {
    install.packages("xts")
    library(xts)
}
```


## Lectura y Visualización

### Datos Originales

Definimos la carpeta de trabajo y leemos el fichero de los datos con la función `read.zoo`. En este ejemplo, los datos están en el fichero `ClientesTotalesXTS.csv` que tiene formato `CSV`. Leer los datos de esta manera tiene como ventaja que las fechas son reconocidas como tal. Al usar una función como `read.csv` entienden las fechas como textos y no siempre son bien entendidas por las funciones que necesitan series temporales como input.


```r
#setwd("C:/Users/rerodriguez/Desktop/script/rev") #seleccionar la carpeta de trabajo, donde se encuentran los datos y donde se guardan resultados.
#datos = read.zoo("ClientesTotalesXTS.csv", sep=',', tz='',header=T, format='%d-%m-%Y') #matriz con los datos. Los nombres de las filas han sido reconocidos como fechas
#datos = exp(datos) #transformación opcional
datos = scan("http://robjhyndman.com/tsdldata/data/fancy.dat")
datos = log(datos) #transformación opcional

head(datos,5) #primeros 5 datos
```

[1] 7.41747 7.78219 7.95181 8.17394 8.23030

```r
tail(datos,5) #últimos 5 datos
```

[1] 10.2607 10.3257 10.3360 10.7501 11.5585

### Objeto XTS
La forma más conocida para la creación de un objeto de la clase serie temporal, es el uso de la función `ts`. En este ejemplo, creamos la serie temporal `client.ts` a partir de `datos`.

```r
client.ts=ts(datos, frequency=12, start=c(1987,1)) #objeto TS
```

Sin embargo, la manipulación de la serie es bastante más natural y amigable utilizando un objeto de la clase `xts`. En este ejemplo se crea el objeto de `client` a partir de `datos` y se hacen consultas básicas sobre su contenido (fechado, primer dato, últimas semanas, número de semanas en la muestra, etc.)

```r
client = as.xts(client.ts) #creación del objeto XTS
is.xts(client) #debe devolver TRU
```

[1] TRUE

```r
periodicity(client) #fechado de los datos
```

Monthly periodicity from ene. 1987 to dic. 1993 

```r
first(client) #primer dato
```

             [,1]
ene. 1987 7.41747

```r
last(client) #último dato
```

             [,1]
dic. 1993 11.5585

```r
first(client, '7 days') #primeros 7 dias
```

             [,1]
ene. 1987 7.41747
feb. 1987 7.78219
mar. 1987 7.95181
abr. 1987 8.17394
may. 1987 8.23030
jun. 1987 8.22006
jul. 1987 8.37784

```r
last(client, '2 weeks') #últimas dos semanas
```

             [,1]
nov. 1993 10.7501
dic. 1993 11.5585

```r
ndays(client) #número de días en la muestra
```

[1] 84

```r
nweeks(client) #número de semanas en la muestra
```

[1] 84

```r
nmonths(client) #número de meses en la muestra
```

[1] 84

```r
nquarters(client) #número de trimestres en la muestra
```

[1] 28

```r
nyears(client) #número de años en la muestra
```

[1] 7

### Selección de datos usando las fechas
Una funcionalidad interesante es la obtención de sub-muestras, utilizando la(s) fecha(s) como criterio(s) de selección.

```r
client['2012-02-01/2012-02-05'] #todos los datos del 01 al 05 de Febrero de 2012
```

     [,1]

```r
first(client['2012-02'], '5 days') #primeros 5 datos de Febrero de 2012
```

     [,1]

```r
last(client['2013'], '1 week') #datos última semana de 2013
```

     [,1]

```r
rbind(client['2011-02-01/2011-02-05'],client['2012-02-01/2012-02-05']) #todos los datos del 01 al 05 de Febrero de 2011 y 2012
```

     [,1]


## Manipulación 

### Cambios de Fechado
El cambio de fechado o periodicidad es una operación muy útil durante el trabajo con series temporales. En este ejemplo, como la variable analizada es Número Total Diario de Clientes, se utiliza la función `sum` para obtener el Número Total Semanal, Mensual, Trimestral y Anual de Clientes.

```r
client.sem=apply.weekly(client, sum) # datos semanales
 first(client.sem, '3 weeks')
```

             [,1]
ene. 1987 7.41747
feb. 1987 7.78219
mar. 1987 7.95181

```r
client.mes=apply.monthly(client, sum) # datos mensuales
 first(client.mes, '3 months')
```

             [,1]
ene. 1987 7.41747
feb. 1987 7.78219
mar. 1987 7.95181

```r
client.qua=apply.quarterly(client, sum) # datos trimestrales
 first(client.qua, '3 quarters')
```

             [,1]
mar. 1987 23.1515
jun. 1987 24.6243
sep. 1987 25.0787

```r
client.yea=apply.yearly(client, sum) # datos anuales
 first(client.yea, '3 years')
```

             [,1]
dic. 1987 100.449
dic. 1988 105.113
dic. 1989 108.676


### Imputación de Datos Faltantes
La falta de algunos datos y/o la presencia de datos errones suele tratarse con procedimientos de imputación - para no perder histórico de la muestra disponible. El paquete `xts` posee funciones que permiten extender hacia adelante o hacia atrás, valores observados en la misma serie temporal.


```r
which(is.na(client)) #identifica las líneas con NA
```

integer(0)

```r
client.na01=na.locf(client) #repite el ultimo anterior a NA
 client.na01[14:17,]
```

             [,1]
feb. 1988 8.55608
mar. 1988 8.88532
abr. 1988 8.47763
may. 1988 8.68286

```r
client.na02=na.locf(client, fromLast=TRUE) # repite el primero despues de NA
 client.na02[14:17,]
```

             [,1]
feb. 1988 8.55608
mar. 1988 8.88532
abr. 1988 8.47763
may. 1988 8.68286

```r
client.na03=na.locf(client, na.rm=TRUE, fromLast=TRUE)
 client.na03[14:17,]
```

             [,1]
feb. 1988 8.55608
mar. 1988 8.88532
abr. 1988 8.47763
may. 1988 8.68286

### Estadísticos en diferentes fechados
El paquete `xts`permite trabajar con series de estadísticos en fechado agregado. Por ejemplo, el máximo del mes, el mínimo del trimestre, etc. El ingrediente indispensable es el vector que indica los puntos de quiebre de la serie. Este vector se obtiene con la función `endpoints`.


```r
aux.mes=endpoints(client,"months") #indica los finales de mes
par(mfrow=c(1,3), cex.lab=0.8,cex.axis=0.8,las=2)
plot(period.sum(client.na01,aux.mes), main="Total del Mes") 
plot(period.min(client.na01,aux.mes), main="Mínimo del Mes")
plot(period.max(client.na01,aux.mes), main="Máximo del Mes")
```

<img src="05-SeriesTemporales_files/figure-html4/unnamed-chunk-11-1.png" width="864" />

```r
aux.qua=endpoints(client,"quarters") #indica los finales de trimestre
par(mfrow=c(1,3),cex.lab=0.8,cex.axis=0.8,las=2)
plot(period.sum(client.na01,aux.qua), main="Total del Trimestre")
plot(period.min(client.na01,aux.qua), main="Mínimo del Trimestre")
plot(period.max(client.na01,aux.qua), main="Máximo del Trimestre")
```

<img src="05-SeriesTemporales_files/figure-html4/unnamed-chunk-11-2.png" width="864" />

### División del conjunto de datos usando fechas
Otra funcionalidad útil es `split`. Permite dividir el objeto original en sub-conjuntos, teniendo en cuenta un fechado y un horizonte. En este caso, se divide el objeto `client` en sub-muestras de 4 meses cada una.

```r
client.by.4months=split(client, f="months",k=4) 
 #divide el conjunto de datos en partes de 4 meses cada una
summary(client.by.4months) #indica el número de elemento que hay en cada parte
```

      Length Class Mode   
 [1,] 4      xts   numeric
 [2,] 4      xts   numeric
 [3,] 4      xts   numeric
 [4,] 4      xts   numeric
 [5,] 4      xts   numeric
 [6,] 4      xts   numeric
 [7,] 4      xts   numeric
 [8,] 4      xts   numeric
 [9,] 4      xts   numeric
[10,] 4      xts   numeric
[11,] 4      xts   numeric
[12,] 4      xts   numeric
[13,] 4      xts   numeric
[14,] 4      xts   numeric
[15,] 4      xts   numeric
[16,] 4      xts   numeric
[17,] 4      xts   numeric
[18,] 4      xts   numeric
[19,] 4      xts   numeric
[20,] 4      xts   numeric
[21,] 4      xts   numeric

### Otras operaciones con los datos
Finalmente, se presenta un ejemplo de una serie obtenida a partir del uso de operaciones básicas de R como `diff` y `log`.

```r
client.inc <- diff(log(client), lag = 1) #Tasa de incremento diario
client.inc <- client.inc[-1] #Eliminamos el primer dato por ser NA
par(mfrow=c(1,1),cex.lab=0.8,cex.axis=0.8,las=2)
plot(client.inc, main = "Nuevos Clientes",
     col = "grey", xlab = "Date", ylab = "Variación", major.ticks='years',
     minor.ticks=FALSE)
```

<img src="05-SeriesTemporales_files/figure-html4/unnamed-chunk-13-1.png" width="864" />


## Gráficos

### Con xts

Los gráficos de objetos `xts` son bastante más visuales o legibles que los objetos `ts`. La principal diferencia está en el reconocimiento de las fechas y su visualización en el eje horizontal.


```r
par(mfrow=c(1,1),cex.lab=0.8,cex.axis=0.8,las=2)
plot(client, main = "Clientes Totales, por día",
     col = "darkblue",xlab = "Date", ylab = "Número de Clientes", major.ticks='quarters',
     minor.ticks=FALSE)
```

<img src="05-SeriesTemporales_files/figure-html4/unnamed-chunk-14-1.png" width="864" />

```r
plot(client.mes['1990-03/1993-12'], main = "Clientes Totales, por mes",
     col = "blue",xlab = "Date", ylab = "Número de Clientes", major.ticks='years',
     minor.ticks=FALSE)
```

<img src="05-SeriesTemporales_files/figure-html4/unnamed-chunk-14-2.png" width="864" />

### Con highcharter

Los gráficos generados con [highcharter](http://jkunst.com/highcharter/index.html) utilizan la biblioteca [Highcharts](http://www.highcharts.com/demo)


```r
highchart() %>% 
  hc_chart(type="line",zoomType="x")%>%
  hc_title(text = "Clientes Totales, por día") %>%
  hc_subtitle(text = "Gráfico Tipo Línea") %>% 
  hc_legend(enabled = T) %>%
  hc_tooltip(valueDecimals= 2,shared=T, crosshairs=T) %>%
  hc_xAxis(type = 'datetime',
           tickInterval=10,
           labels = list(format = '{value:%m-%Y}',rotation=-90)) %>% 
  hc_add_series(data=client.ts, 
                name = "Número de Clientes",
                color = bysCol[1],
                lineWidth= 1) %>%
  hc_credits(enabled = TRUE, # add credits
             text = "Elaborado por Innova-tsn",
             href = "https://www.innova-tsn.com") %>% 
  hc_exporting(enabled = TRUE)
```

<!--html_preserve--><div id="htmlwidget-46b6128ed29cd89df8b8" style="width:100%;height:500px;" class="highchart html-widget"></div>
<script type="application/json" data-for="htmlwidget-46b6128ed29cd89df8b8">{"x":{"hc_opts":{"title":{"text":"Clientes Totales, por día"},"yAxis":{"title":{"text":null}},"credits":{"enabled":true,"text":"Elaborado por Innova-tsn","href":"https://www.innova-tsn.com"},"exporting":{"enabled":true},"plotOptions":{"series":{"turboThreshold":0},"treemap":{"layoutAlgorithm":"squarified"},"bubble":{"minSize":5,"maxSize":25}},"annotationsOptions":{"enabledButtons":false},"tooltip":{"delayForDisplay":10,"valueDecimals":2,"shared":true,"crosshairs":true},"chart":{"type":"line","zoomType":"x"},"subtitle":{"text":"Gráfico Tipo Línea"},"legend":{"enabled":true},"xAxis":{"type":"datetime","tickInterval":10,"labels":{"format":"{value:%m-%Y}","rotation":-90}},"series":[{"data":[[536457600000,7.41746628178892],[539136000000,7.78219431971372],[541555200000,7.95180929991046],[544233600000,8.17393921066467],[546825600000,8.23030014093807],[549504000000,8.22006396816164],[552096000000,8.37784146489105],[554774400000,8.17929513880233],[557452800000,8.52154769678127],[560044800000,8.76771530589945],[562723200000,8.93598247052666],[565315200000,9.89122315128614],[567993600000,7.82397000796815],[570672000000,8.55607538574002],[573177600000,8.88532188995553],[575856000000,8.47762665847418],[578448000000,8.68285677131405],[581126400000,8.5074135259989],[583718400000,8.72893114549063],[586396800000,8.46635242620086],[589075200000,8.61185406956077],[591667200000,8.67164668253439],[594345600000,9.44145844212576],[596937600000,10.2591221555078],[599616000000,8.45893252325831],[602294400000,8.64868275091755],[604713600000,9.20608934916864],[607392000000,8.57636357987714],[609984000000,8.77839216180762],[612662400000,8.79948073955071],[615254400000,8.90240389019008],[617932800000,9.00903414127095],[620611200000,9.05639283818004],[623203200000,9.1789013031408],[625881600000,9.62587725570802],[628473600000,10.4359086073296],[631152000000,8.68627752142817],[633830400000,8.66812383534513],[636249600000,9.42716399454557],[638928000000,8.75931864116395],[641520000000,8.9371028068499],[644198400000,8.88526791030584],[646790400000,9.0022356681753],[649468800000,8.98459970106459],[652147200000,8.99876218328263],[654739200000,9.04507650210367],[657417600000,9.79337465104933],[660009600000,10.3127590737202],[662688000000,8.48190585239446],[665366400000,8.77496693554935],[667785600000,9.17354878610287],[670464000000,9.08490979326442],[673056000000,9.07364626896591],[675734400000,9.23107197940138],[678326400000,9.33048062720624],[681004800000,9.43765282134659],[683683200000,9.36197846933872],[686275200000,9.51833156108381],[688953600000,9.99067895498687],[691545600000,10.7157655267851],[694224000000,8.93787920491441],[696902400000,9.19519526158966],[699408000000,9.58592342560243],[702086400000,9.35766753878483],[704678400000,9.14126463991353],[707356800000,9.4789993981795],[709948800000,9.72512494873588],[712627200000,9.89790248504199],[715305600000,10.083029416227],[717897600000,10.1421638438248],[720576000000,10.4919628691521],[723168000000,11.298762839144],[725846400000,9.2343732547976],[728524800000,9.32962272753498],[730944000000,9.99089568414157],[733622400000,9.76177017454231],[736214400000,9.68020586668178],[738892800000,9.8309991143828],[741484800000,10.1718013908294],[744163200000,10.2606905570263],[746841600000,10.3256593239152],[749433600000,10.3359622617392],[752112000000,10.7500933163369],[754704000000,11.5584786815873]],"name":"Número de Clientes","color":"#07077e","lineWidth":1}]},"theme":{"chart":{"backgroundColor":"transparent"}},"conf_opts":{"global":{"Date":null,"VMLRadialGradientURL":"http =//code.highcharts.com/list(version)/gfx/vml-radial-gradient.png","canvasToolsURL":"http =//code.highcharts.com/list(version)/modules/canvas-tools.js","getTimezoneOffset":null,"timezoneOffset":0,"useUTC":true},"lang":{"contextButtonTitle":"Chart context menu","decimalPoint":".","downloadJPEG":"Download JPEG image","downloadPDF":"Download PDF document","downloadPNG":"Download PNG image","downloadSVG":"Download SVG vector image","drillUpText":"Back to {series.name}","invalidDate":null,"loading":"Loading...","months":["January","February","March","April","May","June","July","August","September","October","November","December"],"noData":"No data to display","numericSymbols":["k","M","G","T","P","E"],"printChart":"Print chart","resetZoom":"Reset zoom","resetZoomTitle":"Reset zoom level 1:1","shortMonths":["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"],"thousandsSep":" ","weekdays":["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]}},"type":"chart","fonts":[],"debug":false},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

<br>
<br>
<br>





```r
hc <- highchart(type="stock") %>%
  hc_title(text = "Clientes Totales, por día") %>%
  hc_subtitle(text = "Gráfico tipo Stock") %>% 
  hc_legend(enabled = T) %>%
  hc_tooltip(valueDecimals= 0) %>%
  hc_add_series(data=client.ts, 
                name = "Número de Clientes",
                color = bysCol[1]) %>%
  hc_credits(enabled = TRUE, # add credits
             text = "Elaborado por Innova-tsn",
             href = "https://www.innova-tsn.com")
hc
```

<!--html_preserve--><div id="htmlwidget-7220ce5bc431d7a7a11b" style="width:100%;height:500px;" class="highchart html-widget"></div>
<script type="application/json" data-for="htmlwidget-7220ce5bc431d7a7a11b">{"x":{"hc_opts":{"title":{"text":"Clientes Totales, por día"},"yAxis":{"title":{"text":null}},"credits":{"enabled":true,"text":"Elaborado por Innova-tsn","href":"https://www.innova-tsn.com"},"exporting":{"enabled":false},"plotOptions":{"series":{"turboThreshold":0},"treemap":{"layoutAlgorithm":"squarified"},"bubble":{"minSize":5,"maxSize":25}},"annotationsOptions":{"enabledButtons":false},"tooltip":{"delayForDisplay":10,"valueDecimals":0},"subtitle":{"text":"Gráfico tipo Stock"},"legend":{"enabled":true},"series":[{"data":[[536457600000,7.41746628178892],[539136000000,7.78219431971372],[541555200000,7.95180929991046],[544233600000,8.17393921066467],[546825600000,8.23030014093807],[549504000000,8.22006396816164],[552096000000,8.37784146489105],[554774400000,8.17929513880233],[557452800000,8.52154769678127],[560044800000,8.76771530589945],[562723200000,8.93598247052666],[565315200000,9.89122315128614],[567993600000,7.82397000796815],[570672000000,8.55607538574002],[573177600000,8.88532188995553],[575856000000,8.47762665847418],[578448000000,8.68285677131405],[581126400000,8.5074135259989],[583718400000,8.72893114549063],[586396800000,8.46635242620086],[589075200000,8.61185406956077],[591667200000,8.67164668253439],[594345600000,9.44145844212576],[596937600000,10.2591221555078],[599616000000,8.45893252325831],[602294400000,8.64868275091755],[604713600000,9.20608934916864],[607392000000,8.57636357987714],[609984000000,8.77839216180762],[612662400000,8.79948073955071],[615254400000,8.90240389019008],[617932800000,9.00903414127095],[620611200000,9.05639283818004],[623203200000,9.1789013031408],[625881600000,9.62587725570802],[628473600000,10.4359086073296],[631152000000,8.68627752142817],[633830400000,8.66812383534513],[636249600000,9.42716399454557],[638928000000,8.75931864116395],[641520000000,8.9371028068499],[644198400000,8.88526791030584],[646790400000,9.0022356681753],[649468800000,8.98459970106459],[652147200000,8.99876218328263],[654739200000,9.04507650210367],[657417600000,9.79337465104933],[660009600000,10.3127590737202],[662688000000,8.48190585239446],[665366400000,8.77496693554935],[667785600000,9.17354878610287],[670464000000,9.08490979326442],[673056000000,9.07364626896591],[675734400000,9.23107197940138],[678326400000,9.33048062720624],[681004800000,9.43765282134659],[683683200000,9.36197846933872],[686275200000,9.51833156108381],[688953600000,9.99067895498687],[691545600000,10.7157655267851],[694224000000,8.93787920491441],[696902400000,9.19519526158966],[699408000000,9.58592342560243],[702086400000,9.35766753878483],[704678400000,9.14126463991353],[707356800000,9.4789993981795],[709948800000,9.72512494873588],[712627200000,9.89790248504199],[715305600000,10.083029416227],[717897600000,10.1421638438248],[720576000000,10.4919628691521],[723168000000,11.298762839144],[725846400000,9.2343732547976],[728524800000,9.32962272753498],[730944000000,9.99089568414157],[733622400000,9.76177017454231],[736214400000,9.68020586668178],[738892800000,9.8309991143828],[741484800000,10.1718013908294],[744163200000,10.2606905570263],[746841600000,10.3256593239152],[749433600000,10.3359622617392],[752112000000,10.7500933163369],[754704000000,11.5584786815873]],"name":"Número de Clientes","color":"#07077e"}]},"theme":{"chart":{"backgroundColor":"transparent"}},"conf_opts":{"global":{"Date":null,"VMLRadialGradientURL":"http =//code.highcharts.com/list(version)/gfx/vml-radial-gradient.png","canvasToolsURL":"http =//code.highcharts.com/list(version)/modules/canvas-tools.js","getTimezoneOffset":null,"timezoneOffset":0,"useUTC":true},"lang":{"contextButtonTitle":"Chart context menu","decimalPoint":".","downloadJPEG":"Download JPEG image","downloadPDF":"Download PDF document","downloadPNG":"Download PNG image","downloadSVG":"Download SVG vector image","drillUpText":"Back to {series.name}","invalidDate":null,"loading":"Loading...","months":["January","February","March","April","May","June","July","August","September","October","November","December"],"noData":"No data to display","numericSymbols":["k","M","G","T","P","E"],"printChart":"Print chart","resetZoom":"Reset zoom","resetZoomTitle":"Reset zoom level 1:1","shortMonths":["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"],"thousandsSep":" ","weekdays":["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]}},"type":"stock","fonts":[],"debug":false},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->
