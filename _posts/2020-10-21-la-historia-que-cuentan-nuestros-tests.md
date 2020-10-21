---
layout: commented_post
title: La historia que cuentan nuestros tests
date: 2020-10-21 11:00:00 +0200
categories: blog
excerpt:
  Nos han contado que nuestros tests pueden llegar a ser la mejor documentación de nuestro proyecto. Qué hace, cómo se usa. Pero, ¿cómo podemos plasmar esto en nuestro código? ¿qué debería documentar exactamente? Continuando con la serie de "los tests también son código" os intentaré contar cómo lo hago yo.
---

Dándole una vuelta más a eso de que [los tests también son código](/blog/2020/06/06/lost-test-tambien-son-codigo.html), quiero contaros cómo interpreto yo el hecho de que los tests sean legibles y **nos cuenten** una o **varias historias** relacionadas con el uso de nuestro software, así como de la posible casuística del mismo. ¡Vamos allá!

## Contando varias historias a través de nuestros tests

Quienes hemos dedicado algo de tiempo a leer sobre testing, especialmente desde que se popularizó TDD, hemos oído que nuestros tests son la mejor documentación de nuestro código. Estoy totalmente de acuerdo, pero... ¿eso qué significa exactamente?
Cada historia que contamos tiene tantos puntos de vista como personajes conviven en ella o necesidades tienen. Entonces... ¿para quién contamos la historia? ¿les interesa saber qué hace nuestro producto? ¿les puede interesar más saber cómo se invoca?

### Narrando lo que hace nuestro producto

¿Por qué me parece relevante este aspecto?. Como consultor freelance, (o como cualquier persona que llega nueva a un proyecto o que vuelve a él después de un tiempo) cambio de proyecto y de cliente cada poco tiempo. Uno de los mayores problemas que me encuentro es que **la definición sobre lo que hace mi producto no está unificada en ningún lugar**, por lo que mis **primeros esfuerzos van SIEMPRE dedicados a investigar qué hace nuestro software**. Y hasta el momento, nunca me he encontrado con nadie con visión de producto que tenga la disponibilidad/disposición de contármelo, por lo que siempre me toca profundizar en un código que no conozco y que, casi nunca, es fácil de inspeccionar.

Como en los post anteriores, vamos a continuar con un ejemplo sencillo, pero espero que didáctico:

Imaginemos que queremos modelar el caso de uso encargado de realizar una transferencia en un banco. En su caso más sencillo podría consistir en un único método/pantalla en la que recogemos todos los datos necesarios para realizar la transferencia. En código podría modelarse tal que así:

```kotlin
data class TransferOrder(
  val sourceIsban: String,
  val destinationIsban: String,
  val destinationName: String,
  val amount: BigDecimal,
  val currency: String,
  val description: String,
  val operationPassword: String
)

data class Transfer(
  val sourcerName: String,
  val sourceIsban: String,
  val destinationIsban: String,
  val destinationName: String,
  val amount: BigDecimal,
  val currency: String,
  val description: String,
  val status: TransferStatus
)

fun makeTransfer(transferOrder: TransferOrder, requester: User): Transfer  
```

Bajo el prisma de la visibilidad de un test tenemos dos alternativas para probar este método. **Caja negra** o **caja blanca**. Aunque, por ahora, solo estaríamos en disposición de hacer pruebas de caja negra ya que aún no tenemos conocimiento acerca de cómo se va a implementar este método. Por mi parte, todo correcto porque yo suelo ser más partidario de hacer pruebas de caja negra ya que en general, desde el punto de vista de los tests, no me interesa saber cómo está implementado algo, sino que me centro en qué puedo esperar de ese fragmento de código. _Obviamente, cuando necesitamos aislar el fragmento de código que queremos probar necesitaremos un mínimo conocimiento de cómo está implementado nuestro SUT, fundamentalmente para configurar los dobles de prueba. Pero más allá de eso, no debería interesarnos en cuestiones de implementación_

En este punto, _asumiendo que es común que no todo el mundo escriba los tests antes que el código_, creo que merece la pena comentar, que en mi experiencia, **cuando código de producción y test están escritos por la misma persona o cuando quien prueba tiene acceso al código, se tiende a no volver a leer la especificación** y nuestros tests pueden llegar a convertirse en una especificación de lo que nuestro código ya hace, más que lo que debería hacer, por lo que lo más habitual es que acabemos con una suite de pruebas tal que así:

```kotlin
@Test
fun `make transfer should create a transfer in Posted status`() {
  //the test!!
}
```

Desde el punto de vista del detalle del test, habrá quién está de acuerdo conmigo en que tiene una apariencia resultona. Hemos usado una frase que se entiende más allá de `fun testMakeTransfer()`, pero... ¿hemos dedicado tiempo a cuestionar si ese método es completo? ¿si hace lo que debería hacer?

El problema, de esta aproximación es que la signatura de nuestro método nos está dando pocas pistas acerca de las cosas que pueden pasar al realizar la transferencia. ¿Hay algo que podría salir mal? De ser así, ¿cómo nos notifican?. Supongo que en este caso tendríamos que echar mano de la especificación para saber qué más cosas pueden pasar. No obstante, os dejo una alternativa más detallada en código para tenerlo todo documentado en el mismo sitio :).

```kotlin
sealed class TransferResult
data class TransferSuccess(val transfer: Transfer): TransferResult
data class InvalidConcept(val concept: String): TransferResult
data class SourceAccountDoesNotBelongToUser(val user: User, val isban: String): TransferResult
data class DestinationAccountAndNameDoesNotMatch(val name: String, val isban: String): TransferResult
data class InvalidPassword(): TransferResult
data class InsufficientFunds(val requestedAmount: BigDecimal, val remainingAmount: BigDecimal): TransferResult

fun makeTransfer(transferOrder: TransferOrder, requester: User): TransferResult
```

Esta alternativa es mucho más descriptiva con respecto a las cosas que podrían pasar cuando hacemos una transferencia a nivel de negocio. En este aspecto, plantear los flujos de prueba es mucho más sencillo. No obstante, siendo justos con la problemática descrita anteriormente. Si con poca información tendíamos a no leer la especificación de la feature, ahora es posible que nos fiemos más aún ya que nos encontramos con un código que parece bastante "honesto". Sin embargo creo que esta aproximación (u otras parecidas) nos incitan a pensar y cuestionar mucho más sobre lo que el cliente espera de nuestro código.

De cualquier modo, y asumiendo que los flujos escritos son los que deberíamos probar podríamos tener una suite de test con estos casos:

```kotlin
@Test
fun testSuccess() {
}

@Test
fun testInvalidConcept() {
}

@Test
fun testSourceAccountDoesNotBelongToUser() {
}

@Test
fun testDestinationAccountAndNameDoesNotMatch() {
}

@Test
fun testInvalidPassword() {
}

@Test
fun testInsufficientFunds() {
}
```

Podríamos decir que esta suite de test parece bastante completa, pero... ¿podríamos hacerlo mejor?

### Nombre de los tests

Con la información del producto que tenemos hasta ahora, creo que solo tendríamos margen para **mejorar el nombrado de los tests**. No obstante, es una mejora sustancial a la que **no siempre le dedicamos demasiado tiempo**. 

Con framewors tipo BDD o lenguajes como Kotlin, solemos hacerlo mejor ya que, mentalmente, nos resulta más fácil hacer una descripción larga cuando escribimos una frase que cuando escribimos un nombre de un método. Habitualmente, aunque ya vamos asimilando la necesidad de tener nombres descriptivos, es normal que nos choque ver un nombre de método larguísimo.
Para quienes no tengáis la suerte de poder escribir la descripción en una frase, os dejo una sugerencia que me hizo una vez [@JorgeCastilloPr](https://twitter.com/JorgeCastilloPr):
**Aunque la convención de vuestro lenguaje sea usar camel case, usad snake case para los nombres de los tests.** Se lee muchísimo mejor para nombres de métodos largos. Os dejo un ejemplo y ya me diréis que se lee mejor:

```kotlin
@Test
fun whenInvalidConceptIsProvidedTheTransferShouldNotBeDone() {}
```

```kotlin
@Test
fun when_invalid_concept_is_provided_the_transfer_should_not_be_done(){}
```

Ahora mismo se me ocurren tres razones por las cuales esto nos resulta doloroso:

1. Tenemos un nombre larguísimo para el método y eso mentalmente nos molesta.
2. En el caso de que nos saltemos la convención del lenguaje es posible que nos duela en el alma, además de tener que configurar el linter para que no nos chille y no siempre sabemos cómo hacerlo.
3. Tenemos que currarnos una descripción escueta pero detallada del flujo que queremos probar.

No obstante esto es muy ventajoso a la hora de revisar la suite de test y nos sirve **como documentación de nuestro producto**.

### ¿A quién le contamos la historia?

> \- Vale, me estas hablando continuamente de que hay que contar una historia pero, ¿a quién se la estamos contando?

Me alegro que me hagáis esta pregunta :P. La verdad es que hay varias personas interesadas en esta historia:

* **Gente de producto con una mínima capacidad para leer algo de código**: de esta manera podrán revisar si todos los escenarios son completos. No digo que tengan que bajarse el repo y revisarlo por su cuenta, pero yo si he encontrado útil, a veces, sentarme con alguien de producto/negocio y enseñarle lo que está escrito a alto nivel. Sinceramente no sé cuánto habrán entendido, pero la experiencia ha sido buena siempre y ha dado pie a conversaciones interesantes.
* **Gente que necesita saber qué hace el producto**: por hablar de situaciones reales, como decía al inicio dle post, casi siempre que llego a un proyecto empezado me encuentro que la documentación de producto está bastante dispersa y, por alguna razón, todo el mundo asume que ya conoces el producto y todas sus peculiaridades por generación espontánea así que rara vez te lo explican durante el proceso de onboarding. Cuando encima te llaman para hacer alguna parte del backend más osucro, te cuesta más aún hacerte una idea de qué ve el usuario final. Y si, puedes llegar a comprender qué hace el código, pero... ¿realmente es lo que se espera que haga?
* **Gente que necesita saber cómo se usa el código**: dado que nuestros tests interactuán de una forma u otra con nuestro código, parece un buen sitio para ver cómo invocarlo, arrancarlo, etc. 
* **Nuestro yo del futuro**: No sé si os pasará lo mismo, pero mi cerebro parece no estar dispuesto a almacenar detalles concretos y, aunque me acuerdo a grandes rasgos de casi todo lo que hago, no es extraño que me plantée por qué he tomado determinada decisión. En este caso los tests pueden ser de gran ayuda.

Seguramente me dejo puntos de vista en el tintero, pero a grandes rasgos quedémonos con que a veces nos puede interesar saber qué hacen nuestros tests, otras veces cómo se usa el código, otras veces nos puede interesar saber por qué se tomaron ciertas decisiones de negocio y cómo afectaron al código. Incluso podríamos llegar a documentar problemas que detectamos en producción (os dejo este ejemplo real de un proyecto: `it should "reproduce bug #394 that fails when add transactions in a different order" in {}`)

Como adelantaba al inicio del post, nuestros tests cuentan más de una historia, y creo que es interesante hacer el ejercicio de pensar como contar cada una de esas historias de la forma más cómoda posible.

### Reutilizando algunos pasos complejos

Por alguna razón, solemos ser bastante más permisivos con la duplicación de código en los tests que en producción y muchas veces renegamos de hacer abstracciones. Esto es, extraer funcionalidad común a un método o una clase.

Creo que este punto puede ser polémico, así que intentaré dejar claro desde el princpio que **esto es solo una sugerencia** tan válida como otra cualquiera y que siempre es susceptible plantear otra aproximación dependiendo del caso.

Habitualmente me encuentro dos motivos principales para no hacer esto:

#### 1. El código de test es tan sencillo que no me importa copiar y pegar

Entiendo que yo soy un poco obsesivo de más con el tema de código duplicado y trato de evitarlo al máximo posible. Quizás a veces peco demasiado de ello, pero creo que copiar/pegar código no debería ser la solución casi nunca.

> Muy probablemente pienso así porque no suelo hacer las cosas bien a la primera casi nunca. Y cuando me quiero dar cuenta me toca arreglar en dos o tres sitios.

El problema de esto, no es solo que tienes el mismo código duplicado una y otra vez, con la poca tolerancia a cambios que ello conlleva. Es que puede **llegar a ser dificil resumir qué hace ese fragmento de código que estás duplicando** y cada vez que nos encontramos con ello **tenemos que hacer el esfuerzo de entender qué hace ese código**. Por no hablar de la perecísima que da enfrentarse a un fichero de test con 600 líneas.
Os intento mostrar un ejemplo real de una abstracción de código con un naming no especialmente bueno para que cada cual valore que valdría más la pena:

En este ejemplo se prueba el acceso a base de datos para recuperar objetos del tipo "BudgetLine" que representa una entrada en los presupuestos de una empresa. Dicha entidad se puede buscar por un montón de criterios:

```kotlin
    @Test
    fun `it should find the line by status criteria`() {
        val budgetLine = givenABudgetLineHasItsDependenciesCreated()
        val criteria = BudgetLineSearchCriteria(statuses = listOf(it.status))
        database.inTransaction {
            val created = budgetLineRepository.create(budgetLine)
            val found = budgetLineRepository.findAllBudgetLines(criteria, Pagination())
            val count = budgetLineRepository.countAllBudgetLines(criteria)
            listOf(created) shouldBe found
            count shouldBe 1
        }
    }
```
Este código no es dificil de entender y no parece que vaya a cambiar, por lo que no me importa copiarlo y pegarlo una y otra vez. Sin embargo si lo duplicase, cada vez que me encuentre con este bloque de estas 10 líneas de código, tendría qué pensar qué hace. Tanto es así que seguramente acabaría utilizando la misma frase para describir este conjunto de test. Algo del estilo de "estos son los tests que encuetran resultados al buscar por un criterio".

Entonces... ¿cuánto me costaría hacer una abstracción y cuánto me costaría comprender qué hace?

```kotlin
@Test
fun `it should find the line by status criteria`() {
  val criteria = BudgetLineSearchCriteria(statuses = listOf(it.status))
  whenFindByMatchingCriteriaShouldReturnAnyResult{
    BudgetLineSearchCriteria(statuses = listOf(it.status))
  }
}

@Test
fun `it should find the line by requester criteria`() {
  val criteria = BudgetLineSearchCriteria(statuses = listOf(it.status))
  whenFindByMatchingCriteriaShouldReturnAnyResult{
    BudgetLineSearchCriteria(requester = it.requester)
  }
}

private fun whenFindByMatchingCriteriaShouldReturnAnyResult(
  criteriaFactory: (BudgetLine) -> BudgetLineSearchCriteria) {
        val budgetLine = givenABudgetLineHasItsDependenciesCreated()
        val criteria = criteriaFactory(budgetLine)
        database.inTransaction {
            val created = budgetLineRepository.create(budgetLine)
            val found = budgetLineRepository.findAllBudgetLines(criteria, Pagination())
            val count = budgetLineRepository.countAllBudgetLines(criteria)
            listOf(created) shouldBe found
            count shouldBe 1
        }
    }
```
\* _Esto es código copiado tal cual de un proyecto real. Con sus typos y sus erratas_ 😅

A priori, debería ser mucho más fácil de leer, ¿no? Pero esto nos lleva al siguiente problema:

#### 2. Si haces abstracciones en un test, añades un nivel de complejidad que te impide ver qué hace el test de un vistazo

Aquí toca cuestionarse justo la inconveniencia del "problema" que acabamos de resolver.
Para mi es una cuestión de gustos, pero creo que es necesario tratar el tema en este post para que, quienes estéis leyendo, podáis elegir en función de vuestras preferencias. Al fin y al cabo, **nuestro día a día consiste en llegar a ciertos compromisos entre lo que ganas y lo que pierdes en cada solución**. Como dicen por ahí: "siempre hay trade offs".

Es cuestión de plantearnos lo siguiente:

* ¿Prefiero tener **toda la información disponible en un mismo sitio** a costa de tener que interpretar toda esa información multiples veces? Esto reduce los niveles de indirección y nos centra el **foco más en cómo lleva a cabo la tarea el código** que en la tarea en sí misma.
* ¿Prefiero tener **una abstracción que no me requiera pensar mucho** y ya, si tengo tempo y ganas, entraré en el detalle? Al fin y al cabo mi IDE me ayuda y llego al detalle de la abstracción a golpe de "ctrl+click"

Pues la respuesta no es trivial y, como digo, depende bastante de la situación. De los gustos de quien está trabajando en el proyecto, de la cantidad y complejidad de las abstracciones qué hacemos. Y esto creo que es algo que debe consensuarse con el equipo. Al fin y al cabo, son quienes integran el equipo, las personas encargadas de entenderlo y mantenerlo.


### Given When Then

Esta forma de definir los escenarios se popularizó mucho a medida que Cucumber fue ganando terreno. Con este framework se pretendía definir los escenarios de nuestros tests en lenguaje natural. Por si necesitas un poquito más de detalle, te dejo el enlace al post donde hablo brevemente sobre las [principales familias de frameworks](/blog/2020/06/08/frameworks-de-tests.html#familia-bdd).

Muy en resumen:

* **GIVEN:** nos ayuda a definir las precondiciones que se deben cumplir antes de ejercitar nuestro SUT.
* **WHEN:** sirve para detallar la acción que queremos probar.
* **THEN:** nos ofrece semantica para realizar las verificaciones.

Si bien los frameworks tipo BDD utilizan el lenguaje [Gherkin](https://cucumber.io/docs/gherkin/) que fuerza a usar este formato, nada nos impide aprovecharnos del mismo en nuestro framework de testing.

Creo que [Pedro Gómez](https://twitter.com/pedro_g_s) lo plasma muy bien en su [emulador de gameboy para android](https://github.com/pedrovgs/AndroidGameBoyEmulator/blob/master/app/src/test/java/com/github/pedrovgs/androidgameboyemulator/core/GameBoyBIOSTest.java) donde tiene unos tests que da gusto leerlos!. Aunque él en general solo usa la nomenclatura de given, pero creo que plasma muy bien la idea y, al menos a mi, me sirvió de inspiración.

> Lo mejor de todo, es que no es postureo. Pedro hace los test de esta manera siempre y creedme si os digo lo mucho que me ha facilitado la vida en todos los proyectos que he compartido con él.

Por si no tenéis ganas de navegar al enlace, os dejo aquí el ejemplo de antes teniendo en cuenta esta aproximaión donde tendríamos algo así:

```kotlin
  @Test
  fun `it should find the line by status criteria`() {
    database.inTransaction {
      val created = givenABudgetlineIsCreated()
      val found = whenFindForAPaginatedCollectionByCriteria(
        BudgetLineSearchCriteria(statuses = listOf(it.status))
      )

      thenShouldFindOnlyTheCreatedLine(found, created)
    }
  }
```

De nuevo, hay quien dirá que estas abstracciones te quitan el detalle de cómo funciona realmente tu código. Y es cierto. Pero como en todo, nos tocará pensar con qué mindset querría leer el test.

Yo personalmente, creo que el precio de perder el detalle de un primer vistazo compensa por el hecho de:

* Estamos **reduciendo la carga cognitiva** a la hora de describir qué hace nuestro producto.
* Hemos **creado una "definición formal" y reutilizable** de nuestras "primitivas" (crear una línea presupuestaria, buscar por criterio, verificar resultados, etc.).
* Tenemos una **batería de pasos que podremos reutilizar en varios tests** y cambiarla en un solo sitio si fuera necesario.
* Si nuestro producto cambia, tan **solo tendríamos que modificar la absracción y nuestros tests seguirían funcionando**.

Por el contrario, cada vez que queramos saber qué implica cada una de esas acciones, tendremos que alejarnos del código del test. Y, si no tenemos cuidado, podemos acabar con un proyecto de test demasiado complejo. 

_En este punto me gustaría comentar que algo "complejo" es, en muchas ocasiones, algo subjetivo. Lo que para un equipo es algo complejo y sofisticado, para otro podría ser rudimentario y sencillo. Así que, por favor, tengamos en cuenta al equipo entero para tomar este tipo de decisiones. No las implementemos solo porque alguien nos dijo que había que hacerlo._

Y aunque seguro que me dejo muchas cosas en el tintero, voy a ir dejándolo aquí que, como de costumbre, ya me he alargado demasiado. 

Muchas gracias por haber llegado hasta aquí ❤️ ❤️.

**¿Y tú? ¿Cómo haces que tus tests sirvan de documentación?**
