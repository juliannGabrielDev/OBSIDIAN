# Tipo, Orden, Grado y Linealidad
## Tipo

Las **ecuaciones diferenciales ordinarias (EDO)** y las **ecuaciones diferenciales parciales (EDP)** se diferencian en varios aspectos clave:

| Característica                         | EDO (Ecuación Diferencial Ordinaria)                                                                           | EDP (Ecuación Diferencial Parcial)                                               |
| -------------------------------------- | -------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| **Número de variables independientes** | Tiene una sola variable independiente.                                                                         | Tiene dos o más variables independientes.                                        |
| **Derivadas**                          | Solo aparecen derivadas ordinarias.                                                                            | Aparecen derivadas parciales.                                                    |
| **Ejemplo general**                    | $$\frac{dy}{dx} + y = 0$$                                                                                      | $$\frac{\partial u}{\partial t} = k \frac{\partial^2 u}{\partial x^2}$$          |
| **Ámbito de aplicación**               | Modela fenómenos unidimensionales, como circuitos eléctricos, crecimiento poblacional, dinámica de partículas. | Modela fenómenos multidimensionales, como propagación del calor, ondas, fluidos. |
| **Solución**                           | Es una función de una sola variable.                                                                           | Es una función de varias variables.                                              |

## Orden

El **orden** de una ecuación diferencial es el orden de la derivada más alta que aparece en la ecuación.

## Grado

El **grado** de una ecuación diferencial es el **exponente** al que está elevada la derivada de mayor orden, siempre que la ecuación esté escrita en forma polinómica en sus derivadas.

>[!NOTE]
>Si la ecuación contiene funciones no polinómicas de las derivadas (como senos, cosenos o fracciones con derivadas en el denominador), **primero debe reescribirse en forma polinómica** antes de determinar su grado. Si hay raíces, funciones trascendentes (senos, logaritmos, etc.) o derivadas en fracciones, la ecuación **no tiene un grado definido**.

## Linealidad

La **linealidad** de una ecuación diferencial se refiere a si la ecuación es **lineal** o **no lineal** en la función desconocida y sus derivadas.

---
### 1. Ecuaciones Diferenciales Lineales

Una ecuación diferencial es **lineal** si se puede escribir en la forma:

$$a_n(x) \frac{d^n y}{dx^n} + a_{n-1}(x) \frac{d^{n-1}y}{dx^{n-1}} + \dots + a_1(x) \frac{dy}{dx} + a_0(x)y = f(x)$$

🔹 Características:  
✅ Los coeficientes $a_n(x), a_{n-1}(x), \dots, a_0(x)$ dependen solo de $x$, pero **no de $y$ ni de sus derivadas**.  
✅ La función $f(x)$ no contiene términos de $y$ ni de sus derivadas elevadas a potencias o en funciones no lineales (senos, exponentes, etc.).

📌 **Ejemplo de ecuación diferencial lineal:**

$$\frac{d^2y}{dx^2} + 3\frac{dy}{dx} + 2y = \sin x$$

✅ Es lineal porque no hay términos como $y^2$, $\sin y$ o productos entre derivadas.

---

### 2. Ecuaciones Diferenciales No Lineales

Una ecuación es **no lineal** si:  
❌ Contiene términos como $y^2$ ,$y^3$, $e^y$, $\sin ⁡y$, $\ln y$.  
❌ Hay productos entre derivadas, como $\left(\frac{dy}{dx}\right)^2$.

📌 **Ejemplo de ecuación diferencial no lineal:**

$$\frac{d^2y}{dx^2} + \left(\frac{dy}{dx}\right)^2 + y^3 = 0$$

❌ Es no lineal porque tiene: $\left(\frac{dy}{dx}\right)^2 y$ y $y^3$.

📌 **Otro ejemplo de ecuación diferencial no lineal:**

$$\frac{d^2y}{dx^2} + e^y = x$$

❌ Es no lineal porque tiene $e^y$.

---

📌 **Regla general:**

- **Lineal** → No hay funciones no lineales de yy ni productos entre derivadas.
- **No lineal** → Si hay términos como $y^2$, $\sin ⁡y$, $y^2$, $\sin y$, $e^y$, o productos de derivadas.