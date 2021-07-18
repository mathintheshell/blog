---
title: "Aprendiendo de los datos"
date: 2021-07-12T08:19:27-05:00
draft: false
excerpt: Una breve introducción al problema del aprendizaje a partir de los datos.
hero: /images/ml-01.jpg
timeToRead: 4
authors:
  - Hugo Authors
---

¿Cómo aprendemos los humanos? En principio la mayoría del conocimiento humano proviene de nuestra experiencia con los objetos, es decir, aprendemos de los datos que obtenemos acerca de ellos y no a partir de algún tipo de definición matemática de lo que son.

Esta habilidad de aprender de los datos, nos ha resultado bastante útil, dado que existe una amplia variedad de problemas que no se pueden resolver de forma analítica. En estas situaciones los datos nos permiten encontrar soluciones empíricas, que no necesariamente explican  el porqué de las cosas, pero si ofrecen resultados útiles para la práctica. Por esta razón, la capacidad de aprender a partir de los datos es una técnica de mucha importancia para todas las profesiones y disciplinas. 

En esta ocasión se va a tratar brevemente los principales elementos que constituyen __el problema del aprendizaje a partir de los datos__, para finalmente entender cómo las máquinas también pueden aprender.

## El problema del aprendizaje 

El poder aprender a partir de los datos es un proceso que puede ser automatizado, es decir, se pueden elaborar algoritmos que realizan esta tarea. En este punto es importante entender que los algoritmos que aprenden de los datos solo tratan de encontrar la mejor solución para predecir resultados, y no necesariamente encuentran el porqué. Aquí los datos guían a los algoritmos para construir la fórmula que ofrece las mejores aplicaciones en el sentido práctico.

En un sentido más matemático, el problema del aprendizaje se puede formular a partir de tres espacios medibles $\mathcal{X}$, $\mathcal{Y}$ y $\mathcal{Z}$, en donde  $\mathcal{Z} \subset \mathcal{X} \times \mathcal{Y}$ y representa a una __relación__ entre los datos de $\mathcal{X}$ e $\mathcal{Y}$. En principio, __la tarea de aprendizaje__ consiste en entender cuál es la estructura de $\mathcal{Z}$ a partir de una __muestra de datos__ $S=(s\_{i})\_{i\in [m]}$ y alguna  __función de perdida__ $\mathcal{L}: \mathcal{M}( \mathcal{X}, \mathcal{Y} )\times \mathcal{Z} \to \mathbb{R}$, en donde $\mathcal{M}( \mathcal{X}, \mathcal{Y} )$ es el conjunto de todas las funciones medibles de $\mathcal{X}$ a $\mathcal{Y}$.  La función $\mathcal{L}$ se emplea principalmente para medir cúal es el _performance_ de nuestro aprendizaje. 

Resolver esta tarea implica hacer la elección de un conjunto de __hipótesis__ $\mathcal{H} \subset \mathcal{M}( \mathcal{X}, \mathcal{Y} )$ y de la  construcción de un __algoritmo de aprendizaje__, es decir, un mapeo:

\begin{equation}\tag{1}
\mathcal{A}: \bigcup\_{ m\in \mathbb{N} } \mathcal{Z}^{m} \to \mathcal{H}
\end{equation}

que a partir de una __muestra de datos__ $S = (s\_i)_{i=1}^m$ de cierto tamaño $m$ logre encontrar un __modelo__ $f\_S = A(S)\in \mathcal{H}$ que se «_comporta bien_» en $S$ y tiene la «_capacidad de generalizar_» para los datos desconocidos en $\mathcal{Z} \setminus S$. Aquí, el buen comportamiento se mide via la función de perdida $\mathcal{L}$ y corresponde a la perdida $\mathcal{L}(f\_S, z)$ e informalmente la capacidad de generalizar quiere decir que el comportamiento de $f\_S$ en $z\in \mathcal{Z}\setminus S$ es similar a $z\in \mathcal{S}$.

Como se puede apreciar la noción de la capacidad de generalizar es bastante vaga, sin embargo no nos centraremos en esto por ahora. Por simplicidad, vamos a centrar nuestra atención en un par de ejemplos para entender mejor la naturaleza del problema del aprendizaje a partir de los datos. 

## Tareas de predicción y clasificación

Una __tarea de predicción__, se pude definir a partir de un conjunto de entrenamiento $S = (x_i, y_i)_{i=1}^m$ que consiste de un entrada de características $x_i\in \mathcal{X}$ y una etiqueta correspondiente $y_i\in \mathcal{Y}$. En el caso de una regresión 1 - dimensional, con $\mathcal{Y} \subset \mathbb{R}$, la función de perdida se define como $\mathcal{L}(f, (x, y)) = (f(x) - y)^2$ y, para una __tarea de clasificación__ binaria con $\mathcal{Y} = \\{-1, 1\\}$, la función de perdida es $\mathcal{L}(f, (x, y)) = 1\_{(-\infty, 0)}(yf(x)).$ Nosotros estamos asumiendo por ahora que las características están en un espacio Euclidiano, es decir, $\mathcal{X}\subset \mathbb{R}^{d}$ en donde $d\in \mathbb{N}$.



[Siguiente entrada](url)

## Referencias
1. [Julius Berner. 2021. The Modern Mathematics of deep learning.](https://deepai.org/publication/the-modern-mathematics-of-deep-learning)
2. [Yaser Abu-Mostafa Data. 2012 - 2015. Learning From Data.](https://work.caltech.edu/telecourse)