---
aliases:
  - Listas en Kotlin
breadcrumb:
  - "[[indice-android|Android]]"
  - "[[indice-kotlin|Kotlin]]"
Fecha: 2025-07-09
---
# Listas en Kotlin 📝

En Kotlin, una lista es una colección ordenada de elementos. Una característica clave es que pueden ser **inmutables** (solo lectura) o **mutables** (se pueden modificar).

---

## Tipos de Listas

Existen principalmente dos tipos de listas en el ecosistema de Kotlin:

|Tipo|Descripción|Creación|
|---|---|---|
|`List`|📜 **Inmutable**. Una vez creada, no se pueden añadir, eliminar o modificar sus elementos.|`listOf()`|
|`MutableList`|✍️ **Mutable**. Permite añadir, eliminar y modificar elementos después de su creación.|`mutableListOf()`|

> [!INFO] Inmutabilidad
> 
> ℹ️ Trabajar con listas inmutables es una buena práctica porque garantiza que los datos no cambiarán de forma inesperada, lo que hace el código más seguro y predecible, especialmente en entornos de programación concurrente.

---

## Creación e Inicialización

Crear listas en Kotlin es un proceso sencillo y directo.

### Listas Inmutables (`List`)

Se utiliza la función `listOf()` para crear una lista de solo lectura. El tipo de los elementos se infiere automáticamente.
```kotlin
// Lista de Strings
val nombres: List<String> = listOf("Ana", "Luis", "Juan")

// Lista de Enteros
val numeros = listOf(1, 2, 3, 4, 5)
```

### Listas Mutables (`MutableList`)

Para listas que necesitan ser modificadas, se usa `mutableListOf()`.

```kotlin
// Lista mutable de Strings
val planetas: MutableList<String> = mutableListOf("Mercurio", "Venus", "Tierra")

// Se pueden añadir nuevos elementos
planetas.add("Marte") // ["Mercurio", "Venus", "Tierra", "Marte"]
```

> [!TIP] Listas vacías
> 
> Pro Tip: Si necesitas crear una lista vacía, debes especificar explícitamente el tipo de datos que contendrá.
> 
> val listaVacia = `mutableListOf<String>()`

---

## Operaciones Comunes ⚙️

Tanto las listas mutables como las inmutables comparten un conjunto de operaciones de lectura.

### Acceso a Elementos

- **Por índice**: Se accede a los elementos mediante su posición (el índice empieza en 0).
    
- **Funciones de acceso**: Métodos como `.first()` y `.last()` para obtener el primer y último elemento.

```kotlin
val frutas = listOf("Manzana", "Pera", "Naranja")

println(frutas[1]) // Imprime "Pera"
println(frutas.first()) // Imprime "Manzana"
println(frutas.last()) // Imprime "Naranja"
```

### Propiedades Útiles

- `.size`: Devuelve el número de elementos en la lista.
    
- `.isEmpty()`: Comprueba si la lista está vacía.
    

### Operaciones Exclusivas de `MutableList`

Las listas mutables tienen métodos para modificar su contenido:

- **`add(elemento)`**: Añade un elemento al final.
    
- **`add(indice, elemento)`**: Añade un elemento en una posición específica.
    
- **`remove(elemento)`**: Elimina la primera aparición de un elemento.
    
- **`removeAt(indice)`**: Elimina el elemento en una posición específica.
    
- **`[indice] = nuevoElemento`**: Modifica un elemento existente.

```kotlin
val colores = mutableListOf("Rojo", "Verde")
colores.add("Azul") // ["Rojo", "Verde", "Azul"]
colores[0] = "Morado" // ["Morado", "Verde", "Azul"]
colores.removeAt(1) // ["Morado", "Azul"]
```

> [!WARNING] Índice fuera de rango
> 
> ¡Ten cuidado! Intentar acceder o modificar un elemento con un índice que no existe (IndexOutOfBoundsException) provocará un error en tiempo de ejecución.

---

## Iteración y Transformaciones ✨

Kotlin ofrece funciones de orden superior muy potentes para trabajar con colecciones.

### Recorrer una Lista

Se puede iterar fácilmente sobre los elementos de una lista usando un bucle `for` o el método `forEach`.

```kotlin
val animales = listOf("Perro", "Gato", "Pájaro")

// Usando for
for (animal in animales) {
    println(animal)
}

// Usando forEach
animales.forEach { animal ->
    println(animal)
}
```

### Funciones de Transformación

- **`.map()`**: Transforma cada elemento de una lista y devuelve una nueva lista con los resultados.
    
- **`.filter()`**: Devuelve una nueva lista con los elementos que cumplen una condición determinada.
    
- **`.sortedBy()`**: Ordena la lista según una propiedad específica.

```kotlin
val numeros = listOf(1, 2, 3, 4, 5, 6)

// .map(): Duplicar cada número
val duplicados = numeros.map { it * 2 } // [2, 4, 6, 8, 10, 12]

// .filter(): Obtener solo los números pares
val pares = numeros.filter { it % 2 == 0 } // [2, 4, 6]
```

