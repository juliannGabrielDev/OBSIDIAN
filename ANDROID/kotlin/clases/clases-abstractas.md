---
aliases:
  - Clases Abstractas 🚀✨
tags:
  - kotlin
  - clases
breadcrumb:
  - "[[indice-android|ANDROID 🤖🔗]]"
  - "[[indice-kotlin|📱 Índice Android]]"
Fecha: 2025-07-04
Docs:
  - https://kotlinlang.org/docs/classes.html#abstract-classes
---
# Clases Abstractas en Kotlin 🚀✨

Las clases abstractas en Kotlin son como planos 🗺️ que no pueden ser instanciados directamente. Su propósito principal es servir como una clase base 🏗️ para otras clases, proporcionando una estructura común y métodos que las subclases deben implementar.

> [!NOTE] ¿Por qué usar clases abstractas? 🤔
> 
> 🗒️ Son útiles cuando quieres definir una interfaz común para un grupo de clases relacionadas, pero no todas las funcionalidades pueden ser implementadas en la clase base.

## Características Clave 🔑

- **No se pueden instanciar:** No puedes crear objetos directamente de una clase abstracta. ¡Imagínate intentar construir una casa solo con un plano! 📏
    
- **Pueden contener propiedades y métodos abstractos:** Estos deben ser implementados por las subclases concretas. ¡Son como espacios en blanco que las subclases tienen que rellenar! ✍️
    
- **Pueden contener propiedades y métodos no abstractos:** También pueden tener implementaciones predeterminadas que las subclases pueden heredar o sobrescribir. Esto es útil para compartir código. ♻️
    
- **Las subclases deben implementar todos los miembros abstractos:** Si una subclase no implementa todos los miembros abstractos de su clase padre abstracta, ¡también debe ser abstracta! 🤯
    

## Sintaxis Básica 📝

Para declarar una clase abstracta, usa la palabra clave `abstract`:

```kotlin
abstract class NombreDeClaseAbstracta {
    // Propiedades y métodos abstractos y no abstractos
}
```

Para declarar un método o propiedad abstracta dentro de una clase abstracta:

```kotlin
abstract class Forma {
    abstract fun calcularArea(): Double // Método abstracto
    abstract var nombre: String // Propiedad abstracta

    fun imprimirInfo() { // Método no abstracto
        println("Esta es una forma.")
    }
}
```

## Implementación de Subclases 🧩

Las subclases que heredan de una clase abstracta deben implementar todos sus miembros abstractos usando la palabra clave `override`:

```kotlin
class Circulo(val radio: Double) : Forma() {
    override fun calcularArea(): Double {
        return Math.PI * radio * radio
    }

    override var nombre: String = "Círculo"
}

class Cuadrado(val lado: Double) : Forma() {
    override fun calcularArea(): Double {
        return lado * lado
    }

    override var nombre: String = "Cuadrado"
}
```

> [!TIP] Diferencia con las interfaces 💡
> 
> 💡 Mientras que las interfaces solo pueden definir la firma de métodos y propiedades, las clases abstractas pueden tener propiedades y métodos con implementación, lo que permite la reutilización de código.

## Ejemplo de Uso 🛠️

```kotlin
fun main() {
    val miCirculo = Circulo(5.0)
    println("Área del ${miCirculo.nombre}: ${miCirculo.calcularArea()}") // Salida: Área del Círculo: 78.53981633974483
    miCirculo.imprimirInfo() // Salida: Esta es una forma.

    val miCuadrado = Cuadrado(4.0)
    println("Área del ${miCuadrado.nombre}: ${miCuadrado.calcularArea()}") // Salida: Área del Cuadrado: 16.0
}
```

## Casos de Uso Comunes 🏢

|Caso de Uso|Descripción|
|---|---|
|**Jerarquías de Clases**|Definir una base común para una familia de objetos relacionados.|
|**Plantillas**|Proporcionar una plantilla con algunos pasos ya implementados y otros abstractos.|
|**Polimorfismo**|Tratar diferentes subclases de manera uniforme a través de la clase abstracta.|

> [!INFO] ¿Qué pasa si no implemento un miembro abstracto? 🚨
> 
> ℹ️ Si una subclase concreta no implementa todos los miembros abstractos de su clase base abstracta, ¡el compilador te lo hará saber con un error!

## Resumen ✍️

Las clases abstractas en Kotlin son una herramienta poderosa para el diseño de software orientado a objetos, permitiendo la creación de jerarquías de clases con comportamiento compartido y obligatorio. Son un pilar fundamental para la herencia y el polimorfismo.