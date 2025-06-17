---
aliases:
  - 🧬 Herencia
tags:
  - python
  - poo
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-05-31
---
# 🧬 Herencia en Python: Creando Familias de Clases

---

## 1. ¿Qué es la Herencia? 🤔 (Familia de Clases)

La **herencia** es un mecanismo que permite crear nuevas clases (llamadas **clases hijas** o **derivadas**) que "heredan" atributos (datos) y métodos (comportamientos) de clases existentes (llamadas **clases padres**, **base** o **superclases**).

- 📌 **Reutilización de Código**: Evita escribir el mismo código una y otra vez.
- 📌 **Relación "ES UN"**: Una clase hija "es un tipo de" su clase padre (ej: un `Perro` **es un** `Animal`).
- 📌 **Jerarquía**: Permite crear una estructura jerárquica de clases, desde las más generales a las más específicas.

💡 **Tip:** Piensa en la herencia genética: heredas características de tus padres, pero también tienes tus propias particularidades.

---

## 2. ¿Por Qué Usar Herencia? 🛠️ (Beneficios)

La herencia ofrece varias ventajas importantes en el diseño de software:

- 📌 **DRY (Don't Repeat Yourself)**: El código común se define una vez en la clase padre y se reutiliza en todas las clases hijas.
- 📌 **Especialización y Extensión**: Las clases hijas pueden añadir nuevos atributos y métodos, o modificar (sobrescribir) los heredados para especializar el comportamiento.
- 📌 **Polimorfismo**: Facilita tratar objetos de diferentes clases hijas de manera uniforme si comparten una interfaz común heredada (¡tema para otra nota!).
- 📌 **Organización Lógica**: Refleja relaciones del mundo real, haciendo el código más intuitivo.

✨ **Tip:** ¡La herencia te ayuda a construir sobre lo que ya existe, de forma ordenada y eficiente!

---

## 3. Creando Herencia Simple 🔗

Para que una clase herede de otra, se especifica la clase padre entre paréntesis después del nombre de la clase hija.

- 📌 **Sintaxis**: `class ClaseHija(ClasePadre):`
- 📌 La clase hija automáticamente tiene acceso a los atributos y métodos (públicos y protegidos) de la clase padre.

**Ejemplo Mini: `Vehiculo` y `Coche`**
```python
class Vehiculo:  # Clase Padre / Base
    def __init__(self, marca, color):
        self.marca = marca
        self.color = color
        print(f"Vehículo {self.marca} ({self.color}) creado.")

    def moverse(self):
        print(f"El vehículo {self.marca} se está moviendo.")

    def detenerse(self):
        print(f"El vehículo {self.marca} se ha detenido.")

class Coche(Vehiculo):  # Clase Hija / Derivada
    def __init__(self, marca, color, numero_puertas):
        # Llamamos al __init__ de la clase padre para inicializar marca y color
        super().__init__(marca, color) # ¡Importante!
        self.numero_puertas = numero_puertas # Atributo específico de Coche
        print(f"Coche con {self.numero_puertas} puertas creado.")

    def tocar_claxon(self): # Método específico de Coche
        print(f"El coche {self.marca} hace: ¡Pip pip! 📢")

# Creamos un objeto de la clase hija
mi_coche = Coche("Seat", "Rojo", 4)
# mi_coche.moverse()      # Heredado de Vehiculo
# mi_coche.detenerse()    # Heredado de Vehiculo
# mi_coche.tocar_claxon() # Propio de Coche
# print(f"Mi coche es un {mi_coche.marca} de color {mi_coche.color} con {mi_coche.numero_puertas} puertas.")
```

🚗 **Tip:** Siempre es buena práctica llamar al constructor (`__init__`) de la clase padre desde el constructor de la clase hija usando `super().__init__(...)`.

---

## 4. Sobrescritura de Métodos (Override) 🔄

Una clase hija puede proporcionar una implementación específica para un método que ya está definido en su clase padre. Esto se llama **sobrescritura** (override).

- 📌 **Misma Firma**: El método en la clase hija debe tener el mismo nombre (y a menudo los mismos parámetros) que el método en la clase padre.
- 📌 **Comportamiento Especializado**: Permite que la clase hija adapte el comportamiento heredado a sus necesidades.

**Ejemplo Mini (continuando con `Vehiculo` y `Bicicleta`):**
```python
class Bicicleta(Vehiculo):
    def __init__(self, marca, color, tipo_manillar):
        super().__init__(marca, "Negro") # Las bicis son siempre negras en este ejemplo :)
        self.color_original = color # Guardamos el color deseado
        self.tipo_manillar = tipo_manillar

    # Sobrescribimos el método moverse
    def moverse(self):
        # Podemos llamar al método original del padre si queremos
        # super().moverse()
        print(f"La bicicleta {self.marca} (color original: {self.color_original}) pedalea velozmente. 🚲")

    def sonar_timbre(self):
        print(f"La bicicleta {self.marca} hace: ¡Ring ring!")

# mi_bici = Bicicleta("Trek", "Azul", "Recto")
# mi_bici.moverse() # Ejecuta el método moverse() de Bicicleta
# mi_bici.detenerse() # Heredado de Vehiculo
# print(f"Color real de la bici (atributo de Vehiculo): {mi_bici.color}")
```

🎨 **Tip:** La sobrescritura permite que las clases hijas "pongan su propio sabor" a los métodos heredados.

---

## 5. La Función `super()` ✨ (Llamando al Padre)

La función `super()` es una herramienta muy útil en la herencia. Permite a una clase hija llamar a métodos de su clase padre (o, más precisamente, a la siguiente clase en el Orden de Resolución de Métodos - MRO).

- 📌 **Acceso al Padre**: Se usa comúnmente en `__init__` para asegurar que la inicialización del padre se ejecute.
- 📌 **Extender, no Reemplazar**: En métodos sobrescritos, `super().nombre_metodo()` te permite ejecutar la lógica del padre y luego añadir la lógica específica de la hija.
```python
class Motocicleta(Vehiculo):
    def __init__(self, marca, color, cilindrada):
        super().__init__(marca, color) # Llama a Vehiculo.__init__
        self.cilindrada = cilindrada

    def moverse(self):
        super().moverse() # Llama a Vehiculo.moverse()
        print(f"La motocicleta {self.marca} ruge con sus {self.cilindrada}cc. 🏍️")

# moto = Motocicleta("Honda", "Negra", 600)
# moto.moverse()
```

🔗 **Tip:** `super()` es la forma "correcta" y flexible de referirse a la clase padre, especialmente útil en herencia múltiple.

---

## 6. Herencia Múltiple 🧩 (Usar con Precaución)

Python permite que una clase herede de **múltiples clases padre**.

- 📌 **Sintaxis**: `class ClaseHija(Padre1, Padre2, ...):`
- 📌 **MRO (Method Resolution Order)**: Python tiene un algoritmo (C3) para determinar el orden en que se buscan los métodos en la jerarquía de clases padres. Es importante entenderlo si usas herencia múltiple para evitar sorpresas.
- 📌 **Complejidad**: Puede hacer el código más complejo de entender y mantener. A menudo se prefieren otras técnicas como la composición o los Mixins si la herencia múltiple se vuelve enrevesada.

```python
class Volador:
    def volar(self):
        print("Estoy volando!")

class Nadador:
    def nadar(self):
        print("Estoy nadando!")

class Pato(Vehiculo, Volador, Nadador): # Hereda de 3 clases!
    def __init__(self, nombre):
        super().__init__(nombre, "Amarillo") # Llama al __init__ de Vehiculo (el primero en MRO para __init__)

    def graznar(self):
        print(f"{self.marca} dice: Cuac!")

# pato_lucas = Pato("Lucas")
# pato_lucas.moverse()
# pato_lucas.volar()
# pato_lucas.nadar()
# pato_lucas.graznar()
# print(Pato.mro()) # Para ver el orden de resolución
```

⚠️ **Tip:** La herencia múltiple es poderosa pero úsala con cuidado. A veces, "menos es más".

---

## ⚡ Repaso Relámpago ⚡

- **Herencia**: Clases hijas heredan de clases padres (reutilización, especialización).
- **Sintaxis**: `class Hija(Padre):`
- **Sobrescritura**: La hija redefine un método del padre para comportamiento específico.
- **`super()`**: Llama a métodos de la clase padre (o siguiente en MRO). Crucial para `__init__` y extender métodos.
- **Beneficios**: DRY, organización, extensibilidad.
- **Herencia Múltiple**: Posible, pero manejar con conocimiento del MRO.