---
aliases:
  - Módulos 🐍
tags:
  - python
  - modulos
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-06-02
---
# Módulos en Python 🐍
- [[#1. ¿Qué son los Módulos? 🤔|1. ¿Qué son los Módulos? 🤔]]
- [[#2. ¿Para qué sirven los Módulos? 💡|2. ¿Para qué sirven los Módulos? 💡]]
- [[#3. ¿Cómo usar Módulos? 🛠️|3. ¿Cómo usar Módulos? 🛠️]]
- [[#4. Módulos Populares (Algunos ejemplos) ✨|4. Módulos Populares (Algunos ejemplos) ✨]]
- [[#5. Creando nuestros propios Módulos ✍️|5. Creando nuestros propios Módulos ✍️]]
- [[#Mini Repaso Final 📝|Mini Repaso Final 📝]]

---
## 1. ¿Qué son los Módulos? 🤔
Imagina que estás construyendo algo grande con LEGOs. En lugar de tener todas las piezas sueltas y mezcladas, las organizas en cajitas temáticas: una caja para ruedas, otra para ventanas, otra para bloques rojos, etc.
- En Python, un **módulo** es básicamente eso: un **archivo** que contiene **definiciones y declaraciones de Python**.
- Estas definiciones pueden ser **funciones**, **clases** o **variables**.
- Piénsalo como una **caja de herramientas** 🔧 o una **librería** 📚 que puedes usar en tus programas.
- El nombre del archivo es el nombre del módulo con el sufijo `.py`.
---
## 2. ¿Para qué sirven los Módulos? 💡
Los módulos son súper útiles por varias razones:
- **Organización del código**: Ayudan a dividir programas grandes en archivos más pequeños y manejables. ¡Adiós al código espagueti! 🍝
- **Reutilización de código**: Puedes escribir un conjunto de funciones útiles en un módulo y luego usarlas en muchos programas diferentes sin tener que copiar y pegar. DRY (Don't Repeat Yourself - No te repitas).
- **Evitar conflictos de nombres**: Como cada módulo tiene su propio "espacio de nombres" (namespace), puedes tener funciones o variables con el mismo nombre en diferentes módulos sin que choquen entre sí.
- **Colaboración**: Diferentes personas pueden trabajar en diferentes módulos de un proyecto grande de forma más sencilla.
- **Funcionalidad extendida**: Python viene con una **biblioteca estándar** (Standard Library) llena de módulos que ofrecen un montón de herramientas listas para usar (como `math` para operaciones matemáticas, `datetime` para fechas y horas, `json` para trabajar con datos JSON, etc.). Además, ¡hay miles de módulos de terceros que puedes instalar!
---
## 3. ¿Cómo usar Módulos? 🛠️
Para usar las herramientas (funciones, clases, variables) que hay dentro de un módulo, primero necesitas "traerlo" a tu programa actual. Esto se hace con la palabra clave **`import`**.
Hay varias formas de hacerlo:
- **`import nombre_del_modulo`**:
    - Esto importa el módulo completo.
    - Para usar algo del módulo, tienes que escribir `nombre_del_modulo.nombre_de_la_funcion()`.
    - Ejemplo:
        ```python
        import math
        print(math.pi)  # Imprime el valor de pi
        print(math.sqrt(16))  # Imprime 4.0
        ```
- **`from nombre_del_modulo import algo_especifico`**:
    - Esto importa solo una función, variable o clase específica del módulo.
    - Puedes usar `algo_especifico` directamente sin el prefijo del nombre del módulo.
    - Ejemplo:
        ```python
        from math import pi, sqrt
        print(pi)
        print(sqrt(25)) # Imprime 5.0
        ```
- **`from nombre_del_modulo import algo_especifico as alias`**:
    - Si el nombre de la función o módulo es muy largo o quieres usar un nombre diferente, puedes usar un **alias** con `as`.
    - Ejemplo con módulo:
        ```python
        import math as m
        print(m.pi)
        print(m.sqrt(36)) # Imprime 6.0
        ```
    - Ejemplo con función específica:
        ```python
        from math import factorial as fact
        print(fact(5)) # Imprime 120
        ```
- **`from nombre_del_modulo import *`**:
    - Esto importa **todo** lo que contiene el módulo directamente al espacio de nombres actual.
    - **¡Cuidado!** ☢️ Generalmente **no se recomienda** usar esta forma porque puede causar **conflictos de nombres** si alguna función o variable del módulo tiene el mismo nombre que algo en tu propio código, y hace más difícil saber de dónde viene cada cosa. Es mejor ser explícito.
---
## 4. Módulos Populares (Algunos ejemplos) ✨
Python tiene muchísimos módulos útiles. Aquí algunos ejemplos:

| **Módulo**     | **Descripción**                                              | **Ejemplo de uso (conceptual)**                 |
| -------------- | ------------------------------------------------------------ | ----------------------------------------------- |
| **`math`**     | Funciones matemáticas (seno, coseno, raíz, etc.)             | `math.sqrt(x)`, `math.sin(angle)`               |
| **`random`**   | Generación de números aleatorios                             | `random.randint(a, b)`, `random.choice(lista)`  |
| **`datetime`** | Manipulación de fechas y horas                               | `datetime.date.today()`, `datetime.timedelta()` |
| **`os`**       | Interacción con el sistema operativo (archivos, directorios) | `os.getcwd()`, `os.listdir()`                   |
| **`sys`**      | Parámetros y funciones específicas del sistema               | `sys.argv`, `sys.exit()`                        |
| **`json`**     | Codificación y decodificación de datos JSON                  | `json.dumps(data)`, `json.loads(string_json)`   |
| **`requests`** | (De terceros) Para hacer solicitudes HTTP                    | `requests.get(url)`                             |
| **`numpy`**    | (De terceros) Computación numérica, arrays                   | `numpy.array([1, 2, 3])`                        |
| **`pandas`**   | (De terceros) Análisis y manipulación de datos               | `pandas.DataFrame(data)`                        |

---
## 5. Creando nuestros propios Módulos ✍️
¡También puedes crear tus propios módulos! Es súper fácil:
1. Crea un archivo Python (con extensión `.py`). Por ejemplo, `mimodulo.py`.
2. Escribe tus funciones, clases o variables dentro de ese archivo.
    ```python
    # mimodulo.py
    def saludar(nombre):
        return f"¡Hola, {nombre}!"
    
    PI_APROXIMADO = 3.14
    ```
3. Guarda el archivo.
4. En otro archivo Python (en la misma carpeta, para empezar), puedes importarlo:
```python
# programa_principal.py
import mimodulo
    
print(mimodulo.saludar("Mundo"))
print(f"Valor de PI aproximado: {mimodulo.PI_APROXIMADO}")
    
# O también:
# from mimodulo import saludar
# print(saludar("Ana"))
```
¡Y listo! Ya tienes tu propio módulo reutilizable. 🎉

---
## Mini Repaso Final 📝
- Un **módulo** es un archivo `.py` con definiciones de Python (funciones, clases, variables).
- Sirven para **organizar** y **reutilizar** código.
- Se usan con la palabra clave **`import`**.
    - `import modulo` (luego usas `modulo.funcion()`)
    - `from modulo import funcion` (luego usas `funcion()` directamente)
    - `import modulo as m` (usas un alias `m.funcion()`)
- Python tiene una gran **biblioteca estándar** de módulos y muchos más de terceros.
- Puedes **crear tus propios módulos** fácilmente.