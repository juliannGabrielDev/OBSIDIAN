---
aliases:
  - 💾 git commit
tags:
  - git
  - git-flow
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-git|🌳 Índice Git]]"
Fecha: 2025-06-17
---

# 💾 `git commit`: Guardando Tus Cambios en el Repositorio 📂
Su propósito principal es capturar una "instantánea" de los cambios que has preparado (agregado al área de _staging_) en tu repositorio local. 📸
Cada _commit_ funciona como un punto de guardado en la historia de tu proyecto. Te permite registrar qué cambios se hicieron, quién los hizo y cuándo. Esto es crucial para llevar un historial detallado, colaborar con otros desarrolladores y poder revertir a versiones anteriores si algo sale mal.

---
## ⚙️ Sintaxis y Opciones Comunes
La forma más básica de usar el comando es simplemente `git commit`. Sin embargo, casi siempre se utiliza con algunas opciones para hacerlo más eficiente.

| **Opción** | **Descripción**                                                                                                                                                          | **Ejemplo de Uso**                                                 |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------ |
| `-m`       | Permite escribir un mensaje de _commit_ directamente en la línea de comandos. Es la forma más rápida y común de hacer un _commit_.                                       | `git commit -m "Agrega la función de inicio de sesión"`            |
| `-a`       | Incluye automáticamente en el _commit_ todos los archivos que ya están siendo rastreados por Git y que han sido modificados o eliminados. **No agrega archivos nuevos.** | `git commit -a -m "Corrige un error en el formulario de contacto"` |
| `--amend`  | Modifica el _commit_ más reciente. Es útil para corregir un mensaje de _commit_ o agregar archivos que olvidaste incluir.                                                | `git commit --amend -m "Nuevo mensaje para el último commit"`      |

> [!TIP]
> 
> 💡 Pro Tip: Puedes combinar las opciones -a y -m para agilizar tu flujo de trabajo: git commit -am "Un mensaje descriptivo". ¡Recuerda que esto no incluirá archivos nuevos que no hayan sido agregados con git add!

### Remover un archivo de un commit

```bash
git rm --cached <archivo>
```

### Restaurar archivos a su último commit

Cuando el archivo **no** está en el área de preparación:

```bash
git checkout<archivo>
```

Cuando el archivo está en el área de preparación:

```bash
git reset <archivo>

git reset --hard <archivo>
```

---
## ✍️ Mejores Prácticas para Mensajes de Commit
Escribir buenos mensajes de _commit_ es un arte que mejora la colaboración y la mantenibilidad de un proyecto. Un mensaje claro y descriptivo ayuda a todos (incluido tu "yo" del futuro) a entender el porqué de cada cambio.
- **Sé claro y conciso**: El mensaje debe explicar qué hace el _commit_.
- **Usa el modo imperativo**: Escribe como si dieras una orden. Por ejemplo, "Agrega" en lugar de "Agregado" o "Agregando".
- **Estructura del mensaje**:
    - **Asunto**: Una línea corta (máximo 50 caracteres) que resuma el cambio.
    - **Cuerpo (opcional)**: Después de una línea en blanco, puedes agregar una descripción más detallada del cambio, explicando el "porqué" y el "cómo".
> [!NOTE] title here  
> 
> 🗒️ Convención de Mensajes: Muchos equipos adoptan convenciones como "Conventional Commits", que prefijan el mensaje con un tipo de cambio (ej. feat:, fix:, docs:, refactor:). Esto ayuda a automatizar la generación de registros de cambios (changelogs) y a navegar el historial más fácilmente.
### Ejemplo de un Buen Mensaje de Commit
```
feat: Implementa la autenticación de usuarios con JWT

- Agrega endpoints para login y registro.
- Utiliza JSON Web Tokens para manejar las sesiones de usuario.
- Incluye validaciones básicas para los campos de entrada.
- Referencia: Tarea JIRA-123
```
> [!WARNING] Precaución
> 
> ⚠️ ¡Cuidado con `git commit --amend!` Si modificas un commit que ya ha sido empujado (pushed) a un repositorio remoto compartido, puedes causar problemas a tus colaboradores. Úsalo principalmente para corregir commits locales que aún no has compartido.
