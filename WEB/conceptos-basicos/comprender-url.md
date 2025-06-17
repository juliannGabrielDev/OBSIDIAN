---
aliases:
  - Entendiendo las URLs 🌐
tags:
  - basicos
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[Indice-conceptos-basicos|💡 Índice de Conceptos Básicos]]"
---
# Entendiendo las URLs 🌐
- [[#1. ¿Qué es una URL?|1. ¿Qué es una URL?]]
- [[#2. Partes de una URL|2. Partes de una URL]]
	- [[#2. Partes de una URL#2.1 Esquema (Scheme) 🔑|2.1 Esquema (Scheme) 🔑]]
	- [[#2. Partes de una URL#2.2 Autoridad (Authority) 🏢|2.2 Autoridad (Authority) 🏢]]
	- [[#2. Partes de una URL#2.3 Ruta (Path) 📂🗺️|2.3 Ruta (Path) 📂🗺️]]
	- [[#2. Partes de una URL#2.4 Parámetros de Consulta (Query String / Query Parameters) ❓⚙️|2.4 Parámetros de Consulta (Query String / Query Parameters) ❓⚙️]]
	- [[#2. Partes de una URL#2.5 Parámetros URL (URL Parameters / Path Parameters) 🛣️📍|2.5 Parámetros URL (URL Parameters / Path Parameters) 🛣️📍]]
	- [[#2. Partes de una URL#2.6 Fragmento (Fragment) 🎯🔖|2.6 Fragmento (Fragment) 🎯🔖]]
- [[#3. Mini Repaso Final 📝✨|3. Mini Repaso Final 📝✨]]

## 1. ¿Qué es una URL?

Una **URL** (Uniform Resource Locator) es, básicamente, la **dirección** única que se utiliza para encontrar un recurso específico en Internet. Este recurso puede ser una página web, una imagen 🖼️, un video 🎬, un archivo, ¡lo que sea! Piensa en ella como la dirección postal que le das al cartero para que encuentre una casa.

Por ejemplo: `https://www.ejemplo.com/productos/ropa?tipo=camisa&color=azul#seccion-ofertas`

¡Vamos a desmenuzar esta URL para ver sus componentes! 🧐

## 2. Partes de una URL

Una URL típica tiene varias partes, cada una con una función específica.

| **Parte**            | **Ejemplo de la URL de arriba**   | **Descripción Sencilla**                                                                                            |
| -------------------- | --------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| **Esquema**          | `https`                           | Indica el **protocolo** que se debe usar. `http` (Hypertext Transfer Protocol) o `https` (HTTP Seguro) son comunes. |
| **Separador**        | `://`                             | Simplemente separa el esquema del resto de la URL.                                                                  |
| **Autoridad**        | `www.ejemplo.com`                 | Contiene el **dominio** (o host) y, a veces, el puerto.                                                             |
| ↳ **Subdominio**     | `www`                             | Parte opcional del dominio, como un "departamento" dentro del sitio principal.                                      |
| ↳ **Dominio**        | `ejemplo`                         | El nombre principal del sitio web.                                                                                  |
| ↳ **TLD**            | `.com`                            | Dominio de Nivel Superior (Top-Level Domain), como `.com`, `.org`, `.es`, etc.                                      |
| ↳ **Puerto**         | (Opcional, no está en el ejemplo) | Número que especifica el "punto de acceso" en el servidor. `80` para HTTP y `443` para HTTPS son los default.       |
| **Ruta (Path)**      | `/productos/ropa`                 | Especifica la **ubicación** del recurso dentro del servidor web, como una carpeta en tu computadora.                |
| **Consulta (Query)** | `?tipo=camisa&color=azul`         | Aquí es donde entran los **parámetros de consulta**. Comienza con `?`.                                              |
| **Fragmento**        | `#seccion-ofertas`                | Indica una **sección específica** dentro de la página. Comienza con `#`.                                            |

---

### 2.1 Esquema (Scheme) 🔑

- Es la primera parte de la URL y le dice al navegador **cómo acceder** al recurso.
- Ejemplos comunes:
    - `http`: Protocolo de Transferencia de Hipertexto (datos no encriptados).
    - `https`: Protocolo de Transferencia de Hipertexto Seguro (¡datos encriptados! 🔒 Más seguro).
    - `ftp`: Protocolo de Transferencia de Archivos (para subir o bajar archivos).
    - `mailto`: Para abrir el cliente de correo electrónico predeterminado.
    - `file`: Para acceder a un archivo local en tu computadora.

---

### 2.2 Autoridad (Authority) 🏢

- Identifica al servidor que aloja el recurso.
- Principalmente se compone de:
    - **Nombre de Dominio (o Host)**: Es el nombre legible por humanos del sitio web (ej. `www.ejemplo.com`).
        - **Subdominio**: Como `www` o `blog` en `blog.ejemplo.com`.
        - **Dominio de segundo nivel**: El nombre principal, como `ejemplo`.
        - **Dominio de nivel superior (TLD)**: Como `.com`, `.org`, `.net`, `.edu`, `.gob`.
    - **Puerto** (Opcional): Si el servidor web usa un puerto no estándar, se especifica aquí después de dos puntos (ej. `www.ejemplo.com:8080`). Si no se especifica, se usa el puerto por defecto del protocolo (`80` para `http`, `443` para `https`).

---

### 2.3 Ruta (Path) 📂🗺️

- Indica la **ubicación específica** del recurso en el servidor web.
- Es como la estructura de carpetas en tu computadora.
- Ejemplo: `/articulos/tecnologia/mejores-smartphones.html`
    - Aquí, `articulos` y `tecnologia` podrían ser como carpetas, y `mejores-smartphones.html` el archivo.

---

### 2.4 Parámetros de Consulta (Query String / Query Parameters) ❓⚙️

- ¡Esta es una parte súper importante para interactuar con las páginas web! 🤓
- Comienzan con un signo de interrogación `?` después de la ruta.
- Consisten en **pares de clave-valor** separados por un signo igual `=` (`clave=valor`).
- Si hay múltiples parámetros, se separan por un ampersand `&`.
- Se usan para **enviar datos adicionales** al servidor web.
    - Por ejemplo, para filtrar resultados de búsqueda, ordenar listas, pasar información de una página a otra, etc.
- En nuestro ejemplo: `?tipo=camisa&color=azul`
    - **Parámetro 1**: `tipo=camisa` (clave: `tipo`, valor: `camisa`)
    - **Parámetro 2**: `color=azul` (clave: `color`, valor: `azul`)
    - La página podría mostrar solo camisas azules.

Ejemplo de Parámetros de Consulta:

Imagina que buscas "laptop gamer" en una tienda online. La URL podría verse así:

`https://www.tiendaonline.com/buscar?termino=laptop+gamer&precio_max=1500`

- `termino=laptop+gamer`: Le dice al servidor qué estás buscando.
- `precio_max=1500`: Le dice al servidor que solo muestre laptops con precio máximo de 1500.

Los **parámetros de consulta** son los que comúnmente se conocen como "query parameters".

---

### 2.5 Parámetros URL (URL Parameters / Path Parameters) 🛣️📍

A veces, el término "parámetro URL" se usa de forma general para cualquier tipo de parámetro en una URL. Sin embargo, en el desarrollo web, también existe un concepto llamado **parámetros de ruta (Path Parameters)**, que a veces se confunden o se incluyen bajo el término "parámetro URL".

- Los **parámetros de ruta** son partes **variables** de la propia ruta de la URL.
- Se utilizan para identificar un recurso específico de manera "amigable" o semántica.
- No usan el `?` ni el `&` como los parámetros de consulta. Están _dentro_ de la ruta.

Ejemplo de Parámetros de Ruta:

Imagina una URL para ver el perfil de un usuario:

`https://www.redsocial.com/usuarios/juanperez`

O para ver un producto específico:

`https://www.tienda.com/productos/12345`

- En el primer caso, `juanperez` podría ser un parámetro de ruta que identifica al usuario.
- En el segundo caso, `12345` podría ser un parámetro de ruta que identifica el producto con ese ID.

El servidor web está configurado para entender que esa parte de la ruta es variable y la usa para cargar el contenido correcto.

**Diferencia clave con los parámetros de consulta:**

- **Parámetros de Ruta**: Forman parte de la estructura de la ruta. Ej: `/usuarios/{idUsuario}` donde `{idUsuario}` es el parámetro.
- **Parámetros de Consulta**: Van después del `?` y sirven para filtrar, ordenar, etc. Ej: `/usuarios?rol=admin`

Es importante no confundirlos, aunque ambos sirven para pasar información. A veces, la gente usa "parámetro URL" para referirse a cualquiera de los dos o, más comúnmente, a los **parámetros de consulta**. Si te preguntan por "parámetro URL", lo más seguro es que se refieran a los **parámetros de consulta** (`?clave=valor`).

---

### 2.6 Fragmento (Fragment) 🎯🔖

- Comienza con un signo de almohadilla `#`.
- Apunta a una **sección específica dentro de una página web** (un "ancla" HTML).
- El navegador usa el fragmento para desplazarse automáticamente a esa parte de la página después de que se carga.
- **Importante**: El fragmento es procesado únicamente por el navegador; **no se envía al servidor**.
- En nuestro ejemplo: `#seccion-ofertas`
    - El navegador intentará encontrar un elemento en la página con `id="seccion-ofertas"` y se desplazará hasta él.

## 3. Mini Repaso Final 📝✨

¡A ver si se nos quedó!

- Una **URL** es la dirección única de un recurso en Internet.
- **Partes principales**:
    - **Esquema**: El protocolo (`http`, `https`).
    - **Autoridad**: Quién aloja el recurso (dominio, puerto).
    - **Ruta**: Dónde está el recurso en el servidor.
    - **Parámetros de Consulta** (`?clave=valor&otraClave=otroValor`): Para enviar datos extra al servidor (filtrar, buscar, etc.). ¡Estos son los "parámetros de consulta" que mencionaste!
    - **Parámetros de Ruta**: Partes variables dentro de la ruta para identificar recursos (ej. `/productos/{id}`). A veces llamados "parámetros URL" de forma más específica.
    - **Fragmento** (`#seccion`): Para ir a una parte específica de la página (solo para el navegador).