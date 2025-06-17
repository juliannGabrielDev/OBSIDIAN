---
aliases:
  - Manejo de Errores en Django Views 🐞
tags:
  - django
  - views
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
---
# Manejo de Errores en Django Views 🐞
- [[#1. ¿Por qué es importante manejar errores?|1. ¿Por qué es importante manejar errores?]]
- [[#2. Tipos comunes de errores en las Vistas|2. Tipos comunes de errores en las Vistas]]
- [[#3. Mecanismos básicos de manejo de errores|3. Mecanismos básicos de manejo de errores]]
	- [[#3. Mecanismos básicos de manejo de errores#a) Bloques `try-except`|a) Bloques `try-except`]]
	- [[#3. Mecanismos básicos de manejo de errores#b) Atajos de Django para errores comunes|b) Atajos de Django para errores comunes]]
- [[#4. Vistas de error personalizadas|4. Vistas de error personalizadas]]
- [[#5. Levantar excepciones HTTP directamente|5. Levantar excepciones HTTP directamente]]
- [[#6. Logging de errores 📝|6. Logging de errores 📝]]
- [[#Mini Repaso Final 🧑‍🏫|Mini Repaso Final 🧑‍🏫]]
## 1. ¿Por qué es importante manejar errores?
Imagina que estás usando una app y de repente ¡PUM! 💥 Algo sale mal y te aparece una página fea con un montón de texto técnico que no entiendes. ¡Qué frustrante! Como desarrolladores, queremos evitar eso.
- **Mejora la experiencia del usuario (UX)**: Mostrar páginas de error personalizadas y amigables es mucho mejor.
- **Facilita la depuración**: Un buen manejo de errores nos da pistas sobre qué salió mal.
- **Mantiene la aplicación estable**: Evita que la aplicación se caiga por completo ante una situación inesperada.
## 2. Tipos comunes de errores en las Vistas
En el día a día con Django, te puedes encontrar con varios tipos de errores cuando procesas una petición en una vista:
- **`Http404`**: El más famoso. Ocurre cuando un recurso solicitado no se encuentra. Por ejemplo, si alguien intenta acceder a `tusitio.com/pagina-que-no-existe`.
- **`PermissionDenied` (Error 403)**: Cuando un usuario intenta acceder a un recurso para el cual no tiene los permisos necesarios.
- **`SuspiciousOperation` (Error 400)**: Indica que una acción del usuario fue sospechosa, como la manipulación de datos de un formulario.
- **Errores genéricos del servidor (Error 500)**: Estos son los peores porque pueden ser causados por muchas cosas:
    - Errores de lógica en tu código Python.
    - Problemas al conectar con la base de datos.
    - Errores al importar módulos.
    - ¡Y un largo etcétera! 😅
## 3. Mecanismos básicos de manejo de errores
Django nos ofrece varias herramientas para lidiar con estos problemas.
### a) Bloques `try-except`
Este es el pan de cada día en Python y, por supuesto, en Django. Puedes envolver partes de tu código que podrían fallar en un bloque `try` y luego capturar excepciones específicas en bloques `except`.
📝 **Ejemplo:**
```python
from django.http import HttpResponse, Http404
from .models import MiModelo

def mi_vista_detalle(request, id_objeto):
    try:
        objeto = MiModelo.objects.get(pk=id_objeto)
        # Hacemos algo con el objeto
        return HttpResponse(f"Mostrando el objeto: {objeto.nombre}")
    except MiModelo.DoesNotExist:
        raise Http404("¡Ups! El objeto que buscas no existe por aquí. 🤷‍♀️")
    except Exception as e:
        # Para cualquier otro error inesperado, podríamos loggearlo
        # y mostrar un error genérico o redirigir.
        print(f"Ocurrió un error inesperado: {e}") # Mejor usar logging en producción
        return HttpResponse("Algo salió muy mal en el servidor. ¡Estamos en ello! 🛠️", status=500)
```
- En el ejemplo, si `MiModelo.objects.get(pk=id_objeto)` no encuentra el objeto, se lanza una excepción `MiModelo.DoesNotExist`.
- Capturamos esa excepción específica y relanzamos una `Http404`. Django sabe qué hacer con `Http404` (mostrar una página de "No encontrado").
- El `except Exception as e` es una red de seguridad para otros errores no anticipados.
### b) Atajos de Django para errores comunes

Django tiene funciones de atajo (shortcuts) que simplifican el manejo de errores comunes, especialmente el `Http404`.
- **`get_object_or_404()`**: Este es súper útil. Intenta obtener un objeto y si no lo encuentra, automáticamente levanta un `Http404`.
    📝 **Ejemplo simplificado con `get_object_or_404`**:
    ```python
    from django.shortcuts import get_object_or_404, render
    from .models import MiModelo
    
    def mi_vista_detalle_mejorada(request, id_objeto):
        objeto = get_object_or_404(MiModelo, pk=id_objeto)
        # Si el objeto no existe, get_object_or_404 lanza Http404 automáticamente.
        return render(request, 'detalle_objeto.html', {'objeto': objeto})
    ```
    ¡Mucho más limpio! ✨
- **`get_list_or_404()`**: Similar al anterior, pero para obtener una lista de objetos. Si la lista está vacía, levanta `Http404`.
## 4. Vistas de error personalizadas
Django viene con vistas de error por defecto para los errores 400, 403, 404 y 500. Pero, ¡son un poco sosas! Podemos (¡y deberíamos!) personalizarlas para que se integren con el diseño de nuestro sitio.
Para personalizar estas vistas:
1. **Crea tus propias plantillas HTML** para cada error en el directorio `templates` de alguna de tus apps, o en el directorio `templates` raíz de tu proyecto.
    - `400.html`
    - `403.html`
    - `404.html`
    - `500.html`
2. **Define las vistas manejadoras (handlers)** en tu archivo `urls.py` raíz (el que está al mismo nivel que `settings.py`).
    📝 **Ejemplo en `proyecto/urls.py`**:
    ```python
    from django.urls import path, include
    from django.conf.urls import handler400, handler403, handler404, handler500
    from mi_app import views as mi_app_views # Suponiendo que tus vistas personalizadas están en mi_app/views.py
    
    urlpatterns = [
        # ... tus otras urls
        path('admin/', admin.site.urls),
        path('', include('mi_app.urls')),
    ]
    
    handler400 = 'mi_app.views.mi_vista_bad_request' # str: 'nombre_app.views.nombre_funcion_vista'
    handler403 = 'mi_app.views.mi_vista_permission_denied'
    handler404 = 'mi_app.views.mi_vista_page_not_found'
    handler500 = 'mi_app.views.mi_vista_server_error'
    ```
    Y en `mi_app/views.py` tendrías funciones como:

```python
from django.shortcuts import render
    
def mi_vista_page_not_found(request, exception):
    return render(request, '404.html', status=404)
    
def mi_vista_server_error(request):
    return render(request, '500.html', status=500)
    
def mi_vista_permission_denied(request, exception):
    return render(request, '403.html', status=403)
    
def mi_vista_bad_request(request, exception):
    return render(request, '400.html', status=400)
```
**Importante**: Para que Django use tus vistas de error personalizadas en producción, debes tener `DEBUG = False` en tu `settings.py`. Si `DEBUG = True`, Django mostrará su propia página de depuración detallada para los errores 500.
## 5. Levantar excepciones HTTP directamente
A veces, dentro de la lógica de tu vista, detectas una condición de error y quieres detener el procesamiento y mostrar una página de error HTTP específica.
- **`raise Http404("Mensaje opcional")`**: Para indicar que un recurso no fue encontrado.
- **`from django.core.exceptions import PermissionDenied`** y luego `raise PermissionDenied("No tienes permiso")`: Para errores de permisos.
- **`from django.core.exceptions import SuspiciousOperation`** y luego `raise SuspiciousOperation("Algo raro pasó")`: Para operaciones sospechosas.
Django se encarga de convertir estas excepciones en las respuestas HTTP adecuadas (404, 403, 400).
## 6. Logging de errores 📝
Aunque mostrar una página de error bonita al usuario es importante, ¡tú como desarrollador necesitas saber qué pasó! Django tiene un sistema de **logging** integrado.
- Cuando `DEBUG = False`, Django automáticamente envía un correo a los `ADMINS` definidos en `settings.py` cada vez que ocurre un error 500.
- Puedes configurar el logging de manera más avanzada para guardar errores en archivos, enviarlos a servicios externos, etc. Esto es un tema más profundo, pero es bueno saber que existe.
## Mini Repaso Final 🧑‍🏫
¡Ok, repasemos lo clave!
- El manejo de errores es **crucial** para la UX y la estabilidad.
- Usa bloques **`try-except`** para capturar excepciones específicas en tus vistas.
- Aprovecha los atajos de Django como **`get_object_or_404()`**.
- **Personaliza tus páginas de error** (400, 403, 404, 500) para una mejor experiencia de usuario, creando plantillas HTML y definiendo los `handler` en `urls.py`.
- Recuerda que `DEBUG = False` es necesario para ver tus errores personalizados en producción.
- Puedes **levantar excepciones HTTP** (`Http404`, `PermissionDenied`) directamente cuando sea necesario.
- No olvides el **logging** para rastrear los errores del lado del servidor.