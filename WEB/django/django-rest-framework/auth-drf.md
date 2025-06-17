---
aliases:
  - 🔑 Autenticación por Token ✨🔒
tags:
  - django
  - django-rest-framework
  - apis
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
  - "[[indice-apis|🔌 Índice APIs]]"
Fecha: 2025-06-12
---
# 🔑 Autenticación por Token en Django REST Framework ✨🔒
La autenticación por token es un método popular y seguro para manejar la autenticación de usuarios en las APIs web. 🌐 Este enfoque se basa en el envío de un token único con cada solicitud después del inicio de sesión inicial. 🚀

---
## 🤔 ¿Qué es la autenticación por token? 📝✨
La autenticación por token implica la emisión de una cadena opaca (el **token**) a un cliente después de que este inicie sesión con éxito utilizando sus credenciales. 🗝️ Este **token** se envía con cada solicitud subsiguiente para probar la identidad del cliente sin tener que enviar el nombre de usuario y la contraseña repetidamente. 🔄 Este método es sin estado, escalable y generalmente más seguro que la autenticación basada en sesiones para APIs. 🛡️

---
## 🛠️ Configuración de la autenticación por Token en DRF ⚙️🚀
Para implementar la autenticación por **token** en tu proyecto de Django REST Framework, deberás seguir algunos pasos clave. ¡Es bastante sencillo! 😉
### 1. Instala DRF 📥💻
Primero, asegúrate de tener Django REST Framework instalado. Si no es así, puedes instalarlo a través de pip:
```shell
pip install djangorestframework ✨
```
### 2. Agrégalo a `INSTALLED_APPS` ➕📜
Añade `'rest_framework'` y `'rest_framework.authtoken'` a tu lista de `INSTALLED_APPS` en `settings.py`:
```python
# settings.py ⚙️
INSTALLED_APPS = [
    # ... otras aplicaciones
    'rest_framework', 🚀
    'rest_framework.authtoken', 🔑
]
```
### 3. Ejecuta las migraciones 🏃‍♀️💨

Aplica las migraciones para crear la tabla de **token** necesaria en tu base de datos:
```shell
python manage.py migrate ✨
```
Este comando crea una tabla donde se almacenarán los **tokens** de usuario. 💾
### 4. Configura las clases de autenticación 🛠️🔒
En tu `settings.py`, necesitas configurar DRF para que use `TokenAuthentication` globalmente. También puedes especificar esto por vista. 🌐
```python
# settings.py ⚙️
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.TokenAuthentication', 🔑
    ],
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated', ✅
    ]
}
```
> [!TIP] Global vs. por Vista 💡
> 
> Puedes establecer `authentication_classes` y `permission_classes` en clases individuales de `APIView` o `ViewSet` para anular las configuraciones globales. ¡Esto ofrece una gran flexibilidad! ✨

---
## 5. Obtención de un Token 🤝🔐
Los usuarios deberán obtener un **token** después de iniciar sesión. DRF proporciona una vista integrada para esto. 🧑‍💻
### a. Agrega la vista de Token a `urls.py` 🔗🛣️
Incluye la vista `obtain_auth_token` en el `urls.py` de tu proyecto:
```python
# urls.py 🌐
from django.urls import path, include
from rest_framework.authtoken.views import obtain_auth_token ✨

urlpatterns = [
    # ... otros patrones de URL
    path('api-token-auth/', obtain_auth_token), 🔑
]
```

### b. Realizando la Solicitud para Obtener un Token 📩🔑
Un cliente ahora puede enviar una solicitud POST a `api-token-auth/` con su nombre de usuario y contraseña para obtener un **token**. 📧

| **Método** | **URL**            | **Cuerpo (JSON)**                                       |
| ---------- | ------------------ | ------------------------------------------------------- |
| POST       | `/api-token-auth/` | `{"username": "miusuario", "password": "micontraseña"}` |

La respuesta se verá así:
```json
{
    "token": "9944b09199c62bcf9418ad846dd0ee4bb4fd6ec2" 🔑
}
```

> [!NOTE] Expiración del Token ⏳
> 
> Por defecto, los tokens de TokenAuthentication de DRF no expiran. Es posible que desees implementar tu propia lógica de expiración de tokens para una seguridad mejorada. 🔒

---
## 6. Uso del Token para Solicitudes Autenticadas 🚀🔐

Una vez que el cliente tiene el **token**, debe incluirlo en el encabezado `Authorization` de todas las solicitudes subsiguientes. 📨

| **Nombre del Encabezado** | **Valor del Encabezado**                         |
| ------------------------- | ------------------------------------------------ |
| `Authorization`           | `Token 9944b09199c62bcf9418ad846dd0ee4bb4fd6ec2` |

Ejemplo con `curl`:
```bash
curl -X GET http://127.0.0.1:8000/api/mis-datos-seguros/ \
-H 'Authorization: Token 9944b09199c62bcf9418ad846dd0ee4bb4fd6ec2' ✨
```
Si el **token** es válido, la solicitud será autenticada y `request.user` se establecerá en la instancia de usuario correspondiente. ✅

---

## 🚨 Consideraciones de Seguridad 🛡️⚠️
- **Solo HTTPS:** Siempre usa HTTPS para evitar que los **tokens** sean interceptados. 🔒🌐
- **Almacenamiento del Token:** Los clientes deben almacenar los **tokens** de forma segura (por ejemplo, en `localStorage` o `sessionStorage` para aplicaciones web, o almacenamiento seguro para aplicaciones móviles). 💾
- **Revocación del Token:** Implementa un mecanismo para revocar **tokens**, especialmente si un usuario cierra sesión o si su **token** se ve comprometido. 🚫
- **Limitación de Tasa:** Protege tu punto final `/api-token-auth/` con limitación de tasa para prevenir ataques de fuerza bruta. ⏱️

> [!WARNING] Datos Sensibles 🛑
> 
> ¡Nunca expongas tu token directamente en los parámetros de consulta de la URL! Siempre usa el encabezado Authorization. ⚠️

---
## 💡 Personalización de la Autenticación por Token ✨🛠️

Puedes extender la `TokenAuthentication` de DRF para implementar lógica personalizada, como la expiración del **token** o la autenticación multifactor. 🧑‍💻
### Ejemplo: Expiración del Token ⏳
Aquí tienes un ejemplo simple de cómo podrías implementar un mecanismo de expiración de **token**. Necesitarías almacenar la marca de tiempo `created` para cada **token**. 🕰️
```python
# myapp/authentication.py 🔑
from rest_framework.authentication import TokenAuthentication
from rest_framework.exceptions import AuthenticationFailed
from datetime import timedelta
from django.utils import timezone

class ExpiringTokenAuthentication(TokenAuthentication):
    def authenticate_credentials(self, key):
        model = self.get_model()
        try:
            token = model.objects.select_related('user').get(key=key) 🔑
        except model.DoesNotExist:
            raise AuthenticationFailed('Token inválido. 🛑')

        if not token.user.is_active:
            raise AuthenticationFailed('Usuario inactivo o eliminado. 🚫')

        # Comprueba si el token ha expirado (ej. después de 24 horas) ⏰
        if token.created < timezone.now() - timedelta(hours=24):
            token.delete() # Opcionalmente elimina el token expirado
            raise AuthenticationFailed('El token ha expirado. ⏳')

        return (token.user, token) ✨
```

Luego, usa esta clase personalizada en tu `settings.py` o directamente en tus vistas. ⚙️
```python
# settings.py ⚙️
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'myapp.authentication.ExpiringTokenAuthentication', 🔑 # Usa tu clase personalizada
    ],
    # ...
}
```

> [!SUCCESS] Consejo de Implementación ✅
> 
> Al implementar una autenticación personalizada, recuerda siempre manejar las excepciones AuthenticationFailed para devolver respuestas de error apropiadas. 🎉