---
published: false
---
# Parte 3 Aun nada

## Nueva forma de evaluar

Para evaluar el desempeño del modelo genere una nueva visualizacion que muestre evaluaciones de boards aleatorias. Esto evidencio que mis modelos efectivamente encuentran los patrones 3x3 pero a veces muestran todos en 0 o no encuentran todos. En resumen esta aprendiendo, pero aun no logra capturar todos los patrones. Esto sugerir que se necesita un modelo mas complejo.


(Fotos de evaluaciones)

{% include carousel.html name=”Example” data="[
  {
    "title": "An image",
    "caption": "A caption",
    "url": "an url"
  },
  {
    "title": "An image",
    "url": "another url"
  },
]" %}

Esto lleva a dos caminos uno mejorar el algoritmo de entrenamiento o cambiar la estructura completa de la red. Me parece mas sensato primero intentar el algoritmo propuesto en el paper de DeepMind del 2003 donde usan dos redes en vez de una. Una red se guarda cada ciertos pasos y esta es la red utilizada para generar los target. La idea de esto es lograr un estimado del target menos ruidoso.

## Un gran error

Detengan todo me di cuenta que tengo el sampler solo consiguiendo 100 valores. Esto significa que estaba entrenando con un daset de solo 100 boards. Tambien aprovecho de arreglar el problema que tenia con los boards que se generan invalidos ahora cada vez que genera board calcula colisiones lo cual siempre entregara boards validos aumenta el tiempo de generacion y tambien rompe algunos sampleos de patrones pero se espera que estos efectos sean infimos.

(FOTO ENTRENAMIENTO V8 vs V6)

Se estabiliza al doble de altura que antes una ganancia, pero aun lejos de los 1000 del programa 


## Perdida y score inversamente proporcionales

Intente de igualmente con las redes mas profundas funcionan de peor forma. Cual es la razon de esto? Tengo una cantidad ilimitada de datos y ocurre que son marcadamente inferiores. De hecho lo mas interesante de todo este asunto es que la funcion de perdida es marcadamente inferior, pero mi funcion de score es mucho mas baja. Lo cual es paradojico y puede indicar que tengo mal configurado el problema ya que esperaria que minimizar la perdida entregue mejores scores.

(Foto perdidad arquitecturas deep)

HABLAR DE DINAMISMO DEL SITEMA

AVERIGUAR DEFINICION DEL PROBLEMA FRENTE A

## Recordar bajar random prob

Otro detalle importante que me acabo de dar cuenta es que la probabilidad de exploracion debe bajarse paulatinamiente o de lo contrario daña el modelo. En mi caso estaba constante en el 0.3 (Si el numero random cae bajo 0.3 realiza accion aleatoria) lo cual produciria cierta aleatoriedad en el proceso que realmente no existe en caso de explotacion.

Tablas de score hasta ahora (Todos con 1000 estados con sampleo aleatorio cada 5 acciones)

Modelo V8 con random_prob=0.3 score 410
Modelo V8 con random_prob=0 score 930.1
Modelo perfecto hasta dos pasos score 1404.7
Modelo completamente aleatorio 114.75

Es importante recalcar que el sistema sufre una importante falla. Sin esta probabilidad aleatoria tiende a quedarse atrapado en acciones redundantes. Es decir termina realizando siempre la misma accion al ser el board y su reaccion muy similares para la red. Esto no es detectado por el score del modelo aleatorio ya que esa aleatoriedad evita bucles y a su vez el algoritmo de evaluacion se resetea cada 5 acciones. Para 

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
