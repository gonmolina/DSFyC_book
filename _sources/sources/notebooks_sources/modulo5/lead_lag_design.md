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

+++ {"id": "a8sV9Q7H6e5p"}

# Ecuaciones útiles para el diseño del Lead por análisis en frecuencia

## Lead

$$D_c = \frac{T_Ds+1}{\alpha T_Ds+1}, \qquad \text{con } 0<\alpha < 1 $$

### Cero del compensador:

$$z=\frac{-1}{T_D}$$

### Polo del compensador

$$z=\frac{-1}{\alpha T_D}$$

### Relación entre el polo y el cero necesaria

$$\alpha(\phi_{mak}) = \frac{1-\sin{\phi_{max}}}{1+\sin{\phi_{max}}}$$

o misma relación escrita al reves

$$ \sin{\phi_{max}}=\frac{1-\alpha}{1+\alpha} $$

### Frecuencia de máxima fase del compensador

$$\omega_{max} = \sqrt{|z||p|}$$

### Ubicación del polo y del cero en función del $\omega_{max}$

$$z=-\omega_{max}\sqrt{\alpha}$$

$$p=-\frac{\omega_{max}}{\sqrt{\alpha}}$$


+++ {"id": "OdZHzOnl9wsW"}

## Procedimiento de Diseño:

1.   Obtener $K$ para
  *   lograr el error requerido, o para 
  *   logar el ancho de bando requerido
2. Evaluar el margen de fase para el sistema con el $K$ anterior
3. Determinar la fase que es necesario agregar y agregarle hasta 10 grados más
4. Determinar el valor de $\alpha$
5. Elegir el $\omega_{max}$ que deseamos como la frecuencia de corte y ubicar el cero $z=-\omega_{max}\sqrt{\alpha}$ y el polo $p=\dfrac{-\omega_{max}}{\sqrt{\alpha}}$
6.Dibujar la respuesta el frecuencia del sistema con el compensador. Evaluar si se cumplen con los requerimientos.
7. Iterar sobre este diseño variando la posición del polo, del cero y de la ganancia. En caso de ser necesario usar dos compensadores.


+++ {"id": "HpC5rVwqAc4c"}

## Ecuaciones útiles para el diseño del Lag por análisis en frecuencia

### Lag

$$D_c = \alpha \frac{T_Is+1}{\alpha T_Is+1}, \qquad \text{con } \alpha > 1$$

#### Cero del compensador:

$$z=\frac{-1}{T_I}$$

#### Polo del compensador

$$p=\frac{-1}{\alpha T_I}$$

#### La ganancia agregada será

$$K = \alpha$$

+++ {"id": "zRNTtO-ZCl67"}

## Procedimiento de diseño de un Lag

1. Determinar la ganancia $K$ que permite cumplir con los requerimientos de margen de fase
2. Dibujar el Diagrama de bode del sistema sin compensar pero con el $K$ obtenido
3. Determinar el $\alpha$ que permita cumplir con las especificaciones de baja frecuencia (error en estado estacionario)
4. Elegir la posición del cero  del compensador que su frecuencia $\omega = \dfrac{1}{T_I}$ encuentre entre una octava y una decada más abajo que la nueva frecuencia de cruce $\omega_c$
5. La otra frecuencia será entonces $\omega=\dfrac{1}{\alpha T_I}$
6. Iterar en el diseño. Ajustar parámestros (polo, cero y ganancia) para cumplir todas las especificaciones.

+++ {"id": "smJzGcFN3LaU"}

## Ejemplo de diseño con un Lead

La siguiente planta representa un sistema térmico:

$$\frac{1}{(\frac{s}{0.5}+1)(s+1)(\frac{s}{2}+1)}$$

Con un compensador Lead logar los siguientes requerimientos:

1. Error de estado estacionario menor a 0.1 frente a una entrada escalón unitario
2. Margen de fase 40 grados

```{code-cell} ipython3
:id: a_hO0smY80xE
:tags: [remove-cell]

import control as ctrl
import numpy as np
import matplotlib.pyplot as plt
```

Vamos a averiguar que sobrevalor esperamos para ese margen de fase

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
id: ib_rohO3IaF6
outputId: 1e3ad7f3-f681-42dc-9b5c-28136e9b4c93
---
zeta=40/100
sv=np.exp(-np.pi*zeta/np.sqrt(1-zeta**2))
sv
```

Ahora definimos nuestro sistema

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 39
id: z9e0jUaRIhg9
outputId: 21c94739-1944-44ed-b6d7-fb536bc05f04
---
s=ctrl.tf('s')
G=1/((s/0.5+1)*(s+1)*(s/2+1))
G
```

Y analizamos la ubicación de los polos a lazo abierto.

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
id: odADyyZYIs4k
outputId: 26fce3a4-3c55-4c8b-e705-2a8671fbdd04
---
G.pole()
```

Vamos a analizar además la ganancia en estado estacionario.

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
id: LG9G5TvSIw80
outputId: f9153dfd-c783-4c17-fdb9-8194705c3562
---
ctrl.dcgain(G)
```

Esto es lo mismo que evaluar el módulo de la función $|G(s)|$ en $s=0$. Verificamos:

```{code-cell} ipython3
np.abs(G(0))
```

Definimos en $K$ la ganancia necesaria que debe tener nuestro sistema para que el error sea 0.1. Recordando que para un sistema de tipo 0

$$ \text{error} = \frac{1}{1+K} $$ 

Se tiene que:

$K$ debe ser igual a 9 para que el error sea 0.1.

```{code-cell} ipython3
:id: Q6mpdAsJJB4i

K=9
```

Como nuestro sistema tiene y auna ganacia de 1, aplicandole el valor de $K$ obtendremos el error de estado estacinario requerido para el sistema. 

Vamos a analizar ahora que pasa con nuestro diagrama de Bode y los margenes de fase de estabilidad con esa ganancia.

```{code-cell} ipython3
:id: rwQl1OIuJEd3

ctrl.bode_plot(K*G, dB=True, margins=True);
plt.gcf().set_size_inches(12,8)
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
id: ULKaC5R7JIYi
outputId: 9e67860f-9bd9-4854-a724-bdfa7b1b6303
---
_, pm, _, _,wp,_ = ctrl.stability_margins(K*G)
pm, wp
```

Vemos que tenemos 7 grados de margen de fase y en una frecuencia aproximada de 1.7 rad/seg. Sieguiendo las recomendaciones escritas anteriormente podemos definie el ángulo que queremos agregar. Debido a que tenemos 7 grados, necesitamos 40 gados en total y se recomienda dejarse 10 grados para ajustar, nos queda un $\phi_{max} = 40 - 7 + 10 =43$

```{code-cell} ipython3
phi_max=43
```

Vamos a pasar esto a rad

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
id: je20tXltN7cu
outputId: 4ed27dc2-24c7-481b-f64d-155946014521
---
phi_max=phi_max*np.pi/180
phi_max
```

Para lograr este ángulo máximo con un compensador de adelanto, se encesita un $\alpha$ de 

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
id: J9oPq8rdKg7x
outputId: 78011690-a393-4a28-8042-79211318ec82
---
alpha = (1-np.sin(phi_max))/(1+np.sin(phi_max))
alpha
```

Vamos a comenzar ubicando el máximo en el punto de donde cruza los 0 db el bode antes de poner el compensador. 

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
id: 64LiWByrT5bp
outputId: 94351dfd-fe3a-4cf0-9e12-015af11e2b9a
---
wmax=wp
wmax
```

Ahora obtenemos el parámetro $T_D$ del lead:

```{code-cell} ipython3
:id: 6HkscH7fLH5S

TD=1/(wmax*np.sqrt(alpha))
```

Por útimo obtenemos las posiciones del polo y del cero

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
id: v5Q6lz9aK63D
outputId: d03e9844-d01d-4e57-9ad6-c5e0599ff559
---
z=-1/TD
p=-1/(alpha*TD)

print(z, p)
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 39
id: q2xhRgwiPzFO
outputId: 01af3123-0d20-475f-de1b-f80f27e3243e
---
Dc=K*ctrl.tf([-1/z, 1],[-1/p, 1])
Dc
```

```{code-cell} ipython3
:id: f33kjdPnQGVh

ctrl.bode([Dc,G, Dc*G], dB=True);
plt.gcf().set_size_inches(12,8)
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 118
id: t9POoe_cQ3Ll
outputId: 7275be3e-354c-4af1-d3ae-5aba7eb129f8
---
ctrl.stability_margins(Dc*G)
```

Podemos ver que el diseño no resultó como queriamos. Esto es debido a que como pusimos un lead con ganacia estacionaria igual a 1, nos agegó ganancia mayores a 1 en la zona de corte de 0 db, produciendo un aumento de la frecuencia en la que corta y una disminución de fase debido a que ahora este margen de fase se mide sobre una frecuencia mayor.

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
id: RBabTj7XSGfK
outputId: d291d3dc-6ab4-4ab5-ad31-dfa142cb13bd
---
T=ctrl.feedback(Dc*G)
t,y = ctrl.step_response(T, np.linspace(0,20,1200))
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
id: RBabTj7XSGfK
outputId: d291d3dc-6ab4-4ab5-ad31-dfa142cb13bd
---
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
  height: 168
id: 834jIztnV_RL
outputId: 6ee29633-a53d-413c-982f-728cb5932ec8
tags: [hide-input]
---
ctrl.step_info(T)
```

+++ {"id": "8lvTo7h65uXr"}

### Rediseño
El problema está en que la frecuencia de corte se me corrión a la izquierda y los 10 gados que agregué no alcanzan para compensar la caida de fase de los tres polos de la planta.

Lo que voy a intentar es agregar mucha más fase y tratar de correr el cero para frecuencias más altas para que no moleste tanto su aumento módulo a en la frecuencia de corte.

Vamos a reescrbir todas las ecuacioes anteriores para que sea más facil de iterar y probar con diferentes valroes de $\phi_{max}$ y $\omega_{max}$.

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
id: rSM4eAxzmiIx
outputId: a8a7512d-3b34-4160-d9cf-5a0e1ff1a05f
---
phi_max=61*(np.pi)/180
alpha = (1-np.sin(phi_max))/(1+np.sin(phi_max))

wmax=5.1
TD=1/(wmax*np.sqrt(alpha))

z=-1/TD
p=z/alpha


Dc=K*ctrl.tf([-1/z,1],[-1/p,1])
print(Dc.zero(), Dc.pole(), Dc.dcgain(), alpha)
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
id: VbDhb3aX7UU-
outputId: 9b8f4691-b54b-4102-cb73-fb33c9f7cfa1
---
_,pm,_,_,wp,_=ctrl.stability_margins(Dc*G)
print(pm,wp)
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
id: -bjavEI6R5RF
outputId: 001af327-e3bb-4ba4-d958-acabbb62d254
---
T=ctrl.feedback(Dc*G)
t,y = ctrl.step_response(T, np.linspace(0,20,1200))
```

```{code-cell} ipython3
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
  height: 168
id: 6PvUSdD6Sa6i
outputId: 12b9f3b1-125a-4edf-fd56-19a205c25c5e
---
ctrl.step_info(T)
```

```{code-cell} ipython3
:id: hTq3YYVDSkpB

ctrl.rlocus(Dc*G);
fig=plt.gcf()
fig.set_size_inches(8,6)
```

+++ {"id": "1K7urwNf_TKq"}

## Problema
La sisguiente planta representa un sistema térmico:
$$\frac{1}{(\frac{s}{0.5}+1)(s+1)(\frac{s}{2}+1)}$$
Con un compensador Lag logar los siguientes requerimientos:

1. Error de estado estacionario menor a 0.1 frente a una entrada escalón unitario
2. Margen de fase 40 grados

+++

Vamos a resolver el mismo problema que resolvimos anteriormente, pero ahora con un Lag.

```{code-cell} ipython3
:id: ltkCND-i7hqk

plt.figure()
ctrl.bode(G, margins=True);
plt.gcf().set_size_inches(12,8)
```

Con el controlador Lag no podemos mejorar el margen de fase, ya que quita fase para toda $\omega$. Para lograr el margen de fase requerido vamos a darle una ganacia al sistema tal que la frecuencia de cruce por 0dB sea la necesria para que el margen de vase de lo necesario para cumplir con este requerimeinto.

Ya que nos piden un margen de fase de 40 grados, lo que vamos a hacer es ver a que frencuencua se tiene -140 de fase y vamos a ver que gancia es necesaria agregarle a la planta para que cruce por 0dB a esta frecuencia, esto es:

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
id: 6LQoQHuA_wcq
outputId: b1699399-2be8-4e7f-b0ff-00c6d96c0e46
---
w1=1.j
K=1/np.abs(G(w1))
K
```

Verificamos calculando los margenes de  estabilidad

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 118
id: nbuzCtEDAKBx
outputId: 6b4c89df-efe1-4dbb-d8ec-6e2440326786
---
ctrl.stability_margins(K*G)
```

Vemos que el margen de fase es de 45 grado (algo más de lo que requerimos) y la frecuencia de cruce por 0 db es 1, comoera de eseperar.

+++

Ahora vemos a calcular el Lag para que nos agregué la ganancia necesaria para cumplir con el error

```{code-cell} ipython3
np.abs(K*G(0))
```

Calculamos la relación $\alpha$ entre el zero y el polo. Esto lo hacemos usando lo que ya sabemos que es que necesitamos una ganancia de 9 sobre la ganacia que tenemos. Esto nos dará la ganacia que necesitamos agregar

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
id: Josdl_W4AtYH
outputId: 7c562ea5-9974-46fa-a0b6-fa7567d39883
---
alpha = 9/ctrl.dcgain(G*K)
print(alpha)
```

Como criterio inicial podemos poner el cero del lag en una frecuencia 5 veces menor a la de cruce por 0 db

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 39
id: Yf6FfJ7VBQjs
outputId: b472978a-1493-4fb8-eba1-796e5c74af0c
---
z=-np.abs(w1)/10
p=z/alpha
Dc2 = K*alpha*ctrl.tf([-1/z,1],[-1/p,1])
Dc2
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 34
id: V-94O4qCCUe_
outputId: cc31e167-dfe4-4af9-8f9e-e28d411cef5e
---
print(Dc2.dcgain(), Dc2.pole(), Dc2.zero())
```

```{code-cell} ipython3
:id: dPCHSMRmDkOg

plt.figure()
ctrl.bode((G, Dc2, Dc2*G), dB=True);
plt.gcf().set_size_inches(12,8)
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 118
id: KZmgBLY5DtGC
outputId: 8b5b4c9b-3ca5-43ae-ac41-93c32144c986
---
ctrl.stability_margins(Dc2*G)
```

```{code-cell} ipython3
:id: wEVDviRaETmK

T1=ctrl.feedback(Dc2*G)
t1,y1 = ctrl.step_response(T1, T=np.linspace(0,20,1200))
```

```{code-cell} ipython3
:tags: [hide-input]

fig, ax = plt.subplots(figsize=(12, 4))
ax.plot(t1,y1)
ax.set_title('Respuesta al escalón')
ax.set_xlabel('Tiempo')
ax.grid()
```

```{code-cell} ipython3
---
colab:
  base_uri: https://localhost:8080/
  height: 168
id: w4CiSdAyZccx
outputId: 2b21cfe0-8f38-4c98-f2ff-403d67ddea14
---
ctrl.step_info(T1)
```

```{code-cell} ipython3

```
