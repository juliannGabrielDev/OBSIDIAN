---
tags:
  - kotlin
  - otros
---
### **Manejo de Excepciones en Kotlin**

Las excepciones son eventos que interrumpen el flujo normal de ejecución de un programa cuando ocurre un error. En Kotlin, puedes manejarlas utilizando bloques `try`, `catch` y `finally`, y también puedes lanzar excepciones con `throw`.

#### **Lanzar Excepciones**

Para lanzar una excepción, utilizas la palabra clave `throw` seguida de una instancia de una clase de excepción:

```kotlin
fun dividir(a : Int, b : Int) : Int {
	if (b == 0) {
		throw IllegalArgumentException("El divisor no puede ser cero")
	}
	return(a / b)
}
```

#### **Capturar Excepciones**

Para manejar las excepciones y evitar que el programa se detenga abruptamente, utilizas bloques `try-catch`:

```kotlin
fun main() {
	try {
		val resultado = dividir(10, 0)
		println("El resultado es $resultado")
	} catch (e: IllegalArgumentException) {
		println("Ocurrió un error: ${e.message}")
	}
}
```

#### **Bloque Finally**

El bloque `finally` se ejecuta siempre, sin importar si se lanzó una excepción o no. Es ideal para liberar recursos o realizar tareas que deben ejecutarse de todas formas:

```kotlin
try {
	// Código que puede lanzar una excepción
} catch(e: Excepcion) {
	// Manejo de la excepción
} finally {
	// Código que siempre se ejecuta
    println("Proceso finalizado")
}
```

#### **rear Excepciones Personalizadas**

Puedes definir tus propias excepciones creando clases que hereden de `Exception`:

```kotlin
class EdadNoValidaException(mensaje: String) : Exception(mensaje)

fun verificarEdad(edad: Int) {
    if (edad < 18) {
        throw EdadNoValidaException("La edad mínima es 18 años")
    }
}

fun main() {
    try {
        verificarEdad(16)
    } catch (e: EdadNoValidaException) {
        println("Error: ${e.message}")
    }
}
```

#### **Try como Expresión**

En Kotlin, `try-catch` puede usarse como una expresión que retorna un valor:

```kotlin
val numero = try {
    "123a".toInt()
} catch (e: NumberFormatException) {
    0
}
println("El número es $numero")
```