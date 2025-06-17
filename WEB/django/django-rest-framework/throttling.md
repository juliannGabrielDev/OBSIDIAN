---
aliases:
  - "🚧 Throttling: Protege tus APIs del Abuso 🛡️🚀"
tags:
  - django
  - django-rest-framework
  - apis
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
  - "[[indice-apis|🔌 Índice APIs]]"
Fecha: 2025-06-13
---
# 🚧 Throttling en Django REST Framework: Protege tus APIs del Abuso 🛡️🚀

El **throttling** (limitación de tasa) es una técnica súper efectiva para prevenir el abuso de tus APIs. Cada solicitud consume poder de cómputo y ancho de banda, y sin monitoreo, tu infraestructura de API podría estar en riesgo. Con el **throttling**, puedes controlar la frecuencia con la que se puede acceder a tu API en un período de tiempo determinado. ¡DRF lo hace muy fácil! ✨

## 🚦 Tipos de Throttling en DRF 🎯
DRF incluye **clases de throttling** preconstruidas para distintos tipos de usuarios. 🧑‍💻
1. **Usuarios anónimos o no autenticados**: Se aplica cuando no hay un **token** válido en el encabezado de la API. 👻
2. **Usuarios autenticados**: Se utiliza cuando hay **tokens** válidos presentes. ✅
Además, puedes crear reglas de **throttling** personalizadas para endpoints específicos. 🛠️
## 🛠️ Configuración de Throttling para Usuarios Anónimos 🚫👥
Vamos a limitar un endpoint para usuarios anónimos a dos llamadas por minuto. ⏱️
### 1. Crea la función `throttle_check` en `views.py` 📝
```python
# views.py 📜
from rest_framework.decorators import api_view, throttle_classes
from rest_framework.response import Response
from rest_framework.throttling import AnonRateThrottle ✨

@api_view(['GET'])
@throttle_classes([AnonRateThrottle]) # Aplica la limitación para usuarios anónimos
def throttle_check(request):
    return Response({"message": "¡Solicitud exitosa!"}) 🚀
```

### 2. Mapea la función en `urls.py` 🔗
```python
# urls.py 🌐
from django.urls import path
from .views import throttle_check

urlpatterns = [
    path('api/throttle-check/', throttle_check), 🚦
]
```
### 3. Configura la tasa en `settings.py` ⚙️
Añade la siguiente configuración en la sección `REST_FRAMEWORK` de tu `settings.py`:
```python
# settings.py ⚙️
REST_FRAMEWORK = {
    # ... otras configuraciones
    'DEFAULT_THROTTLE_RATES': {
        'anon': '2/minute', ⏱️
    }
}
```

> [!NOTE] Unidades de Tiempo ⏳
> 
> En lugar de minute, puedes usar second, hour o day (por ejemplo, 20/day para 20 llamadas por día). 📆

---
## 🔒 Throttling para Usuarios Autenticados 🤝👤
Ahora, limitemos un endpoint a 10 llamadas por minuto para usuarios autenticados.
### 1. Crea la función `throttle_check_auth` en `views.py` 📝
```python
# views.py 📜
from rest_framework.decorators import api_view, throttle_classes
from rest_framework.response import Response
from rest_framework.throttling import UserRateThrottle ✨ # Importa UserRateThrottle

@api_view(['GET'])
@permission_classes([IsAuthenticated])
@throttle_classes([UserRateThrottle]) # Aplica la limitación para usuarios autenticados
def throttle_check_auth(request):
    return Response({"message": "¡Solicitud autenticada exitosa!"}) 🔐
```
### 2. Mapea la función en `urls.py` 🔗
```python
# urls.py 🌐
from django.urls import path
from .views import throttle_check_auth

urlpatterns = [
    path('api/throttle-check-auth/', throttle_check_auth), 🚦
]
```
### 3. Configura la tasa en `settings.py` ⚙️
Añade esta línea en la sección `DEFAULT_THROTTLE_RATES` de tu `settings.py`:
```python
# settings.py ⚙️
REST_FRAMEWORK = {
    'DEFAULT_THROTTLE_RATES': {
        'anon': '2/minute',
        'user': '5/minute', # Configura la limitación para usuarios autenticados 🖐️
    }
}
```

## 🎨 Creación de Reglas de Throttling Personalizadas con Scopes ⚙️✨

¿Qué pasa si quieres una política de **throttling** diferente para algunos endpoints autenticados? ¡Podemos usar **scopes**! 📏
### 1. Crea el archivo `throttles.py` 🆕📄
En el directorio de tu aplicación, crea un nuevo archivo llamado `throttles.py` y añade el siguiente código:
```python
# myapp/throttles.py 🔑
from rest_framework.throttling import UserRateThrottle

class TenCallsPerMinute(UserRateThrottle):
    scope = 'ten' # Define un ámbito llamado 'ten' 🔟
```
Este código crea una nueva política de **throttling** con un **scope** llamado `'ten'`.
### 2. Configura el nuevo scope en `settings.py` ⚙️
Abre `settings.py` y añade la siguiente línea a la sección `DEFAULT_THROTTLE_RATES`:
```python
# settings.py ⚙️
REST_FRAMEWORK = {
    'DEFAULT_THROTTLE_RATES': {
        'anon': '2/minute',
        'user': '5/minute',
        'ten': '10/minute', # Asocia el ámbito 'ten' con 10 llamadas por minuto 🚀
    }
}
```
### 3. Usa la clase personalizada en `views.py` 📝
Importa la clase `TenCallsPerMinute` y úsala como clase de **throttling** para cualquier función de API que necesite esta política personalizada.
```python
# views.py 📜
from rest_framework.decorators import api_view, throttle_classes
from rest_framework.response import Response
from rest_framework.throttling import UserRateThrottle
from .throttles import TenCallsPerMinute # Importa tu clase personalizada ✨

@api_view(['GET'])
@throttle_classes([TenCallsPerMinute]) # Usa la nueva clase de throttling
def throttle_check_custom_auth(request):
    return Response({"message": "¡Solicitud autenticada con límite personalizado!"}) 🎯
```
### 4. Mapea la nueva función en `urls.py` 🔗
```python
# urls.py 🌐
from django.urls import path
from .views import throttle_check_custom_auth

urlpatterns = [
    path('api/throttle-check-custom-auth/', throttle_check_custom_auth), 🚦
]
```
¡Eso es todo! Ahora, los usuarios autenticados pueden acceder a este endpoint 10 veces por minuto, y DRF bloqueará cualquier llamada adicional. 🛑