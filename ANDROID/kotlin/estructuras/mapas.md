---
aliases:
  - Mapas en Kotlin
breadcrumb:
  - "[[indice-android|Android]]"
  - "[[indice-kotlin|Kotlin]]"
Fecha: 2025-07-09
---
# Mapas en Kotlin 🗺️

Un mapa (o diccionario) en Kotlin es una colección que almacena datos en pares de **clave-valor**. Cada clave es única y se utiliza para acceder a su valor correspondiente. Al igual que las listas, los mapas pueden ser inmutables o mutables.

---

## Tipos de Mapas

|Tipo|Descripción|Creación|
|---|---|---|
|`Map`|🔒 **Inmutable**. No se pueden añadir ni eliminar pares clave-valor después de su creación.|`mapOf()`|
|`MutableMap`|✏️ **Mutable**. Permite añadir, eliminar y modificar pares clave-valor.|`mutableMapOf()`|

> [!INFO] Unicidad de las Claves
> 
> ℹ️ La característica más importante de un mapa es que no puede tener claves duplicadas. Si intentas insertar un par con una clave que ya existe, el valor asociado a esa clave se actualizará.

---

## Creación e Inicialización

Crear mapas es sencillo utilizando las funciones de la librería estándar de Kotlin.

### Mapas Inmutables (`Map`)

Se utiliza la función `mapOf()` y el operador `to` para vincular cada clave con su valor.

```kotlin
// Mapa de String a Int
val edades: Map<String, Int> = mapOf(
    "Ana" to 28,
    "Luis" to 31,
    "Juan" to 25
)
```

### Mapas Mutables (`MutableMap`)

Para mapas que necesitan ser modificados, se usa `mutableMapOf()`.

```kotlin
// Mapa mutable de capitales
val capitales: MutableMap<String, String> = mutableMapOf(
    "Francia" to "París",
    "Japón" to "Tokio"
)

// Se pueden añadir nuevos pares
capitales["España"] = "Madrid" 

println(capitales) // {Francia=París, Japón=Tokio, España=Madrid}
```

---

## Operaciones Comunes 🛠️

Los mapas ofrecen métodos específicos para manipular sus datos basados en las claves.

### Acceso a Valores

Se puede acceder a un valor utilizando su clave a través de la sintaxis de corchetes `[]` o el método `.get()`.

```kotlin
val puntuaciones = mapOf("jugador1" to 100, "jugador2" to 85)

// Usando corchetes
val puntos1 = puntuaciones["jugador1"] // 100

// Usando .get() (devuelve null si la clave no existe)
val puntos3 = puntuaciones.get("jugador3") // null
```

> [!TIP] Valores por Defecto
> 
> Pro Tip: Puedes usar la función .getOrDefault() para proporcionar un valor por defecto si la clave no se encuentra, evitando así un resultado null.
> 
> val puntos4 = puntuaciones.getOrDefault("jugador4", 0) // Devuelve 0

### Propiedades Útiles

- `.size`: Devuelve el número de pares clave-valor.
    
- `.keys`: Devuelve una colección con todas las claves.
    
- `.values`: Devuelve una colección con todos los valores.
    
- `.isEmpty()`: Comprueba si el mapa está vacío.
    
- `.containsKey(clave)`: Verifica si el mapa contiene una clave específica.
    

### Operaciones Exclusivas de `MutableMap`

- **`[clave] = valor`** o **`put(clave, valor)`**: Añade o actualiza un par.
    
- **`remove(clave)`**: Elimina un par basándose en su clave.
    
- **`clear()`**: Elimina todos los elementos del mapa.
    
```kotlin
val inventario = mutableMapOf("manzanas" to 5, "naranjas" to 3)
inventario.put("plátanos", 8) // Añade un nuevo par
inventario["manzanas"] = 10    // Actualiza el valor de una clave existente
inventario.remove("naranjas")   // Elimina un par
```

> [!DANGER] Valores Nulos
> 
> Peligro: Si usas el operador !! para forzar un valor no nulo de una clave que no existe, tu programa fallará con una NullPointerException. Es más seguro manejar la posible ausencia de la clave.

---

## Iteración sobre Mapas 🔄

Puedes recorrer las claves, los valores o los pares completos de un mapa.
```kotlin
val usuarios = mapOf(1 to "Alex", 2 to "Beatriz", 3 to "Carlos")

// Iterar sobre los pares (entries)
for ((id, nombre) in usuarios) {
    println("ID: $id, Nombre: $nombre")
}

// Iterar solo sobre las claves
for (id in usuarios.keys) {
    println("ID de usuario: $id")
}

// Iterar solo sobre los valores
usuarios.values.forEach { nombre ->
    println("Nombre: $nombre")
}
```