---
tags:
  - kotlin
  - flujo
---
>[!NOTE]
>El valor devuelto por un cuerpo es la última expresión que tenga.

Una sentencia `if-else` puede utilizarse como una expresión. Es decir, para devolver un valor que pueda almacenarse en una variable.

```kotlin
fun main() {
	val haveSomeExtraMoney = true

	val tip: Double =
		if (haveSomeExtraMoney) {
			10.0
		} else {
			0.0
		}
	println(tip) // 10.0
}
```

Si sólo tiene una expresión en el cuerpo, puede omitir los corchetes.

```kotlin
fun main() {
val haveSomeMoney = true
val tip : Double = if (haveSomeMoney) 10.0 else 0.0
}
```