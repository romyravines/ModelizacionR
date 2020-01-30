# Text Mining




En esta sección veremos como procesar texto y alguna de sus aplicaciones. La minería de textos es un campo de las ciencias de la computación, inteligencia artificial y lingüística que estudia las interacciones entre las computadoras y el lenguaje humano.

## Aplicaciones

* **Extracción de información (*feature extraction*) **: Se centra en obtener nombres propios, organizaciones o eventos, así como fechas y encontrar la relación entre ellas.
 
* **Análisis de sentimientos (*sentiment analysis*) **: Analiza el vocabulario de un texto con el fin de determinar sus cargas emocionales. Para ello se pueden utilizar los llamados *diccionarios de polaridad*, listas de palabras asociadas a mensajes con connotaciones positivas o negativas.
 
* **Clasificación de documentos **: Se encarga de agrupar documentos según la similitud que exista entre ellos.  
 
* **Propagación de contenidos**: Análisis del impacto que causa alguna acción puntual. El problema pasa por identificar qué nodos de la red mencionan la temática en cuestión y estudiar cómo se espera que se propaguen.

## Preprocesamiento del texto

Es necesario un preproceso de los textos para facilitar la creación de unos datos limpios para poder aplicar varios algoritmos de text mining o machine learnig.

* Proceso de **Normalización**: Identificación y corrección de errores ortográficos, pasar todo el texto a minúsculas y borrar signos de puntuación y números o caracteres sin semántica. 

* Proceso de **Tokenización**: Identificación de los términos que componen una frase.

* Proceso de **eliminación de stopwords**: Eliminación de términos carentes de significado de cara a la clasificación, tales como preposiciones, artículos, conjunciones, etc.

* Proceso de **lematización/stemming**: Agrupación de las palabras según su familia léxica, se reduce cada término a su raíz. Este proceso resulta interesante a la hora de ver el número de veces que aparecen y determinar las palabras idóneas que representan el contenido del texto.

* Análisis **Part Of Speech (POS)**: Asignación de una categoría lingüística a cada token o palabra en función de su papel dentro de las sentencias: adjetivo, nombre, preposición, verbo, etc. 

## Document Term Matrix (DTM) 

Una vez realizado el preprocesamiento de los textos estamos listos para crear una matriz donde las filas se corresponden a los textos o documentos y las columnas a los tokens o términos. Esta matriz se suele llamar Document Term Matrix (DTM). 

Hay varias formas de 

* *Term frequency (tf)*: Número de veces que aparece un término *t* en un documento *d*, *f(t,d)*. Usualmente se usa la fórmula normalizada.

$$tf(t,d) = \frac{f(t,d)}{max(f(w,d) : w \in d)}$$

* *Inverse document frequency*: La frecuencia inversa de documento es una medida de si el término es común o no, en la colección de documentos, *D*. Se obtiene dividiendo el número total de documentos por el número de documentos que contienen el término, y se toma el logaritmo de ese cociente.

$$idf(t,D) = \log \frac{\big| D \big|}{\Big| \big\{ d \in D : t \in d \big\} \Big|}$$

* *Term frequency – Inverse document frequency (tf-idf)*: La frecuencia de ocurrencia del término en la colección de documentos.

$$tfidf(t,d,D) = tf(t,d) \times idf(t,D)$$

La DTM será la matriz con la que alimentaremos a los algoritmos de Text Mining. Hay que tener en cuenta que estas matrices pueden ser muy grandes, ya que tienen tantas columnas como palabras en el corpus y para gran cantidad de documentos estas matrices son muy sparse. Se suelen suprimir los términos que con menos frecuencia o los que aparecen en menor cantidad de textos. 

Por otra parte, también se puede complementar la DTM con los n-gramas más frecuentes. Un n-grama es una subsecuencia de n elementos en el texto. 

## Aplicación en R

Veamos ahora como aplicar todo lo visto anteriormente en R.

### Ejemplo 01: Algunas frases



```r
library(tm)
textos = c("Un pequeño paso para el hombre, un gran salto para la humanidad.", 
           "Pueblo que ignora su historia, pueblo que está condenado a repetirla.",
           "Todos somos genios. Pero si juzgas a un pez por su capacidad de trepar árboles, vivirá toda su vida pensando que es un inútil",
           "Más vale morir de pie que vivir de rodillas.", 
           "Nunca te olvides de sonreír, porque el día que no sonrías será un día perdido.")

docs_ini = VCorpus(VectorSource(textos))

writeLines(as.character(docs_ini[[1]]))
```

```
## Un pequeño paso para el hombre, un gran salto para la humanidad.
```

```r
writeLines(as.character(docs_ini[[2]]))
```

```
## Pueblo que ignora su historia, pueblo que está condenado a repetirla.
```

```r
# Cleaning docs
docs = tm_map(docs_ini, removePunctuation)
docs = tm_map(docs, removeNumbers)
docs = tm_map(docs, content_transformer(tolower))
docs = tm_map(docs, removeWords, stopwords("spanish"))
docs = tm_map(docs, stripWhitespace)

# Stemming
docs = tm_map(docs, stemDocument, "spanish")

writeLines(as.character(docs[[1]]))
```

```
## pequeño paso hombr gran salto humanidad
```

```r
writeLines(as.character(docs[[2]]))
```

```
## pueblo ignora historia pueblo condenado repetirla
```

```r
# Create document-term matrix
dtm = DocumentTermMatrix(docs)
inspect(dtm)
```

```
## <<DocumentTermMatrix (documents: 5, terms: 33)>>
## Non-/sparse entries: 33/132
## Sparsity           : 80%
## Maximal term length: 9
## Weighting          : term frequency (tf)
## Sample             :
##     Terms
## Docs árbole capacidad condenado día genio gran historia hombr humanidad
##    1      0         0         0   0     0    1        0     1         1
##    2      0         0         1   0     0    0        1     0         0
##    3      1         1         0   0     1    0        0     0         0
##    4      0         0         0   0     0    0        0     0         0
##    5      0         0         0   2     0    0        0     0         0
##     Terms
## Docs pueblo
##    1      0
##    2      2
##    3      0
##    4      0
##    5      0
```

```r
# Create document-term matrix tf-idf
dtm = DocumentTermMatrix(docs, control=list(weighting=weightTfIdf))
inspect(dtm)
```

```
## <<DocumentTermMatrix (documents: 5, terms: 33)>>
## Non-/sparse entries: 33/132
## Sparsity           : 80%
## Maximal term length: 9
## Weighting          : term frequency - inverse document frequency (normalized) (tf-idf)
## Sample             :
##     Terms
## Docs condenado      día     gran historia     morir       pie   pueblo
##    1  0.000000 0.000000 0.386988 0.000000 0.0000000 0.0000000 0.000000
##    2  0.386988 0.000000 0.000000 0.386988 0.0000000 0.0000000 0.773976
##    3  0.000000 0.000000 0.000000 0.000000 0.0000000 0.0000000 0.000000
##    4  0.000000 0.000000 0.000000 0.000000 0.4643856 0.4643856 0.000000
##    5  0.000000 0.663408 0.000000 0.000000 0.0000000 0.0000000 0.000000
##     Terms
## Docs   rodilla      vale     vivir
##    1 0.0000000 0.0000000 0.0000000
##    2 0.0000000 0.0000000 0.0000000
##    3 0.0000000 0.0000000 0.0000000
##    4 0.4643856 0.4643856 0.4643856
##    5 0.0000000 0.0000000 0.0000000
```

### Ejemplo 02: Opiniones en una encuesta

Veamos ahora un ejemplo con opiniones recopiladas en una encuesta sobre un servicio de Correos.





























