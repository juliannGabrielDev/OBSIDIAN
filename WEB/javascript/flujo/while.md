---
aliases:
  - Bucle while 🔄
tags:
  - js
  - flujo-control
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-javascript|⚡ Índice JavaScript]]"
Fecha: 2025-06-05
---
# Bucle `while` en JavaScript 🔄

El bucle `while` en JavaScript te permite **ejecutar un bloque de código repetidamente mientras una condición específica sea verdadera**. Es perfecto cuando no sabes de antemano cuántas veces necesitas que se repita una acción.

## 💻 Ejemplo de Código

Aquí te muestro un ejemplo sencillo de cómo usar un bucle `while` para contar del 0 al 4:

```js
let contador = 0; // Inicializamos una variable que controlará el bucle

while (contador < 5) { // Condición: el bucle se ejecuta mientras 'contador' sea menor que 5
    console.log("El número actual es: " + contador);
    contador++; // ¡Importante! Incrementamos el contador para que la condición eventualmente se vuelva falsa
}

console.log("¡El bucle ha terminado!");
```

---

## 🚀 Salida del Código

Cuando ejecutes el código anterior, verás la siguiente salida en tu consola:

```
El número actual es: 0
El número actual es: 1
El número actual es: 2
El número actual es: 3
El número actual es: 4
¡El bucle ha terminado!
```

Como puedes notar, el mensaje se imprime cinco veces (para 0, 1, 2, 3 y 4) y luego, cuando `contador` llega a 5, la condición `contador < 5` se vuelve falsa, el bucle finaliza y se ejecuta el último `console.log`.