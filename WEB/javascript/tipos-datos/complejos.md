---
aliases:
  - Tipos de Datos Complejos 📚
tags:
  - js
  - tipos-de-datos
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-javascript|⚡ Índice JavaScript]]"
Fecha: 2025-06-05
---
# Tipos de Datos Complejos en JS 📚

¡Qué onda! Después de ver los primitivos, ahora toca hablar de los <mark style="background: #FF5582A6;">**tipos de datos complejos**</mark> en JavaScript. ¡Estos son como los "contenedores" donde guardamos colecciones de datos o estructuras más elaboradas! 📦

## 1. `Object` (Objetos) 🧩

El tipo `Object` es el rey de los complejos. Son colecciones de pares `clave: valor`. Piensa en ellos como un diccionario o una lista de características.

- Representan una colección de propiedades.
- Las propiedades tienen una clave (string o Symbol) y un valor (cualquier tipo de dato).
- Son mutables (puedes cambiar, añadir o eliminar propiedades).

<mark style="background: #FFF3A3A6;">**Ejemplo mini:**</mark>

```js
let persona = {
  nombre: "Carlos",
  edad: 30,
  estaEstudiante: true
};

console.log(persona.nombre); // "Carlos"
```

📌 **Tip clave:** ¡Los objetos son la base para casi todo lo que es "no primitivo" en JS!

## 2. `Array` (Arreglos/Arrays) 📊

Los arreglos son un tipo especial de objeto. Son listas ordenadas de valores. ¡Imagina una fila de casilleros, cada uno con algo dentro!

- Son listas ordenadas de elementos.
- Los elementos se acceden por su índice (posición, empezando desde `0`).
- Pueden contener cualquier tipo de dato.

<mark style="background: #BBFABBA6;">**Ejemplo mini:**</mark>

```js
let frutas = ["manzana", "banana", "cereza"];
console.log(frutas[0]); // "manzana"

let numeros = [1, 5, 10, 20];
```

📌 **Tip clave:** ¡Súper útiles para almacenar colecciones de elementos del mismo tipo o relacionados!

## 3. `Function` (Funciones) ⚙️

Aunque a veces no se ven como "datos" directamente, en JavaScript las funciones son <mark style="background: #ADCCFFA6;">**objetos de primera clase**</mark>. ¡Puedes pasarlas como argumentos, asignarlas a variables y retornarlas!

- Bloques de código diseñados para realizar una tarea específica.
- Pueden aceptar argumentos y retornar un valor.
- Son tratadas como objetos (pueden tener propiedades).

<mark style="background: #FFB8EBA6;">**Ejemplo mini:**</mark>

```js
function saludar(nombre) {
  return "Hola, " + nombre + "!";
}

let miFuncion = saludar;
console.log(miFuncion("Sofía")); // "Hola, Sofía!"
```

📌 **Tip clave:** ¡Las funciones son el corazón de la interactividad en JS!

## 4. `Date` (Fechas) 📅

El objeto `Date` se usa para trabajar con fechas y horas. ¡Desde obtener la fecha actual hasta manipularla!

- Permite crear, obtener y manipular fechas y horas.
- Basado en milisegundos desde el 1 de enero de 1970 (Epoch).

<mark style="background: #ABF7F7A6;">**Ejemplo mini:**</mark>

```js
let hoy = new Date();
console.log(hoy.getFullYear()); // Año actual

let miCumple = new Date("1995-08-15");
```

📌 **Tip clave:** ¡Manejar fechas es más fácil con el objeto `Date`!

## 5. `RegExp` (Expresiones Regulares) 🔍

Las expresiones regulares son objetos que se usan para buscar patrones dentro de cadenas de texto. ¡Piensa en ellas como un "buscador avanzado" de texto!

- Patrones para buscar o manipular texto.
- Se usan para validación de formatos, extracciones, etc.

<mark style="background: #D2B3FFA6;">**Ejemplo mini:**</mark>

```js
let patronEmail = /^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,6}$/;
let texto = "miemail@ejemplo.com";
console.log(patronEmail.test(texto)); // true
```

📌 **Tip clave:** ¡Son poderosísimas para la manipulación de strings!