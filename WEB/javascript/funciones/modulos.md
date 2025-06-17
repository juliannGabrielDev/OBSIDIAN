---
aliases:
  - Módulos 📦
tags:
  - js
  - funciones
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-javascript|⚡ Índice JavaScript]]"
Fecha: 2025-06-05
---
# Módulos: ¡Organizando Nuestro Código JavaScript! 📦
Imagínate que son como cajitas donde guardamos piezas de código relacionadas para que todo esté más ordenado y sea fácil de reutilizar. ¡Son clave para proyectos grandes!

---

## 1. ¿Qué Son los Módulos? 🧩

Piensa en los módulos como archivos de JavaScript independientes. Cada uno puede tener sus propias variables, funciones o clases. <mark style="background: #BBFABBA6;">Lo genial es que no se mezclan con otros módulos a menos que tú lo digas explícitamente.</mark> Esto evita conflictos y hace que nuestro código sea más limpio.

- **Encapsulación:** Cada módulo es un mundo. Lo que defines dentro de él es privado a menos que lo **exportes**.
- **Reutilización:** Una vez que tienes una "cajita" de código, puedes usarla en muchos lugares sin copiar y pegar. ¡Pura eficiencia!
- **Mantenibilidad:** Si hay un error o necesitas cambiar algo, sabes exactamente en qué módulo buscar.

<mark style="background: #ADCCFFA6;">¡Tip clave!</mark> Los módulos son la base para construir aplicaciones grandes y ordenadas. 🏡

---

## 2. `export`: ¡Compartiendo Nuestro Código! 🤝

El `export` es como decir: <mark style="background: #D2B3FFA6;">"¡Hey, esto de aquí quiero que otros módulos puedan usarlo!"</mark> Hay varias formas de exportar cosas:
```js
// archivo: miCalculadora.js

// Exportación individual
export const PI = 3.14159;

export function sumar(a, b) {
  return a + b;
}

// Exportación por defecto (solo una por módulo)
export default class Calculadora {
  restar(a, b) {
    return a - b;
  }
}
```

- **Exportaciones nombradas:** Exportas variables, funciones o clases por su nombre. Puedes tener varias en un módulo.
- **Exportación por defecto:** Solo puede haber una por módulo. Es lo "principal" que ese módulo ofrece.

<mark style="background: #FFF3A3A6;">¡Tip clave!</mark> Exporta solo lo que otros módulos NECESITEN. ¡Menos es más! 🚀

---

## 3. `import`: ¡Usando Código de Otros! 📥

El `import` es el otro lado de la moneda: <mark style="background: #FFB8EBA6;">"¡Quiero usar eso que exportó el otro módulo!"</mark> La forma de importar depende de cómo se exportó:
```js
// archivo: app.js

// Importando cosas nombradas
import { PI, sumar } from './miCalculadora.js';
console.log(PI); // Salida: 3.14159
console.log(sumar(5, 3)); // Salida: 8

// Importando la exportación por defecto
import MiCalculadora from './miCalculadora.js';
const calc = new MiCalculadora();
console.log(calc.restar(10, 4)); // Salida: 6

// Importando todo lo nombrado como un objeto
import * as Calculos from './miCalculadora.js';
console.log(Calculos.sumar(2, 2)); // Salida: 4
```

- **Importaciones nombradas:** Usas llaves `{}` para traer las cosas por su nombre exacto.
- **Importación por defecto:** Le puedes dar el nombre que quieras al importar, ¡sin llaves!
- **Importar todo:** Con `* as Nombre`, importas todas las exportaciones nombradas como un objeto.

<mark style="background: #FF5582A6;">¡Tip clave!</mark> Asegúrate de que la ruta del archivo (`'./miCalculadora.js'`) sea correcta. 🛣️

---

## 4. Diferencia entre CommonJS (Node.js) y ES Modules ( Navegadores y Node.js Moderno) 🌉

Es importante saber que hay dos "sabores" principales de módulos en JavaScript:

| **Característica** | **ES Modules (ESM)**                                   | **CommonJS (CJS)**                              |
| ------------------ | ------------------------------------------------------ | ----------------------------------------------- |
| **Sintaxis**       | `import`/`export`                                      | `require()`/`module.exports`                    |
| **Ejecución**      | Síncrona para la carga, asíncrona para el contenido    | Síncrona para la carga y ejecución              |
| **Uso**            | Navegadores, Node.js moderno, herramientas de frontend | Principalmente Node.js (versiones más antiguas) |
| **Ventajas**       | Árbol de dependencias estático, tree-shaking           | Simple, extendido en Node.js antiguo            |


```js
// Ejemplo CommonJS (Node.js)
// archivo: miUtil.js
function multiplicar(a, b) { return a * b; }
module.exports = { multiplicar };

// archivo: app.js
const { multiplicar } = require('./miUtil');
console.log(multiplicar(3, 3)); // Salida: 9
```

<mark style="background: #ABF7F7A6;">¡Tip clave!</mark> Hoy en día, casi siempre usarás la sintaxis `import`/`export` (ES Modules). Si trabajas con proyectos antiguos de Node.js, verás `require`/`module.exports`. ¡No te confundas! 💡