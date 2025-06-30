---
aliases:
  - Hoja de Trucos para Ramas en Git ✨🌿
tags:
  - git
  - ramas
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-git|🌳 Índice Git]]"
Fecha: 2025-06-17
---
# Hoja de Trucos para Ramas en Git ✨🌿
Las ramas son una característica central de Git 🌳 que te permiten trabajar en diferentes funcionalidades o correcciones de errores de forma aislada 🏝️. Aquí tienes una tabla de los comandos más importantes relacionados con la gestión de ramas 👇.

## 📋 Tabla de Comandos de Ramas en Git 📊

| **Comando Git 🚀**                         | **Descripción 📝**                                                               | **Ejemplo de Uso 💡**                                   |
| ------------------------------------------ | -------------------------------------------------------------------------------- | ------------------------------------------------------- |
| `git branch`                               | **Lista** todas las ramas locales 📂.                                            | `git branch`                                            |
| `git branch -a`                            | **Lista** todas las ramas, tanto locales como remotas 🌐.                        | `git branch -a`                                         |
| `git branch <nueva_rama> <rama_base>`      | **Crea** una nueva rama local basada en otra rama existente ➕.                   | `git branch feature/nueva-funcionalidad develop`        |
| `git checkout <nombre_rama>`               | Cambia al contexto de una rama existente 🔄 (también usado para archivos).       | `git checkout develop`                                  |
| `git switch <nombre_rama>`                 | **Cambia** a una rama existente (alternativa moderna a `checkout`) ⏩.            | `git switch main`                                       |
| `git checkout -b <nueva_rama> <rama_base>` | **Crea** una nueva rama basada en otra y cambia a ella en un solo paso 🚀.       | `git checkout -b bugfix/login-error main`               |
| `git branch -m <old_name> <new_name>`      | **Renombra** una rama local ✏️. Si omites `<old_name>`, renombra la rama actual. | `git branch -m feature/old-name feature/new-name`       |
| `git merge <rama_a_combinar>`              | **Combina** el historial de la rama especificada en la rama actual 🤝.           | `git merge feature/nueva-funcionalidad`                 |
| `git branch -d <nombre_rama>`              | **Elimina** una rama local. Solo se permite si la rama ha sido fusionada ✅.      | `git branch -d feature/rama-fusionada`                  |
| `git branch -D <nombre_rama>`              | **Elimina** forzadamente una rama local, incluso si no ha sido fusionada ⚠️.     | `git branch -D feature/rama-no-terminada`               |
| `git push origin --delete <nombre_rama>`   | **Elimina** una rama remota 🗑️.                                                 | `git push origin --delete feature/rama-remota-obsoleta` |

## 📌 Consideraciones Adicionales 💡
- **Flujo de Trabajo:** Es común tener una rama `main` o `master` para el código de producción y ramas de desarrollo (`develop`) o de características (`feature/`) para nuevas funcionalidades.
- **Antes de Eliminar:** Asegúrate siempre de que no necesitas los cambios en una rama antes de eliminarla, especialmente con `-D` 🚨.
- **Sincronización Remota:** Recuerda que los comandos de ramas afectan localmente a menos que interactúes con un repositorio remoto usando `git push` o `git fetch` 🔄.
> [!TIP] Ramas remotas 🌐
> 
> Para ver las ramas remotas, puedes usar `git branch -r` o `git branch -a`. Las ramas remotas se ven como `origin/main` o `origin/develop`.
