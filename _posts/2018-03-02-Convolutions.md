---
published: false
---

La idea de este post es tratar de hablar detalladamente de los parametros de la convolucion discreta. Me centrare en una parte muy especifica que trata de explicar intuitivamente como funcionan los parametros respecto a la operacion y la relacion entre convolucion/conv. transpuesta.

Siempre me ha gustado tratar de ver un tema desde varias perpectivas y ver como estas son equivalentes. Con esta idea en mente tratare de mostrar las convoluciones como filtros como matrices y sus conectividades.

# Convolucion

La motivacion para que las redes comenzaran a usar convoluciones parte de la necesidad de encontrar una forma de aprovechar la estructura de los datos. Usualmente en imagenes los pixeles cercanos tienen bastante importancia al por ejemplo buscar un ojo una cara o cualquier tipo de patron visual. Si intentaramos usar una red MLP tiene que aprender a encontrar estas relaciones como un trabajo extra. Una convolucion es en cierta forma una MLP con un prior infinitamente fuerte con sus conexiones vecinas.

La convolucion tambien involucra muchos menos parametros a reutilizar sus pesos por tods los filtros. De hechos las tipicas redes de clasificacion teine su mayor cantidad de parametros al concadenar los filtros y usar una MLP.


(convolucion o correlacion) **

## Parametros de convolucion

Una convolucion es realmente un producto punto que va desplazandose por el espacio es importante entender que hace cada uno de los parametros que la definen.


Stride,
padding,
input,
kernel size *

*** A1 animacion con convolucion configurable

(ecuacion de la convolucion general) **

# Convolucion como matrix

Hablar de equivalencia de convolucion a multiplicacion de matriz toepliz  ***

*** A2 animacion de matriz generica con equivalencia a convolucion 

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
