---
aliases:
  - "use strict: ¡El Modo Estricto de JavaScript! 🧐"
tags:
  - js
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-javascript|⚡ Índice JavaScript]]"
Fecha: 2025-06-05
---
# `use strict`: ¡El Modo Estricto de JavaScript! 🧐

Vamos a hablar de una cosita pequeña pero poderosa en JavaScript: **`'use strict'`**. Es como activar un "modo de seguridad" para tu código, que te ayuda a evitar errores comunes y a escribir JavaScript más robusto.

---

## 1. ¿Qué es `'use strict'`? 🤔

`'use strict'` es una **directiva** (no es una declaración ni una función) que puedes poner al principio de un script o de una función. <mark style="background: #BBFABBA6;">Cuando la activas, JavaScript empieza a trabajar en un "modo estricto"</mark>, lo que significa que impone reglas más estrictas sobre cómo puedes escribir tu código. Es como subir el nivel de dificultad en un juego para ser mejor.

- **Sin `use strict`:** JavaScript es más indulgente con los errores y puede "arreglar" algunas cosas automáticamente, lo que a veces oculta problemas reales.
- **Con `use strict`:** JavaScript es menos tolerante. Si haces algo que no es buena práctica o que puede causar un error, te avisará de inmediato.

<mark style="background: #ADCCFFA6;">¡Tip clave!</mark> `'use strict'` no es un comando, ¡es una declaración de intención! 🎯

---

## 2. ¿Cómo y Dónde se Usa? ✍️

Puedes activar el modo estricto de dos maneras:

### a) Para todo el script 📄

Pon `'use strict';` al principio de tu archivo JavaScript. ¡Asegúrate de que sea la primera línea de código ejecutable!

JavaScript

```
'use strict'; // ¡Modo estricto para todo el archivo!

function miFuncion() {
  // ... código estricto
}

let x = 10;
// ...
```

### b) Para una función específica 🎬

Si solo quieres que una parte de tu código sea estricta, puedes poner `'use strict';` dentro de una función.

```js
function otraFuncion() {
  'use strict'; // ¡Modo estricto solo para esta función!

  // ... código estricto dentro de esta función
}

function funcionNormal() {
  // ... código NO estricto (a menos que el script completo sea estricto)
}
```

<mark style="background: #D2B3FFA6;">¡Tip clave!</mark> Lo más común y recomendado es usarlo al principio del script para que todo tu código se beneficie. 🌟

---

## 3. ¿Qué Hace `'use strict'`? ¡Las Reglas! 🚨

Activar el modo estricto hace varias cosas que te protegen de errores comunes o malas prácticas. Aquí te van algunas:

- **Evita variables globales accidentales:** Sin `use strict`, si olvidas `var`, `let` o `const`, JavaScript crea una variable global. Con `use strict`, ¡te da un error!    
    ```js
    'use strict';
    // sinModificador = 5; // ¡Error! ReferenceError: sinModificador is not defined
    ```
    
- **Prohíbe la asignación a propiedades de solo lectura:** No puedes cambiar valores que no deberían cambiarse.
    
- **Prohíbe duplicar nombres de parámetros:** En funciones, no puedes tener parámetros con el mismo nombre.
    
- **Hace `this` indefinido por defecto:** Dentro de funciones normales (no métodos de objetos) el valor de `this` es `undefined`, lo que evita que `this` se refiera al objeto global (`window` o `global`).
    
    ```js
    'use strict';
    function mostrarThis() {
      console.log(this); // Salida: undefined
    }
    mostrarThis();
    ```
    
- **Lanza errores en silencio de asignaciones fallidas:** Operaciones que fallarían silenciosamente ahora lanzan un error.
    

<mark style="background: #FFF3A3A6;">¡Tip clave!</mark> `'use strict'` te ayuda a escribir código más seguro y a encontrar errores antes. ✅

---

## 4. ¿Por Qué Deberías Usarlo? 👍

Aunque parezca que `'use strict'` te "limita", en realidad te hace un favor gigante:

- **Mejora la depuración:** Al detectar más errores en fases tempranas, pasas menos tiempo buscando bugs extraños.
- **Prepara para el futuro:** Las nuevas versiones de JavaScript son cada vez más estrictas. Usarlo ahora te acostumbra a las buenas prácticas.
- **Evita errores silenciosos:** Algunos errores que pasarían desapercibidos en modo normal, ahora te alertan.
- **Facilita las optimizaciones:** Los motores de JavaScript pueden optimizar mejor el código estricto porque no tienen que manejar casos "raros" o ambiguos.

<mark style="background: #FFB8EBA6;">¡Tip clave!</mark> ¡Usar `'use strict'` es señal de un desarrollador profesional! 🎓