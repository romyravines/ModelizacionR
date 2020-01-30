# Text Mining




En esta secci�n veremos como procesar texto y alguna de sus aplicaciones. La miner�a de textos es un campo de las ciencias de la computaci�n, inteligencia artificial y ling��stica que estudia las interacciones entre las computadoras y el lenguaje humano.

## Aplicaciones

* **Extracci�n de informaci�n (*feature extraction*) **: Se centra en obtener nombres propios, organizaciones o eventos, as� como fechas y encontrar la relaci�n entre ellas.
 
* **An�lisis de sentimientos (*sentiment analysis*) **: Analiza el vocabulario de un texto con el fin de determinar sus cargas emocionales. Para ello se pueden utilizar los llamados *diccionarios de polaridad*, listas de palabras asociadas a mensajes con connotaciones positivas o negativas.
 
* **Clasificaci�n de documentos **: Se encarga de agrupar documentos seg�n la similitud que exista entre ellos.  
 
* **Propagaci�n de contenidos**: An�lisis del impacto que causa alguna acci�n puntual. El problema pasa por identificar qu� nodos de la red mencionan la tem�tica en cuesti�n y estudiar c�mo se espera que se propaguen.

## Preprocesamiento del texto

Es necesario un preproceso de los textos para facilitar la creaci�n de unos datos limpios para poder aplicar varios algoritmos de text mining o machine learnig.

* Proceso de **Normalizaci�n**: Identificaci�n y correcci�n de errores ortogr�ficos, pasar todo el texto a min�sculas y borrar signos de puntuaci�n y n�meros o caracteres sin sem�ntica. 

* Proceso de **Tokenizaci�n**: Identificaci�n de los t�rminos que componen una frase.

* Proceso de **eliminaci�n de stopwords**: Eliminaci�n de t�rminos carentes de significado de cara a la clasificaci�n, tales como preposiciones, art�culos, conjunciones, etc.

* Proceso de **lematizaci�n/stemming**: Agrupaci�n de las palabras seg�n su familia l�xica, se reduce cada t�rmino a su ra�z. Este proceso resulta interesante a la hora de ver el n�mero de veces que aparecen y determinar las palabras id�neas que representan el contenido del texto.

* An�lisis **Part Of Speech (POS)**: Asignaci�n de una categor�a ling��stica a cada token o palabra en funci�n de su papel dentro de las sentencias: adjetivo, nombre, preposici�n, verbo, etc. 

## Document Term Matrix (DTM) 

Una vez realizado el preprocesamiento de los textos estamos listos para crear una matriz donde las filas se corresponden a los textos o documentos y las columnas a los tokens o t�rminos. Esta matriz se suele llamar Document Term Matrix (DTM). 

Hay varias formas de 

* *Term frequency (tf)*: N�mero de veces que aparece un t�rmino *t* en un documento *d*, *f(t,d)*. Usualmente se usa la f�rmula normalizada.

$$tf(t,d) = \frac{f(t,d)}{max(f(w,d) : w \in d)}$$

* *Inverse document frequency*: La frecuencia inversa de documento es una medida de si el t�rmino es com�n o no, en la colecci�n de documentos, *D*. Se obtiene dividiendo el n�mero total de documentos por el n�mero de documentos que contienen el t�rmino, y se toma el logaritmo de ese cociente.

$$idf(t,D) = \log \frac{\big| D \big|}{\Big| \big\{ d \in D : t \in d \big\} \Big|}$$

* *Term frequency � Inverse document frequency (tf-idf)*: La frecuencia de ocurrencia del t�rmino en la colecci�n de documentos.

$$tfidf(t,d,D) = tf(t,d) \times idf(t,D)$$

La DTM ser� la matriz con la que alimentaremos a los algoritmos de Text Mining. Hay que tener en cuenta que estas matrices pueden ser muy grandes, ya que tienen tantas columnas como palabras en el corpus y para gran cantidad de documentos estas matrices son muy sparse. Se suelen suprimir los t�rminos que con menos frecuencia o los que aparecen en menor cantidad de textos. 

Por otra parte, tambi�n se puede complementar la DTM con los n-gramas m�s frecuentes. Un n-grama es una subsecuencia de n elementos en el texto. 

## Aplicaci�n en R

Veamos ahora como aplicar todo lo visto anteriormente en R.

### Ejemplo 01: Algunas frases



```r
library(tm)
textos = c("Un peque�o paso para el hombre, un gran salto para la humanidad.", 
           "Pueblo que ignora su historia, pueblo que est� condenado a repetirla.",
           "Todos somos genios. Pero si juzgas a un pez por su capacidad de trepar �rboles, vivir� toda su vida pensando que es un in�til",
           "M�s vale morir de pie que vivir de rodillas.", 
           "Nunca te olvides de sonre�r, porque el d�a que no sonr�as ser� un d�a perdido.")

docs_ini = VCorpus(VectorSource(textos))

writeLines(as.character(docs_ini[[1]]))
```

```
## Un peque�o paso para el hombre, un gran salto para la humanidad.
```

```r
writeLines(as.character(docs_ini[[2]]))
```

```
## Pueblo que ignora su historia, pueblo que est� condenado a repetirla.
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
## peque�o paso hombr gran salto humanidad
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
## Docs �rbole capacidad condenado d�a genio gran historia hombr humanidad
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
## Docs condenado      d�a     gran historia     morir       pie   pueblo
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





























