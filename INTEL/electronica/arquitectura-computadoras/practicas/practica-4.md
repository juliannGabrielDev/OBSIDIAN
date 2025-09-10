# Práctica 4: Simulación de semáforo de cruce.
Su objetivo es controlar un sistema de dos semáforos de cuatro luces cada uno, conectados al

`PORTD` de un microcontrolador ATmega328P2222. El código sigue una secuencia específica y predefinida de 16 pasos con duraciones variables.

---
```
; =========================================================================
; Nombre del Proyecto: Práctica 4 - Simulación de Semáforo (Pizarrón)
; Microcontrolador: ATmega328P
; Descripción: Sigue la secuencia de 16 pasos y los valores hexadecimales
;              especificados en la foto del pizarrón de clase.
;              Controla 8 LEDs en PORTD.
; =========================================================================

.include "m328pdef.inc"

.cseg
.org 0
    rjmp    inicio

; --- Vector de Reset ---
inicio:
    ; Configurar Stack Pointer (buena práctica)
    ldi     r16, low(RAMEND)
    out     SPL, r16
    ldi     r16, high(RAMEND)
    out     SPH, r16

    ; Configurar PORTD como salida
    ldi     r16, 0xFF  ; Cargar 11111111 en r16
    out     DDRD, r16  ; Poner todos los pines de PORTD como salida

; =========================================================================
; Bucle Principal del Semáforo
; =========================================================================
bucle_semaforo:
    ; La secuencia está tomada directamente de la tabla del pizarrón

    ; --- Paso 1: $81 (2 seg) ---
    ldi     r16, $81
    out     PORTD, r16
    rcall   Delay_2s

    ; --- Paso 2: $80 (0.5 seg) ---
    ldi     r16, $80
    out     PORTD, r16
    rcall   Delay_0_5s

    ; --- Paso 3: $81 (0.5 seg) ---
    ldi     r16, $81
    out     PORTD, r16
    rcall   Delay_0_5s

    ; --- Paso 4: $80 (0.5 seg) ---
    ldi     r16, $80
    out     PORTD, r16
    rcall   Delay_0_5s

    ; --- Paso 5: $81 (0.5 seg) ---
    ldi     r16, $81
    out     PORTD, r16
    rcall   Delay_0_5s
    
    ; --- Paso 6: $80 (0.5 seg) ---
    ldi     r16, $80
    out     PORTD, r16
    rcall   Delay_0_5s

    ; --- Paso 7: $82 (4 seg) ---
    ldi     r16, $82
    out     PORTD, r16
    rcall   Delay_4s

    ; --- Paso 8: $84 (1 seg) ---
    ldi     r16, $84
    out     PORTD, r16
    rcall   Delay_1s

    ; --- Paso 9: $18 (2 seg) ---
    ldi     r16, $18
    out     PORTD, r16
    rcall   Delay_2s

    ; --- Paso 10: $08 (0.5 seg) ---
    ldi     r16, $08
    out     PORTD, r16
    rcall   Delay_0_5s

    ; --- Paso 11: $18 (0.5 seg) ---
    ldi     r16, $18
    out     PORTD, r16
    rcall   Delay_0_5s

    ; --- Paso 12: $08 (0.5 seg) ---
    ldi     r16, $08
    out     PORTD, r16
    rcall   Delay_0_5s
    
    ; --- Paso 13: $18 (0.5 seg) ---
    ldi     r16, $18
    out     PORTD, r16
    rcall   Delay_0_5s

    ; --- Paso 14: $08 (0.5 seg) ---
    ldi     r16, $08
    out     PORTD, r16
    rcall   Delay_0_5s

    ; --- Paso 15: $28 (4 seg) ---
    ldi     r16, $28
    out     PORTD, r16
    rcall   Delay_4s

    ; --- Paso 16: $48 (1 seg) ---
    ldi     r16, $48
    out     PORTD, r16
    rcall   Delay_1s

    rjmp    bucle_semaforo  ; Repetir el ciclo completo

; =========================================================================
; Subrutinas de Retardo (Delays)
; Se han creado 4 delays para coincidir con los tiempos del pizarrón
; =========================================================================
Delay_4s:
    ldi  r17, 46
D4_L1:
    ldi  r18, 153
D4_L2:
    ldi  r19, 250
D4_L3:
    dec  r19
    brne D4_L3
    dec  r18
    brne D4_L2
    dec  r17
    brne D4_L1
    ret

Delay_2s:
    ldi  r17, 23
D2_L1:
    ldi  r18, 153
D2_L2:
    ldi  r19, 250
D2_L3:
    dec  r19
    brne D2_L3
    dec  r18
    brne D2_L2
    dec  r17
    brne D2_L1
    ret

Delay_1s:
    ldi  r17, 12
D1_L1:
    ldi  r18, 153
D1_L2:
    ldi  r19, 250
D1_L3:
    dec  r19
    brne D1_L3
    dec  r18
    brne D1_L2
    dec  r17
    brne D1_L1
    ret

Delay_0_5s: ; Aprox. medio segundo
    ldi  r17, 6
D05_L1:
    ldi  r18, 153
D05_L2:
    ldi  r19, 250
D05_L3:
    dec  r19
    brne D05_L3
    dec  r18
    brne D05_L2
    dec  r17
    brne D05_L1
    ret
```
## 🛠️ Configuración y Mapeo de Pines

Primero, el programa realiza la configuración estándar del `Stack Pointer` y establece todos los pines del `PORTD` como salidas para poder controlar los 8 LEDs de los dos semáforos.

> [!INFO] Mapeo de Semáforos
> 
> ℹ️ Según la descripción, los 8 pines del PORTD se asignan a dos semáforos (S1 y S2) de la siguiente manera:
> 
> |Pin|Luz|Pin|Luz|
> |---|---|---|---|
> |**PD7**|Flecha S1|**PD3**|Flecha S2|
> |**PD6**|Verde S1|**PD2**|Verde S2|
> |**PD5**|Amarillo S1|**PD1**|Amarillo S2|
> |**PD4**|Rojo S1|**PD0**|Rojo S2|

---

## 🚦 La Secuencia de 16 Pasos

El bucle principal (`bucle_semaforo`) ejecuta una secuencia fija donde en cada paso se envía un valor hexadecimal al `PORTD` y se espera un tiempo determinado. La siguiente tabla resume el comportamiento completo del ciclo:

|Paso|Valor (Hex)|Valor (Binario)|Estado Semáforo 1 (F-V-A-R)|Estado Semáforo 2 (F-V-A-R)|Duración|
|---|---|---|---|---|---|
|1|`$81`|`1000 0001`|**Flecha**|**Rojo**|2.0 s|
|2|`$80`|`1000 0000`|**Flecha**|(Apagado)|0.5 s|
|3|`$81`|`1000 0001`|**Flecha**|**Rojo**|0.5 s|
|4|`$80`|`1000 0000`|**Flecha**|(Apagado)|0.5 s|
|5|`$81`|`1000 0001`|**Flecha**|**Rojo**|0.5 s|
|6|`$80`|`1000 0000`|**Flecha**|(Apagado)|0.5 s|
|7|`$82`|`1000 0010`|**Flecha**|**Amarillo**|4.0 s|
|8|`$84`|`1000 0100`|**Flecha**|**Verde**|1.0 s|
|9|`$18`|`0001 1000`|**Rojo**|**Flecha**|2.0 s|
|10|`$08`|`0000 1000`|(Apagado)|**Flecha**|0.5 s|
|11|`$18`|`0001 1000`|**Rojo**|**Flecha**|0.5 s|
|12|`$08`|`0000 1000`|(Apagado)|**Flecha**|0.5 s|
|13|`$18`|`0001 1000`|**Rojo**|**Flecha**|0.5 s|
|14|`$08`|`0000 1000`|(Apagado)|**Flecha**|0.5 s|
|15|`$28`|`0010 1000`|**Amarillo**|**Flecha**|4.0 s|
|16|`$48`|`0100 1000`|**Verde**|**Flecha**|1.0 s|

> [!NOTE] Secuencia Específica
> 
> Esta secuencia de 16 pasos, que incluye parpadeos y transiciones, corresponde a un ejercicio de clase específico y puede no seguir la lógica de un semáforo de cruce convencional.

---

## ⏱️ Subrutinas de Retardo Múltiple

Una característica clave de este código es el uso de **cuatro subrutinas de retardo distintas** para lograr las diferentes duraciones requeridas en la secuencia.

- `Delay_4s`: Crea una pausa de aproximadamente 4 segundos.
    
- `Delay_2s`: Crea una pausa de aproximadamente 2 segundos.
    
- `Delay_1s`: Crea una pausa de aproximadamente 1 segundo.
    
- `Delay_0_5s`: Crea una pausa de aproximadamente medio segundo.
    

Todas las subrutinas usan la misma estructura de bucles anidados de 3 niveles para consumir tiempo. La única diferencia entre ellas es el valor inicial cargado en el registro contador del bucle exterior (`r17`), lo que permite ajustar la duración total de la pausa.