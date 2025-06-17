---
aliases:
  - "Hoja de Trucos: Campos de Formulario 📋"
tags:
  - django
  - forms
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-django|🕷️ Índice Django]]"
Fecha: 2025-06-01
---
# Hoja de Trucos: Campos de Formulario en Django 📋

Cuando definimos un formulario en Django, ya sea un `forms.Form` o un `forms.ModelForm`, cada "cajita" o "espacio" donde el usuario ingresa datos es un **campo**. Django nos da un montón de tipos de campos listos para usar. ¡Aquí te va un resumen de los más comunes y sus características!
- [[#1. Argumentos Comunes a Muchos Campos ⚙️|1. Argumentos Comunes a Muchos Campos ⚙️]]
- [[#2. Tipos de Campos Principales (¡La Carnita! 🥩)|2. Tipos de Campos Principales (¡La Carnita! 🥩)]]
	- [[#2. Tipos de Campos Principales (¡La Carnita! 🥩)#Campos Relacionados con Modelos (¡Súper Útiles con `ModelForm`!)|Campos Relacionados con Modelos (¡Súper Útiles con `ModelForm`!)]]
- [[#3. Notas Rápidas Adicionales ⚡|3. Notas Rápidas Adicionales ⚡]]
- [[#4. Mini Repaso Final 📝|4. Mini Repaso Final 📝]]

## 1. Argumentos Comunes a Muchos Campos ⚙️
Antes de ver los campos específicos, recuerda que la mayoría comparten estos argumentos. ¡Apréndetelos bien!
- `required` (Boolean): Si es `True` (por defecto), el campo no puede estar vacío. Si es `False`, es opcional.
- `label` (String): El nombre amigable del campo que se muestra al usuario (ej: "Nombre de Usuario:").
- `initial` (Valor): Un valor inicial para el campo cuando se muestra por primera vez.
- `widget` (Widget class): Permite especificar cómo se renderiza el campo en HTML (ej: `forms.Textarea` para un `CharField`).
- `help_text` (String): Texto adicional que se muestra junto al campo para dar pistas al usuario.
- `error_messages` (Dictionary): Para personalizar los mensajes de error (ej: `{'required': '¡Oye, este campo es obligatorio!'}`).
- `validators` (List): Una lista de funciones validadoras personalizadas para ese campo.
- `disabled` (Boolean): Si es `True`, el campo se muestra pero no se puede editar, y su valor no se envía con el formulario.
- `localize` (Boolean): Si es `True`, el formato de los datos se adaptará a la localización actual (ej. para fechas y números).
## 2. Tipos de Campos Principales (¡La Carnita! 🥩)
Aquí tienes una tabla con los campos más usados, su propósito y algunos argumentos específicos importantes:

| **Nombre del Campo (forms.FieldType)** | **Propósito Principal**                                               | **Argumentos Específicos/Importantes (además de los comunes)**                         | **Widget por Defecto (HTML típico)**              |
| -------------------------------------- | --------------------------------------------------------------------- | -------------------------------------------------------------------------------------- | ------------------------------------------------- |
| **`CharField`**                        | Texto corto o largo.                                                  | `max_length`, `min_length`, `strip` (si quita espacios al inicio/final), `empty_value` | `TextInput` (`<input type="text">`)               |
| **`IntegerField`**                     | Números enteros.                                                      | `max_value`, `min_value`                                                               | `NumberInput` (`<input type="number">`)           |
| **`FloatField`**                       | Números con decimales (punto flotante).                               | `max_value`, `min_value`                                                               | `NumberInput` (`<input type="number">`)           |
| **`DecimalField`**                     | Números decimales con precisión fija (¡ideal para dinero!).           | `max_value`, `min_value`, `max_digits`, `decimal_places`                               | `NumberInput` (`<input type="number">`)           |
| **`BooleanField`**                     | Valor verdadero/falso.                                                | `required` (un poco diferente, si es `True` la casilla debe estar marcada)             | `CheckboxInput` (`<input type="checkbox">`)       |
| **`EmailField`**                       | Una dirección de correo electrónico válida.                           | `max_length`, `min_length`                                                             | `EmailInput` (`<input type="email">`)             |
| **`URLField`**                         | Una URL válida.                                                       | `max_length`, `min_length`                                                             | `URLInput` (`<input type="url">`)                 |
| **`DateField`**                        | Una fecha.                                                            | `input_formats` (lista de formatos aceptados)                                          | `DateInput` (`<input type="date">`)               |
| **`TimeField`**                        | Una hora.                                                             | `input_formats`                                                                        | `TimeInput` (`<input type="time">`)               |
| **`DateTimeField`**                    | Fecha y hora.                                                         | `input_formats`                                                                        | `DateTimeInput` (`<input type="datetime-local">`) |
| **`ChoiceField`**                      | Permite elegir una opción de una lista predefinida.                   | `choices` (lista de tuplas `(valor, etiqueta)`), `coerce` (para convertir el valor)    | `Select` (`<select>`)                             |
| **`TypedChoiceField`**                 | Similar a `ChoiceField` pero convierte el valor a un tipo específico. | `choices`, `coerce` (función para convertir), `empty_value`                            | `Select` (`<select>`)                             |
| **`MultipleChoiceField`**              | Permite elegir múltiples opciones de una lista.                       | `choices`                                                                              | `SelectMultiple` (`<select multiple>`)            |
| **`FileField`**                        | Para subir archivos.                                                  | `max_length`, `allow_empty_file`                                                       | `ClearableFileInput` (`<input type="file">`)      |
| **`ImageField`**                       | Para subir archivos de imagen (valida que sea imagen).                | Igual que `FileField` (requiere Pillow instalado)                                      | `ClearableFileInput` (`<input type="file">`)      |
| **`UUIDField`**                        | Un Identificador Único Universal.                                     |                                                                                        | `TextInput` (`<input type="text">`)               |
| **`SlugField`**                        | Un "slug" (texto corto para URLs, ej: "mi-articulo").                 | `allow_unicode`                                                                        | `TextInput` (`<input type="text">`)               |
| **`RegexField`**                       | Valida contra una expresión regular.                                  | `regex` (la expresión regular), `max_length`, `min_length`                             | `TextInput` (`<input type="text">`)               |
| **`JSONField`** (Django 3.1+)          | Para datos en formato JSON.                                           | `encoder`, `decoder`                                                                   | `Textarea`                                        |

### Campos Relacionados con Modelos (¡Súper Útiles con `ModelForm`!)
Estos campos son especiales porque trabajan directamente con tus modelos de la base de datos. Se importan desde `django.forms` (anteriormente `django.forms.models`).

| **Nombre del Campo**           | **Propósito Principal**                                                          | **Argumentos Específicos/Importantes**                                                                                                                 | **Widget por Defecto** |
| ------------------------------ | -------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------- |
| **`ModelChoiceField`**         | Permite elegir una instancia de un modelo (ej: elegir un Autor de la BD).        | `queryset` (obligatorio, define qué objetos se pueden elegir), `empty_label` (ej: "---------"), `to_field_name` (qué campo del modelo usar como valor) | `Select`               |
| **`ModelMultipleChoiceField`** | Permite elegir múltiples instancias de un modelo (ej: elegir varias Categorías). | `queryset` (obligatorio)                                                                                                                               | `SelectMultiple`       |

**Ejemplo rápido de `ChoiceField` y `ModelChoiceField`:**
```python
# forms.py
from django import forms
from .models import Autor # Suponiendo que tienes un modelo Autor

class MiFormulario(forms.Form):
    # ChoiceField simple
    PRIORIDAD_CHOICES = [
        ('B', 'Baja'),
        ('M', 'Media'),
        ('A', 'Alta'),
    ]
    prioridad = forms.ChoiceField(choices=PRIORIDAD_CHOICES, label="Prioridad de la tarea")

    # ModelChoiceField
    # Asume que tienes un modelo Autor con un campo 'nombre_completo'
    autor_asignado = forms.ModelChoiceField(
        queryset=Autor.objects.filter(activo=True), # Solo autores activos
        label="Autor Asignado",
        empty_label="Selecciona un autor" # Texto para la opción vacía
    )
```
## 3. Notas Rápidas Adicionales ⚡
- **Personalización de Widgets**: Recuerda que puedes cambiar el `widget` de CUALQUIER campo para controlar cómo se ve. Por ejemplo, un `CharField` puede usar un `forms.PasswordInput` si es para una contraseña.
    ```python
    contrasena = forms.CharField(widget=forms.PasswordInput)
    ```
- **Datos Limpios**: Después de que un formulario es validado con `form.is_valid()`, los datos convertidos al tipo de Python correcto están en `form.cleaned_data`. ¡Siempre usa esto para acceder a los datos!
- **Creación de Campos Propios**: Si ninguno de estos se ajusta, ¡puedes crear tus propios campos personalizados heredando de `forms.Field`! (Esto es más avanzado).
## 4. Mini Repaso Final 📝
¡Entender los campos de formulario es clave! Son los **bloques de construcción** para recoger información de tus usuarios de manera **estructurada y validada**. Conocer los tipos de campos disponibles y sus opciones te permitirá crear formularios robustos y amigables. ¡Esta hoja de trucos es tu aliada! 😉