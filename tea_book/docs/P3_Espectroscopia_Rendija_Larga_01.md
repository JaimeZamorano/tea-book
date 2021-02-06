# Práctica 3 Espectroscopía de Rendija Larga

## Campaña de observación
Esta práctica se concentra en la reducción de una campaña de observación de espectroscopía
de rendija larga realizada con el instrumento IDS del Isaac Newton Telescope (La Palma) en
mayo de 1997. Se trata en este caso del proyecto La función de masa de
las galaxias con formación estelar (PI y observadores: Jesús Gallego & Jaime Zamorano). 

Los estudiantes deben seguir un procedimiento parecido al de la Práctica 2 (no repetimos aquí el esquema), con la diferencia de que en este caso:
- La combinación de imágenes de BIAS debe tener en cuenta diferencias de temperatura en
el detector a la hora de la toma de las calibraciones.
- Al tratarse de unas observaciones antiguas, el detector utilizado presenta corriente de
oscuridad y es necesario procesar imágenes de DARK.
- Las imágenes de calibración incluyen observaciones de arcos, imprescindibles para poder realizar la calibración en longitud de onda. En este sentido, es preciso identificar las líneas de arco y obtener los polinomios de calibración correspondientes.

Una vez procesadas las calibraciones y reducidos los espectros de las galaxias, hay que
proceder a la extracción de espectros unidimensionales.

Se realiza el ajuste simultáneo de las 3 líneas de emisión del complejo Ha+\[N II\], siguiendo un procedimiento automático que, mediante ligaduras, preserve la separación relativa de las tres líneas y el cociente esperado entre las dos líneas de \[N II\] (para este proceso es útil utilizar, por ejemplo, el paquete astropy.modeling). Se determina la velocidad radial de cada espectro individual y, repitiendo el proceso anterior para cada espectro unidimensional, se obtiene la curva de rotación de una galaxia.