---
jupytext:
  formats: ipynb,md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.10.2
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Representación de sistemas dinámicos en espacio de estados

En ingeniería de control, el control clásico se analiza por conveniencia en el dominio de la frecuencia (la variable $s$ o en la variable $\omega$), y está limitado a sistemas lineales con una única entrada y una única salida. La representación de espacios de estado proporciona un modo compacto y conveniente de modelar y analizar sistemas con múltiples entradas y salidas, tanto para sistemas lineales como no lineales.

Una representación de espacios de estados es un modelo matemático de un sistema físico descrito mediante un conjunto de entradas, salidas y variables de estado relacionadas por ecuaciones diferenciales de cualquier orden en el dominio del tiempo, que se combinan en una ecuación diferencial matricial de primer orden. Las variables de entradas, salidas y estados son convenientemente expresadas como vectoresː un vector de entrada, un vector de salida y un vector de estados; y si el sistema dinámico es lineal e invariante en el tiempo, las ecuaciones algebraicas se escriben en forma matricial.

Las *n* variables de estado conforman un **vector de estado**, que da lugar al denominado espacio de estado de *n* dimensiones.

+++

## Variables de estado

Las variables de estado son **el subconjunto más pequeño de variables de un sistema que pueden representar su estado dinámico completo en un determinado instante**. Las variables de estado deben ser independientes entre sí. En el caso de la representación lineal de un sistema, las variables de estado deben ser linealmente independientes entre síː una variable de estado no puede ser una combinación lineal de otras variables de estado.

El número mínimo de variables de estado necesarias para representar un sistema dado es usualmente igual al orden de la ecuación diferencial que define al sistema.

En circuitos eléctricos, el número de variables de estado es a menudo, pero no siempre, igual al número de elementos almacenadores de energía, como bobinas y condensadores.

+++

## Sistemas lineales en espacios de estados

Una forma general de representación de espacios de estado de un sistema lineal con $p$ entradas, $q$ salidas y $n$ variables de estado se escribe de la siguiente forma:

$$
\begin{aligned}
    \dot {\mathbf{x}}(t) & =\mathbf A(t)\mathbf{x}(t)+\mathbf B(t)\mathbf{u}(t)\\ 
    \mathbf {y} (t)&=\mathbf C(t)\mathbf {x} (t)+\mathbf D(t)\mathbf {u}(t) 
\end{aligned}
$$

donde $\mathbf x(t)\in \mathbb{R}^{n}$ es el vector de estados,$\mathbf y(t)\in \mathbb{R}^{q}$ es el vector de salidas, $\mathbf u(t)\in \mathbb{R}^{p}$ es el vector de entradas, $\mathbf A(t)\in \mathbb{R}^{n\times n}$ es la matriz de estados, $\mathbf B(t)\in \mathbb{R}^{n\times p}$ es la matriz de entrada, $\mathbf C(t)\in \mathbb{R}^{q\times n}$ es la matriz de salida, $\mathbf D(t)\in \mathbb{R}^{q\times p}$ es la matriz de transmisión directa y $\dot{\mathbf{x}}(t):= \dfrac{d\mathbf{x}(t)}{dt}$

+++

Nótese que en esta formulación general se supone que todas las matrices son variantes en el tiempo, p. ej.: algunos o todos sus elementos pueden depender del tiempo. En los sistemas invariantes en el tiempo las matrices $\mathbf A$, $\mathbf B$, $\mathbf C$ y $\mathbf D$ son constantes, no son función de $t$.

La variable temporal $t$ puede ser una "continua" ($t\in \mathbb {R}$) o una discreta (p. ej.: $t\in \mathbb {Z}$): en este último caso la variable temporal es generalmente indicada como $k$. Dependiendo de las consideraciones tomadas, la representación del modelo de espacios de estado puede tomar las siguientes formas:

+++

## Tipos de sistemas en su representación en espacio de estados

**Continuo e invariante en el tiempo** 

$$
\begin{aligned}
\dot{\mathbf{x}}(t)= &\mathbf A\mathbf{x}(t)+\mathbf B\mathbf{u}(t)\\ 
\mathbf y(t)= & \mathbf C\mathbf x(t)+\mathbf D \mathbf u(t)
\end{aligned}
$$


**Continuo y variante en el tiempo**

$$
\begin{aligned}
    \dot {\mathbf{x}}(t) & =\mathbf A(t)\mathbf{x}(t)+\mathbf B(t)\mathbf{u}(t)\\ 
    \mathbf {y} (t)&=\mathbf C(t)\mathbf {x} (t)+\mathbf D(t)\mathbf {u}(t) 
\end{aligned}
$$


**Discreto e invariante en el tiempo**

$$
\begin{aligned}
\mathbf{x}(k+1) &=\mathbf A\mathbf {x} (k)+ \mathbf B \mathbf{u}(k)\\
\mathbf {y} (k) &= \mathbf C\mathbf {x} (k)+\mathbf D\mathbf {u} (k)
\end{aligned}
$$


**Discreto y variante en el tiempo**

$$
\begin{aligned}
\mathbf {x} (k+1)&=\mathbf {A} (k)\mathbf {x} (k)+\mathbf {B} (k)\mathbf {u} (k)\\ 
\mathbf {y} (k)&=\mathbf {C} (k)\mathbf {x} (k)+\mathbf {D} (k)\mathbf {u} (k)
\end{aligned}
$$


**Transformada de Laplace del sistema continuo e invariante en el tiempo**

$$
\begin{aligned}
s\mathbf {X} (s)&=\mathbf A\mathbf {X} (s)+\mathbf B\mathbf {U} (s)\\
\mathbf{Y}(s)&=\mathbf C\mathbf{X}(s)+\mathbf D\mathbf {U} (s)
\end{aligned}
$$


**Transformada Z del sistema discreto e invariante en el tiempo** 

$$
\begin{aligned}
z\mathbf {X} (z)&=\mathbf A\mathbf {X} (z)+\mathbf B\mathbf {U} (z)\\
\mathbf {Y} (z)&=\mathbf C\mathbf {X} (z)+\mathbf D\mathbf {U} (z)
\end{aligned}
$$

+++

Nótese que los sistemas invariantes se expresan solamente en el dominio del tiempo; sólo los invariantes se expresan también en el dominio de la frecuencia (con la transformada de Laplace o Z).

La estabilidad y la respuesta natural característica de un sistema puede ser estudiado mediante los autovalores (o valores propios) de la matriz $\mathbf{A}$. La estabilidad de un modelo de espacio de estados invariante en el tiempo puede ser fácilmente determinado observando la función transferencia del sistema en forma factorizada. Si el sistema es SISO, Tendría una forma parecida a la siguiente:

$$G(s) =k \dfrac{(s-z_1)(s-z_2)(s-z_3)}{(s-p_1)(s-p_2)(s-p_3)(s-p_4)}$$

El denominador de la función transferencia es igual al polinomio característico encontrado tomando el determinante de $s\mathbf{I}-\mathbf{A}$.

$$\mathbf{\Lambda }(s)=|s\mathbf{I}-\mathbf{A}|$$

+++

Las raíces de este polinomio (los autovalores) proporcionan los polos en la función transferencia del sistema. Dichos polos pueden ser utilizados para analizar si el sistema es asintótica o marginalmente estable. Otra alternativa para determinar la estabilidad, en la cual no involucra los cálculos de los autovalores, es analizar la estabilidad de Liapunov del sistema. Los ceros encontrados en el numerador de $\textbf{G}(s)$ puede usarse de manera similar para determinar si el sistema es de mínima fase.

El sistema podría ser estable con respecto a sus entradas y salidas y aun así puede resultar internamente inestable. Este podría ser el caso si polos inestables se cancelan con ceros en la misma posición.

+++

## Función transferencia de a partir de las ecuaciones de estado

La función de transferencia de un modelo de espacio de estados continuo e invariante en el tiempo puede ser obtenida de la siguiente manera:

Tomando la transformada de Laplace de

$$\dot{\mathbf{x}}(t)=\mathbf A\mathbf{x}(t)+\mathbf B\mathbf{u}(t)$$

tenemos que

$$s\mathbf {X} (s)=\mathbf A\mathbf {X} (s)+\mathbf B\mathbf {U} (s)$$

Luego, agrupamos y despejamos $\mathbf {X} (s)$, dando:

$$s(\mathbf{I}-\mathbf A)\mathbf {X} (s)=\mathbf B\mathbf {U} (s)$$
       
$$\mathbf {X} (s)=(s\mathbf {I} -\mathbf A)^{-1}\mathbf B\mathbf {U} (s)$$

esto es sustituido por $\mathbf {X} (s)$ en la ecuación de salida

$$\mathbf {Y} (s)=\mathbf C \mathbf {X} (s)+\mathbf D\mathbf {U} (s)$$ 
    
nos queda

$$ \mathbf {Y} (s)=\mathbf C\left((s\mathbf {I} -\mathbf A)^{-1}\mathbf B\mathbf {U} (s)\right)+\mathbf D\mathbf {U} (s)$$

+++

Como la función de transferencia está definida como la tasa de salida sobre la entrada de un sistema, tomamos

$$\mathbf {G} (s)=\dfrac{\mathbf {Y} (s)}{\mathbf {U} (s)}$$

y sustituimos las expresiones previas por $\mathbf {Y} (s)$ con respecto a $\mathbf{U}(s)$, quedando

$$\mathbf {G} (s)=\mathbf C(s\mathbf {I} -\mathbf A)^{-1}\mathbf B+\mathbf D$$

Claramente $\mathbf{G}(s)$ debe tener $q$ por $p$ dimensiones, así como un total de $q\times p$ elementos, o sea es una matriz de funciones transferencias. Entonces para cada entrada hay $p$ funciones de transferencias, una por cada salida. Esta es una  de las razones por la cual la representación de espacios de estados en general es la mejor elección para sistemas MIMO en lugar de la representación en transformadas de Laplace.

```{code-cell} ipython3

```
