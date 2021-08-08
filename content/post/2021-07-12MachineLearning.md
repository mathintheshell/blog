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

En un sentido más matemático, el problema del aprendizaje se puede formular a partir de tres espacios medibles $\mathcal{X}$, $\mathcal{Y}$ y $\mathcal{Z}$, en donde el conjunto $\mathcal{Z}$ es un subconjunto de $\mathcal{X} \times \mathcal{Y}$ que representa a una **relación** entre los datos de $\mathcal{X}$ e $\mathcal{Y}$. En principio, **la tarea de aprendizaje** consiste en entender la relación $\mathcal{Z}$ a partir de una **muestra de datos** $S=(s\_{i})\_{i\in [m]}$ y alguna **función de perdida** $\mathcal{L}: \mathcal{M}( \mathcal{X}, \mathcal{Y} )\times \mathcal{Z} \to \mathbb{R}$ definida sobre el producto cartesiano entre el conjunto $\mathcal{M}( \mathcal{X}, \mathcal{Y} )$ de todas las funciones medibles de $\mathcal{X}$ a $\mathcal{Y}$ y el conjunto $\mathcal{Z}$, y con imagen en los número reales. La función $\mathcal{L}$ se emplea principalmente para medir cúal es el _performance_ del aprendizaje en el algoritmo.

Resolver esta tarea implica hacer la elección de un conjunto de **hipótesis** $\mathcal{H} \subset \mathcal{M}( \mathcal{X}, \mathcal{Y} )$ y de la construcción de un **algoritmo de aprendizaje**, es decir, un mapeo:

\begin{equation}
\mathcal{A}: \bigcup\_{ m\in \mathbb{N} } \mathcal{Z}^{m} \to \mathcal{H}
\end{equation}

que a partir de una **muestra de datos** $S = (s\_i)_{i\in[m]}$ de cierto tamaño $m$ logre encontrar un **modelo** $h\_S = \mathcal{A}(S)\in \mathcal{H}$ con «_buen comportamiento_» en $S$ y «_capacidad de generalizar_» para los datos desconocidos en $\mathcal{Z} \setminus \\{s\_i\\}\_{i\in[m]}$. Aquí, el buen comportamiento se mide via la función de perdida $\mathcal{L}$ y corresponde a la perdida $\mathcal{L}(h\_S, z)$ e informalmente la capacidad de generalizar quiere decir que el comportamiento de $h\_S$ en $z\in \mathcal{Z} \setminus \\{s\_i\\}\_{i\in[m]}$ es similar a $z\in \mathcal{S}$.

Como se puede apreciar la nociones de «_buen comportamiento_» y «_la capacidad de generalizar_» son bastante vagas, sin embargo, para tener una mejor idea de estas nociones, la atención se puede centrar en los conceptos de **función de riesgo**  y de **riesgo empírico** cómo veremos a continuación

La **función de riesgo** para una hipótesis $h\in \mathcal{H}$ con respecto a la distribucción de probabilidad $\mathcal{D}$ sobre $\mathcal{Z}$, se define como: 

\begin{equation}\tag{1}
L\_{D}(h) = \mathbb{E}\_{z\sim \mathcal{D}}[\mathcal{L}(h, z)].
\end{equation}

Observe que para esta definición, la esperanza de la función de perdida de $h$ es sobre los datos de $z$ muestrados aleatoriamente de acuerdo con la distribucción $\mathcal{D}$. Hay que señalar que en la práctica la distribucción $\mathcal{D}$ es esencialmente desconocida. 

Similarmente , el **riesgo empírico** es la perdida esperada sobre un muestra de datos  $S = (s_i)\_{i \in[m]}$, es decir: 
 
\begin{equation}\tag{2}
L\_{S}(h) = \frac{1}{m}\sum\_{i=1}^{m}\mathcal{L}(h, s\_i).
\end{equation}

Ahora bien, si existiera el modelo $h^\*\in \mathcal{H}$ tal que el **riesgo ideal** es cero,  $L\_{D}(h^\*)=0$, entonces  el **riesgo empírico** también es cero,  $L\_{S}(h^\*)=0$, pues el modelo no presentaría ningún error en su tarea de predicción, aunque esto es lo esperado, por lo general, no es posible encontrar el modelo con las características de $h^\*$. Para encontrar el mejor modelo que realice la tarea, los esfuerzos se centran en una muestra de datos $S = (s_i)\_{i \in[m]}$ y un conjunto de hipótesis $\mathcal{H}$, para un encontrar un modelo $h\_s\in \mathcal{H}$ tal que:

\begin{equation}\tag{3}
h_s \in \operatorname\*{argmin}\_{h\in \mathcal{H}} L\_{S}(h).
\end{equation}

De esta forma se estará asegurando el _buen comportamiento_ de $h\_{s}$ en $S$. Sin embargo, esta estrategía encierra un detalle con un gran demonio en su interior, el **sobreentrenamiento**. En la práctica es posible encontrar modelos en donde el riesgo empírico es cero, $L\_{S}(h) = 0$ y la perdida fuera de la muestra es $\mathcal{L}(h, z) \neq 0$ para todo $z\in Z \setminus \\{s\_i\\}\_{i\in[m]}$; un modelo con estas características carece de _la capacidad de generalizar_. Este es un error frecuente, que ocurre con frecuencia cuando solo se utiliza un solo conjunto de datos para entrenar el algoritmo \mathcal{A}. Para evitar esto, es usual dividir el conjunto $S$ en dos, un conjunto para realizar el entrenamiento del algoritmo y el otro para medir su _capacidad de generalizar_. El objetivo es que el modelo encontrado tenga un comportamiento similar en ambos conjuntos.

¿Cómo garantizar _la capacidad de generalizar_? Como hemos visto, aquí hay un problema complejo, que en primer lugar consiste en hacer la elección adecuada de un conjunto de hipótesis $\mathcal{H}$, de manera que para cualquier $\epsilon > 0$ exista una muestra $S$ que garantice que:
\begin{equation}
\forall h\in \mathcal{H}, \\; \\;|L\_{S}(h) - L\_{D}(h)| \leq \epsilon.
\end{equation}
Una vez se ha identificado la clase de hipótesis adecuada, $\mathcal{H}$, debemos en segundo lugar, encontrar la hipótesis $h$ en $\mathcal{H}$ que satisface la ecuación (3). Si se logra conseguir un modelo con estas características, podremos decir, que nuestros modelo que tiene _la capacidad de generalizar_ y tiene _buen comportamiento_.

## Tareas de predicción y clasificación

Veamos algunos ejemplos de problemas de aprendizaje basado en datos:

**Clasificación Multiclase**. Consideremos la tarea de clasificar documentos. Nuestro deseo es diseñar un programa con la capacidad para clasificar una colección de documentos, de acuerdo a diferentes tópicos (e.g., noticias, deportes, biología, medicina). Un algoritmo de aprendizaje para esta tarea debería tener acceso a una colección de documentos correctamente clasificados, $S$, y con base a estos ejemplos, debería entregar una programa (modelo) que puede tomar un nuevo documentos y clasificarlo. Aquí el **conjunto de dominio**, $\mathcal{X}$, es el conjunto de todos los posibles documentos. Es importante señalar, que los documentos debería ser representando por un conjunto de características que podría incluir el número de palabras diferentes en el documentos, el tamaño del documentos, el autor, el origin, etc. El **conjunto de etiquetas**, $\mathcal{Y}$ es el conjunto de todos los posibles tópicos (en este caso, debería ser algún conjunto finito). Una vez hemos identificado nuestros conjuntos de dominio y etiquetas, el otro componente que hace falta es determina una función de perdida adecuada para medir el _perfomance_ de nuestro algoritmo. 

En este caso sobre la variable aleatoria $z$ con rango en $\mathcal{X}\times \mathcal{Y}$ se puede considerar la siguiente función de perdida:

\begin{equation}
\mathcal{L}(h, (x, y)) = \begin{cases}
0 \mbox{ si } h(x) = y \newline \newline
1 \mbox{ si } h(x)\neq y
\end{cases}
\end{equation}

Esta función se usa en problemas de clasificación binaria o multiclase. 

**Regresión**. En esta tarea, el objetivo es encontrar algún patrón simple en los datos --una relación funcional entre los componentes de los datos $\mathcal{X}$ y $\mathcal{Y}$--. Por ejemplo, encontrar la mejor función que predice el peso de nacimiento de un bebe en relación con las medidas obtenidas por ultrasonido del diámetro de su cabeza, el diámetro abdominal y la longitud de su femur. Aquí el dominio es algún subconjunto de $\mathbb{R}^{3}$ (las tres medidas obtenidas por el ultrasonido) y el conjunto de etiquedas de $\mathcal{Y}$ es el conjunto de los números reales (el peso en gramos). En este contexto  es más adecuado llamar a $\mathcal{Y}$ como el conjunto objetivo. En este caso el conjunto de entrenamiento es igual que antes, un subconjunto $S\subseteq \mathcal{X}\times \mathcal{Y}$. Sin embargo, la medida de éxito es diferente. En este ejemplo, se podría evaluar la calidad de la hipótesis $h:\mathcal{X}\to \mathcal{Y}$ por el valor esperado del cuadrado de la diferencia entre las etiquetas correctas y su predicción, es decir:

\begin{equation}
\mathcal{L}(h, (x,y)) = (h(x)-y)^2.
\end{equation}

En otras entradas veremos otras funciones de perdidas utilizadas en tareas de regresión. 

## ¿Cómo aprende las maquinas?


## Aprendizaje agnóstico correcto probablemente aproximado


[Siguiente entrada](url)

## Referencias

1. [Julius Berner. 2021. The Modern Mathematics of deep learning.](https://deepai.org/publication/the-modern-mathematics-of-deep-learning)
2. [Yaser Abu-Mostafa Data. 2012 - 2015. Learning From Data.](https://work.caltech.edu/telecourse)
