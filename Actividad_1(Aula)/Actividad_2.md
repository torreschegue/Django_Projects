# Práctica 2: Página personal con HTML en Django

Esta práctica muestra cómo crear una **página personal con HTML en Django**, comenzando desde un proyecto ya creado o partiendo de la estructura básica de Django.  
El objetivo es que el estudiante aprenda a usar **plantillas HTML**, conectar una **vista** con un **template** y mostrar información personalizada en el navegador.

---

# Objetivo

Crear una página personal en Django utilizando un archivo HTML y una vista que envíe datos dinámicos al navegador.

---

# Lo que aprenderás

Al finalizar esta práctica podrás:

- Crear una plantilla HTML en Django
- Organizar la carpeta `templates`
- Crear una vista con `render()`
- Enviar datos desde Python hacia una página HTML
- Mostrar contenido personalizado en el navegador

---

# Requisitos previos

Antes de comenzar es necesario contar con:

- Python 3 instalado
- pip instalado
- Django instalado
- Haber realizado la práctica 1 o tener un proyecto Django funcionando

Verifica Python y pip con:

```bash
python3 --version
pip --version
```

Verifica Django con:

```bash
django-admin --version
```

---

# Estructura esperada del proyecto

Se trabajará sobre un proyecto como este:

```text
practica_hola_django/
│
├── venv/
├── config/
│   ├── settings.py
│   ├── urls.py
│   └── ...
├── web/
│   ├── views.py
│   ├── urls.py
│   └── ...
└── manage.py
```

Si aún no tienes el proyecto, puedes crearlo con estos comandos:

```bash
mkdir practica_2
cd practica_2
python3 -m venv venv
source venv/bin/activate
pip install django
django-admin startproject config .
python3 manage.py startapp web
```

Después registra la app `web` en `config/settings.py`.

---

# Paso 1: Verificar que la app web esté registrada

Abrir el archivo:

```text
config/settings.py
```

Buscar la lista `INSTALLED_APPS` y comprobar que incluya:

```python
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

Esto permite que Django reconozca la aplicación.

---

# Paso 2: Crear la carpeta templates

Dentro de la carpeta `web`, crear la siguiente estructura:

```text
web/
└── templates/
    └── web/
        └── inicio.html
```

Puedes crearla manualmente o con terminal:

```bash
mkdir -p web/templates/web
```

> En Windows, crea las carpetas manualmente si el comando no funciona.

---

# Paso 3: Crear el archivo HTML

Crear el archivo:

```text
web/templates/web/inicio.html
```

Y agregar el siguiente contenido:

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Página personal</title>
</head>
<body>
    <h1>{{ titulo }}</h1>
    <p>Nombre: {{ nombre }}</p>
    <p>Carrera: {{ carrera }}</p>
    <p>Materia: {{ materia }}</p>
    <p>Bienvenido a mi página personal creada con Django.</p>
</body>
</html>
```

---

# Paso 4: Modificar la vista en views.py

Abrir el archivo:

```text
web/views.py
```

Y reemplazar el contenido por:

```python
from django.shortcuts import render

def inicio(request):
    datos = {
        'titulo': 'Mi Página Personal con Django',
        'nombre': 'Cristian Omar Torres Chegue',
        'carrera': 'Maestría en Innovación y Desarrollo Tecnológico',
        'materia': 'Programación Web'
    }
    return render(request, 'web/inicio.html', datos)
```

## Explicación

- `render()` permite devolver una página HTML
- `'web/inicio.html'` indica el archivo que se mostrará
- `datos` contiene la información que se enviará al template

---

# Paso 5: Verificar o crear las rutas de la app

Abrir o crear el archivo:

```text
web/urls.py
```

Y colocar el siguiente contenido:

```python
from django.urls import path
from .views import inicio

urlpatterns = [
    path('', inicio, name='inicio'),
]
```

Esto indica que la ruta principal ejecutará la vista `inicio`.

---

# Paso 6: Conectar las rutas en el proyecto principal

Abrir el archivo:

```text
config/urls.py
```

Y verificar que tenga este contenido:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('web.urls')),
]
```

Esto conecta las rutas de la app `web` con el proyecto principal.

---

# Paso 7: Ejecutar el servidor

En la terminal, desde la raíz del proyecto, ejecutar:

```bash
python3 manage.py runserver
```

Luego abrir en el navegador:

```text
http://127.0.0.1:8000/
```

---

# Resultado esperado

La página debe mostrar algo similar a:

```text
Mi Página Personal con Django
Nombre: Cristian Omar Torres Chegue
Carrera: Maestría en Innovación y Desarrollo Tecnológico
Materia: Programación Web
Bienvenido a mi página personal creada con Django.
```

Pero con formato HTML en el navegador.

---

# Código completo de los archivos utilizados

## web/views.py

```python
from django.shortcuts import render

def inicio(request):
    datos = {
        'titulo': 'Mi Página Personal con Django',
        'nombre': 'Cristian Omar Torres Chegue',
        'carrera': 'Maestría en Innovación y Desarrollo Tecnológico',
        'materia': 'Programación Web'
    }
    return render(request, 'web/inicio.html', datos)
```

---

## web/urls.py

```python
from django.urls import path
from .views import inicio

urlpatterns = [
    path('', inicio, name='inicio'),
]
```

---

## config/urls.py

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('web.urls')),
]
```

---

## web/templates/web/inicio.html

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Página personal</title>
</head>
<body>
    <h1>{{ titulo }}</h1>
    <p>Nombre: {{ nombre }}</p>
    <p>Carrera: {{ carrera }}</p>
    <p>Materia: {{ materia }}</p>
    <p>Bienvenido a mi página personal creada con Django.</p>
</body>
</html>
```

---

# Estructura final esperada

```text
practica_hola_django/
│
├── venv/
├── config/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
│
├── web/
│   ├── migrations/
│   ├── templates/
│   │   └── web/
│   │       └── inicio.html
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   ├── views.py
│   └── urls.py
│
└── manage.py
```

---

# Actividad extra para el alumno

Modificar la página agregando más información, por ejemplo:

- Edad
- Correo electrónico
- Ciudad
- Hobbies
- Foto personal más adelante usando imágenes estáticas

Ejemplo:

```html
<p>Edad: 28 años</p>
<p>Ciudad: Chilpancingo, Guerrero</p>
<p>Hobby: Desarrollo de software</p>
```

---

# Aprendizajes obtenidos

Con esta práctica el estudiante comprende:

- Cómo funciona una plantilla HTML en Django
- Cómo enviar datos desde una vista a un template
- Cómo personalizar páginas web básicas
- Cómo estructurar una app de Django con buenas bases

---

# Autor

Cristian Omar Torres Chegue  
Práctica 2: Página personal con HTML en Django
