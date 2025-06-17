---
aliases:
  - Formas Creativas con CSS 🌊
tags:
  - css
  - tips
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-css|💄 Índice CSS]]"
Fecha: 2025-06-05
---
# Formas Creativas con CSS 🌊
¡Hoy repasamos cómo crear algunas formas súper interesantes para nuestras secciones web usando solo CSS! Esto es genial para darle un toque más dinámico a nuestras páginas. 😎
## 1. Diagonal 📐
La forma **diagonal** es perfecta para crear un efecto de "corte" o inclinación en nuestras secciones. ¡Le da un toque moderno!
- **¿Cómo funciona?**
    - Usamos la propiedad `transform: skewY()` en un pseudo-elemento (`::after`) para inclinar el fondo de la sección.
    - Definimos un ángulo de inclinación con una variable CSS, `--skew-angle`, lo que lo hace muy fácil de personalizar.
    - ¡Importante! La propiedad `isolation: isolate` en el contenedor principal ayuda a que el `z-index` del pseudo-elemento funcione correctamente y se quede por debajo del contenido.
- **Puntos clave:**
    - Se utiliza un **pseudo-elemento** (`::after`) para el fondo inclinado.
    - La propiedad `transform: skewY()` es la magia detrás de la inclinación.
    - `position: relative` e `isolation: isolate` en el elemento principal son cruciales.
- **Ejemplo de Código (lo más relevante):**
    ```css
    .diagonal {
        --skew-angle: -2deg; /* ¡Podemos cambiar este valor! */
        position: relative;
        isolation: isolate;
    }
    
    .diagonal::after {
        content: "";
        background-image: var(--bg); /* Un degradado chulo */
        position: absolute;
        inset: 0;
        transform: skewY(var(--skew-angle)); /* ¡Aquí la inclinación! */
        z-index: -1; /* Para que quede detrás del contenido */
    }
    ```
## 2. Spikes (Picos) ⛰️
¡Las formas de **picos** son increíbles para simular dientes de sierra o montañas en los bordes de una sección! 🤩
- **¿Cómo funciona?**
    - Se usan **dos pseudo-elementos**, `::before` y `::after`, uno para los picos de arriba y otro para los de abajo.
    - La clave está en la propiedad `mask-image` que usa una imagen SVG de un triángulo (`triangle.svg`).
    - `mask-size` y `mask-repeat: repeat-x` hacen que los triángulos se repitan a lo largo del borde.
    - Para los picos de abajo, simplemente rotamos el pseudo-elemento `::after` media vuelta (`rotate(0.5turn)`).
- **Puntos clave:**
    - Uso de **pseudo-elementos** y **SVG** como máscara (`mask-image`).
    - `mask-repeat: repeat-x` para que los picos se repitan horizontalmente.
    - `transform: rotate()` para los picos invertidos.
- **Ejemplo de Código (lo más relevante):**
    ```css
    .spikes {
        --spike-color: var(--body-bg); /* Color de los picos */
        --spike-width: 30px;
        --spike-height: 30px;
        position: relative;
        background-image: linear-gradient(to right, #fdc830, #f37335); /* Fondo de la sección */
    }
    .spikes::before,
    .spikes::after {
        content: "";
        position: absolute;
        width: 100%;
        height: var(--spike-height);
        background-color: var(--body-bg);
        mask-image: url("assets/triangle.svg"); /* ¡El SVG del triángulo! */
        mask-size: var(--spike-width) var(--spike-height);
        mask-repeat: repeat-x;
    }
    .spikes::before {
        top: 0; /* Picos de arriba */
    }
    .spikes::after {
        bottom: 0;
        transform: rotate(0.5turn); /* Picos de abajo, rotados */
    }
    ```
## 3. Wavy (Ondulado) 🌊
¡Esta es mi favorita! Las formas **onduladas** son geniales para darle un toque orgánico y fluido a nuestras secciones. ¡Parece magia! ✨
- **¿Cómo funciona?**
    - Aquí la cosa se pone un poco más avanzada, ¡usamos `mask` con **gradientes radiales**!
    - Definimos un montón de `radial-gradient` en una variable `--mask` para crear el patrón de ondas.
    - La propiedad `mask` (o `-webkit-mask` para compatibilidad) aplica este patrón como una máscara a la sección, haciendo que solo las áreas del patrón sean visibles.
- **Puntos clave:**
    - Uso de la propiedad `mask` (o `-webkit-mask`).
    - Se utilizan múltiples `radial-gradient` para construir la forma ondulada.
    - Es un poco más complejo, ¡pero el resultado es asombroso!
- **Ejemplo de Código (lo más relevante):**
    ```css
    .wavy {
        position: relative;
        background-image: linear-gradient(to right, #00f260, #0575e6); /* Fondo */
        --mask: radial-gradient(44.6px at 50% 63px, #000 99%, #0000 101%)
                calc(50% - 60px) 0/120px 51% repeat-x,
            radial-gradient(44.6px at 50% -33px, #0000 99%, #000 101%) 50% 30px/120px
                calc(51% - 30px) repeat-x,
            radial-gradient(44.6px at 50% calc(100% - 63px), #000 99%, #0000 101%) 50%
                100%/120px 51% repeat-x,
            radial-gradient(44.6px at 50% calc(100% + 33px), #0000 99%, #000 101%)
                calc(50% - 60px) calc(100% - 30px) / 120px calc(51% - 30px) repeat-x; /* ¡Toda la magia de las ondas! */
        -webkit-mask: var(--mask);
        mask: var(--mask);
    }
    ```
---
### Mini Repaso General 📝

¡En resumen, estas notas nos muestran cómo podemos hacer nuestras secciones web mucho más interesantes con CSS! Vimos tres formas principales: **diagonal** (con `skewY`), **spikes** (usando `mask-image` con SVG y pseudo-elementos), y **wavy** (con máscaras de `radial-gradient`). Todas estas técnicas nos permiten ir más allá de los cuadrados y rectángulos aburridos, ¡haciendo que el diseño web sea aún más divertido y creativo!