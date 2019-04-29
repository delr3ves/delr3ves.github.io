---
layout: commented_post
title:  "La importancia del contexto"
date:   2019-04-28 12:00:00 +0200
categories: blog
excerpt:
  Cada cierto tiempo me entran muchas ganas de poner en orden mis pensamientos, recapitular sobre las situaciones que me encuentro
  en el día a día en mi trabajo y esta vez he pensado, ¿por qué no? voy a intentar plasmarlo en un post.
  <br/>
  De lo que quiero escribir hoy es de la importancia de contextualizar lo que aprendemos. Ya lo fui adelantando con posts como
  <a href="/blog/2015/01/30/de-leer-libros-mal-y-de-joderlos.html">de leer libros mal y de joderlos</a> o <a href="blog/2017/03/25/malos-habitos-equipo-desarrollo-software.html">malos hábitos en un equipo de desarrollo software</a>. Pero creo que por fin he encontrado una forma de expresarme mejor.
  <br/>
  Mi objetivo con este post es intentar transmitir unos conceptos básicos que nos permitan pensar y cuestionar muchas de las citas de autoridad que asumimos como ciertas en todo momento y que a veces podrían no aplicar.
---
Mi objetivo con este post es intentar transmitir unos conceptos básicos que nos permitan pensar y cuestionar muchas de las citas de autoridad que asumimos como ciertas en todo momento y que a veces podrían no aplicar.

Me gustaría comenzar este post con la que yo creo que es una de las mejores definiciones de inteligencia que conozco:

> La inteligencia es la capacidad de distinguir lo fundamental de lo accesorio.

Esa definición se la escuché a mi padre y me pareció una manera mucho más elegante y escueta a lo que yo solía utilizar que es algo del estilo de:

> La inteligencia está relacionada con la cantidad de variables que tienes en cuenta a la hora de tomar una decisión y cuales de ellas consideramos representativas

La primera definición me gusta mucho, sobre todo aplicada al desarrollo software ya que se acerca mucho a las caracteristicas  **esenciales y accidentales** que Brooks rescata de Aristóteles en su ["No Silver Bullet"](http://www.cs.nott.ac.uk/~pszcah/G51ISS/Documents/NoSilverBullet.html) y [Javier Garzás sintentiza bastante bien](https://www.javiergarzas.com/2009/05/del-conocimiento-esencial-y-del.html).

La dificultad de todo esto, es que en muchos casos, lo esencial no es inmutable sino que cambia; puede variar. Es decir, las variables representativas suelen depender del contexto en la mayoría de las ocasiones.

Disculpad si me alargo, pero para continuar tengo que dar unas definiciones relacionadas con la teoría de los tipos lógicos que nos hablan de varios niveles para interpretar la información:

## Dato (o Técnica)

Un dato parte de una observación y determina una unidad de información que podríamos considerar objetiva. Para mi, que me rijo bajo la premisa de que no hay observación sin observador, el nivel de dato sería el único nivel en el que cabe la objetividad.

Lo intento ilustrar con un ejemplo partiendo de esta mesa:
![](/assets/posts/2019-04-28/mesa-verde.jpg)

_En principio estaremos todos de acuerdo en que esto es una mesa, no? Seguramente si, porque es un dato, es objetivo y hace que todos tengamos, a priori, la misma interpretación._

## Contexto (o Metodología)

Hablando del contexto es importante tener claro que todo dato se da en un contexto, es decir, en una serie de circunstancias que rodean a aquello que estamos observando.

_Si por ejemplo yo os preguntara si esta mesa está suficientemente limpia y es adecuada, es probable que la mayoría contestéis que sí, que de hecho es una mesa bastante molona :P._

_Pues bien, ¿si os dijera que esa mesa está en un quirófano y una vez se retiren los platos se va a operar a alguien ahí? seguramente ya no nos parece ni tan limpia ni tan cool, ¿no? más bien todo lo contrario._

_También estaremos de acuerdo casi todos en que el mantel de la mesa es verde, ¿verdad? ¿Qué pensáis que nos contestaría una persona daltónica?_

**El contexto hace que nuestra interpretación del dato cambie por completo.**

Nada nuevo bajo el sol hasta este momento, ¿no?

## Epistemología

Ahora es cuando entra en juego una de las grandes desconocidas... la epistemología. Simplificando mucho podríamos definirla como **nuestra forma de ver la vida**.
Está muy relacionada con nuestra **escala de valores** y nuestras **prioridades**. En función a qué le damos importancia sacaremos unas conclusiones u otras.
La epistemología **es la responsable de cómo nosotros generamos e interpretamos contextos**.

En concreto la epistemología opera de la siguiente manera:

1. **Conocemos**
2. **Encajamos** lo que conocemos en **nuestra teoría descartando**, a menudo de manera inconsciente, **lo que no cuadra con nuestra teoría**.
3. **Actuamos** según lo anterior
4. GOTO 1.

Con respecto al punto 2. [Humberto Maturana](https://es.wikipedia.org/wiki/Humberto_Maturana) decía que

> Somos un sistema informacionalmente cerrado

es decir, toda la información que no somos capaces de asimilar se queda fuera del sistema.

_Volviendo a nuestra mesa, en un contexto de un restaurante donde vamos a comer... ¿consideráis que está suficientemente limpia? para la mayoría seguramente si. Sin embargo si pensamos en Jack Nicholson en Mejor Imposible, es bastante probable que él nos dijera que no come ahí ni de broma. Que ese "cesped" es un nido de gérmenes._

En mi opinión, **la interpretación viene dada por la terna (dato, contexto, epistemología)** y si algo en esa terna cambia, la interpretación cambia.



### **¿Por qué esto es tan importante para hacer software?**

Para mi esto es interesante en la vida en general y en el contexto del software en particular porque, sobre todo cuando empezamos, somos seducidos por las técnicas, que cómo veis están a nivel del dato.

En este aspecto, la técnica entendida como un conjunto de pasos para llevar a cabo una acción nos da poca información sobre el contexto por lo que si solo conocemos la técnica, no sabemos cuándo aplica y cuándo no. Estamos como locos buscando esas _Balas de Plata_ de las que habla [Brooks](https://en.wikipedia.org/wiki/Fred_Brooks) y muchas veces hasta nos permitimos el lujo de evangelizar a todo nuestro entorno porque eso nos parece maravilloso.

He tenido muchísimas conversaciones con  [@pedrovgs](https://twitter.com/pedrovgs), [@jjtorroglosa](https://twitter.com/jjtorroglosa), [@serchinastico](https://twitter.com/serchinastico) y otras tantas con [@manolovn](https://twitter.com/manolovn) y [@Tylosan](https://twitter.com/Tylosan) al respecto y mi conclusión es que, **como desarrolladores, programar y utilizar la técnica es nuestra última opción; es un detalle de implementación. Todo empieza por comprender las necesidades de nuestro cliente, cuestionarlas, masajearlas y simplificarlas tanto que la programación sea sencilla (a ser posible hasta innecesaria)**.

Conocer nuestra epistemología y la de nuestro entorno nos posibilita tener en cuenta más variables y más relevantes a la hora de tomar una decisión y veremos que dependiendo del contexto y de con quién trabajemos, lo que tiene todo el sentido del mundo en un momento dado, deja de tenerlo si las circunstancias cambian. Es decir, tendremos la posibilidad de tomar decisiones con inteligencia.

En los próximos posts intentaré dar ejemplos sobre cómo conocer la epistemología y el contexto, tanto de nuestro proyecto como de nuestros compañeros, puede hacer que tomemos decisiones que de otra manera habríamos obviado.
