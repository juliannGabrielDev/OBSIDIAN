# Estructura del proyecto y rutas API

## Introducción

Esta lectura es una visión general del alcance del proyecto, todos los endpoints necesarios y las notas que tendrá que implementar en el proyecto final. Esta lectura le ayudará a completar con éxito el proyecto, así que léala atentamente y consúltela mientras desarrolla su proyecto de API para no perder el rumbo.

## Alcance

Creará un proyecto de API totalmente funcional para el restaurante Little Lemon, de modo que los desarrolladores de aplicaciones del cliente puedan utilizar las API para desarrollar aplicaciones web y móviles. Las personas con distintos roles podrán examinar, añadir y editar elementos del menú, realizar pedidos, examinar pedidos, asignar personal de entrega a los pedidos y, por último, entregar los pedidos.

La siguiente sección le guiará a través de los puntos finales necesarios con un nivel de autorización y otras notas útiles. Su tarea consiste en crear estos puntos finales siguiendo las instrucciones.

## Estructura

Creará una única aplicación Django llamada LittleLemonAPI e implementará todos los puntos finales de la API en ella. Utilice pipenv para gestionar las dependencias en el entorno virtual. Revise el vídeo sobre [Creación de un proyecto Django utilizando pipenv.](https://www.coursera.org/teach/apis/g98MzcdAEeyduw6ktL3Xvw/content/item/lecture/sn2Ez/video-subtitles)

## Vistas basadas en funciones o clases

Puede utilizar vistas basadas en funciones o clases, o ambas, en este proyecto. Siga la convención de nomenclatura API adecuada en todo el proyecto. Revise el video sobre Vistas [basadas](https://www.coursera.org/teach/apis/g98MzcdAEeyduw6ktL3Xvw/content/item/lecture/4BASB/video-subtitles)

en funciones y clases así como el video sobre [Convenciones de n](https://www.coursera.org/teach/apis/g98MzcdAEeyduw6ktL3Xvw/content/item/lecture/1jSCM/video-subtitles)

omenclatura.

## Grupos de usuarios

Cree los siguientes dos grupos de usuarios y luego cree algunos usuarios al azar y asígnelos a estos grupos desde el panel de administración de Django.

- Gestor
    
- Equipo de entrega
    

Los usuarios no asignados a un grupo serán considerados clientes. Revise el vídeo sobre [roles de](https://www.coursera.org/teach/apis/g98MzcdAEeyduw6ktL3Xvw/content/item/lecture/12F4H/video-subtitles)

usuario.

## Comprobación de errores y códigos de estado adecuados

Debe mostrar mensajes de error con los códigos de estado HTTP adecuados para errores específicos. Estos incluyen cuando alguien solicita un elemento inexistente, realiza solicitudes API no autorizadas o envía datos no válidos en una solicitud POST, PUT o PATCH. Aquí tiene una lista completa.

|**Código de estado HTTP**|**Motivo**|
|---|---|
|200 - Ok|Para todas las solicitudes GET, PUT, PATCH y DELETE realizadas con éxito|
|201 - Created|Para todas las solicitudes POST realizadas con éxito|
|403 - Unauthorized|Si falla la autorización para el token de usuario actual|
|401 – Forbidden|Si falla la autenticación del usuario|
|400 – Bad request|Si falla la validación para las llamadas a POST, PUT, PATCH y DELETE|
|404 – Not found|Si la solicitud se realizó para un recurso inexistente|

## Puntos finales de la API

Aquí están todas las rutas API necesarias para este proyecto agrupadas en varias categorías.

### Puntos finales de registro de usuarios y generación de tokens

Puede utilizar Djoser en su proyecto para crear automáticamente los siguientes puntos finales y funcionalidades por usted.

|**Punto final**|**Rol**|**Método**|**Propósito**|
|---|---|---|---|
|/api/users|No requiere rol|POST|Crea un nuevo usuario con nombre, correo electrónico y contraseña|
|/api/users/users/me/|Cualquiera con un token de usuario válido|GET|Muestra sólo el usuario actual|
|/token/login/|Cualquiera con un nombre de usuario y contraseña válidos|POST|Genera tokens de acceso que pueden ser utilizados en otras llamadas API en este proyecto|

Cuando incluya puntos finales de Djoser, Djoser creará otros puntos finales útiles como se discutió en la [Introducción a la biblioteca Djoser para una mejor autenticación](https://www.coursera.org/learn/apis/lecture/bldmJ/introduction-to-djoser-library-for-better-authentication "Link to Introduction to Djoser library for better authentication video")

video.

## puntos finales Menu-items

|**Punto final**|**Rol**|**Método**|**Finalidad**|
|---|---|---|---|
|/api/menu-items|Cliente, personal de entrega|GET|Lista todos los elementos del menú. Devuelve un código de estado HTTP 200 – Ok|
|/api/menu-items|Cliente, equipo de entrega|POST, PUT, PATCH, DELETE|Deniega el acceso y devuelve un código de estado HTTP 403 – Unauthorized|
|/api/menu-items/{menuItem}|Cliente, tripulación de entrega|GET|Lista un único elemento del menú|
|/api/menu-items/{menuItem}|Cliente, equipo de entrega|POST, PUT, PATCH, DELETE|Devoluciones 403 - Unauthorized|
|||||
|/api/menu-items|Gestor|GET|Lista todos los elementos del menú|
|/api/menu-items|Gestor|POST|Crea un nuevo elemento de menú y lo devuelve 201 - Created|
|/api/menu-items/{menuItem}|Gestor|GET|Lista un único elemento de menú|
|/api/menu-items/{menuItem}|Gestor|PUT, PATCH|Actualiza un elemento del menú|
|/api/menu-items/{menuItem}|Gestor|DELETE|Elimina un elemento del menú|

## Puntos finales de gestión de grupos de usuarios

|**Punto final**|**Rol**|**Método**|**Propósito**|
|---|---|---|---|
|/api/groups/manager/users|Gestor|GET|Devuelve todos los gestores|
|/api/groups/manager/users|Gestor|POST|Asigna el usuario de la carga útil al grupo de gestores y devuelve 201-Created|
|/api/groups/manager/users/{userId}|Gestor|DELETE|Elimina este usuario concreto del grupo de gestores y devuelve 200 – Success si todo está bien.<br><br>Si no se encuentra el usuario, devuelve 404 – Not found|
|/api/groups/delivery-crew/users|Gestor|GET|Devuelve todo el grupo de entrega|
|/api/groups/delivery-crew/users|Gestor|POST|Asigna el usuario de la carga útil al grupo de tripulación de entrega y devuelve 201-Created HTTP|
|/api/groups/delivery-crew/users/{userId}|Gestor|DELETE|Elimina este usuario del grupo de gestores y devuelve 200 – Success si todo es correcto.<br><br>Si no se encuentra el usuario, devuelve 404 – Not found|

## Puntos finales de gestión de carros

|**Punto final**|**Rol**|**Método**|**Propósito**|
|---|---|---|---|
|/api/cart/menu-items|Cliente|GET|Devuelve los artículos actuales en el carrito para el token de usuario actual|
|/api/cart/menu-items|Cliente|POST|Añade el artículo del menú al carrito. Establece el usuario autentificado como id de usuario para estos artículos del carrito|
|/api/cart/menu-items|Cliente|DELETE|Elimina todos los artículos del menú creados por el token de usuario actual|

## Puntos finales de gestión de pedidos

|**Punto final**|**Rol**|**Método**|**Propósito**|
|---|---|---|---|
|/api/orders|Cliente|GET|Devuelve todos los pedidos con artículos de pedido creados por este usuario|
|/api/orders|Cliente|POST|Crea un nuevo artículo de pedido para el usuario actual. Obtiene los artículos del carrito actuales de los puntos finales del carrito y añade esos artículos a la tabla de artículos del pedido. A continuación, borra todos los artículos del carrito para este usuario.|
|/api/orders/{orderId}|Cliente|GET|Devuelve todos los artículos para este id de pedido. Si el ID del pedido no pertenece al usuario actual, muestra un código de estado de error HTTP apropiado.|
|/api/orders|Gestor|GET|Devuelve todos los pedidos con artículos del pedido de todos los usuarios|
|/api/orders/{orderId}|Cliente|PUT, PATCH|Actualiza el pedido. Un gestor puede utilizar este punto final para asignar una tripulación de entrega a este pedido, y también actualizar el estado del pedido a 0 ó 1.<br><br>Si se asigna una tripulación de entrega a este pedido y el status = 0, significa que el pedido está listo para su entrega.<br><br>Si se asigna una tripulación de entrega a este pedido y el status = 1, significa que el pedido ha sido entregado.|
|/api/orders/{orderId}|Gestor|DELETE|Borra este pedido|
|/api/orders|Tripulación de entrega|GET|Devuelve todos los pedidos con artículos asignados a la tripulación de entrega|
|/api/orders/{orderId}|Tripulación de entrega|PATCH|Una tripulación de entrega puede utilizar este punto final para actualizar el estado del pedido a 0 ó 1. La tripulación de entrega no podrá actualizar nada más en este pedido.|

## Paso adicional

Implemente las funciones adecuadas de filtrado, paginación y ordenación para /api/menu-items y /api/orders endpoints . Revise los vídeos sobre Filtrado [y búsqueda](https://www.coursera.org/teach/apis/g98MzcdAEeyduw6ktL3Xvw/content/item/lecture/h7QUx/video-subtitles)

y [Paginación](https://www.coursera.org/teach/apis/g98MzcdAEeyduw6ktL3Xvw/content/item/lecture/mEYFj/video-subtitles) , así como la lectura [Más sobre filtrado y paginación](https://www.coursera.org/teach/apis/g98MzcdAEeyduw6ktL3Xvw/content/item/supplement/oCL3M)

.

## Estrangulamiento

Por último, aplique un poco de throttling para los usuarios autentificados y los usuarios anónimos o no autentificados. Revise el vídeo [Configuración del estrangulamiento de la API](https://www.coursera.org/teach/apis/g98MzcdAEeyduw6ktL3Xvw/content/item/lecture/rPE4B/video-subtitles)

y la lectura [Estrangulamiento de la API para vistas basadas en](https://www.coursera.org/teach/apis/g98MzcdAEeyduw6ktL3Xvw/content/item/supplement/1h6WO)

clases para obtener orientación.

## Conclusión

Ahora que tiene una mejor idea del alcance de este proyecto con los puntos finales esenciales de la API, es hora de empezar a codificar. Buena suerte