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

#  Relación Señal Ruido

## La llegada de fotones a un detector
Utilizando un array de pequeñas dimensiones, se simula la llegada poissoniana de fotones a un detector y la posterior conversión analógica-digital. Esta simulación permite ilustrar conceptos básicos de relación señal/ruido y propagación de incertidumbres.  
**Pendiente de hacer** pero se puede consultar [CCD data reduction guide](https://mwcraig.github.io/ccd-as-book/01-00-Understanding-an-astronomical-CCD-image.html) written by Matt Craig and Lauren Chambers. Editing was done by Lauren Glattly.

## PSF 
Cuando se observa un objeto celeste, pongamos una estrella por ejemplo, con un CCD se forma la imagen de la estrella ocupando varios píxeles. La forma de la imagen es circular si está bien enfocada y el seguimiento del telescopio ha funcionado. Una sección a través de esa imagen nos muestra la luz se reparte en una gaussiana bidimensional. 

Podemos considerar a esa imagen la PSF (point spread function). Su anchura a media altura (FWHM, full width at half maximum) nos informa de la resolución espacial de nuestro sistema. La FWHM de la PSF (point spread function) se puede transformar de píxeles a arcsec con ayuda de la la escala de placa (arcsec/pixel). Buscamos en nuestra observación que la PSF sea pequeña y eso depende no solo del enfoque y seguimiento, sino también del seeing en el momento de la observación.

```{figure} /_static/lecture_specific/p1_intro/intro_06_SN_1.png
---
width: 800px
name: SN_1-fig
---
Sucesivos zoom sobre una imagen para seleccionar una imagen de estrella no saturada.
```
```{figure} /_static/lecture_specific/p1_intro/intro_06_SN_2.png
---
width: 300px
name: SN_2-fig
---
Detalle de la estrella cuya imagen que estamos analizando.
```

```{figure} /_static/lecture_specific/p1_intro/intro_06_SN_3.png
---
width: 300px
name: SN_3-fig
---
Haremos un corte a la imagen en esa zona marcada.
```

```{figure} /_static/lecture_specific/p1_intro/intro_06_SN_4.png
---
width: 300px
name: SN_4-fig
---
Corte transversal en la imagen.
```

```{figure} /_static/lecture_specific/p1_intro/intro_06_SN_5.png
---
width: 500px
name: SN_5-fig
---
Ajuste de una gaussiana más un continuo.
```
El ajuste gaussiano proporciona como resultado 
<pre>
continuo         = 4717.3
posición central = 22.12
sigma            = 2.26
</pre>
Como para esta configuración la escala de placa es de 0.2138 arcsec/pixel (para  pixel = 15µm),

$\sigma = 2.26\;pixel = 2.26 \times 0.2138\;arsec/pixel = 0.483\;arcsec $
$FWHM = 2.355 \times 0.483 = 1.138\;arcsec$

```{figure} /_static/lecture_specific/p1_intro/intro_06_SN_6.png
---
width: 500px
name: SN_6-fig
---
Corte transversal en escala de arcsec.
```

## Señal y ruido
Las medidas van a compañadas de ruido inevitablemente. El cociente entre la señal y el ruido se conoce como relación señal a ruido o señal-ruido (SNR). Veamos la señal y las fuentes de ruido. Véase [Stuart Littlefair course](http://slittlefair.staff.shef.ac.uk/teaching/phy241/lectures/L09/index.html)

Supongamos que la señal detectada por un CCD (en fotoelectrones) se $S_o$ para una estrella. De igual manera tenemos $S_s$ y $S_d$ el número total de fotoelectrones del fondo de cielo y de la corriente de oscuridad respectivamente. Esa medida se debe hacer en un área equivalente al de la medida del flujo de la estrella. El ruido correspondiente,

- $\sqrt{S_o}$  
El Ruido fotónico (shot-noise en inglés) es inherente a la llegada de fotones al detector (photon counting) que no es uniforme sino que sigue una estadística de Poisson. Eso quiere decir que contamos el flujo de fotones por intervalos de tiempo y lo representamos tendremos una distribución de Poisson. Si el número total de fotoelectrones detectado es $S_o$ entonces el ruido fotónico es $\sqrt{S_o}$.
- $\sqrt{S_s}$  
El ruido correspondiente a la medida del fondo de cielo $S_s$ se determina de la misma manera  $\sqrt{S_s}$.
- $\sqrt{S_d}$  
La corriente de oscuridad $S_s$ introduce otro ruido fotónico $\sqrt{S_d}$.
- $R$  
El ruido de lectura del CCD es independiente del tiempo. El ruido es la desviación estándard en la medida del número de electrones.   

Si las fuentes de ruido son independientes podemos calcular el ruido total (que produce incertidumbre y degrada la relación señal-ruido) sumando cuadráticamente cada uno de ellos.
Tendremos entonces (asumiendo que la varianza total es la suma de las varianzas),

$$ N = \sqrt{S_o + S_s + S_d + R^2} $$

$$ SNR = S_o/N = \frac{S_o}{\sqrt{S_o + S_s + S_d + R^2}} $$

En realidad las medidas que hacemos en las imágenes está en cuentas (ADUs). La relación entre fotoelectrones y cuentas es la ganancia $g$. Si el flujo recibido de la fuente es $F_o$ (en ADU/s), fondo de cielo $F_s$ (en ADU/s/pixel) y la corriente de oscuridad $F_d$ (en electrones/s/pixel) entonces la señal en electrones es, 

$$ S_o = F_o\;g\;t  \;(en\;electrones) \quad ; \quad S_s = F_s\;g\;t\;n_p \quad ; \quad S_s = F_d\;t\;n_p$$

donde $g$ es la ganacia (en electrones/ADU), $t$ es el tiempo de exposición y $n_p$ es el número de píxeles de la abertura donde medimos el flujo de la estrella.

Finalmente,

$$ SNR =  \frac{F_o\;g\;t}{\sqrt{F_o\;g\;t+ F_s\;g\;t\;n_p + F_d\;t\;n_p + R^2\; n_p}} $$

$$ SNR =  \frac{F_o\;\sqrt{g\;t}}{\sqrt{F_o\;+ n_p (F_s\;+ F_d/g + R^2/g\;t)}} $$

Para observaciones en las que se consigue suficiente señal se dice que estamos limitados por el objeto ('object limited') y en ese caso $S_o$ es mucho mayor que $S_s$, $S_d$ y $R^2$. La ecuación:  $SNR = S_o/\sqrt{S_o + S_s + S_d + R^2}$ se convierte en $SNR = S_o/\sqrt{S_o}$ ó $SNR = \sqrt{S_o}$.

Cuando la señal es muy pobre o el fondo de cielo es alto estamos en el caso limitados por fondo ('background limited or Sky limited') y entonces  $SNR = S_o/\sqrt{S_s}$. Perdemos SNR cuando el fondo crece así que es mejor observar objetos débiles en noches sin luna.

Si el ruido de lectura es muy alto en comparación a la señal, fondo y corriente de oscuridad  estamos en la situacion en la que esta es la principal fuente de ruido ('read noise limited').  Ahora $SNR = S_o/R$. Recordemos que este parámetro $R$ sólo depende del detector. Estamos en este régimen cuando las exposiciones son demasiado cortas.

```{admonition} Apuntes sobre SNR
:class: tip
Puedes leer las notas del profesor Bolte (ucolick)  
[Signal-to-Noise in Optical Astronomy](http://www.ucolick.org/~bolte/AY257/s_n.pdf)
```

## Fotometría de estrellas
El flujo neto de la estrella se obtiene restando la contribución del cielo ('sky background').

```{figure} /_static/lecture_specific/p1_intro/intro_06_SN_7.png
---
width: 500px
name: SN_7-fig
---
Fotometría de una estrella registrada con un CCD.
```

Supongamos (por simplicidad) que es despreciable el ruido de dark. En ese caso si el flujo recibido de la fuente es $F_o$ (en ADU/s), fondo de cielo $F_s$ (en ADU/s/pixel) y la corriente de oscuridad $F_d$ (en electrones/s/pixel) entonces la relación señal-ruido es,  

$$ SNR =  \frac{F_o\;\sqrt{g\;t}}{\sqrt{F_o\;+ n_p (F_s+ R^2/g\;t)}} $$

donde $g$ es la ganacia (en electrones/ADU), $t$ es el tiempo de exposición y $n_p$ es el número de píxeles de la abertura donde medimos el flujo de la estrella.

Entonces mejorará la SNR en noches oscuras ($F_s$ pequeño, menor brillo de cielo) y con buen 'seeing' ya que la imagen de la estrella ocupará menos píxeles y podremos usar una abertura más pequeña.

