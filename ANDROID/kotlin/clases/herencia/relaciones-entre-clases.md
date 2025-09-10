---
aliases:
  - "Relaciones Entre Clases: IS-A vs. HAS-A"
breadcrumb:
  - "[[indice-android|Android]]"
  - "[[indice-kotlin|Kotlin]]"
---
# Relaciones Entre Clases: IS-A vs. HAS-A 🤝

En la Programación Orientada a Objetos (POO), las clases no existen de forma aislada. Se relacionan entre sí para modelar sistemas complejos. Dos de las relaciones más fundamentales son **IS-A** (Es un) y **HAS-A** (Tiene un).

---

## IS-A (Es un) 👉 Herencia

La relación **IS-A** se implementa mediante la **herencia**. Ocurre cuando una clase (subclase) es un tipo específico de otra clase (superclase). La subclase hereda todas las propiedades y métodos públicos de la superclase.

- **Palabra clave**: "Es un..."
    
- **Ejemplo**: Un `Coche` **es un** `Vehiculo`. Un `Perro` **es un** `Animal`.
    
- **Implementación en Kotlin**: Se usa la palabra clave `open` en la clase padre y el operador `:` en la clase hija.
    

### Ejemplo en Código

```kotlin
// La clase padre debe ser 'open' para permitir la herencia
open class Empleado(val nombre: String) {
    fun trabajar() {
        println("$nombre está trabajando.")
    }
}

// Gerente "ES UN" Empleado
class Gerente(nombre: String, val departamento: String) : Empleado(nombre) {
    fun planificar() {
        println("$nombre está planificando para el departamento de $departamento.")
    }
}
```

Un `Gerente` tiene todo lo que un `Empleado` tiene (el nombre y el método `trabajar()`) y además añade sus propias características.

---

## HAS-A (Tiene un) 👉 Composición o Agregación

La relación **HAS-A** se implementa mediante la **composición** o **agregación**. Ocurre cuando una clase contiene una instancia (un objeto) de otra clase como una de sus propiedades.

- **Palabra clave**: "Tiene un..."
    
- **Ejemplo**: Un `Coche` **tiene un** `Motor`. Una `Universidad` **tiene** `Estudiantes`.
    
- **Implementación en Kotlin**: Se declara una propiedad en una clase cuyo tipo es otra clase.
    

### Ejemplo en Código

```kotlin
class Motor(val tipo: String) {
    fun encender() {
        println("Motor de tipo $tipo encendido. 🔥")
    }
}

// Coche "TIENE UN" Motor
class Coche(val marca: String, val modelo: String) {
    // La instancia de Motor es parte del Coche
    private val motor: Motor = Motor("V8")

    fun arrancar() {
        println("Arrancando el $marca $modelo...")
        motor.encender()
    }
}

val miCoche = Coche("Ford", "Mustang")
miCoche.arrancar()
```

Aquí, la clase `Coche` no hereda de `Motor`, sino que contiene un objeto `Motor` y utiliza su funcionalidad.

---

## Resumen Comparativo

|Característica|Relación IS-A (Herencia)|Relación HAS-A (Composición)|
|---|---|---|
|**Concepto**|Una clase es un tipo de otra.|Una clase contiene a otra.|
|**Implementación**|`: Superclase()`|`val obj: OtraClase()`|
|**Acoplamiento**|Alto (fuerte)|Bajo (débil)|
|**Flexibilidad**|Menos flexible|Más flexible y preferida|
|**Cuándo usarla**|Cuando una clase es una especialización clara de otra.|Cuando una clase necesita la funcionalidad de otra.|

> [!TIP] Prefiere la Composición sobre la Herencia
> 
> Pro Tip: Como regla general en el diseño de software, se recomienda favorecer la composición (HAS-A) sobre la herencia (IS-A). La composición conduce a un diseño más flexible, con componentes que son más fáciles de reutilizar y probar de forma independiente.