---
aliases:
  - Range Media Queries 📏📱
tags:
  - css
  - adaptable
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-css|💄 Índice CSS]]"
Fecha: 2025-06-05
---
# Range Media Queries 📏📱

Las "Range Media Queries" son una forma más moderna y flexible de escribir las **media queries** en CSS. Antes, usábamos `min-width` y `max-width` para definir rangos, pero ahora hay una sintaxis más limpia que nos permite hacer lo mismo e incluso más. ¡Es como una versión mejorada! 😄

```css
@media (width >= 40rem) { ... } /* 640px */
@media (width >= 48rem) { ... } /* 768px */
@media (width >= 64rem) { ... } /* 1024px */
@media (width >= 80rem) { ... } /* 1280px */
@media (width >= 96rem) { ... } /* 1536px */
```