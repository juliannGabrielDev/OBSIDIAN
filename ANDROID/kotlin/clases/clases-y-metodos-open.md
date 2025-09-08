---
aliases:
  - Clases y Métodos "Abiertos" (open)
breadcrumb:
  - "[[indice-android|Android]]"
  - "[[indice-kotlin|Kotlin]]"
Fecha: 2025-07-04
---
# Clases y Métodos "Abiertos" (`open`) en Kotlin 🔓

En Kotlin, por defecto, todas las clases y métodos son **`final`**. Esto significa que no pueden ser extendidos (en el caso de clases) ni sobrescritos (en el caso de métodos) por otras clases o subclases. Esta es una decisión de diseño de Kotlin para promover la **composición sobre la herencia** y fomentar la creación de código más robusto y menos propenso a errores (el "problema del diamante" o el problema de la "clase base frágil"). inherited

Para permitir que una clase sea extendida o un método sea sobrescrito, debes marcarlos explícitamente con la palabra clave **`open`**.

---

## Clases `open`

Cuando declaras una clase con `open`, estás permitiendo que otras clases hereden de ella.

```kotlin
open class Figura(val nombre: String) {
    open fun describir() {
        println("Esta es una figura llamada $nombre. 🎨")
    }
}

class Circulo(nombre: String, val radio: Double) : Figura(nombre) {
    override fun describir() {
        super.describir() // Llama a la implementación de la clase base
        println("Es un círculo con radio $radio. ⭕")
    }

    fun calcularArea(): Double {
        return Math.PI * radio * radio
    }
}

fun main() {
    val miCirculo = Circulo("Círculo Rojo", 5.0)
    miCirculo.describir()
    // Salida:
    // Esta es una figura llamada Círculo Rojo. 🎨
    // Es un círculo con radio 5.0. ⭕
    println("Área del círculo: ${miCirculo.calcularArea()} cm²")
}
```

> [!NOTE] ¡Recordatorio Importante! 💡
> 
> Si una clase está marcada como open, sus métodos también deben ser open para poder ser sobrescritos. Si un método no está marcado como open, seguirá siendo final aunque su clase padre sea open.

---

## Métodos `open`

Para que un método pueda ser sobrescrito por una subclase, no solo su clase debe ser `open`, sino que el método en sí también debe serlo.

```kotlin
open class Animal(val especie: String) {
    open fun hacerSonido() { // Este método puede ser sobrescrito
        println("El animal hace un sonido genérico. 🔊")
    }

    fun dormir() { // Este método NO puede ser sobrescrito (es final por defecto)
        println("El animal está durmiendo. 😴")
    }
}

class Perro(raza: String) : Animal("Perro") {
    override fun hacerSonido() { // Sobrescribe el método open
        println("El perro ladra: ¡Guau! ¡Guau! 🐶")
    }

    // ERROR: 'dormir' en 'Animal' es final y no puede ser sobrescrito.
    // override fun dormir() {
    //     println("El perro está durmiendo profundamente. 💤")
    // }
}

fun main() {
    val miPerro = Perro("Labrador")
    miPerro.hacerSonido() // Salida: El perro ladra: ¡Guau! ¡Guau! 🐶
    miPerro.dormir()     // Salida: El animal está durmiendo. 😴
}
```

---

## ¿Por qué Kotlin fuerza el `final` por defecto?

Esta es una de las diferencias más significativas entre Kotlin y lenguajes como Java (donde las clases y métodos son `open` por defecto). La razón principal es promover un diseño de software más robusto:

1. **Evitar el Problema de la "Clase Base Frágil":** Cuando una clase base puede ser extendida libremente, cualquier cambio en su implementación interna puede romper el comportamiento de las subclases sin que estas lo sepan. Al forzar `final` por defecto, el diseñador de la API debe pensar explícitamente qué partes están destinadas a ser extendidas.
    
2. **Fomentar la Composición:** Kotlin te anima a usar la composición (construir objetos a partir de otros objetos) en lugar de la herencia profunda, lo que a menudo conduce a diseños más flexibles y fáciles de mantener.
    
3. **Rendimiento:** Las llamadas a métodos `final` pueden ser optimizadas por el compilador, ya que no hay necesidad de buscar la implementación correcta en la jerarquía de herencia en tiempo de ejecución.
    

> [!INFO] ¡Diseño de API Responsable! 👨‍💻
> 
> Al marcar una clase o método como open, estás asumiendo la responsabilidad de su futura compatibilidad con las subclases. Usa open solo cuando sea intencional y necesario para tu diseño.

---

## Alternativas a la Herencia Tradicional

Si bien `open` es útil, en Kotlin a menudo se prefieren otras formas de reutilización de código:

- **Interfaces con Implementaciones por Defecto:** Para definir contratos y proporcionar comportamiento predeterminado que las clases pueden implementar.
    
- **Delegación:** Permite que un objeto "delegue" la implementación de una interfaz a otro objeto, fomentando la composición.
    
- **Funciones de Extensión:** Para añadir funcionalidad a clases existentes sin modificar su código fuente o crear subclases.