---
aliases:
  - 📦 *args y **kwargs
tags:
  - python
  - funciones
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-05-31
---
# 📦 `*args` y `**kwargs` en Python: ¡Argumentos Flexibles al Máximo!

---

## 1. ¿Para qué Sirven? 🤔 (El Problema que Resuelven)

A veces, cuando defines una función, no sabes de antemano cuántos argumentos va a recibir. `*args` y `**kwargs` te permiten crear funciones que pueden aceptar un **número variable de argumentos**.

- 📌 **Flexibilidad**: Tu función se adapta a diferentes cantidades de datos de entrada.
- 📌 **Evitar errores**: No tienes que definir N versiones de una función para N argumentos.
- 📌 **Reutilización**: Puedes crear funciones más genéricas.

💡 **Tip:** Piensa en ellos como "cajas mágicas" que recogen todos los argumentos "extra" que le pases a tu función.

---

## 2. `*args` (Argumentos Posicionales Variables) 🌟

`*args` te permite pasar un número variable de **argumentos posicionales** a una función. Dentro de la función, `args` se convierte en una **tupla** que contiene todos esos argumentos posicionales extra.

- 📌 **El asterisco `*`**: Es el que hace la magia. `args` es solo un nombre convencional (podrías usar `*numeros`, `*params`, etc.).
- 📌 **Tupla**: Los argumentos se agrupan en una tupla.
- 📌 **Posicionales**: Son los argumentos que se pasan sin un nombre de palabra clave.

**Ejemplo Mini:**
```python
def listar_compras(articulo_fijo, *extras): # 'extras' será una tupla
  print(f"Necesito comprar: {articulo_fijo}")
  if extras:
    print("Y también:")
    for item in extras:
      print(f"- {item}")

listar_compras("Leche")
# Necesito comprar: Leche

listar_compras("Pan", "Huevos", "Jugo", "Cereal")
# Necesito comprar: Pan
# Y también:
# - Huevos
# - Jugo
# - Cereal
```

✨ **Tip:** `*args` es como decir "y todo lo demás que venga después, en orden".

---

## 3. `**kwargs` (Argumentos de Palabra Clave Variables) 🔑📖

`**kwargs` te permite pasar un número variable de **argumentos de palabra clave** (o nombrados) a una función. Dentro de la función, `kwargs` se convierte en un **diccionario**.

- 📌 **Los dos asteriscos `**`**: Indican que se trata de argumentos de palabra clave. `kwargs` es una convención (podrías usar `**opciones`, `**datos_usuario`).
- 📌 **Diccionario**: Las claves son los nombres de los argumentos y los valores son sus... ¡valores!
- 📌 **De Palabra Clave**: Son los argumentos que se pasan como `nombre=valor`.

**Ejemplo Mini:**
```python
def crear_perfil_usuario(**info_usuario): # 'info_usuario' será un diccionario
  print("\n--- Perfil del Usuario ---")
  if not info_usuario:
    print("Sin información adicional.")
    return
  for clave, valor in info_usuario.items():
    print(f"{clave.replace('_', ' ').capitalize()}: {valor}")

crear_perfil_usuario()
# --- Perfil del Usuario ---
# Sin información adicional.

crear_perfil_usuario(nombre="Elena", edad=28, ciudad="Valencia", profesion="Diseñadora")
# --- Perfil del Usuario ---
# Nombre: Elena
# Edad: 28
# Ciudad: Valencia
# Profesion: Diseñadora
```

🧠 **Tip:** `**kwargs` es como decir "y cualquier otra cosa que me pases con nombre=valor".

---

## 4. Usándolos Juntos y el Orden Importa ✨

Puedes usar `*args` y `**kwargs` en la misma función, junto con argumentos normales. ¡Pero el orden en la definición de la función es crucial!

- 📌 **Orden Correcto**:
    1. Argumentos posicionales normales.
    2. `*args`.
    3. Argumentos de palabra clave normales (si los hay después de `*args`).
    4. `**kwargs`.

**Ejemplo Mini (Orden Completo):**
```python
def funcion_completa(nombre, apellido, *telefonos, email="N/A", **detalles_adicionales):
  print(f"Nombre Completo: {nombre} {apellido}")
  print(f"Email: {email}")
  if telefonos:
    print("Teléfonos:")
    for tel in telefonos:
      print(f"- {tel}")
  if detalles_adicionales:
    print("Detalles Adicionales:")
    for k, v in detalles_adicionales.items():
      print(f"- {k}: {v}")

funcion_completa("Juan", "Pérez", "555-1234", "555-5678", email="juan@example.com", profesion="Músico", hobby="Ajedrez")
# Nombre Completo: Juan Pérez
# Email: juan@example.com
# Teléfonos:
# - 555-1234
# - 555-5678
# Detalles Adicionales:
# - profesion: Músico
# - hobby: Ajedrez

funcion_completa("Ana", "Gomez", profesion="Doctora") # No hay *args, email toma valor por defecto
# Nombre Completo: Ana Gomez
# Email: N/A
# Detalles Adicionales:
# - profesion: Doctora
```

🚦 **Tip:** ¡Recuerda la secuencia: `normales, *args, **kwargs` para no tener errores!

---

## 5. Desempaquetando con `*` y `**` al Llamar 🎁

Los operadores `*` y `**` también se usan al **llamar** a una función para "desempaquetar" elementos de una lista/tupla o un diccionario como argumentos.

- 📌 `*iterable`: Desempaqueta una lista o tupla en argumentos posicionales.
- 📌 `**diccionario`: Desempaqueta un diccionario en argumentos de palabra clave.

**Ejemplo Mini:*
```python
def saludar(mensaje, nombre, emojis="👋"):
  print(f"{mensaje}, {nombre}! {emojis}")

saludo_lista = ["Hola", "Carlos"]
saludar(*saludo_lista) # Hola, Carlos! 👋

saludo_dict = {"mensaje": "Qué tal", "nombre": "Luisa", "emojis": "😊🎉"}
saludar(**saludo_dict) # Qué tal, Luisa! 😊🎉

# Combinado
datos_persona = ["David", 30]
info_extra = {"ciudad": "Bogotá", "profesion": "Chef"}

def ficha_persona(nombre, edad, **otros_datos):
    print(f"{nombre} ({edad} años)")
    for k,v in otros_datos.items():
        print(f"  {k}: {v}")

ficha_persona(*datos_persona, **info_extra)
# David (30 años)
#   ciudad: Bogotá
#   profesion: Chef
```

🎉 **Tip:** ¡El desempaquetado es súper útil cuando tienes los argumentos en una estructura de datos!

---

## ⚡ Repaso Relámpago ⚡

- `*args`: Recoge argumentos **posicionales** extra en una **tupla**.
- `**kwargs`: Recoge argumentos de **palabra clave** extra en un **diccionario**.
- **Orden en definición**: `normales, *args, (keyword_only_args,) **kwargs`.
- **Desempaquetado al llamar**: `*` para listas/tuplas, `**` para diccionarios.
- Permiten crear funciones **flexibles** y adaptables.