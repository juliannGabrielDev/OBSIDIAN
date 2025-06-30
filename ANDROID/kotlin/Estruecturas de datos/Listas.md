---
tags:
  - kotlin
  - estructuras
---
## Creación

```kotlin
val lista = listOf("A", 5, true)
val lista2 = listOf<String>("A", "B", "C")
```

## Añadir elementos

```kotlin
lista2 = lista2 + listOf("D")
```

## Obtener tamaño

```kotlin
println(lista.size)
println(lista.isEmpty())
```

## Comprobar si una lista contiene un elemento

```kotlin
println(lista.contains("A"))
println("A" !in lista)
```

## Listas mutables

```kotlin
val lista3 = mutableListOf("A", "B")

lista3.add("C")
println(lista3)

lista3.remove("B")
println(lista3)
```