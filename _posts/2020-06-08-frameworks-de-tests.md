---
layout: commented_post
title: Las principales familias de frameworks de testing
date: 2020-06-08 12:00:00 +0200
image: /assets/posts/2020-06-08/testing-frameworks-avatar.png
categories: blog
excerpt:
  En este segundo post de la serie "Los tests también son código" quiero contaros como funcionan típicamente las familias de frameworks de tests más comunes para ver cuales son los mecanismos que nos ofrecen a la hora de escribir nuestros test y cómo poder sacarle provecho a dichos mecanismos.
---
En este segundo post de la serie ["Los tests también son código"](/blog/2020/06/06/lost-test-tambien-son-codigo.html) quiero contaros como funcionan típicamente las familias de frameworks de tests más comunes para ver cuales son los mecanismos que nos ofrecen a la hora de escribir nuestros test y cómo poder sacarle provecho a dichos mecanismos.

Del mismo modo que cuando nos planteamos usar un framework para desarrollar nuestro sistema he considerado interesante que conozcamos un poco los principales frameworks de testing. Como veremos, para el caso común, tienen una dinámica bastante sencilla, aunque también nos ofrecen bastantes mecanismos para tunear el desarrollo y la ejecución de nuestros tests. Sin embargo, como decía para la mayoría de los casos, deberíamos ser capaces de probar con los mecanismos básicos ya que como en todo, **añadir sofisticación al asunto simplemente porque podemos, no suele ser una buena idea**.

Antes de comenzar a hablar de los frameworks quiero detenerme un momentito en describir la forma en la que se suele plantear el desarrollo de un test ya que podemos considerar dicho planteamiento como _"la definición de producto de nuestras herramientas de testing"_.

## Fases de un test

Podemos asumir que cada test tiene 4 fases:

1. **Setup** del SUT (system/subject under test): En esta fase preparamos el sistema para probarlo. Elegimos el scope, definimos el comportamiento de los mocks si fuera necesario, o añadimos información en los storages y en definitiva **dejamos nuestro SUT en un estado en el que podamos ejecutar la prueba**.<br/>
<span class="soft">* En mi experiencia, la primera fase, es donde se acumula la mayor parte del código de un test. Además, es el código que más nos aburre escribir. Y ya sabemos que, si nos aburre, tendemos a escaquearnos porque mentalmente supone una carga grandísima afrontar un trabajo que da pereza :s. No obstante, en tests con scope grande, el resto de fases son susceptibles de complicarse también.
</span>

2. **Ejercitar** el SUT: Aquí es donde nuestro código de test interactúa con el código de producción. En tests con scope pequeño, esta fase suele conllevar una sola línea, aunque si probamos flujos más complejos que conllevan varios pasos, podrían ser más.
3. **Verificar**: En esta fase comprobamos que la ejecución ha ido como debería. Hay bastante literatura sobre si habría que hacer una sola aserción por test o no. En cualquier caso, podríamos encapsular varias aserciones simples en una más compleja y darle cierta semántica para convertirla en "una sola aserción".
4. **Teardown**: De un test saludable, esperamos que deje nuestro sistema tal y como estaba antes de ejecutar. Cuando nuestro SUT no tiene estado, muy probablemente no tendremos que hacer nada en esta fase (si acaso resetear algún mock, dependiendo de la librería que usemos). No obstante, en tests con estado, que atacan a una base de datos, un API o cualquier servicio con estado, sería deseable limpiar los datos. Es cierto que **esta fase puede ser compleja y generar problemas. Este es uno de los aspectos por los cuales se fomenta que habitualmente hagamos test con scope pequeño** ya que de esta forma no suele darnos mayor complicación y muchas veces se queda en blanco.
<br/>
<span class="soft">* Como anécdota, os diré que he vivido en primera persona (y he leído otro caso), en el que se borraron todos datos de usuarios en producción al ejecutar tests que gestionaban el teardown de la manera más simple: _"Como estoy en modo test, simplemente borro toda la base de datos y así lo dejo todo como estaba"_. Este planteamiento es perfecto en modo desarrollo, pero ¿qué pasa cuándo alguien ejecuta los tests apuntando contra producción? :S.</span>

## Mecanismos que nos ofrecen los frameworks
Una vez descritas las fases, repasemos brevemente cuales son las principales familias de frameworks para ver, según mi experiencia, cómo solemos mapear las fases. _Ya os adelanto, que ningún framework que yo conozca ofrece un mapeo uno a uno. !Y tiene sentido!_:

### Familia xUnit
Los frameworks de la familia xUnit mapean la ejecución de los tests con métodos, generalmente mediante anotaciones. Además, ofrece típicamente, 4 hooks para ejecutar código antes y después de cada test o de la suite completa.

En el siguiente snippet muestro con "pseudo-kotlin/junit" el mapeo típico entre los mecanismos de los tests y las fases. Las llamadas a métodos son simplemente para poner un ejemplo típico de uso:

```kotlin

  @BeforeClass
  fun setUpPerSuite() {
    //parte del setUp común a todos los tests y costoso por lo que solo se intenta ejecutar una vez.
    initializeSUTAndMocks()
    startServerOrDatabase()
  }

  @Before
  fun setUpPerClass() {
    //parte del setUp común a todos los test y que necesita ejecutarse en cada test
    defineCommonMocksBehaviorOrInitializeDatabase()
  }

  @Test
  fun testExecution() {
    //setUp concreto de este test
    //ejecución del test
    //aserciones del test
    //teardown concreto de este test
  }

  @After
  fun tearDownPerTest() {
    //parte del tearDown común a todos los test pero que se ejecutará en cada test     resetMocks()
  }

  @AfterClass
  fun tearDownPerSuite() {
    //parte del tearDown común a todos los tests pero que se ejecutará una sola vez.
    stopServer()
  }
```
Por aclarar:

* `initializeSUTAndMocks`: Suele ser el momento en el que se inicializa el código a probar, se crean los mocks, etc.
* `startServerOrDatabase`: Algunas de las acciones de esta acción podrían ser la de levantar el stack para probar peticiones pasando por todo el framework (proyecto backend), levantar una base de datos embebida o un sqlite de prueba, o arrancar un MockServer en caso de querer probar la interacción con un API externa.
* `defineCommonMocksBehaviorOrInitializeDatabase`: creo que este nombre se explica solo :S
* `resetMocks`: esta es una llamada típica en esta fase, cuando usamos una librería de mocks que necesita ser reseteada para volver a configurar, o simplemente para evitar comportamientos inesperados.

* `stopServerOrCleanUpDatabase`: Realizaría la acción contraría a `startServerOrDatabase` y por lo tanto nos interesará hacerla una sola vez.

Cuanto menor sea el scope del test, más habitual es que las fases de before y after queden vacías y generalmente lo implementamos todo en el propio test. Aunque esto no tiene porque ser necesariamente así. Por mi parte, os animo a plantearos, en algunos casos, otras estrategias, como no asumir una clase de test por clase que quiero probar.
Siguiendo con el ejemplo del primer post podríamos plantear el siguiente test:

```kotlin
class SignUpSuccessTest {
    val generateId: GenerateIdForUser = mock()
    val notifier: Notifier = mock()
    val userValidator: UserValidator = UserValidator()
    val timeProvider: TimeProvider = DummyTimeProvider()
    val userRepository: UserRepository = mock()
    lateinit var createdUser: User

    val signUp = SignUp(
        generateId = generateId,
        notifier = notifier,
        userValidator = userValidator,
        timeProvider = timeProvider,
        userRepository = userRepository
    )

    @Before
    fun `given a user is successfully created`() {
        val userRequest = givenAValidCreationRequest()
        val userId = "myUserId"
        val creationDate = Date(12345)
        val expectedUserToBeCreated = givenAUserForARequest(userRequest, userId, creationDate)
        generateId.givenIdIsGenerated(userId)
        timeProvider.withTime(creationDate)
        userRepositoryMock.givenUserIdAdded(expectedUserToBeCreated)

        createdUser = signUp(userRequest)
    }


    @Test
    fun `it should return the created user`() {
        createdUser shouldBe expectedUserToBeCreated
    }

    @Test
    fun `it should persist the user`() {
        userRepository.shouldPersistUser(createdUser)
    }

    @Test
    fun `it should notify the signUp`() {
        notifier.shouldNotifySignUp(createdUser)
    }
}
```

**Ventajas**:

* Son bastante simples por lo que su curva de aprendizaje es muy asequible.

**Desventajas**:

* No suelen tener mecanismos sofisticados para agrupar tests (aunque algunos frameworks como [TestNG](https://testng.org/doc/index.html) si que los ofrecen).
* No suelen tener mecanismos sofisticados para ejecutar tests parametrizados.

<span class="soft">* Para quienes trabajáis con este tipo de frameworks y sentís que os está faltando algo, os recomiendo echarle un ojo al índice del libro <a href="http://xunitpatterns.com">xUnit Patterns</a> en busca de inspiración.</span>

### Familia spec

La familia spec, creo que tiene su implementación de referencia en el framework [RSpec](https://rspec.info) de Ruby y la comunidad de Javascript adopto esta aproximación.

Básicamente tienen los mismos mecanismos que la familia xUnit, aunque los suelen ofrecer de otra manera. Con esta familia he trabajado pero menos, así que espero que me perdonéis si hay algún gazapo:

```javascript
//using mocha in JS
describe('Test suite', () => {
  beforeEach(() => {
    //parte del setUp común a todos los test y que necesita ejecutarse en cada test
  });
  before(() => {
    //parte del setUp común a todos los test y que necesita ejecutarse en cada test
  });
  it('should add proper description for this test here', () => {
    //setUp concreto de este test
    //ejecución del test
    //aserciones del test
    //teardown concreto de este test
  });

  afterEach(() => {
    //parte del tearDown común a todos los test pero que se ejecutará en cada test.
  });
  after(() => {
    //parte del tearDown común a todos los tests pero que se ejecutará una sola vez.
  });

});
```

Como vemos, podemos usar los mismos mecanismos que con los frameworks xUnit pero estos, por la forma en la que definen los escenarios de prueba favorecen el uso del lenguaje natural en la descripción.

Además, esta familia tiene una herramienta tan potente como potencialmente liosa: las suites anidadas. De esta manera podemos hacer grupos de test permitiéndonos reutilizar bastante código del setup y el tear down de nuestros tests. El problema de abusar de esta característica es que puede hacer complejo seguir el flujo de ejecución de un test.

Os dejo varias alternativas aquí:

```javascript
describe('Test suite', () => {
  describe('Un solo test con multpiles aserciones', () =>{
    before(() => {
      //setup del test
      //ejecución del código de producción
    });
    it('should happen this thing here', () => {
      //aserciones del test
    });
    it('should also happen this', () => {
      //aserciones del test
    });
    it('... and this', () => {
      //aserciones del test
    });
  });

  describe('una suite anidada para compartir código de setUp', () =>{
    before(() => {
      //setup del test
    });
    it('should test something using a common setUp', () => {
      //ejecución del código de producción
      //aserciones del test
    });
    it('should test another thing using a common setUp', () => {
      //ejecución del código de producción
      //aserciones del test
    });
  });
});
```

Como veis, hay bastantes posibilidades de agrupación, pero tened en cuenta las peculiaridades de cada framework al hacer esto. _Por ejemplo [mocha](https://mochajs.org), no asume que los métodos before(Each) o after(Each) puedan producir errores y si esto sucede, se interrumpe la ejecución del resto de tests del proyecto._ Lo cual no es nada deseable.

**Ventajas**:

* Mecanismos nativos para agrupar tests.
* Favorecen la descripción de los escenarios en un lenguaje muy cercano al de negocio.

**Desventajas**:

* Es muy fácil sobrecomplicar la estructura de la suite de tests.
* No suelen tener mecanismos sofisticados para ejecutar tests parametrizados.

### Familia BDD

Por último, tenemos los frameworks más cercanos al lenguaje de negocio. En este caso la implementación de referencia sería [Cucumber](https://cucumber.io), también de Ruby que pretende ofrecer un mecanismo de comunicación claro entre Producto y Desarrollo. Para ello define un lenguaje llamado [Gherkin](https://cucumber.io/docs/gherkin/) y una forma de mapear dicha especificación a la ejecución de código. Debido a su planteamiento inicial, este tipo de frameworks está pensado para test con un scope grande. Típicamente para pruebas que atacan al sistema completo desde su interfaz.

Os dejo un ejemplo sencillo de un signUp definido con Gherkin:

```gherkin
@website
@recorded
Feature: Test the site's sign up
  In order to test make a didactic example will test a sample website i.e. mysite.com!!

  Scenario Outline: User goes to mysite.com and tries to create an account with valid data so it should be logged into the system.
    Given user is in registration page
    When user creates an user with "valid-user"
    Then the user should be created

  Scenario Outline: User goes to atrapalo.com and tries to create an account with invalid data so system will notify about the error.
    Given user is in registration page
    When user creates an user with "<UserData>"
    Then the user will not be created due to <NumberOfErrors> errors are found
  Examples:
    | UserData                   | NumberOfErrors |
    | user-with-invalid-password | 1              |
    | empty-user                 | 6              |
````

_Por simplificar, he añadido identificadores del tipo de usuario que querría registrar en lugar de todos sus valores. Si queréis ver cómo lo trato, tenéis el código disponible en este [repo](https://github.com/delr3ves/smashtechtalk) que preparé para una sesión de live coding de menos de una hora._

Y su mapeo a la ejecución de pasos:

```kotlin
class NavigationSteps {
    @Given("^user is in registration page$")
    fun userIsInRegistrationPage() {
      //execute the step here
    }
}

class UserCreationSteps {

    @When("^user creates an user with \"(.*?)\"$")
    fun `user creates a user with`(String userDescription) {
    }

    @Then("^the user should be created$")
    fun `the user should be created` {
    }

    @Then("^the user will not be created due to (\\d+) errors are found$")
    fun `the user will not be created due to some errors are found(Integer numberOfReasons) {
    }

}
```

Como veis el mecanismo es sencillo: _mapeamos los pasos definidos en Gherkin a código usando expresiones regulares de las cuales podemos además, extraer valores_. Además, el runner nos ofrece snippets de código cuando los escenarios no encuentran una implementación de los pasos.
Y, por supuesto, como buen framework que se precie, nos proporciona los hooks pertinentes para ejecutar antes y después de cada escenario o de la suite completa.

Está chulo, ¿verdad? Pues para no variar, tiene sus cosas buenas y sus cosas no tan buenas :).

**Ventajas**:

* Describen de los escenarios en lenguaje de negocio.
* Define un vocabulario específico de test que nos permiten saber qué consideramos precondición (**Given**), ejecución (**When**) o aserción (**Then**)
* Se favorece la reutilización de los pasos de cada escenario.

**Desventajas**:

* No hay herramientas que faciliten el refactoring en el código de Gherkin.
* Hay que escribir cada paso en cada escenario. Si por ejemplo necesitas un usuario logado y creado para ejecutar tu test, tendrás que definir esos dos pasos en cada tests.
* Sincronizar los cambios entre Gherkin y el código de test es tedioso una vez se ha implementado el escenario. <span class="soft">A mi me afecta especialmente esto porque mi inglés no es todo lo bueno que debería y a veces quiero cambiar la redacción y ya hay que tocar en muchos sitios.</span>
* Es difícil trabajar con datos dinámicos.
* No hay mecanismos nativos para reutilizar datos entre cada paso. <span class="soft">A veces, el resultado de un paso genera un dato que necesito en el siguiente. Por ejemplo, si pruebo un API rest y creo un recurso, el id se generará en el servidor y luego lo necesitaría para recuperar el recurso, pero no tengo acceso a él.<span>

## Elementos de un framework de testing

Por último creo conveniente hablar de los aspectos principales de un framework de testing para entender cómo podemos adaptarlo a nuestras necesidades y para desmitificar algunas de las técnicas de testing supuestamente modernas y complicadas que en realidad lo que hacen es tunear alguno de estos elementos.

### Runner

Es el elemento encargado de ejecutar los tests en si. No solo debe ser capaz de ejecutar el código escrito en el lenguaje elegido para nuestros tests sino que también, en los casos en los que se interactúa con el código y no con el sistema completo, deben ser capaces de ejecutar el código de producción que estamos probando.

Típicamente estos runners ofrecen una interfaz CLI que nos permite configurar la ejecución para, por ejemplo:

* Ejecutar un subconjunto de tests.
* Configurar un sistema de reporting específico.
* Ejecutar en un modo que permita calcular la cobertura de nuestros tests.
* Arrancar en modo debug.

Es fundamental esta configuración de grano fino, sobre todo si queremos **integrar** el runner con nuestro **IDE** o con nuestra **herramienta de build** (gradle, maven, sbt, rake, make, yarn o whatever...) y, en última instancia, con nuestros servicios de **Intergación Continua**. Por suerte, la mayoría de los frameworks de prueba más utilizados ya ofrecen esta integración con los principales IDEs.

### Lifecycle

Como hemos hablado en la sección anterior, define cómo se ejecutan los test y cuales son los mecanismos que nos ofrecen para preparar dicha ejecución.

### Asserciones

Aunque no es obligatorio que un framework de testing tenga su propia librería de aserciones si que es común. No obstante, como mínimo es necesario ofrecer un mecanismo para notificar si un test ha ejecutado correctamente o ha fallado. Generalmente esto se consigue con una Excepción.

Por dar un poco de contexto, cuando hablamos de librerías de Snapshot o Screenshot testing, en donde nos centramos principalmente es en el componente de Asserciones del framework. _Aunque también influyen en el runner para ejecutar en modo grabación :P_

### Reporting

Este aspecto del framework lo solemos dar por supuesto porque nunca entramos en él, pero es deseable que nuestro framework de testing nos permita configurar la forma en que obtenemos el resultado de nuestros tests. Por un lado, nos interesa a nosotros como personas saber cómo ha ido todo y en caso de fallo, tener la mayor información posible acerca del error.
Por otro lado, nos puede interesar tener un formato más fácilmente procesable por analizadores estáticos de código o nuestro servicio de integración continua.

Además, si conocemos los mecanismos de reporting y ciclo de vida que nos ofrece nuestro framework, podríamos plantearnos hacer cosas tan chulas como grabar en video la ejecución de nuestros tests de sistema y embeberlos en el reporting en caso de fallo.

### Interacción con el SUT

En general, cuando hablamos de test con un scope pequeño donde no atacamos directamente la vista de nuestro sistema, la interacción con el SUT nos la ofrece el propio lenguaje de programación.

Pero en el momento en que queremos interactuar con la vista de nuestro proyecto, necesitaremos alguna librería/fw para hacerlo. Normalmente esto queda fuera del framework en si. No obstante, es interesante mencionar este aspecto ya que nos condicionará mucho la forma en que escribimos nuestros tests. En este caso, podemos encontrar proyectos como:
* [Selenium](https://www.selenium.dev) o [Cypres](https://www.cypress.io) para web
* [Espresso](https://developer.android.com/training/testing/espresso/) para Android
* [Kif](https://github.com/kif-framework/KIF) para iOS

Y otros tantos proyectos que molan mucho cuando nos hablan de ellos y nos dan tanto coraje cuando nos toca utilizarlos porque no nos ofrecen todos los mecanismos que nos gustaría tener.

