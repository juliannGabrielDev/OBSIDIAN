---
aliases:
  - 📊 Listas, Tuplas y Sets en Python
tags:
  - python
  - estructuras
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-05-31
---
# 📊 Listas, Tuplas y Sets en Python: ¡Colecciones al Rescate!
---
## 1. Listas 📝 [ ]
Son colecciones **ordenadas** y **modificables** (mutables). ¡Puedes cambiar, agregar o quitar elementos después de crearla! Permiten elementos duplicados.
- 📌 **Ordenadas**: Los elementos mantienen el orden en que los agregaste.
- 📌 **Mutables**: Puedes cambiar su contenido. ¡Son flexibles!
- 📌 **Duplicados OK**: Puedes tener el mismo valor varias veces.
- 📌 **Sintaxis**: Se definen con corchetes `[]`.
**Ejemplo Mini:**
```python
mi_lista = [1, "Python", 3.0, "Python"]
mi_lista.append("nuevo") # Agrega al final
mi_lista[0] = "cero"   # Modifica un elemento
print(mi_lista)        # ['cero', 'Python', 3.0, 'Python', 'nuevo']
```
💪 **Tip:** ¡Usa listas cuando necesites una colección versátil que pueda cambiar!

---
## 2. Tuplas 🔒 ( )
Son colecciones **ordenadas** pero **inmutables**. Una vez que creas una tupla, ¡no puedes cambiar su contenido! También permiten elementos duplicados.
- 📌 **Ordenadas**: Al igual que las listas, mantienen el orden.
- 📌 **Inmutables**: ¡No se pueden modificar después de su creación! Son como una lista "con candado".
- 📌 **Duplicados OK**: También pueden tener elementos repetidos.
- 📌 **Sintaxis**: Se definen con paréntesis `()`.
**Ejemplo Mini:**
```python
mi_tupla = (1, "Python", 3.0, "Python")
# mi_tupla[0] = "cero"  # ¡Esto daría un error! TypeError
print(mi_tupla[1])     # Acceder a un elemento: 'Python'
print(mi_tupla.count("Python")) # Contar ocurrencias: 2
```
🛡️ **Tip:** ¡Usa tuplas para datos que no deben cambiar, como coordenadas o constantes!

---
## 3. Sets (Conjuntos) 🌀 { }
Son colecciones **desordenadas** y **mutables**, pero **no permiten elementos duplicados**. ¡Cada elemento es único!
- 📌 **Desordenados**: No garantizan ningún orden específico.
- 📌 **Mutables**: Puedes agregar o quitar elementos.
- 📌 **Sin Duplicados**: Si intentas agregar un elemento que ya existe, simplemente no se añade.
- 📌 **Sintaxis**: Se definen con llaves `{}`. Ojo: `{}` vacío crea un diccionario, usa `set()` para un set vacío.
**Ejemplo Mini:**
```python
mi_set = {1, "Python", 3.0, "Python"} # "Python" solo aparece una vez
print(mi_set)                         # El orden puede variar, ej: {1, 3.0, 'Python'}
mi_set.add("nuevo")
mi_set.remove(1)
print(mi_set)
```
🔢 **Tip:** ¡Usa sets para verificar pertenencia o eliminar duplicados rápidamente! Son geniales para operaciones matemáticas de conjuntos (unión, intersección).

---
## 4. ¿Cuándo Usar Cada Uno? 🤔
¡Depende de lo que necesites!
- 📋 **Listas**: Para colecciones generales que necesitas modificar (agregar, quitar, cambiar elementos). Es la más flexible.
- 🔒 **Tuplas**: Cuando tienes datos que no deberían cambiar (integridad de datos). Son más rápidas que las listas para iterar.
- ✨ **Sets**: Cuando necesitas asegurarte de que no haya duplicados o para realizar operaciones de conjuntos. Muy eficientes para buscar elementos.
**Tabla Resumen Mini:**

| **Característica** | **Lista []** | **Tupla ()** | **Set {}** |
| ------------------ | ------------ | ------------ | ---------- |
| Ordenada           | Sí ✅         | Sí ✅         | No ❌       |
| Mutable            | Sí ✅         | No ❌         | Sí ✅       |
| Duplicados         | Sí ✅         | Sí ✅         | No ❌       |

💡 **Tip:** Piensa: ¿Necesito cambiarlo? (Lista/Set). ¿Necesita orden? (Lista/Tupla). ¿Necesito que sea único? (Set).

---
## ⚡ Repaso Relámpago ⚡
- **Listas**: Flexibles, ordenadas, mutables, permiten duplicados. `[1, 2, 2]`
- **Tuplas**: Fijas, ordenadas, inmutables, permiten duplicados. `(1, 2, 2)`
- **Sets**: Únicos, desordenados, mutables, no permiten duplicados. `{1, 2}`
- Elige según si necesitas modificar, mantener un orden o asegurar elementos únicos.