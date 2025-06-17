---
aliases:
  - 🔢 Operaciones con Números
tags:
  - python
  - basico
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-06-02
---
# 🔢 Operaciones con Números en Python
- [[#1. ¿Números con Métodos? 🤔|1. ¿Números con Métodos? 🤔]]
- [[#2. Operadores Numéricos Comunes ➕➖✖️➗|2. Operadores Numéricos Comunes ➕➖✖️➗]]
- [[#3. Funciones Integradas Útiles para Números ⚙️|3. Funciones Integradas Útiles para Números ⚙️]]
- [[#4. Un Caso Especial: Números Complejos ✨|4. Un Caso Especial: Números Complejos ✨]]
- [[#✨ Repaso Relámpago|✨ Repaso Relámpago]]

---
## 1. ¿Números con Métodos? 🤔
A diferencia de las cadenas, los tipos de números básicos como `int` (enteros) y `float` (decimales) **no tienen muchos "métodos"** directos que llames con un punto (como `numero.metodo()`). En su lugar, usamos principalmente:
- **Operadores aritméticos**: `+`, `-`, `*`, `/`, etc.
- **Funciones integradas (built-in)**: `abs()`, `round()`, `sum()`, etc.
- Módulos como `math` para operaciones más avanzadas.
📌 **Tip:** ¡Piensa en los números como bloques fundamentales que manipulas con herramientas (operadores y funciones) en lugar de que tengan sus propias herramientas incorporadas!

---
## 2. Operadores Numéricos Comunes ➕➖✖️➗
Estos son tus mejores amigos para las matemáticas básicas:

| **Operador** | **Descripción**                    | **Mini Ejemplo (a=10, b=3)** | **Resultado** |
| ------------ | ---------------------------------- | ---------------------------- | ------------- |
| `+`          | Suma                               | `a + b`                      | `13`          |
| `-`          | Resta                              | `a - b`                      | `7`           |
| `*`          | Multiplicación                     | `a * b`                      | `30`          |
| `/`          | División (resultado es `float`)    | `a / b`                      | `3.333...`    |
| `//`         | División entera (descarta decimal) | `a // b`                     | `3`           |
| `%`          | Módulo (resto de la división)      | `a % b`                      | `1`           |
| `**`         | Exponenciación (potencia)          | `a ** b`                     | `1000`        |

💪 **Tip:** ¡Estos operadores son la base de casi cualquier cálculo numérico!

---
## 3. Funciones Integradas Útiles para Números ⚙️
Python viene con funciones listas para usar con números:


| **Función**             | **Descripción Breve**                                  | **Mini Ejemplo**             |
| ----------------------- | ------------------------------------------------------ | ---------------------------- |
| `abs(x)`                | Valor absoluto de `x`.                                 | `abs(-5)` → `5`              |
| `round(n, ndigits)`     | Redondea `n` a `ndigits` decimales.                    | `round(3.14159, 2)` → `3.14` |
| `pow(base, exp)`        | `base` elevado a la potencia `exp` (como `**`).        | `pow(2, 3)` → `8`            |
| `min(a, b, ...)`        | El menor de varios números o de un iterable.           | `min(1, 5, -2)` → `-2`       |
| `max(a, b, ...)`        | El mayor de varios números o de un iterable.           | `max(1, 5, -2)` → `5`        |
| `sum(iterable, start?)` | Suma los elementos de un iterable (ej: lista).         | `sum([1, 2, 3])` → `6`       |
| `int(x)`                | Convierte `x` a un número entero.                      | `int(3.9)` → `3`             |
| `float(x)`              | Convierte `x` a un número de punto flotante (decimal). | `float(5)` → `5.0`           |

🧠 **Tip:** ¡Estas funciones te ahorran escribir código extra para tareas comunes!

---
## 4. Un Caso Especial: Números Complejos ✨
Los números complejos (`complex`) sí tienen algunos atributos y un método útil:
- Un número complejo se crea como `c = 3 + 4j` o `complex(3, 4)`.
- `c.real`: Parte real (ej: `3.0`).
- `c.imag`: Parte imaginaria (ej: `4.0`).
- `c.conjugate()`: Devuelve el conjugado complejo (ej: `(3-4j)`).
**Mini Ejemplo:**
```python
num_complejo = 2 + 5j
print(num_complejo.real)         # Output: 2.0
print(num_complejo.imag)         # Output: 5.0
print(num_complejo.conjugate())  # Output: (2-5j)
```

💡 **Tip:** Útil si trabajas en áreas como la ingeniería eléctrica o física.

---
## ✨ Repaso Relámpago

- Los números (`int`, `float`) se manejan con **operadores** y **funciones integradas**.
- Operadores clave: `+`, `-`, `*`, `/`, `//`, `%`, `**`.
- Funciones útiles: `abs()`, `round()`, `min()`, `max()`, `sum()`, `int()`, `float()`.
- Los números **complejos** sí tienen atributos como `.real`, `.imag` y el método `.conjugate()`.