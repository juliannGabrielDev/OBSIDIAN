# 📝 Práctica 1: Auto Increíble con ATmega328p

Este programa en lenguaje ensamblador está diseñado para el microcontrolador

**ATmega328P** y tiene como objetivo controlar 8 LEDs para simular el famoso efecto visual del "Auto Increíble". La secuencia consiste en encender los LEDs uno por uno de izquierda a derecha y luego de regreso.

---

## ⚙️ Análisis del Programa

```
.include "M328pdef.inc"

.CSEG
.ORG 0

	LDI R16, LOW(ramend)
	OUT SPL, R16
	LDI R16, HIGH(ramend)
	OUT SPH, R16

	LDI R16, $FF
	OUT DDRD, R16

inicio:
	LDI R16, $01
	OUT PORTD, R16
	RCALL delay

	LDI R16, $02
	OUT PORTD, R16
	RCALL delay

	LDI R16, $04
	OUT PORTD, R16
	RCALL delay

	LDI R16, $08
	OUT PORTD, R16
	RCALL delay

	LDI R16, $10
	OUT PORTD, R16
	RCALL delay

	LDI R16, $20
	OUT PORTD, R16
	RCALL delay

	LDI R16, $40
	OUT PORTD, R16
	RCALL delay

	LDI R16, $80
	OUT PORTD, R16
	RCALL delay

	LDI R16, $40
	OUT PORTD, R16
	RCALL delay

	LDI R16, $20
	OUT PORTD, R16
	RCALL delay

	LDI R16, $10
	OUT PORTD, R16
	RCALL delay

	LDI R16, $08
	OUT PORTD, R16
	RCALL delay

	LDI R16, $04
	OUT PORTD, R16
	RCALL delay

	LDI R16, $02
	OUT PORTD, R16
	RCALL delay

	RJMP inicio

delay:

	; ============================= 
;    delay loop generator 
;     60000 cycles:
; ----------------------------- 
; delaying 59994 cycles:
          ldi  R17, $63
WGLOOP0:  ldi  R18, $C9
WGLOOP1:  dec  R18
          brne WGLOOP1
          dec  R17
          brne WGLOOP0
; ----------------------------- 
; delaying 6 cycles:
          ldi  R17, $02
WGLOOP2:  dec  R17
          brne WGLOOP2
; ============================= 
RET
```

El código se puede dividir en tres partes fundamentales: la configuración inicial, el bucle principal que genera el efecto y la subrutina de retardo.

### 1. Configuración Inicial

Esta sección prepara el microcontrolador para que funcione correctamente.

- **Inicialización del Stack Pointer:** Las primeras líneas configuran el puntero de pila (`Stack Pointer`) en la última dirección de la memoria RAM. Esto es crucial para que las llamadas a subrutinas (`RCALL`) y las interrupciones funcionen correctamente.
    
    Fragmento de código
    
    ```
    LDI R16, LOW(ramend)
    OUT SPL, R16
    LDI R16, HIGH(ramend)
    OUT SPH, R16
    ```
    
- **Configuración del Puerto D como Salida:** Para poder encender y apagar los LEDs, los pines a los que están conectados deben ser configurados como salidas. En esta práctica, los 8 LEDs se conectan al
    
    **Puerto D** (pines PD0 a PD7).
    
    Fragmento de código
    
    ```
    LDI R16, $FF  ; Carga el valor 0b11111111
    OUT DDRD, R16 ; Escribe en el Registro de Dirección de Datos D
    ```
    
    > [!TIP] Registro DDRD
    > 
    > El registro
    > 
    > `DDRD` define la dirección de cada pin del Puerto D55. Al escribirle
    > 
    > `$FF` (todos los bits en 1), se configuran los 8 pines como **salidas digitales**.
    

### 2. Bucle Principal (`inicio`)

Esta es la lógica central que crea la animación de las luces.

- **Secuencia de Ida (Izquierda a Derecha):** El programa carga valores hexadecimales en el registro `R16` y los envía al `PORTD`7. Cada valor representa un único LED encendido.
    
    - `$01` -> `00000001` (enciende el primer LED)
        
    - `$02` -> `00000010` (enciende el segundo LED)
        
    - ... y así sucesivamente hasta `$80` (`10000000`), que enciende el último LED.
        
- **Secuencia de Vuelta (Derecha a Izquierda):** Después de llegar al último LED, el proceso se invierte, cargando los valores `$40`, `$20`, etc., hasta volver al segundo LED (`$02`).
    
- **Llamada a Retardo:** Después de encender cada LED, se ejecuta `RCALL delay`. Esto llama a una subrutina que crea una pausa, permitiendo que el ojo humano perciba el LED encendido antes de que cambie al siguiente.
    
- **Salto Infinito:** La última instrucción, `RJMP inicio`, hace que el programa salte de nuevo a la etiqueta `inicio`, repitiendo la secuencia de luces indefinidamente.
    

### 3. Subrutina de Retardo (`delay`)

Esta sección del código no hace nada más que consumir tiempo.

> [!INFO] ¿Qué es un retardo por software?
> 
> ℹ️ Es una técnica que utiliza bucles anidados para forzar al microcontrolador a ejecutar instrucciones repetitivas que no tienen un efecto práctico, con el único fin de crear una pausa visible en el programa8.

- El código dentro de la etiqueta `delay:` utiliza dos registros (`R17` y `R18`) como contadores.
    
- Las instrucciones `dec` (decrementar) y `brne` (bifurcar si no es cero) crean bucles que se ejecutan miles de veces.
    
- Una vez que los bucles terminan, la instrucción `RET` devuelve el control del programa al punto justo después de donde se llamó a la subrutina (`RCALL delay`).