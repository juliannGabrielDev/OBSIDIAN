---
aliases:
  - Probando Aplicaciones Django 🧪
tags:
  - django
  - pruebas
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
Fecha: 2025-06-04
---
# Probando Aplicaciones Django 🧪: ¡Asegurando que Nuestro Código Sea a Prueba de Balas! 🛡️

## 1. ¿Por Qué Son Tan Importantes las Pruebas (Tests)? 🤔
Escribir pruebas para tu código tiene un montón de ventajas:
- <mark style="background: #BBFABBA6;">**Confianza en tu Código**</mark>: Te aseguras de que tus funciones, modelos, vistas, etc., hacen lo que esperas que hagan.
- <mark style="background: #ADCCFFA6;">**Prevención de Regresiones**</mark>: Evitan que cambios nuevos o refactorizaciones rompan funcionalidades que ya estaban bien. Si un test que antes pasaba ahora falla, ¡sabes que algo se rompió!
- <mark style="background: #FFF3A3A6;">**Facilitan la Refactorización y el Mantenimiento**</mark>: Puedes cambiar y mejorar tu código con más seguridad, sabiendo que los tests te avisarán si introduces un error.
- <mark style="background: #ABF7F7A6;">**Documentación Viva**</mark>: Los tests son ejemplos de cómo se debe usar tu código. Otros desarrolladores (¡o tu yo del futuro!) pueden leer los tests para entender cómo funciona una parte de la aplicación.
- **Desarrollo Más Rápido (a Largo Plazo)**: Aunque escribir tests toma tiempo al inicio, a la larga ahorras mucho tiempo de depuración manual.
## 2. El Framework de Pruebas de Django 🖼️
Django viene con un sistema de pruebas incorporado, que está basado en el módulo `unittest` de la biblioteca estándar de Python. Además, Django añade herramientas específicas muy útiles, como un **cliente de pruebas** para simular peticiones web.
**Importante**: Cuando ejecutas tus pruebas, Django crea una **base de datos de prueba separada y temporal**. Esto asegura que tus pruebas no afecten tu base de datos de desarrollo real y que cada ejecución de tests comience desde un estado limpio. ¡Genial!
## 3. Escribiendo Nuestra Primera Prueba: Estructura Básica 🏗️
### a. ¿Dónde Van las Pruebas?
- Por defecto, Django buscará pruebas en un archivo llamado `tests.py` dentro de cada una de tus aplicaciones.
- También puedes crear un directorio llamado `tests` dentro de tu app y poner ahí varios archivos que empiecen con `test_` (ej. `test_models.py`, `test_views.py`). Esto es bueno para organizar mejor si tienes muchos tests.
### b. La Clase `TestCase`
Tus clases de prueba deben heredar de `django.test.TestCase`. Esta clase base proporciona un montón de funcionalidades útiles.
```python
# mi_app/tests.py
from django.test import TestCase
from .models import MiModelo # Suponiendo que tienes un modelo

class MisPruebasDeModelo(TestCase):
    # Aquí irán tus métodos de prueba
    pass
```
### c. Métodos de Prueba
- Dentro de tu clase `TestCase`, cada método que deba ejecutarse como una prueba debe **empezar con el prefijo `test_`**.
- Ejemplo: `def test_mi_funcionalidad_especifica(self):`
- Dentro de cada método de prueba, realizarás algunas acciones (ej. crear un objeto, llamar a una función, hacer una petición a una vista) y luego harás **aserciones** para verificar que el resultado es el esperado.
### d. Aserciones Comunes (`self.assert...`)
Las aserciones son la forma de decirle al framework de pruebas: "Espero que esto sea verdad". Si una aserción falla, el test falla. `TestCase` te da muchos métodos de aserción:
- `self.assertEqual(a, b)`: Verifica que `a` sea igual a `b`.
- `self.assertNotEqual(a, b)`: Verifica que `a` no sea igual a `b`.
- `self.assertTrue(x)`: Verifica que `x` sea `True`.
- `self.assertFalse(x)`: Verifica que `x` sea `False`.
- `self.assertIsNone(x)` / `self.assertIsNotNone(x)`: Verifica si `x` es `None` o no.
- `self.assertIn(miembro, contenedor)`: Verifica que `miembro` esté en `contenedor`.
- `self.assertNotIn(miembro, contenedor)`: Verifica que `miembro` no esté en `contenedor`.
- `self.assertRaises(ExcepcionEsperada, funcion_a_llamar, arg1, arg2, ...)`: Verifica que llamar a `funcion_a_llamar` con esos argumentos levante la `ExcepcionEsperada`.
Y para pruebas de vistas, Django añade más aserciones útiles a través del objeto `response` que obtienes del cliente de pruebas:
- `self.assertContains(response, "texto_esperado", status_code=200)`: Verifica que la respuesta contenga "texto_esperado" y opcionalmente que el código de estado sea 200.
- `self.assertNotContains(response, "texto_no_deseado")`.
- `self.assertTemplateUsed(response, "nombre_plantilla.html")`: Verifica que se usó la plantilla correcta.
- `self.assertRedirects(response, "/url_esperada/", status_code=302)`: Verifica que la respuesta sea una redirección a la URL esperada.
## 4. Probando Diferentes Partes de Django 🎯
### 4.1. Probando Modelos (`models.py`)
**Propósito**: Asegurar que tus modelos se comportan como deben: valores por defecto, métodos personalizados, validaciones a nivel de modelo, relaciones, etc.
```python
# mi_app/models.py
from django.db import models
class Articulo(models.Model):
    titulo = models.CharField(max_length=100)
    contenido = models.TextField()
    publicado = models.BooleanField(default=False)

    def __str__(self):
        return self.titulo

    def get_resumen(self):
        return self.contenido[:50] + "..." if len(self.contenido) > 50 else self.contenido

# mi_app/tests.py
from django.test import TestCase
from .models import Articulo

class ArticuloModelTests(TestCase):
    def test_creacion_articulo(self):
        articulo = Articulo.objects.create(titulo="Mi Primer Artículo", contenido="Este es el contenido.")
        self.assertEqual(str(articulo), "Mi Primer Artículo")
        self.assertFalse(articulo.publicado) # Verifica el valor por defecto

    def test_metodo_get_resumen(self):
        contenido_largo = "Este es un contenido muy largo que definitivamente excederá los cincuenta caracteres."
        articulo = Articulo(titulo="Largo", contenido=contenido_largo)
        self.assertEqual(articulo.get_resumen(), "Este es un contenido muy largo que definitivament...")
        
        articulo_corto = Articulo(titulo="Corto", contenido="Corto.")
        self.assertEqual(articulo_corto.get_resumen(), "Corto.")
```
### 4.2. Probando Vistas (`views.py`)
**El Cliente de Pruebas (`self.client`)**: `TestCase` te da `self.client`, un objeto que puede simular peticiones HTTP GET y POST a tus URLs. ¡Es la herramienta principal para probar vistas!
```python
# mi_app/views.py (ejemplo simple)
from django.shortcuts import render
from .models import Articulo
def lista_articulos_publicados(request):
    articulos = Articulo.objects.filter(publicado=True)
    return render(request, 'mi_app/lista_articulos.html', {'articulos_ctx': articulos})

# mi_app/urls.py (ejemplo)
from django.urls import path
from . import views
urlpatterns = [path('articulos-publicados/', views.lista_articulos_publicados, name='articulos_publicados_lista')]

# mi_app/tests.py
from django.test import TestCase
from django.urls import reverse # ¡Útil para no hardcodear URLs!
from .models import Articulo

class VistasArticuloTests(TestCase):
    @classmethod
    def setUpTestData(cls):
        # Crear datos una vez para toda la clase de tests (más eficiente)
        Articulo.objects.create(titulo="Artículo Publicado", contenido="Contenido P.", publicado=True)
        Articulo.objects.create(titulo="Artículo Borrador", contenido="Contenido B.", publicado=False)

    def test_vista_lista_articulos_publicados_accesible(self):
        url = reverse('articulos_publicados_lista') # Obtiene la URL por su nombre
        response = self.client.get(url)
        self.assertEqual(response.status_code, 200) # ¿Respondió OK?
        self.assertTemplateUsed(response, 'mi_app/lista_articulos.html') # ¿Usó la plantilla correcta?

    def test_vista_lista_solo_muestra_publicados(self):
        response = self.client.get(reverse('articulos_publicados_lista'))
        self.assertContains(response, "Artículo Publicado") # ¿Está el publicado?
        self.assertNotContains(response, "Artículo Borrador") # ¿No está el borrador?
        self.assertEqual(len(response.context['articulos_ctx']), 1) # ¿Solo hay uno en el contexto?
        
    def test_crear_articulo_via_post(self): # Suponiendo que tienes una vista POST para crear
        url_crear = reverse('url_crear_articulo') # Necesitas una URL para esto
        datos_articulo = {'titulo': 'Nuevo desde Test', 'contenido': 'Test POST'}
        
        # Antes de POST, no existe
        self.assertFalse(Articulo.objects.filter(titulo='Nuevo desde Test').exists())
        
        response = self.client.post(url_crear, datos_articulo)
        
        self.assertEqual(response.status_code, 302) # Esperamos redirección tras éxito
        # O self.assertEqual(response.status_code, 201) si la API devuelve Creado
        
        # Después de POST, sí existe
        self.assertTrue(Articulo.objects.filter(titulo='Nuevo desde Test').exists())
```
### 4.3. Probando Formularios (`forms.py`)
**Propósito**: Asegurar que tu formulario valida los datos correctamente, muestra los errores adecuados y limpia los datos como se espera.
```python
# mi_app/forms.py (ejemplo)
from django import forms
class ContactoForm(forms.Form):
    nombre = forms.CharField(max_length=100)
    email = forms.EmailField()
    mensaje = forms.CharField(widget=forms.Textarea, min_length=10)

# mi_app/tests.py
from django.test import TestCase
from .forms import ContactoForm

class ContactoFormTests(TestCase):
    def test_formulario_contacto_valido(self):
        datos = {'nombre': 'Ana', 'email': 'ana@example.com', 'mensaje': 'Este es un mensaje de prueba largo.'}
        form = ContactoForm(data=datos)
        self.assertTrue(form.is_valid())

    def test_formulario_contacto_mensaje_corto_invalido(self):
        datos = {'nombre': 'Luis', 'email': 'luis@example.com', 'mensaje': 'Corto'}
        form = ContactoForm(data=datos)
        self.assertFalse(form.is_valid())
        self.assertIn('mensaje', form.errors) # ¿Hay un error en el campo 'mensaje'?
        # Puedes ser más específico sobre el mensaje de error si quieres
        # self.assertTrue("Ensure this value has at least 10 characters" in form.errors['mensaje'][0])

    def test_formulario_contacto_email_invalido(self):
        datos = {'nombre': 'Sara', 'email': 'sara_no_es_email', 'mensaje': 'Otro mensaje de prueba largo.'}
        form = ContactoForm(data=datos)
        self.assertFalse(form.is_valid())
        self.assertIn('email', form.errors)
```
## 5. Ejecutando las Pruebas 🏃💨
Abres tu terminal en la raíz de tu proyecto Django y usas:
- **`python manage.py test`**: Ejecuta TODOS los tests en TODAS las apps de tu proyecto que estén en `INSTALLED_APPS`.
- **`python manage.py test nombre_de_la_app`**: Ejecuta solo los tests de la app especificada.
    - Ejemplo: `python manage.py test blog`
- **`python manage.py test nombre_de_la_app.NombreClaseTests`**: Ejecuta solo los tests de una clase específica.
    - Ejemplo: `python manage.py test blog.ArticuloModelTests`
- **`python manage.py test nombre_de_la_app.NombreClaseTests.test_metodo_especifico`**: Ejecuta solo un método de test.
    - Ejemplo: `python manage.py test blog.ArticuloModelTests.test_creacion_articulo`
**Salida Típica:**
Verás una serie de puntos (.) por cada test que pasa, F por cada fallo (la aserción no se cumplió) y E por cada error (una excepción inesperada ocurrió durante el test). Al final, te da un resumen.
Cobertura de Pruebas (Coverage):
Es útil saber qué porcentaje de tu código está siendo "tocado" por tus pruebas.
1. Instala `coverage`: `pip install coverage`
2. Ejecuta tests con coverage: `coverage run manage.py test`
3. Ve el reporte en consola: `coverage report`
4. Genera un reporte HTML (¡muy visual!): `coverage html` (luego abre `htmlcov/index.html` en tu navegador).
## 6. Manejo de Datos de Prueba (`setUp`, `setUpTestData`, Fixtures) 🍽️
A veces tus tests necesitan datos preexistentes en la base de datos de prueba.
- **`setUp(self)`**: Este método se ejecuta **antes de cada método de prueba** (`test_...`) dentro de la clase `TestCase`. Úsalo si cada test necesita su propio conjunto de objetos frescos o si los tests modifican los objetos.
- **`setUpTestData(cls)`**: Este es un método de clase (`@classmethod`) y se ejecuta **una sola vez por cada clase `TestCase`**, antes que cualquier `setUp` o test. Es <mark style="background: #ADCCFFA6;">mucho más eficiente</mark> para crear objetos que no serán modificados por los tests (ej. datos de referencia).
    ```python
    class MisTestsConDatos(TestCase):
        @classmethod
        def setUpTestData(cls):
            cls.autor_comun = Autor.objects.create(nombre="Autor Famoso")
            print("setUpTestData llamado!") # Se llama una vez
    
        def setUp(self):
            # Esto se llamaría antes de test_uno y antes de test_dos
            self.articulo_test = Articulo.objects.create(titulo="Test", autor=self.autor_comun)
            print("setUp llamado!") 
    
        def test_uno(self):
            self.assertEqual(self.articulo_test.autor.nombre, "Autor Famoso")
    
        def test_dos(self):
            self.articulo_test.titulo = "Cambiado"
            self.articulo_test.save()
            self.assertEqual(self.articulo_test.titulo, "Cambiado")
    ```
- **Fixtures de Django**: Puedes cargar datos desde archivos JSON, YAML o XML a tu base de datos de prueba usando el atributo `fixtures` en tu `TestCase`. Ejemplo: `fixtures = ['mi_app/fixtures/datos_iniciales.json']`. (Es una opción, pero `setUpTestData` suele ser más flexible para muchos casos).
## 7. Buenas Prácticas y Consejos 🌟

- **Pruebas Independientes**: Cada test debe ser autónomo y no depender del resultado o estado dejado por otro.
- **Un Test, Una Cosa**: Intenta que cada método de prueba verifique una sola pieza de funcionalidad.
- **Nombres Descriptivos**: Nombra tus tests de forma que sea obvio qué están probando (ej. `def test_usuario_no_autenticado_es_redirigido_a_login(self):`).
- **Prueba Temprano y a Menudo**: Lo ideal es escribir tests a medida que desarrollas (o incluso antes, si sigues TDD - Test-Driven Development). ¡Y ejecútalos con frecuencia!
- **Mantén las Pruebas Rápidas**: Si los tests tardan mucho, te dará pereza ejecutarlos.
- **Cubre Casos Límite**: No solo pruebes el "camino feliz". ¿Qué pasa con datos vacíos, números negativos, entradas incorrectas, etc.?
## 8. Mini Repaso Final 💡
- Las pruebas son VITALES para la **calidad, confianza y mantenibilidad** de tu código Django.
- Django usa el framework `unittest` y extiende `TestCase` con herramientas útiles (como `self.client`).
- Debes probar tus **modelos** (lógica y datos), **vistas** (respuestas, plantillas, contexto, lógica de peticiones) y **formularios** (validación).
- Ejecuta tus pruebas con `python manage.py test` y sus variantes.
- Usa `setUpTestData` para datos de prueba eficientes.
- ¡Escribe buenos tests y dormirás mejor! 😴👍