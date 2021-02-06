---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

#  Ejemplos y ejercicios


## Astronomía CCD
```{admonition} Astronomía CCD  Ejemplo 0
:class: tip
Determine el campo registrado por una cámara con detector CCD de 1024 $\times$ 2028 píxeles de 22 micras en un telescopio de focal 2m. 
```
<pre>
Escala de placa  P = 206265 / 2000 mm = 103.1325  arcsec/mm
Tamaño lineal del CCD  
   1024 x 22 micras = 22528 micras = 22.528 mm
   2048 x 22 micras = 2 x 22528    = 45.056 mm
Proyectado sobre el cielo
   FoV = (22.528 x 103.1325)  x (45.0.56 x 103.1325) 
       = (2323.36896 arcsec)  x (4646.73792 arcsec)
       = 38.72 arcmin x 77.45 arcmin
</pre>

```{admonition} Astronomía CCD  Ejercicio 0
Determine el campo registrado por una cámara con detector CCD de 1024 $\times$ 2028 píxeles de 22 micras en un telescopio de focal 2m. 
```
```{admonition} Astronomía CCD  Ejercicio 1
MOSCA es una cámara óptica del telescopio de 3.5m de Calar Alto que emplea un reductor de focal. Sabiendo que la focal equivalente del sistema es 9.45 m y que usa un detector de 2048×4096 pıxeles de 15 μm, determine la escala en arcsec/pıxel y el campo registrado en el detector.
```

```{admonition} Astronomía CCD  Ejemplo 1
:class: tip
Se muestran el recorte de una observación con CCD centrado en una imagen de estrella. Supongamos que la señal de la estrella ocupa sólo los píxeles centrales. La observación científica corresponde a 2 minutos de exposición de una estrella de magnitud m=12. Suponiendo que la respuesta espacial es constante en ese área y que la señal de Bias = 100 cuentas y el DARK = 2 cuentas/minuto, determinar cómo sería la imagen de la observación de una estrella de magnitud m=11 con 3 minutos de exposición. 
```


```{figure} /_static/lecture_specific/p1_intro/intro_06_SN_8a.png
---
width: 500px
name: SN_8a-fig
---
Observaciones y primeros pasos para determinar el flujo neto (cuentas/minuto) de una estrella de m=12. A la señal observada le restamos el BIAS=100 cuentas y posteriormente la señal de DARK = 2 cuentas/minuto $\times$ 2 minutos = 4 cuentas.
```

```{figure} /_static/lecture_specific/p1_intro/intro_06_SN_8b.png
---
width: 500px
name: SN_8a-fig
---
Determinación del valor del fondo de cielo (en dos minutos) y substracción a la señal de la estrella. Finalmente obtenemos el flujo en 1 minuto de exposición.
```

```{figure} /_static/lecture_specific/p1_intro/intro_06_SN_9.png
---
width: 500px
name: SN_9-fig
---
Pasos en la determinación de la imagen esperada para una estrella de m=11 en 3 minutos de exposición.
```
En esta figura vemos cómo primero tenemos que determinar las cuentas de una estrella de magnitud m=11. Como hay una diferencia de magnitud $\Delta m = 1$ eso equivale (Pogson)  $2.5 \times$ es decir $90 \times 2.5 = 225$ cuentas en 1 minuto. En tres minutos de exposición serán 675 cuentas (si el coeficiente de extinción se mantiene constante y el sistema es lineal). Calculamos entonces la contribución del fondo de cielo y del Dark en 3 minutos y sumamos todas las contribuciones.

```{admonition} Astronomía CCD  Ejercicio 1
Se muestra una zona de 3x3 píxeles resultado de observar con un detector tipo CCD de forma que se ha registrado una estrella y el fondo de cielo vecino (primer y segundo panel) y algunas imágenes de calibración en la misma zona del chip. Suponiendo una corriente de oscuridad de 2 c/s, determine el flujo neto en c/s que se recibe de la estrella. 
```
<pre>
Estrella (30s)    Cielo (20s)   Dark (0s)  Flat Field (5s)
535  632 466     201 179 179    10 10 10    128 112 112
729 1180 632     201 190 179    10 10 10    128 120 112
535  729 466     201 201 179    10 10 10    128 128 112
</pre>

```{admonition} Astronomía CCD  Ejemplo 2
:class: tip
Se mide en una imagen CCD ya procesada una señal de 2500 y 4500 cuentas (incluyendo el fondo de cielo) para dos estrellas. Una medida en la zona libre de estrellas proporciona 500 cuentas en un área similar a la usada en la medida de las estrellas. Determine la diferencia de magnitud de las estrellas en esa banda.
```
El flujo neto (sin el fondo de cielo se obtiene restando esa contribución a la señal:  
Estrella 1      F1 = 2500 – 500 = 2000 cuentas  
Estrella 2    F2 = 4500 – 500 = 4000  
Y la diferencia de magnitud comparando los flujos con la ecuación de Pogson:    
m2 – m1 = -2.5 log10 (F2/F1) = -2.5 log10 (2) = -0.75 magnitudes

```{admonition} Astronomía CCD Ejemplo 3
:class: tip
Se miden 40 cuentas/pixel en el cielo de una imagen obtenida con CAFOS en el 2.2m del observatorio de Calar Alto (f/4.4) con un CCD de pixel de 30 micras en una exposición de 12s. Suponiendo una constante instrumental de C=24.1 en la banda de observación, determinar el brillo de fondo de cielo en esa banda.
```
<pre>
f = 2.2 m x 4.4 = 9.68 m
P = 206265 / 9680 mm = 21.30837 "/mm 
1 pix = 30 micras 
Tamaño del pixel      21.30837 "/mm x 0.030 = 0.639251 arcsec
área del pixel          0.639251 x 0.639251 = 0.408642 arcsec2

m = C - 2.5 log F(C/s/arcsec2) 
  = 24.1 - 2.5 x log (40/12/0.408642) 
  = 24.1 - 2.5 log 8.157 
  = 24.1 - 2.5 x  0.911536 
  = 24.1 - 2.27 = 21.82  magn/arcsec2
</pre>

```{admonition} Astronomía CCD Ejercicio 4
En una imagen de 180s tomada con una cámara CCD (píxel de 24 micras) acoplada a un telescopio f/6 de 150 cm de diámetro se obtiene un valor medio de 360 cuentas por píxel en el fondo de cielo. Si en esa banda fotométrica la constante instrumental vale 21.2, determine el valor del fondo de cielo en mag/arcsec$^2$ en esa banda en esa dirección.
```

```{admonition} Astronomía CCD Ejercicio 5
Para calibrar una observación CCD de 25s de exposición se dispone de 3 imágenes de DARK de 5, 10 y 30s con valores medios de 560, 570, y 610 cuentas respectivamente y de un Flat Field de 60s. De termine si se puede calibrar y cómo.
```

```{admonition} Astronomía CCD Ejercicio 6
El valor medio de la observación del fondo de cielo usando un detector tipo CCD es de 620, 630, y 640 cuentas para exposiciones de 20, 30 y 40s. ¿Se puede determinar el nivel de BIAS? ¿Qué suposiciones serían necesarias?
```
```{admonition} Astronomía CCD Ejercicio 7
Se dispone de un detector de 1024 × 2048 píxeles de 15 micras. Determine el campo registrado con un telescopio de focal f=3 m y el número de exposiciones necesarias para cubrir un campo de 30×30 arcmin$^2$.
```
```{admonition} Astronomía CCD Ejercicio 8
Se obtienen dos imágenes de 10 s y 20 s respectivamente con un detector lineal. La medida del fondo de cielo en una zona libre de estrellas proporciona 110.000 y 120.000 cuentas en un área de 100 pixeles. Determine el valor del BIAS asumiendo que la corriente de oscuridad es nula.
```

## Relación señal-ruido
```{admonition} Relación señal-ruido  Ejemplo 1
:class: tip
Se obtiene una relación señal/ruido SNR = 10 observando una estrella de magnitud V=14 durante 20s. Determine la SNR que se obtendría con el mismo equipo instrumental en un telescopio del doble de diámetro para una estrella V=12 si integramos 10s.
```

- V=14 texpo= 20s y telescopio D  
Si la SNR = 10 entonces la señal es  S = 100

- V=14 texpo= 20s y telescopio D$\times$2 (área colectora $\times$ 4)  
S = 100 $\times$ 4 = 400  

- V=14 texpo= 10s y telescopio D$\times$2 (la mitad de exposición)  
S = 400/2 = 200  

- V=12 texpo= 10s y telescopio D$\times$2 (Estrella más brillante V=14 --> V=12)  
	-2.5 log (F2/F1) = 14 – 12 = 2   --> F2/F1= 6.31   
	S = 200 $\times$ 6.31 = 1262  
	SNR = $\sqrt{S} = 35$

```{admonition} Relación señal-ruido  Ejercicio 1
En observaciones de fotometría fotoeléctrica se obtiene una SNR=15 observando una cierta estrella durante 10s. Deterine la SNR que obtendíamos con el mismo equipo instrumental (fotómetro, banda, etc.) pero montado en un telescopio del doble de diámetro e integrando 20s.
```
```{admonition} Relación señal-ruido  Ejercicio 2
Se desea obtener relación señal ruido S/N=100 midiendo el fondo de cielo en 100 píxeles cuando se llegan 10 cuentas/s en cada píxel de un detector de 0.25 arcsec de lado en las zonas libres de objetos en una banda en la que la constante instrumental es 25. Determine el tiempo de exposición necesario. Determine la magnitud del fondo de cielo en esa banda
```

```{admonition} Relación señal-ruido  Ejercicio 3
Con el MPF (Multi-Purpose Fotometer) instalado en el JKT (D= 1 m) se obtiene una relación
señal/ruido SNR=30 cuando se integra durante 10 s la señal procedente de una estrella de
magnitud $m_v$=11 si se emplea un filtro de paso de banda $\Delta \lambda$=10 Angstroms. Determínese la SNR que
se obtendría para una estrella de $m_v$=13 si se integra durante 20 s en el INT (D= 2.5 m) y se
emplea un filtro de $\Delta \lambda$ =30 Angstroms en la misma región del espectro.
```


## Fotometría

```{admonition} Fotometria CCD Ejemplo 1
:class: tip
Observando en la banda V de Johnson con un cierto sistema se registran para dos estrellas estándar los resultados que se muestran en la siguiente tabla:
<pre>
Estrella    Vo      F(c)     Texp (s)   altura (deg)
#1          12.50   2054.0   100        40
#2          10.05   2267.8    10        60
</pre>
```
<pre>
m0 = C -2.5 log F(c/s) – K sec z
	Vo 	F(c)	T (s) 	F(c/s) 	altura	z  secz
#1 	12.50 	2054.0 	100   	20.54	40	50 1.556
#2 	10.05 	2267.8 	10 	226.78	60	30 1.155

m0 + 2.5 log F(c/s)    = C - K sec z
12.5 + 2.5 x 1.313  = C – K x 1.556      15.783  = C – K x 1.556
10.0 + 2.5 x 2.356  = C – K x 1.155      15.943  = C – K x 1.155
                                         15.783 – 15.943 = -0.16 = - K ( 1.556 – 1.155)  
--> K = 0.16 / 0.4 = 0.4 
C = 15.943 + 0.4 x 1.155 = 16.405
 
Flujo en cénit     sec z = 1   	
m0    = C      – 2.5 log F(c/s) - K sec z
11.5  = 16.405 – 2.5 log F(c/s) - 0.4 x 1
16.405 -11.5 -0.4 = 2.5 log F    -->  log F = 4.505/2.5 --> F = 63.38 c/s

Flujo fuera de la atmósfera    sec z = 0    ó   K = 0   	
m0    = C      –  2.5 log F(c/s)
11.5  = 16.405 – 2.5 log F(c/s) 
16.405 -11.5  = 2.5 log F    -->  log F = 4.905/2.5 --> F = 91.62 c/s 
</pre>

```{admonition} Fotometria CCD Ejercicio 1
El flujo de una estrella a 60$^o$ de altura es de 300 cuentas/s. Sabiendo que el coeficiente de extinción en esa banda es de 0.2 mag/masa de aire, determine el flujo esperado de esa estrella a 40$^o$ de altura.
```

```{admonition} Fotometria CCD Ejercicio 2
Observando en la banda B de Johnson con un cierto sistema se registran para dos estrellas estándar los resultados que se muestran en la siguiente tabla:

<pre>
Estrella | m$_o$ | F(c)  | Texp (s) | altura |
#1       |  8.2  | 24000 |  60      | 60     |
#2       |  7.9  | 15000 |  30      | 50     |
</pre>

Suponiendo que estamos observando en una noche fotométrica, determínese la constante instrumental y el coeficiente de extinción en B para esa noche. Estímese la extinción en la banda V (supóngase $\lambda_{efectiva}$ 550 nm y 430 nm para V y B respectivamente).
```

```{admonition} Fotometria CCD Ejercicio 3
Observando en la banda B de Johnson con un cierto sistema se registran para dos estrellas estándar y otra de magnitud desconocida. los resultados que se muestran en la siguiente tabla:

<pre>
Estrella | m$_o$ | F(c)  | Texp (s) | altura |
#1       | 9.2   | 21100 | 60       | 60     |
#2       | 8.9   | 12000 | 30       | 40     |
#3       |       | 14400 | 30       | 60     |
</pre>
Suponiendo que estamos observando en una noche fotométrica, determine el coeficiente de
extinción en B para esa noche y la magnitud de la estrella problema en esa banda.
```
```{admonition} Fotometria CCD Ejercicio 4
La extinción atmosférica en la banda de los 300nm (UV-B, causante del bronceado) es de 6 magnitudes por masa de aire. Determine cuantas magnitudes más brillante se ve una estrella desde fuera de la atmósfera que observada en el cénit desde tierra. Determine el factor entre el flujo del Sol cuando se encuentra a una altura de 30$^o$
y de 60$^o$.
```