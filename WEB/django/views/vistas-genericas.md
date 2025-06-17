---
aliases:
  - Vistas Genéricas 🧩🧬🚀
tags:
  - django
  - views
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
Fecha: 2025-06-03
---
# Vistas Genéricas 🧩 en Django: ¡Subclaseando 🧬 para el Poder Supremo 🚀!✨

## 1. ¿Qué Son las Vistas Basadas en Clases (CBV) Genéricas? 🤔
Django viene con un conjunto de "vistas prefabricadas" en forma de clases. Estas **Vistas Genéricas** están diseñadas para manejar patrones súper comunes que encontramos al desarrollar webs:
- Mostrar una lista de objetos (ej. todos los posts de un blog).
- Mostrar los detalles de un objeto específico (ej. un post en particular).
- Mostrar un formulario para crear un objeto nuevo (ej. crear un nuevo post).
- Mostrar un formulario para editar un objeto existente.
- Confirmar y eliminar un objeto.
- Mostrar una página simple con una plantilla.
## 2. ¿Por Qué Subclasear las Vistas Genéricas? 🧐
Podrías usar las vistas genéricas directamente en tu archivo `urls.py` pasándoles algunos atributos. ¡Pero subclasearlas en tu archivo `views.py` es mucho más poderoso y ordenado!
- <mark style="background: #ADCCFFA6;">**Mejor Organización**</mark>: Toda la lógica y configuración para una vista específica se queda encapsulada en su propia clase en `views.py`. ¡Más limpio!
- <mark style="background: #FFF3A3A6;">**Personalización Profunda**</mark>: Puedes sobrescribir métodos específicos de la clase genérica para añadir tu propia lógica personalizada sin tener que reinventar la rueda.
- <mark style="background: #ABF7F7A6;">**Reutilización**</mark>: Puedes crear tus propias vistas genéricas especializadas que hereden de las de Django, y luego usarlas en varios lugares.
- <mark style="background: #FFB86CA6;">**Atributos por Defecto Claros**</mark>: Es más fácil ver y configurar atributos como el modelo (`model`) o el nombre de la plantilla (`template_name`) directamente en la definición de tu clase.
## 3. Vistas Genéricas Comunes para Subclasear y Cómo 🛠️
Aquí te va un repaso de las más usadas y cómo "tunearlas":

---
### 🌟 `TemplateView`

- **Importar desde**: `from django.views.generic.base import TemplateView`
- **Propósito**: Simplemente renderiza una plantilla. Ideal para páginas estáticas o que necesitan un poco de contexto dinámico simple.
- **Atributo Clave Principal**:
    - `template_name = "mi_app/mi_pagina.html"`
- **Método Común a Sobrescribir**:
    - `get_context_data(**kwargs)`: Para añadir datos al contexto que se pasará a la plantilla.
    ```python
    # views.py
    class PaginaAcercaDeView(TemplateView):
        template_name = "paginas/acerca_de.html"
    
        def get_context_data(self, **kwargs):
            context = super().get_context_data(**kwargs)
            context['nombre_empresa'] = "La Super Empresa"
            context['ano_fundacion'] = 2020
            return context
    ```

---

### 📋 `ListView`
- **Importar desde**: `from django.views.generic.list import ListView`
- **Propósito**: Muestra una lista de objetos de un modelo. Automáticamente puede manejar paginación.
- **Atributos Clave Principales**:
    - `model = MiModelo` (El modelo del que listarás objetos).
    - `template_name = "mi_app/lista_mimodelo.html"` (Por defecto busca `app_label/modelo_list.html`).
    - `context_object_name = "mis_objetos"` (Nombre de la variable en la plantilla para la lista. Por defecto es `object_list`).
    - `paginate_by = 10` (Número de objetos por página para la paginación).
    - `ordering = ['-fecha_creacion']` (Para ordenar los objetos).
- **Métodos Comunes a Sobrescribir**:
    - `get_queryset()`: Para personalizar la lista de objetos (ej. filtrar por usuario, aplicar filtros complejos).
    - `get_context_data(**kwargs)`: Para añadir más contexto.
    ```python
    # views.py
    from .models import Articulo
    
    class ArticuloListView(ListView):
        model = Articulo
        template_name = "blog/lista_articulos.html"
        context_object_name = "articulos"
        paginate_by = 5
        ordering = ['-fecha_publicacion']
    
        def get_queryset(self):
            # Ejemplo: Solo mostrar artículos publicados
            return Articulo.objects.filter(publicado=True).order_by('-fecha_publicacion')
    ```

---
### 📄 `DetailView`
- **Importar desde**: `from django.views.generic.detail import DetailView`
- **Propósito**: Muestra los detalles de un objeto específico de un modelo. Espera un `pk` (clave primaria) o `slug` en la URL.
- **Atributos Clave Principales**:
    - `model = MiModelo`
    - `template_name = "mi_app/detalle_mimodelo.html"` (Por defecto `app_label/modelo_detail.html`).
    - `context_object_name = "mi_objeto"` (Por defecto es `object` o el nombre del modelo en minúsculas, ej `articulo`).
    - `pk_url_kwarg = 'id_articulo'` (Si el nombre del parámetro PK en la URL no es `'pk'`).
    - `slug_url_kwarg = 'mi_slug'` (Si el nombre del parámetro slug en la URL no es `'slug'`).
- **Métodos Comunes a Sobrescribir**:
    - `get_object()`: Si necesitas una lógica especial para obtener el objeto.
    - `get_context_data(**kwargs)`: Para añadir más contexto.
    ```python
    # views.py
    class ArticuloDetailView(DetailView):
        model = Articulo
        template_name = "blog/detalle_articulo.html"
        context_object_name = "articulo" # En la plantilla usarás {{ articulo.titulo }}, etc.
    ```

---
### ➕ `CreateView`
- **Importar desde**: `from django.views.generic.edit import CreateView`
- **Propósito**: Muestra un formulario para crear un nuevo objeto de un modelo y procesa su guardado.
- **Atributos Clave Principales**:
    - `model = MiModelo`
    - `template_name = "mi_app/formulario_mimodelo.html"` (Por defecto `app_label/modelo_form.html`).
    - `form_class = MiModelFormPersonalizado` (Si tienes un `forms.ModelForm` personalizado).
    - `fields = ['campo1', 'campo2']` (Si quieres que Django genere el `ModelForm` automáticamente con estos campos). <mark style="background: #FFB86CA6;">No uses `fields` y `form_class` juntos para el mismo modelo.</mark>
    - `success_url = "/url/de/exito/"` (A dónde redirigir tras crear el objeto. Mejor usar `reverse_lazy`).
- **Métodos Comunes a Sobrescribir**:
    - `form_valid(form)`: Para realizar acciones después de que el formulario sea válido y antes/después de guardar (ej. asignar el usuario actual al objeto).
    - `get_success_url()`: Para determinar la URL de éxito dinámicamente.
    - `get_form_kwargs()`: Para pasar argumentos extra al `__init__` del formulario.
    ```python
    # views.py
    from django.urls import reverse_lazy
    
    class ArticuloCreateView(CreateView):
        model = Articulo
        template_name = "blog/crear_articulo.html"
        fields = ['titulo', 'contenido', 'publicado'] # Django creará un ModelForm con estos
        success_url = reverse_lazy('blog:lista_articulos') # 'blog' es el app_name, 'lista_articulos' el name de la URL
    
        def form_valid(self, form):
            form.instance.autor = self.request.user # Asignar el autor actual
            return super().form_valid(form)
    ```
    

---
### ✏️ `UpdateView`
- **Importar desde**: `from django.views.generic.edit import UpdateView`
- **Propósito**: Similar a `CreateView`, pero para mostrar un formulario pre-rellenado para actualizar un objeto existente y procesar su guardado. Espera `pk` o `slug`.
- **Atributos y Métodos**: Prácticamente los mismos que `CreateView`.

---
### 🗑️ `DeleteView`
- **Importar desde**: `from django.views.generic.edit import DeleteView`
- **Propósito**: Muestra una página de confirmación y luego elimina un objeto. Espera `pk` o `slug`.
- **Atributos Clave Principales**:
    - `model = MiModelo`
    - `template_name = "mi_app/confirmar_borrado_mimodelo.html"` (Por defecto `app_label/modelo_confirm_delete.html`).
    - `success_url = reverse_lazy('nombre_url_lista')`
    - `context_object_name = "objeto_a_borrar"`
- **Métodos Comunes a Sobrescribir**:
    - `get_success_url()`
---
### 📝 `FormView`
- **Importar desde**: `from django.views.generic.edit import FormView`
- **Propósito**: Para manejar formularios que **no** están directamente ligados a un modelo (ej. un formulario de contacto que envía un email, un formulario de búsqueda).
- **Atributos Clave Principales**:
    - `template_name = "mi_app/mi_formulario.html"`
    - `form_class = MiFormularioNormal` (Debe ser un `django.forms.Form`).
    - `success_url = reverse_lazy('pagina_gracias')`
- **Métodos Comunes a Sobrescribir**:
    - `form_valid(form)`: Aquí es donde procesas los datos del formulario (ej. enviar el email).
    ```python
    # forms.py
    from django import forms
    class ContactoForm(forms.Form):
        nombre = forms.CharField()
        email = forms.EmailField()
        mensaje = forms.CharField(widget=forms.Textarea)
    
    # views.py
    class ContactoView(FormView):
        template_name = 'contacto/formulario_contacto.html'
        form_class = ContactoForm
        success_url = reverse_lazy('contacto:gracias')
    
        def form_valid(self, form):
            # Aquí procesas los datos del formulario
            nombre = form.cleaned_data['nombre']
            email = form.cleaned_data['email']
            mensaje = form.cleaned_data['mensaje']
            # Lógica para enviar el email...
            print(f"Email de {nombre} ({email}): {mensaje}")
            return super().form_valid(form)
    ```
## 4. Conexión en `urls.py` 🔗
Cuando usas vistas basadas en clases en tu `urls.py`, necesitas llamar al método `.as_view()` de la clase.
```python
# urls.py (por ejemplo, de la app 'blog')
from django.urls import path
from .views import ArticuloListView, ArticuloDetailView, ArticuloCreateView

app_name = 'blog' # Buena práctica para namespacing de URLs

urlpatterns = [
    path('articulos/', ArticuloListView.as_view(), name='lista_articulos'),
    path('articulos/<int:pk>/', ArticuloDetailView.as_view(), name='detalle_articulo'),
    path('articulos/crear/', ArticuloCreateView.as_view(), name='crear_articulo'),
    # ... y así para UpdateView, DeleteView
]
```
## 5. El Poder de `get_context_data(**kwargs)` 💪
Este método es tu comodín para añadir cualquier información extra al contexto que se pasa a la plantilla. Está disponible en casi todas las Vistas Genéricas.
**Patrón Común**:
1. Llama a la implementación del método en la clase padre: `context = super().get_context_data(**kwargs)`. ¡Esto es importante para que el contexto base se cree correctamente!
2. Añade tus propias variables al diccionario `context`.
3. Devuelve el `context`.
```python
# En cualquier subclase de una Vista Genérica
def get_context_data(self, **kwargs):
    context = super().get_context_data(**kwargs) # ¡No olvidar!
    context['numero_aleatorio'] = random.randint(1, 100)
    context['fecha_actual'] = timezone.now()
    # Si es una DetailView y tienes 'self.object' disponible:
    if hasattr(self, 'object') and self.object:
         context['titulo_objeto'] = self.object.titulo 
    return context
```
## 6. Mini Repaso Final 💡

- Las **Vistas Genéricas Basadas en Clases (CBV)** te ahorran escribir código repetitivo para tareas comunes.
- **Subclasearlas** te da el control para personalizarlas y mantener tu código organizado en `views.py`.
- Personalizas mediante:
    - **Atributos de clase** (ej. `model`, `template_name`, `form_class`, `success_url`).
    - **Sobrescribiendo métodos** (ej. `get_queryset()`, `get_context_data()`, `form_valid()`).
- Cada tipo de vista genérica (`ListView`, `DetailView`, `CreateView`, etc.) está hecha para un propósito específico.
- En `urls.py`, usas `.as_view()` para conectar la clase al patrón de URL.