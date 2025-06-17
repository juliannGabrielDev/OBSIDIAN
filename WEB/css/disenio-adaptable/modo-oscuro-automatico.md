---
aliases:
  - Modo Oscuro Automático 🌙🌗
tags:
  - css
  - adaptable
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-css|💄 Índice CSS]]"
Fecha: 2025-06-05
---
# Modo Oscuro Automático 🌙🌗
¡Qué chulada poder hacer que nuestra web cambie de color solita según cómo tenga el usuario configurado su sistema! Esto mejora mucho la experiencia.
## 1. Activando `color-scheme` 🌗
El primer paso es decirle al navegador que nuestra página **soporta tanto el tema claro como el oscuro**. Esto lo hacemos con la propiedad `color-scheme` en el selector `:root` (que representa el elemento raíz de nuestro HTML, casi siempre `<html>`).
```css
:root {
	color-scheme: light dark; /* Indica que la página soporta modo claro y oscuro */
}
```
- **`light dark`**: Con esto, le estamos diciendo al navegador: "¡Oye, mi sitio está listo para el modo claro Y para el modo oscuro!". El navegador usará esta información para saber qué estilos aplicar automáticamente si el usuario tiene una preferencia.
## 2. Definiendo los Colores con `light-dark()` 🎨
Ahora, para que los colores cambien de verdad, usamos la función `light-dark()`. Esta función es súper práctica porque **toma dos valores**: el primero es el color para el **modo claro** y el segundo es el color para el **modo oscuro**.
```css
body {
	background-color: light-dark(white, black); /* Fondo blanco en modo claro, negro en modo oscuro */
	color: light-dark(black, white); /* Texto negro en modo claro, blanco en modo oscuro */
}
```
- **`background-color: light-dark(white, black);`**:
    - Si el sistema del usuario está en modo claro, el fondo será `white` (blanco).
    - Si el sistema del usuario está en modo oscuro, el fondo será `black` (negro).
- **`color: light-dark(black, white);`**:
    - Si el sistema del usuario está en modo claro, el color del texto será `black` (negro).
    - Si el sistema del usuario está en modo oscuro, el color del texto será `white` (blanco).

**💡 ¿Cómo funciona esto?** El navegador detecta la preferencia de tema del sistema operativo del usuario (por ejemplo, si usa el modo oscuro en Windows, macOS, Android o iOS) y aplica automáticamente los colores correspondientes definidos con `light-dark()`. ¡Es casi mágico! ✨
### ¡Mini Repaso! 🚀

Para tener un modo oscuro automático, primero le decimos al navegador que soportamos ambos temas con `color-scheme: light dark;` en `:root`. Luego, usamos la función `light-dark(valor_claro, valor_oscuro)` para definir los colores que se aplicarán según la preferencia del usuario. ¡Así nuestra web se adapta solita! 🤩