---
published: false
---
## A New Post

Enter text in [Markdown](http://daringfireball.net/projects/markdown/). Use the toolbar above, or click the **?** button for formatting help.


Revisando sobre las preguntas que quedaron me puse a averiguar sobre la normalizacion de entradas y salidas

http://www.faqs.org/faqs/ai-faq/neural-nets/part2/
http://yeephycho.github.io/2016/08/03/Normalizations-in-neural-networks/
https://stackoverflow.com/questions/4674623/why-do-we-have-to-normalize-the-input-for-an-artificial-neural-network

Otra cosa que me puse a pensar es que quiza estoy dejando mal configurado el learning rate. Este deberia bajar si se entrenara continuamente, sin embargo estoy re iniciando el entrenamiento cada cierto tiempo lo cual podria inducir saltos en el learning rate.

Efectivamente estaba utilizando GradientDescentOptimizer con un learning rate fijo. 

Experimentare con un learning rate que decae exponencialmente:

decayed_learning_rate = learning_rate * decay_rate ^ (global_step / decay_steps)
![learning rate]({{site.baseurl}}/_posts/learning_rate.png)


Configure esos parametros para que aproximadamente en la iteracion 60.000 el learning rate sea 0.001 partiendo de un valor de 0.1. Otro detalle que me equivoque es que no son 60.000 updates de gradiente son 60.000 / batch_size ya que las iteraciones son transiciones.

Realizando estos cambios obtuve dos modelos con learning_rate decreciente en forma exponencial con las iteraciones de entrenamiento.

![learning rate]({{site.baseurl}}/_posts/pasos_learning_rate.png)
![it_vs_learning_rate]({{site.baseurl}}/_posts/learnig_rate.png)
![loss learning rate]({{site.baseurl}}/_posts/loss_learning_rate.png)


No existio mayor cambio en los resultados tanto en score como en loss. Por lo tanto hay que seguir investigando.

Commit hasta este punto
https://bitbucket.org/aferral/bilge-sup/commits/ed541c8245e7623fdbb29d08a02b9f88ee7af849

Voy a comenzar a guardar los gradientes para observar que ocurre.

![Gradientes modelo resultante]({{site.baseurl}}/_posts/gradientes.png)

La imagen muestra que no hay nada muy extraño decrecen despues de cierto tiempo dando a entender que el modelo aprende normalmente. El modelo muestra que la mayor variacion esta en los bias de las ultimas capas.

Algo que me acabo de dar cuenta es que tengo un relu en la capa final lo que obliga a tener valores positivos cuando hay muchos valores -1. Cambiando esto no ocurrieron grandes cambios. Repasando todo lo anterior me da la impresion que el modelo no es lo suficiente poderoso. Otra idea a explorar es cambiar las reward de -1, +10 con salida lineal a otra cosa.

https://bitbucket.org/aferral/bilge-sup/commits/3247ca82b20d1594df9478a48a2c56f52c78e795




