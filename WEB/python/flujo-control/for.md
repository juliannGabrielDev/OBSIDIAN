---
aliases:
  - 🔁 Bucles for en Python 🔁
tags:
  - python
  - flujo-control
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-05-31
---
# 🔁 Bucles `for` en Python 🔁
---
## 1. ¿Qué es un Bucle `for`? 🤔
Un bucle `for` en Python se usa para **iterar sobre una secuencia** (como una lista, una tupla, un diccionario, un conjunto o una cadena de1 texto) u otros objetos iterables. Es como decir: "Para cada elemento en esta colección, haz esto".
- **Idea Clave**: Recorrer elementos uno por uno.
- Se usa cuando sabes **cuántos elementos** hay en la colección o quieres procesar cada uno de ellos.
📌 **Tip**: Piensa en "para cada manzana en la canasta, revísala".

---
## 2. ¿Cómo Funciona? ⚙️
El bucle `for` toma cada elemento de la secuencia, uno a la vez, y lo asigna a una variable temporal. Luego, ejecuta el bloque de código indentado con esa variable.
1. Toma el **primer elemento** de la secuencia y lo asigna a la `variable_iteradora`.
2. **Ejecuta el bloque de código** usando esa `variable_iteradora`.
3. Toma el **siguiente elemento** de la secuencia y repite el paso 2.
4. Continúa hasta que no queden más elementos en la secuencia.
- **Sintaxis Básica**:
    ```python
    for variable_iteradora in secuencia:
        # bloque de código a ejecutar
        # para cada elemento
    ```
🧠 **Tip**: "Toma uno, haz algo. Toma el siguiente, haz lo mismo... ¡hasta terminar!"

---
## 3. Mini Ejemplos Prácticos 💡
Veamos cómo funciona con diferentes secuencias:
**a) Recorriendo una Lista 📝:**
```python
frutas = ["manzana", "banana", "cereza"]
for fruta in frutas:
    print(f"Me gusta la: {fruta} 🍎")

# Salida:
# Me gusta la: manzana 🍎
# Me gusta la: banana 🍎
# Me gusta la: cereza 🍎
```
- `fruta` toma el valor de cada elemento de la lista `frutas` en cada iteración.
**b) Recorriendo una Cadena de Texto (String) 🧵:**
```python
palabra = "Hola"
for letra in palabra:
    print(letra)

# Salida:
# H
# o
# l
# a
```
- `letra` toma el valor de cada carácter de la cadena `palabra`.
**c) Usando `range()` para Repetir un Número de Veces 🔢:**
`range()` genera una secuencia de números. ¡Es muy útil!
```python
for numero in range(3):  # range(3) genera 0, 1, 2
    print(f"Número de vuelta: {numero}")

# Salida:
# Número de vuelta: 0
# Número de vuelta: 1
# Número de vuelta: 2

for i in range(1, 4): # range(1, 4) genera 1, 2, 3
    print(f"Contando: {i}")

# Salida:
# Contando: 1
# Contando: 2
# Contando: 3
```
- `range(n)` va de `0` a `n-1`.
- `range(inicio, fin)` va de `inicio` a `fin-1`.
- `range(inicio, fin, paso)` va de `inicio` a `fin-1`, saltando de `paso` en `paso`.

📘 **Tip**: `range()` es tu amigo para bucles `for` donde necesitas un contador o un número específico de repeticiones.

---
## 🎓 Repaso Relámpago
Los bucles `for` son esenciales para recorrer elementos en secuencias como listas, cadenas o rangos de números. En cada paso, una variable toma el valor del elemento actual de la secuencia. Son perfectos cuando el número de iteraciones está definido por el tamaño de la secuencia.