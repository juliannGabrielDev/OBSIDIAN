---
aliases:
  - Un Vistazo a los Modelos en Django 🧩💡
tags:
  - django
  - modelos
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
---
# Un Vistazo a los Modelos en Django 🧩💡
- [[#1. ¿Qué Son los Modelos en Django? 🤔|1. ¿Qué Son los Modelos en Django? 🤔]]
- [[#2. ¿Para Qué Sirven? 🎯|2. ¿Para Qué Sirven? 🎯]]
- [[#3. Creando un Modelo: ¡Manos a la Obra! ✍️|3. Creando un Modelo: ¡Manos a la Obra! ✍️]]
- [[#4. Tipos de Campos Comunes 🏷️|4. Tipos de Campos Comunes 🏷️]]
- [[#5. El Método `__str__()` 🗣️|5. El Método `__str__()` 🗣️]]
- [[#6. Migraciones: ¡Sincronizando Modelos y Base de Datos! 🔄|6. Migraciones: ¡Sincronizando Modelos y Base de Datos! 🔄]]
- [[#7. Interactuando con los Datos (QuerySets) 🔎|7. Interactuando con los Datos (QuerySets) 🔎]]
- [[#8. Registrando Modelos en el Admin ⚙️|8. Registrando Modelos en el Admin ⚙️]]
- [[#Mini Repaso Final 📝|Mini Repaso Final 📝]]

## 1. ¿Qué Son los Modelos en Django? 🤔
- Son la **fuente única y definitiva de información** sobre tus datos. ¡Como la chuleta principal de tu base de datos!
- Básicamente, un modelo es una **clase de Python** que representa una **tabla** en tu base de datos.
- Cada **atributo** de esa clase se convierte en un **campo** (o columna) en la tabla de la base de datos.
- Los modelos son la "M" en la arquitectura **MTV (Modelo-Plantilla-Vista)** de Django. Son el corazón donde residen y se estructuran los datos.
---
## 2. ¿Para Qué Sirven? 🎯
- **Definir la estructura de los datos**: Cómo se van a ver, qué tipo de información guardarán (texto, números, fechas, etc.).
- **Interactuar con la base de datos**: Django trae algo llamado **ORM (Object-Relational Mapper)**. ¡Es magia! ✨ Te permite hablar con tu base de datos usando código Python en lugar de SQL puro. Así, puedes crear, leer, actualizar y borrar datos de forma más intuitiva.
- **Validar datos**: Se aseguran de que la información que intentas guardar tenga sentido (por ejemplo, que un email parezca un email).
- **Establecer relaciones**: Permiten conectar diferentes tablas/modelos. Por ejemplo, un `Usuario` puede tener muchos `Posts`.
## 3. Creando un Modelo: ¡Manos a la Obra! ✍️
- Los modelos se definen en un archivo llamado `models.py` que está dentro de cada "app" de Django que creas.
- Cada modelo es una clase que hereda de `django.db.models.Model`.
- Dentro de la clase, defines los campos que tendrá tu tabla.
    - **Ejemplo Sencillito:** Vamos a imaginar que estamos creando una app para una biblioteca. Necesitaríamos un modelo para los `Libros` y otro para los `Autores`.
        ```python
        from django.db import models
        from django.utils import timezone # Para manejar fechas y horas
        
        class Autor(models.Model):
            nombre = models.CharField(max_length=100)
            fecha_nacimiento = models.DateField(null=True, blank=True) # Puede ser opcional
        
            def __str__(self):
                return self.nombre
        
        class Libro(models.Model):
            titulo = models.CharField(max_length=200)
            # Aquí viene la relación: un libro tiene un autor.
            # on_delete=models.CASCADE significa que si borras un autor, sus libros también se borran.
            autor = models.ForeignKey(Autor, on_delete=models.CASCADE)
            fecha_publicacion = models.DateField()
            isbn = models.CharField(max_length=13, unique=True) # ISBN debe ser único
            resumen = models.TextField(blank=True) # Un resumen largo, puede estar vacío
        
            def __str__(self):
                return f"{self.titulo} por {self.autor.nombre}"
        ```
## 4. Tipos de Campos Comunes 🏷️
Django ofrece un montón de tipos de campos para definir tus datos. ¡Aquí van algunos de los más usados!

| **Tipo de Campo** | **Descripción**                                                                            | **Ejemplo en Python (models.py)**                                       |
| ----------------- | ------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------- |
| `CharField`       | Para cadenas de texto cortas (títulos, nombres). Requiere `max_length`.                    | `titulo = models.CharField(max_length=200)`                             |
| `TextField`       | Para cadenas de texto largas (descripciones, contenido de un post).                        | `contenido = models.TextField()`                                        |
| `IntegerField`    | Para números enteros (cantidad, edad).                                                     | `paginas = models.IntegerField()`                                       |
| `FloatField`      | Para números decimales (precio).                                                           | `precio = models.FloatField()`                                          |
| `BooleanField`    | Para valores verdadero/falso (sí/no, activo/inactivo).                                     | `disponible = models.BooleanField(default=True)`                        |
| `DateField`       | Para fechas (día, mes, año).                                                               | `fecha_nacimiento = models.DateField()`                                 |
| `DateTimeField`   | Para fechas y horas.                                                                       | `creado_el = models.DateTimeField(auto_now_add=True)`                   |
| `EmailField`      | Para direcciones de correo electrónico. Valida el formato.                                 | `email = models.EmailField()`                                           |
| `URLField`        | Para URLs. Valida el formato.                                                              | `sitio_web = models.URLField()`                                         |
| `FileField`       | Para subir archivos.                                                                       | `portada = models.FileField(upload_to='portadas/')`                     |
| `ImageField`      | Para subir imágenes (requiere Pillow).                                                     | `avatar = models.ImageField(upload_to='avatars/')`                      |
| `ForeignKey`      | Relación "uno a muchos" (un autor tiene muchos libros). Necesita `on_delete`.              | `autor = models.ForeignKey(Autor, on_delete=models.CASCADE)`            |
| `ManyToManyField` | Relación "muchos a muchos" (un libro puede tener muchos géneros, un género muchos libros). | `generos = models.ManyToManyField(Genero)`                              |
| `OneToOneField`   | Relación "uno a uno" (un usuario tiene un solo perfil).                                    | `perfil_usuario = models.OneToOneField(User, on_delete=models.CASCADE)` |

- **Opciones de Campo importantes:**
    - `max_length`: Para `CharField`, define la longitud máxima.
    - `null=True`: Permite que el campo sea `NULL` en la base de datos (Python usa `None`).
    - `blank=True`: Permite que el campo esté vacío en formularios Django. (Relacionado con la validación, no con la BD directamente).
    - `default`: Un valor por defecto si no se proporciona uno.
    - `unique=True`: Asegura que el valor de este campo sea único en toda la tabla.
    - `auto_now_add=True`: (Para `DateField` o `DateTimeField`) Guarda la fecha/hora actual cuando el objeto se crea por primera vez. ¡No se puede modificar después!
    - `auto_now=True`: (Para `DateField` o `DateTimeField`) Actualiza la fecha/hora cada vez que el objeto se guarda (`.save()`).
    - `on_delete`: (Para `ForeignKey` y `OneToOneField`) Qué hacer si el objeto referenciado se borra. Las opciones comunes son:
        - `models.CASCADE`: Borra también este objeto. (Ej: Si borras un Autor, se borran sus Libros).
        - `models.PROTECT`: Impide borrar el objeto referenciado si hay objetos que dependen de él.
        - `models.SET_NULL`: Pone el campo a `NULL` (requiere `null=True` en el campo).
        - `models.SET_DEFAULT`: Pone el campo a su valor `default` (requiere que `default` esté definido).
## 5. El Método `__str__()` 🗣️

- ¡Este método es tu amigo! Es súper importante definirlo en cada modelo.
- Django usa `__str__(self)` para mostrar una **representación legible** de un objeto del modelo.
- Por ejemplo, si tienes una lista de `Libros` en el panel de administración de Django, en lugar de ver algo como `<Libro object (1)>`, verás lo que devuelva tu método `__str__()`.
    ```python
    class Autor(models.Model):
        nombre = models.CharField(max_length=100)
        # ... otros campos
    
        def __str__(self):
            return self.nombre # Así, un objeto Autor se mostrará por su nombre.
    ```
## 6. Migraciones: ¡Sincronizando Modelos y Base de Datos! 🔄
![[migraciones#3. El Flujo de Trabajo Básico de las Migraciones ♻️]]
## 7. Interactuando con los Datos (QuerySets) 🔎
Una vez que tus modelos están definidos y migrados, ¡puedes empezar a jugar con los datos! Django te da una API para esto llamada **QuerySet**. Es como tu herramienta para hacer consultas.
- **Acceder al "Manager"**: Cada modelo tiene un atributo `objects` (llamado "manager") que es la puerta de entrada a los QuerySets.
- **Ejemplos básicos (CRUD):**
    - **Crear un objeto (Create):**
        ```python
        # Opción 1
        nuevo_autor = Autor.objects.create(nombre="Gabriel García Márquez", fecha_nacimiento="1927-03-06")
        
        # Opción 2
        otro_autor = Autor(nombre="Isabel Allende")
        otro_autor.fecha_nacimiento = "1942-08-02"
        otro_autor.save()
        ```
    - **Leer objetos (Read):**
        ```python
        # Obtener todos los autores
        todos_los_autores = Autor.objects.all()
        for autor in todos_los_autores:
            print(autor.nombre)
        
        # Obtener un autor específico por su ID (pk es primary key)
        autor_gabo = Autor.objects.get(pk=1)
        # O por otro campo único
        # autor_gabo = Autor.objects.get(nombre="Gabriel García Márquez") # ¡Cuidado si no es único!
        
        # Filtrar autores
        autores_nacidos_despues_1940 = Autor.objects.filter(fecha_nacimiento__year__gt=1940) # __gt es "greater than"
        autores_con_a = Autor.objects.filter(nombre__startswith="A")
        ```
    - **Actualizar un objeto (Update):**
        ```python
        autor_a_actualizar = Autor.objects.get(nombre="Isabel Allende")
        autor_a_actualizar.fecha_nacimiento = "1942-08-03" # Corregimos la fecha
        autor_a_actualizar.save()
        
        # Actualizar múltiples objetos a la vez
        # Autor.objects.filter(nombre__contains="Pérez").update(campo_a_modificar="nuevo valor")
        ```
    - **Eliminar un objeto (Delete):**
        ```python
        autor_a_eliminar = Autor.objects.get(nombre="Autor Temporal")
        autor_a_eliminar.delete()
        ```
## 8. Registrando Modelos en el Admin ⚙️
Django viene con un panel de administración súper útil ¡y gratis! Para que tus modelos aparezcan ahí y puedas gestionarlos fácilmente (añadir, editar, borrar datos a través de una interfaz web), necesitas registrarlos.
- Esto se hace en el archivo `admin.py` de tu app.
    ```python
    from django.contrib import admin
    from .models import Autor, Libro # Importas tus modelos
    
    # Registras los modelos
    admin.site.register(Autor)
    admin.site.register(Libro)
    ```
    ¡Y listo! Ahora podrás ver y manipular tus Autores y Libros desde `tu_sitio.com/admin`.
---
## Mini Repaso Final 📝

¡Uf, eso fue bastante! Pero los modelos son la base. Aquí va un resumen rápido:
- Los **modelos** son clases de Python que representan **tablas** en tu base de datos. Definen **campos** (columnas) y sus tipos.
- El **ORM** de Django te deja usar Python para hablar con la base de datos. ¡Adiós SQL (casi siempre)!
- El método `__str__()` es clave para que tus objetos se muestren de forma amigable.
- Las **migraciones** (`makemigrations` y `migrate`) son esenciales para mantener tu base de datos sincronizada con tus modelos.
- Los **QuerySets** (`.objects.all()`, `.filter()`, `.get()`, etc.) son tu forma de hacer consultas y manipular datos.
- Registra tus modelos en `admin.py` para tener una interfaz de administración genial.
¡Dominar los modelos es como tener superpoderes para manejar datos en Django! 💪