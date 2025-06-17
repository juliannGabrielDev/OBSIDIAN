---
aliases:
  - 🚀 Gestión de Usuarios y Grupos con Djoser ✨🔒
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
# 🚀 Gestión de Usuarios y Grupos con Djoser en Django REST Framework ✨🔒

Este documento explica cómo utilizar `Djoser` para el registro de nuevos usuarios y cómo crear un `endpoint` exclusivo para el superadministrador para gestionar la pertenencia de usuarios a grupos. 👤👥

---

## 📌 Registro de Nuevos Usuarios con Djoser 📝✍️

El paquete `Djoser` simplifica enormemente el proceso de registro de usuarios en Django REST Framework. 🤩

### Endpoint de Registro (`/users/`) 🌐

`Djoser` proporciona un `endpoint` dedicado para la gestión de usuarios, el cual es accesible para el registro sin necesidad de autenticación.

- **Acceso para Superusuarios:** Si inicias sesión como superusuario en el administrador de Django y accedes a este `endpoint` (ej., `/auth/users/`), podrás ver **todos los usuarios** registrados.
- **Acceso con Token:** Otros usuarios que hagan una llamada `HTTP GET` a este `endpoint` con un token válido solo podrán ver su **propio nombre de usuario y correo electrónico**.
- **Registro Público:** Este `endpoint` puede aceptar llamadas `HTTP POST` **sin ningún token** para el registro de nuevos usuarios. Solo se requieren `email`, `username` y `password`.

> [!NOTE] Funcionalidad de Djoser 🚀  
> 
> 🗒️ Djoser facilita el proceso de registro y gestión de usuarios, proporcionando endpoints preconfigurados que evitan la necesidad de escribir código adicional para estas funcionalidades básicas.

### Proceso de Registro con Insomnia 🛠️🚀

Para registrar un nuevo usuario:

1. **Abrir Insomnia:** Crea una nueva solicitud `HTTP POST`.
2. **URL:** Dirige la solicitud al `endpoint` `/api/users/` (o la ruta que hayas configurado para Djoser).
3. **Cuerpo de la Solicitud:**
    - Haz clic en la pestaña "Body".
    - Selecciona "Form URL Encoded".
    - Añade tres campos `POST`: `username`, `email` y `password`.
4. **Enviar:** Haz clic en el botón "Send".

Si todo es correcto, el `endpoint` devolverá una respuesta `200 OK` con el nuevo nombre de usuario y dirección de correo electrónico, confirmando que el registro se realizó correctamente **sin necesidad de modificar tu código**. ✅

## 👑 Endpoint para Superadministradores: Gestión de Grupos 🛡️

Necesitarás un `endpoint` específico para que el superadministrador pueda asignar usuarios a grupos o eliminarlos de ellos. 👮‍♂️

### 1. Creación de la Vista (`views.py`) 👩‍💻

Crea una nueva función en tu archivo `views.py` que manejará la lógica de la asignación de grupos.

```python
# views.py
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework.permissions import IsAdminUser # ¡Importante!
from django.contrib.auth.models import User, Group
from django.shortcuts import get_object_or_404
from rest_framework import status

class Managers(APIView):
    permission_classes = [IsAdminUser] # Solo superadministradores

    def post(self, request):
        username = request.data.get('username')
        if not username:
            return Response({"message": "El nombre de usuario es requerido."}, status=status.HTTP_400_BAD_REQUEST)

        user = get_object_or_404(User, username=username)
        manager_group, created = Group.objects.get_or_create(name='Manager') # O el nombre de tu grupo

        manager_group.user_set.add(user) # Añadir usuario al grupo
        return Response({"message": f"Usuario {username} añadido al grupo Manager."}, status=status.HTTP_200_OK)

    def delete(self, request):
        username = request.data.get('username')
        if not username:
            return Response({"message": "El nombre de usuario es requerido."}, status=status.HTTP_400_BAD_REQUEST)

        user = get_object_or_404(User, username=username)
        manager_group = get_object_or_404(Group, name='Manager') # O el nombre de tu grupo

        manager_group.user_set.remove(user) # Eliminar usuario del grupo
        return Response({"message": f"Usuario {username} eliminado del grupo Manager."}, status=status.HTTP_200_OK)
```

> [!TIP] Permiso IsAdminUser 💡  
> 
> 💡 Pro Tip: La clase IsAdminUser (from rest_framework.permissions import IsAdminUser) es clave para restringir el acceso a esta vista únicamente a usuarios que tengan el flag is_staff en True (superadministradores).

### 2. Mapeo de URL (`urls.py`) 🔗

Añade la ruta para esta nueva vista en el archivo `urls.py` de tu aplicación o proyecto.

```python
# urls.py (ej. de tu app o proyecto principal)
from django.urls import path
from .views import Managers # Asegúrate de importar tu vista

urlpatterns = [
    # ... otras URLs
    path('api/groups/manager/users', Managers.as_view(), name='manager-users'),
]
```

### 3. Prueba del Endpoint 🧪📊

- **Con Token de Usuario Normal:** Realiza una llamada al `endpoint` (`/api/groups/manager/users`) con un token de usuario normal. Deberías recibir un error de permisos. 🚫
- **Con Token de Superadministrador:** Realiza la llamada con un token de superadministrador.
    - **Método `POST`:** Envía un campo `POST` llamado `username` con el nombre de usuario que deseas añadir al grupo "Manager". Deberías ver un mensaje de éxito. ✅
    - **Método `DELETE`:** Envía un campo `POST` llamado `username` con el nombre de usuario que deseas eliminar del grupo "Manager". Deberías ver un mensaje de éxito. ✅

> [!INFO] Manejo de Usuarios y Grupos ℹ️  
> 
> ℹ️ Las clases User y Group de django.contrib.auth.models son fundamentales para interactuar con la base de datos de usuarios y la gestión de la pertenencia a grupos.