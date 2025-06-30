---
aliases:
  - Servidores y Contenedores Virtuales 💻📦
tags:
  - basicos
  - entornos-de-produccion
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[Indice-conceptos-basicos|💡 Índice de Conceptos Básicos]]"
---
# Servidores y Contenedores Virtuales 💻📦

## Máquinas Virtuales (VM) 🌐

Las **máquinas virtuales (VM)** son servidores virtuales que se ejecutan sobre un servidor físico utilizando **hipervisores**. Cada VM opera como una computadora individual, con su propio sistema operativo y aplicaciones, y direcciones IP dedicadas. 🖥️✨

### Tipos de Hipervisores 🛠️

- **Hipervisor Tipo 1 (Nativo)**:
    - **Funcionamiento**: Se instala directamente sobre el hardware del servidor.
    - **Eficiencia**: Muy eficiente en la gestión de recursos.
    - **Configuración**: Proceso complejo.
    - **Rendimiento**: Aplicaciones más rápidas debido a recursos dedicados.
    - **Ejemplo**: **KVM** (código abierto).
- **Hipervisor Tipo 2**:
    - **Funcionamiento**: Se ejecuta sobre un sistema operativo anfitrión.
    - **Velocidad**: Generalmente más lento que el Tipo 1.
    - **Administración**: Más sencilla gracias a una consola de gestión simple.
    - **Ejemplo**: **Oracle VirtualBox** (código abierto y gratuito).

---

### Compartir Recursos en VMs 🔄

Las VMs pueden usar recursos de dos maneras:

1. **Recursos Dedicados**:
    - **Ventaja**: Asegura la disponibilidad total de los recursos asignados.
    - **Desventaja**: Puede haber desperdicio si la aplicación no utiliza todos los recursos (CPU, RAM, almacenamiento) asignados, ya que otras VMs no pueden acceder a ellos.
2. **Recursos Compartidos**:
    - **Funcionamiento**: Las VMs pueden usar sus recursos asignados y, si necesitan más, acceder a recursos adicionales de máquinas compartidas cuando estén disponibles.
    - **Consideración**: El uso se monitorea, y el uso excesivo continuo puede ser considerado indebido por los proveedores públicos.

> [!WARNING] ¡Cuidado con el uso excesivo! ⚠️
> 
> 🛑 En entornos de recursos compartidos, el monitoreo es constante. Un uso excesivo y prolongado puede llevar a la terminación del servicio por parte del proveedor.

---

## Contenedorización 🐳

La **contenedorización** permite empaquetar una aplicación y todas sus dependencias en archivos de imagen de contenedor, que luego se ejecutan como **contenedores** utilizando un **motor de contenedores**. Esto elimina la necesidad de preocuparse por el hardware o el sistema operativo subyacente. ¡Más tiempo para desarrollar! 👩‍💻👨‍💻

- **Aplicaciones**: Un contenedor puede alojar una o varias aplicaciones con sus dependencias, o distribuir una aplicación en varios contenedores que trabajen juntos.
- **Eficiencia**: Para una gestión eficiente, es recomendable mantener un proceso o una aplicación por contenedor.
- **Ventajas**:
    - **Tamaño**: Menor que las VMs al no incluir un sistema operativo completo.
    - **Velocidad**: Se ejecutan más rápido.
    - **Portabilidad**: Altamente portátiles y fáciles de implementar.
    - **Gestión**: Más sencillos de gestionar que las VMs.
- **Motor Popular**: **Docker** es uno de los motores de contenedores más utilizados y es de código abierto y gratuito.

---

### Términos Clave en Contenedorización 🔑

- **Pod**: Un grupo de contenedores que están estrechamente acoplados, comparten recursos y trabajan juntos para una tarea específica. 🤝
- **Nodo**: La máquina física o virtual que ejecuta uno o varios pods. 🖥️
- **Clúster**: Un conjunto de múltiples pods y nodos que pueden estar relacionados o funcionar de forma independiente. 🏘️
- **Orquestación de Contenedores**: La gestión, implementación, configuración de redes y escalado de contenedores.
    - **Solución Popular**: **Kubernetes (K8s)** es una de las soluciones de orquestación de contenedores más populares y ampliamente utilizada por desarrolladores a nivel mundial. 🌍