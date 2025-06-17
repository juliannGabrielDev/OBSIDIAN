---
aliases:
  - Reload 🔄
tags:
  - python
  - modulos
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-python|🐍 Índice Python]]"
Fecha: 2025-06-02
---
# Reload en Python 🔄
- [[#1. ¿Qué es `reload()`?|1. ¿Qué es `reload()`?]]
- [[#2. ¿Cómo se usa? 🛠️|2. ¿Cómo se usa? 🛠️]]
- [[#3. ¡Ojo con esto! ⚠️|3. ¡Ojo con esto! ⚠️]]
- [[#✨ Repaso Relámpago|✨ Repaso Relámpago]]

---
## 1. ¿Qué es `reload()`?
Es una función que te permite **recargar un módulo que ya habías importado**. Imagina que modificas el código de un módulo y quieres ver los cambios sin reiniciar todo tu programa o script. ¡Ahí entra `reload()`!
- **Re-ejecuta** el código del módulo.
- **Actualiza** el espacio de nombres (namespace) del módulo con las nuevas definiciones.
- Está en el módulo `importlib`.
📌 **Tip:** ¡Perfecto para cuando estás desarrollando y probando módulos!

---
## 2. ¿Cómo se usa? 🛠️
Usar `reload()` es súper sencillo. Primero, asegúrate de que el módulo ya esté importado. Luego, usas `importlib.reload()`.
- Primero, importa `importlib`: `import importlib`
- Luego, si ya tienes `import mi_modulo`, usas: `importlib.reload(mi_modulo)`
**Ejemplo Mini:**
```python
# mi_modulo.py
# print("Hola desde el módulo!")
# version = 1

# --- En tu script principal ---
import mi_modulo  # Output: Hola desde el módulo!
print(mi_modulo.version) # Output: 1

# (Ahora modificas mi_modulo.py, cambias version = 2 y guardas)

import importlib
importlib.reload(mi_modulo) # Output: Hola desde el módulo! (si el print sigue ahí)
print(mi_modulo.version) # Output: 2
```
🧠 **Tip:** Recuerda, `reload()` espera el _objeto_ del módulo, no el nombre del módulo como string.

---
## 3. ¡Ojo con esto! ⚠️
`reload()` es útil, pero tiene sus truquillos y no siempre es la solución mágica.
- **Objetos existentes:** Si tienes instancias de clases del módulo, estas **no** se actualizan para usar las nuevas definiciones de la clase. Siguen usando la versión antigua.
- **`from ... import ...`:** Si importaste nombres específicos con `from mi_modulo import mi_funcion`, esos nombres en tu script actual **no** se actualizan automáticamente con `reload(mi_modulo)`. Tendrías que reimportarlos o acceder a ellos a través del módulo: `mi_modulo.mi_funcion`.
- **Orden de importación:** Puede volverse complicado si hay dependencias circulares o un orden de importación muy específico.
🧐 **Tip:** Úsalo principalmente en desarrollo interactivo. Para producción, es mejor reiniciar.

---
## ✨ Repaso Relámpago
- `reload()` **recarga** un módulo ya importado.
- Está en `importlib`: `importlib.reload(nombre_del_modulo_objeto)`.
- **Cuidado** con instancias existentes y `from ... import ...`.