---
aliases:
  - Mejores Prácticas para Nombres de Endpoints API 🏷️
tags:
  - apis
  - teoria
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-apis|🔌 Índice APIs]]"
Fecha: 2025-06-06
---
# Mejores Prácticas para Nombres de Endpoints API 🏷️
Los _endpoints_ de las APIs son lo primero que los usuarios (y otros desarrolladores) verán. Un buen diseño no solo especifica los datos que el servidor enviará, sino que también <mark style="background: #ABF7F7A6;">comunica el propósito de la API</mark>, lo que es vital para la claridad y el éxito a largo plazo de tu proyecto.
## Convenciones de Nomenclatura Clave ✅
1. **Formato de URI en Minúsculas y Guiones** 📝
    - Siempre usa **letras minúsculas**.
    - Utiliza **guiones (`-`)** para separar múltiples palabras (ej., `/menu-items`).
    - **Evita** guiones bajos (`_`), _snake case_, _title case_ o _camel case_ para nombres de recursos, ya que pueden dificultar la lectura.
2. **Variables en Camel Case y entre Llaves** `{}` 🐫
    - Si tu API acepta una variable (como un `userId` o `orderId`), represéntala en **camel case** y envuélvela en **llaves** (`{}`).
    - **Ejemplo:** `/orders/{orderId}`, `/customers/{customerId}/orders`.
3. **Claridad y Concisión** 🗣️
    - Usa palabras **claras, concisas y significativas** en los nombres de tus APIs para que sean fáciles de entender a primera vista.
4. **Barras Oblicuas (`/`) para Relaciones Jerárquicas** 🌳
    - Para objetos relacionados, usa **barras oblicuas** para indicar su relación jerárquica.
    - **Ejemplo:** `/library/books/{bookId}/author` (un libro tiene un autor), `/customers/{customerId}/orders` (un cliente tiene pedidos).
5. **Recursos: Siempre Sustantivos, Nunca Verbos** 🖼️
    - Los _endpoints_ de la API REST deben representar **recursos** (cosas), no **acciones**. Por lo tanto, siempre usa **sustantivos** (en plural para colecciones, singular para elementos específicos).
    - **Correcto:** `/books`, `/books/{bookId}`, `/users`
    - **Incorrecto:** `/getAllBooks`, `/getUser/{userId}` (estos usan verbos)
6. **Acciones: Usa Métodos HTTP, No Verbos en la URL** ➡️
    - Las acciones (crear, leer, actualizar, eliminar) se realizan con los **métodos HTTP** (GET, POST, PUT, PATCH, DELETE), no añadiendo verbos al _endpoint_.
    - **Correcto:**
        - GET `/users/{userId}` (para leer un usuario)
        - DELETE `/users/{userId}` (para eliminar un usuario)
        - PUT/PATCH `/orders/{orderId}` (para actualizar un pedido)
    - **Incorrecto:** `/users/{userId}/delete`, `/orders/{orderId}/save`
7. **Evita Extensiones de Archivo** 🚫📄
    - **Nunca** uses extensiones de archivo (ej., `.json`, `.xml`) en tus _endpoints_.
    - **Solución:** Acepta el formato de datos esperado como un **parámetro de consulta (**_**query string parameter**_**)**.
    - **Ejemplo:** `/orders/{orderId}?format=json`, `/orders/{orderId}?format=xml`.
8. **Parámetros de Consulta para Filtrado o Búsqueda** 🔍
    - Para filtrar colecciones o realizar búsquedas, siempre acepta los datos como un **parámetro de consulta** (lo que va después del `?` en una URL).
    - **Ejemplo:** `/menu-items?category=appetizers` (para obtener solo aperitivos).
9. **Sin Barra Oblicua Final (**_**Trailing Slash**_**)** 🙅‍♀️
    - **Nunca** añadas una barra oblicua al final del _endpoint_ de la API.
    - **Correcto:** `/orders/{orderId}`, `/sports/basketball-teams`      
    - **Incorrecto:** `/orders/{orderId}/`, `/sports/basketball-teams/`
10. **Estrategia de Nomenclatura Consistente** 🔄
    - Lo más importante es seguir siempre una **estrategia de nomenclatura consistente** y adherirte a las convenciones estándar de las APIs. Esto facilita que otros desarrolladores usen y comprendan tus APIs.