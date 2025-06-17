---
aliases:
  - 🧬 Serializers
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
# 🧬 Serializers en Django REST Framework
Los `Serializers` son uno de los componentes más cruciales y potentes de Django REST Framework (DRF). Su función principal es actuar como un <mark style="background: #FFB86CA6;">traductor bidireccional</mark> entre los <mark style="background: #FFB86CA6;">tipos de datos complejos</mark> de Python (como los `QuerySets` y las instancias de modelos de Django) y los <mark style="background: #FFB86CA6;">formatos de datos nativos</mark> de Python, <mark style="background: #FFB86CA6;">que luego pueden ser fácilmente renderizados a formatos como JSON o XML</mark>.
En resumen, los serializers se encargan de dos procesos clave:
1. **Serialización**: Convertir un objeto de Python (ej. una instancia del modelo `Book`) en un diccionario de datos primitivos.
2. **Deserialización**: Validar datos entrantes (ej. un payload JSON de una petición `POST`) y convertirlos de nuevo en un objeto complejo de Python.
---
### 🧱 Tipos de Serializers
Existen dos tipos principales de serializers que puedes usar, cada uno con sus ventajas.
#### 1. `serializers.Serializer`
Esta es la clase base que te da un control total y explícito sobre la definición de los campos. Es muy similar a la clase `Form` de Django. Debes definir cada campo manualmente.
```python
from rest_framework import serializers

class CommentSerializer(serializers.Serializer):
    email = serializers.EmailField()
    content = serializers.CharField(max_length=200)
    created = serializers.DateTimeField()
```
> [!TIP] Úsalo cuando...
> 
> Consejo pro: Utiliza serializers.Serializer cuando los datos que estás serializando no se corresponden directamente con un modelo de tu base de datos, por ejemplo, para procesar la entrada de un formulario de contacto o un objeto complejo personalizado.

#### 2. `serializers.ModelSerializer`
![[model-serializer#✨ La Magia del `ModelSerializer`|🧬 Serializadores de Modelos]]

> [!SUCCESS] ¡Magia automática!
> 
> ✅ `ModelSerializer` introspecciona tu modelo `Book` y crea los campos `title`, `author` y `publication_year` con las validaciones y tipos correctos sin que tengas que definirlos explícitamente.

---
### 🔄 El Proceso de Serialización y Deserialización
#### Serialización (Objeto Python ➡️ JSON)
Para serializar un objeto o un QuerySet, simplemente pasas el objeto a la instancia del serializer.
```python
# Obtener un solo objeto
book = Book.objects.get(pk=1)
serializer = BookSerializer(book)
serializer.data 
# Resultado -> {'id': 1, 'title': 'Cien Años de Soledad', 'author': 'Gabriel García Márquez', 'publication_year': 1967}

# Obtener múltiples objetos (un QuerySet)
books = Book.objects.all()
serializer = BookSerializer(books, many=True)
serializer.data
# Resultado -> [ {'id': 1, ...}, {'id': 2, ...} ]
```

> [!INFO] El atributo .data
> 
> El atributo `.data` del serializer contiene la representación en forma de diccionario de tus datos, lista para ser convertida a JSON en la respuesta de la API.

#### Deserialización y Validación (JSON ➡️ Objeto Python)
Este proceso es crucial para manejar datos entrantes de peticiones `POST`, `PUT` o `PATCH`.
1. **Pasar los datos**: Pasa el stream de datos de la petición al serializer.
2. **Validar**: Llama al método `.is_valid()`. Si los datos son correctos, devuelve `True`. Si no, `False`.
3. **Acceder a los datos**: Si la validación es exitosa, los datos limpios y validados están disponibles en el diccionario `.validated_data`.
4. **Guardar el objeto**: Llama a `.save()` para crear o actualizar la instancia del modelo.
```python
from rest_framework.response import Response
from rest_framework.decorators import api_view

@api_view(['POST'])
def create_book(request):
    serializer = BookSerializer(data=request.data)
    if serializer.is_valid():
        serializer.save() # Llama al método .create() del serializer
        return Response(serializer.data, status=201)
    
    # Si no es válido, .errors contiene los detalles de los errores
    return Response(serializer.errors, status=400)
```

> [!WARNING] ¡Valida siempre primero!
> 
> ¡Cuidado! Nunca intentes acceder a `.validated_data` antes de haber llamado a `.is_valid()` y confirmado que es `True`. Si lo haces y los datos son inválidos, se lanzará una excepción.

---

### 🔗 Manejo de Relaciones
Los serializers también manejan de forma elegante las relaciones entre modelos.

| **Clase de Campo**        | **Descripción**                                                                                                      |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| `PrimaryKeyRelatedField`  | Representa la relación usando la clave primaria (ID) del objeto relacionado.                                         |
| `StringRelatedField`      | Representa la relación usando la representación de cadena del objeto (el resultado del método `__str__` del modelo). |
| `HyperlinkedRelatedField` | Representa la relación usando un hipervínculo a la URL del objeto relacionado.                                       |
| **Serializers anidados**  | Incluye la representación completa del objeto relacionado dentro del objeto principal.                               |

---

### ✅ Validación Personalizada
Puedes añadir tu propia lógica de validación sobreescribiendo métodos en la clase del serializer.
- **Validación a nivel de campo**: Usa un método `validate_<nombre_del_campo>`.
    ```python
    def validate_publication_year(self, value):
        if value > 2025:
            raise serializers.ValidationError("El año no puede ser en el futuro.")
        return value
    ```
    
- **Validación a nivel de objeto**: Usa el método `validate`. Es útil cuando necesitas comparar varios campos entre sí.
    ```python
    def validate(self, data):
        # Ejemplo hipotético
        if data['author'] == 'John Doe' and data['title'] == 'Untitled':
            raise serializers.ValidationError("Un autor llamado John Doe no puede tener un libro sin título.")
        return data
    ```