---
tags:
  - kotlin
  - variables
---
Si su texto contiene varias líneas, también es posible definirlo en una variable `String` en Kotlin delimitando el texto con comillas triples.

```kotlin
fun main() {
	val myString : String = """Hello
	This is a String
	It is on a multiple lines.
	"""
}
```

## Plantilla `String`

```kotlin
val age : Int = 20
val myString : String = "My age is $age"
```

## Métodos

- `.length`
- `.startsWith("hel")`: `true` si la cadena empieza por `Hel`
- `.endsWith("lo")`: `true` si la cadena termina con `lo`
- `.first()`: obtener el primer carácter
- `.last()`: obtener el último carácter
- `.equals("hello")`: `true` verdadero si la cadena es igual a `hello`
- `.uppercase()`
- `.lowercase()`
- `.substring(0, 5)`: obtener todos los carácteres a partir del segundo carácter.
- `.string.format()`