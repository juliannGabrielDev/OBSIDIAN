---
tags:
  - kotlin
  - clases
---
Las **clases selladas** (_sealed classes_) en Kotlin son una característica súper útil para modelar jerarquías de tipos limitadas y conocidas en tiempo de compilación.

### Clases Selladas en Kotlin

Las clases selladas permiten restringir la herencia de una clase a un conjunto específico de subclases. Esto es especialmente útil cuando deseas representar estados o tipos con un número fijo de posibilidades.

#### Declaración de una Clase Sellada

Para definir una clase sellada, utilizas la palabra clave `sealed` antes de `class`:

```kotlin
sealed class Resultado
```

Esta clase `Resultado` no puede ser instanciada directamente y sólo puede tener subclases definidas en el mismo archivo.

#### Subclases de una Clase Sellada

Las subclases pueden ser **clases** o **objetos**:

```kotlin
sealed class Resultado

data class Exito(val datos : String) : Resultado()
data class Error(val mensaje : String) : Resultado()
object Cargando : Resultado()
```

#### Ejemplo Práctico: Operaciones Matemáticas

Imagina que quieres representar operaciones matemáticas:

```kotlin
sealed class Operacion {
    data class Suma(val a: Int, val b: Int) : Operacion()
    data class Resta(val a: Int, val b: Int) : Operacion()
    data class Multiplicacion(val a: Int, val b: Int) : Operacion()
    data class Division(val a: Int, val b: Int) : Operacion()
}
```