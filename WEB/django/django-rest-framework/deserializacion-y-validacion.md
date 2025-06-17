---
aliases:
  - Deserialización y Validación ✅
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
# Deserialización y Validación en Django REST Framework ✅
La deserialización es el proceso de tomar datos de entrada (como JSON o XML) y convertirlos en objetos complejos de Python, típicamente instancias de modelos de Django. La validación, por su parte, es el proceso de asegurar que estos datos deserializados cumplan con un conjunto de reglas y restricciones antes de ser guardados en la base de datos o utilizados de otra manera.
Django REST Framework (DRF) proporciona un marco robusto para ambos procesos, integrándolos de manera fluida con los serializadores.
## 1. El Proceso de Deserialización
Cuando un serializador recibe datos para crear o actualizar una instancia, sigue estos pasos:
1. **Instanciación del Serializador:** Se crea una instancia del serializador, pasándole los datos de entrada. Si es para una actualización, también se le pasa la instancia existente.
    ```python
    # Para crear una nueva instancia
    serializer = MyModelSerializer(data={'field1': 'value1', 'field2': 'value2'})
    
    # Para actualizar una instancia existente
    instance = MyModel.objects.get(pk=1)
    serializer = MyModelSerializer(instance, data={'field1': 'new_value1'}, partial=True)
    ```
2. **Acceso a los Datos:** Los datos de entrada se almacenan en el atributo `serializer.initial_data`.
3. **Llamada a `is_valid()`:** Este es el punto clave. Cuando se llama a `serializer.is_valid()`, se desencadena el proceso de validación.
4. **Acceso a los Datos Validados:** Si la validación es exitosa, los datos limpios y validados están disponibles en `serializer.validated_data`. Si no es exitosa, los errores se almacenan en `serializer.errors`.
5. **Persistencia (Guardado):** Una vez que los datos son validados, se llama a `serializer.save()`. Este método utiliza `serializer.create()` o `serializer.update()` internamente para guardar la instancia en la base de datos.
    ```python
    if serializer.is_valid():
        serializer.save()
        # La instancia guardada está disponible en serializer.instance
    else:
        print(serializer.errors)
    ```
    

> [!NOTE] partial=True
> 
> Cuando actualizas una instancia, partial=True permite que solo se envíen los campos que deseas actualizar, en lugar de todos los campos requeridos.
## 2. El Proceso de Validación
La validación en DRF se produce en varios niveles, lo que permite una gran flexibilidad y control sobre la integridad de los datos.
### a) Validación a Nivel de Campo (Field-level Validation) 🎯
Los serializadores realizan validación automática basada en los campos del modelo de Django (por ejemplo, `max_length`, `blank=False`, `null=False`, `unique=True`). Además, puedes añadir validadores personalizados a campos específicos.
Para validadores personalizados a nivel de campo, se define un método con el prefijo `validate_` seguido del nombre del campo.
```python
from rest_framework import serializers

class ProductSerializer(serializers.Serializer):
    name = serializers.CharField(max_length=100)
    price = serializers.DecimalField(max_digits=10, decimal_places=2)

    def validate_price(self, value):
        if value < 0:
            raise serializers.ValidationError("El precio no puede ser negativo.")
        return value
```

> [!TIP] ValidationError
> 
> Para levantar un error de validación, se utiliza serializers.ValidationError(). DRF capturará esta excepción y la incluirá en el diccionario serializer.errors.

### b) Validación a Nivel de Objeto (Object-level Validation) 📦
A veces, la validación de un campo depende del valor de otro campo, o hay reglas de negocio complejas que involucran múltiples campos. Para esto, se utiliza un método `validate` general en el serializador.
Este método recibe el diccionario completo de los datos validados hasta ese momento.
```python
from rest_framework import serializers

class EventSerializer(serializers.Serializer):
    start_date = serializers.DateField()
    end_date = serializers.DateField()

    def validate(self, data):
        """
        Comprueba que la fecha de fin no sea anterior a la fecha de inicio.
        """
        if data['start_date'] > data['end_date']:
            raise serializers.ValidationError("La fecha de fin no puede ser anterior a la fecha de inicio.")
        return data
```

> [!WARNING] Orden de Ejecución
> 
> La validación a nivel de campo se ejecuta antes que la validación a nivel de objeto. Esto significa que cuando el método validate a nivel de objeto es llamado, los campos individuales ya han sido validados.

### c) Validadores Reutilizables (Reusable Validators) 🔁

DRF proporciona un módulo `validators` con validadores comunes que puedes aplicar a tus campos.
```python
from rest_framework import serializers
from rest_framework.validators import UniqueValidator

class UserSerializer(serializers.Serializer):
    email = serializers.EmailField(
        validators=[UniqueValidator(queryset=User.objects.all(), message="Este email ya está registrado.")]
    )
    username = serializers.CharField(
        validators=[UniqueValidator(queryset=User.objects.all(), message="Este nombre de usuario ya existe.")]
    )
```

También puedes crear tus propios validadores personalizados como clases, que permiten una mayor reutilización y encapsulación de la lógica de validación.
```python
from rest_framework import serializers

class NoBadWordsValidator:
    def __call__(self, value):
        if "badword" in value.lower():
            raise serializers.ValidationError("No se permiten palabras ofensivas.")

class CommentSerializer(serializers.Serializer):
    text = serializers.CharField(validators=[NoBadWordsValidator()])
```

## 3. Manejo de Errores

Cuando `is_valid()` devuelve `False`, el atributo `serializer.errors` contendrá un diccionario detallado de los errores de validación.
```python
{
    "field_name_1": [
        "Error message 1 for field 1",
        "Error message 2 for field 1"
    ],
    "field_name_2": [
        "Error message for field 2"
    ],
    "non_field_errors": [
        "Global error message 1 (from object-level validation)"
    ]
}
```

Estos errores se formatean automáticamente como respuestas JSON en las vistas de DRF, lo que facilita que los clientes de la API comprendan qué salió mal.

> [!INFO] Errores Globales
> 
> Los errores de validación a nivel de objeto aparecen bajo la clave non_field_errors por defecto.

## 4. Sobreescribiendo `create()` y `update()`

Para operaciones de escritura más complejas, especialmente con serializadores anidados o lógica de negocio que requiere manipulación de datos antes de guardar, puedes sobrescribir los métodos `create()` y `update()` del serializador.
```python
class OrderSerializer(serializers.ModelSerializer):
    items = OrderItemSerializer(many=True) # Serializador anidado

    class Meta:
        model = Order
        fields = ['id', 'customer', 'status', 'items']

    def create(self, validated_data):
        items_data = validated_data.pop('items')
        order = Order.objects.create(**validated_data)
        for item_data in items_data:
            OrderItem.objects.create(order=order, **item_data)
        return order

    def update(self, instance, validated_data):
        items_data = validated_data.pop('items')
        # Lógica para actualizar los items existentes y/o crear nuevos
        # Esto puede ser bastante complejo dependiendo de la lógica de negocio
        instance.customer = validated_data.get('customer', instance.customer)
        instance.status = validated_data.get('status', instance.status)
        instance.save()
        return instance
```

> [!SUCCESS] Flujo Completo
> 
> El flujo general es: Recibir datos ➡️ Instanciar Serializador ➡️ is_valid() (que ejecuta validaciones de campo y objeto) ➡️ validated_data disponible ➡️ save() (que llama a create() o update()).

La deserialización y validación son componentes críticos para asegurar la calidad y seguridad de los datos en cualquier API. DRF los integra de manera excelente, permitiendo a los desarrolladores definir reglas claras y manejar los errores de manera eficiente.