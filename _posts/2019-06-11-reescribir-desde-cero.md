---
layout: commented_post
title: ¿Reescribir desde cero? Yo me lo pensaría dos veces
date: 2019-06-10 13:00:00 +0200
categories: blog
summary:
  A lo largo de mi carrera me han pedido reescribir desde 0 todo el proyecto en el que trabajaba al menos 4 o 5 veces. Voy a intentar contaros mi experiencia por si os puede resultar útil.
excerpt:
  <p>A lo largo de mi carrera me han pedido reescribir desde 0 todo el proyecto en el que trabajaba al menos 4 o 5 veces. Voy a intentar contaros mi experiencia por si os puede resultar útil. Os adelanto las conclusiones por si no os apetece leer toda la retahíla. Para mi lo más importante de todo es que <span class="soft-emphasis">aprender a refactorizar y lidiar con código que nos da asco, nos aporta unos skills mucho más valiosos como desarrolladores que ser el hecho de ser la repera limonera escribiendo código nuevo.</span></p>
---
A lo largo de mi carrera me han pedido reescribir desde 0 todo el proyecto en el que trabajaba al menos 4 o 5  veces. Voy a intentar contaros mi experiencia por si os puede resultar útil. Como el post es un poco largo he tratado de dejaros las conclusiones lo más arriba posible y ya si tenéis más interés en cada caso específico he tratado de detallarlo algo más para que entendáis mi razonamiento.

No obstante, quiero aclarar a qué me refiero cuando hablo de reescribir desde cero. Así estaremos en la misma página ;)

Para mi reescribir de cero es comenzar un nuevo proyecto que debería hacer lo mismo que el proyecto que ya tenemos. Que implementamos siguiendo la estrategia de todo o nada, es decir, que hasta que no esté listo no podrá ponerse en producción y que su ejecución es incompatible con la del código actual.

**TLDR;**

---
Os adelanto las conclusiones por si no os apetece leer toda la retahíla. Para mi lo más importante de todo es que:
>Aprender a refactorizar y lidiar con código que nos da asco, nos aporta unos skills mucho más valiosos como desarrolladores que ser el hecho de ser la repera limonera escribiendo código nuevo.

* Me parece **irresponsable comenzar un proyecto a "todo o nada"** donde si al final hay que parar la reescritura, apenas podremos sacar algo de valor.
* Salvo contadas excepciones, suele haber una alternativa mejor a reescribir TODO el software.
* **Reemplazar las partes que nos molestan** suele ser más prudente y no necesariamente mucho más lento.
* **Pretender reescribir** el software, que otra gente ha hecho con mucho esfuerzo, más rápido y mejor me parece, en general, un acto de **soberbia**.
* Lo más probable que te entrampen los "poyaques". __"Po ya que estamos, hazme no sé que funcionalidad..."__.
* Si necesitamos reescribir el código porque no podemos hacernos con el control del código actual, **posiblemente estaremos condenados a una reescritura tras otra**.
* Si no somos capaces de entender qué hace el código actual, ¿cómo pretendemos replicar la funcionalidad actual?
* El punto anterior genera un problema serio ya que es muy probable que **nadie tenga el conocimiento completo de lo que hace nuestro producto actualmente.** Y quizás quien lo tenga ya esté cansado de decirlo (durante el desarrollo original) y pase muchas cosas por alto al contártelo durante la reescritura.
* Normalmente, no congelar la funcionalidad actual, hace prácticamente inviable la reescritura, pero congelarla, hace prácticamente inviable el negocio.

---

Por si os interesa mi razonamiento más en detalle:

Así a a grandes rasgos me planteo dos aspectos importantes a tener en cuenta. **¿está alguien usando el software que queremos reescribir?** y por otro lado, **¿va a reescribir el software el mismo equipo que se encarga del código actual?**

La primera pregunta es relevante, porque cuando nadie usa nuestro software, la reescritura en general se vuelve más sencilla. En lugar de un refactor puro en el que la funcionalidad se mantiene intacta, se puede plantear un proyecto nuevo. Idealmente no partimos de cero porque alguien tendrá algo de conocimiento, pero en general un proyecto nuevo genera mucha más ilusión que reescribir la misma cosa de otra manera.

Con respecto al equipo. Cuando veo que un equipo quiere reescribir desde cero el software que ha escrito previamente me planteo lo siguiente: **¿qué ha cambiado para que ahora sepas escribir un software que no se te vaya de las manos de nuevo?** y lo qué es peor... **¿si ya sabes escribir software de la forma que te gusta, por qué no empiezas a hacerlo ya?** Es cierto, que un equipo que ya conoce el problema puede aprovechar ese conocimiento adquirido sobre el dominio y simplificar el diseño. Pero también habría que cuestionarse por qué ese equipo no es capaz de controlar su software.

Si por el contrario, el equipo encargado de la reescritura es completamente nuevo (he visto como se contrataba a _rockstar, ninja, hiper-mega-maxi-top-developers_ para estas cosas), tendremos el problema de que no conocen el negocio y sus particularidades, que suelen traducirse en forma de condiciones raras en ese código que se quiere tirar. **¿Podemos prescindir de todo ese conocimiento?**

Un equipo mixto de gente que ha estado en el desarrollo previo junto nuevas personas que aporten una visión sobre cómo trabajar podría ser una buena mezcla. Aunque no creo que garantice el éxito.

**El software tiende a la entropía**. Es una cuestión de probabilidad, hay muchas más alternativas que no nos gustan de las que si nos gustan. Sinceramente creo que es mejor dedicar el tiempo a aprender a lidiar con esta entropía, a adquirir un gusto más variado y ser más tolerantes con el trabajo de los demás (o con el de nuestro yo del pasado).

En general cuando ponemos un fragmento de código suficientemente grande en producción es susceptible de dar problemas. Siempre hace falta algo de fine tuning, de optimizaciones, de resolver problemas de performance y problemas de negocio por malos entendidos... Cuando este sistema está arrancando, los pocos usuarios que hay suelen ser más permisivos, pero cuando es un sistema que ya funcionaba, justificar estos problemas de cara a un negocio que lleva un tiempo parado por la reescritura y a usuarios que esperan nuevas funcionalidades suele ser difícil. Si a esto se junta que en general la reescritura se suele promocionar de manera interna como "la solución a nuestros males" el chasco es doblemente mayor. **Rara vez cumpliremos las expectativas.**

### ¿Mi propuesta?

Casi siempre es más sensato **seguir un desarrollo incremental e ir reemplazando las piezas que más nos molestan poco a poco**. Hacer el esfuerzo de comprender donde estamos fallando y cómo podemos aportar valor con nuestros cambios. Poco a poco iremos recuperando el control del sistema y, seguramente, la confianza por parte de los equipos no técnicos.

Creo que con esto dejo clara mi opinión al respecto.
>Reescribir desde cero no me parece una buena idea casi nunca!

No obstante, por si os interesa mi experiencia, he resumido en [otro post](/blog/2019/06/10/mi-experiencia-tras-varias-reescrituras) los intentos de reescritura en los que he formado parte que ahora mismo me vienen a la mente
