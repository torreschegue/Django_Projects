# CRUD de Libros con Django y SQLite

## Objetivo

Desarrollar una aplicación web en **Django** que permita realizar operaciones **CRUD** sobre una entidad llamada **Libro**, utilizando **SQLite** como base de datos. El proyecto incluirá:

- Configuración del proyecto Django.
- Acceso a base de datos con SQLite.
- Modelo de datos para guardar libros.
- Lógica de servicios con métodos para cada operación.
- Vistas para conectar la lógica con la interfaz.
- Plantillas HTML con CSS integrado para usar el CRUD desde el navegador.
- Explicación paso a paso desde cero.

---

# 1. Requisitos previos

Antes de comenzar, verifica que tienes instalado:

- Python 3.10 o superior
- pip
- un editor como VS Code, PyCharm o similar
- terminal o consola

Verifica la versión de Python:

```bash
python --version
```

O en Linux:

```bash
python3 --version
```

---

# 2. Crear carpeta del proyecto

Crea una carpeta para el proyecto y entra en ella:

```bash
mkdir crud_django_libros
cd crud_django_libros
```

---

# 3. Crear entorno virtual

## En Linux/macOS

```bash
python3 -m venv venv
source venv/bin/activate
```

## En Windows

```bash
python -m venv venv
venv\Scripts\activate
```

Cuando el entorno virtual esté activo, verás algo similar a esto:

```bash
(venv)
```

---

# 4. Instalar Django

```bash
pip install django
```

Verifica la instalación:

```bash
django-admin --version
```

---

# 5. Crear el proyecto Django

```bash
django-admin startproject config .
```

Esto creará la estructura base del proyecto.

### Estructura inicial esperada

```bash
crud_django_libros/
├── venv/
├── config/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
└── manage.py
```

---

# 6. Crear la aplicación de libros

```bash
python manage.py startapp libros
```

Ahora la estructura será similar a:

```bash
crud_django_libros/
├── config/
├── libros/
│   ├── migrations/
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
├── manage.py
└── venv/
```

---

# 7. Registrar la aplicación en Django

Abre el archivo `config/settings.py` y agrega `libros` en `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'libros',
]
```

---

# 8. Configuración de base de datos SQLite

Django ya trae SQLite por defecto. En `config/settings.py` debe aparecer algo así:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

## ¿Qué significa esto?

- `ENGINE`: define el motor de base de datos.
- `sqlite3`: indica que se usará SQLite.
- `NAME`: el archivo físico donde se guardarán los datos.

No es necesario instalar nada adicional para SQLite.

---

# 9. Crear el modelo de datos para libros

Abre `libros/models.py` y reemplaza su contenido por el siguiente:

```python
from django.db import models


class Libro(models.Model):
    titulo = models.CharField(max_length=150)
    autor = models.CharField(max_length=100)
    editorial = models.CharField(max_length=100)
    anio_publicacion = models.PositiveIntegerField()
    isbn = models.CharField(max_length=20, unique=True)
    cantidad_paginas = models.PositiveIntegerField()
    disponible = models.BooleanField(default=True)
    fecha_creacion = models.DateTimeField(auto_now_add=True)
    fecha_actualizacion = models.DateTimeField(auto_now=True)

    class Meta:
        db_table = 'libros'
        ordering = ['titulo']

    def __str__(self):
        return f"{self.titulo} - {self.autor}"
```

## Explicación del modelo

- `titulo`: nombre del libro.
- `autor`: autor del libro.
- `editorial`: editorial del libro.
- `anio_publicacion`: año en que fue publicado.
- `isbn`: identificador único del libro.
- `cantidad_paginas`: total de páginas.
- `disponible`: indica si el libro está disponible.
- `fecha_creacion`: fecha automática de registro.
- `fecha_actualizacion`: fecha automática de actualización.

---

# 10. Crear migraciones y base de datos

Ejecuta los siguientes comandos:

```bash
python manage.py makemigrations
python manage.py migrate
```

## Resultado esperado

- Se creará el archivo `db.sqlite3`.
- Se crearán las tablas internas de Django.
- Se creará la tabla `libros`.

---

# 11. Crear la capa de servicios

Aunque Django permite trabajar directamente desde las vistas, aquí vamos a separar la lógica en una capa de servicios para tener mejor organización.

Dentro de la app `libros`, crea un archivo llamado `services.py`.

### Archivo: `libros/services.py`

```python
from .models import Libro


class LibroService:

    @staticmethod
    def obtener_todos():
        return Libro.objects.all()

    @staticmethod
    def obtener_por_id(libro_id):
        return Libro.objects.get(id=libro_id)

    @staticmethod
    def crear_libro(data):
        return Libro.objects.create(
            titulo=data['titulo'],
            autor=data['autor'],
            editorial=data['editorial'],
            anio_publicacion=data['anio_publicacion'],
            isbn=data['isbn'],
            cantidad_paginas=data['cantidad_paginas'],
            disponible='disponible' in data
        )

    @staticmethod
    def actualizar_libro(libro, data):
        libro.titulo = data['titulo']
        libro.autor = data['autor']
        libro.editorial = data['editorial']
        libro.anio_publicacion = data['anio_publicacion']
        libro.isbn = data['isbn']
        libro.cantidad_paginas = data['cantidad_paginas']
        libro.disponible = 'disponible' in data
        libro.save()
        return libro

    @staticmethod
    def eliminar_libro(libro):
        libro.delete()
```

## Métodos implementados

- `obtener_todos()`: lista todos los libros.
- `obtener_por_id(libro_id)`: busca un libro por ID.
- `crear_libro(data)`: registra un nuevo libro.
- `actualizar_libro(libro, data)`: actualiza un libro existente.
- `eliminar_libro(libro)`: elimina un libro.

---

# 12. Crear las vistas

Abre `libros/views.py` y coloca lo siguiente:

```python
from django.shortcuts import render, redirect, get_object_or_404
from .models import Libro
from .services import LibroService



def lista_libros(request):
    libros = LibroService.obtener_todos()
    return render(request, 'libros/lista.html', {'libros': libros})



def crear_libro(request):
    if request.method == 'POST':
        LibroService.crear_libro(request.POST)
        return redirect('lista_libros')
    return render(request, 'libros/formulario.html', {'modo': 'crear'})



def editar_libro(request, libro_id):
    libro = get_object_or_404(Libro, id=libro_id)

    if request.method == 'POST':
        LibroService.actualizar_libro(libro, request.POST)
        return redirect('lista_libros')

    return render(request, 'libros/formulario.html', {
        'modo': 'editar',
        'libro': libro
    })



def eliminar_libro(request, libro_id):
    libro = get_object_or_404(Libro, id=libro_id)

    if request.method == 'POST':
        LibroService.eliminar_libro(libro)
        return redirect('lista_libros')

    return render(request, 'libros/eliminar.html', {'libro': libro})
```

---

# 13. Crear las rutas de la aplicación

Crea el archivo `libros/urls.py`:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.lista_libros, name='lista_libros'),
    path('crear/', views.crear_libro, name='crear_libro'),
    path('editar/<int:libro_id>/', views.editar_libro, name='editar_libro'),
    path('eliminar/<int:libro_id>/', views.eliminar_libro, name='eliminar_libro'),
]
```

Ahora abre `config/urls.py` y modifica el contenido así:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('libros.urls')),
]
```

---

# 14. Crear las carpetas de plantillas

Dentro de `libros`, crea esta estructura:

```bash
libros/
├── templates/
│   └── libros/
│       ├── lista.html
│       ├── formulario.html
│       └── eliminar.html
```

---

# 15. Crear la vista HTML + CSS para listar libros

## Archivo: `libros/templates/libros/lista.html`

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lista de Libros</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f6f8;
            margin: 0;
            padding: 30px;
        }

        .container {
            max-width: 1100px;
            margin: auto;
            background: white;
            padding: 25px;
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
        }

        h1 {
            text-align: center;
            color: #333;
            margin-bottom: 20px;
        }

        .acciones-superiores {
            margin-bottom: 20px;
            text-align: right;
        }

        .btn {
            display: inline-block;
            padding: 10px 16px;
            border-radius: 6px;
            text-decoration: none;
            font-weight: bold;
            color: white;
            border: none;
            cursor: pointer;
        }

        .btn-crear {
            background-color: #28a745;
        }

        .btn-editar {
            background-color: #007bff;
        }

        .btn-eliminar {
            background-color: #dc3545;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 10px;
        }

        th, td {
            padding: 12px;
            text-align: center;
            border-bottom: 1px solid #ddd;
        }

        th {
            background-color: #343a40;
            color: white;
        }

        tr:hover {
            background-color: #f1f1f1;
        }

        .disponible {
            color: green;
            font-weight: bold;
        }

        .no-disponible {
            color: red;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>CRUD de Libros</h1>

        <div class="acciones-superiores">
            <a href="{% url 'crear_libro' %}" class="btn btn-crear">+ Nuevo libro</a>
        </div>

        <table>
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Título</th>
                    <th>Autor</th>
                    <th>Editorial</th>
                    <th>Año</th>
                    <th>ISBN</th>
                    <th>Páginas</th>
                    <th>Disponible</th>
                    <th>Acciones</th>
                </tr>
            </thead>
            <tbody>
                {% for libro in libros %}
                <tr>
                    <td>{{ libro.id }}</td>
                    <td>{{ libro.titulo }}</td>
                    <td>{{ libro.autor }}</td>
                    <td>{{ libro.editorial }}</td>
                    <td>{{ libro.anio_publicacion }}</td>
                    <td>{{ libro.isbn }}</td>
                    <td>{{ libro.cantidad_paginas }}</td>
                    <td>
                        {% if libro.disponible %}
                            <span class="disponible">Sí</span>
                        {% else %}
                            <span class="no-disponible">No</span>
                        {% endif %}
                    </td>
                    <td>
                        <a href="{% url 'editar_libro' libro.id %}" class="btn btn-editar">Editar</a>
                        <a href="{% url 'eliminar_libro' libro.id %}" class="btn btn-eliminar">Eliminar</a>
                    </td>
                </tr>
                {% empty %}
                <tr>
                    <td colspan="9">No hay libros registrados.</td>
                </tr>
                {% endfor %}
            </tbody>
        </table>
    </div>
</body>
</html>
```

---

# 16. Crear la vista HTML + CSS para formulario de alta y edición

## Archivo: `libros/templates/libros/formulario.html`

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% if modo == 'crear' %}Registrar Libro{% else %}Editar Libro{% endif %}</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #eef2f7;
            margin: 0;
            padding: 30px;
        }

        .container {
            max-width: 700px;
            margin: auto;
            background: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
        }

        h1 {
            text-align: center;
            margin-bottom: 25px;
            color: #333;
        }

        .form-group {
            margin-bottom: 15px;
        }

        label {
            display: block;
            font-weight: bold;
            margin-bottom: 6px;
            color: #444;
        }

        input[type="text"],
        input[type="number"] {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 6px;
            box-sizing: border-box;
        }

        .checkbox-group {
            margin-top: 10px;
        }

        .acciones {
            margin-top: 20px;
            display: flex;
            justify-content: space-between;
        }

        .btn {
            padding: 10px 18px;
            border: none;
            border-radius: 6px;
            color: white;
            text-decoration: none;
            font-weight: bold;
            cursor: pointer;
        }

        .btn-guardar {
            background-color: #28a745;
        }

        .btn-volver {
            background-color: #6c757d;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>{% if modo == 'crear' %}Registrar Libro{% else %}Editar Libro{% endif %}</h1>

        <form method="post">
            {% csrf_token %}

            <div class="form-group">
                <label for="titulo">Título</label>
                <input type="text" id="titulo" name="titulo" value="{{ libro.titulo|default:'' }}" required>
            </div>

            <div class="form-group">
                <label for="autor">Autor</label>
                <input type="text" id="autor" name="autor" value="{{ libro.autor|default:'' }}" required>
            </div>

            <div class="form-group">
                <label for="editorial">Editorial</label>
                <input type="text" id="editorial" name="editorial" value="{{ libro.editorial|default:'' }}" required>
            </div>

            <div class="form-group">
                <label for="anio_publicacion">Año de publicación</label>
                <input type="number" id="anio_publicacion" name="anio_publicacion" value="{{ libro.anio_publicacion|default:'' }}" required>
            </div>

            <div class="form-group">
                <label for="isbn">ISBN</label>
                <input type="text" id="isbn" name="isbn" value="{{ libro.isbn|default:'' }}" required>
            </div>

            <div class="form-group">
                <label for="cantidad_paginas">Cantidad de páginas</label>
                <input type="number" id="cantidad_paginas" name="cantidad_paginas" value="{{ libro.cantidad_paginas|default:'' }}" required>
            </div>

            <div class="checkbox-group">
                <label>
                    <input type="checkbox" name="disponible" {% if libro.disponible %}checked{% endif %}>
                    Disponible
                </label>
            </div>

            <div class="acciones">
                <button type="submit" class="btn btn-guardar">Guardar</button>
                <a href="{% url 'lista_libros' %}" class="btn btn-volver">Volver</a>
            </div>
        </form>
    </div>
</body>
</html>
```

---

# 17. Crear la vista HTML + CSS para eliminar

## Archivo: `libros/templates/libros/eliminar.html`

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Eliminar libro</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f8f9fa;
            padding: 30px;
        }

        .container {
            max-width: 500px;
            margin: auto;
            background: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
            text-align: center;
        }

        h1 {
            color: #dc3545;
        }

        p {
            margin: 20px 0;
            font-size: 18px;
        }

        .btn {
            display: inline-block;
            padding: 10px 16px;
            border-radius: 6px;
            text-decoration: none;
            color: white;
            font-weight: bold;
            border: none;
            cursor: pointer;
            margin: 5px;
        }

        .btn-eliminar {
            background-color: #dc3545;
        }

        .btn-cancelar {
            background-color: #6c757d;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Eliminar libro</h1>
        <p>¿Deseas eliminar el libro <strong>{{ libro.titulo }}</strong>?</p>

        <form method="post">
            {% csrf_token %}
            <button type="submit" class="btn btn-eliminar">Sí, eliminar</button>
            <a href="{% url 'lista_libros' %}" class="btn btn-cancelar">Cancelar</a>
        </form>
    </div>
</body>
</html>
```

---

# 18. Registrar el modelo en el panel admin

Opcionalmente puedes registrar el modelo para administrarlo desde Django Admin.

Abre `libros/admin.py`:

```python
from django.contrib import admin
from .models import Libro

admin.site.register(Libro)
```

---

# 19. Crear un superusuario

```bash
python manage.py createsuperuser
```

Ingresa:

- nombre de usuario
- correo opcional
- contraseña

Luego podrás entrar al panel en:

```bash
http://127.0.0.1:8000/admin/
```

---

# 20. Ejecutar el servidor

```bash
python manage.py runserver
```

Abre en tu navegador:

```bash
http://127.0.0.1:8000/
```

---

# 21. Paso a paso de uso del CRUD

## Paso 1. Abrir la página principal

Al entrar a la URL principal se mostrará una tabla con la lista de libros.

## Paso 2. Crear un nuevo libro

1. Haz clic en **+ Nuevo libro**.
2. Captura los datos del libro.
3. Presiona **Guardar**.
4. Serás redirigido a la lista y verás el nuevo registro.

## Paso 3. Editar un libro

1. En la tabla, haz clic en **Editar**.
2. Cambia los datos necesarios.
3. Presiona **Guardar**.
4. El sistema actualizará la información.

## Paso 4. Eliminar un libro

1. En la tabla, haz clic en **Eliminar**.
2. Verás una pantalla de confirmación.
3. Presiona **Sí, eliminar**.
4. El registro desaparecerá de la lista.

## Paso 5. Visualizar resultados

Cada acción tendrá efecto directamente en la base de datos SQLite, por lo que los cambios quedarán almacenados en `db.sqlite3`.

---

# 22. Estructura final del proyecto

```bash
crud_django_libros/
├── config/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
├── libros/
│   ├── migrations/
│   ├── templates/
│   │   └── libros/
│   │       ├── eliminar.html
│   │       ├── formulario.html
│   │       └── lista.html
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── services.py
│   ├── urls.py
│   ├── views.py
│   └── tests.py
├── db.sqlite3
├── manage.py
└── venv/
```

---

# 23. Resumen de responsabilidades por archivo

## `models.py`
Define la estructura de la tabla `libros`.

## `services.py`
Contiene la lógica de negocio y operaciones CRUD.

## `views.py`
Recibe peticiones del usuario y conecta servicios con plantillas HTML.

## `urls.py`
Define las rutas del sistema.

## `templates/libros/*.html`
Muestran la interfaz visual del CRUD con HTML y CSS.

---

# 24. Posibles mejoras

Una vez que este CRUD funcione, puedes mejorarlo agregando:

- validaciones con `forms.py`
- mensajes de éxito o error
- paginación
- búsqueda por título o autor
- separación del CSS en archivos estáticos
- diseño responsive más avanzado
- pruebas unitarias
- uso de `class-based views`

---

# 25. Conclusión

Con este proyecto desarrollaste una aplicación CRUD completa en Django usando SQLite. Además de crear el modelo y la base de datos, organizaste la lógica mediante una capa de servicios y construiste una interfaz funcional con HTML y CSS integrado. Esta base te servirá para proyectos más grandes donde después puedas incorporar validaciones, autenticación, formularios más robustos o migrar a motores como MySQL o PostgreSQL.

---

# 26. Comandos rápidos

```bash
# Crear entorno virtual
python -m venv venv

# Activarlo en Windows
venv\Scripts\activate

# Activarlo en Linux/macOS
source venv/bin/activate

# Instalar Django
pip install django

# Crear proyecto
django-admin startproject config .

# Crear app
python manage.py startapp libros

# Crear migraciones
python manage.py makemigrations

# Aplicar migraciones
python manage.py migrate

# Crear superusuario
python manage.py createsuperuser

# Ejecutar servidor
python manage.py runserver
```

