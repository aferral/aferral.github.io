---
published: false
---

La idea de este post es tratar de hablar detalladamente de los parametros de la convolucion discreta. Me centrare en una parte muy especifica que trata de explicar intuitivamente como funcionan los parametros respecto a la operacion y la relacion entre convolucion/conv. transpuesta.

Siempre me ha gustado tratar de ver un tema desde varias perpectivas y ver como estas son equivalentes. Con esta idea en mente tratare de mostrar las convoluciones como filtros como matrices y sus conectividades.

# Convolucion

# Eficiencia de parametros

La motivacion para usar convoluciones en redes neuronales parte de utilizar datos como imagenes. En imagenes muchas veces la informacion de un pixel tiene mucha relacion con los pixeles vecinos y son estas relaciones la verdadera informacion a extraer de la imagen, por ejemplo pixeles negros rodeados de pixeles blancos podria indicar la presencia de ojos. Si bien una tipica red fully conected es capaz de aprender estas relaciones (al usar combinaciones de pixeles) es bastante poco eficiente al una unidad estar influenciada por todos los pixeles a la vez. Una alternativa mucho mas razonable podria ser solo pesos con pixeles cercanos. Esta necesidad de tomar solo pixeles cercanos hace que esta operaciones sea por ventanas.

# Informacion espacial

En una red MLP una capa solo genera una activacion, en cambio la convolucion por cada ventana genera una activacion. Esta es una gran diferencia pues permite guardar informacion espacial o el donde esa ubicada tal activacion. 


(convolucion o correlacion) **

## Parametros de convolucion

La convolucion viene determinada por los siguientes parametros :

* Kernel: Una capa fully conected en el fondo es solamente un producto punto entre una entrada y un vector de pesos. La convolucion es otra vez un producto punto, pero que se define en una ventana de pesos. Esta ventana de pesos se denomina kernel, el cual en el caso 2D es una matriz de valores. 

* Stride: Corresponde a el paso en cada dimension para cada ventana.

* Padding: Corresponde a agregar ceros u otro valor en los bordes de la entrada.

*** A1 animacion con convolucion configurable

Si bien hablare principalmente de la convolucion en 2D esta se puede definir en dimensiones mayores como: 

\sum_{k_1=-\infty}^{\infty} \sum_{k_2=-\infty}^{\infty}...\sum_{k_M=-\infty}^{\infty} h(k_1,k_2,...,k_M)x(n_1-k_1,n_2-k_2,...,n_M-k_M)
 [1]

Que viene siendo aplicar el mismo concepto de mover una ventana y multiplicar solo que ahora en un cubo, hiper cubo, etc.

[1] https://en.wikipedia.org/wiki/Multidimensional_discrete_convolution

# Convolucion como matrix

Una asociacion util es entender una capa fully conected como una multiplicacion por una matriz.

FORMULA MATRIZ


Hablar de equivalencia de convolucion a multiplicacion de matriz toepliz  ***

Como mencione antes las convoluciones podrian generarse por capas fully conected esto se puede en cierta forma demostrar utilizando estas matrices.

*** A2 animacion de matriz generica con equivalencia a convolucion 

Es decir podemos generar dado un filtro convolucional la matriz con sus unidades fully conected equivalentes. Esto es un claro ejemplo de como las convoluciones ahorran complejidad a la red ya que un solo filtro involucra muchas unidades ocultas "simuladas". 

Esta asociacion tambien tiene importancia para la convolucion transpuesta como mencionare mas abajo.



# Conectividad de filtros

Si se habla que celda de entrada es influenciada por cada posicion de filtro tendremos este driagra. De ahora no parece mucha utilidad pero la tendra. ***

*** A3 diagrama conectividad filtros

# Formulas de salida

# Transposed convolucion

Diversos nombres que tiene y porque no deberia llamarse deconvolucion.

MOtivacion lograr mismo size de convolucion con mismo filtro. Usos en diversas aplicaciones

# Forma de derivarla por conectividad

# Forma de derivarla por matrix

# Visualizacion por superposicion (lo mismo que conectividad)

# Formulas de salida y calculo de conv a deconv.

Enter text in [Markdown](http://daringfireball.net/projects/markdown/). Use the toolbar above, or click the **?** button for formatting help.


Dudas interesantes (extras)

** Calculo detallado de formulas
Mucho sobre como calcular casos donde el kernel no es impar
--- https://www.tensorflow.org/api_guides/python/nn#Notes_on_SAME_Convolution_Padding

** Ejemplos varias dimensiones

Finalmente lo explican bien
aca
https://www.youtube.com/watch?v=Xk7myx9_OmU&t=392s


-- Con varias dimensiones
-- Ejempo con parametros


Referencias
[1] https://arxiv.org/pdf/1603.07285.pdf
http://deeplearning.net/software/theano/tutorial/conv_arithmetic.html
https://medium.com/mlreview/a-guide-to-receptive-field-arithmetic-for-convolutional-neural-networks-e0f514068807
