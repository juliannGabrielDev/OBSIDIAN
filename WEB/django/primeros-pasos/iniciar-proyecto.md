---
aliases:
  - Cómo Iniciar un Proyecto Django 🚀
tags:
  - django
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
---
# Cómo Iniciar un Proyecto Django 🚀

Para empezar a trabajar con Django, necesitas crear un nuevo proyecto. Piensa en esto como la carpeta principal que contendrá todas tus aplicaciones y configuraciones. ¡Es súper sencillo!
- [[#1. Crear el Entorno Virtual (¡Recomendado!) 🐍|1. Crear el Entorno Virtual (¡Recomendado!) 🐍]]
- [[#2. Instalar Django 📦|2. Instalar Django 📦]]
- [[#3. Crear el Proyecto Django ✨|3. Crear el Proyecto Django ✨]]
- [[#4. Ejecutar el Servidor de Desarrollo ▶️|4. Ejecutar el Servidor de Desarrollo ▶️]]

## 1. Crear el Entorno Virtual (¡Recomendado!) 🐍

Antes de instalar Django, es una **buena práctica** crear un entorno virtual. Esto aísla las dependencias de tu proyecto de las de otros proyectos en tu computadora, evitando conflictos.
- Abre tu terminal o línea de comandos.
- Navega a la carpeta donde quieres guardar tu proyecto.

### Método A: Usando Pipenv (¡Recomendado! ✨)

Pipenv simplifica la gestión de entornos virtuales y dependencias. Si no lo tienes instalado, primero instala Pipenv globalmente (solo una vez):

```shell
pip3 install --user pipenv
```
Si `pipenv` no se encuentra después de la instalación (como `command not found`), es posible que necesites asegurarte de que `$HOME/.local/bin` esté en tu `PATH`. Puedes agregar esta línea a tu `~/.bashrc` o `~/.zshrc` y luego ejecutar `source ~/.bashrc` o `source ~/.zshrc`:

```shell
echo Linux
export PATH="$HOME/.local/bin:$PATH"
```

Una vez que tengas Pipenv, no necesitas crear el entorno virtual manualmente. Pipenv lo hará por ti cuando instales tu primera dependencia.

```
# ¡No hay comando aquí, Pipenv lo gestionará en el siguiente paso!
```

### Método B: Usando `venv` (Tradicional)

Si prefieres el método tradicional o tienes problemas con Pipenv:

- Crea el entorno virtual:
    ```shell
    python -m venv nombre_de_tu_entorno
    ```
    (Puedes usar `venv` o el nombre que prefieras para tu entorno, como `env`).
- Activa el entorno virtual:
    - **Windows**:
        ```shell
        nombre_de_tu_entorno\Scripts\activate
        ```
    - **macOS/Linux**:
        ```shell
        source nombre_de_tu_entorno/bin/activate
        ```
        Verás el nombre de tu entorno entre paréntesis en tu terminal, ¡lo que significa que está activo!
## 2. Instalar Django 📦

Con tu entorno virtual activo, ahora puedes instalar Django de forma segura.

- Instala Django usando `pip`:
    
    ```bash
    pip install django
    ```
    
    ¡Listo! Ya tienes Django instalado en tu entorno virtual.

## 3. Crear el Proyecto Django ✨

Ahora sí, es el momento de crear el esqueleto de tu proyecto Django.

- Asegúrate de que tu entorno virtual esté activo.
- Usa el comando `startproject` de Django:
    
    ```bash
    django-admin startproject mi_primer_proyecto .
    ```
    
    - `mi_primer_proyecto`: Es el **nombre de tu proyecto**. Puedes ponerle el nombre que quieras.
    - `.`: El **punto al final es importante** porque le dice a Django que cree la estructura del proyecto en el directorio actual, evitando una carpeta extra anidada.

Después de ejecutar este comando, verás una nueva carpeta con el nombre de tu proyecto y un archivo `manage.py`. ¡Felicidades, tu proyecto Django ha sido iniciado!

## 4. Ejecutar el Servidor de Desarrollo ▶️

Para ver tu proyecto en acción, puedes iniciar el servidor de desarrollo de Django.

- Dentro de la carpeta de tu proyecto (donde está `manage.py`), ejecuta
    
    ```bash
    python manage.py runserver
    ```
    
- Abre tu navegador y ve a `http://127.0.0.1:8000/`.

Si todo salió bien, verás una página predeterminada de Django que dice "The install worked successfully! Congratulations!". 🎉

¡Y eso es todo para iniciar tu primer proyecto Django! Ahora estás listo para empezar a crear aplicaciones dentro de él.