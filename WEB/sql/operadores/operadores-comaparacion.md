---
aliases:
  - Operadores de Comparación en SQL ⚖️
tags:
  - sql
  - operadores
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-sql|📊 Índice SQL]]"
---
# Operadores de Comparación en SQL ⚖️

Los **operadores de comparación** en SQL se utilizan para, ¡sorpresa!, comparar dos expresiones. El resultado de una comparación es siempre un valor booleano: **VERDADERO** (`TRUE`), **FALSO** (`FALSE`), o a veces **DESCONOCIDO** (`UNKNOWN`, especialmente cuando se involucran `NULL`s). Son la base de la cláusula `WHERE` para seleccionar filas específicas.
- [[#1. ¿Cuáles son los operadores de comparación?|1. ¿Cuáles son los operadores de comparación?]]
- [[#2. ¿Cómo los usamos en SQL? 🔍|2. ¿Cómo los usamos en SQL? 🔍]]
- [[#3. Comparando Diferentes Tipos de Datos 🔢🔤📅|3. Comparando Diferentes Tipos de Datos 🔢🔤📅]]
- [[#4. ¡Cuidado con los `NULL`! 🤔|4. ¡Cuidado con los `NULL`! 🤔]]
- [[#Mini Repaso Final 💡|Mini Repaso Final 💡]]


---

## 1. ¿Cuáles son los operadores de comparación?

Estos son los que nos permiten "hacer preguntas" sobre nuestros datos:

|   |   |   |   |
|---|---|---|---|
|**Operador**|**Símbolo(s)**|**Descripción**|**Ejemplo (conceptual)**|
|Igual a|`=`|Comprueba si dos valores son iguales.|`Precio = 100`|
|Distinto de|`!=` o `<>`|Comprueba si dos valores no son iguales.|`Ciudad != 'México'`|
|Mayor que|`>`|Comprueba si un valor es mayor que otro.|`Edad > 18`|
|Menor que|`<`|Comprueba si un valor es menor que otro.|`Stock < 5`|
|Mayor o igual que|`>=`|Comprueba si un valor es mayor o igual que otro.|`Calificacion >= 70`|
|Menor o igual que|`<=`|Comprueba si un valor es menor o igual que otro.|`FechaPedido <= NOW()`|

---

## 2. ¿Cómo los usamos en SQL? 🔍

Principalmente, los usamos en la cláusula `WHERE` para filtrar las filas que devuelve una consulta `SELECT`. También se pueden usar en otras cláusulas como `HAVING` o en las condiciones de los `JOIN`.

**Ejemplos en consultas SQL:**

Supongamos que tenemos una tabla llamada `Empleados` con las columnas `Nombre`, `Departamento`, `Salario` y `FechaContratacion`.

- **Igual a (`=`)**: Para encontrar empleados de un departamento específico.
    ```sql
    SELECT Nombre, Salario
    FROM Empleados
    WHERE Departamento = 'Ventas';
    ```
    
    🧑‍💼 Esto listará solo los empleados del departamento de 'Ventas'.
    
- **Distinto de (`!=` o `<>`)**: Para encontrar empleados que _no_ son de un departamento específico.
```sql
SELECT Nombre, Departamento
FROM Empleados
WHERE Departamento != 'Recursos Humanos';
-- También podrías usar: WHERE Departamento <> 'Recursos Humanos';
```
    
    🚶‍♀️ Muestra empleados de cualquier departamento excepto 'Recursos Humanos'.
    
- **Mayor que (`>`)**: Para encontrar empleados con un salario superior a cierta cantidad.
    ```sql
    SELECT Nombre, Salario
    FROM Empleados
    WHERE Salario > 50000;
    ```
    
    💰 Lista empleados que ganan más de 50000.
    
- **Menor que (`<`)**: Para encontrar productos con pocas unidades en stock (usando una tabla `Productos` imaginaria).

```sql
SELECT NombreProducto, Stock
FROM Productos
WHERE Stock < 10;
```

    
📦 Muestra productos con menos de 10 unidades.
    
- **Mayor o igual que (`>=`)**: Para encontrar empleados contratados a partir de cierta fecha.
    ```sql
    SELECT Nombre, FechaContratacion
    FROM Empleados
    WHERE FechaContratacion >= '2023-01-01';
    ```
    
    🗓️ Lista empleados contratados el 1 de enero de 2023 o después.
    
- **Menor o igual que (`<=`)**: Para encontrar empleados con un salario hasta cierta cantidad.
    ```sql
    SELECT Nombre, Salario
    FROM Empleados
    WHERE Salario <= 30000;
    ```
    
    💸 Muestra empleados que ganan 30000 o menos.
---

## 3. Comparando Diferentes Tipos de Datos 🔢🔤📅

Los operadores de comparación no solo funcionan con números:

- **Números**: Se comparan según su valor matemático. `100` es mayor que `50`.
- **Cadenas de Texto (Strings)**: Se comparan alfabéticamente (según el "collation" o conjunto de caracteres de la base de datos). `'Manzana'` es menor que `'Pera'`. `'casa'` puede ser igual o diferente a `'Casa'` dependiendo de si la comparación es sensible a mayúsculas/minúsculas (esto varía según el SGBD y la configuración).
- **Fechas y Horas**: Se comparan cronológicamente. `'2024-05-20'` es menor que `'2024-05-21'`.

---

## 4. ¡Cuidado con los `NULL`! 🤔

Un punto súper importante es cómo interactúan los operadores de comparación con los valores `NULL`.

- Cualquier comparación directa con `NULL` usando los operadores estándar (`=`, `!=`, `<`, `>`, `<=`, `>=`) generalmente resulta en **`UNKNOWN`** (desconocido), no `TRUE` ni `FALSE`.
    - Por ejemplo, `Salario = NULL` o `Salario != NULL` no funcionarán como podrías esperar para encontrar o excluir filas con salarios desconocidos.
- Para comprobar si un valor es `NULL`, debes usar los operadores especiales **`IS NULL`** o **`IS NOT NULL`**.
    - Para encontrar empleados cuyo departamento es desconocido:
        ```sql
        SELECT Nombre
        FROM Empleados
        WHERE Departamento IS NULL;
        ```
        
    - Para encontrar empleados cuyo departamento sí está registrado (no es nulo):
        ```sql
        SELECT Nombre
        FROM Empleados
        WHERE Departamento IS NOT NULL;
        ```
        

---

## Mini Repaso Final 💡

¡Dominados los operadores de comparación! Son clave para filtrar datos.

- Usamos **`=`**, **`!=` (o `<>`)**, **`>`**, **`<`**, **`>=`**, **`<=`** para comparar valores.
- Son fundamentales en la cláusula **`WHERE`**.
- Funcionan con **números, textos y fechas**.
- ¡Recuerda! Para valores **`NULL`**, usa **`IS NULL`** o **`IS NOT NULL`**. ¡No los compares directamente con `=` o `!=`!

¡Con esto, tus consultas SQL serán mucho más precisas y poderosas! ¡A seguir practicando! 🚀✨