---
tags:
  - kotlin
  - clases
---
La clases **Pair** y **Triple** en Kotlin son estructuras de datos simples que permiten almacenar conjuntos de datos de dos y tre valores, respectivamente. Son especialmente útiles cuando necesitas manejar grupos pequeños de datos sin crear una clase personalizada.

## Uso de la clase Pair

La clase `Pair<A, B>` almacena dos valores que pueden ser de tipos diferentes.

```kotlin
val coordenadas = Pair(10, 20)
println("X: ${coordenadas.first}, Y: ${coordenadas.second}")
```

## Uso de la clase Triple

La clase `Triple<A, B, C>` extiende este concepto a tres valores.

```kotlin
val persona = Triple("Ana", 28, "Ingeniera")
println("Nombre: ${persona.first}, Edad: ${persona.second}, Profesión: ${persona.third}")
```

## Destructuración de Pairs y Triples

```kotlin
val (x, y) = coordenadas
println("X: $x, Y: $y")

val (nombre, edad, profesion) = persona
println("$nombre tiene $edad años y es $profesion")
```

## Cuando usar Pair y Triple:

- **Datos Temporales:** Son ideales para datos transitorios o retornos rápidos de funciones.
    
- **Prototipado Rápido:** Útiles en etapas iniciales del desarrollo.