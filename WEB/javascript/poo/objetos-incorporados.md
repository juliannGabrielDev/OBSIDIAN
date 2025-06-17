---
aliases:
  - 📝 Objetos Incorporados
tags:
  - js
  - poo
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-javascript|⚡ Índice JavaScript]]"
Fecha: 2025-06-05
---
# 📝 Objetos Incorporados en JavaScript
## 1. ¿Qué son los Objetos Incorporados (Built-in)?
Son objetos que ya vienen "de fábrica" en JavaScript. No necesitas crearlos desde cero, simplemente los usas. <mark style="background: #ADCCFFA6;">Son como las herramientas básicas en tu caja de programación</mark>, listas para cualquier tarea.
- **Disponibles siempre**: No requieren ninguna instalación o librería externa.
- **Funcionalidades clave**: Ofrecen métodos y propiedades para tareas comunes.
- **Ahorran tiempo**: ¡Imagina tener que crear tu propio objeto `Math`! 😅
📌 **Tip:** Piensa en ellos como los ayudantes estándar de JavaScript. ¡Conócelos bien!
---
## 2. Los Más Comunes: Tabla Resumen 📋
Aquí te dejo una tabla con algunos de los objetos más usados para que los tengas a la mano. Son tus mejores amigos en el día a día.

| **Objeto**    | **Para qué sirve**                                                                                             | **Ejemplo Mini**               |
| ------------- | -------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| **`Object`**  | El papá de todos los objetos. Usado para crear objetos.                                                        | `const persona = {};`          |
| **`Array`**   | Para manejar listas o colecciones de datos ordenados.                                                          | `const frutas = ['🍎', '🍌'];` |
| **`String`**  | Para trabajar con texto. ¡Tiene un montón de métodos útiles!                                                   | `let saludo = "Hola";`         |
| **`Number`**  | Para manejar números, tanto enteros como decimales.                                                            | `let edad = 25;`               |
| **`Boolean`** | Representa valores de verdadero (`true`) o falso (`false`).                                                    | `let esMayor = true;`          |
| **`Date`**    | Para trabajar con fechas y horas. ¡Súper útil! 🗓️                                                             | `const hoy = new Date();`      |
| **`Math`**    | Contiene constantes y funciones matemáticas. <mark style="background: #FFF3A3A6;">No es un constructor.</mark> | `Math.random();`               |
| **`JSON`**    | Para intercambiar datos. Convierte objetos a texto y viceversa.                                                | `JSON.stringify({ a: 1 });`    |

🧠 **Tip:** No tienes que memorizar todos los métodos, pero sí saber <mark style="background: #BBFABBA6;">qué objeto usar para cada tipo de dato</mark>. La práctica hace al maestro.

---
## 3. El Objeto Global 🌍
JavaScript tiene un objeto global que contiene todas las variables y funciones globales. En el navegador, este objeto es `window`. <mark style="background: #D2B3FFA6;">Cualquier variable que declaras fuera de una función se vuelve una propiedad de `window`</mark>.
- **Acceso a todo**: Desde `window` puedes acceder a casi todo en el entorno del navegador.
- **Funciones globales**: `parseInt()`, `parseFloat()`, `isNaN()` son en realidad métodos del objeto global.

```js
var miVariableGlobal = "Estoy en todas partes";
console.log(window.miVariableGlobal); // "Estoy en todas partes"
```
✨ **Conclusión Clave:** El objeto global es el contenedor principal de tu código en el navegador. ¡Ten cuidado con no contaminarlo demasiado!