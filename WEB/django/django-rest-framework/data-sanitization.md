---
aliases:
  - "✨ Saneamiento de Datos: ¡Protegiendo tu API! 🛡️📝"
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
# ✨ Saneamiento de Datos: ¡Protegiendo tu API! 🛡️📝

## 🌟 ¡Introducción al Saneamiento de Datos! ✨

El saneamiento de datos es un **proceso crucial** para limpiar los datos y proteger tu proyecto API de amenazas dañinas 🚫. Sin un saneamiento adecuado, tu API podría ser vulnerable a ataques como la **inyección SQL** 👾, mientras que las aplicaciones cliente podrían enfrentarse a **cross-site scripting (XSS)** o **secuestro de sesión** mediante inyección de JavaScript 😱. La validación de datos por sí sola no es suficiente para prevenir estos ataques sofisticados 🙅‍♀️. Aunque Django maneja parte del saneamiento automáticamente, a menudo necesitarás implementar **procesos adicionales** para cumplir con los requisitos específicos de tu proyecto 🛠️.

En esta nota, aprenderás cómo protegerte contra la **inyección de scripts** y la **inyección SQL** utilizando técnicas efectivas de saneamiento de datos dentro de **Django Rest Framework (DRF)** 🚀.

---
## 🧹 Sanea HTML y JavaScript 💻⛔

A menos que lo intencionalmente, siempre debes **verificar** si la entrada del usuario contiene etiquetas HTML y neutralizarlas 🧑‍💻. Esto se hace convirtiendo los caracteres especiales HTML en **entidades HTML** 🔄. ¿Por qué? Porque los actores maliciosos pueden inyectar JavaScript dañino usando etiquetas `<script>` 😈 o añadir rastreadores no deseados con etiquetas `<img>` 🕵️‍♀️.

Imagina que alguien introduce: `Pasta de Tomate <script>alert(‘hola’)</script>` como un elemento del menú 🍝. Si no saneas estos datos, el script se ejecutará con éxito cuando se muestre el título del menú 😬. Si bien un simple `alert('hola')` puede parecer inofensivo, los atacantes pueden inyectar **código mucho más malicioso** ☠️.

---

> [!TIP] 💡 Paquete Bleach para Saneamiento de HTML/JS ✨
> 
> Existe un popular paquete de Python de terceros llamado bleach que puede ayudarte a limpiar HTML y JavaScript de manera efectiva 🧼. Convierte todos los caracteres HTML especiales (como <, >) y las etiquetas en entidades HTML, impidiendo que los navegadores los ejecuten como HTML 🚫.

### 🛠️ ¡Instala Bleach! ⬇️

Aquí te explicamos cómo poner en marcha `bleach` 🚀:

1. **Paso 1: Instala con `pipenv`** 📦
    
    ```bash
    pipenv install bleach
    ```
    
    Este comando añade el paquete `bleach` a las dependencias de tu proyecto 📁.
    
2. Paso 2: ¡Importa bleach! 📥
    
    En tu archivo serializers.py, importa el módulo bleach de esta manera:
    
    ```python
    import bleach
    ```
    
    ¡Ahora estás listo para usar su poder de limpieza! 💪
    
3. Paso 3: Sanea los Datos del Campo 🚿
    
    Puedes sanear campos usando los métodos validate_field() y validate() dentro de tu serializador 🤔. El módulo bleach proporciona una función clean() para este propósito ✨.
    
    - **Sanear un solo campo (ej. `title`)** 🏷️: Añade un método `validate_title()` encima de la clase `Meta` en tu `MenuItemSerializer`:
        
        ```python
        def validate_title(self, value):
            return bleach.clean(value)
        ```
        
        Esto asegura que el campo `title` se limpie antes de guardarlo 🛡️.

### 🧪 ¡Pruébalo! ✅

Si envías una **solicitud POST** al endpoint `menu-items` con etiquetas HTML en el campo `title`, los datos de entrada se **sanearán correctamente** ✅. ¡Observa cómo la etiqueta de script se convierte en entidades HTML, haciéndola inofensiva! 🎉

---

> [!INFO] ℹ️ Sin Saneamiento 🛑
> 
> Sin saneamiento, los datos de entrada se guardarían en la base de datos exactamente como se enviaron, dejándote vulnerable a ataques 😱.

### 🔄 Sanea Múltiples Campos con `validate()` 📝

También puedes sanear el campo `title` (o varios campos) dentro del método `validate()` ✨. ¡Esto te permite centralizar tu lógica de saneamiento! 🎯

```python
def validate(self, attrs):
    attrs['title'] = bleach.clean(attrs['title']) # ¡Limpia el campo de título! ✨
    if(attrs['price'] < 2):
        raise serializers.ValidationError('Price should not be less than 2.0')
    if(attrs['inventory'] < 0):
        raise serializers.ValidationError('Stock cannot be negative')
    return super().validate(attrs)
```

Este enfoque es excelente para limpiar múltiples campos desde una ubicación única y organizada 📍.

---

## ⛔ Prevención de Inyección SQL 🔐

La **inyección SQL** es un ataque común en el que usuarios maliciosos inyectan consultas SQL en los datos de entrada para realizar acciones no autorizadas en tu base de datos 🖥️💥.

### 🛡️ Buenas Prácticas para la Prevención de Inyección SQL 🔒

Prevenir la inyección SQL es relativamente sencillo si sigues estas reglas 📏. Aunque generalmente no es aconsejable ejecutar SQL crudo, hay casos en los que es necesario. En tales situaciones, **debes escapar los parámetros** usando **marcadores de posición de cadena** ✅. **Nunca** mantengas el marcador de posición dentro de comillas, ya que esto te expone a riesgos 🚫.

> [!WARNING] ⚠️ ¡Evita el SQL Crudo! 🛑
> 
> Siempre evita ejecutar consultas SQL crudas a menos que sea absolutamente necesario. ¡Utiliza el ORM de Django siempre que sea posible para la seguridad incorporada! 🛡️

Aquí tienes ejemplos de formas correctas e incorrectas de prevenir la inyección SQL:

| **Método**                                                         | **Ejemplo**                                                                                                                            | **Explicación**                                                                                                                                                                                  |
| ------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **✅ Forma Correcta:** &lt;br>Consulta Parametrizada (Sin Comillas) | `python<br>limit = request.GET.get(‘limit’)<br>MenuItem.objects.raw('SELECT * FROM LittleLemonAPI_menuitem LIMIT %s', [limit])<br>`    | El marcador de posición `%s` se utiliza correctamente **sin comillas**, y el parámetro se pasa de forma segura como parte de la ejecución de la consulta. ¡Este es el **enfoque más seguro**! 🚀 |
| **❌ Incorrecto:** &lt;br>Formato de Cadena                         | `python<br>limit = request.GET.get(‘limit’)<br>MenuItem.objects.raw('SELECT * FROM LittleLemonAPI_menuitem LIMIT %s' % limit)<br>`     | El valor `limit` se **inserta directamente** en la cadena de consulta utilizando el formato de cadena. Esto no escapa la entrada correctamente, haciendo que la consulta sea **vulnerable** 😱.  |
| **❌ Incorrecto:** &lt;br>Marcador de Posición en Comillas          | `python<br>limit = request.GET.get(‘limit’)<br>MenuItem.objects.raw("SELECT * FROM LittleLemonAPI_menuitem LIMIT '%s' ", [limit])<br>` | Colocar `%s` entre comillas (simples o dobles) lo convierte en un **literal de cadena** en SQL. Esto anula el propósito de las consultas parametrizadas y te deja **expuesto** 🚨.               |
