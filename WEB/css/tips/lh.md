---
aliases:
  - Unidades lh para Mejorar Párrafos ✍️
tags:
  - css
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-css|💄 Índice CSS]]"
Fecha: 2025-09-07
---
# Unidades `lh` para Mejorar Párrafos ✍️

La unidad `lh` (line-height) es una unidad de longitud relativa en **CSS** que se basa en la altura de línea calculada del elemento. Es similar a la unidad `em` pero, en lugar de estar relacionada con el tamaño de la fuente (`font-size`), se basa en el **espacio vertical** que ocupa cada línea de texto, incluyendo el espacio entre líneas. 📏

## ¿Por Qué Usar `lh`?

Usar `lh` puede ser muy útil para crear diseños más consistentes y legibles. 🤓 Al basar las medidas en la altura de la línea, se asegura una mejor **proporción** entre los elementos de texto y los espacios que los rodean. Esto es especialmente beneficioso para:

- **Espaciado vertical:** Asegura que el espacio entre párrafos o elementos adyacentes sea proporcional al tamaño de la línea.
    
- **Tamaños de `margin` y `padding`:** Permite que los márgenes y rellenos sean consistentes con el flujo de texto.
    

---

## Ejemplos de Sintaxis ✨

Aquí te mostramos cómo puedes usar `lh` en tus hojas de estilo CSS para mejorar el espaciado de tus párrafos.

CSS

```
/* Ejemplo básico para un párrafo */
p {
  font-size: 1.25rem;
  line-height: 1.6; /* 1.6 veces el tamaño de la fuente */
  margin-bottom: 1.5lh; /* 1.5 veces la altura de la línea */
}
```

> [!TIP] Usa la unidad `lh` para espaciar elementos relacionados con el texto, como títulos y subtítulos, para mantener un ritmo vertical uniforme.

---

## Tabla de Comparación

|Unidad|Descripción|Uso Recomendado|
|---|---|---|
|**`em`**|Relativa al `font-size` del elemento.|Para medidas internas del texto.|
|**`rem`**|Relativa al `font-size` del elemento raíz (`html`).|Para tamaños de fuente globales.|
|**`lh`**|Relativa a la altura de línea (`line-height`).|Para espaciado vertical entre bloques de texto.|
|**`px`**|Unidad absoluta.|Elementos con medidas fijas, no relacionadas con el texto.|

---

> [!NOTE] La unidad `lh` es compatible con la mayoría de los navegadores modernos, pero es crucial verificar la compatibilidad en **CanIUse.com** para proyectos críticos.

**Resumen:**

- `lh` se basa en la altura de línea de un elemento.
    
- Es útil para espaciado vertical de párrafos y elementos de texto.
    
- Permite crear diseños más legibles y consistentes.
    
- Sintaxis: `margin-bottom: 1.5lh;`.