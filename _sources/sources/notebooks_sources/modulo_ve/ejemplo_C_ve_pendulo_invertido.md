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

```{code-cell} ipython3
---
colab: {}
colab_type: code
executionInfo:
  elapsed: 878
  status: ok
  timestamp: 1600887583376
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: 9d3suj4ejvVZ
tags: [remove-cell]
---
import control as ctrl
import numpy as np
import matplotlib.pyplot as plt
```

# Diseño del sistema de control del pédulo invertido

+++ {"colab_type": "text", "id": "zLhphrmYQfRZ"}

Seguimos con el ejemplo de teoría, pero con un pequeño cambio:

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 170
colab_type: code
executionInfo:
  elapsed: 75
  status: ok
  timestamp: 1600887592097
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: xTsRWAtAjvVg
outputId: 9826c964-7267-4d9c-ef74-6c5164c1b0ed
---
w0=2
A=[[0,1],[-w0**2,0]]
B=[[0],[1]]
C=[1,0]
D=0

pendulo=ctrl.ss(A,B,C,D)
pendulo
```

+++ {"colab_type": "text", "id": "X5tpCzCqQxbc"}

## Obtención de la ley de control

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
colab_type: code
executionInfo:
  elapsed: 66
  status: ok
  timestamp: 1600887592433
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: Bmx-2ifgjvVw
outputId: ea5a22d1-a038-4e0f-d016-0708b8d22fb0
---
K=ctrl.acker(pendulo.A,pendulo.B,[-2*w0,-2*w0])
K
```

+++ {"colab_type": "text", "id": "j8aD5bcEQ6Mp"}

## Diseño del estimador completo

```{code-cell} ipython3
---
colab: {}
colab_type: code
executionInfo:
  elapsed: 164
  status: ok
  timestamp: 1600887592864
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: N0WrjtyXjvVm
---
L=ctrl.acker(pendulo.A.T,pendulo.C.T,[-4*w0,-4*w0]).T
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 51
colab_type: code
executionInfo:
  elapsed: 89
  status: ok
  timestamp: 1600887592948
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: mIW2WWYsjvVr
outputId: b094c105-e46c-4c02-d1e2-4ab11d1f0c09
---
L
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
colab_type: code
executionInfo:
  elapsed: 86
  status: ok
  timestamp: 1600887593114
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: 8I5qf9F-jvV1
outputId: 0ed843f1-0c77-4a13-a54f-b1791f508b2c
---
np.linalg.eigvals(pendulo.A-pendulo.B@K)
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
colab_type: code
executionInfo:
  elapsed: 80
  status: ok
  timestamp: 1600887593710
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: f_tJSKkajvV5
outputId: d0add423-274e-4d53-9e88-fca5ac30c441
---
np.linalg.eigvals(pendulo.A-L@pendulo.C)
```

+++ {"colab_type": "text", "id": "0ILtOeUgRDu6"}

## Definimos el sistema compensador ( entrada $y$, salida $u$)

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 170
colab_type: code
executionInfo:
  elapsed: 89
  status: ok
  timestamp: 1600887595681
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: f9-yRdoTjvV_
outputId: 33cd2459-3801-4c74-c127-23f42271dd69
---
s=pendulo
Acomp=s.A-s.B@K-L@s.C
Bcomp=L
Ccomp=-K
comp=ctrl.ss(Acomp,Bcomp,Ccomp,0)
comp
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
colab_type: code
executionInfo:
  elapsed: 74
  status: ok
  timestamp: 1600887596319
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: TBr5EF4LjvWD
outputId: b0be4a9b-a5de-4cd8-8748-8bd40a517640
---
np.linalg.eigvals(comp.A)
```

+++ {"colab_type": "text", "id": "GVl0c3o-SQdr"}

Vemos que los polos del compensador son estables!

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
colab_type: code
executionInfo:
  elapsed: 77
  status: ok
  timestamp: 1600887597398
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: oNNODB36jvWH
outputId: 2dd72ebc-c192-4ffe-e2f8-c10fa237f44a
---
 comp.zero()
```

+++ {"colab_type": "text", "id": "YXLS-XtZR0Wz"}

Verificamos que los polos a lazo cerrado del sistema compensado son los adecuados:

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 51
colab_type: code
executionInfo:
  elapsed: 58
  status: ok
  timestamp: 1600887600472
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: ju4lV5k-RylN
outputId: 657d1a21-388a-48ee-f591-b8603b2726e5
---
sys_comp = ctrl.feedback(-s*comp)
sys_comp.pole()
```

+++ {"colab_type": "text", "id": "SYwU4LmaScIE"}

Ahora verificamos que el sistema a lazo cerrado tiene efectivamente los polos donde se pretendía. Igualmente será necesario rediseñar para lograr un controlador que sea correcto.

+++ {"colab_type": "text", "id": "dePTqAuIjvWM"}

## Sistema completo.


```{code-cell} ipython3
---
colab: {}
colab_type: code
executionInfo:
  elapsed: 99
  status: ok
  timestamp: 1600887322154
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: okSMV0NUjvWM
---
sys_completo =  ctrl.connect(ctrl.append(s,comp),[[1,2],[2,1]],[1],[1,2])
t,y = ctrl.initial_response(sys_completo, X0=[1,0,0,0], T=np.linspace(0,3,1001))
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 430
colab_type: code
executionInfo:
  elapsed: 1141
  status: ok
  timestamp: 1600887607512
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: hBc4qmV7jvWT
outputId: 63552704-e0d2-44fa-afe9-e1847e92880f
tags: [hide-input]
---
fig, ax = plt.subplots(2,1,figsize=(12,8))
ax[0].plot(t,y[0,:],label="sys completo")
ax[0].set_title('Respuesta al escalón en la referencia')
ax[0].set_xlabel("Tiempo")
ax[0].set_ylabel("Salida")
ax[0].grid()
ax[1].plot(t,y[1,:],label="sys completo")
ax[1].set_title('Respuesta al escalón en la referencia')
ax[1].set_xlabel("Tiempo")
ax[1].set_ylabel("Salida")
ax[1].grid()
fig.tight_layout()
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 51
colab_type: code
executionInfo:
  elapsed: 52
  status: ok
  timestamp: 1600887620966
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: nT5TtqmxjvWc
outputId: 79570990-5350-4c92-b3b8-1b8e177fd6be
---
np.linalg.eigvals(sys_completo.A)
```

+++ {"colab_type": "text", "id": "TvMLcJ3H2Y8H"}

# Ubicación óptima de los autovalores del estimador

```{code-cell} ipython3
---
colab: {}
colab_type: code
executionInfo:
  elapsed: 55
  status: ok
  timestamp: 1600887631034
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: dtXrd3-X2iOo
---
B1=B
```

```{code-cell} ipython3
---
colab: {}
colab_type: code
executionInfo:
  elapsed: 53
  status: ok
  timestamp: 1600887631672
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: 54pz7Qhm8b8I
---
def conjugate_tf(G):
    num = ctrl.tf(G).num[0][0]
    den = ctrl.tf(G).den[0][0]
    nume = [num[i]*((-1)**(len(num)%2+1-i)) for i in range(0, len(num))]
    dene = [den[i]*((-1)**(len(den)%2+1-i)) for i in range(0, len(den))]
    return ctrl.tf(nume,dene)
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 455
colab_type: code
executionInfo:
  elapsed: 777
  status: ok
  timestamp: 1600887835105
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: xetdmVeT8kDR
outputId: c2600b06-8bec-4c5a-91c0-1e3f95447346
---
aux1 = ctrl.ss(A,B1,C,D)
aux2 = conjugate_tf(aux1)
G_srle = ctrl.tf(aux1)*aux2
ctrl.rlocus(G_srle, xlim=[-8,8], ylim=[-10,10]); #aumento los límites para ver la zona donde quiero ubicar el polo
plt.gcf().set_size_inches(8,6)
```

```{code-cell} ipython3
---
colab: {}
colab_type: code
executionInfo:
  elapsed: 55
  status: ok
  timestamp: 1600887870395
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: gcEmIb7P-aDR
---
k=12000 # lo busco haciendo clcik sobre las lineas del root locus simétrico
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 51
colab_type: code
executionInfo:
  elapsed: 80
  status: ok
  timestamp: 1600888864627
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: 9ejI9yPM-ga9
outputId: b8a25523-4f02-4b5d-caea-a8a37210da09
---
r,k = ctrl.rlocus(G_srle, kvect=[8000],Plot=False)
r
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
colab_type: code
executionInfo:
  elapsed: 81
  status: ok
  timestamp: 1600888865196
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: Y_dALMUc-n2U
outputId: c793c049-acc5-4a4a-f16f-381b3fe757cb
---
rsel = r[np.real(r)<0]
rsel
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 51
colab_type: code
executionInfo:
  elapsed: 75
  status: ok
  timestamp: 1600888865838
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: dZcgHM_y-0bc
outputId: 0c51db02-5c94-4041-eaa9-74d70485f98a
---
L = ctrl.place(pendulo.A.T, pendulo.C.T, rsel).T
L
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
colab_type: code
executionInfo:
  elapsed: 85
  status: ok
  timestamp: 1600888866534
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: ld31qNduYSCZ
outputId: 0aeb2430-2c10-46fb-a5a1-2d0f4fb16962
---
comp_srl = ctrl.ss(s.A-s.B@K-L@s.C, L, -K, 0)
comp_srl.pole()
```

```{code-cell} ipython3
---
colab: {}
colab_type: code
executionInfo:
  elapsed: 112
  status: ok
  timestamp: 1600888867379
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: 6QhVYlZC_Jxa
---
sys_completo_srl =  ctrl.connect(ctrl.append(s,comp_srl),[[1,2],[2,1]],[1],[1,2])
t_srl, y_srl = ctrl.initial_response(sys_completo_srl, X0=[1,0,0,0], T=np.linspace(0,3,1001))
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 51
colab_type: code
executionInfo:
  elapsed: 76
  status: ok
  timestamp: 1600888870584
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: Ix5XjZeCXalo
outputId: afbfbabf-9545-47e7-b0e1-30003a4fa714
---
L
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
colab_type: code
executionInfo:
  elapsed: 75
  status: ok
  timestamp: 1600888871273
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: qXhyVGAJXg-W
outputId: a84b8e34-25d5-4043-846b-ff9f17d9ad0d
---
K
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 430
colab_type: code
executionInfo:
  elapsed: 608
  status: ok
  timestamp: 1600889024676
  user:
    displayName: gonzalo molina
    photoUrl: ''
    userId: 03146719216223860883
  user_tz: 180
id: w9zPIgY_Xi0e
outputId: 7b4e10e0-63d7-45e3-d8db-dcac5abb2d0e
tags: [hide-input]
---
fig, ax = plt.subplots(2,1,figsize=(12,8))
ax[0].plot(t,y[0,:],label="sys completo 2do orden dominante")
ax[0].plot(t_srl,y_srl[0,:],label="sys completo SRL")
ax[0].set_title('Respuesta al escalón en la referencia')
ax[0].set_xlabel("Tiempo")
ax[0].set_ylabel("Salida")
ax[0].grid()
ax[1].plot(t,y[1,:],label="sys completo 2do orden dominante")
ax[1].plot(t_srl,y_srl[1,:],label="sys completo SRL")
ax[1].set_title('Respuesta al escalón en la referencia')
ax[1].set_xlabel("Tiempo")
ax[1].set_ylabel("Salida")
ax[1].grid()
fig.tight_layout()
```

```{code-cell} ipython3
:colab: {}
:colab_type: code
:id: oavQtEvUa9QK


```
