---
aliases:
  - Vistas Basadas en Clases (CBVs)
tags:
  - django
  - views
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
---
# Vistas Basadas en Clases (CBVs)
¡Uff, las vistas! Son el corazón de nuestra aplicación web, ¿verdad? Se encargan de tomar una petición web, hacer algo con ella (como buscar datos en la base de datos) y devolver una respuesta. Tradicionalmente, Django usaba funciones para esto, ¡pero las clases son mucho más poderosas y organizadas! 💪
- [[#1. ¿Qué son las Vistas Basadas en Clases (CBVs)? 🤔|1. ¿Qué son las Vistas Basadas en Clases (CBVs)? 🤔]]
- [[#2. La vista básica: `View` 🚀|2. La vista básica: `View` 🚀]]
- [[#3. CBVs genéricas: ¡Tu mejor amigo para el CRUD! 🧑‍💻|3. CBVs genéricas: ¡Tu mejor amigo para el CRUD! 🧑‍💻]]
	- [[#3. CBVs genéricas: ¡Tu mejor amigo para el CRUD! 🧑‍💻#3.1 `TemplateView`: Mostrar una plantilla 🖼️|3.1 `TemplateView`: Mostrar una plantilla 🖼️]]
	- [[#3. CBVs genéricas: ¡Tu mejor amigo para el CRUD! 🧑‍💻#3.2 `ListView`: Listar objetos de un modelo 📋|3.2 `ListView`: Listar objetos de un modelo 📋]]
	- [[#3. CBVs genéricas: ¡Tu mejor amigo para el CRUD! 🧑‍💻#3.3 `DetailView`: Mostrar los detalles de un objeto 🔍|3.3 `DetailView`: Mostrar los detalles de un objeto 🔍]]
	- [[#3. CBVs genéricas: ¡Tu mejor amigo para el CRUD! 🧑‍💻#3.4 CreateView, UpdateView, DeleteView: ¡El resto del CRUD! ✍️🔄❌|3.4 CreateView, UpdateView, DeleteView: ¡El resto del CRUD! ✍️🔄❌]]
- [[#4. Conectando las CBVs a tus URLs 🔗|4. Conectando las CBVs a tus URLs 🔗]]
- [[#Mini Repaso General:|Mini Repaso General:]]
## 1. ¿Qué son las Vistas Basadas en Clases (CBVs)? 🤔
Las **Vistas Basadas en Clases** (del inglés **Class-Based Views**, o **CBVs**) son una forma de escribir vistas en Django utilizando **clases de Python** en lugar de funciones. La idea principal es aprovechar la **programación orientada a objetos (POO)** para hacer nuestro código más **reutilizable**, **mantenible** y **legible**.
- **Ventajas de las CBVs:**
    - **Reutilización de código:** Podemos heredar de clases existentes y sobrescribir solo lo que necesitamos. ¡Menos código repetido!
    - **Organización:** Ayudan a estructurar mejor la lógica de nuestra aplicación.
    - **Legibilidad:** A veces, el código de las CBVs es más fácil de entender, especialmente cuando se trata de operaciones comunes.
    - **Manejo de métodos HTTP:** Las CBVs tienen métodos específicos para manejar diferentes tipos de peticiones HTTP (GET, POST, PUT, DELETE, etc.).
- **¿Cuándo usarlas?**
    - Casi siempre. Para tareas comunes como listar objetos, mostrar detalles, crear, actualizar o eliminar (conocidas como **CRUD**), Django ya nos ofrece CBVs genéricas que nos ahorran un montón de tiempo.
    - Cuando nuestra lógica es más compleja y queremos tener una estructura más clara.
## 2. La vista básica: `View` 🚀
La clase `View` es la **clase base** para todas las vistas basadas en clases en Django. Es como el punto de partida. Si queremos crear una vista que no encaja en las CBVs genéricas, podemos heredar directamente de `View`.
- **Ejemplo de una `View` sencilla:**
    ```python
    # En tu archivo views.py
    from django.views import View
    from django.http import HttpResponse
    
    class MiPrimeraVista(View):
        def get(self, request):
            # Este método se ejecuta cuando la petición es un GET
            return HttpResponse("¡Hola desde mi primera CBV!")
    
        def post(self, request):
            # Este método se ejecuta cuando la petición es un POST
            return HttpResponse("¡Recibí tu POST, gracias!")
    ```
    - Aquí, definimos un método `get()` y un método `post()`. Django automáticamente llamará al método correspondiente según el tipo de petición HTTP. ¡Súper útil!
## 3. CBVs genéricas: ¡Tu mejor amigo para el CRUD! 🧑‍💻
Django nos proporciona una serie de **CBVs genéricas** que ya implementan la lógica para las operaciones más comunes. ¡Esto nos ahorra muchísimo trabajo! Aquí te presento algunas de las más usadas:
### 3.1 `TemplateView`: Mostrar una plantilla 🖼️
- Sirve para mostrar una **plantilla HTML** sin necesidad de interactuar con la base de datos o lógica compleja.
- **Uso:**
    ```python
    # En tu archivo views.py
    from django.views.generic import TemplateView
    
    class PaginaInicio(TemplateView):
        template_name = 'mi_app/inicio.html' # Ruta a tu plantilla
    ```
### 3.2 `ListView`: Listar objetos de un modelo 📋
- Ideal para mostrar una **lista de objetos** de un modelo de la base de datos.
- **Atributos clave:**
    - `model`: El modelo del cual queremos listar los objetos.
    - `queryset`: Una consulta personalizada si no queremos usar `model.objects.all()`.
    - `context_object_name`: El nombre de la variable que estará disponible en la plantilla (por defecto es `object_list`).
    - `template_name`: La plantilla a renderizar.
- **Ejemplo:**
    ```python
    # En tu archivo views.py
    from django.views.generic import ListView
    from .models import Producto # Asumiendo que tienes un modelo Producto
    
    class ListaProductos(ListView):
        model = Producto
        template_name = 'mi_app/lista_productos.html'
        context_object_name = 'productos' # Ahora puedes acceder a 'productos' en tu plantilla
    ```
    - En `lista_productos.html` podrás hacer un `{% for producto in productos %}`.
### 3.3 `DetailView`: Mostrar los detalles de un objeto 🔍
- Se usa para mostrar los **detalles de un único objeto** de un modelo, normalmente basado en su **ID o slug** en la URL.
- **Atributos clave:**
    - `model`: El modelo del cual queremos mostrar los detalles.
    - `context_object_name`: El nombre de la variable en la plantilla (por defecto es `object`).
    - `template_name`: La plantilla a renderizar.
- **Ejemplo:**
    ```python
    # En tu archivo views.py
    from django.views.generic import DetailView
    from .models import Producto
    
    class DetalleProducto(DetailView):
        model = Producto
        template_name = 'mi_app/detalle_producto.html'
        context_object_name = 'producto'
    ```
    - En `detalle_producto.html` podrás acceder al `producto` y mostrar sus atributos.
### 3.4 CreateView, UpdateView, DeleteView: ¡El resto del CRUD! ✍️🔄❌
Estas tres CBVs se encargan de crear, actualizar y eliminar objetos. Son muy potentes porque pueden generar automáticamente formularios a partir de tus modelos.
- **Atributos clave (comunes):**
    - `model`: El modelo con el que se está trabajando.
    - `fields`: Una tupla o lista de los campos del modelo que quieres incluir en el formulario.
    - `form_class`: Una clase de formulario personalizada si no quieres que Django lo cree automáticamente.
    - `template_name`: La plantilla para el formulario.
    - `success_url`: La URL a la que redirigir después de una operación exitosa (crear, actualizar, eliminar).
- **Ejemplo `CreateView`:**
    ```python
    # En tu archivo views.py
    from django.views.generic.edit import CreateView
    from .models import Producto
    from django.urls import reverse_lazy # Para redirigir
    
    class CrearProducto(CreateView):
        model = Producto
        fields = ['nombre', 'descripcion', 'precio'] # Campos para el formulario
        template_name = 'mi_app/producto_form.html' # Una plantilla genérica para formularios
        success_url = reverse_lazy('lista_productos') # Redirige a la lista después de crear
    ```
- `UpdateView` y `DeleteView` funcionan de manera similar, pidiendo un `pk` o `slug` en la URL para identificar el objeto a actualizar o eliminar.
## 4. Conectando las CBVs a tus URLs 🔗
Una vez que defines tus CBVs, necesitas conectarlas a tus URLs en el archivo `urls.py`. La forma de hacerlo es usando el método `.as_view()`.
- **Ejemplo en `urls.py`:**
    ```python
    # En tu archivo urls.py de tu aplicación (ej. mi_app/urls.py)
    from django.urls import path
    from .views import MiPrimeraVista, ListaProductos, DetalleProducto, CrearProducto
    
    urlpatterns = [
        path('hola/', MiPrimeraVista.as_view(), name='mi_primera_vista'),
        path('productos/', ListaProductos.as_view(), name='lista_productos'),
        path('productos/<int:pk>/', DetalleProducto.as_view(), name='detalle_producto'),
        path('productos/crear/', CrearProducto.as_view(), name='crear_producto'),
    ]
    ```
    - El método `.as_view()` convierte la clase en una función que Django puede manejar como una vista.
## Mini Repaso General:
¡Listo! Las **Vistas Basadas en Clases (CBVs)** en Django nos permiten escribir código de vista más **organizado** y **reutilizable** usando la programación orientada a objetos. En lugar de funciones, usamos **clases** que pueden heredar de otras clases para extender su funcionalidad. La clase `View` es la base, y Django nos ofrece **CBVs genéricas** como `TemplateView` para mostrar plantillas, `ListView` para listar objetos, `DetailView` para mostrar detalles, y `CreateView`, `UpdateView`, `DeleteView` para manejar las operaciones CRUD (crear, leer, actualizar, borrar). Para usarlas, simplemente las importamos, las configuramos con sus atributos (`model`, `template_name`, `fields`, `success_url`, etc.) y las conectamos a nuestras URLs usando el método `.as_view()`. ¡Simplifican muchísimo el desarrollo en Django!