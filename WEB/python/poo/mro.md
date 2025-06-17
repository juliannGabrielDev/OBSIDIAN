---
aliases:
  - "📜 MRO: Orden de Resolución de Métodos"
tags:
  - python
  - poo
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-05-31
---
# 📜 MRO: Orden de Resolución de Métodos en Python

---

## 1. ¿Qué es el MRO? 🤔 (El Camino a Seguir)

El **MRO** es la secuencia o el **orden específico** en el que Python busca un método o atributo en una jerarquía de clases. Cuando llamas a un método en un objeto, Python sigue este orden para encontrar la primera aparición de ese método.

- 📌 **Herencia Múltiple**: Es crucial cuando una clase hereda de varias clases base.
- 📌 **Evita Ambigüedad**: Define claramente qué método "gana" si varias clases padre lo implementan.
- 📌 **Algoritmo C3**: Python usa un algoritmo llamado C3 Linearization para determinar este orden. Es sofisticado pero predecible.

💡 **Tip:** Piensa en el MRO como el "mapa de carreteras" que Python usa para encontrar dónde está definido un método que quieres usar.

---

## 2. ¿Por qué es Importante? 🗺️

El MRO es fundamental para la **previsibilidad** y el **correcto funcionamiento** de la herencia, sobre todo en escenarios complejos.

- 📌 **Comportamiento Predecible**: Sabes exactamente qué versión de un método se ejecutará.
- 📌 **Manejo del "Problema del Diamante"**: Resuelve situaciones donde una clase hereda de dos clases que, a su vez, heredan de una misma clase abuela común. El MRO asegura que la clase abuela se visite solo una vez de forma consistente.
- 📌 **`super()`**: La función `super()` depende intrínsecamente del MRO para saber a qué método "padre" (o más bien, "siguiente en el MRO") llamar.

✨ **Tip:** Sin un MRO bien definido, la herencia múltiple sería un caos. ¡El MRO pone orden!

---

## 3. ¿Cómo lo Determina Python? (Algoritmo C3) ⚙️

Python utiliza el algoritmo de **linearización C3**. No necesitas memorizar los detalles intrincados del algoritmo, pero sí entender sus principios:

- 📌 **Consistencia**: Mantiene un orden lógico y coherente.
- 📌 **Monotonicidad**: Si una clase C hereda de B, y B hereda de A, entonces el MRO de C siempre respetará el MRO de B (es decir, A vendrá después de B, si B no lo sobreescribe).
- 📌 **Preservación del Orden Local de Precedencia**: Si una clase hereda de `Base1, Base2`, Python intentará que `Base1` y sus ancestros vengan antes que `Base2` y los suyos en el MRO, siempre que no viole las otras reglas.

🤓 **Tip:** Aunque suene complejo, el C3 hace un excelente trabajo garantizando un MRO sensato. ¡Confía en él!

---

## 4. Viendo el MRO de una Clase 🔍

Python te permite inspeccionar fácilmente el MRO de cualquier clase. ¡Es muy útil para depurar o entender la jerarquía!

- 📌 **`NombreClase.mro()`**: Método que devuelve una lista de clases en el orden de resolución.
- 📌 **`NombreClase.__mro__`**: Atributo especial (una tupla) que también contiene el MRO.
- 📌 **`help(NombreClase)`**: La función `help()` también muestra el MRO en la información de la clase.

**Ejemplo Mini (Problema del Diamante Básico):**
```python
class A: pass
class B(A): pass
class C(A): pass
class D(B, C): pass # D hereda de B y C, ambas de A

# Veamos el MRO de D
print(D.mro())
# Salida esperada:
# [<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>]
# Nota: object es la clase base de todas las clases en Python.
```

📋 **Tip:** `print(MiClase.mro())` es tu mejor amigo para entender la ruta de búsqueda.

---

## 5. El "Problema del Diamante" 💎 y `super()`

El MRO es la solución elegante al "problema del diamante". `super()` utiliza este MRO para llamar al método de la **siguiente clase en la secuencia**, no necesariamente el padre directo.

- 📌 **Herencia en Diamante**: `ClaseD` hereda de `ClaseB` y `ClaseC`. `ClaseB` y `ClaseC` heredan de `ClaseA`.
- 📌 **`super()` Inteligente**: Cuando usas `super().metodo()` dentro de un método en la jerarquía, Python consulta el MRO de la instancia actual para determinar qué implementación de `metodo` ejecutar a continuación. Esto asegura que cada clase en la jerarquía se llame una sola vez en el orden correcto.

**Ejemplo Conceptual con `super()`:**

```python
class A:
    def accion(self):
        print("Acción en A")

class B(A):
    def accion(self):
        print("Acción en B")
        super().accion() # Llama a A.accion según el MRO

class C(A): # Si no hubiera herencia múltiple, super() llamaría a A
    def accion(self):
        print("Acción en C")
        super().accion()

class D(B, C): # MRO(D) = [D, B, C, A, object]
    def accion(self):
        print("Acción en D")
        super().accion() # Llama a B.accion (siguiente en MRO de D)

mi_objeto_d = D()
mi_objeto_d.accion()
# Salida:
# Acción en D
# Acción en B
# Acción en C  <- Aquí super() en B llama a C.accion (siguiente en MRO de D después de B)
# Acción en A  <- Aquí super() en C llama a A.accion (siguiente en MRO de D después de C)
```

🤝 **Tip:** `super()` y MRO trabajan juntos para hacer que la herencia múltiple sea cooperativa y ordenada.

---

## ⚡ Repaso Relámpago ⚡

- **MRO**: Es el **orden** en que Python busca métodos/atributos en clases heredadas.
- Crucial para la **herencia múltiple** y resolver ambigüedades como el "problema del diamante".
- Determinado por el algoritmo de **linearización C3**.
- Puedes verlo con `NombreClase.mro()` o `NombreClase.__mro__`.
- La función **`super()`** usa el MRO para llamar al método "siguiente" en la jerarquía de forma colaborativa.