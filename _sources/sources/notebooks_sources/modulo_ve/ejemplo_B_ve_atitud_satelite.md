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

+++ {"id": "lxZfKlZ-kVxN", "colab_type": "text"}

# Diseño del sistema de control del atitud de un satélite

+++ {"id": "_EzuAq45kVxO", "colab_type": "text"}

## Definimos el sistema:

```{code-cell} ipython3
---
colab: {}
colab_type: code
executionInfo:
  elapsed: 763
  status: ok
  timestamp: 1600889891217
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: ByjXN7_nkVxP
---
import numpy as np
import control as ctrl
import matplotlib.pyplot as plt
%matplotlib inline
```

+++ {"id": "yr0zAycUkVxV", "colab_type": "text"}

Diseñar un compensador para la planta: 

$$ \dot{\mathbf{x}}=\begin{bmatrix}
0&1\\
0&0\\
\end{bmatrix}\mathbf{x}+\begin{bmatrix}
0\\
1
\end{bmatrix}u$$

$$ y=\begin{bmatrix}
1&0\end{bmatrix}\mathbf{x}.$$

que ubique los polos tal que $\omega_n =1$ y $\zeta=0.707$:

```{code-cell} ipython3
---
colab: {}
colab_type: code
executionInfo:
  elapsed: 94
  status: ok
  timestamp: 1600889892775
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: FqZgbz_IkVxW
---
A=[[0,1],[0,0]]
B=[[0],[1]]
C=[[1,0]]
D=[[0]]
sys=ctrl.ss(A,B,C,D)
```

+++ {"id": "hl1OfdGorC8C", "colab_type": "text"}

## Análisis de Controlalibidad y Observabilidad

+++ {"id": "IWMewafdkVxb", "colab_type": "text"}

Vemos que el sistema es controlable usando las matrices C y B  dadas. Aunque no lo sería se fuesen diferentes (es decir se actuara o se midiera diferente).

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 85
colab_type: code
executionInfo:
  elapsed: 85
  status: ok
  timestamp: 1600889894935
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: JlpOQeukkVxc
outputId: d94493a0-d0b9-4be6-95a3-7ec74cf3d525
---
print(np.linalg.matrix_rank(ctrl.obsv(A,C)))

print(np.linalg.matrix_rank(ctrl.obsv(A,[[0,1]])))

print(np.linalg.matrix_rank(ctrl.ctrb(A,B)))

print(np.linalg.matrix_rank(ctrl.ctrb(A,[[1],[0]])))
```

+++ {"id": "lL9UNOaXkVxh", "colab_type": "text"}

Para cumplir con los requerimientos debemos ubicar los polos en $\alpha_c = -0.7070 \pm 0.0707j$.

+++ {"id": "01TsvxNmrR05", "colab_type": "text"}

## Ley de Control

+++ {"id": "iKyQsCp8kVxi", "colab_type": "text"}

Usando el comando `acker` o `place` obtenemos la ley de control que ubica los polos del sistema donde queremos.

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
colab_type: code
executionInfo:
  elapsed: 82
  status: ok
  timestamp: 1600889897764
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: wOfCTiznkVxj
outputId: 6cb91894-36a2-4406-f69b-482b6746e62f
---
wnc=1
zetac=np.sqrt(2)/2
polyc=[1,2*zetac*wnc,wnc**2]
alpha_c =np.roots(polyc)
print(alpha_c)
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
colab_type: code
executionInfo:
  elapsed: 72
  status: ok
  timestamp: 1600889898437
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: qu9HKSJhkVxo
outputId: 5ec4c221-09d0-4cf8-a31c-25e54cbfced1
---
K = ctrl.place(A,B,alpha_c)
print(K)
```

+++ {"id": "SQQ6KAikkVxt", "colab_type": "text"}

Usando `acker`

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
colab_type: code
executionInfo:
  elapsed: 98
  status: ok
  timestamp: 1600889899763
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: gPb4kAERkVxv
outputId: b98e5c5f-2142-49a8-9ece-2f1b07190f91
---
K = ctrl.acker(A,B,alpha_c)
print(K)
```

+++ {"id": "FyEuZ0KGkVx1", "colab_type": "text"}

Controlamos que las cuentas esten bien hechas, viendo que los autovalores de $\mathbf{A}-\mathbf{BK}$ den donde tienen que dar:

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
colab_type: code
executionInfo:
  elapsed: 98
  status: ok
  timestamp: 1600889901123
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: zpxsXvuukVx2
outputId: 02687b9e-1290-4b63-9e1d-75c470a1cbef
---
l,_=np.linalg.eig(A-B@K)
print(l)
```

+++ {"id": "NJPe9GI6rmU_", "colab_type": "text"}

## Diseño del estimador

+++ {"id": "Pwd3TbEIkVx7", "colab_type": "text"}

Ahora ubicamos los polos del estimador 5 veces más rápido, pero con un $\zeta$ que haga que no oscile ($\zeta=0.5$).

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
colab_type: code
executionInfo:
  elapsed: 91
  status: ok
  timestamp: 1600889904116
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: u6E7pRxokVx8
outputId: 47e8a7b4-e1e0-416b-844b-023d381c0b45
---
wne=5
zetae=0.5
polye =[1,2*zetae*wne,wne**2]
rootse=np.roots(polye)
print(rootse)
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 51
colab_type: code
executionInfo:
  elapsed: 104
  status: ok
  timestamp: 1600889904774
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: 7lFWGEbNkVyA
outputId: 7b0eacb0-b62f-464b-cae4-14366c2318d9
---
Lt=ctrl.place(sys.A.T,sys.C.T,rootse).T
print(Lt)
```

+++ {"id": "Pvirfdbtr3pJ", "colab_type": "text"}

## Definición del sistema de control

+++ {"id": "E-f_6hKZkVyE", "colab_type": "text"}

Expresando las ecuaciones de estado del controlador $D_c$ nos queda:

$$\begin{eqnarray}
\mathbf{\dot{\hat{x}}}&=&(\mathbf{A-BK-LC}) \mathbf{\hat {x}}+\mathbf{L}y\\
u&=&-\mathbf{K}\mathbf{\hat{x}}\end{eqnarray}$$

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 39
colab_type: code
executionInfo:
  elapsed: 199
  status: ok
  timestamp: 1600889907573
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: VCOqKt8EkVyK
outputId: 14b6abd4-acd1-48c6-be92-c37bba6abebb
---
Dc=ctrl.ss(A-B@K-Lt@C,Lt,-K,[0])
ctrl.tf(Dc)
```

+++ {"id": "hbw1qSG7kVyP", "colab_type": "text"}

Con este controlador podemos hacer un análisis de sistema igual que lo hacíamos con control clásico.

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 283
colab_type: code
executionInfo:
  elapsed: 1521
  status: ok
  timestamp: 1600889910845
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: imZqGhTykVyQ
outputId: 11e91e3f-2dde-4ab1-dc81-eedb859fbae0
---
_=ctrl.bode(sys*-Dc,dB=True)
plt.gcf().set_size_inches(12,8)
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
colab_type: code
executionInfo:
  elapsed: 77
  status: ok
  timestamp: 1600889936944
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: YH91gmBqkVyU
outputId: 5b47a582-936e-4e79-b36c-aa16bd72bbd6
---
ctrl.margin(sys*-Dc)
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 280
colab_type: code
executionInfo:
  elapsed: 510
  status: ok
  timestamp: 1600889937979
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: Hx6MMMBPkVyY
outputId: 95d82932-75f5-490f-cb0c-9c8de96c66a2
---
_=ctrl.rlocus(sys*-Dc,grid=True)
plt.gcf().set_size_inches(8,6)
```

+++ {"id": "f8xlqFUckVyd", "colab_type": "text"}

Podemos ver que este diseño tiene un margen de fase de 50 grados aproximadamente.

+++ {"id": "KXY5AUwYkVyd", "colab_type": "text"}

## Con estimador reducido

+++ {"id": "bwdu9HtukVye", "colab_type": "text"}

Pondremos ahora el polo del estimador en -5rad/seg.

```{code-cell} ipython3
---
colab: {}
colab_type: code
executionInfo:
  elapsed: 65
  status: ok
  timestamp: 1600889940684
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: LSbkFwhlkVyf
---
polesered=[-5]
Aaa=sys.A[0:1,0:1]
Aab=sys.A[0:1,1:]
Aba=sys.A[1:,0:1]
Abb=sys.A[1:,1:]

Ba = sys.B[0:1,0]
Bb = sys.B[1:,0]
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
colab_type: code
executionInfo:
  elapsed: 78
  status: ok
  timestamp: 1600889941979
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: jx4poHFnkVyj
outputId: 2d1417eb-0326-46ce-cba8-053796e04893
---
Ltred = ctrl.acker(Abb.T,Aab.T,polesered).T
Ltred
```

+++ {"id": "QD2AnykekVyn", "colab_type": "text"}

Ahora podemos obtener la ley de control igualmente mediante estas ecuaciones del controlador para un sistema con un estimador reducido.

Para esto necesitamos devidir las $\mathbf K$ que multiplican a la parte medida de los esatados ($\mathbf{K_a}$) de la no medida ($\mathbf{K_b}$).

+++ {"id": "qoo2qOtRsNz1", "colab_type": "text"}

## Definición del sistema  controlador con estimador de orden reducido

```{code-cell} ipython3
---
colab: {}
colab_type: code
executionInfo:
  elapsed: 48
  status: ok
  timestamp: 1600889946396
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: hMD5R5vUkVyo
---
Ka=K[0,0:1]
Kb=K[0,1:]

Ar = Abb-Ltred@Aab-(Bb-Ltred@Ba)*Kb
Br = Ar@Ltred + Aba - Ltred@Aaa - (Bb-Ltred@Ba)@Ka
Cr = -Kb
Dr = -Ka-Kb@Ltred
```

```{code-cell} ipython3
---
colab: {}
colab_type: code
executionInfo:
  elapsed: 53
  status: ok
  timestamp: 1600889947052
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: eCDPLt6EkVyr
---
Dcr = ctrl.ss(Ar,Br,Cr,Dr)
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 39
colab_type: code
executionInfo:
  elapsed: 118
  status: ok
  timestamp: 1600889947808
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: XrcVjH4XkVyv
outputId: e57c990e-6f61-49bb-c274-591aab00099e
---
ctrl.tf(-Dcr)
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 283
colab_type: code
executionInfo:
  elapsed: 1165
  status: ok
  timestamp: 1600889950193
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: ekU26Rt_kVyz
outputId: 0e769e42-6446-4f9e-ee37-636d20cf0e01
---
_=ctrl.bode((-Dcr*sys),dB=True)
plt.gcf().set_size_inches(12,8)
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
colab_type: code
executionInfo:
  elapsed: 69
  status: ok
  timestamp: 1600889952306
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: wwy7D__MkVy2
outputId: c7dda6fb-74b7-4978-c1da-c2aa24c3010b
---
ctrl.margin(-Dcr*sys)
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 280
colab_type: code
executionInfo:
  elapsed: 673
  status: ok
  timestamp: 1600889955804
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: pVsv_5d3kVy6
outputId: c69ee9cd-22ff-48ea-8f37-6f827424f9e8
---
_=ctrl.rlocus(-Dcr*sys)
plt.gcf().set_size_inches(8,6)
```

```{code-cell} ipython3
---
colab: {}
colab_type: code
executionInfo:
  elapsed: 47
  status: ok
  timestamp: 1600889958948
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: DAXSgDfUkVy9
---

```

```{code-cell} ipython3
:colab: {}
:colab_type: code
:id: 7qXNJ52OkVzB


```
