---
layout: commented_post
title: Property based testing desde la perspectiva de la generación de datos
date: 2019-11-29 12:00:00 +0200
categories: blog
excerpt:
  TBD
---
Haciendo un brevísimo resumen, sobre las implicaciones de la estrategia de tests basados en propiedades cabe destacar que:

1. No se centran tanto en los efectos que nuestro software provoca, sino en las propiedades que se podrían observar en él. _Este aspecto tiene especial relevancia cuando nuestro SUT está libre de efectos colaterales aunque no es del todo relevante para este post así que no lo detallaré más :P_.
2. Nuestros tests no se ejecutan una sola vez, sino que, se ejecutan multiples veces con distintos valores.
3. **No conocemos el input específico con el cual se ejecutarán nuestros tests, sino que generamos datos de manera aleatoria y esos serán nuestros datos.**

Este post se centrará en este último punto. Este aspecto es tan relevante a la hora de plantear los tests, que esta estrategia, además de llamarse Property Based Testing también es conocida como Generative Testing. En mi opinión, elegir un nombre u otro, incita a priorizar más un aspecto u otro de esta técnica.

Las implicaciones más evidentes de no saber exactamente con qué datos se ejecutará nuestro test son que:

* Tenemos que plantear los tests (sobre todo las aserciones) de una manera diferente a la acostumbrada.
* Evitamos tener que tomar la decisión de con qué dato probamos.
* Abrimos la puerta al "no determinismo" de nuestros tests. En este aspecto hay que destacarlo como algo positivo (porque encuentra casos de error que no habríamos encontrado) y negativo (porque tendemos a no hacer mucho caso a los errores cuando un test falla a veces si y a veces no).

## ¿Qué es eso de que no conozco el input específico de mis tests?

Los test basados en propiedades utilizan un input generado "aleatoriamente" para ejecutar nuestros test. Para ello hace uso de un generador de datos. En ese aspecto, nuestras pruebas pueden adoptar dos posiciones:

1. **Genero datos completamente aleatorios.** Esta no es una buena estrategia ya que nuestro test tendrá que añadir lógica condicional para ejecutar un test u otro en función del tipo de datos. O para descartar la ejecución con esos valores específicos. Quienes hemos escrito algún que otro test, ya sabemos lo chungo que es meter lógica condicional en un test, así que mejor descartemos esta estrategia como norma general.

2. **Genero datos con unas restricciones para el escenario que quiero probar.** Para ello, la técnica de las **clases de equivalencia** nos encaja perfectamente.

### Clases de Equivalencia

Las técnica de [clases (o particiones) de equivalencia](https://en.wikipedia.org/wiki/Equivalence_partitioning) nos viene a poner un poco de orden a algo que habitualmente gestionamos con nuestra intuición.
>Hay un conjunto de datos que se, a priori, comportan igual ante nuestro software. Es decir, nos hace ir por el mismo camino. De modo, que si **tomásemos un dato cualquiera de dicho conjunto, podríamos asumir que con el resto valores se va a comportar de la misma manera**.

#### 🤔 ¿Me puedes poner un ejemplo?

Imaginemos que **queremos probar un registro de usuario** y nuestro SUT recibe el:
* **nombre de usuario**: que será obligatorio.
* **password**: que debería estar tener entre 8 y 16 caracteres incluyendo números y letras.

Además, es posible que el nombre de usuario ya esté cogido por lo tanto el sistema debería validarlo.

A partir de aquí generaríamos las clases de equivalencia:

**Nombre:**
* <span class="success-text">√ Cualquier cadena no vacía</span>
* <span class="error-text">_x_ Cadena vacía</span>

**Password:**
* <span class="success-text">√ Cualquier cadena entre 8 y 16 caracteres que contenga letras y números</span>
* <span class="error-text">_x_ Cualquier cadena con menos de 8 caracteres</span>
* <span class="error-text">_x_ Cualquier cadena con más de 16 caracteres</span>
* <span class="error-text">_x_ Cualquier cadena 8 y 16 caracteres que no contenga letras</span>
* <span class="error-text">_x_ Cualquier cadena 8 y 16 caracteres que no contenga números</span>
* <span class="error-text">_x_ Cualquier cadena 8 y 16 caracteres que contenga simbolos no alfanuméricos</span>

Sin entrar en mucho detalle sobre la técnica de clases de equivalencia, ésta nos define una regla para generar casos de prueba a partir de las particiones definidas.

### ¿Cómo encaja esto con los tests de propiedades?

Cuando dedicamos tiempo y esfuerzo a razonar sobre los posibles datos que podría aceptar mi sistema y cómo debería comportarse el mismo, estamos definiendo también cuáles son los generadores que deberíamos implementar, además pensamos en un nombre con semántica de negocio para dichos generadores y esto ya aporta valor en sí mismo.

Desde hace varios años vengo utilizando cada uno de los tres aspectos de property based testing juntos o por separados como parte de mi estrategida de tests.

En el caso de los generadores, además, aprovecho en otros tests que no podrían considerarse cómo de propiedades para generar el input. De manera que aprovecho el propio generador como ["object mother"](https://martinfowler.com/bliki/ObjectMother.html) sin añadir apenas boilerplate en mi código de tests.

Y mejor aún, también podemos usar esos mismos generadores de código para crear las fixtures de nuestro sistema en entornos de desarrollo.

Todo ventajas, no creéis?

Bueno, siendo sinceros, lleva cierta práctica coger soltura para implementar los generadores y no siempre es sencillo modelar la clase de equivalencia deseada. Pero si os ha interesado el tema, trataré de contaros cómo escribo mis generadores en los distintos lenguajes en los que trabajo ;).
