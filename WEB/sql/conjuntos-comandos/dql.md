---
aliases:
  - Data Query Language
tags:
  - sql
  - comandos
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-sql|📊 Índice SQL]]"
---
# Data Query Language
- [[#1. ¿Qué es DQL? (Consultando Datos) 📊|1. ¿Qué es DQL? (Consultando Datos) 📊]]
- [[#2. SELECT: El Rey del DQL 👑|2. SELECT: El Rey del DQL 👑]]
- [[#3. Cláusulas Comunes en SELECT ✨|3. Cláusulas Comunes en SELECT ✨]]
	- [[#3. Cláusulas Comunes en SELECT ✨#a) FROM: De Dónde Viene la Info 📥|a) FROM: De Dónde Viene la Info 📥]]
	- [[#3. Cláusulas Comunes en SELECT ✨#b) WHERE: Filtrando Filas 🔍|b) WHERE: Filtrando Filas 🔍]]
	- [[#3. Cláusulas Comunes en SELECT ✨#c) GROUP BY: Agrupando Resultados 📦|c) GROUP BY: Agrupando Resultados 📦]]
	- [[#3. Cláusulas Comunes en SELECT ✨#d) HAVING: Filtrando Grupos 🕵️‍♀️|d) HAVING: Filtrando Grupos 🕵️‍♀️]]
	- [[#3. Cláusulas Comunes en SELECT ✨#e) ORDER BY: Ordenando los Resultados ↕️|e) ORDER BY: Ordenando los Resultados ↕️]]
	- [[#3. Cláusulas Comunes en SELECT ✨#f) LIMIT / TOP: Limitando la Cantidad 🤏|f) LIMIT / TOP: Limitando la Cantidad 🤏]]
- [[#Mini Repaso Final ✨|Mini Repaso Final ✨]]


## 1. ¿Qué es DQL? (Consultando Datos) 📊

- **DQL** significa **Data Query Language** (Lenguaje de Consulta de Datos).
- El propósito principal del DQL es **recuperar (consultar) datos** de la base de datos sin modificarlos. Es el comando para "leer" la información.
- Es como ir a la biblioteca de tu base de datos y pedir libros específicos basados en criterios. 📚
- El comando estrella del DQL es **`SELECT`**. ✨

## 2. SELECT: El Rey del DQL 👑

- Ya lo mencionamos en DML, pero `SELECT` es tan fundamental para consultar que se le considera el comando principal de DQL.
    
- Te permite especificar exactamente qué datos quieres ver y cómo los quieres ver.
    
- **Sintaxis básica (recordatorio):**
    
    ```sql
    SELECT columna1, columna2, ... -- Qué columnas
    FROM nombre_tabla -- De qué tabla(s)
    WHERE condicion; -- (Opcional) Filtrar filas
    ```
    

Pero `SELECT` es mucho más que esto, ¡tiene un montón de cláusulas para refinar tu consulta!

## 3. Cláusulas Comunes en SELECT ✨

El orden en que se suelen escribir (y procesar lógicamente) las cláusulas en una sentencia `SELECT` completa es importante:

`SELECT ... FROM ... WHERE ... GROUP BY ... HAVING ... ORDER BY ... LIMIT/TOP ...`

Vamos a ver las más importantes:

### a) FROM: De Dónde Viene la Info 📥

- Ya lo sabes, esta cláusula es **obligatoria** en casi todas las consultas `SELECT`.
    
- Indica la **tabla o tablas** de las que quieres recuperar los datos.
    
- Aquí es donde usas los **`JOIN`** (que vimos en relaciones) para combinar datos de múltiples tablas.
    
- **Ejemplo:**
    
    ```sql
    SELECT nombre, puesto
    FROM Personal; -- Seleccionamos de la tabla Personal
    
    -- O usando JOIN para obtener pedidos y el nombre del cliente
    SELECT P.id_pedido, C.nombre
    FROM Pedidos P
    JOIN Clientes C ON P.id_cliente = C.id_cliente;
    ```
    

### b) WHERE: Filtrando Filas 🔍

- Ya lo usamos también, esta cláusula es **opcional** pero súper común.
    
- Se usa para **filtrar las filas** y obtener solo aquellas que cumplen una o más **condiciones**.
    
- La condición usa operadores de comparación (`=`, `!=`, `>`, `<`, `>=`, `<=`) y operadores lógicos (`AND`, `OR`, `NOT`).
- Operadores especiales para `WHERE`:
	- `BETWEEN`: Se usa para seleccionar valores dentro de un rango específico (inclusive). Es muy útil para fechas, números o cadenas.
		```sql
		SELECT nombre, fecha_contratacion 
		FROM Personal 
		WHERE fecha_contratacion BETWEEN '2022-01-01' AND '2022-12-31'; 
		-- Empleados contratados en 2022 SELECT nombre_producto, precio FROM Productos WHERE precio BETWEEN 10.00 AND 50.00; -- Productos con precio entre 10 y 50
		```
	- `LIKE`: Se usa para buscar un patrón específico en una columna de texto. Se combina con caracteres comodín:
		- `%`: Representa cero o más caracteres.
		- `_`: Representa un solo carácter.
		```sql
		SELECT nombre, email
		FROM Personal WHERE email
		LIKE '%@ejemplo.com'; -- Emails que terminan en @ejemplo.com SELECT nombre_cliente FROM Clientes WHERE nombre_cliente LIKE 'A%'; -- Clientes cuyo nombre empieza con 'A' SELECT nombre_producto FROM Productos WHERE nombre_producto LIKE '_a%'; -- Productos cuyo segundo carácter es 'a'
		```
	- `IN`: Se usa para especificar múltiples valores posibles para una columna en la cláusula `WHERE`. Es una forma concisa de escribir múltiples condiciones `OR`.
		```sql
		SELECT nombre, puesto 
		FROM Personal
		WHERE puesto IN ('Desarrollador', 'Diseñador', 'Analista'); -- Empleados que son desarrolladores, diseñadores o analistas SELECT id_pedido, total FROM Pedidos WHERE id_pedido IN (101, 105, 203); -- Pedidos con IDs específicos
		```
- Ejemplo general de `WHERE`:
    
    ```sql
    SELECT *
    FROM Personal
    WHERE puesto = 'Desarrollador'; -- Solo queremos los desarrolladores
    
    SELECT nombre, email
    FROM Personal
    WHERE fecha_contratacion > '2023-01-01' AND puesto = 'Ingeniero'; -- Empleados contratados después de 2023 Y que son ingenieros
    ```
    

### c) GROUP BY: Agrupando Resultados 📦

- Esta cláusula se usa para **agrupar filas** que tienen los mismos valores en una o más columnas.
    
- Es muy útil cuando quieres realizar **funciones de agregación** (como `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`) sobre cada grupo.
    
- Por ejemplo, para contar cuántos empleados hay por cada puesto.
    
- **Sintaxis básica:**
    
    ```sql
    SELECT columna_para_agrupar, funcion_agregacion(columna_a_calcular)
    FROM nombre_tabla
    WHERE condicion -- Opcional, filtra filas ANTES de agrupar
    GROUP BY columna_para_agrupar;
    ```
    
- Ejemplo:
    
    ```sql
    -- Contar cuántos empleados hay por cada puesto
    SELECT puesto, COUNT(*) AS total_por_puesto
    FROM Personal
    GROUP BY puesto;
    
    -- Calcular el promedio de total de pedido por cliente (usando tablas Pedidos y Clientes)
    -- SELECT C.nombre, AVG(P.total) AS promedio_pedido
    -- FROM Pedidos P
    -- JOIN Clientes C ON P.id_cliente = C.id_cliente
    -- GROUP BY C.nombre; -- Agrupamos por el nombre del cliente
    ```
    

`GROUP BY` condensa múltiples filas en una sola fila por cada grupo.

### d) HAVING: Filtrando Grupos 🕵️‍♀️

- Esta cláusula es **opcional** y **siempre va después de `GROUP BY`**.
    
- Se usa para **filtrar los grupos** creados por la cláusula `GROUP BY`.
    
- A diferencia de `WHERE` (que filtra filas individuales), `HAVING` filtra grupos completos basados en una condición que usualmente involucra funciones de agregación.
    
- **Sintaxis básica:**
    
    ```sql
    SELECT columna_para_agrupar, funcion_agregacion(columna_a_calcular)
    FROM nombre_tabla
    WHERE condicion -- Opcional, filtra filas ANTES de agrupar
    GROUP BY columna_para_agrupar
    HAVING condicion_para_grupo; -- Condición que filtra grupos
    ```
    
- Ejemplo:
    
    ```sql
    -- Contar cuántos empleados hay por puesto, pero solo mostrar los puestos que tienen más de 5 empleados
    SELECT puesto, COUNT(*) AS total_por_puesto
    FROM Personal
    GROUP BY puesto
    HAVING COUNT(*) > 5; -- Filtramos los grupos donde el conteo es mayor a 5
    
    -- Mostrar clientes cuyo promedio de pedido es mayor a 100 (usando tablas Pedidos y Clientes)
    -- SELECT C.nombre, AVG(P.total) AS promedio_pedido
    -- FROM Pedidos P
    -- JOIN Clientes C ON P.id_cliente = C.id_cliente
    -- GROUP BY C.nombre
    -- HAVING AVG(P.total) > 100; -- Filtramos los grupos donde el promedio es mayor a 100
    ```
    

`HAVING` es como el `WHERE` para los resultados de `GROUP BY`.

### e) ORDER BY: Ordenando los Resultados ↕️

- Esta cláusula es **opcional**.
    
- Se usa para **ordenar el conjunto de resultados** de la consulta.
    
- Puedes ordenar por una o más columnas, de forma ascendente (`ASC`, por defecto) o descendente (`DESC`).
    
- **Sintaxis básica:**
    
    ```sql
    SELECT columna1, columna2, ...
    FROM nombre_tabla
    WHERE ...
    GROUP BY ...
    HAVING ...
    ORDER BY columna_a_ordenar ASC/DESC; -- Ordena por esta columna
    ```
    
- Ejemplo:
    
    ```sql
    -- Seleccionar todos los empleados ordenados por nombre alfabéticamente
    SELECT *
    FROM Personal
    ORDER BY nombre ASC; -- ASC es opcional, es el default
    
    -- Seleccionar empleados ordenados por fecha de contratación, del más nuevo al más antiguo
    SELECT id_empleado, nombre, fecha_contratacion
    FROM Personal
    ORDER BY fecha_contratacion DESC;
    
    -- Ordenar los puestos por la cantidad de empleados que tienen, del más numeroso al menos
    SELECT puesto, COUNT(*) AS total_por_puesto
    FROM Personal
    GROUP BY puesto
    ORDER BY total_por_puesto DESC; -- O ORDER BY 2 DESC; (ordenar por la segunda columna seleccionada)
    ```
    

`ORDER BY` hace que tus resultados sean mucho más legibles y útiles.

### f) LIMIT / TOP: Limitando la Cantidad 🤏

- Esta cláusula es **opcional** y se usa para **limitar el número de filas** que devuelve la consulta.
- La sintaxis varía: `LIMIT` es común en MySQL, PostgreSQL, SQLite; `TOP` se usa en SQL Server y MS Access.
- Es útil cuando solo necesitas los primeros N resultados, por ejemplo, para paginación.
- **Sintaxis básica:**

```sql
-- Ejemplo con LIMIT (MySQL, PostgreSQL, SQLite)
SELECT columna1, ...
FROM nombre_tabla
WHERE ...
ORDER BY ... -- Casi siempre se usa ORDER BY con LIMIT para que la "parte de arriba" sea significativa
LIMIT cantidad_filas;

-- Ejemplo con TOP (SQL Server)
SELECT TOP cantidad_filas columna1, ...
FROM nombre_tabla
WHERE ...
ORDER BY ... -- TOP también se suele usar con ORDER BY
```

- Ejemplo:

```sql
-- Obtener los 5 empleados más recientes (asumiendo fecha_contratacion es la fecha de inicio)
SELECT nombre, fecha_contratacion
FROM Personal
ORDER BY fecha_contratacion DESC
LIMIT 5; -- En SQL Server sería SELECT TOP 5 ...

-- Obtener los 10 productos más caros (asumiendo tabla Productos con columna precio)
-- SELECT nombre, precio
-- FROM Productos
-- ORDER BY precio DESC
-- LIMIT 10; -- En SQL Server sería SELECT TOP 10 ...
```

`LIMIT`/`TOP` es genial para optimizar y mostrar solo la cantidad de datos que necesitas.

## Mini Repaso Final ✨

¡Para terminar este repaso de DQL! El **Data Query Language** se trata de **consultar y recuperar datos** de la base de datos, y su comando principal es **`SELECT`**. 🎉 `SELECT` se puede usar con muchas cláusulas para refinar la consulta:

- **`FROM`**: De qué tabla(s) obtener los datos. 📥
- **`WHERE`**: Para filtrar filas individuales. 🔍
- **`GROUP BY`**: Para agrupar filas y usar funciones de agregación. 📦
- **`HAVING`**: Para filtrar los grupos después de agrupar. 🕵️‍♀️
- **`ORDER BY`**: Para ordenar los resultados. ↕️
- **`LIMIT`/`TOP`**: Para limitar el número de filas devueltas. 🤏

¡El comando `SELECT` con sus cláusulas es la herramienta más poderosa para obtener información útil de tu base de datos! ¡Es donde pasas la mayor parte del tiempo! 💪