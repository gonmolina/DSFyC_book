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
import control as ctrl
import numpy as np
import matplotlib.pyplot as plt
```

# Problema de levitador magnético 

Definimos el sistema en estudio

```{code-cell} ipython3
G=ctrl.tf(-40,[1,20,-100,-2000])
G
```

```{code-cell} ipython3
G.pole()
```

```{code-cell} ipython3
s=ctrl.tf('s')
```

Otra forma de definirlo:

```{code-cell} ipython3
Gs=-40/((s+20)*(s**2-100))
Gs
```

Lo analizamos con el bode

```{code-cell} ipython3
ctrl.bode(Gs, dB=True)
plt.gcf().set_size_inches(12,8)
```

```{code-cell} ipython3
zeta=0.46
sv=np.exp(-np.pi*zeta/(np.sqrt(1-zeta**2)))
sv
```

Obtenermos el $\omega_n$

```{code-cell} ipython3
wn=4/0.4/0.46
wn
```

Del resutlado anterior defino $\omega_c=22$, calculo la ganancia necesaria para obtener esa frecuencia de corte

```{code-cell} ipython3
wc=22
k1=1/np.abs(G(wc*1j))
k1
```

```{code-cell} ipython3
ctrl.stability_margins(G*k1)
```

```{code-cell} ipython3
T=ctrl.feedback(k1*G)
T.pole()
```

Vemos que hicimos un controlador inestable. Vamos a estudiar la estabildad con Nyquist (en la pizarra). Luego verificamos con el Nyquist.

```{code-cell} ipython3
ctrl.rlocus(-G);
plt.gcf().set_size_inches(8,6)
```

```{code-cell} ipython3
ctrl.rlocus(G);
plt.gcf().set_size_inches(8,6)
```

Vemos que pasa en la frecuencia de cruce que necesitamos

```{code-cell} ipython3
ctrl.bode(-k1*G);
plt.gcf().set_size_inches(12,8)
```

```{code-cell} ipython3
ctrl.stability_margins(-k1*G)
```

Podemos ver que el sistema neceista más de 90 grados lo cual es imposible de conseguir con un solo cero. Vamos a tener que agregar un cero y un lead como mínimo. 

Vamos a tratar de agregar un cero, en algún lugar que no molesta al transitorio pero que agrega fase. Este lugar podria ser -10, tapando el polo real negativo de la planta.

```{code-cell} ipython3
d1=(s+10)
```

```{code-cell} ipython3
ctrl.bode(-k1*G*d1);
plt.gcf().set_size_inches(12,8)
```

```{code-cell} ipython3
phi_max=50*np.pi/180
alpha = (1-np.sin(phi_max))/(1+np.sin(phi_max))
alpha
```

```{code-cell} ipython3
wmax=22
TD=1/(wmax*np.sqrt(alpha))
z=-1/TD
p=-1/(alpha*TD)
z, p
```

```{code-cell} ipython3
d2=(-s/z+1)/(-s/p+1)
d2
```

```{code-cell} ipython3
kd2=1/np.abs(d2(wc*1j))
ctrl.bode(d2*kd2, dB=True);
plt.gcf().set_size_inches(12,8)
```

```{code-cell} ipython3
C1=d1*d2*kd2
ctrl.bode(-G*d1*d2*kd2, dB=True);
fig=plt.gcf()
fig.set_size_inches(12,8)
```

```{code-cell} ipython3
k3=1/np.abs((-G*d1*d2*kd2)(22j))
k3
```

```{code-cell} ipython3
T=ctrl.feedback(-G*d1*d2*kd2*k3)
T.pole()
```

Vemos que este diseno no resultó del todo correcto. Este se debe a varias razones:
- poca ganancia en estado estacionario
- un polo negativo chico que resulta dominante frente a los que diseñamos

```{code-cell} ipython3
t,y=ctrl.step_response(T, T=np.linspace(0,5,2000))
```

```{code-cell} ipython3
fig, ax = plt.subplots(figsize=(12, 4))
ax.plot(t,y)
ax.set_title('Respuesta al escalón')
ax.set_xlabel('Tiempo')
ax.grid()
```

```{code-cell} ipython3
L=-G*d1*d2*kd2*k3
ctrl.bode(L, dB=True);
```

```{code-cell} ipython3
:tags: [hide_input]

fig=plt.gcf()
fig.set_size_inches(12,8)
```

```{code-cell} ipython3
G
```

```{code-cell} ipython3
G.pole()
```

Para lograr un controlador que en principio funcione y epnsar de otra manera los lead-lag podemos hacer loop-shaping, y después ajusta a nuestro a lo que necesitemos.

```{code-cell} ipython3
Cls=((s+10)*(s+10)*(s+20))/s
```

```{code-cell} ipython3
ctrl.bode(-G*Cls);
fig=plt.gcf()
fig.set_size_inches(12,8)
```

Analizamos que margen de fase tenemos a 22 rad y luego vemos la respuesta. Podemos después redisenar y mejorar aumentando la frecuencia de corte. Recordar que los isstemas inestables son más faciles de controlar más rápido que lento

```{code-cell} ipython3
kls=1/np.abs((G*Cls)(wc*1j))
```

```{code-cell} ipython3
ctrl.bode(-G*Cls*kls);
fig=plt.gcf()
fig.set_size_inches(12,8)
```

```{code-cell} ipython3
ctrl.stability_margins(-G*Cls*kls)
```

```{code-cell} ipython3
T=ctrl.feedback(-G*Cls*kls)
t,y=ctrl.step_response(T)
```

```{code-cell} ipython3
:tags: [hide_input]

fig, ax = plt.subplots(figsize=(12, 4))
ax.plot(t,y)
ax.set_title('Respuesta al escalón')
ax.set_xlabel('Tiempo')
ax.grid()
```

```{code-cell} ipython3
:tags: []

Cls
```

```{code-cell} ipython3
Cls2=Cls*1/(s/400+1)**2
Cls2
```

```{code-cell} ipython3
Tu=ctrl.feedback(-Cls2,G)
t,y=ctrl.step_response(Tu, T=np.linspace(0,1,2000))
```

```{code-cell} ipython3
:tags: [hide_input]

fig, ax = plt.subplots(figsize=(12, 4))
ax.plot(t,y)
ax.set_title('Respuesta al escalón')
ax.set_xlabel('Tiempo')
ax.grid()
```

```{code-cell} ipython3

```
