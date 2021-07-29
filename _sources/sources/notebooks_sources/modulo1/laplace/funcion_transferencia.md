---
jupytext:
  formats: ipynb,md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.10.3
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Función transferencia

+++ {"slideshow": {"slide_type": "slide"}}

## Modelos de sistemas lineales e invariantes en el tiempo

La forma típica de representar un sistema lineal e invariante en el tiempo (LTI) SISO, es mediante el uso de la transformada de Laplace.

Sea un sistema con condiciones iniciales nulas, entonces el sistema se puede expresar matemáticamente como:

$$\frac{Y(s)}{U(s)}=H(s)$$

donde $Y(s)$ es la transformada de Laplace de la salida $y(t)$ para una entrada arbitraria $u(t)$, cuya transformada de Laplace es $U(s)$. 

Notar que con esta definición, lo que se transforma al dominio de Laplace son las **señales** de entrada y salida. No es un sistema.

+++ {"slideshow": {"slide_type": "subslide"}}

```{important}
- Se puede demostrar **que** $H(s)$ **es la respuesta del sistema a una señal de entrada impulso unitario** $\delta(t)$.
- Para sistemas Lineales Invariantes en el Tiempo (LTI), la relación $\dfrac{Y(s)}{U(s)}$ se mantiene constante para todo $U(s)$.
```

+++ {"slideshow": {"slide_type": "slide"}}

### Obtención de la función transferencia de un sistema:

La función transferencia de un sistema se puede obtener de varias formas:

1. Aplicando la definición, es decir, aplicando $u(t)$ para medir $y(t)$, transformar ambas señales al dominio de Laplace y luego obtener $H(s)=\dfrac{Y(s)}{U(s)}$. Esto no es práctico.
1. Aplicar un impulso $u(t)=\rho(t)$ y hacer las transformada de Laplace de la respuesta. Es un caso particular del método anterior haciendo que $U(s)=1$.
1. A partir de las ecuaciones temporales que dominan la dinámica del sistema se resuelve $\dfrac{Y(s)}{U(s)}$. Esta es la forma en que por lo general se obtienen las funciones transferencias de los sistemas en forma analítica.

+++ {"slideshow": {"slide_type": "slide"}}

### Ejemplo general:
En general un sistema dinámico se representa como una ecuación diferencial de orden $n$, Es decir:

$$ \begin{aligned}
 & a_n\frac{d^ny(t)}{dt^n}+a_{n-1}\frac{d^{n-1}y(t)}{dt^{n-1}}+\cdots+a_{1}\frac{dy(t)}{dt}+a_0y(t) = \dots \\
  &\dots  b_p\frac{d^pu(t)}{dt^p}+b_{p-1}\frac{d^{p-1}u(t)}{dt^{p-1}}+\cdots+b_{1}\frac{du(t)}{dt}+b_0u(t)
  \end{aligned}
 $$

Donde $y(t)$ es la salida del sistema y $u(t)$ es la entrada del sistema, haciendo la transformando por Laplace sabiendo que $\mathcal{L}\{u(t)\}=U(s)$ y que $\mathcal{L}\{y(t)\}=Y(s)$, tenemos:

$$
\begin{aligned}
 & a_ns^nY(s)+a_{n-1}s^{n-1}Y(s)+\cdots+a_1sY(s)+a_0Y(s)+ \mbox{term. cond. iniciales de y(t)} = \cdots \\
 \dots & b_ps^pU(s)+b_{p-1}s^{p-1}U(s)+\cdots+b_1sU(s)+b_0U(s)+ \mbox{term. cond. iniciales de u(t)} 
 \end{aligned}
$$
 

si asumimos que las condiciones iniciales para $y(t)$ y $u(t)$ son nulas, se reduce a:

$$
\begin{aligned}
 & a_ns^nY(s)+a_{n-1}s^{n-1}Y(s)+\cdots+a_1sY(s)+a_0Y(s) = \cdots \\ 
\cdots & b_ps^nU(s)+b_{p-1}s^{p-1}U(s)+\cdots+b_1sU(s)+b_0U(s)
\end{aligned}
$$

Despejando podemos obtener:

$$H(s)=\frac{Y(s)}{U(s)}=\frac{b_ps^p+b_{p-1}s^{p-1}+\cdots+b_1s+b_0}{a_ns^n+a_{n-1}s^{n-1}+\cdots+a_1s+a_0}$$

+++

```{important}

En general los sistemas dinámicos en el dominio temporal se describen matemáticamente a partir de ecuaciones diferenciales. 
Es importante notar, que en el dominio transformado de Laplace estas ecuaciones diferenciales se transforman en ecuaciones algebraicas, resultando en un tratamiento matemático más sencillo.
```

+++

## Ejemplo #1: Sistema electromecánico (motor eléctrico)

### Desarrollo

Supongamos CI nulas, es decir $i_a(t=0)=0$ y $(\theta_m(t=0)=0)$ entonces de las ecuaciones del motor obtenidas en ejemplos anteriores y aplicando la transformada de Laplace, tenemos:

$$ 
\left\{\begin{array}{c}
s~I_a(s) = \frac{1}{L_a} E_a(s) -\frac{ R_a}{L_a} I_a(s) -  \frac{K_b}{L_a} s~\Theta_m(s)  \\
s^2~\Theta_m(s) =   \frac{K_t}{J_m} I_a(s) - \frac{D_m}{J_m}s~\Theta_m(s) - \frac{1}{J_m} T_r(s)
\end{array}\right.
$$

+++

agrupando las variables y despejando de la segunda ecuación la corriente $I_a(s)$:

$$ 
\begin{aligned}
\left(s+ \frac{ R_a}{L_a}\right)I_a(s) &= \frac{1}{L_a} E_a(s) - \frac{K_b}{L_a} s~\Theta_m(s)  \\
 I_a(s) &= \frac{J_m}{K_t}\left(s^2+ \frac{D_m}{J_m}s\right)~\Theta_m(s) + \frac{1}{K_t} T_r(s)
\end{aligned}
$$

+++

Este sistema tiene 2 entradas, la entrada que podemos controlar $E_a(s)$ y la entrada de perturbación $T_r(s)$, por ser un sistema LTI podemos hallar las FTs de la siguiente forma:

$$ 
\left(s+ \frac{ R_a}{L_a}\right)\frac{J_m}{K_t}\left(s^2+ \frac{D_m}{J_m}s\right)\Theta_m(s) + \left(s+ \frac{ R_a}{L_a}\right)\frac{1}{K_t} T_r(s) = \frac{1}{L_a} E_a(s) - \frac{K_b}{L_a} s\Theta_m(s) 
$$

luego,

$$ 
\left(\left(s+ \frac{ R_a}{L_a}\right)~\frac{J_m}{K_t}\left(s^2+ \frac{D_m}{J_m}s\right)+\frac{K_b}{L_a} s\right)\Theta_m(s)  = \frac{1}{L_a} E_a(s) - \left(s+ \frac{ R_a}{L_a}\right)\frac{1}{K_t} T_r(s)
$$

+++

luego considerando $T_r=0$, hallamos la FT desde la entrada $E_a$ a la salida $\Theta_m$:

$$ 
\frac{\Theta_m(s)} {E_a(s)}  = \frac{\frac{1}{L_a}}{\left(s+ \frac{ R_a}{L_a}\right)\frac{J_m}{K_t}\left(s^2+ \frac{D_m}{J_m}s\right)+\frac{K_b}{L_a} s}  
$$

finalmente si consideramos que $E_a=0$, hallamos la FT desde la entrada de perturbación $T_r$ a la salida $\Theta_m$:

$$
\frac{\Theta_m(s)} {T_r(s)}  =- \frac{\left(s+ \frac{ R_a}{L_a}\right)\frac{1}{K_t} }{\left(s+ \frac{ R_a}{L_a}\right)\frac{J_m}{K_t}\left(s^2+ \frac{D_m}{J_m}s\right)+\frac{K_b}{L_a} s}
$$

+++ {"slideshow": {"slide_type": "slide"}}

## Ejemplo #2: masa resorte

### Resolución de las ecuaciones
#### Ejemplo sistema mecánico
<img style="display:block; margin-left: auto; margin-right: auto;" src="ejemplo_sis_mec.png" width="300" alt="Ejemplo sistema mecánico">

+++ {"slideshow": {"slide_type": "subslide"}}

Las ecuaciones que dominan este sistema son:

$$
\begin{eqnarray}
F(t)-bv(t)-kx(t)&= Ma(t)\\
F(t)-b\dot{x}(t)-kx(t)&=M\ddot{x}(t)\\
u(t)&=F(t)\\
y(t)&=x(t)
\end{eqnarray}
$$

+++ {"slideshow": {"slide_type": "subslide"}}

Podemos obtener la función transferencia de este sistema mediante reemplazando $x$ por $y$ y $F$ por $u$ y transformando por Laplace, usando la propiedad de la derivada de la transformada de Laplace. Esto es:

$$F(s)-bsX(s)-kX(s)=Ms^2X(s)$$
$$U(s)-bsY(s)-kY(s)=Ms^2Y(s)$$

Despejando se tiene:

$$H(s)=\frac{Y(s)}{U(s)}=\frac{1}{Ms^2+bs+k}$$

+++ {"slideshow": {"slide_type": "slide"}}

Vamos a definir el sistema según el ejemplo anterior con valores de
$M=1$, $b=0.1$ y $k = 10$.

```{code-cell} ipython3
---
slideshow:
  slide_type: fragment
---
import control as ctrl
import numpy as np
import matplotlib.pyplot as plt
# %matplotlib qt5 #descomentar para obtener ventanas emergentes de las figuras
```

Definamos primero las constantes del sistema

```{code-cell} ipython3
---
slideshow:
  slide_type: fragment
---
M=1
b=0.01
k=1
```

Ahora deinamos la función transferencia

```{code-cell} ipython3
---
slideshow:
  slide_type: fragment
---
G=ctrl.tf([1],[M,b,k])
```

Obtengamos los polos (ceros del denominador) de la función transfrencia $G$

```{code-cell} ipython3
G.pole()
```

+++ {"slideshow": {"slide_type": "slide"}}

Ahora vamos a ver la respuesta al escalón.

```{code-cell} ipython3
---
slideshow:
  slide_type: subslide
---
T1=np.linspace(0,1000,101)
t1,y1=ctrl.step_response(G, T=T1) # cantidad de puntos por defecto
```

```{code-cell} ipython3
---
slideshow:
  slide_type: subslide
tags: [hide-input]
---
fig, ax =  plt.subplots(1, 1, figsize=(10,4))
ax.plot(t1,y1,label="101 puntos de sim")
ax.grid()
ax.set_xlabel('Tiempo [s]')
ax.set_ylabel('$x$')
ax.legend();
```

Vemos que esto no se ve del todo razonable. Esas lineas cortadas no se lucen muy armónicas. Vamos a agregar más puntos a la simulación.

```{code-cell} ipython3
---
slideshow:
  slide_type: subslide
---
T2=np.linspace(0,1000,1001)
t2,y2=ctrl.step_response(G,T=T2)
```

```{code-cell} ipython3
---
slideshow:
  slide_type: subslide
tags: [hide-input]
---
fig, ax =  plt.subplots(1, 2, figsize=(12,4))
ax[0].plot(t1,y1,label="101 puntos de sim")
ax[0].plot(t2,y2,alpha=0.6, label="1001 puntos de sim")
ax[0].grid()
ax[0].set_xlabel('Tiempo [s]')
ax[0].set_ylabel('$x$')
ax[0].set_title('Evolución completa')
ax[1].plot(t1, y1)
ax[1].plot(t2, y2, alpha=0.6)
ax[1].set_xlim([0,30])
ax[1].grid()
ax[1].set_xlabel('Tiempo [s]')
ax[1].set_ylabel('$x$')
ax[1].set_title('Primeros segundos de la evolución')
fig.tight_layout()
```

En estas figuras podemos ver como la simulación que devuelve solo 100 puntos no nos muestra de forma correcta la evolución del sistema. Sin embargo, los puntos donde está calculado los valores de la respuesta al escalón si son correctos.

Con 1001 puntos ya podemos ver una evolución mucho más prolija. Sin embargo, para no ver las puntas triangulares que vemos en la figura, debemos agregar algunos puntos más a la simulación.

```{code-cell} ipython3
---
slideshow:
  slide_type: subslide
---
T3=np.linspace(0,1400,5001)
t3,y3=ctrl.step_response(G,T=T3)
```

```{code-cell} ipython3
---
slideshow:
  slide_type: subslide
---
fig, ax =  plt.subplots(1, 2, figsize=(12,4))
ax[0].plot(t3,y3,label="5001 puntos de sim")
ax[0].grid()
ax[0].set_xlabel('Tiempo [s]')
ax[0].set_ylabel('$x$')
ax[0].set_title('Evolución completa')
ax[1].plot(t3, y3)
ax[1].set_xlim([0,30])
ax[1].grid()
ax[1].set_xlabel('Tiempo [s]')
ax[1].set_ylabel('$x$')
ax[1].set_title('Primeros segundos de la evolución');
fig.tight_layout()
```

En los primeros segundos vemos una dinámica que produce rápidas oscilaciones, y se va que muy lentamente el sistema empieza a disminuir esas oscilaciones. En la figura de la izquierda vemos que esta oscilaciones tienden a 0, en un tiempo suficientemente grande. Estos sistemas que tienen dinámicas tan diferentes, por un lado la "lentas" como la que produce la extinción de las oscilaciones en un tiempo grande, y por otro lado las rápidas que se manifiestan en las oscilaciones se los conoce como `sistemas rígidos` o `sistemas stiff`.

```{admonition} Sistemas stiff
Son aquellos sistemas que contienen dinámicas muy rápidas y muy lentas actuando a la vez.
```
