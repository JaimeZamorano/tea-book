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
# Primeros pasos

## Lectura de las observaciones
Usamos la misma estrategia que en la práctica de Fotometría de imagen CCD.  
Las cabeceras de los ficheros FITS son bastante escuetas.

<pre>
SIMPLE   = True
BITPIX   = 16
NAXIS    = 2
NAXIS1   = 1124
NAXIS2   = 250
EXTEND   = False
BSCALE   = 1.0
BZERO    = 32768.0
ORIGIN   = NOAO-IRAF FITS Image Kernel July 1999
DATE     = 2000-10-27T12:00:38
IRAF-TLM = 11:36:29 (27/10/2000)
RUN      = 45262
OBJECT   = dark 900
OBSTYPE  = DARK
IMAGETYP = DARK
DETECTOR = TEK3
PREFLASH = 0.0
BUINT    = ADU
GAIN     = 1.28
READNOIS = 5.24
CCDSPEED = STANDARD
CCDNCHIP = 1
CCDCHIP  = TEK3
CCDTYPE  = TEK1024AR
CCDXSIZE = 1124
CCDYSIZE = 1124
CCDXIMSI = 1024
CCDYIMSI = 1024
CCDXIMST = 50
CCDYIMST = 0
CCDWMODE = True
CCDWXO1  = 0
CCDWYO1  = 487
CCDWXS1  = 1124
CCDWYS1  = 250
CCDWXO2  = 0
...
CCDWYS4  = 0
CCDXPIXE = 2.4e-05
CCDYPIXE = 2.4e-05
CCDXBIN  = 1
CCDYBIN  = 1
CCDSTEMP = 173
CCDATEMP = 171
UTSTART  = 04:29:48.0
ELAPSED  = 901.0
DARKTIME = 900.0
EXPOSED  = 900.0
EXPTIME  = 900.0
COMMENT  = 
</pre>

Podemos hacer un listado de las imágenes disponibles la primera noche,


```{code-cell} ipython3
for i in range(len(filelist)):
    file = filelist[i]
    HDUList_object = fits.open(file)
    primary_HDU = HDUList_object[0]
    header = primary_HDU.header
    print(i,str(file)[37:],header['EXPTIME'],'   ',header['OBJECT'])
```

Y aparecen los ficheros y su contenido que coincide con lo que tenemos en el cuaderno de observaciones,

<pre>
 0 45225.fits  10.0     sky flat
 1 45226.fits 300.0     sky flat
 2 45227.fits 300.0     sky flat 300
 3 45228.fits   0.0     bias
 4 45229.fits   0.0     bias
 5 45230.fits   0.0     bias
 6 45231.fits   0.0     bias
 7 45232.fits   0.0     bias
 8 45233.fits 600.0     UCM1253+2756 PA=18
 9 45234.fits 600.0     UCM1247+2701 PA=50
10 45235.fits 600.0     UCM1247+2701 PA=50
11 45236.fits 900.0     UCM1254+2802 PA=25
12 45237.fits 900.0     UCM1254+2802 PA=25
13 45238.fits 900.0     UCM1255+2734 PA=14
14 45239.fits 900.0     UCM1255+2819 PA=27
15 45240.fits   1.0     arc CUNE 1s
16 45241.fits 600.0     UCM1255+3125 PA=18
17 45242.fits   1.0     CUNE 1s cenwave 67
18 45243.fits 600.0     U1256+2701 PA=300
19 45244.fits 600.0     U1256+2701 PA=300
20 45245.fits 600.0     U1256+2722 PA=330
21 45246.fits 600.0     U1256+2722 PA=315
22 45247.fits 600.0     U1256+2732 PA=0 (1
23 45248.fits 600.0     U1256+2732 PA=0 (2
24 45249.fits 600.0     U1428+2727 PA=60 (
25 45250.fits 900.0     U1429+2645 PA=0 (1
26 45251.fits 900.0     U1430+2947 PA=90 (
27 45252.fits 900.0     U1431+2702 PA=0 (1
28 45253.fits   1.0     arc 1 CUNE 1s
29 45254.fits   1.0     arc 1 CUNE 1s
30 45255.fits   1.0     arc 1 CUNE 1s
31 45256.fits   0.0     BIAS
32 45257.fits   0.0     BIAS
33 45258.fits   0.0     BIAS
34 45259.fits   0.0     BIAS
35 45260.fits   0.0     BIAS
36 45261.fits  30.0     dark 30
37 45262.fits 900.0     dark 900
38 45263.fits 600.0     dark 600
39 45264.fits   5.0     flat tung 5s
40 45265.fits   2.0     flat tung 2s
41 45266.fits   2.0     flat tung 2s
42 45267.fits   2.0     flat tung 2s
43 45268.fits   2.0     flat tung 2s
44 45269.fits   2.0     flat tung 2s
</pre>

Tenemos observaciones 
- Sky Flats   (0-2)
- BIAS        (3-7) y (31-35)
- Ciencia
- Arcos       (28-30) por ejemplo
- DARKs       (36-38)
- Lamp Flats  (39-44) 


## Visualización
Podemos visualizarlos con ``DS9`` o usar Python sabiendo por la lista anterior qué ficheros debemos enseñar. Leemos todos los ficheros por comodidad.

```{code-cell} ipython3
image_data, header , filename = [] , [], []
for i in range(len(filelist)):
    file = filelist[i]
    HDUList_object = fits.open(file)
    primary_HDU = HDUList_object[0]
    header.append(primary_HDU.header)
    image_data.append(primary_HDU.data)
    filename.append(str(file)[37:])
    HDUList_object.close()
```

Y mostramos algunos de ellos.  Por ejemplo los arcos de calibración en longitud de onda (ficheros 28 a 30)

```{code-cell} ipython3
fig, ax = plt.subplots(ncols=1, nrows=3, figsize=(10, 8))
n = 0
for i in range(28,31):
    img = ax[n].imshow(image_data[i], cmap='gray', origin='lower',vmin=600,vmax=1000)
    ax[n].set_xlabel('X axis')
    ax[n].set_ylabel('Y axis')
    ax[n].text(150,200,filename[i],color='w',fontsize=20)
    ax[n].text(540,200,round(np.mean(image_data[i][100:200,600:800]),2),color='w',fontsize=20)
    divider = make_axes_locatable(ax[n])
    cax = divider.append_axes("right", size="5%", pad=0.05)
    fig.colorbar(img, cax=cax)
    n = n+1
```
```{figure} /_static/lecture_specific/p3_espectroscopia/p3_2_visu_1.png
---
width: 800px
name: visu-1-fig
---
Imágenes tres arcos de calibración. 
```


```{figure} /_static/lecture_specific/p3_espectroscopia/p3_2_visu_4.png
---
width: 600px
name: visu-4-fig
---
Imágenes de algunos BIAS al principio de la noche. Se muestra una estadística en el centro de la imagen. El tercer BIAS tiene algo raro en la parte baja. Hay que descartarlo.
```
```{figure} /_static/lecture_specific/p3_espectroscopia/p3_2_visu_3.png
---
width: 600px
name: visu-3-fig
---
Imágenes de algunos BIAS al final de la noche.
```
Se nota una pequeña diferencia entre la señal de BIAS al principio y final de la noche. Si se mira la cabecera de las imágenes se aprecia que la temperatura del chip es un poco diferente (173 frente a 171).


```{figure} /_static/lecture_specific/p3_espectroscopia/p3_2_visu_2.png
---
width: 800px
name: visu-2-fig
---
FLAT de cielo. Se aprecian líneas del cielo. También las zonas de overscan y underscan a ambos lados.
```

```{figure} /_static/lecture_specific/p3_espectroscopia/p3_2_visu_5.png
---
width: 800px
name: visu-5-fig
---
FLAT de lámpara de tungsteno. Como el espectro es continuo no aparecen las líneas espectrales del FLAT de cielo. 
```
En los dos FLATs se aprecian unas líneas horizontales que resultan de pequeñas imperfecciones o, más plausible, polvo en alguna parte de la rendija. 


```{figure} /_static/lecture_specific/p3_espectroscopia/p3_2_visu_6.png
---
width: 800px
name: visu-6-fig
---
Dos de los espectros de galaxias. 
````
La línea más brillante es H$\alpha$, escoltada por [NII]$\lambda$6548 y [NII]$\lambda$ 6584 Angstroms (3 veces más brillante, a la izquierda). Es decir que la longitud de onda crece hacia la izquierda. Para la galaxia de abajo se puede ver el doblete de [SII]$\lambda$6720 mientras que en la galaxia de arriba (con mayor desplazamiento al rojo) ese doblete ha salido fuera del intervalo espectral. Es interesante notar que el número de impactos de rayos cósmicos es mayor al ser exposiciones largas.

## Listas

**Método 1**

Ya hemos aprendido a seleccionar imágenes por tipo u otras categorías. Se basa en usar ``ImageFileCollection`` de ``ccdproc`` que nos crea una tabla de astropy. Preguntemos por los keywords disponibles en la cabecera de los ficheros.

```{code-cell} ipython3
from ccdproc import ImageFileCollection
from ccdproc.utils.sample_directory import sample_directory_with_files
ic_all = ImageFileCollection(directory, keywords='*')
ic_all.summary['file', 'imagetyp', 'exptime', 'naxis1', 'naxis2']
````

```{figure} /_static/lecture_specific/p3_espectroscopia/p3_2_visu_7.png
---
width: 400px
name: visu-7-fig
---
Salida de ic_all.summary['file', 'imagetyp', 'exptime', 'naxis1', 'naxis2'] 
````

```{code-cell} ipython3
print(ic_all.summary.colnames)
````

<pre>
['file', 'simple', 'bitpix', 'naxis', 'naxis1', 'naxis2', 'extend', 
'bscale', 'bzero', 'origin', 'date', 'iraf-tlm', 'run', 
'object', 'obstype', 'imagetyp', 'observer', 'telescop', 'instrume', 
'ra', 'dec', 'equinox', 'zendist', 'airmass', 'date-obs', 'agtvposx', 
'agtvposy', 'agtvfilt', 'agposx', 'agposy', 'agndflnr', 'agndfpos', 
'agcolfil', 'agcolflp', 'flipmirr', 'agarcid', 'fieldang', 'guidfoc', 
'slitwdin', 'slitangl', 'pslitwdt', 'dekslide', 'dekslidp', 'filtslid', 
'filternr', 'ndfiltnr', 'ndfilsn', 'collcoat', 'collpos', 'hartmanr', 
'hartmanl', 'camera', 'gratname', 'gratrule', 'gratangl', 'gratshut', 
'cenwave', 'detector', 'preflash', 'buint', 'gain', 'readnois', 
'ccdspeed', 'ccdnchip', 'ccdchip', 'ccdtype', 'ccdxsize', 'ccdysize', 
'ccdximsi', 'ccdyimsi', 'ccdximst', 'ccdyimst', 'ccdwmode', 'ccdwxo1', 
'ccdwyo1', 'ccdwxs1', 'ccdwys1', 'ccdwxo2', 'ccdwyo2', 'ccdwxs2', 
'ccdwys2', 'ccdwxo3', 'ccdwyo3', 'ccdwxs3', 'ccdwys3', 'ccdwxo4', 
'ccdwyo4', 'ccdwxs4', 'ccdwys4', 'ccdxpixe', 'ccdypixe', 'ccdxbin', 
'ccdybin', 'ccdstemp', 'ccdatemp', 'utstart', 'elapsed', 'darktime', 
'exposed', 'exptime', 'comment']
</pre>

Seleccionamos los 5 que necesitaremos.
```{code-cell} ipython3
keys = ['file','OBJECT' , 'EXPTIME' , 'OBSTYPE', 'CCDATEMP']
ic1 = ImageFileCollection(directory, keywords=keys) # only keep track of keys
ic1.summary.colnames
```

<pre>['file', 'OBJECT', 'EXPTIME', 'OBSTYPE', 'CCDATEMP']</pre>

Por ejemplo para BIAS tenemos
```{code-cell} ipython3
matches = (ic1.summary['OBSTYPE'] == 'BIAS') 
bias_list = ic1.summary['file'][matches]
for i in range(len(bias_list)):
    HDUList_object = fits.open(directory+bias_list[i])
    primary_HDU = HDUList_object[0]
    header = primary_HDU.header
    print('BIAS',i,bias_list[i],header['EXPTIME'],header['CCDATEMP'])
````

<pre>
BIAS 0 45228.fits 0.0 173
BIAS 1 45229.fits 0.0 173
BIAS 2 45230.fits 0.0 173
BIAS 3 45231.fits 0.0 173
BIAS 4 45232.fits 0.0 173
BIAS 5 45256.fits 0.0 171
BIAS 6 45257.fits 0.0 172
BIAS 7 45258.fits 0.0 171
BIAS 8 45259.fits 0.0 171
BIAS 9 45260.fits 0.0 171
</pre>
Donde vemos que efectivamente los BIAS al final de la noche se tomaron con el chip un poco más frío.
<pre>
CCDSTEMP=       173          / Required temperature of CCD, in Kevlin
CCDATEMP=       171          / Actual temperature of CCD, in Kevlin  
</pre>

**Método 2**  
Este segundo método es más fácil todavía.

```{code-cell} ipython3
bias_images = ic1.files_filtered(regex_match=True,imagetyp='bias|light')
print(bias_images)
```
<pre>
['45228.fits' '45229.fits' '45230.fits' '45231.fits' '45232.fits'
 '45256.fits' '45257.fits' '45258.fits' '45259.fits' '45260.fits']
</pre>
Como no queremos usar 45230.fits que en la visualización hemos visto que es raro,

```{code-cell} ipython3
bias_images = np.delete(bias_images,2)
print(bias_images)
````
<pre>
['45228.fits' '45229.fits' '45231.fits' '45232.fits' '45256.fits'
 '45257.fits' '45258.fits' '45259.fits' '45260.fits']
</pre>

```{code-cell} ipython3
flat_images = ic1.files_filtered(regex_match=True,object='flat')
print(flat_images)
````
<pre>
['45225.fits' '45226.fits' '45227.fits' '45264.fits' '45265.fits'
 '45266.fits' '45267.fits' '45268.fits' '45269.fits']
</pre>

```{code-cell} ipython3
dark_images = ic1.files_filtered(regex_match=True,object='dark')
print(dark_images)
````
<pre>
['45261.fits' '45262.fits' '45263.fits']
</pre>

