---
aliases:
  - Explorando los QuerySets 🕵️‍♂️
tags:
  - django
  - modelos
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
---
# Explorando los QuerySets de Django 🕵️‍♂️ ¡Nuestra Herramienta para Hablar con la Base de Datos!
- [[#1. ¿Qué Es Exactamente un QuerySet? 🤔|1. ¿Qué Es Exactamente un QuerySet? 🤔]]
- [[#2. ¿Cómo Obtenemos un QuerySet? 🎣|2. ¿Cómo Obtenemos un QuerySet? 🎣]]
- [[#3. La Pereza de los QuerySets (Lazy Evaluation) 😴|3. La Pereza de los QuerySets (Lazy Evaluation) 😴]]
- [[#4. Filtrando y Encadenando QuerySets 🔗|4. Filtrando y Encadenando QuerySets 🔗]]
- [[#5. Métodos que NO Devuelven QuerySets (y Ejecutan la Consulta) 💥|5. Métodos que NO Devuelven QuerySets (y Ejecutan la Consulta) 💥]]
- [[#6. QuerySets y el Acceso a Datos Relacionados 🧑‍🤝‍🧑|6. QuerySets y el Acceso a Datos Relacionados 🧑‍🤝‍🧑]]
- [[#7. Slicing y Limitando QuerySets 🍰|7. Slicing y Limitando QuerySets 🍰]]
- [[#8. ¿Cuándo NO Usar `update()` Masivo para Modificar? 🧐|8. ¿Cuándo NO Usar `update()` Masivo para Modificar? 🧐]]
- [[#Mini Repaso Final 📝|Mini Repaso Final 📝]]

---

## 1. ¿Qué Es Exactamente un QuerySet? 🤔

- Un **QuerySet** es, en esencia, una **colección de objetos (o filas) de tu base de datos** que coinciden con ciertos criterios.
- **No es una lista de Python directamente**, aunque en muchos casos se comporta de forma similar (puedes iterar sobre él, hacerle slicing, etc.).
- Representa una consulta a la base de datos. Puedes construir esta consulta paso a paso.
- Lo más importante: los QuerySets son **"perezosos" (lazy)**. Esto significa que Django no ejecuta la consulta a la base de datos hasta que es absolutamente necesario. ¡Hablaremos más de esto porque es genial! ✨
- Se obtienen a través del **Manager** de un modelo (generalmente, `NombreModelo.objects`).

---

## 2. ¿Cómo Obtenemos un QuerySet? 🎣

La forma más común de obtener un QuerySet es usando el "manager" de tu modelo. Por defecto, cada modelo tiene un manager llamado `objects`.

Aquí algunos de los métodos más comunes que devuelven QuerySets (y que por lo tanto, puedes seguir encadenando):

- **`all()`**: Devuelve un QuerySet con **todos los objetos** del modelo.
    ```python
    todos_los_articulos = Articulo.objects.all()
    ```
    
- `filter(**kwargs)`: Devuelve un QuerySet con los objetos que **cumplen ciertas condiciones** (filtros).

    ```python
    articulos_de_ana = Articulo.objects.filter(autor__nombre='Ana')
    articulos_recientes_de_ana = Articulo.objects.filter(autor__nombre='Ana', estado='publicado')
    ```
    
- `exclude(**kwargs)`: Devuelve un QuerySet con los objetos que **NO cumplen** ciertas condiciones.
    ```python
    articulos_no_borradores = Articulo.objects.exclude(estado='borrador')
    ```
    
- `order_by(*fields)`: Devuelve un QuerySet con los objetos **ordenados** según los campos que le pases.
    ```python
    articulos_por_fecha = Articulo.objects.order_by('fecha_publicacion') # Ascendente
    articulos_por_fecha_desc = Articulo.objects.order_by('-fecha_publicacion') # Descendente
    articulos_por_autor_y_fecha = Articulo.objects.order_by('autor__nombre', '-fecha_publicacion')
    ```
    
- **`distinct(*fields)`**: (Principalmente útil con PostgreSQL) Devuelve un QuerySet donde se eliminan las filas duplicadas según los campos especificados. Si no se especifican campos, depende de la base de datos cómo maneje la distinción de toda la fila.
    ```python
    # Si tienes varios artículos con el mismo título pero diferente ID, .distinct('titulo') podría ayudar
    # titulos_unicos = Articulo.objects.distinct('titulo')
    ```
    
- **Otros métodos avanzados**: `annotate()`, `values()`, `values_list()`, `dates()`, `select_related()`, `prefetch_related()` también devuelven QuerySets (o variantes de ellos).

---

## 3. La Pereza de los QuerySets (Lazy Evaluation) 😴

¡Este es un concepto clave! Cuando escribes algo como:
```python
articulos_publicados = Articulo.objects.filter(estado='publicado')
```

...**¡Django NO ejecuta ninguna consulta a la base de datos todavía!** Simplemente ha creado un objeto QuerySet que _representa_ esa consulta.

La consulta SQL real se ejecuta contra la base de datos solo cuando el QuerySet se **"evalúa"**. ¿Cuándo ocurre esto?

- **Al iterar sobre él**:
    ```python
    for articulo in articulos_publicados: # ¡Ahora sí, consulta a la BD!
        print(articulo.titulo)
    ```
    
- **Al convertirlo a una lista**: `lista_articulos = list(articulos_publicados)`
- **Al hacer "slicing" con un `step`**: `algunos_articulos = articulos_publicados[::2]`
- **Al acceder a un índice específico** (si no ha sido evaluado y cacheado antes): `primer_articulo = articulos_publicados[0]`
- **Al llamar a `repr()` o `len()` sobre él**: `print(articulos_publicados)` o `numero = len(articulos_publicados)`
- **Al usarlo con ciertas plantillas de Django** que fuerzan su evaluación.
- **Al llamar a métodos que devuelven un único objeto o valor** (como `get()`, `count()`, `exists()`, etc., que veremos más adelante).

Ventaja de la pereza:

Es súper eficiente. Puedes construir QuerySets muy complejos, añadiendo filtros, ordenamientos, etc., y Django solo hará una única consulta optimizada a la base de datos al final, cuando realmente necesites los datos.

Cacheo:

Una vez que un QuerySet se evalúa y los resultados se cargan desde la base de datos, Django guarda (cachea) esos resultados en el QuerySet. Si accedes de nuevo al mismo QuerySet (por ejemplo, iterando sobre él una segunda vez), usará los resultados cacheados en lugar de golpear la base de datos de nuevo.
```python
mis_articulos = Articulo.objects.filter(autor__nombre='Carlos') # Nada aún
print("Voy a iterar...")
for art in mis_articulos: # Consulta a la BD aquí, resultados se cachean en 'mis_articulos'
    print(art.titulo)

print("Segunda iteración...")
for art in mis_articulos: # Usa la caché, no hay nueva consulta a la BD
    print(art.contenido_corto)

cantidad = len(mis_articulos) # Usa la caché
```

Si modificas el QuerySet (ej. `mis_articulos.order_by('titulo')`), se considera un _nuevo_ QuerySet y su evaluación implicará una nueva consulta si es necesario.

---

## 4. Filtrando y Encadenando QuerySets 🔗

Como muchos métodos (`filter`, `exclude`, `order_by`) devuelven nuevos QuerySets, puedes **encadenarlos** para construir consultas más específicas.
```python
articulos_filtrados_y_ordenados = Articulo.objects \
    .filter(autor__nombre__startswith='L') \
    .filter(fecha_publicacion__year=2024) \
    .exclude(categoria='Antiguo') \
    .order_by('-fecha_creacion', 'titulo')
```

¡Todo esto sigue siendo perezoso! La consulta a la BD solo se hará cuando `articulos_filtrados_y_ordenados` se evalúe.

Lookups de Campo (el __ mágico):

Para hacer filtros más potentes, Django usa "lookups" que se añaden al nombre del campo con doble guion bajo (__).

- `exact`: Coincidencia exacta (es el default). `nombre__exact='Juan'` o `nombre='Juan'`
- `iexact`: Exacta, insensible a mayúsculas/minúsculas. `nombre__iexact='juan'`
- `contains`: Si el campo contiene una subcadena. `titulo__contains='Django'`
- `icontains`: `contains` insensible a mayúsculas/minúsculas.
- `startswith`, `istartswith`: Si empieza con.
- `endswith`, `iendswith`: Si termina con.
- `in`: Si el valor está en una lista. `id__in=[1, 5, 10]`
- `gt`, `gte`, `lt`, `lte`: Mayor que (`>`), mayor o igual (`>=`), menor que (`<`), menor o igual (`<=`). `precio__gt=100.00`
- `isnull`: Si el valor es `NULL` (True o False). `fecha_finalizacion__isnull=True`
- `range`: Si el valor está en un rango. `fecha_publicacion__range=(datetime.date(2023, 1, 1), datetime.date(2023, 12, 31))`
- **Para relaciones ForeignKey**: Puedes "cruzar" la relación usando `__`. `Articulo.objects.filter(autor__email='autor@ejemplo.com')` (filtra artículos cuyo autor tiene ese email). `Comentario.objects.filter(articulo__autor__nombre='Ana')` (¡incluso múltiples saltos!).

---

## 5. Métodos que NO Devuelven QuerySets (y Ejecutan la Consulta) 💥

Estos métodos realizan una acción que requiere que la consulta se ejecute inmediatamente y devuelven algo distinto a un QuerySet.

- `get(**kwargs)`: Devuelve **un único objeto** que coincide con los `kwargs`.
    
    - Si no encuentra ningún objeto: lanza `Modelo.DoesNotExist`.
    - Si encuentra más de un objeto: lanza `Modelo.MultipleObjectsReturned`.
    - ¡Es para cuando esperas exactamente un resultado!
    ```python
    try:
        articulo_unico = Articulo.objects.get(pk=1) # pk es "primary key"
    except Articulo.DoesNotExist:
        print("El artículo no existe.")
    except Articulo.MultipleObjectsReturned:
        print("¡Hay más de un artículo con esos criterios!")
    ```
    
- **`count()`**: Devuelve el **número de objetos** en el QuerySet. Es más eficiente que hacer `len(queryset.all())` si solo necesitas el conteo.
    ```python
    num_articulos_publicados = Articulo.objects.filter(estado='publicado').count()
    ```
    
- **`first()`**: Devuelve el **primer objeto** del QuerySet (según el orden definido), o `None` si el QuerySet está vacío.
- **`last()`**: Devuelve el **último objeto** del QuerySet, o `None` si está vacío.
- **`exists()`**: Devuelve `True` si el QuerySet contiene al menos un objeto, y `False` si no. Muy eficiente si solo quieres saber si hay "algo".
    ```python
    hay_articulos_de_hoy = Articulo.objects.filter(fecha_publicacion=date.today()).exists()
    ```
    
- `create(**kwargs)`: Crea un nuevo objeto, lo guarda en la base de datos y lo devuelve.
    ```python
    nuevo_autor = Autor.objects.create(nombre='Elena', email='elena@ejemplo.com')
    ```
    
- `get_or_create(defaults=None, **kwargs)`: Intenta obtener un objeto usando `kwargs`. Si no existe, lo crea usando `kwargs` y los valores opcionales de `defaults`. Devuelve una tupla `(objeto, created_boolean)`. `created_boolean` es `True` si se creó un nuevo objeto.
    ```python
    autor, creado = Autor.objects.get_or_create(
        email='nuevo@ejemplo.com',
        defaults={'nombre': 'Nuevo Autor'}
    )
    if creado:
        print(f"Se creó el autor: {autor.nombre}")
    else:
        print(f"Se encontró el autor: {autor.nombre}")
    ```
    
- `update_or_create(defaults=None, **kwargs)`: Similar a `get_or_create`, pero si el objeto existe, lo actualiza con los valores de `defaults`. Devuelve `(objeto, created_boolean)`.
- `update(**kwargs)`: Actualiza **todos los objetos** en el QuerySet con los valores de `kwargs`. Devuelve el número de filas afectadas.
    
    - **¡Importante!** Esta es una operación SQL masiva. **No llama al método `save()`** de cada instancia del modelo, ni emite señales `pre_save`/`post_save`. Usar si no necesitas esa lógica personalizada.
    ```python
    filas_actualizadas = Articulo.objects.filter(estado='borrador').update(estado='revisión_pendiente')
    ```
    
- **`delete()`**: Elimina **todos los objetos** en el QuerySet de la base de datos. Devuelve el número de objetos eliminados y un diccionario con los tipos de objetos eliminados (útil por las cascadas).
    
    - **¡Cuidado!** Esto es una eliminación masiva y puede tener efectos en cascada si tus modelos tienen `on_delete=models.CASCADE`.
    ```python
    num_eliminados, detalles = Articulo.objects.filter(autor__activo=False).delete()
    ```
    

---

## 6. QuerySets y el Acceso a Datos Relacionados 🧑‍🤝‍🧑

Ya mencionamos cómo filtrar usando campos de modelos relacionados (`autor__nombre`). Cuando recuperas objetos y luego necesitas acceder a sus datos relacionados, recuerda los optimizadores:

- **`select_related(*fields)`**: Para relaciones `ForeignKey` y `OneToOneField`. "Sigue" las relaciones y trae los datos relacionados en la misma consulta SQL usando `JOIN`s. Evita consultas adicionales cuando accedes a esos campos relacionados después.
    ```python
    # Sin select_related, acceder a articulo.autor.nombre haría una consulta por cada artículo
    articulos = Articulo.objects.select_related('autor').all()
    for articulo in articulos:
        print(f"'{articulo.titulo}' por {articulo.autor.nombre}") # autor.nombre ya está pre-cargado
    ```
    
- **`prefetch_related(*lookups)`**: Para `ManyToManyField` y relaciones `ForeignKey` inversas (o `OneToOneField` inversas). Hace una consulta separada para cada "lookup" y une los datos en Python. Es más eficiente que múltiples consultas individuales dentro de un bucle.
    ```python
    # Sin prefetch_related, acceder a publicacion.etiquetas.all() haría una consulta por cada publicación
    publicaciones = Publicacion.objects.prefetch_related('etiquetas').all()
    for publicacion in publicaciones:
        print(f"Publicación: {publicacion.titulo}")
        for etiqueta in publicacion.etiquetas.all(): # etiquetas ya están pre-cargadas para esta publicación
            print(f"  - Etiqueta: {etiqueta.nombre}")
    ```
    

---

## 7. Slicing y Limitando QuerySets 🍰

Puedes "rebanar" (slice) un QuerySet de forma similar a una lista de Python para limitar el número de resultados.

- `primeros_diez_articulos = Articulo.objects.all()[:10]`
    - Esto se traduce a `LIMIT 10` en SQL.
- `articulos_del_11_al_20 = Articulo.objects.all()[10:20]`
    - Esto se traduce a `LIMIT 10 OFFSET 10` en SQL.

Generalmente, el slicing no evalúa el QuerySet completo si no ha sido cacheado. Sin embargo, no se permiten índices negativos (como `[:-1]`) directamente en un QuerySet que aún no ha sido evaluado, ya que no hay forma fácil de traducir eso a SQL eficiente sin conocer el total.

---

## 8. ¿Cuándo NO Usar `update()` Masivo para Modificar? 🧐

El método `.update()` es rápido para cambios masivos, pero como dijimos, **no llama al método `.save()`** de cada instancia. ¿Por qué importa esto?

- Tu modelo podría tener lógica personalizada en su método `save()` (ej. calcular un campo, actualizar una fecha, limpiar datos).
- Podrías tener señales `pre_save` o `post_save` conectadas a tu modelo que necesitan ejecutarse.

Si necesitas que esa lógica o esas señales se ejecuten para cada objeto, debes iterar sobre el QuerySet y llamar a `.save()` en cada instancia:
```python
articulos_a_revisar = Articulo.objects.filter(estado='revisión_pendiente')
for articulo in articulos_a_revisar:
    articulo.revisado_por = supervisor_actual # Alguna lógica para asignar revisor
    articulo.ultima_modificacion = timezone.now() # Actualizar un campo
    articulo.save() # Esto sí llama al método save() del modelo 'Articulo' y dispara señales
```

Esto será más lento que un `.update()` masivo porque implica más trabajo en Python y potencialmente más queries individuales si no se maneja con cuidado, pero asegura que toda tu lógica de modelo se ejecute.

---

## Mini Repaso Final 📝

- Los **QuerySets** son la forma principal de Django para representar y ejecutar **consultas a la base de datos**.
- Son **perezosos (lazy)**: la consulta real se retrasa hasta que los datos son necesarios, lo que es muy eficiente.
- Se obtienen del **manager** del modelo (ej. `Modelo.objects`).
- Permiten **encadenar filtros** y operaciones (ej. `.filter().exclude().order_by()`).
- Algunos métodos devuelven nuevos QuerySets (manteniendo la pereza), mientras que otros **ejecutan la consulta** y devuelven resultados directos (ej. `.get()`, `.count()`, `.first()`, `.exists()`, `.create()`, `.update()`, `.delete()`).
- `select_related` y `prefetch_related` son tus mejores amigos para optimizar el acceso a datos relacionados.
- ¡Son increíblemente poderosos y flexibles! Conocerlos bien es clave para ser un buen desarrollador Django.