---
aliases:
  - Mapeando Objetos Modelo a Plantillas 🚀
tags:
  - django
  - templates
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
Fecha: 2025-06-03
---
# De la Base de Datos a la Web: Mapeando Objetos Modelo a Plantillas en Django 🚀
Recordemos: las plantillas nos sirven para mostrar datos dinámicos. Muy a menudo, esos datos son información que hemos guardado en nuestra base de datos usando los modelos de Django. Por ejemplo, una lista de artículos de un blog, los detalles de un producto, el perfil de un usuario, etc.
## 1. El Proceso General: Modelo ➡️ Vista ➡️ Plantilla 🗺️
El flujo para llevar los datos de tus modelos a una página web suele ser así:
1. **Modelo (`models.py`)**: Define la estructura de tus datos (tus tablas en la base de datos).
2. **Vista (`views.py`)**:
    - Usa el **ORM** (Mapeador Objeto-Relacional) de Django para hacer consultas a la base de datos y obtener objetos de tus modelos.
    - Prepara estos objetos (o listas de objetos) en un **diccionario de contexto**.
    - Llama a `render()` para pasar el contexto a una plantilla.
3. **Plantilla (`.html`)**:
    - Recibe el contexto.
    - Usa el Lenguaje de Plantillas de Django (DTL) para acceder a los atributos de los objetos modelo y mostrarlos.
    - Si recibe una lista de objetos, usa un bucle `{% for %}` para mostrarlos todos.
## 2. Paso 1: Tener un Modelo Definido (`models.py`) 📝
Primero, necesitas un modelo. Supongamos que tenemos una app llamada `blog` y queremos mostrar artículos.
```python
# blog/models.py
from django.db import models
from django.utils import timezone # Para la fecha

class Articulo(models.Model):
    titulo = models.CharField(max_length=200)
    contenido = models.TextField()
    fecha_publicacion = models.DateTimeField(default=timezone.now)
    # Podríamos añadir un autor, categorías, etc.

    def __str__(self):
        return self.titulo
```
<mark style="background: #FFF3A3A6;">No olvides</mark>: Después de crear o modificar modelos, siempre ejecuta:
1. `python manage.py makemigrations nombre_de_tu_app` (o solo `makemigrations` si es un modelo nuevo)
2. `python manage.py migrate`
Y, claro, ¡añade algunos datos a tu base de datos a través del admin de Django o la shell para tener algo que mostrar!
## 3. Paso 2: Consultar los Objetos del Modelo en la Vista (`views.py`) 🧐
Aquí es donde el ORM de Django entra en acción. En tu `views.py`, importas tu modelo y haces consultas.
### a. Obtener un Solo Objeto (Ej. para una página de detalle)
Para mostrar los detalles de un artículo específico, necesitas obtener solo ese objeto.
```python
# blog/views.py
from django.shortcuts import render, get_object_or_404 # ¡get_object_or_404 es muy útil!
from .models import Articulo # Importamos nuestro modelo

def detalle_articulo_view(request, articulo_id):
    # Obtenemos el artículo por su ID (pk es 'primary key').
    # Si no lo encuentra, get_object_or_404 automáticamente devuelve un error 404 (Página no encontrada).
    articulo_obj = get_object_or_404(Articulo, pk=articulo_id)
    
    contexto = {
        'articulo': articulo_obj # Pasamos el objeto al contexto
    }
    return render(request, 'blog/detalle_articulo.html', contexto)
```
### b. Obtener Múltiples Objetos (Ej. para una lista de artículos)
Para mostrar una lista de todos los artículos, o artículos que cumplen ciertos criterios.
```python
# blog/views.py (continuación)

def lista_articulos_view(request):
    # Obtenemos TODOS los artículos, ordenados por fecha de publicación descendente
    lista_de_articulos = Articulo.objects.all().order_by('-fecha_publicacion')
    
    # Ejemplo de filtro: solo artículos que contengan "Django" en el título
    # lista_de_articulos = Articulo.objects.filter(titulo__icontains='Django').order_by('-fecha_publicacion')
    
    contexto = {
        'articulos': lista_de_articulos # Pasamos el QuerySet (la lista de objetos) al contexto
    }
    return render(request, 'blog/lista_articulos.html', contexto)
```
>[!INFO] Recuerda:
>`Articulo.objects.all()`: Trae todos los objetos.
>`Articulo.objects.filter(...)`: Trae objetos que cumplen una condición.
>`Articulo.objects.get(...)`: Trae un único objeto que cumple una condición (da error si encuentra más de uno o ninguno).
>`.order_by(...)`: Ordena los resultados.
## 4. Paso 3: Mostrar los Datos del Modelo en la Plantilla (HTML) ✨
Ahora, en tus plantillas, usas DTL para acceder a esta información.
### a. Mostrar un Solo Objeto (`blog/templates/blog/detalle_articulo.html`)
Aquí, la variable `articulo` en la plantilla es el objeto `Articulo` que pasamos desde la vista.
```html
{% extends "base.html" %} {# Asumiendo que tienes un base.html #}

{% block titulo %}{{ articulo.titulo }}{% endblock titulo %}

{% block contenido %}
    <h1>{{ articulo.titulo }}</h1>
    <p class="meta">Publicado el: {{ articulo.fecha_publicacion|date:"d F Y, H:i" }}</p>
    
    <div>
        {{ articulo.contenido|linebreaks }} {# El filtro linebreaks convierte saltos de línea en <p> y <br> #}
    </div>

    <a href="{% url 'nombre_url_lista_articulos' %}">Volver a la lista</a> {# Asume que tienes una URL llamada así #}
{% endblock contenido %}
```
Accedemos a los campos del modelo usando la notación de punto: `{{ articulo.titulo }}`, `{{ articulo.contenido }}`, etc.
### b. Mostrar una Lista de Objetos (`blog/templates/blog/lista_articulos.html`)
Aquí, la variable `articulos` es el QuerySet (parecido a una lista) de objetos `Articulo`. Usamos un bucle `{% for %}`.
```html
{% extends "base.html" %}

{% block titulo %}Lista de Artículos{% endblock titulo %}

{% block contenido %}
    <h1>Nuestros Artículos</h1>
    
    {% if articulos %}
        <ul>
            {% for art in articulos %} {# 'art' será cada objeto Articulo en la iteración #}
                <li>
                    <h2>
                        <a href="{% url 'nombre_url_detalle_articulo' art.pk %}">
                            {{ art.titulo }}
                        </a>
                    </h2>
                    <p>Publicado: {{ art.fecha_publicacion|date:"d M Y" }}</p>
                    <p>{{ art.contenido|truncatewords:30 }}</p> {# Muestra un extracto #}
                </li>
            {% endfor %}
        </ul>
    {% else %}
        <p>No hay artículos para mostrar en este momento. ¡Vuelve pronto!</p>
    {% endif %}
{% endblock contenido %}
```
Dentro del bucle, `art` (o el nombre que le des) representa un objeto `Articulo` individual en cada iteración. Así puedes acceder a `{{ art.titulo }}`, `{{ art.pk }}` (la clave primaria, útil para enlaces), etc.
## 5. Ejemplo Completo (¡Juntando Todo!) 🧩
**`blog/models.py`**: (Como el de arriba)
**`blog/urls.py`**: (Necesitas configurar las URLs)
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.lista_articulos_view, name='lista_articulos'), # Asume que es la raíz de la app blog
    path('articulo/<int:articulo_id>/', views.detalle_articulo_view, name='detalle_articulo'),
]
```
(No olvides incluir estas URLs de la app `blog` en el `urls.py` principal del proyecto).
**`blog/views.py`**: (Como los ejemplos de arriba)
**`blog/templates/blog/lista_articulos.html`**: (Como el ejemplo de arriba)
**`blog/templates/blog/detalle_articulo.html`**: (Como el ejemplo de arriba)
**`templates/base.html` (Ejemplo muy básico en la raíz del proyecto):**
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>{% block titulo %}Mi Blog{% endblock %}</title>
    {% load static %}
    <link rel="stylesheet" href="{% static 'css/estilo.css' %}">
</head>
<body>
    <header><h1>El Blog de Django</h1></header>
    <main>
        {% block contenido %}{% endblock %}
    </main>
    <footer><p>&copy; {% now "Y" %}</p></footer>
</body>
</html>
```
## 6. ¿Y las Relaciones entre Modelos (ForeignKey, ManyToManyField)? 🤝

¡El DTL también las maneja de forma intuitiva!
- ForeignKey (Relación Uno a Muchos):
    Si Articulo tiene un autor = models.ForeignKey(User, on_delete=models.CASCADE), en la plantilla puedes hacer:
    ```html
    <p>Autor: {{ articulo.autor.username }}</p> 
    <p>Email del autor: {{ articulo.autor.email }}</p>
    ```
    Django sigue la relación y te da el objeto `User` relacionado.
- ManyToManyField (Relación Muchos a Muchos) o Relación Inversa de ForeignKey:
    Estos devuelven un "manager" de objetos relacionados. Necesitas usar .all (o filtrar más) y luego iterar.
    Si Articulo tiene tags = models.ManyToManyField(Tag):
    ```html
    <p>Tags:
        {% for tag_obj in articulo.tags.all %}
            <span class="tag">{{ tag_obj.nombre }}</span>
        {% empty %}
            Sin tags.
        {% endfor %}
    </p>
    ```
    Si un modelo `Categoria` tiene una relación `ForeignKey` _desde_ `Articulo` (es decir, `Articulo` tiene `categoria = ForeignKey(Categoria)`), desde un objeto `categoria_obj` puedes acceder a todos sus artículos así:
    ```html
    <h3>Artículos en {{ categoria_obj.nombre }}:</h3>
    <ul>
        {% for art in categoria_obj.articulo_set.all %} {# articulo_set es el nombre por defecto del manager inverso #}
            <li>{{ art.titulo }}</li>
        {% endfor %}
    </ul>
    ```
## 7. Mini Repaso Final 💡

1. Tus **Modelos** (`models.py`) definen la estructura de los datos en la base de datos.
2. Tus **Vistas** (`views.py`) usan el ORM de Django para **consultar** la base de datos y obtener instancias de esos modelos (objetos individuales o QuerySets/listas de objetos).
3. Las vistas luego pasan estos objetos/QuerySets a las **Plantillas** (`.html`) a través del **diccionario de contexto** en la función `render()`.
4. En las **Plantillas**, usas el DTL para acceder a los atributos de los objetos (`{{ objeto.atributo }}`) y para iterar sobre listas de objetos (`{% for objeto_individual in lista_de_objetos %}`).