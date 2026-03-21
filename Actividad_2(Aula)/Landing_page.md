# 🧑‍💻 Práctica: Landing Page profesional en Django

## 🎯 Objetivo de la práctica

Crear una landing page personal con las siguientes secciones:

* Barra de navegación:

  * Home
  * Servicios
  * Contacto

* Home:

  * Información personal
  * Experiencia como Software Developer

* Servicios:

  * Desarrollo de API RESTful
  * Microservicios
  * Diseño de base de datos
  * Desarrollo de aplicaciones móviles

* Contacto:

  * Formulario visual (solo diseño)

---

# ⚙️ 1. Requisitos previos

Antes de comenzar, asegúrate de tener instalado:

* Python 3
* pip
* Editor de código (VS Code, PyCharm, etc.)

Verificar instalación:

```bash
python --version
pip --version
```

---

# 📁 2. Crear carpeta del proyecto

```bash
mkdir landing_django
cd landing_django
```

---

# 🧪 3. Crear entorno virtual

## Windows

```bash
python -m venv venv
venv\Scripts\activate
```

## Linux/macOS

```bash
python3 -m venv venv
source venv/bin/activate
```

---

# 📦 4. Instalar Django

```bash
pip install django
```

Verificar:

```bash
django-admin --version
```

---

# 🚀 5. Crear el proyecto Django

```bash
django-admin startproject portfolio_project .
```

---

# 🧩 6. Crear la app

```bash
python manage.py startapp web
```

---

# 🏗️ 7. Estructura del proyecto

```bash
landing_django/
├── manage.py
├── portfolio_project/
└── web/
```

---

# ⚙️ 8. Registrar la app

Editar:

```bash
portfolio_project/settings.py
```

Agregar:

```python
'web',
```

---

# 📂 9. Crear estructura de carpetas

Dentro de `web/`:

```bash
web/
├── templates/
│   └── web/
│       └── index.html
├── static/
│   └── web/
│       └── css/
│           └── styles.css
```

---

# 👨‍💻 10. Crear la vista

Archivo:

```bash
web/views.py
```

```python
from django.shortcuts import render

def home(request):
    return render(request, 'web/index.html')
```

---

# 🔗 11. Crear urls de la app

Archivo:

```bash
web/urls.py
```

```python
from django.urls import path
from .views import home

urlpatterns = [
    path('', home, name='home'),
]
```

---

# 🌐 12. Conectar URLs

Editar:

```bash
portfolio_project/urls.py
```

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('web.urls')),
]
```

---

# 🧾 13. Crear HTML

Archivo:

```bash
web/templates/web/index.html
```

```html
{% load static %}
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Mi Landing Page</title>
    <link rel="stylesheet" href="{% static 'web/css/styles.css' %}">
</head>
<body>

<header>
    <nav>
        <ul>
            <li><a href="#home">Home</a></li>
            <li><a href="#servicios">Servicios</a></li>
            <li><a href="#contacto">Contacto</a></li>
        </ul>
    </nav>
</header>

<section id="home">
    <h1>Hola, soy Cristian</h1>
    <p>Soy Software Developer con experiencia en desarrollo web, APIs y microservicios.</p>
</section>

<section id="servicios">
    <h2>Servicios</h2>
    <ul>
        <li>API RESTful</li>
        <li>Microservicios</li>
        <li>Base de datos</li>
        <li>Apps móviles</li>
    </ul>
</section>

<section id="contacto">
    <h2>Contacto</h2>
    <form>
        <input type="text" placeholder="Nombre">
        <input type="email" placeholder="Correo">
        <textarea placeholder="Mensaje"></textarea>
        <button>Enviar</button>
    </form>
</section>

</body>
</html>
```

---

# 🎨 14. Crear CSS

Archivo:

```bash
web/static/web/css/styles.css
```

```css
body {
    font-family: Arial;
    margin: 0;
}

nav {
    background: black;
    padding: 15px;
}

nav ul {
    display: flex;
    list-style: none;
    gap: 20px;
}

nav a {
    color: white;
    text-decoration: none;
}

section {
    padding: 40px;
}
```

---

# ⚙️ 15. Configurar archivos estáticos

En `settings.py`:

```python
STATIC_URL = 'static/'
```

---

# ▶️ 16. Ejecutar servidor

```bash
python manage.py runserver
```

Abrir en navegador:

```text
http://127.0.0.1:8000/
```

---

# 🎉 Resultado esperado

Una landing page básica con:

* Navegación funcional
* Sección personal
* Servicios
* Formulario de contacto

---

# 🧠 Aprendizajes clave

* Estructura de Django
* Vistas y URLs
* Templates
* Archivos estáticos
* HTML + CSS integrado con Django

---
