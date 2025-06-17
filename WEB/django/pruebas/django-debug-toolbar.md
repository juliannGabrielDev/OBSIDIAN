---
aliases:
  - 🔧 Django Debug Toolbar
tags:
  - django
  - pruebas
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
Fecha: 2025-06-09
---
# 🔧 Django Debug Toolbar

La Django Debug Toolbar es un panel de depuración configurable que aparece en el lateral de tu navegador mientras desarrollas. Te proporciona una enorme cantidad de información de depuración sobre la petición actual, el estado de la aplicación y el rendimiento.

---

### ✨ Características Principales

Esta herramienta te ofrece varios paneles informativos, cada uno enfocado en un aspecto diferente de tu aplicación:

- **Versiones:** Muestra las versiones de Python, Django y las aplicaciones instaladas.
- **Tiempo:** Información detallada del tiempo de procesamiento de la petición.
- **Settings:** Muestra todos los valores de tu archivo `settings.py`.
- **Headers:** Muestra las cabeceras HTTP de la petición y la respuesta.
- **Petición (Request):** Detalles de la petición `HttpRequest`, incluyendo los datos de `GET`, `POST` y las variables de sesión.
- **Consultas SQL:** El panel más útil. Muestra todas las consultas a la base de datos que se ejecutaron para generar la página, cuánto tiempo tomó cada una y el SQL exacto.
- **Archivos Estáticos:** Información sobre los archivos estáticos encontrados por `staticfiles`.
- **Plantillas (Templates):** Muestra qué plantillas se usaron, el contexto de cada una y su árbol de herencia.
- **Caché:** Información sobre las llamadas al sistema de caché.
- **Señales (Signals):** Lista las señales que se dispararon durante la petición.
- **Logs:** Muestra los mensajes de log de Python.

---

### 🚀 Instalación y Configuración

Integrar la Debug Toolbar en tu proyecto es un proceso sencillo que se realiza en pocos pasos.

#### 1. Instalación del Paquete

Primero, instala el paquete usando `pip`.

```shell
pip install django-debug-toolbar
```

#### 2. Configuración en `settings.py`

A continuación, debes realizar tres cambios en el archivo `settings.py` de tu proyecto.

- **Añadir a `INSTALLED_APPS`**: Asegúrate de que `debug_toolbar` esté en tu lista de aplicaciones instaladas. Colócala cerca del principio de la lista si es posible.
    
    ```python
    # settings.py
    INSTALLED_APPS = [
        # ... otras apps
        "debug_toolbar",
        # ... más apps
    ]
    ```
    
- **Añadir a `MIDDLEWARE`**: La toolbar tiene su propio middleware que procesa la información de la petición.
    
    ```python
    # settings.py
    MIDDLEWARE = [
        # ... otros middleware
        "debug_toolbar.middleware.DebugToolbarMiddleware",
        # ... más middleware
    ]
    ```
    
- **Definir `INTERNAL_IPS`**: La toolbar solo se muestra si la dirección IP de tu petición está en la lista `INTERNAL_IPS`. Para desarrollo local, esto es suficiente.
    
    ```python
    # settings.py
    INTERNAL_IPS = [
        "127.0.0.1",
    ]
    ```
    
    > [!WARNING] Despliegue en Producción
    > 
    > ¡Cuidado! La Debug Toolbar nunca debe estar activa en un entorno de producción. Expone información muy sensible de la configuración y la base de datos. Asegúrate de que tu configuración de producción no la incluya.
    

#### 3. Configuración de las URLs

Finalmente, necesitas añadir las URLs de la toolbar a tu archivo `urls.py` principal.

```python
# urls.py
from django.conf import settings
from django.urls import include, path

urlpatterns = [
    # ... tus otras urls
]

if settings.DEBUG:
    urlpatterns += [
        path("__debug__/", include("debug_toolbar.urls")),
    ]
```

> [!TIP] Condicional con settings.DEBUG
> 
> Consejo pro: Al envolver la URL de la toolbar en un if settings.DEBUG:, te aseguras de que solo esté disponible cuando el modo de depuración de Django esté activado, añadiendo una capa extra de seguridad.

Una vez completados estos pasos, ejecuta tu servidor de desarrollo (`python manage.py runserver`) y visita una página de tu sitio. ¡Deberías ver una pestaña "djdt" flotando en el lado derecho de tu pantalla!

---

### 🔍 Uso Práctico: Detectando Problemas

La Django Debug Toolbar es excelente para diagnosticar problemas comunes.

- **Problema N+1 en Consultas**: Si ves un gran número de consultas SQL duplicadas en el panel de SQL, probablemente tengas un "problema N+1". Puedes solucionarlo usando `select_related` o `prefetch_related` en tus QuerySets.
- **Contexto de Plantilla Incorrecto**: Si una variable no aparece en tu plantilla como esperabas, el panel de "Templates" te permite inspeccionar el diccionario de contexto exacto que se le pasó, ayudándote a encontrar errores de tipeo o lógica.
- **Rendimiento Lento**: El panel de "Tiempo" te desglosa cuánto tarda cada parte del ciclo de vida de la petición (base de datos, renderizado, etc.), permitiéndote identificar cuellos de botella.