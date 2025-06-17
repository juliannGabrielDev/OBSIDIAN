---
aliases:
  - Entendiendo los Permisos en Django 🔑
tags:
  - django
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
Fecha: 2025-06-02
---
# Entendiendo los Permisos en Django 🔑
Imagina que vas a un restaurante 🍽️. Tú, como cliente, puedes sentarte en la mesa y comer, ¿verdad? Pero no puedes entrar a la cocina y ponerte a cocinar. En cambio, un chef o un mesero (el personal) sí pueden entrar a la cocina. ¡Los permisos en las aplicaciones web funcionan de forma muy parecida! Controlan qué usuarios pueden realizar qué acciones.
Django tiene un sistema incorporado para manejar esto, que se encarga tanto de la **autenticación** (quién eres) como de la **autorización** (qué puedes hacer).
## 1. Tipos de Usuarios en Django 🧑‍🤝‍🧑
Antes de hablar de permisos, es clave entender que en Django un usuario puede ser de tres tipos principales. Piensa en ellos como "niveles de acceso":
- **Superusuario (`is_superuser=True`)**:
    - Es el jefe supremo, el administrador total del sistema. 👑
    - Tiene **todos los permisos** habidos y por haber.
    - Puede añadir, cambiar, eliminar otros usuarios y manejar todos los datos del proyecto.
- **Usuario de Personal (`is_staff=True`)**:
    - Este usuario puede **acceder al panel de administración** de Django.
    - **¡Ojo!** Que pueda entrar al admin no significa que pueda hacer de todo allí. Los permisos para crear, leer, actualizar o borrar datos (CRUD) se le deben dar **explícitamente**.
    - Un superusuario es, por defecto, también un usuario de personal.
- **Usuario Regular (`is_staff=False`, `is_superuser=False`)**:
    - Es el usuario común y corriente. Por defecto, todos los nuevos usuarios son de este tipo.
    - **No puede** acceder al panel de administración.
Además, los usuarios tienen una banderita `is_active`. Por defecto es `True`. Se puede poner en `False` si la cuenta falla la autenticación repetidamente o si se ha baneado al usuario.
## 2. Creación de Usuarios (Un Vistazo Rápido) ➕
Aunque podemos crear usuarios desde el panel de admin, también se puede hacer desde la "shell" de Django:
- **`create_user()`**: Función para crear un objeto de usuario normal.
- Para hacer a un usuario parte del personal: `usuario.is_staff = True` y luego `usuario.save()`.
- **`createsuperuser`**: Este es un **comando** que ejecutas en tu terminal (`python manage.py createsuperuser`). Te pedirá nombre, email (opcional) y contraseña para el nuevo superusuario.
Una vez creado, ¡el nuevo usuario aparecerá en la lista del panel de admin!
## 3. Permisos a Nivel de Modelo (¡Automagia!) ✨
Cuando creas un modelo en tu aplicación Django (en `models.py`), ¡Django es tan genial que **automáticamente crea cuatro permisos básicos** para ese modelo! Estos son:
- `add` (añadir/crear objetos de ese modelo)
- `change` (modificar/actualizar objetos)
- `delete` (borrar objetos)
- `view` (ver/leer objetos) - Este es más reciente, desde Django 2.1.
Estos permisos siguen un patrón de nomenclatura muy lógico:
`nombre_de_la_app.accion_nombre_del_modelo` (todo en minúsculas).
**Ejemplo:**
Si tienes una app llamada `myapp` y dentro un modelo llamado `MiModelo` (class `MiModelo(models.Model):` ...), los permisos automáticos serán:
- `myapp.add_mimodelo`
- `myapp.change_mimodelo`
- `myapp.delete_mimodelo`
- `myapp.view_mimodelo`
¡Así de fácil! No tienes que definirlos manualmente al inicio.
## 4. ¿Cómo Chequeo si un Usuario Tiene un Permiso? 🤔
En tu código Python (por ejemplo, en una vista), puedes verificar si un usuario tiene un permiso específico usando el método `has_perm()` del objeto usuario. Devuelve `True` o `False`.
```python
# En una vista (views.py)
from django.core.exceptions import PermissionDenied

def mi_vista_especial(request):
    if not request.user.has_perm('myapp.change_mimodelo'):
        # Si el usuario NO tiene el permiso para cambiar MiModelo...
        raise PermissionDenied # ¡Le negamos el acceso!
    
    # Si llegamos aquí, el usuario SÍ tiene el permiso
    # ... hacer cosas que requieren ese permiso ...
    return HttpResponse("¡Tienes permiso para cambiar MiModelo!")
```
Así puedes controlar quién hace qué en partes específicas de tu lógica de negocio.
## 5. Grupos: La Forma Inteligente de Asignar Permisos 👨‍👩‍👧‍👦
Asignar permisos uno por uno a cada usuario puede ser una pesadilla, ¡especialmente si tienes muchos usuarios! 😵‍💫 Imagina tener 100 cocineros y tener que darles los mismos 5 permisos a cada uno...
¡Django tiene una solución elegante!: los **Grupos**.
- Un **Grupo** en Django es básicamente una **etiqueta a la que le asocias una lista de permisos**.
- Puedes crear un grupo (ej. "Editores de Blog", "Moderadores de Comentarios") y asignarle todos los permisos que esos roles necesitan.
- Luego, en lugar de dar permisos individuales, simplemente **añades usuarios a esos grupos**.
- Un usuario puede pertenecer a **varios grupos** si es necesario. ¡Heredará los permisos de todos ellos!
- Volviendo al ejemplo del restaurante: podrías tener un grupo "Personal de Cocina" (con permisos para acceder a recetas, inventario de ingredientes) y otro grupo "Meseros" (con permisos para tomar pedidos, ver mesas).
**Importante**: Aunque un usuario pertenezca a un grupo, ¡aún puedes añadirle o quitarle permisos individuales si es necesario! Los permisos de grupo y los individuales se suman.
Crear y asignar usuarios a grupos se puede hacer fácilmente desde el panel de administración de Django.
## 6. Aplicando Permisos en las Vistas (¡Decoradores al Rescate!) 🛡️
Para proteger vistas enteras de forma rápida, Django ofrece decoradores. Uno muy útil es `@permission_required`.
```python
# views.py
from django.contrib.auth.decorators import permission_required

@permission_required('myapp.add_mimodelo', login_url='/url_a_donde_redirigir_si_no_tiene_permiso/')
def vista_para_anadir_mimodelo(request):
    # Solo usuarios con el permiso 'myapp.add_mimodelo' pueden acceder aquí.
    # Si no tienen el permiso (y no están logueados), se les redirige a login_url.
    # Si están logueados pero no tienen el permiso, verán un error 403 (Forbidden).
    # ... lógica para añadir un MiModelo ...
    pass
```
## 7. Mini Repaso Final 💡
- Django maneja **autenticación** (quién eres) y **autorización** (qué puedes hacer).
- Hay tres tipos principales de usuarios: **Superusuario**, **Staff** (`is_staff`) y **Regular**.
- Django crea **automáticamente** permisos de `add`, `change`, `delete`, `view` para cada modelo.
- Usa `request.user.has_perm('app.accion_modelo')` para verificar permisos en tu código.
- Los **Grupos** son la forma más eficiente de asignar conjuntos de permisos a múltiples usuarios.
- El decorador `@permission_required` es útil para proteger vistas enteras.