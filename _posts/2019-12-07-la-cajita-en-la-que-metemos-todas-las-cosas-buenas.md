---
layout: commented_post
title: La cajita en la que metemos todas las cosas buenas
date: 2019-12-07 12:00:00 +0200
categories: blog
excerpt:
  <p>Hace unos meses estaba yo de cháchara con una de esas personas cracks y didácticas donde las haya y, por qué no, punta de lanza de la programación funcional en España :P. En un momento de la noche tuvimos la siguiente conversación:</p>
  <blockquote>
  <p>(yo) - ya, pero a mi la programación funcional no me resuelve cómo comunicarme con mi cliente. Ni cómo proveer la semántica que yo quiero a mi código, ni los malos entendidos, ni cómo hacer la build de mi sistema de manera automática...</p>
  <p>- claro que sí. Eso es programación funcional. <span class="soft-emphasis">"Tú eres un funciopata y ni lo sabías"</span>...</p>
  </blockquote>
  <p>Entonces lo vi claro. <span class="soft-emphasis">"Para ti, la programación funcional es como para mi el _muchero_"</span>. Me explico...</p>

---

Que no os engañe el título, en este post pretendo hablar de comunicación. Al menos dar los elementos que yo considero básicos, todo ello bajo el prisma de un fenómeno que no es poco común y que hasta hace relativamente poco no había sido capaz de ver. Aprovecho para compartirlo por si sirve para algo :).

Hace unos meses estaba yo de cháchara con una de esas personas cracks y didácticas donde las haya y, por qué no, punta de lanza de la programación funcional en España :P. En un momento de la noche tuvimos la siguiente conversación:

> (yo) \- ya, pero a mi la programación funcional no me resuelve cómo comunicarme con mi cliente. Ni cómo proveer la semántica que yo quiero a mi código, ni los malos entendidos, ni cómo hacer la build de mi sistema de manera automática...
>
\- claro que sí. Eso es programación funcional. **Tú eres un funciopata y ni lo sabías...**

Entonces lo vi claro. **"Para ti, la programación funcional es como para mi el _muchero_"**. Me explico...

En mi casa, cuando mi hermana y yo éramos pequeños, había una cajita mu chica que llamábamos el _"muchero"_. Ahí cabía todo lo que nos gustaba. Los recuerdos, los deseos, los piropos... era algo imaginario, y había hueco para todas las cosas buenas.

Es una tontería, pero una vez que me di cuenta de esto, aprendí a ver algunos matices que hasta entonces se me estaban pasando por alto. Ya no era tan relevante hablar sobre la programación funcional como de nuestras prioridades, de las cosas que nos gustan y que no. Y es que **muchas veces cuando alguien hace apología de algo de manera muy persistente, es probable que ese algo tenga un significado muy diferente para esa persona de lo que lo tiene para nosotros.**

Es importante saberlo. Esto puede cambiar nuestra forma de entender su mensaje y pasar de un **"ojú que cosa más cansina de persona"** generando así cierto rechazo a un **"vale, le estás dando mucha importancia a esto, esto y esto y ahí si que estamos de acuerdo... !me mola tu rollo!"** consiguiendo que nos interesemos.

Esto va un paso más allá. No es raro que esas personas apliquen los siguientes silogismos de manera errónea (cambio de ejemplo, pero muy poquito):

1. El TDD es bueno.
2. Hablar con el cliente es bueno.
3. Hablar con el cliente es TDD.

O siguiendo el mismo razonamiento

1. El TDD es bueno.
2. No hacer los tests antes del código no es TDD.
3. No hacer los tests antes del código no es bueno.

Al final actuamos bajo la hipótesis del mundo cerrado donde **"Todo lo que no está en mi universo de lo bueno, debe ser malo"**.

<details open>
  <summary class="collapsible-title">Os dejo por aquí otros ejemplos que veo de esta práctica en el contexto del desarrollo software</summary>
    <div class="collapsible-details">
    <ul>
      <li> <span class="emphasis">TDD</span>: Parece que todo se resuelve con TDD, si dirigimos nuestro desarrollo por los tests, de repente no tenemos problemas de comunicación, ni hay malos entendidos entre lo que creímos entender y lo que en realidad nos pidieron. El tooling asociado a nuestro flujo de desarrollo va mucho más fino. Nuestra suite de test ya no es indeterminista y ejecuta en 15 milisegundos...
      Todos los mecanismos que, a priori ponemos para que nuestro flujo de trabajo vaya bien, parece que los hemos implementado porque hacemos TDD.
      Mientras que, si no hacemos TDD, es prácticamente imposible que nuestro software funcione. Y somos unos irresponsables por no hacer TDD y deberíamos irnos a casa con cierta preocupación porque no tenemos una red de seguridad.
      </li><li> <span class="emphasis">DDD</span>: Me resuelve tantos o más problemas que TDD :P. La comunicación ahora fluye más aún. Tengo todos mis contextos bien separaditos y símplemente con borrar un paquete de mi proyecto desactivo features sin tener que hacer nada más. Mis microservicios ahora son más bonitos y tienen un pelo pantene. Por fin he conseguido domar sus rizos y ,como por arte de magia, el acoplamiento de mis monolitos ha desaparecido.
      </li><li> <span class="emphasis">Lenguajes tipados</span>: Todos los bugs que surgen en los lenguajes no tipados, los podría haber encontrado un compilador. Está claro que si tenemos un compilador y encima nuestro lenguaje tiene unos tipos muy muy estrictos, es imposible que algo falle. El compilador habla con nuestro cliente por nosotros y sabe desplegar el sistema a producción. Si salimos en la tele y viene un pico de tráfico bestial, el compilador autoescala por nosotros, en definitiva:<span class="soft-emphasis">In compiler we trust!!</span>
      </li><li> <span class="emphasis">Lenguajes no tipados</span>: Toda la dificultad de añadir nueva funcionalidad viene dada por la rigidez que nos da el sistema de tipos, mientras que si tengo la flexibilidad que me da un sistema no tipado, puedo hacer cualquier cosa. Es tan sencillo como modelar la plastilina. Además, puedo probar lo que hago sin tener que esperar ad infinitum a que mi sistema se construya. Pongo el watcher en mi proyecto y se ejecutan todos los tests al momento y si hago cualquier cambio y rompo algo, me entero al instante. ¿Cuánto dices que tarda tu gradle en ejecutar? :P.
      </li>
    </ul>
    </div>
</details>

**Ya sé que esto es contenido escrito, pero tratad de ver el tonillo de broma. Simplemente estoy haciendo una caricatura de algunas posturas que he visto y he adoptado (y seguramente sigo haciéndolo).** Para nada es mi intención decir que estas prácticas, técnicas, metodologías, etc. son malas. Todo lo contrario, creo que en su dosis, con sentido común y en el momento adecuado son muy potentes.

**Lo que si critico es el mensaje que se puede llegar a transmitir cuando nos ponemos en modo dogmático.**

## Algunos aspectos sobre comunicación

Os dejo por aquí un par de ideas que yo trato de tener presente casi siempre que estoy hablando con alguien y veo que no nos llegamos a entender.

> En comunicación hay un emisor, un receptor y un mensaje. Y **el mensaje lo interpreta el receptor**, no el emisor

En una época donde tratamos de evitar lo subjetivo, como emisores, intentamos que nuestro mensaje no tenga ambigüedad posible. Sin embargo, siento deciros que el lenguaje natural es ambiguo, nos guste o no. Así que cabrearse cuando alguien no nos entiende y responder con un **"pero si está claro"**. No es buena idea, salvo que queramos molestar a la otra persona. Creo que es mejor intentar **reformular el mensaje para ver si, de esta manera, el emisor parece comprender mi mensaje** (ojo que la respuesta que nos de el emisor es un nuevo mensaje en sí 🤔).
_Aunque esto da para tesis, yo lo voy a dejar aquí :P._

> Todo mensaje tiene un componente verbal y uno no verbal

Esto es importante, porque en general nos centramos mucho en el componente verbal, que es el que se puede contrastar, se puede recordar y a priori es el que hacemos de manera consciente así que es el único contenido del que nos solemos responsabilizar.

Sin embargo, el componente no verbal suele ser mucho más rico en información. Aquí incluimos el tono, los gestos, momento y **el contexto** en el que se emite y se recibe el mensaje, **nuestras acciones** etc. En general, nos transmite mucho de manera inconsciente pero, es muy fácil, para la parte emisora negarlo. Simplemente hay que responder con el clásico **"yo no he dicho eso"**. Y como emisores, si no estamos acostumbrados, nos cuesta mucho identificar cómo obtuvimos esas conclusiones generando cierta duda.

En general, el componente no verbal nos aporta muchísima información:

* Acepto tu mensaje porque me fío de tu criterio.
* O, por el contrario, no lo acepto porque no me fio de ti.
* Me dices una cosa, pero haces otra distinta y soy capaz de recordar lo que me has dicho, pero me quedo con mal sabor de boca y no sé por qué.
* No te veo muy convencido de lo que me estás diciendo.
* ...


## ¿A dónde quiero ir a parar?

Mi hipótesis, es que **ciertas palabras o conceptos transmiten unas ideas completamente distintas a una persona y a otra.**

Aquí hay unos cuantos escenarios reales que en mi experiencia han dado lugar a malos entendidos:

### Hablamos del mismo concepto/palabra pero con significados distintos

> \- Es mejor hacer muchos tests unitarios y minimizar los tests de aceptance.
>
\- Pero los test de aceptance son los que validan que el programa hace lo que nos piden, si no los haces, no estás probando desde la perspectiva del cliente.
>
\- !Qué dices los unitarios son mejores. Los tests de aceptance son lentísimos y un rollo de mantener

Aquí el concepto de **test de aceptance** tiene dos significados claramente distintos. Para una parte un test de aceptance seguramente es un test con un scope de todo el sistema que ejercita la UI. Lo cual suele ser más caro de implementar, ejecutar y mantener. En eso estamos de acuerdo.
Sin embargo, hay otra definición de aceptance independiente del scope. Simplemente te habla de que se valide la lógica de negocio pedida. Si por ejemplo nos piden un listado ordenado o filtrado, podríamos hacer un tests unitario del componente que filtra u ordena y podríamos considerarlo de aceptance y unitario.

Luego podríamos discutir sobre quien tiene la definición correcta, pero al menos, entenderíamos el mensaje inicial.

### Hablamos de distintos conceptos/palabras pero con el mismo significado

> \- Es mejor hacer muchos tests unitarios y minimizar los tests de sistema.
>
\- Hay que hacer muchos tests que cumplan las propiedades FIRST da igual si son unitarios o si son más grandes. Mejor, así hacemos menos tests.

Muy probablemente estemos hablando de lo mismo. Test que se ejecutan rápido, son fiables, no tienen mucho mantenimiento, podemos ver dónde está el fallo rápidamente... Sin embargo, la conversación deja de fluir. Esto es especialmente típico, cuando tú posición es, aparentemente "contraria" a la mía.

Ante este escenario, a lo mejor, otra parte del mensaje nos sirve para desatrancar la conversación. Se me ocurre que preguntarle a la otra persona _"¿qué es para tí tal cosa?"_ nos ayuda a tener otra conversación que si nos enriquece.

### Hablamos del mismo concepto/palabra con significados distintos y nos creemos que usamos el mismo significado :S

 > \- Es mejor hacer muchos tests unitarios y minimizar los tests de sistema.
 >
 \- Estoy de acuerdo, los test de sistema dan mucha guerra.

 Para mi un test unitario puede incluir alguna dependencia, siempre y cuando no hable con sistemas externos. Sin embargo, para otra mucha gente un test unitario solo prueba métodos de una sola clase. Por lo tanto, estamos hablando de lo mismo, pero cuando la otra persona vea mi tests, lo mismo se lleva una sorpresa.


Y para no variar, me he alargado más de lo que me habría gustado, así que lo dejo aquí, pero espero que estas reflexiones te ayuden a tener mejores conversaciones en tu día a día.
