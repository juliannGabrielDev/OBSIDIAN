---
aliases:
  - Clases data 📝✨
tags:
  - kotlin
  - clases
breadcrumb:
  - "[[indice-android|ANDROID 🤖🔗]]"
  - "[[indice-kotlin|📱 Índice Android]]"
Fecha: 2025-07-04
Docs:
  - https://kotlinlang.org/docs/data-classes.html
---
# Clases `data` en Kotlin 📝✨

Las **clases `data`** en Kotlin son una herramienta poderosa y concisa diseñada para simplificar la creación de clases cuyo propósito principal es **contener datos**. 📦 Su característica más destacada es que el compilador de Kotlin genera automáticamente una serie de métodos útiles, lo que reduce drásticamente el código repetitivo que tendrías que escribir manualmente en Java. 🚀

---

## ¿Por qué usar `data class`? 🤔💡

El principal beneficio de las `data class` es la **generación automática de métodos clave**:

- **`equals()`**: Compara dos objetos por el valor de sus propiedades, no por su referencia en memoria. ¡Esto es crucial para comparar si dos objetos son "iguales" en contenido! 🎯
    
- **`hashCode()`**: Genera un código hash consistente basado en los valores de las propiedades. Es esencial cuando usas objetos en colecciones como `HashSet` o `HashMap`. 📚
    
- **`toString()`**: Proporciona una representación de cadena legible del objeto, mostrando el nombre de la clase y los valores de todas sus propiedades. Ideal para depuración. 🗣️
    
- **`componentN()`**: Permite la deconstrucción de objetos en variables individuales. Muy útil para asignaciones múltiples.
    
- **`copy()`**: Crea una copia de un objeto, permitiendo modificar algunas de sus propiedades mientras se mantienen las demás. ¡Perfecto para trabajar con inmutabilidad! 🔄
    

> [!TIP] ¡Adiós al Boilerplate! 🧹
> 
> Antes de las data class, tendrías que escribir manualmente equals(), hashCode(), y toString() para cada clase de datos. Las data class hacen todo esto por ti, lo que te permite enfocarte en la lógica de tu aplicación.

---

## Declaración Básica de una `data class` 📜✨

Para declarar una `data class`, simplemente añades la palabra clave `data` antes de `class`. Todas las propiedades declaradas en el constructor primario se usan para la generación de los métodos automáticos.

```kotlin
data class Usuario(val id: Int, var nombre: String, val email: String?)
```

**Puntos clave:**

- Todas las propiedades en el constructor primario de una `data class` deben ser `val` (inmutable) o `var` (mutable). Es una buena práctica preferir `val` para promover la inmutabilidad. 🔒
    
- Una `data class` requiere al menos una propiedad en su constructor primario.
    
- No pueden ser `abstract`, `open`, `sealed` o `inner`.
    
- Pueden implementar interfaces y extender otras clases (aunque esto último es menos común).
    

---

## Métodos Generados Automáticamente en Acción 🚀

Veamos cómo se comportan los métodos generados:

```kotlin
fun main() {
    val usuario1 = Usuario(1, "Ana", "ana@example.com")
    val usuario2 = Usuario(1, "Ana", "ana@example.com")
    val usuario3 = Usuario(2, "Pedro", "pedro@example.com")

    // 1. equals()
    println("usuario1 == usuario2: ${usuario1 == usuario2}") // Salida: true (comparación por valor)
    println("usuario1 == usuario3: ${usuario1 == usuario3}") // Salida: false

    // 2. hashCode()
    println("HashCode usuario1: ${usuario1.hashCode()}")
    println("HashCode usuario2: ${usuario2.hashCode()}")
    println("HashCode usuario3: ${usuario3.hashCode()}")
    // Si usuario1 y usuario2 son iguales, sus hash codes también serán iguales.

    // 3. toString()
    println("usuario1.toString(): ${usuario1.toString()}") // Salida: Usuario(id=1, nombre=Ana, email=ana@example.com)

    // 4. componentN() y Deconstrucción
    val (id, nombre, email) = usuario1
    println("Deconstrucción: ID=$id, Nombre=$nombre, Email=$email")

    // 5. copy()
    val usuario1Copia = usuario1.copy() // Copia exacta
    val usuario1Modificado = usuario1.copy(nombre = "Ana María") // Copia con nombre modificado

    println("usuario1Copia: $usuario1Copia") // Salida: Usuario(id=1, nombre=Ana, email=ana@example.com)
    println("usuario1Modificado: $usuario1Modificado") // Salida: Usuario(id=1, nombre=Ana María, email=ana@example.com)
    println("usuario1 == usuario1Copia: ${usuario1 == usuario1Copia}") // Salida: true
    println("usuario1 === usuario1Copia: ${usuario1 === usuario1Copia}") // Salida: false (diferentes referencias en memoria)
}
```

---

## Inmutabilidad y `data class` 🔒

Es una práctica muy recomendada usar `val` para todas las propiedades en una `data class` para hacerla inmutable. Esto tiene beneficios significativos:

- **Previsibilidad:** El estado de un objeto no cambia después de su creación.
    
- **Seguridad en Concurrencia:** No hay problemas de sincronización en entornos multi-hilo.
    
- **Facilidad de Prueba:** Los objetos inmutables son más fáciles de probar.
    

Cuando necesitas un objeto con propiedades modificadas, usas el método `copy()` en lugar de mutar el objeto original.

---

## Propiedades Declaradas Fuera del Constructor Primario ⚠️

Las propiedades declaradas fuera del constructor primario de una `data class` **no** son incluidas en la generación automática de `equals()`, `hashCode()`, `toString()`, etc.

```kotlin
data class Producto(val id: Int, val nombre: String) {
    var stock: Int = 0 // Esta propiedad no es considerada por equals(), hashCode(), etc.
}

fun main() {
    val producto1 = Producto(1, "Laptop").apply { stock = 10 }
    val producto2 = Producto(1, "Laptop").apply { stock = 20 }

    println("producto1 == producto2: ${producto1 == producto2}") // Salida: true (¡stock no influye!)
    println("producto1.toString(): ${producto1.toString()}") // Salida: Producto(id=1, nombre=Laptop) (¡stock no se muestra!)
}
```

> [!WARNING] ¡Ten Cuidado! 🚨
> 
> Si una propiedad es parte de la identidad de tus datos y esperas que afecte a equals() o hashCode(), asegúrate de que esté en el constructor primario.

---

## Herencia con `data class` 🌳

Las `data class` pueden extender otras clases o implementar interfaces, pero con algunas consideraciones:

- Kotlin recomienda el uso de **clases selladas (`sealed class`)** con `data class` anidadas para modelar jerarquías de datos con un conjunto finito de subtipos.

```kotlin
sealed class ResultadoCarga {
    data class Exitoso(val datos: String) : ResultadoCarga()
    data class Error(val codigo: Int, val mensaje: String) : ResultadoCarga()
    object Cargando : ResultadoCarga() // Un 'object' si no tiene estado
}

fun procesarResultado(resultado: ResultadoCarga) {
    when (resultado) {
        is ResultadoCarga.Exitoso -> println("Carga exitosa: ${resultado.datos}")
        is ResultadoCarga.Error -> println("Error ${resultado.codigo}: ${resultado.mensaje}")
        ResultadoCarga.Cargando -> println("Cargando datos...")
    }
}
```

Las `data class` son una característica fundamental de Kotlin para la programación orientada a datos, facilitando la creación de código limpio, conciso y robusto.