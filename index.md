--- 
title: "Modelización en R"
author: "Aula Innova. Innova-tsn"
date: "Sabadell. Enero-Febrero, 2018"
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
  bookdown::pdf_book:
    keep_tex: yes
    fig_caption: true
    latex_engine: xelatex
  bookdown::html_book:
  css: [style.css, font-awesome.min.css]  
documentclass: book
bibliography: [book.bib]
biblio-style: apalike
link-citations: yes
description: "Curso-taller preparado para profesionales analiticos."
---


# Presentación {-}



<img src="C:/Users/romy.rodriguez/Documents/INNOVA/Formacion/MiCurso/ModelizacionR/imgs/sabadellcurso.jpg" width="75%" />

**Modelización con R** es un manual práctico para la creación y utilización de modelos analíticos con el lenguaje R [@R-base]. En particular, esta versión del manual ha sido desarrollada para el equipo de _modelling_ del **Banc Sabadell** en Barcelona. 

El objetivo de este manual es presentar de manera resumida, los principales algoritmos del _analytics_, destacando cuándo deben ser utilizados y cómo evaluar su performance.

Cada capítulo corresponde a un tópico. Se pueden leer de manera individual. Todos los ejemplo están desarrollados con datos incluidos en las librerias de R y/o se encuentran disponibles en la web.

Para realizar las prácticas sugeridas en este manual, el lector requiere un portátil donde tenga instaladas las últimas versiones de R y RStudio. Además debe contar con una conexión a internet  y la posibilidad de instalar las librerías que se citan a lo largo del documento.
