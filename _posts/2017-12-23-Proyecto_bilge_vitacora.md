---
published: false
---
## Proyecto de aprendizaje de refuerzo

# Contexto
Como contexto estoy intentado crear un agente de refuerzo para un juego de puzzle. Los algoritmos a analizar son una variante de Q learning con redes convolucionales.

La idea de este post es mantener algunos apuntes de los resultados y registrar el proceso para encontrar mejores modelos.


# Resultados hasta ahora

De momento tengo terminado el codigo base para realizar los agentes. Se implementaron dos agentes uno de iteracion de valores (utiliza modelo del problema) y otro con Q learning (utiliza solo transiciones guardadas). Para la estimacion de la funcion de Q(s,a) se eligio una red convolucional de creacion propia. La idea consistia en aprovechar la simetria de los patrones ante diversos colores, es decir no tiene importancia el color de una pieza solamente importan todos los patrones de pares de piezas. 

Se itero el algoritmo por aproximadamente 40.000 iteraciones y los resultados fueron los siguientes:

![score cada 1000 iteraciones]({{site.baseurl}}/_posts/grafico_score1.png)
![perdida por iteracion]({{site.baseurl}}/_posts/loss_1.png)



El primer grafico corresponde a evaluar el score del agente en juegos aleatorios por 10.000 pasos. Es una forma de probar que tan bien el agente juega. Esta metrica se calcula cada 1000 pasos de entrenamiento de la red, una iteracion es observar una transicion estado,accion,nuevo_estado.

El segundo grafico es la perdida de la red segun mean square error respecto al valor Q(s,a) estimado y el Q(s',a') + reward. En este caso estoy mostrando la perdidad de un batch por lo tanto se espera que sea ruidosa. Esta se calcula cada vez que se actualiza la red lo que ocurre cada vez que se llena un batch de valores.


Hay en una parte un salto brusco, esto es debido a que se cambio la forma en que recibia los estados a observar. En un inicio era aleatorio, posteriormente se colocaron estados cercanos a un combo. Cuando se realizar un combo se consigue una gran recompensa, en caso contario se da recompensa negativa. Por esto al inicio existian pocos peeks (no conseguia recompensa por lo tanto un bias negativo era una buena predicccion). Espero que al usar estados tan cercanos a las recompensas el aprendizaje sea mas facil para la red ya que de usar complemente aleatorio tomaria mucho tiempo explorar.

Los graficos en las imagenes estan con 0.9 de smoth, de hecho se observan los originales de fondo. Es claro que son muy ruidosos, por lo tanto queda la duda si hay una real mejora de la funcion perdida de la red. En la evaluaciones por score pareciera que hay una mejora, pero sigo algo escéptico a su desempeño al no existir considerable mejora en la perdida.

El proyecto hasta este punto esta en 
[linkCommit](https://bitbucket.org/aferral/bilge-sup/commits/8d43f3a4015976ca17d48507295a8c19b50f2a13)

# Pasos a seguir

* Creo que es necesario observar un estimado de perdida mas certero. Por lo tanto tomare varios batch para calcular el estimado de loss. Esto entregara algo menos ruidoso. Notar que esto sera solo para temas de graficar el entrenamiento seguira siendo de un solo batch.

* ¿Puede la escala del error afectar el desempeño? De momento las recompensas son 10 para combo, -1 cualquier otra cosa y un gamma de 0.1. ¡ Como estos valores influyen en el entrenamiento respecto a su magnitud ?








