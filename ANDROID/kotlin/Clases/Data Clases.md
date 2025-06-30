---
tags:
  - kotlin
  - clases
---
Las *data classes* en Kotlin son una herramiento para simplificar la gestión y manipulación de datos. Actúan como contenedores que almacenan datos y proporcionan funcionalidades adicionales sin necesidad de escribir código *boilerplate*.

## Frente a una clase básica

```kotlin
class Perro(  
    val name : String  
)  
data class Gato(  
    val name : String  
)  
  
fun main() {  
    val perro1 : Perro = Perro("Firulais")  
    val perro2 : Perro = Perro("Firulais")  
    val perro3 : Perro = Perro("Tobby")  
  
    println(perro1 == perro1) // true  
    println(perro1 == perro2) // false  
    println(perro1) // Perro@34c45dca  
  
  
    val gato1 : Gato = Gato("Sr. Bigotes")  
    val gato2 : Gato = Gato("Sr. Bigotes")  
    val gato3 : Gato = Gato("Pelusa")  
  
    println(gato1 == gato1) // true  
    println(gato1 == gato2) // true  
    println(gato1) // Gato(name=Sr. Bigotes)  
}
```

## Definición

Para definir una *data class*, utilizamos la palabra clave `data` antes de la declaración de la clase:

```kotlin
data class Persona(val nombre : String, val edad : Int)
```

Con esta simple declaración, Kotlin genera automáticamente varios métodos útiles:

- `equals()`: para comparar instancias basadas en sus propiedades.
- `hashCode()`: para obtener un código hash consistente con `equals()`.
- `toString()`: para obtener una representación en texto del objeto.
- `copy()`: para crear copias del objeto con la posibilidad de modificar algunas propiedades.
- Componentes `componentN()`: para soporte de destructuración.

## Uso de los métodos generados:

```kotlin
fun main() {
	val persona1 = Persona("Ana", 28)
	val persona2 = Persona("Ana", 28)

	println(persona1 == persona2) // true
	println(persona1) // Persona(nombre=Ana, edad=28)

	val persona3 = persona1.copy(edad = 29)
}
```

## Desestructuración de objetos

Las *data class* permiten extraer sus propiedades mediante desestructuración:

```kotlin
val (nombre, edad) = persona1
println("Nombre: $nombre, Edad: $edad")
```


