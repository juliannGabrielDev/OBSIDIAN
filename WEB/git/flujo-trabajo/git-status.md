---
aliases:
  - 🔍 git status
tags:
  - git
  - git-flow
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-git|🌳 Índice Git]]"
Fecha: 2025-06-17
---
# 🔍 `git status`: Revisa el Estado de Tu Repositorio 📝
El comando `git status` es tu ventana principal para conocer el estado actual de tu directorio de trabajo y del área de _staging_ (preparación). Te permite ver qué cambios han sido rastreados, cuáles no, y qué está listo para ser incluido en el próximo _commit_. Es, sin duda, uno de los comandos que más usarás en tu día a día con Git. 🧐
Este comando no realiza ningún cambio en tu proyecto; es una herramienta puramente informativa y segura de usar en cualquier momento.

---
## 📊 ¿Qué Te Muestra `git status`?
La salida de `git status` te informa sobre tres estados principales en los que pueden encontrarse tus archivos:
1. **Cambios preparados para el commit (_Changes to be committed_)**: Archivos que han sido agregados al área de _staging_ con `git add`. Estos cambios están listos para ser guardados en el próximo _commit_.
2. **Cambios no preparados para el commit (_Changes not staged for commit_)**: Archivos que están siendo rastreados por Git, pero que han sido modificados desde el último _commit_ y no se han agregado al área de _staging_.
3. **Archivos no rastreados (_Untracked files_)**: Archivos nuevos en tu directorio de trabajo que Git no conoce y, por lo tanto, no sigue sus cambios.
> [!INFO]
> 
> ℹ️ Además, git status te indicará en qué rama (branch) te encuentras actualmente y si esta está sincronizada con su contraparte remota.
### 📋 Ejemplo de Salida Común
```shell
# On branch main
# Your branch is up to date with 'origin/main'.
#
# Changes to be committed:
#   (use "git restore --staged <file>..." to unstage)
#       modified:   README.md
#
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git restore <file>..." to discard changes in working directory)
#       modified:   CONTRIBUTING.md
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#       docs/INSTALL.md
#
```
---
## ✨ Opciones de Visualización
Aunque `git status` es muy descriptivo, a veces necesitas una vista más compacta.

| Opción                 | Alias | Descripción                                                                                                                |
| ---------------------- | ----- | -------------------------------------------------------------------------------------------------------------------------- |
| `--short`              | `-s`  | Ofrece una salida mucho más concisa y fácil de leer. Cada archivo modificado aparece en una línea con un código de estado. |
| `--branch`             | `-b`  | Muestra la rama actual y su estado de seguimiento de forma más detallada, incluso en el formato corto.                     |
| `--untracked-files=no` |       | Oculta la lista de archivos no rastreados, útil en proyectos con muchos archivos generados automáticamente.                |

### 💨 Ejemplo con `git status -s`
```shell
 M README.md
 M CONTRIBUTING.md
?? docs/INSTALL.md
```
En este ejemplo:
- `README.md` tiene cambios preparados (`M` en verde en la primera columna).
- `CONTRIBUTING.md` tiene cambios modificados pero no preparados (`M` en rojo en la segunda columna).
- `docs/INSTALL.md` es un archivo no rastreado (`??` en rojo).
> [!WARNING]
> 
> ⚠️ git status te proporciona comandos útiles como sugerencia. Por ejemplo, te dice que uses git add <file>... para rastrear un archivo o git restore --staged <file>... para sacar un archivo del área de staging. ¡Presta atención a estos mensajes!
