---
tags:
  - kotlin
  - estructuras
---
Un mapa representa una colección desordenada de pares clave-valor. Las claves son únicas y cada una de ellas mapea exactamente a un valor.

## Creación de mapas

```kotlin
val mapInmutable = mapOf(
    "clave1" to "valor1",
    "clave2" to "valor2",
    "clave3" to "valor3"
)

val mapMutable = mutableMapOf<String, String>(
    "claveA" to "valorA",
    "claveB" to "valorB"
)
```

## Métodos

- `put` : mutables
- `remove` : mutables
- `size`