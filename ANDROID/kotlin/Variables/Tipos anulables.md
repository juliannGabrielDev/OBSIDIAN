---
tags:
  - kotlin
  - variables
---
## Definición

En kotlin, una variable puede marcarse como anulable agregando un `?` al tipo. Eso indica que la variable puede contener un valor nulo.

```kotlin
val nombre : String? = null
```

## Acceso seguro

Al trabajar con variables anulables, el operador seguro `?` te permite acceder a propiedades o métodos sin provocar una excepción en caso de null.

```kotlin
val longitud Int? = nombre?.length
```

## Operador Elvis

Para asignar un valor predeterminado cuando se encuentra un `null`, se utiliza el operador Elvis `?:`. Esto garantiza que siempre tendrás un resultado no nulo:

```kotlin
val longitudForzada : int = nombre?.length ?: 0
```

## Afirmación no nula

Si estás seguro de que una afirmación no es nula, puedes usar el operador `!!` para afirmar su no nulidad. ¡Cuidado! Si la variable resulta ser null, se lanzará una excepción.

```kotlin
val longitudForzada = nombre!!.length
```