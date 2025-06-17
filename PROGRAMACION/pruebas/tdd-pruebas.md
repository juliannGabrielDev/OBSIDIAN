---
aliases:
  - 🧪 Desarrollo Dirigido por Pruebas (TDD)
tags:
  - pruebas
breadcrumb:
  - "[[indice-web|🧑‍💻 Programación 💻✨]]"
Fecha: 2025-06-02
---
# 🧪 Desarrollo Dirigido por Pruebas (TDD)
- [[#1. ¿Qué es TDD? 🎯|1. ¿Qué es TDD? 🎯]]
- [[#2. El Ciclo TDD: Rojo-Verde-Refactorizar ♻️|2. El Ciclo TDD: Rojo-Verde-Refactorizar ♻️]]
- [[#3. Ventajas Principales ✨|3. Ventajas Principales ✨]]
- [[#4. Herramientas Comunes (Ejemplos) 🧰|4. Herramientas Comunes (Ejemplos) 🧰]]
- [[#✨ Repaso Relámpago|✨ Repaso Relámpago]]

---
## 1. ¿Qué es TDD? 🎯
El **Desarrollo Dirigido por Pruebas** (Test-Driven Development) es una forma de programar donde **primero escribes una prueba automática que falla**, luego escribes el código mínimo necesario para que esa prueba pase, y finalmente refactorizas (limpias y mejoras) el código.
- Se enfoca en ciclos cortos de desarrollo.
- Las pruebas guían el diseño de tu código.
- Busca crear software robusto y bien probado desde el inicio.
📌 **Tip:** ¡Es como construir con un plano (la prueba) antes de poner los ladrillos (el código)!
---
## 2. El Ciclo TDD: Rojo-Verde-Refactorizar ♻️
Este es el corazón de TDD y se repite constantemente para cada pequeña funcionalidad:
1. 🔴 **Rojo**:
    - Escribes una **prueba automatizada** para una pequeña funcionalidad que aún no existe.
    - Ejecutas la prueba. Obviamente, **falla** (se pone "roja") porque el código para esa funcionalidad todavía no está escrito.
2. 🟢 **Verde**:
    - Escribes el **código más simple y mínimo posible** para que la prueba que acabas de escribir **pase** (se ponga "verde").
    - No te preocupes por la perfección en este paso, solo por hacer que la prueba pase.
3. 🔵 **Refactorizar**:
    - Ahora que la prueba pasa, puedes **mejorar y limpiar el código** que escribiste (tanto el de la prueba como el funcional).
    - Elimina duplicación, mejora la legibilidad, optimiza si es necesario, ¡pero sin cambiar el comportamiento externo! (Las pruebas te aseguran esto).
🧐 **Tip:** ¡Rojo -> Verde -> Refactorizar! Repite este mantra hasta que la funcionalidad esté completa.

---
## 3. Ventajas Principales ✨
Adoptar TDD trae consigo varios beneficios importantes:
- **Código más robusto**: Al tener pruebas para cada parte, reduces errores.
- **Mejor diseño**: Pensar en cómo probar algo te obliga a diseñar componentes más pequeños y desacoplados.
- **Red de seguridad**: Las pruebas te avisan si rompes algo al hacer cambios o refactorizar.
- **Documentación viva**: Las pruebas sirven como ejemplos de cómo usar tu código.
- **Menos tiempo depurando**: Encuentras errores antes y de forma más aislada.
🛠️ **Tip:** ¡TDD es una inversión inicial que te ahorra muchos dolores de cabeza a largo plazo!
---
## 4. Herramientas Comunes (Ejemplos) 🧰
Existen muchos frameworks y bibliotecas para ayudarte a escribir y ejecutar pruebas automatizadas, dependiendo del lenguaje:
- **Python**: `unittest` (integrado), `pytest`.
- **Java**: `JUnit`, `TestNG`.
- **JavaScript**: `Jest`, `Mocha`, `Jasmine`.
- **Ruby**: `RSpec`, `Minitest`.
⚙️ **Tip:** Elige la herramienta que mejor se adapte a tu lenguaje y proyecto. ¡Lo importante es la disciplina TDD!

---
## ✨ Repaso Relámpago
- TDD: **Pruebas primero**, luego código.
- Ciclo: **Rojo** (prueba falla) -> **Verde** (código mínimo para pasar) -> **Refactorizar** (mejorar).
- Beneficios: Código **robusto**, buen **diseño**, **seguridad** al cambiar.
- Usa **frameworks de prueba** para facilitar el proceso.