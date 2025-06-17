---
aliases:
  - 🚀 Ordenamiento en Django REST Framework 🍽️
tags:
  - django
  - django-rest-framework
  - apis
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
Fecha: 2025-06-11
---
# 🚀 Ordenamiento en Django REST Framework 🍽️

Cuando haces una consulta a tu API, como en el ejemplo `http://127.0.0.1:8000/api/menu-items/?ordering=-price,inventory`, estás diciéndole a DRF cómo quieres que se ordenen los resultados. ¡Veamos los detalles! 🕵️‍♀️
## ⚙️ ¿Cómo funciona?
DRF utiliza el concepto de "filtros de ordenamiento" para permitir que los usuarios de la API especifiquen el orden de los resultados. Esto se logra generalmente a través de un `backend` de filtro, como `DjangoFilterBackend` o `OrderingFilter`. 🔍
### 🎯 Parámetro `ordering`
El parámetro clave aquí es `ordering`. Lo añades a la URL de tu consulta después de un `?` (query parameter) y le asignas uno o varios campos por los que quieres ordenar.
Por ejemplo, en `http://127.0.0.1:8000/api/menu-items/?ordering=-price,inventory`:
- `ordering`: Es el parámetro que indica que queremos ordenar.
- `-price`: Significa que queremos ordenar por el campo `price` (precio) en **orden descendente**. El guion `-` antes del nombre del campo indica orden descendente.
- `inventory`: Significa que después de ordenar por `price`, los elementos con el mismo precio se ordenarán por el campo `inventory` (inventario) en **orden ascendente** (por defecto).
> [!TIP] ¡Consejo pro!
> 
> Si no pones un `-` antes del nombre del campo, el orden será ascendente por defecto. ¡Es como contar de 1 a 10! 🔢
## 🛠️ Configuración en DRF
Para que esto funcione en tu API de DRF, necesitas configurar un `filter_backends` en tu `ViewSet` o `GenericAPIView`.
```python
from rest_framework import generics, filters
from .models import MenuItem
from .serializers import MenuItemSerializer

class MenuItemListView(generics.ListAPIView):
    queryset = MenuItem.objects.all()
    serializer_class = MenuItemSerializer
    filter_backends = [filters.OrderingFilter] # 👈 ¡Aquí está la magia! ✨
    ordering_fields = ['price', 'inventory', 'name'] # Los campos por los que se puede ordenar
    ordering = ['price'] # Orden predeterminado (opcional)
```
En el código anterior:
- `filter_backends = [filters.OrderingFilter]`: Habilita el filtro de ordenamiento para esta vista.
- `ordering_fields = ['price', 'inventory', 'name']`: Define una lista de campos por los cuales los clientes de la API pueden ordenar. ¡Si un campo no está aquí, no se podrá usar para ordenar! 🚫
- `ordering = ['price']`: Establece un orden predeterminado si el cliente no especifica el parámetro `ordering` en la URL.

> [!INFO] ¡Información adicional!
> 
> Puedes usar __all__ en ordering_fields si quieres permitir el ordenamiento por cualquier campo del modelo. ¡Pero ten cuidado, esto puede exponer más de lo que quieres! 😬
## 📚 Ejemplos de uso

Aquí tienes algunos ejemplos de cómo puedes usar el parámetro `ordering`:

| **URL de Consulta**                                              | **Descripción**                                                                                                                                                                                            |
| ---------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `http://127.0.0.1:8000/api/menu-items/?ordering=price`           | Ordena los elementos del menú por `price` en orden ascendente. ⬆️                                                                                                                                          |
| `http://127.0.0.1:8000/api/menu-items/?ordering=-price`          | Ordena los elementos del menú por `price` en orden descendente. ⬇️                                                                                                                                         |
| `http://127.0.0.1:8000/api/menu-items/?ordering=name,-inventory` | Ordena los elementos primero por `name` ascendente, y luego, para los elementos con el mismo `name`, por `inventory` descendente. 🔠📦                                                                     |
| `http://127.0.0.1:8000/api/menu-items/?ordering=inventory`       | Ordena los elementos del menú por `inventory` en orden ascendente. 📈                                                                                                                                      |
| `http://127.0.0.1:8000/api/menu-items/`                          | Si `ordering` está definido en la vista (ej. `ordering = ['price']`), se usará ese orden predeterminado. De lo contrario, el orden será el que defina tu `queryset` inicial o ninguno en particular. 🤷‍♀️ |

> [!WARNING] ¡Cuidado!
> 
> Si intentas ordenar por un campo que no está en ordering_fields, DRF simplemente ignorará ese campo en la consulta de ordenamiento. ¡No te dará un error, pero tampoco ordenará como esperas! 😅