---
aliases:
  - 🔧 Instalación de Django REST Framework
tags:
  - django
  - django-rest-framework
  - apis
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
  - "[[indice-apis|🔌 Índice APIs]]"
Fecha: 2025-06-07
---
# 🔧 Instalación de Django REST Framework

A continuación se detalla cómo instalar y configurar **Django REST Framework (DRF)**, la herramienta esencial para construir APIs web con Django. Cubriremos dos métodos populares de gestión de paquetes: `pip` y `pipenv`.

> [!INFO] ¿Qué es Django REST Framework?
> 
> DRF es un toolkit potente y flexible construido sobre Django que simplifica enormemente la creación de APIs RESTful.

---

## 📦 Método 1: Instalación con `pip`

`pip` es el gestor de paquetes estándar de Python. Es ideal para proyectos sencillos o cuando no se necesita un manejo complejo de entornos virtuales.

### 1. Instalar el paquete

Abre tu terminal y ejecuta el siguiente comando para instalar Django y Django REST Framework.

```shell
pip install django djangorestframework
```

### 2. Verificar la instalación

Puedes listar los paquetes instalados para asegurarte de que `djangorestframework` esté presente.

```shell
pip freeze
```

Deberías ver `djangorestframework` junto con su número de versión en la lista.

---

## 🌳 Método 2: Instalación con `pipenv`

`pipenv` es una herramienta que gestiona tanto los paquetes como los entornos virtuales de un proyecto de forma automática, creando un `Pipfile` para las dependencias.

> [!TIP] ¿Por qué usar pipenv?
> 
> Consejo pro: pipenv aísla las dependencias de tu proyecto, evitando conflictos entre diferentes proyectos y asegurando que las compilaciones sean deterministas y replicables.

### 1. Instalar `pipenv`

Si aún no lo tienes, primero instala `pipenv` en tu sistema.

```shell
pip install pipenv
```

### 2. Crear el entorno e instalar paquetes

Navega a la carpeta de tu proyecto en la terminal. Luego, usa `pipenv` para instalar Django y DRF. `pipenv` creará automáticamente un entorno virtual para ti.

```shell
pipenv install django djangorestframework
```

### 3. Activar el entorno virtual

Para trabajar dentro del entorno del proyecto, actívalo con:

```shell
pipenv shell
```

Notarás que el prompt de tu terminal cambia, indicando que el entorno está activo.

---

## ⚙️ Configuración en tu Proyecto Django

Independientemente del método de instalación que hayas elegido, los siguientes pasos son necesarios para integrar DRF en tu proyecto.

### 1. Añadir `rest_framework` a `INSTALLED_APPS`

Abre el archivo `settings.py` de tu proyecto Django y añade `'rest_framework'` a la lista de `INSTALLED_APPS`.

```python
# settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    # Aplicaciones de terceros
    'rest_framework',

    # Tus aplicaciones
    'tu_app',
]
```

> [!WARNING] Orden de las aplicaciones
> 
> ¡Cuidado! Aunque generalmente no causa problemas, es una buena práctica colocar las aplicaciones de terceros después de las de Django y antes de las tuyas.

### 2. Configurar las URLs (Opcional)

Si deseas utilizar la API navegable de DRF, es una buena idea incluir sus URLs de login y logout. Abre el archivo `urls.py` principal de tu proyecto.

```python
# urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    # Añade esta línea para las vistas de login/logout de la API navegable
    path('api-auth/', include('rest_framework.urls')),
]
```

> [!SUCCESS] ✅ ¡Configuración Completa!
> 
> ¡Éxito! Con estos pasos, Django REST Framework ya está instalado y configurado en tu proyecto. Ya puedes empezar a crear tus serializers, views y routers para construir tu API.

