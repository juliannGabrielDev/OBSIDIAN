---
aliases:
  - Sombreado Doble en CSS ✨
tags:
  - css
  - tips
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-css|💄 Índice CSS]]"
Fecha: 2025-06-05
---
# Sombreado Doble en CSS ✨
¡Mira qué fácil es añadir dos sombras a un mismo elemento! Esto es súper útil para dar un efecto más profundo o un diseño más creativo.
## 1. Aplicando Sombreado Doble 👥
Para lograr un sombreado doble, simplemente **añadimos varias declaraciones de `box-shadow` separadas por comas** dentro de la misma propiedad `box-shadow`. Cada declaración representa una sombra diferente. ¡Así de sencillo!
```css
.elemento {
	box-shadow: 5px 5px var(--color-1), /* Primera sombra */
	            10px 10px var(--color-2); /* Segunda sombra */
}
```
- **`5px 5px var(--color-1)`**: Esta es la **primera sombra**.
    - `5px`: Desplazamiento horizontal de la sombra.
    - `5px`: Desplazamiento vertical de la sombra.
    - `var(--color-1)`: El color de esta sombra (¡usando una variable CSS para ser más ordenados!).
- **`10px 10px var(--color-2)`**: Y esta es la **segunda sombra**.
    - `10px`: Desplazamiento horizontal.
    - `10px`: Desplazamiento vertical.
    - `var(--color-2)`: El color de la segunda sombra.
### ¡Mini Repaso! 🚀
Para hacer un sombreado doble en CSS, solo tienes que usar la propiedad `box-shadow` y **separar cada definición de sombra con una coma**. Cada definición puede tener su propio **desplazamiento** y **color**, permitiéndote crear efectos visuales muy interesantes y con profundidad.