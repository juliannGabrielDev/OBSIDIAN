---
aliases:
  - Manejando Formatos de Salida con Renderizadores en DRF 🎨
tags:
  - django
  - django-rest-framework
  - apis
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
  - "[[indice-apis|🔌 Índice APIs]]"
Fecha: 2025-06-11
---
# Manejando Formatos de Salida con Renderizadores en DRF 🎨
Los renderizadores en Django REST Framework (DRF) son componentes cruciales que determinan el formato de la respuesta de tu API. Permiten que tu aplicación sea más versátil y utilizable al soportar múltiples tipos de contenido, adaptándose a las necesidades de diferentes clientes.
### 🚀 Renderizadores Principales
DRF gestiona la presentación de datos a través de una lista de clases renderizadoras. El cliente puede elegir el formato deseado especificando la cabecera `Accept` en su solicitud.

| **Renderizador**         | **Paquete/Módulo**         | **Cabecera Accept** | **Descripción**                                                             |
| ------------------------ | -------------------------- | ------------------- | --------------------------------------------------------------------------- |
| **JSONRenderer**         | Incorporado                | `application/json`  | El renderizador por defecto. Devuelve datos en formato JSON.                |
| **BrowsableAPIRenderer** | Incorporado                | `text/html`         | Genera una interfaz HTML interactiva para probar la API desde el navegador. |
| **XMLRenderer**          | `djangorestframework-xml`  | `application/xml`   | Renderizador de terceros para devolver datos en formato XML.                |
| **YAMLRenderer**         | `djangorestframework-yaml` | `application/yaml`  | Renderizador de terceros para datos en formato YAML.                        |
| CSVRenderer              | `djangorestframework-csv`  | `text/csv`          | Renderizador de terceros para devolver datos en formato CSV.                |

> [!TIP] Negociación de Contenido
> 
> Consejo pro: DRF utiliza la negociación de contenido para seleccionar el renderizador. Revisa la cabecera Accept de la solicitud HTTP. Si el navegador la solicita (text/html), usa la API Navegable. Si la cabecera es application/json o no se especifica, usa JSON por defecto.
### ⚙️ Configuración Global de Renderizadores
Puedes definir los renderizadores disponibles para todo tu proyecto en el archivo `settings.py`. Esto se hace dentro del diccionario `REST_FRAMEWORK`.
```python
# settings.py

REST_FRAMEWORK = {
    'DEFAULT_RENDERER_CLASSES': [
        'rest_framework.renderers.JSONRenderer',
        'rest_framework.renderers.BrowsableAPIRenderer', # Comenta esta línea para desactivarla globalmente
    ]
}
```

> [!WARNING] Desactivar la API Navegable
> 
> ¡Cuidado! Si comentas o eliminas BrowsableAPIRenderer de la configuración, solo obtendrás respuestas en JSON (o el primer renderizador de la lista), incluso si accedes desde un navegador web.
### ➕ Añadir Soporte para XML
Ampliar los formatos de salida de tu API es un proceso sencillo.
1. Instalar el Paquete 📦
    Asegúrate de estar en tu entorno virtual y ejecuta:
    ```shell
    pipenv install djangorestframework-xml
    ```
2. Actualizar la Configuración 📝
    Añade XMLRenderer a la lista de renderizadores en settings.py:
    
```python
    # settings.py
    
    REST_FRAMEWORK = {
        'DEFAULT_RENDERER_CLASSES': [
            'rest_framework.renderers.JSONRenderer',
            'rest_framework.renderers.BrowsableAPIRenderer',
            'rest_framework_xml.renderers.XMLRenderer', # Nueva línea
        ]
    }
```
> [!SUCCESS] ¡Listo para XML!
> 
> ✅ ¡Éxito! Ahora tu API puede servir datos en formato XML. Simplemente realiza una solicitud desde un cliente como Insomnia o Postman con la cabecera Accept configurada en application/xml.
### ➕ Añadir Soporte para CSV
Siguiendo un proceso similar, puedes añadir soporte para CSV:
1. Instalar el Paquete 📦 Asegúrate de estar en tu entorno virtual y ejecuta:
    ```shell
    pipenv install drf-csv
    ```
    
2. Actualizar la Configuración 📝 Añade `CSVRenderer` a la lista de renderizadores en `settings.py`:
    ```python
    # settings.py
    
    REST_FRAMEWORK = {
        'DEFAULT_RENDERER_CLASSES': [
            'rest_framework.renderers.JSONRenderer',
            'rest_framework.renderers.BrowsableAPIRenderer',
            'rest_framework_xml.renderers.XMLRenderer',
            'drf_csv.renderers.CSVRenderer', # Nueva línea
        ]
    }
    ```

> [!SUCCESS] ¡Listo para CSV!
> 
> ✅ ¡Éxito! Tu API ahora es capaz de servir datos en formato CSV. Puedes probarlo haciendo una solicitud desde un cliente REST con la cabecera `Accept` configurada en `text/csv`. DRF, serialización, formatos, APIs, adaptabilidad