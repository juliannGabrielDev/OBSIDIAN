---
aliases:
  - Clases Enum
breadcrumb:
  - "[[indice-android|Android]]"
  - "[[indice-kotlin|Kotlin]]"
Fecha: 2025-07-04
Docs:
  - https://kotlinlang.org/docs/enum-classes.html
---
# Clases Enum en Kotlin 🏷️

Las **clases `enum`** en Kotlin son una forma especial de definir un conjunto fijo de constantes. Son muy útiles cuando sabes de antemano todos los valores posibles de una variable, como los días de la semana, los colores o los estados de un proceso. 

---

## ¿Por qué usar Enums?

- **Seguridad de Tipo:** 🔒 Evitan errores al restringir los valores posibles.
    
- **Legibilidad:** 📖 Hacen el código más fácil de entender.
    
- **Mantenimiento:** 🛠️ Simplifican la adición o eliminación de valores.
    

> [!TIP] ¡Simplifica tu Código! 💡
> 
> Las enums son ideales para reemplazar conjuntos de constantes String o Int que pueden llevar a errores tipográficos.

---

## Declaración Básica de una Enum

Para declarar una `enum` en Kotlin, usas la palabra clave `enum class` seguida del nombre de la enum y sus constantes separadas por comas.

```kotlin
enum class Direccion {
    NORTE,
    SUR,
    ESTE,
    OESTE
}
```

---

## Propiedades y Comportamiento Adicional

Las enums en Kotlin son mucho más que simples listas de nombres. Puedes:

- **Declarar propiedades:** Cada constante enum puede tener sus propias propiedades.
    
- **Definir métodos:** Puedes agregar funciones a tus enums.
    
- **Implementar interfaces:** Una enum puede implementar interfaces.
    

Veamos un ejemplo:

```kotlin
enum class EstadoPedido(val descripcion: String, val codigo: Int) {
    PENDIENTE("El pedido está en espera", 100),
    PROCESANDO("El pedido se está preparando", 200),
    ENVIADO("El pedido ya fue despachado", 300),
    ENTREGADO("El pedido ha sido entregado", 400); // ¡Punto y coma aquí!

    fun obtenerMensaje(): String {
        return "Estado: $descripcion (Código: $codigo)"
    }
}
```

> [!NOTE] ¡Importante el Punto y Coma! ❗
> 
> Si la última constante de tu enum tiene miembros (propiedades o funciones), debes terminarla con un punto y coma ; antes de definir los miembros.

### Accediendo a los Valores y Métodos 🎯

|Propiedad/Método|Descripción|
|---|---|
|`name`|Devuelve el nombre de la constante como un `String`.|
|`ordinal`|Devuelve la posición de la constante (0-indexado).|
|`values()`|Devuelve un `Array` de todas las constantes enum.|
|`valueOf(name)`|Devuelve la constante enum con el nombre especificado.|

```kotlin
fun main() {
    val estadoActual = EstadoPedido.PROCESANDO
    println("Nombre: ${estadoActual.name} 🏷️") // Salida: Nombre: PROCESANDO
    println("Ordinal: ${estadoActual.ordinal} #️⃣") // Salida: Ordinal: 1
    println("Descripción: ${estadoActual.descripcion} 📝") // Salida: Descripción: El pedido se está preparando
    println("Mensaje: ${estadoActual.obtenerMensaje()} 🗣️") // Salida: Mensaje: Estado: El pedido se está preparando (Código: 200)

    for (estado in EstadoPedido.values()) {
        println(estado.obtenerMensaje())
    }

    val estadoPorNombre = EstadoPedido.valueOf("ENVIADO")
    println("Estado por nombre: ${estadoPorNombre.descripcion} ✅")
}
```

---

## Cuándo usar Enums vs. Sealed Classes 

A menudo, las **clases `enum`** y las **`sealed class`** (clases selladas) pueden parecer similares, ya que ambas restringen los tipos posibles. Sin embargo, tienen diferencias clave:

- **`enum class`**: Para un conjunto **finito y fijo** de instancias donde cada instancia es un objeto único. No puedes crear nuevas instancias en tiempo de ejecución.
    
- **`sealed class`**: Para un conjunto **finito de subclases**, donde cada subclase puede tener múltiples instancias. Son más flexibles y permiten que cada subclase tenga su propio estado.
    

> [!INFO] ¡Recuerda! ℹ️
> 
> Si necesitas crear múltiples objetos del mismo "tipo" o los tipos tienen estados variables, las sealed class son probablemente una mejor opción. Si solo necesitas un conjunto de constantes, las enum son perfectas.

---

## 🛑 ¡Cuidado con `valueOf()`!

> [!WARNING] ¡Maneja Excepciones! ⚠️
> 
> La función valueOf() lanzará un IllegalArgumentException si el nombre pasado no coincide con ninguna constante enum. Siempre que uses valueOf(), considera envolverla en un bloque try-catch o validar la entrada.
