---
aliases:
  - HEAD en Git
tags:
  - git
  - ramas
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-git|🌳 Índice Git]]"
Fecha: 2025-06-17
---
# `HEAD` en Git
En Git, **`HEAD`** 🤯 es una referencia simbólica crucial que apunta al **commit actual** en el que te encuentras 📍. Esencialmente, es la forma que tiene Git de saber dónde estás "parado" en tu historial de versiones 📜.
## 📌 ¿Qué significa `HEAD`? 🤔
`HEAD` no es un commit en sí mismo, sino un **puntero** 👆.
- Normalmente, **`HEAD` apunta a la punta de la rama local** 🌿 que tienes actualmente seleccionada (por ejemplo, `main`, `develop`, o cualquier rama de característica). Esto significa que cuando haces un nuevo commit, ese commit se añade a la rama a la que `HEAD` está apuntando.
- Sin embargo, **`HEAD` también puede apuntar directamente a un commit específico** 📦, en lugar de a una rama. Esto se conoce como "estado de `HEAD` separado" o "detached HEAD" 🔗. Sucede cuando haces `git checkout` a un commit o a un tag directamente, en lugar de a una rama.
## 💡 ¿Cómo lo ves? 👀
Puedes ver a dónde apunta `HEAD` usando el siguiente comando:

```bash
git show HEAD
```
O para una referencia más detallada:
```bash
git reflog
```
## 🚀 Ejemplos Comunes de Uso 📋
### 1. **`HEAD` apuntando a una rama (lo más común)** 🔗
Cuando estás en una rama normal:
```bash
git branch
* main
  develop
```
En este caso, `HEAD` está apuntando a la rama `main`. Si haces un `git commit`, el nuevo commit se añadirá a `main`.
### 2. **`HEAD` apuntando a un commit específico (detached HEAD)** 🕵️‍♀️
Si haces `git checkout <hash_del_commit>`:
```bash
git checkout a1b2c3d
```
Ahora `HEAD` apunta directamente al commit `a1b2c3d` y no a una rama. Si haces commits en este estado, esos commits **no pertenecerán a ninguna rama existente** y podrían perderse si no creas una rama a partir de ellos antes de cambiar de nuevo a otra rama ⚠️.
> [!WARNING] ¡Cuidado con detached HEAD! ⚠️
> 
> Si te encuentras en un estado de detached HEAD y haces nuevos commits, estos commits no estarán asociados a ninguna rama. Si luego cambias a otra rama (por ejemplo, git checkout main) sin haber creado una nueva rama para tus commits, podrías perder esos commits nuevos una vez que HEAD se mueva de nuevo. Para guardar el trabajo, crea una nueva rama: git branch nueva-rama-desde-commit y luego git checkout nueva-rama-desde-commit.

### 3. **Referenciando commits relativos a `HEAD`** ↔️
Puedes usar `HEAD` para referenciar commits anteriores o específicos en relación con tu posición actual:
- **`HEAD~`** o **`HEAD~1`**: El commit padre inmediato de `HEAD` ⏪.
- **`HEAD~<n>`**: El enésimo commit padre de `HEAD` (por ejemplo, `HEAD~3` es el abuelo).
- **`HEAD^`**: El primer padre de un commit de fusión (si un commit tiene múltiples padres debido a una fusión).
Ejemplo:
```bash
git log HEAD~2..HEAD
```
Este comando mostrará los dos últimos commits desde `HEAD` hasta el commit actual.