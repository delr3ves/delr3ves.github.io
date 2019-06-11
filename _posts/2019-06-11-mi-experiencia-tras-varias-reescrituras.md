---
layout: commented_post
title: Mi experiencia con las reescrituras de varios proyectos
date: 2019-06-10 12:00:00 +0200
categories: blog
summary:
  Os dejo aquí unas cuantas historias de abuelo cebolleta como parte de de mi reflexión sobre las reescrituras desde cero.
excerpt:
  <p>
  Os dejo aquí unas cuantas historias de abuelo cebolleta como parte de de mi reflexión sobre las reescrituras desde cero.
  </p>
---
Os dejo aquí estas historias de abuelo cebolleta como parte de de mi reflexión sobre las [reescrituras desde cero de un proyecto](/blog/2019/06/10/reescribir-desde-cero). Si no has leído este otro post te lo recomiendo ya que es donde están las conclusiones aprendidas gracias a estas anécdotas.

### Primer intento

**Contexto**:
  * Proyecto en producción con casi 100K usuarios de pago.
  * Parte del equipo original.

**Resultado**: Ni se intentó.

**¿Qué aprendí?**:
  * A veces nos motivamos más de la cuenta.

<details>
<summary class="collapsible-title">Cuéntame más, please :).</summary>
  <div class="collapsible-details">
  <p>Recuerdo perfectamente cuando nos llevaron a una sala a un compañero y a mi y nos dijeron:</p>

  <blockquote>Tenemos una noticia que os va a molar bastante. Vamos a reescribr todo el proyecto en Scala y queremos que lo hagáis vosotros dos. Tenemos un mes y medio.</blockquote>

  <p>
  De aquellas, el proyecto llevaba arrancado casi dos años y por él habían pasado más de 15 personas. Sobra decir que ni siquiera lo intentamos :S. Así que de este "intento" de reescritura no aprendí nada útil, porque todo esto quedó en una anécdota de la que reirnos aún habiendo pasado casi 10 años.
  </p>
  </div>
</details>

### Segundo intento

**Contexto**:
* Prototipo en producción con apenas usuarios activos con acceso gratuito a la plataforma.
* Equipo técnico completamente nuevo (sin nadie del equipo viejo).
* Equipo de producto se mantiene.

**Resultado**: Plataforma migrada y mucho más robusta.

**¿Qué aprendí?**:
* El proyecto no se va a congelar por mucho que nos empeñemos.
* A la gente que piensa el producto le cansa (y con razón) tener que definir y discutir de nuevo todo el detalle del producto con una nueva persona. Y a veces obvian detalles importantes pensando que ya nos los habían contado.

<details>
<summary class="collapsible-title">Cuéntame más, please :).</summary>
  <div class="collapsible-details">
  <p>Tras mi salida de la empresa anterior me incorporé a una startup que hacía videos cortos con un modelo de datos muy parecido a Twitter. ¿Conocéis Vine? Pues como Vine, pero con videos redondos!
  </p>
  <p>En este caso, los fundadores, habían contratado a una de esas empresas super-punteras para que les hicieran el primer prototipo. Tras validarlo, decidieron contratar un equipo para continuar el desarrollo. El estado era de unos 3K usuarios en la plataforma.
  </p>
  <p>Cuando entré, lo primero que me llamó la atención fue que ni siquiera tenía acceso al código, así que tuve que buscarme las castañas para entrar en las máquinas y como el backend era PHP pues saqué el código de ahí. Cuando vi cómo estaba, me pareció imposible tocar ese código. Un PHP from hell cuya única misión era formar un XML muy chungo que servía de API para las apps. Por supuesto, mi primera reacción fue <span class="soft-emphasis">"esto hay que reescribirlo"</span>. Tras bastantes conversaciones con los socios de la empresa y el resto del recién llegado equipo técnico, decidimos que era lo más sensato. El trabajo previo había sido de 7 meses de consultoría de, supuestamente, hasta 4 personas. Aunque los fundadores esperaban la reescritura en un par de meses, al final estimamos que podríamos tener la completa en 6 meses pero con un MVP antes del primer mes y medio para poder ir usándolo. No creo que pasara una semana cuando se estaban pidiendo funcionalidades nuevas. Al final se llegó a la conclusión de que con el prototipo que tenían habían sacado suficientes conclusiones como para redefinir el producto así que en lugar de una reescritura, se convirtió en un proyecto completamente nuevo que fue saliendo más o menos en tiempo y todos fuimos muy felices. Del proyecto viejo solo nos quedó migrar los usuarios y sus datos que nos llevó más o menos una semana de trabajo. Pero el nuevo producto desarrollado, aunque mantenía la esencia, era bastante diferente.
  </p>
</div>
</details>

### "Tercer intento"

**Contexto**:
* El sistema aún no está en producción.
* Equipo de desarrollo completamente nuevo.
* Parte del equipo viejo sigue en la empresa aunque en otros proyectos.

**Resultado**: Plataforma migrada y mucho más rápida, robusta y predecible.

**¿Qué aprendí?**:
* Podemos cambiar casi todo nuestro sistema de manera incremental sin que el negocio se resienta.
* Al no ser una reescritura de cero, no hubo ningún ritual, ni reuniones, ni pedir permisos.
* No hace falta tener el código impecable para poder manejarlo, podemos ir mejorando paulatinamente y en cada paso obtener valor.
* Las arquitecturas complejas generan un sobrecoste muy grande en un equipo pequeño.

<details>
<summary class="collapsible-title">Cuéntame más, please :).</summary>
  <div class="collapsible-details">

<p>Entrecomillo lo de "tercer intento" porque en realidad esto no fue una reescritura desde cero, pero creo que contar la experiencia puede aportaros valor. El siguiente proyecto al que fui acababa de empezar cuando me contrataron. Llevaría un mes y medio de desarrollo de un par de personas que se salieron del proyecto y entramos dos personas nuevas. Cuando llegamos había cosas que parecían más complejas de lo necesario, pero también estaba la persona que había diseñado y pensado todo el sistema y, además de tener mucho peso en la empresa, parecía saber de lo que hablaba, así que ¿por qué no? le dimos una oportunidad.
</p>
<p>Era un sistema del que se esperaba que recibiera muchísima carga. Del orden de miles de peticiones por segundo que debíamos atender en menos de 100ms por requisitos del estándar que implementábamos. Para ello usábamos Node.js, Haddop, Apache Storm y cualquier cosa que sonase a rápido y a Big Data.
</p>
<p>En ningún momento nos planteamos reescribir desde cero, pero si había cosas que no nos cuadraban. Todos los frameworks que usamos eran nuevos para nosotros así que no conocíamos bien su estilo, sin embargo, había cosas que nos chillaban. Lógica de negocio en la capa de presentación, acoplamiento con los storages (mongo, mysql, redis, hdfs y yo que sé qué más...), nada de tests ni de código testable. <span class="soft-emphasis">Tengo pensado escribir un post sobre lo que aprendí mientras nos hacíamos con el control del código para que fuera modular/testable/legible, etc.</span>
</p>
<p>
Empezamos a crear casos de uso y extraer la lógica ahí. Comenzamos a preparar el entorno para poder inyectar dependencias y por lo tanto hacer el código más testable (ojo que inyección de dependencias no es lo mismo que inyector de dependencias, eso vino mucho más tarde). Un día nos dimos cuenta de que Storm no nos valía para hacer lo que pretendíamos, así que con toda la lógica de negocio en un core agnóstico a la capa de presentación, decidimos probar un nuevo framework. Por suerte para nosotros, el estándar que implemetábamos solo contemplaba tres puntos de entrada por lo que la interacción con el framework era reducida. A pesar de que la lógica de negocio era muy complicada, la teníamos encapsulada en un core. Así que en un día teníamos en producción una prueba de concepto con un nuevo framework que se adaptaba a nuestras necesidades. A la semana ya estaba todo en producción.
</p>
<p>
A pesar de que el código inicial no nos encantaba, decidimos ir aprendiendo de él, razonando el negocio a partir de lo que había y dejándolo más a nuestro gusto y esto nos permitió hacernos con el control de un código que, a priori, no entendíamos del todo.
</p>
</div>
</details>

### Cuarto intento

**Contexto**:
* Plataforma en producción con millones de peticiones al mes.
* Única y nada despreciable fuente de ingresos del negocio.
*  *Muy pocos casos de uso* desde el punto de vista del usuario.
* El equipo original ya no forma parte de la empresa.

**Resultado**: Plataforma migrada y mucho más rápida, robusta y predecible.

**¿Qué aprendí?**:
* Qué no se hagan las cosas a tú manera, no quiere decir que estén mal hechas.
* Hacer un esfuerzo en comunicación a los equipos no técnicos hace que todo sea mucho menos traumático.
* Las personas a cargo de del sistema que se quiere migrar sufren mucho así que es buena práctica compartir dicha responsabilidad.
* Tratar el código que no te gusta con desprecio, hace que sufras mucho, mientras que con un poco de mimo, todo se vuelve mucho más llevadero. Vamos, que el código tiene la fea tendencia de hacerte un __"Rebota, rebota y en tu culo explota."__

<details>
<summary class="collapsible-title">Cuéntame más, please :).</summary>
  <div class="collapsible-details">

  <p>
  Esta reescritura fue en la misma empresa que en el caso anterior. En este caso yo forme solo parte de la gestión del proyecto y apoyo con la plataforma antigua así que puedo contar menos de los detalles de la reescritura.</p>
  <p>
  Aquí nos encontramos con un sistema antiguo y complejo que fallaba casi a diario haciendo perder bastante dinero a la empresa. También teníamos un departamento de ventas con mucho peso en la junta directiva y que ejercía mucha presión con nuevas ventas por lo que a priori la reescritura se descartaba.
  </p>
  <p>
  Aquí hubo un poco de conflictos, porque mi rol en este proyecto fue más de intermediario entre la capa de management (el CEO de la empresa sobre todo) y el equipo técnico.
  </p>
  <p>
  Mi planteamiento fue seguir las mismas pautas que con el refactor anterior solo que este proyecto era mucho más complejo. Las estimaciones que manejábamos eran la de tener el sistema 100% migrado en unos 9 meses. Sin embargo esperábamos estabilizarlo en menos tiempo.
  </p>
  <p>
  Sin embargo, el resto del equipo planteaba la reescritura como única opción viable. A ellos no les interesaba seguir 9 meses en la empresa con ese proyecto y lo entiedo perfectamente. Nos juntamos todas las partes y finalmente se decidió la reescritura. Yo por mi parte, me propuse para mantener la plataforma antigua y dar un poco de aire a los desarrolladores. Yo ya había tenido mi temporada en la empresa de estar en los proyectos punteros y chulos y creo que hay que ser dar la oportunidad a todo el mundo de divertirse. Durante ese tiempo, el equipo trabajó rápido y bien, estudiaron muchísimo la casuistica del negocio y la simplifacaron tanto que la solución fue mucho más sencilla de lo que se esperaba. En menos de 6 meses el nuevo proyecto estaba funcionando muchísimo mejor que el anterior. En este caso me alegro de poder tragarme mis propias palabras y decir que, aunque yo no hubiera optado por ese camino porque me parecía arriesgar de más. Mis compis hicieron un trabajo buenísimo y me alegro de no haber hecho más presión para salirme con la mía.
  </p>
</div>
</details>
### Quinto, sexto, séptimo, ..., intento

**Contexto**:
* Plataforma en producción con millones de usarios activos
* Facturación mensual bastante alternativa
* Única fuente de ingresos del negocio
* *Una casuística grandísima* desde el punto de vista de usuarios y back-office.
* Mismo equipo de desarrollo con algunas nuevas incorporaciones.

**Resultado**: 4 intentos fallidos de reescritura y un negocio super resentido

**¿Qué aprendí?**:
* Si sabes escribir código que te gusta, empieza a escribirlo ya aunque el resto del código te de asquete.
* Si no sabes escribir código que con el que te sientes a gusto, ¿qué te hace pensar que empezar de cero va a ser una buena solución?
* Es una cabronada saber que no vas a poder cumplir las expectativas y aún así no lo comunicas al resto del equipo. No lo hagáis, please :)

<details>
<summary class="collapsible-title">Cuéntame más, please :).</summary>
  <div class="collapsible-details">
    <p>
      Para mi esta es la historia más común y la que justifica mis miedos a las reescrituras a pesar de que he vivido más casos de éxito que de fracaso.
      En este caso nos contrataron al equipo de <a href="https://trycircular.com/">Circular</a> (de aquellas se llamaban Vernon Labs) para el management, al equipo de <a href="https://www.karumi.com/">Karumi</a> para mobile y a mi para backend/web y plataforma. Todos como cosultores externos, tras la salida del CTO de la empresa.
      </p>
      <p>
      Llevaban un año y medio con la funcionalidad congelada porque estaban reescribiendo el código en un nuevo framework. Esta reescritura ya era el segundo intento fallido, pero tenían grandes esperanzas. La estrategia que siguieron fue la de desarrollar un sistema en paralelo y cuando estuviera listo lo desplegarían todo de golpe. Para lidiar con las necesidades por parte de negocio, la estrategia, que se convirtió en un mantra, fue</p>

      <blockquote>El nuevo sistema ya tendrá todas estas mejoras.</blockquote>

      <p>
      Básicamente mi trabajo consistía en terminar la migración que estaba a medias. Cuando entré y vi el código de producción y el nuevo código, lo primero que pensé es que no se había mejorado tanto, que el nuevo proyecto no justificaba un año y medio de parón en el proyecto con lo que eso resiente un negocio. No obstante, esa fue la primera impresión y en general trato de ser cauto así que simplemente "seguí con el plan" hasta tener un poco más de conocimiento. Mientras se hacía la migración, surgieron algunas necesidades del producto actual, por lo tanto hubo que desarrollar. En ese momento mi planteamiento fue hacernos con el control del código antiguo, añadir test en las partes nuevas, aprender a refactorizar, darle semántica al código, etc. Al principio el equipo se resistía porque:
      </p>

      <blockquote>Total, ¿para que mejorar esto si lo vamos a tirar en un mes?</blockquote>

      <p>A lo que yo respondía</p>

      <blockquote>Mientras no deje de estar, podemos asumir que este código va a estar en producción para siempre.</blockquote>

      <p>
      Además, les decía, que las prisas me las estaban metiendo a mi y que yo era el responsable y no les estaba metiendo ninguna prisa. Que prefería que no corrieran y que practicaran en un entorno que... a peor no iba a ir :P
      Un mes más tarde, vimos que la plataforma a la que se quería migrar era más inestable que la vieja que ya estaba en producción tras un poco de mimo. Que además la migración tanto de datos como de código era muy compleja y no garantizaba el funcionamiento y tras estudiar el código viejo, vimos que la mayoría de los problemas y de necesidades de cambio estaban bastante acotadas. Estudiamos en detalle la información proporcionada por algunos analizadores de código, vimos que había muchísimo código duplicado que podríamos abstraer y reducir el tamaño de la actual plataforma. Y que además como era un código mierdoso, podíamos aprovecharlo para aprender a refactorizar y a pasar de la forma de trabajar que tenía el equipo a una forma más adecuada, más estándar y más atractiva de cara a futuras contrataciones de una manera orgánica. El resultado fue que empezamos a sacar funcionalidades más rápido, el proyecto cada vez era más estable y desde la capa de management cada vez estaban más contentos con nuestro trabajo y había una mayor confianza cuando pedíamos tiempo para mejorar una parte del código. Seguimos con esta dinámica de trabajo hasta que nos fuimos (al fin y al cabo estábamos como consultores externos echando una mano de manera temporal).
      </p>

      <p>
      Con nuestra salida, entró un nuevo CTO y decidió que lo suyo eran los microservicios y el DDD. Así que se volvió a plantear una nueva reescritura. Tras 6 meses, no había nada que enseñar ni nada que aprovechar del código nuevo. ¿El resultado?, un montón de dinero, ilusión y esfuerzos a la basura. Vamos, una pena :(
      </p>
      <p>
      Sin entrar en detalles, unos meses más tarde, tras una nuevo viraje de la empresa, se decidió otra reescritura. Os podéis imaginar en que quedó la cosa :S
      </p>

</div>
</details>
