---
aliases:
  - Notación Big O 🚀
tags:
  - conceptos
breadcrumb:
  - "[[indice-web|🧑‍💻 Programación 💻✨]]"
Fecha: 2025-06-03
---
# Notación Big O 🚀

## 1. ¿Qué es la Notación Big O? 🤔

La **Notación Big O** es una forma matemática de describir el **límite superior** del tiempo de ejecución o el espacio de memoria que un algoritmo utilizará en el peor de los casos, a medida que el tamaño de la entrada (input) tiende a infinito. En palabras más sencillas, nos dice cómo se **escala** un algoritmo.
- Nos ayuda a entender cómo el **rendimiento** de un algoritmo cambia a medida que la cantidad de datos de entrada aumenta.
- Se enfoca en el <mark style="background: #FFB8EBA6;">**peor escenario**</mark> (worst-case scenario), lo que nos da una garantía de rendimiento.
- Ignora constantes y términos de menor orden, porque lo que realmente importa es la **tasa de crecimiento** a medida que la entrada se vuelve muy grande. Por ejemplo, si un algoritmo tarda `5n^2 + 3n + 20` operaciones, su Big O es `O(n^2)`.
## 2. ¿Por qué es Importante? 💡
Entender la Notación Big O es crucial porque:
- Permite **comparar** la eficiencia de diferentes algoritmos que resuelven el mismo problema.
- Ayuda a **predecir** cómo se comportará un algoritmo con grandes cantidades de datos.
- Es fundamental para escribir código **eficiente** y **escalable**, especialmente en aplicaciones grandes.
- Facilita la toma de decisiones sobre qué estructura de datos o algoritmo elegir para una tarea específica. <mark style="background: #BBFABBA6;">¡Elegir bien puede hacer una gran diferencia!</mark>
## 3. Complejidades Comunes y Ejemplos 📊
Aquí te presento una tabla con las complejidades más comunes, de la más eficiente a la menos eficiente, junto con ejemplos sencillos:

| **Big O**            | **Nombre**      | **Descripción**                                                                      | **Ejemplo Sencillo (conceptual)**                                                                                                                                                                                                                        |
| -------------------- | --------------- | ------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **O(1)**             | **Constante**   | El tiempo de ejecución es el mismo, sin importar el tamaño de la entrada.            | Acceder a un elemento de un array por su índice: `array[5]`.                                                                                                                                                                                             |
| **O(log n)**         | **Logarítmica** | El tiempo de ejecución crece logarítmicamente con el tamaño de la entrada.           | Búsqueda binaria en un array ordenado. <mark style="background: #FFF3A3A6;">¡Muy eficiente para búsquedas!</mark>                                                                                                                                        |
| **O(n)**             | **Lineal**      | El tiempo de ejecución crece linealmente con el tamaño de la entrada.                | Recorrer todos los elementos de un array una vez: `for item in array: print(item)`.                                                                                                                                                                      |
| **O(n log n)**       | **Log-Lineal**  | Una combinación común, a menudo vista en algoritmos de ordenamiento eficientes.      | Algoritmos de ordenamiento como Merge Sort o Quick Sort (en su caso promedio y mejor).                                                                                                                                                                   |
| **O(n<sup>2</sup>)** | **Cuadrática**  | El tiempo de ejecución crece proporcionalmente al cuadrado del tamaño de la entrada. | Bucles anidados que iteran sobre la misma colección: `for i in array: for j in array: ...`.                                                                                                                                                              |
| **O(2<sup>n</sup>)** | **Exponencial** | El tiempo de ejecución se duplica con cada elemento adicional en la entrada.         | Algunos algoritmos recursivos que resuelven un problema de tamaño `n` haciendo dos llamadas recursivas de tamaño `n-1`, como el cálculo de Fibonacci de forma recursiva simple. <mark style="background: #FF5582A6;">¡Se vuelve lento muy rápido!</mark> |
| **O(n!)**            | **Factorial**   | El tiempo de ejecución crece de forma factorial.                                     | Generar todas las permutaciones posibles de una lista. El problema del viajante (TSP) resuelto por fuerza bruta.                                                                                                                                         |
**Ilustración Conceptual del Crecimiento:**
Imagina que `n` es el número de elementos:
- **O(1):** Siempre toma el mismo tiempo, no importa si son 10 o 1 millón de elementos.
- **O(log n):** Si tienes 1000 elementos, podría tomar, digamos, 10 pasos. Si duplicas a 2000, solo toma 11 pasos. ¡Crece muy lento!
- **O(n):** Si tienes 1000 elementos, toma 1000 "unidades de tiempo". Si duplicas a 2000, toma 2000 "unidades de tiempo".
- **O(n<sup>2</sup>):** Si tienes 10 elementos, toma 100 "unidades de tiempo". Si tienes 100 elementos, ¡toma 10,000! <mark style="background: #FFB86CA6;">Crece rápido.</mark>
- **O(2<sup>n</sup>):** Si tienes 10 elementos, toma 1024 "unidades de tiempo". Si tienes 20, ¡toma más de un millón!
## 4. Reglas Generales para Calcular Big O ✍️
Cuando analizas un algoritmo, puedes seguir estas reglas:
1. **Peor Caso:** Siempre considera el escenario que tomaría más tiempo.
2. **Eliminar Constantes:** Si un algoritmo realiza `2n` operaciones, su Big O es `O(n)`, no `O(2n)`. Las constantes no importan a gran escala.
    - _Ejemplo:_ `O(500)` es `O(1)`. `O(13n^2)` es `O(n^2)`.
3. **Términos No Dominantes:** Si un algoritmo tiene operaciones `n^2 + n`, el término `n` es mucho menos significativo que `n^2` para valores grandes de `n`. Así que el Big O es `O(n^2)`. <mark style="background: #ADCCFFA6;">Nos quedamos con el término que crece más rápido.</mark>
    - _Ejemplo:_ `O(n^3 + 50n^2 + 10000)` es `O(n^3)`.
4. **Operaciones Secuenciales (una después de otra):** Se suman las complejidades.
    - _Ejemplo:_ Un bloque de código `O(n)` seguido de un bloque `O(log n)` es `O(n + log n)`. Según la regla 3, esto se simplifica a `O(n)`.
5. **Operaciones Anidadas (un bucle dentro de otro):** Se multiplican las complejidades.
    - _Ejemplo:_ Un bucle externo que corre `n` veces y un bucle interno que corre `m` veces para cada iteración del externo, resulta en `O(n*m)`. Si ambos corren `n` veces, es `O(n*n) = O(n^2)`.
**Ejemplo Práctico de Análisis:**
```python
def ejemplo_funcion(lista):
  print(lista[0]) # O(1)

  for elemento in lista: # O(n) - Bucle 1
    print(elemento)

  for elemento in lista: # O(n) - Bucle 2 (secuencial)
    for otro_elemento in lista: # O(n) - Bucle 3 (anidado dentro del 2)
      print(elemento, otro_elemento)
```
Análisis:
- La primera línea es **O(1)**.
- El primer bucle `for` es **O(n)**.
- El segundo bloque de bucles anidados: el externo es `O(n)` y el interno es `O(n)`, entonces juntos son `O(n*n) = O(n^2)`.
Sumando las complejidades (secuencial): O(1) + O(n) + O(n^2).
Aplicando la regla de términos no dominantes, nos quedamos con el más grande: <mark style="background: #D2B3FFA6;">O(n<sup>2</sup>)</mark>.
## 5. Mini Repaso Final ✨
- **Notación Big O** describe la <mark style="background: #ABF7F7A6;">**eficiencia**</mark> (tiempo o espacio) de un algoritmo en el peor de los casos.
- Se enfoca en cómo el rendimiento **escala** con el aumento del tamaño de la entrada (`n`).
- Ignora constantes y términos menos significativos.
- Complejidades comunes incluyen **O(1)** (genial 👍), **O(log n)**, **O(n)**, **O(n log n)**, **O(n<sup>2</sup>)**, y **O(2<sup>n</sup>)** (peligroso para entradas grandes 👎).
- Conocer Big O te ayuda a escribir código más **rápido** y **escalable**.