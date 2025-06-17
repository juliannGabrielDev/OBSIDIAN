---
aliases:
  - Herencia de Plantillas 🧱✨
tags:
  - django
  - plantillas
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
Fecha: 2025-06-03
---
# Herencia de Plantillas en Django: ¡Construyendo Páginas Como si Fueran LEGO! 🧱✨
## 1. ¿Qué es la Herencia de Plantillas y Por Qué Mola Tanto? 😎
La **herencia de plantillas** es una técnica del Lenguaje de Plantillas de Django (DTL) que te permite definir una **estructura base común** para todas (o muchas) de las páginas de tu sitio web. Luego, en las plantillas individuales (las "hijas"), solo te preocupas por definir o modificar las partes que son únicas para esa página.
**¿Por qué es tan genial?**
- <mark style="background: #BBFABBA6;">**DRY (Don't Repeat Yourself - No te Repitas)**</mark>: ¡Este es el gran beneficio! Evitas copiar y pegar el mismo HTML (como el encabezado, la barra de navegación, el pie de página) en cada archivo `.html`. Menos código repetido = menos errores y más fácil de leer.
- **Consistencia**: Asegura que todas tus páginas tengan la misma estructura y apariencia base, lo que da una experiencia de usuario más coherente.
- **Mantenimiento Fácil**: Si necesitas cambiar algo común, como añadir un nuevo enlace en la navegación o actualizar el pie de página, solo lo haces en un lugar (la plantilla base) y ¡PUM! магиа (magia) ✨, se actualiza en todas las páginas que heredan de ella.
## 2. Las Piezas Clave: `{% extends %}` y `{% block %}` 🧩
La herencia de plantillas se basa en dos etiquetas principales del DTL:
### a. `{% extends "nombre_plantilla_base.html" %}`
- Esta etiqueta le dice a Django: "Oye, esta plantilla no empieza de cero, sino que va a usar `nombre_plantilla_base.html` como su esqueleto".
- <mark style="background: #FFB86CA6;">**Importante**</mark>: Debe ser la **primera etiqueta** en tu archivo de plantilla hija. No puede haber nada antes (ni siquiera espacios en blanco, aunque Django a veces es permisivo con esto).
- Solo puedes tener **un** `{% extends %}` por plantilla.
### b. `{% block nombre_del_bloque %}` ... `{% endblock nombre_del_bloque %}`
- **En la plantilla base (padre)**: Los `{% block ... %}` definen "agujeros" o secciones nombradas que las plantillas hijas pueden rellenar. Puedes poner contenido por defecto dentro de un bloque en la plantilla base.
    ```html
    <title>{% block titulo %}Mi Sitio Genial{% endblock titulo %}</title> 
    ```
- **En la plantilla hija**: Usas `{% block nombre_del_bloque %}` para proporcionar el contenido específico para ese bloque. Si una plantilla hija no define un bloque particular que existe en el padre, se usará el contenido de ese bloque del padre (si lo tiene).
    ```html
    {% extends "base.html" %}
    
    {% block titulo %}Página Específica - Mi Sitio Genial{% endblock titulo %}
    ```
- El `nombre_del_bloque` en la etiqueta de cierre `{% endblock nombre_del_bloque %}` es opcional, pero <mark style="background: #FFF3A3A6;">muy recomendado</mark> para que tu código sea más legible, especialmente con bloques anidados o largos.
## 3. ¿Cómo Funciona el LEGO? El Proceso de Herencia ⚙️
1. **Creas una plantilla base** (comúnmente llamada `base.html`, pero puede ser cualquier nombre). Esta contiene el esqueleto HTML principal de tu sitio: la estructura `<html>`, `<head>`, `<body>`, las partes comunes como la navegación principal, el pie de página, etc.
2. **Defines bloques** en esta plantilla base usando `{% block nombre_del_bloque %}` en las áreas donde esperas que el contenido varíe de una página a otra (ej. el título de la página, el contenido principal, scripts o estilos específicos de una página).
3. **En tus plantillas específicas** (ej. `contacto.html`, `lista_productos.html`), la primera línea será `{% extends "base.html" %}`.
4. Luego, en estas plantillas hijas, **sobrescribes los bloques** que necesites rellenar con contenido específico para esa página, usando la misma sintaxis `{% block nombre_del_bloque %} ... {% endblock %}`.
Cuando Django renderiza una plantilla hija, primero carga la plantilla padre (`base.html`). Luego, reemplaza los bloques de la plantilla padre con los bloques correspondientes definidos en la plantilla hija.
## 4. Ejemplo Práctico: Armando Nuestra Web 🏗️
Imagina que tenemos una carpeta `templates` en la raíz de nuestro proyecto.
**`templates/base.html` (Plantilla Base):**
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block titulo %}Mi Super Tienda Online{% endblock titulo %}</title>
    {% load static %}
    <link rel="stylesheet" href="{% static 'css/estilo_base.css' %}">
    {% block css_adicional %}{% endblock css_adicional %}
</head>
<body>
    <header>
        <div class="logo">MiTienda</div>
        <nav>
            <a href="{% url 'inicio' %}">Inicio</a>
            <a href="{% url 'productos_lista' %}">Productos</a>
            <a href="{% url 'contacto' %}">Contacto</a>
        </nav>
    </header>

    <main class="container">
        {% block contenido %}
            <p>Bienvenido a nuestra tienda. ¡Explora nuestros productos!</p>
        {% endblock contenido %}
    </main>

    <footer>
        <p>&copy; {% now "Y" %} Mi Super Tienda Online. Todos los derechos reservados.</p>
        {% block scripts_adicionales %}{% endblock scripts_adicionales %}
    </footer>
</body>
</html>
```
`templates/productos/lista_productos.html` (Plantilla Hija para una app productos):
(Recuerda la estructura de carpetas: `productos/templates/productos/lista_productos.html`)
```html
{% extends "base.html" %} {# Heredamos de la base que está en el directorio raíz de templates #}
{% load static %}

{% block titulo %}Nuestros Productos - Mi Super Tienda Online{% endblock titulo %}

{% block css_adicional %}
    <link rel="stylesheet" href="{% static 'productos/css/estilo_lista.css' %}">
{% endblock css_adicional %}

{% block contenido %}
    <h2>Catálogo de Productos</h2>
    {% if lista_de_productos %}
        <ul class="lista-productos">
            {% for producto in lista_de_productos %}
                <li>
                    <a href="{% url 'detalle_producto' producto.slug %}">{{ producto.nombre }}</a>
                    <span>${{ producto.precio }}</span>
                </li>
            {% endfor %}
        </ul>
    {% else %}
        <p>¡Ups! Parece que no hay productos disponibles en este momento.</p>
    {% endif %}
{% endblock contenido %}

{% block scripts_adicionales %}
    <script src="{% static 'productos/js/filtros_productos.js' %}"></script>
{% endblock scripts_adicionales %}
```
## 5. Niveles de Herencia y `{{ block.super }}` 🚀
### a. Múltiples Niveles de Herencia
¡Puedes tener más de un nivel de herencia! Es como con los LEGO, puedes tener una pieza base, otra que se encaja en ella, y otra más encima.
- `pagina_detalle_producto.html` podría heredar de `productos/base_productos.html`.
- Y `productos/base_productos.html` podría heredar de `base.html`.
Esto es útil para secciones grandes de tu sitio que comparten una sub-estructura común, pero diferente de otras secciones.
### b. `{{ block.super }}`
A veces, no quieres reemplazar completamente el contenido de un bloque del padre, sino **añadirle algo**. Para eso está `{{ block.super }}`.
- Dentro de un bloque en una plantilla hija, `{{ block.super }}` renderizará el contenido del **mismo bloque tal como estaba definido en la plantilla padre**.
Ejemplo:
Supongamos que en base.html tenemos un bloque para scripts:
```html
{% block scripts %}
    <script src="{% static 'js/sitio_global.js' %}"></script>
{% endblock scripts %}
```
Y en una plantilla hija, queremos incluir `sitio_global.js` Y ADEMÁS un script específico de esa página:
```html
{% extends "base.html" %}

{% block scripts %}
    {{ block.super }} {# <-- Esto incluirá sitio_global.js #}
    <script src="{% static 'js/pagina_especifica.js' %}"></script>
{% endblock scripts %}
```
La página `hija.html` terminará cargando AMBOS scripts, en ese orden. ¡Magia!
## 6. Consejos y Buenas Prácticas 🌟
- **Planifica tu `base.html`**: Piensa bien en las secciones comunes y qué bloques necesitarás. Una buena base simplifica mucho el trabajo posterior.
- **Nombres de Bloque Claros**: Usa nombres descriptivos para tus bloques (ej. `{% block page_title %}`, `{% block main_content %}`, `{% block footer_scripts %}`).
- **Consistencia**: Mantén una estructura similar en cómo defines los bloques en tus plantillas base.
> [!WARNING] No Abuses de los Niveles
>Demasiados niveles de herencia (más de 2 o 3) pueden hacer que sea difícil seguir de dónde viene cada cosa. A veces, un `{% include %}` es una mejor opción para componentes pequeños y reutilizables.
## 7. Mini Repaso Final 💡

- La herencia de plantillas es tu mejor amiga para el código **DRY** (No Repetirás Código).
- Usas `{% extends "plantilla_padre.html" %}` (¡siempre al inicio!) para indicar de quién heredas.
- Defines "huecos" en la plantilla padre con `{% block nombre_bloque %}` y los rellenas en las hijas.
- Si quieres añadir al contenido del padre en vez de reemplazarlo, usa `{{ block.super }}` dentro de un bloque hijo.
- ¡Permite crear sitios web complejos de forma organizada y mantenible!