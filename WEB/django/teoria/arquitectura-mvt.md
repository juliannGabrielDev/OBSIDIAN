---
aliases:
  - 🏗️ Arquitectura MVT en Django
tags:
  - django
  - teoria
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
---
# 🏗️ Arquitectura MVT en Django

La arquitectura **MVT** (Modelo-Vista-Plantilla, o _Model-View-Template_ en inglés) es el patrón de diseño que utiliza **Django**. Es una variante del conocido patrón **MVC** (Modelo-Vista-Controlador). ¡Básicamente, nos ayuda a mantener nuestro código organizado y separado por responsabilidades! ✨
- [[#1. Modelo (Model) 🧱|1. Modelo (Model) 🧱]]
- [[#2. Vista (View) 👀|2. Vista (View) 👀]]
- [[#3. Plantilla (Template) 📄|3. Plantilla (Template) 📄]]
- [[#¿Cómo funciona todo junto? 🤔 El Flujo MVT|¿Cómo funciona todo junto? 🤔 El Flujo MVT]]
- [[#🌟 Mini Repaso Final|🌟 Mini Repaso Final]]

---

## 1. Modelo (Model) 🧱

El **Modelo** es la capa que se encarga de la **lógica de datos** de la aplicación. ¡Piensa en él como el cerebro detrás de la información! 🧠

- Representa la **estructura de los datos**. Usualmente, cada modelo en Django se mapea a una tabla en la base de datos.
    
- Define los **campos** y **comportamientos** de los datos que estás almacenando.
    
- Es responsable de **interactuar con la base de datos**: crear, leer, actualizar y borrar registros (lo que conocemos como **CRUD** - Create, Read, Update, Delete).
    
- Django tiene un **ORM** (Object-Relational Mapper) incorporado que facilita muchísimo esta interacción, ¡así no tenemos que escribir SQL directamente si no queremos!
    
    - **Ejemplo**: Si estamos haciendo una app de blog, podríamos tener un modelo `Articulo` con campos como `titulo`, `contenido`, `fecha_publicacion` y `autor`.
        
    ```python
    # ejemplo_app/models.py
    from django.db import models
    from django.contrib.auth.models import User
    
    class Articulo(models.Model):
        titulo = models.CharField(max_length=200)
        contenido = models.TextField()
        fecha_publicacion = models.DateTimeField(auto_now_add=True)
        autor = models.ForeignKey(User, on_delete=models.CASCADE)
    
        def __str__(self):
            return self.titulo
    ```
    

---

## 2. Vista (View) 👀

La **Vista** es la capa que maneja la **lógica de la aplicación**. Recibe las solicitudes HTTP (como cuando un usuario visita una URL), procesa la información y decide qué datos enviar a la plantilla. ¡Es como el director de orquesta! 🎶

- Recibe una **petición web** y devuelve una **respuesta web**.
    
- Interactúa con los **Modelos** para obtener los datos necesarios.
    
- Selecciona una **Plantilla** para presentar los datos al usuario.
    
- No se encarga de _cómo_ se ven los datos (eso es trabajo de la Plantilla), sino de _qué_ datos se muestran.
    
- En Django, las vistas pueden ser **funciones** (Function-Based Views - FBV) o **clases** (Class-Based Views - CBV).
    
    - **Ejemplo**: Una vista podría obtener todos los artículos de la base de datos y pasarlos a una plantilla para que los muestre.
        
    ```python
    # ejemplo_app/views.py
    from django.shortcuts import render
    from .models import Articulo
    
    def lista_articulos(request):
        articulos = Articulo.objects.all().order_by('-fecha_publicacion')
        contexto = {'articulos': articulos}
        return render(request, 'ejemplo_app/lista_articulos.html', contexto)
    ```
    

---

## 3. Plantilla (Template) 📄

La **Plantilla** es la capa de **presentación**. Define cómo se muestran los datos al usuario. ¡Es la cara bonita de nuestra aplicación! 😊

- Generalmente son archivos **HTML** que contienen marcadores de posición y lógica de presentación básica (como bucles o condicionales) usando el **lenguaje de plantillas de Django** (Django Template Language - DTL).
    
- Recibe los datos de la **Vista** (a través de un diccionario llamado "contexto") y los renderiza dinámicamente.
    
- Se enfoca en la **apariencia** y la **interfaz de usuario (UI)**.
    
- Ayuda a separar la lógica de la presentación, lo que hace que el código sea más limpio y fácil de mantener.
    
    - **Ejemplo**: Una plantilla podría mostrar una lista de títulos de artículos, cada uno siendo un enlace al artículo completo.
    
    ```python
    {% extends 'base.html' %}
    
    {% block content %}
      <h1>📝 Lista de Artículos</h1>
      <ul>
        {% for articulo in articulos %}
          <li>
            <h2><a href="#">{{ articulo.titulo }}</a></h2>
            <p>Por: {{ articulo.autor.username }} - {{ articulo.fecha_publicacion|date:"d M Y" }}</p>
          </li>
        {% empty %}
          <li>No hay artículos disponibles. 😥</li>
        {% endfor %}
      </ul>
    {% endblock %}
    ```
    

---

## ¿Cómo funciona todo junto? 🤔 El Flujo MVT

Imaginemos que un usuario quiere ver la lista de artículos en nuestro blog:

1. El **navegador** del usuario envía una **petición HTTP** a una URL específica (ej: `/articulos/`).
2. **Django** recibe la petición. A través del archivo `urls.py`, determina qué **Vista** debe manejar esa URL.
3. La **Vista** (`lista_articulos` en nuestro ejemplo) se ejecuta.
4. La **Vista** interactúa con el **Modelo** (`Articulo`) para solicitar los datos necesarios (ej: "dame todos los artículos").
5. El **Modelo** consulta la **base de datos** y devuelve los datos a la **Vista**.
6. La **Vista** procesa los datos y los pasa a la **Plantilla** (`lista_articulos.html`) a través de un **contexto**.
7. La **Plantilla** renderiza el HTML final, insertando los datos del contexto en los lugares apropiados.
8. **Django** envía la **respuesta HTTP** (el HTML renderizado) de vuelta al navegador del usuario.
9. El **navegador** muestra la página al usuario. ¡Y listo! 🎉

**Diagrama Simplificado del Flujo:**

```
Usuario <--> Navegador <--> URL (Django) --> Vista --> Modelo <--> Base de Datos
                                                  ^        |
                                                  |        v
                                                  --- Plantilla
```

_(Nota: En realidad, la Vista le pide a la Plantilla que se renderice con los datos, y la Plantilla devuelve el HTML renderizado a la Vista, que luego lo envía como respuesta)._

---

## 🌟 Mini Repaso Final

- **MVT** es el patrón de diseño de Django que separa las responsabilidades en **Modelo**, **Vista** y **Plantilla**.
- **Modelo**: Maneja los datos y la interacción con la base de datos. ¡El guardián de la información! 🛡️
- **Vista**: Recibe peticiones, obtiene datos del modelo y selecciona una plantilla. ¡El cerebro de la lógica! 💡
- **Plantilla**: Define cómo se presentan los datos al usuario. ¡La cara visible! 🎨
- Este patrón ayuda a tener un código más **organizado**, **mantenible** y **escalable**.