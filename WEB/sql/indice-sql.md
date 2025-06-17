---
aliases:
  - 📊 Índice SQL
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
---
![[mysql.svg]]
# 🧭 SQL
- [[#Teoría|Teoría]]
- [[#Tablas|Tablas]]
- [[#Conjuntos de Comandos|Conjuntos de Comandos]]
- [[#Tipos de Uniones|Tipos de Uniones]]
- [[#operadores|Operadores]]
- [[#Otras Cláusulas|Otras Cláusulas]]
- [[#Recursos|Recursos]]
---
## Teoría

| Nombre                                            | Palabras Clave                                                                                                                                           |
| ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [[intro\|Introducción a las Bases de Datos 📂]]   | Bases de datos relacionales, tablas                                                                                                                      |
| [[esquema\|Esquema de la Base de Datos 🏗️💾]]    | Tablas, Columnas, Tipos de Datos, Claves Primarias, Claves Foráneas, Relaciones, Restricciones, Índices, Vistas, Procedimientos Almacenados y Funciones. |
| [[modelo-relacional\|El Modelo Relacional 🏛️📊]] | Restricciones, Registro, Dominio, Cardinalidad, Grado, Clave, Atributo, Relación.                                                                        |
| [[normalizacion\|Normalización 🥇]]               | 1NF, 2NF, 3FN, anomalia de inserción, anomalia de actualización, anomalia de eliminación.                                                                |
## Tablas

| Nombre                                                                | Palabras Clave                                                                                                                                                               |
| --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [[como-se-relacionan-los-datos\|¿Cómo se relacionan los Datos? 🤔🔗]] | Primary Key, Foreign Key, uniendo tablas.                                                                                                                                    |
| [[tipos-datos\|Tipos de Datos 🔢]]                                    | `INT`, `INTEGER`, `SMALLINT`, `MEDIUMINT`, `BIGINT`, `DECIMAL`, `FLOAT`, `REAL`, `DOUBLE` - `VARCHAR`, `CHAR`, `TEXT` - `DATE`, `TIME`, `DATETIME`, `TIMESTAMP` - `BOOLEAN`. |
| [[restricciones-integridad\|Restricciones de Integridad 🚫📝]]        | `NOT NULL`, `UNIQUE`, `PRIMARY KEY`, `FOREIGN KEY`, `CHECK`, `DEFAULT`.                                                                                                      |
| [[tipos-claves\|Tipos de Claves 🔑]]                                  | Super Key, Candidate Key, Primary Key, Alternate Key, Foreign Key, Composite Key.                                                                                            |
## Conjuntos de Comandos

| Nombre                              | Palabras Clave                                                    |
| ----------------------------------- | ----------------------------------------------------------------- |
| [[dcl\|Data Control Language]]      | `GRANT`, `REVOKE`.                                                |
| [[ddl\|Data Definition Language]]   | `CREATE`, `ALTER`, DROP, `TRUNCATE`, `RENAME`.                    |
| [[dml\|Data Manipulation Language]] | `SELECT`, `INSERT`, `UPDATE`, `DELETE`.                           |
| [[dql\|Data Query Language]]        | `FROM`, `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY`, `LIMIT / TOP`. |
## Tipos de Uniones

| Nombre                                                       | Palabras Clave                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------------ |
| [[inner-join\|INNER JOIN en SQL 🔗]]                         | `INNER JOIN`, `ON`, `SELECT`, `FROM`.                              |
| [[left-right-join\|LEFT JOIN y RIGHT JOIN en SQL ↔️]]        | `LEFT JOIN`, `RIGHT JOIN`, `NULL`, `FROM`, `ON`.                   |
| [[full-outer-join\|FULL OUTER JOIN (o FULL JOIN) en SQL 🌍]] | `FULL OUTER JOIN`, `FULL JOIN`, `NULL`, `FROM`, `ON`, `UNION ALL`. |

## Operadores

| Nombre                                                      | Palabras Clave                                                                |
| ----------------------------------------------------------- | ----------------------------------------------------------------------------- |
| [[operadores-aritmeticos\|Operadores Aritméticos ➕➖✖️➗]]    | Suma (`+`), Resta (`-`), Multiplicación (`*`), División (`/`) y Módulo (`%`). |
| [[operadores-comaparacion\|Operadores de Comparación ⚖️]]   | `=`, `!=` (o `<>`), `>`, `<`, `>=`, `<=`, `IS NULL`, `IS NOT NULL`.           |
| [[funciones-agregacion\|Funciones de Agregación en SQL 📊]] | `SQL`, `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`, `GROUP BY`, `HAVING`.             |
## Otras Cláusulas

| Nombre                                        | Palabras Clave                         |
| --------------------------------------------- | -------------------------------------- |
| [[select-distinct\|SELECT DISTINCT en SQL ✨]] | `DISTINCT`, Eliminar filas duplicadas. |
## Recursos

| Nombre                                                             | Tipo   |
| ------------------------------------------------------------------ | ------ |
| [[diagrama-consulta-sql.jpg\|🖼️ Diagrama de Consulta SQL]]        | Imagen |
| [[join-sql.jpg\|🔗 SQL JOINs Explicados]]                          | Imagen |
| [[sql-basics-cheat-sheet-a3.pdf\|📚 Hoja de trucos de SQL Básico]] | PDF    |
| [[tutorial-de-sql.pdf\|📖 Tutorial Completo de SQL]]               | PDF    |
