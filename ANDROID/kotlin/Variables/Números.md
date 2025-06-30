---
tags:
  - kotlin
  - variables
---
Todos los números pueden ser negativos, y puede utilizar guiones bajos (`_`) dentro de números más largos para mejorar su legibilidad.

```kotlin
fun main() {
	val million : Int = 1_000_000
	println(million) // 1000000
}
```

## Conversión

Puede transformar un tipo de número en otro utilizando métodos de transformación. Comienzan con el prefijo `to` y a continuación el tipo que desee.

```kotlin
fun main() {
	val i : Int = 10
	val l : Long = i.toLong()
}
```

## Prefijo y Postfijo

- Postfijo: `a++`. Se ejecuta después de evaluar la ecuación
- Prefijo: `++ a`. Se ejecuta antes de evaluar la ecuación.