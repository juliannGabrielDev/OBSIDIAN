---
aliases:
  - Funciones de Agregación en SQL 📊
tags:
  - sql
  - operadores
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-sql|📊 Índice SQL]]"
---
# Funciones de Agregación en SQL 📊

Las funciones de agregación en SQL son como herramientas mágicas que nos permiten realizar cálculos sobre un conjunto de filas y devolver un único valor de resumen. ¡Imagínate tener un montón de datos y querer saber el promedio, el total o cuántos hay! Para eso sirven.
- [[#1. ¿Qué son las Funciones de Agregación? 🤔|1. ¿Qué son las Funciones de Agregación? 🤔]]
- [[#2. Las Funciones de Agregación Más Comunes ✨|2. Las Funciones de Agregación Más Comunes ✨]]
	- [[#2. Las Funciones de Agregación Más Comunes ✨#Ejemplos Prácticos 🚀|Ejemplos Prácticos 🚀]]
- [[#3. Combinando con `GROUP BY` y `HAVING` 🧩|3. Combinando con `GROUP BY` y `HAVING` 🧩]]
- [[#4. Diferencia entre `WHERE` y `HAVING` 💡|4. Diferencia entre `WHERE` y `HAVING` 💡]]
	- [[#4. Diferencia entre `WHERE` y `HAVING` 💡#Ejemplo para entenderlo mejor:|Ejemplo para entenderlo mejor:]]
	- [[#4. Diferencia entre `WHERE` y `HAVING` 💡#Mini Repaso Final 🧠|Mini Repaso Final 🧠]]

## 1. ¿Qué son las Funciones de Agregación? 🤔

* Son funciones que operan sobre un grupo de filas (a menudo definidas por la cláusula `GROUP BY`).
* Devuelven un **único valor** para cada grupo o para todo el conjunto de resultados si no se usa `GROUP BY`.
* ¡Son esenciales para el análisis de datos y la creación de informes! 📈

## 2. Las Funciones de Agregación Más Comunes ✨

Aquí te presento las funciones de agregación más populares que usamos en SQL:

| Función | Descripción                                                | Ejemplo de Uso                                      |
| :------ | :--------------------------------------------------------- | :-------------------------------------------------- |
| `COUNT()` | Cuenta el número de filas o valores no nulos en una columna. | `SELECT COUNT(*) FROM Empleados;`                   |
| `SUM()`   | Calcula la suma total de los valores numéricos en una columna. | `SELECT SUM(Salario) FROM Empleados;`               |
| `AVG()`   | Calcula el promedio de los valores numéricos en una columna. | `SELECT AVG(Edad) FROM Clientes;`                   |
| `MIN()`   | Encuentra el valor mínimo en una columna.                  | `SELECT MIN(FechaContratacion) FROM Empleados;`     |
| `MAX()`   | Encuentra el valor máximo en una columna.                  | `SELECT MAX(Precio) FROM Productos;`                |

### Ejemplos Prácticos 🚀

* **`COUNT(*)` vs `COUNT(columna)`**:
    * `COUNT(*)` cuenta **todas las filas**, incluyendo las que tienen valores `NULL`.
    * `COUNT(nombre_columna)` cuenta las filas donde `nombre_columna` **no es `NULL`**.

    ```sql
    -- Contar todos los empleados
    SELECT COUNT(*) AS TotalEmpleados
    FROM Empleados;

    -- Contar cuántos empleados tienen un correo electrónico
    SELECT COUNT(Email) AS EmpleadosConEmail
    FROM Empleados;
    ```

* **`SUM()`**:
    ```sql
    -- Calcular el total de ventas
    SELECT SUM(Monto) AS VentasTotales
    FROM Ventas;
    ```

* **`AVG()`**:
    ```sql
    -- Calcular el salario promedio
    SELECT AVG(Salario) AS SalarioPromedio
    FROM Empleados;
    ```

* **`MIN()` y `MAX()`**:
    ```sql
    -- Encontrar el precio más bajo y el más alto
    SELECT MIN(Precio) AS PrecioMinimo, MAX(Precio) AS PrecioMaximo
    FROM Productos;
    ```

## 3. Combinando con `GROUP BY` y `HAVING` 🧩

Aquí es donde se pone interesante. Podemos usar las funciones de agregación con `GROUP BY` para agrupar las filas y aplicar la función a cada grupo. ¡Y `HAVING` nos ayuda a filtrar esos grupos!

* **`GROUP BY`**: Agrupa filas que tienen los mismos valores en una o más columnas para que las funciones de agregación puedan operar sobre cada grupo.
    ```sql
    -- Salario promedio por departamento
    SELECT Departamento, AVG(Salario) AS SalarioPromedio
    FROM Empleados
    GROUP BY Departamento;
    ```

* **`HAVING`**: Filtra los resultados de los grupos creados por `GROUP BY`. ¡Es como un `WHERE` pero para grupos!
    ```sql
    -- Departamentos con más de 10 empleados
    SELECT Departamento, COUNT(*) AS TotalEmpleados
    FROM Empleados
    GROUP BY Departamento
    HAVING COUNT(*) > 10;
    ```

## 4. Diferencia entre `WHERE` y `HAVING` 💡

¡Esto es súper importante y a veces confuso!

| Característica | `WHERE`                                       | `HAVING`                                           |
| :------------- | :-------------------------------------------- | :------------------------------------------------- |
| **Cuándo se usa** | Antes de agrupar las filas.                   | Después de agrupar las filas.                      |
| **Qué filtra** | Filas individuales.                           | Grupos de filas.                                   |
| **Con qué** | Columnas, operadores de comparación.         | Resultados de funciones de agregación o columnas. |

### Ejemplo para entenderlo mejor:

```sql
-- Empleados con salario mayor a 3000, y luego agrupar y ver departamentos con más de 5 empleados
SELECT Departamento, COUNT(*) AS NumeroEmpleados, AVG(Salario) AS SalarioPromedio
FROM Empleados
WHERE Salario > 3000 -- Filtra filas individuales antes de agrupar
GROUP BY Departamento
HAVING COUNT(*) > 5; -- Filtra grupos ya creados
```

---

### Mini Repaso Final 🧠

¡Uff, eso fue bastante! En resumen, las **funciones de agregación en SQL** (`COUNT`, `SUM`, `AVG`, `MIN`, `MAX`) nos permiten calcular valores de resumen sobre conjuntos de datos. Son fundamentales para el análisis. Las usamos a menudo con `GROUP BY` para realizar cálculos por categorías y con `HAVING` para filtrar esos grupos. ¡Recuerda que `WHERE` filtra filas antes de agrupar, y `HAVING` filtra grupos después de agrupar!