---
aliases:
  - Property Delegates
breadcrumb:
  - "[[indice-android|Android]]"
  - "[[indice-kotlin|Kotlin]]"
---
# Property Delegates en Kotlin delegates) delegates)

Un **property delegate** (delegado de propiedad) es un patrón de diseño en Kotlin que te permite delegar la lógica de los `getter` y `setter` de una propiedad a un objeto auxiliar. En lugar de implementar la lógica directamente en la propiedad, la "delegas" a otra clase.

Esto es útil para reutilizar lógicas comunes como:

- Inicialización perezosa (lazy initialization).
    
- Observar cambios en el valor de una propiedad.
    
- Validar nuevos valores antes de asignarlos.
    
- Almacenar propiedades en un mapa (`map`) o en una base de datos.
    

La sintaxis utiliza la palabra clave `by`.

```kotlin
class MiClase {
    // La lógica de 'miPropiedad' es manejada por 'Delegado()'
    var miPropiedad: String by Delegado()
}
```

---

## Delegados Estándar de Kotlin

Kotlin viene con varios delegados útiles ya implementados en su librería estándar.

### 1. `lazy` 😴

El delegado `lazy` se usa para la **inicialización perezosa**. El valor de la propiedad no se calcula hasta la primera vez que se accede a ella. Después del primer acceso, el valor se guarda en memoria (se cachea) y se devuelve en accesos posteriores sin volver a calcularlo.

Es ideal para propiedades cuyo cálculo es costoso y no siempre se necesitan.

```kotlin
class SesionUsuario {
    val datosUsuario: String by lazy {
        println("Calculando datos del usuario...") // Esto solo se imprime una vez
        "Información confidencial cargada desde la red"
    }
}

fun main() {
    val sesion = SesionUsuario()
    println("Sesión creada, pero los datos aún no se han cargado.")
    println(sesion.datosUsuario) // Se calculan los datos aquí
    println(sesion.datosUsuario) // Se devuelve el valor cacheado
}
```

### 2. `Delegates.observable()` 🧐

Este delegado te permite **observar cambios** en el valor de una propiedad. Recibe un lambda que se ejecuta _después_ de que el valor ha sido asignado. El lambda recibe el valor antiguo y el nuevo.

```kotlin
import kotlin.properties.Delegates

class Configuracion {
    var colorFondo: String by Delegates.observable("<sin definir>") { prop, antiguo, nuevo ->
        println("El color de fondo cambió de '$antiguo' a '$nuevo'. ✨")
    }
}

fun main() {
    val config = Configuracion()
    config.colorFondo = "Azul"   // Imprime el mensaje
    config.colorFondo = "Verde"  // Imprime el mensaje de nuevo
}
```

### 3. `Delegates.vetoable()` 🚫

Similar a `observable`, pero este delegado permite **vetar** o rechazar un cambio. El lambda se ejecuta _antes_ de que el nuevo valor sea asignado y debe devolver un booleano:

- `true`: El cambio se acepta.
    
- `false`: El cambio se rechaza y el valor de la propiedad no se modifica.

```kotlin
import kotlin.properties.Delegates

class Empleado {
    var salario: Int by Delegates.vetoable(1000) { prop, antiguo, nuevo ->
        if (nuevo > antiguo) {
            println("Aumento de salario aceptado: de $antiguo a $nuevo.")
            true // Aceptar el cambio
        } else {
            println("Rechazado: El nuevo salario ($nuevo) no puede ser menor o igual al actual ($antiguo).")
            false // Rechazar el cambio
        }
    }
}

fun main() {
    val emp = Empleado()
    emp.salario = 1200 // Se acepta
    emp.salario = 1100 // Se rechaza
    println("Salario final: ${emp.salario}") // Imprime 1200
}
```

> [!TIP] Creando tus Propios Delegados
> 
> Pro Tip: Puedes crear tus propios delegados implementando las interfaces ReadOnlyProperty o ReadWriteProperty. Esto te obliga a implementar los métodos getValue() y setValue(), dándote control total sobre la lógica de la propiedad. ¡Es una herramienta muy poderosa para escribir código limpio y reutilizable!