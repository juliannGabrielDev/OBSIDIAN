# Restricciones en el Modelo Relacional (Nota Corta)

Las **restricciones** (o _constraints_) son reglas que se definen en el esquema de la base de datos para asegurar la **exactitud, validez e integridad** de los datos. ¡Son fundamentales para mantener la calidad de nuestra información!

---

## 1. Restricciones de Clave 🔑

Estas restricciones se centran en asegurar la **unicidad** de las filas o la identificación única dentro de las tablas.

- **Clave Primaria (Primary Key)**:
    
    - **Propósito**: Identifica de forma **única** cada fila (tupla) en una tabla.
    - **Características**: No puede contener valores `NULL` y sus valores deben ser únicos en toda la columna (o conjunto de columnas) designada. Solo puede haber una clave primaria por tabla.
    - _Ejemplo_: `ID_Cliente` en una tabla `Clientes`.
- **Única (Unique Constraint)**:
    
    - **Propósito**: Asegura que todos los valores en una columna (o conjunto de columnas) sean diferentes entre sí, pero **a diferencia de la PK, puede permitir un valor `NULL`** (dependiendo del SGBD, a veces más de uno).
    - _Ejemplo_: El `Email` en una tabla `Usuarios` podría ser una restricción `UNIQUE`.

---

## 2. Restricciones de Dominio 🏷️

Definen el conjunto de **valores válidos** que un atributo (columna) puede tomar. Ayudan a asegurar que los datos sean del tipo correcto y estén dentro de un rango aceptable.

- **Tipo de Dato (Data Type)**:
    
    - **Propósito**: Especifica el tipo de información que la columna puede almacenar (ej. `INT`, `VARCHAR`, `DATE`, `BOOLEAN`).
    - _Ejemplo_: Una columna `Edad` debe ser `INT`.
- **NOT NULL**:
    
    - **Propósito**: Asegura que una columna **no pueda tener valores `NULL`** (desconocidos o vacíos). El campo debe tener siempre un valor.
    - _Ejemplo_: El `Nombre` de un producto no puede ser `NULL`.
- **CHECK**:
    
    - **Propósito**: Permite definir una condición que los valores de una columna deben cumplir.
    - _Ejemplo_: En una columna `Precio`, una restricción `CHECK (Precio > 0)` asegura que el precio sea siempre positivo. En una columna `Genero`, `CHECK (Genero IN ('M', 'F', 'Otro'))`.

---

## 3. Restricciones de Integridad Referencial 🔗

Estas restricciones mantienen la **consistencia entre tablas relacionadas** mediante el uso de claves foráneas. Aseguran que las relaciones sean válidas y que no haya "registros huérfanos".

- **Clave Foránea (Foreign Key)**:
    
    - **Propósito**: Una columna (o conjunto de columnas) en una tabla que hace referencia a la clave primaria de otra tabla (o la misma).
    - **Objetivo**: Garantiza que un valor en la columna de la clave foránea **exista** como un valor en la columna de la clave primaria referenciada, o que sea `NULL` (si se permite).
    - _Ejemplo_: En una tabla `Pedidos`, la columna `ID_Cliente` (FK) debe referenciar un `ID_Cliente` (PK) válido en la tabla `Clientes`.
- **Acciones Referenciales**:
    
    - **Propósito**: Definen qué sucede con los registros dependientes (en la tabla con la FK) cuando un registro referenciado (en la tabla con la PK) es **eliminado (`ON DELETE`)** o **actualizado (`ON UPDATE`)**.
    - _Opciones comunes_:
        - `CASCADE`: Si se elimina/actualiza el padre, se eliminan/actualizan los hijos.
        - `SET NULL`: Si se elimina/actualiza el padre, la FK en los hijos se establece a `NULL`.
        - `SET DEFAULT`: Si se elimina/actualiza el padre, la FK en los hijos se establece a su valor por defecto.
        - `RESTRICT` / `NO ACTION`: Impide la eliminación/actualización del padre si existen hijos. (Es el comportamiento por defecto en muchos SGBD).

---

## Mini Repaso Final 💡

- **Restricciones de Clave**: Aseguran unicidad (PK, UNIQUE).
- **Restricciones de Dominio**: Validan los datos de las columnas (Tipos, NOT NULL, CHECK).
- **Restricciones de Integridad Referencial**: Mantienen consistencia entre tablas relacionadas (FK y sus acciones).

¡Estas restricciones son tus aliadas para una base de datos confiable y bien estructurada! 👍