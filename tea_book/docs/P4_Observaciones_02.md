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

# Ayudas para preparar la petición de tiempo

## Cartas de identificación
Antes del mundo de internet los observadores usaban cartas de identificación que en principio eran copias de los atlas estelares y más tarde fotos de las exploraciones fotográficas como el Atlas de Monte Palomar (POSS). Esas placas fotográficas se escanearon en su momento para producir el DPOSS o Digital Palomar Observatory Sky Survey. 

Se puede seleccionar una región del cielo y el campo de visión de la carta en plataformas web como [The STScI Digitized Sky Survey](https://stdatu.stsci.edu/cgi-bin/dss_form) que permite descargar en GIF o en FITS. 

```{figure} /_static/lecture_specific/p4_observaciones/p4_1_peticion_16.png
---
width: 400px
name: peticion-16-fig
---
Interfaz de STScI Digitized Sky Survey con parámetros para una carta de 120x120 arcmic centrada en M31. 
```

```{figure} /_static/lecture_specific/p4_observaciones/p4_1_peticion_17.png
---
width: 300px
name: peticion-17-fig
---
Carta de identificación en M31 obtenida con los parámetros de antes.
```

Con las utilidades de la exploración de Sloan [SDSS](https://www.sdss.org/) podemos preparar cartas similares aunque en este caso las observaciones proceden de imágenes digitales directamente. [SDSS Visual Tools](http://skyserver.sdss.org/dr14/en/tools/chart/charthome.aspx)

```{figure} /_static/lecture_specific/p4_observaciones/p4_1_peticion_18.png
---
width: 400px
name: peticion-18-fig
---
Interfaz [SDSS Finding Chart](http://skyserver.sdss.org/dr14/en/tools/chart/navi.aspx) y carta de identificación. 
```

La herramienta de visualización más potente es [aladin](https://aladin.u-strasbg.fr/)

```{figure} /_static/lecture_specific/p4_observaciones/p4_1_peticion_19.png
---
width: 600px
name: peticion-19-fig
---
Aladin puede usarse en el ordenador propio o en la página web.
```

Otros enlaces a utilidades de cartas de identificación

```{figure} /_static/lecture_specific/p4_observaciones/p4_1_peticion_20.png
---
width: 500px
name: peticion-20-fig
---
Carta obtenida con [IRSA IPAC finding chart](https://irsa.ipac.caltech.edu/applications/finderchart/) que proporciona las imágenes FITS en varias bandas de SDSS.
```

## Visibilidad de los observables
Normalmente los observables están definidos por el propio proyecto de investigación. Por ejemplo si queremos obtener la curva de luz del tránsito de un exoplaneta tendremos que elegir un observatorio desde el que la estrella sea visible y tenga la instrumentación adecuada y observar en la fecha y hora del tránsito. Sin estar tabn condicionados algo parecido pasa si queremos observar el Centro Galáctico, por ejemplo. No en todas las fechas es accesible desde todos los observatorios. Entonces en una petición de tiempo tendríamos que solicitar observaciones en determinadas fechas.  
Si lo que tenemos es una muestra que deseamos observar tendremos que agrupar la lista de objetos por coordenadas (Ascensión Recta) de forma que las diferentes sublistas sean observables en las mismas fechas. 


Algunos observatorios disponen de utilidades para determinar la visibilidad de los objetos celestes en distintos observatorios (latitud y longitud) y fecha. Se les proporciona la lista de observables y devuelven un gráfico con las alturas sobre el horizonte (y distancia cenital) a lo largo de la noche. De esta forma podemos elegir mejor la fecha para que mejore la visibilidad y/o descartar alguno de los objetos de la lista. 

- [Object Seasonal Visibility](http://www.briancasey.org/artifacts/astro/observability.cgi)
- [CAHA VES](https://www.caha.es/pedraz/ves.html)
- [ING STARALT Object Visibility](http://catserver.ing.iac.es/staralt/)

Nosotros usaremos el último enlace ``startalt``. Veamos un ejemplo


```{admonition} Visibilidad Ejemplo 1
:class: tip
Determine la mejor fecha para observar esta lista de estrellas en el observatorio de Calar Alto.
<pre>
#HR     Star            RA2000         Dec2000          Sp.Type       
#                       h:m:s           d:m:s           
9087    29 Psc          00 01 49.4      -03 01 39       B7 III-IV    
718     Xi2 Cet         02 28 09.5      08 27 36        B9 III        
1544    Pi2 Ori         04 50 36.7      08 54 01        Al V          
3454    Eta Hya         08 43 13.5      03 23 55        B3 V          
4468    Theta Crt       11 36 40.9      -09 48 08       B9.5 V        
4963    Theta Vir       13 09 57.0      -05 32 20       A1 IV         
5501    108 Vir         14 45 30.2      00 43 02        B9.5 V        
7001    Alpha Lyr       18 36 56.3      38 47 01        AO V          
7596    58 Aql          19 54 44.8      00 16 25        AO II1        
7950    Epsilon Aqr     20 47 40.6      -09 29 45       A1 V          
8634    Zeta Peg        22 41 27.7      10 49 53        B8 V          
</pre>
```

Vemos que están ya ordenadas por ascensión recta y que tenemos estrellas casi para cualquier fecha. La declinación es importante porque aunque busquemos la fecha idónea para una estrella (aquella en que la estrella culmine en mitad de la noche), la altura en el meridiano depende de la latitud del obervatorio y de la declinación del objeto. La aplicación on line se encarga de estos cálculos. Empecemos calculando la mejor fecha para una de las estrellas.

```{figure} /_static/lecture_specific/p4_observaciones/p4_2_visibilidad_1.png
---
width: 600px
name: visibilidad-1-fig
---
Interfaz de STARALT con el observatorio de Calar Alto y fecha de 9 de febrero de 2021 para la estrella 3454 Eta Hya.
```

```{figure} /_static/lecture_specific/p4_observaciones/p4_2_visibilidad_2.png
---
width: 600px
name: visibilidad-2-fig
---
Resultado de STARALT con los parámertros mostrados anteriormente y fecha de 9 de febrero de 2021.
```
Nuestra estrella culmina justo en mitad de la noche y no tenemos Luna. Cuando tenemos una lista podemos escribirla en el recuadro pero es mejor crear un fichero con el formato adecuado y subirlo.

<pre>
HR9087    00 01 49.4      -03 01 39     
HR718     02 28 09.5      +08 27 36    
HR1544    04 50 36.7      +08 54 01    
HR3454    08 43 13.5      +03 23 55    
HR4468    11 36 40.9      -09 48 08    
HR4963    13 09 57.0      -05 32 20    
HR5501    14 45 30.2      +00 43 02   
HR7001    18 36 56.3      +38 47 01   
HR7596    19 54 44.8      +00 16 25   
HR7950    20 47 40.6      -09 29 45   
HR8634    22 41 27.7      +10 49 53 
</pre>

```{figure} /_static/lecture_specific/p4_observaciones/p4_2_visibilidad_3.png
---
width: 600px
name: visibilidad-3-fig
---
Resultado de STARALT para las estrellas de la lista.
```

La línea discontinua vertical marca el final del crepúsculo (Twilight) y por lo tanto el comienzo de la noche astronómica. Las gráficas para cada estrella están numeradas. así vemos que las estrellas #1 y #11 no las podremos observar en esa fecha ya que están poniéndose por el horizonte al empezar la noche. La estrella #2 está a 50$^o$ de altura y bajando luego hay que observarla cuanto antes. Las estrellas #5 a #8 se observan menjor en la segunda parte de la noche ya que deseamos observarlas cuando alcancen mayor altura porque tendrán menor extinción. De las estrellas #9 y #10 nos podemos olvidar en esta fecha.

Cada mes (año/12) que nos movamos en fecha las gráficas se mueven dos horas (dia/12). 


```{figure} /_static/lecture_specific/p4_observaciones/p4_2_visibilidad_4.png
---
width: 600px
name: visibilidad-4-fig
---
Resultado de STARALT para las estrellas de la lista el 9 de marzo de 2021.
```

Y para ver una fecha con Luna elegimos el 23 de marzo.

```{figure} /_static/lecture_specific/p4_observaciones/p4_2_visibilidad_5.png
---
width: 600px
name: visibilidad-5-fig
---
Resultado de STARALT para las estrellas de la lista el 23 de marzo de 2021.
```
La Luna se pone al final de la noche. Tal vez para este proyecto de estrellas brillantes no sea un problema pero sí lo sería para la observación de objetos débiles.

```{admonition} Visibilidad Ejercicio 1
:class: tip
Nos han asignado la noche del 19 de agosto para observar en Calar Alto la galaxia Arp222. Determine las mejores horas de la noche.
```

```{admonition} Visibilidad Ejercicio 2
:class: tip
Determine la fecha que permita observar el mayor número de galaxias de la lista adjunta.
<pre>
Name             H Ura  RA    2000  Dec  Con  Size  Mag  Type      Car Vic
NGC  221  Arp168    60   0 42.7   40 52  And   7.6  9.0  cE2        14   7
NGC  772  Arp078 H 129   1 59.3   19  1  Ari   7.1 11.1  SA(s)bI+  125  22
NGC 1023  Arp135 H  62   2 40.4   39  4  Per   8.7 10.4  SB(rs)0-   55  29
NGC 1241  Arp304   266   3 11.3   -8 55  Eri   3.0 12.7  SAB(rs)bc 206  33
NGC 1569  Arp210    19   4 30.8   64 51  Cam   2.9 11.9  IBm       336  43
NGC 6412  Arp038    12  17 29.6   75 42  Dra   2.3 12.3  SA(s)c    304 179
NGC 6946  Arp029 H  32  20 34.8   60  9  Cep  11.0  9.7  SAB(rs)cd 239 226
NGC 7314  Arp014   347  22 35.8  -26  3  PsA   4.6 11.7  SAB(rs)c: 278 245
NGC 7448  Arp013 H 213  23  0.1   15 59  Peg   2.7 12.2  SA(rs)bc  251 248
NGC 7727  Arp222 H 260  23 39.9  -12 18  Aqr   4.2 11.6  SAB:(s)0p  83 254
</pre>

Advertencia:  
Las coordenadas no necesitan ser muy precisas para estas gráficas pero hay que tener cuidado que entren en el formato requerido por STARALT.
```

En el caso de que nos asignen una fecha de observación (como nos va a ocurrir con las prácticas) necesitaremos adecuar los observables a esa fecha. Por ejemplo si queremos observar galaxias del catálogo de Arp y las fechas de observación son el 16 y 17 de abril en Calar alto, por ejemplo tendremos que elegir las de ascensión recta entorno a RA=13H o circumpolares.

```{admonition} Visibilidad Ejercicio 3
:class: tip
Busque 6 galaxias del catálogo de Arp para observar durante las noches de  16 y 17 de abril de 2021 en Calar Alto. Procure que cubran toda la noche de observación.
[Lista de las 90 galaxias más brillantes del catálogo de Arp](https://arpgalaxy.com/rsaarp.html)  
Otra opción es buscarlas en [NED search by parameters](https://ned.ipac.caltech.edu/byparams)
```



