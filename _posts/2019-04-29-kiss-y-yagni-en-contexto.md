---
layout: commented_post
title:  KISS y YAGNI en contexto
date:   2019-04-29 12:00:00 +0200
categories: blog
excerpt:
  <p>Tras hablar de <a href="/blog/2019/04/28/la-importancia-del-contexto.html">la importancia del contexto</a> es el momento de ver cómo de útil es esto de la epistemología para decidir cuándo aplica y cuándo no.
  </p>
  <p>
  Así que quiero aprovechar este post para darle contexto a los principios KISS y YAGNI y así no acabar <a href="/blog/2015/01/30/de-leer-libros-mal-y-de-joderlos.html">leyéndolos mal y jodiéndolos</a>.
  </p>
---

Tras hablar de [la importancia del contexto](/blog/2019/04/28/la-importancia-del-contexto.html) es el momento de ver cómo de útil es esto de la epistemología para decidir cuándo aplica y cuándo no.
Así que quiero aprovechar este post para darle contexto a los principios KISS y YAGNI y así no acabar [leyéndolos mal y jodiéndolos](/blog/2015/01/30/de-leer-libros-mal-y-de-joderlos.html).

Antes de nada, para quienes no estéis familiarizados con estos principios:

## KISS (Keep it simple, stupid)

El principio de diseño [KISS](https://en.wikipedia.org/wiki/KISS_principle) nos viene a decir que un sistema funciona mejor si se mantiene simple. Y ya que hablamos de simplicidad, no podemos dejar de hablar de complejidad, para lo cual os recomiendo echarle un ojo a [Out of the tar pit](http://curtclifton.net/papers/MoseleyMarks06a.pdf) de Ben Moseley y Peter Marks. En él se habla bastante extensamente de la complejidad y toman de Brooks la distinción clara entre:

* **Complejidad esencial**: aquella que es intrínseca al problema a resolver.

* **Complejidad accidental**: la que introducimos nosotros de manera artificial por la forma en que resolvemos el problema.


## YAGNI (You aren't gonna need it)

El principio [YAGNI](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it) nos dice que no deberíamos añadir más funcionalidad de la estrictamente necesaria. Relacionado con este princpio, pero desde la perspectiva de producto, tenemos también el concepto de [MVP (Minimum Viable Product)](https://en.wikipedia.org/wiki/Minimum_viable_product).


Bien, estos dos principios parecen de lo más razonables, ¿no? **no compliques tu programa** y **no hagas más de lo necesario**. Lo que no es tan fácil de ver es cuándo algo es más complicado de lo que debería o cual es la funcionalidad justa y necesaria.

## Con respecto a la complejidad:

* Hay quien dirá que no merece la pena habilitar las nuevas funcionalidades con [feature flag](https://www.martinfowler.com/articles/feature-toggles.html).
* Cuando el desarrollador no tiene conocimiento de los principios de diseño, seguramente hacer una abstracción para ocultar el hecho de que vamos a usar un motor de persistencia es complicado. Así que ¿quién quiere implementar un _[Repository](https://www.martinfowler.com/eaaCatalog/repository.html)_ o un _[DAO](https://en.wikipedia.org/wiki/Data_access_object)_ pudiendo llamar directamente al _entity manger_ en caso de que usemos un _[ORM](https://en.wikipedia.org/wiki/Object-relational_mapping)_? Hay quien podría pensar que esa abstracción es compleja, que no merece la pena añadirla.
* Quienes tengan el convencimiento de que escribir tests te va a hacer el desarrollo más lento y no estén acostumbrados a trabajar con [inyección de dependencias](https://martinfowler.com/articles/injection.html?) para hacer su código testable, podrían justificar bajo el principio KISS que no hace falta todo esto.
* Ya ni entro a hablar de las [type classes](http://learnyouahaskell.com/types-and-typeclasses) de programación funcional que tiene asustados a un tercio de los desarrolladores que conozco. Mientras que otro tercio promete que es una forma sencilla y elegante de resolver problemas de manera robusta...


Todos estos ejemplos los he vivido en un lado u otro de la conversación. Y es que tenemos que entender que la mayoría de **las prácticas, principios y técnicas que utilizamos en nuestro día a día nos suelen resultar sencillas porque las conocemos y las que no conocemos nos pueden parecer muy complejas**. Pero esto no implica que el compañero que tenemos al lado no sepa de lo que está hablando y le resulte sencillo llevar a cabo la tarea. Así que, por favor, antes de descartar algo por el hecho de que añada complejidad accidental al problema (seguramente no uséis estas palabras, pero si algo del estilo de "_esto ahora no merece la pena_" o "_es que esto complica mucho todo_") dadle una oportunidad al que dice saber hacerlo. Eso si, si al final vais por la solución "compleja", procurad que el resto del equipo la comprenda para minimizar esa complejidad al máximo.


## Con respecto a anticipar:

El princpio YAGNI viene a decirnos **no anticipemos**. Para justificar la importancia de esto, voy a cambiar radicalmente de tema y os voy a hablar de la ansiedad (que para algo estoy rodeado de psiquiatras en casa :P).

En palabras de mi padre, el _Dr. Arroyo_ que lleva casi 40 años tratando la ansiedad:

> La ansiedad es la necesidad de más control del que la situación nos permite.

Cuando la ansiedad tiene un origen epistemológico, es decir por nuestra educación y forma de ver la vida, la seguridad es el valor más prioritario en cualquier situación. En este caso, es probable que nosotros mismos quienes nos generamos la ansiedad. Y lo hacemos fundamentalmente mediante tres mecanismos:


1. **Preguntarse por qué**: en un sistema biológico/estadístico/social y en general complejo donde la cantidad de variables que manejamos es tan grande que en la práctica es inviable formular un modelo causa-efecto. En estos casos nuestro sistema se comportará de manera no determinista por lo que en general no podremos prever lo que pasará, ni que nos llevó al lugar en el que estamos ahora mismo, sino que tendremos que conformarnos con acciones/explicaciones que inclinen la balanza hacia el lado que más nos interesa. Pero nunca al 100%.

2. **Controlar lo incontrolable**: es la propia definición de la ansiedad, y **tendremos que conformarnos con tener, únicamente, el control de nuestro actos. Nada más.** Ni los actos de los que nos rodean, ni nuestros pensamientos ni mucho menos los suyos están bajo nuestro control, así que intentar controlarlos muy probablemente incurra en un esfuerzo inútil.

3. **Anticipar**: en este aspecto vemos como muchas veces gastamos una cantidad de recursos para evitar que ocurra algo que de todas formas era poco probable que ocurriera. Para lidiar con la necesidad de anticipar, podríamos hacer el ejercicio, consciente al principio y automático después, de pensar si es posible que ocurra eso que estamos anticipando. Y de ser posbile, debemos planternos si es probable.

Este último mecanismo es el que le da contexto y sentido al principio YAGNI. Sin embargo últimamente veo con demasiada frecuencia a compañeros evitando hacer ciertas cosas que hasta ahora se habían considerado importantes bajo la excusa del YAGNI y del KISS. Prácticas como la [integración continua](https://martinfowler.com/articles/continuousIntegration.html), la automatización de ciertas tareas (como [análisis de código](https://en.wikipedia.org/wiki/Static_program_analysis), gesitón de migraciones de bases de datos, etc.), desarrollar bajo los principios de la [arquitectura hexagonal](https://marcus-biel.com/hexagonal-architecture/), o la gestión de copys, etc. quedan relegados a un segundo plano porque _todavía no nos hace falta todo esto_.

A mi esto me parece un problema grande y os comento mi razonamiento cuando me planteo si debo hacer una tarea ahora o más adelante por si os sirve a vosotros también:

Cuando la tarea no necesita hacerse inmediatamente pienso: **¿cuánto me cuesta hacerlo ahora?** y **¿cuánto me costaría hacerlo en el futuro?**. Si el esfuerzo es parecido... ya lo haré cuando lo necesite. Sino ya empiezo a plantearme: **¿es posible que lo necesite en el futuro?** y **¿es probable?**. Si además de ser más caro hacerlo más adelante, es posible y es probable que lo tenga que hacer, creo que habría que plantearse seriamente hacerlo ahora.

Si os fijáis todos los ejemplos que he puesto anteriormente son relativamente sencillos de llevar a cabo al principio, pero pueden volverse inviables según se va retrasando la tarea.

Tras estas reflexiones me gustaría dejar claro que ambos principios de diseño rigen mi día a día y que me parecen fundamentales. Pero, como siempre, es bueno saber en qué contexto aplican y en cual debemos obviarlos. Una vez que tenemos los recursos suficientes para decidir si finalmente es importante hacer cierta tarea o no, yo soy muy partidario de intentar reducir las necesidades del negocio y la funcionalidad de nuestro sistema todo lo que se pueda. Si logramos un compromiso entre el equipo de desarrollo y el de producto, todos viviremos mucho mejor ;)
