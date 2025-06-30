---
tags:
  - kotlin
  - flujo
---
En Kotlin, el bucle for se utiliza principalmente para iterar sobre rangos, colecciones y arrays.

### Sintaxis básica:

```kotlin
// Iterando sobre un rango
for (i in 1..5) {
    println(i)  // Imprime números del 1 al 5
}

// Iterando sobre una colección
val frutas = listOf("manzana", "banana", "naranja")
for (fruta in frutas) {
    println(fruta)
}

// Iterando con índices
for ((index, value) in array.withIndex()) {
    println("$index: $value")
}
```

### Características especiales:

- until: Excluye el último número (1 until 5 = 1,2,3,4)
- downTo: Cuenta hacia atrás (5 downTo 1 = 5,4,3,2,1)
- step: Define el incremento (1..10 step 2 = 1,3,5,7,9)

```kotlin
// Ejemplos
for (i in 1 until 5) println(i)     // 1,2,3,4
for (i in 5 downTo 1) println(i)    // 5,4,3,2,1
for (i in 0..10 step 2) println(i)  // 0,2,4,6,8,10
```

A diferencia de otros lenguajes, Kotlin no tiene el bucle for tradicional con inicialización, condición e incremento (`for(i=0;i<n;i++)`).