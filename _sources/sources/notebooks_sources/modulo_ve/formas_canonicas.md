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

# Formas canónicas de un espacio de estados

Para un sistema dado, existen un número infinito de posibles modelos en espacios de estados que nos darán las mismas salidas para las mismas entradas. Por lo tanto es deseable tener ciertas formas estandarizadas de estructuras de espacios de estados. Llamaremos a estas, las **formas canónicas**. 

Dado un sistema en su forma de función transferencia, es posible obtener cada una de sus formas canónicas es espacios de estados. Considerar el siguiente sistema lineal definido por sus ecuaciones diferenciales:

+++

$$  
y^{(n)}(t) + a_{1} ~ y^{(n-1)}(t) + ~ \cdots ~ + a_{n-1} ~ \dot{y}(t) + a_n ~ y(t) = b_0 ~ u^{(n)}(t) + ~ \cdots ~ + b_{n-1} ~ \dot{u}(t) + b_n ~ u(t) 
$$

+++

donde  $u$ es la entrada, $y$ es la salida y $y^{(n)}$ representa la $n-th$ derivada de $y$ con respecto al tiempo. Haciendo la transformada de Laplace se tiene:

+++

$$  
Y(s)(s^n + a_{1}s^{n-1} + ~ \cdots ~ + a_{n-1}s + a_n ) = U(s)(b_0 s^{n} + ~ \cdots ~ + b_{n-1} ~ s + b_n) 
$$

+++

y se logar la siguiente función transferencia:

+++

$$  
\frac{Y(s)}{U(s)} = \frac{b_0 s^{n} + ~ \cdots ~ + b_{n-1} ~ s + b_n}{s^n + a_{1}s^{n-1} + ~ \cdots ~ + a_{n-1}s + a_n}
$$

+++

## Forma canónica de controlabilidad

Para obtener la forma **canónica controlable** debemos escribir las matrices del sistema en espacio de estado ${A_c, B_c, C_c, D_c}$, de la siguiente manera:

+++


$$
\begin{bmatrix} \dot{x_1}\\\dot{x_2} \\ \vdots \\ \dot{x_{n-1}}\\ \dot{x_n} \end{bmatrix} 
= \underbrace{\begin{bmatrix}
  -a_{1} & -a_{2} & \cdots & -a_{n-1} & -a_{n} \\
   1 & 0 &  \cdots & 0 & 0  \\
  \vdots & \ddots & \vdots & \vdots & \vdots\\
  0 & \cdots & 1  & 0 & 0\\
  0 & 0 & \cdots & 1 & 0
 \end{bmatrix}}_{=A_c} \begin{bmatrix} {x_1}\\ {x_2} \\ \vdots \\ {x_{n-1}} \\ {x_n} \end{bmatrix} 
 + \underbrace{\begin{bmatrix} 1 \\0\\ \vdots \\0\\0 \end{bmatrix}}_{=B_c}~ u
$$
 
$$
y= \underbrace{\begin{bmatrix} b_1-a_1 b_0 & b_2-a_2 b_0  & \cdots & b_{n-1}-a_{n-1} b_0  & b_n-a_n b_0 \end{bmatrix}}_{ =~C_c} 
   \begin{bmatrix} {x_1}\\ {x_2}\\ \vdots \\ {x_{n-1}}\\ {x_n} \end{bmatrix} 
 + \underbrace{\begin{bmatrix}b_0\end{bmatrix}}_{=~D_c} ~u
 $$

+++

### Ejemplo 1 

Considerar el sistema dado por

+++

$$  
\frac{Y(s)}{U(s)} = \frac{s + 3}{s^2 + 3s + 2}
$$

+++

Obtener la representación de estados en la forma **canónica de controlabilidad**.

Por inspección vemos que, $n = 2$ (el exponente más grande del denominador en $s$), por lo tanto  $a_1 = 3,~ a_2 = 2,~ b_0 = 0,~ b_1 = 1$ y $b_2 = 3$. Por lo tanto tenemos que:

+++

$$
\begin{bmatrix} \dot{x_1}\\\dot{x_2} \end{bmatrix} 
= 
~
\underbrace{\begin{bmatrix}
-2 & -3\\
1 & 0
\end{bmatrix}}_{=~A_c}
~ 
\begin{bmatrix} {x_1}\\{x_2} \end{bmatrix} 
+ 
\underbrace{\begin{bmatrix} 1 \\0\end{bmatrix} }_{=~B_c}
u 
\\
y = ~ \underbrace{\begin{bmatrix} 1 & 3\end{bmatrix}}_{=~C_c} 
\begin{bmatrix} {x_1}\\{x_2} \end{bmatrix} 
+
~ \underbrace{\begin{bmatrix} 0 \end{bmatrix} }_{=~D_c}
u
$$

+++

## Forma Canónica de Observabilidad

+++

Suponiendo que $A_c, B_c, C_c, D_c$ son las matrices de un sistema en su forma canónica de controlabilidad, el sistema en su forma **canónica de observable** tendrá la siguiente forma:

$$ A_o = A_c^T \quad B_o =B_c^T \quad C_o = B_C^T \quad D_o=D_c $$

+++

### Ejemplo 2:

Obtener la representación del sistema del ejemplo 1 en su forma canónica observable:

+++

$$
\begin{bmatrix} \dot{x_1}\\\dot{x_2} \end{bmatrix} 
= 
~
\underbrace{\begin{bmatrix}
-2 & 1\\
-3 & 0
\end{bmatrix}}_{=~A}
~ 
\begin{bmatrix} {x_1}\\{x_2} \end{bmatrix} 
+ 
\underbrace{\begin{bmatrix} 1 \\3\end{bmatrix} }_{=~B}
u 
\\
y = ~ \underbrace{\begin{bmatrix} 1 & 0\end{bmatrix}}_{=~C} 
\begin{bmatrix} {x_1}\\{x_2} \end{bmatrix} 
+
~ \underbrace{\begin{bmatrix} 0 \end{bmatrix} }_{=~D}
u
$$
