---
aliases:
  - Propiedades de Texto ✨📝
tags:
  - css
  - texto
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-css|💄 Índice CSS]]"
Fecha: 2025-06-05
---
# Propiedades de Texto ✨📝
La representación visual del contenido de texto puede modificarse mediante cuatro propiedades principales: `text-transform`, `font-style`, `font-weight` y `text-decoration`.

|Propiedad|Valores|Descripción|
|---|---|---|
|`text-transform`|`none`, `uppercase`, `lowercase`, `capitalize`, `full-width`|Modificar las propiedades del texto|
|`font-style`|`normal`, `italic`, `oblique`|Opciones de estilo de fuente, como cursiva|
|`font-weight`|`normal`, `weight`, `lighter`, `bolder`, `100-900`|Otras opciones de estilo de fuente como el cambio de énfasis, por ejemplo poner el texto en negrita|
|`text-decoration`|`none`, `underline`, `overline`, `line-through`|Abreviatura de elementos auxiliares añadidos al texto mediante otras propiedades como `text-decoration-line`|

A continuación se indican las propiedades adicionales que ayudan a configurar los efectos de estilo.

|Alineación del texto|Para la alineación horizontal del texto|
|---|---|
|`text-align-last`|Alineación para la última línea cuando el texto se configura como justificado|
|`text-combine-upright`|Múltiples caracteres en el espacio de un solo carácter colocado en vertical como en mandarín|
|`text-decoration-color`|Configuración del color de la decoración del texto|
|`text-decoration-line`|Tipo de línea en la decoración del texto, como subrayado, sobrelineado, etc|
|`text-decoration-style`|Estilos añadidos a las líneas bajo el texto, como ondulado, punteado, etc|
|`text-decoration-thickness`|Grosor de la línea de decoración|
|`text-emphasis`|Abreviatura de otras propiedades como el color y el estilo|
|`text-indent`|La sangría de la primera línea|
|`text-justify`|Especifica el método de justificación utilizado cuando text-align es “justify”|
|`text-orientation`|Orientación del texto en una línea, como lateral, vertical, etc|
|`text-shadow`|Añade sombra al texto|
|`text-underline-position`|Declara la posición del subrayado establecido mediante la propiedad text-decoration|

Aparte de éstas, hay algunas propiedades más que ayudan a modificar la alineación y a definir el ámbito del texto con sus contenedores.

|Propiedad|Valores|Descripción|
|---|---|---|
|`text-overflow`|`clip`, `ellipsis`|Determina el comportamiento de desbordamiento del texto con el contenedor|
|`word-wrap`|`normal`, `anywhere`, `break-word`|Se aplica a los elementos en línea, alias de overflow-wrap|
|`word-break`|`normal`, `break-all`, `keep-all`, `break-word`|Se utiliza en palabras largas para decidir si las palabras deben romperse o desbordarse|
|`writing-mode`|`horizontal-tb`, `vertical-lr`, `vertical-rl`|Puede establecer la dirección del texto en vertical u horizontal|

Las propiedades mencionadas son las que se pueden utilizar para dar efectos al texto.