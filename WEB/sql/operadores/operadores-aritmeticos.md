---
aliases:
  - Operadores Aritméticos en SQL ➕➖✖️➗
tags:
  - sql
  - operadores
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-sql|📊 Índice SQL]]"
---
# Operadores Aritméticos en SQL ➕➖✖️➗

Los **operadores aritméticos** en SQL nos permiten realizar operaciones matemáticas con los valores numéricos de nuestras tablas. ¡Imagina que son como la calculadora integrada de SQL! 🧮
- [[#1. ¿Cuáles son los operadores aritméticos?|1. ¿Cuáles son los operadores aritméticos?]]
- [[#2. ¿Cómo los usamos en SQL? 🧐|2. ¿Cómo los usamos en SQL? 🧐]]
- [[#3. Puntos Clave a Recordar 📝|3. Puntos Clave a Recordar 📝]]
- [[#Mini Repaso Final 💡|Mini Repaso Final 💡]]


---

## 1. ¿Cuáles son los operadores aritméticos?

Son los símbolos que usamos para las operaciones matemáticas básicas. ¡Seguro ya los conoces!

|   |   |   |   |
|---|---|---|---|
|**Operador**|**Símbolo**|**Descripción**|**Ejemplo (simple)**|
|Suma|`+`|Suma dos o más números.|`5 + 3`|
|Resta|`-`|Resta un número de otro.|`10 - 4`|
|Multiplicación|`*`|Multiplica dos o más números.|`6 * 7`|
|División|`/`|Divide un número entre otro.|`20 / 4`|
|Módulo|`%`|Devuelve el resto de una división.|`10 % 3` (da 1)|

---

## 2. ¿Cómo los usamos en SQL? 🧐

Generalmente, los usamos en la cláusula `SELECT` para crear columnas calculadas, o en la cláusula `WHERE` para filtrar registros basados en cálculos.

**Ejemplos en consultas SQL:**

Supongamos que tenemos una tabla llamada `Productos` con las columnas `Nombre`, `Precio` y `CantidadEnStock`.

- **Suma**: Para calcular el precio total si aumentamos $5 a cada producto.
    ```sql
    SELECT Nombre, Precio, Precio + 5 AS PrecioAumentado
    FROM Productos;
    ```
    
    ✨ Aquí `PrecioAumentado` es una nueva columna con el cálculo.
    
- **Resta**: Para calcular un precio con descuento.
    ```sql
    SELECT Nombre, Precio, Precio - (Precio * 0.10) AS PrecioConDescuento
    FROM Productos;
    ```
    
    💸 ¡Calculando un 10% de descuento!
    
- **Multiplicación**: Para calcular el valor total del inventario por producto.
    ```sql
    SELECT Nombre, Precio, CantidadEnStock, Precio * CantidadEnStock AS ValorInventario
    FROM Productos;
    ```
    
    📈 `ValorInventario` nos da el valor total de ese producto en stock.
    
- **División**: Para calcular cuántas docenas tenemos de un producto (si `CantidadEnStock` fueran unidades).
    ```sql
    SELECT Nombre, CantidadEnStock, CantidadEnStock / 12 AS DocenasEnStock
    FROM Productos;
    ```
    
    📦 `DocenasEnStock` nos mostraría la cantidad en docenas. _Ojo aquí: si `CantidadEnStock` es un entero, el resultado podría truncarse en algunos sistemas SQL. Para obtener decimales, a veces hay que convertir uno de los números a decimal._
    
- **Módulo**: Para saber si la cantidad en stock es par o impar.
    ```sql
    SELECT Nombre, CantidadEnStock, CantidadEnStock % 2 AS EsPar (0 si es par, 1 si es impar)
    FROM Productos;
    ```
    
    🤔 Útil para ciertas lógicas de distribución o clasificación.

---

## 3. Puntos Clave a Recordar 📝

- **Tipos de Datos**: ¡Cuidado! Los operadores funcionan mejor con tipos de datos **numéricos** (como `INT`, `DECIMAL`, `FLOAT`). Si intentas sumar una cadena de texto con un número directamente, podrías obtener un error o un resultado inesperado, dependiendo del sistema gestor de base de datos (SGBD).
- **Precedencia de Operadores**: Al igual que en matemáticas, SQL sigue un orden para realizar las operaciones. La multiplicación (`*`) y la división (`/`) se realizan antes que la suma (`+`) y la resta (`-`). Puedes usar **paréntesis `()`** para cambiar el orden de evaluación.
    - Ejemplo: `SELECT 5 + 2 * 3;` dará `11` (primero `2*3=6`, luego `5+6=11`).
    - Ejemplo con paréntesis: `SELECT (5 + 2) * 3;` dará `21` (primero `5+2=7`, luego `7*3=21`).
- **División por Cero**: ¡Mucho ojo! Intentar dividir entre cero (`0`) generalmente causará un **error** en tu consulta. Algunos SGBD pueden manejarlo de formas específicas, pero es una buena práctica evitarlo.
- **Valores `NULL`**: Si alguno de los operandos en una operación aritmética es `NULL`, el resultado de la operación generalmente también será `NULL`.
    - Ejemplo: `SELECT 10 + NULL;` probablemente devolverá `NULL`.

---

## Mini Repaso Final 💡

¡Listo! Los **operadores aritméticos en SQL** son herramientas fundamentales para hacer cálculos.

- **Suma (`+`)**, **Resta (`-`)**, **Multiplicación (`*`)**, **División (`/`)** y **Módulo (`%`)** son los principales.
- Se usan comúnmente en `SELECT` para crear columnas calculadas o en `WHERE` para filtrar.
- ¡Ojo con los **tipos de datos**, la **precedencia de operadores** (usa paréntesis si es necesario!), la **división por cero** y cómo se manejan los valores **`NULL`**!

¡Espero que estas notas te sirvan para tu repaso! ¡A seguir practicando con SQL! 💪😊