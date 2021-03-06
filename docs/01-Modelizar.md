
# (PART) MODELOS ANALÍTICOS {-} 




# Modelizar {#modelizacion}

<!-- ---------## Motivación---------------------------------------------- -->

Los sistemas analíticos  están presentes en todas las áreas de negocio de todos los sectores empresariales.  A muy alto nivel, se puede contar con modelos predictivos en la gestión de clientes, gestión del riesgo y en el soporte a las operaciones. Así mismo, dichos sistemas pueden estar desagregados por segmento, producto, región geográfica, etc. 

Tomando como ejemplo el sector bancario, algunas de las aplicaciones de la modelización son:


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

<!-- <img src='http://r4ds.had.co.nz/diagrams/data-science-model.png' alt="Modelling" style="float:width:90%;"> -->

<img src="imgs/data-science-model.png" width="90%" />

 > _Data_ y _Analytics_ van de la mano.  

<br>

Más generalmente, como lo expone @mar2015, en el context de _Big Data_, **apply analytics** (A), se encuentra en el corazón de la estrategia para crear  _SMART Business_ y aprovechar al máximo el valor de los datos.

<!-- <img src='https://pbs.twimg.com/media/CDgmVL8VIAAlZSa.jpg' alt="SMART Model" style="float:width:60%;"> -->


<img src="imgs/CDgmVL8VIAAlZSa.jpg" width="90%" />

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



