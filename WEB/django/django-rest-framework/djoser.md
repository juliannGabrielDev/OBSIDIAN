---
aliases:
  - "Djoser: Autenticación 🔑 y Gestión de Usuarios 👤✨"
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
# Djoser: Autenticación 🔑 y Gestión de Usuarios 👤 en Django REST Framework ✨
Djoser es una librería que facilita la creación de _endpoints_ 🌐 de autenticación 🔒 y gestión de usuarios 👥 para Django REST Framework (DRF). Es súper útil para construir APIs RESTful 🚀 de manera rápida y eficiente. ¡Di adiós a escribir todo desde cero! 🎉
## ¿Por Qué Usar Djoser? 🤔
Djoser nos ahorra un montón de tiempo ⏰ y esfuerzo. Proporciona _endpoints_ listos para usar 🛠️ para funciones comunes como registro 📝, inicio de sesión 🚪, restablecimiento de contraseña 🔑 y más. ¡Es como tener un kit de herramientas 🧰 de autenticación prefabricado!

> [!TIP] ¡Ahorra Tiempo! ⏱️
> 
> 💡 Pro Tip: Djoser se encarga de la lógica compleja 🧠 de autenticación, permitiéndote enfocarte en las características únicas 🌟 de tu aplicación.
## Características Clave 🌟 de Djoser
Aquí te presento algunas de las funcionalidades más importantes que ofrece Djoser:
- **Registro de Usuario** 📝: Crea nuevas cuentas de usuario de forma sencilla.
- **Inicio de Sesión** 🚪: Permite a los usuarios autenticarse y obtener tokens 🔑.
- **Restablecimiento de Contraseña** 🔐: Flujo seguro para recuperar contraseñas olvidadas.
- **Activación de Cuenta** ✅: Verifica la dirección de correo electrónico del usuario.
- **Cambio de Contraseña** 🔄: Permite a los usuarios cambiar su contraseña actual.
- **Gestión de Usuarios** 👤: _Endpoints_ para ver y actualizar perfiles de usuario.
- **Soporte para Diferentes Tipos de Autenticación** 🛡️: Funciona con Token, JWT, y Session.
## Configuración Básica 🛠️ de Djoser
Para empezar a usar Djoser, sigue estos sencillos pasos:
1. **Instalación** ⬇️:
    ```shell
    pip install djoser
    pip install djangorestframework-simplejwt # O la autenticación que prefieras
    ```
2. **Agregar a `INSTALLED_APPS`** ➕ en `settings.py`:
    ```python
    INSTALLED_APPS = [
        # ... otras apps
        'rest_framework',
        'djoser',
        'rest_framework_simplejwt', # Si usas JWT
    ]
    ```
    
3. **Configurar `REST_FRAMEWORK`** ⚙️ en `settings.py`:
    ```python
    REST_FRAMEWORK = {
        'DEFAULT_AUTHENTICATION_CLASSES': (
            'rest_framework_simplejwt.authentication.JWTAuthentication',
            'rest_framework.authentication.SessionAuthentication', # Para la API de Django Admin
        ),
        'DEFAULT_PERMISSION_CLASSES': (
            'rest_framework.permissions.IsAuthenticated',
        ),
    }
    ```
4. **Incluir URLs de Djoser** 🔗 en tu `urls.py` principal:
    ```python
    from django.contrib import admin
    from django.urls import path, include
    
    urlpatterns = [
        path('admin/', admin.site.urls),
        path('auth/', include('djoser.urls')),
        path('auth/', include('djoser.urls.jwt')), # Si usas JWT
        # path('auth/', include('djoser.urls.authtoken')), # Si usas Token
    ]
    ```
5. **Ejecutar Migraciones** 🏗️:
    ```shell
    python manage.py migrate
    ```
> [!INFO] ¡Opciones de Autenticación! 📚
> 
> ℹ️ Djoser es flexible y se integra con varios métodos de autenticación. JWT (JSON Web Tokens) es una opción popular para APIs sin estado.
## Endpoints ✨ Comunes de Djoser
Aquí tienes una tabla 📋 con algunos de los _endpoints_ más utilizados que Djoser genera automáticamente:

| **Método HTTP 🌐**      | **Endpoint 🔗**                  | **Descripción 📝**                                          |
| ----------------------- | -------------------------------- | ----------------------------------------------------------- |
| `POST`                  | `/users/`                        | Crea un nuevo usuario (registro) ✨                          |
| `POST`                  | `/jwt/create/`                   | Obtiene tokens JWT (inicio de sesión) 🔑                    |
| `POST`                  | `/jwt/refresh/`                  | Refresca un token JWT expirado 🔄                           |
| `POST`                  | `/users/set_password/`           | Cambia la contraseña de un usuario 🔐                       |
| `POST`                  | `/users/reset_password/`         | Solicita un restablecimiento de contraseña 📧               |
| `POST`                  | `/users/reset_password_confirm/` | Confirma el restablecimiento de contraseña ✅                |
| `POST`                  | `/users/activation/`             | Activa la cuenta de usuario 💡                              |
| `GET` / `PUT` / `PATCH` | `/users/me/`                     | Obtiene o actualiza los detalles del usuario autenticado 👤 |

> [!WARNING] ¡Seguridad! 🚨
> 
> ⚠️ ¡Importante! Siempre asegúrate de usar HTTPS 🔒 en producción para proteger las credenciales de tus usuarios.
## Personalización de Djoser 🎨
Djoser es altamente personalizable 🎉. Puedes adaptar su comportamiento y la salida de sus _endpoints_:
- **Modelos de Usuario Personalizados** 🧑‍💻: Si usas un `AbstractUser` o `AbstractBaseUser`, Djoser se integra sin problemas.
- **Serializadores Personalizados** 📝: Extiende los serializadores de Djoser para añadir o modificar campos.
- **Vistas Personalizadas** ⚙️: Sobrescribe las vistas de Djoser para una lógica más compleja.
- **URLs Personalizadas** 🔗: Modifica las rutas de los _endpoints_.
- **Plantillas de Correo Electrónico** 📧: Personaliza los correos de activación y restablecimiento de contraseña.

> [!DANGER] ¡Cuidado con las Sobreescrituras! 🛑
> 
> 🛑 Danger: Al personalizar, ten cuidado de no romper la funcionalidad principal 🐞. ¡Prueba exhaustivamente! 🧪