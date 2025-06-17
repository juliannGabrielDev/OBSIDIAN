---
aliases:
  - Tipos de Datos Primitivos 📘
tags:
  - js
  - tipos-de-datos
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-javascript|⚡ Índice JavaScript]]"
Fecha: 2025-06-05
---
# Tipos de Datos Primitivos en JS 📘
Vamos a repasar algo súper básico pero esencial en JavaScript: los tipos de datos primitivos. ¡Es como conocer los ingredientes principales de una receta! 🧠
## 1. `String` (Cadenas de Texto) 📝
Es una secuencia de caracteres, ¡como palabras o frases! Siempre van entre comillas simples o dobles.
- Representan texto.
- Inmutables (no cambian una vez creados).
- Se usan con `''` o `""`.
<mark style="background: #FFF3A3A6;">**Ejemplo mini:**</mark>
```js
let saludo = "¡Hola mundo!";
let nombre = 'Ana';
```
📌 **Tip clave:** ¡Las cadenas son para todo lo que sea texto!
## 2. `Number` (Números) 🔢
¡Para todo lo que sea contar o medir! Pueden ser enteros o decimales.
- Números enteros y con punto decimal.
- Incluye valores especiales como `Infinity` y `NaN` (Not a Number).
<mark style="background: #BBFABBA6;">**Ejemplo mini:**</mark>
```js
let edad = 25;
let precio = 99.99;
let resultado = 10 / 0; // Infinity
let error = "hola" * 2; // NaN
```

📌 **Tip clave:** ¡JavaScript no distingue entre enteros y flotantes, todo es `Number`!
## 3. `Boolean` (Booleanos) ✅❌
¡Estos son súper simples! Solo pueden ser `true` (verdadero) o `false` (falso). ¡Perfectos para decisiones!
- Representan valores lógicos.
- Solo `true` o `false`.
<mark style="background: #ADCCFFA6;">**Ejemplo mini:**</mark>
```js
let esMayorDeEdad = true;
let tienePermiso = false;
```

📌 **Tip clave:** ¡Ideales para condiciones y control de flujo!
## 4. `Undefined` (Indefinido) ❓
Cuando una variable se declara pero no se le asigna un valor, ¡es `undefined`! Es como una caja vacía que aún no tiene nada dentro.
- Valor por defecto de variables no inicializadas.
- Significa "no se ha definido un valor todavía".
<mark style="background: #FFB8EBA6;">**Ejemplo mini:**</mark>
```js
let miVariable;
console.log(miVariable); // undefined
```

📌 **Tip clave:** ¡No lo confundas con `null`!
## 5. `Null` (Nulo) 🚫
Representa la ausencia intencional de cualquier valor o un "valor vacío". Tú mismo asignas `null` para decir "aquí no hay nada".
- Representa la ausencia de un valor.
- Debe ser asignado explícitamente.
<mark style="background: #FF5582A6;">**Ejemplo mini:**</mark>
```js
let carritoDeCompras = null;
```
📌 **Tip clave:** ¡`null` es un tipo de dato primitivo, pero `typeof null` devuelve 'object'! (¡Es una curiosidad del lenguaje!)
## 6. `Symbol` (Símbolo) 👾
¡Estos son más avanzados! Se usan para crear identificadores únicos y prevenir conflictos en propiedades de objetos.
- Valores únicos e inmutables.
- Se usan principalmente para propiedades de objetos.
<mark style="background: #D2B3FFA6;">**Ejemplo mini:**</mark>
```js
const idUnico = Symbol('mi_id');
const otroIdUnico = Symbol('mi_id');
console.log(idUnico === otroIdUnico); // false
```
📌 **Tip clave:** ¡Útiles para evitar colisiones de nombres!
## 7. `BigInt` (Entero Grande) 📏
Para números enteros ¡realmente grandes! Cuando `Number` ya no es suficiente, `BigInt` al rescate. Se le añade una `n` al final.
- Para números enteros que exceden la capacidad de `Number`.
- Se declara añadiendo `n` al final del número.
<mark style="background: #ABF7F7A6;">**Ejemplo mini:**</mark>
```js
const numeroGrande = 9007199254740991n + 10n;
```
📌 **Tip clave:** ¡Útil para operaciones con números gigantescos!