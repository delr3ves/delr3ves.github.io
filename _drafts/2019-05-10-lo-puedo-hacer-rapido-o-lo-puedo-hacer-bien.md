---
layout: commented_post
title:  Lo puedo hacer rápido o lo puedo hacer bien
date:   2019-04-10 12:00:00 +0200
categories: blog
excerpt:
  De todas las creencias del ideario informático, una de las que más me duele cuando la escucho es la archiconocida <q>Lo puedo hacer rápido o lo puedo hacer bien</q>
---
De todas las creencias del ideario informático, una de las que más me duele cuando la escucho es la archiconocida

>Lo puedo hacer rápido o lo puedo hacer bien

con una de sus variantes estrella:

>Esta feature son dos días, pero con test cuatro

a lo que muchas veces desde producto nos contestan:

> ¿pero como tardas tanto? (si eso poner un if)

y eso ya hace que nos hierva la sangre imposibilitando la conversación...

Yo en la primera afirmación veo tres conceptos importantes que conviene aclarar antes de seguir:


## Rápido

Mi primera reflexión va sobre el concepto de rápido. **¡¡¡Siempre tenemos prisa!!!**. Yo creo que para poder tener una conversación razonable con la parte que nos mete prisa es importante entender el origen de esa prisa que puede ser por múltiples razones, a menudo por más de una a la vez:

* **Tenemos una ventana de tiempo muy ajustada y, o salimos pronto o perdemos la oportunidad:** Este es un caso legítimo, pero debería ser poco habitual. En mi opinión, si nuestro negocio siempre tiene esa urgencia por salir y sus ventanas de tiempo son ajustadas, estamos condenados a fracasar. Y deberíamos plantearnos otro tipo de soluciones.

* **Queremos minimizar la Ley de Parkinson que dice que _"el trabajo se expande hasta llenar el tiempo disponible para que se termine"_:** nos guste o no esto es un problema real, aunque ahora lo llamamos procrastinar que suena como más gracioso. En cierto modo tiene sentido, que las personas encargadas del producto traten de reducir ese tiempo para evitar dilatar la tarea más de lo necesario. Y creo que a muchos nos viene bien que se haga un poco. Pero siempre con sentido común.

* Para mi la razón principal es que, **el mayor coste de un proyecto de desarrollo es el tiempo de sus desarrolladores** y por lo tanto, terminar la tarea cuanto antes, es la forma más inmediata y evidente de avaratar el coste.

* Por último, me viene un caso también bastante típico en el que es el propio equipo de desarrollo quien tiene esa sensación de urgencia. A menudo porque la tarea que está hacinedo le aburre y otras veces porque directamente estamos en "modo deportivo".

Con esto quiero decir, que **la mayoría de las veces, cuando alguien nos dice que quiere algo rápido, lo que nos quiere decir es que quiere algo barato**. Y si nosotros somos conscientes de esto es muy probable que empecemos a tener conversaciones mucho más provechosas con la otra parte sobre por qué hacer algo a priori más lento, puede llegar a ser más rápido. Por fin estaremos hablando el mismo lenguaje!

Además **rápido es un concepto relativo y depende de la tarea a llevar a cabo.** _¿Qué es más rápido, correr los 100 metros en 2 minutos o la maratón en 2 horas?_

Debemos tener claro que las cosas llevan su tiempo y hacer software es una tarea delicada y tediosa. No debemos subestimarla porque luego nos estallará en la cara.

## Bien

Mi siguiente reflexión va sobre el significado de **bien** para nosotros y para el resto del equipo. No creo que haya un criterio único y permanente aunque si que hay algunas cuestiones que parecen ser condición necesaria (pero no suficente) para considerar algo como _de calidad_.

Lo primero que me llama la atención de la afirmación _"lo puedo hacer rápido o lo puedo hacer bien"_ es que de primeras no consideremos **rápido** como parte de nuestro criterio de bien.

Sin entrar, de momento, en detalles técnicos, para mi una tarea está bien resuelta si:

* Resuelve el problema que justifica llevar a cabo la tarea.
* Perdura el tiempo que se espera de ella.
* El coste de mantenimiento es razonable. Es decir, no se dispara en el tiempo.
* Y por supuesto que esté hecha en el menor tiempo posible. O lo que es lo mismo... todo lo rápido que podamos.

Entrando en detalles técnicos, entiendo que cuando hablamos de hacer las cosas bien nos referimos a tener en cuenta todos esos criterios que la industria ha establecido como "de calidad", entre los que se encuentran:

* Testing automático (incluso TDD)
* Pull Requests
* Continuous Integration/Delivery/Deployment
* Micro servicios
* RX
* Clean architecture
* Con feature flags
* Con métricas de uso y rendimiento
* (_y cualquier otra buzzword que se nos ocurra_)...

Es curioso, **lo interiorizado que tenemos, que todas estas prácticas que acabo de mencionar incrementan el tiempo de desarrollo**, cuando todas ellas están pensadas precisamente para lo contrario.

Tenemos un prejuicio, heredado del mundo físico que nos dice que **hacer las cosas bien es más lento y por lo tanto más caro que hacerlas mal** y es algo que tenemos que dejar de lado cuanto antes para poder mejorar en nuestro trabajo.

Veo dos problemas claros aquí:

1. No tenemos asimiladas estas técnicas y por lo tanto nos parecen más difíciles de lo que son.
2. Elegimos mal la unidad de estudio.

> La decisión epistemológica más importante que se hace es la unidad de estudio. Si nos equivocamos en la elección de la unidad podemos encontrarnos enfrentando el organismo a su ambiente.
(Gregory Bateson)

Me explico. Si la unidad de estudio es la tarea a desarrollar, quizás, aplicar todas estas técnicas puedan introducir un sobrecoste.
No obstante, las tareas rara vez se realizan de forma aislada, sino que se realizan en el contexto de un proyecto. En este caso, la unidad de estudio podría ser el proyecto, donde lo interesante es minimizar el impacto en la suma de todas sus tareas.

Y por úlitmo me gustaría resaltar que **nuestra definición de bien cambia con el tiempo**. Tanto nosotros como desarrolladores o equipo, como el proyecto y la empresa para la que trabajamos, nos comportamos como un ser vivo y por lo tanto cambiamos, maduramos y nuestras prioridades cambian y por lo tanto la definición de bien puede (y seguramente sea así) hacerlo también. Es importante tener esto en cuenta tanto a la hora de tomar una decisión, como a la hora de juzgar el pasado. Idealmente deberíamos tener la conciencia tranquila pensando que en cada momento tomamos la decisión que creíamos que era mejor en el momento en el que tomamos la decisión y con el conocimiento que teníamos entonces. Juzgar en el presente, no solo es muy fácil, sino que suele ser bastante injusto.

## Hecho

Sin entrar en la famosa _"[definition of done](https://www.scruminc.com/definition-of-done/)"_, creo que es importante destacar aquí que nos movemos en el mundo de las ideas. Y por lo tanto tenemos una complicación adicional a cuando trabajamos de manera material.

Para empezar, muchas veces estamos haciendo algo que desconocemos por completo. Y otras tantas, lo que nos piden no es real.

¿Alguien por estos lares vería razonable que os pidiera construir un puente desde Lisboa a New York? La cuestión es que me gusta mucho conducir y las vistas al mar me flipan así que me gustaría hacerme "la ruta transatlántica". Además como las vacaciones están cerca, me gustaría construir el puente en un mes, es que si nos pasamos de plazo ya no tiene sentido. ¿Quién se anima? Mirad que es una oportunidad única. !Y un reto super interesante! eso no lo ha hecho nadie y seguro que nos forramos.

Os suena todo esto? Yo lo veo una y otra vez. Y muchas veces cuando se me ocurre decir, _"eso es una locura, ni es viable, ni lo necesitas"_, me miran raro y/o mal. Por supuesto a veces me equivoco y por eso, aunque a veces me expreso mal y sueno tajante, intento ser siempre razonable y estar dispuesto a sorprenderme y a comerme mis palabras por si resulta que la cosa no era tan complicada como pensaba.

Relacionando el concepto de *hecho* con el de *rápido*, o mejor dicho con el *coste*, creo que es interesante pararse a pensar las consecuencias de hacer o no hacer las cosas.

Muchas veces para llegar a tiempo, decidimos implementar únicamente el golden path asumiendo que todo es maravilloso y que las cosas irán siempre bien (cosa que luego nunca pasa). Al final hemos reducido el coste de desarrollo considerablemente a costa de incrementar el coste de mantenimiento más de lo necesario. Por eso debemos tener siempre en cuenta que:

```
coste de feature = desarrollo + mantenimiento + oportunidad
```

Cuando hablo de coste, no me refiero solo a monedas. Sino también a bienestar personal, armonía en el día a día y cualquier valor tangible o intangible. Sabiendo esto, podremos tomar una decisión mucho mejor. En general me atrevería a decir que habitualmente no tendremos que llegar a la perfección casi nunca, pero tampoco deberíamos abusar de las ñapas.


Por último me gustaría dedicar unas líneas para pensar qué hace que una tarea nos consuma tiempo:

1. <span style="text-decoration:line-through">Burocracia</span>
2. <span class="high-priority">**Comprender el problema**</span>
3. <span class="high-priority">**Pensar su solución**</span>
4. <span class="low-priority">Llevarla a cabo</span>
5. <span class="high-priority">**Probarla**</span>

La cuestión es que para mi los puntos <span class="high-priority">2</span> y <span class="high-priority">3</span> son los más importantes con diferencia. En general, es en el momento de comprender el problema cuando tratamos de razonarlo y simplificarlo de manera que la solución sea evidente y llevarlo a cabo (el punto <span class="low-priority">4</span>) sea una tarea relativamente sencilla.

En el momento de pensar es cuando asimilamos los conceptos del negocio que estamos modelando y adquirimos/definimos el vocabulario de negocio que luego será común entre producto, desarrollo y nuestro código.

El problema de todo esto, es que tanto los profanos como, en ocasiones, las personas encargadas del desarrollo, pueden considerar todo ese conocimiento como algo intangible y solo al desarrollarlo se considera que se está trabajando. Si no tenemos algo físico, no vemos el valor adquirido. La consecuencia de este proceder que tenemos tan arraigado, es que cuando tenemos ese sentimiento de urgencia, **de lo primero que prescindimos es de lo más importante: de pensar (puntos <span class="high-priority">2</span> y <span class="high-priority">3</span>) y de probar**. Al final acabamos con algo tangible, pero que hace aguas por todos lados y que nos da unos dolores de cabeza importantes. Pero no pasa nada, lo llamamos deuda técnica y se acabó el problema. Como le hemos puesto un nombre elegante, podemos seguir pa'lante como si nada :P.

**¿Qué podemos hacer al respecto?** Pues en mi opinión, como con cualquier sistema que pretendamos perpetuar, **no debemos maximizar una variable en detrimento de las demás**, ya que sino el sistema acaba volviéndose insostenible.
