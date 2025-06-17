---
aliases:
  - 🗺️ Tipos de Enrutamiento en Django REST Framework (DRF)
tags:
  - django
  - django-rest-framework
  - apis
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
  - "[[indice-apis|🔌 Índice APIs]]"
Fecha: 2025-06-09
---
# 🗺️ Tipos de Enrutamiento en Django REST Framework (DRF)

El enrutamiento en Django REST Framework (DRF) es el mecanismo que mapea las URLs de tu API a las vistas correspondientes que procesan las peticiones. DRF ofrece varias formas de configurar tus rutas, desde el enfoque tradicional de Django hasta potentes routers que automatizan el proceso para los `ViewSets`.

---

### 🛣️ Rutas Regulares (Estilo Django)

Este es el método fundamental y más explícito para definir rutas en tu aplicación. Se utiliza la función `path()` de Django dentro del archivo `urls.py`.

#### 1. Vistas Basadas en Funciones

Es el enfoque más simple. Se mapea una URL directamente a una función en tu `views.py`. Se usan decoradores como `@api_view` para especificar los métodos HTTP permitidos.

**`views.py`**

```python
from rest_framework.decorators import api_view
from rest_framework.response import Response

@api_view(['GET', 'POST'])
def books(request):
    if request.method == 'GET':
        return Response({'message': 'Listado de libros'})
    elif request.method == 'POST':
        return Response({'message': 'Libro creado'})
```

**`urls.py`**

```python
from django.urls import path
from . import views

urlpatterns = [
    path('books', views.books),
]
```

> [!INFO] Mapeo Directo
> 
> Con esta configuración, una petición GET o POST a /api/books será gestionada por la función books.

#### 2. Vistas Basadas en Clases (`APIView`)

Para una mejor organización y reutilización del código, puedes usar vistas basadas en clases que heredan de `APIView`. Los métodos de la clase (`get`, `post`, `put`, etc.) se mapean automáticamente a los verbos HTTP correspondientes.

**`views.py`**

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status

class BookView(APIView):
    def get(self, request, pk):
        return Response({"message": f"Mostrando el libro con id {pk}"}, status=status.HTTP_200_OK)

    def put(self, request, pk):
        return Response({"title": request.data.get('title')}, status=status.HTTP_200_OK)
```

**`urls.py`**

```python
from django.urls import path
from . import views

urlpatterns = [
    path('books/<int:pk>', views.BookView.as_view()),
]
```

> [!TIP] El método .as_view()
> 
> Es crucial llamar a .as_view() en la URLconf. Este método es el que conecta tu clase con el sistema de rutas de Django, creando una instancia de la vista para cada petición.

---

### 🤖 Enrutamiento Automático con Routers para `ViewSets`

Cuando trabajas con `ViewSets`, DRF proporciona `Routers` que generan automáticamente las patrones de URL por ti, ahorrando una cantidad significativa de código repetitivo. Un `ViewSet` es una clase que agrupa la lógica para un conjunto de vistas relacionadas (listar, crear, recuperar, actualizar, destruir).

#### `ViewSet` de Ejemplo

Consideremos el siguiente `ViewSet`:

```python
# views.py
from rest_framework import viewsets, status
from rest_framework.response import Response

class BookViewSet(viewsets.ViewSet):
    def list(self, request):
        return Response({"message": "Todos los libros"}, status=status.HTTP_200_OK)

    def create(self, request):
        return Response({"message": "Creando un libro"}, status=status.HTTP_201_CREATED)

    def retrieve(self, request, pk=None):
        return Response({"message": f"Mostrando el libro {pk}"}, status=status.HTTP_200_OK)

    def update(self, request, pk=None):
        return Response({"message": f"Actualizando el libro {pk}"}, status=status.HTTP_200_OK)
    
    def destroy(self, request, pk=None):
        return Response({"message": f"Eliminando el libro {pk}"}, status=status.HTTP_200_OK)
```

#### 1. `SimpleRouter`

El `SimpleRouter` es la opción más básica. Crea las rutas estándar para las acciones `list`, `create`, `retrieve`, `update`, `partial_update`, y `destroy`.

**`urls.py`**

```python
from rest_framework.routers import SimpleRouter
from . import views

# Inicializa el router
router = SimpleRouter(trailing_slash=False)

# Registra el ViewSet
router.register('books', views.BookViewSet, basename='books')

# Las URLs del router se añaden a urlpatterns
urlpatterns = router.urls
```

> [!SUCCESS] ¡Rutas Generadas!
> 
> SimpleRouter crea automáticamente los siguientes patrones de URL:
> 
| URL | Método HTTP | Acción del ViewSet | 
| :--- | :--- | :--- | 
| /books | GET | list |
| /books | POST | create | 
| /books/{pk} | GET | retrieve | 
| /books/{pk} | PUT | update | 
| /books/{pk} | PATCH | partial_update |
| /books/{pk} | DELETE | destroy |

#### 2. `DefaultRouter`

El `DefaultRouter` hace todo lo que hace `SimpleRouter` y añade una característica extra muy útil: **una vista raíz de la API**.

**`urls.py`**

```python
from rest_framework.routers import DefaultRouter
from . import views

# Es casi idéntico al SimpleRouter
router = DefaultRouter(trailing_slash=False)
router.register('books', views.BookViewSet, basename='books')
# Podrías registrar más ViewSets aquí
# router.register('authors', views.AuthorViewSet, basename='authors')

urlpatterns = router.urls
```

> [!NOTE] Vista Raíz de la API
> 
> Al usar DefaultRouter, si navegas a la raíz de tu API (ej. /api/), obtendrás una página autogenerada que lista todos los endpoints registrados. Esto es extremadamente útil para la exploración y depuración de la API.

> [!DANGER] trailing_slash=False
> 
> 🛑 Importante: Por defecto, los routers de DRF añaden una barra (/) al final de las URLs. Si prefieres URLs sin esta barra (una práctica común en APIs modernas), asegúrate de instanciar el router con trailing_slash=False.