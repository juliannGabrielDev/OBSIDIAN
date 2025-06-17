---
aliases:
  - "📚 Filtrado y Paginación Avanzados en DRF: ¡Potencia tus APIs! 🚀"
tags:
  - django
  - django-rest-framework
  - apis
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
  - "[[indice-apis|🔌 Índice APIs]]"
Fecha: 2025-06-12
---
# 📚 Filtrado y Paginación Avanzados en DRF: ¡Potencia tus APIs! 🚀
Ya sabes cómo usar el filtrado y la paginación en **vistas basadas en funciones** en DRF 🧑‍💻. Pero, DRF también ofrece **clases de filtrado y paginación integradas** que te permiten implementar estas funcionalidades rápidamente en **vistas basadas en clases** 🤩. En esta nota, aprenderás a usar estas clases para **filtrar, buscar y paginar** tus resultados de forma eficiente.

---
## 🏗️ Andamiaje del Proyecto con Vistas Basadas en Clases ✨
Para empezar, usaremos una vista basada en clases que extienda `ModelViewSet` para implementar rápidamente un endpoint CRUD (Crear, Leer, Actualizar, Borrar) para los elementos del menú 🛠️.
- Paso 1: Crea la Vista Basada en Clases 📄    
    Añade estas líneas de código en el archivo views.py para definir tu MenuItemViewSet:
    ```python
    # myapp/views.py
    from rest_framework import viewsets
    from .models import MenuItem
    from .serializers import MenuItemSerializer
    
    class MenuItemViewSet(viewsets.ModelViewSet):
        queryset = MenuItem.objects.all()
        serializer_class = MenuItemSerializer
    ```
- Paso 2: Mapea la Vista en urls.py 🔗
    Abre tu archivo urls.py y mapea la clase MenuItemViewSet al endpoint menu-items. Para este ejemplo, solo mapearemos los métodos GET 🌐.
    ```python
    # myapp/urls.py
    from django.urls import path
	from . import views

	urlpatterns = [
	    path('menu-items',views.MenuItemsViewSet.as_view({'get':'list'})),
	    path('menu-items/<int:pk>', views.MenuItemsViewSet.as_view({'get':'retrieve'})),
	]
    ```
- Paso 3: Configura los Filtros por Defecto en settings.py ⚙️
    Finalmente, abre tu archivo settings.py y añade las clases `OrderingFilter` y `SearchFilter` como `DEFAULT_FILTER_BACKENDS` en la sección `REST_FRAMEWORK`.
    ```python
    # settings.py
    REST_FRAMEWORK = {
        'DEFAULT_FILTER_BACKENDS': [
            'rest_framework.filters.OrderingFilter', # Para ordenar
            'rest_framework.filters.SearchFilter'    # Para buscar
        ],
        # Otros ajustes de DRF...
    }
    ```
    Una vez completados estos pasos, podrás listar todos los elementos del menú visitando `http://127.0.0.1:8000/api/menu-items/` y cualquier elemento individual del menú visitando `http://127.0.0.1:8000/api/menu-items/1/` ✅.
---
## ↕️ Ordenación y Clasificación con `OrderingFilter` 📊
Para implementar la ordenación por campos como `price` e `inventory`, usa la clase `OrderingFilter` incorporada de DRF. Simplemente especifica estos campos en la lista `ordering_fields` de tu `MenuItemViewSet` 🏷️.
```python
# myapp/views.py
from rest_framework import viewsets
from rest_framework.filters import OrderingFilter # Importa el filtro
from .models import MenuItem
from .serializers import MenuItemSerializer

class MenuItemViewSet(viewsets.ModelViewSet):
    queryset = MenuItem.objects.all()
    serializer_class = MenuItemSerializer
    filter_backends = [OrderingFilter] # Asegúrate de que este filtro esté activo
    ordering_fields = ['price', 'inventory'] # 👈 Campos por los que se puede ordenar
```

Si visitas `http://127.0.0.1:8000/api/menu-items/`, notarás un **nuevo botón de filtro** en la parte superior derecha de la interfaz navegable de la API 🎉. Al hacer clic, aparecerá una ventana emergente con las opciones de ordenación ✨. Puedes hacer clic en cualquiera de estas opciones para ordenar la salida de la API por `price` e `inventory` en orden ascendente o descendente.
También puedes ordenar la salida por ambos campos usando la cadena de consulta `ordering=price,inventory`, así: `http://127.0.0.1:8000/api/menu-items?ordering=price,inventory` 🚀.

---
## 📄 Paginación con Clases Integradas de DRF ➡️
El uso de las clases de paginación incorporadas en DRF hace que paginar el resultado de la API sea **extremadamente fácil** 🤩.
Añade estas dos líneas en la sección `REST_FRAMEWORK` del archivo `settings.py`:
```python
# settings.py
REST_FRAMEWORK = {
    # ... otros ajustes ...
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination', # 👈 Clase de paginación por defecto
    'PAGE_SIZE': 2 # 👈 Cuántos elementos mostrar por página
}
```

La propiedad `PAGE_SIZE` indica a DRF cuántos elementos debe mostrar por página 📏. Ahora, si visitaras el endpoint de los elementos del menú, observarías cómo el resultado se ha paginado en la interfaz navegable de la API y cómo ha cambiado el formato de los datos de salida. ¡Debajo del botón de filtro también verás los números de página! Solo se muestran dos registros por página porque así se ha configurado en el archivo `settings.py` ✅.

---
## 🔍 Búsqueda con `SearchFilter` 🔎
Puedes añadir la función de búsqueda para que los clientes de la API puedan buscar por el campo del título 🕵️‍♀️. Para ello, añade `search_fields=['title']` en la clase `MenuItemViewSet` 🎯.
```python
# myapp/views.py
from rest_framework import viewsets
from rest_framework.filters import OrderingFilter, SearchFilter # Importa los filtros
from .models import MenuItem
from .serializers import MenuItemSerializer

class MenuItemViewSet(viewsets.ModelViewSet):
    queryset = MenuItem.objects.all()
    serializer_class = MenuItemSerializer
    filter_backends = [OrderingFilter, SearchFilter] # Asegúrate de que ambos filtros estén activos
    ordering_fields = ['price', 'inventory']
    search_fields = ['title'] # 👈 Campo por el que se puede buscar
```

Cuando abras el endpoint `menu-items` y pulses el botón de filtro, aparecerá un **nuevo campo de búsqueda** 💡. Puedes usar tanto la ordenación como la búsqueda juntas ✨.
Puedes escribir cualquier cosa y DRF buscará dentro del campo de título y mostrará la salida en consecuencia 📝. El valor por defecto de `lookup_field` para la búsqueda en DRF es `icontains` (insensible a mayúsculas y minúsculas). Si el cliente busca "ILLA", coincidirá con todos los elementos de menú en cuyo título aparezca "ILLA" (por ejemplo, "VANILLA" o "vanilla") como resultados de la búsqueda 🍦.
### 🧩 Búsqueda en Campos Anidados (Relacionados) 🌳
¿Qué ocurre si el cliente de la API también quiere buscar en el título de una categoría, como "Icecream" o "main"? 🤔 En tu `serializers.py`, la categoría se estableció como un campo relacionado con el modelo `MenuItem` en la clase `MenuItemSerializer`. Los clientes buscarán en el campo del título del modelo de categoría.
La convención de nomenclatura para buscar en un modelo relacionado es: `RelatedModelName_FieldName` (o `related_name__field_name` si usas `related_name` en tu modelo). En este caso, el nombre del modelo relacionado es `category` y el nombre del campo es `title`. Por lo tanto, para buscar en el campo del título del modelo de categoría, es necesario pasar `category__title` en la lista `search_fields` 🎯.
```python
# myapp/views.py (actualizando MenuItemViewSet)
from rest_framework import viewsets
from rest_framework.filters import OrderingFilter, SearchFilter
from .models import MenuItem
from .serializers import MenuItemSerializer

class MenuItemViewSet(viewsets.ModelViewSet):
    queryset = MenuItem.objects.all()
    serializer_class = MenuItemSerializer
    filter_backends = [OrderingFilter, SearchFilter]
    ordering_fields = ['price', 'inventory']
    search_fields = ['title', 'category__title'] # 👈 Ahora busca también en el título de la categoría
```
Añadiendo estas líneas de código, los clientes de la API podrán buscar texto tanto en los títulos de los elementos de menú como en los títulos de las categorías 🎉. ¡Observa que la paginación y la ordenación seguirán funcionando junto con la función de búsqueda! ✅