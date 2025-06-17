---
aliases:
  - Textarea con Altura Automática 📏✍️
tags:
  - css
  - tips
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-css|💄 Índice CSS]]"
Fecha: 2025-06-05
---
# Textarea con Altura Automática 📏✍️
¡Qué buena idea para formularios, comentarios o cualquier lugar donde el usuario escriba mucho! Hacer que el `textarea` crezca solito es un puntazo para la experiencia de usuario.
## 1. Configurando el `textarea` para Altura Automática ✨
Para que nuestro `textarea` ajuste su altura según el contenido que el usuario escriba, usamos la propiedad `field-sizing: content;`. ¡Es la clave mágica aquí!
```css
textarea {
    width: 400px;
    resize: none;
    field-sizing: content;
    max-height: 100px;
}
```
- **`width: 400px;`**: Le damos un **ancho fijo** al `textarea`. Esto es importante para que el texto se ajuste dentro de ese ancho y la altura pueda calcularse correctamente.
- **`resize: none;`**: Con esto, **deshabilitamos la opción de redimensionar** el `textarea` manualmente por parte del usuario. Como va a crecer automáticamente, no necesitamos que lo haga manualmente.
- **`field-sizing: content;`**: ¡Esta es la **propiedad estrella**! Esta propiedad CSS hace que el `textarea` ajuste su altura de forma **automática** para acomodar todo el contenido que se escribe dentro de él, sin necesidad de JavaScript. ¡Es una pasada!
- **`max-height: 100px;`**: Es buena idea establecer una **altura máxima**. Así, si el usuario escribe muchísimo, el `textarea` crecerá hasta esta altura y, si sigue escribiendo, aparecerá una barra de desplazamiento. Esto evita que el `textarea` ocupe toda la pantalla.

>[!NOTE] Dato Extra:
>Antes, para lograr esto, teníamos que usar JavaScript. Pero ahora, con `field-sizing: content;`, ¡CSS lo hace todo por nosotros! ¡Menos código, más fácil! 🎉
