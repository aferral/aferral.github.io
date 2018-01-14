---
published: false
---
## A New Post

Enter text in [Markdown](http://daringfireball.net/projects/markdown/). Use the toolbar above, or click the **?** button for formatting help.

Para evaluar el desempeño del modelo genere una nueva visualizacion que muestre evaluaciones de boards aleatorias. Esto evidencio que mis modelos efectivamnete encuentran los patrones 3x3 pero a veces muestran todos en 0 o no encuentran todos. En resumen esta aprendiendo, pero aun no logra capturar todos los patrones. Esto puede otra vez sugerri que el modelo aun no es lo suficimente complejo.


Un muy buen ejemplo de la arquitectura que quiero replicar con detalles tecnicos bien explicados.
https://storage.googleapis.com/deepmind-media/dqn/DQNNaturePaper.pdf


Intente con una arquitectura de mas capas y al parecer tampoco funciona lo que indica que mi modelo no es un bueno modelo. Esto lleva a dos caminos uno mejorar el algoritmo de entrenamiento o cambiar la estructura completa de la red. Primero intentare cambiar algo el entrenamiento de la red con un red gemela que se encargue de producir el target y la red de estimacion que actualizo. Esto es la implementacion que se utilizo en el paper de deepmind DQN.


Hipotesis a testear ahora:

Mejorara con redes gemelas?
Como afecta esto la estabilidad



Un experimento que me tincaba hacer es tomar el mejor modelo hasta ahora en cnn q learning v6 y entrenarlo ahora en el dataset random . CAMBIAR EL LEARNING RATE ANTES

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
