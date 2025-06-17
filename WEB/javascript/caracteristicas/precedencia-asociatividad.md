---
aliases:
  - ¡Precedencia y Asociatividad! 🌐
tags:
  - js
  - caracteristicas
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-javascript|⚡ Índice JavaScript]]"
Fecha: 2025-06-05
---
# ¡Precedencia y Asociatividad en JavaScript! 🌐

Los conceptos de precedencia y asociatividad son universales en la programación, pero sus reglas específicas pueden variar un poco entre lenguajes. En JavaScript, funcionan de manera muy similar a Python y a la mayoría de los lenguajes tipo C.

---

## 🧐 Precedencia de Operadores en JavaScript

La **precedencia** en JavaScript sigue un orden jerárquico que determina qué operadores se evalúan primero en una expresión. La tabla de precedencia es bastante extensa, pero aquí te presento algunos de los operadores más comunes, de mayor a menor precedencia:

### 📋 Ejemplos de Precedencia (de mayor a menor)

1. **Paréntesis `()`:** Siempre son lo primero. Permiten agrupar y forzar el orden de evaluación.
    ```js
    let resultado = (5 + 3) * 2; // (8) * 2 = 16
    console.log(resultado); // 16
    ```
    
2. **Acceso a Propiedades `.` y Llamada a Funciones `()`:**
    
    ```js
    let obj = {
        prop: function() { return 10; }
    };
    let valor = obj.prop(); // Primero accede a 'prop', luego llama a la función
    console.log(valor); // 10
    ```
    
3. **Operadores Unarios (como `++`, `--`, `!`, `typeof`, `-` unario, `+` unario):**
    
    ```js
    let a = 5;
    let b = -a; // -5
    let c = !true; // false
    console.log(b, c); // -5 false
    ```
    
4. **Exponenciación `**`:** (Introducido en ES2016)
    
    ```js
    let potencia = 2 ** 3; // 2 elevado a 3 = 8
    console.log(potencia); // 8
    ```
    
5. **Multiplicación `*`, División `/`, Módulo `%`:**
    
    ```js
    let res = 10 - 2 * 3; // Primero 2 * 3 = 6, luego 10 - 6 = 4
    console.log(res); // 4
    ```
    
6. **Suma `+`, Resta `-`:**
    
    ```js
    let res2 = 5 + 3 - 1; // Se evalúa después de *, /, %
    console.log(res2); // 7
    ```
    
7. **Operadores de Comparación (`<`, `>`, `<=`, `>=`, `instanceof`, `in`):**
    
    ```js
    let comparacion = 5 * 2 > 8 + 1; // (10 > 9) = true
    console.log(comparacion); // true
    ```
    
8. **Operadores de Igualdad (`==`, `!=`, `===`, `!==`):**
        
    ```js
    let igualdad = 10 / 2 === 5; // (5 === 5) = true
    console.log(igualdad); // true
    ```
    
9. **Operador Lógico AND `&&`:**

    ```js
    let logicoAnd = true && false || true; // (true && false) = false, luego (false || true) = true
    console.log(logicoAnd); // true
    ```
    
10. **Operador Lógico OR `||`:**

    ```js
    let logicoOr = false || true && false; // (true && false) = false, luego (false || false) = false
    console.log(logicoOr); // false
    ```
    
11. **Operador Ternario (Condicional) `? :`:**

    ```js
    let edad = 18;
    let mensaje = (edad >= 18) ? "Adulto" : "Menor";
    console.log(mensaje); // Adulto
    ```
    
12. **Operadores de Asignación (`=`, `+=`, `-=`, etc.):** Siempre lo último en evaluar, ya que asignan un valor.
        
    ```js
    let x = 5;
    x += 3 * 2; // Primero 3 * 2 = 6, luego x += 6 => x = x + 6 = 11
    console.log(x); // 11
    ```
    

---

## 🔗 Asociatividad de Operadores en JavaScript

La **asociatividad** en JavaScript, al igual que en otros lenguajes, define el orden de evaluación para operadores con la **misma precedencia**.

### Tipos de Asociatividad:

1. **De Izquierda a Derecha (Left-to-Right):** La mayoría de los operadores en JavaScript son de izquierda a derecha. Se evalúan en el orden en que aparecen.
    
    - Ejemplos: `+`, `-`, `*`, `/`, `%`, `&&`, `||`, operadores de comparación, etc.

    ```js
    let res = 10 - 5 + 2;
    // (10 - 5) = 5
    // 5 + 2 = 7
    console.log(res); // 7
    ```
    
2. **De Derecha a Izquierda (Right-to-Left):** Menos comunes, pero importantes.
    
    - Ejemplos: Operadores de asignación (`=`, `+=`, `-=`, etc.), el operador de exponenciación `**`, operadores unarios (como `!`, `typeof`), el operador ternario `? :`.

    ```js
    // Exponenciación
    let exp = 2 ** 3 ** 2;
    // Primero 3 ** 2 = 9
    // Luego 2 ** 9 = 512
    console.log(exp); // 512
    
    // Asignación encadenada
    let a, b, c;
    a = b = c = 10; // c = 10, luego b = c, luego a = b
    console.log(a, b, c); // 10 10 10
    ```

---

## 🚀 Mejores Prácticas

- **¡Usa Paréntesis!** Siempre que tengas dudas sobre la precedencia o asociatividad, o si la expresión podría ser confusa para otro desarrollador (o para ti mismo en el futuro), **¡usa paréntesis!** Hacen el código explícito y mucho más legible.
    
    ```js
    // En lugar de:
    let valorComplejo = a + b * c / d - e;
    // Usa:
    let valorClaro = (a + (b * c) / d) - e; // ¡Mucho más claro!
    ```
    
- **Consulta la Documentación:** Si trabajas con operadores menos comunes o con combinaciones complejas, siempre es una buena idea consultar la tabla de precedencia oficial de MDN Web Docs para JavaScript, que es muy completa.