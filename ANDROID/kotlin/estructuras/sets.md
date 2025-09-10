---
aliases:
  - Sets en Kotlin
breadcrumb:
  - "[[indice-android|Android]]"
  - "[[indice-kotlin|Kotlin]]"
Fecha: 2025-07-09
---
# Sets en Kotlin 📝

Un `Set` es una colección de elementos únicos y sin un orden específico. La característica principal que lo distingue de otras colecciones es que **no permite elementos duplicados**.

## Características Principales

- **Elementos Únicos**: Si intentas agregar un elemento que ya existe en el `Set`, la operación simplemente se ignorará.
    
- **Sin Orden**: A diferencia de las listas, los `Set` no garantizan el orden de sus elementos. No puedes acceder a un elemento por su índice.
    
- **Inmutabilidad/Mutabilidad**: Al igual que otras colecciones en Kotlin, existen dos tipos de `Set`:
    
    - `Set`: Es de solo lectura (inmutable).
        
    - `MutableSet`: Permite agregar, eliminar y modificar sus elementos.
        

---

## Declaración de Sets

Puedes crear `Sets` utilizando las siguientes funciones de la librería estándar:

### Set Inmutable (`setOf`)

Se utiliza para crear un `Set` de solo lectura.

Kotlin

```
val numeros: Set<Int> = setOf(1, 2, 3, 4, 4, 5)
println(numeros) // Resultado: [1, 2, 3, 4, 5]
```

### Set Mutable (`mutableSetOf`)

Permite crear un `Set` que puede ser modificado después de su creación.
```kotlin
val vocales: MutableSet<Char> = mutableSetOf('a', 'e', 'i')
vocales.add('o')
vocales.add('a') // No se agregará de nuevo
vocales.remove('e')
println(vocales) // El orden no está garantizado
```

---

## Operaciones Comunes ⚙️

|Operación|Método|Descripción|Ejemplo|
|---|---|---|---|
|**Tamaño**|`.size`|Devuelve la cantidad de elementos.|`numeros.size`|
|**Agregar**|`.add()`|Añade un elemento (solo en `MutableSet`).|`vocales.add('u')`|
|**Eliminar**|`.remove()`|Elimina un elemento (solo en `MutableSet`).|`vocales.remove('i')`|
|**Verificar**|`in`|Comprueba si un elemento existe.|`if (3 in numeros)`|
|**Unión**|`.union()`|Devuelve un `Set` con elementos de ambos.|`setA.union(setB)`|
|**Intersección**|`.intersect()`|Devuelve un `Set` con elementos comunes.|`setA.intersect(setB)`|
|**Diferencia**|`.subtract()`|Devuelve un `Set` con elementos que no están en el otro.|`setA.subtract(setB)`|

---

## Set vs. List

La elección entre `Set` y `List` depende de tus necesidades:

|Característica|`Set`|`List`|
|---|---|---|
|**Duplicados**|❌ No permitidos|✅ Permitidos|
|**Orden**|❌ No garantizado|✅ Orden de inserción|
|**Acceso**|Por valor|Por índice (`list[0]`)|
|**Uso Típico**|Almacenar elementos únicos, como IDs.|Almacenar una secuencia de elementos.|

> [!TIP] ¿Cuándo usar un Set?
> 
> Pro Tip: Utiliza un Set cuando necesites asegurarte de que cada elemento en tu colección sea único y el orden no sea importante. Es muy eficiente para comprobar la existencia de un elemento.
