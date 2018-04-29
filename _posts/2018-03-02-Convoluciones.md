---
layout: post
published: true
---

La idea de este post es tratar de explicar de forma sencilla (espero) la utilidad de la convolucion discreta en el contexto de entrenar redes neuronales. Me centrare en explicar intuitivamente como funcionan los parametros respecto a la operacion y de paso explicar de donde sale la convolucion transpuesta.

# Motivacion: ¿Por que usar la convolucion?

La motivacion para usar convoluciones en redes neuronales parte de utilizar datos como imagenes. En imagenes muchas veces la informacion de un pixel esta altamente relacionada con sus pixeles vecinos y son estos patrones a nivel macro los que realmente importan para el problema a resolver. Por ejemplo en un problema de clasificacion de imagenes areas de pixeles negros rodeados por areas circulares de pixeles blancos podria indicar la presencia de ojos. Si bien una tipica red fully conected es capaz de aprender estas relaciones (al usar combinaciones de pixeles) demostrare que es bastante poco eficiente al estar observando la imagen en su globalidad.

![Figura 1: Ejemplo de MLP no puede detectar patrones invariantes a traslaciones.](https://image.ibb.co/ihHFmx/fig1.png =50x)

Una ejemplo bastante sencillo presente en la figura anterior es el siguiente. Supongamos que quiero una red que se active si encuentra en patron 0,1,0 en algun lugar de la entrada. En la figura se observa una linea con valores donde podria ubicarse tal patron, una red fully conected va tratar de ajustar sus pesos a la localizacion exacta, es decir trata de conseguir el patron global tomando todas las entradas. Sin embargo esto no es adecuado para el problema ya que de mover el patron la red pierde toda oportunidad de localizarlo con esa unidad aprendida. Normalmente si se entrenara una MLP para resolver este problema este modelo entrenaria varias unidades con el mismo patron pero en diversas localizaciones. Es facil de observar que lo que requiero realmente es una unidad que pueda observar una ventana de pixeles cercanos mas concretamente el patron objetivo es de 3 pixeles por lo que con 3 pesos bastaria. Esta operacion sin nombre es la convolucion.

 En una convolucion se definen pesos que se comparten en todas las posiciones posibles de la ventana deslizable sobre la entrada. Cada posicion de la ventana genera una activacion mediante un producto punto entre sus pesos y la entrada, es decir lo mismo que una capa fully conected pero local a una ventana. Es importante recalcar la gran diferencia es que ahora una sola unidad nos entrega muchas activaciones (no solo 1 como en el caso fully conected) con un numero menor de pesos ya que no hay que considerar un peso para cada entrada.

Dado una unidad de convolucion (de ahora en adelante filtro) se va generando un mapa de activaciones. Este filtro tiene pesos para cada elemento en la ventana, estos pesos se llaman kernel. Las activaciones de un filtro se ordenan segun la estructura de la entrada, si la entrada es una matriz 2D se va generar un mapa 2D al aplicar un filtro de convolucion. Esta es una gran diferencia ya que al mantener la estructura se guarda informacion espacial o el donde esa ubicada tal activacion.

A modo de resumen las convoluciones ahorran parametros ya que se requieren muchas unidades fully conected para encontrar caracteristicas en diversas localizaciones, permiten tener operaciones invariantes a traslacion al ser la misma operacion aplicada en diversas posiciones y mantienen informacion espacial al generar mapas de activaciones.

Esta operacion es posible de apilar (crear capas sobre capas) las misma forma que las capas fully conected. Y al igual que en redes MLP a medida que logramos colocar mas capas las entradas que mayormente activan estas unidades se vuelven cada vez mas complejas. Existe literatura que indica que si entrenamos en un dataset como IMAGENET (dataset de clasificacion de objetos cotidianos) observaremos en las primeras capas bordes o patrones simples mientras que en las ultimas llegamos a encontrar detectores de objetos.

# Parametros de convolucion

La convolucion viene determinada por los siguientes parametros :

* <u>Kernel: </u> Una capa fully conected en el fondo es solamente un producto punto entre una entrada y un vector de pesos. La convolucion es otra vez un producto punto, pero se define en una ventana de pesos. Los pesos de esta ventana se llaman kernel, el cual en el caso 2D es una matriz de valores. 

* <u>Stride: </u> Corresponde a el paso en cada dimension para cada ventana. Stride 1 significa que avanzamos un pixel entre una ventana y otra.

* <u>Padding: </u> Indica que hacer en los bordes de la imagen. Normalmente se usa cero padding que es agregar ceros en los bordes de la entrada.

Para entender mejor como varia la convolucion 2D respecto a estos parametros tengo la siguiente animacion:

<iframe src="/conv.html" width="600" height="400" marginwidth="0" marginheight="0" scrolling="yes"></iframe>



En adelante la convolucion se muestra solo es 2D, pero esto solo es por una mas facil visualizacion se puede definir la convolucion en una dimension N centrada en cierta cordenada como : 

$$Conv(n_1,n_2, ... n_M) = \sum_{k_1 = dim_1} \sum_{k_2 = dim_2}...\sum_{k_M = dim_M} Kernel(k_1,k_2,...,k_M) \cdot Input(n_1-k_1,n_2-k_2,...,n_M-k_M)$$

Que viene siendo aplicar el mismo concepto de mover una ventana y multiplicar solo que en espacios de mayor dimension.




# Convolucion como matrix


Es util ver la aplicacion de una capa fully conected como una multiplicacion entre vectores, donde la entrada se multiplica por el vector correspondiente a los pesos para cada entrada.

$$ X \cdot F = A \quad  \| \quad (1,f) \cdot (f,1) = (1,1)$$

A su vez se puede expresar el resultado de una convolucion como una serie de unidades fully conected que solo tienen conexiones de forma local (aunque normalmente las convoluciones no se implementen de esta forma) se observa de esta forma la gran cantidad de parametros que son posibles de ahorrar. 


![Figura 2: Como la convolucion y las capas fully conected se pueden comparar .](https://image.ibb.co/dmrEwx/fig2.png =50x)


En la figura se muestra una convolucion, su mapa de activacion resultante y 4 unidades fully conected que realizan el mismo trabajo.

Esta asociacion tambien tiene importancia para la convolucion transpuesta como mencionare mas abajo.



# Conectividad de filtros

Cuando se realiza el mapa de activaciones se usan diversas ventanas, si se numerca cada ventana desde izquierda a derecha arriba a abajo y se numera en la entrada que ventanas pasan por cada elemento tenemos una imagen como la siguiente.

![Figura 3: Diagrama de conexiones de los filtros .](https://image.ibb.co/bJKX1x/fig3.png =50x)

Esta figura muestra que areas de la entrada son utilizadas para construir las diversas activaciones. Por ahora no parece mucha utilidad pero la tendra. 




# Transposed convolucion

Esta capa muchas veces se conoce como fractionally strided convolution, transposed convolution o la mal nombrada deconvolucion. Pero una forma facil de definirla es que operacion realizo para tener un mapa de activacion del tamaño de la entrada partiendo desde la salida. 

Siendo mas especifico supongamos que tenemos una entrada, un kernel y un mapa de activaciones resultante. La tranposed convolution son los parametros de convolucion que producirian dado la salida un mapa de activacion del tamaño de la entrada y a su vez mantener el patron de conectividad con el mismo kernel. Es importante destacar que la transposed convolution no es la operacion inversa de la convolucion ya que no produce los mismos VALORES sino que reproduce la CONECTIVIDAD por esto no debe llamarse deconvolucion. 

Por ahora parece algo bastante difuso, pero espero que esto se logre aclarar con variados ejemplos y recordando que la transposed convolution sigue siendo una convolucion.

La utilidad de las tranpsosed convolution es que permite aumentar las dimensiones de mapas de activacion. Ejemplos de estos son redes que generan imagenes a partir de una representacion de menor dimensionalidad. Estos modelos han tenido gran impacto en los ultimos años como son las encoders convolucionales, GANS, VAE convolucional, etc.

Veamos un ejemplo con el cual revisaremos la transposed convolution dada varias persepctivas.

![Figura 3: Diagrama de conexiones de los filtros .](https://image.ibb.co/bJKX1x/fig3.png =50x)

Lo que quiero obtener es una operacion que dada el mapa 2x2 como entrada y el mismo kernel 3x3 pueda obtener otro mapa de entrada 3x3 con la misma conectividad de la convolucion original. Con conectividad me refiero a que si la salida original fue armada por 9 unidades (3x3 este caso) esta nueva operacion deberia influenciar 9 bloques en las mismas posiciones.

# Deducir a partir de parametros de convoluciones

En [1] se plantea detalladamente como mediante el calculo de convoluciones es posible realizar la operacion de la convolucion traspuesta. De estas se extraen las siguientes formulas.  Para ello es necesario mantener el kernel variando stride y padding. 

No voy a entrar en detalle sobre el calculo de estas operaciones (para eso ir a [1]), pero en este caso observemos que hay overlap en la conectividad de los filtros, esto indica que necesitamos avanzar "lento" en el filtro de salida para reconstruir el mapa de entrada. Esdecir hay que agregar padding de forma que al mover el filtro se mantengan los overlap de los valores observados en el diagrama de conectividad. En este caso implica un stride fraccional de 1 elemento o agregar 1 cero entremedio de cada valor del filtro de salida.

![Figura 4: Creando convolucion equivalente.](https://image.ibb.co/dancKH/fig4.png =50x)



Se observa que esta convolucion sobre esta imagen con ceros entremedio efectivamente mantiene la conectividad.



# Deduciendo a partir de convolucion como multiplicaciones de matrices

Esta es la forma mas simple de obtener una operacion que mantenga la conectividad. Para esto volvemos a ver las convoluciones como una multiplicacion de matrices. 

Sea la convolucion observada en los ejemplos anteriores. Notar que aca simplemente nombre el padding como parte de la entrada esto no tiene mayor importancia ya que es un caso hasta mas general.

$$
\left[\begin{matrix}I_{00} & I_{01} & I_{02} & I_{03} & I_{04}\\I_{10} & I_{11} & I_{12} & I_{13} & I_{14}\\I_{20} & I_{21} & I_{22} & I_{23} & I_{24}\\I_{30} & I_{31} & I_{32} & I_{33} & I_{34}\\I_{40} & I_{41} & I_{42} & I_{43} & I_{44}\end{matrix}\right]
$$

Estiremos la entrada del ejemplo a un vector para usar multiplicacion de matrices.

$$
X = \left[\begin{array}{ccccccccccccccccccccccccc}I_{00} & I_{01} & I_{02} & I_{03} & I_{04} & I_{10} & I_{11} & I_{12} & I_{13} & I_{14} & I_{20} & I_{21} & I_{22} & I_{23} & I_{24} & I_{30} & I_{31} & I_{32} & I_{33} & I_{34} & I_{40} & I_{41} & I_{42} & I_{43} & I_{44}\end{array}\right]
$$

Luego las posiciones de convoluciones un filtro involucradas son


$$\left [ \left[\begin{matrix}k_{00} & k_{01} & k_{02} & 0 & 0\\k_{10} & k_{11} & k_{12} & 0 & 0\\k_{20} & k_{21} & k_{22} & 0 & 0\\0 & 0 & 0 & 0 & 0\\0 & 0 & 0 & 0 & 0\end{matrix}\right], \quad \left[\begin{matrix}0 & 0 & k_{00} & k_{01} & k_{02}\\0 & 0 & k_{10} & k_{11} & k_{12}\\0 & 0 & k_{20} & k_{21} & k_{22}\\0 & 0 & 0 & 0 & 0\\0 & 0 & 0 & 0 & 0\end{matrix}\right], \quad \left[\begin{matrix}0 & 0 & 0 & 0 & 0\\0 & 0 & 0 & 0 & 0\\k_{00} & k_{01} & k_{02} & 0 & 0\\k_{10} & k_{11} & k_{12} & 0 & 0\\k_{20} & k_{21} & k_{22} & 0 & 0\end{matrix}\right], \quad \left[\begin{matrix}0 & 0 & 0 & 0 & 0\\0 & 0 & 0 & 0 & 0\\0 & 0 & k_{00} & k_{01} & k_{02}\\0 & 0 & k_{10} & k_{11} & k_{12}\\0 & 0 & k_{20} & k_{21} & k_{22}\end{matrix}\right]\right ]$$

Tomamos cada una de estas matrices y la estiramos en un vector. No es dificil darse cuenta que a convolucion es la multiplicacion de matrices entre el vector estirado de entrada con la matriz formada por estirar y apilar las matrices anteriores.


$$
U = \left[\begin{matrix}k_{00} & 0 & 0 & 0\\k_{01} & 0 & 0 & 0\\k_{02} & k_{00} & 0 & 0\\0 & k_{01} & 0 & 0\\0 & k_{02} & 0 & 0\\k_{10} & 0 & 0 & 0\\k_{11} & 0 & 0 & 0\\k_{12} & k_{10} & 0 & 0\\0 & k_{11} & 0 & 0\\0 & k_{12} & 0 & 0\\k_{20} & 0 & k_{00} & 0\\k_{21} & 0 & k_{01} & 0\\k_{22} & k_{20} & k_{02} & k_{00}\\0 & k_{21} & 0 & k_{01}\\0 & k_{22} & 0 & k_{02}\\0 & 0 & k_{10} & 0\\0 & 0 & k_{11} & 0\\0 & 0 & k_{12} & k_{10}\\0 & 0 & 0 & k_{11}\\0 & 0 & 0 & k_{12}\\0 & 0 & k_{20} & 0\\0 & 0 & k_{21} & 0\\0 & 0 & k_{22} & k_{20}\\0 & 0 & 0 & k_{21}\\0 & 0 & 0 & k_{22}\end{matrix}\right]
$$

Podemos expresar la convolucion como:

$$ X \cdot U = O \quad  \| \quad (1,f) \cdot (f,u) = (1,u)$$

Ahora podemos definir otra operacion de la siguiente forma.

$$ O \cdot T = C \quad  \| \quad (1,u) \cdot (u,f) = (1,f)$$

Donde se cumple que la matrix $$T$$ tiene dimensiones transpuestas a la matriz $$U$$

Para combrobar que efectivamente este tiene sentido se puede que ocurre al multiplicar $$O$$ con $$U^{T}$$. Se observa como la conectividad entre el filtro de salida y el kernel es consistente a los diagramas de conectividad originales. Es decir la salida esta colocandose justamente de la misma forma que fue usada en un inicio.

$$
O=\left[\begin{matrix}O_{00} & O_{01}\\O_{10} & O_{11}\end{matrix}\right]
$$

Luego C seria

$$
C = \left[\begin{matrix}O_{00} k_{00} & O_{00} k_{01} & O_{00} k_{02} + O_{01} k_{00} & O_{01} k_{01} & O_{01} k_{02}\\O_{00} k_{10} & O_{00} k_{11} & O_{00} k_{12} + O_{01} k_{10} & O_{01} k_{11} & O_{01} k_{12}\\O_{00} k_{20} + O_{10} k_{00} & O_{00} k_{21} + O_{10} k_{01} & O_{00} k_{22} + O_{01} k_{20} + O_{10} k_{02} + O_{11} k_{00} & O_{01} k_{21} + O_{11} k_{01} & O_{01} k_{22} + O_{11} k_{02}\\O_{10} k_{10} & O_{10} k_{11} & O_{10} k_{12} + O_{11} k_{10} & O_{11} k_{11} & O_{11} k_{12}\\O_{10} k_{20} & O_{10} k_{21} & O_{10} k_{22} + O_{11} k_{20} & O_{11} k_{21} & O_{11} k_{22}\end{matrix}\right]
$$

Si quieres mas detalles de esto aca esta el jupyter notebook que uso para generarlos ([Link](https://drive.google.com/file/d/1wQXRsPqAiFC_DoiJ8paraslB7DlqxD6j/view?usp=sharing)).

# Deduccion a partir de visualizacion por superposicion (lo mismo que conectividad)

Otra forma de entender la convolucion transpuesta es observando el patron de conectividad detalladamente y observar que al realizar la convolucion estamos tomando todos los valores de la ventana colapsando el patron. Entonces la convolucion transpuesta deberia realizar la operacion inveras de dado un valor central expandir a la conectividad dada. Por mi parte me gusta ver esta operacion como si tuvieses un estampador que alimentas con un valor arriba y saca 9 valores abajo el cual vas aplicando por las mismas posiciones que se aplicaron los filtros, de forma que se produce el patron de conectividad.



![Figura 5: Observar como superposicion de filtros .](https://image.ibb.co/m59JRx/fig5.png =50x)




# En resumen

Espero que con todo esto se entienda la importancia tanto de la convolucion como de la convolucion traspuesta. Ademas de las formas de entenderla respecto a las capas fully conected.

# Referencias y links interesantes
1. Detalle sobre calculos de parametros de salida. https://arxiv.org/pdf/1603.07285.pdf
2. http://deeplearning.net/software/theano/tutorial/conv_arithmetic.html
3. https://medium.com/mlreview/a-guide-to-receptive-field-arithmetic-for-convolutional-neural-networks-e0f514068807

4. Detalles del calculo de padding SAME implementacion tensorflow https://www.tensorflow.org/api_guides/python/nn#Notes_on_SAME_Convolution_Padding
5. Un buen video donde explican transposed convolution https://www.youtube.com/watch?v=Xk7myx9_OmU&t=392s
6. https://en.wikipedia.org/wiki/Multidimensional_discrete_convolution
