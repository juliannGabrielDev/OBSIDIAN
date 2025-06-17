---
aliases:
  - "Cambiando de Base de Datos: ¡Hola, MySQL! 🐬"
tags:
  - django
  - configurar-db
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
Fecha: 2025-06-02
---
# Cambiando de Base de Datos en Django: ¡Hola, MySQL! 🐬
Por defecto, Django usa **SQLite**, una base de datos que vive en un solo archivo. ¡Es súper fácil para empezar porque Python ya la entiende sin instalar nada extra! Pero Django es flexible y también se lleva bien con otras bases de datos más potentes como:
- PostgreSQL
- MariaDB
- MySQL
- Oracle
Hoy nos vamos a enfocar en cómo conectar nuestra app Django con **MySQL**.
- [[#1. ¿Por qué MySQL en Lugar de SQLite? 🤔|1. ¿Por qué MySQL en Lugar de SQLite? 🤔]]
- [[#2. Paso 1: Instalar MySQL Server 💾|2. Paso 1: Instalar MySQL Server 💾]]
- [[#3. Paso 2: Instalar el Conector (Driver) de MySQL para Python 🐍🔗🐬|3. Paso 2: Instalar el Conector (Driver) de MySQL para Python 🐍🔗🐬]]
- [[#4. Paso 3: Configurar Django (`settings.py`) ⚙️|4. Paso 3: Configurar Django (`settings.py`) ⚙️]]
- [[#5. Paso 4: Crear las Tablas (¡Migraciones al Rescate!) 🏗️|5. Paso 4: Crear las Tablas (¡Migraciones al Rescate!) 🏗️]]
- [[#6. Extra: Extensión VS Code para MySQL (¡Para Curiosos!) 🔍|6. Extra: Extensión VS Code para MySQL (¡Para Curiosos!) 🔍]]
- [[#7. Mini Repaso Final 💡|7. Mini Repaso Final 💡]]
## 1. ¿Por qué MySQL en Lugar de SQLite? 🤔
SQLite es como una navaja suiza: útil y portable. Pero tiene sus límites:
- Es "sin servidor" (serverless), lo que significa que no está pensada para muchas conexiones a la vez.
- No tiene un sistema de gestión de usuarios robusto como las bases de datos grandes.
- Es ideal para apps pequeñas o para prototipos que luego crecerán.
**MySQL**, en cambio, es una base de datos de código abierto que brilla por:
- **Escalabilidad**: Puede manejar muchos más datos y usuarios.
- **Autenticación y Gestión de Usuarios**: Tiene un sistema de seguridad más completo.
- **Arquitectura Cliente-Servidor**: Está diseñada para que múltiples aplicaciones se conecten a ella.
## 2. Paso 1: Instalar MySQL Server 💾
- **Descarga**: Ve a [https://www.mysql.com/downloads/](https://www.mysql.com/downloads/) y descarga el instalador para tu sistema operativo (por ejemplo, para Windows, el `mysql-installer-web-community-...msi`). Sigue los pasos del asistente. ¡Acuérdate de la contraseña que le pongas al usuario `root`!
- **Acceder a la línea de comandos de MySQL**:
    - Abre una terminal o símbolo del sistema.
    - Escribe:
        ```bash
        mysql -u root -p
        ```
    - Te pedirá la contraseña que pusiste durante la instalación. Si todo va bien, verás el prompt `mysql>`.
- **Crear tu base de datos**:
    - Dentro del prompt `mysql>`, escribe (puedes cambiar `mydatabase` por el nombre que quieras):
        ```sql
        CREATE DATABASE mydatabase;
        ```
    - Para ver que se creó:
        ```sql
        SHOW DATABASES;
        ```
        Deberías ver `mydatabase` en la lista. ¡Genial! 🎉
## 3. Paso 2: Instalar el Conector (Driver) de MySQL para Python 🐍🔗🐬
Para que Python (y por lo tanto Django) pueda hablar con MySQL, necesitamos un "traductor" o "conector", conocido como un controlador DB API.
Django recomienda mysqlclient.
- Abre tu terminal (la misma donde usas `pip` para instalar paquetes de Python) y escribe:
    ```bash
    pip install mysqlclient
    ```
    (Si usas entornos virtuales, ¡asegúrate de tener activado el correcto!).
## 4. Paso 3: Configurar Django (`settings.py`) ⚙️
Ahora le decimos a Django cómo encontrar y usar nuestra nueva base de datos MySQL. Abre el archivo `settings.py` de tu proyecto Django.
- Busca la variable `DATABASES`. Por defecto, estará configurada para SQLite. ¡Vamos a cambiarla!
- Reemplaza la configuración de SQLite con la de MySQL. La base de datos principal se llama `'default'`.
```python
# settings.py

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',  # Le decimos a Django que use el motor de MySQL
        'NAME': 'mydatabase',                 # El nombre de la BD que creaste en MySQL
        'USER': 'root',                       # Tu usuario de MySQL (si creaste otro, úsalo)
        'PASSWORD': 'tu_contraseña_de_mysql', # La contraseña de ese usuario
        'HOST': '127.0.0.1',                  # O 'localhost'. Si MySQL está en otro servidor, pones su IP.
        'PORT': '3306',                       # El puerto por defecto de MySQL.
        'OPTIONS': {
            # Esta opción es importante para evitar problemas con ciertos modos de SQL.
            'init_command': "SET sql_mode='STRICT_TRANS_TABLES'",
            # Podrías necesitar configurar el charset si usas caracteres especiales:
            # 'charset': 'utf8mb4',
        }
    }
}
```
**Otros parámetros opcionales que puedes poner en `OPTIONS`**:
- `sql_mode`: Ya lo vimos, controla el comportamiento de SQL.
- `default-character-set`: El juego de caracteres a usar (ej. `utf8` o `utf8mb4` para emojis y caracteres más variados).
- `read_default_file`: Si tienes configuraciones de MySQL en un archivo `my.cnf`, Django puede leerlo.
- `init_command`: Un comando SQL que se ejecuta cada vez que Django se conecta a la BD.
## 5. Paso 4: Crear las Tablas (¡Migraciones al Rescate!) 🏗️
Cuando inicias un proyecto Django (`startproject`), se incluyen varias apps por defecto como `admin`, `auth` (para usuarios y permisos), `sessions`, etc. Estas apps tienen modelos que necesitan sus tablas en la base de datos.
- Ejecuta el comando `migrate` para que Django cree todas estas tablas en tu nueva base de datos MySQL:
    ```bash
    python manage.py migrate
    ```
- **Para verificar en MySQL (opcional)**:
    - Vuelve a tu terminal de `mysql -u root -p`.
    - Escribe:
        ```sql
        USE mydatabase;
        SHOW TABLES;
        ```
    - ¡Deberías ver un montón de tablas que Django creó (ej. `django_migrations`, `auth_user`, `auth_group`, etc.)!
## 6. Extra: Extensión VS Code para MySQL (¡Para Curiosos!) 🔍
Si usas Visual Studio Code, hay extensiones que te permiten conectarte a MySQL y ver tus bases de datos y tablas de forma gráfica, ¡sin tener que usar la terminal de MySQL para todo!
- Busca "MySQL" en el panel de extensiones de VS Code e instala una popular (por ejemplo, la de Oracle o alguna otra bien valorada).
- Generalmente te pedirán:
    - **Hostname/Domain name**: `localhost` o `127.0.0.1`
    - **User**: `root` (o tu usuario)
    - **Password**: Tu contraseña
>[!DANGER] ¡Posible Error y Solución! 🚨
>A veces, especialmente con versiones nuevas de MySQL, puedes encontrar un error como: _"Client does not support authentication protocol requested by server; consider upgrading MySQL client"_. Esto suele ser porque el usuario `root` (o el que estés usando) está configurado con un plugin de autenticación más nuevo que el `mysqlclient` no entiende del todo bien por defecto. 
>**Solución (en la terminal de MySQL)**:
>1. Cambia el plugin de autenticación para tu usuario (reemplaza `'tu_contraseña'` con tu contraseña real):
>`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'tu_contraseña';`
>2. Refresca los privilegios:
>`FLUSH PRIVILEGES;`
>3. Intenta conectar desde VS Code de nuevo. ¡Debería funcionar!
Una vez conectado en VS Code, podrás expandir tu `mydatabase` y ver todas las tablas.
## 7. Mini Repaso Final 💡
Cambiar Django a MySQL implica estos pasos clave:
1. **Instalar MySQL Server** en tu máquina.
2. **Crear una base de datos** en MySQL (ej. `mydatabase`).
3. **Instalar el conector `mysqlclient`** para Python (`pip install mysqlclient`).
4. **Configurar `DATABASES` en `settings.py`** con los datos de tu MySQL.
5. **Ejecutar `python manage.py migrate`** para crear las tablas de Django en MySQL.