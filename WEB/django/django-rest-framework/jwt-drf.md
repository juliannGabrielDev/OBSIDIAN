---
aliases:
  - Autenticación con JWT en Django 🔐🔑✨
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
# Autenticación con JWT en Django 🔐🔑✨

La autenticación con JSON Web Tokens (JWT) en Django ofrece una alternativa moderna y sin estado a las sesiones tradicionales. 🚀 Es ideal para APIs RESTful y aplicaciones de una sola página (SPAs).

## ¿Qué es JWT? 🤔💡

Un JWT es un token compacto y seguro URL que define una forma compacta y autocontenida de transmitir información de forma segura entre dos partes. Se compone de tres partes, separadas por puntos:
- **Header (Cabecera) 🎉:** Contiene el tipo de token (JWT) y el algoritmo de cifrado (ej., HMAC SHA256 o RSA).
- **Payload (Carga Útil) 📦:** Contiene las "claims" o declaraciones sobre la entidad (normalmente el usuario) y metadatos adicionales.
- **Signature (Firma) ✍️:** Se utiliza para verificar que el emisor del JWT es quien dice ser y para asegurarse de que el mensaje no ha sido alterado.

## Ventajas de JWT en Django ✅🚀
- **Sin Estado (Stateless) 👻:** El servidor no necesita almacenar información de sesión, lo que simplifica la escalabilidad.
- **Rendimiento ⚡:** Menos consultas a la base de datos ya que el token contiene la información necesaria.
- **Multi-Dispositivo 📱💻:** Fácil de usar en diferentes clientes (web, móvil) sin problemas de sesiones compartidas.
- **Seguridad 🔒:** La firma asegura la integridad y autenticidad del token.

> [!NOTE] ¿Cuándo usar JWT?  
> 
> 🗒️ JWT es excelente para APIs RESTful, microservicios y aplicaciones que necesitan una autenticación ligera y escalable.

## Pasos para Implementar JWT en Django 🪜🛠️

Aquí te presento los pasos clave para integrar JWT en tu proyecto Django:

### 1. Instalación de `djangorestframework-simplejwt` 📥✨

Este paquete simplifica enormemente la implementación de JWT en Django REST Framework.

```bash
pip install djangorestframework-simplejwt
```

### 2. Configuración en `settings.py` ⚙️📄

Añade las aplicaciones necesarias y configura los ajustes de JWT.

```python
# settings.py

INSTALLED_APPS = [
    # ... otras apps
    'rest_framework',
    'rest_framework_simplejwt',
]

REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    )
}

from datetime import timedelta

SIMPLE_JWT = {
    'ACCESS_TOKEN_LIFETIME': timedelta(minutes=5),  # Tiempo de vida del token de acceso
    'REFRESH_TOKEN_LIFETIME': timedelta(days=1),    # Tiempo de vida del token de refresco
    'ROTATE_REFRESH_TOKENS': True,                  # Rotar tokens de refresco
    'BLACKLIST_AFTER_ROTATION': True,               # Poner en la lista negra tokens antiguos
    'UPDATE_LAST_LOGIN': False,                     # No actualizar 'last_login' por cada token

    'ALGORITHM': 'HS256',
    'SIGNING_KEY': 'tu_secreto_muy_seguro', # ¡Cambia esto en producción!
    'VERIFYING_KEY': None,
    'AUDIENCE': None,
    'ISSUER': None,
    'JWK_URL': None,
    'LEEWAY': 0,

    'AUTH_HEADER_TYPES': ('Bearer',),
    'AUTH_HEADER_NAME': 'HTTP_AUTHORIZATION',
    'USER_ID_FIELD': 'id',
    'USER_ID_CLAIM': 'user_id',
    'USER_AUTHENTICATION_RULE': 'rest_framework_simplejwt.authentication.default_user_authentication_rule',

    'AUTH_TOKEN_CLASSES': ('rest_framework_simplejwt.tokens.AccessToken',),
    'TOKEN_TYPE_CLAIM': 'token_type',
    'TOKEN_USER_CLASS': 'rest_framework_simplejwt.models.TokenUser',

    'JTI_CLAIM': 'jti',

    'SLIDING_TOKEN_LIFETIME': timedelta(minutes=5),
    'SLIDING_TOKEN_REFRESH_LIFETIME': timedelta(days=1),
}
```

> [!WARNING] Clave Secreta 🔑⚠️  
> 
> ⚠️ ¡Importante! La SIGNING_KEY debe ser una cadena aleatoria y muy segura. En producción, ¡no uses una cadena fija como esta! Utiliza variables de entorno o un generador de claves.

### 3. Configuración de URLs 🌐🔗

Añade las rutas para obtener y refrescar tokens.

```python
# urls.py de tu proyecto
from django.contrib import admin
from django.urls import path, include
from rest_framework_simplejwt.views import (
    TokenObtainPairView,
    TokenRefreshView,
)

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/token/', TokenObtainPairView.as_view(), name='token_obtain_pair'),
    path('api/token/refresh/', TokenRefreshView.as_view(), name='token_refresh'),
    # ... otras URLs de tu API
]
```

### 4. Protegiendo Vistas 🛡️🔒

Ahora, tus vistas de Django REST Framework estarán protegidas por JWT.

```python
# app_name/views.py
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework.permissions import IsAuthenticated

class HelloView(APIView):
    permission_classes = (IsAuthenticated,) # Asegura que el usuario esté autenticado

    def get(self, request):
        content = {'message': f'Hola, {request.user.username}!'}
        return Response(content)
```

## Flujo de Autenticación con JWT 🔄📊

Aquí te presento un esquema del flujo de autenticación:

| **Paso**                   | **Descripción**                                                                                                                              |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Login**               | El cliente envía credenciales (usuario/contraseña) al endpoint `/api/token/`.                                                                |
| **2. Obtención de Tokens** | El servidor valida las credenciales y devuelve un `access` token y un `refresh` token.                                                       |
| **3. Acceso a Recursos**   | El cliente adjunta el `access` token en el header `Authorization: Bearer <access_token>` para acceder a recursos protegidos.                 |
| **4. Refresco de Token**   | Cuando el `access` token expira, el cliente envía el `refresh` token al endpoint `/api/token/refresh/` para obtener un nuevo `access` token. |

> [!TIP] Tokens de Acceso y Refresco 💡🔄  
> 
> 💡 Pro Tip: El access token debe tener un tiempo de vida corto por seguridad, mientras que el refresh token puede tener un tiempo de vida más largo para evitar que el usuario tenga que loguearse constantemente.

## Consideraciones de Seguridad 🔐🛑

- **HTTPS/SSL:** Siempre utiliza HTTPS para todas las comunicaciones para evitar la intercepción de tokens.
- **Almacenamiento de Tokens:** Almacena los tokens de forma segura en el cliente (ej., `localStorage` o `cookies` con `HttpOnly` y `Secure` flags).
- **Revocación de Tokens:** Implementa mecanismos para revocar tokens si es necesario (ej., al cambiar una contraseña o cerrar sesión).
- **Payload Limitado:** No incluyas información sensible en el payload del token, ya que no está encriptado, solo firmado.

> [!INFO] ¿Cómo revocar un token?  
> 
> ℹ️ djangorestframework-simplejwt permite la rotación de tokens de refresco y la inclusión en la lista negra de tokens, lo que es una forma de "revocación" efectiva. También puedes implementar una lista negra propia en la base de datos para tokens de acceso.