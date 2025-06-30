---
tags:
  - kotlin
  - flujo
---
El bucle `while` en Kotlin es una estructura de control que ejecuta un bloque de código repetidamente mientras una condición sea verdadera.

```kotlin
// Estructura básica
while (condición) {
    // código a ejecutar
}
```

Ejemplo práctico:

```kotlin
fun main() {
    var contador = 1
    
    while (contador <= 5) {
        println("Número: $contador")
        contador++
    }
}
```

También existe el `do-while` que garantiza al menos una ejecución del bloque:

```kotlin
fun main() {
    var numero = 1
    
    do {
        println("Ejecutando... $numero")
        numero++
    } while (numero <= 3)
}
```

## Características importantes:

- La condición se evalúa antes de cada iteración (excepto en do-while)
- Si la condición es falsa desde el inicio, el código no se ejecuta (excepto en do-while)
- Es importante tener una condición de salida para evitar bucles infinitos

Uso común del while:

- Procesar datos hasta encontrar un valor específico
- Leer entrada del usuario hasta que sea válida
- Ejecutar tareas repetitivas con una condición de parada