---
aliases:
  - Tipos de Datos
tags:
  - sql
  - tablas
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-sql|📊 Índice SQL]]"
---
# Tipos de Datos
- [[#2. Tipos de Datos Numéricos 🔢|2. Tipos de Datos Numéricos 🔢]]
- [[#3. Tipos de Datos de Texto (Cadenas) 🔤|3. Tipos de Datos de Texto (Cadenas) 🔤]]
- [[#4. Tipos de Datos de Fecha y Hora 📅⏰|4. Tipos de Datos de Fecha y Hora 📅⏰]]
- [[#5. Tipo de Dato Booleano ✅❌|5. Tipo de Dato Booleano ✅❌]]
- [[#6. ¡Ojo! Variaciones entre SGBD 🚨|6. ¡Ojo! Variaciones entre SGBD 🚨]]
- [[#7. Ejemplo con CREATE TABLE ✏️|7. Ejemplo con CREATE TABLE ✏️]]
- [[#Mini Repaso Final ✨|Mini Repaso Final ✨]]

## 1. ¿Por qué son Importantes los Tipos de Datos? 🤔

- Le dicen a la base de datos **cómo almacenar** la información en esa columna (cuánta memoria usar, cómo interpretarla).
- Aseguran la **integridad de los datos**: la base de datos se asegura de que solo metas el tipo de dato correcto en la columna correcta (no puedes meter texto en una columna de números, por ejemplo).
- Permiten hacer **operaciones correctas**: puedes sumar números, comparar fechas, pero no sumar texto.
- Ayudan a **optimizar el rendimiento** y el espacio de almacenamiento.

## 2. Tipos de Datos Numéricos 🔢

Se usan para guardar números. Hay diferentes tipos dependiendo de si necesitas enteros o decimales, y del rango de valores que esperas guardar.

- **`INT`** o **`INTEGER`**: Números enteros (sin decimales). Es uno de los más comunes.
    - _Ejemplo:_ ID de un empleado, cantidad de productos en stock.
- **`SMALLINT`**, **`MEDIUMINT`**, **`BIGINT`**: Variaciones de `INT` para enteros más pequeños o más grandes. Usan diferente cantidad de memoria. Elige el más adecuado para el rango de números que necesitas.
- **`DECIMAL(p, s)`** o **`NUMERIC(p, s)`**: Números decimales de **precisión y escala fijas**. `p` es la precisión total (número total de dígitos), `s` es la escala (número de dígitos después del punto decimal). Es ideal para valores monetarios o cálculos exactos.
    - _Ejemplo:_ Precio de un producto (`DECIMAL(10, 2)` podría guardar hasta 10 dígitos en total, 2 de ellos después del punto).
- **`FLOAT`**, **`REAL`**, **`DOUBLE`** o **`DOUBLE PRECISION`**: Números decimales de **precisión flotante**. Son buenos para cálculos científicos o donde la precisión exacta no es crítica, pero pueden tener pequeños errores de redondeo. Usan más o menos memoria dependiendo de si son `FLOAT` (precisión simple) o `DOUBLE` (precisión doble).

## 3. Tipos de Datos de Texto (Cadenas) 🔤

Se usan para guardar caracteres y cadenas de texto.

- **`VARCHAR(n)`**: Cadena de caracteres de **longitud variable**. `n` es el número máximo de caracteres que puede almacenar. Solo ocupa el espacio necesario para el texto guardado (más un pequeño extra para saber la longitud). ¡Es el más común para texto!
    - _Ejemplo:_ Nombre de un cliente (`VARCHAR(100)`), dirección (`VARCHAR(255)`).
- **`CHAR(n)`**: Cadena de caracteres de **longitud fija**. Siempre ocupa `n` caracteres, rellenando con espacios si el texto es más corto. Es útil si sabes que el texto siempre tendrá una longitud fija (como códigos postales en algunos países), pero generalmente `VARCHAR` es más eficiente.
    - _Ejemplo:_ Código de país (`CHAR(2)`).
- **`TEXT`** (y variantes como `TINYTEXT`, `MEDIUMTEXT`, `LONGTEXT` en MySQL), **`VARCHAR(MAX)`** (SQL Server): Para guardar cadenas de texto muy largas. No especificas un tamaño fijo `n`.

## 4. Tipos de Datos de Fecha y Hora 📅⏰

Se usan para guardar información de tiempo.

- **`DATE`**: Guarda solo la fecha (año, mes, día).
    - _Ejemplo:_ Fecha de nacimiento, fecha de contratación.
- **`TIME`**: Guarda solo la hora (hora, minuto, segundo).
    - _Ejemplo:_ Hora de inicio de un evento.
- **`DATETIME`**: Guarda la fecha y la hora.
- **`TIMESTAMP`**: Guarda la fecha y la hora, a menudo incluyendo información de zona horaria y/o actualizándose automáticamente. Puede variar su comportamiento entre SGBD.

## 5. Tipo de Dato Booleano ✅❌

Se usa para guardar valores que son verdaderos o falsos.

- **`BOOLEAN`** o **`BOOL`**: Guarda un valor de verdad (TRUE o FALSE).
    - _Ejemplo:_ Si un producto está disponible (TRUE) o no (FALSE).
- **¡Ojo!** No todos los SGBD tienen un tipo `BOOLEAN` nativo. Algunos usan un tipo numérico pequeño (como `TINYINT(1)` o `BIT`) donde 0 es falso y 1 es verdadero.

## 6. ¡Ojo! Variaciones entre SGBD 🚨

Es súper importante recordar que aunque los nombres y conceptos son similares, la **implementación exacta** de los tipos de datos puede variar un poco entre diferentes Sistemas de Gestión de Base de Datos (MySQL, PostgreSQL, SQL Server, Oracle, SQLite, etc.). ¡Siempre revisa la documentación específica del SGBD que estás usando!

## 7. Ejemplo con CREATE TABLE ✏️

Volviendo a nuestra tabla `Personal`, usando algunos de estos tipos de datos:

```sql
-- Creamos (o recreamos si la borramos antes) la tabla Personal con tipos de datos más específicos
CREATE TABLE Personal (
    id_empleado INT PRIMARY KEY, -- Número entero para el ID
    nombre VARCHAR(100), -- Texto variable hasta 100 caracteres para el nombre
    puesto VARCHAR(50), -- Texto variable hasta 50 caracteres para el puesto
    fecha_contratacion DATE, -- Solo guarda la fecha
    email VARCHAR(100) UNIQUE, -- Texto variable hasta 100 caracteres, debe ser único
    salario DECIMAL(10, 2), -- Número decimal con 10 dígitos en total, 2 después del punto
    activo BOOLEAN -- O podría ser TINYINT(1) en algunos SGBD, para saber si está activo (verdadero/falso)
);
```

Aquí definimos claramente qué tipo de información esperamos en cada columna.

## Mini Repaso Final ✨

¡Para terminar! Los **tipos de datos** son esenciales para definir qué tipo de información puede contener cada columna en una base de datos. Ayudan a la integridad, el almacenamiento y las operaciones correctas. Los tipos más comunes son:

- **Numéricos**: `INT`, `DECIMAL`, `FLOAT` para enteros y decimales. 🔢
- **Texto**: `VARCHAR`, `CHAR`, `TEXT` para cadenas de caracteres. 🔤
- **Fecha/Hora**: `DATE`, `TIME`, `DATETIME`, `TIMESTAMP`. 📅⏰
- **Booleano**: `BOOLEAN` (o similar) para verdadero/falso. ✅❌ ¡Y siempre ten en cuenta que los nombres y detalles pueden variar un poco entre diferentes SGBD! 😉