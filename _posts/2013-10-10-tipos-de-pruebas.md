---
layout: commented_post
title:  Tipos de Prueba
date:   2013-10-10 12:00:00 +0100
categories: blog
excerpt:
  Siguiendo con el hilo del anterior Post y en un intento por aclarar mis ideas intentaré resumir los tipos de pruebas. Seguramente haya tantas clasificaciones diferentes como Post sobre el tema podáis encontrar, no obstante creo que las ideas básicas coinciden.
---

Siguiendo con el hilo del anterior Post y en un intento por aclarar mis ideas intentaré resumir los tipos de pruebas. Seguramente haya tantas clasificaciones diferentes como Post sobre el tema podáis encontrar, no obstante creo que las ideas básicas coinciden.

Dicha clasificación servirá también para establecer un vocabulario común ya que la intención es contaros más experiencias y leer vuestros comentarios :)

Es posible que veáis que en algunos casos me extiendo más que en otros, no obstante, cada una de estas clasificaciones merece al menos una entrada así que si os sabe a poco, espero poder satisfacer vuestra curiosidad en futuros artículos ;)

y ahora, al turrón… Lo primero es comentar que no hay una única manera de clasificar los tipos de tests, de los distintos prismas con que se mire:

## Alcance

Relacionado con el tamaño de nuestro SUT (System Under Test) podemos distinguir entre:

### Pruebas de Sistema

Se pretende probar el sistema de extremo a extremo. Desde la interfaz de usuario hasta servicios externos que utiliza nuestro sistema (por ejemplo una Base de Datos, un servicio Rest, …)

### Pruebas de Integración

En este caso nuestro SUT será un subconjunto de nuestro sistema. Es típico este tipo de pruebas para verificar la correcta interacción entre los distintos módulos. Es también común utilizar este tipo de pruebas para probar esa “fina capa” que debería conectar nuestro sistema con otros servicios externos.

### Pruebas Unitarias

En este caso se pretende probar un único módulo de nuestro sistema. Muchas veces no queda clara la distinción entre un test de integración y uno unitario. ¿Se debería probar un solo método de una sola clase? ¿da igual que sea más de un método mientras sean de la misma clase?. El hecho es que no siempre es fácil aislar un objeto, incluso no siempre se puede ejecutar un solo método de manera aislada. No obstante, en mi opinión, lo fundamental es que nuestros test unitarios no interactuen con sistemas externos.

## Visibilidad

Dependiendo de si diseñamos nuestras pruebas conociendo **cómo** funciona el sistema o simplemente nos basamos en lo **qué** tiene que hacer.

### Tests de Caja Blanca
Lo tests de Caja Blanca asumen que tenemos acceso al código a probar y diseñamos nuestras pruebas en base a dicho código. Este tipo de pruebas, a menudo, tienen el problema de que nos obsesionemos con conseguir una cobertura de código alta y nos olvidemos de verificar que lo que estamos probando hace lo que tiene que hacer… o incluso si debería hacer eso u otra cosa :S

### Tests de Caja Negra
Las pruebas de Caja Negra simplemente se centran en que nuestro SUT haga lo que tiene que hacer. No importa como lo haga. En este caso vemos nuestro SUT como un sistema que transforma un conjunto de datos de entrada en otro de salida. Este tipo de pruebas suelen ser enemigas de (ayudan a detectar) diseños pobres ya que si nuestro SUT tiene efectos colaterales u ocultan los datos de salida será difícil (sino imposible) hacer comprobaciones sobre el resultado de la operación.

## Finalidad
En función de qué es lo que esperamos obtener con nuestros tests podemos clasificarlos en:

### Tests de Aceptación
Tratan de comprobar que se satisfacen los requisitos del cliente. En principio no solo se trata de requisitos funcionales, sino los no funcionales como documentación, tiempos de respuesta, escalabilidad, etc.

En la práctica mucha gente habla de pruebas de Aceptación como el subconjunto de pruebas de Sistema que verifica que nuestro producto construido se puede utilizar.

### Tests Funcionales
Comprueban los requisitos funcionales. Es decir, que el sistema tiene todas las funcionalidades que queríamos que tuviera.

### Tests No Funcionales
Además de los requisitos funcionales es importante tener en cuenta otras necesidades que no implican nuevas “features” para nuestro proyecto. Entre las pruebas no funcionales, unas de las más populares son:

### Tests de Stress, Carga, Performance, …
Relacionados con la capacidad de respuesta de nuestro sistema tanto en situaciones anómalas como en las situaciones esperadas. De esta manera podemos saber como y cuando nuestro sistema degrada. Estas pruebas pretenden responder cuestiones como ¿cuantos usuarios podemos soportar? ¿que pasa cuando vengan los “setecientos-mil-trillones” de usuarios (que por cierto nunca suelen llegar :P)?
