---
aliases:
  - 🪆 Sombreado de Variables ⚡
tags:
  - js
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-javascript|⚡ Índice JavaScript]]"
Fecha: 2025-06-05
---
# 🪆 Sombreado de Variables en JS ⚡
¡Hola, compañero! Hoy vamos a repasar un concepto súper interesante en JavaScript: el sombreado de variables. ¡Es como cuando una variable "esconde" a otra! 😉
## 1. ¿Qué es el Sombreado de Variables?
El sombreado de variables (o _variable shadowing_) ocurre cuando declaras una variable con el mismo nombre en un <mark style="background: #BBFABBA6;">ámbito interno</mark> (como dentro de una función o un bloque `if`) que ya existe en un <mark style="background: #ADCCFFA6;">ámbito externo</mark>. La variable interna <mark style="background: #FF5582A6;">"cubre"</mark> o "sombrea" a la externa dentro de su propio ámbito. ¡Solo ves la de adentro!
- **Idea Clave:** La variable de adentro "gana" temporalmente.
- **No es un error:** Es un comportamiento esperado en JS.
<p style="text-align: center;">🎓 Tip: ¡Imagina que la variable interna es como un toldo que tapa el sol a la externa!</p>
## 2. `var` y el Sombreado (¡Cuidado con esto!)
Con `var`, el sombreado puede ser un poco <mark style="background: #FFB86CA6;">engañoso</mark> por su alcance de función.
- **`var` es de ámbito de función:** Si declaras una `var` dentro de una función, solo existe ahí.
- **Si declaras `var` en un bloque `if`/`for`:** ¡No hay sombreado real! Re-declara la misma variable del ámbito superior. ¡Es un dolor de cabeza a veces!

| **Ámbito** | **Declaración** | **Comportamiento**                                                 |
| ---------- | --------------- | ------------------------------------------------------------------ |
| Global     | `var x = 10;`   | Variable inicial                                                   |
| Función    | `var x = 20;`   | Sombra a la global                                                 |
| Bloque     | `var x = 30;`   | <mark style="background: #FFB86CA6;">¡Re-declara la global!</mark> |


```js
var mensaje = "Hola global";

function saludar() {
  var mensaje = "Hola función"; // Sombra a la global
  console.log(mensaje); // "Hola función"
}

if (true) {
  var mensaje = "Hola bloque"; // ¡Esta VAR re-declara la global!
  console.log(mensaje); // "Hola bloque"
}

saludar();
console.log(mensaje); // "Hola bloque" (¡sorpresa!)
```
<p style="text-align: center;">📘 Tip: Usa <code>var</code> con mucha precaución en bloques, ¡puede llevar a errores inesperados!</p>

## 3. `let` y `const` (¡Los amos del sombreado!)

Aquí es donde el sombreado funciona <mark style="background: #BBFABBA6;">de maravilla</mark> y es más predecible. `let` y `const` tienen <mark style="background: #ABF7F7A6;">ámbito de bloque</mark>.
- **`let` y `const` son de ámbito de bloque:** Cada vez que los declaras dentro de un `{}` (bloque), crean una nueva variable para ese bloque.
- **Sombreado claro:** La variable interna es completamente diferente a la externa.

```js
let nombre = "Ana";

if (true) {
  let nombre = "Pedro"; // Sombra a la 'nombre' global
  console.log(nombre); // "Pedro"
}

console.log(nombre); // "Ana"
```

| **Tipo** | **Ámbito** | **Sombreado**                                                  |
| -------- | ---------- | -------------------------------------------------------------- |
| `var`    | Función    | Confuso en bloques                                             |
| `let`    | Bloque     | <mark style="background: #BBFABBA6;">Claro y Predecible</mark> |
| `const`  | Bloque     | <mark style="background: #BBFABBA6;">Claro y Predecible</mark> |
<p style="text-align: center;">📌 Tip: Siempre que puedas, usa <code>let</code> o <code>const</code> para un código más claro y menos sorpresas con el sombreado.</p>