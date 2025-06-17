---
aliases:
  - Depurando Aplicaciones 🐞🐛
tags:
  - django
  - pruebas
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
Fecha: 2025-06-04
---
# Depurando Aplicaciones Django 🐞: ¡Encontrando Bichos como un Detective! 🕵️‍♂️🐛
## 1. ¿Qué es Depurar y Por Qué es Necesario? 🤔
**Depurar** (o _debugging_ en inglés) es el proceso de encontrar y corregir errores (bugs) en tu código. Los errores son normales y parte del desarrollo. ¡Hasta los programadores más experimentados los cometen! Lo importante es saber cómo encontrarlos y aplastarlos. 🔨
Django, al ser un framework robusto, nos ofrece varias ayudas para esta tarea.
## 2. Técnicas y Herramientas Comunes de Depuración en Django 🛠️
Aquí te va una lista de las herramientas y técnicas más usadas:
### 2.1. La Página de Error Detallada de Django (`DEBUG=True`) 📄💥
Esta es tu **primera línea de defensa** cuando estás desarrollando.
- **Activación**: En tu archivo `settings.py`, asegúrate de que `DEBUG = True`.
- **¿Qué muestra?**: Cuando ocurre un error en tu código (una excepción no capturada), en lugar de un error genérico, Django te muestra una página amarilla muy detallada con:
    - **Traceback Completo**: La secuencia de llamadas que llevó al error. <mark style="background: #FFF3A3A6;">Empieza a leerlo desde abajo hacia arriba</mark>; usualmente la causa raíz del error en tu código está cerca del final de la traza que involucra tus archivos.
    - **Variables Locales**: Los valores de las variables en cada nivel del traceback. ¡Súper útil para ver qué valor tenía una variable justo antes del error!
    - **Información de la Petición**: Datos de `GET`, `POST`, `COOKIES`, `SESSION`, y `META` (cabeceras HTTP).
    - **Configuración (`Settings`)**: Un listado de tu configuración de Django.
- <mark style="background: #FF5582A6;">**¡MUY IMPORTANTE!**</mark>: `DEBUG = True` **NUNCA** debe estar activado en un entorno de producción (cuando tu sitio ya está online para el público). Mostraría información sensible que podría ser explotada. En producción, `DEBUG = False`.
### 2.2. Sentencias `print()` (El Clásico Confiable) 🗣️
A veces, lo más simple es lo más efectivo.
- **Uso**: Simplemente añade `print("Mi variable:", mi_variable)` o `print("Llegué hasta aquí en el código")` en tu `views.py`, `models.py`, etc.
- **Salida**: El resultado del `print()` aparecerá en la **consola** donde estás corriendo el servidor de desarrollo (`python manage.py runserver`).
- **Pros**: Fácil y rápido de implementar para chequear valores o flujos.
- **Contras**: Puede llenar tu código de `prints` que luego tienes que quitar, no es interactivo, y si olvidas alguno en producción, podría imprimir a la consola del servidor (aunque no al usuario).
```python
# views.py
def mi_vista(request):
    variable_importante = "Hola Mundo"
    print(f"El valor de variable_importante es: {variable_importante}") # Saldrá en la consola
    # ... resto de tu lógica ...
    if variable_importante == "Adiós":
        print("Entró al if inesperado!")
    return render(request, "mi_template.html", {'dato': variable_importante})
```
### 2.3. Logging (Registro de Eventos) 📜
Es la evolución profesional del `print()`. Django usa el módulo `logging` de Python.
- **Uso**:
    1. Importa: `import logging`
    2. Obtén un logger: `logger = logging.getLogger(__name__)` (usar `__name__` es una buena práctica).
    3. Registra mensajes: `logger.debug("Mensaje de depuración detallado")`, `logger.info("Información general")`, `logger.warning("Algo podría estar mal")`, `logger.error("¡Ocurrió un error!")`, `logger.critical("¡Error muy grave!")`.
- **Configuración**: Puedes hacer una configuración básica en `settings.py` o configuraciones más avanzadas para enviar logs a archivos, rotar logs, etc.
- **Beneficios**:
    - Puedes controlar el nivel de detalle de los mensajes (ej. en desarrollo ves `DEBUG` e `INFO`, en producción solo `WARNING` y `ERROR`).
    - <mark style="background: #BBFABBA6;">Puedes dejar las sentencias de logging en tu código de producción</mark> (configuradas al nivel adecuado).
    - Más estructurado y configurable que `print()`.
```python
# settings.py (configuración muy básica)
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'console': {
            'class': 'logging.StreamHandler',
        },
    },
    'root': {
        'handlers': ['console'],
        'level': 'DEBUG', # En producción cambiar a 'INFO' o 'WARNING'
    },
}

# views.py
import logging
logger = logging.getLogger(__name__)

def otra_vista(request, user_id):
    logger.info(f"Procesando petición para user_id: {user_id}")
    try:
        # ... algo que podría fallar ...
        resultado = 10 / int(user_id) # user_id podría ser 0 o no ser número
        logger.debug(f"Resultado del cálculo: {resultado}")
    except Exception as e:
        logger.error(f"Error en otra_vista para user_id {user_id}: {e}", exc_info=True) # exc_info=True añade el traceback al log
        # ... manejar el error ...
    return render(request, "otra_template.html")
```
### 2.4. El Debugger de Python (PDB) 🤓💻
PDB es el depurador interactivo integrado en Python. Te permite pausar la ejecución de tu código y examinarlo línea por línea en la terminal.
- **Uso**:
    1. Importa: `import pdb`
    2. Pon un "punto de interrupción": `pdb.set_trace()` en la línea donde quieres que se detenga la ejecución.
- **Funcionamiento**: Cuando Django llega a `pdb.set_trace()`, la ejecución se detiene en la consola donde corre `runserver` y verás un prompt `(Pdb)`.
- **Comandos PDB Comunes**:
    - `n` (next): Ejecuta la línea actual y pasa a la siguiente (sin entrar en funciones).
    - `s` (step): Ejecuta la línea actual y, si es una llamada a función, entra en ella.
    - `c` (continue): Continúa la ejecución normal hasta el siguiente `set_trace()` o el final.
    - `l` (list): Muestra el código fuente alrededor de la línea actual.
    - `p nombre_variable`: Imprime el valor de `nombre_variable`.
    - `pp nombre_variable`: Imprime "bonito" (pretty print) el valor de `nombre_variable`.
    - `args`: Muestra los argumentos de la función actual.
    - `q` (quit): Termina la sesión de depuración (y usualmente el request falla).
    - `h` (help): Muestra ayuda de los comandos.
- **Pros**: Inspección interactiva muy potente, no necesitas modificar el código tanto como con `print`.
- **Contras**: Detiene el flujo de la petición, es basado en terminal (puede ser un poco tosco al principio).
```python
# views.py
import pdb

def vista_con_pdb(request):
    dato_interesante = "valor inicial"
    # ... algo de código ...
    dato_interesante = request.GET.get('parametro', 'valor por defecto')
    
    pdb.set_trace() # La ejecución se detendrá aquí en la consola
    
    # En la consola (Pdb) podrás escribir:
    # p dato_interesante
    # p request.user
    # ... etc.
    
    resultado_final = dato_interesante.upper()
    return render(request, "template_pdb.html", {'resultado': resultado_final})
```
### 2.5. Django Debug Toolbar 📊🔧
Esta es una herramienta de terceros <mark style="background: #D2B3FFA6;">INCREÍBLEMENTE ÚTIL</mark>.
- **Instalación**:
    1. `pip install django-debug-toolbar`
    2. Añade `'debug_toolbar'` a `INSTALLED_APPS` en `settings.py`.
    3. Añade sus URLs a tu `urls.py` principal:
        ```python
        # urls.py principal
        from django.conf import settings
        from django.urls import path, include
        
        urlpatterns = [
            # ... tus otras urls ...
        ]
        
        if settings.DEBUG: # Solo añadir en modo DEBUG
            import debug_toolbar
            urlpatterns = [
                path('__debug__/', include(debug_toolbar.urls)),
            ] + urlpatterns
        ```
    4. Configura el middleware en `settings.py` (el orden puede ser importante, usualmente después de `GZipMiddleware` si lo usas, y antes de otros que modifiquen la respuesta):
        ```python
        # settings.py
        MIDDLEWARE = [
            # ... otros middlewares ...
            'debug_toolbar.middleware.DebugToolbarMiddleware',
            # ... otros middlewares ...
        ]
        ```
    5. Asegúrate de que `INTERNAL_IPS = ['127.0.0.1']` esté en `settings.py` para que se muestre.
- **¿Qué ofrece?**: Una barra lateral flotante en tu navegador (cuando `DEBUG=True`) que te da acceso a un montón de información sobre la petición actual:
    - Versiones de Django y Python.
    - Tiempo de la petición.
    - Configuración (`Settings`).
    - Cabeceras HTTP.
    - Variables de la Petición (GET, POST, session, cookies).
    - **Consultas SQL**: <mark style="background: #ADCCFFA6;">¡Fundamental!</mark> Muestra cuántas consultas se hicieron, cuánto tardaron y las sentencias SQL exactas. Ideal para optimizar.
    - Archivos Estáticos y de Plantillas usados.
    - Contexto de las Plantillas.
    - Señales (Signals).
    - Logs.
    - Caché.

### 2.6. Debuggers de IDE (PyCharm, VS Code, etc.) 🖱️💻
La mayoría de los Entornos de Desarrollo Integrado (IDEs) modernos vienen con depuradores visuales muy potentes.
- **Funcionamiento**: Permiten poner "breakpoints" (puntos de interrupción) en tu código haciendo clic al lado del número de línea. Cuando la ejecución llega ahí, se pausa.
- **Características**:
    - Ejecución paso a paso (next, step into, step over, step out).
    - Inspección visual de variables, el stack de llamadas.
    - Consola interactiva para evaluar expresiones en el contexto actual.
- **Configuración**: Cada IDE tiene su forma de configurar el debugger para un proyecto Django, pero suelen ser bastante directos (ej. configurar un "Django Server" run configuration).
- <mark style="background: #ABF7F7A6;">Para muchos, es la forma más cómoda y eficiente de depurar.</mark>
### 2.7. Escribir Pruebas (Tests) 🧪✅
Aunque no es una herramienta de depuración _en tiempo de ejecución_ de un error ya ocurrido, las pruebas son cruciales para:
- **Prevenir bugs**: Escribir pruebas para tus funciones y vistas ayuda a asegurar que hacen lo que esperas.
- **Localizar bugs**: Cuando un cambio en el código hace que una prueba que antes pasaba ahora falle, te da una pista muy clara de dónde se introdujo el problema.
- Django tiene un excelente framework de pruebas integrado (basado en `unittest` de Python). Usa `TestCase` y el `Client` de pruebas.
### 2.8. Django Shell y Shell Plus 🐚
- `python manage.py shell`: Te da un intérprete de Python con tu entorno Django cargado. Puedes importar tus modelos, hacer consultas ORM, probar lógica de negocio pequeña, etc.
- **`shell_plus` (de `django-extensions`)**: Una versión mejorada del shell.
    - Instala: `pip install django-extensions` y añade `'django_extensions'` a `INSTALLED_APPS`.
    - Usa: `python manage.py shell_plus`
    - Ventaja principal: <mark style="background: #FFB8EBA6;">¡Auto-importa todos tus modelos</mark> y algunas otras utilidades de Django! Súper cómodo.
## 3. Consejos Generales para la Depuración 💡
- **Lee los mensajes de error con atención**: El traceback es tu mejor amigo. Intenta entender qué te está diciendo.
- **Divide y vencerás**: Si no sabes dónde está el error, empieza a comentar secciones de tu código hasta que el error desaparezca. Luego, reintroduce las secciones una por una.
- **Verifica tus supuestos**: ¿Estás 100% seguro de que esa variable contiene el valor que crees que tiene en ese punto? Usa `print`, pdb o el debugger de tu IDE para confirmarlo.
- **Googlea el error**: Copia y pega el mensaje de error (especialmente la última línea del traceback) en Google. Es muy probable que alguien más haya tenido el mismo problema y encontrado una solución (Stack Overflow es tu destino frecuente).
- **Toma un descanso**: Si llevas mucho tiempo atascado, aléjate un rato. A veces, una mente fresca ve la solución que antes se te escapaba. ☕
- **Usa control de versiones (Git)**: Si un bug apareció de repente, revisa los últimos commits (`git log`, `git diff`). Puede que un cambio reciente sea el culpable.
## 4. Depuración en Producción (¡Con MUCHO Cuidado!) 🌍
- **`DEBUG = False` SIEMPRE**.
- **Logging es CLAVE**: Configura tu logging para que capture al menos `WARNING` y `ERROR` y los guarde en archivos o los envíe a un servicio centralizado.
- **Servicios de Monitoreo de Errores**: Considera usar herramientas como Sentry, Rollbar, Bugsnag, etc. Estas herramientas capturan las excepciones que ocurren en tu sitio en producción, te notifican, y te dan información detallada para analizarlas.
## 5. Mini Repaso Final ✨
- La **página de error amarilla de Django** (`DEBUG=True`) es tu primera parada en desarrollo.
- `print()` es rápido para verificaciones simples; `logging` es más robusto y para producción.
- **PDB** y los **debuggers de IDE** te dan control interactivo paso a paso.
- **Django Debug Toolbar** es una joya para inspeccionar peticiones, SQL, y más.
- Las **pruebas** no solo previenen bugs, sino que ayudan a identificar dónde se originan.
- La **shell de Django** (especialmente `shell_plus`) es genial para probar cosas pequeñas.