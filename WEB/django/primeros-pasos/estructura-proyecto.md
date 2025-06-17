---
aliases:
  - Estructura de un proyecto Django 📚
tags:
  - django
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
---
# Estructura de un proyecto Django 📚

La estructura de un proyecto Django puede parecer un poco compleja al principio, pero una vez que la entiendes, verás que es muy lógica y te ayuda a mantener todo organizado. ¡Vamos a ello!
- [[#1. El Proyecto Principal (Project) 📂|1. El Proyecto Principal (Project) 📂]]
- [[#2. Las Aplicaciones (Apps) ✨|2. Las Aplicaciones (Apps) ✨]]
	- [[#2. Las Aplicaciones (Apps) ✨#Ejemplo de cómo se conectan las URLs del proyecto con las de las apps:|Ejemplo de cómo se conectan las URLs del proyecto con las de las apps:]]
- [[#3. Archivos Estáticos (Static Files) y Plantillas (Templates) 🖼️📄|3. Archivos Estáticos (Static Files) y Plantillas (Templates) 🖼️📄]]
- [[#Resumen General de la Estructura 📝|Resumen General de la Estructura 📝]]

## 1. El Proyecto Principal (Project) 📂

Cuando inicias un nuevo proyecto Django con `django-admin startproject nombre_del_proyecto`, se crea una carpeta principal que contiene varios archivos y directorios. Este es el "corazón" de tu proyecto.

- **`nombre_del_proyecto/`**: Esta es la carpeta raíz de tu proyecto. Dentro de ella, encontrarás:
    - **`manage.py`**: ¡Este es un archivo **crucial**! Es una utilidad de línea de comandos que te permite interactuar con tu proyecto Django. Con él puedes ejecutar el servidor de desarrollo, hacer migraciones, crear nuevas apps, etc.
    - **`nombre_del_proyecto/` (subdirectorio)**: Sí, hay otra carpeta con el mismo nombre dentro de la principal. Esta es la que contiene la configuración global de tu proyecto.
        - **`__init__.py`**: Indica que este directorio debe ser tratado como un paquete Python.
        - **`settings.py`**: Este archivo contiene todas las **configuraciones** de tu proyecto Django. Aquí defines la base de datos, las aplicaciones instaladas, las rutas de archivos estáticos, las claves secretas, ¡y mucho más! Es uno de los archivos que más vas a modificar.
        - **`urls.py`**: Aquí se definen los **patrones de URL** de tu proyecto. Es como el "mapa" que le dice a Django qué vista debe ejecutar cuando el usuario accede a una URL específica. Puedes incluir URLs de tus aplicaciones aquí.
        - **`wsgi.py`**: Este archivo es para la implementación de la **interfaz WSGI** (Web Server Gateway Interface). Ayuda a Django a servir tus aplicaciones web en un entorno de producción (por ejemplo, con Gunicorn o Apache).
        - **`asgi.py`**: Similar a `wsgi.py`, pero para la implementación de la **interfaz ASGI** (Asynchronous Server Gateway Interface), que permite el manejo de peticiones asíncronas, como WebSockets.

## 2. Las Aplicaciones (Apps) ✨

Django está diseñado para ser modular. Esto significa que puedes dividir tu proyecto en aplicaciones más pequeñas y reutilizables. Cada aplicación es una funcionalidad específica dentro de tu proyecto. Por ejemplo, en un blog, podrías tener una app para "posts", otra para "comentarios", y otra para "usuarios".

Para crear una app, usas `python manage.py startapp nombre_de_la_app`. Esto crea una nueva carpeta con la siguiente estructura básica:

- **`nombre_de_la_app/`**:
    - **`migrations/`**: Esta carpeta guarda los archivos de **migración** de la base de datos. Cada vez que haces cambios en tus modelos (tablas de la base de datos), Django crea un archivo de migración aquí para que puedas aplicar esos cambios a tu base de datos.
    - **`__init__.py`**: De nuevo, indica que es un paquete Python.
    - **`admin.py`**: Aquí registras tus modelos para que aparezcan en el **panel de administración** de Django. Esto es súper útil para gestionar tus datos sin escribir código SQL.
    - **`apps.py`**: Contiene la configuración de tu aplicación, como su nombre.
    - **`models.py`**: ¡Este es un archivo muy importante! Aquí defines tus **modelos**, que son clases Python que representan las tablas de tu base de datos. Cada atributo de la clase es una columna de la tabla.
    - **`tests.py`**: Aquí escribes tus **pruebas unitarias** para asegurar que tu código funciona como esperas. ¡Muy recomendable para mantener la calidad del código!
    - **`views.py`**: Este archivo contiene las **vistas** de tu aplicación. Las vistas son funciones o clases que reciben una petición HTTP, procesan la lógica de negocio (interactuando con los modelos), y devuelven una respuesta HTTP (generalmente renderizando una plantilla HTML).
    - **`urls.py` (opcional, pero recomendado)**: Aunque las URLs principales están en el `urls.py` del proyecto, es una **buena práctica** crear un `urls.py` dentro de cada aplicación. Esto ayuda a mantener las URLs específicas de la app organizadas y facilita su reutilización. Luego, simplemente incluyes este `urls.py` de la app en el `urls.py` principal del proyecto.

### Ejemplo de cómo se conectan las URLs del proyecto con las de las apps:

En `mi_proyecto/mi_proyecto/urls.py`:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('blog/', include('blog.urls')), # Incluyendo las URLs de la app 'blog'
]
```

En `mi_proyecto/blog/urls.py`:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.lista_posts, name='lista_posts'),
    path('<int:post_id>/', views.detalle_post, name='detalle_post'),
]
```

Con esto, si accedes a `/blog/`, Django buscará en el `urls.py` de la app `blog` y ejecutará la vista `lista_posts`. Si accedes a `/blog/123/`, ejecutará `detalle_post`.

## 3. Archivos Estáticos (Static Files) y Plantillas (Templates) 🖼️📄

Aunque no se crean por defecto dentro de cada app, es una práctica común tener directorios para archivos estáticos y plantillas.

- **`static/`**: Esta carpeta contiene todos los archivos **estáticos** de tu aplicación, como CSS, JavaScript e imágenes. Django los recopila y sirve en producción.
- **`templates/`**: Aquí se guardan las **plantillas HTML** que tus vistas renderizan. Django usa un motor de plantillas para insertar datos dinámicos en estos archivos.

Es buena práctica tener un `static/` y un `templates/` dentro de cada app, o un directorio `static/` y `templates/` a nivel de proyecto si son archivos globales. ¡Tú decides cómo organizarte!

## Resumen General de la Estructura 📝

Para que quede clarísimo, aquí tienes un repaso rápido de los puntos más importantes:

- **Proyecto Principal**: Contiene la configuración global (`settings.py`, `urls.py` principal), y el `manage.py` para interactuar con tu proyecto.
- **Aplicaciones (Apps)**: Son módulos autocontenidos que encapsulan funcionalidades específicas. Cada app tiene sus propios modelos (`models.py`), vistas (`views.py`), URLs (`urls.py`), migraciones y, opcionalmente, plantillas y archivos estáticos.
- **`manage.py`**: Tu herramienta principal para todo lo relacionado con Django (servidor, migraciones, etc.).
- **`settings.py`**: Donde configuras absolutamente todo el comportamiento de tu proyecto.
- **`urls.py`**: El mapa de URLs que conecta las peticiones con las vistas.
- **`models.py`**: Define la estructura de tu base de datos (tablas y campos).
- **`views.py`**: La lógica de tu aplicación, procesa peticiones y devuelve respuestas.