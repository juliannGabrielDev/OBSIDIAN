# Práctica 2: Letreros en un display en ensamblador.
Su objetivo es mostrar una secuencia de caracteres predefinida en un display de 7 segmentos de cátodo común, utilizando el microcontrolador ATmega328P2.

---

## ⚙️ Análisis del Programa

El código se estructura en tres partes principales: la configuración del microcontrolador, el bucle principal que muestra el letrero y una subrutina para generar pausas.

```
.include "m328pdef.inc"

.cseg
.org 0
    rjmp inicio

inicio:
    ldi r16, low(RAMEND)
    out SPL, r16
    ldi r16, high(RAMEND)
    out SPH, r16

    ldi r16, 0xFF
    out DDRD, r16


bucle_letrero:
    ; C
    ldi r16, $39
    out PORTD, r16
    rcall delay

    ; E
    ldi r16, $79
    out PORTD, r16
    rcall delay

    ; L
    ldi r16, $38
    out PORTD, r16
    rcall delay

	; --
	ldi r16, $00
    out PORTD, r16
    rcall delay

    ; 3
    ldi r16, $4F
    out PORTD, r16
    rcall delay

    ; 1
    ldi r16, $06
    out PORTD, r16
    rcall delay

	; 7
    ldi     r16,  $07
    out     PORTD, r16
    rcall   delay

    ; 1
    ldi     r16, $06
    out     PORTD, r16
    rcall   delay

	; 2
    ldi r16, $5B
    out PORTD, r16
    rcall delay

    ; 3
    ldi r16, $4F
    out PORTD, r16
    rcall delay

	; 1
    ldi     r16, $06
    out     PORTD, r16
    rcall   delay

    ; 0
    ldi     r16, $3F
    out     PORTD, r16
    rcall   delay

	; 1
    ldi     r16, $06
    out     PORTD, r16
    rcall   delay

    ; 4
    ldi     r16, $66
    out     PORTD, r16
    rcall   delay
    
    ldi r16, 0x00
    out PORTD, r16
    rcall delay

    rjmp    bucle_letrero

delay:
; ============================= 
;    delay loop generator 
;     700000 cycles:
; ----------------------------- 
; delaying 699996 cycles:
          ldi  R17, $16
WGLOOP0:  ldi  R18, $65
WGLOOP1:  ldi  R19, $68
WGLOOP2:  dec  R19
          brne WGLOOP2
          dec  R18
          brne WGLOOP1
          dec  R17
          brne WGLOOP0
; ----------------------------- 
; delaying 3 cycles:
          ldi  R17, $01
WGLOOP3:  dec  R17
          brne WGLOOP3
; ----------------------------- 
; delaying 1 cycle:
          nop
; =============================
ret
```

### **1. Configuración Inicial**

Esta sección prepara el hardware para la tarea.

- **Puntero de Pila (Stack Pointer)**: Se inicializa para asegurar que las llamadas a subrutinas (`rcall`) funcionen correctamente.
    
- **Configuración del Puerto D**: Se configura el `PORTD` como salida3. Esto es esencial, ya que los pines de este puerto se conectarán a los segmentos del display para encenderlos o apagarlos4.
    
    ```
    ldi r16, 0xFF  ; Carga el valor binario 11111111 en R16
    out DDRD, r16  ; Establece todos los pines del Puerto D como salidas
    ```
    

### **2. Lógica del Letrero (`bucle_letrero`)** 📜

Este es el núcleo del programa. Se encarga de enviar los patrones correctos al display para formar cada carácter.

- El programa carga secuencialmente un valor hexadecimal en el registro `r16`.
    
- Cada valor representa el patrón de segmentos que deben encenderse para formar un carácter específico.
    
- La instrucción `out PORTD, r16` envía este patrón al display, encendiendo los segmentos correspondientes.
    

> [!TIP] ¿Cómo funcionan los patrones?
> 
> Se asume que los 7 segmentos del display (a, b, c, d, e, f, g) están conectados a los pines del `PORTD` de `PD0` a `PD6`. 
> 
> Un `1` en un bit enciende el segmento correspondiente. El patrón se forma así: `(bit 7 no usado) g f e d c b a`.
> 
> **Ejemplo para la letra 'C' (`$39`)**:
> 
> - Hexadecimal: `$39`
>     
> - Binario: `0011 1001`
>     
> - Segmentos encendidos (de derecha a izquierda): `a`, `d`, `e`, `f`. Esto forma una 'C' mayúscula en el display.
>     

La secuencia que muestra el programa es: **C-E-L-[espacio]-3-1-7-1-2-3-1-0-1-4-[espacio]**. Después de mostrar el último carácter, el programa salta (`rjmp`) al inicio del bucle para repetir el letrero continuamente.

### **3. Subrutina de Retardo (`delay`)** ⏱️

Esta subrutina genera una pausa para que cada carácter permanezca visible en el display por un momento antes de cambiar al siguiente.

- Utiliza un sistema de **tres bucles anidados** (con los registros `R17`, `R18` y `R19`) para consumir una gran cantidad de ciclos de reloj del procesador.
    
- La instrucción `ret` finaliza la subrutina y devuelve el control al bucle principal del letrero.