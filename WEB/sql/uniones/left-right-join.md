---
aliases:
  - LEFT JOIN y RIGHT JOIN en SQL ↔️
tags:
  - sql
  - uniones
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-sql|📊 Índice SQL]]"
---
# LEFT JOIN y RIGHT JOIN en SQL ↔️
Mientras que el [[inner-join|INNER JOIN]] solo nos da las filas que coinciden en ambas tablas, el `LEFT JOIN` y el `RIGHT JOIN` son más permisivos y nos permiten incluir filas de una tabla, incluso si no tienen una coincidencia en la otra. ==¡Son muy útiles para ver dónde hay datos faltantes o no coincidentes!==
- [[#1. LEFT JOIN (también conocido como LEFT OUTER JOIN) ⬅️|1. LEFT JOIN (también conocido como LEFT OUTER JOIN) ⬅️]]
	- [[#1. LEFT JOIN (también conocido como LEFT OUTER JOIN) ⬅️#Sintaxis Básica del LEFT JOIN ✨|Sintaxis Básica del LEFT JOIN ✨]]
	- [[#1. LEFT JOIN (también conocido como LEFT OUTER JOIN) ⬅️#Ejemplo Práctico de LEFT JOIN 🚀|Ejemplo Práctico de LEFT JOIN 🚀]]
- [[#2. RIGHT JOIN (también conocido como RIGHT OUTER JOIN) ➡️|2. RIGHT JOIN (también conocido como RIGHT OUTER JOIN) ➡️]]
	- [[#2. RIGHT JOIN (también conocido como RIGHT OUTER JOIN) ➡️#Sintaxis Básica del RIGHT JOIN ✨|Sintaxis Básica del RIGHT JOIN ✨]]
	- [[#2. RIGHT JOIN (también conocido como RIGHT OUTER JOIN) ➡️#Ejemplo Práctico de RIGHT JOIN 🚀|Ejemplo Práctico de RIGHT JOIN 🚀]]
- [[#3. Cuándo Usar Cada Uno 🧐|3. Cuándo Usar Cada Uno 🧐]]
	- [[#3. Cuándo Usar Cada Uno 🧐#Mini Repaso Final 🧠|Mini Repaso Final 🧠]]

## 1. LEFT JOIN (también conocido como LEFT OUTER JOIN) ⬅️
El `LEFT JOIN` devuelve **todas las filas de la tabla de la izquierda** (la primera tabla mencionada en la cláusula `FROM`), y las filas coincidentes de la tabla de la derecha. Si no hay coincidencia para una fila de la izquierda, las columnas de la tabla de la derecha aparecerán con valores `NULL`.
- **Idea principal**: "Dame todo lo de la izquierda, y si hay algo que coincida a la derecha, también dámelo."
    
    ```
         +-----------------+   +-----------------+
         |                 |   |                 |
         |     Tabla A     |   |     Tabla B     |
         |  (Izquierda)    |   |    (Derecha)    |
         |  +--------------+---+------------+    |
         |  |   Todas las filas de A        |    |
         |  |   +-----------------------+   |    |
         |  |   | Coincidencia (INNER)  |   |    |
         |  |   +-----------------------+   |    |
         |  +-------------------------------+    |
         +-----------------+   +-----------------+
    ```
### Sintaxis Básica del LEFT JOIN ✨
```sql
SELECT column1, column2, ...
FROM TablaIzquierda
LEFT JOIN TablaDerecha
ON TablaIzquierda.columna_comun = TablaDerecha.columna_comun;
```

### Ejemplo Práctico de LEFT JOIN 🚀
Usando nuestras tablas `Clientes` y `Pedidos` (invertiremos el orden para el `LEFT JOIN`):
**Tabla `Clientes` (ahora la izquierda):**

| **ID_Cliente** | **Nombre_Cliente** | **Ciudad** |
| -------------- | ------------------ | ---------- |
| 1              | Ana López          | Madrid     |
| 2              | Juan Pérez         | Barcelona  |
| 3              | María García       | Sevilla    |
| 4              | Carlos Ruiz        | Valencia   |

**Tabla `Pedidos` (ahora la derecha):**

| **ID_Pedido** | **ID_Cliente** | **Fecha_Pedido** |
| ------------- | -------------- | ---------------- |
| 101           | 1              | 2024-01-15       |
| 102           | 3              | 2024-01-16       |
| 103           | 1              | 2024-01-17       |
| 104           | 2              | 2024-01-18       |

Si queremos ver a todos los clientes y, si tienen, sus pedidos:
```sql
SELECT
    C.Nombre_Cliente,
    P.ID_Pedido,
    P.Fecha_Pedido
FROM
    Clientes C
LEFT JOIN
    Pedidos P ON C.ID_Cliente = P.ID_Cliente;
```

**Resultado de la consulta:**

| **Nombre_Cliente** | **ID_Pedido** | **Fecha_Pedido** |
| ------------------ | ------------- | ---------------- |
| Ana López          | 101           | 2024-01-15       |
| Ana López          | 103           | 2024-01-17       |
| María García       | 102           | 2024-01-16       |
| Juan Pérez         | 104           | 2024-01-18       |
| Carlos Ruiz        | NULL          | NULL             |

¡Ves! "Carlos Ruiz" aparece, aunque no tenga ningún pedido, porque `Clientes` es la tabla de la izquierda.
## 2. RIGHT JOIN (también conocido como RIGHT OUTER JOIN) ➡️
El `RIGHT JOIN` es lo opuesto al `LEFT JOIN`. Devuelve **todas las filas de la tabla de la derecha** (la segunda tabla mencionada en la cláusula `FROM`), y las filas coincidentes de la tabla de la izquierda. Si no hay coincidencia para una fila de la derecha, las columnas de la tabla de la izquierda aparecerán con valores `NULL`.
- **Idea principal**: "Dame todo lo de la derecha, y si hay algo que coincida a la izquierda, también dámelo."
    
    ```
         +-----------------+      +-----------------+
         |                 |      |                 |
         |     Tabla A     |      |     Tabla B     |
         |  (Izquierda)    |      |    (Derecha)    |
         |    +------------+-------------------+    |
         |    |            Todas las filas de B|    |
         |    |   +-----------------------+    |    |
         |    |   | Coincidencia (INNER)  |    |    |
         |    |   +-----------------------+    |    |
         +-----------------+      +-----------------+
    ```

### Sintaxis Básica del RIGHT JOIN ✨
```sql
SELECT column1, column2, ...
FROM TablaIzquierda
RIGHT JOIN TablaDerecha
ON TablaIzquierda.columna_comun = TablaDerecha.columna_comun;
```
### Ejemplo Práctico de RIGHT JOIN 🚀
Usando nuestras tablas `Clientes` y `Pedidos` de nuevo:
**Tabla `Clientes` (ahora la izquierda):**

| **ID_Cliente** | **Nombre_Cliente** | **Ciudad** |
| -------------- | ------------------ | ---------- |
| 1              | Ana López          | Madrid     |
| 2              | Juan Pérez         | Barcelona  |
| 3              | María García       | Sevilla    |
| 4              | Carlos Ruiz        | Valencia   |

**Tabla `Pedidos` (ahora la derecha):**

| **ID_Pedido** | **ID_Cliente** | **Fecha_Pedido** |
| ------------- | -------------- | ---------------- |
| 101           | 1              | 2024-01-15       |
| 102           | 3              | 2024-01-16       |
| 103           | 1              | 2024-01-17       |
| 104           | 2              | 2024-01-18       |
| 105           | 99             | 2024-01-19       |

Si queremos ver todos los pedidos y, si tienen, la información del cliente:
```sql
SELECT
    C.Nombre_Cliente,
    P.ID_Pedido,
    P.Fecha_Pedido
FROM
    Clientes C
RIGHT JOIN
    Pedidos P ON C.ID_Cliente = P.ID_Cliente;
```

**Resultado de la consulta:**

| **Nombre_Cliente** | **ID_Pedido** | **Fecha_Pedido** |
| ------------------ | ------------- | ---------------- |
| Ana López          | 101           | 2024-01-15       |
| Ana López          | 103           | 2024-01-17       |
| María García       | 102           | 2024-01-16       |
| Juan Pérez         | 104           | 2024-01-18       |
| NULL               | 105           | 2024-01-19       |

¡Ahora el pedido `105` aparece, pero el `Nombre_Cliente` es `NULL` porque no hay un cliente asociado en la tabla `Clientes`!
## 3. Cuándo Usar Cada Uno 🧐

- **`INNER JOIN`**: Cuando solo te interesan las filas que tienen una **coincidencia en ambas tablas**.
- **`LEFT JOIN`**: Cuando quieres **todas las filas de la tabla de la izquierda**, sin importar si tienen una coincidencia en la derecha. Útil para encontrar "huérfanos" en la tabla izquierda (filas que no tienen relación en la derecha).
- **`RIGHT JOIN`**: Cuando quieres **todas las filas de la tabla de la derecha**, sin importar si tienen una coincidencia en la izquierda. Útil para encontrar "huérfanos" en la tabla derecha. (Aunque en la práctica, un `LEFT JOIN` suele ser más común y puedes lograr el mismo resultado invirtiendo el orden de las tablas).
---
### Mini Repaso Final 🧠

Los `JOIN` en SQL son fundamentales para combinar datos de diferentes tablas. El `INNER JOIN` es el más estricto, trayendo solo las filas que coinciden en ambas. El `LEFT JOIN` nos asegura que todas las filas de la tabla de la izquierda estén presentes en el resultado, rellenando con `NULL` si no hay coincidencia a la derecha. El `RIGHT JOIN` hace lo mismo, pero para la tabla de la derecha. ¡Entender la dirección del "outer" (izquierda o derecha) es clave para saber qué datos esperar!