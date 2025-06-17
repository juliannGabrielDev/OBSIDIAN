---
aliases:
  - INNER JOIN en SQL 🔗
tags:
  - sql
  - uniones
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-sql|📊 Índice SQL]]"
---
# INNER JOIN en SQL 🔗
Un `INNER JOIN` es el tipo de unión más común y básico en SQL. Lo usamos cuando queremos combinar filas de **dos o más tablas** basándonos en una condición de coincidencia entre ellas. ¡Piensa en ello como encontrar los puntos en común entre tus tablas!
- [[#1. ¿Qué es un INNER JOIN? 🤔|1. ¿Qué es un INNER JOIN? 🤔]]
- [[#2. Sintaxis Básica del INNER JOIN ✨|2. Sintaxis Básica del INNER JOIN ✨]]
- [[#3. Ejemplo Práctico 🚀|3. Ejemplo Práctico 🚀]]
- [[#4. Claves Importantes al Usar INNER JOIN 💡|4. Claves Importantes al Usar INNER JOIN 💡]]
	- [[#4. Claves Importantes al Usar INNER JOIN 💡#Mini Repaso Final 🧠|Mini Repaso Final 🧠]]

## 1. ¿Qué es un INNER JOIN? 🤔

- Un `INNER JOIN` combina filas de dos tablas **solo cuando hay una coincidencia** en los valores de la columna especificada en ambas tablas.
- Si una fila en una tabla no tiene una coincidencia en la otra tabla, **no se incluirá** en el resultado.
- Es como la **intersección** de conjuntos en matemáticas. Venn diagram.
    ```
         +-----------------+   +-----------------+
         |                 |   |                 |
         |     Tabla A     |   |     Tabla B     |
         |     +-----------+---+-----------+     |
         |     | Coincidencia (INNER JOIN) |     |
         |     +-----------+---+-----------+     |
         |                 |   |                 |
         +-----------------+   +-----------------+
    ```

## 2. Sintaxis Básica del INNER JOIN ✨

La forma más común de escribir un `INNER JOIN` es esta:

```sql
SELECT column1, column2, ...
FROM TablaA
INNER JOIN TablaB
ON TablaA.columna_comun = TablaB.columna_comun;
```

- `SELECT column1, column2, ...`: Aquí especificas las **columnas que quieres ver** en el resultado final, que pueden venir de cualquiera de las tablas unidas.
- `FROM TablaA`: La **primera tabla** desde la que vas a obtener datos.
- `INNER JOIN TablaB`: Indica que vas a unir `TablaA` con `TablaB` usando un `INNER JOIN`.
- `ON TablaA.columna_comun = TablaB.columna_comun`: Esta es la **condición de unión**. Especificas las columnas en cada tabla que deben tener valores coincidentes para que las filas se unan. ¡Estas columnas suelen ser claves primarias y foráneas!
## 3. Ejemplo Práctico 🚀

Imagina que tenemos dos tablas: `Pedidos` y `Clientes`.
**Tabla `Pedidos`:**

| **ID_Pedido** | **ID_Cliente** | **Fecha_Pedido** |
| ------------- | -------------- | ---------------- |
| 101           | 1              | 2024-01-15       |
| 102           | 3              | 2024-01-16       |
| 103           | 1              | 2024-01-17       |
| 104           | 2              | 2024-01-18       |
| 105           | 99             | 2024-01-19       |

**Tabla `Clientes`:**

| **ID_Cliente** | **Nombre_Cliente** | **Ciudad** |
| -------------- | ------------------ | ---------- |
| 1              | Ana López          | Madrid     |
| 2              | Juan Pérez         | Barcelona  |
| 3              | María García       | Sevilla    |
| 4              | Carlos Ruiz        | Valencia   |

Si queremos ver qué pedidos fueron hechos por qué clientes (solo los que tenemos registrados), usaríamos un `INNER JOIN`:

```sql
SELECT
    P.ID_Pedido,
    C.Nombre_Cliente,
    P.Fecha_Pedido,
    C.Ciudad
FROM
    Pedidos P -- Le ponemos un alias 'P' para que sea más corto
INNER JOIN
    Clientes C ON P.ID_Cliente = C.ID_Cliente; -- Le ponemos un alias 'C'
```

**Resultado de la consulta:**

| **ID_Pedido** | **Nombre_Cliente** | **Fecha_Pedido** | **Ciudad** |
| ------------- | ------------------ | ---------------- | ---------- |
| 101           | Ana López          | 2024-01-15       | Madrid     |
| 103           | Ana López          | 2024-01-17       | Madrid     |
| 102           | María García       | 2024-01-16       | Sevilla    |
| 104           | Juan Pérez         | 2024-01-18       | Barcelona  |

**¡Ojo!** El pedido `105` del `ID_Cliente 99` no aparece porque no hay un `ID_Cliente 99` en la tabla `Clientes`. ¡Esa es la clave de un `INNER JOIN`!

## 4. Claves Importantes al Usar INNER JOIN 💡

- **Identificar las columnas comunes**: Antes de hacer un `JOIN`, tienes que saber qué columnas se relacionan entre las tablas. ¡Normalmente son las **claves primarias** y **foráneas**!
- **Claridad con los alias**: Usar **alias** para las tablas (`P` para `Pedidos`, `C` para `Clientes` en el ejemplo) hace que tus consultas sean más cortas y fáciles de leer, especialmente cuando trabajas con muchas tablas o nombres largos.
- **Múltiples condiciones**: Puedes tener más de una condición en el `ON` si es necesario, usando `AND` u `OR`.
    ```sql
    -- Ejemplo con múltiples condiciones (aunque menos común para INNER JOIN simple)
    SELECT *
    FROM Empleados E
    INNER JOIN Departamentos D
    ON E.ID_Departamento = D.ID_Departamento AND E.Estatus = 'Activo';
    ```
    

---
### Mini Repaso Final 🧠

El `INNER JOIN` en SQL es tu mejor amigo cuando necesitas combinar información de varias tablas, pero **solo te interesan las filas que tienen una coincidencia** en ambas. Siempre especificas las tablas que quieres unir y la condición (`ON`) que define cómo se relacionan. ¡Recuerda que las filas sin coincidencia se quedan fuera!