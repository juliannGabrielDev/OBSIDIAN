---
aliases:
  - Serializadores de Relaciones 💖
tags:
  - django
  - django-rest-framework
  - apis
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
  - "[[indice-apis|🔌 Índice APIs]]"
Fecha: 2025-06-10
---
# Serializadores de Relaciones en Django REST Framework 💖

Los serializadores en Django REST Framework (DRF) son fundamentales para convertir los datos complejos de los modelos de Django, como los de una base de datos, en formatos más simples como JSON o XML, que pueden ser fácilmente consumidos por APIs web. Cuando trabajamos con modelos que tienen relaciones (como `ForeignKey`, `ManyToManyField` o `OneToOneField`), es crucial manejar estas relaciones de manera efectiva en nuestros serializadores para asegurar que los datos se presenten de forma coherente y útil.
[[relaciones-entre-modelos|Entendiendo las Relaciones entre Modelos en Django 🔗]]
DRF ofrece varias maneras de manejar los campos de relación, cada una con sus propias ventajas y casos de uso específicos.
## Tipos de Campos de Serializador para Relaciones
### 1. `PrimaryKeyRelatedField` 🔑
Este es el tipo de campo de serializador predeterminado para las relaciones. Representa el objetivo de la relación mediante su clave primaria.
**Características:**
- **Simple:** Solo devuelve el ID del objeto relacionado.
- **Eficiente:** Requiere menos consultas a la base de datos si solo necesitas el ID.
- **Escritura:** Permite escribir el ID del objeto relacionado para establecer la relación.
**Ejemplo:**
```python
from rest_framework import serializers
from .models import Album, Track

class TrackSerializer(serializers.ModelSerializer):
    album = serializers.PrimaryKeyRelatedField(queryset=Album.objects.all())

    class Meta:
        model = Track
        fields = ['id', 'name', 'album']
```

> [!TIP] Uso en Escritura
> 
> Cuando se usa PrimaryKeyRelatedField para escritura (crear o actualizar), el queryset es obligatorio para que DRF pueda validar que el ID proporcionado realmente existe.
### 2. `StringRelatedField` 💬
Este campo representa el objetivo de la relación utilizando su representación de cadena (lo que devuelve el método `__str__` del modelo relacionado).
**Características:**
- **Lectura:** Útil solo para operaciones de lectura. No puede usarse para crear o actualizar la relación directamente.
- **Legible:** Proporciona una representación más legible para los humanos que un simple ID.
**Ejemplo:**
```python
from rest_framework import serializers
from .models import Album, Track

class TrackSerializer(serializers.ModelSerializer):
    album = serializers.StringRelatedField()

    class Meta:
        model = Track
        fields = ['id', 'name', 'album']
```

> [!WARNING] Solo Lectura
> 
> Si intentas pasar un nombre de cadena a StringRelatedField en una operación de escritura, DRF generará un error ya que no sabe cómo mapear esa cadena a un objeto de base de datos.
### 3. `SlugRelatedField` ✒️
Similar a `StringRelatedField`, pero utiliza un campo específico del objeto relacionado (como un `slug` o un `nombre_unico`) para su representación.
**Características:**
- **Lectura/Escritura:** Puede usarse tanto para lectura como para escritura, siempre que el campo `slug_field` sea único en el modelo relacionado.
- **Controlable:** Te permite elegir qué campo del modelo relacionado usar para la representación.
**Ejemplo:**
```python
from rest_framework import serializers
from .models import Album, Track

class TrackSerializer(serializers.ModelSerializer):
    album = serializers.SlugRelatedField(
        queryset=Album.objects.all(),
        slug_field='title' # Asume que el modelo Album tiene un campo 'title'
    )

    class Meta:
        model = Track
        fields = ['id', 'name', 'album']
```

> [!INFO] Unicidad del slug_field
> 
> Para que SlugRelatedField funcione correctamente en operaciones de escritura, el campo especificado en slug_field debe ser único en el modelo relacionado.
### 4. Serializadores Anidados (Nested Serializers) 🌲
Esta es una de las formas más potentes y comunes de manejar relaciones. Implica incluir la representación completa de un serializador de un modelo relacionado dentro de otro serializador.
**Características:**
- **Detallado:** Proporciona todos los campos del objeto relacionado.
- **Conveniente:** Permite ver o editar datos relacionados en una sola solicitud.
- **Escritura (Cuidado):** La escritura en serializadores anidados puede volverse compleja, especialmente con relaciones de uno a muchos o muchos a muchos. DRF maneja las relaciones de uno a uno de forma más directa. Para relaciones uno a muchos y muchos a muchos, a menudo se requiere sobrescribir los métodos `create()` y `update()` del serializador principal.
**Ejemplo:**
```python
from rest_framework import serializers
from .models import Album, Track

class AlbumSerializer(serializers.ModelSerializer):
    class Meta:
        model = Album
        fields = ['id', 'title', 'artist']

class TrackSerializer(serializers.ModelSerializer):
    album = AlbumSerializer() # Serializador anidado

    class Meta:
        model = Track
        fields = ['id', 'name', 'album']
```

> [!WARNING] Rendimiento con Serializadores Anidados
> 
> Los serializadores anidados pueden llevar a un rendimiento pobre si no se utilizan técnicas de optimización como `select_related()` o `prefetch_related()` en las vistas para evitar el problema de "N+1 consultas".
### 5. `HyperlinkedRelatedField` 🔗
Este campo representa la relación como un hipervínculo a la URL detallada del objeto relacionado.
**Características:**
- **RESTful:** Promueve el principio HATEOAS (Hypermedia as the Engine of Application State).
- **Lectura/Escritura:** Puede usarse para lectura y escritura.
- **Requiere URL:** Necesita una vista con un nombre de URL que pueda resolver la URL del objeto relacionado.
**Ejemplo:**
```python
from rest_framework import serializers
from .models import Album, Track

class TrackSerializer(serializers.ModelSerializer):
    album = serializers.HyperlinkedRelatedField(
        view_name='album-detail', # Nombre de la URL para la vista de detalle del Album
        read_only=True # O false si permites escritura
    )

    class Meta:
        model = Track
        fields = ['id', 'name', 'album', 'url'] # 'url' es importante si estás usando HyperlinkedModelSerializer
```

> [!DANGER] Configuración de URL
> 
> Si usas HyperlinkedRelatedField, asegúrate de que tus vistas tengan nombres de URL apropiados configurados en tus urls.py.
## Consideraciones Adicionales
### Relaciones Inversas (Reverse Relationships) 🔄
Cuando tienes una `ForeignKey`, el modelo "hijo" tiene un campo que apunta al "padre". Pero desde el lado del "padre", puedes acceder a una relación inversa para obtener todos sus "hijos". Para exponer esta relación inversa en un serializador, debes declararla explícitamente.
**Ejemplo**:
Si Track tiene una ForeignKey a Album, desde Album puedes acceder a `track_set`.
```python
from rest_framework import serializers
from .models import Album, Track

class TrackSerializer(serializers.ModelSerializer):
    class Meta:
        model = Track
        fields = ['id', 'name']

class AlbumSerializer(serializers.ModelSerializer):
    # Relación inversa: tracks es el related_name o por defecto '<nombre_modelo>_set'
    tracks = TrackSerializer(many=True, read_only=True)

    class Meta:
        model = Album
        fields = ['id', 'title', 'artist', 'tracks']
```

> [!NOTE] many=True
> 
> Usa many=True cuando la relación inversa sea de uno a muchos (ej. un álbum puede tener muchas pistas).
### `source` y `read_only` 💡
- **`source='nombre_campo'`**: Utiliza esto cuando el nombre del campo en tu serializador difiere del nombre del campo en tu modelo o cuando necesitas acceder a un atributo o método en el objeto.
- **`read_only=True`**: Marca un campo como de solo lectura, lo que significa que se incluirá en la representación serializada cuando se lea, pero se ignorará para las operaciones de actualización y creación.
### Optimización de Consultas (N+1 Problem) 🚀
Cuando se utilizan serializadores anidados o `StringRelatedField`, es muy fácil caer en el problema de N+1 consultas. Esto ocurre cuando se recupera una lista de objetos y luego, para cada objeto, se realiza una consulta adicional para obtener sus datos relacionados.
Para evitar esto, utiliza `select_related()` para relaciones `ForeignKey` y `OneToOneField`, y `prefetch_related()` para `ManyToManyField` y relaciones inversas de `ForeignKey` en tus `QuerySet`s de la vista:
```python
# views.py
from rest_framework import generics
from .models import Album
from .serializers import AlbumSerializer

class AlbumList(generics.ListAPIView):
    queryset = Album.objects.all().prefetch_related('tracks') # Optimización
    serializer_class = AlbumSerializer
```

---

La elección del tipo de serializador para relaciones depende de tus necesidades específicas: la cantidad de datos que necesitas exponer, si la relación debe ser editable, y la facilidad de uso para los clientes de tu API. Comprender y aplicar estos conceptos te permitirá construir APIs robustas y eficientes con Django REST Framework.