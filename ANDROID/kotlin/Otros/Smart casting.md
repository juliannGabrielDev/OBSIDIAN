---
tags:
  - kotlin
  - otros
---
Smart casting es una característica de Kotlin que te permite usar una variable como si ya estuviera convertida a un determinado tipo tras comprobarlo mediante sentencias condicionales. El compilador se encarga de aplicar el cast de forma automática.

```kotlin
fun imprimirLongitud(valor : Any) {
	if (valor is String) {
		println("Longitud: ${valor.length}")
	} else {
		println("No es un String")
	}
}
```

## Condiciones para el smart casting
Para que smart casting funcione, la variable debe ser inmutable o no modificarse entre la comprobación y su uso. De lo contrario, el compilador no puede garantizar la seguridad del tipo.

## Uso con expresiones `when`

```kotlin
fun describir(elemento: Any) {
    when (elemento) {
        is String -> println("Es un String con longitud ${elemento.length}")
        is Int -> println("Es un Int con valor $elemento")
        else -> println("Tipo desconocido")
    }
}
```