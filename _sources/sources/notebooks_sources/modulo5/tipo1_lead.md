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

# Problema con sistema de tipo 1

```{code-cell} ipython3
:tags: [remove-cell]

import control as ctrl
import numpy as np
import matplotlib.pyplot as plt
```

+++ {"id": "DY-PCt6BUuum"}

Para el sistema :

$$ G(s) = \frac{10}{s\left(\frac{s}{2.5}+1\right)\left(\frac{s}{6}+1\right)} $$

+++

Se requiere un sistema que tenga un margen de fase de 45 grados y una constante de velocidad $K_v=10$

```{code-cell} ipython3
:id: FIWjpMlgh-jy

s=ctrl.tf('s')
G1=10/((s/2.5+1)*(s/6+1)*s)
```

Analizamos los polos del sistema a lazo abierto

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
id: 3wzuqv1tvisZ
outputId: e4a0e8c3-a2b4-4fff-e740-44ff7153ccfb
---
G1.pole()
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
id: 3vL2_QJUvuNC
outputId: 500d5365-c17d-48a6-cadf-4b4f5c0d94d7
---
ctrl.bode(G1, dB=True);
plt.gcf().set_size_inches(12,8)
```

```{code-cell} ipython3
_,pm,_,_,wp,_=ctrl.stability_margins(G1)
print(pm,wp)
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 51
id: -tl9FHGMvwUQ
outputId: 2640ef1c-0e2f-4b2e-c94b-ff6b3453f6cc
---
ctrl.dcgain(ctrl.minreal(G1*s)) # el minreal es para simplificar el pole en cero y pueda evaluarla
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 51
id: y6CungtPxaxT
outputId: 50754d66-1f23-469d-b178-273ec8952eb4
---
phi_max=(45+10-pm)
print(f"El ángulo máximo a agregar es {phi_max}")
phi_max = phi_max*np.pi/180
alpha = (1-np.sin(phi_max))/(1+np.sin(phi_max))
print(f"Esto produce un esto pruduce una relación z/p de {alpha}")
```

+++ {"id": "O1xRwcsGXtjd"}

Probamos poniendo el el $\omega_{max}$ en la frecuencia de corte.

```{code-cell} ipython3
:id: tPbtVkgyyJzq

wmax=wp
# phi_max=85

phi_max = phi_max*np.pi/180
alpha = (1-np.sin(phi_max))/(1+np.sin(phi_max))
alpha
```

```{code-cell} ipython3
:id: IoQl7vEMynro

TD=1/(wmax*np.sqrt(alpha))
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
id: oh-9CVGKynrt
outputId: 58151a28-a36b-4e10-b3cc-3d9573bed1a9
---
z=-1/TD
p=-1/(alpha*TD)
print(z, p)
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 119
id: 0CEovpvsyp4V
outputId: 0a0686e8-4855-493f-a2d8-0bc082ef1ca3
---
Dc=ctrl.tf([-1/z,1],[-1/p,1])
ctrl.stability_margins(G1*Dc)
```

+++ {"id": "Imfb6pEtX49F"}

Vemos que esto no sirve por que la curva de fase se baja demasiado. Deberíamos rediseñar.

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
id: SViR-hZE0Z_3
outputId: 2d717167-6f36-4dc8-cfc6-48bb47276560
---
wmax=12
phi_max=60*(np.pi)/180

alpha = (1-np.sin(phi_max))/(1+np.sin(phi_max))
TD=1/(wmax*np.sqrt(alpha))
z=-1/TD
p=z/alpha

Dc=ctrl.tf([-1/z,1],[-1/p,1])
print(Dc.zero(), Dc.pole(), Dc.dcgain(), alpha)
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 119
id: Hc5joTg1YM4N
outputId: 82c249ed-5875-4355-e2a7-66f3637c21f6
---
ctrl.stability_margins(G1*Dc)
```

+++ {"id": "VYg2W3IYZVAB"}

Vemos que no se puede llegar a los 45 grados de margen de fase con solo un compensador de adelanto.

Voy a probar con dos dejando el siguiente compensador:

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
id: qGLWg2DbYt-R
outputId: ffdde9c2-e3e3-486d-85f6-e7c264a49732
---
wmax=9
phi_max=35*(np.pi)/180
alpha = (1-np.sin(phi_max))/(1+np.sin(phi_max))
TD=1/(wmax*np.sqrt(alpha))
z=1/TD
p=z/alpha

Dc1=ctrl.tf([1/z,1],[1/p,1])
print(Dc1.zero(), Dc1.pole(), Dc1.dcgain(), alpha)
```

+++ {"id": "ET_qSL9LZ8SZ"}

Probamos con la misma idea:

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
id: KFr4agFqZ20c
outputId: 2f5f0222-f48c-469d-b845-18246438dd01
---
wmax=10
phi_max=40*(np.pi)/180
alpha = (1-np.sin(phi_max))/(1+np.sin(phi_max))
TD=1/(wmax*np.sqrt(alpha))
z=1/TD
p=z/alpha


Dc2=ctrl.tf([1/z,1],[1/p,1])
print(Dc2.zero(), Dc2.pole(), Dc2.dcgain(), alpha)
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 119
id: e8ft-0SqaK-j
outputId: efc6a38e-3013-4d13-a191-48b2c5c6698d
---
ctrl.stability_margins(G1*Dc1*Dc2)
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 430
id: 9ihS0xvbaRQh
outputId: 532cc01b-bf93-45a3-a87e-4c0f720824e7
---
T=ctrl.feedback(G1*Dc1*Dc2)
t,y = ctrl.step_response(T, T=np.linspace(0,2,2401))
```

```{code-cell} ipython3
:tags: [hide-input]

fig, ax = plt.subplots(figsize=(12, 4))
ax.plot(t,y)
ax.set_title('Respuesta al escalón')
ax.set_xlabel('Tiempo')
ax.grid()
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 430
id: P9TBjHZca_ck
outputId: 7bed89f7-7887-4073-f32e-56a4b635c4dd
---
T=ctrl.feedback(G1*Dc1*Dc2)
t,y = ctrl.step_response(T*ctrl.tf(1,[1,0]), T=np.linspace(0,500,2401))
```

```{code-cell} ipython3
:tags: [hide-input]

fig, ax = plt.subplots(figsize=(12, 4))
ax.plot(t,y)
ax.set_title('Respuesta al escalón')
ax.set_xlabel('Tiempo')
ax.grid()
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
id: sGul_zAKcYCK
outputId: d1647ac9-fc6b-428c-faf5-86ca313d66ec
---
error = y[-1]-t[-1]
print(error)
```

```{code-cell} ipython3
(Dc1*Dc2).pole()
```

```{code-cell} ipython3
(Dc1*Dc2).zero()
```

```{code-cell} ipython3

```
