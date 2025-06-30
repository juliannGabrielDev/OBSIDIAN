---
tags:
  - kotlin
  - clases
---
Las `enum class` en Kotlin son una herramienta fantástica para representar conjuntos limitados de valores constantes.

### Enum Classes en Kotlin

Las **clases enum** en Kotlin se utilizan para definir tipos que tienen un número fijo de constantes conocidas. Son ideales para modelar datos como días de la semana, estados de un proceso, direcciones, entre otros.

#### Declaración de una Enum Class

Para definir una `enum class`, utiliza la palabra clave `enum` antes de `class` y enumera las constantes:

```kotlin
enum class Direccion {
	NORTE,
	SUR,
	ESTE,
	OESTE
}
```

#### Uso Básico

Puedes asignar y comparar valores de una enum directamente:

```kotlin
val direccionActual = Direccion.NORTE

if (direccionActual == Direccion.NORTE) {
	println("Estás yendo hacia el norte")
}
```

También puedes iterar sobre los valores de la enum:

```kotlin
for (direccion in Direccion.values()) {
 println(direccion)
}
```

#### Propiedades y Métodos en Enums

Las enums en Kotlin pueden tener propiedades y métodos asociados a cada constante.

**Ejemplo con propiedades:**

```kotlin
enum class DiaSemana(val esLaboral : Boolean) {
	LUNES(true),
	MARTES(true),
	MIERCOLES(true),
	JUEVES(true),
	VIERNES(true),
	SABADO(false),
	DOMINGO(false)
}
```

Aquí, cada constante de `DiaSemana` tiene una propiedad `esLaboral` que indica si es un día laboral o no.

**Ejemplo con métodos:**

```kotlin
enum class Operacion {
	SUMA {
		override fun aplicar(a : Int, b : Int) = a + b
	},
	RESTA {
		override fun aplicar(a : Int, b : Int) = a - b
	},
	MULTIPLICACION {
		override fun aplicar(a : Int, b : Int) = a * b
	},
	DIVISION {
		override fun aplicar(a : Int, b : Int) = a / b
	}
}
```

Cada constante implementa el método `aplicar`, proporcionando su propia lógica.

#### Uso de Enums con Propiedades y Métodos

```kotlin
val dia = DiaSemana.SABADO
println("¿Es laboral? ${dia.esLaboral}") // ¿Es laboral? false

val resultado = Operacion.MULTIPLICACION.aplicar(4, 5)
println("El resultado es $resultado") // El resultado es 20
```

### Implementación de Interfaces

Las enums pueden implementar interfaces, lo que permite añadir comportamiento común:

```kotlin
interface Accion {
    fun ejecutar()
}

enum class EstadoSistema : Accion {
    INICIAR {
        override fun ejecutar() { println("Sistema iniciando...") }
    },
    DETENER {
        override fun ejecutar() { println("Sistema deteniéndose...") }
    },
    REINICIAR {
        override fun ejecutar() { println("Sistema reiniciando...") }
    }
}
```