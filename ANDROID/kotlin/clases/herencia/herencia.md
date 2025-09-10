---
aliases:
  - Herencia en Kotlin
breadcrumb:
  - "[[indice-android|Android]]"
  - "[[indice-kotlin|Kotlin]]"
---
# Herencia en Kotlin 👪

La herencia es un principio de la programación orientada a objetos que permite a una clase (llamada **clase hija** o subclase) adquirir las propiedades y métodos de otra clase (llamada **clase padre** o superclase). En Kotlin, la herencia ayuda a reutilizar código y a crear jerarquías lógicas.

A diferencia de otros lenguajes, en Kotlin todas las clases son `final` por defecto, lo que significa que **no se puede heredar de ellas** a menos que se marquen explícitamente como `open`.

---

## La Palabra Clave `open`

Para que una clase pueda ser heredada, debes marcarla con la palabra clave `open`. Lo mismo aplica para los métodos y propiedades que desees sobrescribir en la clase hija.

```kotlin
// Clase Padre (Superclase)
open class Vehiculo(val marca: String) {
    open fun acelerar() {
        println("El vehículo está acelerando.")
    }

    fun frenar() {
        println("El vehículo está frenando.")
    }
}
```

- **`open class`**: Permite que otras clases hereden de `Vehiculo`.
    
- **`open fun`**: Permite que el método `acelerar()` sea sobrescrito por las clases hijas.
    
- `frenar()`: Al no ser `open`, este método no puede ser modificado por las clases hijas.
    

---

## Creando la Clase Hija (Subclase)

Para heredar de una clase, se utiliza el operador de dos puntos (`:`) seguido del constructor de la clase padre.

Kotlin

```kotlin
// Clase Hija (Subclase)
class Coche(marca: String, val modelo: String) : Vehiculo(marca) {
    // Aquí Coche hereda 'marca' y los métodos de Vehiculo
}
```

En este ejemplo, la clase `Coche` hereda de `Vehiculo`. El constructor de `Coche` recibe `marca` y se la pasa al constructor de `Vehiculo` (`: Vehiculo(marca)`).

---

## Sobrescribir Métodos (`override`)

Para modificar el comportamiento de un método heredado (que debe ser `open`), se utiliza la palabra clave `override` en la clase hija.

Dentro del método sobrescrito, puedes usar la palabra clave `super` para llamar a la implementación original del método en la clase padre.

```kotlin
class CocheDeportivo(marca: String, modelo: String) : Vehiculo(marca) {

    // Sobrescribiendo el método 'acelerar'
    override fun acelerar() {
        super.acelerar() // Llama al método original de Vehiculo
        println("¡El coche deportivo acelera más rápido! 🏎️💨")
    }
}
```

### Poniéndolo todo junto:

```kotlin
fun main() {
    val miVehiculo = Vehiculo("Genérico")
    miVehiculo.acelerar() // "El vehículo está acelerando."

    println("-----")

    val miDeportivo = CocheDeportivo("Ferrari", "F8")
    miDeportivo.acelerar() // Llama a la versión sobrescrita
    // "El vehículo está acelerando."
    // "¡El coche deportivo acelera más rápido! 🏎️💨"

    miDeportivo.frenar() // "El vehículo está frenando." (método heredado no modificado)
}
```

> [!SUCCESS] Ventajas de la Herencia
> 
> - **Reutilización de Código**: Evitas duplicar propiedades y métodos comunes.
>     
> - **Polimorfismo**: Puedes tratar un objeto de una clase hija como si fuera una instancia de la clase padre.
>     
> - **Organización**: Creas una estructura de clases clara y jerárquica.
>