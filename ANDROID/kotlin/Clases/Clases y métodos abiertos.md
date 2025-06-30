---
tags:
  - kotlin
  - clases
---
## Clases y métodos `open` en Kotlin

En Kotlin, las clases y métodos son **finales por defecto**. Esto significa que no se pueden heredar ni sobrescribir.  Para permitir la herencia y la sobrescritura, debe marcarlos explícitamente con la palabra clave `open`.

### Declarar una clase `open`

Para que una clase pueda sar heredada, declárala como `open`:

```kotlin
open class Animal {
	open fun hacerSonido() {
		println("El animal hace un sonido")
	}
}
```

### Heredar de una clase `open`

Ahora puedes crear una subclase que herede de `Animal`:

```kotlin
class Perro : Animal() {
	override fun hacerSonido() {
		println("El perro ladra")
	}
}
```

### Métodos `open` y sobrescritura

Los métodos que pueden ser sobrescritos deben ser marcados como `open`. En la subclase, utilizas `override` para sobrescribirlo.

```kotlin
open class Vehiculo {
	open fun encender() {
		println("El vehículo se ha encendido")
	}
}

class Coche : Vehiculo() {
	override fun encender() {
		println("El coche está en marcha")
	}
}
```

## Llamada al constructor de la superclase en Kotlin

En Kotlin, cuando heredas de una clase, es esencial llamar al constructor de la superclase para asegurar una correcta inicialización. Esta llamada se realiza para que las propiedades y el comportamiento de la superclase se configuren antes de que la subclase añada sus propias características.

### Constructor primario

Si la superclase tiene un **constructor primario**, la subclase debe proporcionar los parámetros necesarios al heredar:

```kotlin
open class Persona(val nombre : String)

class Estudiante(nombre : String, val universidad : String) : Persona(nombre)
```

En este ejemplo:

- `Persona` es la superclase con un constructor primario que recibe `nombre`.
- `Estudiante` hereda de `Persona` y pasa `nombre` al constructor de la superclase en la declaración de la clase.

### Constructor secundario

Si trabajas con **constructores secundarios**, puedes llamar al constructor de la superclase utilizando `super`:

```kotlin
open class Empleado {
	constructor(nombre : String) {
		println("Empleado: $nombre")
	}
}

class Gerente : Empleado {
	constructor(nombre : String, departamento : String) : super(nombre) {
		println("Gerente del departamento de $departamento")
	}
}
```

### Llamada a superclase en métodos sobrescritos

Cunado sobrescribes un método, puedes usar `super` para acceder a la implementación de la superclas:

```kotlin
open class Vehiculo {
	open fun arrancar() {
		println("El vehículo arranca")
	}
}

class Coche : Vehiculo() {
	override fun arrancar() {
		super.arrancar()
		println("El coche está en marcha")
	}
}

fun main() {
	val miCoche = Coche()
	miCoche.arrancar()
	// El vehículo arranca
    // El coche está en marcha
}
```