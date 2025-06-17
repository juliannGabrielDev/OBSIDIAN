---
aliases:
  - Eventos para Medios ⏯️📅
tags:
  - js
  - interaccion
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-javascript|⚡ Índice JavaScript]]"
Fecha: 2025-06-05
---
# Eventos para Medios en JavaScript ⏯️📅
## 1. El Héroe: `addEventListener` 🎧

Este método es la clave para "escuchar" lo que pasa con tus elementos de audio y video. Le dices qué evento escuchar y qué función ejecutar cuando ocurra. ¡Así de fácil!

- **¿Qué hace?**: <mark style="background: #ADCCFFA6;">Asigna un "oyente" a un elemento HTML.</mark>
- **Sintaxis**: `elemento.addEventListener('evento', funcionAEjecutar);`
- **Ejemplo**: `video.addEventListener('play', () => { console.log('¡El video está corriendo!'); });`

📌 **Tip**: Piensa en `addEventListener` como ponerle un timbre a tu video. Cuando alguien lo toca (evento `play`), suena la alarma (tu función).

---

## 2. Eventos Esenciales de Medios ⏯️

Tu audio o video te "habla" a través de estos eventos. ¡Aprende a entenderlo!

| **Evento**         | **¿Cuándo se dispara?**                             |
| ------------------ | --------------------------------------------------- |
| **`play`**         | Cuando el medio empieza o reanuda su reproducción.  |
| **`pause`**        | Cuando la reproducción se pausa.                    |
| **`ended`**        | Cuando el medio llega al final.                     |
| **`timeupdate`**   | Mientras se reproduce, ¡se dispara constantemente!  |
| **`volumechange`** | Si subes, bajas o silencias el volumen.             |
| **`loadeddata`**   | Cuando los primeros datos del medio se han cargado. |

🧠 **Ejemplo práctico**: Quieres mostrar el tiempo actual de un video.

```js
const video = document.querySelector('#miVideo');
const tiempoActual = document.querySelector('#tiempo');

video.addEventListener('timeupdate', () => {
  // Math.floor para no mostrar decimales feos
  tiempoActual.innerText = Math.floor(video.currentTime);
});
```

---

## 3. Métodos para Tomar el Control 🕹️

No solo escuches, ¡también da órdenes! Estos métodos te permiten controlar la reproducción.

- `play()`: <mark style="background: #BBFABBA6;">Inicia la reproducción.</mark> `video.play();`
- `pause()`: <mark style="background: #FFF3A3A6;">Pausa la reproducción.</mark> `video.pause();`
- `load()`: <mark style="background: #ABF7F7A6;">Recarga el elemento de medio.</mark> Útil si cambias la fuente (`src`). `video.load();`
- `muted`: Propiedad (no método) para silenciar. `video.muted = true;`
- `volume`: Propiedad para controlar el volumen (de 0.0 a 1.0). `video.volume = 0.5;` // ¡A mitad de volumen!

📘 **Tip**: Combina eventos y métodos. Por ejemplo, puedes crear un botón que al hacerle clic, ejecute `video.play()` o `video.pause()`. ¡Tú tienes el poder!