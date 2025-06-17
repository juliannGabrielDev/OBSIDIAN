---
aliases:
  - ✨ La Importancia de la Validación de Datos en DRF 🔒
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
# ✨ La Importancia de la Validación de Datos en DRF 🔒
En el desarrollo de aplicaciones web, la **validación de datos** es un paso crucial que garantiza la integridad y seguridad de la información. Asegura que los datos proporcionados por el usuario sean válidos, completos y aptos para ser almacenados en tu base de datos. ¡Vamos a explorar cómo Django REST Framework (DRF) nos facilita esta tarea! 🚀
## 🛡️ ¿Qué es la Validación de Datos?
La validación es el proceso de verificar que los datos enviados por un usuario:
- Estén en el **formato correcto**. ✅
- Cumplan con los **requisitos establecidos**. 📏
- Sean **seguros** para añadir a la base de datos. 🔐
Los serializadores en DRF ofrecen diversas características para validar estos datos mientras construyes tus APIs. Para entenderlo mejor, consideremos algunos escenarios de entrada de usuario al añadir o modificar elementos del menú en un proyecto API, y cómo deberían ser validados.

| **Campo** | **Valor**            | **Estado**                                                               |
| --------- | -------------------- | ------------------------------------------------------------------------ |
| `price`   | `0`                  | **No válido**, el precio de un artículo no puede ser 0. 🚫               |
| `stock`   | `negative value`     | **No válido**, el stock no puede ser inferior a 0. 📉                    |
| `title`   | `Valores duplicados` | **No válido**, no puede haber más de un artículo con el mismo nombre. 📛 |

Además de estas validaciones comunes, cada proyecto puede tener requisitos adicionales. Por ejemplo, en el proyecto "Little Lemon", el precio podría no ser inferior a $2.00. Si alguien intenta añadir artículos con un precio menor, debería producirse un error.

---
## 🚀 Validación en DRF: Métodos y Técnicas
DRF proporciona varias maneras de implementar la validación dentro de tus serializadores. A continuación, veremos las cuatro formas principales de validar campos en el `MenuItemSerializer`.
### 📝 `MenuItemSerializer` y `CategorySerializer`
Aquí tenemos los serializadores base que usaremos para los ejemplos:
```python
from rest_framework import serializers
from decimal import Decimal
from .models import MenuItem, Category
 
class CategorySerializer (serializers.ModelSerializer):
    class Meta:
        model = Category
        fields = ['id','slug','title']
 
class MenuItemSerializer(serializers.ModelSerializer):
    stock =  serializers.IntegerField(source='inventory')
    price_after_tax = serializers.SerializerMethodField(method_name = 'calculate_tax')
    category_id = serializers.IntegerField(write_only=True)
    category = CategorySerializer(read_only=True)
    class Meta:
        model = MenuItem
        fields = ['id','title','price','stock', 'price_after_tax','category','category_id']
    
    def calculate_tax(self, product:MenuItem):
        return product.price * Decimal(1.1)
```
---
### Método 1: ✍️ Condiciones en el Campo
Puedes aplicar reglas de validación directamente al declarar el campo en tu serializador. Por ejemplo, para asegurarte de que el `price` no sea inferior a `2.0`:
```python
# Dentro de MenuItemSerializer, antes de la clase Meta
price = serializers.DecimalField(max_digits=6, decimal_places=2, min_value=2)
```
Si realizas una llamada `POST` al endpoint `menu-items` con un precio de `1`, DRF te mostrará un error indicando que el precio debe ser mayor o igual a `2`. ¡La validación funciona! ✅

---
### Método 2: 🔑 Argumentos de Palabra Clave en `Meta`
Este método es útil si no declaras explícitamente el campo en el serializador (es decir, DRF lo infiere del modelo). Puedes añadir una sección `extra_kwargs` dentro de la clase `Meta` para aplicar validaciones adicionales.
```python
class Meta:
    model = MenuItem
    fields = ['id','title','price','stock', 'price_after_tax','category','category_id']
    extra_kwargs = {
        'price': {'min_value': 2},
        'stock':{'source':'inventory', 'min_value': 0} # Agregamos validación para stock
    }
```

Con `extra_kwargs`, si envías una solicitud `POST` con un precio de `1` o un `stock` negativo, recibirás los errores de validación correspondientes. ¡Es una forma limpia de mantener las validaciones específicas de campo en un solo lugar! 📦

---
### Método 3: 🛠️ Uso del Método `validate_field()`
DRF te permite escribir métodos de validación específicos para cada campo. El patrón de nombres es `validate_` seguido del nombre del campo (ej., `validate_price()`). El valor enviado por el usuario se pasa como argumento.
```python
# Dentro de MenuItemSerializer
def validate_price(self, value):
    if value < 2:
        raise serializers.ValidationError('Price should not be less than 2.0')
    return value  # ¡Importante retornar el valor si pasa la validación!
    
def validate_stock(self, value):
    if value < 0:
        raise serializers.ValidationError('Stock cannot be negative')
    return value  # ¡Importante retornar el valor si pasa la validación!
```

En estos métodos, si el `value` no cumple con el requisito, se lanza un `serializers.ValidationError` con un mensaje descriptivo. ¡Esto te da un control granular sobre cada campo! 🎯

---
### Método 4: 🧐 Uso del Método `validate()`
Este método te permite validar múltiples campos a la vez, recibiendo todos los valores de entrada como un diccionario de atributos (`attrs`). Es ideal para validaciones que dependen de la combinación de varios campos.

> [!NOTE] ¡Importante!
> 
> Si utilizas este método, asegúrate de eliminar los métodos `validate_price` y `validate_stock` individuales para evitar redundancias.

```python
# Dentro de MenuItemSerializer, antes de la clase Meta
def validate(self, attrs):
    if(attrs['price'] < 2):
        raise serializers.ValidationError('Price should not be less than 2.0')
    if(attrs['inventory'] < 0): # ¡Ojo! Usamos 'inventory' aquí, el nombre real del campo del modelo
        raise serializers.ValidationError('Stock cannot be negative')
    return super().validate(attrs) # ¡No olvides llamar al método padre!
```

Este método es poderoso para validaciones cruzadas entre campos. Recuerda que, dentro del método `validate()`, debes referirte al nombre real del campo del modelo (`inventory`) si usaste `source` en tu serializador.

---
## 🤝 Validación Única: `UniqueValidator` y `UniqueTogetherValidator`
A veces, necesitas asegurarte de que no haya entradas duplicadas en tu base de datos. Para esto, DRF ofrece validadores de unicidad.
```python
from rest_framework.validators import UniqueValidator, UniqueTogetherValidator
```
### `UniqueValidator` (Campo Único)
Para garantizar la unicidad de un solo campo, como el `title` en la tabla `MenuItem`, puedes usar `UniqueValidator`.
- **Opción 1: En `extra_kwargs` de `Meta`**
    ```python
    extra_kwargs = {
        'title': {
            'validators': [
                UniqueValidator(
                    queryset=MenuItem.objects.all()
                )
            ]
        }
    }
    ```
- **Opción 2: Al declarar el campo explícitamente**
    ```python
    title = serializers.CharField(
        max_length=255,
        validators=[UniqueValidator(queryset=MenuItem.objects.all())]
    )
    ```

Si un cliente intenta enviar una entrada con un título duplicado, DRF generará un error de validación. 🛑
### `UniqueTogetherValidator` (Combinación de Campos Únicos)
Cuando la unicidad depende de la combinación de dos o más campos, `UniqueTogetherValidator` es tu aliado. Por ejemplo, para asegurar que no haya dos artículos con el mismo `title` _y_ el mismo `price`:
```python
# Dentro de la clase Meta del serializador
validators = [
    UniqueTogetherValidator(
        queryset=MenuItem.objects.all(),
        fields=['title', 'price'] # Los campos cuya combinación debe ser única
    ),
]
```

Con esta validación, si un cliente envía una entrada duplicada con la misma combinación de título y precio, DRF lo detectará y devolverá un error. ❌

---
## 🎯 Conclusión

La **validación de datos** es un pilar fundamental en la construcción de APIs robustas y seguras con Django REST Framework. Ya sea que necesites aplicar condiciones simples, validaciones a nivel de campo, verificaciones cruzadas entre campos o asegurar la unicidad de tus datos, DRF te ofrece las herramientas necesarias para cada escenario.