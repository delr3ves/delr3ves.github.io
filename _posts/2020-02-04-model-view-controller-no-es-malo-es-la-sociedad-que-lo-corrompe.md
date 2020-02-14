---
layout: commented_post
title: El Model View Controller no es malo, es la sociedad, que lo corrompe
date: 2020-02-14 12:00:00 +0200
categories: blog
excerpt:
  <p>Tenía en el tintero escribir sobre este patrón, porque es muy común verlo fracasar en muchos proyectos y quería aportar una visón que a mi me ha funcionado bastante tiempo (me sigue funcionando, de hecho) y que creo que es coherente y compatible con otras tendencias en el mundo del desarrollo como la arquitectura hexagonal. Sin embargo, siempre lo voy posponiendo, porque pienso que son cosas muy trilladas y que ya no aportan nada nuevo.</p>
  <p>¿Qué me ha hecho cambiar de opinión? Pues el otro día vi un video de los amigos de Codely en el que hablaban sobre la inconveniencia de seguir usando este patrón. En él dan una explicación que no termina de encajar con lo que yo aprendí en su día así que me gustaría poder aportar mi visión por si os sirve de ayuda.</p>
---
Tenía en el tintero escribir sobre este patrón, porque es muy común verlo fracasar en muchos proyectos y quería aportar una visón que a mi me ha funcionado bastante tiempo (me sigue funcionando, de hecho) y que creo que es coherente y compatible con otras tendencias en el mundo del desarrollo como la [arquitectura hexagonal](https://alistair.cockburn.us/hexagonal-architecture/). Sin embargo, siempre lo voy posponiendo, porque pienso que son cosas muy trilladas y que ya no aportan nada nuevo.

¿Qué me ha hecho cambiar de opinión? Pues el otro día vi un video de los [amigos de Codely](https://codely.tv/) en el que [hablaban sobre la inconveniencia de seguir usando este patrón](https://www.youtube.com/watch?v=kCgjV2QeRyk). En él dan una explicación que no termina de encajar con lo que yo aprendí en su día así que me gustaría poder aportar mi visión por si os sirve de ayuda. No sin antes dar las gracias a [César](https://twitter.com/eltator). Hace un montón de años, pero aún recuerdo como si fuera ayer su explicación del patrón.

Voy a intentar seguir el mismo hilo conductor que seguí con el post de [los singletons no son malos, es la sociedad que los corrompe](/blog/2015/06/04/los-singletons-no-son-malos-es-la-sociedad-que-los-corrompe.html) para intentar razonar por qué yo creo que el patrón MVC sigue aplicando.

## ¿Qué responsabilidades tienen, el modelo, la vista y el controlador y cómo los interpreto yo?

<div class="disclaimer">
<spam class="soft-emphasis">Disclaimer:</spam> Empiezo a pensar que soy yo el que ha confundido las responsabilidades de MVC y le atribuyo cosas que no son. Si este fuera el caso, creo que aún así el contenido del post será interesante. 😅😅
</div>

>Tal y como yo lo interpreto, este patrón pretende aportar una solución al problema de desacoplar el procesado de la información de su representación hacia y desde el exterior.

Para ello define 3 componentes/responsabilidades que intentaré explicar en el orden en que entiendo que entran en juego.

### Vista
Representa la forma en que el sistema ofrece su información al mundo exterior y cómo recibe las órdenes/eventos del mismo.
Como vemos, la definición es bastante laxa y no se centra en vistas Web, APIs Rest, aplicaciones CLI o un sistema de Colas. Simplemente habla de representación de datos y de captación de eventos.

### Controlador
Como yo lo entiendo, opera de la siguiente forma:

1. Desde la vista llega una petición conteniendo "tipos básicos" en el sentido en que no aportan información real de negocio. Bien puede ser a través de un formulario, un json, xml un array de argumentos (strings, enteros, etc.). El controller **traduce esos datos sin semántica a entidades de dominio**, es decir, estructuras que aportan semántica a nuestro negocio. _Esta traducción, puede conllevar una validación básica para asegurar la entidad de dominio se puede formar_.

2. **Orquesta la llamada específica al** método (idealmente un sólo método) que implementa el **caso de uso**, siempre desde el punto de vista de negocio, agnóstico a la infraestructura o a la representación y espera su respuesta.

3. **Traduce el resultado del caso de uso a un modelo de datos que entienda la vista** para devolver el resultado al usuario de nuestro sistema.

### Modelo

Desde el punto de vista del patrón MVC (sin entrar en lo que luego los frameworks hacen), ofrece una fachada que encapsula todo nuestro negocio. En terminología DDD, esto definiría el [lenguaje ubicuo](https://github.com/jatubio/5minutos_laravel/wiki/Resumen-sobre-DDD.-Domain-Driven-Design#lenguaje-ubicuo-ubiquitous-language). También hablamos de dicha fachada como "casos de uso" o "servicios".

**Resumiendo mucho:**

> * La **vista** ofrece la puerta al mundo exterior de nuestro sistema pintando los datos bonitos.
 * El **controlador** se encarga de traducir los datos entre el protocolo propuesto por la vista y las entidades de negocio.
 * Por último, el **modelo** se encarga de TODO lo demás.

Según esta definición, que espero que no sea muy descabellada para la mayoría, **el modelo tiene una responsabilidad órdenes de magnitud mayor que los otros dos componentes del patrón**, así como una independencia grande de la tecnología concreta y el protocolo de comunicación al exterior.

## ¿Dónde está la confusión?
Voy por partes:

### Definiciones súper laxas

1. La primera vez que yo leí sobre este patrón, era común leer algo del estilo de **el controller es el encargado de la lógica de control** _¿WTF, qué es exactamente lógica de control? ¿Me pesa más la palabra lógica o la palabra control?¿lógica de qué? ¿control de qué?_ Yo creo que esta fue una de las mayores fuentes de malos entendidos ya que se tradujo en "el controller implementa la lógica de TODO" lo cual hizo bastante daño ya que el **controller está muy acoplado a la vista** por lo tanto **acabábamos teniendo sistemas donde era muy difícil reutilizar la lógica de negocio**. <br/>_Uno de los ejemplos modernos de este problema lo podemos encontrar en la documentación de las primeras versiones populares de Android incitando a meter toda la lógica en el Activity_

2. Por otro lado, la mayoría de nuestros proyectos suelen empezar, aunque luego siempre se complican, siendo tan apasionantes como "tengo que leer datos de una fuente de datos (generalmente una base de datos en backend y un API en frontend) y pintarlos". Así que empezaron a proliferar frameworks que prometían resolver todos nuestros problemas bajo el nombre de MVC (MV*). Cuando en realidad lo que hacían, desde el punto de vista del modelo, era ofrecernos una abstracción para acceder a los datos. Ya sea mediante el patrón [Active Record](https://es.wikipedia.org/wiki/Active_record) o mediante un [Entity Manager](), que en el mejor de los casos, se escondía detrás del [patrón DAO](https://es.wikipedia.org/wiki/Objeto_de_acceso_a_datos).<br/>
_En mi opinión, como MVC ha sido sinónimo de "está bien hecho", los frameworks han aprovechado esto para hacer su marketing._

3. Finalmente, la nomenclatura tampoco ayudó, ya que "Modelo" significa un montón de cosas en el contexto del desarrollo software. Podríamos hablar del [Modelo de Datos](https://es.wikipedia.org/wiki/Modelo_de_datos), el Modelo de negocio (la definición del lenguaje ubicuo y su implementación), el [Modelo de dominio](https://es.wikipedia.org/wiki/Modelo_de_dominio) <br/>
Yo creo que el patrón MVC se refiere al modelo en su sentido más amplio, es decir el Modelo de negocio, donde tenemos una implementación agnóstica de cómo se representan sus datos en el exterior. Por lo cual, como decía antes, es un componente bastante grande.

### Frameworks pretenciosos

Cuando hacemos un proyecto, nos ilusiona muchísimo poder resolver muchos problemas y llegar a cuanta más gente mejor. Supongo que, del mismo modo, la gente detrás del desarrollo de frameworks gordos, deben tener esta misma idea en mente.

\- 🤔_"Cuantos más problemas resuelva y más gente me use, mejor."_

El caso es que hemos acabado por confundir la abstracción por la concreción. Es decir, confundimos el patrón MVC que desacopla la lógica de negocio de la vista por el framework de turno (Django, Ruby on Rails, Symfony o Spring, por nombrar algunos).

En este sentido se ven claramente dos tendencias:

#### 1. Frameworks gordotes que hacen de todo.
En general ofrecen un stack lo más completo posible:
1. Negocian contenido (generalmente http o CLI)
2. Ofrecen integración con storages (bases de datos o clientes REST)
3. Validación de datos. Generalmente campo a campo o con alguna validación un poco más compleja, pero a menudo insuficiente. Que además muchas veces está pensada para que sea el framework quien lo llama a la altura del controller. Cuando muchas de esas validaciones serían de negocio.
4. Gestión de formularios o input de datos, tratando de hacer un binding desde la vista hasta el modelo, saltándose la capa intermedia.
5. Integración con redes sociales
6. ...

Esto tiene la ventaja de que, casi no tenemos que tomar decisiones y si hacemos todo tal y como nos dice el framework, suele pasar que tenemos un montón de tareas comunes prácticamente resueltas por lo que el setup de nuestros proyectos es muy rápido y es bastante vistoso de cara a quien nos pide la funcionalidad inicial.

Sin embargo, no todo son ventajas ya que todo esto hace que nuestro modelo, en el mejor de los casos, esté muy acoplado con el framework (siendo difícil de migrar llegado el momento). Y, en el peor y más común de los casos, escribimos mucha lógica de negocio en el controlador (siendo difícil de mantener).

Si se nos da bien la cosa, al final acabaremos sabiendo manejar muy bien todas y cada una de las features que nos ofrece le framework. Sin embargo, nos resultará imposible cambiar de tecnología. Ni en el proyecto actual, donde no somos capaces de utilizar otras alternativas.
Ni para empezar proyectos nuevos, ya que acabamos por olvidar (o directamente no llegamos a aprender) técnicas más genéricas y agnósticas al framework que nos permiten adquirir [conocimiento esencial en lugar de accidental](https://www.javiergarzas.com/2009/05/del-conocimiento-esencial-y-del.html).

#### 2. Microframeworks rodeados de componentes y utilidades

En este caso los frameworks suelen ofrecer lo mínimo para comunicar nuestro modelo de negocio con el exterior. Y además suelen permitir, mediante plugins, añadir nueva funcionalidad ya sea en el ciclo de vida del framework o simplemente con recomendaciones de librerías que resuelven ciertos problemas muy específicos.

Aquí la ventaja, que también podría verse como un inconveniente, es que será el equipo de desarrollo quien confecciona todo el stack. Esto implica tomar muchas decisiones que no siempre son fáciles de tomar. A cambio, aprendemos y, seguramente, seremos capaces de extrapolar mucho conocimiento valioso.

Como desventaja, salvo que tengamos mucha soltura, tardaremos más en hacer el setup de nuestro proyecto y, hasta que tengamos todo el tooling listo, posiblemente iremos más lento.

## Entonces, ¿soy yo una de esas personas que corrompe el MV*?

Según mi opinión, seguramente lo seas, si:

* Consideras que hacer MVC es seguir las pautas de un framework que dice ser MVC.
* Piensas que el modelo consiste en la integración con la base de datos.
* Acoplas tu lógica de negocio a lógica de presentación.
* Implementas la lógica de negocio en un lugar en el que, si quisieras cambiar la interfaz de usuario (por ejemplo, de una vista a un servicio de Android o de una Web a una aplicación CLI), tuvieras que mover bastante código de sitio. O lo que es lo mismo, añades lógica de negocio en el controlador/presenter/...

## Conclusiones

* Casi todo lo que se habla en este post sobre MVC, también aplica a MVP, MVVM, y cualquier MV*).
* El objetivo de MV* es desacoplar la vista del procesamiento de datos añadiendo para ello, un constructo en medio.
* Mi experiencia dice que no aplicamos bien el patrón.
* Normalmente, cuando los frameworks hablan de MV*, nos resuelven bien la Vista y el Controller/Presenter/...
* Ningún framework, por definición, te va a poder resolver el modelo al completo, aunque si que puede aportar utilidades que faciliten la tarea.
* Por lo tanto, MVC no tiene una implementación canónica en ningún framework.
* El hecho de que un framework nos fuerce una forma de trabajar que nos dificulta en exceso desacoplar nuestro modelo, debería hacer que saltaran nuestras alarmas y que analicemos con clama los pros y contras de utilizarlo.
