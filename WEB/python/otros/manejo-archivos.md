---
aliases:
  - Manejo de Archivos 📂
tags:
  - python
  - otros
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-06-02
---
# Manejo de Archivos en Python 📂
- [[#1. ¿Qué es el Manejo de Archivos? 🤔|1. ¿Qué es el Manejo de Archivos? 🤔]]
- [[#2. Apertura y Cierre de Archivos: La base 🗝️|2. Apertura y Cierre de Archivos: La base 🗝️]]
- [[#3. Modos de Apertura Comunes 🚪|3. Modos de Apertura Comunes 🚪]]
- [[#4. Trabajando con Archivos de Texto Plano (.txt) 📝|4. Trabajando con Archivos de Texto Plano (.txt) 📝]]
- [[#5. Trabajando con Archivos JSON (.json) 🧬|5. Trabajando con Archivos JSON (.json) 🧬]]
- [[#6. Trabajando con Archivos CSV (.csv) 📊|6. Trabajando con Archivos CSV (.csv) 📊]]
- [[#Mini Repaso Final 📝|Mini Repaso Final 📝]]

---
## 1. ¿Qué es el Manejo de Archivos? 🤔
El **manejo de archivos** se refiere a cómo nuestros programas de Python pueden **leer datos desde archivos** y **escribir datos hacia archivos**. Esto es fundamental porque nos permite:
* **Persistir datos**: Guardar información para que no se pierda cuando el programa termina.
* **Leer configuraciones**: Cargar ajustes o parámetros desde un archivo.
* **Procesar grandes cantidades de datos**: Trabajar con datasets que no cabrían cómodamente en la memoria.
* **Intercambiar datos**: Compartir información con otros programas o sistemas.

---
## 2. Apertura y Cierre de Archivos: La base 🗝️
Para trabajar con un archivo, primero necesitamos **abrirlo** y, muy importante, ¡**cerrarlo** cuando terminemos!
* **`open()`**: Es la función incorporada para abrir un archivo. Necesita al menos la ruta del archivo y, opcionalmente, el modo de apertura.
    ```python
    # Sintaxis básica
    # archivo = open("nombre_del_archivo.txt", "modo")
    ```
* **`close()`**: Es el método que se usa sobre el objeto archivo para cerrarlo.
    * **¡Importante!** Siempre debes cerrar los archivos después de usarlos para:
        * Asegurar que todos los datos se escriban correctamente en el disco.
        * Liberar los recursos del sistema que el archivo estaba utilizando.
* **`with open(...) as ...:`**: Esta es la **forma recomendada y más segura** de trabajar con archivos.
    * Automáticamente se encarga de cerrar el archivo, ¡incluso si ocurren errores!
    ```python
    # Forma recomendada
    # with open("nombre_del_archivo.txt", "modo") as archivo:
    #     # Hacer cosas con el archivo
    # # Aquí el archivo ya está cerrado automáticamente
    ```

---
## 3. Modos de Apertura Comunes 🚪
Cuando abres un archivo, especificas un **modo** que le dice a Python qué quieres hacer con él.

| Modo | Descripción                                                                                               | Puntero | ¿Crea archivo si no existe? | ¿Sobrescribe? |
| :--- | :-------------------------------------------------------------------------------------------------------- | :------ | :-------------------------- | :------------ |
| `r`  | **Leer** (read). Modo por defecto. Error si el archivo no existe.                                         | Inicio  | No                          | No            |
| `w`  | **Escribir** (write). Crea el archivo si no existe. **Sobrescribe** el contenido si el archivo ya existe. | Inicio  | Sí                          | Sí            |
| `a`  | **Añadir** (append). Crea el archivo si no existe. Agrega datos al final del archivo si ya existe.        | Final   | Sí                          | No            |
| `r+` | **Leer y escribir**. Error si el archivo no existe.                                                       | Inicio  | No                          | Según se use  |
| `w+` | **Escribir y leer**. Crea el archivo si no existe. **Sobrescribe** el contenido si el archivo ya existe.  | Inicio  | Sí                          | Sí            |
| `a+` | **Añadir y leer**. Crea el archivo si no existe. Agrega datos al final. Puntero al final para escribir.   | Final   | Sí                          | No            |
| `b`  | **Modo binario** (ej. `rb`, `wb`). Para archivos no textuales como imágenes o ejecutables.                | N/A     | N/A                         | N/A           |
| `t`  | **Modo texto**. Modo por defecto. (ej. `rt`, `wt`).                                                       | N/A     | N/A                         | N/A           |

---
## 4. Trabajando con Archivos de Texto Plano (.txt) 📝
Estos son los archivos más simples, contienen solo texto.
* ### Leer archivos TXT:
    * **`read()`**: Lee todo el contenido del archivo como una sola cadena.
        ```python
        with open("mi_archivo.txt", "r") as f:
            contenido_completo = f.read()
            print(contenido_completo)
        ```
    * **`readline()`**: Lee una sola línea del archivo. Cada vez que lo llamas, lee la siguiente línea.
        ```python
        with open("mi_archivo.txt", "r") as f:
            linea1 = f.readline()
            print(f"Línea 1: {linea1.strip()}") # .strip() para quitar saltos de línea
            linea2 = f.readline()
            print(f"Línea 2: {linea2.strip()}")
        ```
    * **`readlines()`**: Lee todas las líneas del archivo y las devuelve como una lista de cadenas.
        ```python
        with open("mi_archivo.txt", "r") as f:
            todas_las_lineas = f.readlines()
            for linea in todas_las_lineas:
                print(linea.strip())
        ```
    * **Iterar sobre el objeto archivo**: Es la forma más común y eficiente para leer línea por línea.
        ```python
        with open("mi_archivo.txt", "r") as f:
            for linea in f:
                print(linea.strip()) # .strip() es útil para quitar el '\n' al final
        ```
* ### Escribir en archivos TXT:
    * **`write(string)`**: Escribe la cadena `string` en el archivo. No añade un salto de línea automáticamente.
        ```python
        with open("salida.txt", "w") as f: # Modo 'w' sobrescribe o crea
            f.write("Hola, mundo!\n")      # \n para nueva línea
            f.write("Este es un archivo de texto.\n")
        ```
    * **`writelines(lista_de_cadenas)`**: Escribe cada cadena de la lista en el archivo. Tampoco añade saltos de línea.
        ```python
        lineas_a_escribir = ["Primera línea\n", "Segunda línea\n", "Tercera línea\n"]
        with open("salida_writelines.txt", "w") as f:
            f.writelines(lineas_a_escribir)
        ```
    * Para **añadir** contenido sin borrar lo anterior, usa el modo **`"a"`** (append).
        ```python
        with open("salida.txt", "a") as f: # Modo 'a' añade al final
            f.write("Esta línea se añade al final.\n")
        ```

---
## 5. Trabajando con Archivos JSON (.json) 🧬
**JSON** (JavaScript Object Notation) es un formato ligero de intercambio de datos, muy fácil de leer para humanos y fácil de interpretar para las máquinas.
* **¿Qué es JSON?**
    * Se basa en dos estructuras:
        * Una colección de pares **clave-valor** (como los diccionarios en Python).
        * Una **lista ordenada** de valores (como las listas en Python).
    * Las claves son cadenas (strings), los valores pueden ser strings, números, booleanos (`true`/`false`), arrays (listas) u otros objetos JSON (diccionarios).
    * Ejemplo de un archivo `datos.json`:
        ```json
        {
          "nombre": "Carlos",
          "edad": 30,
          "es_estudiante": false,
          "cursos": [
            {"titulo": "Python Básico", "duracion_horas": 40},
            {"titulo": "Análisis de Datos", "duracion_horas": 60}
          ],
          "direccion": null
        }
        ```
* Para trabajar con JSON en Python, usamos el módulo **`json`**. ¡No olvides importarlo! `import json`
* ### Leer archivos JSON:
    * **`json.load(archivo_abierto)`**: Lee un archivo JSON y lo convierte en un objeto Python (generalmente un diccionario o una lista).
        ```python
        import json

        with open("datos.json", "r") as f:
            datos_python = json.load(f) # Convierte JSON a diccionario/lista Python

        print(datos_python["nombre"])
        print(datos_python["cursos"][0]["titulo"])
        ```

* ### Escribir en archivos JSON:
    * **`json.dump(objeto_python, archivo_abierto, indent=None)`**: Convierte un objeto Python (diccionario, lista) a una cadena JSON y la escribe en el archivo.
        * `indent`: Parámetro opcional para que el JSON se escriba de forma "bonita" (indentado).
        ```python
        import json

        datos_para_guardar = {
            "producto": "Laptop",
            "precio": 1200.50,
            "disponible": True,
            "caracteristicas": ["16GB RAM", "512GB SSD", "Pantalla 14 pulgadas"]
        }

        with open("producto.json", "w") as f:
            json.dump(datos_para_guardar, f, indent=4) # indent=4 para buena legibilidad
        ```
    * También existe `json.loads(string_json)` para convertir una cadena JSON a Python, y `json.dumps(objeto_python)` para convertir un objeto Python a una cadena JSON (sin escribir a archivo directamente).
---
## 6. Trabajando con Archivos CSV (.csv) 📊
**CSV** (Comma-Separated Values) es un formato de archivo de texto donde los valores están separados por comas (o algún otro delimitador como punto y coma o tabulación). Es muy común para datos tabulares (como hojas de cálculo).
* **¿Qué es CSV?**
    * Cada línea del archivo es una **fila** de datos.
    * Cada fila contiene uno o más **campos** (columnas), separados por un **delimitador** (comúnmente una coma `,`).
    * Ejemplo de un archivo `contactos.csv`:
        ```csv
        nombre,email,telefono
        Ana Pérez,ana.perez@email.com,555-1234
        Luis García,luis.garcia@email.com,555-5678
        Sofía Rodríguez,sofia.r@email.com,555-8765
        ```
* Para trabajar con CSV en Python, usamos el módulo **`csv`**. ¡No olvides importarlo! `import csv`
* ### Leer archivos CSV:
    * **`csv.reader(archivo_abierto, delimiter=',')`**: Crea un objeto lector que itera sobre las filas del archivo CSV. Cada fila se devuelve como una lista de cadenas.
        ```python
        import csv

        with open("contactos.csv", "r", newline='') as f: # newline='' es importante
            lector_csv = csv.reader(f, delimiter=',')

            # Opcional: Leer la cabecera
            # cabecera = next(lector_csv)
            # print(f"Cabeceras: {cabecera}")

            for fila in lector_csv:
                # fila es una lista de strings, ej: ['Ana Pérez', 'ana.perez@email.com', '555-1234']
                print(f"Nombre: {fila[0]}, Email: {fila[1]}, Teléfono: {fila[2]}")
        ```
        * **`newline=''`**: Se recomienda al abrir archivos CSV para evitar problemas con diferentes finales de línea.
    * **`csv.DictReader(archivo_abierto)`**: Similar a `csv.reader`, pero cada fila se devuelve como un diccionario donde las claves son los nombres de las columnas (tomados de la primera fila, la cabecera).
        ```python
        import csv

        with open("contactos.csv", "r", newline='') as f:
            lector_dict_csv = csv.DictReader(f)
            for fila_dict in lector_dict_csv:
                # fila_dict es un diccionario, ej:
                # {'nombre': 'Ana Pérez', 'email': 'ana.perez@email.com', 'telefono': '555-1234'}
                print(f"Nombre: {fila_dict['nombre']}, Email: {fila_dict['email']}")
        ```
* ### Escribir en archivos CSV:
    * **`csv.writer(archivo_abierto, delimiter=',')`**: Crea un objeto escritor para escribir datos en un archivo CSV.
        * **`writerow(lista_de_valores)`**: Escribe una fila en el archivo CSV.
        * **`writerows(lista_de_listas_de_valores)`**: Escribe múltiples filas.
        ```python
        import csv

        datos_a_escribir = [
            ["Producto", "Precio", "Stock"],
            ["Camisa", 25.99, 150],
            ["Pantalón", 45.50, 100],
            ["Zapatos", 70.00, 75]
        ]

        with open("inventario.csv", "w", newline='') as f:
            escritor_csv = csv.writer(f, delimiter=',')
            # Escribir una fila a la vez
            # escritor_csv.writerow(datos_a_escribir[0]) # Cabecera
            # escritor_csv.writerow(datos_a_escribir[1])
            # escritor_csv.writerow(datos_a_escribir[2])
            # escritor_csv.writerow(datos_a_escribir[3])

            # O escribir todas las filas de golpe
            escritor_csv.writerows(datos_a_escribir)
        ```
    * **`csv.DictWriter(archivo_abierto, fieldnames=lista_cabeceras)`**: Para escribir diccionarios en un archivo CSV.
        * Necesitas especificar `fieldnames` (los nombres de las columnas).
        * Usa `writeheader()` para escribir la fila de cabecera.
        * Usa `writerow(diccionario)` para escribir una fila desde un diccionario.
        ```python
        import csv

        cabeceras = ["nombre", "puesto", "departamento"]
        empleados = [
            {'nombre': 'Laura', 'puesto': 'Desarrolladora', 'departamento': 'TI'},
            {'nombre': 'Marcos', 'puesto': 'Diseñador', 'departamento': 'Marketing'},
            {'nombre': 'Elena', 'puesto': 'Gerente', 'departamento': 'Ventas'}
        ]

        with open("empleados.csv", "w", newline='') as f:
            escritor_dict_csv = csv.DictWriter(f, fieldnames=cabeceras)

            escritor_dict_csv.writeheader() # Escribe la fila de cabeceras
            for empleado in empleados:
                escritor_dict_csv.writerow(empleado)
            # O también: escritor_dict_csv.writerows(empleados)
        ```

---
## Mini Repaso Final 📝

* **Siempre** usa `with open(...) as ...:` para asegurar que los archivos se cierren correctamente.
* Elige el **modo de apertura** (`r`, `w`, `a`, `r+`, etc.) según lo que necesites hacer.
* Para archivos **.txt**:
    * Leer: `read()`, `readline()`, `readlines()`, o iterar sobre el archivo.
    * Escribir: `write()`, `writelines()`.
* Para archivos **.json**:
    * Importa el módulo `json`.
    * Leer: `json.load(archivo_abierto)` para convertir JSON a Python.
    * Escribir: `json.dump(objeto_python, archivo_abierto)` para convertir Python a JSON.
* Para archivos **.csv**:
    * Importa el módulo `csv`.
    * Leer: `csv.reader()` (para listas) o `csv.DictReader()` (para diccionarios). Recuerda `newline=''`.
    * Escribir: `csv.writer()` (con `writerow`/`writerows` para listas) o `csv.DictWriter()` (con `writeheader` y `writerow`/`writerows` para diccionarios). Recuerda `newline=''`.