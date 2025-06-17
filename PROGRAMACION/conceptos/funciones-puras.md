---
aliases:
  - Funciones Puras 💧
tags:
  - conceptos
breadcrumb:
  - "[[indice-web|🧑‍💻 Programación 💻✨]]"
Fecha: 2025-06-03
---
# Funciones Puras 💧
## 1. ¿Qué Son las Funciones Puras? 🤔
Una **función pura** es una función que cumple con dos condiciones principales:
- <mark style="background: #FFF3A3A6;">**Siempre devuelve el mismo resultado para las mismas entradas**</mark> (es **determinista**). No importa cuántas veces la llames con los mismos argumentos, siempre te dará la misma salida.
- <mark style="background: #BBFABBA6;">**No tiene efectos secundarios**</mark>. Esto significa que la función no modifica ningún estado fuera de su propio ámbito. No cambia variables globales, no escribe en un archivo, no imprime en la consola, no modifica sus argumentos si son objetos o arrays, etc.
**En resumen:** Su única tarea es calcular un valor de salida basado en sus entradas, ¡y nada más! 🧺
## 2. Características Clave 🔑

| **Característica**          | **Descripción**                                                                                               | **Ejemplo Sencillo (Conceptual)** |
| --------------------------- | ------------------------------------------------------------------------------------------------------------- | --------------------------------- |
| **Determinista**            | Dada una entrada `x`, siempre retornará `y`.                                                                  | `suma(2, 3)` siempre será `5`.    |
| **Sin Efectos Secundarios** | No altera el "mundo exterior". No modifica variables globales ni realiza operaciones de E/S (Entrada/Salida). | No escribe en la consola.         |

## 4. Ejemplos Prácticos 💻

Veamos un ejemplo simple en pseudocódigo (¡o imagínalo en tu lenguaje favorito!):

**Función Pura:**
```
funcion sumar(a, b):
  retornar a + b
```
- Si llamas `sumar(2, 3)`, siempre obtendrás `5`.
- No modifica nada fuera de la función.
**Función Impura (con efecto secundario):**
```
variable_global = 10

funcion sumar_y_modificar_global(a, b):
  variable_global = variable_global + a + b // Efecto secundario: modifica una variable global
  retornar variable_global
```

- `sumar_y_modificar_global(2, 3)` no solo devuelve un valor, sino que también cambia `variable_global`. Si la llamas de nuevo, el estado global será diferente.

**Función Impura (no determinista debido a entrada externa):**
```
funcion obtener_numero_aleatorio_mas_entrada(a):
  numero_aleatorio = generar_numero_aleatorio() // Entrada externa no explícita
  retornar a + numero_aleatorio
```
- `obtener_numero_aleatorio_mas_entrada(5)` devolverá un resultado diferente cada vez debido a `generar_numero_aleatorio()`.