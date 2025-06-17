---
aliases:
  - 🏛️ Clases
tags:
  - python
  - poo
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-05-31
---
# 🏛️ Clases en Python: ¡Construyendo tus Propios Tipos de Datos!

---

## 1. ¿Qué son las Clases? (El Molde Blueprint) 📝

Una **clase** es como un **molde** o un **plano** para crear objetos. Define un conjunto de **atributos** (datos o características) y **métodos** (acciones o comportamientos) que tendrán los objetos creados a partir de ella.

- 📌 **Molde/Plantilla**: Define la estructura y el comportamiento.
- 📌 **Atributos**: Son las variables que almacenan datos sobre el objeto (ej: color, tamaño, nombre).
- 📌 **Métodos**: Son las funciones que definen lo que el objeto puede hacer (ej: correr, saltar, calcular).
- 📌 **Abstracción**: Permiten modelar cosas del mundo real o conceptos.

💡 **Tip:** Piensa en una clase como la receta para hacer galletas. La receta es la clase.

---

## 2. ¿Qué son los Objetos? (La Creación 🍪)

Un **objeto** (también llamado **instancia**) es una **creación específica** a partir de una clase. Si la clase es el molde de galletas, cada galleta que haces con ese molde es un objeto.

- 📌 **Instancia de una Clase**: Una "copia" concreta del molde.
- 📌 **Estado Propio**: Cada objeto tiene sus propios valores para los atributos definidos en la clase (ej: una galleta puede ser de chocolate, otra de vainilla).
- 📌 **Comportamiento Compartido**: Todos los objetos de la misma clase pueden realizar las acciones (métodos) definidas en la clase.

🍪 **Tip:** Siguiendo el ejemplo, cada galleta individual que horneas es un objeto de la clase "Galleta".

---

## 3. Definiendo una Clase Sencilla 🛠️

Para definir una clase, usamos la palabra clave `class` seguida del nombre de la clase (por convención, empieza con Mayúscula - CamelCase).

- 📌 **`class NombreClase:`**: Así empieza la definición.
- 📌 **`__init__(self, ...)` (Constructor)**: Es un método especial que se llama automáticamente cuando creas un nuevo objeto. Se usa para inicializar los atributos del objeto. `self` es el primer argumento obligatorio.
- 📌 **`self`**: Representa la instancia particular del objeto que se está creando o usando. Con `self.atributo = valor` creamos o asignamos atributos al objeto.
- 📌 **Métodos**: Son funciones definidas dentro de la clase. También llevan `self` como primer argumento para poder acceder a los atributos y otros métodos del objeto.

**Ejemplo Mini: Clase `Coche`
```python
class Coche:
    # Constructor (__init__)
    def __init__(self, marca, modelo, color):
        self.marca = marca          # Atributo de instancia
        self.modelo = modelo        # Atributo de instancia
        self.color = color          # Atributo de instancia
        self.encendido = False      # Atributo con valor inicial fijo
        self.velocidad = 0

    # Método
    def arrancar(self):
        if not self.encendido:
            self.encendido = True
            print(f"El {self.marca} {self.modelo} ha arrancado. ¡Vroom vroom! 💨")
        else:
            print(f"El {self.marca} {self.modelo} ya estaba encendido.")

    # Otro método
    def acelerar(self, incremento):
        if self.encendido:
            self.velocidad += incremento
            print(f"El {self.marca} {self.modelo} acelera a {self.velocidad} km/h.")
        else:
            print("No puedes acelerar, el coche está apagado.")

    def describir(self):
        estado = "encendido" if self.encendido else "apagado"
        print(f"Soy un {self.marca} {self.modelo} de color {self.color}. Estoy {estado} y voy a {self.velocidad} km/h.")
```

🧱 **Tip:** El `__init__` es como la "configuración inicial" de cada objeto que creas.

---

## 4. Creando y Usando Objetos (Instanciación) 🚗💨

Una vez definida la clase, puedes crear múltiples objetos (instancias) de ella.

- 📌 **Crear un objeto**: `nombre_objeto = NombreClase(argumentos_para_init)`
- 📌 **Acceder a atributos**: `nombre_objeto.atributo`
- 📌 **Llamar a métodos**: `nombre_objeto.metodo(argumentos_del_metodo)`

**Ejemplo Mini (usando la clase `Coche`):**
```python
# Crear objetos (instancias) de la clase Coche
mi_coche = Coche("Toyota", "Corolla", "Rojo")
coche_de_ana = Coche("Ford", "Fiesta", "Azul")

# Acceder a atributos y usar métodos
mi_coche.describir()
# Soy un Toyota Corolla de color Rojo. Estoy apagado y voy a 0 km/h.

mi_coche.arrancar()
# El Toyota Corolla ha arrancado. ¡Vroom vroom! 💨

mi_coche.acelerar(50)
# El Toyota Corolla acelera a 50 km/h.

mi_coche.describir()
# Soy un Toyota Corolla de color Rojo. Estoy encendido y voy a 50 km/h.

print(f"\nEl coche de Ana es un {coche_de_ana.marca} {coche_de_ana.modelo}.")
# El coche de Ana es un Ford Fiesta.
coche_de_ana.arrancar()
# El Ford Fiesta ha arrancado. ¡Vroom vroom! 💨
```

🔑 **Tip:** Cada objeto es independiente y tiene su propio estado, ¡aunque compartan el mismo "diseño" de la clase!

---

## 5. El Famoso `self` 🤔

Ya lo mencionamos, pero es ¡crucial! `self` es una referencia **a la instancia particular del objeto** sobre la cual se está llamando un método.

- 📌 **Referencia al Objeto Actual**: Permite que un método acceda a los atributos y otros métodos de ESE objeto específico.
- 📌 **Primer Argumento**: Es el primer argumento que Python pasa automáticamente a los métodos de instancia (incluyendo `__init__`). No lo pasas tú al llamar al método desde el objeto (`mi_objeto.metodo()`, Python lo hace por ti).
- 📌 **Convención**: Se llama `self` por convención, pero técnicamente podría ser cualquier nombre (¡aunque es mejor seguir la convención!).

💬 **Tip:** `self` es como decir "yo mismo" o "este objeto en particular" dentro de la clase.

---

## ⚡ Repaso Relámpago ⚡

- **Clase**: Es el **molde** o plantilla. Define atributos y métodos.
- **Objeto**: Es una **instancia** (creación) de una clase. Tiene su propio estado.
- **`__init__(self, ...)`**: El **constructor**, inicializa los atributos del objeto.
- **`self`**: Referencia al **objeto actual** dentro de la clase.
- **Atributos**: Variables que guardan **datos** del objeto (`self.dato`).
- **Métodos**: Funciones que definen **acciones** del objeto (`def mi_accion(self):`).
- La POO ayuda a organizar el código de forma modular y reutilizable.