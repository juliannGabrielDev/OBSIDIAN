---
aliases:
  - ✨ Compresiones en Python
tags:
  - python
  - estructuras
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-05-31
---
# ✨ Compresiones en Python: ¡Código Mágico y Compacto!
- [[#1. ¿Qué son las Compresiones? 🤔|1. ¿Qué son las Compresiones? 🤔]]
- [[#2. List Comprehensions (Comprensión de Listas) 📝|2. List Comprehensions (Comprensión de Listas) 📝]]
- [[#3. Dictionary Comprehensions (Comprensión de Diccionarios) 🔑📖|3. Dictionary Comprehensions (Comprensión de Diccionarios) 🔑📖]]
- [[#4. Set Comprehensions (Comprensión de Conjuntos) 🌀|4. Set Comprehensions (Comprensión de Conjuntos) 🌀]]
- [[#5. ¿Por Qué Usarlas? 👍|5. ¿Por Qué Usarlas? 👍]]
- [[#⚡ Repaso Relámpago ⚡|⚡ Repaso Relámpago ⚡]]

---
## 1. ¿Qué son las Compresiones? 🤔
Son una forma súper **concisa** y **elegante** de crear listas, diccionarios o sets a partir de otros iterables (como listas, tuplas, rangos, etc.). ¡Es como un bucle `for` pero en una sola línea y más "pythonico"!
- 📌 **Sintaxis compacta**: Menos líneas de código.
- 📌 **Legibilidad**: A menudo, más fáciles de leer que un bucle `for` equivalente para crear colecciones.
- 📌 **Eficiencia**: Generalmente son más rápidas que los bucles `for` explícitos para estas tareas.
💡 **Tip:** Piensa en ellas como "recetas rápidas" para construir tus colecciones.

---
## 2. List Comprehensions (Comprensión de Listas) 📝
Te permiten crear **listas** de forma rápida. La sintaxis básica es: `[expresion for elemento in iterable if condicion]` (la parte del `if` es opcional).
- 📌 **`expresion`**: Lo que quieres hacer con cada `elemento`.
- 📌 **`for elemento in iterable`**: El bucle que recorre tu colección original.
- 📌 **`if condicion`** (Opcional): Para filtrar elementos.
**Ejemplos Mini:**
```python
# Crear una lista de cuadrados
cuadrados = [x**2 for x in range(1, 6)]
# Resultado: [1, 4, 9, 16, 25]
print(f"Cuadrados: {cuadrados} 🔢")

# Crear una lista de números pares del 0 al 9
pares = [num for num in range(10) if num % 2 == 0]
# Resultado: [0, 2, 4, 6, 8]
print(f"Pares: {pares} ✌️")
```
💪 **Tip:** ¡Son perfectas para transformar o filtrar listas existentes en una nueva!

---
## 3. Dictionary Comprehensions (Comprensión de Diccionarios) 🔑📖
Similar a las listas, pero para crear **diccionarios**. La sintaxis es: `{clave: valor for elemento in iterable if condicion}`.
- 📌 **`clave: valor`**: El par que formará parte del nuevo diccionario.
- 📌 El resto funciona igual que en las listas.
**Ejemplo Mini:**
```python
# Crear un diccionario con números y sus cuadrados
cuadrados_dict = {x: x**2 for x in range(1, 6)}
# Resultado: {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
print(f"Diccionario de cuadrados: {cuadrados_dict} 🖼️")

# Crear un diccionario a partir de una lista de tuplas (o dos listas)
nombres = ["Ana", "Luis", "Eva"]
edades = [30, 25, 28]
dic_edades = {nombre: edad for nombre, edad in zip(nombres, edades) if edad > 26}
# Resultado: {'Ana': 30, 'Eva': 28}
print(f"Diccionario de edades (>26): {dic_edades} 🧑‍🤝‍🧑")
```
🧠 **Tip:** ¡Geniales para construir diccionarios al vuelo a partir de otros datos!

---
## 4. Set Comprehensions (Comprensión de Conjuntos) 🌀
Para crear **sets** (conjuntos), que recuerdan, no tienen elementos duplicados y son desordenados. La sintaxis es: `{expresion for elemento in iterable if condicion}`.
- 📌 ¡Ojo! Se usan llaves `{}` como los diccionarios, pero no tienen el formato `clave: valor`.
**Ejemplo Mini:**
```python
# Crear un set de cuadrados (los duplicados se ignoran automáticamente)
numeros = [1, 2, 2, 3, 4, 4, 4, 5]
cuadrados_set = {x**2 for x in numeros}
# Resultado (el orden puede variar): {1, 4, 9, 16, 25}
print(f"Set de cuadrados: {cuadrados_set} 🎲")

# Crear un set de letras únicas de una palabra
palabra = "otorrinolaringologo"
letras_unicas = {letra for letra in palabra if letra in "aeiou"}
# Resultado (el orden puede variar): {'o', 'i', 'a'}
print(f"Vocales únicas: {letras_unicas} 🎤")
```
✨ **Tip:** ¡Útiles para obtener elementos únicos de un iterable aplicando alguna transformación o filtro!

---
## 5. ¿Por Qué Usarlas? 👍
- 📌 **Concisión**: Código más corto y directo. ¡Menos tecleo!
- 📌 **Legibilidad (a menudo)**: Para transformaciones y filtros simples, son más fáciles de entender de un vistazo que un bucle `for` completo.
- 📌 **Eficiencia**: Pueden ser un poco más rápidas que los bucles `for` explícitos para crear colecciones porque algunas optimizaciones ocurren "detrás de cámaras" en C (para CPython).
🧐 **Ojo:** No abuses. Si la lógica es muy compleja, un bucle `for` tradicional podría ser más legible.
🎉 **Tip:** ¡Practica usándolas y verás cómo tu código se vuelve más "Python Pro"!

---
## ⚡ Repaso Relámpago ⚡
- Las compresiones son una forma **compacta** de crear **listas, diccionarios y sets**.
- **Listas**: `[expr for x in iter if cond]` -> `[1, 4, 9]`
- **Diccionarios**: `{k:v for x in iter if cond}` -> `{1:1, 2:4}`
- **Sets**: `{expr for x in iter if cond}` -> `{1, 4, 9}` (elementos únicos)
- Son **eficientes**, **legibles** (para casos simples) y muy **pythonicas**.