---
aliases:
  - 🚦 Estrangulamiento (Throttling) de API para Vistas Basadas ⚙️🚀
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
# 🚦 Estrangulamiento (Throttling) de API para Vistas Basadas en Clases en Django REST Framework ⚙️🚀
El **estrangulamiento (throttling)** es crucial para proteger tus APIs de abusos y gestionar el consumo de recursos. En Django REST Framework (DRF), aplicar **throttling** a **vistas basadas en clases (Class-Based Views - CBV)** es un proceso sencillo y potente. ✨

---
## 🤔 ¿Por qué usar Throttling con CBV? 🎯
Las **vistas basadas en clases** proporcionan una forma estructurada y reutilizable de manejar las solicitudes HTTP. Integrar el **throttling** directamente en ellas permite una gestión de límites de tasa más modular y limpia, alineándose con el diseño de DRF. 🏗️

---
## 🛠️ Configuración Básica de Throttling en CBV 🧱
Aplicar **throttling** a una **vista basada en clases** es tan fácil como definir el atributo `throttle_classes` dentro de la clase. 📝
### 1. Clases de Throttling Comunes 👥
DRF ofrece clases de **throttling** predefinidas para diferentes escenarios:
- `AnonRateThrottle`: Para usuarios no autenticados (anónimos). 👻
- `UserRateThrottle`: Para usuarios autenticados. ✅
### 2. Ejemplo de Implementación en una CBV 🚀
Vamos a crear una **vista basada en clases** que utilice **throttling** para usuarios anónimos y autenticados.
```python
# myapp/views.py 📜
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework.throttling import AnonRateThrottle, UserRateThrottle
from rest_framework import status

class ThrottledAPIView(APIView):
    # Define las clases de throttling para esta vista
    throttle_classes = [AnonRateThrottle, UserRateThrottle] # Aplica ambas por defecto ✨

    def get(self, request, format=None):
        return Response({"message": "¡Acceso permitido a la vista estrangulada!"}, status=status.HTTP_200_OK) 🚀
```
### 3. Mapear la Vista en `urls.py` 🔗
Asegúrate de que tu vista esté mapeada en el archivo `urls.py` de tu aplicación o proyecto.
```python
# myproject/urls.py o myapp/urls.py 🌐
from django.urls import path
from myapp.views import ThrottledAPIView

urlpatterns = [
    path('api/throttled-cbv/', ThrottledAPIView.as_view(), name='throttled-cbv'), 🚦
]
```
### 4. Configurar las Tasas en `settings.py` ⚙️

Como con las vistas basadas en funciones, debes definir las tasas de **throttling** en tu `settings.py` bajo `DEFAULT_THROTTLE_RATES`.
```python
# settings.py ⚙️
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.TokenAuthentication',
        # ... otras autenticaciones
    ],
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticatedOrReadOnly', # O IsAuthenticated para proteger completamente
    ],
    'DEFAULT_THROTTLE_RATES': {
        'anon': '5/minute',  # 5 llamadas por minuto para usuarios anónimos 👻
        'user': '20/minute', # 20 llamadas por minuto para usuarios autenticados ✅
    }
}
```
---
## 🎨 Estrangulamiento Personalizado con Scopes en CBV ✨🛠️

Al igual que con las vistas basadas en funciones, puedes definir **clases de throttling personalizadas** y asignarlas a **scopes** específicos para tener un control más granular. 📏
### 1. Define tu Clase de Throttling Personalizada (en `myapp/throttles.py`) 📝
```python
# myapp/throttles.py 🔑
from rest_framework.throttling import UserRateThrottle

class CustomUserRateThrottle(UserRateThrottle):
    scope = 'custom_throttle_scope' # Define un scope único para esta regla 🎯
```
### 2. Actualiza `settings.py` con el Nuevo Scope ⚙️
Añade la tasa para tu nuevo **scope** en `DEFAULT_THROTTLE_RATES`.
```python
# settings.py ⚙️
REST_FRAMEWORK = {
    # ... otras configuraciones
    'DEFAULT_THROTTLE_RATES': {
        'anon': '5/minute',
        'user': '20/minute',
        'custom_throttle_scope': '100/hour', # 100 llamadas por hora para este scope específico ⏱️
    }
}
```
### 3. Aplica la Clase Personalizada a tu CBV 🚀
Importa tu clase de **throttling** personalizada y úsala en el atributo `throttle_classes` de tu **vista basada en clases**.
```python
# myapp/views.py 📜
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from .throttles import CustomUserRateThrottle # Importa tu clase personalizada ✨

class CustomThrottledAPIView(APIView):
    # Usa tu clase de throttling personalizada
    throttle_classes = [CustomUserRateThrottle] # Solo esta regla se aplica aquí 🎯

    def get(self, request, format=None):
        return Response({"message": "¡Acceso permitido con límite personalizado!"}, status=status.HTTP_200_OK) 🌟
```
### 4. Mapear la Nueva Vista en `urls.py` 🔗
```python
# myproject/urls.py o myapp/urls.py 🌐
from django.urls import path
from myapp.views import CustomThrottledAPIView

urlpatterns = [
    path('api/custom-throttled-cbv/', CustomThrottledAPIView.as_view(), name='custom-throttled-cbv'), 🚦
]
```
---
## 🚨 Consideraciones Importantes 🛡️⚠️
- **Orden de `throttle_classes`**: Si especificas múltiples clases en `throttle_classes`, se aplicarán todas. La primera clase que active una limitación detendrá la solicitud. 🚫
- **Combinación con Permisos**: El **throttling** funciona de la mano con los **permisos**. Asegúrate de que tus permisos estén configurados correctamente para que los usuarios autenticados o no autenticados reciban las reglas de **throttling** esperadas. 🔐
- **Atributo `scope`**: El atributo `scope` en tus clases de **throttling** personalizadas es la clave para conectar esas clases con las tasas definidas en `settings.py`. ¡Debe ser único y descriptivo! 🏷️
- **Pruebas Rigurosas**: Siempre prueba tus configuraciones de **throttling** exhaustivamente, especialmente en entornos de desarrollo y staging, para asegurarte de que se comporten como esperas. 🧪

> [!TIP] Diferencias con Vistas Basadas en Funciones 💡
> 
> Para las vistas basadas en funciones, usamos el decorador @throttle_classes. Para las vistas basadas en clases, simplemente definimos el atributo throttle_classes dentro de la clase. ¡Es la misma lógica, pero con sintaxis adaptada! 🔄