---
layout: commented_post
title: Property Based Testing desde la perspectiva de la generación de datos
date: 2019-12-04 12:00:00 +0200
categories: blog
excerpt:
  Los test basados en propiedades utilizan un input generado "aleatoriamente" para ejecutar nuestros test. Para ello hace uso de un generador de datos. Pero, ¿se comportará nuestro código siempre de la misma forma con cualquier dato aleatorio? ¿Debemos tratar todos los posibles casos en nuestro test?
---
En el anterior post, vimos las tres peculiaridades fundamentales [de Property Based Testing](/blog/2019/11/30/introduccion-a-property-based-testing.html). En este post nos centraremos en esta:

>No tenemos un control absoluto de los datos con los que ejercitamos el sistema.

Las implicaciones más evidentes de no saber exactamente con qué datos se ejecutará nuestro test son que:

* Tenemos que plantear los tests (sobre todo las aserciones) de una manera diferente a la acostumbrada.
* Evitamos tener que tomar la decisión de con qué dato probamos.
* Podemos llegar a observar un comportamiento, aparentemente no determinista de nuestros tests. Ya que podemos tener tests que a veces pasan y otras veces no. _Aunque en realidad lo que suele pasar es que nuestro código falla con determinados valores, o bien, no está preprarado para ejecutar un mismo test varias veces._

## ¿Qué es eso de que no conozco el input específico de mis tests?

Los test basados en propiedades utilizan un input generado "aleatoriamente" para ejecutar nuestros test. Para ello hace uso de un generador de datos. En ese aspecto, nuestras pruebas pueden adoptar dos posiciones:

1. **Genero datos completamente aleatorios.** Esta no es una buena estrategia ya que nuestro test tendrá que añadir lógica condicional para ejecutar un test u otro en función del tipo de datos. O para descartar la ejecución con esos valores específicos. Quienes hemos escrito algún que otro test, ya sabemos lo chungo que es meter lógica condicional en un test, así que mejor descartemos esta estrategia como norma general. _Sin embargo, destaco este punto aquí, porque no es raro encontrarse ejemplos donde se escriben los tests de propiedades de esta manera :S._

2. **Genero datos con unas restricciones para el escenario que quiero probar.** Para ello, la técnica de las **clases de equivalencia** nos encaja perfectamente.

### Clases de Equivalencia

Las técnica de [clases (o particiones) de equivalencia](https://en.wikipedia.org/wiki/Equivalence_partitioning) nos viene a poner un poco de orden a algo que habitualmente gestionamos con nuestra intuición.
>Hay un conjunto de datos que, a priori, se comportan igual ante nuestro software. Es decir, nos hace ir por el mismo camino. De modo, que si **tomásemos un dato cualquiera de dicho conjunto, podríamos asumir que con el resto valores se va a comportar de la misma manera**.

#### 🤔 ¿Me puedes poner un ejemplo?

Si buscamos [tarjetas de crédito de prueba para stripe en su página](https://stripe.com/docs/testing#cards), encontramos un ejemplo bastante bueno:
![Tarjetas de crédio inválidas de Stripe](/assets/posts/2019-12-04/stripe-failure-cards.png)

Junto a cada número de tarjeta vemos la descripción del flujo de error específico. Es decir, hay un grupo de tarjetas que provocan la compra o un fallo específico. Vemos entonces, que en la columna de la derecha podríamos ver la descripción de la clase de equivalencia, y en la columna de la izquierda un valor concreto de dicha clase.

Por si acaso, pondré otro ejemplo:

Imaginemos que **queremos probar un registro de usuario** y nuestro programa recibe el:
* **nombre de usuario**: que será obligatorio.
* **password**: que debería estar tener entre 6 y 10 caracteres incluyendo números y letras.

Algo tal que así:
![Ejemplo de formulario de registro](/assets/posts/2019-12-04/signup-sample-form.png)

A partir de aquí generaríamos las clases de equivalencia:

**Nombre:**
* <span class="success-text">√ Cualquier cadena no vacía</span>
* <span class="error-text">_x_ Cadena vacía</span>

**Password:**
* <span class="success-text">√ Cualquier cadena entre 8 y 16 caracteres que contenga letras y números</span>
* <span class="error-text">_x_ Cualquier cadena con menos de 6 caracteres</span>
* <span class="error-text">_x_ Cualquier cadena con más de 10 caracteres</span>
* <span class="error-text">_x_ Cualquier cadena 6 y 10 caracteres que no contenga letras</span>
* <span class="error-text">_x_ Cualquier cadena 6 y 10 caracteres que no contenga números</span>
* <span class="error-text">_x_ Cualquier cadena 6 y 10 caracteres que contenga símbolos no alfanuméricos</span>

Una vez identificadas las clases de equivalencia (o particiones), podremos definir nuestros casos de prueba. Para ello nos aseguraremos de ejercitar nuestro sistema con datos de cada una de las particiones para ejecutar los flujos posibles.

### ¿Cómo encaja esto con los tests de propiedades?

Conociendo la técnica de clases de equivalencia, es fácil plantear/implementar los generadores como un _"seleccionador de valores dentro de una partición"_. De este modo, dedicamos nuestro tiempo y esfuerzo a razonar sobre los posibles datos que podría aceptar mi sistema y cómo debería comportarse el mismo en lugar de pensar en qué dato específico elijo para mi prueba y cuál podría ser el más adecuado.
Es decir, estoy pensando en qué tipo de datos acepta mi sistema, en cómo se comportará ante estos datos y en cómo llamar a ese conjunto de datos aportando más semántica de negocio en nuestros tests.

### ¿Esto es exclusivo de los test de propiedades?

Para nada es exclusivo de los tests de propiedades. Esta técnica, nos ayuda a decidir nuestros casos de prueba. El problema es que si no le damos forma a esta técnica, acabamos generando muchos valores en nuestros tests, normalmente inicializando objetos inline. 

Un primer refinamiento, que evita duplicidad de código al inicializar todos estos objetos, sería el uso de ["object mother"](https://martinfowler.com/bliki/ObjectMother.html) para reutilizar la creación de estos objetos sin añadir boilerplate a nuestro código de test. Y aportando además un nombre con semántica al tipo de dato que necesitamos.

En mi caso, yo suelo aprovechar los generadores que ya estoy utilizando en mis test de propiedades, para implementar mis object mother. De esta forma, evito tomar la decisión acerca de los valores específicos que debería usar en mi test.

**Todo ventajas, ¿no creéis?**

Bueno, siendo sinceros, lleva cierta práctica coger soltura para implementar los generadores y no siempre es sencillo modelar la clase de equivalencia deseada. Pero si os ha interesado el tema, trataré de contaros cómo escribo mis generadores en los distintos lenguajes en los que trabajo ;).
