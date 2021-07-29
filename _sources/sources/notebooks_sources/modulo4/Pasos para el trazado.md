---
jupytext:
  formats: ipynb,md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.0
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Pasos para trazar el lugar geométrico de las raíces

## PASO 1. Dibujamos los polos y ceros de la función de transferencia a lazo abierto (con x y o) respectivamente

+++

## PASO 2. Encontramos la parte del eje real de los lugares geométricos de las raíces

Si tomamos un punto de prueba sobre el eje real, la contribución en ángulo de los ceros o polos complejos conjugados se anularán, puesto que uno de los ángulos se cancela con el de su conjugado.

Entonces debemos considerar solo los polos y ceros que se encuentran sobre el eje real. Los mismos aportarán 0° si el punto de prueba está a la derecha del polo o cero, y aportará con -180° o +180° si el punto de prueba está a la izquierda del polo o cero, respectivamente.

Conclusión: Es lugar geométrico de las raíces, aquel lugar del eje real que está a la izquierda de un número impar de polos y ceros.

+++

## PASO 3. Dibujamos las asíntotas para valores grandes de K

Cuando $K$ tiende a infinito, la ecuación $1 + KG(s) = 0$ se satisface solamente si  $G(s) = 0$.
Esto puede ocurrir de dos maneras:

1. En los ceros de $G(s)$, o sea las raíces del polinomio $b(s)$.
1. Escribamos la ecuación característica de la siguiente manera:

$$
\begin{align*}
1+K\frac{b(s)}{a(s)} & = & 0\\
1+K\frac{s^m+b_1 s^{m-1}+\ldots+b_m}{s^n+a_1 s^{n-1}+\ldots+a_n} & = & 0\\
\end{align*}
$$

+++

Si $n > m$, $G(s)$ tiende a cero cuando $s$ tiende a infinito.

Para $K$ grandes, $m$ ceros se cancelarían con $m$ polos, y los $(n-m)$ polos restantes se verían agrupados como en un solo polo múltiple en $\alpha$:

$$1+K\frac{1}{(s-\alpha)^{n-m}}=0,\qquad s\rightarrow\infty,K\rightarrow\infty$$

+++

Tomemos un punto de prueba $s_o=re^{j\phi}$, para un valor grande y fijo de $r$, y $\phi$ variable. Aplicando el criterio de la fase, tenemos:

$$(n-m)\phi_i=180^o+l360^o$$

El ángulo $\phi_l$ nos dará el ángulo que forman la asíntotas, que serán de:

$$\phi_l=\frac{180^o+l360^o}{n-m}, \qquad l=0,1,2,\ldots,n-m-1$$

El lugar de origen de las asíntotas está en $s_o=\alpha$. Averigüemos cuánto vale $\alpha$:

$$s^n+a_1 s^{n-1}+\ldots+a_n=(s-p_1)(s-p_2)\ldots(s-p_n)$$

Es fácil de deducir que $a_1=-\sum_{i=1}^np_i$, el opuesto de la sumatoria de los polos a lazo abierto.

Por otro lado, la ecuación característica la podemos escribir de la siguiente manera:

$$s^n+a_1 s^{n-1}+\ldots+a_n+K(s^m+b_1 s^{m-1}+\ldots+b_m)=0$$

+++

Si $m < (n-1)$, tenemos que $K$ no afectará el coeficiente de $s^{n-1}$, que será $a_1$; y por lo tanto, si llamamos $ri$ las raíces de esta ecuación, tenemos que:

$$-a_1=\sum_{i=1}^n r_i = \sum_{i=1}^np_i$$

Ahora, para $K$ grandes, m de las raíces coinciden con los ceros $zi$, y $(n-m)$ son del sistema asintótico $\dfrac{1}{(s-\alpha)^{n-m}}$, cuyos polos sumarán $(n-m)\alpha$. Por lo tanto:

$$\sum_{i=1}^n r_i=+(n-m)\alpha+\sum_{i=1}^m z_i= \sum_{i=1}^n p_i$$

Entonces, el centro de las asíntotas estará dado por:

$$\alpha = \dfrac{\sum_{i=1}^n p_i -\sum_{i=1}^m z_i}{n-m}$$

+++

## PASO 4 (solo para mejor resultado en esos puntos). Calculamos los ángulos de salida y de llegada de polos y ceros

Esto lo analizamos considerando un punto de prueba muy cercano al polo o al cero, y aplicando el criterio de fase.

+++

## PASO 5 (es importante este valor). Calculamos los puntos donde los lugares geométrico de las raíces cruzan el eje imaginario

Esto podemos obtener determinando el $K$, para el cual el sistema se torna inestable (con el criterio de Routh), y luego determinando las raíces de la ecuación característica para ese $K$.

Otra manera de determinarlo es reemplazar en la ecuación característica $s$ por $j\omega$, de esta manera tenemos dos ecuaciones (una para la parte real de la ecuación y otra para la imaginaria), con dos incógnitas: $K$ y $\omega$.

+++

## PASO 6. Estimamos la localización de las raíces múltiples, especialmente en el eje real, y determinamos los ángulos de llegada y salida de estos lugares

Supongamos que para un determinado $K$, existen raíces múltiples. Llamemos en ese caso $r_i$, a las raíces de la ecuación característica para ese $K$, o sea:

$$1+KG(s)=0=(s-r_1)(s-r_2)\ldots (s-r_n)=F(s)$$

Llamemos $r_{\alpha}$ a la raíz múltiple, entonces habrá un término de la productoria que será: $(s- r_{\alpha})^a$, con $a \ge 2$. O sea:

$$F(s) = (s-r_{\alpha})^a \prod^{n-1}_{i=1}(s-r_i)$$

+++

Derivemos esta productoria, y evaluémos a la misma en $s = r_\alpha$:

$$
\begin{align*}
\left.\frac{dF(s)}{ds}\right|_{s=r_\alpha}  = &\alpha(s-r_\alpha)^{\alpha-1}\prod_{i=1}^{n-a}(s-r_i)^a + (s-r_\alpha)^a \prod_{i=2}^{n-a}(s-r_i)+\cdots+(s-r_\alpha)^a\prod_{i=1}^{n-a-1}(s-r_i)\\
\left.\frac{dF(s)}{ds}\right|_{s=r_\alpha}   = & 0 + 0 +\cdots+0
\end{align*}
$$

+++

Aplicando este resultado a la ecuación característica, y sabiendo que se debe cumplir en $s = r_\alpha$:

$$a(s)+Kb(s)=0$$

y

$$\left.\frac{da(s)}{ds}\right|_{s=r_\alpha}+K\left.\frac{db(s)}{ds}\right|_{s=r_\alpha}=0$$

Que es equivalente a

$$\left.\frac{da(s)}{ds}\right|_{s=r_\alpha}-\frac{a(s)}{b(s)}\left.\frac{db(s)}{ds}\right|_{s=r_\alpha}=0$$

Si dividimos esta ecuación por $-b(s)$, obtendremos que:

$$\frac{d}{ds}\left(\left.-\frac{a(s)}{b(s)}\right)\right|_{s=r_\alpha}=0$$

Entonces tenemos que la condición de raíces múltiples es la siguiente:

$$\frac{d}{ds}\left(\left.\frac{-1}{G(s)}\right)\right|_{s=r_\alpha}=0$$

+++

## PASO 7. Completamos el dibujo, combinando los resultados anteriores

+++

## Trazado del lugar geométrico de las raíces para ganancias negativas

Para ganancias negativas, $K < 0$, el criterio de la fase (o del ángulo) cambia por:

$$\angle G(s) = l360^o, \qquad \text{con}\quad l=0,1,2,\ldots$$

puesto que la ecuación característica es $1-(-K)G(s)=0$.

Dejamos como ejercicio al lector analizar como afecta este cambio a los siete pasos descriptos anteriormente para el tazado geométrico de las raíces.

+++

## Ejemplo de trazado de lugar de las raíces

Tenemos la siguiente función de transferencia a lazo abierto:

$$ G(s) = \frac{s+1}{s^2(s+9)}$$

En la figura que sigue mostramos los resultados de los sucesivos pasos seguidos para el trazado del lugar geométrico de las raíces para esta función de transferencia.

Para el paso 3, el cálculo del centro y ángulo de las asíntotas son:

$$\phi_l=\frac{180^o+l360^o}{3-1}=90^o,270^o$$

$$\sigma_a=\frac{-9-0-0+1}{3-1}=-4$$

+++

Para el paso 4, el cálculo de los ángulos $\alpha$ de salida de los polos en el origen (suponiendo un apartamiento pequeño de los mismos), aplicando el criterio del ángulo:

$$2\alpha+0^o-0^o=180^0+n360^o \Rightarrow \alpha=-90^o, 90^o$$

De la misma manera podemos determinar el ángulo de salida del otro polo, y el de llegada al cero.

+++

Para el paso 6, determinamos la raíz múltiple realizando la derivada, la cual vale $-\sigma_m=-3$. Podemos comprobar que la multiplicidad de esta raíz es 3, ya que la derivada segunda de $\frac{-1}{G(s)}$ con respecto a $s$ evaluada en ese punto también es cero. Por lo tanto los ángulos $\beta$ de entrada a ese polo múltiple es:

$$2+\alpha+0^o-0^o=180^o+n360^o \Rightarrow \alpha = -90^o,90^o$$

De la misma manera podemos determinar el ángulo de salida del otro polo, y el de llegada al cero.

+++

Para el paso 6, determinamos la raíz múltiple realizando la derivada, la cual vale $\sigma_m=-3$. Podemos comprobar que la multiplicidad de esta raíz es 3, ya que la derivada segunda de $\frac{-1}{G(s)}$ con respecto a $s$ evaluada en ese punto también es cero. Por lo tanto los ángulos $\beta$ de entrada a ese polo múltiple es:

$$3\beta-180^o=n360^o \Rightarrow \beta=-60^o,60^o,180^o$$

Y los ángulos $\gamma$ de salida son:

$$3\gamma-180^o=180^o+n360^o \Rightarrow \gamma=-120^o,0^o,120^o$$

:::{figure-md} pasos-lr
<img src="pasos_trazado_fig1.png" width=800px>

Pasos del lugar de las raíces
:::

:::{figure-md} resultados-lr
<img src="pasos_trazado_fig2.png" width=500px>

Resultado final
:::

En Python, lugar de las raíces se puede resolver fácilmente usando el módulo de control.
