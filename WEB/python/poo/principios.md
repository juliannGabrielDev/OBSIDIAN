---
aliases:
  - 🏛️✨ Principios de la POO en Python
tags:
  - python
  - poo
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-05-31
---
# 🏛️✨ Principios de la POO en Python: ¡Las Claves del Éxito!

Generalmente se habla de cuatro pilares: **Encapsulamiento, Abstracción, Herencia y Polimorfismo**. ¡Vamos a repasarlos!

---

## 1. Encapsulamiento 💊 (Proteger tus Datos y Métodos)

El encapsulamiento consiste en **agrupar los datos (atributos) y los métodos (comportamientos) que operan sobre esos datos dentro de una unidad (la clase)**. También implica restringir el acceso directo a algunos de los componentes internos del objeto.

- 📌 **Ocultación de Información**: Protege el estado interno del objeto de modificaciones externas no deseadas.
- 📌 **Control de Acceso**: Define qué partes del objeto son públicas (accesibles desde fuera) y cuáles son privadas o protegidas.
- 📌 **Convenciones en Python**:
    - `_nombre_atributo` (un guion bajo): Indica que es "protegido" (convención, no restricción real). Para uso interno de la clase o subclases.
    - `__nombre_atributo` (dos guiones bajos): Activa el "name mangling", haciéndolo más difícil de acceder desde fuera (convención para "privado").

**Ejemplo Mini: Una `CuentaBancaria`**
```python
class CuentaBancaria:
    def __init__(self, titular, saldo_inicial):
        self.titular = titular       # Público
        self.__saldo = saldo_inicial # "Privado" por name mangling (_CuentaBancaria__saldo)
        self._tipo_cuenta = "Ahorro" # "Protegido" por convención

    def depositar(self, cantidad):
        if cantidad > 0:
            self.__saldo += cantidad
            print(f"Depósito exitoso. Saldo actual: ${self.__saldo}")
        else:
            print("Cantidad de depósito no válida.")

    def retirar(self, cantidad):
        if 0 < cantidad <= self.__saldo:
            self.__saldo -= cantidad
            print(f"Retiro exitoso. Saldo actual: ${self.__saldo}")
        else:
            print("Fondos insuficientes o cantidad no válida.")

    def obtener_saldo(self): # Método público para acceder al saldo
        return self.__saldo

# cuenta_ana = CuentaBancaria("Ana Pérez", 1000)
# cuenta_ana.depositar(500)
# print(f"Saldo de Ana: ${cuenta_ana.obtener_saldo()}")
# print(cuenta_ana.titular)
# print(cuenta_ana._tipo_cuenta)
# Intenta acceder directamente a __saldo (no es fácil debido al name mangling)
# print(cuenta_ana.__saldo) # Esto daría AttributeError
# print(cuenta_ana._CuentaBancaria__saldo) # Así se podría, pero no se debe
```

🛡️ **Tip:** El encapsulamiento es como tener una caja fuerte para los datos importantes de tus objetos, ¡solo se accede a través de los mecanismos que tú defines!

---

## 2. Abstracción 🎨 (Mostrar lo Esencial, Ocultar lo Complejo)

La abstracción consiste en **mostrar al usuario solo la funcionalidad esencial** de un objeto y **ocultar los detalles complejos de su implementación**. Se centra en el "¿QUÉ?" hace un objeto, en lugar del "¿CÓMO?" lo hace.

- 📌 **Simplificar la Interfaz**: Proporciona una interfaz clara y sencilla para interactuar con el objeto.
- 📌 **Reducir Complejidad**: El usuario no necesita conocer los detalles internos para usar el objeto.
- 📌 **Ejemplo Conceptual**: Piensa en conducir un coche. Usas el volante, los pedales y la palanca de cambios (la interfaz abstracta), pero no necesitas saber cómo funciona el motor de combustión interna o la transmisión (los detalles de implementación).

**Ejemplo Mini (Conceptual):**
```python
# Imagina una clase 'MandoTV'
class MandoTV:
    def __init__(self, marca_tv):
        self.__marca_tv = marca_tv # Detalle interno
        self.__conexion_establecida = self._conectar_a_tv() # Detalle interno

    def _conectar_a_tv(self): # Método "privado", detalle de implementación
        print(f"Conectando al TV {self.__marca_tv}...")
        # Lógica compleja de conexión aquí...
        return True

    # Interfaz pública y abstracta para el usuario:
    def encender(self):
        if self.__conexion_establecida:
            print(f"TV {self.__marca_tv} encendida.")

    def cambiar_canal(self, canal):
        if self.__conexion_establecida:
            print(f"Cambiando al canal {canal} en TV {self.__marca_tv}.")

# mando = MandoTV("Samsung")
# mando.encender()
# mando.cambiar_canal(5)
# El usuario no se preocupa de _conectar_a_tv()
```

🧐 **Tip:** La abstracción es como el panel de control de una nave espacial: te da los botones importantes sin que tengas que ser ingeniero aeroespacial para usarla.

---

## 3. Herencia 🧬 (Reutilizar y Extender)

La herencia permite crear nuevas clases (clases hijas) que **heredan atributos y métodos de clases existentes** (clases padres). Esto fomenta la reutilización de código y crea una jerarquía de clases.

- 📌 **Reutilización de Código**: El código común se define en la clase padre.
- 📌 **Relación "ES UN"**: Una `Perro` ES UN `Animal`.
- 📌 **Especialización**: Las clases hijas pueden añadir nueva funcionalidad o modificar la heredada (sobrescritura).

**Ejemplo Mini (Recordatorio Rápido):**
```python
class Figura: # Clase Padre
    def __init__(self, color):
        self.color = color
    def describir(self):
        print(f"Soy una figura de color {self.color}")

class Circulo(Figura): # Clase Hija
    def __init__(self, color, radio):
        super().__init__(color) # Hereda __init__ del padre
        self.radio = radio
    def dibujar(self): # Método específico
        print(f"Dibujando un círculo {self.color} de radio {self.radio}")

# mi_circulo = Circulo("azul", 5)
# mi_circulo.describir() # Heredado
# mi_circulo.dibujar()   # Propio
```

👨‍👩‍👧 **Tip:** La herencia es como construir con bloques de LEGO: usas piezas base (padres) y añades piezas especiales (hijas) para crear algo nuevo. (¡Ya lo vimos más a fondo!)

---

## 4. Polimorfismo 🎭 (Múltiples Formas)

El polimorfismo (del griego "muchas formas") es la capacidad de **objetos de diferentes clases de responder al mismo mensaje (llamada de método) de diferentes maneras específicas para cada clase**.

- 📌 **"Muchas Formas"**: Un mismo método puede tener comportamientos distintos dependiendo del objeto que lo invoque.
- 📌 **Duck Typing en Python**: "Si camina como un pato y grazna como un pato, entonces es un pato". Python se centra en el comportamiento del objeto (si tiene el método que se intenta llamar) más que en su tipo exacto.
- 📌 **Simplifica el Código**: Permite escribir código más genérico que puede operar con diferentes tipos de objetos.

**Ejemplo Mini: Diferentes animales `hablan`**
```python
class Perro:
    def __init__(self, nombre): self.nombre = nombre
    def hablar(self):
        return f"{self.nombre} dice: ¡Guau!"

class Gato:
    def __init__(self, nombre): self.nombre = nombre
    def hablar(self):
        return f"{self.nombre} dice: ¡Miau!"

class Vaca:
    def __init__(self, nombre): self.nombre = nombre
    def hablar(self):
        return f"{self.nombre} dice: ¡Muuu!"

def presentar_animal_y_su_sonido(animal):
    # No importa si animal es Perro, Gato o Vaca,
    # siempre que tenga un método hablar()
    print(animal.hablar())

# fido = Perro("Fido")
# michi = Gato("Michi")
# lola = Vaca("Lola")

# presentar_animal_y_su_sonido(fido)
# presentar_animal_y_su_sonido(michi)
# presentar_animal_y_su_sonido(lola)
```

🎤 **Tip:** El polimorfismo es como un festival de talentos: todos suben al escenario (se llama el mismo método), ¡pero cada uno muestra su acto único!

---

## ⚡ Repaso Relámpago ⚡

- 💊 **Encapsulamiento**: Agrupar y proteger datos/métodos. Controlar acceso.
- 🎨 **Abstracción**: Mostrar lo esencial, ocultar detalles complejos. Simplificar interfaz.
- 🧬 **Herencia**: Reutilizar y extender código. Clases padres e hijas.
- 🎭 **Polimorfismo**: Objetos diferentes responden al mismo mensaje de formas distintas.