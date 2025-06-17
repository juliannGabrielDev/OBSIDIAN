---
aliases:
  - "🐍 Excepciones en Python: ¡Que no te agarren en curva!"
tags:
  - python
  - flujo-control
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-05-31
---
# 🐍 Excepciones en Python: ¡Que no te agarren en curva!
---
## 1. ¿Qué son las Excepciones? 🤔
Son **errores** que ocurren mientras tu programa se está ejecutando. ¡Ups! 💥 Cuando Python se topa con algo que no puede manejar, lanza una excepción y, si no la cachas, ¡tu programa se detiene!
- 📌 **Error en tiempo de ejecución**: No es un error de sintaxis (esos los ves antes).
- 📌 **Interrumpen el flujo**: El programa para de golpe si no se manejan.
- 📌 **Objetos**: En Python, las excepciones son objetos que representan el error.
💡 **Tip:** Piensa en ellas como "alertas" de que algo inesperado pasó.
---
## 2. ¿Por qué Manejar Excepciones? 🛡️
Porque queremos que nuestros programas sean **robustos** y no se rompan por cualquier cosita. Manejarlas te permite controlar el error y decidir qué hacer.
- 📌 **Evitar crashes**: ¡Que el programa no muera!
- 📌 **Mensajes amigables**: Mostrar al usuario qué pasó de forma entendible.
- 📌 **Continuar ejecución**: A veces, puedes corregir el problema o seguir con otra tarea.
✨ **Tip:** ¡Manejar excepciones es como tener un plan B para tu código!
---
## 3. El Bloque `try-except` 🧱
¡Esta es la herramienta clave! Pones el código que podría fallar dentro del bloque `try`. Si ocurre una excepción, el código dentro del bloque `except` se ejecuta.
- 📌 **`try`**: Aquí va el código "riesgoso".
- 📌 **`except`**: Si ocurre el error esperado en `try`, se ejecuta este bloque. Puedes especificar qué tipo de excepción atrapar.
**Ejemplo Mini:**
```python
try:
  resultado = 10 / 0 # ¡Oh, oh! División por cero
except ZeroDivisionError:
  print("🧠 ¡Oye! No puedes dividir entre cero.")
```
💪 **Tip:** `try` es "intentar" y `except` es "atrapar" el error si falla.

---
## 4. Excepciones Comunes 📋
Hay muchos tipos de excepciones, ¡pero algunas son las más populares!
- 📌 **`ZeroDivisionError`**: Intentar dividir por cero. `10 / 0`
- 📌 **`TypeError`**: Operación con un tipo de dato incorrecto. `"hola" + 5`
- 📌 **`NameError`**: Usar una variable que no existe. `print(mi_variable_fantasma)`
- 📌 **`FileNotFoundError`**: Intentar abrir un archivo que no se encuentra.
- 📌 **`ValueError`**: Función recibe argumento con tipo correcto pero valor inapropiado. `int("abc")`
- 📌 **`IndexError`**: Acceder a un índice fuera de rango en una lista. `lista = [1,2]; print(lista[5])`
🧐 **Tip:** Conocer las comunes te ayuda a anticipar problemas.
---
## 5. Cláusulas `else` y `finally` ✨
¡Hay más! Estas cláusulas te dan aún más control.
- 📌 **`else`**: Este bloque se ejecuta **solo si no hubo ninguna excepción** en el bloque `try`.
- 📌 **`finally`**: Este bloque se ejecuta **siempre**, haya o no haya excepción. ¡Es perfecto para limpiar recursos!
**Ejemplo Mini:**
```python
try:
  num = int(input("Dime un número: "))
except ValueError:
  print("🧠 ¡Eso no es un número válido!")
else:
  print(f"¡Bien! Tu número es {num}.")
finally:
  print("✨ ¡Bloque finally ejecutado!")
```
🧹 **Tip:** `finally` es tu "siempre haré esto", ideal para cerrar archivos o conexiones.

---
## ⚡ Repaso Relámpago ⚡
- Las excepciones son **errores en ejecución**.
- Se manejan con **`try-except`** para evitar que el programa se detenga.
- `else` se ejecuta si **no hay error**.
- `finally` se ejecuta **siempre**.
- Conocer excepciones comunes ayuda a **prevenir fallos**.