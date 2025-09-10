# 📝 Mnemónicos y Configuración de ATmega328p

Aquí tienes una guía de referencia rápida sobre los mnemónicos más comunes y la configuración inicial para microcontroladores ATmega328p.

## Mnemónicos Más Utilizados

Estos son comandos esenciales para la programación en ensamblador de microcontroladores AVR.

| Mnemónico | Descripción                                                      |
| --------- | ---------------------------------------------------------------- |
| **LDI**   | Carga un dato inmediato (hexadecimal) a un registro específico.  |
| **IN**    | Transfiere datos desde los pines de un puerto hacia un registro. |
| **OUT**   | Envía los datos almacenados en un registro hacia un puerto.      |
| **MOV**   | Mueve o copia el contenido de un registro a otro.                |
| **ROL**   | Rota los bits de un registro una posición a la izquierda. ⬅️     |
| **ROR**   | Rota los bits de un registro una posición a la derecha. ➡️       |
| **INC**   | Incrementa en 1 el valor contenido en un registro.               |
| **DEC**   | Decrementa en 1 el valor contenido en un registro.               |
| **CLR**   | Limpia o pone en ceros (0) todos los bits de un registro.        |
| **RJMP**  | Realiza un salto relativo a una etiqueta dentro del código.      |
| **RCALL** | Llama a una subrutina ubicada en una etiqueta.                   |
| **RET**   | Regresa al punto desde donde se llamó a una subrutina (RCALL).   |

### Mnemónicos de Comparación y Salto

- **CPSE**: Compara dos registros. Si son iguales, salta la siguiente instrucción. Si son diferentes, la ejecuta.
    
- **BRNE**: Salta a una etiqueta si el resultado de una operación anterior fue diferente de cero. 
    

---

## 🚀 Código de Inicialización (ATmega328p)

Estas son las primeras 7 líneas que se suelen usar para configurar el microcontrolador ATmega328p.

Fragmento de código

```
.INCLUDE "M328pDEF.INC"
.CSEG
.ORG 0
LDI R16, LOW (RAMEND)
OUT SPL, R16
LDI R16, HIGH (RAMEND)
OUT SPH, R16
```


---

## ⚙️ Configuración de Puertos

> [!INFO] Registros de Dirección de Datos (DDRx)
> 
> ℹ Para establecer si un puerto funcionará como entrada o salida, se utiliza el registro DDRx correspondiente (por ejemplo, DDRB para el Puerto B).

### Configurar un Puerto como Salida

Para que un puerto envíe señales (ej. encender un LED), se configura como salida.

> [!TIP] Puerto como Salida 📤
> 
> Se carga el valor hexadecimal $FF (todos los bits en 1) en el registro DDRx del puerto deseado.

**Ejemplo para el Puerto B:**

Fragmento de código

```
LDI R16, $FF      ; Carga el valor 255 (11111111 en binario) en el registro 16
OUT DDRB, R16     ; Configura todos los pines del Puerto B como salida
```


### Configurar un Puerto como Entrada

Para que un puerto reciba señales (ej. leer un botón), se configura como entrada.

> [!TIP] Puerto como Entrada 📥
> 
> Se carga el valor hexadecimal $00 (todos los bits en 0) en el registro DDRx del puerto deseado.

**Ejemplo para el Puerto B:**

Fragmento de código

```
LDI R16, $00      ; Carga el valor 0 en el registro 16
OUT DDRB, R16     ; Configura todos los pines del Puerto B como entrada
```


> [!NOTE] Nomenclatura de Puertos
> 
> La letra
> 
> `B` en los ejemplos (`DDRB`) puede ser sustituida por la letra del puerto que se desee configurar, como `A` o `D`.