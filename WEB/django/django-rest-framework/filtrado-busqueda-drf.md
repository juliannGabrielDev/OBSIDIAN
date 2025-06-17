---
aliases:
  - "Optimización de API: Filtrado y Búsqueda 🔍"
tags:
  - django
  - django-rest-framework
  - apis
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
  - "[[indice-apis|🔌 Índice APIs]]"
Fecha: 2025-06-11
---
# Optimización de API: Filtrado y Búsqueda en Django REST Framework 🔍

Casi todas las aplicaciones y sitios web modernos proporcionan a los usuarios una **función de búsqueda**, una de las formas más básicas en que los usuarios interactúan con Internet. 
Cuando una aplicación del cliente hace una llamada HTTP al punto de conexión de los elementos del menú, <mark style="background: #FF5582A6;">mostrará todos los elementos y sus categorías</mark>.
Pero, <mark style="background: #D2B3FFA6;">¿qué sucede si un visitante solo quiere ver los elementos de la categoría de aperitivos o solo los platos principales, en lugar de todo a la vez?</mark> Tal vez un visitante prefiera ver solo los elementos que están dentro de un rango de precio específico, como de $7 a $15. De manera similar, un visitante puede querer buscar elementos del menú que contengan la palabra "pizza" o bebidas con la palabra "mojito". Aquí es donde entra en juego la función de filtrado.
## 🎯 Filtrado
El filtrado es un proceso que permite a las aplicaciones del cliente obtener un subconjunto de los resultados de la API en función de algunos criterios.
### Opciones de Procesamiento de Filtrado
Tienes dos opciones cuando las aplicaciones del cliente quieren un subconjunto del resultado, por ejemplo, todos los elementos del menú de la categoría de aperitivos o de postres:
1. **Procesar el filtrado en el cliente:** No haces nada y para mostrar todos los elementos con los nombres de su categoría, la aplicación del cliente luego procesa estos datos y administra todo el filtrado de su lado.
    - **Ventajas:** Necesitas menos tiempo para desarrollar la API.
    - **Desventajas:**
        - Debes obtener todos los registros de la base de datos cada vez. Esto podría no ser un problema para cientos de registros, pero cuando hay miles, diez mil o incluso millones de registros, esto no es sostenible.
        - Creará una carga significativa en el servidor y obtener 10,000 registros de la base de datos no tiene ningún sentido cuando solo necesitas 10 o 20.
2. **Procesar el filtrado en el servidor (recomendado):** Como desarrollador de API, procesas esas condiciones en el servidor y entregas los resultados que coincidan con esos criterios.    
    - **Ventajas:**
        - Coloca menos carga en el servidor.
        - Reduce el tiempo de desarrollo de las aplicaciones del cliente porque todo el filtrado se realiza del lado del servidor.
    - **Desventajas:** Tomará un tiempo más para desarrollar.
### Implementación del Filtrado en `views.py`
Las aplicaciones del cliente pueden pasar una cadena de consulta al punto de conexión de los elementos del menú con la cadena de consulta `?category=` y luego el elemento que están buscando, como `main` o `ice cream`. También pueden filtrar los elementos del menú por precio, agregando `?to_price=3` al punto de conexión de la API. O pueden usar ambos criterios con un `&` en el medio.
Veamos el modelo `menu item`. El modelo `category` está vinculado dentro de `menu item` y tiene el campo `title`. Utilizará este campo para filtrar registros.
```python
# views.py
from django.shortcuts import get_object_or_404
from rest_framework.response import Response
from rest_framework.decorators import api_view
from .models import MenuItem
from .serializers import MenuItemSerializer

@api_view(['GET'])
def menu_items(request):
    items = MenuItem.objects.select_related('category').all()

    category_name = request.query_params.get('category')
    to_price = request.query_params.get('to_price')

    if category_name:
        items = items.filter(category__title=category_name)
    if to_price:
        items = items.filter(price__lte=to_price)

    serializer = MenuItemSerializer(items, many=True)
    return Response(serializer.data)
```
**Explicación del código:**
- `category_name = request.query_params.get('category')`: Recupera el valor del parámetro de consulta `category` de la URL.
- `to_price = request.query_params.get('to_price')`: Recupera el valor del parámetro de consulta `to_price` de la URL.
- **Filtrado por categoría (`category__title`):** Si el `category_name` está presente, filtra los elementos donde el `title` de la `category` (modelo vinculado) coincide con `category_name`. La notación `category__title` es crucial para acceder a campos de modelos relacionados.
- **Filtrado por precio (`price__lte`):** Si `to_price` está presente, filtra los elementos donde el `price` es menor o igual (`lte`) al valor de `to_price`. `lte` es un operador condicional o una búsqueda de campo que significa "less than or equal". Para filtrar por un precio exacto, puede usar `price=to_price`.
**Es hora de probarlo:** Visite el punto de conexión de `menu items` con los valores de filtro que configuró y notará la diferencia. Ahora el resultado se ha filtrado correctamente según los criterios que pasó.
## 🔎 Búsqueda
Luego, implementemos la búsqueda. Si la aplicación del cliente proporciona un parámetro de búsqueda seguido de algunos caracteres, realizará una búsqueda y devolverá esos elementos del menú cuyos nombres comienzan con esos caracteres.
### Implementación de la Búsqueda en `views.py`
Hace esto agregando `?search=chocolate` al final de la URL `localhost`. Esto debería devolver todos los elementos del menú empezando por "chocolate", como "chocolate cake" y "chocolate ice cream".
Al igual que antes, obtenga la búsqueda de parámetros de la cadena de consulta:
```python
# views.py
from django.shortcuts import get_object_or_404
from rest_framework.response import Response
from rest_framework.decorators import api_view
from .models import MenuItem
from .serializers import MenuItemSerializer

@api_view(['GET'])
def menu_items(request):
    items = MenuItem.objects.select_related('category').all()

    category_name = request.query_params.get('category')
    to_price = request.query_params.get('to_price')
    search_term = request.query_params.get('search') # Nuevo parámetro de búsqueda

    if category_name:
        items = items.filter(category__title=category_name)
    if to_price:
        items = items.filter(price__lte=to_price)
    if search_term: # Lógica para la búsqueda
        items = items.filter(title__startswith=search_term) # Buscar por título que comienza con...

    serializer = MenuItemSerializer(items, many=True)
    return Response(serializer.data)
```
**Tipos de búsquedas de campo para el campo `title`:**
- **`startswith`**: Buscar si el título comienza con los caracteres proporcionados.
- **`contains`**: Buscar si los caracteres están presentes en cualquier parte del título.
- **`icontains`**: Versión que no distingue entre mayúsculas y minúsculas de `contains`.
- **`istartswith`**: Versión que no distingue entre mayúsculas y minúsculas de `startswith`.
**Ejemplos de búsqueda:**
- `items.filter(title__contains=search_term)`: Búsqueda sensible a mayúsculas y minúsculas en cualquier parte del título.
- `items.filter(title__icontains=search_term)`: Búsqueda insensible a mayúsculas y minúsculas en cualquier parte del título.
Ahora ya sabe cómo optimizar los resultados de su API y realizar búsquedas y filtrado en sus proyectos de DRF. También aprendió sobre las búsquedas de campos, que funcionan como operadores de comparación durante este proceso. ¡Bien hecho!
## Operadores de Búsqueda de Campo (Field Lookups)
Aquí hay una tabla que resume algunos de los operadores de búsqueda de campo más comunes en Django ORM:

| **Operador**           | **Descripción**                                            | **Ejemplo**                             |
| ---------------------- | ---------------------------------------------------------- | --------------------------------------- |
| `exact`                | Coincidencia exacta                                        | `field__exact='value'`                  |
| `iexact`               | Coincidencia exacta (insensible a mayúsculas/minúsculas)   | `field__iexact='value'`                 |
| `contains`             | Contiene la subcadena (sensible a mayúsculas/minúsculas)   | `field__contains='substring'`           |
| `icontains`            | Contiene la subcadena (insensible a mayúsculas/minúsculas) | `field__icontains='substring'`          |
| `startswith`           | Empieza con (sensible a mayúsculas/minúsculas)             | `field__startswith='prefix'`            |
| `istartswith`          | Empieza con (insensible a mayúsculas/minúsculas)           | `field__istartswith='prefix'`           |
| `endswith`             | Termina con (sensible a mayúsculas/minúsculas)             | `field__endswith='suffix'`              |
| `iendswith`            | Termina con (insensible a mayúsculas/minúsculas)           | `field__iendswith='suffix'`             |
| `gt`                   | Mayor que                                                  | `field__gt=10`                          |
| `gte`                  | Mayor o igual que                                          | `field__gte=10`                         |
| `lt`                   | Menor que                                                  | `field__lt=10`                          |
| `lte`                  | Menor o igual que                                          | `field__lte=10`                         |
| `in`                   | En una lista de valores                                    | `field__in=[1, 2, 3]`                   |
| `range`                | Dentro de un rango (inclusivo)                             | `field__range=(start, end)`             |
| `isnull`               | Es `NULL` o no                                             | `field__isnull=True`                    |
| `regex`                | Coincidencia de expresión regular (sensible)               | `field__regex=r'expresion'`             |
| `iregex`               | Coincidencia de expresión regular (insensible)             | `field__iregex=r'expresion'`            |
| `date`                 | Coincidencia por fecha (solo fecha)                        | `datetime_field__date=date(2023, 1, 1)` |
| `year`, `month`, `day` | Coincidencia por partes de una fecha/hora                  | `datetime_field__year=2023`             |
