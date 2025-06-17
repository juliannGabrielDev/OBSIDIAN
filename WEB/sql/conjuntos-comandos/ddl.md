---
aliases:
  - Data Definition Language
tags:
  - sql
  - comandos
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-sql|📊 Índice SQL]]"
---
# Data Definition Language
- [[#1. ¿Qué es DDL? 🤔|1. ¿Qué es DDL? 🤔]]
- [[#2. CREATE: Construyendo Objetos 🏗️|2. CREATE: Construyendo Objetos 🏗️]]
- [[#3. ALTER: Modificando la Estructura 🔧|3. ALTER: Modificando la Estructura 🔧]]
- [[#4. DROP: Eliminando Objetos 👋|4. DROP: Eliminando Objetos 👋]]
- [[#5. TRUNCATE: Vaciando Tablas Rápido 🧹|5. TRUNCATE: Vaciando Tablas Rápido 🧹]]
- [[#6. RENAME: Cambiando Nombres ✏️|6. RENAME: Cambiando Nombres ✏️]]
- [[#Mini Repaso Final ✨|Mini Repaso Final ✨]]

## 1. ¿Qué es DDL? 🤔

- **DDL** significa **Data Definition Language** (Lenguaje de Definición de Datos).
- Son los comandos de SQL que usamos para **definir, modificar o eliminar la estructura** (el "esqueleto" o schema) de los objetos de la base de datos.
- Piensa en ellos como los planos y las herramientas para construir o cambiar el edificio de la base de datos, no para poner o quitar los muebles (eso sería DML).
- Los comandos DDL **afectan la estructura**, no los datos que están dentro.
- Cuando ejecutas un comando DDL, generalmente se aplica de forma **permanente** y **automática** (implícitamente commit). ¡Así que hay que tener cuidado! ⚠️

## 2. CREATE: Construyendo Objetos 🏗️

- Este comando se usa para **crear nuevos objetos** en la base de datos.
    
- ¿Qué objetos puedes crear? Los más comunes son:
    
    - **DATABASE**: Para crear una nueva base de datos completa.
    - **TABLE**: Para crear una nueva tabla.
    - **INDEX**: Para crear índices (ayudan a buscar datos más rápido).
    - **VIEW**: Para crear vistas (tablas virtuales basadas en consultas).
    - **SCHEMA**: Para crear un esquema (una colección de objetos).
- **Sintaxis básica (para DATABASE y TABLE):**
    
    ```sql
    -- Crear una base de datos
    CREATE DATABASE nombre_base_de_datos;
    
    -- Crear una tabla
    CREATE TABLE nombre_tabla (
        columna1 tipo_dato restricciones,
        columna2 tipo_dato restricciones,
        ...
        restricciones_tabla (PRIMARY KEY, FOREIGN KEY, etc.)
    );
    ```
    
- Ejemplo:
    
    ```sql
    -- Creamos una base de datos para nuestra tienda
    CREATE DATABASE MiTienda;
    
    -- Después de crear la base de datos, la seleccionamos para trabajar en ella (el comando puede variar según el SGBD, pero 'USE' es común)
    -- USE MiTienda;
    
    -- Creamos una tabla simple para los empleados
    CREATE TABLE Empleados (
        id_empleado INT PRIMARY KEY, -- Llave primaria
        nombre VARCHAR(100),
        puesto VARCHAR(50),
        fecha_contratacion DATE
    );
    ```
    

¡Listo! Ya tenemos la estructura básica para guardar info de empleados.

## 3. ALTER: Modificando la Estructura 🔧

- Este comando se usa para **modificar la estructura** de un objeto existente.
    
- Lo más común es usar `ALTER TABLE` para cambiar la definición de una tabla.
    
- ¿Qué puedes hacer con `ALTER TABLE`?
    
    - **ADD COLUMN**: Añadir una nueva columna.
    - **DROP COLUMN**: Eliminar una columna existente.
    - **MODIFY COLUMN** (o `ALTER COLUMN` según el SGBD): Cambiar el tipo de dato o las restricciones de una columna.
    - **ADD CONSTRAINT**: Añadir restricciones (como PRIMARY KEY, FOREIGN KEY, UNIQUE, CHECK).
    - **DROP CONSTRAINT**: Eliminar restricciones.
- **Sintaxis básica (para TABLE):**
    
    ```sql
    -- Añadir una columna
    ALTER TABLE nombre_tabla
    ADD COLUMN nueva_columna tipo_dato restricciones;
    
    -- Eliminar una columna
    ALTER TABLE nombre_tabla
    DROP COLUMN nombre_columna_a_eliminar;
    
    -- Modificar una columna (sintaxis varía)
    -- Ejemplo (MySQL):
    -- ALTER TABLE nombre_tabla
    -- MODIFY COLUMN columna_a_modificar nuevo_tipo_dato nuevas_restricciones;
    -- Ejemplo (SQL Server/PostgreSQL):
    -- ALTER TABLE nombre_tabla
    -- ALTER COLUMN columna_a_modificar TYPE nuevo_tipo_dato; -- o SET NOT NULL, DROP NOT NULL, etc.
    ```
    
- Ejemplo:
    
    ```sql
    -- A nuestra tabla Empleados le falta el email, ¡vamos a añadirlo!
    ALTER TABLE Empleados
    ADD COLUMN email VARCHAR(100) UNIQUE; -- Añadimos la columna email y le ponemos restricción UNIQUE (no emails repetidos)
    
    -- Ahora nos damos cuenta que el campo puesto debería permitir más caracteres
    -- Ejemplo usando sintaxis tipo MySQL:
    -- ALTER TABLE Empleados
    -- MODIFY COLUMN puesto VARCHAR(100);
    
    -- Si quisiéramos eliminar la columna fecha_contratacion (¡cuidado con esto!)
    -- ALTER TABLE Empleados
    -- DROP COLUMN fecha_contratacion;
    ```
    

`ALTER` es súper útil cuando necesitas ajustar tu diseño de base de datos sobre la marcha.

## 4. DROP: Eliminando Objetos 👋

- Este comando se usa para **eliminar completamente un objeto** de la base de datos.
    
- Al usar `DROP`, **se elimina la estructura Y todos los datos** que contenía el objeto. ¡Es como demoler el edificio! 💥
    
- Puedes eliminar bases de datos, tablas, índices, vistas, etc.
    
- **Sintaxis básica:**
    
    ```sql
    -- Eliminar una base de datos
    DROP DATABASE nombre_base_de_datos;
    
    -- Eliminar una tabla
    DROP TABLE nombre_tabla;
    ```
    
- Ejemplo:
    
    ```sql
    -- Si ya no necesitamos la tabla Empleados (¡piénsalo bien antes de hacer esto!)
    DROP TABLE Empleados;
    
    -- Si quisiéramos eliminar toda la base de datos MiTienda (¡MUCHO cuidado aquí!)
    -- DROP DATABASE MiTienda;
    ```
    

`DROP` es potente y peligroso si no estás seguro. ¡Úsalo con precaución!

## 5. TRUNCATE: Vaciando Tablas Rápido 🧹

- Este comando se usa para **eliminar TODAS las filas** de una tabla.
    
- A diferencia de `DROP TABLE`, `TRUNCATE TABLE` **mantiene la estructura** de la tabla intacta. Es como vaciar completamente el edificio pero dejando la estructura en pie.
    
- Es generalmente **más rápido y eficiente** que usar `DELETE FROM nombre_tabla;` (que es un comando DML) para eliminar todas las filas, especialmente en tablas muy grandes.
    
- `TRUNCATE` suele **reiniciar los contadores** de auto-incremento (como el ID que se genera automáticamente).
    
- **Sintaxis básica:**
    
    ```sql
    TRUNCATE TABLE nombre_tabla
    ```
    
- Ejemplo:
    
    ```sql
    -- Si quisiéramos borrar todos los registros de empleados pero mantener la tabla para añadir nuevos
    TRUNCATE TABLE Empleados;
    ```
    

Útil para "resetear" una tabla sin tener que recrearla.

## 6. RENAME: Cambiando Nombres ✏️

- Este comando se usa para **cambiar el nombre** de un objeto de la base de datos.
    
- La sintaxis puede variar un poco entre los diferentes SGBD. A veces es un comando `RENAME` directo, otras veces se usa `ALTER TABLE RENAME TO`.
    
- **Sintaxis básica (puede variar):**
    
    ```sql
    -- Cambiar nombre de tabla (Ejemplo con ALTER TABLE)
    ALTER TABLE nombre_tabla_actual
    RENAME TO nuevo_nombre_tabla;
    
    -- Cambiar nombre de columna (Ejemplo con ALTER TABLE, varía mucho)
    -- ALTER TABLE nombre_tabla
    -- RENAME COLUMN nombre_columna_actual TO nuevo_nombre_columna;
    ```
    
- Ejemplo (usando sintaxis común para tablas):
    
    ```sql
    -- Decidimos que 'Empleados' no es el mejor nombre, ¡vamos a cambiarlo a 'Personal'!
    ALTER TABLE Empleados
    RENAME TO Personal
    ```
    

`RENAME` es útil para refactorizar o mejorar la claridad de los nombres de tus objetos.

## Mini Repaso Final ✨

¡En resumen! Los **comandos DDL** (Data Definition Language) son la parte de SQL que usamos para **gestionar la estructura** de nuestra base de datos. Los más importantes son:

- **`CREATE`**: Para crear bases de datos, tablas, etc. 🏗️
- **`ALTER`**: Para modificar la estructura de objetos existentes (añadir/quitar columnas, cambiar tipos, etc.). 🔧
- **`DROP`**: Para eliminar completamente objetos y sus datos. 👋
- **`TRUNCATE`**: Para vaciar rápidamente todas las filas de una tabla, manteniendo la estructura. 🧹
- **`RENAME`**: Para cambiar el nombre de objetos. ✏️

¡Dominar estos comandos es clave para poder diseñar y mantener tu base de datos! 💪