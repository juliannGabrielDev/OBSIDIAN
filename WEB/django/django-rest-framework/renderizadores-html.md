---
aliases:
  - Renderizadores de HTML 🌐
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
# Renderizadores de HTML en Django REST Framework 🌐
Django REST Framework (DRF) no solo es para APIs JSON. También ofrece renderizadores que permiten servir contenido HTML, integrándose con el sistema de plantillas de Django. Esto es útil para construir aplicaciones web tradicionales o para proporcionar una interfaz navegable para tus APIs.
### 📄 TemplateHTMLRenderer
El `TemplateHTMLRenderer` renderiza datos a HTML utilizando el sistema de plantillas estándar de Django. A diferencia de otros renderizadores, los datos que se pasan a la `Response` no necesitan ser serializados previamente. En su lugar, el `Response` recibe un diccionario de datos que se utilizará como contexto para la plantilla.
**Características Clave:**
- **Integración con Plantillas de Django:** Utiliza las plantillas de Django para generar la salida HTML.
- **Contexto de Plantilla:** Los datos pasados a la `Response` se convierten directamente en el contexto para la plantilla.
- **Nombre de Plantilla:** El nombre de la plantilla se puede especificar de varias maneras:
    - Un argumento `template_name` explícito en la `Response`.
    - Un atributo `.template_name` explícito establecido en la clase del renderizador.
    - El resultado de llamar a `view.get_template_names()`.
**Ejemplo de Uso:**
```python
from rest_framework.renderers import TemplateHTMLRenderer
from rest_framework.response import Response
from rest_framework.views import APIView

class UserDetail(APIView):
    renderer_classes = [TemplateHTMLRenderer]
    template_name = 'user_detail.html'

    def get(self, request, pk):
        user = self.get_object() # Suponiendo que get_object() recupera el usuario
        return Response({'user': user}) # 'user' será accesible en la plantilla como {{ user }}
```

> [!TIP] Despliegue Híbrido
> 
> Si tu API incluye vistas que pueden servir tanto páginas web regulares como respuestas de API (por ejemplo, JSON), puedes considerar hacer de TemplateHTMLRenderer tu renderizador predeterminado. Esto es útil para compatibilidad con navegadores antiguos que pueden enviar encabezados Accept incorrectos.
### 🧱 StaticHTMLRenderer
El `StaticHTMLRenderer` es un renderizador más simple que simplemente devuelve HTML pre-renderizado. A diferencia del `TemplateHTMLRenderer`, no espera un diccionario de contexto para una plantilla, sino que la respuesta ya debe ser el HTML final.
**Características Clave:**
- **HTML Directo:** Devuelve directamente el string HTML que se le proporciona.
- **Sin Plantillas:** No utiliza el motor de plantillas de Django.
**Ejemplo de Uso:**
```python
from rest_framework.renderers import StaticHTMLRenderer
from rest_framework.response import Response
from rest_framework.views import APIView

class MyStaticPageView(APIView):
    renderer_classes = [StaticHTMLRenderer]

    def get(self, request):
        html_content = "<h1>¡Hola desde HTML estático!</h1><p>Este es un ejemplo simple.</p>"
        return Response(html_content)
```

> [!INFO] Cuándo usar cada uno
> 
> - Utiliza `TemplateHTMLRenderer` cuando necesites renderizar datos dinámicos utilizando las plantillas de Django, similar a cómo funcionan las vistas tradicionales de Django.
> - Utiliza `StaticHTMLRenderer` cuando ya tienes el HTML generado (por ejemplo, desde un sistema de construcción de frontend) y solo necesitas servirlo directamente sin procesamiento adicional del lado del servidor.