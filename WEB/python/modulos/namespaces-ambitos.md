---
aliases:
  - 🌌 Espacios de Nombres y Ámbitos
tags:
  - python
  - modulos
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-06-02
---
# 🌌 Espacios de Nombres y Ámbitos en Python
- [[#1. ¿Qué son los Espacios de Nombres (Namespaces)? 🏷️|1. ¿Qué son los Espacios de Nombres (Namespaces)? 🏷️]]
- [[#2. ¿Y los Ámbitos (Scopes)? 🎯|2. ¿Y los Ámbitos (Scopes)? 🎯]]
- [[#3. La Regla LEGB 📜|3. La Regla LEGB 📜]]
- [[#4. `locals()` y `globals()` 🔍|4. `locals()` y `globals()` 🔍]]
- [[#5. Palabras Clave `global` y `nonlocal` 🔑|5. Palabras Clave `global` y `nonlocal` 🔑]]
- [[#✨ Repaso Relámpago|✨ Repaso Relámpago]]

---
## 1. ¿Qué son los Espacios de Nombres (Namespaces)? 🏷️
Un **espacio de nombres** es como un diccionario donde Python guarda todos los **nombres** (variables, funciones, clases) y los objetos a los que se refieren. Cada módulo, función o clase tiene su propio espacio de nombres.
- Asegura que los nombres sean **únicos** en su contexto.
- Evita **colisiones** (que dos cosas diferentes se llamen igual y causen confusión).
- Ejemplos: Módulo global, espacio local de una función.
📌 **Tip:** ¡Imagina una caja de etiquetas donde cada etiqueta (nombre) apunta a un solo objeto!

---
## 2. ¿Y los Ámbitos (Scopes)? 🎯
El **ámbito** (o scope) de un nombre es la **región del código** donde ese nombre es directamente accesible sin usar prefijos. Define la "visibilidad" de una variable u objeto.
- Python decide qué espacio de nombres usar según dónde estés en el código.
- Un nombre puede existir en varios namespaces, pero solo uno es "directamente visible".
🧐 **Tip:** Es como preguntar "¿Desde dónde puedo ver y usar esta variable sin más?"

---
## 3. La Regla LEGB 📜
Cuando usas un nombre, Python lo busca siguiendo un orden específico, conocido como la regla **LEGB**:
1. **L**ocal: El ámbito más interno, como dentro de una función.
2. **E**nclosing function locals: Ámbitos de funciones anidadas (de la más interna a la más externa).
3. **G**lobal: El ámbito del módulo actual.
4. **B**uilt-in: Nombres predefinidos en Python (ej: `print()`, `len()`).
- Python se detiene en cuanto encuentra el nombre.
- Si no lo encuentra en ningún lado, ¡`NameError`!
**Ejemplo Mini:**
```python
x = "Global X" # G

def exterior():
    y = "Enclosing Y" # E
    def interior():
        z = "Local Z" # L
        print(z, y, x, len) # L, E, G, B
    interior()

exterior()
```
🧠 **Tip:** ¡LEGB es el mapa del tesoro para encontrar dónde vive un nombre!

---
## 4. `locals()` y `globals()` 🔍
Python te da herramientas para "espiar" estos espacios de nombres:
- `locals()`: Devuelve un **diccionario** con los nombres (variables y sus valores) en el **ámbito local actual**.
- `globals()`: Devuelve un **diccionario** con los nombres en el **ámbito global actual** (el del módulo).
**Ejemplo Mini:**
```python
a_global = 10

def mi_funcion():
    b_local = 5
    print("Locals:", locals())
    print("Globals:", globals()['a_global']) # Accediendo a 'a_global'

mi_funcion()
# Locals: {'b_local': 5}
# Globals: 10
```
🛠️ **Tip:** ¡Útiles para depurar y entender qué nombres están disponibles dónde!

---
## 5. Palabras Clave `global` y `nonlocal` 🔑
A veces necesitas modificar variables que no están en tu ámbito local inmediato:
- `global nombre_variable`: Indica que `nombre_variable` se refiere a la variable en el **ámbito global**, permitiéndote modificarla desde dentro de una función.
- `nonlocal nombre_variable`: Similar, pero para variables en un **ámbito "enclosing"** (de una función anidada superior), que no sea el global.
**Ejemplos Mini:**
```python
# global
contador_global = 0
def incrementar_global():
    global contador_global
    contador_global += 1
incrementar_global()
print(contador_global) # Output: 1

# nonlocal
def funcion_exterior():
    contador_enclosing = 0
    def funcion_interior():
        nonlocal contador_enclosing
        contador_enclosing += 1
        print(contador_enclosing)
    funcion_interior() # Output: 1
    funcion_interior() # Output: 2
funcion_exterior()
```
✨ **Tip:** Usa `global` y `nonlocal` con cuidado; a veces es mejor pasar variables como argumentos y retornarlas.

---
## ✨ Repaso Relámpago
- **Namespaces**: Contenedores de nombres (variables).
- **Scopes**: Regiones donde los nombres son visibles.
- **LEGB**: Orden de búsqueda (Local, Enclosing, Global, Built-in).
- `locals()`/`globals()`: Para **ver** los namespaces.
- `global`/`nonlocal`: Para **modificar** variables en ámbitos superiores.