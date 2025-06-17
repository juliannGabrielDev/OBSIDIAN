---
aliases:
  - Animaciones CSS Impulsadas por Scroll  🎬🚀
tags:
  - css
  - animaciones
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-css|💄 Índice CSS]]"
Fecha: 2025-06-05
---
# Animaciones CSS Impulsadas por Scroll  🎬🚀
- [[#1. ¿Qué Son Exactamente? 🤔|1. ¿Qué Son Exactamente? 🤔]]
- [[#2. Conceptos Clave y Cómo Funcionan ⚙️|2. Conceptos Clave y Cómo Funcionan ⚙️]]
- [[#3. Propiedades CSS Clave 🛠️|3. Propiedades CSS Clave 🛠️]]
- [[#4. Beneficios y Casos de Uso 🌟|4. Beneficios y Casos de Uso 🌟]]
- [[#5. Cosillas a Tener en Cuenta ⚠️|5. Cosillas a Tener en Cuenta ⚠️]]
- [[#🏁 Mini Repaso Final 🏁|🏁 Mini Repaso Final 🏁]]

¡A darle! 🚀 Las **animaciones impulsadas por scroll** (o _scroll-driven animations_) en CSS son una forma de animar elementos basándose en la posición de desplazamiento (scroll) del usuario en la página. Esto significa que a medida que te mueves hacia arriba o hacia abajo, ¡las cosas se mueven, cambian o aparecen! Es como darle vida a la página mientras navegas. 🤩
## 1. ¿Qué Son Exactamente? 🤔
Imagina que estás leyendo un artículo y, a medida que te desplazas, una imagen aparece suavemente desde un lado, o una barra de progreso se va llenando. ¡Eso es una animación impulsada por scroll!
- Son animaciones que se **vinculan directamente a la acción de hacer scroll**.
- A diferencia de las animaciones tradicionales que se ejecutan una vez o en bucle independientemente del scroll, estas **dependen de la posición del scroll** para progresar.
- Permiten crear **experiencias de usuario más interactivas y atractivas** visualmente. 🖼️
- Tradicionalmente, esto requería JavaScript, ¡pero ahora CSS tiene herramientas nativas para lograrlo! Esto es genial porque puede ser **más performante**.
## 2. Conceptos Clave y Cómo Funcionan ⚙️
Para entender cómo funcionan, necesitamos conocer algunos conceptos nuevos que CSS ha introducido. Principalmente se basan en **Líneas de Tiempo de Desplazamiento** (_Scroll Timelines_).
- **Línea de Tiempo de Desplazamiento (Scroll Timeline):**
    - Es el corazón de estas animaciones. En lugar de que una animación progrese con el tiempo (segundos), progresa con el **desplazamiento**.
    - Podemos pensar en la barra de scroll como una "línea de tiempo". El inicio del scroll es el 0% de la animación y el final del scroll es el 100%.
- **Tipos de Líneas de Tiempo de Desplazamiento:**
    - `scroll()`: La animación progresa según el desplazamiento dentro de un **contenedor de scroll** (scrollport). Puede ser el documento completo o un elemento específico que tenga overflow.
        - **Ejes:** Puedes definir si la animación se basa en el scroll **vertical** (`block`) o **horizontal** (`inline`). Por defecto, es `block`.
    - `view()`: La animación progresa según la **visibilidad de un elemento** dentro del viewport o un contenedor de scroll. A medida que el elemento entra o sale de la vista, la animación avanza.
        - **Fases de Visibilidad:** Se pueden definir rangos como `entry`, `exit`, `cover`, `contain`.
- **Vinculación a Animaciones CSS:**
    - Estas líneas de tiempo se vinculan a animaciones CSS estándar (definidas con `@keyframes`).
    - Se utilizan nuevas propiedades como `animation-timeline` para hacer esta vinculación.
**Ejemplo conceptual:**
Imagina una barra de progreso Horizontal:
```
|--------------------------------------|  <-- Contenedor de Scroll
^                                      ^
Inicio del scroll                Fin del scroll
(Animación al 0%)                (Animación al 100%)
```
A medida que haces scroll hacia abajo, la animación que definiste (por ejemplo, cambiar el ancho de un `<div>`) progresa.
## 3. Propiedades CSS Clave 🛠️
Para implementar estas animaciones, usaremos principalmente algunas propiedades nuevas y otras ya conocidas:
- `animation-timeline`: Esta es la propiedad estrella. Especifica la línea de tiempo que controlará la animación.
    - Ejemplo: `animation-timeline: scroll(root block);` (La animación se controla por el scroll vertical del documento principal).
    - Ejemplo: `animation-timeline: view(selector horizontal);` (La animación se controla por la visibilidad horizontal del elemento con 'selector').
- `animation-name`: El nombre de tus `@keyframes`.
- `animation-duration`: Aunque la duración real la controla el scroll, esta propiedad puede ser necesaria para que la animación sea válida, aunque su valor temporal se ignora en favor del progreso del scroll. A veces se recomienda `animation-duration: auto;` o un valor simbólico como `1s`.
- `animation-timing-function`: Cómo progresa la animación entre keyframes (ej. `linear`, `ease-in-out`).
- `@keyframes`: Donde defines los estados de tu animación (ej. `from { opacity: 0; } to { opacity: 1; }`).
**Tabla de Propiedades Importantes:**

| **Propiedad**          | **Descripción**                                                                   | **Ejemplo de Valor**                  |
| ---------------------- | --------------------------------------------------------------------------------- | ------------------------------------- |
| `animation-timeline`   | Establece la línea de tiempo de progreso de la animación.                         | `scroll()`, `view()`                  |
| `scroll-timeline-name` | (Opcional) Define un nombre para una línea de tiempo de scroll en un elemento.    | `my-custom-timeline`                  |
| `scroll-timeline-axis` | (Opcional) Define el eje de scroll (`block`, `inline`, `vertical`, `horizontal`). | `block` (para scroll vertical)        |
| `view-timeline-name`   | (Opcional) Define un nombre para una línea de tiempo de vista en un elemento.     | `my-view-timeline`                    |
| `view-timeline-axis`   | (Opcional) Define el eje de visibilidad (`block`, `inline`).                      | `block`                               |
| `view-timeline-inset`  | Ajusta los límites de la "caja de visibilidad" para `view()`.                     | `10% 0%` (10% desde arriba, 0% abajo) |

Es común usar las propiedades abreviadas como `animation` y luego especificar `animation-timeline`.
```css
/* Ejemplo Básico con scroll() */
.animated-element {
  animation-name: grow-and-fade;
  animation-duration: 1s; /* Valor simbólico, el scroll manda */
  animation-timing-function: linear;
  animation-timeline: scroll(root block); /* Animación basada en el scroll vertical del documento */
}

@keyframes grow-and-fade {
  0% {
    transform: scale(0.5);
    opacity: 0;
  }
  50% {
    opacity: 1;
  }
  100% {
    transform: scale(1.2);
    opacity: 0;
  }
}

/* Ejemplo Básico con view() */
.card {
  animation-name: slide-in;
  animation-duration: 1s; /* Valor simbólico */
  animation-timing-function: ease-out;
  animation-timeline: view(); /* Se anima cuando entra/sale de la vista */
  animation-range-start: entry 0%; /* Comienza cuando el elemento empieza a entrar */
  animation-range-end: entry 50%; /* Termina cuando el elemento ha entrado al 50% */
}

@keyframes slide-in {
  from {
    transform: translateX(-100px);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}
```
## 4. Beneficios y Casos de Uso 🌟
- **Mejora del Rendimiento:** Al ser nativas del navegador, pueden ser más fluidas y eficientes que las soluciones basadas en JavaScript que manipulan el DOM en cada evento de scroll. ⚡
- **Menos Código JavaScript:** Simplifica el desarrollo al reducir la dependencia de JS para este tipo de efectos.
- **Declarativas:** La sintaxis CSS es más declarativa, lo que puede hacer el código más fácil de entender y mantener.
- **Accesibilidad:** Es importante asegurarse de que las animaciones no dificulten la accesibilidad. Se debe respetar la preferencia `prefers-reduced-motion`. ♿
**Casos de Uso Comunes:**
- **Apariciones y Desvanecimientos:** Elementos que aparecen suavemente al hacer scroll.
- **Barras de Progreso:** Indicadores que se llenan según el scroll (ej. progreso de lectura de un artículo). 📊
- **Efectos Parallax Sencillos:** Elementos que se mueven a diferentes velocidades al hacer scroll.
- **Galerías de Imágenes Interactivas:** Imágenes que cambian o se animan al desplazarse por ellas. 📸
- **Storytelling Visual:** Guiar al usuario a través de una narrativa visual a medida que hace scroll. 📖
## 5. Cosillas a Tener en Cuenta ⚠️
- **Soporte de Navegadores:** Esta es una característica relativamente nueva. Es crucial verificar el soporte en [Can I use...](https://caniuse.com/) para `animation-timeline` y propiedades relacionadas. Chrome y Edge tienen buen soporte, Firefox está en ello.
- **Complejidad:** Aunque CSS lo hace más sencillo, animaciones muy complejas pueden seguir siendo un desafío.
- **Fallback:** Considerar qué verán los usuarios con navegadores no compatibles. Quizás una versión estática o una animación más simple con JS como fallback.
- **No Abusar:** Como con cualquier efecto visual, es importante no sobrecargar la página. Las animaciones deben mejorar la experiencia, no distraer o entorpecer. ✨
---
## 🏁 Mini Repaso Final 🏁
¡Okay, resumen rápido! 🧠
- Las **animaciones CSS impulsadas por scroll** permiten que los elementos se animen en respuesta al **desplazamiento** del usuario.
- Funcionan mediante **Líneas de Tiempo de Desplazamiento** (`scroll()`) o **Líneas de Tiempo de Vista** (`view()`).
- La propiedad clave es `animation-timeline`, que vincula una animación `@keyframes` a una de estas líneas de tiempo.
- Son **más performantes** que las soluciones JavaScript y hacen el código más **declarativo**.
- Usos típicos: efectos de aparición, barras de progreso, parallax.
- ¡Ojo con el **soporte de navegadores** y no exageres con los efectos!
¡Y eso es todo por ahora sobre las animaciones CSS impulsadas por scroll! Es una herramienta súper potente para crear webs más vivas y modernas. ¡A practicar se ha dicho! 💻💨