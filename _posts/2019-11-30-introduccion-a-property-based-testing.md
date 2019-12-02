---
layout: commented_post
title: Introducción a Property Based Testing
date: 2019-11-29 12:00:00 +0200
categories: blog
excerpt:
  Este post va a ser una, no tan, breve introducción a la estrategia de test basados en propiedades. No voy a entrar en definiciones rigurosas ni en los ejemplos clásicos ya que en internet hay muchísimos otros artículos mucho más detallados y, seguramente, más interesantes que este. Este post, lo escribo como introducción a una serie en la que trataré de detallar cómo implemento, en mi día a día, mi suite de test apoyándome en esta estrategia. Como siempre que escribo, trataré de contar mis impresiones tras su implantación varios proyectos durante los últimos años, qué me ha funcionado y qué no.
---
Quiero empezar este post dando las gracias a [Pedro Gómez](https://twitter.com/pedro_g_s), en primer lugar, porque mucho de lo que escribiré aquí, lo he aprendido junto a él y en segundo lugar, por revisarme siempre los posts antes de publicarlos y aportar un feedback valiosísimo para que éstos transmitan mejor el mensaje. Mil gracias, Pedro :*

**Spolier:** Si leéis hasta el final, os pasaré varios repos donde podréis ver cómo he jugado con distintos frameworks y lenguajes para configurar las distintas opciones que nos brindan :P.

---

_Property Based Testing_ o _Generative Testing_ es una estrategia de pruebas adoptada inicialmente por el paradigma de programación funcional. Para mí tiene tres peculiaridades fundamentales:

* No nos fijamos tanto en el estado de nuestro sistema como en las propiedades que debe cumplir el mismo.
* No tenemos un control absoluto de los datos con los que ejercitamos el sistema.
* Cada uno de nuestros tests se ejecuta múltiples veces con distintos valores.

Su uso tendría más o menos esta pinta:

````scala
"Odd numbers should never be disvisible by 2" in {
  forAll(oddNumberGenerator) { number =>
    number % 2 shouldBe 1
  }
}
````

Super simplificando, una librería de property based testing escrita, por ejemplo, en scala haría algo tal que así:

````scala
trait Generator[T] {
  def gen: Option[T]
}

def forAll(generator: Generator[T], block: (T) => TestResult, options: Opts = Opts.executedAtLeat(100)): TestResult = {
  @tailRec
  def execute(generator: Genrator[T], block: (T) => TestResult, left: Int, result: TestResult) {
    val hasFinished = left == 0 || result.isFailure
    if (hasFinished) {
      result
    } else {
        generator.gen() match {
          case value => execute(generator, block, left -1, block(value))
          case _     => execute(generator, block, left, result)
        }
    }
  }
  execute(generator, block, options,minSuccess, TestResult.success)
}
````

Es decir, en la práctica, escribiremos el código de nuestro test dentro de un bucle que se ejecutará mientras haya datos de prueba con los que ejecutar. Para quienes hayáis trabajado con tests parametrizados, esto os resultará ligeramente familiar.

Pero vamos a indagar un poco más en cada uno de los puntos comentados:

## Nos centramos en las propiedades que nuestro sistema debe cumplir

Este podría ser el punto clave, visto desde la perspectiva de su nombre más común, _Test de Propiedades_. Este aspecto supone un cambio de mentalidad bastante grande para quienes tenemos un background basado en la orientación a objetos. Y en concreto a quienes fundamentalmente dedicamos nuestro día a día a pintar APIs o sacar datos de una base de datos :P.
En nuestra cabeza está muy interiorizado el pensar en acciones y en los efectos que éstas causan y no tanto en ver nuestro software en términos de inputs y outputs.
Evitando poner el clásico ejemplo de las propiedades de la suma, que seguramente encontraréis en cualquier tutorial, voy a poner algunos ejemplos sobre cómo intento plantear yo algunas de las propiedades.

Cuando tengo que **guardar algún dato en una base de datos/API/etc.**, por ejemplo un usuario, mentalmente planteo que tengo una colección de usuarios (el patrón [Repository](https://martinfowler.com/eaaCatalog/repository.html) va muy bien para esto ;)). Con este planteamiento, algunas de las propiedades que debe cumplir mi colección son:

* Cuando añado un usuario a mi colección, ésta debería contener un usuario más.
* Mi usuario existe dentro de dicha colección.

Si os fijáis, hay poco cambio aquí, pero mentalmente comienzas a abrir la puerta a otros escenarios y a otras peculiaridades mirando el software, y sus tests, de otra manera. Para este caso concreto, las propiedades de una colección son casi siempre las mismas, por lo tanto, con lenguajes ligeramente sofisticados, ya podemos reutilizar nuestros tests de propiedades para distintos tipos de datos. **Desde este prisma, lo mismo nos da guardar usuarios, que productos de una tienda así que podemos programar los tests una vez y ejecutarlos en diferentes escenarios** :)

Tras este cambio de mentalidad, **yo ahora tiendo a pensar mucho más en aspectos (propiedades) de mi software como la idempotencia, la operación inversa, etc.** a los que hasta ahora apenas había prestado atención.

Por ejemplo, si quiero comprobar la idempotencia de un método, puedo verificar que ejecutar dos veces el mismo método tiene exactamente las mismas consecuencias que ejecutarlos una vez.

Por otro lado, hay muchos métodos, que a priori, parecen hacer la operación inversa de algún otro método que teníamos. En este caso es importante tener conocimiento sobre si esta propiedad se cumple. Por ilustrarlo con ejemplos:

* ¿Qué pasa cuando serializo un elemento previamente deserializado? ¿obtengo el elemento inicial o es otro diferente? No es que siempre se tenga que cumplir esta propiedad, pero no es raro introducir bugs porque hemos asumido que esto era cierto y resulta que no lo es. Así que el acostumbrarme a pensar en ello me ha hecho ser consciente de las cosas que me pierdo cuando esto no se cumple.
* Cuando añado un elemento a mi colección y luego lo elimino, ¿ha cambiado algo en dicha colección? ¿se han desordenado los elementos, por ejemplo?, ¿me ha quedado algún hueco vacío en la colección?
* ...

## No tenemos control absoluto de nuestros datos

Cómo se veía en el ejemplo inicial, podemos dotar a nuestro dataset de ciertas restricciones o peculiaridades, pero en el momento de escribir el test, no tenemos conocimiento de los datos concretos que recibirá nuestro software.

En este punto, el otro nombre por el cual se conoce a esta estrategia de testing, _Test Generativos_, nos muestra la importancia que tienen los **Generadores**. Aunque cada librería es susceptible de implementarlos de una manera u otra, trataré de describir los aspectos más típicos que me he encontrado:

### Generando nuestro input

Las librerías nos ofrecen generadores de tipos básicos y a partir de ellos, podemos componer generadores más complejos. _Aunque dedicaré más posts a hablar sobre cómo creo yo estos generadores, ilustro aquí, con un ejemplo, dos alternativas para refinar nuestros generadores._:

**Descartar los datos que no cumplen las restricciones** _(puede que el código no compile tal cual, pero espero que os hagáis una idea ;))_:.

````scala
//Generate a number between 5 and 10 with a restrictive approach:
val numberGenerator = Gen.chooseNum(Int.MinValue, Int.MaxValue).filter(_ >= 5 && _ <= 10)
````
vs **Generar datos que cumplan las restricciones deseadas:**
````scala
//Generate a number between 5 and 10 with a non restrictive approach:
val firstAlternative = Gen.choose(5, 10)
val secondAlternative = Gen.oneOf(5 to 10)
````

Como véis, dependiendo de lo complejos o restrictivos que sean nuestros generadores, **existe una posibilidad de que no sean capaces de generar un dato**.  En el primer caso, conseguir que se genere un número entre 5 y 10 partiendo del espectro de los números enteros, prácticamente una cuestión de suerte. Algo que no deberíamos presuponer :).

En este caso es relevante saber cómo reaccionará la librería, ya que hay algunas como [Scalacheck](https://www.scalacheck.org/) que acaba fallando por un alto ratio de descartes (en seguida entramos en detalle sobre este parámetro). Sin embargo otras como [Kotlintest](https://github.com/kotlintest/kotlintest) que se quedarán bloqueadas hasta que encuentren un dato, por lo tanto podrían hacer que nuestra suite de test tardara muchísimo tiempo.

### Replicando una ejecución

Por suerte para nosotros, esta técnica ha sido ampliamente usada por personas que reniegan de los efectos colaterales y los generadores no iban a ser una excepción :). La implicación práctica de esto, es que los **generadores serán capaces de reproducir la estrategia de generación a partir de una semilla**.

Esto hace que otro elemento común en las librerías que implementan los tests de propiedades, es que **podamos repetir una ejecución exacta de nuestros tests** proporcionando una semilla determinada. De este modo, podremos replicar ejecuciones de nuestra suite de pruebas. Esto es especialmente útil a la hora de depurar tests en caso de fallos.
Normalmente, como parte del log de error de los tests, la librería nos proporcionará la semilla utilizada en el generador, **pero como no iba a ser todo tan bonito, os diré que en la práctica, algunas librerías tienen francamente escondida esta opción, mientras que otras lo priorizan sobre otras muchas cosas :)**

### Refinando los casos de prueba

Los generadores que ofrece la librería estándar, suelen implementar una serie de reglas que nos ayudan a probar casos un peculiares.
Por ejemplo:

* cuando requerimos un string, el generador suele generar entre otras, la cadena vacía.
* cuando modelamos un rango de números, el generador suele incluir los extremos.
* cuando necesitamos un número sin restricciones, el generador suele incluir el 0, un positivo y un negativo.
* ...

Con esta estrategia, estamos tratando de encontrar aquellos casos donde el hemos usado la típica comparación de "mayor qué" en lugar de "mayor o igual" y ese tipo de fallitos tontos que nos generan dolores de cabeza :)

Hay librerías como [Kotlintest](https://github.com/kotlintest/kotlintest) que tienen este aspecto muy presente exponiendo un mecanismo para definir los valores específicos que siempre se pasarán en nuestros generadores custom.

### Acotando el error
Al usar datos generados, es muy probable que los valores que ejecuta el test no nos digan nada y nos resulte difícil razonar qué datos generan el error. Para ello las librerías tratan de simplificar y acotar el conjunto de datos que hacen que nuestro test falle. Esto se conoce como [**shrinking**](https://propertesting.com/book_shrinking.html).

_Quiero pensar que es tiene que ver con el hecho de que no he sido capaz de configurar bien este aspecto, pero en el momento en que he usado generadores ligeramente más complejos que un simple tipo básico, **a mi este mecanismo me ha funcionado bastante mal** y no me ha llegado a aclarar casi nada :S. De hecho diría que hasta ha generado confusión, porque en algunas librerías, como scalacheck, el shrinking se salta algunas restricciones de los datos generándote casos de prueba que no deberían haberse ejecutado. Esto hacía que se generasen errores, a priori imposibles en el flujo que se estaba probando con el consiguiente rato pensando WTF!!!._

## Nuestro sistema se ejecuta múltiples veces

A mi se me ocurre que, al menos, es importante dedicar un rato a pensar sobre las siguientes cuestiones:

1. **El tiempo de ejecución de nuestros test se multiplica por el número de ejecuciones:** Teniendo en cuenta que la programación funcional maximiza la cantidad de código libre de efectos colaterales, este tipo de tests suelen ser bastante rápidos ya que nos quitamos las operaciones de entrada/salida, que suelen ser una fuente habitual de incrementos de tiempos de ejecución. No obstante, es relevante tener en mente este aspecto de los tests basados en propiedades, ya que si nuestra suite es lenta, aplicar esta técnica, podría ralentizar aún más la ejecución. En este aspecto, es importante saber cuándo y cuándo no escribir este tipo de tests. En la práctica, yo no he notado un incremento importante en el tiempo de ejecución de mis tests.

2. **¿Cuántas veces es "múltiples" veces?:** En general, todas las librerías suelen tener un parámetro de configuración _"min success"_ o similar que nos dice cuantas veces habría que ejecutar nuestros tests. No obstante, a veces también se introducen otros parámetros como _"max discarded"_, para el caso en el que hagamos generadores muy restrictivos, esto haría fallar el test en lugar de congelarlo hasta que se encuentre un valor que cumpla la restricción.

3. **La idempotencia en mis tests cobra una importancia fundamental:** de las propiedades [FIRST](https://hackernoon.com/test-f-i-r-s-t-65e42f3adc17) tan deseables para nuestros tests automáticos cobran especial importancia las tres primeras. _Fast_, **Isolated** y **Repeatable**. Si voy a ejecutar varias veces mis tests, es importante que mi SUT (system under test) mantenga el estado entre ejecuciones. O al menos que, como mínimo, nuestros tests no asuman un estado previo para poder ser ejecutados. De otra manera, no podremos ejecutar varias veces nuestros tests.

## Conclusiones

Os dejo por aquí mis conclusiones sobre esta estrategia de pruebas. En los siguientes posts intentaré detallar algunos aspectos que os podrían resultar útiles para comenzar a incluir esta técnica en vuestra suite de pruebas.

* En mi experiencia tras varios workshops explicando esta técnica, al principio nos cuesta entender que los tests se ejecutan varias veces con datos diferentes y que no tengamos control sobre los datos. Pero no desesperéis, se acaba asimilando ;).
* No tener conocimiento, en tiempo de escritura de los datos específicos, hace que nuestro planteamiento de test cambie. En concreto hay que echarle imaginación a la fase de las aserciones.
* No hace falta ser 100% puristas para sacarle provecho a esta técnica. Conociendo los conceptos fundamentales que implementan las librerías, podemos tomar solo lo que necesitemos. Por ejemplo, yo implemento mis [Object Mother](https://martinfowler.com/bliki/ObjectMother.html) con los generadores. Incluso hemos llegado a usar dichos generadores para crear fixtures para nuestro entorno de desarrollo :) (ideaca de [Pedro](https://twitter.com/pedro_g_s)).
* Esta estrategia **NO sustituye** nuestra suite de tests tradicional. Es deseable implementar determinadas pruebas con valores específicos y conocidos ya que no siempre seremos capaces de encontrar el juego de propiedades completo de nuestro sistema.

-------

_Y como lo prometido es deuda, a los que hayáis llegado hasta aquí, os dejo un enlace al repositorio del taller de [Property Based Testing](https://github.com/delr3ves/PropertiesWorkshop) que preparé en su momento. Tenéis, además, las soluciones propuestas a los ejercicios en 5 lenguajes diferentes con sus distintos frameworks. Para mi es especialmente útil, el punto ["Jugando con la librería"](https://github.com/delr3ves/PropertiesWorkshop#jugando-con-la-librer%C3%ADa-20) ya que muestra cómo configurar las distintas librerías. Muchas de estas configuraciones no están bien documentadas y hay que indagar bastante, por lo cual muchas veces acabamos por desistir así que creo que es interesante tener las soluciones a mano para ahorraros tiempo._
