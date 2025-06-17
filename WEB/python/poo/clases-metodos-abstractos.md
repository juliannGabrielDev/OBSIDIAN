---
aliases:
  - Clases y Métodos Abstractos
tags:
  - python
  - poo
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-05-31
---
# Clases y Métodos Abstractos en Python: Definiendo Contratos

---

## 1. ¿Qué son las Clases Abstractas? 🤔 (El Contrato Incompleto)

Una **Clase Abstracta** es como una plantilla o un "contrato" para otras clases. **No puedes crear instancias (objetos) directamente de una clase abstracta**. Su propósito principal es ser heredada por otras clases (subclases).

- 📌 **Plantilla Base**: Define una estructura común que las subclases deben seguir.
- 📌 **No Instanciable**: No puedes hacer `objeto = MiClaseAbstracta()`. Daría error.
- 📌 **Puede tener Métodos Abstractos y Concretos**:
    - **Métodos Abstractos**: Se declaran pero no se implementan en la clase abstracta.
    - **Métodos Concretos**: Tienen una implementación y las subclases los heredan tal cual (o pueden sobreescribirlos).
- 📌 **Módulo `abc`**: En Python, se usan con el módulo `abc` (Abstract Base Classes). Se hereda de `ABC`.

💡 **Tip:** Piensa en una clase abstracta como un "esqueleto" que otras clases deben "rellenar" y completar. Es un "quiero que todas mis subclases tengan ESTO, pero ellas decidirán CÓMO".

---

## 2. ¿Qué son los Métodos Abstractos? 📜 (La Tarea Pendiente)

Un **Método Abstracto** es un método que se declara en una clase abstracta, pero **no tiene una implementación (cuerpo)** en ella. Es básicamente una firma de método.

- 📌 **Declarados, no Definidos**: Tienen nombre y parámetros, pero su cuerpo suele ser `pass` o simplemente no existir.
- 📌 **Obligatorios para Subclases**: Las subclases concretas (no abstractas) que heredan de una clase abstracta **DEBEN** implementar (darle un cuerpo) a todos sus métodos abstractos.
- 📌 **Decorador `@abstractmethod`**: Se marcan con este decorador del módulo `abc`.

✍️ **Tip:** Un método abstracto es como una "tarea pendiente" que la clase abstracta le deja a sus hijas. "¡Tú te encargas de cómo hacer esto!"

---

## 3. ¿Por Qué Usarlos? 📐 (El Propósito)

Usar clases y métodos abstractos te ayuda a:

- 📌 **Definir una Interfaz Común (Contrato)**: Aseguran que todas las subclases tengan un conjunto mínimo de métodos, lo que facilita el polimorfismo. Puedes tratar objetos de diferentes subclases de manera uniforme.
- 📌 **Forzar Implementación**: Garantizan que las subclases no se olviden de implementar funcionalidades cruciales.
- 📌 **Diseño de APIs y Frameworks**: Son fundamentales para crear esqueletos de cómo deben funcionar ciertos componentes.
- 📌 **Evitar Código Duplicado**: Los métodos concretos en la clase abstracta pueden proporcionar funcionalidad común a todas las subclases.

🤝 **Tip:** Son una herramienta poderosa para el diseño de software, promoviendo la consistencia y la extensibilidad.

---

## 4. Creando Clases y Métodos Abstractos 🛠️

Para crear clases y métodos abstractos en Python, usamos el módulo `abc`.

- 📌 **Importar**: `from abc import ABC, abstractmethod`
- 📌 **Heredar de `ABC`**: La clase abstracta debe heredar de `ABC`.
- 📌 **Decorar**: Los métodos abstractos se decoran con `@abstractmethod`.

**Ejemplo Mini: `Figura`**
```python
from abc import ABC, abstractmethod

class Figura(ABC):  # Hereda de ABC para ser abstracta

    def __init__(self, nombre):
        self.nombre = nombre

    @abstractmethod  # Este método ES abstracto
    def calcular_area(self):
        pass  # Sin implementación aquí

    @abstractmethod  # Otro método abstracto
    def dibujar(self):
        pass

    def mostrar_nombre(self):  # Este es un método CONCRETO
        print(f"Soy una figura: {self.nombre}")

# Intentar crear una instancia de Figura daría error:
# figura_generica = Figura("Genérica") # TypeError: Can't instantiate abstract class Figura with abstract methods calcular_area, dibujar
```

🏗️ **Tip:** Si una clase hereda de `ABC` y tiene al menos un `@abstractmethod`, se considera abstracta.

---

## 5. Implementando la Clase Abstracta ✍️ (Cumpliendo el Contrato)

Las subclases que heredan de una clase abstracta deben proporcionar implementaciones concretas para **todos** los métodos abstractos.

- 📌 **Implementación Obligatoria**: Si una subclase no implementa todos los métodos abstractos de su clase padre abstracta, ¡esa subclase también se vuelve abstracta y no podrá ser instanciada!

**Ejemplo Mini (continuando con `Figura`):**
```python
import math

class Circulo(Figura): # Hereda de Figura
    def __init__(self, nombre, radio):
        super().__init__(nombre) # Llama al __init__ de Figura
        self.radio = radio

    # Implementación OBLIGATORIA de calcular_area
    def calcular_area(self):
        return math.pi * (self.radio ** 2)

    # Implementación OBLIGATORIA de dibujar
    def dibujar(self):
        print(f"Dibujando un círculo llamado {self.nombre} con radio {self.radio} O")

class Cuadrado(Figura):
    def __init__(self, nombre, lado):
        super().__init__(nombre)
        self.lado = lado

    def calcular_area(self):
        return self.lado ** 2

    def dibujar(self):
        print(f"Dibujando un cuadrado llamado {self.nombre} con lado {self.lado} ☐")

# Ahora sí podemos crear instancias de las subclases concretas
mi_circulo = Circulo("Círculo Uno", 5)
mi_cuadrado = Cuadrado("Cuadrado Alfa", 4)

mi_circulo.mostrar_nombre() # Usa el método concreto de Figura
print(f"Área del círculo: {mi_circulo.calcular_area()}")
mi_circulo.dibujar()

mi_cuadrado.mostrar_nombre()
print(f"Área del cuadrado: {mi_cuadrado.calcular_area()}")
mi_cuadrado.dibujar()
```

✅ **Tip:** Una vez que una subclase implementa todos los métodos abstractos, se vuelve "concreta" y puedes crear objetos de ella.

---

## ⚡ Repaso Relámpago ⚡

- **Clase Abstracta**: Molde no instanciable (hereda de `ABC`). Define una interfaz.
- **Método Abstracto**: Declarado con `@abstractmethod` en la clase abstracta, sin implementación allí.
- **Propósito**: Establecer un **contrato** que las subclases deben cumplir, promoviendo un diseño consistente.
- **Implementación**: Las subclases **deben** implementar todos los métodos abstractos para poder ser instanciadas.
- El módulo `abc` es tu herramienta para esto (`ABC`, `abstractmethod`).