---
aliases:
  - Interfaces en Kotlin
breadcrumb:
  - "[[indice-android|Android]]"
  - "[[indice-kotlin|Kotlin]]"
Fecha: 2025-07-04
Docs:
  - https://kotlinlang.org/docs/interfaces.html
---
# Interfaces en Kotlin🔌

Las **interfaces** en Kotlin son contratos que definen un conjunto de propiedades y métodos abstractos (sin implementación) o con implementaciones por defecto. 🤝 No pueden ser instanciadas directamente, pero las clases pueden **implementarlas** para comprometerse a proporcionar una implementación para esos miembros.

---

## ¿Por qué usar Interfaces? 🤔💡

- **Abstracción:** 🧠 Permiten definir un comportamiento común que múltiples clases deben seguir, sin preocuparse por los detalles de implementación.
    
- **Polimorfismo:** 🌈 Hacen posible tratar objetos de diferentes clases de manera uniforme si implementan la misma interfaz.
    
- **Modularidad:** 🧱 Ayudan a diseñar sistemas más flexibles y fáciles de mantener al desacoplar la definición del comportamiento de su implementación.
    
- **Herencia Múltiple de Comportamiento:** Aunque Kotlin no soporta la herencia múltiple de clases, las interfaces permiten que una clase implemente múltiples interfaces, heredando así múltiples conjuntos de comportamientos.
    

> [!TIP] ¡Define Contratos! 📝
> 
> Piensa en una interfaz como un "contrato". Cualquier clase que "firme" ese contrato (es decir, implemente la interfaz) debe cumplir con todas las cláusulas especificadas en él.

---

## Declaración Básica de una Interfaz 📜✍️

Para declarar una interfaz, usas la palabra clave `interface`:

```kotlin
interface Clickable {
    fun click() // Método abstracto, sin implementación
}
```

Las clases implementan una interfaz usando el operador `:` y proporcionando una implementación para todos sus miembros abstractos:

```kotlin
class Boton : Clickable {
    override fun click() {
        println("¡Botón clickeado! 🖱️")
    }
}

fun main() {
    val miBoton = Boton()
    miBoton.click() // Salida: ¡Botón clickeado! 🖱️
}
```

---

## Interfaces con Implementaciones por Defecto (Default Methods) 🚀🛠️

A diferencia de Java 7 y versiones anteriores, las interfaces en Kotlin pueden contener implementaciones por defecto para métodos, propiedades abstractas y propiedades con getters/setters.

```kotlin
interface Rectangulo {
    val ladoA: Int
    val ladoB: Int

    // Propiedad con getter por defecto
    val area: Int
        get() = ladoA * ladoB

    // Método con implementación por defecto
    fun describir() {
        println("Este es un rectángulo con lados $ladoA y $ladoB. Área: $area 📐")
    }
}

class Cuadrado(val lado: Int) : Rectangulo {
    override val ladoA: Int = lado
    override val ladoB: Int = lado

    // No es necesario sobrescribir 'area' o 'describir' si la implementación por defecto es suficiente
}

fun main() {
    val miCuadrado = Cuadrado(5)
    miCuadrado.describir() // Salida: Este es un rectángulo con lados 5 y 5. Área: 25 📐
    println("Área directamente: ${miCuadrado.area} 📏") // Salida: Área directamente: 25 📏
}
```

---

## Resolviendo Conflictos de Implementación 🤯🔍

Cuando una clase implementa múltiples interfaces que tienen miembros con la misma firma (nombre y parámetros) y al menos una de ellas tiene una implementación por defecto, Kotlin requiere que la clase resuelva explícitamente el conflicto.


```kotlin
interface A {
    fun foo() { println("A") }
    fun bar()
}

interface B {
    fun foo() { println("B") } // Mismo nombre de método que en A
    fun bar() { println("bar") }
}

class C : A, B {
    override fun bar() {
        super<B>.bar() // Llama a la implementación de 'bar' de la interfaz B
    }

    override fun foo() {
        super<A>.foo() // Llama a la implementación de 'foo' de la interfaz A
        super<B>.foo() // Llama a la implementación de 'foo' de la interfaz B
        println("C")
    }
}

fun main() {
    val c = C()
    c.foo()
    // Salida:
    // A
    // B
    // C
    c.bar() // Salida: bar
}
```

En este caso, la clase `C` usa `super<Interfaz>.metodo()` para especificar cuál implementación de la interfaz desea llamar. Esto es un ejemplo de cómo Kotlin maneja la herencia de implementación.

---

## Propiedades en Interfaces 📦 Properties

Las interfaces pueden declarar propiedades. Estas pueden ser:

- **Abstractas:** Deben ser implementadas por las clases concretas que implementan la interfaz.
    
- **Con `get()`/`set()` definidos:** Pueden tener un `getter` o `setter` implementado por defecto, o ambos. Si tienen un `getter`/`setter` por defecto, la clase que implementa la interfaz no necesita proporcionar una implementación para esa propiedad a menos que quiera sobrescribirla.
    
```kotlin
interface Vehiculo {
    val marca: String // Propiedad abstracta, debe ser implementada
    var velocidad: Int // Propiedad abstracta, debe ser implementada (puede tener setter)

    val esRapido: Boolean
        get() = velocidad > 100 // Getter por defecto
}

class Coche(override val marca: String) : Vehiculo {
    override var velocidad: Int = 0 // Implementación de la propiedad abstracta
}

fun main() {
    val miCoche = Coche("Ford")
    miCoche.velocidad = 120
    println("Marca: ${miCoche.marca}") // Salida: Marca: Ford
    println("Velocidad: ${miCoche.velocidad} km/h") // Salida: Velocidad: 120 km/h
    println("¿Es rápido? ${miCoche.esRapido}") // Salida: ¿Es rápido? true (usa el getter por defecto)
}
```

Las interfaces son un pilar fundamental en el diseño de APIs y arquitecturas de software flexibles en Kotlin.