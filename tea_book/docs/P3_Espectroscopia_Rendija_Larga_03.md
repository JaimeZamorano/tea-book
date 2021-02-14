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
# Correción de BIAS

## Visualización de los BIAS
Repetimos aquí los pasos para crear la lista de BIAS y visualizarlos.

- La lista de BIAS disponibles 
```{code-cell} ipython3
from ccdproc import ImageFileCollection
from ccdproc.utils.sample_directory import sample_directory_with_files
ic_all = ImageFileCollection(directory, keywords='*')
# Only some keywords
keys = ['file','OBJECT' , 'EXPTIME' , 'OBSTYPE' , 'CCDSTEMP', 'CCDATEMP']
ic1 = ImageFileCollection(directory, keywords=keys) 
# Only BIAS files selected by 'imagetyp' keyword. 
bias_images = ic1.files_filtered(regex_match=True,imagetyp='bias|light')
print(bias_images)
````
<pre>
['45228.fits' '45229.fits' '45230.fits' '45231.fits' '45232.fits'
 '45256.fits' '45257.fits' '45258.fits' '45259.fits' '45260.fits']
</pre>

- Leemos esos ficheros de BIAS
```{code-cell} ipython3
image_bias, filename_bias = [], []
for file in bias_images:
    print(file)
    image = CCDData.read(directory+file, unit="adu")
    image_bias.append(image)
    filename_bias.append(file)
````
- Para hacer una estadística localizada  
```{code-cell} ipython3
# auxiliary function to display a rectangle and compute mean value within it
def draw_rectangle(ax, image_data, x1, x2, y1, y2, color, text=False):
    ax.plot((x1, x1), (y1, y2), color, lw=1)
    ax.plot((x2, x2), (y1, y2), color, lw=1)
    ax.plot((x1, x2), (y1, y1), color, lw=1)
    ax.plot((x1, x2), (y2, y2), color, lw=1)
    if text:
        media = image_data[y1:y2,x1:x2].mean()
        std   = image_data[y1:y2,x1:x2].std()
        ax.text((x1+x2)/2, y1+(y2-y1)/8, str(int(media)), 
                ha='center', va='center', color=color, fontsize=15)        
        ax.text((x1+x2)/2, y2-(y2-y1)/8, str(round(std,1)), 
                ha='center', va='top', color=color, fontsize=15)
    return media, std
````
    
- Los visualizamos
```{code-cell} ipython3
mean_bias, std_bias, expo_bias = [], [], []
fig, ax = plt.subplots(ncols=1, nrows=len(bias_images), figsize=(14, 20))
n = 0
for i in range(len(bias_images)):
    img = ax[n].imshow(image_bias[i], cmap='gray', origin='lower',vmin=685,vmax=700)
    ax[n].set_xlabel('X axis')
    ax[n].set_ylabel('Y axis')
    ax[n].text(100,200,filename_bias[i],color='w',fontsize=20)
    divider = make_axes_locatable(ax[n])
    mean,std = draw_rectangle(ax[n], image_bias[i].data , 400, 600, 50, 200, color='w',text=True)
    mean_bias.append(mean)
    std_bias.append(std)
    expo_bias.append(image_bias[i].header['EXPTIME'])
    cax = divider.append_axes("right", size="5%", pad=0.05)
    fig.colorbar(img, cax=cax)
    n = n+1
```

```{figure} /_static/lecture_specific/p3_espectroscopia/p3_3_dark_0.png
---
width: 600px
name: dark-0-fig
---
Imágenes de BIAS. algo raro pasa con 45230.fits
````

```{code-cell} ipython3
# Retiramos el tercer fichero 45230.fits
bias_images = np.delete(bias_images,2)
```

## Corriente de oscuridad
Podemos hacer una estadística de los ficheros de BIAS y DARK para comprobar si tenemos corriente de oscuridad apreciable. Creamos la lista y leemos los DARKs. 

Un análisis muy completo sobre modelización de la corriente de oscuridad se puede leer en [https://mwcraig.github.io/ccd-as-book/03-01-Dark-current-The-ideal-case.html](https://mwcraig.github.io/ccd-as-book/03-01-Dark-current-The-ideal-case.html)
y sobre píxeles calientes en [https://mwcraig.github.io/ccd-as-book/03-02-Real-dark-current-noise-and-other-artifacts.html](https://mwcraig.github.io/ccd-as-book/03-02-Real-dark-current-noise-and-other-artifacts.html). Aquí no vamos a entrar en tantos detalles.


```{code-cell} ipython3
dark_images = ic1.files_filtered(regex_match=True,object='dark')
print(dark_images)
````
<pre>
['45261.fits' '45262.fits' '45263.fits']
</pre>

Leemos las imágenes. En este caso almacenamos la expoción en una lista en este paso. Antes lo hicimos al visualizar los BIAS.

```{code-cell} ipython3
image_dark , filename_dark , expo_dark = [], [], []
for file in dark_images:
    print(directory+file)
    image = CCDData.read(directory+file, unit="adu")
    image_dark.append(image)
    filename_dark.append(file)    
    expo_dark.append(image.header['EXPTIME'])
```
Otra vez aprovechamos el bucle de la visualización para hacer la estadística. Eso nos permite cambiar las posiciones de la zona donde deseamos hacer la estadística.

```{code-cell} ipython3
mean_dark, std_dark = [], []
fig, ax = plt.subplots(ncols=1, nrows=len(dark_images), figsize=(14, 6))
n = 0
for i in range(len(dark_images)):
    img = ax[n].imshow(image_dark[i], cmap='gray', origin='lower',vmin=650,vmax=750)
    ax[n].set_xlabel('X axis')
    ax[n].set_ylabel('Y axis')
    ax[n].text(100,200,filename_dark[i],color='w',fontsize=20)
    ax[n].text(900,200,str(expo_dark[i])+' s',color='w',fontsize=20)
    divider = make_axes_locatable(ax[n])
    draw_rectangle(ax[n], image_dark[i].data , 700, 850, 50, 200, color='w',text=True)
    mean,std = draw_rectangle(ax[n], image_dark[i].data , 80, 160, 10, 100, color='w',text=True)
    mean_dark.append(mean)
    std_dark.append(std)
    cax = divider.append_axes("right", size="5%", pad=0.05)
    fig.colorbar(img, cax=cax)
    n = n+1
```

```{figure} /_static/lecture_specific/p3_espectroscopia/p3_3_dark_1.png
---
width: 600px
name: dark-1-fig
---
Imágenes de DARK.
````

El resultado informa de que hay corriente de oscuridad que podemos ver en una gráfica sencilla.

```{code-cell} ipython3
fig, ax = plt.subplots(ncols=1, nrows=1, figsize=(9, 4))
ax.plot(expo_bias,mean_bias,'bo')
ax.plot(expo_dark,mean_dark,'ro')
ax.set_xlabel('texpo [s]')
ax.set_ylabel('Mean [counts]')
````

```{figure} /_static/lecture_specific/p3_espectroscopia/p3_3_dark_6.png
---
width: 600px
name: dark-6-fig
---
Estadística de los BIAS y DARKs.
````

Sin necesidad de ajuste vemos hay una diferencia de  700-686 = 14 cuentas en 900 segundos. No es mucho pero nuestras imágenes de ciencia tienen 600s y 900s.

Al combinar imágenes de BIAS pretendemos reducir el ruido inherente a cada BIAS de forma que el ruido del super BIAS sea menor que el de un sólo BIAS.


```{figure} /_static/lecture_specific/p3_espectroscopia/p3_3_dark_5.png
---
width: 600px
name: dark-5-fig
---
Comparación de dos BIAS (principio y fin de la noche) y del super_BIAS resultado de la combinación de todos los BIAS válidos disponibles. No parece que haya una estructura en el super_BIAS que presenta un aspecto uniforme.
````

## Sustracción del overscan
Un análisis detallado de cuando conviene retirar el overscan se puede encontrar en [https://mwcraig.github.io/ccd-as-book/01-08-Overscan.html](https://mwcraig.github.io/ccd-as-book/01-08-Overscan.html). En la práctica 2 de Fotometría CCD no lo usamos pero en este caso puede ser necesario.

Recordemos que la señal térmica (dark) se registra en el overscan junto al BIAS. Es decir que el overscan de cada imagen es una buena representación de la señal que debemos retirar para quedarnos con la parte lineal. El overscan entonces incluye BIAS, dark y ruido de lectura. En la substracción simple del overscan se realiza un promedio que reduce el ruido. Si hay estructura en el BIAS el asunto se complica porque deberíamos ajustar un modelo.


```{figure} /_static/lecture_specific/p3_espectroscopia/p3_3_dark_3.png
---
width: 600px
name: dark-3-fig
---
Imagen de ciencia donde se aprecia overscan.
````

```{figure} /_static/lecture_specific/p3_espectroscopia/p3_3_dark_2.png
---
width: 600px
name: dark-2-fig
---
Información para la localización del overscan y underscan.
````
**Opciones**

Casos posibles en el supuesto de existencia de corriente de oscuridad medible (de lo ideal a lo real)

- a) Tenemos múltiples imágenes de DARK del mismo tiempo de exposición que nuestras imágenes de ciencia.  
Estupendo, promediamos y se la restamos.
- b) Tenemos overscan.  
En cada imagen ya tenemos el nivel de cuentas que hay que retirar. La corrección de overscan nos retirará el BIAS y DARK en una sólo operación.
- c) Sólo tenemos una cuantas imágenes de DARK de diferentes tiempos de exposición.  
Restar el único DARK de tiempo de exposición como el de la imagen de ciencia no es una opción porque esa imagen de DARK tiene ruido de lectura (y rayos cósmicos). Añadiríamos ruido a nuestra imagen de ciencia.  
Pero podemos estimar el ruido térmico (cuentas/s/pixel) y restar un superBIAS + contribución de dark escalada al tiempo de exposición. Para una exposición de 400s podríamos fabricar un DARK = superBIAS + 400 s $\times$ 14c/900s 

En el super_BIAS mostrado en la sección anterior se notan diferencias de señal entre los primeros BIAS y los del final de la noche como ya habíamos visto en la primera visualización de los datos.

**Resumiendo:**   
En nuestro caso tenemos corriente de oscuridad. No es excesiva pero sí medible. En condiciones normales nos hubíeramos preocupado en observar series de DARKs del mismo tiempo de exposición de nuestros objetos (600s y 900s) para combinarlos y mejorar la estadística y evitar los rayos cósmicos. El proyecto científico no exige una calibración fotométrica ya que estamos interesados en medir velocidades radiales a partir de las líneas espectrales. Por lo tanto nuestra correción de corriente de oscuridad es la que logramos restando la señal media del overscan.

## Master BIAS
En este caso el proceso correcto para crear un master_BIAS es:

- Corregir los BIAS de overscan
- Recortar los BIAS
- Combinar ahora las imágenes de BIAS


Usaremos ``subtract_overscan``  [(información)](https://ccdproc.readthedocs.io/en/latest/api/ccdproc.subtract_overscan.html) donde le debemos pasar la zona del chip donde está el overscan. En este caso las columnas primeras hasta la 50.


```{code-cell} ipython3
import ccdproc
from ccdproc import CCDData, Combiner, subtract_overscan
```
```{code-cell} ipython3
image_bias_no_scan = []
for i in range(len(bias_images)):
    no_scan = subtract_overscan(image_bias[i], overscan=image_bias[i][:, 0:50])
    image_bias_no_scan.append(no_scan)
```
Combinamos, promediamos,
```{code-cell} ipython3
combiner = Combiner(image_bias_no_scan)
master_BIAS = combiner.median_combine()
````

Recortamos el master_BIAS (trimming) pero usando comandos simples de Python. 

```{code-cell} ipython3
master_BIAS_N1 = master_BIAS[:,50:-50]
print(master_BIAS.shape, master_BIAS_N1.shape)
````
<pre>
(250, 1124) (250, 1024)
</pre>
donde hemos retirado las 50 primeras y 50 últimas columnas quedándonos con la zona útil. 

```{code-cell} ipython3
fig, ax = plt.subplots(ncols=1, nrows=3, figsize=(14, 9))
img = ax[0].imshow(image_bias[0], cmap='gray', origin='lower',vmin=685,vmax=700)
ax[0].text(100,200,filename_bias[0],color='k',fontsize=20)
draw_rectangle(ax[0], image_bias[0].data , 400, 600, 30, 170, color='w',text=True)
img = ax[1].imshow(image_bias[-1], cmap='gray', origin='lower',vmin=685,vmax=700)
ax[1].text(100,200,filename_bias[-1],color='w',fontsize=20)
draw_rectangle(ax[1], image_bias[-1].data , 400, 600, 30, 170, color='w',text=True)
img = ax[2].imshow(master_BIAS_N1, cmap='gray', origin='lower',vmin=0,vmax=5)
ax[2].text(100,200,'master_BIAS_N1',color='w',fontsize=20)
for n in range(6):
    draw_rectangle(ax[2], master_BIAS.data , 50+n*150, 200+n*150, 30, 170, color='w',text=True)
````

```{figure} /_static/lecture_specific/p3_espectroscopia/p3_3_dark_7.png
---
width: 600px
name: dark-7-fig
---
BIAS individuales y master_BIAS de la primera noche.
````

Vemos que el ruido ha disminuido en un factor $1/\sqrt(n)$ es decir con $n=7$ $1/2.65$  y pasamos de 6.9 a 2.8 cuentas.

```{code-cell} ipython3
fig, ax = plt.subplots(ncols=1, nrows=1, figsize=(10,4))
step = 2
bins = np.arange(670,720,step)
h_single = ax.hist(image_bias[0].data.flatten(), bins=bins, density=True, 
             label="single BIAS", alpha=0.4);
# Adding mean value of sinle BIAS to compare histograms
h_master = ax.hist(master_BIAS_N1.data.flatten()+693, bins=bins, density=True, 
             label="master_BIAS_N1", alpha=0.4);
ax.legend()
ax.grid()
#print(h_single)

# To fit a gaussian
from scipy.optimize import curve_fit      
xx = np.arange(670,720,0.2)
# Gaussian function definition
def gaus(x,a,x0,sigma):
    return a*np.exp(-(x-x0)**2/(2*sigma**2))
# Transforming from bins edges to bins' centers
x = np.add(h_single[1][:-1],step/2.)
# values obtained from histogram
y = h_single[0]
# Fitting  (with some initial guesses)
popt,pcov = curve_fit(gaus,x,y,p0=[1,690,10])
# From results we can get center and width 
sigma = popt[2]
# Plotting the fit 
y_new = gaus(xx,*popt)
plt.plot(xx,y_new,lw=2,label="norm sigma = "+str(round(sigma,1)))
# again for the master_BIAS
y = h_master[0]
popt,pcov = curve_fit(gaus,x,y,p0=[1,690,10])
sigma = popt[2]
y_new = gaus(xx,*popt)
plt.plot(xx,y_new,lw=2,label="norm sigma = "+str(round(sigma,1)))

ax.legend()
````


```{figure} /_static/lecture_specific/p3_espectroscopia/p3_3_dark_8.png
---
width: 800px
name: dark-8-fig
---
Histogramas comparados de un sólo BIAS y del master_BIAS
````

## Comprobando la corrección de BIAS
Por seguridad vamos a comprobar la corrección de BIAS en una imagen de ciencia para comprobar que no inroduce más ruido del que tenemos. Las imágenes de ciencia tienen muy pocas cuentas salvo en la zona donde se encuentran los espectros de las galaxias. Usaremos la imagen #21 45246.fits 600.0     U1256+2722 PA=315 171.


```{code-cell} ipython3
image_science = CCDData.read(directory+'45246.fits', unit="adu")
```

```{code-cell} ipython3
fig, ax = plt.subplots(ncols=1, nrows=1, figsize=(14,6))
img = ax.imshow(image_science, cmap='viridis', origin='lower',vmin=680,vmax=740)
ax.set_xlabel('X axis')
ax.set_ylabel('Y axis')
ax.text(150,200,image_science.header['object'],color='w',fontsize=20)
divider = make_axes_locatable(ax)
cax = divider.append_axes("right", size="5%", pad=0.05)
fig.colorbar(img, cax=cax)
draw_rectangle(ax, image_science.data , 720, 820, 110, 210, color='w',text=True)
draw_rectangle(ax, image_science.data , 0, 50, 40, 240, color='w',text=True)
```

```{figure} /_static/lecture_specific/p3_espectroscopia/p3_4_bias_1.png
---
width: 800px
name: bias-1-fig
---
Aspecto de la imagen de ciencia original. Es el espectro de la galaxia UCM1256+2722.
````

```{code-cell} ipython3
science_no_scan = subtract_overscan(image_science, overscan=image_science[:, 0:50])
````

```{figure} /_static/lecture_specific/p3_espectroscopia/p3_4_bias_2.png
---
width: 800px
name: bias-2-fig
---
Aspecto de la imagen de ciencia corregida de overscan.
````

```{code-cell} ipython3
t_science_image = ccdproc.trim_image(science_no_scan[:,50:-50])
````

```{code-cell} ipython3
bias_subtracted_science = ccdproc.subtract_bias(t_science_image, master_BIAS_N1)
````


```{code-cell} ipython3
fig, ax = plt.subplots(ncols=1, nrows=2, figsize=(14, 6))
img = ax[0].imshow(science_no_scan, cmap='viridis', origin='lower',vmin=0,vmax=40)
draw_rectangle(ax[0], science_no_scan.data , 720, 820, 110, 210, color='w',text=True)
draw_rectangle(ax[0], science_no_scan.data , 350, 450, 150, 210, color='w',text=True)

img = ax[1].imshow(bias_subtracted_science, cmap='viridis', origin='lower',vmin=0,vmax=40)
draw_rectangle(ax[1], bias_subtracted_science.data, 670, 770, 110, 210, color='w',text=True)
draw_rectangle(ax[1], bias_subtracted_science.data, 300, 400, 150, 210, color='w',text=True)
```

```{figure} /_static/lecture_specific/p3_espectroscopia/p3_4_bias_3.png
---
width: 800px
name: bias-3-fig
---
Aspecto de la imagen de ciencia corregida de overscan (arriba) y luego además de BIAS (abajo).
````

```{code-cell} ipython3
fig, ax = plt.subplots(ncols=1, nrows=1, figsize=(10,4))
bins=np.arange(-30,30,2)
ax.hist(science_no_scan.data.flatten(), bins=bins, align='mid', density=True, 
             label="original", alpha=0.4);
ax.hist(bias_subtracted_science.data.flatten(), bins=bins, align='mid', density=True, 
             label="BIAS subtracted", alpha=0.4);
ax.legend()
```

```{figure} /_static/lecture_specific/p3_espectroscopia/p3_4_bias_4.png
---
width: 600px
name: bias-4-fig
---
Histogramas parecidos, sólo un poco desplazados.
````
