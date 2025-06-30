---
tags:
  - kotlin
  - clases
---
En Kotlin, los métodos son funciones que están asociadas a una clase o a un objeto.

## Definiendo un método básico

```kotlin
class Persona {
	fun saludar() {
		println("Hola ¿Cómo estas?")
	}
}
```

## Parámetros y Tipos de Retorno

Los métodos pueden recibir parámetros y devolver valores, permitiendo interactuar y procesar información.

```kotlin
class Calculadora {
	fun suma(a : Int, b : int) : Int {
		return a + b
	}

	fun multiplicar(a : Int, b : Int) = a * b
}
```

## Métodos Estáticos con Companion Objects

A veces necesitamos métodos que puedan ser llamados sin crear una instancia de la clase.

```kotlin
class Utilidades {
	companion object {
		fun imprimirMensaje(mensaje : String) {
			prinln(mensaje)
		}
	}
}

fun main() {
	Utilidades.imprimirMensaje("Mensaje sin instanciar la clase")
}
```

## Extensión de funciones

¿Y si pudieras añador nuevos métodos a clases existentes sin heredarlas? En Kotlin, puedes.

```kotlin
fun String.esPalindromo() : Boolean {
	return this == this.reversed()
}

println("radar".esPalindromo()) // true
```

## Funciones de orden superior y Lambdas

Kotlin maneja las funciones como ciudadanos de primera clase, lo que significa que puedes pasarlas y utilizarlas como argumentos.

```kotlin
fun operarNumeros(a : Int, b : Int, operacion : (Int, Int) : Int) ; int {
	return operacion(a, b)
}

fun main () {
	val suma = operarNumeros(5, 3) {x, y -> x + y}
	print(suma) // 8
}
```

## Visibilidad y modificadores de acceso

Controla quien puede acceder a tus métodos es crucial para mantener la integridad del código.

- **public** Accesible desde cualquier lugar.
- **private** Solo dentro de la clase donde se define.
- **protected** En la clase y sus subclases.
- **Internal** Dentro del mismo módulo.

## Sobrecarga de métodos

```kotlin
class Mensaje {
	fun enviar(destinatario : String) {/*...*/}
	fun enviar(destinatario : String, copia : String) {/*...*/}
	fun enviar(destinatario : String, adjuntos : List<File>) {/*...*/}
}
```

Es similar a hablar diferentes idiomas con el mismo verbo.

## Métodos Abstractos y Sobrescritura

Las clases y métodos abstractos establecen un contrato que las subclases debem cumplir.

```kotlin
abstract class Animal() {
	abstract fun hacerSonido()
}

class Gato : Animal() {
	override fun hacerSonido() {
		println("Miau")
	}
} 
```

## Desestructuración en Métodos

Kotlin facilita trabajar con clases que tienen múltiples propiedades.

```kotlin
data class Usuario(val nombre : String, val edad : Int)

fun imprimirUsuario(usuario : Usuario) {
	val (nombre, edad) = usuario
	println("Nombre: $nombre, Edad: $edad")
}
```