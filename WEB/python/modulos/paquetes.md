---
aliases:
  - 📦 Paquetes
tags:
  - python
  - modulos
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-06-02
---
# 📦 Paquetes en Python
- [[#1. ¿Qué son los Paquetes? 📂|1. ¿Qué son los Paquetes? 📂]]
- [[#2. Estructura Típica 🏗️|2. Estructura Típica 🏗️]]
- [[#3. ¿Para qué sirve `__init__.py`? 🤔|3. ¿Para qué sirve `__init__.py`? 🤔]]
- [[#4. ¿Cómo se Usan? 🚀|4. ¿Cómo se Usan? 🚀]]
- [[#✨ Repaso Relámpago|✨ Repaso Relámpago]]

---
## 1. ¿Qué son los Paquetes? 📂
Imagina que tienes muchos módulos (archivos `.py`) relacionados. Un **paquete** es simplemente una **carpeta** que contiene esos módulos y un archivo especial llamado `__init__.py`. ¡Es como organizar tus apuntes en diferentes carpetas temáticas!
- Agrupan módulos relacionados.
- Ayudan a evitar conflictos de nombres entre módulos.
- Permiten una estructura de proyecto más ordenada.

📌 **Tip:** ¡Piensa en ellos como las carpetas de tu explorador de archivos, pero para código!

---
## 2. Estructura Típica 🏗️
Un paquete básico se ve así:
```
mi_paquete/            <-- Carpeta principal del paquete
    __init__.py        <-- Archivo especial que lo define como paquete
    modulo1.py         <-- Un módulo dentro del paquete
    modulo2.py         <-- Otro módulo
    sub_paquete/       <-- Puede tener sub-paquetes (otras carpetas)
        __init__.py
        modulo_sub.py
```
- La clave es la carpeta y el archivo `__init__.py`.
- Puedes tener módulos sueltos o más sub-carpetas (sub-paquetes) dentro.
🧠 **Tip:** La organización es tu amiga, ¡especialmente en proyectos grandes!

---
## 3. ¿Para qué sirve `__init__.py`? 🤔
Este archivo, aunque puede estar vacío, hace varias cosas importantes:
- **Le dice a Python:** "¡Oye, esta carpeta es un paquete!"
- Puede contener código de inicialización para el paquete (se ejecuta cuando importas el paquete).
- Puedes definir la variable `__all__` para controlar qué se importa con `from mi_paquete import *`.
- Puede hacer que los submódulos o funciones específicas estén disponibles directamente al importar el paquete.
**Ejemplo Mini en `mi_paquete/__init__.py`:**
```python
# mi_paquete/__init__.py
print("¡Paquete 'mi_paquete' importado!")
from .modulo1 import una_funcion  # Hace una_funcion accesible como mi_paquete.una_funcion
```
🧐 **Tip:** Aunque `__init__.py` puede estar vacío, ¡no te olvides de él!

---
## 4. ¿Cómo se Usan? 🚀
Para usar un módulo dentro de un paquete, usas la notación de punto.
- Importar un módulo específico: `import mi_paquete.modulo1`
- Luego accedes a sus funciones/variables: `mi_paquete.modulo1.nombre_funcion()`
- O importar algo específico del módulo: `from mi_paquete.modulo1 import nombre_funcion`
- Entonces la usas directamente: `nombre_funcion()`
**Ejemplo Mini:**
Si mi_paquete/modulo1.py tiene def saludar(): print("¡Hola!")
```python
import mi_paquete.modulo1
mi_paquete.modulo1.saludar()  # Output: ¡Hola!

from mi_paquete.modulo1 import saludar
saludar()  # Output: ¡Hola!
```
🛠️ **Tip:** ¡La notación de punto `.` es tu guía para navegar dentro del paquete!

---
## ✨ Repaso Relámpago
- Un paquete es una **carpeta** con módulos y un `__init__.py`.
- `__init__.py` **identifica** la carpeta como paquete y puede inicializar cosas.
- Se usan para **organizar** código y se accede con `paquete.modulo`.