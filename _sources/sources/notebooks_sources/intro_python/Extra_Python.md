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

# Introducción a Python

Este cuaderno usará Python como lenguaje de programación. Esto significa que
la mayor parte de lo aprendido en este curso puede ser desarrollado y
aplicado en cuadernos. Los cuadernos se dividen en celdas donde se puede
poner código y luego correrlo haciendo **Mayúscula + Enter**.

Aquí intenta ase intentará mostrarles algunos de los tipo de datos más comunes de
Python con el fin de comenzar a interiorizarnos tanto con el lenguaje como con los
cuadernos de de Jupyter.

+++

## Números

Python cuenta con varios formatos o tipos de datos para representar números.

### Enteros

Cuando una nueva variable numérica se define Python por defecto lo hace como
entero. Por ejemplo:

```{code-cell} ipython3
num = 1
type(num)
```

### Punto flotante

En caso de necesitar números con decimales que no pueden ser representados
mediante enteros, Python automáticamente los transforma en números punto
flotantes, o `float`:

```{code-cell} ipython3
num = 1.1
type(num)
```

En caso de necesitar que un número entero sea representado como un flotante,
se puede resolver agregando un punto detrás de entero, por ejemplo:

```{code-cell} ipython3
num = 1.
type(num)
```

Otra forma de hacerlo, mucho menos común es:

```{code-cell} ipython3
num = float(1)
type(num)
```

### Números complejos

Python tiene soporte nativo para números complejos usando la letra `j` de la
siguiente manera:

```{code-cell} ipython3
num = 1+2j
print('La variable num toma el siguiente valor: ', num)
print('La variable num es del tipo: ', type(num))
```

Python soporta ciertas operaciones de forma nativa con los números complejos:

```{code-cell} ipython3
a = 1 + 2j
b = 2 + 5.8j
print(' La suma de a + b es: ', a+b)
```

```{code-cell} ipython3
print('El conjugado de a vale: ', a.conjugate())
```

Fácilmente podemos obtener la parte real e imaginaria

```{code-cell} ipython3
print('La parte real de a es ', a.real, ' y la parte imaginaria es ',
      a.imag)
```

Notar la diferencia entre el **método** `conjugate` y la **propiedad**
`real` y la **propiedad** `imag`. Mientras que `conjugate` se comporta como
una función propia de número complejo y debemos llamarla usando paréntesis,
`imag` y `real` se comportan como datos o variables del número complejo.

+++

## Listas

Ejemplo de lista, se acumulan con un patron

```{code-cell} ipython3
inputs = [2, 1, 3, 2, 4, 3, 6]
result = []  # Comenzamos con una lista vacía
for i in inputs:  # Iteramos sobre la lista inputs
    if i < 4:  # si se cumple esta condición agregamos a result
        result.append(i**2)

result
```

Es común ver en Python incluya una sintaxis algo diferente del `for`
y el `if` cuando se tiene que ejecutar una sola línea. El siguiente es un
ejemplo con ambos cosas.

```{code-cell} ipython3
result = [i**2 for i in inputs if i < 4]
result.append(1)
result
```

## Diccionarios

Una vez familiarizado con las listas, vemos los diccionarios, que son otro
tipo de contenedores ordenados.

```{code-cell} ipython3
lst = [1, 2, 3]
```

Podemos recuperar elementos de la lista mediante _indexado_

```{code-cell} ipython3
lst[1]
```

Un diccionario nos da un contenedor como una lista, pero los índices pueden
ser mucho más generales, no solo números, sino cadenas o variables de
cualquier tipo, como por ejemplo `sympy` (y una gran cantidad de otros tipos)

```{code-cell} ipython3
dic = {'a': 100, 2: 45, 100: 45}
print(dic['a'], dic[2])
```

```{code-cell} ipython3
:tags: [raises-exception]

print(dic[0])
```

## Tuplas

Finalmente, otro contenedor es la tupla:

```{code-cell} ipython3
x = 1, 2, 3
a, b, c = x
d, e, f = 4, 5, 6
```

```{code-cell} ipython3
def f(x):
    Ca, Cb, Cc = x
    return Cb


k = f(x)
print(k)
```

```{code-cell} ipython3
li = [1, 2, 3, 4]
type(li)
```

Las tuplas son como listas, pero se crean con comas:

```{code-cell} ipython3
t = 1, 2, 3, 4
type(t)
```

En algunos casos, es útil usar paréntesis para agrupar tuplas, pero tenga en
cuenta que no son requerimientos en la sintaxis:

```{code-cell} ipython3
t2 = (1, 2, 3, 4)
type(t2)
```

Es importante entender que la coma, no los paréntesis forman tuplas:

```{code-cell} ipython3
only_one = (((((((1)))))))
type(only_one)
```

```{code-cell} ipython3
only_one = 1,
type(only_one)
```

```{code-cell} ipython3
len(only_one)
```

La única excepción a esta regla es que una tupla vacía se construye con `()`:

```{code-cell} ipython3
empty = ()
type(empty)
```

```{code-cell} ipython3
len(empty)
```

Las diferencias entre las tuplas y las listas son que las tuplas son
inmutables (no se pueden cambiar "inplace")

```{code-cell} ipython3
li.append(1)
li
```

Pero si ejecutamos

```{code-cell} ipython3
:tags: [raises-exception]

t.append(1)
```

### Expansión de la tupla

Una característica muy útil y general del operador de asignación en Python es
que las tuplas se expandirán y asignarán en patrones coincidentes:

```{code-cell} ipython3
a, b = 1, 2
```

Esto es bastante sofisticado y puede manejar estructuras anidadas y
expandirse a listas:

```{code-cell} ipython3
[(a, b,), c, d] = [(1, 2), 1, 4]
[f, g, h] = [(1, 2), 1, 4]
```

```{code-cell} ipython3
f = 1, 2
print(f)
g = f, 3
print(g)
h = f[0], f[1], 3
print(h)
```

## El bucle for en Python

Como resumen en Python el bucle `for` lo que hace es recorrer los elementos de
objeto del tipo "iterable". Por ejemplo una lista, un diccionario o un array
de numpy son iterables.

Para una lista:

```{code-cell} ipython3
lista_variada = [1, 2, 3., "hola"]
for e in lista_variada:
    print(e)
```

Para un diccionario,  va a devolver en cada iteración la `key` es decir:

```{code-cell} ipython3
mi_diccionario = {"el_1": "Primer item", 2: "segundo item", "3": 45*3.2}
```

```{code-cell} ipython3
for key in mi_diccionario:
    print("La key es de tipo ", type(key), "y es ", key)
    print("El items de tipo ", type(mi_diccionario[key]),
          "y es ", mi_diccionario[key])
```

Finalmente para un `array` de `numpy` me va a devolver cada elemento del
array

```{code-cell} ipython3
import numpy as np

n = np.linspace(1, 1000, 7)
for i in n:
    print("Elemento al cuadrado", i**2)
```

## Funciones

Las funciones en Python se definen a partir de la palabra calve `def`.
Pueden tomar argumentos posicionales, y argumentos con nombres y con valores por defecto. El siguiente es un ejemplo.

```{code-cell} ipython3
def cambio_de_escala(x, m=1, h=0):
    y = [i*m+h for i in x]
    return y
```

El argumento x es un argumento posicional y es obligatorio a la hora
de llamar a la función `cambio_de_escala`. Los otros dos, pueden estar o no
presentes a la hora de llamar. Si no están presentes, usan el valor por defecto
definido en la función.

```{code-cell} ipython3
x = [1, 2, 3, 4, 5]
x1 = cambio_de_escala(x)
print(x1)
```

Podemos ver que asumió que `m=0` y `h=1`. También podríamos llamarla a la
función de la siguiente manera:

```{code-cell} ipython3
x2 = cambio_de_escala(x, m=2, h=1)
print(x2)
```

O lo que es lo mismo:

```{code-cell} ipython3
x2 = cambio_de_escala(x, h=1, m=2)
print(x2)
```

O equivalentemente:

```{code-cell} ipython3
x2 = cambio_de_escala(x, 1, 2)
print(x2)
```

Pero si intentamos sin darle valores a `x` tendremos:

```{code-cell} ipython3
:tags: [raises-exception]

x3 = cambio_de_escala(m=1, h=2)
print(x3)
```
