# Práctica 4: Mini sitio personal con navegación, templates reutilizables y formulario en Django

Esta práctica está diseñada para continuar con el aprendizaje básico de Django, incorporando más elementos útiles de una aplicación web real.  
Aquí aprenderás a crear un **mini sitio personal** con varias páginas, un **menú de navegación**, una **plantilla base reutilizable** y un **formulario de contacto** con respuesta dinámica.

Todo se trabaja paso a paso para que puedas desarrollarlo desde tu proyecto actual de Django.

---

# Objetivo

Construir un mini sitio web en Django con las siguientes páginas:

- Inicio
- Acerca de mí
- Contacto

Además, aprenderás a:

- Crear varias vistas
- Configurar varias rutas
- Usar una plantilla base
- Reutilizar HTML con herencia de templates
- Crear navegación entre páginas
- Procesar un formulario sencillo
- Mostrar datos enviados por el usuario

---

# Lo que aprenderás

Al finalizar esta práctica podrás:

- Trabajar con varias páginas dentro de una app Django
- Crear un menú de navegación
- Usar `{% extends %}` y `{% block %}`
- Reutilizar una plantilla base
- Capturar varios datos desde un formulario
- Mostrar contenido dinámico en una página web
- Organizar mejor la estructura de templates

---

# Requisitos previos

Antes de comenzar es necesario contar con:

- Python 3 instalado
- pip instalado
- Django instalado
- Un proyecto Django creado
- Una app llamada `web`
- Haber realizado las prácticas anteriores o tener una estructura funcional de Django

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
practica_4/
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
mkdir practica_4
cd practica_4
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

Esto permite que Django reconozca la aplicación dentro del proyecto.

---

# Paso 2: Crear la estructura de templates

Dentro de la carpeta `web`, crear la siguiente estructura:

```text
web/
└── templates/
    └── web/
        ├── base.html
        ├── inicio.html
        ├── acerca.html
        └── contacto.html
```

En Linux o macOS puedes crearla con:

```bash
mkdir -p web/templates/web
touch web/templates/web/base.html
touch web/templates/web/inicio.html
touch web/templates/web/acerca.html
touch web/templates/web/contacto.html
```

En Windows puedes crear las carpetas y archivos manualmente.

---

# Paso 3: Crear la plantilla base

Abrir el archivo:

```text
web/templates/web/base.html
```

Y agregar el siguiente contenido:

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block titulo %}Mi sitio Django{% endblock %}</title>
</head>
<body>
    <header>
        <h1>Mi sitio web con Django</h1>

        <nav>
            <a href="{% url 'inicio' %}">Inicio</a> |
            <a href="{% url 'acerca' %}">Acerca de mí</a> |
            <a href="{% url 'contacto' %}">Contacto</a>
        </nav>

        <hr>
    </header>

    <main>
        {% block contenido %}
        {% endblock %}
    </main>

    <hr>

    <footer>
        <p>Práctica 4 - Programación Web con Django</p>
    </footer>
</body>
</html>
```

## Explicación

- `base.html` será la plantilla principal
- Todas las demás páginas heredarán esta estructura
- Aquí se reutiliza el encabezado, menú y pie de página

---

# Paso 4: Crear la página de inicio

Abrir el archivo:

```text
web/templates/web/inicio.html
```

Y agregar el siguiente contenido:

```html
{% extends 'web/base.html' %}

{% block titulo %}Inicio{% endblock %}

{% block contenido %}
    <h2>Bienvenido</h2>
    <p>Esta es la página principal de mi mini sitio web en Django.</p>
    <p>Aquí aprenderé a trabajar con varias páginas y navegación.</p>
{% endblock %}
```

## Explicación

- `{% extends 'web/base.html' %}` indica que esta página hereda de `base.html`
- `{% block contenido %}` permite definir el contenido único de esta página

---

# Paso 5: Crear la página Acerca de mí

Abrir el archivo:

```text
web/templates/web/acerca.html
```

Y agregar el siguiente contenido:

```html
{% extends 'web/base.html' %}

{% block titulo %}Acerca de mí{% endblock %}

{% block contenido %}
    <h2>Acerca de mí</h2>
    <p>Nombre: Cristian Omar Torres Chegue</p>
    <p>Carrera: Maestría en Innovación y Desarrollo Tecnológico</p>
    <p>Materia: Programación Web</p>
    <p>Intereses: Python, Django, desarrollo web y backend.</p>
{% endblock %}
```

---

# Paso 6: Crear la página de contacto con formulario

Abrir el archivo:

```text
web/templates/web/contacto.html
```

Y agregar el siguiente contenido:

```html
{% extends 'web/base.html' %}

{% block titulo %}Contacto{% endblock %}

{% block contenido %}
    <h2>Formulario de contacto</h2>

    <form method="post">
        {% csrf_token %}

        <label for="nombre">Nombre:</label><br>
        <input type="text" id="nombre" name="nombre"><br><br>

        <label for="correo">Correo:</label><br>
        <input type="email" id="correo" name="correo"><br><br>

        <label for="mensaje">Mensaje:</label><br>
        <textarea id="mensaje" name="mensaje"></textarea><br><br>

        <button type="submit">Enviar</button>
    </form>

    {% if nombre %}
        <hr>
        <h3>Datos recibidos:</h3>
        <p><strong>Nombre:</strong> {{ nombre }}</p>
        <p><strong>Correo:</strong> {{ correo }}</p>
        <p><strong>Mensaje:</strong> {{ mensaje }}</p>
    {% endif %}
{% endblock %}
```

## Explicación

- El formulario usa `method="post"` para enviar datos al servidor
- `{% csrf_token %}` protege el formulario
- Si existe `nombre`, se mostrarán los datos enviados por el usuario

---

# Paso 7: Crear las vistas en views.py

Abrir el archivo:

```text
web/views.py
```

Y colocar el siguiente contenido:

```python
from django.shortcuts import render

def inicio(request):
    return render(request, 'web/inicio.html')

def acerca(request):
    return render(request, 'web/acerca.html')

def contacto(request):
    datos = {}

    if request.method == 'POST':
        datos['nombre'] = request.POST.get('nombre')
        datos['correo'] = request.POST.get('correo')
        datos['mensaje'] = request.POST.get('mensaje')

    return render(request, 'web/contacto.html', datos)
```

## Explicación

- `inicio()` carga la página principal
- `acerca()` carga la página de información personal
- `contacto()` procesa los datos del formulario y los envía al template

---

# Paso 8: Configurar las rutas de la app

Abrir o crear el archivo:

```text
web/urls.py
```

Y agregar el siguiente contenido:

```python
from django.urls import path
from .views import inicio, acerca, contacto

urlpatterns = [
    path('', inicio, name='inicio'),
    path('acerca/', acerca, name='acerca'),
    path('contacto/', contacto, name='contacto'),
]
```

## Explicación

Estas rutas permitirán acceder a:

- `/` → Inicio
- `/acerca/` → Acerca de mí
- `/contacto/` → Contacto

---

# Paso 9: Conectar las rutas en el proyecto principal

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

# Paso 10: Ejecutar el servidor

Desde la raíz del proyecto, ejecutar:

```bash
python3 manage.py runserver
```

Luego abrir en el navegador:

```text
http://127.0.0.1:8000/
```

También puedes entrar directamente a:

```text
http://127.0.0.1:8000/acerca/
http://127.0.0.1:8000/contacto/
```

---

# Resultado esperado

El proyecto debe mostrar un mini sitio con:

- una página principal
- una página acerca de mí
- una página de contacto
- un menú funcional
- un formulario que permite capturar nombre, correo y mensaje
- una respuesta dinámica mostrando los datos recibidos

---

# Código completo de los archivos utilizados

## web/views.py

```python
from django.shortcuts import render

def inicio(request):
    return render(request, 'web/inicio.html')

def acerca(request):
    return render(request, 'web/acerca.html')

def contacto(request):
    datos = {}

    if request.method == 'POST':
        datos['nombre'] = request.POST.get('nombre')
        datos['correo'] = request.POST.get('correo')
        datos['mensaje'] = request.POST.get('mensaje')

    return render(request, 'web/contacto.html', datos)
```

---

## web/urls.py

```python
from django.urls import path
from .views import inicio, acerca, contacto

urlpatterns = [
    path('', inicio, name='inicio'),
    path('acerca/', acerca, name='acerca'),
    path('contacto/', contacto, name='contacto'),
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

## web/templates/web/base.html

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block titulo %}Mi sitio Django{% endblock %}</title>
</head>
<body>
    <header>
        <h1>Mi sitio web con Django</h1>

        <nav>
            <a href="{% url 'inicio' %}">Inicio</a> |
            <a href="{% url 'acerca' %}">Acerca de mí</a> |
            <a href="{% url 'contacto' %}">Contacto</a>
        </nav>

        <hr>
    </header>

    <main>
        {% block contenido %}
        {% endblock %}
    </main>

    <hr>

    <footer>
        <p>Práctica 4 - Programación Web con Django</p>
    </footer>
</body>
</html>
```

---

## web/templates/web/inicio.html

```html
{% extends 'web/base.html' %}

{% block titulo %}Inicio{% endblock %}

{% block contenido %}
    <h2>Bienvenido</h2>
    <p>Esta es la página principal de mi mini sitio web en Django.</p>
    <p>Aquí aprenderé a trabajar con varias páginas y navegación.</p>
{% endblock %}
```

---

## web/templates/web/acerca.html

```html
{% extends 'web/base.html' %}

{% block titulo %}Acerca de mí{% endblock %}

{% block contenido %}
    <h2>Acerca de mí</h2>
    <p>Nombre: Cristian Omar Torres Chegue</p>
    <p>Carrera: Maestría en Innovación y Desarrollo Tecnológico</p>
    <p>Materia: Programación Web</p>
    <p>Intereses: Python, Django, desarrollo web y backend.</p>
{% endblock %}
```

---

## web/templates/web/contacto.html

```html
{% extends 'web/base.html' %}

{% block titulo %}Contacto{% endblock %}

{% block contenido %}
    <h2>Formulario de contacto</h2>

    <form method="post">
        {% csrf_token %}

        <label for="nombre">Nombre:</label><br>
        <input type="text" id="nombre" name="nombre"><br><br>

        <label for="correo">Correo:</label><br>
        <input type="email" id="correo" name="correo"><br><br>

        <label for="mensaje">Mensaje:</label><br>
        <textarea id="mensaje" name="mensaje"></textarea><br><br>

        <button type="submit">Enviar</button>
    </form>

    {% if nombre %}
        <hr>
        <h3>Datos recibidos:</h3>
        <p><strong>Nombre:</strong> {{ nombre }}</p>
        <p><strong>Correo:</strong> {{ correo }}</p>
        <p><strong>Mensaje:</strong> {{ mensaje }}</p>
    {% endif %}
{% endblock %}
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
│   │       ├── base.html
│   │       ├── inicio.html
│   │       ├── acerca.html
│   │       └── contacto.html
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
mkdir practica_4
cd practica_4
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

Esto ocurre cuando Django no encuentra alguno de los archivos HTML.

Verifica que existan exactamente estas rutas:

```text
web/templates/web/base.html
web/templates/web/inicio.html
web/templates/web/acerca.html
web/templates/web/contacto.html
```

---

## Error: NoReverseMatch

Esto ocurre cuando una URL usada en `base.html` no coincide con los nombres definidos en `web/urls.py`.

Revisa que existan exactamente estos nombres:

```python
name='inicio'
name='acerca'
name='contacto'
```

---

## Error: Forbidden 403 CSRF verification failed

Esto sucede cuando falta la etiqueta:

```html
{% csrf_token %}
```

Debe estar dentro del formulario de `contacto.html`.

---

## Error: No module named web.urls

Revisa que `web/urls.py` exista y tenga el contenido correcto.

---

## Error: la app no responde

Verifica que `'web',` esté agregado en `INSTALLED_APPS`.

---

# Actividades extra para el alumno

Puedes mejorar esta práctica agregando:

- una nueva página llamada `servicios`
- estilos CSS básicos
- validación para que no se muestren datos vacíos
- una lista de hobbies en la página acerca de mí
- un footer con nombre del alumno, grupo o semestre

---

# Aprendizajes obtenidos

Con esta práctica el estudiante comprende:

- Cómo crear un mini sitio con varias páginas en Django
- Cómo reutilizar una plantilla base
- Cómo navegar entre páginas usando URLs con nombre
- Cómo procesar formularios con varios campos
- Cómo organizar mejor una aplicación web básica

---

# Autor

Cristian Omar Torres Chegue  
Práctica 4: Mini sitio personal con navegación, templates reutilizables y formulario en Django
