---
tags:
  - kotlin
  - clases
---
Las **clases abstractas** pueden considerarse como un híbrido entre una clase abierta y una interfaz.

Hacer una clase abstracta tiene dos consecuencias:

- Pueden tener métodos y propiedades abstractos. Estos elementos se marcan con un modificador `abstract` y no tienen cuerpo. Son sólo definiciones, similares a las interfaces, que deben ser sobrescritas en las subclases.
- Las clases abstractas no pueden usarse para crear objetos. Sin embargo, pueden heredar subclases de ellas. Esto es resultado de la primera consecuencia: ==no puede crear objetos con métodos o propiedades abstractos. Éstos deben ser sobrescritos==.

>[!WARNING]
>Puede utilizar clases abstractas como sustituto de las interfaces, pero esto se considera una mala práctica.

Cada clase puede heredar de una sola clase pero implementar muchas interfaces. Se considera que las interfaces son un concepto más fácil de aprender que las clases abstractas. La principal ventaja de las clases abstractas es que pueden tener métodos no abiertos (en las interfaces, todos los elementos son abiertos) y propiedades no abstractas.

```kotlin
abstract class Animal(val nombre : String) {
	fun dormir() {
		println("$nombre está durmiendo. Zzz...")
	}

	abstract fun hacerSonido()
}
```

Implementando subclases:

```kotlin
class Perro(nombre : String) : Animal(nombre) {
	override fun hacerSonido() {
		println("$nombre dice: Guau guau")
	}
}

class Gato(nombre : String) : Animal(nombre) {
	override fun hacerSonido() {
		println("$nombre dice: Miau")
	}
}
```