---
aliases:
  - Entendiendo las Relaciones entre Modelos en Django 🔗
tags:
  - django
  - modelos
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
---
# Entendiendo las Relaciones entre Modelos en Django 🔗
- [[#1. ¿Por Qué Necesitamos Relaciones? 🤔|1. ¿Por Qué Necesitamos Relaciones? 🤔]]
- [[#2. Relación Uno a Muchos (ForeignKey) 🧍➡️📚|2. Relación Uno a Muchos (ForeignKey) 🧍➡️📚]]
- [[#3. Relación Muchos a Muchos (ManyToManyField) 🧑‍🤝‍🧑`<->`🏷️|3. Relación Muchos a Muchos (ManyToManyField) 🧑‍🤝‍🧑`<->`🏷️]]
- [[#4. Relación Uno a Uno (OneToOneField) 👤<->💳|4. Relación Uno a Uno (OneToOneField) 👤<->💳]]
- [[#5. Elegir la Relación Correcta: ¡La Pregunta Clave! 🤔💭|5. Elegir la Relación Correcta: ¡La Pregunta Clave! 🤔💭]]
- [[#Mini Repaso Final 📝|Mini Repaso Final 📝]]

## 1. ¿Por Qué Necesitamos Relaciones? 🤔
- Imagina que tienes una app de blog. Tienes **Usuarios** que escriben **Artículos**, y esos **Artículos** pueden tener **Comentarios** y varias **Etiquetas**. ¡Todo está conectado!
- En el mundo real, los datos raramente existen de forma aislada.
    - Un **autor** escribe _varios_ **libros**.
    - Un **libro** puede tener _varios_ **géneros**.
    - Un **usuario** tiene _un solo_ **perfil detallado**.
- Las relaciones nos permiten:
    - **Conectar nuestros modelos** para representar estas conexiones lógicas de forma eficiente.
    - **Evitar la redundancia de datos** (no queremos escribir el nombre del autor en cada libro una y otra vez).
    - Mantener la **integridad de los datos** (que los datos sean consistentes y correctos).
    - Hacer **consultas más potentes y significativas** (como "tráeme todos los libros de este autor").
---
## 2. Relación Uno a Muchos (ForeignKey) 🧍➡️📚
Esta es, quizás, la relación más común que vas a usar.
- **Concepto**: Un objeto de un modelo (el lado "uno") puede estar relacionado con **muchos** objetos de otro modelo (el lado "muchos"). Pero, cada objeto del lado "muchos" solo puede estar relacionado con **un único** objeto del lado "uno".
    - **Ejemplo clásico**: Un `Autor` puede escribir muchos `Libros`. Pero un `Libro` específico es escrito por un solo `Autor`.
- **¿Cómo se define?**: Se usa `models.ForeignKey()` en el modelo que representa el lado "muchos" de la relación.
- **Ejemplo de Código**:
    ```python
    from django.db import models
    
    class Autor(models.Model): # El lado "uno"
        nombre = models.CharField(max_length=100)
        email = models.EmailField(unique=True)
    
        def __str__(self):
            return self.nombre
    
    class Articulo(models.Model): # El lado "muchos"
        titulo = models.CharField(max_length=200)
        contenido = models.TextField()
        fecha_publicacion = models.DateTimeField(auto_now_add=True)
        # ¡Aquí está la magia! Este artículo pertenece a UN Autor.
        autor = models.ForeignKey(
            Autor, # El modelo al que nos conectamos (el lado "uno")
            on_delete=models.CASCADE, # ¿Qué pasa si se borra el Autor?
            related_name='articulos' # Nombre para acceder desde Autor a sus Articulos
        )
    
        def __str__(self):
            return self.titulo
    ```
    
- **Parámetro Clave: `on_delete`**: ¡Este es súper importante! Le dice a Django qué hacer con los objetos del lado "muchos" (los `Articulos`) si el objeto del lado "uno" (el `Autor`) al que están conectados es eliminado.
    - `models.CASCADE`: ¡El más común! Si se borra el `Autor`, todos sus `Articulos` se borran también. 💣 ¡Efecto dominó!
    - `models.PROTECT`: Impide que se borre el `Autor` si tiene `Articulos` asociados. Django lanzará un error `ProtectedError`.
    - `models.SET_NULL`: Si el `Autor` se borra, el campo `autor` en sus `Articulos` se pondrá a `NULL`. Para esto, necesitas que el campo `ForeignKey` permita `null=True` (ej: `autor = models.ForeignKey(Autor, on_delete=models.SET_NULL, null=True)`).
    - `models.SET_DEFAULT`: Similar a `SET_NULL`, pero pone un valor por defecto que hayas definido. Necesitas `default=valor_por_defecto` en el `ForeignKey`.
    - `models.SET(valor_o_funcion)`: Permite establecer un valor específico o el resultado de una función.
    - `models.DO_NOTHING`: ¡No hace nada! Esto puede dejar tu base de datos en un estado inconsistente si no tienes cuidado. ¡Usar con mucha precaución! ⚠️
- **Parámetro `related_name`**:
    - Permite especificar el nombre que usarás para acceder a la relación desde el lado "uno" hacia el lado "muchos".
    - En nuestro ejemplo, desde un objeto `Autor`, puedes acceder a todos sus artículos usando `un_autor.articulos.all()`.
    - Si no lo pones, Django lo crea por defecto como `nombre_del_modelo_en_minusculas_set` (ej: `articulo_set`). ¡Pero `related_name` es más legible!
- **Accediendo a los datos relacionados**:
    - **Desde `Articulo` (lado "muchos") hacia `Autor` (lado "uno")**: Simplemente accedes al atributo.
        ```python
        un_articulo = Articulo.objects.get(pk=1)
        nombre_del_autor = un_articulo.autor.nombre
        email_del_autor = un_articulo.autor.email
        ```
    - **Desde `Autor` (lado "uno") hacia sus `Articulos` (lado "muchos")**: Usas el `related_name` (o el nombre por defecto `_set`).
        ```python
        un_autor = Autor.objects.get(email='autor_famoso@ejemplo.com')
        todos_sus_articulos = un_autor.articulos.all() # Gracias a related_name='articulos'
        # Si no hubiéramos usado related_name:
        # todos_sus_articulos = un_autor.articulo_set.all()
        
        for articulo_de_autor in todos_sus_articulos:
            print(articulo_de_autor.titulo)
        ```
---

## 3. Relación Muchos a Muchos (ManyToManyField) 🧑‍🤝‍🧑`<->`🏷️
Esta relación es para cuando las cosas se pueden mezclar más libremente.
- **Concepto**: Un objeto de un modelo puede estar relacionado con **varios** objetos de otro modelo, ¡y **viceversa**!
    - **Ejemplo**: Un `Articulo` puede tener muchas `Etiquetas` (tags como "Python", "Django", "Tutorial"). Y una `Etiqueta` (como "Django") puede estar asociada a muchos `Articulos`.
    - Otro ejemplo: Un `Estudiante` puede estar inscrito en muchos `Cursos`. Un `Curso` puede tener muchos `Estudiantes`.
- **¿Cómo se define?**: Usando `models.ManyToManyField()`. Puedes poner este campo en _cualquiera_ de los dos modelos que se relacionan. Django es lo suficientemente inteligente para crear una **tabla intermedia** especial en la base de datos para manejar esta relación. ¡Tú no la ves directamente en `models.py` (a menos que uses `through`) pero ahí está!
- **Ejemplo de Código**:
    ```python
    from django.db import models
    
    class Etiqueta(models.Model):
        nombre = models.CharField(max_length=50, unique=True)
    
        def __str__(self):
            return self.nombre
    
    class Publicacion(models.Model): # Podría ser nuestro Articulo de antes
        titular = models.CharField(max_length=100)
        contenido = models.TextField()
        # Definimos la relación muchos a muchos aquí:
        etiquetas = models.ManyToManyField(
            Etiqueta, # El modelo con el que se relaciona
            related_name='publicaciones' # Para acceder desde Etiqueta a sus Publicaciones
        )
        # También podrías haber puesto el ManyToManyField en Etiqueta, apuntando a Publicacion.
    
        def __str__(self):
            return self.titular
    ```
- **¿Dónde poner el `ManyToManyField`?**: Técnicamente da igual. A menudo se pone en el modelo que "agrupa" o parece más "principal" en la relación, o simplemente donde tenga más sentido para tu lógica.
- **Accediendo a los datos relacionados**: Se usa un "manager" especial que te permite agregar, quitar y consultar los objetos relacionados.
    - **Desde `Publicacion` a sus `Etiquetas`**:
        ```python
        una_publicacion = Publicacion.objects.get(pk=1)
        etiquetas_de_la_publicacion = una_publicacion.etiquetas.all()
        
        # Para añadir una etiqueta:
        nueva_etiqueta = Etiqueta.objects.get(nombre='Django Tips')
        una_publicacion.etiquetas.add(nueva_etiqueta)
        
        # Para quitar una etiqueta:
        etiqueta_a_quitar = Etiqueta.objects.get(nombre='Principiantes')
        una_publicacion.etiquetas.remove(etiqueta_a_quitar)
        
        # Para asignar un conjunto de etiquetas (reemplaza las existentes):
        # etiquetas_seleccionadas = Etiqueta.objects.filter(nombre__in=['Python', 'WebDev'])
        # una_publicacion.etiquetas.set(etiquetas_seleccionadas)
        
        # Para borrar todas las etiquetas de una publicación:
        # una_publicacion.etiquetas.clear()
        ```
    - **Desde `Etiqueta` a sus `Publicaciones`**: Usamos el `related_name`.
        ```python
        etiqueta_django = Etiqueta.objects.get(nombre='Django')
        publicaciones_con_esa_etiqueta = etiqueta_django.publicaciones.all() # Gracias a related_name
        ```
- **Parámetro `through` (Un poco más avanzado ✨)**:
    - A veces, necesitas guardar **información adicional sobre la relación misma**. Por ejemplo, en una relación entre `Estudiante` y `Curso`, podrías querer guardar la `fecha_inscripcion` o la `calificacion_obtenida`.
    - Para esto, puedes crear un **modelo intermedio personalizado** y decirle a Django que lo use con el parámetro `through='NombreDelModeloIntermedio'`.
    - Este modelo intermedio tendrá dos `ForeignKey` (uno a `Estudiante` y otro a `Curso`) y los campos adicionales que necesites.
---

## 4. Relación Uno a Uno (OneToOneField) 👤<->💳

Esta es la más "exclusiva" de las relaciones.
- **Concepto**: Es como un `ForeignKey` pero con la restricción de que un objeto de un modelo solo puede estar relacionado con **un único** objeto del otro modelo, y viceversa. ¡Es una pareja perfecta! 💍
    - **Ejemplo**: Un `Usuario` (el modelo `User` que viene con Django) puede tener un único `PerfilDeUsuario` (donde guardas info extra como biografía, foto, etc.). Y ese `PerfilDeUsuario` pertenece a un único `Usuario`.
    - Otro ejemplo: Un `Ciudadano` tiene un único `NumeroDeIdentificacionNacional`.
- **¿Cómo se define?**: Usando `models.OneToOneField()`. Puedes pensar en él como un `ForeignKey(unique=True)`, pero con semántica ligeramente diferente en cómo se accede a la relación inversa.
- **Ejemplo de Código**:
    ```python
    from django.db import models
    from django.contrib.auth.models import User # El modelo User que ya trae Django
    
    class Perfil(models.Model):
        # Aquí la relación uno a uno con el modelo User.
        usuario = models.OneToOneField(
            User,
            on_delete=models.CASCADE, # Si se borra el User, se borra su Perfil.
            primary_key=True # Opcional, pero común. Hace que el ID del Perfil sea el mismo que el del User.
                             # Esto también hace que el acceso inverso sea más directo.
        )
        bio = models.TextField(blank=True, null=True)
        web_personal = models.URLField(blank=True, null=True)
        fecha_nacimiento = models.DateField(null=True, blank=True)
    
        def __str__(self):
            return f"Perfil de {self.usuario.username}"
    ```
- **Parámetro Clave: `on_delete`**: Funciona igual que en `ForeignKey`. `models.CASCADE` es muy común aquí.
- **Parámetro `primary_key=True`**: Si lo usas, este campo `OneToOneField` se convertirá en la clave primaria del modelo `Perfil`. Es una práctica común cuando el modelo "extiende" a otro (como un perfil extendiendo al usuario).
- **Accediendo a los datos relacionados**:
    - **Desde `Perfil` al `Usuario`**:
        ```python
        un_perfil = Perfil.objects.get(pk=1) # O get(usuario__username='nombre_usuario')
        nombre_del_usuario = un_perfil.usuario.username
        ```
    - **Desde `Usuario` al `Perfil`**:
        - Si el `OneToOneField` en `Perfil` se llama `usuario`, Django crea un descriptor inverso en `User` llamado `perfil` (el nombre de la clase del modelo relacionado, en minúsculas). ¡Así de simple!
        - Si hubieras usado `related_name='mi_perfil_custom'` en el `OneToOneField`, entonces accederías con `un_usuario.mi_perfil_custom`
        ```python
        un_usuario = User.objects.get(username='pepito_grillo')
        try:
            bio_del_usuario = un_usuario.perfil.bio # Acceso directo (perfil es el nombre de la clase Perfil en minúscula)
            print(f"Bio de {un_usuario.username}: {bio_del_usuario}")
        except Perfil.DoesNotExist: # Importante manejar esto, ya que un User podría no tener Perfil aún
            print(f"{un_usuario.username} aún no tiene un perfil creado.")
        ```
        

---

## 5. Elegir la Relación Correcta: ¡La Pregunta Clave! 🤔💭

Para saber qué relación usar, hazte estas preguntas sobre tus modelos (llamémoslos A y B):
1. ¿Puede una instancia de **A** estar relacionada con **muchas** instancias de **B**?
2. ¿Puede una instancia de **B** estar relacionada con **muchas** instancias de **A**?

| **Pregunta 1 (A -> muchos B)** | **Pregunta 2 (B -> muchos A)** | **Tipo de Relación**                  | **Ejemplo**                            | **Dónde poner el campo**                                                |
| ------------------------------ | ------------------------------ | ------------------------------------- | -------------------------------------- | ----------------------------------------------------------------------- |
| ✅ Sí                           | ❌ No                           | **Uno a Muchos (ForeignKey)**         | `Autor` (A) tiene muchos `Libros` (B)  | `ForeignKey` en `Libro` (B)                                             |
| ❌ No                           | ✅ Sí                           | **Uno a Muchos (ForeignKey)**         | `Libro` (A) pertenece a un `Autor` (B) | `ForeignKey` en `Libro` (A)                                             |
| ✅ Sí                           | ✅ Sí                           | **Muchos a Muchos (ManyToManyField)** | `Estudiante` (A) y `Curso` (B)         | `ManyToManyField` en A o B                                              |
| ❌ No                           | ❌ No                           | **Uno a Uno (OneToOneField)**         | `Usuario` (A) y `PerfilDetallado` (B)  | `OneToOneField` en A o B (usualmente en el "dependiente" o "extensión") |

---

## Mini Repaso Final 📝

¡Las relaciones son el pegamento de tus datos en Django!

- **`ForeignKey` (Uno a Muchos)**: Un papá (`Autor`) tiene muchos hijos (`Articulo`). Pones el `ForeignKey` en el modelo "hijo" apuntando al "papá". No olvides `on_delete` y `related_name`.
- **`ManyToManyField` (Muchos a Muchos)**: Amigos que comparten muchos gustos (`Publicacion` y `Etiqueta`). Django crea una tabla mágica para unirlos. Puedes poner el campo en cualquiera de los dos modelos. `related_name` es útil. Considera `through` para info extra de la relación.
- **`OneToOneField` (Uno a Uno)**: Almas gemelas (`User` y `Perfil`). Cada uno tiene solo uno del otro. `on_delete` también aplica. `primary_key=True` es una opción interesante.