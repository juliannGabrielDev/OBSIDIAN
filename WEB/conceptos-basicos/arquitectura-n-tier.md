---
aliases:
  - Arquitectura N-Tier 💻🏛️✨
tags:
  - basicos
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[Indice-conceptos-basicos|💡 Índice de Conceptos Básicos]]"
---
# Arquitectura N-Tier 💻🏛️✨
La **arquitectura N-Tier** (o de múltiples capas) es un modelo de diseño de software que divide una aplicación en distintas **capas lógicas** y **niveles físicos**. Cada capa tiene una responsabilidad específica, lo que promueve la separación de intereses y una mejor gestión de las dependencias. 🧩

---
## 🏗️ Capas Fundamentales
Este modelo organiza una aplicación en las siguientes capas principales:
- **Capa de Presentación 🎨 (Frontend):** Es la interfaz con la que el usuario interactúa directamente. Se encarga de mostrar los datos y capturar las entradas del usuario.
    - **Tecnologías:** HTML, CSS, JavaScript, React, Angular, Vue.js.
- **Capa de Lógica de Negocio 🧠 (Backend):** Procesa las entradas del usuario, aplica las reglas de negocio y toma decisiones. Actúa como intermediario entre la capa de presentación y la capa de datos.
    - **Tecnologías:** Java (Spring), C# (.NET), Python (Django, Flask), Node.js.
- **Capa de Acceso a Datos 💾:** Se encarga de la comunicación con la base de datos, gestionando el almacenamiento y la recuperación de la información.
    - **Tecnologías:** JDBC, Hibernate, Entity Framework.
- **Capa de Datos 🗃️:** Es donde residen los datos, generalmente en un sistema de gestión de bases de datos (SGBD).
    - **Tecnologías:** MySQL, PostgreSQL, Microsoft SQL Server, Oracle.
> [!INFO] Nivel vs. Capa
> 
> ℹ️ Aunque a menudo se usan indistintamente, "capa" se refiere a una división lógica del código, mientras que "nivel" se refiere a la infraestructura física donde se ejecuta esa capa. En una arquitectura de 3 niveles, cada capa puede ejecutarse en un servidor diferente.

---

## 🌟 Ventajas y Desventajas

| **Ventajas 👍**              | **Desventajas 👎**                   |
| ---------------------------- | ------------------------------------ |
| Escalabilidad                | Mayor Complejidad                    |
| Mantenimiento Simplificado   | Posibles Cuellos de Botella          |
| Flexibilidad Tecnológica     | Incremento en la Latencia de Red     |
| Reutilización de Componentes | Costos de Desarrollo y Mantenimiento |
| Seguridad Mejorada           | Requiere un Diseño Cuidadoso         |
| Desarrollo Paralelo          | Depuración más Compleja              |

---
## 🚀 Ejemplos de Implementación
La arquitectura N-Tier es muy común en diversas aplicaciones:
- **Aplicaciones Web de Comercio Electrónico:** 🛒
    - **Presentación:** El sitio web donde los clientes navegan y compran.
    - **Lógica:** Gestiona el carrito de compras, procesa pagos y calcula envíos.
    - **Datos:** Almacena información de productos, clientes y pedidos.
- **Sistemas de Gestión Empresarial (ERP, CRM):** 🏢
    - **Presentación:** La interfaz de escritorio o web para los empleados.
    - **Lógica:** Automatiza procesos de negocio como finanzas, recursos humanos y ventas.
    - **Datos:** Centraliza toda la información crítica de la empresa.
- **Aplicaciones Bancarias en Línea:** 🏦
    - **Presentación:** El portal web o la aplicación móvil del banco.
    - **Lógica:** Valida transacciones, verifica saldos y aplica medidas de seguridad.
    - **Datos:** Guarda de forma segura las cuentas de los clientes y el historial de transacciones.
> [!WARNING] Cuidado con el Monolito Disfrazado
> 
> ⚠️ Si las capas no están bien definidas y desacopladas, una aplicación N-Tier puede convertirse en un "monolito distribuido", heredando la complejidad de un sistema distribuido sin obtener sus beneficios de escalabilidad y flexibilidad.

> [!TIP] Empieza con 3 Capas
> 
> 💡 La arquitectura de 3 capas (Presentación, Lógica de Negocio y Datos) es el punto de partida más común y efectivo para la mayoría de las aplicaciones. Solo añade más capas si la complejidad del sistema realmente lo justifica.