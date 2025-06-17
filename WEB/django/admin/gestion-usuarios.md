---
aliases:
  - Tuneando el Admin 🛠️
tags:
  - django
  - admin
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
Fecha: 2025-06-01
---
# Tuneando el Admin de Django: Usuarios y Modelos 🛠️

El panel de administración de Django (`django.contrib.admin`) es una de las cosas más geniales que tiene. ¡Nos da una interfaz lista para usar para manejar los datos de nuestra aplicación! Viene instalado por defecto, así que lo encuentras en `INSTALLED_APPS` en tu `settings.py`.
- [[#1. Gestión de Usuarios Básica en el Admin 🧑‍💻|1. Gestión de Usuarios Básica en el Admin 🧑‍💻]]
- [[#2. El "Problemilla" con el Admin por Defecto 😟|2. El "Problemilla" con el Admin por Defecto 😟]]
- [[#3. Personalizando el Admin de Usuarios (`UserAdmin`) 🔧|3. Personalizando el Admin de Usuarios (`UserAdmin`) 🔧]]
	- [[#3. Personalizando el Admin de Usuarios (`UserAdmin`) 🔧#a. Desregistrar el `UserAdmin` por Defecto|a. Desregistrar el `UserAdmin` por Defecto]]
	- [[#3. Personalizando el Admin de Usuarios (`UserAdmin`) 🔧#b. Registrar Nuestro Propio `UserAdmin` Personalizado|b. Registrar Nuestro Propio `UserAdmin` Personalizado]]
	- [[#3. Personalizando el Admin de Usuarios (`UserAdmin`) 🔧#c. Haciendo Campos de Solo Lectura (`readonly_fields`)|c. Haciendo Campos de Solo Lectura (`readonly_fields`)]]
	- [[#3. Personalizando el Admin de Usuarios (`UserAdmin`) 🔧#d. Restringir Edición de Campos por Rol (Ej. `username`)|d. Restringir Edición de Campos por Rol (Ej. `username`)]]
- [[#4. Personalizando la Vista de Otros Modelos 🖼️|4. Personalizando la Vista de Otros Modelos 🖼️]]
	- [[#4. Personalizando la Vista de Otros Modelos 🖼️#a. Ejemplo: Un Modelo `Person`|a. Ejemplo: Un Modelo `Person`]]
	- [[#4. Personalizando la Vista de Otros Modelos 🖼️#b. Registrar el Modelo `Person` en el Admin|b. Registrar el Modelo `Person` en el Admin]]
	- [[#4. Personalizando la Vista de Otros Modelos 🖼️#c. Personalizar la Lista con `ModelAdmin`|c. Personalizar la Lista con `ModelAdmin`]]
- [[#5. Mini Repaso Final 💡|5. Mini Repaso Final 💡]]
## 1. Gestión de Usuarios Básica en el Admin 🧑‍💻
- **Superusuario**: Es el que tiene todo el poder. Puede añadir, modificar y borrar usuarios y grupos. Cuando entras como superusuario, ves todas las opciones.
- **Añadir Usuarios**: Desde el admin, puedes crear nuevos usuarios fácilmente.
- **Permisos**: Un usuario sin permisos no puede hacer mucho. Los permisos se pueden dar:
    - **Individualmente**: A cada usuario directamente.
    - **Por Grupos**: Creas un grupo, le das permisos, y luego metes usuarios a ese grupo. ¡Así todos los del grupo tienen los mismos permisos! Mucho más ordenado. 👍
- **La banderita `is_staff`**: Si un usuario tiene `is_staff` en `True`, puede entrar al panel de administración. ¡Ojo! Solo entrar, no necesariamente hacer de todo.
## 2. El "Problemilla" con el Admin por Defecto 😟
El admin de Django es potente, ¡pero hay que tener cuidado! Si no asignamos los permisos con cabeza, podemos tener riesgos de seguridad.

- **Un detalle importante**: Por defecto, un usuario que es `staff` (puede entrar al admin) podría llegar a editar sus **propios permisos** e incluso ¡darse permisos de superusuario! 😱 Esto no es lo ideal.
- **Ejemplo**: Imagina un superusuario llamado `Admin` y un usuario `test123` que es `staff`. `test123` podría, en teoría, volverse superpoderoso por sí mismo.
**¿Qué hacemos para evitar esto?** ¡Personalizar el admin!
## 3. Personalizando el Admin de Usuarios (`UserAdmin`) 🔧
Para tener más control sobre cómo se manejan los usuarios en el admin, vamos a "meterle mano" al `UserAdmin`.
### a. Desregistrar el `UserAdmin` por Defecto
Primero, le decimos a Django que no use la configuración estándar para el modelo User.
Esto se hace en el archivo admin.py de tu aplicación (si no existe, ¡créalo!).
```python
# admin.py de tu aplicación

from django.contrib import admin
from django.contrib.auth.models import User # El modelo User que usa Django

# 1. Quitamos el admin por defecto para el modelo User
admin.site.unregister(User)
```
### b. Registrar Nuestro Propio `UserAdmin` Personalizado
Ahora, creamos nuestra propia clase que herede de `UserAdmin` y la registramos. Usamos el decorador `@admin.register(User)` que es una forma moderna y cool de hacerlo.
```python
# admin.py (continuación)

from django.contrib.auth.admin import UserAdmin # La clase base que vamos a extender

@admin.register(User) # 2. Registramos nuestra nueva clase para el modelo User
class MiUserAdminPersonalizado(UserAdmin):
    pass # Por ahora no hace nada nuevo, pero ya tenemos el control
```
Si ahora entras al admin como superusuario, no verás cambios... ¡aún!
### c. Haciendo Campos de Solo Lectura (`readonly_fields`)
Hay campos que, una vez creados, no queremos que nadie (ni siquiera un superusuario despistado) modifique. Por ejemplo, la fecha en que un usuario se unió (`date_joined`).
La clase `UserAdmin` tiene una propiedad `readonly_fields`. Es una lista de los nombres de los campos que queremos proteger.
```python
# admin.py (modificamos nuestra clase)

# from django.contrib.auth.admin import UserAdmin # Ya debería estar importado arriba
# @admin.register(User) # Ya debería estar decorando la clase

class MiUserAdminPersonalizado(UserAdmin):
    readonly_fields = [
        'date_joined', # El campo 'date_joined' ahora será de solo lectura
    ]
    # pass # ya no es necesario si tenemos contenido
```
Ahora, en el formulario de edición de un usuario en el admin, el campo `date_joined` aparecerá, pero ¡no se podrá editar! Estará "deshabilitado". 😎
### d. Restringir Edición de Campos por Rol (Ej. `username`)
Imagina que no queremos que un usuario `staff` (que no es superusuario) pueda cambiar el `username` de otros usuarios. Cambiar un `username` puede causar problemas (¡el otro usuario podría no poder loguearse!). Esto solo debería poder hacerlo un superusuario.
Para esto, necesitamos sobrescribir el método `get_form()` de nuestra clase `MiUserAdminPersonalizado`. Este método es el que decide qué formulario se usa para crear o editar un usuario.
```python
# admin.py (modificamos nuestra clase MiUserAdminPersonalizado)

# from django.contrib.auth.admin import UserAdmin
# @admin.register(User)

class MiUserAdminPersonalizado(UserAdmin):
    readonly_fields = [
        'date_joined',
    ]

    def get_form(self, request, obj=None, **kwargs):
        # Primero, obtenemos el formulario tal como lo haría la clase padre
        form = super().get_form(request, obj, **kwargs)
        
        # Verificamos si el usuario que está haciendo la petición NO es un superusuario
        is_superuser = request.user.is_superuser
        
        if not is_superuser:
            # Si no es superusuario, deshabilitamos el campo 'username'
            # 'base_fields' es donde viven los campos del formulario antes de que se procesen del todo
            if 'username' in form.base_fields: # Buena práctica chequear si existe
                 form.base_fields['username'].disabled = True
        
        # Devolvemos el formulario (modificado o no)
        return form
```
¡Con esto, si un usuario `staff` (como `test123`) intenta editar otro usuario, el campo `username` estará bloqueado! Solo el `Admin` (superuser) podrá cambiarlo. ¡Más seguro! ✨
## 4. Personalizando la Vista de Otros Modelos 🖼️
Las mismas ideas de personalización se pueden aplicar a cualquier modelo que registres en el admin, no solo a los usuarios.
### a. Ejemplo: Un Modelo `Person`
Supongamos que creas una nueva app llamada myapp (recuerda añadirla a `INSTALLED_APPS` en `settings.py`).
Y en `myapp/models.py` tienes:
```python
# myapp/models.py
from django.db import models

class Person(models.Model):
    last_name = models.TextField() # Usar CharField suele ser más apropiado para nombres
    first_name = models.TextField() # Usar CharField suele ser más apropiado para nombres

    # ¡Añadamos esto para que se vea bonito en el admin!
    def __str__(self):
        return f"{self.last_name}, {self.first_name}"
```

**Importante el `__str__(self)`**: Este método le dice a Django cómo "imprimir" un objeto `Person`. Sin él, en el admin verías cosas como "Person object (1)", "Person object (2)", ¡nada útil! Con `__str__`, verás "Perez, Juan".
### b. Registrar el Modelo `Person` en el Admin
En `myapp/admin.py` (si no existe, créalo):
```python
# myapp/admin.py
from django.contrib import admin
from .models import Person # Importamos nuestro modelo

# Registro básico
# admin.site.register(Person) # Comentamos esto para usar la forma personalizada
```
### c. Personalizar la Lista con `ModelAdmin`
Para más control sobre cómo se muestra `Person` en la lista del admin, creamos una clase que herede de `admin.ModelAdmin`.
```python
# myapp/admin.py (continuación)

@admin.register(Person) # Usamos el decorador, ¡es más elegante!
class PersonAdmin(admin.ModelAdmin):
    # Para mostrar campos como columnas en la lista de personas
    list_display = ("last_name", "first_name") 
    
    # Para añadir una barra de búsqueda que busque en estos campos
    # Ojo: __startswith es un "lookup" de Django para "empieza con"
    search_fields = ("first_name__startswith", "last_name__startswith") 
```
Ahora, en el admin, la lista de "Persons" mostrará columnas para `last_name` y `first_name`, y tendrás una barra de búsqueda. ¡Mucho más pro! 🌟
## 5. Mini Repaso Final 💡
- El **admin de Django** es súper potente para gestionar datos y usuarios.
- Por defecto, tiene algunas **configuraciones que podríamos querer cambiar** por seguridad o usabilidad (ej. que un `staff` edite sus permisos o el `username` de otros).
- Podemos **personalizar** cómo se comportan los modelos en el admin:
    - Para el modelo `User`, heredamos de `UserAdmin`.
    - Para otros modelos, heredamos de `ModelAdmin`.
- Pasos clave para personalizar `UserAdmin`:
    1. `admin.site.unregister(User)`
    2. Crear una clase `MiUserAdmin(UserAdmin)` y decorarla con `@admin.register(User)`.
    3. Usar `readonly_fields` para campos no editables.
    4. Sobrescribir `get_form()` para lógica condicional (ej. deshabilitar campos según el rol del usuario).
- Para otros modelos (`PersonAdmin`):
    - Usar `list_display` para configurar las columnas en la vista de lista.
    - Usar `search_fields` para añadir una barra de búsqueda.
    - ¡No olvidar el método `__str__` en tus modelos!