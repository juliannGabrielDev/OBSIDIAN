---
aliases:
  - Beautiful Soup 🌐✨
tags:
  - python
  - scraping
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-07-12
---
# Beautiful Soup: Tu Aliado para Web Scraping 🌐✨

Beautiful Soup es una librería de Python que te permite **extraer datos** de archivos HTML y XML de una manera sencilla y eficiente. Es una herramienta indispensable para el **web scraping** 🕷️📊.

## ¿Qué es Web Scraping? 🎣

El **web scraping** es el proceso de recolectar información de sitios web de forma automatizada. Es como si un robot leyera las páginas por ti y extrajera los datos que necesitas 🤖📄.

> [!NOTE] ¡Importante! 📝
> 
> Siempre asegúrate de tener permiso para hacer scraping en un sitio web. Algunos sitios prohíben explícitamente el scraping en sus términos de servicio o a través de archivos robots.txt. ¡Sé un scraper responsable! 🙏

---

## Instalación y Uso Básico 🚀

Para empezar a usar Beautiful Soup, primero necesitas instalarlo.

```bash
pip install beautifulsoup4
```

Una vez instalado, puedes usarlo para analizar contenido HTML.

```python
from bs4 import BeautifulSoup

html_doc = """
<html>
<head><title>Mi Título</title></head>
<body>
<p class="titulo"><b>El Gran Título</b></p>
<a href="http://ejemplo.com/el_link" id="link1">Enlace de Ejemplo</a>
<p class="parrafo">Este es un párrafo.</p>
</body>
</html>
"""

soup = BeautifulSoup(html_doc, 'html.parser')

print(soup.prettify())
```

---

## Navegando el Árbol HTML 🌳🔍

Beautiful Soup transforma el documento HTML en un árbol de objetos Python, lo que facilita la navegación y búsqueda.

### Objetos Principales ✨

- **Tag:** Representa una etiqueta HTML o XML (ej. `<p>`, `<a>`).    
- **NavigableString:** Representa el texto dentro de una etiqueta.
- **BeautifulSoup:** El objeto completo que representa el documento analizado.
- **Comment:** Un tipo especial de NavigableString para comentarios HTML.

### Métodos de Búsqueda Comunes 🔎

|Método|Descripción|Ejemplo|
|---|---|---|
|`find()`|Encuentra la **primera ocurrencia** de una etiqueta.|`soup.find('p')`|
|`find_all()` / `findAll()`|Encuentra **todas las ocurrencias** de una etiqueta.|`soup.find_all('a')`|
|`select()`|Busca elementos usando **selectores CSS**.|`soup.select('p.parrafo')`|

> [!TIP] Selectores CSS 💡
> 
> Los selectores CSS son increíblemente poderosos para búsquedas específicas. Puedes seleccionar por clase (`.clase`), por ID (`#id`), por atributos (`[atributo="valor"]`), y más.

---

## Extracción de Datos 💰

Una vez que has encontrado el elemento que buscas, puedes extraer su contenido o atributos.

```python
# Acceder al título
print(soup.title) # <title>Mi Título</title>
print(soup.title.string) # Mi Título

# Encontrar un párrafo por clase
parrafo_titulo = soup.find('p', class_='titulo')
print(parrafo_titulo.get_text()) # El Gran Título

# Encontrar un enlace y sus atributos
enlace = soup.find(id='link1')
print(enlace.get('href')) # http://ejemplo.com/el_link
print(enlace.string) # Enlace de Ejemplo
```

> [!WARNING] ¡Cuidado con class_! ⚠️
> 
> Cuando busques por el atributo `class`, usa `class_` en Python porque class es una palabra reservada del lenguaje.

---

## Ejemplo Completo de Extracción 🧑‍💻

```python
import requests
from bs4 import BeautifulSoup

url = "http://books.toscrape.com/" # Un sitio para practicar scraping

try:
    response = requests.get(url)
    response.raise_for_status() # Lanza un error para códigos de estado HTTP erróneos
except requests.exceptions.RequestException as e:
    print(f"Error al conectar: {e} 🛑")
    exit()

soup = BeautifulSoup(response.text, 'html.parser')

# Extraer todos los títulos de los libros en la página principal
titulos_libros = soup.find_all('h3')
print("\n📚 Títulos de Libros en la Página Principal:")
for titulo in titulos_libros:
    print(titulo.a.get('title'))

# Extraer los precios de los libros
precios_libros = soup.find_all('p', class_='price_color')
print("\n💲 Precios de Libros:")
for precio in precios_libros:
    print(precio.get_text())

```

---

Keywords: `BeautifulSoup`, `web scraping`, `Python`, `HTML`, `XML`, `find`, `find_all`, `select`, `requests`, `parseo web`, `extracción de datos`.