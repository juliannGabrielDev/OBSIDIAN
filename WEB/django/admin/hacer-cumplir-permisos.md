---
aliases:
  - Haciendo Cumplir los Permisos 🚦
tags:
  - django
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
Fecha: 2025-06-02
---
# Haciendo Cumplir los Permisos en Django: ¡Que Nadie Entre Sin Su Llave! 🚦

Ya sabemos que el Admin de Django es genial para _asignar_ permisos. Pero, ¿qué pasa fuera del Admin, en el resto de nuestra app? Ahí es donde nosotros, los desarrolladores, tenemos que asegurarnos de que esos permisos se _cumplan_. ¡Django nos da herramientas para hacerlo en diferentes niveles!
## 1. Permisos Personalizados a Nivel de Modelo 🧩
Además de los permisos automáticos (`add`, `change`, `delete`, `view`), podemos crear nuestros propios permisos especiales para un modelo. Esto se hace en la clase `Meta` del modelo.
**Ejemplo: Un modelo `Product` con un permiso para cambiar la categoría.**
```python
# models.py
from django.db import models

class Product(models.Model):
    # ProductID = models.IntegerField() # Cuidado: campos con mayúscula no es convención.
    # Y si es ID, Django pone uno automático (pk) o puedes usar PrimaryKey=True.
    # Asumamos que es un ID de producto externo.
    product_id_externo = models.IntegerField() 
    nombre = models.TextField() # CharField suele ser mejor para nombres
    categoria = models.TextField() # CharField podría ser mejor aquí también

    class Meta:
        permissions = [
            ('can_change_category', 'Puede cambiar la categoría del producto'),
            # Aquí puedes añadir más permisos personalizados
            # ('can_approve_product', 'Puede aprobar productos'),
        ]

    def __str__(self):
        return self.nombre
```
Este permiso `can_change_category` (que se guardará en la base de datos como `myapp.can_change_category` si la app se llama `myapp`) aparecerá en el Admin de Django para que se lo puedas asignar a usuarios o grupos.
**¡Pero OJO!** ⚠️
- El modelo en sí **no hace cumplir estos permisos automáticamente** fuera del Admin. ¿Por qué? Porque el modelo, cuando lo usas en tu código, no sabe quién es el `request.user` (el usuario que está haciendo la petición).
- La **lógica para hacer cumplir los permisos** (verificar si el usuario tiene `can_change_category`) la tenemos que poner nosotros, generalmente en las **vistas**.
## 2. Haciendo Cumplir Permisos en las Vistas (¡El Control Principal!) 👮
Las vistas son el lugar más común para verificar y hacer cumplir los permisos. ¡Aquí es donde Django sabe quién es `request.user`!
### a. El Objeto `request.user`
- Si el usuario ha iniciado sesión, `request.user` es el objeto `User` correspondiente.
- Si no ha iniciado sesión, `request.user` es una instancia de `AnonymousUser`.
### b. Verificación Manual Básica
Puedes chequear si el usuario es anónimo y negarle el acceso:
```python
# views.py
from django.core.exceptions import PermissionDenied

def mi_vista_privada(request):
    if request.user.is_anonymous: # is_anonymous es un atributo, no un método con ()
        raise PermissionDenied("¡Acceso denegado! Debes iniciar sesión.")
    # ... lógica de la vista para usuarios logueados ...
```
### c. Decorador `@login_required`
Este es el más simple para vistas que solo usuarios logueados pueden ver. Si un usuario no logueado intenta acceder, lo redirige a la página de login (definida en `settings.LOGIN_URL`).
```python
# views.py
from django.http import HttpResponse
from django.contrib.auth.decorators import login_required

@login_required # ¡Así de fácil!
def mi_vista_logueados(request):
    return HttpResponse(f"Hola, {request.user.username}! Esta es una vista para usuarios logueados.")
```
### d. Decorador `@user_passes_test`
Este decorador es más flexible. Le pasas una función (un "test") que recibe un objeto `user` y debe devolver `True` (si el usuario pasa el test y puede acceder) o `False` (si no).
**Ejemplo**: Solo usuarios autenticados Y con el permiso `myapp.can_change_category`.
```python
# views.py
from django.contrib.auth.decorators import user_passes_test

def tiene_permiso_cambiar_categoria(user):
    # user.is_authenticated es un atributo en Django moderno
    if user.is_authenticated and user.has_perm("myapp.can_change_category"):
        return True
    return False

@user_passes_test(tiene_permiso_cambiar_categoria, login_url='/acceso_denegado/') # login_url es opcional
def vista_cambiar_categoria(request):
    # ... Lógica para cambiar la categoría del producto ...
    return HttpResponse("Categoría cambiada (si tenías permiso).")
```
Si `tiene_permiso_cambiar_categoria` devuelve `False`, el usuario es redirigido a `login_url` (si se especifica) o ve un error 403 (Forbidden).
### e. Decorador `@permission_required` (¡El más directo para permisos!)
Este es el más específico para chequear un permiso concreto.
```python
# views.py
from django.contrib.auth.decorators import permission_required

@permission_required("myapp.can_change_category", login_url='/sin_permiso/')
def vista_solo_con_permiso_categoria(request):
    # ... Lógica de la vista ...
    return HttpResponse("Tienes el permiso 'can_change_category'.")
```
Si el usuario no tiene `myapp.can_change_category`, es redirigido a `login_url` o ve un error 403.
### f. Vistas Basadas en Clases (CBV) y `PermissionRequiredMixin`
Para las CBV, Django nos da "Mixins". Un Mixin es como una extensión que le añade funcionalidad a tu clase. `PermissionRequiredMixin` es el que usamos para permisos.
```python
# views.py
from django.contrib.auth.mixins import PermissionRequiredMixin
from django.views.generic import ListView
from .models import Product # Asumiendo que Product está en el mismo models.py

class ProductListView(PermissionRequiredMixin, ListView):
    model = Product
    template_name = "myapp/product_list.html" # Buena práctica poner templates en carpetas por app
    context_object_name = "productos"
    
    # Aquí especificamos el permiso necesario
    permission_required = "myapp.view_product" 
    # También puedes poner una tupla/lista si necesitas MÚLTIPLES permisos:
    # permission_required = ("myapp.view_product", "myapp.change_product")

    # Opcional: A dónde redirigir si no tiene permiso (si no, error 403)
    # login_url = '/acceso_denegado_cbv/' 
    # raise_exception = True # Si quieres que lance PermissionDenied en vez de redirigir
```
## 3. Haciendo Cumplir Permisos en las Plantillas (Templates) 🖼️
En las plantillas, podemos mostrar u ocultar contenido dependiendo de si el usuario está autenticado o tiene ciertos permisos.
El contexto de la plantilla (gracias a los `context_processors` de Django) suele tener disponibles las variables:
- `{{ user }}`: El objeto usuario actual.
- `{{ perms }}`: Un objeto que permite chequear permisos.
```html
<body>
    {% if user.is_authenticated %}
        <p>Bienvenido, {{ user.username }}!</p>
        
        {% if perms.myapp.can_change_category %}
            <button>Editar Categoría del Producto</button>
        {% else %}
            <p>No tienes permiso para editar categorías.</p>
        {% endif %}

        {% if perms.myapp.add_product %}
            <a href="{% url 'url_para_anadir_producto' %}">Añadir Nuevo Producto</a>
        {% endif %}

    {% else %}
        <p>Por favor, <a href="{% url 'login' %}">inicia sesión</a> para ver más.</p>
    {% endif %}
</body>
```
**Importante**: Esto solo controla **lo que se muestra**, no la lógica de acceso real. Si un usuario listo se sabe la URL para "Editar Categoría", podría intentar acceder. ¡Por eso la validación en la vista es CRUCIAL! La plantilla es solo para la interfaz.
## 4. Haciendo Cumplir Permisos en los Patrones de URL (`urls.py`) 🌐
También puedes aplicar decoradores de permisos directamente en tus `urls.py`. Esto es útil si quieres proteger un conjunto de URLs o una vista sin modificar su código directamente.
```python
# urls.py
from django.urls import path
from django.contrib.auth.decorators import login_required, permission_required
from . import views # Asumiendo que tus vistas están en views.py

urlpatterns = [
    path('area_usuarios/', login_required(views.mi_vista_logueados), name='area_usuarios'),
    
    path('cambiar_categoria_producto/', 
         permission_required('myapp.can_change_category', login_url='/sin_permiso/')(views.vista_cambiar_categoria), 
         name='cambiar_categoria'),
    
    # Para vistas basadas en clases, a veces se usa .as_view()
    # path('lista_productos_segura/', 
    #      permission_required('myapp.view_product')(views.ProductListView.as_view()), 
    #      name='lista_productos_cbv'),
    # Sin embargo, para CBVs es más común usar el Mixin directamente en la clase.
]
```
## 5. Mini Repaso Final 💡
- Los **permisos personalizados** se definen en la `class Meta` de un modelo, pero los modelos no los hacen cumplir por sí solos fuera del Admin.
- La **aplicación principal de permisos** ocurre en las **Vistas**:
    - Verificación manual con `request.user.is_authenticated` y `request.user.has_perm()`.
    - Decoradores para vistas basadas en funciones: `@login_required`, `@user_passes_test`, `@permission_required`.
    - Mixins para vistas basadas en clases: `LoginRequiredMixin`, `PermissionRequiredMixin`.
- En las **Plantillas**, usamos `{{ user }}` y `{{ perms }}` para mostrar/ocultar contenido, ¡pero esto es solo visual!
- En los **Patrones de URL**, podemos envolver vistas con decoradores de permisos.