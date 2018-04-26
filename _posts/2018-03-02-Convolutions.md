---
published: false
---

La idea de este post es tratar de introducir los parametros de la convolucion discreta. Me centrare en una parte muy especifica que trata de explicar intuitivamente como funcionan los parametros respecto a la operacion y la relacion entre convolucion/conv. transpuesta.

Siempre me ha gustado tratar de ver un tema desde varias perpectivas y ver como estas son equivalentes. Con esta idea en mente tratare de mostrar las convoluciones como filtros como matrices y sus conectividades.

# Convolucion


## Motivacion de uso de convolucion en vez de capas fully conected

Figura mostrando patron. Mostrar redes MLP que los encuentran
Imagen Linea de puntos patron de 3 [1,0,1]  Mlp que se activa mas pesos localizados. Imagen 2 se movio el patron unidad debe moverse. Imagen 3. Mlp movil

La motivacion para usar convoluciones en redes neuronales parte de utilizar datos como imagenes. En imagenes muchas veces la informacion de un pixel tiene mucha relacion con los pixeles vecinos y son estas relaciones la verdadera informacion a extraer de la imagen, por ejemplo pixeles negros rodeados de pixeles blancos podria indicar la presencia de ojos. Si bien una tipica red fully conected es capaz de aprender estas relaciones (al usar combinaciones de pixeles) es bastante poco eficiente ya que cada unidad va tener que aprender que casi todos los pesos son ceros.

Una caricatura de esto podria ser la siguiente. Supongamos que quiero una red que se active si encuentra en patron 1,0,1 en la entrada aca estoy tratando que la red detecte un patron localizado. En la figura de ejemplo se observa que dado una imagen de entrenamiento una red tipica va tratar de ajustar sus pesos a la localizacion exacta al ser pesos para cada unidad de la entrada, es decir trata de conseguir el patron global tomando todas las entradas. Sin embargo esto no es adecuado para el problema ya que si se mueve el patron la red pierde toda oportunidad de localizarlo con esa unidad aprendida, pero el patron sigue ahi. Normalmente si se entrenara una MLP para resolver este problema entrenaria varias unidades con el mismo paatron en diversas localizaciones, pero esto no es para nada eficiente. Ahora realmente lo que requiero es una unidad que pueda observar una ventana de pixeles cercanos ya que el patron es de 3 pixeles con 3 pesos bastaria, pero estos pesos se deberian mover por ventana como en la figura 3, ahora una sola unidad va generar un mapa de activaciones no solo un activacion como en la mlp, pero he logrado con solo una unidad resolver el problema invariante a traslaciones.

Las convoluciones son una alternativa mucho mas razonable donde solo pesos con pixeles cercanos se usan. Esto tiene la gran ventaja que se aplica la misma funcion a elementos trasladados. 

Como mencionaba las convoluciones por unidad generan un mapa de activaciones. Por eso la unidad de una convolucion es el filtro que posee un kernel este produce un mapa de activaciones donde cada activacion tiene asociada una localizacoin en la imagen o una ventana. Esta es una gran diferencia ya que al mantener la estructura 2D de las activaciones se guarda informacion espacial o el donde esa ubicada tal activacion. Para un ejemplo de convolucion 2D ir a figura interactiva

A modo de resumen las convoluciones ahorran parametros ya que se requieren muchas unidades fully conected para encontrar caracteristicas en diversas localizaciones, permiten tener operaciones invariantes a traslacion al ser la misma operacion aplicada en diversas posiciones y mantienen informacion espacial al generar mapas de activaciones.



## Parametros de convolucion

La convolucion viene determinada por los siguientes parametros :

* Kernel: Una capa fully conected en el fondo es solamente un producto punto entre una entrada y un vector de pesos. La convolucion es otra vez un producto punto, pero que se define en una ventana de pesos. Esta ventana de pesos se denomina kernel, el cual en el caso 2D es una matriz de valores. 

* Stride: Corresponde a el paso en cada dimension para cada ventana.

* Padding: Corresponde a agregar ceros u otro valor en los bordes de la entrada. Dentro de estos padding estan los mas famosos VALID y SAME. 

*** A1 animacion con convolucion configurable

Si bien hablare principalmente de la convolucion en 2D esta se puede definir en dimensiones mayores como: 

\sum_{k_1=-\infty}^{\infty} \sum_{k_2=-\infty}^{\infty}...\sum_{k_M=-\infty}^{\infty} h(k_1,k_2,...,k_M)x(n_1-k_1,n_2-k_2,...,n_M-k_M)
 [1]

Que viene siendo aplicar el mismo concepto de mover una ventana y multiplicar solo que ahora en un cubo, hiper cubo, etc.



## Convolucion como matrix

Es util entender una capa fully conected como una multiplicacion por una matriz. Y a su vez se puede expresar el resultado de una convolucion como una serie de unidades fully conected que solo tienen conexiones de forma local (aunque normalmente las convoluciones no se implementen de esta forma) de esta forma se observa la cantidad de parametros que son posibles de ahorrar. 

$a /cdot b$
FORMULA MATRIZ

*** A2 animacion de matriz generica con equivalencia a convolucion 


Esta asociacion tambien tiene importancia para la convolucion transpuesta como mencionare mas abajo.



## Conectividad de filtros

Si se habla de la forma en que cada de entrada es influenciada por cada posicion de filtro tendremos este diagrama. De ahora no parece mucha utilidad pero la tendra. 

*** A3 diagrama conectividad filtros



# Transposed convolucion

Esta capa muchas veces se conoce como fractionally strided convolution, transposed convolution o  mal nombrada deconvolucion. Para explicar esta operacion supongamos que tenemos una entrada, un kernel y una salida resultante de la convolucion de ciertos parametros de la entrada con el kernel, la tranposed convolution son los parametros de convolucion que producirian dado la salida denuevo el patron de conectividad de la entrada con el mismo kernel. Lo importante es que la transposed convolution no es la operacion inversa de la convolucion ya que no produce los mismos VALORES sino que reproduce la CONECTIVIDAD por esto no debe llamarse deconvolucion. 

Veamos un ejemplo:

EJEMPLO

Ahora expresemos esto en las diversas formas de entender la convolucion que mencione mas arriba.

## Por conectividad

## Por matrix

## Visualizacion por superposicion (lo mismo que conectividad)

## Formulas de salida y calculo de conv a deconv.



# Referencias y links interesantes
[1] Detalle sobre calculos de parametros de salida. https://arxiv.org/pdf/1603.07285.pdf
[2] http://deeplearning.net/software/theano/tutorial/conv_arithmetic.html
[3] https://medium.com/mlreview/a-guide-to-receptive-field-arithmetic-for-convolutional-neural-networks-e0f514068807

[4] Detalles del calculo de padding SAME implementacion tensorflow https://www.tensorflow.org/api_guides/python/nn#Notes_on_SAME_Convolution_Padding
[5] Un buen video donde explican transposed convolution https://www.youtube.com/watch?v=Xk7myx9_OmU&t=392s
[6] https://en.wikipedia.org/wiki/Multidimensional_discrete_convolution

