---
aliases:
  - Tipos de Claves 🔑
tags:
  - sql
  - tablas
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-sql|📊 Índice SQL]]"
---
# Tipos de Claves 🔑
- [[#1. ¿Qué son las Claves en Bases de Datos?|1. ¿Qué son las Claves en Bases de Datos?]]
- [[#2. Super Key: El Conjunto Identificador Mayor 🧐|2. Super Key: El Conjunto Identificador Mayor 🧐]]
- [[#3. Candidate Key: La Super Key Minimalista ✨|3. Candidate Key: La Super Key Minimalista ✨]]
- [[#4. PRIMARY KEY: ¡Nuestra Llave Elegida! 🛡️ (Relación con Candidate)|4. PRIMARY KEY: ¡Nuestra Llave Elegida! 🛡️ (Relación con Candidate)]]
- [[#5. Alternate Key: Las Candidatas que No Ganaron 🥈|5. Alternate Key: Las Candidatas que No Ganaron 🥈]]
- [[#6. FOREIGN KEY: La Llave para Conectar Tablas 🤝|6. FOREIGN KEY: La Llave para Conectar Tablas 🤝]]
- [[#7. Composite Key: Llaves de Varias Columnas 🧩|7. Composite Key: Llaves de Varias Columnas 🧩]]
- [[#8. Ejemplo en Código (CREATE TABLE) ✏️|8. Ejemplo en Código (CREATE TABLE) ✏️]]
- [[#Mini Repaso Final ✨|Mini Repaso Final ✨]]

## 1. ¿Qué son las Claves en Bases de Datos?

- Una clave (key) es un **atributo** (una columna) o un **conjunto de atributos** (varias columnas) en una tabla.
- Su propósito principal es:
    - **Identificar de forma única** una fila dentro de una tabla.
    - **Establecer relaciones** entre diferentes tablas.
- Son fundamentales para el diseño del modelo relacional y para asegurar la integridad de los datos.

## 2. Super Key: El Conjunto Identificador Mayor 🧐

- Es el concepto más amplio. Una **Super Key** es **cualquier conjunto de una o más columnas** que, combinadas, pueden identificar de forma **única** una fila dentro de una tabla.
- Puede contener columnas adicionales que no son estrictamente necesarias para la identificación única.
- _Ejemplo:_ En una tabla `Clientes` con columnas `id_cliente`, `email`, `CURP`, `nombre`, `apellido`.
    - `{id_cliente}` es una Super Key (si `id_cliente` es único).
    - `{email, nombre}` podría ser una Super Key (si la combinación de email y nombre es única, aunque es riesgoso).
    - `{id_cliente, nombre, apellido}` también es una Super Key (si `{id_cliente}` ya es única, añadir nombre y apellido sigue garantizando la unicidad, pero son redundantes para identificar).
    - `{CURP}` es una Super Key (si la CURP es única para cada cliente).
- Básicamente, si un conjunto de columnas incluye algo que ya identifica de forma única la fila, todo ese conjunto es una Super Key.

## 3. Candidate Key: La Super Key Minimalista ✨

- Una **Candidate Key** es una **Super Key minimalista**.
- Minimalista significa que si quitas cualquier columna de ese conjunto, **ya no puede identificar** de forma única una fila.
- Representan todas las posibles opciones que tienes para identificar una fila de forma única sin información extra.
- _Ejemplo (basado en el anterior):_ En la tabla `Clientes` con `id_cliente`, `email`, `CURP`, `nombre`, `apellido`, si asumimos que `id_cliente`, `email` y `CURP` son _cada uno por sí solos_ únicos e irrepetibles:
    - `{id_cliente}` es una Candidate Key (si quitas `id_cliente`, no puedes identificar).
    - `{email}` es una Candidate Key (si quitas `email`, no puedes identificar).
    - `{CURP}` es una Candidate Key (si quitas `CURP`, no puedes identificar).
    - `{email, nombre}` probablemente NO es una Candidate Key si `{email}` por sí solo ya es único (porque `nombre` sería redundante para la unicidad). Pero podría ser una Candidate Key si solo la _combinación_ de email y nombre es única (y ninguno por separado lo es).
- Las Candidate Keys son las **verdaderas opciones** para ser la llave principal de tu tabla.

## 4. PRIMARY KEY: ¡Nuestra Llave Elegida! 🛡️ (Relación con Candidate)

- Ya la conoces. La **PRIMARY KEY** es la **Candidate Key que el diseñador de la base de datos elige** como el identificador principal y oficial para una tabla.
- Solo puede haber **una** `PRIMARY KEY` por tabla.
- Como vimos, debe ser `UNIQUE` y `NOT NULL`.
- Es la llave que usamos para referenciar filas desde otras tablas (a través de `FOREIGN KEY`).
- **Relación con Candidate Keys:**
    - Toda `PRIMARY KEY` es una `Candidate Key`.
    - Pero no toda `Candidate Key` es la `PRIMARY KEY` (puede haber otras).
- _Ejemplo:_ De las Candidate Keys `{id_cliente}`, `{email}`, `{CURP}` de nuestra tabla `Clientes`, nosotros **elegimos** `{id_cliente}` como la `PRIMARY KEY`.

## 5. Alternate Key: Las Candidatas que No Ganaron 🥈

- Una **Alternate Key** (o Llave Alterna, o a veces llamada Secondary Key) es **cualquier Candidate Key que NO fue seleccionada** como la `PRIMARY KEY`.
- Son identificadores únicos válidos para una fila, pero no son el principal.
- En SQL, se implementan a menudo con la restricción `UNIQUE`.
- _Ejemplo:_ Si `{id_cliente}` es la `PRIMARY KEY` en la tabla `Clientes`, entonces `{email}` y `{CURP}` son **Alternate Keys**.

## 6. FOREIGN KEY: La Llave para Conectar Tablas 🤝

- También ya la repasamos en restricciones. La **FOREIGN KEY** es una columna (o conjunto de columnas) en una tabla ("hija") que **hace referencia a la `PRIMARY KEY`** (o a una `UNIQUE` Key) en **otra tabla** ("padre").
- Su función es **establecer y mantener relaciones** entre tablas y asegurar la integridad referencial.
- _Ejemplo:_ En la tabla `Pedidos`, la columna `id_cliente` es una **FOREIGN KEY** que referencia al `id_cliente` (`PRIMARY KEY`) en la tabla `Clientes`.

## 7. Composite Key: Llaves de Varias Columnas 🧩

- Una **Composite Key** (o Llave Compuesta) es una clave (puede ser Super, Candidate, Primary, Unique) que consiste en **DOS o MÁS columnas**.
- La combinación de valores en estas múltiples columnas es lo que garantiza la unicidad (o se usa para la relación).
- Son muy comunes en tablas de enlace para relaciones Muchos a Muchos.
- _Ejemplo:_ En una tabla `Inscripciones` que registra qué estudiante está inscrito en qué curso, la combinación de `id_estudiante` y `id_curso` podría ser la `PRIMARY KEY` (y por lo tanto, una Composite Primary Key).

```
Tabla Inscripciones:
+--------------+-----------+----------+
| id_estudiante| id_curso  | fecha_insc|
+--------------+-----------+----------+
| 1            | 101       | 2024-09-01|
| 1            | 102       | 2024-09-01|  -- Estudiante 1 en dos cursos
| 2            | 101       | 2024-09-01|  -- Curso 101 con dos estudiantes
+--------------+-----------+----------+
^ Combinación de estas dos es la PRIMARY KEY (Composite Key)
```

## 8. Ejemplo en Código (CREATE TABLE) ✏️

Así es como se definen algunas de estas claves usando DDL en `CREATE TABLE`:

```sql
CREATE TABLE Clientes (
    id_cliente INT PRIMARY KEY, -- Define id_cliente como la PRIMARY KEY (es una Candidate Key elegida)
    email VARCHAR(100) UNIQUE, -- Define email como UNIQUE (es otra Candidate Key, por lo tanto, una Alternate Key)
    CURP VARCHAR(18) UNIQUE,   -- Define CURP como UNIQUE (es otra Candidate Key, por lo tanto, una Alternate Key)
    nombre VARCHAR(100) NOT NULL,
    apellido VARCHAR(100)
);

-- Ejemplo de tabla de enlace con Composite Primary Key y Foreign Keys
CREATE TABLE Inscripciones (
    id_estudiante INT,
    id_curso INT,
    fecha_inscripcion DATE,
    PRIMARY KEY (id_estudiante, id_curso), -- Define una Composite PRIMARY KEY con 2 columnas
    FOREIGN KEY (id_estudiante) REFERENCES Estudiantes(id_estudiante), -- FK a tabla Estudiantes
    FOREIGN KEY (id_curso) REFERENCES Cursos(id_curso) -- FK a tabla Cursos
);
```

Aquí vemos cómo las restricciones `PRIMARY KEY` y `UNIQUE` implementan los conceptos de Candidate, Primary y Alternate Keys, y `FOREIGN KEY` implementa esa conexión, que puede ser parte de una Composite Key.

## Mini Repaso Final ✨

¡Un último repaso sobre las claves! Son columnas (o conjuntos) usadas para identificar filas o conectar tablas:

- **Super Key**: Cualquier conjunto de columnas que identifica una fila de forma única (puede tener columnas de más). 🧐
- **Candidate Key**: Una Super Key minimalista (el identificador único más pequeño posible). ✨
- **PRIMARY KEY**: La Candidate Key que elegimos como el identificador oficial (única por tabla, NOT NULL). 🛡️
- **Alternate Key**: Cualquier Candidate Key que no fue elegida como la Primary Key (se implementa con `UNIQUE`). 🥈
- **FOREIGN KEY**: Columna que referencia a una PK (o UNIQUE) en otra tabla para crear relaciones. 🤝
- **Composite Key**: Cualquier clave (PK, UNIQUE, etc.) que está formada por DOS o MÁS columnas. 🧩

Entender estos tipos de claves es fundamental para diseñar modelos de datos eficientes y correctos. ¡Vamos bien! 💪