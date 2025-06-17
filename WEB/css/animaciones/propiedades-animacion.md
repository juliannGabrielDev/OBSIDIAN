---
aliases:
  - 🎬 Propiedades de Animación CSS 🎬
tags:
  - css
  - animaciones
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-css|💄 Índice CSS]]"
Fecha: 2025-06-05
---
# 🎬 Propiedades de Animación CSS 🎬
- [[#1. La Base de Todo: `@keyframes` 🔑|1. La Base de Todo: `@keyframes` 🔑]]
- [[#2. Las Propiedades `animation-*` 🛠️|2. Las Propiedades `animation-*` 🛠️]]
- [[#3. El Atajo: La Propiedad `animation` (Shortcut!)|3. El Atajo: La Propiedad `animation` (Shortcut!)]]
- [[#4. Consejos para Buenas Animaciones 👍|4. Consejos para Buenas Animaciones 👍]]
- [[#🏁 Mini Repaso Final 🏁|🏁 Mini Repaso Final 🏁]]


Las animaciones CSS nos permiten cambiar gradualmente los estilos de un elemento a lo largo del tiempo. Podemos hacer que las cosas se muevan, cambien de color, tamaño, opacidad ¡y mucho más! 🪄
## 1. La Base de Todo: `@keyframes` 🔑
Antes de poder usar las propiedades de animación, necesitamos definir la animación en sí. Esto se hace con la regla `@keyframes`.
- **`@keyframes`** define los **pasos** o **estados** de una secuencia de animación.
- Cada `@keyframes` debe tener un **nombre único** que luego usaremos para aplicarlo a un elemento.
- Dentro de `@keyframes`, definimos los estilos para puntos específicos en el tiempo usando porcentajes (de `0%` a `100%`) o las palabras clave `from` (equivalente a `0%`) y `to` (equivalente a `100%`).
**Ejemplo de `@keyframes`:**
```css
@keyframes cambiarColorYmover {
  0% { /* o from */
    background-color: blue;
    transform: translateX(0px);
  }
  50% {
    background-color: yellow;
    transform: translateX(100px);
  }
  100% { /* o to */
    background-color: red;
    transform: translateX(0px);
  }
}
```
En este ejemplo, la animación llamada `cambiarColorYmover` hará que un elemento cambie su color de fondo de azul a amarillo y luego a rojo, mientras se mueve 100px a la derecha y luego regresa.
## 2. Las Propiedades `animation-*` 🛠️
Una vez que tenemos nuestros `@keyframes` definidos, usamos varias propiedades `animation-*` en el selector del elemento que queremos animar.
Aquí te va una tabla con las más importantes:

| **Propiedad**                   | **Descripción**                                                                                          | **Ejemplo de Valor**                                                        | **Valor por Defecto**             |
| ------------------------------- | -------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- | --------------------------------- |
| **`animation-name`**            | Especifica el nombre de la regla `@keyframes` que se aplicará.                                           | `cambiarColorYmover`, `slide-in`, `none`                                    | `none`                            |
| **`animation-duration`**        | Define cuánto tiempo tomará un ciclo completo de la animación.                                           | `2s` (2 segundos), `500ms` (500 milisegundos)                               | `0s` (la animación no se ejecuta) |
| **`animation-timing-function`** | Describe cómo la animación progresa entre keyframes (aceleración).                                       | `linear`, `ease`, `ease-in`, `ease-out`, `ease-in-out`, `cubic-bezier(...)` | `ease`                            |
| **`animation-delay`**           | Especifica un retraso antes de que comience la animación.                                                | `1s` (espera 1 segundo), `-500ms` (comienza 500ms "dentro" de la animación) | `0s` (sin retraso)                |
| **`animation-iteration-count`** | Establece cuántas veces se repetirá la animación.                                                        | `3`, `infinite` (para repetición infinita)                                  | `1`                               |
| **`animation-direction`**       | Define si la animación debe reproducirse hacia adelante, hacia atrás o alternar entre ciclos.            | `normal`, `reverse`, `alternate`, `alternate-reverse`                       | `normal`                          |
| **`animation-fill-mode`**       | Especifica qué estilos aplicará la animación al elemento cuando no se está ejecutando (antes o después). | `none`, `forwards`, `backwards`, `both`                                     | `none`                            |
| **`animation-play-state`**      | Permite pausar (`paused`) o reanudar (`running`) una animación.                                          | `running`, `paused`                                                         | `running`                         |

¡Vamos a ver cada una un poquito más a fondo! 🧐
- **`animation-name`**:
    - Simplemente le dice al elemento: "¡Oye, tú vas a usar esta animación (`@keyframes`) que definí antes!"
    - Si pones `none`, no se aplica ninguna animación.
- **`animation-duration`**:
    - ¿Cuánto quieres que dure la magia? ⏳ Si pones `0s`, no verás nada.
    - Ejemplo: `animation-duration: 3s;` (la animación dura 3 segundos).
- **`animation-timing-function`**:
    - Esto es como el "ritmo" de la animación.
        - `linear`: Velocidad constante. 🐢...🐢...🐢
        - `ease`: Comienza lento, acelera en el medio, termina lento (por defecto). 🐇💨...🐢
        - `ease-in`: Comienza lento. 🐢...🐇💨
        - `ease-out`: Termina lento. 🐇💨...🐢
        - `ease-in-out`: Comienza y termina lento. 🐢...🐇💨...🐢
        - `cubic-bezier(n,n,n,n)`: ¡Para control total sobre la curva de velocidad! (para los más pros 🤓).
- **`animation-delay`**:
    - ¿Quieres que espere un poco antes de empezar? 🕒
    - Ejemplo: `animation-delay: 1s;` (la animación comenzará 1 segundo después de ser aplicada).
    - Un valor negativo puede hacer que la animación comience como si ya hubiera estado ejecutándose durante ese tiempo.
- **`animation-iteration-count`**:
    - ¿Cuántas veces se repite el show? 🔄
    - Ejemplo: `animation-iteration-count: infinite;` (¡fiesta sin fin!).
- **`animation-direction`**:
    - ¿En qué dirección va?
        - `normal`: Del 0% al 100% (por defecto). ▶️
        - `reverse`: Del 100% al 0%. ◀️
        - `alternate`: Primero normal, luego reverse, luego normal... 왔다 갔다 (ida y vuelta). 왔다 갔다
        - `alternate-reverse`: Primero reverse, luego normal, luego reverse... ◀️▶️◀️▶️
- **`animation-fill-mode`**:
    - Define qué pasa con los estilos del elemento **antes** de que empiece la animación (si hay un `animation-delay`) o **después** de que termine.
        - `none`: No aplica estilos de la animación fuera de su ejecución (el elemento vuelve a sus estilos originales).
        - `forwards`: El elemento conserva los estilos del último keyframe (`100%` o `0%` si es `reverse`) cuando la animación termina. ➡️⏹️
        - `backwards`: El elemento obtiene los estilos del primer keyframe (`0%` o `100%` si es `reverse`) durante el `animation-delay`. ⏹️⬅️
        - `both`: Aplica las reglas de `forwards` y `backwards`. ↔️
- **`animation-play-state`**:
    - ¿Necesitas una pausa? ⏸️
    - `running`: La animación se está ejecutando.
    - `paused`: La animación está en pausa. (Muy útil para controlar con JavaScript, por ejemplo, al pasar el mouse).
## 3. El Atajo: La Propiedad `animation` (Shortcut!)
Para no escribir todas las propiedades una por una, ¡existe un atajo! शॉर्टकट (shorthand). La propiedad `animation` te permite establecer la mayoría de estas propiedades en una sola línea. ¡Súper práctico!
El orden general recomendado (aunque es flexible para la mayoría) es:
```css
.shortcut {
	animation: [name] [duration] [timing-function] [delay] [iteration-count] [direction] [fill-mode] [play-state];
}
```
**Ejemplo usando el atajo:**
```css
.mi-elemento {
  /* Usando propiedades individuales */
  /*
  animation-name: cambiarColorYmover;
  animation-duration: 3s;
  animation-timing-function: ease-in-out;
  animation-delay: 0.5s;
  animation-iteration-count: infinite;
  animation-direction: alternate;
  animation-fill-mode: forwards;
  */

  /* Usando el atajo animation */
  animation: cambiarColorYmover 3s ease-in-out 0.5s infinite alternate forwards;
}
```
¡Mucho más compacto! 😎 Solo necesitas especificar los valores que quieres usar. Los que no pongas, tomarán su valor por defecto. `animation-name` y `animation-duration` suelen ser los mínimos necesarios para que algo visible ocurra.
## 4. Consejos para Buenas Animaciones 👍
- **Prioriza el Rendimiento:** Animar propiedades como `transform` (`translate`, `scale`, `rotate`) y `opacity` suele ser más performante porque el navegador las puede optimizar mejor (a menudo las maneja la GPU). Animar cosas como `width`, `height`, `margin`, `padding` puede causar "reflows" o "re-layouts" que son más costosos. 📈
- ** sutileza es clave:** A menudo, las animaciones sutiles y rápidas son más efectivas que las largas y exageradas.
- **Considera la Experiencia del Usuario (UX):** Las animaciones deben **mejorar** la experiencia, no frustrar ni distraer al usuario. 🎯
- **Accesibilidad:** Respeta la preferencia `prefers-reduced-motion`. Si un usuario ha indicado que prefiere movimiento reducido, tus animaciones deberían desactivarse o simplificarse.
    ```css
    @media (prefers-reduced-motion: reduce) {
      .animated-element {
        animation: none !important; /* O una animación muy sutil */
        transition: none !important; /* También para transiciones */
      }
    }
    ```
    
- **Prueba en Diferentes Navegadores:** Siempre es bueno verificar que tus animaciones se vean bien en los principales navegadores. 🌐
---
## 🏁 Mini Repaso Final 🏁
¡Repasito veloz! 🚀
- Las animaciones CSS se definen con **`@keyframes`**, donde estableces los diferentes estados de la animación.
- Se aplican a los elementos usando las propiedades **`animation-*`**:
    - `animation-name`: Nombre del `@keyframes`.
    - `animation-duration`: Cuánto dura.
    - `animation-timing-function`: Cómo acelera/desacelera.
    - `animation-delay`: Cuánto espera para empezar.
    - `animation-iteration-count`: Cuántas veces se repite.
    - `animation-direction`: Si va normal, al revés o alterna.
    - `animation-fill-mode`: Estilos antes/después de la animación.
    - `animation-play-state`: Para pausar/reanudar.
- Existe la propiedad **atajo `animation`** para definir la mayoría de estas en una sola línea.
- Anima `transform` y `opacity` para mejor **rendimiento**.
- ¡No olvides la **accesibilidad** y la **experiencia del usuario**!

¡Y listo! Con estas propiedades, puedes empezar a crear efectos visuales geniales para tus páginas web. ¡Ahora a practicar y darles vida a tus diseños! 💻💫