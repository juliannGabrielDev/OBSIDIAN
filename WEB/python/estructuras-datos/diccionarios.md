---
aliases:
  - 📖 Diccionarios en Python
tags:
  - python
  - estructuras
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-05-31
---
# 📖 Diccionarios en Python: ¡Tus Datos Bien Organizados!
---
## 1. ¿Qué son los Diccionarios? 🔑
Imagina un diccionario de verdad. Tienes una **palabra (clave)** y su **definición (valor)**. ¡En Python es igual! Son colecciones que almacenan datos en pares `clave: valor`.
- 📌 **Pares Clave-Valor**: Cada elemento tiene una clave única y un valor asociado.
- 📌 **Mutables**: Puedes agregar, modificar o eliminar pares después de crearlos.
- 📌 **Claves Únicas**: No puede haber dos claves iguales. Si intentas añadir una clave que ya existe, actualizas su valor.
- 📌 **Ordenados (desde Python 3.7+)**: Antes eran desordenados, pero ahora mantienen el orden de inserción. ¡Qué bien!
- 📌 **Sintaxis**: Se definen con llaves `{}` y los pares separados por comas: `{"clave1": valor1, "clave2": valor2}`.
**Ejemplo Mini:**
```python
alumno = {
    "nombre": "Carlos",
    "edad": 22,
    "curso": "Python Avanzado",
    "activo": True
}
print(alumno) # {'nombre': 'Carlos', 'edad': 22, 'curso': 'Python Avanzado', 'activo': True}
```
🧠 **Tip:** Piensa en ellos como etiquetas (claves) para tus datos (valores). ¡Fácil de buscar!

---
## 2. Características Principales 🌟
Los diccionarios son muy potentes por estas razones:
- 📌 **Acceso Rápido**: Accedes a los valores directamente por su clave, ¡es muy eficiente!
- 📌 **Flexibles**: Los valores pueden ser de cualquier tipo (números, strings, listas, ¡incluso otros diccionarios!).
- 📌 **Legibles**: Hacen tu código más entendible porque las claves describen los datos.
**Ejemplo con claves diversas:**
```python
config = {
    "usuario_id": 123,
    "preferencias": ["dark_mode", "notificaciones"],
    1: "dato_con_clave_numerica"
}
```
✨ **Tip:** ¡Son ideales para representar objetos del mundo real o configuraciones!

---
## 3. Operaciones Comunes 🛠️
Puedes hacer muchas cosas con los diccionarios:
- 📌 **Acceder a un valor**: Usando la clave entre corchetes `mi_diccionario["clave"]` o con el método `get("clave", "valor_por_defecto")`.
- 📌 **Añadir/Modificar**: `mi_diccionario["nueva_clave"] = nuevo_valor`. Si la clave existe, se modifica; si no, se añade.
- 📌 **Eliminar un par**: Con `del mi_diccionario["clave"]` o el método `pop("clave")` (que además devuelve el valor eliminado).
- 📌 **Verificar si existe una clave**: Usando el operador `in`: `"clave" in mi_diccionario`.
- 📌 **Obtener todas las claves**: `mi_diccionario.keys()`
- 📌 **Obtener todos los valores**: `mi_diccionario.values()`
- 📌 **Obtener todos los pares (clave, valor)**: `mi_diccionario.items()`
**Ejemplo Mini de Operaciones:**
```python
perfil = {"user": "AnaG", "followers": 150}

# Acceder
print(perfil["user"])  # AnaG
print(perfil.get("location", "Desconocida")) # Desconocida

# Añadir / Modificar
perfil["following"] = 50
perfil["followers"] = 155
print(perfil) # {'user': 'AnaG', 'followers': 155, 'following': 50}

# Eliminar
del perfil["following"]
print(perfil) # {'user': 'AnaG', 'followers': 155}
```
💪 **Tip:** El método `.get()` es genial porque evita errores si la clave no existe, ¡puedes darle un valor por defecto!

---
## 4. Iterar sobre Diccionarios 🔄
Recorrer los elementos de un diccionario es muy común.
- 📌 **Iterar sobre claves (por defecto)**:
    ```python
    for k in mi_diccionario:
        print(k, mi_diccionario[k])
    ```
- 📌 **Iterar sobre valores**:
    ```python
    for v in mi_diccionario.values():
        print(v)
    ```
- 📌 **Iterar sobre pares clave-valor (items)**: ¡Esta es la más usada!
    ```python
    for clave, valor in mi_diccionario.items():
        print(f"La clave {clave} tiene el valor {valor}")
    ```
**Ejemplo Mini de Iteración:**
```python
notas = {"matematicas": 9, "historia": 7, "ingles": 10}
for materia, calificacion in notas.items():
    print(f"En {materia} sacaste un {calificacion} 🥳")
```
🚶‍♀️ **Tip:** Usar `.items()` para iterar es súper claro y te da la clave y el valor de una vez.

---
## ⚡ Repaso Relámpago ⚡
- Los diccionarios almacenan datos como **pares `clave: valor`**.
- Son **mutables** y (desde Python 3.7+) **ordenados** por inserción.
- Las **claves deben ser únicas** e inmutables (strings, números, tuplas).
- Se accede, modifica y elimina usando las **claves**.
- Métodos útiles: `.get()`, `.keys()`, `.values()`, `.items()`, `.pop()`.