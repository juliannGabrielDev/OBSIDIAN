---
aliases:
  - Nulabilidad
breadcrumb:
  - "[[indice-android|Android]]"
  - "[[indice-kotlin|Kotlin]]"
---
# Nulabilidad en Kotlin 💧

Kotlin, a diferencia de otros lenguajes como Java, aborda la nulabilidad directamente en su sistema de tipos para evitar el famoso `NullPointerException` (NPE). Esto lo logra distinguiendo entre **tipos que pueden contener `null`** y **tipos que no**.

## Tipos no Nulos (Non-nullable)

Por defecto, todas las variables en Kotlin son **no nulas**. Esto significa que no puedes asignarles el valor `null` y debes inicializarlas con un valor real.

```kotlin
var nombre: String = "Ana"
// nombre = null // 💥 Error de compilación
```

Esta es una garantía de seguridad: si puedes declarar una variable sin el operador de nulabilidad, nunca tendrás que preocuparte por un `NullPointerException` al usarla.

---

## Tipos Nulos (Nullable)

Para permitir que una variable pueda contener `null`, debes declararla explícitamente como "nulable" añadiendo un signo de interrogación (`?`) al final del tipo.


```kotlin
var apellido: String? = "García"
apellido = null // ✅ Válido
```

> [!WARNING] Cuidado con los tipos nulos
> 
> ¡Ten cuidado! Cuando una variable es nulable, el compilador de Kotlin te obligará a manejar la posibilidad de que sea null antes de poder usar sus propiedades o métodos.

---

## Manejo Seguro de Nulos

Kotlin proporciona varias formas elegantes y seguras para trabajar con variables nulables.

### 1. Llamada Segura (`?.`)

El operador de llamada segura (`?.`) ejecuta la acción solo si la variable **no es nula**. Si es nula, toda la expresión devuelve `null` y se evita la excepción.

```kotlin
val texto: String? = null
val longitud = texto?.length // longitud será null, sin errores.

println(longitud)
```

### 2. Operador Elvis (`?:`)

El operador Elvis (`?:`) es como un "plan B". Se usa junto con la llamada segura y te permite proporcionar un valor por defecto si la expresión a la izquierda es `null`.


```kotlin
val nombre: String? = null
val nombreAMostrar = nombre ?: "Invitado" // Si nombre es null, usa "Invitado"

println(nombreAMostrar) // Resultado: Invitado
```

### 3. Afirmación de no Nulidad (`!!`)

El operador de doble exclamación (`!!`) es una forma de decirle al compilador: "Confía en mí, sé que esta variable no es nula en este punto". Convierte una variable nulable en su versión no nulable, pero **si la variable resulta ser `null`, lanzará un `NullPointerException`**.

> [!DANGER] Uso de !!
> 
> Peligro: Usa el operador !! con mucha precaución. Generalmente, es una señal de que tu código podría ser mejorado. Prioriza siempre las llamadas seguras y el operador Elvis.

```kotlin
val nombre: String? = "Juan"
val longitud = nombre!!.length // Si nombre fuera null, esto explotaría
```

### 4. Bloque `let` con Llamada Segura

Una forma idiomática y muy segura de trabajar con objetos nulables es usar `.let`. El bloque de código dentro de `let` solo se ejecuta si el objeto no es nulo.

```kotlin
val email: String? = "contacto@ejemplo.com"

email?.let {
    // Dentro de este bloque, 'it' es no nulo
    println("Enviando correo a $it")
}
```