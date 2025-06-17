---
aliases:
  - Normalización de Bases de Datos
tags:
  - sql
  - teoria
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-sql|📊 Índice SQL]]"
---
# Normalización de Bases de Datos

¡Uff, la normalización es como poner en orden nuestra habitación para encontrar todo más fácil! 🧹 En las bases de datos, significa organizar las tablas y columnas para reducir la **redundancia** de datos y mejorar la **integridad** de los mismos. ¡Así evitamos problemas y la base funciona mejor!
- [[#1. ¿Qué es la Normalización? 🤔|1. ¿Qué es la Normalización? 🤔]]
- [[#2. Anomalías (¡Problemas que queremos evitar!) 🚨|2. Anomalías (¡Problemas que queremos evitar!) 🚨]]
- [[#3. Las Formas Normales (¡Las reglas de oro! ✨)|3. Las Formas Normales (¡Las reglas de oro! ✨)]]
	- [[#3. Las Formas Normales (¡Las reglas de oro! ✨)#3.1. Primera Forma Normal (1FN) 🥇|3.1. Primera Forma Normal (1FN) 🥇]]
	- [[#3. Las Formas Normales (¡Las reglas de oro! ✨)#3.2. Segunda Forma Normal (2FN) 🥈|3.2. Segunda Forma Normal (2FN) 🥈]]
	- [[#3. Las Formas Normales (¡Las reglas de oro! ✨)#3.3. Tercera Forma Normal (3FN) 🥉|3.3. Tercera Forma Normal (3FN) 🥉]]
- [[#4. ¿Por qué es tan importante? 💪|4. ¿Por qué es tan importante? 💪]]
- [[#5. Desnormalización (¡A veces hay excepciones! 😅)|5. Desnormalización (¡A veces hay excepciones! 😅)]]
	- [[#5. Desnormalización (¡A veces hay excepciones! 😅)#¡Mini Repaso! 📝|¡Mini Repaso! 📝]]

## 1. ¿Qué es la Normalización? 🤔

La normalización es un proceso que usamos para diseñar bases de datos relacionales "saludables". Consiste en aplicar un conjunto de reglas, llamadas **formas normales** (FN), para estructurar nuestras tablas de manera eficiente.

- **Objetivos principales:**
    - **Reducir la redundancia de datos:** Esto significa que la misma información no se repite en muchos lugares, ¡así ahorramos espacio y evitamos errores! 🚫
    - **Mejorar la integridad de los datos:** Nos aseguramos de que la información sea consistente y correcta. Por ejemplo, que un nombre de cliente se escriba igual en todas partes. ✅
    - **Eliminar anomalías de actualización:** Si cambiamos un dato, no queremos tener que cambiarlo en mil sitios. La normalización lo facilita.
    - **Facilitar consultas y mantenimiento:** Una base de datos bien organizada es más fácil de entender y trabajar con ella.

## 2. Anomalías (¡Problemas que queremos evitar!) 🚨

Cuando una base de datos no está normalizada, pueden aparecer unas "anomalías" que son un dolor de cabeza:

- **Anomalía de Inserción:** No podemos añadir un dato si no tenemos otro dato relacionado. Imagina querer registrar un curso sin tener alumnos inscritos, ¡pero la tabla nos pide que al menos haya un alumno! 🤦‍♀️
    - **Ejemplo:** En una tabla desnormalizada de `Cursos_Alumnos`, no podríamos añadir un nuevo curso hasta que no tuviéramos al menos un alumno para asignarle.
- **Anomalía de Actualización:** Si un dato se repite, tenemos que actualizarlo en todos los lugares donde aparece. ¡Imagínate cambiar el nombre de un profesor en 20 filas! Si se nos olvida una, ¡tendremos inconsistencia! 😫
    - **Ejemplo:** Si el nombre de un profesor aparece en varias filas de una tabla `Asignaturas_Profesor`, y el profesor cambia de nombre, tendríamos que actualizar cada una de esas filas.
- **Anomalía de Eliminación:** Si borramos una fila, podemos perder información importante que no estaba duplicada. ¡Oops! 🙈
    - **Ejemplo:** Si en la misma tabla `Cursos_Alumnos` eliminamos la última fila de un curso, podríamos perder toda la información sobre ese curso si no está en otro lugar.

## 3. Las Formas Normales (¡Las reglas de oro! ✨)

Existen varias formas normales, pero las más comunes y las que casi siempre usamos son las tres primeras: 1FN, 2FN y 3FN. ¡Hay más, pero estas son las básicas para empezar!

### 3.1. Primera Forma Normal (1FN) 🥇

¡Esta es la más básica! Una tabla está en 1FN si cumple estas condiciones:

- **Valores atómicos:** Cada columna debe contener solo un valor único. No puede haber listas o grupos de valores dentro de una celda.
    - **¡No! ❌:** Una columna `Teléfonos` con "555-1234, 555-5678"
    - **¡Sí! ✅:** Tener dos columnas `Teléfono1`, `Teléfono2` o mejor aún, una tabla separada de teléfonos.
- **No hay grupos repetitivos:** No tenemos columnas que se repitan con un número (como `Producto1`, `Producto2`, `Producto3`).
- **Clave primaria definida:** Cada fila debe ser única y tener una forma de identificarla (la famosa **clave primaria**).

**Antes de 1FN:**

| **ID_Pedido** | **Productos** | **Cantidades** |
| ------------- | ------------- | -------------- |
| 1             | Laptop, Mouse | 1, 1           |
| 2             | Teclado       | 1              |

**Después de 1FN:**

| **ID_Pedido** | **Producto** | **Cantidad** |
| ------------- | ------------ | ------------ |
| 1             | Laptop       | 1            |
| 1             | Mouse        | 1            |
| 2             | Teclado      | 1            |

### 3.2. Segunda Forma Normal (2FN) 🥈

Para que una tabla esté en 2FN, primero debe estar en 1FN. Además:

- **Cada atributo no clave debe depender completamente de la clave primaria.** Esto es súper importante si tenemos claves primarias compuestas (formadas por varias columnas).
    - Si tenemos una clave primaria formada por `(ID_Alumno, ID_Curso)`, y la columna `Nombre_Alumno` depende solo de `ID_Alumno` (y no de `ID_Curso`), ¡entonces no está en 2FN! `Nombre_Alumno` debería ir en una tabla de `Alumnos`.

**Antes de 2FN:**

| **ID_Curso** | **Nombre_Curso** | **ID_Profesor** | **Nombre_Profesor** |
| ------------ | ---------------- | --------------- | ------------------- |
| C001         | Matemáticas      | P001            | Juan Pérez          |
| C002         | Física           | P001            | Juan Pérez          |
| C003         | Química          | P002            | Ana López           |

Aquí, `ID_Curso` es la clave primaria. `Nombre_Profesor` depende de `ID_Profesor`, no directamente de `ID_Curso`.

**Después de 2FN:**

**Tabla Cursos:**

| **ID_Curso** | **Nombre_Curso** | **ID_Profesor** |
| ------------ | ---------------- | --------------- |
| C001         | Matemáticas      | P001            |
| C002         | Física           | P001            |
| C003         | Química          | P002            |

**Tabla Profesores:**

| **ID_Profesor** | **Nombre_Profesor** |
| --------------- | ------------------- |
| P001            | Juan Pérez          |
| P002            | Ana López           |

### 3.3. Tercera Forma Normal (3FN) 🥉

Para que una tabla esté en 3FN, primero debe estar en 2FN. Además:

- **No debe haber dependencias transitivas.** Esto significa que ningún atributo no clave debe depender de otro atributo no clave. Si una columna `X` determina una columna `Y`, y `Y` determina una columna `Z`, ¡eso es una dependencia transitiva! `Z` debería ir en su propia tabla.

**Antes de 3FN:**

| **ID_Pedido** | **Fecha_Pedido** | **ID_Cliente** | **Nombre_Cliente** | **Ciudad_Cliente** |
| ------------- | ---------------- | -------------- | ------------------ | ------------------ |
| P001          | 2023-10-26       | CL001          | María García       | Madrid             |
| P002          | 2023-10-27       | CL002          | Pedro Sánchez      | Barcelona          |

Aquí, `ID_Pedido` es la clave primaria. `Ciudad_Cliente` depende de `ID_Cliente`, y `ID_Cliente` es un atributo no clave.

**Después de 3FN:**

**Tabla Pedidos:**

| **ID_Pedido** | **Fecha_Pedido** | **ID_Cliente** |
| ------------- | ---------------- | -------------- |
| P001          | 2023-10-26       | CL001          |
| P002          | 2023-10-27       | CL002          |

**Tabla Clientes:**

| **ID_Cliente** | **Nombre_Cliente** | **Ciudad_Cliente** |
| -------------- | ------------------ | ------------------ |
| CL001          | María García       | Madrid             |
| CL002          | Pedro Sánchez      | Barcelona          |

## 4. ¿Por qué es tan importante? 💪

Normalizar una base de datos nos trae muchos beneficios:

- **Menos errores:** Al reducir la redundancia, hay menos posibilidades de inconsistencias.
- **Menor espacio de almacenamiento:** No repetimos los mismos datos una y otra vez.
- **Consultas más rápidas:** Aunque parezca contradictorio a veces se necesitan más "joins" (uniones de tablas), una base bien diseñada puede ser más eficiente.
- **Mantenimiento más sencillo:** Si cambiamos un dato, solo lo hacemos en un lugar.
- **Mejor escalabilidad:** La base de datos es más flexible para crecer y añadir nuevas funcionalidades.

## 5. Desnormalización (¡A veces hay excepciones! 😅)

Aunque la normalización es genial, a veces, para mejorar el **rendimiento** de ciertas consultas (especialmente en bases de datos muy grandes o sistemas de data warehousing), podemos hacer un proceso llamado **desnormalización**. Esto es básicamente introducir algo de redundancia de forma controlada para que algunas consultas sean más rápidas, ¡pero siempre con cuidado para no caer en las anomalías! Es un balance entre la optimización y la integridad.

---

### ¡Mini Repaso! 📝

¡Bueno, gente! En resumen, la **normalización de bases de datos** es un proceso esencial para que nuestras bases de datos sean eficientes y confiables. La idea es organizar la información en tablas para evitar **redundancia** y mantener la **integridad** de los datos. Nos ayuda a prevenir las **anomalías de inserción, actualización y eliminación**. Las **formas normales** (1FN, 2FN, 3FN son las principales) son las reglas que seguimos para lograrlo. ¡Y aunque la normalización es lo ideal, a veces la **desnormalización** puede ser útil para optimizar el rendimiento! ¡Así que ya saben, una base de datos bien normalizada es una base de datos feliz! 😊