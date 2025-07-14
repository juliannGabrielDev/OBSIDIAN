---
aliases:
  - Clases Pair y Triple ✌️🤟
tags:
  - kotlin
  - clases
breadcrumb:
  - "[[indice-android|ANDROID 🤖🔗]]"
  - "[[indice-kotlin|📱 Índice Android]]"
Fecha: 2025-07-04
Docs:
  - https://kotlinlang.org/api/core/kotlin-stdlib/kotlin/-triple/
  - https://kotlinlang.org/api/core/kotlin-stdlib/kotlin/-pair/
---
# Clases `Pair` y `Triple` en Kotlin ✌️🤟

En Kotlin, las clases **`Pair`** y **`Triple`** son clases de datos simples y concisas que se utilizan para agrupar un número fijo de elementos relacionados. Son extremadamente útiles cuando necesitas devolver múltiples valores de una función o agrupar datos de forma temporal sin la necesidad de definir una `data class` completa. 📦✨

---

## Clase `Pair` (Par) ✌️

La clase `Pair` se usa para agrupar **dos** elementos. Es una solución ligera y conveniente cuando solo necesitas manejar un par de valores juntos.

### Declaración y Uso Básico

Puedes crear una instancia de `Pair` usando su constructor o la función `to` (infix function), que a menudo es más legible.

```kotlin
fun obtenerCoordenadas(): Pair<Int, Int> {
    return Pair(10, 20) // Usando el constructor
}

fun obtenerUsuarioYEdad(): Pair<String, Int> {
    return "Alice" to 30 // Usando la función 'to' (más idiomático)
}

fun main() {
    val coordenadas = obtenerCoordenadas()
    println("Coordenada X: ${coordenadas.first}, Coordenada Y: ${coordenadas.second} 📍")

    val (nombre, edad) = obtenerUsuarioYEdad() // Deconstrucción
    println("Usuario: $nombre, Edad: $edad 🧑‍🤝‍🧑")

    // También puedes acceder directamente a los valores
    val parGenerico = "Hola" to 123
    println("Primer elemento: ${parGenerico.first}, Segundo elemento: ${parGenerico.second} 👋")
}
```

### Propiedades

- **`first`**: Accede al primer elemento del par.
    
- **`second`**: Accede al segundo elemento del par.
    

### Deconstrucción

Una de las características más potentes de `Pair` (y `Triple`) es la **deconstrucción**. Esto te permite asignar los elementos del par directamente a variables separadas, lo que hace el código muy conciso y legible.

```kotlin
val (clave, valor) = "producto" to 12.99
println("Clave: $clave, Valor: $valor 🔑💰")
```

---

## Clase `Triple` (Tripleta) 🤟

La clase `Triple` se usa para agrupar **tres** elementos. Funciona de manera muy similar a `Pair`, pero con un elemento adicional.

### Declaración y Uso Básico

```kotlin
fun obtenerEstudianteInfo(): Triple<String, Int, Double> {
    return Triple("Bob", 20, 8.5) // Usando el constructor
}

fun main() {
    val (nombreEstudiante, edadEstudiante, promedio) = obtenerEstudianteInfo() // Deconstrucción
    println("Estudiante: $nombreEstudiante, Edad: $edadEstudiante, Promedio: $promedio 🎓")

    // Acceso directo
    val colorRGB = Triple(255, 0, 0)
    println("Color RGB: R=${colorRGB.first}, G=${colorRGB.second}, B=${colorRGB.third} 🌈")
}
```

### Propiedades

- **`first`**: Accede al primer elemento.
    
- **`second`**: Accede al segundo elemento.
    
- **`third`**: Accede al tercer elemento.
    

### Deconstrucción

Al igual que `Pair`, `Triple` también soporta la deconstrucción:

```kotlin
val (x, y, z) = Triple(1, 2, 3)
println("Coordenadas 3D: X=$x, Y=$y, Z=$z 🌐")
```

---

## ¿Cuándo usar `Pair` y `Triple` vs. `data class`? 🤔

Esta es una pregunta común y la elección depende del contexto:

- **Usa `Pair` o `Triple` cuando:**
    
    - Necesitas agrupar un número **pequeño y fijo** de elementos **temporalmente**.
        
    - Los nombres de los elementos (`first`, `second`, `third`) son lo **suficientemente descriptivos** en el contexto.
        
    - No esperas que la estructura de datos evolucione o gane más propiedades/comportamiento.
        
    - Por ejemplo, devolver múltiples valores de una función o como claves/valores temporales en un `Map`.
        
- **Usa una `data class` cuando:**
    
    - Necesitas agrupar **dos o más** elementos que tienen un **significado semántico claro** y nombres específicos (`userId`, `productName`, `price`).
        
    - La estructura de datos es **más compleja** o es probable que **evolucione** con el tiempo (añadir más propiedades, métodos).
        
    - Quieres beneficiarte de todos los métodos generados automáticamente por las `data class` (`equals`, `hashCode`, `toString`, `copy`, etc.) de forma controlada por el nombre de las propiedades.
        
    - La claridad y la auto-documentación del código son prioritarias.
        

> [!INFO] Claridad sobre Conveniencia 💡
> 
> Si bien Pair y Triple son convenientes, usar una data class con nombres descriptivos para sus propiedades casi siempre es preferible para la claridad del código a largo plazo, especialmente si la estructura de datos se usa en muchos lugares o representa una entidad importante en tu dominio. Pair y Triple son excelentes para casos de uso muy específicos y locales.
