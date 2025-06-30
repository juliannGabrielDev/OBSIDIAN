---
aliases:
  - git add ➕📂
tags:
  - git
  - git-flow
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-git|🌳 Índice Git]]"
Fecha: 2025-06-17
---
# Comando `git add` ➕📂
El comando `git add` es un paso fundamental en el flujo de trabajo de Git para registrar cambios en el historial del proyecto. Su función principal es **agregar cambios del directorio de trabajo al área de preparación** (conocida como _staging area_ o _index_). 🌳➡️📦
Piénsalo como una caja donde colocas los cambios específicos que quieres incluir en tu próximo "punto de guardado" (commit). Esto te permite tener un control preciso sobre qué modificaciones formarán parte del historial.
Existen varias formas de utilizar `git add`, dependiendo de qué cambios quieras preparar.

| **Comando**                      | **Descripción**                                                                                                                   |
| -------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| `git add <archivo>`              | 📄 Agrega un archivo específico al área de preparación.                                                                           |
| `git add <directorio>/`          | 📁 Agrega todos los archivos dentro de un directorio.                                                                             |
| `git add .`                      | ✨ Agrega todos los archivos nuevos y modificados en el directorio actual y subdirectorios.                                        |
| `git add -A` o `git add --all`   | 🌐 Agrega **todos** los cambios en el repositorio (nuevos, modificados y eliminados).                                             |
| `git add -u`                     | 🔄 Agrega las modificaciones y eliminaciones de archivos que ya están siendo rastreados por Git, pero ignora los archivos nuevos. |
| `git add -p` o `git add --patch` | interactive Agrega porciones de cambios de manera interactiva.                                                                    |
> [!TIP] ¿Cuál es la diferencia entre . y -A?
> 
> 💡 Pro Tip: Usa git add . para preparar los cambios solo en tu directorio actual y sus subdirectorios. Usa git add -A para preparar todos los cambios en cualquier parte de tu repositorio local, sin importar en qué carpeta te encuentres.

---
## 🎯 Modo Interactivo (`--patch`)
El modo interactivo es una de las herramientas más potentes de `git add`. Te permite revisar cada cambio (o "hunk") dentro de un archivo y decidir si quieres agregarlo o no.
Para iniciar el modo interactivo, usa:
```bash
git add -p
```
Git te mostrará cada bloque de cambios y te dará varias opciones:
- **`y`**: (yes) Sí, agregar este bloque.
- **`n`** : (no) No, no agregar este bloque.
- **`q`**: (quit) Salir, no agregar nada más.
- **`a`**: (all) Agregar este bloque y todos los siguientes en el archivo.
- **`d`**: (don't) No agregar este bloque ni los siguientes en el archivo.
- **`s`**: (split) Dividir el bloque actual en bloques más pequeños si es posible.
- **`e`**: (edit) Editar manualmente el bloque.
- **`?`**: Muestra la ayuda.

> [!NOTE] Preparando el terreno para commits atómicos ⚛️
> 
> 🗒️ El modo interactivo es ideal para crear commits atómicos, es decir, confirmaciones que agrupan cambios pequeños y lógicamente relacionados. Esto hace que tu historial sea mucho más limpio y fácil de entender.

---
## ⚠️ Advertencias y Consejos

> [!WARNING] ¡Cuidado con git add .!
> 
> ⚠️ Usar git add . o git add -A sin antes verificar los cambios con git status puede llevar a que agregues archivos no deseados al área de preparación, como archivos de configuración local, dependencias o compilaciones.

> [!SUCCESS] Buenas Prácticas ✅
> 
> ✅ Verifica siempre: Antes de agregar, ejecuta git status para ver un resumen de tus cambios.
> ✅ Sé específico: Prefiere git add `<archivo>` o git add -p para tener control total.
> ✅ Usa .gitignore: Configura un archivo .gitignore para que Git ignore automáticamente los archivos y carpetas que nunca deben ser parte del repositorio.
