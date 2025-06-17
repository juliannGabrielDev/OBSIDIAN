---
aliases:
  - "¡@layer: Organiza tu CSS como un Profesional! 📚"
tags:
  - css
  - general
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-css|💄 Índice CSS]]"
Fecha: 2025-06-05
---
# ¡@layer: Organiza tu CSS como un Profesional! 📚

¡Hey, compañero/a! ¿Listo para llevar la organización de tu CSS al siguiente nivel? Con **`@layer`**, puedes agrupar tus estilos en "capas" para controlar la cascada y la especificidad de una forma súper intuitiva. ¡Es como tener carpetas para tus reglas CSS! 📁

---
## 1. ¿Qué es `@layer`?
`@layer` es una regla CSS que te permite definir **capas en cascada** explícitamente. Esto significa que puedes decirle al navegador qué grupo de estilos debe tener prioridad sobre otro, ¡incluso si tienen la misma especificidad o si una regla viene después en el código! Es una forma de organizar y modular tu CSS. 🏗️
- **Organización:** Agrupa estilos relacionados (ej. base, componentes, utilidades).
- **Control de Cascada:** Las reglas en capas posteriores tienen más peso que las de capas anteriores, ¡sin importar la especificidad!
- **Modularidad:** Facilita el trabajo en equipos y la reutilización de código.
**Tip clave:** ¡`@layer` es la solución para la guerra de especificidad! 🛡️

---
## 2. ¿Cómo se Declaran las Capas?
Puedes declarar tus capas de dos maneras principales:
### 2.1. Declaración Previa (Orden Definido)
Define el orden de tus capas al principio de tu hoja de estilos. ¡Esto es lo más recomendable! Los estilos dentro de una capa declarada más tarde tendrán mayor precedencia.
```css
@layer reset, base, components, utilities; /* ¡Este es el orden de las capas! */

/* Estilos para la capa 'reset' */
@layer reset {
  * { margin: 0; padding: 0; }
}

/* Estilos para la capa 'base' */
@layer base {
  body { font-family: sans-serif; }
  h1 { font-size: 2em; }
}

/* Estilos para la capa 'components' */
@layer components {
  .button {
    padding: 10px 20px;
    background-color: blue;
    color: white;
  }
}

/* Estilos para la capa 'utilities' */
@layer utilities {
  .text-red { color: red; }
}
```
**Tip clave:** ¡Declara el orden de tus capas primero para tener un control total! 📋
### 2.2. Declaración Ad-Hoc (Según Aparición)
También puedes declarar una capa directamente cuando la usas. El orden de las capas se establece a medida que se encuentran en el código. ¡Pero ten cuidado! Si no las defines previamente, el orden dependerá de cuándo aparezca la primera regla de esa capa.
```css
@layer base {
  body { font-family: Arial; }
}

/* Más tarde en el código... */

@layer components { /* 'components' se define después de 'base' */
  .card { border: 1px solid gray; }
}

/* Y aún más tarde... */

@layer utilities { /* 'utilities' se define después de 'components' */
  .mt-10 { margin-top: 10px; }
}
```
**Tip clave:** Esta forma es más flexible, pero puede ser confusa si no tienes claro el orden de aparición. ¡La declaración previa es más robusta! 🧐

---
## 3. ¿Cómo Funciona la Cascada con `@layer`?
Aquí es donde `@layer` realmente brilla y cambia las reglas de la cascada y la especificidad:
1. **Origen de la Hoja de Estilos:** Los estilos del navegador y los del usuario siempre tienen su propia prioridad.
2. **Capas `@layer`:** Los estilos dentro de `@layer` tienen prioridad según el orden en que se declaran las capas. ¡Una regla en una capa posterior siempre anulará una regla en una capa anterior, **incluso si la regla en la capa anterior tiene más especificidad**!
3. **Estilos Fuera de `@layer`:** Los estilos que no están dentro de ninguna `@layer` se aplican _después_ de todas las capas, y tienen mayor prioridad que cualquier estilo dentro de las capas.
4. **`!important`:** Sigue siendo el rey, anulando todo, incluso dentro de `@layer`.
**Ejemplo mini:**
```css
@layer base, components; /* Orden: base < components */

@layer base {
  p {
    color: blue; /* Especificidad: 0,0,0,1 */
  }
}

@layer components {
  .my-paragraph {
    color: green; /* Especificidad: 0,0,1,0 (Más específico que 'p' de base) */
  }
}

/* Estilo fuera de cualquier capa */
p {
  color: purple; /* Especificidad: 0,0,0,1 (Igual que 'p' de base) */
}

/* En el HTML: */
<p class="my-paragraph">¡Hola!</p>
```
**Resultado:** El texto será **purple**.
1. `p` en `base` (color: blue)
2. `.my-paragraph` en `components` (color: green)
3. `p` fuera de la capa (color: purple)
Aquí, el estilo de `.my-paragraph` (verde) gana sobre `p` (azul) porque `components` está después de `base`. Pero el `p` de **fuera de la capa** (morado) gana sobre **todos** los estilos dentro de las capas, ¡sin importar la especificidad o el orden de la capa!
**Tip clave:** ¡Recuerda que los estilos fuera de `@layer` tienen la última palabra sobre los estilos _dentro_ de `@layer`! 🤯

---
### Repaso Relámpago ⚡
**`@layer`** te permite organizar tu CSS en capas, dándote un control explícito sobre la cascada. Las capas se procesan en el orden en que se declaran, y las reglas en capas posteriores tienen mayor prioridad. Sin embargo, los estilos declarados **fuera de cualquier `@layer`** siempre tienen mayor prioridad que los estilos dentro de ellas. ¡Es una herramienta potente para CSS modular y fácil de mantener!