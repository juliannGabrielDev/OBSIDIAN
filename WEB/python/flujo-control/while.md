---
aliases:
  - 🐍 Bucles while en Python 🐍
tags:
  - python
  - flujo-control
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-05-31
---
# 🐍 Bucles `while` en Python 🐍
---
## 1. ¿Qué es un Bucle `while`? 🤔
Un bucle `while` es como decirle a Python: "**Mientras** esta condición sea verdadera, sigue haciendo esto". Repite un bloque de código una y otra vez, ¡mientras la condición se cumpla!
- **Idea Clave**: Repetición basada en una condición.
- Se usa cuando **no sabes exactamente cuántas veces** necesitas repetir algo, solo hasta que algo cambie.
📌 **Tip**: Piensa en "mientras tengas sed, sigue bebiendo agua".
---
## 2. ¿Cómo Funciona? ⚙️
El bucle `while` sigue estos pasos:
1. Verifica si una **condición** es `True` (verdadera).
2. Si es `True`, **ejecuta el código** dentro del bucle.
3. Vuelve al paso 1.
4. Si la condición es `False` (falsa), el bucle termina y el programa continúa con lo que sigue.
- **Componentes Esenciales**:
    - **Inicialización**: A menudo necesitas una variable antes del bucle.
    - **Condición**: La expresión que se evalúa.
    - **Bloque de código**: Las instrucciones a repetir.
    - **Actualización**: Algo dentro del bucle debe eventualmente hacer que la condición sea `False` (¡sino, bucle infinito!).
🧠 **Tip**: "Verifica, ejecuta, repite... ¡hasta que no más!"
---
## 3. Mini Ejemplo Práctico 💡
Imaginemos que queremos contar del 1 al 3:
```python
contador = 1  # 1. Inicialización
while contador <= 3:  # 2. Condición
    print(f"Número: {contador}")  # 3. Bloque de código
    contador = contador + 1  # 4. Actualización
print("¡Terminó el conteo!")
```
**Salida:**
```
Número: 1
Número: 2
Número: 3
¡Terminó el conteo!
```
- Aquí, `contador <= 3` es la condición.
- `contador = contador + 1` asegura que el bucle eventualmente termine.
📘 **Tip**: ¡La actualización es crucial para no quedarte atrapado!
---
## 🎓 Repaso Relámpago

Los bucles `while` son geniales para repetir código mientras una condición sea verdadera. Recuerda **inicializar**, tener una **condición** clara y, súper importante, **actualizar** algo para que el bucle pueda terminar. ¡Evita los bucles infinitos!