Esta lectura explica el uso de URL con nombre en el URLconf de una app Django y cómo el uso de un espacio de nombres ayuda a resolver el mismo nombre de URL en más de una app.

En cada app, hay un archivo urls.py que define la lista de patrones de URL para esa app. Cada patrón se construye mediante la función django.urls.path(). Sus argumentos son una cadena de ruta URL, el nombre de la función de vista que se le va a asignar y un nombre de argumento opcional.

A continuación se muestra urls.py de una app:

1

#demoapp/urls.py 

2

from django.urls import path 

3

from . import views 

4

​

5

  urlpatterns = [ 

6

    path('', views.index, name='index'), 

7

    path('login/', views.login, name='login') 

8

] 

Se incluye en el urlpatterns del proyecto.

1

from django.contrib import admin 

2

from django.urls import path, include 

3

​

4

  urlpatterns = [ 

5

    path('admin/', admin.site.urls), 

6

    path('demo/', include('demoapp.urls')), 

7

] 

Normalmente, la URL de solicitud del cliente se mapea contra la función para que el flujo de la aplicación se dirija hacia ella.

Las referencias URL utilizadas internamente por la aplicación son cadenas codificadas.

Por ejemplo, el atributo de acción de una plantilla de formulario apunta hacia la URL /demoapp/login para que, cuando se envíe el formulario, se invoque la vista de inicio de sesión mapeada a esta URL.

1

<form action='/demoapp/login', method='POST> 

2

    # ...

3

</form> 

## función reverse()

Sin embargo, las URL codificadas de forma rígida hacen que la aplicación sea menos escalable y difícil de mantener a medida que crece el proyecto. En tal caso, puede obtener la URL a partir del parámetro de nombre utilizado en la función path().

Inicie el shell de Django.

1

python manage.py shell 

La función reverse() de Django devuelve la ruta URL a la que está asignada.

1

>>> from django.urls import reverse 

2

>>> reverse('index') 

3

'/demo/' 

El problema surge cuando la función de vista del mismo nombre se define en más de una aplicación. Aquí es donde se necesita la idea de un namespace.

## Espacio de nombres de la aplicación

El espacio de nombres de la aplicación se crea definiendo la variable app_name en el urls.py de la aplicación y asignándole el nombre de la app. En el script demoapp/urls.py, realice el cambio utilizando el siguiente código:

1

#demoapp/urls.py

2

from django.urls import path  

3

from . import views    

4

app_name='demoapp' 

5

urlpatterns = [  

6

    path('', views.index, name='index'),      

7

] 

app_name define el espacio de nombres de la aplicación para que las vistas de esta app se identifiquen con él.

Para obtener la ruta URL de la función index(), llame a la función reverse() anteponiéndole el espacio de nombres.

1

>>> reverse('demoapp:index') 

2

'/demo/' 

Para apreciar la ventaja de definir un espacio de nombres, añada otra app en el proyecto, por ejemplo, newapp. Proporcione una función de vista index() en ella y defina app_name en su archivo URLConf (es decir urls.py).

1

#newapp/urls.py 

2

from django.urls import path 

3

from . import views 

4

app_name='newapp' 

5

urlpatterns = [ 

6

    path('', views.index, name='index'), 

7

] 

Actualice el urls.py del proyecto.

1

from django.contrib import admin 

2

from django.urls import path, include 

3

​

4

urlpatterns = [ 

5

    path('admin/', admin.site.urls), 

6

    path('demo/', include('demoapp.urls')), 

7

    path('newdemo/', include('newapp.urls')), 

8

] 

La función reverse() se ejecuta para la vista índice en el espacio de nombres newapp.

1

>>> reverse('newapp:index') 

2

'/newdemo/' 

Puede ver que Django diferencia entre URLs con el mismo nombre en múltiples aplicaciones con espacio de nombres de aplicación.

## Espacio de nombres de instancia

También puede utilizar el parámetro namespace en la función include() al añadir el urlpattern de una app al de un proyecto.

1

#in demoproject/urls.py 

2

urlpatterns=[ 

3

    # ... 

4

    path('demo/', include('demoapp.urls', namespace='demoapp')), 

5

    # ... 

6

] 

Este espacio de nombres se denomina instance namespace.

## Uso del espacio de nombres en la vista

Supongamos que desea que el usuario sea redirigido condicionalmente a la vista de inicio de sesión desde dentro de otra vista. Necesita obtener la URL de la vista de inicio de sesión y enviar el control a ella con HttpResponsePermanentRedirect.

1

from django.http import HttpResponsePermanentRedirect 

2

from django.urls import reverse 

3

4

def myview(request): 

5

    .... 

6

    return HttpResponsePermanentRedirect(reverse('demoapp:login'))

## namespace en la etiqueta url

Se enviará un formulario HTML a la URL especificada en el atributo action.

1

<form action="/demoapp/login" method="post"> 

2

​

3

#form attributes 

4

​

5

<input type='submit'> 

6

​

7

</form> 

A continuación, el formulario será procesado por la vista asignada a esta URL. Sin embargo, como se mencionó anteriormente, no se desea una URL codificada. En su lugar, utilice la etiqueta url del lenguaje de plantillas Django. Devuelve la ruta absoluta de la URL nombrada.

Utilice la etiqueta url para obtener la ruta de la URL de forma dinámica, como se muestra a continuación:

1

<form action="{% url 'login' %}" method="post"> 

2

#form attributes 

3

<input type='submit'> 

4

</form> 

De nuevo, la vista de inicio de sesión puede estar presente en varias aplicaciones. Utilice la URL nombrada cualificada con el espacio de nombres para resolver el conflicto.

1

<form action="{% url 'demoapp:login' %}" method="post"> 

2

#form attributes 

3

<input type='submit'> 

4

</form> 

El navegador muestra el formulario de inicio de sesión como se indica a continuación:

![Vista del navegador del formulario de inicio de sesión con pestañas de nombre de usuario y contraseña, así como botón de envío](https://d3c33hcgiwev3.cloudfront.net/imageAssetProxy.v1/lyoNkV0pTDufhWIoe8bRhQ_0bedde03463c4fc8940cdb5d162364e1_C5M2L3_Itm02_1.png?expiry=1748476800000&hmac=CL4KckQJT4OdwPe_oh77EmqgUySomQidCJPlHwjk_ao)

Así, el concepto de espacio de nombres ayuda a resolver el conflicto que surge cuando varias aplicaciones de un mismo proyecto tienen vistas con el mismo nombre.

Usted ha cubierto algunos de los conceptos aquí en línea con el uso de plantillas de formulario en Django. Si tiene dificultades para entender algunas cosas, tenga la seguridad de que será mucho más claro una vez que cubra el tema de las plantillas en este curso.

En esta lectura, ha explorado más a fondo el concepto de URL Namespacing y Views en Django.