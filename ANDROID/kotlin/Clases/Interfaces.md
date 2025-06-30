---
tags:
  - kotlin
  - clases
---
## ¿Qué es una intefas en Kotlin?

Imagina que las interfaces son como guías que indican que funciones y propiedades deben existir, pero dejan libertad para decidir cómo se implementan. Son esencialmente plantillas que las clases deben seguir.

```kotlin
interface Volable {
	fun volar()
}
```

Aquí, `Volable` es una interfaz que declara una función `volar()`. Cualquier clase que implemente `Volable` se compromete a proporcionar su propia versión de `volar()`.

## Implementando una Interfaz

```kotlin
class Pajaro : Volable {
	override fun volar() {
		println("El pájaro agita sus alas y se eleva hacia el cielo.")
	}
}
```

La palabra `override` indica que estamos proporcionando nuestra propia implementación de `volar()` definida en la interfaz.

## Propiedades en Interfaces

Las interfaces pueden declarar propiedades, pero no almacenan su estado. Las clases que las implementan deben proporcionar el estado o implementar los getters y setters.

```kotlin
interface Animal {
	val nombre : String
	fun sonido() : String
}

class Perro(override val name : String) : Animal {
	override fun sonido() = "Guau"
}

val miPerro = Perro("Firulais")
println("${miPerro.nombre} dice ${miPerro.sonido()}")
```

## Funciones con implementaciones por defecto

A diferencia de otros lenguajes, Kotlin permite que las interfaces contengan implementaciones por defecto.

```kotlin
interface Saludable {
	fun saludar() {
		println("Hola, Encantado de conocerte.")
	}
}

class Persona : Saludable

val alguien = Persona()
alguien.saludar() // Hola, Encantado de conocerte.
```

Si es necesario, las clases pueden **sobrescribir** estas funciones.

## Múltiples intefaces y herencia

Kotlin permite que una clase implemente múltiples interfaces, lo que facilita la creación de clases con combinaciones únicas de comportamientos.

```kotlin
interface Nadable {
	fun nadar() {
		println("Nadando en el agua.")
	}
}

class Pato : Volable, Nadable {
	override fun volar() {
		println("El pato aletea y vuelva a baja altura.")
	}
}

val donald = Pato()
donald.volar()
donald.nadar()
```

## Resolviendo Conflictos entre Interfaces

Cuando varias interfaces proporcionan implementaciones por defecto de la misma función, debes resolver el conflicto en tu clase.

```kotlin
interface A {
	fun mostrar() = println("Interfaz A")
}

interface B {
	fun mostrar() = println("Interfaz B")
}

class C : A, B {
	override fun mostrar() {
		println("Resolviendo conflicto: ")
		super<A>.mostrar()
		super<B>.mostrar()
	}
}

val objectC = c()
objectC.mostrar()
```
```
Resolviendo conflicto:
Interfaz A
Interfaz B
```

## Uso de clase para lograr Polimorfismo

Las interfaces permiten que diferentes clases sean tratadas de manera uniforme.

```kotlin
interface Transportable {
	fun transportar()
}

class Coche : Transportable {
	override fun transportar() = println("Conduciendo un coche.")
}

class Bicicleta : Transportable {
	override fun transportar() = println("Pedaleando una bicicleta.")
}

fun iniciarViaje(medio : Transportable) {
	medio.transportar()
}
iniciarViaje(Coche())
iniciarViaje(Bicicleta())
```