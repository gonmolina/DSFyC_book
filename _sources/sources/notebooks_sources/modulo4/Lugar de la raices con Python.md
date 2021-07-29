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

# Lugar de las raíces usando Python

```{code-cell} ipython3
:tags: [remove-cell]

import control as ctrl
import numpy as np
import matplotlib.pyplot as plt
```

```{code-cell} ipython3
G=ctrl.tf([1,1],[1,9, 0, 0])
_=ctrl.rlocus(G, grid=False, plot=True)
```

El símbolo `_` evita que envíe a consola lo que devuelve la función `rlocus`. Si queremos obtener lo que devuelve deberíamos escribir.

```{code-cell} ipython3
r=ctrl.rlocus(G, plot=False)
type(r)
```

Como podemos ver, r es una tupla con dos array. El primero contiene la posición de las raíces y la segunda la ganancia para las cuales se dan esas raíces.

Recordando lo que vimos en la introducción a Python podemos expandir la tupla y poner los dos datos (ganancias y raíces para cada ganancia) en dos variables separadas.

```{code-cell} ipython3
roots_list, k = r
```

o bien directamente podemos hacer lo mismo con

```{code-cell} ipython3
roots_list, k = ctrl.rlocus(G, plot=True, grid=False)
```

## Condición de Magnitud y Fase para un punto en particular del plano $s$

+++

En Python para evaluar la función transferencia en un punto del plano $s$, solo es necesario:

```{code-cell} ipython3
G2=ctrl.tf([1],[1, 4, 8, 0])
pto=-0.667+2j
G2(pto)
```

Para calcular $K$ debemos saber que en ese punto la modulo de $\left|KG(s)\right|$ debe ser 1. Entonces

$$
K=\frac{1}{\left|G(s_o)\right|}
$$

Sabiendo que la función `abs` de `numpy` aplicada sobre un número complejo o un vector devuelve el módulo,  podemos realizar la cuenta anterior en Python de la siguiente manera:

```{code-cell} ipython3
K=1/np.abs(G2(pto)) 
K
```

Para verificar que ese punto es efectivamente lugar geométrico de las raíces podemos hacer

```{code-cell} ipython3
np.angle(G2(pto))*180/np.pi
```

```{code-cell} ipython3
# o en grados
np.angle(G2(pto),deg=True)
```

Podemos ver que si bien el resultado no es 180, es un número muy cercando a este, por que el lugar de las raíces pasa por un punto muy cercano al evaluado cuando la ganancia es un número muy cercano a 11.85.

```{code-cell} ipython3
z1=G2(-.5+2.2j)
z2=G2(-.5-2.2j)
theta1=np.angle(z1,deg=True)
theta2=np.angle(z2,deg=True)
print(theta1,theta2)
```

Con el cálculo anterior pretendo mostrar que la función `angle()` devuelve el ángulo de un número complejo, el signo depende del cuadrante donde se encuentre el número complejo. Por lo que, hay que tener cuidado a la hora de usarlo e interpretarlo.

+++

### Trazado del lugar geométrico de las raíces para ganancias negativas

Internamente Python grafica el lugar de las raíces tomando una serie de ganancias y calculando los polos del sistema a lazo cerrado.

Para obtener el Lugar geométrico de las raíces para ganancias negativas lo único necesario será cambiar el signo a la función transferencia.

Para el ejemplo anterior, el lugar de las raíces sería:

```{code-cell} ipython3
_=ctrl.rlocus(-G2, grid=False)
```

