---
aliases:
  - El Lenguaje de Plantillas 📜 de Django (DTL) 🏗️✨
tags:
  - django
  - plantillas
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
Fecha: 2025-06-03
---
# El Lenguaje de Plantillas 📜 de Django (DTL) 🏗️✨
- [[#1. ¿Qué es el DTL y por qué Existe? 🤔|1. ¿Qué es el DTL y por qué Existe? 🤔]]
- [[#2. Componentes Principales del DTL|2. Componentes Principales del DTL]]
	- [[#2. Componentes Principales del DTL#2.1. Variables `{{ ... }}`|2.1. Variables `{{ ... }}`]]
	- [[#2. Componentes Principales del DTL#2.2. Filtros `|filtro`|2.2. Filtros `|filtro`]]
	- [[#2. Componentes Principales del DTL#2.3. Etiquetas (Tags) `{% ... %}`|2.3. Etiquetas (Tags) `{% ... %}`]]
	- [[#2. Componentes Principales del DTL#2.4. Comentarios `{# ... #}`|2.4. Comentarios `{# ... #}`]]
- [[#3. Creación de Filtros y Etiquetas Personalizadas 🌟|3. Creación de Filtros y Etiquetas Personalizadas 🌟]]
- [[#4. Mini Repaso Final 💡|4. Mini Repaso Final 💡]]

## 1. ¿Qué es el DTL y por qué Existe? 🤔
El **Lenguaje de Plantillas de Django (DTL)** es el sistema que usa Django para generar HTML (o cualquier otro formato de texto) de forma dinámica. Su principal objetivo es <mark style="background: #BBFABBA6;">separar la lógica de presentación de la lógica de negocio</mark> (que vive en tus vistas de Python).
**Filosofía de Diseño Clave**:
- **NO es Python**: Aunque te recuerde a Python, el DTL es intencionalmente más simple. Esto es para evitar que pongas lógica de programación compleja en tus plantillas. ¡Las plantillas son para mostrar cosas, no para calcularlas!
- **Seguridad primero**: Viene con protección contra ataques comunes como Cross-Site Scripting (XSS) gracias al autoescapado de HTML.
## 2. Componentes Principales del DTL
El DTL tiene cuatro elementos fundamentales que usarás todo el tiempo:
### 2.1. Variables `{{ ... }}`
Las variables son marcadores de posición que se reemplazarán con valores cuando la plantilla se renderice.
- **Sintaxis**: Se escriben entre llaves dobles: `{{ nombre_de_la_variable }}`.
- **Origen del Valor**: Las variables obtienen sus valores del **contexto**, que es un diccionario que pasas desde tu vista de Django a la plantilla.
    ```python
    # views.py
    def mi_vista(request):
        contexto = {'nombre_usuario': 'Alex', 'edad': 30}
        return render(request, 'mi_plantilla.html', contexto)
    ```
    ```html
    <p>Hola, {{ nombre_usuario }}. Tienes {{ edad }} años.</p> 
    ```
- **Resolución de Variables (Dot Lookup - Búsqueda por Punto)**: Cuando usas un punto (`.`) en una variable (ej. `{{ objeto.atributo }}`), Django intenta encontrar el valor en este orden:
    1. Búsqueda de clave de diccionario: `diccionario['atributo']`
    2. Búsqueda de atributo de objeto: `objeto.atributo`
    3. Búsqueda de índice de lista: `lista[int(atributo)]`
    4. Llamada a método de objeto (si es un "callable" sin argumentos): `objeto.atributo()` .
    - Ejemplos:
        - `{{ articulo.titulo }}` (atributo)
        - `{{ perfil.configuracion.tema }}` (clave de diccionario anidada o atributo anidado)
        - `{{ mi_lista.0 }}` (primer elemento de una lista)
        - `{{ usuario.get_full_name }}` (si `get_full_name` es un método sin argumentos, se llamará y se mostrará su resultado)
- **Si una variable no existe o es inválida**: Por defecto, Django la reemplazará con una cadena vacía (`''`). Esto se puede configurar en `settings.py` si necesitas un comportamiento diferente.
### 2.2. Filtros `|filtro`
Los filtros modifican el valor de una variable antes de que se muestre. ¡Son súper útiles para formatear!
- **Sintaxis**: `{{ variable|nombre_filtro }}` o `{{ variable|nombre_filtro:argumento_del_filtro }}`.
- **Encadenamiento**: Puedes aplicar múltiples filtros en secuencia: `{{ variable|filtro1|filtro2:"argumento" }}`.
**Algunos Filtros Comunes y Útiles**:

| **Filtro**              | **Descripción**                                                                                      | **Ejemplo**                                        |
| ----------------------- | ---------------------------------------------------------------------------------------------------- | -------------------------------------------------- |
| `lower`                 | Convierte a minúsculas.                                                                              | `{{ "HOLA"`                                        |
| `upper`                 | Convierte a mayúsculas.                                                                              | `{{ "hola"`                                        |
| `capfirst`              | Primera letra en mayúscula.                                                                          | `{{ "hola mundo"`                                  |
| `title`                 | Cada palabra empieza con mayúscula.                                                                  | `{{ "hola mundo"`                                  |
| `truncatechars:N`       | Acorta a N caracteres y añade "...".                                                                 | `{{ "Texto largo..."`                              |
| `truncatewords:N`       | Acorta a N palabras y añade "...".                                                                   | `{{ "Uno dos tres cuatro"`                         |
| `length`                | Devuelve la longitud (de una cadena o lista).                                                        | `{{ mi_lista`                                      |
| `date:"D, d M Y"`       | Formatea un objeto fecha/hora.                                                                       | `{{ fecha_obj`                                     |
| `time:"H:i"`            | Formatea la parte de la hora.                                                                        | `{{ fecha_obj`                                     |
| `timesince`             | Tiempo transcurrido desde una fecha (ej. "2 horas, 3 minutos").                                      | `{{ comentario.fecha`                              |
| `add:valor`             | Suma `valor` a la variable (intenta convertir a int).                                                | `{{ cantidad`                                      |
| `floatformat:N`         | Formatea un flotante a N decimales.                                                                  | `{{ precio`                                        |
| `default:"-"`           | Valor por defecto si la variable es `False` o vacía.                                                 | `{{ telefono`                                      |
| `default_if_none:"N/A"` | Valor por defecto si la variable es `None`.                                                          | `{{ variable_puede_ser_none`                       |
| `safe`                  | <mark style="background: #FF5582A6;">Desactiva el autoescapado de HTML</mark>. ¡Usar con precaución! | `{{ contenido_html_confiable`                      |
| `escape`                | Escapa HTML (se hace por defecto, pero útil para forzar).                                            | `{{ texto_con_simbolos`                            |
| `linebreaks`            | Convierte saltos de línea en `<p>` y `<br>`.                                                         | `{{ mi_texto_multilinea`                           |
| `linebreaksbr`          | Convierte saltos de línea en `<br>`.                                                                 | `{{ mi_texto_multilinea`                           |
| `join:", "`             | Une los elementos de una lista con el separador.                                                     | `{{ lista_tags`                                    |
| `pluralize`             | Añade una "s" si el valor no es 1.                                                                   | `Tienes {{ num_mensajes }} mensaje{{ num_mensajes` |
| `pluralize:"es,es"`     | Sufijos personalizados para singular/plural.                                                         | `{{ num_items }} {{ num_items`                     |
| `filesizeformat`        | Convierte bytes a formato legible (KB, MB...).                                                       | `{{ tamano_archivo_bytes`                          |
| `slugify`               | Convierte a un "slug" (texto para URLs).                                                             | `{{ "Mi Título Genial"`                            |
### 2.3. Etiquetas (Tags) `{% ... %}`
Las etiquetas realizan la "lógica" en la plantilla. Pueden hacer bucles, condicionales, cargar información externa, heredar de otras plantillas, etc.
- **Sintaxis**: `{% nombre_tag argumentos_opcionales %}`. Algunas etiquetas necesitan una etiqueta de cierre: `{% endnombre_tag %}`.
**Etiquetas Esenciales y Sus Poderes**:
- **`{% if %}`, `{% elif %}`, `{% else %}`, `{% endif %}`** (Control de Flujo Condicional)
    - Permite mostrar contenido basado en condiciones.
    - Operadores: `==`, `!=`, `<`, `>`, `<=`, `>=`, `in`, `not in`, `is`, `is not`.
    - Operadores booleanos: `and`, `or`, `not`. <mark style="background: #FFB86CA6;">Ojo: No se pueden usar paréntesis `()` para agrupar expresiones lógicas complejas dentro de un `if` de DTL</mark>. Si necesitas lógica muy compleja, ¡hazla en la vista!
    ```html
    {% if user.is_authenticated and user.is_staff %}
        <p>Eres Staff y estás autenticado.</p>
    {% elif user.is_authenticated %}
        <p>Estás autenticado pero no eres Staff.</p>
    {% else %}
        <p>No estás autenticado.</p>
    {% endif %}
    ```
- **`{% for item in lista %}` ... `{% empty %}` ... `{% endfor %}`** (Bucles)
    - Itera sobre cada elemento de una secuencia (lista, tupla, queryset).
    - `{% empty %}`: Se muestra si la lista está vacía.
    - **Variables `forloop`**: Dentro de un bucle `for`, tienes acceso a:
        - `forloop.counter`: Contador de iteración (1-indexed).
        - `forloop.counter0`: Contador de iteración (0-indexed).
        - `forloop.revcounter`: Contador inverso (N...1).
        - `forloop.revcounter0`: Contador inverso (N-1...0).
        - `forloop.first`: `True` si es la primera iteración.
        - `forloop.last`: `True` si es la última iteración.
        - `forloop.parentloop`: Si hay bucles anidados, referencia al `forloop` del bucle padre.
    ```html
    <ul>
    {% for fruta in lista_de_frutas %}
        <li class="{% if forloop.first %}primero{% elif forloop.last %}ultimo{% endif %}">
            {{ forloop.counter }}. {{ fruta.nombre }}
        </li>
    {% empty %}
        <li>No hay frutas en la lista.</li>
    {% endfor %}
    </ul>
    ```
- **`{% extends "base.html" %}`** (Herencia de Plantillas)
    - <mark style="background: #ADCCFFA6;">Debe ser la primera etiqueta</mark> en la plantilla. Indica que esta plantilla "hereda" la estructura de `base.html`.
- **`{% block nombre_del_bloque %}` ... `{% endblock nombre_del_bloque %}`** (Herencia de Plantillas)
    - Define secciones en una plantilla base que las plantillas hijas pueden sobrescribir. El `nombre_del_bloque` en `endblock` es opcional pero buena práctica.
- **`{% include "otro_template.html" %}`** (Inclusión de Plantillas)
    - Inserta el contenido renderizado de otra plantilla.
    - `{% include "avatar.html" with user_avatar=user.profile.avatar_url size="50px" %}` (puedes pasarle un contexto específico al `include`).
- **`{% url 'nombre_de_la_ruta_url' arg1 arg2 ... %}`** (Resolución de URLs)
    - Genera dinámicamente una URL basada en el nombre de un patrón de URL definido en tu `urls.py`. ¡Esto evita URLs "hardcodeadas"!
    - Ejemplo: `<a href="{% url 'detalle_articulo' articulo.id %}">Ver artículo</a>`
- **`{% load nombre_biblioteca_tags %}`** (Carga de Bibliotecas)
    - Carga etiquetas y filtros personalizados o de bibliotecas incorporadas.
    - Ejemplo: `{% load static %}`, `{% load humanize %}` (para filtros como `intcomma`).
- **`{% static 'ruta/relativa/al/archivo.css' %}`** (Archivos Estáticos)
    - Genera la URL completa para un archivo estático (CSS, JS, imagen). Requiere `{% load static %}`.
    - Ejemplo: `<link rel="stylesheet" href="{% static 'css/estilos.css' %}">`
- **`{% csrf_token %}`** (Seguridad en Formularios)
    - <mark style="background: #BBFABBA6;">Imprescindible</mark> dentro de cualquier etiqueta `<form>` que use el método `POST`. Protege contra ataques CSRF.
- **`{% comment %}` ... `{% endcomment %}`** (Comentarios Multi-línea de DTL)
    - Todo lo que esté dentro de estas etiquetas será ignorado por el motor de plantillas.
- **`{% with expresion_compleja as nombre_mas_simple %}` ... `{% endwith %}`** (Asignación Temporal)
    - Útil para asignar el resultado de una expresión (posiblemente costosa o larga de escribir) a una variable temporal y usarla dentro del bloque `with`.
    ```html
    {% with total_items=carrito.get_total_items %}
        <p>Tienes {{ total_items }} ítem{{ total_items|pluralize }} en tu carrito.</p>
    {% endwith %}
    ```
- **`{% now "formato_fecha_hora" %}`** (Fecha y Hora Actual)
    - Muestra la fecha y/o hora actual, formateada según la cadena de formato (similar al filtro `date`).
    - Ejemplo: `Página generada el: {% now "d M Y H:i:s" %}`
- **`{% cycle 'clase-fila-par' 'clase-fila-impar' as nombre_del_ciclo %}`** (Alternar Valores)
    - Útil para alternar valores en un bucle, por ejemplo, para clases CSS en filas de una tabla.
    ```html
    {% for item in items %}
        <tr class="{% cycle 'par' 'impar' %}"><td>{{ item }}</td></tr>
    {% endfor %}
    ```
- **`{% verbatim %}` ... `{% endverbatim %}`** (Salida Literal)
    - Evita que el DTL procese el contenido entre estas etiquetas. Útil si necesitas mostrar código que usa sintaxis similar a DTL (ej. plantillas de AngularJS, Vue, etc.).
### 2.4. Comentarios `{# ... #}`
Estos son comentarios para los desarrolladores. No se renderizan en el HTML final.
- **Sintaxis**: `{# Este es un comentario de plantilla. Puede ocupar varias líneas. #}`.
## 3. Creación de Filtros y Etiquetas Personalizadas 🌟
Si las etiquetas y filtros incorporados no son suficientes, ¡Django te permite crear los tuyos! Esto es un tema un poco más avanzado, pero es bueno saber que el DTL es extensible. Se hace creando "template tags" en Python dentro de tus aplicaciones.
## 4. Mini Repaso Final 💡
- El DTL es el lenguaje de Django para la **capa de presentación**, diseñado para ser simple y seguro.
- **Variables `{{ ... }}`**: Muestran datos del contexto. Se pueden modificar con **Filtros `| ...`**.
- **Etiquetas `{% ... %}`**: Realizan lógica, bucles, herencia, cargan recursos, etc.
- **Comentarios `{# ... #}`** (o `{% comment %}`): Para notas que no se ven en la página final.
- Su objetivo es mantener la **lógica compleja en las vistas de Python** y la lógica de presentación en las plantillas.