---
aliases:
  - URLs con Expresiones Regulares en Django 🐍
tags:
  - django
  - views
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
---
# URLs con Expresiones Regulares en Django 🐍
- [[#1. ¿Qué onda con las URLs en Django?|1. ¿Qué onda con las URLs en Django?]]
- [[#2. ¿Y qué son las Expresiones Regulares (Regex)? 🧐|2. ¿Y qué son las Expresiones Regulares (Regex)? 🧐]]
- [[#3. `path()` vs. `re_path()`: ¿Cuál uso? 🤔|3. `path()` vs. `re_path()`: ¿Cuál uso? 🤔]]
- [[#4. Patrones Regex Comunes en URLs 📝|4. Patrones Regex Comunes en URLs 📝]]
- [[#5. Grupos Nombrados: ¡Haciendo la vida más fácil! ✨|5. Grupos Nombrados: ¡Haciendo la vida más fácil! ✨]]
- [[#6. Tips y Buenas Prácticas 💡|6. Tips y Buenas Prácticas 💡]]
- [[#Mini Repaso Final 🚀|Mini Repaso Final 🚀]]

## 1. ¿Qué onda con las URLs en Django?

En Django, cuando un usuario pide una página web, Django revisa una lista de **patrones de URL** para ver cuál coincide con la petición. Es como un mapa que le dice a Django: "Si alguien pide `/articulos/2024/`, llévalo a esta función (vista) que muestra los artículos de 2024". 🗺️

Para definir estos patrones, usamos el archivo `urls.py` dentro de nuestras apps o en el proyecto principal.

## 2. ¿Y qué son las Expresiones Regulares (Regex)? 🧐

Las **expresiones regulares** (o **regex**) son secuencias de caracteres que definen un patrón de búsqueda. Suenan complicadas, ¡pero son súper útiles para encontrar y manipular texto! En el contexto de las URLs de Django, nos sirven para definir patrones más flexibles y específicos que una simple cadena de texto.

Por ejemplo, si queremos que una URL acepte cualquier número de 4 dígitos (como un año), una regex es perfecta para eso.

## 3. `path()` vs. `re_path()`: ¿Cuál uso? 🤔

Django nos da dos funciones principales para definir patrones de URL:

- `path()`: Usa un sistema de "conversores de ruta" más simple y legible para capturar partes de la URL. Por ejemplo, `<int:id_articulo>` captura un entero y lo pasa como `id_articulo` a la vista. ¡Es la opción recomendada para la mayoría de los casos! 👍
- `re_path()`: Esta es la que nos interesa hoy. Nos permite usar toda la potencia de las **expresiones regulares** para definir patrones de URL. Se usa cuando los conversores de `path()` no son suficientes o necesitamos una lógica de coincidencia más compleja.

Mira este ejemplo básico en un archivo `urls.py`:

```python
from django.urls import path, re_path
from . import views

urlpatterns = [
    # Usando path() para un artículo específico por ID (entero)
    path('articulos/<int:id_articulo>/', views.detalle_articulo_por_id),

    # Usando re_path() para un artículo por año (4 dígitos)
    re_path(r'^articulos/(?P<anio>[0-9]{4})/$', views.articulos_por_anio),
]
```

En el ejemplo de `re_path()`:

- `r''`: Indica que es una cadena "raw" (cruda), para que Python no interprete los `\` de forma especial.
- `^`: Coincide con el inicio de la cadena.
- `articulos/`: Coincide literalmente con "articulos/".
- `(?P<anio>[0-9]{4})`: ¡Esta es la parte chida!
    - `(?P<anio>...)`: Captura el texto que coincida con el patrón dentro de los paréntesis y lo nombra `anio`. Este nombre se usará como argumento en la vista.
    - `[0-9]{4}`: Coincide con exactamente 4 dígitos (del 0 al 9).
- `/`: Coincide literalmente con la barra final.
- `$`: Coincide con el fin de la cadena.

## 4. Patrones Regex Comunes en URLs 📝

Aquí van algunos ejemplos de expresiones regulares que te puedes encontrar o necesitar:

| **Descripción**                                   | **Expresión Regular**                                                           | **Ejemplo de URL Coincidente**                  |
| ------------------------------------------------- | ------------------------------------------------------------------------------- | ----------------------------------------------- |
| Cualquier cadena de texto                         | `(.*)`                                                                          | `/pagina/cualquier-cosa/`                       |
| Un número (uno o más dígitos)                     | `([0-9]+)` o `(\d+)`                                                            | `/producto/123/`                                |
| Un slug (letras, números, guiones, guiones bajos) | `([-a-zA-Z0-9_]+)` o `([\w-]+)`                                                 | `/blog/mi-primer-post/`                         |
| Un año (4 dígitos)                                | `([0-9]{4})`                                                                    | `/eventos/2025/`                                |
| Mes y día (MM/DD)                                 | `([0-9]{2})/([0-9]{2})`                                                         | `/fecha/12/31/`                                 |
| UUID                                              | `([a-fA-F0-9]{8}-[a-fA-F0-9]{4}-[a-fA-F0-9]{4}-[a-fA-F0-9]{4}-[a-fA-F0-9]{12})` | `/objeto/550e8400-e29b-41d4-a716-446655440000/` |

**¡Ojo!** Es importante que tus expresiones regulares sean lo más específicas posible para evitar coincidencias no deseadas.

## 5. Grupos Nombrados: ¡Haciendo la vida más fácil! ✨

Como viste en el ejemplo de `re_path()`, usamos `(?P<nombre_grupo>...)`. Esto se llama **grupo nombrado**.

- `?P<nombre_grupo>`: Le da un nombre al grupo capturado.
- Django pasará el valor capturado a tu vista como un argumento con ese nombre.

Ejemplo:

```python
# urls.py
from django.urls import re_path
from . import views

urlpatterns = [
    re_path(r'^reportes/(?P<anio>[0-9]{4})/(?P<mes>[0-9]{2})/$', views.reporte_mensual),
]

# views.py
from django.http import HttpResponse

def reporte_mensual(request, anio, mes):
    return HttpResponse(f"Mostrando reporte para el mes {mes} del año {anio}.")
```

En este caso, la vista `reporte_mensual` recibirá dos argumentos: `anio` y `mes`, ¡gracias a los grupos nombrados! 😎

## 6. Tips y Buenas Prácticas 💡

- **Prefiere `path()`**: Si puedes lograrlo con `path()` y sus conversores, ¡úsalo! Es más legible y menos propenso a errores.
- **Sé específico**: Evita regex demasiado generales como `(.*)` a menos que sea estrictamente necesario y estés seguro de lo que haces.
- **Prueba tus regex**: Usa herramientas online (como regex101.com) para probar tus expresiones regulares antes de ponerlas en Django. ¡Te ahorrará dolores de cabeza! 🤕➡️😄
- **Usa cadenas "raw"**: Siempre usa `r''` para tus patrones de regex en Python para evitar problemas con las barras invertidas.
- **No olvides `^` y `$`**: `^` (inicio) y `$` (fin) son importantes para asegurar que toda la URL coincida, y no solo una parte. Django los añade automáticamente en `path()`, pero en `re_path()` tú tienes el control.
- **Documenta**: Si tienes una regex compleja, añade un comentario explicando qué hace. ¡Tu yo del futuro (u otros desarrolladores) te lo agradecerá! 🙏

---

## Mini Repaso Final 🚀

¡A ver si quedó claro!

- Django usa **patrones de URL** para dirigir las peticiones a las **vistas** correctas.
- Podemos usar `path()` (más simple, con conversores) o `re_path()` (con **expresiones regulares** completas).
- Las **expresiones regulares** nos dan mucha flexibilidad para definir patrones complejos en las URLs.
- Los **grupos nombrados** `(?P<nombre>...)` en `re_path()` permiten pasar los valores capturados como argumentos con nombre a nuestras vistas.
- Es bueno ser **específico** con las regex y probarlas bien.

¡Y eso es todo por hoy sobre expresiones regulares en las URLs de Django! ¡Espero que este repaso te sirva un montón! 🎉