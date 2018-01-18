---
published: false
---
## Parte 3 Aun nada



Para evaluar el desempeño del modelo genere una nueva visualizacion que muestre evaluaciones de boards aleatorias. Esto evidencio que mis modelos efectivamente encuentran los patrones 3x3 pero a veces muestran todos en 0 o no encuentran todos. En resumen esta aprendiendo, pero aun no logra capturar todos los patrones. Esto sugerir que se necesita un modelo mas complejo.




Un muy buen ejemplo de la arquitectura que quiero replicar con detalles tecnicos bien explicados.
https://storage.googleapis.com/deepmind-media/dqn/DQNNaturePaper.pdf


Intente con una arquitectura de mas capas y el desempeño es peor.

Esto lleva a dos caminos uno mejorar el algoritmo de entrenamiento o cambiar la estructura completa de la red. Primero intentare cambiar algo el entrenamiento de la red con un red gemela que se encargue de producir el target y la red de estimacion que actualizo. Esto es la implementacion que se utilizo en el paper de deepmind DQN.


AAAAA detengan todo me di cuenta que tengo el sampler solo consiguiendo 100 valores. Esto significa que estaba entrenando con un daset de solo 100 boards. Tambien aprovecho de arreglar el problema que tenia con los boards que se generan invalidos ahora cada vez que genera board calcula colisiones lo cual siempre entregara boards validos (aumenta el tiempo de generacion y tambien rompe algunos sampleos de patrones pero se espera que estos efectos sean infimos).

Se estabiliza al doble de altura que antes una ganancia, pero aun lejos de los 1000 del programa 


Hipotesis a testear ahora:

Mejorara con redes gemelas?
Como afecta esto la estabilidad



REDES GEMELAS

REDES GEMELAS + REWARD CLIPING + REWARDS +1 -1

Mucho cuidado con las reward
https://medium.com/@karpathy/yes-you-should-understand-backprop-e2f06eab496b


Ideas a revisar

* Redes gemelas
* Cambiar rewards
* ajustar salida red
* Revisar influencia de samples no reales
* Aumentar capas
