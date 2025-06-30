---
tags:
  - kotlin
  - funciones
---
En Kotlin, las funciones son bloques de código reutilizables que realizan una tarea específica. Permiten organizar el código de manera más estructurada y modular.

## Sintaxis básica

```kotlin
fun nombreDeLaFuncion(parametro1: Tipo1, parametro2: Tipo2): TipoDeRetorno {
    // Cuerpo de la función
    return valorDeRetorno
}

```

## Ejemplos de funciones

### Función simple

```kotlin
fun saludar(nombre: String): String {
    return "Hola, $nombre!"
}

// Uso
val mensaje = saludar("Julian")
println(mensaje) // Imprime: Hola, Julian!

```

### Función de una sola expresión

Kotlin permite simplificar funciones que solo contienen una expresión:

```kotlin
fun sumar(a: Int, b: Int): Int = a + b

```

### Parámetros con valores predeterminados

```kotlin
fun saludar(nombre: String, mensaje: String = "Hola"): String {
    return "$mensaje, $nombre!"
}

// Uso
println(saludar("Julian")) // Imprime: Hola, Julian!
println(saludar("Julian", "Bienvenido")) // Imprime: Bienvenido, Julian!

```

### Parámetros nombrados

Kotlin permite llamar a funciones usando parámetros nombrados para mejorar la legibilidad:

```kotlin
fun formatearDatos(nombre: String, edad: Int, ciudad: String): String {
    return "$nombre tiene $edad años y vive en $ciudad"
}

// Uso con parámetros nombrados
val info = formatearDatos(
    nombre = "Julian",
    edad = 25,
    ciudad = "Guadalajara"
)

```

### Funciones de extensión

Kotlin permite extender clases con nuevas funcionalidades sin heredar de ellas:

```kotlin
fun String.invertir(): String {
    return this.reversed()
}

// Uso
val texto = "Kotlin"
println(texto.invertir()) // Imprime: niltoK

```

### Funciones de orden superior

Las funciones pueden recibir otras funciones como parámetros o devolverlas como resultado:

```kotlin
fun operacion(a: Int, b: Int, operador: (Int, Int) -> Int): Int {
    return operador(a, b)
}

// Uso
val resultado = operacion(5, 3) { x, y -> x + y } // Suma: 8
val producto = operacion(5, 3) { x, y -> x * y } // Multiplicación: 15

```

## Funciones lambda

Las expresiones lambda son funciones anónimas que pueden ser pasadas como parámetros o almacenadas en variables:

```kotlin
val suma = { a: Int, b: Int -> a + b }
println(suma(5, 3)) // Imprime: 8

// Uso como parámetro
val numeros = listOf(1, 2, 3, 4, 5)
val pares = numeros.filter { it % 2 == 0 }
println(pares) // Imprime: [2, 4]

```

## Funciones infix

Las funciones infix permiten una sintaxis más natural para ciertas operaciones:

```kotlin
infix fun Int.hasta(hasta: Int): IntRange {
    return this..hasta
}

// Uso
val rango = 1 hasta 5 // Equivalente a 1..5
println(rango.toList()) // Imprime: [1, 2, 3, 4, 5]

```

## Funciones con número variable de argumentos (vararg)

```kotlin
fun sumarTodos(vararg numeros: Int): Int {
    return numeros.sum()
}

// Uso
val total = sumarTodos(1, 2, 3, 4, 5) // Resultado: 15

```

## Funciones suspendidas (para corrutinas)

Kotlin soporta corrutinas mediante funciones suspendidas:

```kotlin
suspend fun obtenerDatos(): Datos {
    delay(1000) // Simula operación de larga duración
    return Datos()
}

```

## Buenas prácticas

- Nombres descriptivos para las funciones
- Funciones pequeñas con una única responsabilidad
- Documentar funciones complejas
- Utilizar parámetros nombrados para mejorar la legibilidad
- Aprovechar los valores predeterminados para parámetros opcionales