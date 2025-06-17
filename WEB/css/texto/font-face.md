---
aliases:
  - font-face ✒️
tags:
  - css
  - texto
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-css|💄 Índice CSS]]"
Fecha: 2025-06-05
---
# ¡@font-face: Tu Mejor Amigo para Fuentes Personalizadas! ✒️

¡Hola, compañero/a! ¿Cansado de las mismas fuentes aburridas en tus webs? Con **`@font-face`**, puedes usar cualquier fuente que quieras, ¡sin depender de si el usuario la tiene instalada! Es como incrustar la fuente directamente en tu página. 🚀

---
## 1. ¿Qué es `@font-face`?
**`@font-face`** es una regla CSS que te permite cargar fuentes personalizadas en tu sitio web. En lugar de usar solo las fuentes que ya vienen preinstaladas en el sistema de los usuarios (como Arial o Times New Roman), `@font-face` le dice al navegador dónde encontrar el archivo de la fuente y cómo debe usarse. ¡Así, tu diseño se verá exactamente como quieres en cualquier dispositivo! ✨
- **Libertad creativa:** Usa fuentes únicas para darle personalidad a tu web.
- **Consistencia:** Asegura que todos los usuarios vean la misma fuente, sin importar su sistema operativo.
- **Mejora de diseño:** Eleva la estética y la legibilidad de tu contenido.
**Tip clave:** ¡Es esencial para un diseño web con identidad propia! 📌

---
## 2. ¿Cómo se Usa `@font-face`?
La regla `@font-face` necesita al menos dos propiedades para funcionar:
1. **`font-family`**: Define el **nombre** que le darás a tu fuente personalizada. ¡Es el nombre que usarás después en tu CSS para aplicar la fuente!
2. **`src`**: Indica la **ruta** al archivo de la fuente. Aquí es donde le dices al navegador dónde encontrarla. Puedes especificar múltiples formatos para asegurar compatibilidad.
Aquí tienes la estructura básica:
```css
@font-face {
  font-family: 'MiFuenteSuperCool'; /* Dale un nombre a tu fuente */
  src: url('ruta/a/mi-fuente.woff2') format('woff2'), /* El archivo de la fuente y su formato */
       url('ruta/a/mi-fuente.woff') format('woff');   /* Opciones para compatibilidad */
  /* Puedes añadir más propiedades aquí (ver abajo) */
}
```
### Formatos de Fuente Comunes
Es crucial ofrecer varios formatos para que la fuente funcione en la mayoría de los navegadores:

| **Formato** | **Extensión** | **¿Para qué sirve?**                              |
| ----------- | ------------- | ------------------------------------------------- |
| **WOFF2**   | `.woff2`      | ¡El más nuevo y eficiente! Excelente compresión.  |
| **WOFF**    | `.woff`       | Buen soporte, sucesor de TTF/OTF.                 |
| TTF         | `.ttf`        | TrueType, buen soporte pero archivos más grandes. |
| OTF         | `.otf`        | OpenType, similar a TTF.                          |
| SVG         | `.svg`        | Para navegadores antiguos (SVG Fonts).            |
| EOT         | `.eot`        | Para Internet Explorer antiguos.                  |

**Tip clave:** ¡Siempre incluye al menos `.woff2` y `.woff` para una buena cobertura! 🌐

---
## 3. Propiedades Adicionales (¡Hazla más útil!)

Además de `font-family` y `src`, puedes usar otras propiedades para controlar cómo se comporta tu fuente:

- **`font-weight`**: Define el **grosor** de la fuente (ej. `normal`, `bold`, `400`, `700`). Útil si tienes varias variantes de grosor de la misma fuente.
- **`font-style`**: Define el **estilo** (ej. `normal`, `italic`, `oblique`).
- **`font-display`**: Controla cómo se renderiza la fuente mientras se carga (¡muy importante para el rendimiento!).
    - **`auto`**: Comportamiento por defecto del navegador.
    - **`block`**: El texto no se muestra hasta que la fuente se carga. Puede causar un "flash de texto invisible" (FOIT).
    - **`swap`**: El texto se muestra inmediatamente con una fuente de respaldo y se "intercambia" cuando la fuente personalizada está lista. ¡Lo más común y recomendado!
    - **`fallback`**: Similar a `swap`, pero con un período de bloqueo muy corto.
    - **`optional`**: Si la fuente se carga rápido, se usa; si no, se usa la de respaldo.
**Ejemplo completo:**
```css
@font-face {
  font-family: 'Open Sans';
  src: url('fonts/OpenSans-Regular.woff2') format('woff2'),
       url('fonts/OpenSans-Regular.woff') format('woff');
  font-weight: 400; /* Para el grosor normal */
  font-style: normal;
  font-display: swap; /* ¡Muestra el texto con una fuente de respaldo hasta que cargue! */
}

@font-face {
  font-family: 'Open Sans'; /* ¡Mismo nombre de familia para el negrita! */
  src: url('fonts/OpenSans-Bold.woff2') format('woff2'),
       url('fonts/OpenSans-Bold.woff') format('woff');
  font-weight: 700; /* Para el grosor negrita */
  font-style: normal;
  font-display: swap;
}

/* Ahora puedes usarla en tu CSS */
body {
  font-family: 'Open Sans', Arial, sans-serif; /* Si no carga, usa Arial o genérica */
}

h1 {
  font-weight: 700; /* Esto seleccionará la variante negrita de 'Open Sans' */
}
```
**Tip clave:** ¡Usa `font-display: swap;` para una mejor experiencia de usuario y evitar el FOIT! 💨