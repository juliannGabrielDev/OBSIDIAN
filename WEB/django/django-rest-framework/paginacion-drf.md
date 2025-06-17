---
aliases:
  - "📝 Paginación: ¡Controlando tus Datos! 🚀"
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
# 📝 Paginación en Django REST Framework: ¡Controlando tus Datos! 🚀

## 📚 Introducción a la Paginación: ¿Por qué es tan importante? 🧐

A estas alturas, seguro sabes que es una **excelente práctica** que las APIs envíen los resultados en fragmentos más pequeños 🤏. Imagina una base de datos con **1000 pedidos** y que un cliente visita el endpoint de pedidos 📦. Si no hay paginación, ¡el cliente recibirá los 1000 pedidos cada vez!, cuando solo necesita los **últimos 10** 🤯.

Esta operación innecesaria añade una **enorme carga** al servidor de la base de datos 💥 y **desperdicia ancho de banda** 📉. Los desarrolladores utilizan la **paginación para segmentar los resultados** ✂️. Las aplicaciones cliente pueden entonces decidir el **número de página** y **cuántos registros quieren por página** 📄.

La paginación es una **técnica fundamental** que debes dominar para **ahorrar recursos del servidor** al proporcionar un número limitado de registros 💰. ¡Este video te mostrará cómo implementar la paginación en tu proyecto de API de DRF! 🎥

---

## 🧭 Conceptos Clave de Paginación ✨

Para entender cómo funciona, piensa en un ejemplo práctico. Si el cliente solicita el **7º y 8º elemento del menú**, puede hacer una solicitud HTTP GET usando el endpoint habitual, pero con parámetros de consulta adicionales 🤔:

```
/api/menu-items?per_page=2&page=4
```

- **`per_page=2`**: Significa que cada página contendrá **2 elementos** ✌️.
- **`page=4`**: Significa que estamos solicitando la **4ª página** de resultados 📖.

Así es como se distribuirían los elementos:

- **Página 1:** Registros 1 y 2 🥇
- **Página 2:** Registros 3 y 4 🥈
- **Página 3:** Registros 5 y 6 🥉
- **Página 4:** Registros 7 y 8 (¡los que el cliente quiere!) 🎉

---

> [!TIP] 💡 ¡Un consejo útil sobre seguridad! 🔒
> 
> Siempre debes limitar el número máximo de registros que un cliente puede solicitar por página 🛑. Esta es una medida de seguridad crucial para evitar el uso indebido de los endpoints de tu API 🚫.

Por ejemplo, si estableces el límite máximo en **10 registros por página** desde un endpoint de tu API, pero el cliente necesita 20 registros del 31 al 40, entonces el cliente deberá hacer **dos llamadas a esta API** ✌️:

1. La **primera llamada** solicitará `10 por página` de la `página 3`, lo que proporcionará los registros del 21 al 30.
2. La **segunda llamada** proporcionará los siguientes 10 registros, del 31 al 40, de la `página 4`.

---

> [!WARNING] ⚠️ ¡Manejo de Solicitudes Incorrectas! ❌
> 
> Si un cliente solicita 50 registros en una sola llamada a la API cuando el límite máximo es 10, puedes enviar un mensaje de estado HTTP 400 - Bad Request 🚫. Esto indica que la solicitud del cliente no es válida según tus reglas de paginación.

---

## 🚀 Implementación de Paginación en Django REST Framework 💻

¡Vamos a implementar la paginación en la aplicación Little Lemon! 🍋

1. **Abre el archivo `views.py`** y ve a la función que maneja los elementos del menú 📁. Esta función aceptará dos parámetros de la cadena de consulta: `per_page` y `page` 📄.
    
2. **Define parámetros por defecto:** Justo después de la variable `ordering`, añade estas dos líneas de código ✨:
    
    ```python
    per_page = request.GET.get('per_page', 2) # Valor por defecto: 2
    page = request.GET.get('page', 1)         # Valor por defecto: 1
    ```
    
    Si el cliente no proporciona ningún valor, `per_page` será `2` y `page` será `1` automáticamente ✅.
    
3. ¡Importa el Paginador! 📥
    
    Necesitas importar `Paginator` y `EmptyPage` de Django para manejar la paginación y posibles errores:
    
    ```python
    from django.core.paginator import Paginator, EmptyPage
    ```
    
4. **Inicializa el Paginador:** Ahora, justo antes de los elementos del serializador, añade este código que inicializará el objeto `Paginator` ⚙️:

    ```python
    paginator = Paginator(menu_items, per_page) # Crea un paginador: 'menu_items' son tus datos, 'per_page' el tamaño de la página
    ```
    
    Esto significa que estás creando un paginador donde el número de elementos `per_page` es igual al valor que recibiste o el predeterminado 📝.
    
5. **Recupera los elementos de la página:** Ahora puedes recuperar los elementos de una página usando `items = paginator.page(page)` 📄.
---

> [!WARNING] ⚠️ ¡Manejo de Páginas Inexistentes! 🚫
> 
> Un cliente puede solicitar una página que no existe, lo que generará un error EmptyPage ❌. Por eso es necesario incluir esta línea dentro de un bloque try-except para un manejo robusto de errores 🛡️.
    
```python
    try:
        items = paginator.page(page) # Intenta obtener los elementos de la página solicitada
    except EmptyPage:
        items = [] # Si la página no existe, devuelve un array vacío
```
    
Si la página existe, devolverá los elementos de esa página. En caso contrario, será una matriz vacía, evitando errores a tu cliente 👋.
    
6. ¡Verifica el Endpoint! 🧪
    
    Ahora, visita el endpoint de menu-items sin una cadena de consulta. ¡Deberías ver solo dos elementos del menú por página! ✨ ¡Excelente! El paginador funciona a la perfección.
    
7. **Prueba con Cadenas de Consulta:** Ahora, llama al endpoint `menu-items` con las cadenas de consulta necesarias, por ejemplo, configura los valores `per_page=3` y `page=1` 🎯. ¡Ahora verás **tres elementos del menú**! 🎉