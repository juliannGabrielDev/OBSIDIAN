---
aliases:
  - 🚀 Vistas Genéricas y ViewSets en DRF
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
# 🚀 Vistas Genéricas y ViewSets en DRF

Django REST Framework (DRF) proporciona un poderoso conjunto de Vistas Genéricas (`Generic Views`) y `ViewSets` listos para usar. Estas clases están diseñadas para acelerar drásticamente el desarrollo de APIs al abstraer la lógica repetitiva y común, permitiéndote escribir menos código y enfocarte en la lógica de negocio única de tu proyecto.

---

### 🗂️ ViewSets: Agrupando Lógica Relacionada

Un `ViewSet` es una clase que agrupa la lógica para un conjunto de vistas relacionadas. En lugar de definir una clase de vista separada para listar, crear, obtener, actualizar y eliminar un recurso, un `ViewSet` lo maneja todo en un solo lugar. La clase base `ViewSet` hereda de `APIView`.

Su principal ventaja es que, al usarlo con los `Routers` de DRF, las URLs se generan automáticamente, simplificando enormemente la configuración de `urls.py`.

Los `ViewSets` utilizan una convención de nombres para sus métodos que se mapean a las operaciones CRUD estándar:

| **Método del ViewSet** | **Método HTTP** | **Propósito**                        |
| ---------------------- | --------------- | ------------------------------------ |
| `list()`               | `GET`           | Mostrar una colección de recursos.   |
| `create()`             | `POST`          | Crear un nuevo recurso.              |
| `retrieve()`           | `GET`           | Mostrar un único recurso específico. |
| `update()`             | `PUT`           | Reemplazar completamente un recurso. |
| `partial_update()`     | `PATCH`         | Actualizar parcialmente un recurso.  |
| `destroy()`            | `DELETE`        | Eliminar un recurso.                 |

#### Tipos de ViewSets

- **`viewsets.ViewSet`**: La clase base. Debes implementar la lógica de cada método (listar, crear, etc.) manualmente. Te da control total pero requiere más código.
- **`viewsets.ModelViewSet`**: ¡El más potente para CRUD! Hereda de `GenericViewSet` y mezcla el comportamiento de todas las vistas genéricas de CRUD. Con solo definir un `queryset` y un `serializer_class`, automáticamente te proporciona la implementación completa de `list`, `create`, `retrieve`, `update` y `destroy`.
- **`viewsets.ReadOnlyModelViewSet`**: Una versión simplificada del `ModelViewSet` que solo provee acciones de lectura: `list` y `retrieve`. Ideal para recursos que no deben ser modificados a través de la API.

> [!SUCCESS] ModelViewSet en Acción
> 
> ✅ Con ModelViewSet, puedes construir una API CRUD completa con solo unas pocas líneas de código. Es la opción preferida para la mayoría de los casos de uso estándar.
> 
> 
> ```python
> from rest_framework import viewsets
> from .models import Book
> from .serializers import BookSerializer
> 
> class BookViewSet(viewsets.ModelViewSet):
>     queryset = Book.objects.all()
>     serializer_class = BookSerializer
> ```

---

### 🧱 Vistas Genéricas: Bloques de Construcción

Las Vistas Genéricas (`Generic Views`) son clases que proporcionan comportamientos específicos y reutilizables, como listar objetos o crear uno nuevo. Son excelentes cuando necesitas construir tus vistas pieza por pieza o cuando la estructura de tus URLs no encaja bien con los `Routers` de los `ViewSets`.

> [!INFO] Queryset y Serializador
> 
> Al igual que los `ModelViewSet`, las vistas genéricas requieren que definas los atributos `queryset` y `serializer_class` para funcionar.

Aquí tienes una tabla con las vistas genéricas más comunes:

| **Clase de Vista Genérica**    | **Métodos HTTP Soportados**  | **Propósito**                                     |
| ------------------------------ | ---------------------------- | ------------------------------------------------- |
| `CreateAPIView`                | `POST`                       | Crear un nuevo recurso.                           |
| `ListAPIView`                  | `GET`                        | Mostrar una colección de recursos.                |
| `RetrieveAPIView`              | `GET`                        | Mostrar un único recurso.                         |
| `DestroyAPIView`               | `DELETE`                     | Eliminar un único recurso.                        |
| `UpdateAPIView`                | `PUT`, `PATCH`               | Actualizar un recurso.                            |
| `ListCreateAPIView`            | `GET`, `POST`                | Listar una colección y crear recursos.            |
| `RetrieveUpdateAPIView`        | `GET`, `PUT`, `PATCH`        | Obtener y actualizar un recurso.                  |
| `RetrieveDestroyAPIView`       | `GET`, `DELETE`              | Obtener y eliminar un recurso.                    |
| `RetrieveUpdateDestroyAPIView` | `GET`,`PUT`,`PATCH`,`DELETE` | ¡La navaja suiza! Obtener, actualizar y eliminar. |

---

### 🛠️ Personalización Avanzada

Aunque estas clases automatizan mucho, DRF te da control total para personalizar su comportamiento.

#### Autenticación y Permisos

Puedes controlar el acceso a tus vistas de forma global o selectiva.

- **Permisos para toda la clase:**
    
    ```python
    from rest_framework.permissions import IsAuthenticated
    
    class MyView(generics.ListCreateAPIView):
        permission_classes = [IsAuthenticated]
        # ... resto de la configuración
    ```
    
- **Permisos selectivos por método HTTP:**
    
    > [!TIP]
    > 
    > Consejo pro: Usa `get_permissions()` para aplicar permisos diferentes a `GET` (lectura) que a `POST` o `PUT` (escritura).
        
    ```python
    def get_permissions(self):
        if self.request.method == 'GET':
            return [] # Permite el acceso sin autenticación para GET
        return [IsAuthenticated()] # Requiere autenticación para otros métodos
    ```
    

#### Filtrar Querysets


Para devolver solo los objetos que pertenecen al usuario que realiza la petición, puedes sobreescribir `get_queryset()`.

```python
def get_queryset(self):
    """
    Esta vista debe retornar una lista de todos los pedidos
    para el usuario actualmente autenticado.
    """
    user = self.request.user
    return Order.objects.filter(owner=user)
```

#### Anular Métodos

Si necesitas una lógica completamente diferente, siempre puedes sobreescribir los métodos principales (`list`, `create`, etc.).

```python
# En una clase que hereda de ListAPIView
def list(self, request, *args, **kwargs):
    # Lógica personalizada aquí
    print("¡Se ha llamado al método list personalizado!")
    return Response({"message": "Esta es una respuesta estática y personalizada"})
```