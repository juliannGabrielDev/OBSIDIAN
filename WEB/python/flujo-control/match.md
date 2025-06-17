---
aliases:
  - 🎯 Declaraciones match-case en Python 🎯
tags:
  - python
  - flujo-control
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-05-31
---
# 🎯 Declaraciones `match-case` en Python 🎯
---
## 1. ¿Qué es `match-case`? 🤔

La estructura `match-case` (introducida en Python 3.10) te permite comparar un valor (el "sujeto") contra varios "patrones" (los `case`). Es una forma más expresiva y potente de manejar múltiples condiciones, especialmente cuando necesitas verificar la _estructura_ de tus datos, no solo valores simples.
- **Idea Clave**: Compara un sujeto con diferentes patrones hasta que uno coincide.
- Es como un `if-elif-else` pero con superpoderes para desglosar datos.
📌 **Tip**: Piensa en ello como un clasificador: "Si el objeto se parece a ESTO, haz A; si se parece a AQUELLO, haz B".
---
## 2. ¿Cómo Funciona? ⚙️
1. Tomas un valor, llamado el **sujeto**, y lo pones después de `match`.
2. Luego, defines varios bloques `case`, cada uno con un **patrón**.
3. Python revisa los `case` en orden. El primer patrón que **coincida** con el sujeto se ejecuta.
4. Si ningún patrón coincide, se puede usar un `case _:` (guion bajo) como un "comodín" o caso por defecto, ¡similar al `else`!
- **Componentes Esenciales**:
    - `match sujeto:`: La expresión que quieres evaluar.
    - `case patron_1:`: Un patrón a comparar. Si coincide, se ejecuta su bloque.
    - `case patron_2:`: Otro patrón.
    - `case _:`: (Opcional) El comodín, si nada más coincide.
🧠 **Tip**: "Compara el sujeto, encuentra el primer patrón que calce, ¡y actúa!"
---
## 3. Mini Ejemplo Práctico (Simple) 💡

Imagina que tienes un código de estado HTTP y quieres imprimir un mensaje:
```python
status_code = 404

match status_code:
    case 200:
        print("Todo OK 👍")
    case 404:
        print("No encontrado 🤷‍♀️")
    case 500:
        print("Error del servidor 💥")
    case _:  # Caso por defecto
        print("Código desconocido")

# Salida: No encontrado 🤷‍♀️
```
- Aquí, `status_code` es el sujeto.
- `200`, `404`, `500` son patrones literales.
📘 **Tip**: ¡Mucho más legible que un montón de `if-elif-else` para estos casos!
---

## 4. Un Poco Más Avanzado: Patrones 🧩
Lo genial de `match-case` es que los patrones pueden ser más que solo valores:
- **Literales**: Como `200` o `"hola"`.
- **Variables (Captura)**: `case x:` captura el valor en `x`.
- **Secuencias**: `case [x, y]:` coincide con una lista/tupla de dos elementos y los captura.
- **Mapas (Diccionarios)**: `case {"nombre": nombre, "edad": edad}:` coincide con un diccionario con esas claves.
- **Condiciones (Guardas)**: `case x if x > 0:` coincide si `x` es mayor que 0.
**Ejemplo con captura y guarda:**
```python
punto = (3, 0) # Una tupla representando (x, y)

match punto:
    case (0, 0):
        print("En el origen 📍")
    case (x, 0) if x > 0: # Captura 'x' y añade una condición 'if'
        print(f"En el eje X positivo, en x={x}")
    case (0, y):
        print(f"En el eje Y, en y={y}")
    case (x, y):
        print(f"En otro lugar: x={x}, y={y}")
    case _:
        print("No es un punto válido")

# Salida: En el eje X positivo, en x=3
```
🌟 **Tip**: ¡Los patrones te permiten "desempacar" y verificar estructuras de datos de forma elegante!

---
## 🎓 Repaso Relámpago

`match-case` es una herramienta poderosa en Python (3.10+) para comparar un valor "sujeto" con una serie de "patrones". Ejecuta el código del primer patrón que coincida. Es excelente para simplificar la lógica condicional compleja y para trabajar con la estructura de los datos. ¡No olvides el caso comodín `case _:` si lo necesitas!