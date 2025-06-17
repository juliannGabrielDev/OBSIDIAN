---
aliases:
  - Data Manipulation Language
tags:
  - sql
  - comandos
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-sql|📊 Índice SQL]]"
---
# Data Manipulation Language
- [[#1. ¿Qué es DML? (y la diferencia con DDL) 🤔|1. ¿Qué es DML? (y la diferencia con DDL) 🤔]]
- [[#2. SELECT: Consultando Datos 🔍|2. SELECT: Consultando Datos 🔍]]
- [[#3. INSERT: Añadiendo Nuevos Datos ➕|3. INSERT: Añadiendo Nuevos Datos ➕]]
- [[#4. UPDATE: Modificando Datos Existentes ✍️|4. UPDATE: Modificando Datos Existentes ✍️]]
- [[#5. DELETE: Eliminando Filas ❌|5. DELETE: Eliminando Filas ❌]]
- [[#6. Ojo: DML y Transacciones (Commit/Rollback) 🔄💾|6. Ojo: DML y Transacciones (Commit/Rollback) 🔄💾]]
- [[#Mini Repaso Final ✨|Mini Repaso Final ✨]]


## 1. ¿Qué es DML? (y la diferencia con DDL) 🤔

- **DML** significa **Data Manipulation Language** (Lenguaje de Manipulación de Datos).
- Son los comandos de SQL que usamos para **interactuar con los DATOS** que están _dentro_ de las tablas. 📄✍️🗑️
- A diferencia de DDL que cambia la _estructura_ (tablas, columnas), DML cambia el _contenido_ de las tablas (las filas de datos).
- Piensa en DML como usar los muebles y objetos dentro del edificio que construiste con DDL. Pones cosas, las usas, las cambias de lugar, o las tiras.

## 2. SELECT: Consultando Datos 🔍

- ¡Este es probablemente el comando DML que más usarás! 😉
    
- Se usa para **recuperar (leer) datos** de una o varias tablas.
    
- Te permite especificar qué columnas quieres ver, de qué tabla(s), y qué filas cumplen ciertas condiciones.
    
- **Sintaxis básica:**
    
    ```sql
    SELECT columna1, columna2, ... -- Qué columnas quieres ver
    FROM nombre_table -- De qué tabla
    WHERE condition; -- (Opcional) Qué filas quieres incluir (filtra filas)
    ```
    

Puedes usar `*` para seleccionar todas las columnas.

- Ejemplos:
    
    ```sql
    -- Seleccionar todas las columnas de todos los empleados
    SELECT *
    FROM Personal; -- Usamos el nombre 'Personal' de nuestro ejemplo DDL
    
    -- Seleccionar solo el nombre y puesto de los empleados
    SELECT nombre, puesto
    FROM Personal;
    
    -- Seleccionar empleados que son 'Ingeniero'
    SELECT id_empleado, nombre
    FROM Personal
    WHERE puesto = 'Ingeniero';
    
    -- Seleccionar empleados cuyo ID es mayor a 100
    SELECT *
    FROM Personal
    WHERE id_empleado > 100;
    ```
    

El `WHERE` es clave para filtrar y obtener solo los datos que te interesan. ¡Super potente!

## 3. INSERT: Añadiendo Nuevos Datos ➕

- Este comando se usa para **agregar nuevas filas de datos** a una tabla existente.
    
- **Sintaxis básica:**
    
    ```sql
    INSERT INTO nombre_tabla (columna1, columna2, ...) -- En qué tabla y qué columnas (opcional si insertas en todas)
    VALUES (valor1, valor2, ...); -- Los valores para esas columnas, en el mismo orden
    ```
    

Si vas a insertar valores en _todas_ las columnas de la tabla en el orden en que fueron definidas, puedes omitir la lista de columnas después del nombre de la tabla, pero ¡no es una buena práctica! Es mejor siempre especificar las columnas.

- Ejemplos:
    
    ```sql
    -- Añadir un nuevo empleado a la tabla Personal
    INSERT INTO Personal (id_empleado, nombre, puesto, fecha_contratacion, email)
    VALUES (101, 'Ana Gómez', 'Desarrollador', '2024-01-15', 'ana.gomez@ejemplo.com');
    
    -- Añadir otro empleado (si la columna email permite NULL)
    INSERT INTO Personal (id_empleado, nombre, puesto, fecha_contratacion)
    VALUES (102, 'Luis Pérez', 'Diseñador', '2024-02-20');
    ```
    

¡Así es como llenamos nuestras tablas con información!

## 4. UPDATE: Modificando Datos Existentes ✍️

- Este comando se usa para **cambiar los valores** en filas existentes de una tabla.
    
- **Sintaxis básica:**
    
    ```sql
    UPDATE nombre_tabla -- Qué tabla vas a modificar
    SET columna1 = nuevo_valor1, -- Qué columna(s) y su nuevo valor
        columna2 = nuevo_valor2, ...
    WHERE condicion; -- (¡CRÍTICO!) Qué filas vas a actualizar
    ```
    

**¡MUCHO CUIDADO!** Si olvidas la cláusula `WHERE`, ¡actualizarás _todas_ las filas de la tabla! 😱

- **Ejemplos:**
    
    ```sql
    -- Cambiar el puesto del empleado con ID 101
    UPDATE Personal
    SET puesto = 'Ingeniero Senior'
    WHERE id_empleado = 101;
    
    -- Corregir el email de un empleado
    UPDATE Personal
    SET email = 'luis.p@ejemplo.com'
    WHERE nombre = 'Luis Pérez'; -- Puedes usar otras columnas en el WHERE
    
    -- Aumentar el sueldo (si tuviéramos columna sueldo) a todos los ingenieros (ejemplo hipotético)
    -- UPDATE Personal
    -- SET sueldo = sueldo * 1.10
    -- WHERE puesto = 'Ingeniero';
    ```
    

`UPDATE` es esencial para mantener la información al día.

## 5. DELETE: Eliminando Filas ❌

- Este comando se usa para **eliminar una o varias filas** de datos de una tabla.
    
- **Sintaxis básica:**
    
    ```sql
    DELETE FROM nombre_tabla -- De qué tabla vas a eliminar
    WHERE condicion; -- (¡CRÍTICO!) Qué filas vas a eliminar
    ```
    

**¡OTRO GRAN CUIDADO!** Si olvidas la cláusula `WHERE`, ¡eliminarás _todas_ las filas de la tabla! 😵

- Ejemplo:
    
    ```sql
    -- Eliminar al empleado con ID 102
    DELETE FROM Personal
    WHERE id_empleado = 102;
    
    -- Eliminar a todos los empleados que no tienen email (si email puede ser NULL)
    -- DELETE FROM Personal
    -- WHERE email IS NULL;
    
    -- Eliminar a todos los empleados (¡vaciar la tabla!)
    -- DELETE FROM Personal; -- ¡Esto borra TODAS las filas! Es como un TRUNCATE, pero DML y más lento en tablas grandes.
    ```
    

`DELETE` te permite limpiar o remover datos que ya no son necesarios.

## 6. Ojo: DML y Transacciones (Commit/Rollback) 🔄💾

- A diferencia de los comandos DDL que se aplican de forma permanente inmediatamente, los comandos DML (INSERT, UPDATE, DELETE) suelen ejecutarse dentro de **transacciones**.
- Una transacción es una secuencia de una o más operaciones DML que se consideran una sola unidad lógica.
- Puedes usar los comandos `COMMIT;` para guardar los cambios de forma permanente en la base de datos, o `ROLLBACK;` para deshacer los cambios y volver al estado anterior a que empezara la transacción.
- Esto te da una capa de seguridad: puedes hacer varias operaciones DML y si algo sale mal, puedes deshacerlas todas con `ROLLBACK`.

## Mini Repaso Final ✨

¡Para cerrar! Los **comandos DML** (Data Manipulation Language) son tu día a día al trabajar con bases de datos. Te permiten **manipular los datos** que están dentro de la estructura creada por DDL:

- **`SELECT`**: Para leer o consultar datos. 🔍
- **`INSERT`**: Para añadir nuevas filas. ➕
- **`UPDATE`**: Para modificar datos existentes. ✍️
- **`DELETE`**: Para eliminar filas. ❌ ¡Y recuerda que las operaciones DML se pueden agrupar en transacciones y deshacer (`ROLLBACK`) si es necesario! Son las herramientas para interactuar directamente con la información. 💪