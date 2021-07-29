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

+++ {"colab_type": "text", "id": "V-kf_C3Hhklj"}

# Transformación lineal del espacio de estado

Consideremos un sistema descripto por las ecuaciones de estado

$$
\begin{array}{rcl}
\dot{\mathbf {x}} &=&  \mathbf A \mathbf x + \mathbf B \mathbf u\\
 y &=& \mathbf C \mathbf x + Du
 \end{array}
$$

Una transformación lineal mediante la matriz $\mathbf T$, que hace:

$$\mathbf x= \mathbf T \mathbf z$$

Sustituyendo la ecuación de la transformación en la de la ecuación de estados

$$
\begin{array}{rl}
\dot{\mathbf {x}} = \mathbf {T\dot z} &= \mathbf {AT} \mathbf z + \mathbf B \mathbf u\\
\dot{\mathbf {z}} & = \mathbf {T}^{-1}\mathbf{AT} \mathbf z + \mathbf {T}^{-1}\mathbf B \mathbf u
 \end{array}
$$

Sustituyendo el la ecuación de salida se tiene:

 $$y = \mathbf C \mathbf{Tz} + Du$$
 
Comparando las ecuaciones del sistema transformado, podemos ver que la matriz $\mathbf T$ transforma al sistema en:

$$
\begin{array}{lll}
\mathbf{\bar A} & = & \mathbf {T^{-1}} \mathbf A \mathbf T\\
\mathbf{\bar B} & = & \mathbf{T^{-1} B}\\
\mathbf{\bar C} & = & \mathbf{CT}\\
\bar D &=& D
\end{array}
$$


+++ 

# Transformación a la forma canónica de controlabilidad

La matrices de un sistema de orden 3 lineal e invariante en el tiempo representado mediante su forma canónica de controlabilidad tiene la forma:

$$
A_c = \begin{bmatrix}
-a_1&-a_2&-a_3\\
1&0&0\\
0&1&0\\
\end{bmatrix},  \qquad B_c=\begin{bmatrix}
1\\
0\\
0\\
\end{bmatrix}$$

$$
C_c=\begin{bmatrix}
c_1&c_2&c_3
\end{bmatrix}, \qquad D_c=\begin{bmatrix}0\end{bmatrix}$$

+++ 

Como vimos anteriormente, una transformación lineal mediante la matriz $\mathbf T$, que hace:

$$\mathbf x=\mathbf{Tz}$$

Por lo tanto podemos, llamando $\mathbf{\bar A} = \mathbf A_c$, tenemos que:

$$\mathbf{A_c} \mathbf {T^{-1}}=\mathbf{T^{-1}} \mathbf {A}$$

Escribimos $\mathbf {T^{-1}}$ como vectores fila, es decir:

$$\mathbf{T^{-1}} = \begin{bmatrix}
\mathbf{t_1}\\
\mathbf{t_2}\\
\mathbf{t_3}\\
\end{bmatrix}$$

$$
\begin{bmatrix}
-a_1&-a_2&-a_3\\
1&0&0\\
0&1&0\\
\end{bmatrix} 
\begin{bmatrix}
\mathbf{t_1}\\
\mathbf{t_2}\\
\mathbf{t_3}\\
\end{bmatrix} = \begin{bmatrix}
\mathbf{t_1}\mathbf A\\
 \mathbf{t_2}\mathbf A\\
 \mathbf{t_3}\mathbf A\\
\end{bmatrix}
$$

+++ 

Trabajando con las columnas 2 y 3 se tiene que:

$$\left.\begin{array}{lll}
\mathbf{t_1} &=&\mathbf{t_2 A}\\
\mathbf{t_2} &=& \mathbf{t_3 A}\end{array}\right\}\implies \mathbf{t_1} =\mathbf{t_3A}^2
$$

+++ 

Por otro lado, asumiendo que $\mathbf B_c$ está en la forma canónica de control, tenemos la relación

$$ \mathbf{T^{-1}}\mathbf B=\mathbf{B_c}$$ 

o:

$$  \begin{bmatrix}
\mathbf{t_1}\mathbf B\\
\mathbf{t_2}\mathbf B\\
\mathbf{t_3}\mathbf B\\
\end{bmatrix} = \begin{bmatrix}
1\\
0\\
0\\
\end{bmatrix}$$  

Combinando las ecuaciones anteriores se tiene que:

$$\begin{array}{lcccl}
\mathbf{t_3}\mathbf B &=& 0\\
\mathbf{t_2}\mathbf B &=& \mathbf{t_3}\mathbf {AB} &=& 0\\
\mathbf{t_1}\mathbf B &=& \mathbf{t_3}\mathbf {A^2B} &=& 1\\
\end{array}$$

Estas ecuaciones pueden ser escritas de la siguiente manera:

$$\mathbf{t_3}\left[\mathbf B\quad \mathbf{AB}\quad \mathbf{A^2B}\right] = \left[0\quad 0 \quad 1 \right]$$

$$\mathbf{t_3}=[0\quad 0 \quad 1]\mathcal{C}^{-1}$$

donde $\mathcal C$ es la matriz de controlabilidad, con $\mathcal C = \left[\mathbf B\quad \mathbf{AB}\quad 
\mathbf{A^2B}\right]$. 

Obteniendo $\mathbf{t_3}$ podemos obtener el resto de las filas de $\mathbf{T^{-1}}$.

Por lo tanto, en general para un sistema de orden $n$, la matriz de controlabilidad es:

$$\mathcal C =\left[ \mathbf{B}\quad\mathbf{AB}\quad\mathbf{A^2B}\quad\dots\quad\mathbf{A^{n-1}B}\right]$$

Por lo tanto, la última columna de la matriz $\mathbf{T^{-1}}$ resulta:

$$\mathbf{t_n}=\left[0\quad 0\quad\dots\quad 1\right]\mathcal C^{-1}$$

La matriz $T^{-1}$  se construye como:

$$
\mathbf{T^{-1}}=\begin{bmatrix}
\mathbf{t_n}\mathbf{A^{n-1}}\\
\mathbf{t_n}\mathbf{A^{n-2}}\\
\vdots\\
\mathbf{t_n}\\
\end{bmatrix}
$$

+++ 

## Definición:
**Un sistema es controlable si existe su forma canónica de controlabilidad.**

Podemos ver de las ecuaciones anteriores que solamente podremos obtener $\mathbf{T^{-1}}$ y por ende $\mathbf{T}$, si existe $\mathcal{C}^{-1}$. Para esto tenemos que asegurar que el rango de $\mathcal C$ sea igual al orden del sistema $n$.

Es importante notar que al definir la controlabilidad no depende de la matriz $\mathbf{C}$, sino que depende solo de la matriz $\mathbf{A}$ y $\mathbf{B}$.

La matriz $\mathbf{B}$ es la relacionada con las entradas del sistema. Nada tiene que ver con las salidas. En caso del que el sistema no sea controlable será necesario rediseñar la forma en que actuamos sobre del sistema. Nada resolveremos respecto a la controlabilidad cambiando, mejorando o agregando mediciones.



# Transformación a la forma canónica de observabilidad


La matrices de un sistema de orden 3 lineal e invariante en el tiempo representado mediante su forma canónica de observabilidad tiene la forma:

$$
\mathbf{A_o} = \begin{bmatrix}
-a_1&1&0\\
-a_2&0&1\\
-a_3&0&0\\
\end{bmatrix},  \qquad \mathbf{B_o}=\begin{bmatrix}
b_1\\
b_2\\
b_3\\
\end{bmatrix}$$

$$
\mathbf{C_o}=\begin{bmatrix}
1&0&0
\end{bmatrix}, \qquad D_o=\begin{bmatrix}0\end{bmatrix}$$

+++ 

Como vimos anteriormente, una transformación lineal mediante la matriz $\mathbf{T}$, que hace:

$$\mathbf{x}=\mathbf{Tz}$$

+++

Esto transforma a mi sistema en:

$$
\begin{array}{rcl}
\bar{\mathbf{A}} &=& \mathbf{T^{-1} A T}\\
\bar{\mathbf{B}} &=& \mathbf{T^{-1} B}\\
\bar{\mathbf{C}} &=&\mathbf{CT}\\
\bar D &=& D
\end{array}
$$

+++ 

Como queremos obtener el forma canónica de observabilidad llamamos $\mathbf{\bar A} = \mathbf{A_o}$, $\mathbf{\bar B} =  \mathbf{B_o}$, $\mathbf{\bar C} = \mathbf{C_o}$ y a $\bar D = D_o$. 

De las ecuaciones de la transformación tenemos:

$$\mathbf{TA_o}=\mathbf{AT}$$

Escribiendo $\mathbf{T}$ como:

$$
\mathbf{T}=\begin{bmatrix}
\mathbf{t_1}&\mathbf{t_2}&\mathbf{t_3}
\end{bmatrix}$$

donde $\mathbf{t_1}, \mathbf{t_2}, \mathbf{t_3}$ son vectores columnas que se corresponden con las columnas de $\mathbf{T}$.

+++ 

De esta manera tenemos que:

$$
\begin{bmatrix}
\mathbf{t_1}&\mathbf{t_2}&\mathbf{t_3}
\end{bmatrix}\begin{bmatrix}
-a_1&1&0\\
-a_2&0&1\\
-a_3&0&0\\
\end{bmatrix}=\begin{bmatrix}
\mathbf{At_1}&\mathbf{At_2}&\mathbf{At_3}
\end{bmatrix}$$

+++ 

De esta manera podemos obtener las siguientes igualdades:

$$
\left.\begin{array}{lll}
\mathbf{t_1} &=& \mathbf{At_2}\\
\mathbf{t_2} &=& \mathbf{At_3}\end{array}\right\}\implies \mathbf{t_1} =\mathbf{A^2t_3}$$

+++ 

Usando la ecuación de la transformación de la matriz $\mathbf{C_o=CT}$, se tiene que:

$$\begin{bmatrix}
1&0&0
\end{bmatrix}=\begin{bmatrix}
\mathbf{Ct_1}&\mathbf{Ct_2}&\mathbf{Ct_3}
\end{bmatrix}=\begin{bmatrix}
\mathbf{CA}^2\mathbf{t_3}&\mathbf{CAt_3}&\mathbf{Ct_3}
\end{bmatrix}$$

+++ 

Esto lo podemos reordenar como un sistema de ecuaciones en forma matricial de la siguiente manera:

$$\begin{bmatrix}
\mathbf{C}\\
\mathbf{CA}\\
\mathbf{CA}^2
\end{bmatrix}\mathbf{t_3} = \begin{bmatrix}
0\\
0\\
1
\end{bmatrix}$$

+++ 

Llamando a la matriz de observabilidad 

$$\mathcal{O}=\begin{bmatrix}
\mathbf{C}\\
\mathbf{CA}\\
\mathbf{CA}^2
\end{bmatrix}$$

podemos obtener $\mathbf{t_3}$ que es la última columna de la matriz de transformación $\mathbf{T}$, y con esta la otras columnas de la matriz de transformación.

+++ 

En general para un sistema de orden $n$, la matriz $\mathcal{O}$ resulta

$$\mathcal{O}=\begin{bmatrix}
\mathbf{C}\\
\mathbf{CA}\\
\vdots\\
\mathbf{CA}^{n-1}
\end{bmatrix}$$

y la última columna de la matriz de transformación resulta:

$$\mathbf{t_n} = \mathcal{O}^{-1}\begin{bmatrix}
0\\
0\\
\vdots\\
1
\end{bmatrix}
$$

```{admonition} Definición

Un sistema es observable si existe su forma canónica de observabilidad.
```

Podemos ver de las ecuaciones anteriores que solamente podremos obtener $\mathbf{T}$ si existe $\mathcal{O}^{-1}$. Para esto tenemos que asegurar que el rango de $\mathcal O$ sea igual al orden del sistema $n$.

Es importante notar que definir la observabilidad no depende de la matriz $\mathbf{B}$, sino que depende solo de la matriz $\mathbf{A}$ y $\mathbf{C}$.

La matriz $\mathbf{C}$ es la relacionada con las salidas del sistema. Nada tiene que ver con las entradas. En caso del que el sistema no sea observable será necesario rediseñar la forma en que tomamos las mediciones del sistema. Nada resolveremos respecto a la observabilidad modificando los actuadores.

+++

## Ejemplo masa resorte: obtener la forma canónica de observabilidad y con matriz de transformación T

```{code-cell} ipython3
:tags: [remove-cell]

import control as ctrl
import numpy as np
```

+++ 

Definimos los parámetros del sistema

```{code-cell} ipython3

b=20
k=10
m=1
```

+++ 

Definimos las matrices del sistema a partir de las ecuaciones diferenciales:

```{code-cell} ipython3

A=np.array([[0,1],
   [-k/m, -b/m]])

B=np.array([[0,1/m ]]).T
C=np.array([[1,0]])
D=np.array([0])
sys = ctrl.ss(A,B,C,D)
sys
```

+++ 

Para obtener la forma canónica de observabilidad debemos obtener la matriz $\mathcal{O}$. Como el sistema es de orden 2 es:

$$\mathcal{O}=\begin{bmatrix}
\mathbf C\\
\mathbf{CA}
\end{bmatrix}$$

+++

Apilamos verticalmente $\mathbf C$ y $\mathbf{CA}$

```{code-cell} ipython3

O=np.vstack((sys.C, sys.C@sys.A))
O
```

+++ 

Ahora calculamos la última columna de $\mathbf T$.

```{code-cell} ipython3

t2=np.linalg.inv(O)*np.matrix("0;1")
t2
```

+++ 

Calculamos la columna $\mathbf {t_1}$

```{code-cell} ipython3
t1=A@t2
```

```{code-cell} ipython3
t1
```

+++ 

Apilamos horizontalmente $\mathbf{t_1}$ y $\mathbf{t_2}$.

```{code-cell} ipython3
T=np.hstack((t1,t2))
T
```

```{code-cell} ipython3
invT=np.linalg.inv(T)
invT
```

+++

Finalmente calculamos las matrices del sistema transformado:

```{code-cell} ipython3

Ao=invT@sys.A@T
Bo=invT@B
Co=sys.C@T
Do=D
```

+++ 

Podemos verificar que las funciones transferencias del sistema antes de transformar y después de transformarlo son las mismas:

```{code-cell} ipython3
sys_o=ctrl.ss(Ao,Bo,Co,Do)
sys_o
```

```{code-cell} ipython3
print("sistema sin transformar:",ctrl.tf(sys),"sistema en su forma canónica de observabilidad:", ctrl.tf(sys_o))
```

+++

## Obtención de la forma canónica de controlabilidad


Para obtener la forma canónica de controlabilidad debemos obtener la matriz $\mathcal{C}$. Como el sistema es de orden 2 es:

$$\mathcal{C}=\begin{bmatrix}
\mathbf B & \mathbf{AB}
\end{bmatrix}$$

Apilamos horizontalmente $\mathbf B$ y $\mathbf{AB}$

```{code-cell} ipython3
Con=np.hstack((sys.B,sys.A@sys.B))
Con
```

+++ 

Ahora calculamos la última fila de $\mathbf{T}^{-1}$, $\mathbf{t2}$

```{code-cell} ipython3
t2=np.matrix([[0,1]])*np.linalg.inv(Con)
t2
```

+++

Calculamos la fila $\mathbf {t_1}$

```{code-cell} ipython3
t1=t2@A
```

```{code-cell} ipython3
t1
```

+++ 
Apilamos verticalmente $\mathbf{t_1}$ y $\mathbf{t_2}$.

```{code-cell} ipython3
invT=np.vstack((t1,t2))
T=np.linalg.inv(invT)
```

+++

Finalmente calculamos las matrices del sistema transformado:

```{code-cell} ipython3
Ac=invT*sys.A*T
Bc=invT@B
Cc=sys.C@T
Dc=D
```

+++

Podemos verificar que las funciones transferencias del sistema antes de transformar y después de transformarlo son las mismas:

```{code-cell} ipython3
sys_c=ctrl.ss(Ac,Bc,Cc,Dc)
```

```{code-cell} ipython3
print("sistema sin transformar:",ctrl.tf(sys),"sistema en su forma canónica de controlabilidad:", ctrl.tf(sys_c))
```

+++

## Transformación a la forma canónica modal:

Veremos solo el caso de autovalores reales simples.

### Definición.

Sea la matriz $\mathbf{A} \in \Bbb R^{n\times n}$. Diremos que  $\lambda \in \Bbb K (= \Bbb R\text{ o } \Bbb C)$ es un autovalor de $\mathbf{A}$ si existe un vector $\mathbf{v} \in \Bbb K^n$, con $\mathbf v \neq 0$ tal que:

$$\mathbf{Av}=\lambda \mathbf{v}$$

en cuyo caso se dice que $\mathbf{v}$ es un autovector asociado al autovalor $\lambda$.

+++

Suponiendo que $\mathbf A$ tienen $n$ autovalores diferentes, entonces podemos escribir en forma matricial $n$ ecuaciones para los $n$ pares autovalores-autovector

+++

$$
\begin{bmatrix}
\mathbf{v_1}&\mathbf{v_2}&\mathbf{v_3}
\end{bmatrix}\begin{bmatrix}
\lambda_1&0&0\\
0&\lambda_2&0\\
0&0&\lambda_3\\
\end{bmatrix}=\mathbf A\begin{bmatrix}
\mathbf{v_1}&\mathbf{v_2}&\mathbf{v_3}
\end{bmatrix}$$

+++

Si hacemos que:

$$\mathbf{T}=\begin{bmatrix}
\mathbf{v_1}&\mathbf{v_2}&\mathbf{v_3}
\end{bmatrix}$$

tenemos que:

$$\begin{bmatrix}
\lambda_1&0&0\\
0&\lambda_2&0\\
0&0&\lambda_3\\
\end{bmatrix}=\mathbf{T}^{-1}\mathbf{A T}$$


donde:

$$\mathbf{A_m}=\begin{bmatrix}
\lambda_1&0&0\\
0&\lambda_2&0\\
0&0&\lambda_3\\
\end{bmatrix}$$

Nuevamente $\mathbf{B_m}$ se obtiene como $\mathbf{B_m}=\mathbf{T}^{-1}\mathbf{B}$ y $\mathbf{C_m}=\mathbf{CT}$ 

La forma canónica modal tiene la particularidad de independizar los estados unos de otros. Es decir, cada estado se conecta solo con la entrada y con la salida y no se conectan entre si (cada estado no depende de otro estado).

Esta forma canónica nos da la posibilidad de estudiar controlabilidad y observabilidad sin pasar por las matrices $\mathcal C$ y $\mathcal O$. 

Si todos los estados están conectados con la entrada entonces el sistema es controlable. Para que se de esto, la matriz $\mathbf{B_m}$  tiene que tener todas sus componentes distintas de 0.

Si todos los estados están conectados con la salida entonces el sistema es observable. Para que se de esto, la matriz $\mathbf{C_m}$ tiene que tener todas sus componentes distintas de 0.

+++ 

## Ejemplo de transformación a la forma canónica modal

La matriz de transformación son los autovectores. 

El módulo linalg de numpy tiene una función que se llama eig, que los devuelve los autovectores y autovalores. Los autovalores lo devuelve en forma de ndarray y luego devuelvo los autovalores como un array. Vamos a utilizar esta función para calcular la matriz de transformación $T$. Los autovalores los vamos a usar para verificar que luego de aplicar la transformación la matriz $A_m$ resulte ser diagonal con los autovalores en la diagonal.

```{code-cell} ipython3
lamb,v=np.linalg.eig(A)
print("autovectores:\n ", v)
print("autovalores:\n ", lamb )
```

+++

La matriz de transformación T es la de los autovectores. Entonces:

```{code-cell} ipython3
T=v
invT=np.linalg.inv(T)
```

+++

Calculamos la transformación:

```{code-cell} ipython3
Am=invT@A@T
Bm=invT@B
Cm=C@T
Dm=D
```

```{code-cell} ipython3
sys_m = ctrl.ss(Am,Bm,Cm,Dm)
```

```{code-cell} ipython3
sys_m
```

+++

Observando la matriz $\mathbf{A}$ del sistema en su forma canónica modal tiene en su diagonal lo autovalores, y fuera de esto valores igual a 0 o muy cercanos a ceros. Las diferencias con ceros son error numéricos y podemos considerar que la transformación resultó correcta.

+++ 

Por otro lado podemos ver que todos las componentes de B en esta forma canónica son distintos de 0, por lo que el sistema es controlable.

+++ 

Finalmente, para el sistema en la forma canónica modal, todas las componentes de C son distintas de 0,por lo que el sistema es observable.

+++ 

```{warning}
Respecto al uso de numpy. La multiplicación de matrices usando el * la entiende como elemento a elemento si el tipo de datos es un ndarray o un array. Si usamos ese tipo de datos, entonces para que numpy entienda multiplicación de matrices se debe usar el símbolo @. Si el tipo de datos que se está operando es una matriz de `numpy` entonces la multiplicación de matrices se puede hacer con *.

```
