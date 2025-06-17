---
aliases:
  - 💡 Funciones lambda
tags:
  - python
  - funciones
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-05-31
---
# 💡 Funciones `lambda` en Python: ¡Pequeñas pero Poderosas!

---

## 1. ¿Qué son las Funciones `lambda`? 🤔

Una función `lambda` es una **función anónima pequeña y de una sola línea**. "Anónima" significa que no se define con la palabra clave `def` y no tiene un nombre formal como las funciones normales. Se crean usando la palabra clave `lambda`.

- 📌 **Anónimas**: No tienen nombre propio (a menos que las asignes a una variable, ¡pero ese no es su uso principal!).
- 📌 **Una sola expresión**: Su cuerpo solo puede contener una única expresión, no múltiples sentencias o bloques de código complejos.
- 📌 **Retorno implícito**: Automáticamente devuelven el resultado de esa expresión.
- 📌 **Sintaxis**: `lambda argumentos: expresion`

**Ejemplo Mini:**
```python
# Una lambda que suma dos números
sumar = lambda x, y: x + y
resultado = sumar(5, 3)
print(f"Suma con lambda: {resultado} ➕") # Suma con lambda: 8

# Una lambda que devuelve el cuadrado de un número
cuadrado = lambda n: n * n
print(f"Cuadrado de 4: {cuadrado(4)} 🔢") # Cuadrado de 4: 16
```

🧠 **Tip:** Piensa en `lambda` como una "función instantánea" para tareas simples.

---

## 2. Características Clave 🌟

Las `lambda` tienen algunas particularidades que las hacen especiales:

- 📌 **Concisas**: Su principal ventaja es la brevedad.
- 📌 **Argumentos Flexibles**: Pueden tener cero, uno o múltiples argumentos, igual que las funciones `def`.
    - `lambda: "Hola"` (sin argumentos)
    - `lambda x: x * 2` (un argumento)
    - `lambda x, y, z: x + y + z` (múltiples argumentos)
- 📌 **Limitadas a una expresión**: No puedes poner un `if/else` complejo, bucles `for` o `while` dentro de una `lambda` (aunque sí puedes usar expresiones condicionales ternarias: `x if C else y`).

**Ejemplo con expresión condicional:**
```python
es_par_lambda = lambda num: "Par" if num % 2 == 0 else "Impar"
print(f"7 es: {es_par_lambda(7)} 🤔") # 7 es: Impar
```

✨ **Tip:** Su limitación a una expresión es lo que las mantiene simples y enfocadas.

---

## 3. ¿Cuándo Usarlas? 🎯

Las `lambda` brillan cuando necesitas una función pequeña para un uso rápido, especialmente como argumento de funciones de orden superior (funciones que toman otras funciones como entrada).

- 📌 **Argumentos de `map()`**: Para aplicar una transformación simple a cada elemento.
    ```python
    numeros = [1, 2, 3, 4]
    dobles = list(map(lambda x: x * 2, numeros)) # [2, 4, 6, 8]
    print(f"Dobles con map y lambda: {dobles} ✌️")
    ```
    
- 📌 **Argumentos de `filter()`**: Para seleccionar elementos basados en una condición simple.
    ```python
    numeros = [1, 2, 3, 4, 5, 6]
    impares = list(filter(lambda x: x % 2 != 0, numeros)) # [1, 3, 5]
    print(f"Impares con filter y lambda: {impares}  lọc")
    ```
    
- 📌 **Argumentos `key` en `sorted()`, `min()`, `max()`**: Para especificar cómo ordenar o comparar elementos complejos.
    ```python
    puntos = [(1, 5), (3, 2), (0, 8)]
    # Ordenar por el segundo elemento de cada tupla
    ordenados_y = sorted(puntos, key=lambda punto: punto[1]) # [(3, 2), (1, 5), (0, 8)]
    print(f"Puntos ordenados por Y: {ordenados_y} 📊")
    ```
    

🚀 **Tip:** Si vas a usar una función solo una vez y es muy simple, ¡`lambda` es tu amiga!

---

## 4. `lambda` vs. Funciones `def` Normales 🤔

Aunque ambas definen funciones, tienen sus diferencias y usos ideales:

| **Característica** | **lambda**                               | **def (función normal)**                |
| ------------------ | ---------------------------------------- | --------------------------------------- |
| **Nombre**         | Anónima (sin nombre formal)              | Con nombre                              |
| **Cuerpo**         | Una sola expresión                       | Múltiples sentencias, lógica compleja   |
| **`return`**       | Implícito                                | Explícito (necesita `return`)           |
| **Docstrings**     | No directamente                          | Sí, para documentación                  |
| **Complejidad**    | Ideal para funciones muy simples         | Para cualquier nivel de complejidad     |
| **Uso principal**  | Como argumento, funciones de un solo uso | Funciones reutilizables, más elaboradas |

**Ejemplo Comparativo:**
```python
# Con lambda
multiplicar_lambda = lambda a, b: a * b
print(f"Lambda: 3*4 = {multiplicar_lambda(3,4)}")

# Con def
def multiplicar_def(a, b):
  """Esta función multiplica dos números."""
  return a * b
print(f"Def: 3*4 = {multiplicar_def(3,4)}")
```

⚖️ **Tip:** No fuerces una `lambda` si la lógica se vuelve complicada. Una función `def` bien nombrada es más legible en esos casos.

---

## ⚡ Repaso Relámpago ⚡

- Las funciones `lambda` son **anónimas**, de **una sola expresión** y con **retorno implícito**.
- Sintaxis: `lambda argumentos: expresion`.
- Ideales para **tareas simples** y como argumentos de funciones como `map()`, `filter()`, `sorted()`.
- **No reemplazan** a las funciones `def` para lógica más compleja o reutilizable.
- Son una herramienta para escribir código más **conciso** en situaciones específicas.