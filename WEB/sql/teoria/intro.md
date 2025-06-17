---
aliases:
  - Introducción a las Bases de Datos
tags:
  - sql
  - teoria
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-sql|📊 Índice SQL]]"
---
# Introducción a las Bases de Datos
- [[#1. ¿Qué es una Base de Datos? 🤔|1. ¿Qué es una Base de Datos? 🤔]]
- [[#2. ¿Por qué necesitamos Bases de Datos? ✨|2. ¿Por qué necesitamos Bases de Datos? ✨]]
- [[#3. El Sistema de Gestión de Bases de Datos (DBMS) 🛠️|3. El Sistema de Gestión de Bases de Datos (DBMS) 🛠️]]
- [[#4. Bases de Datos Relacionales y SQL 🤝|4. Bases de Datos Relacionales y SQL 🤝]]
- [[#5. Estructura Básica: Tablas, Filas y Columnas 🧱|5. Estructura Básica: Tablas, Filas y Columnas 🧱]]
- [[#6. Ejemplo Básico de SQL: Creando una Tabla ✏️|6. Ejemplo Básico de SQL: Creando una Tabla ✏️]]
- [[#Mini Repaso Final ✨|Mini Repaso Final ✨]]

## 1. ¿Qué es una Base de Datos? 🤔

- Piensa en una base de datos como un **archivo gigante y super organizado** donde guardamos información. 📂
- No es solo un montón de datos tirados por ahí; ¡tiene una **estructura**! Esto hace que sea fácil encontrar, añadir o modificar la información.
- Imagínala como si fuera una colección de hojas de cálculo interconectadas, pero mucho más potente y segura.
- Su objetivo principal es **almacenar datos** de manera **eficiente, segura y organizada**.
## 2. ¿Por qué necesitamos Bases de Datos? ✨
- En el mundo real (y en la programación), manejamos **muchísima información**. Desde usuarios de una app, productos en una tienda online, hasta registros médicos o datos de sensores.
- Guardarla en archivos de texto simples o hojas de cálculo normales se vuelve un **caos** rápidamente. 🤯
- Las bases de datos nos permiten:
    - **Almacenar grandes volúmenes de datos.**
    - **Acceder a la información rápidamente.**
    - **Evitar la redundancia** (no tener los mismos datos repetidos por todos lados).
    - **Mantener la consistencia** de los datos (que la información sea correcta y no contradictoria).
    - **Controlar quién puede ver o modificar** la información (seguridad).
    - **Manejar múltiples usuarios** accediendo al mismo tiempo.
## 3. El Sistema de Gestión de Bases de Datos (DBMS) 🛠️
- La base de datos es el _archivo_ o el _contenedor_ de datos.
- El **DBMS** (Database Management System) es el **software** que usamos para interactuar con esa base de datos. Es como el "cerebro" que administra todo. 🧠
- Ejemplos de DBMS famosos: MySQL, PostgreSQL, Oracle, SQL Server, SQLite, MongoDB (este último es NoSQL, ¡pero la idea del DBMS aplica!).
- El DBMS nos permite **crear, leer, actualizar y eliminar datos** de la base de datos (lo que se conoce como operaciones CRUD - Create, Read, Update, Delete).
## 4. Bases de Datos Relacionales y SQL 🤝
- Hay varios tipos de bases de datos, pero las **relacionales** son las más comunes, especialmente cuando hablamos de **SQL**.
- En las bases de datos relacionales, la información se organiza en **tablas**. 📊
- Estas tablas se pueden **relacionar** entre sí usando campos comunes (claves). Por ejemplo, una tabla de "Clientes" y una tabla de "Pedidos" se pueden relacionar usando el ID del cliente.
- **SQL** (Structured Query Language) es el **lenguaje estándar** que usamos para **comunicarnos** con la mayoría de los DBMS relacionales.
- Con SQL le "pedimos" al DBMS que haga cosas con los datos: "dame todos los clientes de México", "añade un nuevo producto", "actualiza el email de este usuario", etc.
## 5. Estructura Básica: Tablas, Filas y Columnas 🧱
- Como dijimos, en bases de datos relacionales, todo gira en torno a las **tablas**.
- Una **Tabla** es una colección de datos sobre un tema específico (por ejemplo, la tabla `Usuarios` guarda info sobre los usuarios). Piensa en ella como una hoja de cálculo individual.
- Cada **Columna** (también llamada **campo** o **atributo**) representa una característica o tipo de dato. Por ejemplo, en la tabla `Usuarios`, podríamos tener columnas como `id`, `nombre`, `email`, `fecha_registro`. Las columnas definen qué _tipo_ de información vamos a guardar. 🏷️
- Cada **Fila** (también llamada **registro** o **tupla**) representa una instancia única del tema de la tabla. En la tabla `Usuarios`, cada fila sería un usuario individual con sus datos específicos en cada columna. 🧍‍♀️🧍‍♂️
## 6. Ejemplo Básico de SQL: Creando una Tabla ✏️
Aquí un pequeño ejemplo de cómo usarías SQL para decirle al DBMS que cree una tabla:
```sql
-- Esto es un comentario en SQL
-- Creamos una tabla llamada 'Productos'
CREATE TABLE Productos (
    id INT PRIMARY KEY, -- Una columna para el ID del producto (número entero)
                        -- PRIMARY KEY significa que es el identificador único de cada producto
    nombre VARCHAR(255), -- Una columna para el nombre del producto (texto, hasta 255 caracteres)
    precio DECIMAL(10, 2), -- Una columna para el precio (número decimal con 10 dígitos en total y 2 después del punto)
    stock INT -- Una columna para la cantidad en stock (número entero)
);

-- Después de crear la tabla, podríamos insertar datos, consultarlos, actualizarlos, etc.
-- Por ejemplo:
-- INSERT INTO Productos (id, nombre, precio, stock) VALUES (1, 'Laptop', 1200.00, 50);
-- SELECT * FROM Productos WHERE stock < 10;
```

Este código le dice al DBMS: "Hey, quiero una tabla nueva que se llame `Productos`, y va a tener estas columnas con estos tipos de datos".
## Mini Repaso Final ✨
¡Okay, resumen rápido! Una **base de datos** es un lugar súper organizado para guardar información digital. Usamos un **DBMS** (software) para gestionarla. Las **bases de datos relacionales** son muy comunes, guardan datos en **tablas** (con filas y columnas) y usamos **SQL** como el lenguaje para interactuar con ellas (crear, leer, actualizar, eliminar datos). ¡Es la base de casi cualquier aplicación que maneje información! 💪