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

¿Cómo aprendemos los humanos? En principio la mayoría del conocimiento humano proviene de nuestra experiencia con los objetos, es decir, aprendemos de los datos que obtenemos acerca de ellos y no a partir de algún tipo de definición matemática.

Esta habilidad de aprender de los datos, nos ha resultado bastante útil, dado que existe una amplia variedad de problemas que no se pueden resolver de forma analítica. En estas situaciones los datos nos permiten encontrar soluciones empíricas, que no necesariamente explican el porqué de las cosas, pero si ofrecen resultados útiles para la práctica. Por esta razón, la capacidad de aprender a partir de los datos es una técnica de mucha importancia para todas las profesiones y disciplinas.

En esta ocasión se va a tratar brevemente los principales elementos que constituyen **el problema del aprendizaje a partir de los datos**, para finalmente entender cómo las máquinas también pueden aprender.

## El problema del aprendizaje

El poder aprender a partir de los datos es un proceso que puede ser automatizado, es decir, se pueden elaborar algoritmos que realizan esta tarea. En este punto es importante entender que los algoritmos que aprenden de los datos solo tratan de encontrar la mejor solución para predecir resultados, y no necesariamente encuentran el porqué. Aquí los datos guían a los algoritmos para construir la fórmula que ofrece las mejores aplicaciones en el sentido práctico.

En un sentido más matemático, el problema del aprendizaje se puede formular a partir de tres espacios medibles $\mathcal{X}$, $\mathcal{Y}$ y $\mathcal{Z}$, en donde $\mathcal{Z} \subset \mathcal{X} \times \mathcal{Y}$ representa a una **relación** entre los datos de $\mathcal{X}$ e $\mathcal{Y}$. En principio, **la tarea de aprendizaje** consiste en entender cuál es la estructura de $\mathcal{Z}$ a partir de una **muestra de datos** $S=(s\_{i})\_{i\in [m]}$ y alguna **función de perdida** $\mathcal{L}: \mathcal{M}( \mathcal{X}, \mathcal{Y} )\times \mathcal{Z} \to \mathbb{R}$, en donde $\mathcal{M}( \mathcal{X}, \mathcal{Y} )$ es el conjunto de todas las funciones medibles de $\mathcal{X}$ a $\mathcal{Y}$. La función $\mathcal{L}$ se emplea principalmente para medir cúal es el _performance_ de nuestro aprendizaje.

Resolver esta tarea implica hacer la elección de un conjunto de **hipótesis** $\mathcal{H} \subset \mathcal{M}( \mathcal{X}, \mathcal{Y} )$ y de la construcción de un **algoritmo de aprendizaje**, es decir, un mapeo:

\begin{equation}
\mathcal{A}: \bigcup\_{ m\in \mathbb{N} } \mathcal{Z}^{m} \to \mathcal{H}
\end{equation}

que a partir de una **muestra de datos** $S = (s\_i)_{i\in[m]}$ de cierto tamaño $m$ logre encontrar un **modelo** $f\_S = A(S)\in \mathcal{H}$ con «_buen comportamiento_» en $S$ y «_capacidad de generalizar_» para los datos desconocidos en $\mathcal{Z} \setminus S$. Aquí, el buen comportamiento se mide via la función de perdida $\mathcal{L}$ y corresponde a la perdida $\mathcal{L}(f\_S, z)$ e informalmente la capacidad de generalizar quiere decir que el comportamiento de $f\_S$ en $z\in \mathcal{Z}\setminus S$ es similar a $z\in \mathcal{S}$.

Como se puede apreciar la nociones de «_buen comportamiento_» y «_la capacidad de generalizar_» son bastante vagas, sin embargo, para tener una mejor idea de estas nociones, podemos centrar la atención en el comportamiento de función $\mathcal{L}$ sobre los conjuntos $\mathcal{H}\times S$ y $\mathcal{H}\times (Z\setminus S)$, en donde, $\mathcal{L}\_{in} = \mathcal{L}|\_{\mathcal{H}\times S}$ se define como **la perdida en la muestra** o simplemente **la perdida empírica** y $\mathcal{L}\_{out} = \mathcal{L}|\_{\mathcal{H}\times (Z \setminus S)}$ se define como **la perdida fuera de la muestra** o simplemente **la perdida ideal**. Ahora bien, si existiera el modelo $f^*\in \mathcal{H}$ tal que $f^\*(\mathcal{X}) = \mathcal{Z}$, entonces la perdida empírica y la perdida ideal serían nulas, pues el modelo no presentaría ningún error en su tarea de predicción. En la práctica, en general, no es posible encontrar el modelo con las características de $f^\*$. Sin embargo los esfuerzos se centran en encontrar un modelo que mínimiza el **error empírico**: 
\begin{equation}
  L\_{in}(f) = \frac{1}{m}\sum\_{i=1}^{m}\mathcal{L}\_{in}(f, z_i),
\end{equation}
es decir:, para una muestra de datos $S = (s\_i)_{i \in[m]}$ y un conjunto de hipótesis $\mathcal{H}$, se busca un modelo $f\_s\in \mathcal{H}$ tal que:
\begin{equation}\tag{1}
f_s \in \operatorname\*{argmin}\_{f\in \mathcal{H}} L\_{in}(f).
\end{equation}

De esta forma se está asegurando el _buen comportamiento_ de $f\_{s}$ en $S$. Sin embargo, esta estrategía encierra un detalle con un gran demonio en su interior, el **sobreentrenamiento**. Por otro lado, en la práctica es posible encontrar modelos en donde el error empírico es cero, $L\_{in}(f) = 0$  y el error ideal es diferente de cero, $L\_{out}(f) \neq 0$ para todo $z\_i\in Z \setminus S$; un modelo con estas características carece de _la capacidad de generalizar_. Este es un error frecuente, que sucede cuando se buscan modelos que se aprenden muestras de memoría. 

¿Entonces cómo garantizar _la capacidad de generalizar_? Aquí hay un problema complejo, que en primer lugar consiste en la elección adecuada de un conjunto de hipótesis $\mathcal{H}$, de manera que para cualquier $\epsilon > 0$ exista una muestra $S$ que garantice que:
\begin{equation}
  \forall h\in \mathcal{H}, \\; \\;|L\_{in}(f) - L\_{out}(f)| \leq \epsilon.
\end{equation}
Una vez se ha identificado la clase de hipotesis adecuada, $\mathcal{H}$, debemos en segundo lugar, encontrar la hipótesis $f$ en $\mathcal{H}$ que satisface la ecuación (1). Si se logra conseguir un modelo con estas características, podremos decir, que tiene _la capacidad de generalizar_.

## Tareas de predicción y clasificación
 Aqu´
Una **tarea de predicción**, se pude definir a partir de un conjunto de entrenamiento $S = (x_i, y_i)_{i\in[m]}$ que consiste de una entrada de características $x_i\in \mathcal{X}$ y una etiqueta correspondiente $y_i\in \mathcal{Y}$. En el caso de una regresión 1 - dimensional, con $\mathcal{Y} \subset \mathbb{R}$, la función de perdida se define como $\mathcal{L}(f, (x, y)) = (f(x) - y)^2$ y, para una **tarea de clasificación** binaria con $\mathcal{Y} = \\{-1, 1\\}$, la función de perdida es $\mathcal{L}(f, (x, y)) = 1\_{(-\infty, 0)}(yf(x)).$ Nosotros estamos asumiendo por ahora que las características están en un espacio Euclidiano, es decir, $\mathcal{X}\subset \mathbb{R}^{d}$ en donde $d\in \mathbb{N}$.

[Siguiente entrada](url)

## Referencias

1. [Julius Berner. 2021. The Modern Mathematics of deep learning.](https://deepai.org/publication/the-modern-mathematics-of-deep-learning)
2. [Yaser Abu-Mostafa Data. 2012 - 2015. Learning From Data.](https://work.caltech.edu/telecourse)
