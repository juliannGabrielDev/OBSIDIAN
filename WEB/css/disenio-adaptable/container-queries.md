---
aliases:
  - Container Queries 📦🔄
tags:
  - css
  - adaptable
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-css|💄 Índice CSS]]"
Fecha: 2025-06-05
---
# Container Queries 📦🔄

Las "Container Queries" (o "consultas de contenedor") son una característica de CSS que nos permite aplicar estilos a un elemento **basándonos en el tamaño o las características de su _elemento padre_ o _contenedor_ directo**, en lugar de la ventana del navegador (viewport). ¡Esto es un cambio de juego para los componentes reutilizables! 🧱
## 1. El problema que resuelven las Container Queries 🎯

Piensa en un componente como una tarjeta de producto. Si usaras **media queries** (que consultan el _viewport_), esa tarjeta se vería igual, sin importar si está en una barra lateral estrecha o en el área principal de la página, que es más ancha. Eso es un problema porque el diseño de la tarjeta debería adaptarse al espacio que _realmente tiene_, no al tamaño de toda la pantalla.
- **Limitación de Media Queries:** Solo reaccionan al **tamaño del viewport**. Si pones un componente en un lugar más pequeño dentro de un viewport grande, el componente no se adapta por sí solo. 😢
- **Solución con Container Queries:** Ahora, los componentes pueden ser **verdaderamente modulares**. Una tarjeta puede tener un diseño compacto cuando está en un contenedor pequeño y un diseño más expandido cuando está en un contenedor más grande, ¡todo sin que el viewport cambie de tamaño!
## 2. ¿Cómo funcionan las Container Queries? 🛠️
Para usar una Container Query, necesitamos dos cosas:
### 2.1. Declarar un contenedor de consulta (Query Container) 📥

Primero, le decimos al navegador qué elemento queremos que actúe como "contenedor" que sus hijos podrán consultar. Esto se hace con la propiedad `container-type` (o su abreviatura `container`).
- **`container-type`:**
    - `inline-size`: El contenedor será consultable por su **ancho** (la dimensión en línea, que normalmente es horizontal). ¡Este es el más común y útil!
    - `size`: El contenedor será consultable tanto por su **ancho** como por su **altura** (dimensiones en línea y en bloque). Ten cuidado con este, ya que puede causar bucles si el contenido cambia el tamaño del contenedor, y el contenedor a su vez cambia el contenido.
    - `normal`: El elemento no es un contenedor de consulta de tamaño, pero sí lo es para las consultas de estilo (¡otro tipo de Container Queries!).
- **Sintaxis:**
    ```css
    .mi-componente-contenedor {
        container-type: inline-size;
        /* Opcional: darle un nombre al contenedor para referenciarlo */
        container-name: card-wrapper;
    }
    ```
    - La propiedad `container` es un **shorthand** para `container-type` y `container-name` (si lo usas).
        ```css
        .mi-componente-contenedor {
            container: card-wrapper / inline-size;
        }
        ```
        

### 2.2. Escribir la Container Query con `@container` 💬
Una vez que tenemos un contenedor, podemos usar la regla `@container` para aplicar estilos a los elementos **dentro de ese contenedor**, basándonos en las características del contenedor.
- **Sintaxis básica:**
    ```css
    @container (min-width: 400px) {
        /* Estilos que se aplican a los hijos si el contenedor es de 400px o más de ancho */
    }
    
    /* ¡También podemos usar la sintaxis de rango que vimos antes! */
    @container (width >= 400px) {
        /* Mismos estilos */
    }
    
    @container (400px <= width <= 700px) {
        /* Estilos si el contenedor está entre 400px y 700px */
    }
    ```
- **Consultando un contenedor con nombre:** Si le pusiste un nombre a tu contenedor, puedes ser más específico.
    ```css
    @container card-wrapper (width >= 700px) {
        .card-title {
            font-size: 1.8em;
        }
        .card-image {
            float: left;
            margin-right: 20px;
        }
    }
    ```
## 3. Unidades de longitud de Container Query (CQ Units) 📏

¡Esto es super cool! Las Container Queries introducen nuevas unidades de longitud que son **relativas al tamaño del contenedor de consulta**, no al viewport.

| **Unidad** | **Descripción**                                 |
| ---------- | ----------------------------------------------- |
| `cqw`      | 1% del **ancho** del contenedor de consulta.    |
| `cqh`      | 1% de la **altura** del contenedor de consulta. |
| `cqi`      | 1% del **tamaño en línea** del contenedor.      |
| `cqb`      | 1% del **tamaño en bloque** del contenedor.     |
| `cqmin`    | El valor más pequeño entre `cqi` y `cqb`.       |
| `cqmax`    | El valor más grande entre `cqi` y `cqb`.        |

- **Ejemplo de uso:**
    ```css
    @container (width >= 400px) {
        .card-title {
            /* El tamaño de fuente será el 5% del ancho del contenedor */
            font-size: 5cqw;
        }
    }
    ```
    Esto es increíble para tipografía y espaciado que escalan proporcionalmente dentro de un componente.
## 4. Diferencias clave entre Media Queries y Container Queries 🆚
Es importante entender cuándo usar cada una:

| **Característica**      | **Media Queries (@media)**                                                                                 | **Container Queries (@container)**                                                            |
| ----------------------- | ---------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| **Base de la consulta** | **Viewport** (tamaño de la ventana del navegador) y características del dispositivo.                       | **Contenedor padre** de un elemento.                                                          |
| **Alcance**             | Global (afecta a toda la página).                                                                          | Local (afecta a los hijos de un contenedor específico).                                       |
| **Uso principal**       | **Diseño general de la página** (macro-layouts).                                                           | **Diseño de componentes individuales** (micro-layouts).                                       |
| **Casos de uso**        | Cambiar el layout de la página, ocultar/mostrar secciones principales, ajustar tamaños de fuente globales. | Adaptar tarjetas, widgets, formularios, barras de navegación laterales al espacio disponible. |
| **Modularidad**         | Limitada para componentes reutilizables.                                                                   | Permite componentes verdaderamente modulares y reutilizables.                                 |
## 5. Casos de uso comunes para Container Queries 📦
- **Componentes de tarjeta:** Un diseño de tarjeta que cambia de vertical a horizontal cuando su contenedor es lo suficientemente ancho.
- **Widgets de barra lateral:** Un widget que muestra más o menos información dependiendo de si la barra lateral es estrecha o ancha.
- **Listas de productos:** Un listado de productos que cambia el número de columnas o la disposición de la información según el ancho del contenedor de la lista.
- **Formularios:** Campos de formulario que se apilan o se colocan en línea según el espacio disponible en su contenedor.