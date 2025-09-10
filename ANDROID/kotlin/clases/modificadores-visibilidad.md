---
aliases:
  - Modificadores de Visibilidad
breadcrumb:
  - "[[indice-android|Android]]"
  - "[[indice-kotlin|Kotlin]]"
---
# Modificadores de Visibilidad en Kotlin 👁️

Los modificadores de visibilidad en Kotlin determinan desde dónde se puede acceder a las clases, objetos, propiedades, funciones y constructores. Ayudan a encapsular el código, ocultando los detalles de implementación y exponiendo solo lo necesario.

Por defecto, todas las declaraciones en Kotlin son `public`.

---

## `public`

Es el modificador por defecto. Si una declaración es `public`, es visible en **todas partes**: dentro de la misma clase, en clases derivadas, en el mismo módulo y en otros módulos.

```kotlin
// public es redundante aquí, ya que es el valor por defecto
public class MiClase {
    public val propiedadVisible = "Soy visible para todos! 👋"
}
```

---

## `private`

Este es el modificador más restrictivo. Una declaración `private` solo es visible **dentro del mismo archivo o de la misma clase** en la que fue declarada.

```kotlin
class CajaFuerte {
    private val secreto = "1234" // Solo accesible dentro de la clase CajaFuerte

    fun mostrarSecreto() {
        // Válido, estamos dentro de la misma clase
        println("El secreto es $secreto")
    }
}

fun main() {
    val caja = CajaFuerte()
    // println(caja.secreto) // 💥 Error de compilación: 'secreto' es privado
}
```

> [!INFO] private a nivel de archivo
> 
> ℹ️ Si declaras una función o clase como private fuera de otra clase (en el nivel superior de un archivo), solo será visible dentro de ese mismo archivo .kt.

---

## `protected`

Una declaración `protected` es visible dentro de su **clase y en sus subclases** (clases que heredan de ella). No es visible a nivel de paquete como en Java.

```kotlin
open class Padre {
    protected val apellido = "García"
}

class Hijo : Padre() {
    fun presentar() {
        // Válido, 'Hijo' hereda de 'Padre'
        println("Mi apellido es $apellido")
    }
}

fun main() {
    val h = Hijo()
    // println(h.apellido) // 💥 Error de compilación: 'apellido' es protected
}
```

---

## `internal`

Este es un modificador específico de Kotlin. Una declaración `internal` es visible **en cualquier lugar dentro del mismo módulo**.

Un **módulo** es un conjunto de archivos de Kotlin que se compilan juntos (por ejemplo, un proyecto de IntelliJ, un módulo de Maven o Gradle). Es muy útil para ocultar la implementación interna de una librería al resto del mundo, pero permitiendo que las clases de la propia librería interactúen entre sí libremente.

```kotlin
// --- Módulo A ---
internal class ApiInterna {
    internal fun conectar() {
        println("Conectando... ⚙️")
    }
}

class ClienteApi {
    fun usarApi() {
        val api = ApiInterna() // Válido, estamos en el mismo módulo
        api.conectar()
    }
}

// --- Módulo B (que depende del Módulo A) ---
// val api = ApiInterna() // 💥 Error de compilación: ApiInterna no es visible aquí
```

### Tabla Resumen

|Modificador|Clase|Subclase|Mismo Módulo|Otro Módulo|
|---|---|---|---|---|
|**`public`**|✅|✅|✅|✅|
|**`internal`**|✅|✅|✅|❌|
|**`protected`**|✅|✅|❌|❌|
|**`private`**|✅|❌|❌|❌|