---
aliases:
  - APIs REST 🚪
tags:
  - apis
  - teoria
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-apis|🔌 Índice APIs]]"
Fecha: 2025-06-06
---
# APIs REST: La Puerta a tus Datos 🚪

Las **APIs (Application Programming Interfaces)** son como pasarelas que te permiten acceder y modificar datos de un _back-end_. Una API es **RESTful** si sigue un estilo arquitectónico llamado **REST (Representational State Transfer)**. Es muy popular entre los desarrolladores por su facilidad de desarrollo e implementación, lo que permite crear APIs listas para producción en poco tiempo. 🚀

Una API RESTful facilita la comunicación con un servidor para acceder a los datos que impulsan tu aplicación. Pero, para ser considerada RESTful, debe cumplir con ciertas restricciones clave:

## Restricciones de una API RESTful 🔗

1. **Arquitectura Cliente-Servidor** 🤝
    
    - Debe haber un **servidor** que proporciona los recursos (datos).
        
    - Debe haber un **cliente** que consume esos recursos.
        
2. **Sin Estado (Stateless)** 🧠❌
    
    - El servidor **no guarda información de estado** sobre el cliente que realiza la llamada.
        
    - Esto significa que el servidor no "recuerda" quién hace la solicitud ni qué datos solicitó previamente sin la información de usuario adecuada.
        
    - El estado se guarda **solo en la máquina del cliente**, no en el servidor. Esto influye en cómo diseñas tus _endpoints_ o URL.
        
3. **Cacheadas (Cacheable)** 💾💨
    
    - Las respuestas de la API pueden ser **guardadas en caché** por navegadores web, servidores o cualquier sistema intermedio.
        
    - El _caching_ ayuda a **reducir la carga del servidor** al usar resultados almacenados en caché en lugar de hacer una nueva solicitud cada vez.
        
4. **Sistema en Capas (Layered System)** 🧅
    
    - La arquitectura completa del sistema se puede **dividir o desacoplar en múltiples capas**.
        
    - Puedes **añadir o eliminar una capa** en cualquier momento sin afectar el resto del sistema.
        
    - Ejemplos de capas: Firewall, Balanceador de Carga, Servidor Web, Base de Datos.
        
5. **Interfaz Uniforme** 📏
    
    - El sistema debe ofrecer un **sistema de comunicación uniforme** para acceder a los recursos.
        
    - Esto implica **URLs únicas** para cada recurso.
        
    - Debe haber una forma unificada de modificar un recurso o continuar interactuando con él a través de su representación (por ejemplo, en formato **XML** o **JSON**).
        
6. **Código bajo Demanda (Code on Demand)** (Opcional) 💻
    
    - La API puede entregar **lógica de negocio o código** que el cliente puede ejecutar.
        
    - Esto puede mejorar el resultado de la respuesta.
        
    - Un ejemplo sería una etiqueta `<script>` que carga JavaScript desde un _endpoint_ de la API para mostrar ítems especiales del menú.
        

## Recursos: El Corazón de las APIs REST ❤️‍🩹

Los **recursos** son una parte fundamental de cualquier API REST. Representan los datos con los que interactúas. La forma en que se presentan estos recursos es clave.

Imagina la aplicación de un restaurante:

- **Ver todos los pedidos:** `little.lemon/orders`
    
    - Tipo de recurso: una lista de objetos de pedidos.
        
- **Ver un pedido específico (ID 16):** `little.lemon/orders/16`
    
    - Tipo de recurso: un objeto de pedido.
        
- **Ver el cliente de un pedido específico:** `little.lemon/orders/16/customer`
    
    - Tipo de recurso: un objeto de cliente.
        
- **Ver los ítems del menú de un pedido específico:** `little.lemon/orders/16/menuitems`
    
    - Tipo de recurso: un objeto de ítem del menú.
        
- **Navegar por todo el menú:** `little.lemon/menuitems`
    
    - Tipo de recurso: un objeto de ítem del menú (devuelve todos los ítems).
        
- **Filtrar menú por categoría (ej. aperitivos):** `little.lemon/menuitems?category=appetizers`
    
    - Tipo de recurso: un objeto de ítem del menú (devuelve solo aperitivos). El _endpoint_ es el mismo, pero el parámetro `category` filtra la salida.
        

### La Statelessness y los Recursos: Un Punto Clave 🤔

Es crucial recordar que el servidor **solo representa lo que pides en ese momento**. No recuerda ninguna interacción previa. Si pides información del pedido 16 y luego quieres los ítems del menú de ese pedido, **no puedes simplemente enviar una solicitud a `/menuitems`**. Esto te devolvería _todos_ los ítems del menú, porque el servidor no "recuerda" que tu llamada anterior fue sobre el pedido 16.

Para obtener los ítems del menú de un pedido específico (como el 16), debes **suministrar explícitamente el número de pedido** en la URL: `orders/16/menuitems`.

En resumen, **"sin estado"** significa que el servidor no puede reconocer al cliente automáticamente; cada llamada a la API debe incluir la información necesaria para que el servidor procese la solicitud de forma independiente.

¡Has aprendido los fundamentos de las APIs REST, sus restricciones y cómo se manejan los recursos! Esto te ayudará a construir APIs fáciles de usar y mantener. Hay mucho más por aprender, como las convenciones de nomenclatura correctas, que verás más adelante.