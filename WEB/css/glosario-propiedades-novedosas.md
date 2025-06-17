---
aliases:
  - Propiedades CSS Novedosas ✨
tags:
  - css
breadcrumb:
  - "[[indice-css|💄 Índice CSS]]"
---
# Propiedades CSS Novedosas ✨
¡Qué emoción ver estas propiedades nuevas que nos hacen la vida más fácil en CSS! Vamos a darles un vistazo rápido.
## 1. `margin-inline`, `margin-block` ↔️↕️
Estas propiedades son como un atajo para los márgenes. `margin-inline` controla los márgenes horizontales (izquierda y derecha en un contexto de escritura de izquierda a derecha), y `margin-block` los verticales (arriba y abajo). ¡Súper útil para el espaciado!
```css
div {
	margin-inline: auto; /* Centra horizontalmente el div */
	margin-block: auto; /* Centra verticalmente el div */
}
```
## 2. `aspect-ratio` 🎬
¿Quieres que tus videos o imágenes mantengan una proporción específica? ¡`aspect-ratio` es la solución! Así evitas que se deformen.
```css
.video {
    aspect-ratio: 16/9; /* Mantiene una proporción de 16:9 */
}
```
## 3. `accent-color` 🎨
¡Dale un toque de color a los elementos de tu formulario! `accent-color` te permite cambiar el color de los checkboxes, radio buttons y otros controles. ¡Personalización a tope!
```css
:root {
    accent-color: purple; /* Tiñe los elementos de formulario de morado */
}
```
## 4. `text-underline-offset` 📏
Si no te gusta cómo queda la línea del subrayado pegada al texto, `text-underline-offset` te permite separarla. ¡Un pequeño detalle que hace la diferencia!
```css
a:not([class]) {
    text-underline-offset: 0.25em; /* Separa el subrayado del texto */
}
```
## 5. `width: fit-content` 📦
Con `fit-content`, un elemento tomará el ancho de su contenido. ¡Ideal para botones o cajas que no quieres que ocupen todo el espacio disponible!
```css
.fit-content {
  width: fit-content; /* El ancho se ajusta al contenido */
}
```
## 6. `overscroll-behavior` 🚀
¡Evita el molesto "scroll de la página principal" cuando llegas al final de un área con scroll! `overscroll-behavior: contain` lo aísla y mantiene el scroll dentro de su contenedor. ¡Adiós a los saltos inesperados!
```css
.sidebar, .article {
  overscroll-behavior: contain; /* Evita que el scroll afecte la página principal */
}
```
## 7. `text-wrap` 📝
¡Esto es genial para evitar "huérfanas" (una sola palabra en una línea)! `text-wrap` tiene dos valores:

- `balance`: Intenta equilibrar el número de caracteres por línea, ideal para títulos.
- `pretty`: Se enfoca en evitar las palabras huérfanas en bloques de texto más largos.
```css
:is(h1, h2, h3, h4, .text-balance) {
  text-wrap: balance; /* Equilibra las líneas en títulos */
}

p {
  text-wrap: pretty; /* Evita huérfanas en párrafos */
}
```
## 8. `scrollbar-gutter` 🛤️
¿Has notado cómo el contenido "salta" cuando aparece la barra de desplazamiento? `scrollbar-gutter` reserva el espacio para la barra, manteniendo tu diseño estable. ¡Diseño más limpio!
```css
.contenedor {
  overflow: auto;
  scrollbar-gutter: stable; /* Mantiene el espacio para la barra de desplazamiento */
}
```
## 9. `scrollbar-color`, `scrollbar-width` 🌈↔️
¡Personaliza tus barras de desplazamiento! Con `scrollbar-color` le das color al pulgar y al riel, y con `scrollbar-width` controlas su grosor. ¡Estilo a tope!
```css
body {
    scrollbar-color: red blue; /* El pulgar es rojo, el riel es azul */
    scrollbar-width: auto; /* Puede ser none, thin o auto */
}
```
## 10. `white-space` 🚫↔️
¡Controla cómo se manejan los espacios en blanco! `white-space: nowrap` es súper útil para evitar que el texto se divida en varias líneas, manteniéndolo todo en una sola.
```css
span {
    white-space: nowrap; /* Evita que el texto se divida en líneas */
}
```