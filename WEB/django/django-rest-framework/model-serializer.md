---
aliases:
  - 🧬 Serializadores de Modelos
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
# 🧬 Serializadores de Modelos en Django REST Framework

Los `ModelSerializer` son una subclase especializada de los `Serializers` de Django REST Framework (DRF) que simplifican drásticamente el proceso de crear APIs para interactuar con modelos de Django. <mark style="background: #D2B3FFA6;">Actúan como un puente inteligente y automatizado entre tus modelos de base de datos y representaciones como JSON</mark>.
Su principal objetivo es eliminar el código repetitivo al manejar operaciones CRUD (Crear, Leer, Actualizar, Eliminar).

---
### ✨ La Magia del `ModelSerializer`
A diferencia del `Serializer` base, donde debes definir cada campo manualmente, el `ModelSerializer` ofrece una potente capa de abstracción:
- **Generación automática de campos**: Introspecciona tu modelo y crea un campo de serializer para cada campo del modelo.
- **Validadores automáticos**: Aplica automáticamente los validadores del modelo (ej. `max_length`, `unique`) a los campos del serializer.
- **Implementaciones `create()` y `update()`**: Proporciona una implementación por defecto para los métodos `.create()` y `.update()`, lo que te permite guardar instancias en la base de datos con solo llamar a `serializer.save()`.
#### Ejemplo Práctico
Imagina que tienes el siguiente modelo en tu aplicación:
**`models.py`**
```python
from django.db import models

class Product(models.Model):
    name = models.CharField(max_length=150)
    description = models.TextField(blank=True)
    sku = models.CharField(max_length=50, unique=True)
    price = models.DecimalField(max_digits=10, decimal_places=2)
    is_active = models.BooleanField(default=True)

    def __str__(self):
        return self.name
```
Crear un serializer para este modelo es increíblemente simple:
**`serializers.py`**
```python
from rest_framework import serializers
from .models import Product

class ProductSerializer(serializers.ModelSerializer):
    class Meta:
        model = Product
        fields = ['id', 'name', 'sku', 'price', 'description', 'is_active']
```

> [!SUCCESS] Código Mínimo, Máxima Potencia
> 
> ✅ ¡Eso es todo! Con estas pocas líneas, `ProductSerializer` ya sabe cómo validar y manejar todos los campos del modelo `Product`. No fue necesario redefinir `CharField`, `DecimalField`, etc.

---
### ⚙️ Configurando la Clase `Meta`
La clase interna `Meta` es el centro de control del `ModelSerializer`. Aquí defines qué modelo usar y qué campos incluir o excluir.
- `model`: El modelo de Django que el serializer va a manejar. **(Obligatorio)**
- `fields`: Una lista con los nombres de los campos del modelo a incluir.
    - Para incluir todos los campos: `fields = '__all__'`
- `exclude`: Una lista con los nombres de los campos del modelo a excluir.
    - No puedes usar `fields` y `exclude` al mismo tiempo.
- `read_only_fields`: Una lista de campos que se incluirán en la salida (serialización) pero se ignorarán en la entrada (deserialización). Útil para campos como `created_at` o `id`.
- `write_only_fields`: Una lista de campos que se pueden usar en la entrada (deserialización) pero no se mostrarán en la salida. Perfecto para campos como `password`.

---
### 🔄 El Flujo de Datos: De JSON a Objeto y Viceversa
#### 1. Serialización (Objeto ➡️ JSON)
Se usa para leer datos de la base de datos y enviarlos en una respuesta.
```python
# Obtener un producto
product = Product.objects.get(sku="ABC-123")
serializer = ProductSerializer(product)
print(serializer.data) 
# Output: {'id': 1, 'name': 'Laptop Pro', 'sku': 'ABC-123', ...}

# Para un listado (QuerySet)
products = Product.objects.filter(is_active=True)
serializer = ProductSerializer(products, many=True)
print(serializer.data)
# Output: [ {'id': 1, ...}, {'id': 2, ...} ]
```

> [!INFO] many=True
> 
> Cuando serializas un QuerySet (múltiples objetos), es fundamental pasar el argumento many=True para que el serializer sepa que debe procesar una lista.
#### 2. Deserialización (JSON ➡️ Objeto)
Se usa para procesar datos que llegan en una petición (`POST` o `PUT`).
```python
# request.data contiene el JSON parseado de la petición
incoming_data = {'name': 'Nuevo Producto', 'sku': 'XYZ-789', 'price': '99.99'}

serializer = ProductSerializer(data=incoming_data)

if serializer.is_valid(raise_exception=True):
    # .save() llamará al método .create() porque no se pasó una instancia inicial
    new_product = serializer.save() 
    print(f"Producto '{new_product.name}' creado con éxito.")
```

> [!DANGER] ¡La Validación es Clave!
> 
> 🛑 Peligro: Siempre debes llamar a .is_valid() antes de intentar guardar o acceder a .validated_data. Si los datos son inválidos y no lo haces, se lanzará una excepción. Usar raise_exception=True es una buena práctica que devuelve automáticamente una respuesta 400 Bad Request con los errores.

---
### ✅ Personalización y Validación Avanzada
Aunque `ModelSerializer` automatiza mucho, sigues teniendo control total.
- **Cambiar el nombre de un campo**:
	```python
	class MenuItemSerializer(serializers.ModelSerializer):
		stock = serializers.IntegerField(source='inventory') # Aquí está la clave
		
		class Meta:
			model = MenuItem
			# Remplazar el nombre de el campo 'inventory' por 'stock'
			fields = ['id', 'title', 'price', 'stock']
	```
- **Uso de métodos**:
	```python
	class MenuItemSerializer(serializers.ModelSerializer):
		price_after_tax = serializers.SerializerMethodField(method_name='calculate_tax')

		class Meta:
			model = MenuItem
			fields = ['id', 'title', 'price', 'price_after_tax']

		def calculate_tax(self, product:MenuItem):
			return product.price * Decimal(1.1)
	```
- **Añadir campos extra**: Puedes declarar campos que no existen en el modelo.
    ```python
    class ProductSerializer(serializers.ModelSerializer):
        discounted_price = serializers.SerializerMethodField()
    
        class Meta:
            model = Product
            fields = ['id', 'name', 'price', 'discounted_price']
    
        def get_discounted_price(self, obj):
            # obj es la instancia del producto
            return obj.price * 0.90
    ```
    
- **Sobrescribir campos**: Si necesitas un comportamiento diferente para un campo, simplemente decláralo explícitamente en el serializer.
    ```python
    class ProductSerializer(serializers.ModelSerializer):
        # El campo 'name' ahora tendrá un validador personalizado
        name = serializers.CharField(max_length=150, validators=[...])
    
        class Meta:
            model = Product
            fields = '__all__'
    ```
    
- **Validación a nivel de objeto**: Usa el método `validate` para lógicas que involucren múltiples campos.
    ```python
    def validate(self, data):
        # 'data' es un diccionario con los datos de entrada
        if data['price'] < 0:
            raise serializers.ValidationError("El precio no puede ser negativo.")
        return data
    ```