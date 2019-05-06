---
layout: commented_post
title:  Lo puedo hacer rápido o lo puedo hacer bien
date:   2019-05-06 12:00:00 +0200
categories: blog
excerpt:
  De todas las creencias del ideario informático, una de las que más me duele cuando la escucho es la archiconocida <q>Lo puedo hacer rápido o lo puedo hacer bien</q>. En este post intentaré explicar que no es bueno maximizar una cualidad (bien) en detrimento de las demás (rápido). O viceversa...
---
De todas las creencias del ideario informático, una de las que más me duele cuando la escucho es la archiconocida:

>Lo puedo hacer rápido o lo puedo hacer bien

con una de sus variantes estrella:

>Esta feature son dos días, pero con test cuatro

a lo que muchas veces desde producto nos contestan:

> ¿pero cómo tardas tanto? (si eso poner un if)

y eso ya hace que nos hierva la sangre imposibilitando la conversación...

Con este post pretendo exponer que, desde mi punto de vista, la calidad y la velocidad no están reñidas en el desarrollo software. Sino más bien al contrario. Para ello voy a intentar desgranar los tres conceptos importantes de los que habla la primera frase.

## Rápido

<details>
<summary class="collapsible-title">Cuéntame la versión larga, please :).</summary>
  <div class="collapsible-details">
    {% include long-version/lo_podemos_hacer_rapido.html %}
  </div>
</details>


**TLDR;**

---
_Es importante entender que la mayoría de las veces, cuando se quieren las cosas rápido, no es porque haya una urgencia real. Lo que se quiere es abaratar el proceso de desarrollo ya que el coste principal del proyecto se debe al coste de las personas implicadas en el mismo. De este modo conoceremos la motivación de la persona que transmite urgencia para así poder tener una conversación más adecuada._

_Por otro lado, lo más común que haya cierta/bastante incertidumbre durante el desarrollo por lo que es muy difícil predecir el tiempo que tardaríamos con una alternativa u otra. Y sino somos capaces de saber cuál es el tiempo razonable de desarrollo, ¿cómo podríamos saber que significa rápido o lento?_

---

## Bien

<details>
<summary class="collapsible-title">Cuéntame la versión larga, please :).</summary>
  <div class="collapsible-details">
  {% include long-version/lo_podemos_hacer_bien.html %}
  </div>
</details>

**TLDR;**

---
_No creo que haya un criterio fijo para determinar que algo está bien hecho. Es más, a menudo, **nuestra definición de bien cambia con el tiempo**. Sin embargo, deberíamos tratar de estar contentos con nuestras decisiones y nuestro trabajo._

_Debemos conformarnos con hacer las cosas lo mejor que sepamos en el momento en que las hacemos y en las condiciones en que las hacemos. No obstante, es responsabilidad nuestra intentar negociar tanto con el alcance de la tarea como con el tiempo de la misma antes de decidir no hacer "las cosas bien"._

---


## Hecho

<details>
<summary class="collapsible-title">Cuéntame la versión larga, please :).</summary>
  <div class="collapsible-details">
    {% include long-version/lo_podemos_hacer.html %}
    </div>
</details>

**TLDR;**

---
_Nos movemos en el mundo de las ideas y por lo tanto, no nos regimos por las limitaciones físicas. Esto hace que se nos planteen ideas que o bien son bastante laxas o bien no son viables. Además, es posible que la idea que nos comentan se pueda llevar a cabo en varias fases sin necesidad de implementarla entera. No olvidemos, que la mayoría de las veces podemos plantear el proceso de desarrollo como iterativo e **incremental** por lo tanto es buena práctica comprender cuál es la parte imprescindible de la tarea y de cuál podemos prescindir en una primera versión._

---

## Conclusiones

* Pues en mi opinión, como con cualquier sistema que pretendamos perpetuar, **no debemos maximizar una variable en detrimento de las demás**, ya que si no, el sistema acaba volviéndose insostenible. No apliquemos la misma formula a cada caso. Cada tarea forma parte de un conjunto mayor y juega un papel más o menos relevante en el mismo. No tienen por qué aplicar los mismos principios a una feature que vivirá un día o una semana, que una que sea core de nuestra aplicación.

* La mayoría de las veces, **hacer las cosas bien, no es ni más lento ni más difícil que hacerlas mal**. Es simplemente que **no sabemos hacerlas y por lo tanto necesitamos dedicar tiempo a aprender**. Cuando no tenemos el hábito de estudiar, atribuimos mucha incertidumbre al hecho de buscar y aprender a hacer las cosas, creo que es por ello por lo que asumimos que hacerlas bien va a ser lento. Sin embargo, ¿cuánto tiempo nos puede ahorrar el hecho de aprender algo en nuestro proyecto actual?¿y en los futuros proyectos?

* Por último, intentemos comprender el origen de esa prisa y tratemos de **negociar con el alcance de la tarea**. Tengamos muy presente el aspecto de **desarrollo incremental** que nos permite el desarrollo software frente a el mundo físico.
