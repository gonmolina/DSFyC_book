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

+++ 

# Inserción de referencias

+++ 
Para introducir el seguimiento de referencias, siguiendo la idea de control clásico uno podría hacer $u=-\mathbf K \mathbf x + r$, siendo $r$ la referencia. Sin embargo esto en general llevaría a tener errores de estados estacionarios muy grandes.

La forma correcta sería tratar de calcular los estados y la entrada en estado estacionario($t \rightarrow \infty$). Si entramos con el valor de estado estacionario de $u$ y hacemos que para $t \rightarrow \infty$ la realimentación sea 0, entonces nos aseguramos que el sistema tendrá error nulo. Matemáticamente esto lo podemos lograr haciendo que:

$$u=u_{ss}-\mathbf K(\mathbf x - \mathbf{x_ss})$$

Para resolver cuanto vales $u_ss$ y $\mathbf{x_ss}$ debemos expresar las ecuaciones de estado con las condiciones de estado estacionario, es decir:

$$
\begin{eqnarray}
0&=&\mathbf{Ax_{ss}}+\mathbf{B}u_{ss} \\
yss&=&\mathbf{Cx_{ss}}+\mathbf{D}u_{ss}
\end{eqnarray}
$$

Entonces se quiere resolver los valores para los cuales $y_{ss }=r_{ss}$ para cualquier $y_ss$. Para hacer esto hacemos que $\mathbf{x_{ss}}=\mathbf{N_x}r_{ss}$ y $u_{ss}=N_ur_{ss}$. Haciendo esto tenemos el siguiente sistema de ecuaciones:

$$
\begin{bmatrix}
\mathbf A & \mathbf B\\
\mathbf C & D
\end{bmatrix} \begin{bmatrix}
\mathbf{N_x}\\
N_u\\ \end{bmatrix} =\begin{bmatrix}
\mathbf 0\\
1\end{bmatrix}
$$

+++ 

De esta manera

$$\begin{bmatrix}
\mathbf{N_x}\\
N_u\\ \end{bmatrix} =
\begin{bmatrix}
\mathbf A & \mathbf B\\
\mathbf C & D
\end{bmatrix}^{-1}\begin{bmatrix}
\mathbf 0\\
1\end{bmatrix}.
$$

Con estos valores, entonces podemos introducir la referencia (de forma "no robusta") haciendo:

$$u=N_ur-\mathbf {K}(\mathbf{x} - \mathbf{N_x}r)$$


Sabiendo cuanto vale la ley de control, es decir $\mathbf{K}$, podemos simplificar la expresión anterior de la siguiente manera:

$$u=-\mathbf{Kx}+\bar Nr$$ 

donde $\bar N = N_u+\mathbf{KN_x}$

+++ 

Para poder ver el esfuerzo de control en la salida del sistema que agreguemos tenemos que  expresar las ecuaciones de estado en función de las entradas (ahora la entrada es $r$) y los estados (siguen siendo $\mathbf{x}$). Entonces la ecuación de salida que tengo que agregar es 

$$u=-\mathbf{K x}+\bar Nr$$


````{admonition} Funciones para el cálculo del seguimiento a referencia
:class: Hint

Con esta función podemos calcular las matrices para el seguimiento a referencia:

```{code} ipython3

def seguimiento_referencia(sys):
    aux1=np.vstack((np.hstack((sys.A,sys.B)),
                   np.hstack((sys.C,sys.D))))
    n=np.shape(sys.A)[0]
    aux2=np.vstack((np.zeros((n,1)),[1]))
    N=np.linalg.inv(aux1)@aux2
    Nx=N[0:n]
    Nu=N[n]
    return Nx, Nu
```

Para simplificar la implementación podemos usar solo esta ganancia:

```{code} ipython3
def calculate_Nbar(Nx,Nu,K):
    return Nu+K@Nx
```

````


+++ 


