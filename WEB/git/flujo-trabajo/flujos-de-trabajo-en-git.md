---
aliases:
  - 🌊 Flujos de Trabajo en Git
tags:
  - git
  - git-flow
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-git|🌳 Índice Git]]"
Fecha: 2025-06-17
---
# 🌊 Flujos de Trabajo en Git: Estrategias de Ramificación 🌳

Elegir un flujo de trabajo (`workflow`) en Git es una decisión crucial que define cómo un equipo de desarrollo colabora y gestiona el código de un proyecto. Un buen flujo de trabajo agiliza el desarrollo, facilita la gestión de versiones y minimiza los conflictos. 👨‍💻👩‍💻

A continuación, se presentan los flujos de trabajo más populares, cada uno con sus propias reglas y estrategias de ramificación.

---

## 🚀 Git Flow

**Git Flow** es un modelo de ramificación estricto y robusto, ideal para proyectos con ciclos de lanzamiento programados y una necesidad clara de mantener múltiples versiones en producción.

### 🌿 Ramas Principales

Este flujo de trabajo se centra en dos ramas de larga duración:

| **Rama**            | **Propósito**                                                                                                                            |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `main` (o `master`) | Contiene el código de producción. Cada commit en `main` es una nueva versión estable y está etiquetado con un número de versión (`tag`). |
| `develop`           | Es la rama de integración para las nuevas funcionalidades. Contiene el código más reciente que se incluirá en el próximo lanzamiento.    |

### 🌿 Ramas de Soporte

Para gestionar el desarrollo, se utilizan ramas temporales con propósitos específicos:

- **`feature/*`**: Se crean a partir de `develop` para trabajar en nuevas funcionalidades. Una vez terminadas, se fusionan de nuevo en `develop`.
- **`release/*`**: Se crean desde `develop` cuando se prepara un nuevo lanzamiento. Permiten pulir la versión (corregir bugs, generar documentación) sin detener el desarrollo de nuevas características en `develop`. Una vez lista, la rama `release` se fusiona tanto en `main` como en `develop`.
- **`hotfix/*`**: Se crean a partir de `main` para solucionar errores críticos en producción. Una vez corregido el error, la rama `hotfix` se fusiona tanto en `main` como en `develop`.

> [!INFO] title here  
> 
> ℹ️ Git Flow es excelente para proyectos que requieren un control de versiones explícito, como software de escritorio o aplicaciones que se instalan localmente.

> [!WARNING] title here  
> 
> ⚠️ Su complejidad puede ser excesiva para proyectos más simples o para equipos que practican la Integración y Entrega Continua (CI/CD), donde los lanzamientos son frecuentes.

---

## 🐙 GitHub Flow

**GitHub Flow** es un flujo de trabajo mucho más simple y ligero, optimizado para equipos que realizan despliegues continuos. Su lema es "todo lo que está en `main` es desplegable".

### ⚙️ ¿Cómo Funciona?

1. **Todo comienza en `main`**: La rama `main` siempre contiene una versión estable y lista para ser desplegada.
2. **Crea una rama descriptiva**: Para cualquier cambio (nueva funcionalidad, corrección de error), se crea una nueva rama a partir de `main` con un nombre descriptivo (ej. `fix-login-button`).
3. **Haz commits y empuja los cambios**: Trabaja en tu rama, haciendo commits locales y empujándolos regularmente al repositorio remoto.
4. **Abre un _Pull Request_**: Cuando tu cambio esté listo para ser revisado, abre un _Pull Request_ (Solicitud de Fusión). Esto inicia una discusión y una revisión del código con el equipo.
5. **Despliega para verificar**: Algunos equipos despliegan la rama del _Pull Request_ a un entorno de pruebas para verificar los cambios en un entorno real.
6. **Fusiona (`Merge`)**: Una vez que el _Pull Request_ es aprobado y verificado, se fusiona con la rama `main`.
7. **Despliega `main`**: Inmediatamente después de la fusión, los cambios en `main` pueden ser desplegados a producción.

> [!TIP] title here  
> 
> 💡 Pro Tip: GitHub Flow es ideal para aplicaciones web y proyectos que se benefician de la entrega continua. Fomenta ciclos de desarrollo cortos y revisiones de código constantes.

---

## 🦊 GitLab Flow

**GitLab Flow** es un punto intermedio entre la simplicidad de GitHub Flow y la estructura de Git Flow. Adapta la idea de GitHub Flow pero añade más flexibilidad para manejar múltiples entornos y lanzamientos.

### ✨ Variantes Principales

1. **Ramas de Entorno**: Además de `main`, puedes tener ramas de larga duración que representen tus entornos de despliegue. Por ejemplo: `main` → `staging` → `production`. El flujo de cambios es unidireccional.
2. **Ramas de Lanzamiento**: Para proyectos que necesitan versionar sus lanzamientos, se pueden crear ramas estables por versión (ej. `release-2.0`, `release-3.1`) a partir de `main`. Las correcciones de errores se aplican primero a `main` y luego se pasan (`cherry-pick`) a las ramas de lanzamiento que lo necesiten.

>[!SUCCESS] **Ventaja Principal:** 
> GitLab Flow es más estructurado que GitHub Flow, lo que permite un mejor control sobre los despliegues en diferentes entornos sin la complejidad total de Git Flow. Es muy adecuado para proyectos que usan DevOps y tienen un pipeline de CI/CD bien definido.