---
aliases:
  - ⛓️ Métodos de Cadena
tags:
  - python
  - basico
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-06-02
---
# ⛓️ Métodos de Cadena en Python
- [[#1. ¿Qué son los Métodos de Cadena? 🤔|1. ¿Qué son los Métodos de Cadena? 🤔]]
- [[#2. ¡Tabla de Métodos Populares! 📝|2. ¡Tabla de Métodos Populares! 📝]]
- [[#✨ Repaso Relámpago|✨ Repaso Relámpago]]

---
## 1. ¿Qué son los Métodos de Cadena? 🤔
Los **métodos de cadena** son como herramientas especiales que vienen con cada string (cadena de texto) en Python. Te permiten realizar un montón de operaciones y transformaciones sobre esas cadenas de forma sencilla.
- Se llaman usando la **notación de punto**: `mi_cadena.metodo()`.
- ¡Importante! Las cadenas son **inmutables**. Esto significa que los métodos no cambian la cadena original; en su lugar, **devuelven una nueva cadena** con el resultado.
📌 **Tip:** ¡Piensa en ellos como funciones integradas exclusivas para strings!

---
## 2. ¡Tabla de Métodos Populares! 📝
Aquí tienes algunos de los métodos más usados. ¡Son súper útiles!

| **Método**         | **Descripción Breve**                                                | **Mini Ejemplo (cad = " holaMundo! ")**     |
| ------------------ | -------------------------------------------------------------------- | ------------------------------------------- |
| `upper()`          | Convierte todo a MAYÚSCULAS.                                         | `cad.upper()` → `" HOLAMUNDO! "`            |
| `lower()`          | Convierte todo a minúsculas.                                         | `cad.lower()` → `" holamundo! "`            |
| `capitalize()`     | Primera letra mayúscula, resto minúsculas.                           | `cad.strip().capitalize()` → `"Holamundo!"` |
| `title()`          | Primera letra de cada palabra en mayúscula.                          | `"hola mundo".title()` → `"Hola Mundo"`     |
| `strip()`          | Elimina espacios al inicio y al final.                               | `cad.strip()` → `"holaMundo!"`              |
| `lstrip()`         | Elimina espacios al inicio (izquierda).                              | `cad.lstrip()` → `"holaMundo! "`            |
| `rstrip()`         | Elimina espacios al final (derecha).                                 | `cad.rstrip()` → `" holaMundo!"`            |
| `split(sep)`       | Divide la cadena en una lista, usando `sep` como separador.          | `"a,b,c".split(',')` → `['a', 'b', 'c']`    |
| `join(iterable)`   | Une los elementos de un iterable con la cadena.                      | `"-".join(["x","y"])` → `"x-y"`             |
| `replace(old,new)` | Reemplaza todas las ocurrencias de `old` por `new`.                  | `cad.replace("o", "0")` → `" h0laMund0! "`  |
| `find(sub)`        | Devuelve el índice de la primera aparición de `sub` (-1 si no está). | `cad.find("Mun")` → `6`                     |
| `index(sub)`       | Igual que `find()`, pero da error si no encuentra.                   | `cad.index("Mun")` → `6`                    |
| `startswith(pref)` | `True` si la cadena empieza con `pref`.                              | `cad.strip().startswith("hola")` → `True`   |
| `endswith(suff)`   | `True` si la cadena termina con `suff`.                              | `cad.strip().endswith("!")` → `True`        |
| `isdigit()`        | `True` si todos los caracteres son dígitos.                          | `"123".isdigit()` → `True`                  |
| `isalpha()`        | `True` si todos son letras (y no está vacía).                        | `"abc".isalpha()` → `True`                  |
| `isalnum()`        | `True` si todos son alfanuméricos (y no está vacía).                 | `"Python3".isalnum()` → `True`              |
| `isspace()`        | `True` si todos son espacios en blanco.                              | `" ".isspace()` → `True`                    |

🧠 **Tip:** ¡Experimenta con ellos! La mejor forma de aprenderlos es usándolos.

---
## ✨ Repaso Relámpago
- Los métodos de cadena son **funciones** para manipular strings.
- Se usa `cadena.metodo()`.
- Devuelven una **nueva cadena** (son inmutables).
- Hay muchísimos, ¡esta tabla solo es el comienzo!