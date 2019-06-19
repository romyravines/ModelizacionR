--- 
title: "Modelización en R"
author: "Romy Rodríguez-Ravines"
date: "2019"
site: bookdown::bookdown_site
output:
  bookdown::gitbook:
    lib_dir: assets
    split_by: section
    config:
     toc:
       collapse: section
       scroll_highlight: yes
     toolbar:
       position: fixed
     edit : null
     download: yes
     search: yes
     fontsettings:
       theme: white
       family: sans
       size: 2
     sharing: null
  bookdown::html_book:
  css: [style_Romy, font-awesome.min.css]  
documentclass: book
bibliography: [book.bib]
biblio-style: apalike
link-citations: yes
description: "Manual básico para profesionales que utilizan analítica avanzada."
---


# Presentación {-}




`


**Modelización con R** es un manual práctico para la creación y utilización de modelos analíticos con el lenguaje R [@R-base]. Esta versión del manual puede  ser utilizada por equipos analíticos, _data scientists_ de cualquier organización. 

El objetivo de este manual es presentar de manera resumida, los principales algoritmos del _analytics_, destacando cuándo deben ser utilizados y cómo evaluar los resultados que ofrece.

Cada capítulo corresponde a un tipo de modelo o problema predictivo. Se pueden leer de manera individual. Todos los ejemplos están desarrollados con datos incluídos en las librerias de R y/o se encuentran disponibles en la web. Son de uso libre.

Para realizar las prácticas sugeridas en este manual, el lector requiere un portátil donde tenga instaladas las últimas versiones de R y RStudio. Además debe contar con una conexión a internet  y la posibilidad de instalar las librerías que se citan a lo largo del documento.


Empezamos!

<img src="imgs/browsing.png" width="30%" />

<!-- caption: "Icons [Freepik](https://www.freepik.com/) from [Flaticon](https://www.flaticon.com/)" -->
