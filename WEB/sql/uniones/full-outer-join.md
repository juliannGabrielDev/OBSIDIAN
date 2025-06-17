---
aliases:
  - FULL OUTER JOIN (o FULL JOIN) en SQL 🌍
tags:
  - sql
  - uniones
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-sql|📊 Índice SQL]]"
---
# FULL OUTER JOIN (o FULL JOIN) en SQL 🌍
El `FULL OUTER JOIN` es el tipo de unión que nos permite obtener **todas las filas de ambas tablas** (la izquierda y la derecha), combinándolas cuando hay una coincidencia y rellenando con `NULL` en las columnas de la tabla donde no hay coincidencia. ¡Es como la unión de conjuntos en matemáticas!
- [[#1. ¿Qué es un FULL OUTER JOIN? 🤔|1. ¿Qué es un FULL OUTER JOIN? 🤔]]
- [[#2. Sintaxis Básica del FULL OUTER JOIN ✨|2. Sintaxis Básica del FULL OUTER JOIN ✨]]
- [[#3. Ejemplo Práctico 🚀|3. Ejemplo Práctico 🚀]]
- [[#4. Cuándo Usar FULL OUTER JOIN 🧐|4. Cuándo Usar FULL OUTER JOIN 🧐]]
- [[#5. Simulando un FULL OUTER JOIN (si tu DB no lo soporta) 🤓|5. Simulando un FULL OUTER JOIN (si tu DB no lo soporta) 🤓]]
	- [[#5. Simulando un FULL OUTER JOIN (si tu DB no lo soporta) 🤓#Mini Repaso Final 🧠|Mini Repaso Final 🧠]]

## 1. ¿Qué es un FULL OUTER JOIN? 🤔

- Un `FULL OUTER JOIN` devuelve **todas las filas de la tabla de la izquierda Y todas las filas de la tabla de la derecha**.
- Si una fila de la tabla izquierda no tiene una coincidencia en la derecha, las columnas de la tabla derecha aparecerán con `NULL`.
- Si una fila de la tabla derecha no tiene una coincidencia en la izquierda, las columnas de la tabla izquierda aparecerán con `NULL`.
- Es la **unión (union)** de los resultados de un `LEFT JOIN` y un `RIGHT JOIN`, eliminando duplicados.
    ```
         +-----------------+                      +-----------------+
         |                 |                      |                 |
         |     Tabla A     |                      |     Tabla B     |
         |  (Izquierda)    |                      |    (Derecha)    |
         |  +--------------+----------------------+--------------+  |
         |  |   Filas solo de A | Coincidencia | Filas solo de B |  |
         |  +----------------------------------------------------+  |
         +-----------------+                      +-----------------+
    ```
## 2. Sintaxis Básica del FULL OUTER JOIN ✨

La sintaxis es muy similar a la de los otros `JOIN`s:
```sql
SELECT column1, column2, ...
FROM TablaIzquierda
FULL OUTER JOIN TablaDerecha
ON TablaIzquierda.columna_comun = TablaDerecha.columna_comun;
```

- `SELECT column1, column2, ...`: Las columnas que quieres en tu resultado final.
- `FROM TablaIzquierda`: La primera tabla.
- `FULL OUTER JOIN TablaDerecha`: Indica que quieres combinarla con `TablaDerecha` usando un `FULL OUTER JOIN`.
- `ON TablaIzquierda.columna_comun = TablaDerecha.columna_comun`: La condición de unión que especifica qué columnas deben coincidir.
## 3. Ejemplo Práctico 🚀
Volvamos a nuestras tablas, pero con algunos datos adicionales para ver el `FULL OUTER JOIN` en acción.
**Tabla `Clientes`:**

| **ID_Cliente** | **Nombre_Cliente** | **Ciudad** |
| -------------- | ------------------ | ---------- |
| 1              | Ana López          | Madrid     |
| 2              | Juan Pérez         | Barcelona  |
| 3              | María García       | Sevilla    |
| 4              | Carlos Ruiz        | Valencia   |

**Tabla `Pedidos`:**

| **ID_Pedido** | **ID_Cliente** | **Fecha_Pedido** |
| ------------- | -------------- | ---------------- |
| 101           | 1              | 2024-01-15       |
| 102           | 3              | 2024-01-16       |
| 103           | 1              | 2024-01-17       |
| 104           | 2              | 2024-01-18       |
| 105           | 99             | 2024-01-19       |
| 106           | 1              | 2024-01-20       |

Si queremos ver toda la actividad: todos los clientes (tengan o no pedidos) y todos los pedidos (tengan o no un cliente registrado):
```sql
SELECT
    C.Nombre_Cliente,
    P.ID_Pedido,
    P.Fecha_Pedido
FROM
    Clientes C
FULL OUTER JOIN
    Pedidos P ON C.ID_Cliente = P.ID_Cliente;
```
**Resultado de la consulta:**

| **Nombre_Cliente** | **ID_Pedido** | **Fecha_Pedido** |
| ------------------ | ------------- | ---------------- |
| Ana López          | 101           | 2024-01-15       |
| Ana López          | 103           | 2024-01-17       |
| Ana López          | 106           | 2024-01-20       |
| María García       | 102           | 2024-01-16       |
| Juan Pérez         | 104           | 2024-01-18       |
| Carlos Ruiz        | NULL          | NULL             |
| NULL               | 105           | 2024-01-19       |

¡Aquí podemos ver todo!
- Las filas donde `Clientes` y `Pedidos` **coinciden** (Ana, María, Juan).
- La fila donde hay un `Cliente` pero **no hay un `Pedido` relacionado** (`Carlos Ruiz`).
- La fila donde hay un `Pedido` pero **no hay un `Cliente` relacionado** (`ID_Pedido 105`).
## 4. Cuándo Usar FULL OUTER JOIN 🧐
- Se usa cuando necesitas un **panorama completo** de ambas tablas, incluyendo las filas que no tienen una coincidencia en la otra tabla.
- Es ideal para la **detección de datos inconsistentes o faltantes**. Por ejemplo, para encontrar clientes sin pedidos o pedidos sin un cliente válido.
- Aunque es muy poderoso, no todos los sistemas de bases de datos lo soportan (por ejemplo, MySQL hasta ciertas versiones no lo tenía nativamente, se simula con `UNION` de `LEFT JOIN` y `RIGHT JOIN`). Sin embargo, PostgreSQL, SQL Server y Oracle sí lo soportan ampliamente.
## 5. Simulando un FULL OUTER JOIN (si tu DB no lo soporta) 🤓
Si estás trabajando con un sistema que no soporta `FULL OUTER JOIN`, puedes lograr el mismo resultado combinando un `LEFT JOIN` y un `RIGHT JOIN` con el operador `UNION ALL`:

```sql
SELECT
    C.Nombre_Cliente,
    P.ID_Pedido,
    P.Fecha_Pedido
FROM
    Clientes C
LEFT JOIN
    Pedidos P ON C.ID_Cliente = P.ID_Cliente

UNION ALL -- Combina los resultados de las dos consultas

SELECT
    C.Nombre_Cliente,
    P.ID_Pedido,
    P.Fecha_Pedido
FROM
    Clientes C
RIGHT JOIN
    Pedidos P ON C.ID_Cliente = P.ID_Cliente
WHERE
    C.ID_Cliente IS NULL; -- Esto es clave para evitar duplicados del INNER JOIN
```
El `WHERE C.ID_Cliente IS NULL` en el `RIGHT JOIN` se asegura de que solo se incluyan las filas de la tabla de la derecha que no tuvieron una coincidencia en la tabla de la izquierda (porque las que sí coincidieron ya fueron cubiertas por el `LEFT JOIN`).

---
### Mini Repaso Final 🧠
El `FULL OUTER JOIN` es el `JOIN` que te da **todo**: las coincidencias de ambas tablas y las filas que solo existen en una de ellas, rellenando con `NULL` cuando no hay un par. Es perfecto para análisis de completitud de datos y para ver todas las relaciones posibles entre dos conjuntos de información. ¡Es el más "todo incluido" de los `JOIN`s!