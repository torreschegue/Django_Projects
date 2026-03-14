# Práctica 1: Hola Mundo con Django

Este ejercicio muestra cómo crear una aplicación web básica utilizando
**Python y Django**, comenzando completamente desde cero sin descargar
ningún repositorio.

Todo el proyecto se construye paso a paso utilizando únicamente la
terminal y los comandos de Django.

------------------------------------------------------------------------

# Objetivo

Crear un proyecto básico en Django que muestre el mensaje **Hola Mundo
con Django** en el navegador web.

------------------------------------------------------------------------

# Requisitos previos

Antes de comenzar es necesario tener instalado en el sistema:

-   Python 3
-   pip

Puedes verificarlo ejecutando los siguientes comandos en la terminal:

``` bash
python3 --version
pip --version
```

------------------------------------------------------------------------

# Paso 1: Crear la carpeta del proyecto

Abrir una terminal y crear una carpeta donde se desarrollará el
proyecto.

``` bash
mkdir Django_Project
cd Django_Project
mkdir Practica_1
cd Practica_1
```

------------------------------------------------------------------------

# Paso 2: Crear un entorno virtual

Un entorno virtual permite aislar las librerías del proyecto.

``` bash
python3 -m venv venv
```

Esto creará una carpeta llamada **venv** donde se almacenarán las
dependencias del proyecto.

------------------------------------------------------------------------

# Paso 3: Activar el entorno virtual

### En Linux o macOS

``` bash
source venv/bin/activate
```

### En Windows

``` bash
venv\Scripts\activate
```

Cuando el entorno virtual esté activo, la terminal mostrará algo similar
a:

    (venv)

------------------------------------------------------------------------

# Paso 4: Instalar Django

Con el entorno virtual activo, instalar Django con el siguiente comando:

``` bash
pip install django
```

Verificar la instalación ejecutando:

``` bash
django-admin --version
```

------------------------------------------------------------------------

# Paso 5: Crear el proyecto Django

Crear el proyecto ejecutando:

``` bash
django-admin startproject config .
```

El punto **"."** indica que el proyecto se creará dentro de la carpeta
actual.

Después de ejecutar el comando se crearán los siguientes archivos:

    config/
    manage.py

------------------------------------------------------------------------

# Paso 6: Ejecutar el servidor de desarrollo

Ejecutar el siguiente comando:

``` bash
python3 manage.py runserver
```

Abrir el navegador y acceder a:

    http://127.0.0.1:8000/

Si todo funciona correctamente aparecerá la página de bienvenida de
Django.

------------------------------------------------------------------------

# Paso 7: Crear una aplicación

En Django, el proyecto puede contener múltiples aplicaciones.

Crear una aplicación llamada **web**.

``` bash
python3 manage.py startapp web
```

Esto creará una nueva carpeta llamada **web**.

------------------------------------------------------------------------

# Paso 8: Registrar la aplicación en Django

Abrir el archivo:

    config/settings.py

Buscar la sección **INSTALLED_APPS** y agregar la aplicación **web**.

``` python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'web',
]
```

Esto permite que Django reconozca la aplicación dentro del proyecto.

------------------------------------------------------------------------

# Paso 9: Crear una vista

Abrir el archivo:

    web/views.py

Y agregar el siguiente código:

``` python
from django.http import HttpResponse

def inicio(request):
    return HttpResponse("Hola Mundo con Django")
```

Esta función responderá a las solicitudes del navegador mostrando el
mensaje.

------------------------------------------------------------------------

# Paso 10: Crear el archivo de rutas de la aplicación

Crear un archivo nuevo llamado:

    web/urls.py

Agregar el siguiente contenido:

``` python
from django.urls import path
from .views import inicio

urlpatterns = [
    path('', inicio, name='inicio'),
]
```

Esto define que la ruta principal ejecutará la función **inicio**.

------------------------------------------------------------------------

# Paso 11: Conectar las rutas de la app con el proyecto

Abrir el archivo:

    config/urls.py

Modificar el contenido para que quede así:

``` python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('web.urls')),
]
```

Esto conecta las rutas de la aplicación con el proyecto principal.

------------------------------------------------------------------------

# Paso 12: Ejecutar nuevamente el servidor

Ejecutar nuevamente el servidor:

``` bash
python3 manage.py runserver
```

Abrir el navegador y acceder a:

    http://127.0.0.1:8000/

Ahora el navegador mostrará el mensaje:

    Hola Mundo con Django

------------------------------------------------------------------------

# Estructura final del proyecto

La estructura del proyecto debería verse de la siguiente manera:

    practica_hola_django/
    │
    ├── venv/
    │
    ├── config/
    │   ├── __init__.py
    │   ├── asgi.py
    │   ├── settings.py
    │   ├── urls.py
    │   └── wsgi.py
    │
    ├── web/
    │   ├── migrations/
    │   ├── __init__.py
    │   ├── admin.py
    │   ├── apps.py
    │   ├── models.py
    │   ├── tests.py
    │   ├── views.py
    │   └── urls.py
    │
    └── manage.py

------------------------------------------------------------------------

# Aprendizajes obtenidos

Después de completar esta práctica se comprende cómo:

-   Crear un proyecto Django desde cero
-   Crear una aplicación
-   Registrar aplicaciones en Django
-   Crear vistas
-   Configurar rutas
-   Ejecutar el servidor de desarrollo
-   Mostrar contenido en el navegador

------------------------------------------------------------------------

# Autor

Cristian Omar Torres Chegue\
Práctica introductoria de programación web con Django y Python.
