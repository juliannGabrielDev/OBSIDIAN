---
aliases:
  - 🕷️ Índice Django
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
---
![[django.svg]]
# 🧭 Django
- [[#Teoría|Teoría]]
- [[#Primeros Pasos|Primeros Pasos]]
- [[#Views|Views]]
- [[#Modelos|Modelos]]
- [[#Formularios]]
- [[#Admin]]
- [[#Configuración de MySQL]]
- [[#Plantillas]]
- [[#Pruebas]]
- [[#Django Rest Framework]]

>[!IMPORTANT] Importante
>**[[hoja-trucos-comandos|📌 Hoja de Trucos de Comandos 🚀]]**
---
## Teoría
| Nombre                                               | Palabras Clave                                                                        |
| ---------------------------------------------------- | ------------------------------------------------------------------------------------- |
| [[arquitectura-mvt\|Arquitectura MVT en Django 🏗️]] | Modelo, Vista, Plantilla, Arquitectura de Software, Patrón de Diseño, ORM, HTTP, URL. |
## Primeros Pasos
| Nombre                                                                | Palabras Clave                                                                                                                                                     |
| --------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [[estructura-proyecto\|Estructura de un proyecto Django 📚]]          | `manage.py`, `settings.py`, `urls.py`, `models.py`, `views.py`, Migraciones, Archivos estáticos, Plantillas (Templates).                                           |
| [[iniciar-proyecto\|Cómo Iniciar un Proyecto Django 🚀]]              | Entorno virtual, `venv`, Activar entorno, Instalar Django, `pip install django`, Crear proyecto, `startproject`, `manage.py`, Servidor de desarrollo, `runserver`. |
| [[app\|Estructura de una App en Django: Creación y Configuración 📦]] | `startapp`, `INSTALLED_APPS`, `settings.py`, Vistas, `views.py`, `HttpResponse`, URLs de la app, `include`, `runserver`.                                           |
## Views

| Nombre                                                                      | Palabras Clave                                                                                                                                                                                                   |
| --------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [[http-request\|Entendiendo HttpRequest 🚀]]                                | request, `GET`, `POST`, method, user, session, headers, body, views, HTTP.                                                                                                                                       |
| [[http-response\|Descifrando HttpResponse en Django 📬🗣️💬]]               | `JsonResponse`, `httpresponseredirect`, `HttpResponseNotFound`, `Content-Type`, status code, headers, views, HTTP, respuesta, HTML, JSON.                                                                        |
| [[expresiones-regulares-url\|URLs con Expresiones Regulares en Django 🐍]]  | URLs, expresiones regulares, regex, `re_path`, `path`, patrones de URL, grupos nombrados.                                                                                                                        |
| [[applications-namespaces\|Espacios de Nombres de Aplicación en Django 📦]] | URLs, espacios de nombres, app_name, namespacing, reverse, url template tag, organización de proyectos, colisión de nombres.                                                                                     |
| [[instance-namespaces\|Espacios de Nombres de Instancia en Django 🚀]]      | URLs, `include`, `app_name`, reutilización de apps, organización de proyectos, `request.resolver_match`.                                                                                                         |
| [[vistas-basadas-clases\|Vistas Basadas en Clases (CBVs)]]                  | CBVs, `View`, `TemplateView`, `ListView`, `DetailView`, `CreateView`, `UpdateView`, `DeleteView`, CRUD.                                                                                                          |
| [[manejo-errores-vistas\|Manejo de Errores en Django Views 🐞]]             | `try-except`, `Http404`, `PermissionDenied`, `SuspiciousOperation`, error 500, error 404, error 403, error 400, `get_object_or_404`, `handler404`, `handler500`, `handler403`, `handler400`, `DEBUG`, `logging`. |
| [[vistas-genericas\|Vistas Genéricas 🧩🧬🚀]]                               | `TemplateView`, `ListView`, `DetailView`, `CreateView`, `UpdateView`, `DeleteView`, `FormView`, `get_queryset`, `get_context_data`, `form_valid`, `success_url`, `model`, `template_name`, `.as_view()`.         |
## Modelos

| Nombre                                                                              | Palabras Clave                                                                                                                   |
| ----------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| [[modelos\|Un Vistazo a los Modelos en Django 🧩💡]]                                | `Model`, migraciones, `Makemigrations`, `QuerySet`, `get`, `create`, `save` `delete`, admin, `default`, `on_delete`.             |
| [[relaciones-entre-modelos\|Entendiendo las Relaciones entre Modelos en Django 🔗]] | `ManyToManyField`, `OneToOneField`, uno a muchos, ORM, `on_delete`, CASCADE, PROTECT, SET_NULL, `related_name`, `primary_key`.   |
| [[migraciones\|Migraciones en Django 🏗️]]                                          | `makemigrations`, `migrate`, `showmigrations`, `sqlmigrate`, `rollback`.                                                         |
| [[querysets\|Explorando los QuerySets 🕵️‍♂️]]                                      | ORM, `Modelo.objects`, `filter`, `exclude`,` order_by`, `get`, `count`, `first`, `last`, `exists`, `create`, `update`, `delete`. |
## Formularios

| Nombre                                                                            | Palabras Clave                                                                                                                          |
| --------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| [[forms\|Entendiendo los Formularios en Django 🚀]]                               | `Form`, `widgets`, `is_valid`, cleaned_data, CSRF, `request.POST`, `request.GET                                                         |
| [[model-form\|Entendiendo los Formularios en Django 🚀 (Continuación ModelForm)]] | `ModelForm`, `Meta` class, modelos.                                                                                                     |
| [[campos\|Hoja de Trucos: Campos de Formulario 📋]]                               | `CharField`, `IntegerField`, `BooleanField`, `ChoiceField`, `ModelChoiceField`, `widgets`, `required`, `label`, `help_text`, `initial`. |
| [[lectura-datos\|Lectura de Datos de Formularios 📬]]                             | `request.POST`, `request.FILES`, `is_valid`, `cleaned_data`, validación.                                                                |
## Admin

| Nombre                                                           | Palabras Clave                                                                                                                                           |
| ---------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [[admin\|🎓 Django Admin: ¡Tu Panel de Control al Instante! 🚀]] | Panel, Datos, Modelos, Superusuario.                                                                                                                     |
| [[gestion-usuarios\|Tuneando el Admin 🛠️]]                      | `readonly_fields`, permisos, superusuario, `is_staff`.                                                                                                   |
| [[permisos\|Entendiendo los Permisos en Django 🔑]]              | `is_staff`, `has_perm`, `PermissionDenied`, `@permission_required`.                                                                                      |
| [[hacer-cumplir-permisos\|Haciendo Cumplir los Permisos 🚦]]     | `@login_required`, `@user_passes_test`, `@permission_required`, `PermissionRequiredMixin`, plantillas, `{{ perms }}`, `{{ user }}`, `urls.py`, `path()`. |
## Configuración de MySQL

| Nombre                                                          | Palabras Clave                                                                 |
| --------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| [[configurar-db\|Cambiando de Base de Datos: ¡Hola, MySQL! 🐬]] | `settings.py`, `mysqlclient`, `migrate`, `STRICT_TRANS_TABLES`, DB API driver. |

## Plantillas

| Nombre                                                                          | Palabras Clave                                                                                                                                                                                            |
| ------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [[plantillas\|El Lenguaje de Plantillas 📜 de Django (DTL) 🏗️✨]]               | Plantillas, variables, filtros, etiquetas, tags, `{{ }}`, `{% %}`, `{# #}`, `\|safe`, `\|date`, `{% if %}`, `{% for %}`, `{% extends %}`, `{% block %}`, `{% url %}`, `{% static %}`, `{% csrf_token %}`. |
| [[django/plantillas/herencia\|Herencia de Plantillas 🧱✨]]                      | `{% extends %}`, `{% block %}`, `{{ block.super }}`, DTL, DRY, base.html.                                                                                                                                 |
| [[mapeando-objetos-modelo-plantillas\|Mapeando Objetos Modelo a Plantillas 🚀]] | ORM, QuerySet, `views.py`, `models.py`, `render`, contexto, DTL, `{{ objeto.atributo }}`, `{% for %}`, `get_object_or_404`, ForeignKey, ManyToManyField.                                                  |
## Pruebas

| Nombre                                            | Palabras Clave                                                                                                                |
| ------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| [[debugging\|Depurando Aplicaciones 🐞🐛]]        | `DEBUG=True`, traceback, `print()`, Django Debug Toolbar, IDE debugger, `manage.py shell`, `shell_plus`.                      |
| [[testing\|Probando Aplicaciones Django 🧪]]      | `unittest`, `TestCase`, `self.client`, aserciones, `manage.py test`, cobertura, coverage, `setUp`, `setUpTestData`, fixtures. |
| [[django-debug-toolbar\|🔧 Django Debug Toolbar]] | Rendimiento, introspección, optimización, `QuerySet`, configuración.                                                          |
## Django Rest Framework

| Nombre                                                                                                         | Palabras Clave                                                                                                                                       |
| -------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| [[setup-django-rest-framework\|🔧 Instalación de Django REST Framework]]                                       | Instalación, `pip`, `pipenv`.                                                                                                                        |
| [[tipos-de-enrutamiento-drf\|🗺️ Tipos de Enrutamiento en Django REST Framework (DRF)]]                        | URLconf, ViewSets, APIView, Endpoints, Automatización.                                                                                               |
| [[vistas-genericas-y-viewsets-drf\|🚀 Vistas Genéricas y ViewSets en DRF]]                                     | Abstracción, productividad, CRUD, herencia, personalización.                                                                                         |
| [[serializers\|🧬 Serializers]]                                                                                | Abstracción, validación, transformación, representación, anidamiento.                                                                                |
| [[model-serializer\|🧬 Serializadores de Modelos]]                                                             | `ModelSerializer`, validación, mapeo, CRUD.                                                                                                          |
| [[relationship-serializers\|Serializadores de Relaciones 💖]]                                                  | `PrimaryKeyRelatedField`, `StringRelatedField`, `SlugRelatedField`, `HyperlinkedRelatedField`.                                                       |
| [[deserializacion-y-validacion\|Deserialización y Validación ✅]]                                               | Transformación, Integridad, Reglas, Errores, Persistencia.                                                                                           |
| [[formato-de-salida-con-renderizadores\|Manejando Formatos de Salida con Renderizadores en DRF 🎨]]            | `json`, `xml`, `djangorestframework-xml`, `REST_FRAMEWORK`.                                                                                          |
| [[renderizadores-html\|Renderizadores de HTML 🌐]]                                                             | `TemplateHTMLRenderer`, `StaticHTMLRenderer`.                                                                                                        |
| [[filtrado-busqueda-drf\|Optimización de API: Filtrado y Búsqueda 🔍]]                                         | API Optimization, data querying, efficient APIs, Django ORM, query parameters.                                                                       |
| [[ordering-drf\|🚀 Ordenamiento en Django REST Framework 🍽️]]                                                 | `queries`, `ordering`, `__all__`, `ordering_fields`, `filters.OrderingFilter`.                                                                       |
| [[validacion\|✨ La Importancia de la Validación de Datos en DRF 🔒]]                                           | `validate_field()`, `validate_price()`, `validate_stock()`, `validate()`, `UniqueTogetherValidator`, `UniqueValidator`, `extra_kwargs`, `min_value`. |
| [[data-sanitization\|✨ Saneamiento de Datos: ¡Protegiendo tu API! 🛡️📝]]                                      | `bleach`, `validate_`, inyección SQL, `validate()`.                                                                                                  |
| [[paginacion-drf\|📝 Paginación: ¡Controlando tus Datos! 🚀]]                                                  | `Paginator`, `try`, `except`, `EmptyPage`, `page`, `per_page`.                                                                                       |
| [[filtrado-y-paginacion-avanzados\|📚 Filtrado y Paginación Avanzados en DRF: ¡Potencia tus APIs! 🚀]]         | Clase `OrderingFilter`, clase `SearchFilter`, `DEFAULT_FILTER_BACKENDS`, `ordering_fields`.                                                          |
| [[cache\|🚀 Almacenamiento en Caché ⚡]]                                                                        | Caché de Base de Datos, Caché de Aplicación/Objeto, Caché de Página/Fragmento, Caché de CDN, Caché de Navegador, Caché de Gateway/Proxy Inverso.     |
| [[auth-drf\|🔑 Autenticación por Token ✨🔒]]                                                                   | `rest_framework.authtoken`, `authentication_classes`, `permission_classes`.                                                                          |
| [[throttling\|🚧 Throttling: Protege tus APIs del Abuso 🛡️🚀]]                                                | `throttle_classes`, `2/minute`, `AnonRateThrottle`, `UserRateThrottle`.                                                                              |
| [[throttling-para-vistas-basadas-en-clases\|🚦 Estrangulamiento (Throttling) de API para Vistas Basadas ⚙️🚀]] | `AnonRateThrottle`, `UserRateThrottle`.                                                                                                              |
| [[djoser\|Djoser: Autenticación 🔑 y Gestión de Usuarios 👤✨]]                                                 | `Djoser`, _endpoints_.                                                                                                                               |
| [[jwt-drf\|Autenticación con JWT en Django 🔐🔑✨]]                                                             | `djangorestframework-simplejwt`, `SIMPLE_JWT`, `INSTALLED_APPS`.                                                                                     |
| [[gestion-de-usuarios-y-grupos-djoser\|🚀 Gestión de Usuarios y Grupos con Djoser ✨🔒]]                        | `IsAdminUser`, `POST`, `DELETE`, `Djoser`.                                                                                                           |
