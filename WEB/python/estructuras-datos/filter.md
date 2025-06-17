---
aliases:
  - 🔍 La Función filter() en Python
tags:
  - python
  - estructuras
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-05-31
---
# 🔍 La Función `filter()` en Python: ¡Seleccionando lo que Importa!

---

## 1. ¿Qué es `filter()`? 🤔

La función `filter()` construye un **iterador** a partir de los elementos de un iterable (como una lista, tupla, etc.) para los cuales una función devuelve `True`. ¡Es como un colador para tus datos!

- 📌 **Propósito**: Filtrar elementos de una secuencia.
- 📌 **Argumentos**: Necesita dos cosas:
    1. Una `funcion` que devuelva `True` o `False` (un predicado).
    2. Un `iterable` (lista, tupla, set, etc.) cuyos elementos se van a evaluar.
- 📌 **Devuelve**: Un **objeto filtro** (un iterador). Para ver los resultados como una lista, normalmente lo conviertes con `list()`.
- 📌 **Sintaxis**: `filter(funcion, iterable)`

💡 **Tip:** `filter()` te ayuda a "quedarte con lo bueno" según tus criterios.

---

## 2. ¿Cómo Funciona? ⚙️

La `funcion` que le pasas a `filter()` se aplica a cada elemento del `iterable`. Si la función devuelve `True` para un elemento, ese elemento se incluye en el resultado. Si devuelve `False`, se descarta.

- 📌 La función de filtro debe ser booleana (o evaluarse como tal).
- 📌 `filter()` no modifica el iterable original, crea uno nuevo (el objeto filtro).

**Ejemplo Mini:**
```python
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# 1. Definimos la función de filtro
def es_impar(numero):
  return numero % 2 != 0

# 2. Usamos filter()
numeros_impares_filtro = filter(es_impar, numeros)

# 3. Convertimos a lista para verlos
numeros_impares_lista = list(numeros_impares_filtro)
print(f"Números impares: {numeros_impares_lista}  विषम") # [1, 3, 5, 7, 9]
```

✨ **Tip:** ¡La función de filtro es tu "detector" de elementos deseados!

---

## 3. `filter()` con Funciones `lambda` ⚡

A menudo, la condición de filtro es simple y se puede expresar con una **función `lambda`** (una función anónima pequeña). ¡Esto hace el código más compacto!

- 📌 `lambda argumentos: expresion`
- 📌 Perfecto para filtros de una sola línea.

**Ejemplo Mini con `lambda`:**
```python
edades = [12, 17, 18, 21, 30, 8]

# Filtrar mayores de edad (>= 18)
mayores_edad = list(filter(lambda edad: edad >= 18, edades))
print(f"Mayores de edad: {mayores_edad} 🔞") # [18, 21, 30]

# Filtrar palabras largas de una lista
palabras = ["sol", "casa", "bicicleta", "extraordinario", "luz"]
largas = list(filter(lambda p: len(p) > 5, palabras))
print(f"Palabras largas: {largas} 📏") # ['bicicleta', 'extraordinario']
```

🚀 **Tip:** `lambda` y `filter()` son grandes amigos para filtros rápidos y directos.

---

## 4. `filter()` vs. List Comprehensions 🤔

Puedes lograr resultados similares a `filter()` usando **List Comprehensions** (Comprensión de Listas), y a menudo estas últimas son preferidas por ser más legibles y "pythonicas" para muchos casos.

- 📌 **List Comprehension**: `[expresion for elemento in iterable if condicion]`
- 📌 **Legibilidad**: Para filtros sencillos, una list comprehension puede ser más clara.
- 📌 **Uso**: `filter()` puede ser más útil si la función de filtro ya está definida en otro lugar o es más compleja y reutilizable.

**Ejemplo Mini Comparativo:**
```python
numeros = [1, 2, 3, 4, 5, 6]

# Con filter() y lambda
pares_filter = list(filter(lambda x: x % 2 == 0, numeros))
print(f"Pares con filter: {pares_filter} ✌️") # [2, 4, 6]

# Con List Comprehension
pares_lc = [x for x in numeros if x % 2 == 0]
print(f"Pares con List Comp: {pares_lc} ✅") # [2, 4, 6]
```

⚖️ **Tip:** Elige la que haga tu código más claro. Si la lógica de filtrado es una simple condición, la list comprehension suele ganar.

---

## ⚡ Repaso Relámpago ⚡

- `filter(funcion, iterable)` crea un iterador con elementos para los que `funcion` devuelve `True`.
- La `funcion` debe ser un predicado (retornar `True` o `False`).
- Se usa mucho con `lambda` para filtros concisos.
- Las **List Comprehensions** son una alternativa popular y a menudo más legible para filtros.
- El resultado de `filter()` es un iterador, ¡conviértelo a `list()` si necesitas una lista!