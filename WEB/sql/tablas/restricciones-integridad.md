---
aliases:
  - Restricciones de Integridad 🚫📝
tags:
  - sql
  - teoria
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-sql|📊 Índice SQL]]"
---
# Restricciones de Integridad 🚫📝
- [[#2. NOT NULL: ¡No Puede Estar Vacío!|2. NOT NULL: ¡No Puede Estar Vacío!]]
- [[#3. UNIQUE: ¡Valor Único! 💎|3. UNIQUE: ¡Valor Único! 💎]]
- [[#4. PRIMARY KEY: Identificador Único (¡Ya Visto!) 🔑🛡️|4. PRIMARY KEY: Identificador Único (¡Ya Visto!) 🔑🛡️]]
- [[#5. FOREIGN KEY: Enlazando Tablas (¡Ya Visto!) 🤝🔗|5. FOREIGN KEY: Enlazando Tablas (¡Ya Visto!) 🤝🔗]]
- [[#6. CHECK: Validando con una Condición 🤔✅|6. CHECK: Validando con una Condición 🤔✅]]
- [[#7. DEFAULT: Valor por Defecto ✨|7. DEFAULT: Valor por Defecto ✨]]
- [[#Mini Repaso Final ✨|Mini Repaso Final ✨]]

## 1. ¿Qué son las Restricciones de Integridad? 📐

- Son **reglas** definidas a nivel de la base de datos (en las tablas y columnas) que se aplican automáticamente para **garantizar la validez y consistencia** de los datos.
- Evitan que se inserten, actualicen o eliminen datos que violen esas reglas. ¡Es como un "guardián" de la calidad de los datos! 🛡️
- Se definen típicamente cuando **creamos la tabla** (`CREATE TABLE`) o cuando la **modificamos** (`ALTER TABLE`).

## 2. NOT NULL: ¡No Puede Estar Vacío!

- Esta restricción asegura que una columna **siempre debe tener un valor**.
    
- No se permite insertar o actualizar una fila si el valor de esa columna es `NULL` (vacío o desconocido).
    
- **Sintaxis básica:**
    
    ```sql
    nombre_columna tipo_dato NOT NULL
    ```
    
- **Ejemplo:** En la tabla `Personal`, el nombre de un empleado **no puede ser nulo**.
    
    ```sql
    CREATE TABLE Personal (
        id_empleado INT PRIMARY KEY,
        nombre VARCHAR(100) NOT NULL, -- ¡El nombre es obligatorio!
        puesto VARCHAR(50),
        fecha_contratacion DATE,
        email VARCHAR(100) UNIQUE,
        salario DECIMAL(10, 2),
        activo BOOLEAN
    );
    ```
    

## 3. UNIQUE: ¡Valor Único! 💎

- Esta restricción asegura que **todos los valores en una columna** (o un conjunto de columnas) **sean diferentes**.
    
- No permite insertar filas donde el valor ya existe en esa columna.
    
- **¡Ojo!** A diferencia de la `PRIMARY KEY`, una columna `UNIQUE` **puede contener valores `NULL`**, y puede haber varias filas con `NULL` en una columna `UNIQUE` (el estándar SQL lo permite, aunque algunos SGBD pueden tener variaciones).
    
- Una tabla puede tener múltiples restricciones `UNIQUE`, pero solo una `PRIMARY KEY`.
    
- **Sintaxis básica:**
    
    ```sql
    nombre_columna tipo_dato UNIQUE
    -- O a nivel de tabla para múltiples columnas
    -- UNIQUE (columna1, columna2)
    ```
    
- **Ejemplo:** El email de un empleado debe ser único.
    
    ```sql
    CREATE TABLE Personal (
        id_empleado INT PRIMARY KEY,
        nombre VARCHAR(100) NOT NULL,
        puesto VARCHAR(50),
        fecha_contratacion DATE,
        email VARCHAR(100) UNIQUE, -- ¡Cada email debe ser diferente!
        salario DECIMAL(10, 2),
        activo BOOLEAN
    );
    ```
    

## 4. PRIMARY KEY: Identificador Único (¡Ya Visto!) 🔑🛡️

- Ya la conoces de cuando hablamos de relaciones. La `PRIMARY KEY` es una restricción especial que es una **combinación** de `NOT NULL` y `UNIQUE`.
- Identifica de forma **única** a cada fila en la tabla.
- Una tabla **solo puede tener UNA** `PRIMARY KEY`.
- Se usa como el "objetivo" al que apuntan las `FOREIGN KEY`.
- **Sintaxis básica:**

```sql
nombre_columna tipo_dato PRIMARY KEY
-- O a nivel de tabla para una clave primaria compuesta
-- PRIMARY KEY (columna1, columna2)
```

- **Ejemplo:** El `id_empleado` es nuestra llave primaria.

```sql
CREATE TABLE Personal (
    id_empleado INT PRIMARY KEY, -- Único e irrepetible, y no puede ser NULL
    nombre VARCHAR(100) NOT NULL,
    puesto VARCHAR(50),
    fecha_contratacion DATE,
    email VARCHAR(100) UNIQUE,
    salario DECIMAL(10, 2),
    activo BOOLEAN
);
```

## 5. FOREIGN KEY: Enlazando Tablas (¡Ya Visto!) 🤝🔗

- También la vimos al hablar de relaciones. La `FOREIGN KEY` es una restricción que **crea un enlace** entre dos tablas.
    
- Una columna en una tabla (la tabla "hija" o de referencia) hace referencia a la `PRIMARY KEY` (o una columna `UNIQUE`) en otra tabla (la tabla "padre" o referenciada).
    
- Asegura la **integridad referencial**: no puedes tener un valor en la columna `FOREIGN KEY` que no exista en la columna referenciada de la tabla padre. ¡No puedes tener un pedido de un cliente que no existe! 🙅‍♀️
    
- **Sintaxis básica (a nivel de tabla, que es lo más común):**
    
    ```sql
    FOREIGN KEY (columna_fk) REFERENCES tabla_padre(columna_pk)
    [ ON DELETE accion ]
    [ ON UPDATE accion ]
    ```
    

Las **acciones `ON DELETE` y `ON UPDATE`** son super importantes para definir qué pasa con las filas hijas cuando la fila padre referenciada es eliminada o actualizada:

- **`CASCADE`**: Si se elimina/actualiza el padre, también se eliminan/actualizan las filas hijas correspondientes. ¡Efecto dominó! 🌊
- **`SET NULL`**: Si se elimina/actualiza el padre, el valor de la columna FK en las filas hijas se establece a `NULL` (si la columna FK no tiene `NOT NULL`).
- **`RESTRICT`** (o `NO ACTION`): Impide la eliminación/actualización del padre si existen filas hijas que lo referencian. ¡Es la opción más segura por defecto en muchos SGBD! 🛑
- **Ejemplo:** En la tabla `Pedidos`, la columna `id_cliente` es una FK que referencia a `Clientes`.

```sql
CREATE TABLE Clientes (
    id_cliente INT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL
);

CREATE TABLE Pedidos (
    id_pedido INT PRIMARY KEY,
    id_cliente INT, -- Esta es la columna FK
    total DECIMAL(10, 2),
    -- Definimos la restricción FOREIGN KEY:
    FOREIGN KEY (id_cliente) REFERENCES Clientes(id_cliente)
    ON DELETE CASCADE -- Si se elimina un cliente, ¡se eliminan todos sus pedidos! (¡Cuidado con esto!)
    ON UPDATE CASCADE -- Si cambia el ID del cliente, se actualiza automáticamente en los pedidos
);
```

## 6. CHECK: Validando con una Condición 🤔✅

- Esta restricción te permite definir una **condición personalizada** que los valores de una columna (o de una fila completa) deben cumplir.
    
- La base de datos verifica la condición cada vez que intentas insertar o actualizar una fila.
    
- **Sintaxis básica:**
    
    ```sql
    nombre_columna tipo_dato CHECK (condicion_de_la_columna)
    -- O a nivel de tabla
    -- CHECK (condicion_de_la_fila)
    ```
    
- **Ejemplo:** El salario de un empleado debe ser positivo, y el puesto no puede ser 'Practicante' si es activo.
    
    ```sql
    CREATE TABLE Personal (
        id_empleado INT PRIMARY KEY,
        nombre VARCHAR(100) NOT NULL,
        puesto VARCHAR(50),
        fecha_contratacion DATE,
        email VARCHAR(100) UNIQUE,
        salario DECIMAL(10, 2) CHECK (salario >= 0), -- El salario no puede ser negativo
        activo BOOLEAN,
        -- Restricción a nivel de tabla: si está activo, el puesto no es 'Practicante'
        CHECK (NOT (activo = TRUE AND puesto = 'Practicante'))
    );
    ```
    

`CHECK` te da mucha flexibilidad para poner reglas específicas de tu negocio.

## 7. DEFAULT: Valor por Defecto ✨

- Esta no es una restricción de "integridad" en el sentido de validar datos, sino de proporcionar un **valor predeterminado** a una columna si no se especifica un valor durante una inserción.
    
- Asegura que la columna tenga un valor aunque no lo proporciones tú.
    
- **Sintaxis básica:**
    
    ```sql
    nombre_columna tipo_dato DEFAULT valor_predeterminado
    ```
    
- **Ejemplo:** Si no especificamos si un empleado está activo al insertarlo, por defecto asumimos que sí lo está.
    
    ```sql
    CREATE TABLE Personal (
        id_empleado INT PRIMARY KEY,
        nombre VARCHAR(100) NOT NULL,
        puesto VARCHAR(50),
        fecha_contratacion DATE,
        email VARCHAR(100) UNIQUE,
        salario DECIMAL(10, 2) CHECK (salario >= 0),
        activo BOOLEAN DEFAULT TRUE -- Si no se especifica, será TRUE
        -- CHECK (NOT (activo = TRUE AND puesto = 'Practicante')) -- Restricción anterior
    );
    ```
    

`DEFAULT` simplifica las sentencias `INSERT` cuando hay valores comunes.

## Mini Repaso Final ✨

¡Listo! Las **restricciones de integridad** son reglas para asegurar la calidad y consistencia de los datos en tu base de datos. Se definen con DDL (`CREATE TABLE`, `ALTER TABLE`). Las más importantes son:

- **`NOT NULL`**: El valor no puede ser vacío. 🚫📝
- **`UNIQUE`**: Todos los valores deben ser diferentes. 💎
- **`PRIMARY KEY`**: Identificador único (NOT NULL + UNIQUE). 🔑🛡️
- **`FOREIGN KEY`**: Enlaza tablas y mantiene la integridad referencial (con opciones `ON DELETE`/`ON UPDATE`). 🤝🔗
- **`CHECK`**: Valida datos con una condición personalizada. 🤔✅
- **`DEFAULT`**: Asigna un valor si no se proporciona uno. ✨

¡Usar restricciones adecuadamente es clave para tener una base de datos robusta y con datos confiables! 💪