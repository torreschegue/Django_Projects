# Práctica 3: Formulario básico y respuesta dinámica en Django

Esta práctica muestra cómo crear un **formulario básico en Django** y cómo mostrar una **respuesta dinámica** en la misma página web.  
El objetivo es que el estudiante aprenda a recibir datos desde un formulario HTML, procesarlos en una vista y devolver una respuesta personalizada en el navegador.

---

# Objetivo

Crear una página web en Django con un formulario que permita capturar el nombre del usuario y mostrar un mensaje dinámico de bienvenida.

---

# Lo que aprenderás

Al finalizar esta práctica podrás:

- Crear un formulario HTML básico en Django
- Usar el método `POST`
- Capturar datos enviados desde el navegador
- Utilizar `request.POST`
- Mostrar una respuesta dinámica en un template
- Mantener la estructura básica de una aplicación Django

---

# Requisitos previos

Antes de comenzar es necesario contar con:

- Python 3 instalado
- pip instalado
- Django instalado
- Tener un proyecto Django creado
- Haber realizado la práctica 1 y práctica 2 o contar con una app `web` funcionando

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

Se trabajará sobre una estructura similar a la siguiente:

```text
practica_3/
│
├── venv/
├── config/
│   ├── settings.py
│   ├── urls.py
│   └── ...
├── web/
│   ├── views.py
│   ├── urls.py
│   ├── templates/
│   │   └── web/
│   └── ...
└── manage.py
```

Si aún no tienes el proyecto, puedes crearlo con estos comandos:

```bash
mkdir practica_3
cd practica_3
python3 -m venv venv
source venv/bin/activate
pip install django
django-admin startproject config .
python3 manage.py startapp web
```

Y después registrar la app `web` en `config/settings.py`.

---

# Paso 1: Verificar que la app web esté registrada

Abrir el archivo:

```text
config/settings.py
```

Buscar la lista `INSTALLED_APPS` y verificar que incluya:

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

---

# Paso 2: Crear la carpeta templates si no existe

Dentro de la carpeta `web`, verificar o crear esta estructura:

```text
web/
└── templates/
    └── web/
        └── formulario.html
```

En Linux o macOS puedes crearla con:

```bash
mkdir -p web/templates/web
```

---

# Paso 3: Crear el archivo HTML del formulario

Crear el archivo:

```text
web/templates/web/formulario.html
```

Y agregar el siguiente contenido:

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Formulario de saludo</title>
</head>
<body>
    <h1>Formulario básico con Django</h1>

    <form method="post">
        {% csrf_token %}
        <label for="nombre">Escribe tu nombre:</label>
        <input type="text" id="nombre" name="nombre" placeholder="Tu nombre">
        <button type="submit">Enviar</button>
    </form>

    {% if nombre %}
        <h2>Hola {{ nombre }}, bienvenido a Django</h2>
    {% endif %}
</body>
</html>
```

---

# Paso 4: Modificar la vista en views.py

Abrir el archivo:

```text
web/views.py
```

Y colocar el siguiente código:

```python
from django.shortcuts import render

def formulario(request):
    nombre = None

    if request.method == 'POST':
        nombre = request.POST.get('nombre')

    return render(request, 'web/formulario.html', {'nombre': nombre})
```

## Explicación

- `request.method == 'POST'` verifica si el usuario envió el formulario
- `request.POST.get('nombre')` obtiene el valor escrito en el campo `nombre`
- `render()` devuelve el template HTML con el dato capturado

---

# Paso 5: Crear o modificar el archivo de rutas de la app

Abrir o crear el archivo:

```text
web/urls.py
```

Y agregar el siguiente contenido:

```python
from django.urls import path
from .views import formulario

urlpatterns = [
    path('', formulario, name='formulario'),
]
```

Esto indica que la ruta principal cargará el formulario.

---

# Paso 6: Conectar las rutas de la app con el proyecto principal

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

---

# Paso 7: Ejecutar el servidor

Desde la raíz del proyecto, ejecutar:

```bash
python3 manage.py runserver
```

Luego abrir en el navegador:

```text
http://127.0.0.1:8000/
```

---

# Paso 8: Probar el formulario

Al abrir la página aparecerá un formulario con un campo de texto y un botón.

Escribe un nombre, por ejemplo:

```text
Cristian
```

Haz clic en **Enviar**.

La página mostrará el mensaje:

```text
Hola Cristian, bienvenido a Django
```

---

# Resultado esperado

La página debe mostrar primero el formulario y, después de enviar un nombre, la respuesta dinámica debajo del botón.

---

# Código completo de los archivos utilizados

## web/views.py

```python
from django.shortcuts import render

def formulario(request):
    nombre = None

    if request.method == 'POST':
        nombre = request.POST.get('nombre')

    return render(request, 'web/formulario.html', {'nombre': nombre})
```

---

## web/urls.py

```python
from django.urls import path
from .views import formulario

urlpatterns = [
    path('', formulario, name='formulario'),
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

## web/templates/web/formulario.html

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Formulario de saludo</title>
</head>
<body>
    <h1>Formulario básico con Django</h1>

    <form method="post">
        {% csrf_token %}
        <label for="nombre">Escribe tu nombre:</label>
        <input type="text" id="nombre" name="nombre" placeholder="Tu nombre">
        <button type="submit">Enviar</button>
    </form>

    {% if nombre %}
        <h2>Hola {{ nombre }}, bienvenido a Django</h2>
    {% endif %}
</body>
</html>
```

---

# Estructura final esperada

```text
practica_3/
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
│   │       └── formulario.html
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

# Comandos principales usados en la práctica

```bash
mkdir -p web/templates/web
python3 manage.py runserver
```

Si aún no existe el proyecto, primero crear todo con:

```bash
mkdir practica_hola_django
cd practica_hola_django
python3 -m venv venv
source venv/bin/activate
pip install django
django-admin startproject config .
python3 manage.py startapp web
python3 manage.py runserver
```

---

# Posibles errores comunes

## Error: TemplateDoesNotExist

Esto ocurre cuando Django no encuentra el archivo HTML.

Verifica que exista exactamente esta ruta:

```text
web/templates/web/formulario.html
```

---

## Error: Forbidden 403 CSRF verification failed

Esto sucede cuando falta la etiqueta:

```html
{% csrf_token %}
```

Debe estar dentro del formulario.

---

## Error: No module named web.urls

Revisa que `web/urls.py` exista y contenga el código correcto.

---

## Error: la app no responde

Verifica que `'web',` esté agregado en `INSTALLED_APPS`.

---

## Error: no se muestra el saludo

Asegúrate de que el campo del formulario tenga:

```html
name="nombre"
```

Y que en la vista exista:

```python
request.POST.get('nombre')
```

---

# Actividad extra para el alumno

Modificar el formulario para capturar más datos, por ejemplo:

- nombre
- edad
- carrera

Y mostrar un mensaje más completo.

Ejemplo:

```text
Hola Cristian, tienes 28 años y estudias programación web.
```

Esto puede lograrse agregando más campos al formulario y capturándolos en la vista.

---

# Aprendizajes obtenidos

Con esta práctica el estudiante comprende:

- Cómo crear formularios básicos en Django
- Cómo capturar información enviada por el usuario
- Cómo procesar datos con `POST`
- Cómo mostrar respuestas dinámicas en una plantilla HTML

---

# Autor

Cristian Omar Torres Chegue  
Práctica 3: Formulario básico y respuesta dinámica en Django
