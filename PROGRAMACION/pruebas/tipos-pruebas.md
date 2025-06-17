---
aliases:
  - 🔍 Tipos de Pruebas de Software
tags: 
breadcrumb:
  - "[[indice-web|🧑‍💻 Programación 💻✨]]"
Fecha: 2025-06-02
---
# 🔍 Tipos de Pruebas de Software
- [[#1. ¿Por qué Tantos Tipos? 🤔|1. ¿Por qué Tantos Tipos? 🤔]]
- [[#2. Pruebas Unitarias (Unit Tests) 🧱|2. Pruebas Unitarias (Unit Tests) 🧱]]
- [[#3. Pruebas de Integración (Integration Tests) 🧩|3. Pruebas de Integración (Integration Tests) 🧩]]
- [[#4. Pruebas de Sistema (System Tests) 🖥️|4. Pruebas de Sistema (System Tests) 🖥️]]
- [[#5. Pruebas de Aceptación (Acceptance Tests - UAT) 👍|5. Pruebas de Aceptación (Acceptance Tests - UAT) 👍]]
- [[#6. Otros Tipos Importantes (Mención Rápida) 🚀|6. Otros Tipos Importantes (Mención Rápida) 🚀]]
- [[#✨ Repaso Relámpago|✨ Repaso Relámpago]]

---
## 1. ¿Por qué Tantos Tipos? 🤔
No todas las pruebas son iguales ni buscan lo mismo. Usamos diferentes tipos de pruebas para **evaluar distintos aspectos** de una aplicación y en **diferentes momentos** del desarrollo. El objetivo final es siempre asegurar la **calidad** y encontrar errores antes de que lleguen al usuario.
- Cada tipo se enfoca en un **nivel** o **característica** específica.
- Ayudan a construir confianza en el software.
📌 **Tip:** ¡Piensa en ello como diferentes especialistas médicos: cada uno revisa una parte diferente para asegurar que todo esté sano!
---
## 2. Pruebas Unitarias (Unit Tests) 🧱
Estas son las pruebas más básicas. Se enfocan en la **unidad más pequeña de código** que se puede aislar, como una función, un método de una clase o un componente individual.
- **Objetivo**: Verificar que cada "ladrillo" del software funcione correctamente por sí solo.
- **Quién**: Generalmente las escriben los desarrolladores.
- **Características**: Rápidas de ejecutar, aisladas.
**Ejemplo Mini**: Si tienes una función `sumar(a, b)`, una prueba unitaria verificaría que `sumar(2, 3)` devuelve `5`.
🔬 **Tip:** ¡Son la base de una pirámide de pruebas sólida!

---
## 3. Pruebas de Integración (Integration Tests) 🧩
Una vez que las unidades funcionan bien por separado, las pruebas de integración verifican que **diferentes partes o módulos del sistema trabajen juntos correctamente**.
- **Objetivo**: Encontrar errores en las interfaces y las interacciones entre componentes.
- **Quién**: Desarrolladores o testers especializados.
- **Características**: Más lentas que las unitarias, pueden requerir entornos más complejos.
**Ejemplo Mini**: Probar si el módulo de "carrito de compras" se comunica bien con el módulo de "inventario" para verificar la disponibilidad de un producto.
🤝 **Tip:** ¡Aseguran que las piezas del rompecabezas encajen bien!

---
## 4. Pruebas de Sistema (System Tests) 🖥️
Estas pruebas evalúan el **sistema completo y totalmente integrado** para verificar que cumple con los requisitos especificados. Se prueba el software como si fuera un usuario final.
- **Objetivo**: Validar el comportamiento del sistema completo contra los requisitos funcionales y no funcionales.
- **Quién**: Generalmente un equipo de QA (Quality Assurance) independiente.
- **Características**: Se realizan en un entorno lo más parecido posible al de producción.
**Ejemplo Mini**: Probar el flujo completo de un usuario registrándose, buscando un producto, añadiéndolo al carrito y finalizando la compra en una tienda online.
🌍 **Tip:** ¡Es como una prueba de manejo general antes de entregar el coche!

---
## 5. Pruebas de Aceptación (Acceptance Tests - UAT) 👍
También conocidas como Pruebas de Aceptación del Usuario (UAT). Estas pruebas las realiza el **cliente o el usuario final** para asegurarse de que el sistema satisface sus necesidades y expectativas y pueden "aceptar" el producto.
- **Objetivo**: Confirmar que el software está listo para ser usado en el mundo real y cumple con los criterios de negocio.
- **Quién**: Cliente, usuarios finales, a veces con apoyo del equipo de QA.
- **Características**: Es una de las últimas fases antes del lanzamiento.
**Ejemplo Mini**: El cliente de una tienda online realiza una serie de casos de uso que representan sus operaciones diarias y da el visto bueno.
✅ **Tip:** ¡La prueba de fuego para ver si el cliente está contento!

---
## 6. Otros Tipos Importantes (Mención Rápida) 🚀
Hay muchos más, pero aquí van algunos clave:
- **Pruebas de Regresión**: Aseguran que los cambios nuevos no hayan roto funcionalidades que ya existían y funcionaban bien. 🔙
- **Pruebas de Rendimiento (Performance)**: Evalúan la velocidad, estabilidad y escalabilidad del sistema bajo carga (ej. pruebas de carga, estrés). 💨
- **Pruebas de Usabilidad**: Verifican qué tan fácil e intuitivo es usar el sistema para un usuario. 🧑‍💻
- **Pruebas de Seguridad**: Buscan vulnerabilidades y protegen contra ataques. 🛡️
✨ **Tip:** La combinación adecuada de estos tipos de pruebas depende del proyecto y sus riesgos.

---
## ✨ Repaso Relámpago
- **Unitarias**: Pequeñas partes de código.
- **Integración**: Cómo interactúan las partes.
- **Sistema**: El software completo funcionando.
- **Aceptación (UAT)**: El cliente valida.
- **Otros**: Regresión, rendimiento, usabilidad, seguridad, etc.