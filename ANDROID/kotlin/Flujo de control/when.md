---
tags:
  - kotlin
  - flujo
---
## Sentencia `when`

```kotlin
fun main() {
	println("Is it going to rain?")
	val probability : Byte  = 70
	when {
		probability < 40 -> println("Unlikely")
		probability <= 80 -> println("Likely")
		probability < 100 -> println("Yes")
		else -> println("What?")
	}
}
```

## `when` como expresión

La única regla es que una expresión `when` debe tener un bloque `else` para que tenga un valor que devolver en caso de que no se elija ningún otro bloque.

```kotlin
fun main() {
	println("Is it to rain?")
	val probability : Byte = 70
	val text = when {
		probability < 40 -> "Unlikely"
		probabilty <= 80 -> "Likely"
		probability < 100 -> "Yes"
		else -> "What?"
	}
	println(text)
}
```

## `when` con un valor

```kotlin
fun main() {
	val number : Int = 1
	when (number) {
		1 -> println("Missed hit")
		2, 3, 4, 5 -> println("Hit with value $number")
		6 -> println("Critical hit")
		7..9 -> println("$number")
	}
}
```

## Comprobación de tipo

```kotlin
fun main() {
	var value : Any = "ABC" 
	println(value is String)

	value = 123
	
}
```