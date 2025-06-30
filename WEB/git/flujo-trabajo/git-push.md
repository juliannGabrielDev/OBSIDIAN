---
aliases:
  - git push ✨
tags:
  - git
  - git-flow
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-git|🌳 Índice Git]]"
Fecha: 2025-06-17
---
# # ✨ Comando `git push` ✨

El comando `git push` 🚀 se utiliza para subir el contenido de tu repositorio local 🏡 a un repositorio remoto ☁️. Es una operación fundamental para compartir tu trabajo 🤝 y colaborar con otros 🧑‍💻.

---

## 📌 ¿Cómo funciona? 🤔

Cuando ejecutas `git push`, Git toma los commits 📦 de tu rama local actual 📍 y los envía a la rama correspondiente en el repositorio remoto 🌐.

---

## 📝 Uso Básico 🛠️

La forma más común de usar `git push` es la siguiente:

```bash
git push <nombre_remoto> <nombre_rama>
```

- `<nombre_remoto>`: Generalmente es `origin` 🔗, que es el alias por defecto para el repositorio remoto del que clonaste.
- `<nombre_rama>`: Es la rama que deseas subir (por ejemplo, `main` o `develop`) 🌿.

---

## 💡 Ejemplos Comunes 📋

### 🚀 Subir la rama actual a `origin`

```bash
git push origin HEAD
```

> [!TIP] Atajo útil 💡
> 
> HEAD se refiere a la rama actual en la que te encuentras. Es un atajo muy práctico.

### 🌟 Subir una rama específica

Si quieres subir la rama `feature/nueva-funcionalidad` a `origin`:

```bash
git push origin feature/nueva-funcionalidad
```

### ⬆️ Establecer un "upstream" (rama de seguimiento)

La primera vez que subes una rama, es recomendable establecer un seguimiento 🕵️ para que las futuras operaciones `git push` y `git pull` sean más sencillas:

Bash

```bash
git push -u origin <nombre_rama>
```

> [!INFO] ¿Qué es -u o --set-upstream? ℹ️
> 
> Este flag 🚩 crea una conexión entre tu rama local y la rama remota. Así, en el futuro, podrás simplemente usar git push (sin especificar el remoto y la rama) y Git sabrá a dónde enviar tus cambios.

### 🗑️ Eliminar una rama remota

Para eliminar una rama del repositorio remoto ❌:

```bash
git push origin --delete <nombre_rama>
```

---

## ⚠️ Consideraciones Importantes 🛑

- **Conflictos**: Si intentas subir cambios que entran en conflicto con los del repositorio remoto, Git te pedirá que primero hagas un `git pull` para fusionar 🔄 los cambios.
- **Permisos**: Asegúrate de tener los permisos necesarios para escribir en el repositorio remoto 🔑.
- **Forzar `push`**: El uso de `git push --force` o `git push -f` 🚨 debe hacerse con **extrema precaución**. Sobrescribe el historial remoto, lo que puede causar problemas a otros colaboradores 👥.