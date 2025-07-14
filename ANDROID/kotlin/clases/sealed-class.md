---
aliases:
  - Clases Selladas (sealed class) 🔒🎨
tags:
  - kotlin
  - clases
breadcrumb:
  - "[[indice-android|ANDROID 🤖🔗]]"
  - "[[indice-kotlin|📱 Índice Android]]"
Fecha: 2025-07-04
Docs:
  - https://kotlinlang.org/docs/sealed-classes.html
---
# Clases Selladas (`sealed class`) en Kotlin 🔒🎨

Las **clases selladas (`sealed class`)** en Kotlin son una forma restringida de jerarquía de clases que te permite definir un conjunto finito y predefinido de subclases directas. 🏗️ Esto es especialmente útil para modelar estados, eventos o valores que pueden ser de un número limitado de tipos, conocidos en tiempo de compilación. 🚀

---

## ¿Por qué usar `sealed class`? 🤔💡

El principal beneficio de las `sealed class` radica en el **control exhaustivo y la seguridad de tipo** que ofrecen:

- **Enumeración Limitada de Tipos:** Garantizan que todas las subclases directas de una clase sellada se declaren dentro del mismo archivo (o del mismo módulo si usas Kotlin 1.5+ y las `sealed interface`). Esto significa que el compilador "conoce" todas las subclases posibles. 🧠
    
- **Expresiones `when` Exhaustivas:** ✨ Gracias a este conocimiento, el compilador puede verificar que has manejado _todos_ los casos posibles en una expresión `when` sin necesidad de una cláusula `else`. Esto elimina la posibilidad de errores en tiempo de ejecución debido a casos no manejados.
    
- **Modelado de Estados:** Son ideales para representar estados en sistemas (por ejemplo, `Loading`, `Success`, `Error`) o eventos en arquitecturas como MVI (Model-View-Intent).
    
- **Alternativa a `enum` más Flexible:** Mientras que las `enum class` son para un conjunto fijo de _instancias únicas_, las `sealed class` son para un conjunto fijo de _subtipos_, donde cada subtipo puede tener su propio estado y múltiples instancias.
    

> [!TIP] ¡Seguridad en tu when! ✅
> 
> La característica más potente de las sealed class es la capacidad de hacer que tus expresiones when sean exhaustivas sin else, asegurando que no se te olvide ningún caso.

---

## Declaración Básica de una `sealed class` 📜✍️

Para declarar una clase sellada, usas la palabra clave `sealed class` antes de `class`. Sus subclases pueden ser **`data class`**, **`object`** (si no tienen estado y son singulares) o **clases normales**.

```kotlin
sealed class ResultadoCarga {
    // Subclases que representan diferentes estados del resultado de una carga
    data class Exitoso(val datos: String) : ResultadoCarga() // Contiene datos
    data class Error(val mensaje: String, val codigo: Int) : ResultadoCarga() // Contiene info de error
    object Cargando : ResultadoCarga() // Estado sin datos ni propiedades adicionales
    object Vacio : ResultadoCarga() // Otro estado sin datos
}
```

**Puntos clave:**

- La clase sellada en sí es abstracta y no puede ser instanciada directamente.
    
- Todas las subclases directas deben estar definidas en el **mismo archivo** (o en el mismo módulo para las `sealed interface` a partir de Kotlin 1.5).
    
- Las subclases de una clase sellada pueden ser `data class`, `object`, o clases normales. La elección depende de si la subclase necesita almacenar datos (`data class`), es un singleton (`object`), o es una clase regular con comportamiento.
    

---

## `when` Exhaustivo con `sealed class` 🚦✨

Aquí es donde las `sealed class` realmente brillan. Observa cómo el compilador no requiere una cláusula `else` si cubres todas las subclases:

```kotlin
fun procesarResultado(resultado: ResultadoCarga) {
    when (resultado) {
        is ResultadoCarga.Exitoso -> {
            println("Carga exitosa: ${resultado.datos} 🎉")
        }
        is ResultadoCarga.Error -> {
            println("Error de carga (${resultado.codigo}): ${resultado.mensaje} 🛑")
        }
        ResultadoCarga.Cargando -> { // Nota: no es necesario 'is' para objetos
            println("Datos cargando... ⏳")
        }
        ResultadoCarga.Vacio -> {
            println("No hay datos disponibles. ⚪")
        }
        // No se necesita 'else' porque todos los casos posibles están cubiertos.
    }
}

fun main() {
    procesarResultado(ResultadoCarga.Cargando)
    procesarResultado(ResultadoCarga.Exitoso("Datos de usuario cargados"))
    procesarResultado(ResultadoCarga.Error("Fallo de red", 500))
    procesarResultado(ResultadoCarga.Vacio)
}
```

Si intentaras añadir una nueva subclase a `ResultadoCarga` y olvidaras añadir un `when` para ella, el compilador te avisaría. Esto es increíblemente valioso para refactorizaciones y para mantener la robustez del código. 🛡️

---

## Diferencias entre `enum class`, `sealed class` y Clases Abstractas 🆚🧐

Es común confundir estos tres conceptos, pero tienen propósitos distintos:

|Característica|`enum class`|`sealed class`|Clases Abstractas (`abstract class`)|
|---|---|---|---|
|**Instancias**|Conjunto **fijo y único** de instancias (objetos singleton).|Conjunto **finito** de _subtipos_. Cada subtipo puede tener múltiples instancias.|Conjunto **ilimitado** de subtipos.|
|**Estado**|Las constantes pueden tener propiedades, pero su estado es fijo.|Cada subtipo puede tener su **propio estado distinto**.|Las subclases pueden tener cualquier estado.|
|**Uso Principal**|Constantes fijas conocidas (días de la semana, colores).|Modelado de **estados limitados**, resultados, eventos.|Herencia polimórfica general, define una base común.|
|**`when` Exhaustivo**|Sí (todas las constantes son conocidas).|Sí (todas las subclases directas son conocidas).|No (se necesita `else` o `is` para cada tipo conocido).|
|**Ubicación Subclases**|N/A (son las constantes)|Mismo archivo/módulo (directas).|Cualquier archivo/módulo.|

> [!INFO] ¡Elige la Herramienta Correcta! 🛠️
> 
> Usa enum class para un conjunto fijo de valores que son esencialmente constantes. Usa sealed class para un conjunto fijo de tipos que pueden tener su propio estado. Usa abstract class para jerarquías de herencia más generales donde no hay una limitación en el número de subtipos.

---

## `sealed interface` (Kotlin 1.5+) 🆕✨

A partir de Kotlin 1.5, también puedes declarar **interfaces selladas (`sealed interface`)**. Funcionan de manera similar a las `sealed class` pero con la flexibilidad de las interfaces (una clase puede implementar múltiples interfaces selladas, por ejemplo). Además, las implementaciones de una `sealed interface` pueden estar en **cualquier archivo dentro del mismo módulo** (no solo el mismo archivo), lo que las hace aún más flexibles para organizar código.

```kotlin
sealed interface Forma {
    data class Circulo(val radio: Double) : Forma
    data class Cuadrado(val lado: Double) : Forma
    object SinForma : Forma // Puedes tener objetos también
}
```

Las `sealed class` y `sealed interface` son herramientas poderosas para crear código más seguro, legible y mantenible en Kotlin, especialmente cuando se trabaja con estados y datos que pertenecen a un conjunto bien definido de posibilidades.