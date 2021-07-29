---
jupytext:
  encoding: '# -*- coding: utf-8 -*-'
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

# Introducción a los cuadernos de Jupyter

Estaremos utilizando Python a través de una interfaz conveniente
que son los [Jupyter Notebook](http://jupyter.org/).
Si a este cuaderno lo está visualizando a través de una pagina web, puede
descargarlo haciendo click en icono de descarga ubicado arriba a la derecha
y seleccionando el formato `ipynb`. También se puede ejecutar a desde la web,
usando *binder*, haciendo click donde se encuentra el cohete. Los archivos
con la extensión `.ipynb` son cuadernos de jupyter o jupyter notebooks.

+++

Existen varias alternativas para visualizar los cuadernos de Jupyter. Paso a
describir las más utilizadas:

- Jupyter Notebook: se encuentra instalada con el software recomendado para
la materia y consiste en una página web, que es servida por nuestra propia
computadora. Es relativamente liviana en cuanto a recursos computacionales y
con esta se pueden ver, ejecutar y editar los cuadernos

- Jupyter Lab: es similar a la anterior, también se encuentra pre instalada
con el software recomendado para la materia, pero cuenta con algunas
funcionalidades extra como explorador de variables, ayuda contextual,
explorador de archivos, y otras funcionalidades instalables a través de
plugins. Esta es la forma recomendada por la cátedra para acceder a los
cuadernos

- Spyder: este es un entorno de desarrollo integrado (IDE) pre-instalado con
el software recomendado por la cátedra. Para los cuadernos se usa un plugin
que se llama spyder-notebook. Este IDE trata de emular el entorno ofrecido
por Matlab. Es una buena elección para personas que están habituadas a
utilizar Matlab y quieren o deben comenzar a utilizar Python y los cuadernos
de jupyter.

- Visual Studio Code: es un software gratuito de microsoft, que con un
plugin (Python es el nombre del mismo) soporta todo lo referente a los cuadernos.
Si bien utiliza jupyter para mostrar los cuadernos, provee una interface propia.
Es recomendable para personas que están habituadas a programar o que quieran además
de utilizar los cuadernos, escribir programas en Python o cualquier otro lenguaje en
un mismo entorno. La principal desventaja es que suele ser algo complejo de configurar.
Sin embargo cuenta con una gran documentación en la web. Se puede descarga
[aquí](https://code.visualstudio.com/).

+++

## Un recorrido rápido

Tómese un segundo para recorrer la interfaz del cuaderno con el software que
haya elegido para visualizarlo y editarlo. Haga doble click sobre las celdas
y mire el código fuente de cada una. Trate de entender la forma es que se
escriben los títulos, que se definen las ecuaciones y como es diferencia una
celda donde se va escribir código y una donde se escribe texto o ecuaciones
matemáticas.

Ahora que está familiarizado con la nomenclatura, ¡ejecutemos algo de código!

*Evalúe la celda a continuación para imprimir por pantalla el mensaje
"Hello World" haciendo clic dentro de la celda y luego presionando
Shift + Enter*

```{code-cell} ipython3
for word in ['Hello', 'World']:
    print(word)
```

```{code-cell} ipython3
a = 1
a
```

+++ {"lang": "es"}

### Matemáticas en cuadros de texto

El editor de texto admite matemáticas en notación [$\LaTeX$](latex). Puede
hacer doble clic en un cuadro de texto para ver los códigos utilizados para
ingresarlo:

$$f(a)=\int^{a=\infty}_{a=0} \frac{1}{a+2} \mathrm{d}a$$

Haga doble clic en la fórmula anterior para ver el código que la produjo.

+++

Aprovechemos lo aprendido para introducirnos en el mundo de programación
científica con Python.

Los siguientes secciones de la introducción son a nivel informátivo. No es necesario
estudiarlo en esta materia, pero puede ser útil utilizarlo como material de consulta
para resolver problemas comunes de la materia así como también de otras disciplinas
científicas/ingenieriles.
