---
published: false
---

La idea de este post es tratar de hablar detalladamente de los parametros de la convolucion discreta. Me centrare en una parte muy especifica que trata de explicar intuitivamente como funcionan los parametros respecto a la operacion y la relacion entre convolucion/conv. transpuesta.

Siempre me ha gustado tratar de ver un tema desde varias perpectivas y ver como estas son equivalentes. Con esta idea en mente tratare de mostrar las convoluciones como filtros como matrices y sus conectividades.

# Convolucion

# Eficiencia de parametros

La motivacion para usar convoluciones en redes neuronales parte de utilizar datos como imagenes. En imagenes muchas veces la informacion de un pixel tiene mucha relacion con los pixeles vecinos y son estas relaciones la verdadera informacion a extraer de la imagen, por ejemplo pixeles negros rodeados de pixeles blancos podria indicar la presencia de ojos. Si bien una tipica red fully conected es capaz de aprender estas relaciones (al usar combinaciones de pixeles) es bastante poco eficiente ya que cada unidad va tener que aprender que casi todos los pesos son ceros. Una alternativa mucho mas razonable podria ser solo pesos con pixeles cercanos. Esta necesidad de tomar solo pixeles cercanos hace que esta operaciones sea por ventanas.

# Informacion espacial

En una red MLP una capa solo genera una activacion, en cambio la convolucion por cada ventana genera una activacion. Esta es una gran diferencia pues permite guardar informacion espacial o el donde esa ubicada tal activacion. 


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

Es util entender una capa fully conected como una multiplicacion por una matriz. Y a su vez se puede expresar el resultado de una convolucion como una serie de unidades fully conected que solo tienen conexiones de forma local (aunque normalmente las convoluciones no se implementen de esta forma) de esta forma se observa la cantidad de parametros que son posibles de ahorrar. 

$a /cdot b$
FORMULA MATRIZ




*** A2 animacion de matriz generica con equivalencia a convolucion 


Esta asociacion tambien tiene importancia para la convolucion transpuesta como mencionare mas abajo.



# Conectividad de filtros

Si se habla de la forma en que cada de entrada es influenciada por cada posicion de filtro tendremos este diagrama. De ahora no parece mucha utilidad pero la tendra. 


*** A3 diagrama conectividad filtros


# Transposed convolucion

Esta capa muchas veces se conoce como fractionally strided convolution, transposed convolution o  mal nombrada deconvolucion. Para explicar esta operacion supongamos que tenemos una entrada, un kernel y una salida resultante de la convolucion de ciertos parametros de la entrada con el kernel, la tranposed convolution son los parametros de convolucion que producirian dado la salida denuevo el patron de conectividad de la entrada con el mismo kernel. Lo importante es que la transposed convolution no es la operacion inversa de la convolucion ya que no produce los mismos VALORES sino que reproduce la CONECTIVIDAD. Por esto no debe llamarse deconvolucion. 

Veamos un ejemplo:

EJEMPLO

Ahora expresemos esto en las diversas formas de entender la convolucion que mencione mas arriba.


# Forma de derivarla por conectividad

# Forma de derivarla por matrix

# Visualizacion por superposicion (lo mismo que conectividad)

# Formulas de salida y calculo de conv a deconv.



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
