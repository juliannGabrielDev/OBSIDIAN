---
aliases:
  - If y When en Kotlin (Sentencias y Expresiones)
breadcrumb:
  - "[[indice-android|Android]]"
  - "[[indice-kotlin|Kotlin]]"
Fecha: 2025-07-09
---
# If y When en Kotlin (Sentencias y Expresiones) 🕵️

En Kotlin, tanto `if` como `when` pueden ser utilizados no solo como sentencias (que ejecutan código) sino también como expresiones (que devuelven un valor). Esta dualidad permite escribir código más conciso y funcional.

---

## `if` - Condicional Básico

### `if` como Sentencia

Es el uso tradicional, donde se ejecuta un bloque de código si una condición es verdadera.
```kotlin
val calificacion = 6
if (calificacion >= 7) {
    println("Aprobado 👍")
} else {
    println("Reprobado 👎")
}
```

### `if` como Expresión

Cuando se usa como expresión, la última línea del bloque `if` o `else` se convierte en el valor de retorno, que puede ser asignado a una variable. El bloque `else` es **obligatorio**.
```kotlin
val calificacion = 8
val mensaje = if (calificacion >= 7) {
    "¡Felicidades, aprobaste! 🎉"
} else {
    "Sigue intentando, reprobaste."
}
println(mensaje) // "¡Felicidades, aprobaste! 🎉"
```

El `return` es implícito en la última línea del bloque.

---

## `when` - Múltiples Ramas

`when` es el reemplazo mejorado del `switch` de otros lenguajes. Es más potente y flexible.

### `when` como Sentencia

Ejecuta un bloque de código que coincide con un valor o condición.
```kotlin
val dia = 3
when (dia) {
    1 -> println("Lunes")
    2 -> println("Martes")
    3 -> println("Miércoles")
    else -> println("Día no válido")
}
```

> [!INFO] Flexibilidad de when
> 
> ℹ️ Las condiciones en un when pueden ser más que solo constantes. Puedes usar rangos (in 1..5), comprobaciones de tipo (is String), o cualquier expresión booleana.

```kotlin
val valor: Any = 10
when (valor) {
    is String -> println("Es un texto")
    in 1..10 -> println("Está en el rango del 1 al 10")
    else -> println("Es otra cosa")
}
```

### `when` como Expresión

Al igual que `if`, `when` puede devolver un valor. La rama `else` es generalmente **obligatoria** para asegurar que la expresión siempre retorne un valor.
```kotlin
val httpStatus = 200
val mensajeHttp = when (httpStatus) {
    200, 201 -> "Éxito ✅"
    404 -> "No encontrado 🤷"
    500 -> "Error del servidor 🔥"
    else -> "Código desconocido"
}
println("Respuesta del servidor: $mensajeHttp") // "Respuesta del servidor: Éxito ✅"
```

> [!TIP] Código Limpio
> 
> Pro Tip: Usar if y when como expresiones te ayuda a evitar variables mutables (var) y a escribir código más declarativo y fácil de leer, lo cual es una práctica recomendada en Kotlin.