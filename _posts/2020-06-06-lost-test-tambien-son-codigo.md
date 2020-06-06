---
layout: commented_post
title: Los tests también son código
date: 2020-06-06 12:00:00 +0200
categories: blog
image: /assets/posts/2020-06-06/tests-are-code-avatar.png
excerpt:
  A pesar de lo evidente que pueda parecer esta afirmación, me he decidido a escribir este post porque llevo muchos años
  con la sensación de que no somos del todo conscientes de esto.

  ¿En qué me baso para hacer esta afirmación? Pues tanto en conversaciones como en revisiones de código veo.
---
A pesar de lo evidente que pueda parecer esta afirmación, me he decidido a escribir este post porque **llevo muchos años
con la sensación de que no somos del todo conscientes** de esto.

¿En qué me baso para hacer esta afirmación? Pues tanto en conversaciones como en código que he escrito a lo largo de los años, código que he visto en proyectos donde he trabajado o que reviso a día de hoy donde veo problemas del tipo:

* Código duplicado cuando hacer una abstracción que lo encapsulara es trivial.
* Ningún esfuerzo por hacer código legible.
* Descripciones en los tests (nombre de los métodos) muy poco descriptivos.
* Mogollón de boilerplate  por no saber utilizar las herramientas.
* ...

Lo dije en su día y lo mantengo. Mucha gente ha aprendido a hacer tests por presión social. Es como esa película,
ese libro o ese grupo de música que deberíamos conocer porque todo el mundo lo hace y cuando nos preguntan, en lugar de decir
"mmmm, pues no lo conozco" decimos "sí, me suena, pero ahora mismo...". Al final acabamos por aprender a las malas y con pocas
ganas porque nos puede parecer un rollo o algo impuesto.

> En ese sentido, en algunos proyectos los tests llevan un poco de rollete Schrödinger. Alguien nos obliga y alguien nos prohibe al mismo tiempo hacerlos :S.

Lo que pretendo con esta serie es contar cómo lo hago yo por si os sirve de ayuda. Y por si alguien me da pistas para arreglar mis fallos.
Pero sobre todo, quedaos con que:

>los tests son código y los mismos mecanismos de que utilizáis en vuestro código son susceptibles de serviros aquí.

No obstante, hay cierta problemática específica de los tests en la que intentaré centrar en esta serie de posts.

**Eso sí, como normalmente no hay nadie que pruebe el código que prueba nuestro código** (obviando los tests de mutaciones) **debemos intentar mantenerlo lo más simple y declarativo posible :)**

Antes de entrar de lleno en la materia, en este primer post, me gustaría daros un poco de contexto para ver código de algunos tests que he ido encontrando y tener un punto de partida sobre el que poder ir iterando:

<div class="disclaimer">
Este código lo he escrito sobre la marcha y no garantizo que sea sintácticamente correcto al 100%. Además, por simplicidad, hay artefactos de los que no he añadido el código. Digo esto porque si váis buscando el más mínimo detalle es posible que encontréis algún error, pero espero que sea suficientemente ilustrativo.
</div>

Tratemos de probar  este código de producción (_con un número incómodo a la par que típico de dependencias y con su cuestionable gestión de flujos alternativos con excepciones_ :P):

```kotlin
class SignUp(
    private val generateId: GenerateIdForUser,
    private val notifier: Notifier,
    private val userValidator: UserValidator,
    private val timeProvider: TimeProvider,
    private val userRepository: UserRepository
) {
    @Throws(ValidationError::class)
    operator fun invoke(request: UserCreationRequest): User {
        val userToBeCreated = request.toCreatedUserInTime(
            id = generateId(),
            creationDate =timeProvider.now()
        )
        userValidator.validateSignUp(userToBeCreated)
        val createdUser = userRepository.add(userToBeCreated)
        notifier.notifySignUp(createdUser)
        return createdUser
    }
}
```

Una posible suite de test podría ser:

```kotlin
class SignUpTest {
    val generateMockedId: GenerateIdForUser = mock()
    val notifierMock: Notifier = mock()
    val userValidatorMock: UserValidator = mock()
    val timeProviderMock: TimeProvider = mock()
    val userRepositoryMock: UserRepository = mock()

    val signUp = SignUp(
        generateId = generateMockedId,
        notifier = notifierMock,
        userValidator = userValidatorMock,
        timeProvider = timeProviderMock,
        userRepository = userRepositoryMock
    )

    @Test
    fun testSignUp() {
        val userRequest = UserCreationRequest(
            email = "sergio@serch.dev",
            firstName = "Sergio",
            lastName = "Arroyo",
            imageUrl = "http://myavatar/delr3ves"
        )
        val expectedUserToBeCreated = User(
            id = "myUserId",
            email = "sergio@serch.dev",
            firstName = "Sergio",
            lastName = "Arroyo",
            imageUrl = "http://myavatar/delr3ves",
            sessionToken = null,
            roles = setOf(),
            creationDate = Date(12345),
            enabled = true
        )
        whenever(generateId.invoke()).thenReturn("myUserId")
        whenever(timeProvider.now()).thenReturn(Date(12345))
        whenever(userRepositoryMock.add(expectedUserToBeCreated)).thenReturn(expectedUserToBeCreatedr)

        val createdUser = signUp(userRequest)

        createdUser shouldBe expectedUserToBeCreated
        verify(userRepositoryMock).add(expectedUserToBeCreated)
        verify(notifier).notifySignUp(createdUser)
    }

    @Test(expected = ValidationError::class)
    fun testFailingSingUp() {
        val userRequest = UserCreationRequest(
            email = "sergio@serch.dev",
            firstName = "Sergio",
            lastName = "Arroyo",
            imageUrl = "http://myavatar/delr3ves"
        )
        whenever(userValidatorMock.validateSignUp(any())).thenThrow(ValidationError("any error"))
        try {
            signUp(userRequest)
        } finally {
            verify(userRepositoryMock, never()).add(expectedUserToBeCreated)
            verify(notifier, never()).notifySignUp(any())
        }

    }
}
```

_Esto es un poco frustrante porque en realidad, de todo este código de pruebas, solo hay 5 o 6 líneas que realmente están probando (ejecutando o verificando) nuestro código. Todo lo demás es preparación de nuestros tests :(._

Supongo que además, cada cual habrá visto algún aspecto cuestionable en esta pequeña suite de tests. Sin embargo, he tratado de condensar en un ejemplo simple (y con un código de producción sencillo), muchos de los escenarios que me encuentro en la mayoría de los proyectos que tenían los tests y cuyo código de producción estaba razonablemente cuidado. Es decir, **código de test descuidados de equipos que se preocupan por su código de producción**.

Algunas de ellas son:

* Uso frecuente de magic strings y magic numbers.
* Descripción de los escenarios bastante escueta.
* Es dificil distinguir las partes del test.
* Bastante código de inicialización de objectos.
* Bastante código de configuración de mocks.
* Varias aserciones en cada test.
* Quizás se podría considerar que hay sobreespecificación al tener una configuración estricta de los mocks.
* En general tests tediosos de leer por lo que posibles defectos se nos podrían pasar por alto.
* ...

¿Y qué os parecería si hiciera los siguientes cambios?

```kotlin
class SignUpTest {
    val generateId: GenerateIdForUser = mock()
    val notifier: Notifier = mock()
    val userValidator: UserValidator = UserValidator()
    val timeProvider: TimeProvider = DummyTimeProvider()
    val userRepository: UserRepository = mock()

    val signUp = SignUp(
        generateId = generateId,
        notifier = notifier,
        userValidator = userValidator,
        timeProvider = timeProvider,
        userRepository = userRepository
    )

    @Test
    fun `signUp should return the created user`() {
        val createdUser = givenUserIsSuccessfullyCreated()
        createdUser shouldBe expectedUserToBeCreated
    }

    @Test
    fun `signUp should persist the user`() {
        val createdUser = givenUserIsSuccessfullyCreated()
        userRepository.shouldPersistUser(createdUser)
    }

    @Test
    fun `signUp should notify the signUp`() {
        val createdUser = givenUserIsSuccessfullyCreated()
        notifier.shouldNotifySignUp(createdUser)
    }

    @Test(expected = ValidationError::class)
    fun `signUp with invalid request should fail`() {
      forall(
        row(givenRequestWithInvalidEmail()),
        row(givenRequestWithInvalidPassword())
      ) { invalidRequest ->
        signUp(userRequest)
      }
    }

    @Test
    fun `signUp with invalid request should not persist the user`() {
      givenUserCanNotBeCreatedShouldNotPerformAction {
        userRepository.shouldNotPersistAnyUser()
      }
    }

    @Test
    fun `signUp with invalid request should not notify the signUp`() {
      givenUserCanNotBeCreatedShouldNotPerformAction {
        notifier.shouldNotNotifyTheSignUp()
      }
    }

    private fun givenUserIsSuccessfullyCreated(): User {
        val userRequest = givenAValidCreationRequest()
        val userId = "myUserId"
        val creationDate = Date(12345)
        val expectedUserToBeCreated = givenAUserForARequest(userRequest, userId, creationDate)
        generateId.givenIdIsGenerated(userId)
        timeProvider.withTime(creationDate)
        userRepositoryMock.givenUserIdAdded(expectedUserToBeCreated)

        return signUp(userRequest)
    }

    private fun givenUserCanNotBeCreatedShouldNotPerformAction(assertActionDintHappen: () -> Unit) {
      try {
        signUp(givenInvalidUserRequest())
        fail("Should fail")
      } catch (e: Exception) {
        assertActionDintHappen()
      }

    }
}
```

Quizás en este ejemplo aislado tengamos más líneas de código, pero yo creo que queda bastante más legible.

En los siguientes posts, iremos viendo cuál ha sido mi razonamiento para llegar desde la primera aproximación para probar hasta esta úlitma, en mi opinión más legible y con código reutilizable, no solo por esta suite de test sino por muchas otras.
