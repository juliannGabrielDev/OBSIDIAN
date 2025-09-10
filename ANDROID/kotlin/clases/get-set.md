---
aliases:
  - Getters y Setters Personalizados
breadcrumb:
  - "[[indice-android|Android]]"
  - "[[indice-kotlin|Kotlin]]"
---
# Getters y Setters Personalizados en Kotlin 🚗

En Kotlin, cada propiedad (`val` o `var`) declarada en una clase viene con un **getter** (y un **setter** para `var`) generado automáticamente. Sin embargo, puedes personalizarlos para añadir lógica específica cada vez que una propiedad es leída o modificada.

Esto se hace a través de los descriptores de acceso `get()` y `set()`.

---

## Propiedades y el `field` de Respaldo

Cuando personalizas un `getter` or `setter`, necesitas una forma de hacer referencia al valor real de la propiedad. Si usaras el nombre de la propiedad directamente dentro de su propio `getter` o `setter`, causarías una recursión infinita.

Para evitar esto, Kotlin proporciona un identificador especial llamado `field` (campo de respaldo), que almacena el valor de la propiedad.

> [!INFO] ¿Qué es el field?
> 
> ℹ️ El field es un identificador especial que solo puedes usar dentro de los descriptores de acceso get y set. Representa el almacenamiento subyacente de la propiedad.

---

## Getters Personalizados (`get`)

Un `getter` personalizado te permite ejecutar código cada vez que se lee el valor de una propiedad. Es útil para calcular valores derivados, formatear datos o implementar validaciones de solo lectura.

**Caso de uso**: Una propiedad que siempre devuelve un valor en mayúsculas.

```kotlin
class Usuario(nombre: String) {
    val nombreEnMayusculas: String
        get() = nombre.uppercase() // Getter personalizado, no necesita 'field'
}

val user = Usuario("Ana")
println(user.nombreEnMayusculas) // Llama al getter -> Imprime "ANA"
```

**Caso de uso**: Exponer una propiedad calculada.

```kotlin
class Rectangulo(val ancho: Int, val alto: Int) {
    val area: Int
        get() = this.ancho * this.alto // El área se calcula al ser leída
}

val r = Rectangulo(10, 5)
println(r.area) // Llama al getter -> Imprime 50
```

---

## Setters Personalizados (`set`)

Un `setter` personalizado se ejecuta cada vez que se intenta asignar un nuevo valor a una propiedad `var`. Dentro del `setter`, la palabra clave `value` contiene el valor que se está asignando.

**Caso de uso**: Validar un valor antes de asignarlo.

```kotlin
class Persona {
    var edad: Int = 0
        set(valor) { // 'valor' es el nombre por defecto, puedes cambiarlo
            if (valor >= 0) {
                field = valor // Asigna al campo de respaldo solo si es válido
            } else {
                println("La edad no puede ser negativa. 😠")
            }
        }
}

val p = Persona()
p.edad = 25 // Llama al setter -> field = 25
println(p.edad) // 25

p.edad = -5 // Llama al setter -> Imprime "La edad no puede ser negativa."
println(p.edad) // Sigue siendo 25
```

> [!TIP] Propiedades de solo escritura
> 
> Pro Tip: Si defines un setter como private, la propiedad solo podrá ser modificada desde dentro de la misma clase, creando una especie de propiedad de "solo escritura" desde el exterior.

```kotlin
class Contador {
    var valor: Int = 0
        private set // El setter solo es visible dentro de la clase Contador

    fun incrementar() {
        valor++ // Válido, estamos dentro de la clase
    }
}

val c = Contador()
c.incrementar()
// c.valor = 5 // 💥 Error de compilación, el setter es privado
```