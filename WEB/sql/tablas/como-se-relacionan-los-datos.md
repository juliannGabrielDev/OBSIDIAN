---
aliases:
  - ¿Cómo se relacionan los Datos?
tags:
  - sql
  - tablas
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-sql|📊 Índice SQL]]"
---
# ¿Cómo se relacionan los Datos?
- [[#1. ¿Por qué Relacionar los Datos? 🤔🔗|1. ¿Por qué Relacionar los Datos? 🤔🔗]]
- [[#2. La Clave de la Relación: ¡Las Llaves (Keys)! 🔑|2. La Clave de la Relación: ¡Las Llaves (Keys)! 🔑]]
- [[#3. Llave Primaria (Primary Key - PK) 🛡️|3. Llave Primaria (Primary Key - PK) 🛡️]]
- [[#4. Llave Foránea (Foreign Key - FK) 🤝|4. Llave Foránea (Foreign Key - FK) 🤝]]
- [[#5. Visualizando las Relaciones 📊|5. Visualizando las Relaciones 📊]]
- [[#6. Tipos Comunes de Relaciones 👇|6. Tipos Comunes de Relaciones 👇]]
- [[#7. ¿Cómo Manejamos N:N? ¡Tabla Intermedia! 🌉|7. ¿Cómo Manejamos N:N? ¡Tabla Intermedia! 🌉]]
- [[#8. SQL Example: Definiendo Llaves y Uniendo Tablas ✨|8. SQL Example: Definiendo Llaves y Uniendo Tablas ✨]]
- [[#Mini Repaso Final ✨|Mini Repaso Final ✨]]

## 1. ¿Por qué Relacionar los Datos? 🤔🔗

- Imagina que tienes una tabla gigante con TODA la información: Cliente, dirección del Cliente, productos del Pedido, precio de los productos, quién vendió el pedido, etc. ¡Sería una locura! 😵‍💫
- Esto lleva a **datos repetidos** (redundancia). Si un cliente pide varias cosas, tendrías que repetir su nombre y dirección en cada línea del pedido. Si cambia de dirección, ¡tienes que buscar y actualizarlo en un montón de lugares! 👎
- Las relaciones nos ayudan a **evitar la redundancia** y asegurar la **consistencia** de los datos.
- Dividimos la información en **tablas más pequeñas y manejables** (por ejemplo, una tabla para Clientes, otra para Productos, otra para Pedidos).
- Luego, **conectamos** estas tablas usando **campos comunes**.

## 2. La Clave de la Relación: ¡Las Llaves (Keys)! 🔑

- La magia de las bases de datos relacionales para conectar tablas está en el uso de **llaves** (o keys). Hay dos tipos principales que usamos para esto:
    - **Llave Primaria (Primary Key - PK)**
    - **Llave Foránea (Foreign Key - FK)**

## 3. Llave Primaria (Primary Key - PK) 🛡️

- La PK es como la **identificación única** de cada fila en una tabla. 🆔
- Cada tabla **debe tener** (o debería tener) una PK.
- Sus valores **no se pueden repetir** dentro de la misma tabla (son únicos).
- Sus valores **no pueden ser NULOS** (no puede estar vacía).
- Nos asegura que podemos **identificar** a cada registro de forma inequívoca.
- Ejemplo: En una tabla `Clientes`, el `id_cliente` suele ser la PK. Cada cliente tiene un `id_cliente` diferente.

## 4. Llave Foránea (Foreign Key - FK) 🤝

- La FK es el **vínculo** real entre las tablas.
- Una FK es una **columna** (o varias columnas) en **una tabla** que **hace referencia** a la **Llave Primaria** de **otra tabla**. 👀
- Es la forma de decir: "Esta fila de esta tabla está relacionada con esta fila específica de aquella otra tabla".
- Ejemplo: En una tabla `Pedidos`, tendríamos una columna `id_cliente`. Esta columna `id_cliente` sería una **Llave Foránea** que apunta al `id_cliente` (la PK) en la tabla `Clientes`. Esto relaciona cada pedido con el cliente que lo hizo.
- La FK **sí puede tener valores repetidos** (varios pedidos pueden ser del mismo cliente) y **sí puede ser NULA** (si el pedido no está asociado a un cliente, aunque esto depende del diseño).

## 5. Visualizando las Relaciones 📊

- Piensa en esto: tienes la tabla `Clientes` con `id_cliente` (PK) y la tabla `Pedidos` con `id_pedido` (PK) y `id_cliente` (FK).
- Un **enlace** se forma cuando el valor de `id_cliente` en una fila de la tabla `Pedidos` coincide con el valor de `id_cliente` en una fila de la tabla `Clientes`. ¡Así sabemos qué cliente hizo qué pedido!

```
Tabla Clientes:             Tabla Pedidos:
+-----------+---------+     +-----------+-----------+------------+
| id_cliente| nombre  |     | id_pedido | id_cliente| total      |
+-----------+---------+     +-----------+-----------+------------+
| 1         | Ana     | <---| 101       | 1         | 50.00      |
| 2         | Luis    | <---| 102       | 1         | 35.00      |  -- Ana tiene 2 pedidos
| 3         | Sofía   |     | 103       | 2         | 120.00     |  -- Luis tiene 1 pedido
+-----------+---------+     | 104       | 3         | 80.00      |  -- Sofía tiene 1 pedido
                            +-----------+-----------+------------+

^ PK de Clientes              ^ FK de Pedidos que referencia a PK de Clientes
```

## 6. Tipos Comunes de Relaciones 👇

Basándonos en cómo se conectan las llaves, podemos tener diferentes tipos de relaciones:

- **Uno a Uno (1:1):** Una fila en la Tabla A se relaciona con _como máximo_ una fila en la Tabla B, y viceversa. (Ej: Una persona y su pasaporte - aunque podría no tener pasaporte, la relación es 1 a 1 si existe). Menos común en la práctica para dividir datos a menos que sea por seguridad o manejo de datos opcionales.
- **Uno a Muchos (1:N):** Una fila en la Tabla A se relaciona con _cero, una o muchas_ filas en la Tabla B. Pero una fila en la Tabla B se relaciona con _como máximo_ una fila en la Tabla A. ¡Es el tipo más común! (Ej: Un Cliente (1) puede tener Muchos Pedidos (N)). Se implementa poniendo la PK de la tabla "Uno" como FK en la tabla "Muchos".
- **Muchos a Muchos (N:N):** Una fila en la Tabla A se relaciona con _cero, una o muchas_ filas en la Tabla B, Y una fila en la Tabla B se relaciona con _cero, una o muchas_ filas en la Tabla A. (Ej: Un Producto (N) puede estar en Muchos Pedidos (N), y un Pedido (N) puede contener Muchos Productos (N)). 🤯 Este tipo **no se puede implementar directamente** con solo dos tablas y llaves. Necesitamos una **tabla intermedia** (o tabla de enlace/unión).

## 7. ¿Cómo Manejamos N:N? ¡Tabla Intermedia! 🌉

- Para una relación Muchos a Muchos (N:N), creamos una **tercera tabla**.
    
- Esta tabla intermedia contiene **Llaves Foráneas** de las dos tablas originales.
    
- Ejemplo de Productos y Pedidos (N:N):
    
    - Tabla `Productos` (`id_producto` PK, nombre, precio)
    - Tabla `Pedidos` (`id_pedido` PK, fecha, id_cliente)
    - Tabla `Detalle_Pedido` (`id_detalle` PK opcional, **`id_pedido` FK**, **`id_producto` FK**, cantidad, subtotal)
- La tabla `Detalle_Pedido` enlaza un pedido específico (`id_pedido`) con un producto específico (`id_producto`), ¡y así registramos qué productos están en qué pedido! La combinación de `id_pedido` y `id_producto` a menudo forma una clave compuesta única en la tabla intermedia.
    
    ```
    Tabla Pedidos <---> Tabla Detalle_Pedido <---> Tabla Productos
    (1:N)                  (N:1)
    ```
    

## 8. SQL Example: Definiendo Llaves y Uniendo Tablas ✨

Así es como se vería al crear tablas con llaves en SQL y cómo usarías un `JOIN` para combinarlas:

```sql
-- Creamos la tabla Clientes (el "Uno" en la relación 1:N con Pedidos)
CREATE TABLE Clientes (
    id_cliente INT PRIMARY KEY, -- Definimos la Llave Primaria
    nombre VARCHAR(100),
    email VARCHAR(100)
);

-- Creamos la tabla Pedidos (el "Muchos" en la relación 1:N con Clientes)
CREATE TABLE Pedidos (
    id_pedido INT PRIMARY KEY,
    fecha DATE,
    total DECIMAL(10, 2),
    id_cliente INT, -- Esta columna va a ser la Llave Foránea
    -- Definimos la Llave Foránea:
    -- Le decimos que la columna id_cliente referencia a la columna id_cliente en la tabla Clientes
    FOREIGN KEY (id_cliente) REFERENCES Clientes(id_cliente)
    -- ON DELETE CASCADE, ON UPDATE CASCADE, etc. son opciones para manejar qué pasa si se borra o actualiza el registro padre
);

-- Ahora, ¿cómo consultamos los pedidos Y el nombre del cliente que los hizo?
-- ¡Usamos JOIN! Unimos las tablas donde la FK de Pedidos coincide con la PK de Clientes
SELECT
    P.id_pedido,
    P.fecha,
    P.total,
    C.nombre AS nombre_cliente -- Traemos el nombre del cliente
FROM
    Pedidos P -- Le ponemos un alias 'P' a la tabla Pedidos
JOIN
    Clientes C ON P.id_cliente = C.id_cliente; -- Unimos donde el id_cliente coincida
```

Este `JOIN` es la forma en SQL de "seguir el enlace" creado por la Llave Foránea para combinar información de tablas relacionadas.

## Mini Repaso Final ✨

Para que los datos en diferentes tablas se "hablen" en una base de datos relacional, usamos **relaciones** basadas en **Llaves**. La **Llave Primaria (PK)** identifica de forma única cada fila en una tabla. La **Llave Foránea (FK)** en una tabla apunta a la PK de otra tabla, creando el **vínculo**. La relación **Uno a Muchos (1:N)** es la más común. Las relaciones **Muchos a Muchos (N:N)** requieren una **tabla intermedia** con FKs a las dos tablas originales. Usamos comandos SQL como `FOREIGN KEY` en `CREATE TABLE` para definir estas relaciones y `JOIN` en `SELECT` para consultar datos a través de ellas. ¡Es la base para tener datos organizados y consistentes! 🎉