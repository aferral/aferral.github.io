---
published: false
---
# Parte 3 Aun nada

## Nueva forma de evaluar

Para evaluar el desempeño del modelo genere una nueva visualizacion que muestre evaluaciones de boards aleatorias. Esto evidencio que mis modelos efectivamente encuentran los patrones 3x3 pero a veces muestran todos en 0 o no encuentran todos. En resumen esta aprendiendo, pero aun no logra capturar todos los patrones. Esto sugerir que se necesita un modelo mas complejo.


(Fotos de evaluaciones)


Esto lleva a dos caminos uno mejorar el algoritmo de entrenamiento o cambiar la estructura completa de la red. Primero intentare cambiar algo el entrenamiento de la red con un red gemela que se encargue de producir el target y la red de estimacion que actualizo. Esto es la implementacion que se utilizo en el paper de deepmind DQN.

## Un gran error

Detengan todo me di cuenta que tengo el sampler solo consiguiendo 100 valores. Esto significa que estaba entrenando con un daset de solo 100 boards. Tambien aprovecho de arreglar el problema que tenia con los boards que se generan invalidos ahora cada vez que genera board calcula colisiones lo cual siempre entregara boards validos aumenta el tiempo de generacion y tambien rompe algunos sampleos de patrones pero se espera que estos efectos sean infimos.

(FOTO ENTRENAMIENTO V8 vs V6)

Se estabiliza al doble de altura que antes una ganancia, pero aun lejos de los 1000 del programa 


## Perdida y score inversamente proporcionales

Intente de igualmente con las redes mas profundas funcionan de peor forma. Cual es la razon de esto? Tengo una cantidad ilimitada de datos y ocurre que son marcadamente inferiores. De hecho lo mas interesante de todo este asunto es que la funcion de perdida es marcadamente inferior, pero mi funcion de score es mucho mas baja. Lo cual es paradojico y puede indicar que tengo mal configurado el problema ya que esperaria que minimizar la perdida entregue mejores scores.

(Foto perdidad arquitecturas deep)

HABLAR DE DINAMISMO DEL SITEMA

AVERIGUAR DEFINICION DEL PROBLEMA FRENTE A


## Habilidad del modelo para buscar movimientos a mas de 1 paso
Me gustaria ahora comparar este modeo con el modelo de revisar exaustivamente a un paso. Quiero evaluar si efectivamente mi modelo esta planeando al largo plazo. Lo 


Hipotesis a testear ahora:

Mejorara con redes gemelas?
Como afecta esto la estabilidad


REDES GEMELAS

REDES GEMELAS + REWARD CLIPING + REWARDS +1 -1

Fuentes importantes:

* Un muy buen ejemplo de la arquitectura que quiero replicar con detalles tecnicos bien explicados.
https://storage.googleapis.com/deepmind-media/dqn/DQNNaturePaper.pdf

* Mucho cuidado con las reward
https://medium.com/@karpathy/yes-you-should-understand-backprop-e2f06eab496b


Ideas a revisar

* Redes gemelas
* Cambiar rewards
* ajustar salida red
* Aumentar capas
