---
aliases:
  - "Estructura de una App en Django: Creación y Configuración 📦"
tags:
  - django
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
---
# Estructura de una App en Django: Creación y Configuración 📦

Como vimos antes, una app en Django es como un módulo autocontenido que se encarga de una funcionalidad específica. ¡Vamos a crear una y ver cómo encaja en nuestro proyecto!
- [[#1. Crear una Nueva Aplicación (App) ➕|1. Crear una Nueva Aplicación (App) ➕]]
- [[#2. Actualizar la Configuración del Proyecto para Incluir la App ⚙️|2. Actualizar la Configuración del Proyecto para Incluir la App ⚙️]]
- [[#3. Añadir una Función de Vista Sencilla 📝|3. Añadir una Función de Vista Sencilla 📝]]
- [[#4. Crear un Archivo `urls.py` para la App (¡Buena Práctica!) 🗺️|4. Crear un Archivo `urls.py` para la App (¡Buena Práctica!) 🗺️]]
- [[#5. Incluir las URLs de la App en el `urls.py` Principal del Proyecto 🔗|5. Incluir las URLs de la App en el `urls.py` Principal del Proyecto 🔗]]
- [[#6. ¡Probarlo! 🎉|6. ¡Probarlo! 🎉]]

## 1. Crear una Nueva Aplicación (App) ➕

Una vez que tienes tu proyecto Django iniciado y estás dentro de la carpeta raíz de tu proyecto (donde está `manage.py` y el subdirectorio de tu proyecto principal), es súper fácil crear una nueva app.

- **Abre tu terminal** y asegúrate de estar en la raíz de tu proyecto (donde está `manage.py`).
- Ejecuta el comando `startapp`:
    
    ```bash
    python manage.py startapp mi_app_blog
    ```
    
    - `mi_app_blog`: Este es el **nombre de tu nueva aplicación**. ¡Elige un nombre descriptivo para su funcionalidad!

¡Boom! Django creará una nueva carpeta llamada `mi_app_blog` con la estructura básica de una app. Si echas un vistazo, verás archivos como `models.py`, `views.py`, `admin.py`, `migrations/`, etc.

## 2. Actualizar la Configuración del Proyecto para Incluir la App ⚙️

Crear la app no es suficiente. Django necesita saber que existe y que forma parte de tu proyecto. Para esto, tenemos que "registrarla" en el archivo de configuración principal.

- Abre el archivo **`settings.py`** de tu proyecto principal.
    - Por ejemplo: `mi_primer_proyecto/mi_primer_proyecto/settings.py`
- Busca la lista `INSTALLED_APPS`. Aquí es donde Django sabe qué aplicaciones están activas en tu proyecto.
- **Añade el nombre de tu nueva app** a esta lista:

```python
# mi_primer_proyecto/mi_primer_proyecto/settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'mi_app_blog', # <-- ¡Aquí está nuestra nueva app! No olvides la coma.
]
```

¡Listo! Con esto, Django ya sabe que tu aplicación `mi_app_blog` es parte del proyecto y puede empezar a interactuar con ella.

## 3. Añadir una Función de Vista Sencilla 📝

Las **vistas** son el corazón de la lógica de una app. Son funciones o clases que reciben una petición web y devuelven una respuesta. Vamos a crear una vista muy simple para nuestra `mi_app_blog`.

- Abre el archivo **`views.py`** dentro de la carpeta de tu nueva app.
    - Por ejemplo: `mi_app_blog/views.py`
- Añade el siguiente código:

```python
# mi_app_blog/views.py

from django.shortcuts import render
from django.http import HttpResponse # Importamos HttpResponse para devolver una respuesta simple

def hola_mundo(request):
    """
    Una vista muy simple que devuelve un saludo.
    """
    return HttpResponse("<h1>¡Hola desde mi primera app de Django! 👋</h1>")

def acerca_de(request):
    """
    Otra vista para una página 'Acerca de'.
    """
    return HttpResponse("<h2>Esta es una app de blog creada con Django. ¡Qué emoción! ✨</h2>")
```

Aquí hemos creado dos funciones de vista: `hola_mundo` y `acerca_de`. Ambas toman un objeto `request` como argumento (que representa la petición HTTP) y devuelven un `HttpResponse` con un poco de HTML.

## 4. Crear un Archivo `urls.py` para la App (¡Buena Práctica!) 🗺️

Aunque las URLs principales están en el `urls.py` del proyecto, es una **excelente práctica** crear un archivo `urls.py` dentro de cada aplicación para manejar sus propias URLs. Esto mantiene tu proyecto organizado y hace que tus apps sean más reutilizables.

- Dentro de la carpeta `mi_app_blog`, crea un nuevo archivo llamado **`urls.py`**.
    - Por ejemplo: `mi_app_blog/urls.py`
- Añade el siguiente código a este nuevo archivo:

```python
# mi_app_blog/urls.py

from django.urls import path
from . import views # Importamos las vistas de nuestra app

urlpatterns = [
    path('', views.hola_mundo, name='hola_mundo'), # URL para la vista 'hola_mundo'
    path('acerca/', views.acerca_de, name='acerca_de'), # URL para la vista 'acerca_de'
]
```

- **`path('', views.hola_mundo, name='hola_mundo')`**: Esta línea mapea la ruta vacía (la raíz de nuestra app, por ejemplo, `/blog/`) a la función `hola_mundo` en `views.py`. El `name='hola_mundo'` es un nombre que le damos a esta URL para poder referenciarla fácilmente en otras partes del proyecto.
- **`path('acerca/', views.acerca_de, name='acerca_de')`**: Esta mapea la ruta `/acerca/` (por ejemplo, `/blog/acerca/`) a la función `acerca_de`.

## 5. Incluir las URLs de la App en el `urls.py` Principal del Proyecto 🔗

Finalmente, necesitamos decirle al `urls.py` principal de nuestro proyecto que "incluya" las URLs de nuestra nueva app.

- Abre el archivo **`urls.py`** de tu proyecto principal.
    - Por ejemplo: `mi_primer_proyecto/mi_primer_proyecto/urls.py`
- **Importa `include`** de `django.urls`.
- Añade una nueva línea en `urlpatterns` para tu app:

```python
# mi_primer_proyecto/mi_primer_proyecto/urls.py

from django.contrib import admin
from django.urls import path, include # ¡Asegúrate de importar 'include'!

urlpatterns = [
    path('admin/', admin.site.urls),
    path('blog/', include('mi_app_blog.urls')), # <-- ¡Aquí incluimos las URLs de nuestra app!
]
```

- **`path('blog/', include('mi_app_blog.urls'))`**: Esto significa que cuando Django reciba una petición que comience con `/blog/`, le pasará el resto de la URL (después de `/blog/`) al `urls.py` de `mi_app_blog` para que lo procese.

## 6. ¡Probarlo! 🎉

¡Hemos hecho varios cambios, es hora de ver si todo funciona!

- Asegúrate de que estás en la raíz de tu proyecto (donde está `manage.py`).
- Ejecuta el servidor de desarrollo:
    
    ```bash
    python manage.py runserver
    ```
    
- Abre tu navegador y navega a:
    - `http://127.0.0.1:8000/blog/` (deberías ver "¡Hola desde mi primera app de Django! 👋")
    - `http://127.00.1:8000/blog/acerca/` (deberías ver "Esta es una app de blog creada con Django. ¡Qué emoción! ✨")