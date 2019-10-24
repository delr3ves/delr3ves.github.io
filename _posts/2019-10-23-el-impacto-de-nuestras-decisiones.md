---
layout: commented_post
title: El impacto de nuestras decisiones
date: 2019-10-23 12:00:00 +0200
categories: blog
excerpt:
  <p>Por nuestro trabajo, tomamos una gran cantidad de decisiones a diario. La mayoría de las veces lo hacemos de manera inconsciente lo cual no es necesariamente malo. Pero creo que adquirir consciencia de este hecho puede tener dos beneficios fundamentales:</p>
  <ul>
    <li>Saber comunicar las decisiones que hemos tomado al resto de las personas interesadas.</li>
    <li>Al adquirir consciencia del impacto de nuestras decisiones inconscientes, a lo mejor nos da menos miedo tomar otras decisiones que antes no nos atrevíamos a tomar.</li>
  </ul>
---

---
**TLDR;**

Por nuestro trabajo, tomamos una gran cantidad de decisiones a diario. La mayoría de las veces lo hacemos de manera inconsciente lo cual no es necesariamente malo. Pero creo que adquirir consciencia de este hecho puede tener dos beneficios fundamentales:

* Saber comunicar las decisiones que hemos tomado al resto de las personas interesadas.
* Al adquirir consciencia del impacto de nuestras decisiones inconscientes, a lo mejor nos da menos miedo tomar otras decisiones que antes no nos atrevíamos a tomar.

---

Intentaré explicarme mejor para ver si os consigo convencer ;P :

> Desarrollar una idea, consiste fundamentalmente en **comprenderla y simplificarla**, establecer un procedimiento y finalmente implementarla.

Cómo comprender la idea y otros aspectos de la comunicación, los dejo para otros posts, sorry. Hoy  partiremos de un conocimiento ya sea laxo o profundo de lo que debemos desarrollar.

**La primera decisión que tomamos es la de aceptar la tarea.** Cuando lo tenemos claro es una decisión lógica, pero ¿qué pasa cuando no comprendemos lo que tenemos que hacer? En mi experiencia, la mayoría de las veces, seguimos adelante. Dependiendo del tipo de relación que tengamos con el cliente (con quien pide funcionalidades en general), esto puede acarrear muchísimas consecuencias.

Si no tenemos trato diario con el cliente, tenderemos a rellenar todos los huecos que deja la definición del producto (y suelen ser muchos) con lo que consideremos adecuado. Muchas de las veces, dándole mucho peso a la solución que nos cueste menos trabajo. De nuevo, esto no tiene por que ser malo y hasta podría encajar perfectamente con la filosfía [Lean](https://es.wikipedia.org/wiki/Lean_software_development).

Pero si no comunicamos y confirmamos nuestras decisiones, **según avanze el proyecto, irá habiendo una discrepancia mayor entre la imagen mental que tiene el cliente**, que no está al día de cada detalle/decisión, **y la solución implementada.**

## La importancia de comunicar nuestras decisiones

A partir de cierto punto, no son pocas las veces que veo como **vamos perdiendo el respeto a nuestros managers**, tech leads o PMs y en general cualquier a persona que no conoce los detalles del día a día pero que necesita una visión global del proyecto.

Y desde el punto de vista del management, vemos como **nuestro equipo está cada vez menos alineado con nosotros** (o nosotros con nuestro equipo) con la consiguiente **pérdida de confianza**. Y por lo tanto cada vez permitimos menos iniciativa por parte de nuestro equipo :(.

Pues bien, para mi una de las razones principales se debe a la cantidad de decisiones inconscientes que tomamos. Al fin y al cabo, implementar la solución puede implicar miles de instrucciones y cada una de esas instrucciones es una decisión en si misma.

Para que no estemos todo el rato hablando sobre abstracto, os dejo algunos ejemplos de decisiones que me vienen a la mente.

### Decisiones puramente técnicas

<details>
  <summary class="collapsible-title">
    Si te dedicas al desarrollo, es muy probable que trabajes o hayas trabajado con un lenguaje que tarde o temprano acaba lanzando alguna excepción al intentar realizar una acción.
  </summary>
  <div class="collapsible-details">
    Por ejemplo, si intentases persistir una entidad en una base de datos, podría pasar que:
    <ul>  
      <li>la entidad esté duplicada y no te deje persistir</li>
      <li>la base de datos esté caída y falle</li>
      <li>no tengas conexión de red</li>
      <li>el disco esté lleno</li>
      <li>miles de cosas más...</li>
    </ul>

    Todo esto son potenciales excepciones que podría lanzar la librería que hemos decidido usar. Pues bien, ¿qué hacemos?

    <ul>
      <li><span class="soft-emphasis">¿trasladamos todos estos flujos alternativos a producto?</span> al final le estamos dando un montón de trabajo extra a producto por los detalles de implementación (complejidad accidental) que hemos introducido nosotros.</li>
      <li><span class="soft-emphasis">¿tratamos todos los errores como "NoSeHaPodidoGuardarLaEntidad"?</span> a lo mejor introducimos comportamientos extraños en el producto que generan contactos por parte de los usuarios y bastante tiempo de depuración intentando comprender que pasa.</li>
      <li><span class="soft-emphasis">¿miramos para otro lado y esperamos a que la posibilidad de fallo se convierta en un fallo real?</span> simplemente optamos por posponer la decisión con el consiguiente error 500 en el server o un posible crash o "blancazo" en las apps/web.</li>
    </ul>

    Sin contexto, ninguna decisión es mejor que la otra. Pero si <span class="soft-emphasis">es importante que ese conocimiento sobre esa decisión no te la quedes solo para ti</span>.
  </div>
</details>

<details>
<summary class="collapsible-title">
 Estoy trabajando con una base de datos relacional y tengo que almacenar una estructura de datos compleja y que cambia frecuentemente.
</summary>
<div class="collapsible-details">
  Si coleguis, al final acabamos cayendo y decidimos guardar los datos en un JSON en una columna de la muette. Pues si vamos por este camino:
  <ul>
    <li>¿podremos hacer búsquedas fácilmente por esos campos que están guardados en el JSON?</li>
    <li>¿será fácil editar un solo campo?</li>
    <li>¿cuantas veces podría fallar por guardar mal ese JSON y que luego no lo sepa parsear de vuelta?</li>
  </ul>
</div>
</details>

### Decisiones al modelar nuestro dominio

Intentaré ilustrar con algunos casos que me he encontrado:

<details>
<summary class="collapsible-title">
Imaginaos que tenemos una tienda online y queremos modelar la dirección de entrega de un comprador. Ya nos han avisado de que en un futuro es muy probable que quieran múltiples direcciones de envío, sin embargo a día de hoy, la UI asume una dirección única.
</summary>
<div class="collapsible-details">
  Ahora nos toca decidir si modelamos la dirección como un elemento simple o como un array.

  Siguiendo los princpios YAGNI y KISS, lo suyo sería tener un elemento simple. Es más sencillo de modelar y ya lo cambiaremos cuando lo necesitemos. Pero hay que tener en cuenta ciertas implicaciones.
  <ul>
    <li>En caso de querer persistirlo en una base de datos relacional, ¿lo haré como parte de la tabla de la entidad usuario?. Lo ideal sería que no, pero es muy goloso ahorrarnos unas cuantas queries extras o un join y traernos la dirección junto con el resto de datos de usuario. ¿Cuánto me complicaría la migración de datos esta decisión?</li>

    <li>En caso de tener un lenguaje dinámico, ¿cuánto me costaría hacer el cambio de signatura en los métodos para lidiar con arrays con un mínimo de garantías de no haber roto nada?¿es mi suite de pruebas tan robusta como para hacerlo sin miedo?</li>
  </ul>

  Por otro lado, modelar directamente el array cuando no es necesario, podría sobrecomplicar el producto innecesariamente ya que es por todos conocidos que <span class="emphasis">"esto lo haremos más adelante"</span> generalmente acaba en <span class="emphasis">"esto al final no se va a hacer"</span>.
</div>
</details>

<details>
<summary class="collapsible-title">
  A veces tenemos entidades de negocio que se parecen mucho a otras que ya hemos definido y tenemos que decidir como modelarlas. ¿Reutilizamos una clase para las dos entidades o las modelamos por serparado?
</summary>
<div class="collapsible-details">
  <p>
  Por ejemplo si modelaramos un sistema de mensajería por múltiples canales, podríamos ver que todos los mensajes tienen un emisor, un receptor y un mensaje. Podríamos modelarlos como una única entidad. Sin embargo, si pensamos en ellos como email, push notification, sms, etc. Vemos que, aunque tienen similitudes, también hay diferencias suficientes como para modelarlas por separado.
  </p>
  <p>
    Si optamos por modelarlas como entidades separadas, probablemente optemos también por alamacenarlas por separado también.
    ¿cuánto nos costará entonces formar una lista ordenada y paginada de todos los tipos de notificaciones?
  </p>
  <p>
    Si optamos por modelarlas como una sola clase, dependiendo del lenguaje con el que lo hagamos, tendremos que lidiar todo el rato con posibles valores nulos y con operaciones exclusivas de cada uno de los tipos de mensaje.
    Por lo que añadir una nueva operación podría llevar mucho más esfuerzo del que parecía razonable a priori.
  </p>
  <p class="barely-emphasis">
    Soy consciente de que este ejemplo está un poco cogido por los pelos, porque los lenguajes modernos tienen una capacidad de modelar tipos muy potente, pero aún así he vivido en más de una ocasión esta duda y sigo sin encontrar una forma que no nos haga prescindir de algo.
  </p>
</div>
</details>

## Concluyendo

Me hubiera gustado encontrar ejemplos mejores, pero espero que con estos sean suficiente para defender mi hipótesis.

**La cuestión es que si no comunicamos nuestras decisiones, es muy posible que estemos encaminando el proyecto en una dirección en la que nuestro cliente podría no querer ir.**

Cada decisión facilita un camino pero dificulta otros. Llegado ese momento, **es importante identificar cuando revertir nuestra decisión será excesivamente costoso** para verificar que la decisión tomada es la correcta.


### Algunas ideas para minimizar este problema

* Es fundamental adquirir consciencia de que **el mensaje lo interpreta quien lo recibe y no quien lo emite**.
* Al ser conscientes de que **el lenguaje es ambiguo y dar por hecho que quedarán cosas por definir**, seremos más propensos a establecer una conversación donde es legítimo preguntar casi cualquier cosa.
* Como norma general, por favor, cuando alguien venga a preguntarnos una duda, no contestemos con un **pero si eso ya está claro**. Si te han preguntado, es posible que no lo tengan tan claro.
* Tras terminar una tarea, intentemos hacer el ejercicio explícito de pensar en las decisiones que hemos tomado, ver cómo afectan al negocio y comunicarlas. Si no damos con decisiones sensibles muy probablemente no hemos buscado bien (pero tampoco nos obsesionemos :S).
* Ya que, tarde o temprano, la vamos a liar parda, cuando hay algo que pensamos que es positivo, hagámoslo. Me explico: asumir que nuestras decisiones inconsciente tienen un impacto bestial puede ayudarnos a tener menos miedo a tomar otras decisiones. Por ejemplo, a lo largo de los años he ido viendo como **mis compis se escudan en el "es que no me dejan hacer esto"** como razón fundamental para no hacer algo que defendíamos como ["la manera de hacer bien las cosas"](/blog/2019/05/06/lo-puedo-hacer-rapido-o-lo-puedo-hacer-bien#bien). Creemos que no tenemos permiso para hacer algo que debería tener un impacto positivo en nuestro día a día y sin embargo hacemos cosas que afectan negativamente sin darnos cuenta de ello y por lo tanto sin pedir permiso.
