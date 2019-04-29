---
layout: commented_post
title:  "Los singletons no son malos, es la sociedad que los corrompe"
date:   2015-06-04 13:00:00 +0200
categories: blog
excerpt:
  Que levante la mano quien haya implementado en algún momento el patron singleton.
  <br/>
  Ahora que levante la mano quien no se haya arrepentido de esa decisión.
  <br/>
  Finalmente que levante la mano quien no vaya diciendo por ahí que el singleton es [evil|crap|el mal|un antipatron|…].
  <br/>
  Bien, si llevas un rato con la mano levantada es posible que seas parte de esa sociedad que corrompe a los pobres singletons.
---

Que levante la mano quien haya implementado en algún momento el patron singleton.

Ahora que levante la mano quien no se haya arrepentido de esa decisión.

Finalmente que levante la mano quien no vaya diciendo por ahí que el singleton es [evil\|crap\|el mal\|un antipatron\|…].

Bien, si llevas un rato con la mano levantada es posible que seas **parte de esa sociedad que corrompe a los pobres singletons.**

Muy rápidamente, según wikipedia:

>El patrón de diseño singleton (instancia única) está diseñado para restringir la creación de objetos pertenecientes a una clase o el valor de un tipo a un único objeto.

## Las llamadas a getInstance son el mal
Además como detalle de implementación típicamente los singleton proveen un método estático llamado getInstance. ¿Te suena? Este bonito método tan fácil de invocar suele ser la razón fundamental por la que la has liado con este patrón.

A lo largo de mi carrera he hecho alguna que otra entrevista de trabajo. En casi todas ellas me han preguntado por patrones de diseño. Casi siempre ha salido a la palestra el Singleton y casi siempre surgen conversaciones del tipo: “el problema de los singletons es que hace que el códgio sea menos testable”…

Bien, **tu código no debería ser menos testable porque utilices un singleton. Es menos testable porque invocaste a getInstance donde no tenías que invocarlo.**

Si te dedicas a obtener la instancia del objeto allá donde lo necesites sin tratar dicha instancia como una dependencia más, entonces tienes un código difícil de probar (al menos unitariamente). Aquí tienes la diferencia:


{% highlight java %}
public User create(User user) throws InvalidUserException {
 	if (isValid(user)) {
 		UserDao userDao = UserDaoSingleton.getInstance();
 		return userDao.create(user);
 	}
 	throw new InvalidUserException();
}
{% endhighlight %}

{% highlight java %}
public User create(User user) throws InvalidUserException {
	if (isValid(user)) {
		return this.userDao.create(user);
	}
	throw new InvalidUserException();
}
 {% endhighlight %}

 El problema de la primera implementación es que llama al método getInstance dentro del método haciendo que sustituir la instancia de UserDao por un doble de pruebas sea muy complicado.

 Además hay un problema añadido. Muchas veces, por el hecho de que **obtener cierto tipo de instancias objetos típicamente complicados de crear**, como conexiones a bases de datos, clientes rest, etc. sea sencillo es muy probable que en algún momento acabes haciendo código espageti. **Es fácil mezclar la lógica de negocio con la lógica de creación.**


## ¿Para qué necesitas realmente un singleton?

El hecho de que necesites una y solo una instancia de una clase concreta en tu aplicación es algo que debería hacer saltar todas tus alarmas. A priori yo intentaría buscar otra alternativa. El hecho es que si utilizas singletons para mantener servicios con estado, estas generando una deuda técnica que te resultará muy cara y dificil de pagar en pocas semanas. **Si necesitas el singleton para guardar estado, lo que estas tienes en realidad es una bonita variable global un poco más ¿sofisticada?**

Trata de buscar una alternativa, porque eso más que mal olor es un olor terrible!


## ¿Cuándo es bueno usar un singleton?

La respuesta más eviedente es: **cuando necesitas objetos que son caros de crear.**

Si le echas un ojo al GOF, verás que es un patrón de creación no de comportamiento, así que a priori esta parece una respuesta coherente.

Ya, pero ¿solo se es bueno usarlo para eso? Por la parte que me toca yo también lo uso por ejemplo para tener un cacheo de primer nivel de ciertas operaciones costosas. Pero no diseño teniendo en cuenta que ese dato va a estar ahí cacheado y en memoria. Diseño de manera que mi aplicación sea agnóstica a esta decisión.

Vamos, que diseño pensando que la instancia que me venga puede ser cualquiera. Y luego en la parte de mi aplicación que se encarga de la lógica de creación (generalmente el injector de dependencias) ya decido si quiero un singleton o no por ejemplo, por cuestiones de eficiencia.


## ¿Cuándo es una malísima idea utilizar un singleton?
Cuando mi aplicación asume que yo voy a tener una instancia determinada en un estado determinado para poder funcionar.

Esta solución la he visto especialmente en código PHP donde el universo se reconstruye para cada petición “nivel de request”.

Pongo un ejemplo:

Vale, el ejemplo es un poco mierder, pero no se me ocurre nada mejor ahora mismo :P.


![]({{ site.baseurl }}/assets/posts/2015-06-04/singleton-sequence-diagram.png)

Básicamente tenemos una clase que setea un usuario para que luego otra clase pueda conseguirlo. Esto es un fallo claro de diseño y en este caso específico es muy fácil de refactorizar. Simplemente haciendo viajar los parámetros que hacen falta por el flujo de ejecución. Soy consciente de que hay gente que tiene pereza de esta solución pero no hacerlo es un error por muchas razones que se escapan del objetivo de este post así que lo resumiré simplemente diciendo que me parece sano y deseable que nuestras clases pidan la información que se necesita de manera explícita y los métodos funcionen de manera aislada sin depender de que alguien antes haya ejecutado un método previo.

Bueno, creo que me estoy enrollando de más.

## Conclusiones

Por concluir me gustaría dejar claro que:

* **Singleton NO es lo mismo que getInstance**
* **Se deben evitar las llamadas a getInsance** dentro de un método que contiene lógica de negocio.
* **No diseñes tu aplicación para que dependa de un estado previo**
* **No asumas que la dependencia con la que colaboras será siempre la misma**

Si tienes en cuenta todo esto, el patron Singleton se vuelve un patron útil que te proporcionará ventajas sin los inconvenientes de los que la gente suele quejarse.
