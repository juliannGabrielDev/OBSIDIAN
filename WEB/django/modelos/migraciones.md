---
aliases:
  - Migraciones en Django 🏗️
tags:
  - django
  - modelos
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
---
# Migraciones en Django 🏗️: ¡Construyendo y Actualizando Nuestra Base de Datos!
- [[#1. ¿Qué Rayos Son las Migraciones? 🤔|1. ¿Qué Rayos Son las Migraciones? 🤔]]
- [[#2. ¿Por Qué Son Tan Importantes? 🌟|2. ¿Por Qué Son Tan Importantes? 🌟]]
- [[#3. El Flujo de Trabajo Básico de las Migraciones ♻️|3. El Flujo de Trabajo Básico de las Migraciones ♻️]]
- [[#4. Comandos Útiles de Migraciones 🛠️|4. Comandos Útiles de Migraciones 🛠️]]
- [[#5. ¿Qué Hay Dentro de un Archivo de Migración? 📜 (Un Vistazo Rápido)|5. ¿Qué Hay Dentro de un Archivo de Migración? 📜 (Un Vistazo Rápido)]]
- [[#6. Buenas Prácticas y Consejos 💡|6. Buenas Prácticas y Consejos 💡]]
- [[#Mini Repaso Final 📝|Mini Repaso Final 📝]]

---
## 1. ¿Qué Rayos Son las Migraciones? 🤔
- Imagina que las migraciones son como un **sistema de control de versiones (tipo Git) pero para la estructura de tu base de datos**. ¡Así de geniales son!
- Cuando cambias tus modelos en Django (por ejemplo, añades un nuevo campo a una tabla, creas un modelo nuevo, o eliminas uno), necesitas una forma de decirle a tu base de datos real: "¡Oye, actualízate con estos cambios!".
- Las migraciones son **archivos de Python** que Django genera. Estos archivos **describen los cambios** que necesitas aplicar a tu esquema de base de datos para que coincida con tus modelos.
- En lugar de escribir comandos SQL complicados (como `ALTER TABLE`, `CREATE TABLE`) tú mismo, Django lo hace por ti (la mayor parte del tiempo) a través de estas migraciones.
---
## 2. ¿Por Qué Son Tan Importantes? 🌟
Usar el sistema de migraciones de Django es fundamental por varias razones:
- **Sincronización**: Mantienen el **esquema de tu base de datos perfectamente sincronizado** con la definición de tus modelos en `models.py`. ¡No más discrepancias!
- **Trabajo en Equipo 🤝**: Permiten que varios desarrolladores trabajen en el mismo proyecto. Cada uno puede hacer cambios en los modelos, generar migraciones, y luego los demás pueden aplicar esas mismas migraciones para tener sus bases de datos al día.
- **Evolución del Proyecto 🌱**: Tu aplicación cambiará con el tiempo. Las migraciones te permiten **evolucionar la estructura de tu base de datos de forma controlada y rastreable**
- **Despliegue Simplificado 🚀**: Cuando despliegas tu aplicación en un nuevo servidor, simplemente corres las migraciones y Django se encarga de configurar la base de datos correctamente.
- **Abstracción de la Base de Datos**: Django escribe las migraciones de una forma que (en su mayoría) es independiente de la base de datos específica que estés usando (PostgreSQL, MySQL, SQLite, etc.). Django se encarga de traducir esas operaciones al SQL específico de tu BD.
- **Reversibilidad (con cuidado)**: Aunque no siempre es trivial, las migraciones están diseñadas para poder ser "deshechas" (rollback), lo que puede ser un salvavidas.
---
## 3. El Flujo de Trabajo Básico de las Migraciones ♻️
El proceso para trabajar con migraciones generalmente sigue estos tres pasos:
1. **Paso 1: Modificar tus Modelos** ✏️
    - Abres tu archivo `models.py` en alguna de tus apps de Django.
    - Haces los cambios que necesites:
        - Añadir un nuevo modelo (una nueva clase).
        - Añadir un nuevo campo a un modelo existente (un nuevo atributo).
        - Cambiar un campo (ej. cambiar `max_length` de un `CharField`, añadir `null=True`).
        - Eliminar un campo o un modelo.
        - Añadir o cambiar opciones de `Meta` en un modelo.
2. **Paso 2: Crear Archivos de Migración (`makemigrations`)** 📝
    - Una vez que has guardado tus cambios en `models.py`, vas a tu terminal y ejecutas:
        ```bash
        python manage.py makemigrations [nombre_de_la_app]
        ```
        - Si no especificas `[nombre_de_la_app]`, Django buscará cambios en todas tus apps.
        - **¿Qué hace este comando?** Django compara el estado actual de tus modelos con el estado registrado en los archivos de migración existentes. Si detecta diferencias, **genera un nuevo archivo de migración** (ej: `0002_auto_20250530_1030.py`) dentro de la carpeta `migrations/` de la app correspondiente.
        - Este archivo contiene **código Python que describe las operaciones** necesarias para llevar la base de datos desde el estado anterior al nuevo estado (ej: `migrations.CreateModel(...)`, `migrations.AddField(...)`, `migrations.AlterField(...)`).
3. **Paso 3: Aplicar las Migraciones a la Base de Datos (`migrate`)** ⚙️
    - Ya tienes tu "plan de cambios" (el archivo de migración). Ahora necesitas ejecutar ese plan en tu base de datos. En la terminal, ejecutas:
        ```bash
        python manage.py migrate [nombre_de_la_app] [nombre_de_la_migracion]
        ```
        - Si no especificas nada, Django aplicará todas las migraciones pendientes de todas las apps.
        - Si especificas `[nombre_de_la_app]`, solo aplicará las de esa app.
        - Si especificas `[nombre_de_la_app] [nombre_de_la_migracion]`, aplicará las migraciones de esa app hasta la migración especificada (puede ser para aplicar o para deshacer).
        - **¿Qué hace este comando?** Django lee los archivos de migración que aún no se han aplicado. Para cada uno, traduce las operaciones de Python a **comandos SQL** y los ejecuta contra tu base de datos, modificando su estructura.
        - Django lleva un registro de qué migraciones ya se han aplicado en una tabla especial en tu base de datos llamada `django_migrations`. ¡Así sabe cuáles faltan por aplicar!
---
## 4. Comandos Útiles de Migraciones 🛠️
Además de `makemigrations` y `migrate`, hay otros comandos que te serán muy útiles:
- `python manage.py showmigrations [nombre_de_la_app]`
    - Te muestra una **lista de todas las migraciones** en tu proyecto (o en una app específica).
    - Marca con una `[X]` las que ya han sido aplicadas y con `[ ]` las que no. ¡Súper útil para saber dónde estás parado!
    ```
    app_blog
     [X] 0001_initial
     [X] 0002_articulo_fecha_actualizacion
     [ ] 0003_auto_20250531_0900
    ```
- `python manage.py sqlmigrate nombre_de_la_app nombre_de_la_migracion`
    - Ejemplo: `python manage.py sqlmigrate app_blog 0003_auto_20250531_0900`
    - Este comando **NO aplica la migración**, sino que te **muestra el SQL exacto** que Django ejecutaría para esa migración en tu base de datos configurada.
    - ¡Es genial para aprender, para depurar, o simplemente para tener curiosidad de qué está haciendo Django por debajo!
- **Deshacer Migraciones (Rollback)**:
    - Para deshacer la última migración aplicada en una app, puedes migrar a la migración _anterior_ a la última. Si `0003` es la última, y quieres volver al estado de `0002`:
        ```bash
        python manage.py migrate nombre_de_la_app 0002_nombre_de_la_migracion_anterior
        ```
    - Para deshacer todas las migraciones de una app (¡volver al inicio!), puedes usar `zero`:
        ```
        python manage.py migrate nombre_de_la_app zero
        ```
        
    - **¡MUCHO CUIDADO CON ESTO!** 💣 Deshacer migraciones puede implicar la **pérdida de datos** si la migración que estás deshaciendo eliminó una tabla o un campo que contenía información. Siempre ten respaldos, especialmente en producción.
---
## 5. ¿Qué Hay Dentro de un Archivo de Migración? 📜 (Un Vistazo Rápido)
Los archivos de migración que Django genera no son magia negra, ¡son Python! Si abres uno, verás algo así:
- Una clase llamada `Migration` que hereda de `django.db.migrations.Migration`.
- Un atributo `initial = True` si es la primera migración de la app.
- Un atributo `dependencies = []`: Una lista de otras migraciones de las que esta depende. Pueden ser migraciones anteriores de la misma app o migraciones de otras apps (ej. si tienes un `ForeignKey` a un modelo de otra app).
- Un atributo `operations = []`: ¡Esta es la parte importante! Es una lista de **objetos de operación** que le dicen a Django qué hacer.
    - `migrations.CreateModel(...)`: Para crear una nueva tabla (modelo).
    - `migrations.AddField(...)`: Para añadir una nueva columna (campo) a una tabla existente.
    - `migrations.AlterField(...)`: Para modificar un campo existente.
    - `migrations.DeleteModel(...)`: Para eliminar una tabla.
    - `migrations.RemoveField(...)`: Para eliminar un campo.
    - ¡Y muchas más! (`RenameModel`, `RenameField`, `RunPython` para código Python customizado, `RunSQL` para SQL crudo).
    _Ejemplo muy simplificado de una migración que crea un modelo:_
    ```python
    # Generated by Django X.Y on YYYY-MM-DD HH:MM
    from django.db import migrations, models
    import django.db.models.deletion # Si hay ForeignKeys
    
    class Migration(migrations.Migration):
    
        initial = True # O False si no es la primera
        dependencies = [
            # ('otra_app', '0001_initial'), # Ejemplo de dependencia
        ]
        operations = [
            migrations.CreateModel(
                name='MiNuevoModelo', # Nombre del modelo
                fields=[ # Lista de campos
                    ('id', models.AutoField(auto_created=True, primary_key=True, serialize=False, verbose_name='ID')),
                    ('nombre', models.CharField(max_length=100)),
                    ('descripcion', models.TextField(blank=True, null=True)),
                    # ('autor', models.ForeignKey(on_delete=django.db.models.deletion.CASCADE, to='otra_app.autor')),
                ],
            ),
        ]
    ```
---
## 6. Buenas Prácticas y Consejos 💡
- ✅ **Haz migraciones pequeñas y enfocadas**: Es mejor tener varias migraciones pequeñas que una gigante que cambie mil cosas. Facilita la depuración y el rollback si algo sale mal.
- 🔄 **Siempre ejecuta `makemigrations` DESPUÉS de cambiar `models.py`** (¡y antes de hacer `commit` a Git!).
- 💾 **Añade tus archivos de migración a tu control de versiones (Git)**. Son una parte esencial del código de tu proyecto. Sin ellos, otros (o tú en el futuro) no podrán replicar la estructura de la BD.
- 🧪 **Prueba las migraciones en desarrollo/staging ANTES de aplicarlas en producción**. ¡Siempre!
- 😬 **Evita editar archivos de migración manualmente** a menos que sepas MUY BIEN lo que estás haciendo. Es posible, y a veces necesario para casos complejos (como renombrar un campo sin perder datos de forma simple), pero es una zona de riesgo.
- 🗣️ **Si trabajas en equipo**:
    - Comunica bien los cambios en los modelos.
    - Antes de empezar a trabajar y hacer tus propios `makemigrations`, asegúrate de tener la última versión del código (con `git pull`) y corre `python manage.py migrate` para aplicar cualquier migración pendiente de tus compañeros. Esto evita conflictos de migraciones.
- 📛 **Nombres de migraciones**: Django los genera automáticamente (ej: `0002_auto_...`). Si necesitas hacer migraciones más especializadas, como "data migrations" (para manipular datos existentes) o migraciones vacías para operaciones personalizadas, puedes crearlas y nombrarlas:
    - `python manage.py makemigrations --empty nombre_de_tu_app --name mi_operacion_custom`
---
## Mini Repaso Final 📝
¡Las migraciones son tus aliadas para mantener tu base de datos al día con tus modelos de Django!
- Son como un **control de versiones para el esquema** de tu base de datos.
- El flujo principal es:
    1. **Modificas `models.py`**.
    2. Ejecutas `python manage.py makemigrations` para **crear el archivo de migración** (el "plan").
    3. Ejecutas `python manage.py migrate` para **aplicar los cambios a la base de datos** (ejecutar el "plan").
- Comandos clave: `makemigrations`, `migrate`, `showmigrations`, `sqlmigrate`.
- Recuerda: ¡son cruciales para el desarrollo individual, el trabajo en equipo y los despliegues!