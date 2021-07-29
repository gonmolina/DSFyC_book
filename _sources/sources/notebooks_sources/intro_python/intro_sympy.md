---
jupytext:
  encoding: '# -*- coding: utf-8 -*-'
  formats: md:myst,ipynb
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

# Introducción a SymPy

`SymPy es una librería para Python de matemática simbólica. Para poder usarlo necesitamos importar [el paquete SymPy](http://docs.sympy.org/latest/index.html).

```{code-cell} ipython3
import sympy as sp
```

Para imprimir las fórmulas por pantalla y obtener una visualización tipográfica agradable, además es necesario ejecutar:

```{code-cell} ipython3
sp.init_printing()
```

Tenga en cuenta que esto puede cambiar según cual sea de la versión de sympy

+++

Luego, ya para comenzar con la operatorio simbólica, necesitamos crear un símbolo.

```{code-cell} ipython3
x = sp.Symbol('x')
x
```

+++ {"lang": "es"}

`SymPy` nos permite hacer muchas operaciones matemáticas que serían tediosas a
mano. Por ejemplo, podemos expandir un polinomio:

```{code-cell} ipython3
polynomial = (2*x + 3)**4
polynomial.expand()
```

+++ {"lang": "es"}

Observe lo que sucedió: definimos un nuevo nombre llamado `polynomial`
y luego usamos el método `.expand()` para expandir el polinomio. Podemos ver
todos los métodos asociados con un objeto escribiendo su nombre y un punto y
luego presionando "tabulador".

Acceda a la lista de métodos para la variable polynomial ingresando "." y
presionando tabulador al final de la línea en la celda a continuación.

+++ {"lang": "es"}

Para obtener ayuda sobre cualquier método, podemos escribir su nombre y
agregar un `?` al final, luego evaluar la celda.

Obtenga ayuda sobre el método `.expand()` mediante la evaluación de la celda
a continuación:

```{code-cell} ipython3
polynomial.expand?
```

+++ {"lang": "es"}

También es posible obtener ayuda para una función colocando el cursor entre
los paréntesis y presionando Mayúscula + Tabulador

+++ {"lang": "es"}

Por supuesto, también podemos factorizar polinomios:

```{code-cell} ipython3
(x**2 + 2*x + 1).factor()
```

## Cálculo

`SymPy` sabe integrar y diferenciar.

```{code-cell} ipython3
polynomial.diff(x)  # First derivative
```

```{code-cell} ipython3
polynomial.diff(x, 2)  # Second derivative
```

```{code-cell} ipython3
# indefinite integral - note no constant of integration is added
polynomial.integrate(x)
```

```{code-cell} ipython3
# Note that integrate takes one argument which is a tuple for the definite
# integral
polynomial.integrate((x, 1, 2))
```

+++ {"lang": "es"}

## Límites

Podemos evaluar los límites usando `SymPy`, incluso para límites "interesantes"
donde necesitaríamos la regla de L'Hopital

```{code-cell} ipython3
sp.limit((2*sp.sin(x) - sp.sin(2*x))/(x - sp.sin(x)), x, 0)
```

+++ {"lang": "es"}

## Aproximación

`SymPy` tiene soporte incorporado para la expansión de series de Taylor

```{code-cell} ipython3
nonlinear_expression = sp.sin(x)

# taylor expansion in terms of the x variable, around x=2, first order.
sp.series(nonlinear_expression, x, 2, 2)
```

+++ {"lang": "es"}

Para eliminar el término de perdida use `.removeO()`

```{code-cell} ipython3
temp = sp.series(nonlinear_expression, x, 2, 2)
temp.removeO()
```

+++ {"lang": "es"}

También notará que el comportamiento predeterminado de `SymPy` es retener
representaciones exactas de ciertos números:

```{code-cell} ipython3
number = sp.sqrt(2)*sp.pi
number
```

+++ {"lang": "es"}

Para convertir las representaciones exactas de arriba en representaciones
aproximadas de [punto flotante](https://en.wikipedia.org/wiki/Floating_point), use uno de estos métodos:

- `sympy.N` funciona con expresiones complicadas que también contienen variables.
- `float` devolverá un número de tipo `float` de Python normal y es útil cuando se interactúa
con programas que no son de `SymPy`.

```{code-cell} ipython3
sp.N(number*x)
```

```{code-cell} ipython3
float(number)
```

## Resolver ecuaciones

`SymPy` puede ayudarnos a resolver y manipular  ecuaciones utilizando la
función `solve`. Como muchas funciones de resolución, encuentra ceros de una
función, por lo que tenemos que reescribir las ecuaciones de igualdad para
que sean iguales a cero,

$$
\begin{aligned}
 2x^2 + 2 &= 4 \\
 2x^2 + 2 - 4 &= 0
\end{aligned}
$$

```{code-cell} ipython3
solutions = sp.solve(2*x**2 + 2 - 4)
solutions
```

```{code-cell} ipython3
solutions[0]
```

+++ {"lang": "es"}

También podemos usar `sp.Eq` para construir ecuaciones

```{code-cell} ipython3
equation = sp.Eq(2*x**2 + 2, 4)
equation
```

+++ {"lang": "es"}

La función roots nos dará también la multiplicidad de las raíces.

```{code-cell} ipython3
sol = sp.roots(equation)
sol
```

Esto no dice que la ecuación anterior tiene una solución -1 y otra solución
igual 1.

+++ {"lang": "es"}

También podemos resolver sistemas de ecuaciones pasando una lista de
ecuaciones para resolver y pidiendo una lista de variables para resolver.

```{code-cell} ipython3
x, y = sp.symbols('x, y')
sp.solve([x + y - 2,
          x - y - 0], [y, x])
```

+++ {"lang": "es"}

Esto incluso funciona con variables simbólicas en las ecuaciones.

```{code-cell} ipython3
a, b, c = sp.var('a, b, c')
solution = sp.solve([a*x + b*y - 2,
                     a*x - b*y - c], [x, y])
solution
```

```{code-cell} ipython3
solution.items()
```

```{code-cell} ipython3
a = [i for i in solution.values()]
```

```{code-cell} ipython3
a[0]
```

```{code-cell} ipython3
a[1]
```
