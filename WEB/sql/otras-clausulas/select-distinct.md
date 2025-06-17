---
aliases:
  - SELECT DISTINCT en SQL ✨
tags:
  - sql
  - otras-clausulas
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-sql|📊 Índice SQL]]"
---
# SELECT DISTINCT en SQL ✨

La cláusula `SELECT DISTINCT` en SQL se utiliza para **eliminar las filas duplicadas** de los resultados de una consulta. Es decir, si una columna tiene el mismo valor repetido varias veces, `DISTINCT` nos asegura que ese valor aparezca solo una vez en el resultado. ¡Ideal para limpiar y obtener listas únicas!
- [[#1. ¿Para qué sirve `SELECT DISTINCT`?|1. ¿Para qué sirve `SELECT DISTINCT`?]]
- [[#2. Sintaxis Básica|2. Sintaxis Básica]]
- [[#3. Ejemplos Prácticos 🤓|3. Ejemplos Prácticos 🤓]]
- [[#4. Consideraciones Importantes 🧐|4. Consideraciones Importantes 🧐]]
- [[#Mini Repaso Final 💡|Mini Repaso Final 💡]]

---

## 1. ¿Para qué sirve `SELECT DISTINCT`?

Su principal objetivo es sencillo:

- Mostrar solo los **valores diferentes** o **únicos** de una o más columnas especificadas.
- Evitar la redundancia en los resultados de la consulta.

Imagina que tienes una lista de clientes y quieres saber de qué ciudades son, ¡pero no quieres ver "Guadalajara" repetido 100 veces si tienes 100 clientes allí! Solo quieres saber que "Guadalajara" es una de las ciudades. 🏙️

---

## 2. Sintaxis Básica

La sintaxis es muy directa. Simplemente añades la palabra clave `DISTINCT` después de `SELECT`:

```sql
SELECT DISTINCT columna1, columna2, ...
FROM nombre_tabla;
```

- Puedes aplicar `DISTINCT` a **una sola columna** o a **múltiples columnas**.
    - Si se aplica a múltiples columnas, `DISTINCT` considerará una fila como duplicada solo si **todos los valores** en las columnas seleccionadas son idénticos a los de otra fila.

---

## 3. Ejemplos Prácticos 🤓

Vamos a usar una tabla imaginaria llamada `Pedidos` con las columnas `ID_Pedido`, `Cliente`, `Ciudad_Envio` y `Producto`.

**Tabla `Pedidos` (ejemplo):**

| **ID_Pedido** | **Cliente** | **Ciudad_Envio** | **Producto** |
| ------------- | ----------- | ---------------- | ------------ |
| 1             | Ana         | Guadalajara      | Laptop       |
| 2             | Luis        | Autlán           | Teclado      |
| 3             | Ana         | Guadalajara      | Mouse        |
| 4             | Pedro       | Zapopan          | Laptop       |
| 5             | Luis        | Autlán           | Monitor      |
| 6             | Ana         | Guadalajara      | Teclado      |

- Obtener las ciudades de envío únicas:
    
    Si solo hacemos SELECT Ciudad_Envio FROM Pedidos; obtendríamos:
    
    ```
    Guadalajara
    Autlán
    Guadalajara
    Zapopan
    Autlán
    Guadalajara
    ```
    
    Pero con `DISTINCT`:
    
    ```sql
    SELECT DISTINCT Ciudad_Envio
    FROM Pedidos;
    ```
    
    El resultado sería:
    
    ```
    Guadalajara
    Autlán
    Zapopan
    ```
    
    ¡Mucho más limpio! ✨
    
- **Obtener los clientes únicos:**
    
    ```sql
    SELECT DISTINCT Cliente
    FROM Pedidos;
    ```
    
    Resultado:
    
    ```
    Ana
    Luis
    Pedro
    ```
    
- **Obtener combinaciones únicas de Cliente y Ciudad_Envio:**
    
    ```sql
    SELECT DISTINCT Cliente, Ciudad_Envio
    FROM Pedidos;
    ```
    
    Resultado:
    
    ```
    Ana, Guadalajara
    Luis, Autlán
    Pedro, Zapopan
    ```
    
    Nota que "Ana, Guadalajara" aparece solo una vez, aunque Ana hizo múltiples pedidos a Guadalajara. "Luis, Autlán" también aparece una vez.

---

## 4. Consideraciones Importantes 🧐

- **`NULL`**: `DISTINCT` trata los valores `NULL` como cualquier otro valor. Si tienes múltiples filas donde la columna seleccionada es `NULL`, `SELECT DISTINCT` devolverá una sola fila con `NULL` para esa columna (si no hay otros valores `NULL` en otras columnas consideradas por el `DISTINCT` en caso de múltiples columnas).
- **Rendimiento**: Usar `DISTINCT` puede tener un impacto en el rendimiento de la consulta, especialmente en tablas muy grandes. Esto se debe a que la base de datos necesita ordenar o procesar los datos para identificar y eliminar los duplicados. ¡Tenlo en cuenta si trabajas con millones de registros! 🐢➡️🐇
- **`COUNT(DISTINCT columna)`**: A menudo se usa `DISTINCT` dentro de funciones de agregación como `COUNT` para contar el número de valores únicos.
    
    ```sql
    SELECT COUNT(DISTINCT Ciudad_Envio) AS Numero_Ciudades_Unicas
    FROM Pedidos;
    ```
    
    Esto nos daría `3` en nuestro ejemplo.
---

## Mini Repaso Final 💡

`SELECT DISTINCT` es tu amigo para obtener listas sin repeticiones.

- **Propósito**: Eliminar filas duplicadas de los resultados de tu consulta.
- **Uso**: `SELECT DISTINCT columna(s) FROM tabla;`
- Aplica a **una o varias columnas**. Si son varias, la combinación completa de valores de esas columnas debe ser única.
- Trata los **`NULL`** como un valor más al determinar la unicidad.
- Puede afectar el **rendimiento** en tablas grandes.

¡Con `SELECT DISTINCT`, tus resultados serán únicos y directos al grano! 😉