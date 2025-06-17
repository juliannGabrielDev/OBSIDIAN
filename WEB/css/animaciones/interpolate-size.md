---
aliases:
  - "interpolate-size: allow-keywords; 📐✨"
tags:
  - css
  - animaciones
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-css|💄 Índice CSS]]"
Fecha: 2025-06-05
---
# `interpolate-size: allow-keywords;` 📐✨
¡Esta es una propiedad que nos va a ayudar un montón cuando queramos animar propiedades que usan palabras clave! Tradicionalmente, no podíamos animar directamente valores como `auto` o `fit-content` en CSS, pero ¡eso está cambiando!
## 1. Animando Tamaños con Palabras Clave 🤸‍♀️
La propiedad `interpolate-size` es parte del módulo CSS de Transiciones y Animaciones Nivel 2. Cuando la establecemos a `allow-keywords;`, estamos permitiendo que el navegador **interpole (es decir, haga una transición suave)** entre valores de tamaño que son palabras clave y valores de tamaño numéricos.
```css
.elemento-animado {
    /* Otras propiedades de estilo */
    interpolate-size: allow-keywords;
}
```
- **`interpolate-size: allow-keywords;`**: Al aplicar esto a un elemento (o al elemento padre que contenga propiedades que queremos animar), le estamos dando permiso al navegador para que intente una interpolación suave cuando una de las propiedades de tamaño (como `width`, `height`, `min-width`, `max-height`, etc.) cambie de un valor numérico a una palabra clave (como `auto`, `fit-content`, `max-content`, `min-content`) o viceversa.