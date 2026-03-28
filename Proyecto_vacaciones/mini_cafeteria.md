# ☕ Mini Sistema Web de Cafetería con Django

## 📌 Descripción

Este proyecto consiste en el desarrollo de un **mini sistema web de cafetería** utilizando **Django y SQLite**, el cual permite gestionar productos, clientes y pedidos mediante una interfaz web.

El sistema implementa operaciones CRUD completas, relaciones entre modelos y validaciones básicas de negocio.

---

## 🎯 Objetivo

Desarrollar una aplicación web que permita:

* Administrar productos de cafetería
* Gestionar clientes
* Registrar pedidos
* Controlar stock
* Aplicar reglas básicas de negocio

---

## 🛠️ Tecnologías utilizadas

* Python 3.x
* Django
* SQLite
* HTML5
* CSS3

---

## 📁 Estructura del proyecto

```
mini_cafeteria/
│── manage.py
│── mini_cafeteria/
│   │── settings.py
│   │── urls.py
│
│── cafeteria/
│   │── migrations/
│   │── templates/
│   │   │── cafeteria/
│   │   │   │── base.html
│   │   │   │── inicio.html
│   │   │   │── productos_lista.html
│   │   │   │── producto_form.html
│   │   │   │── clientes_lista.html
│   │   │   │── cliente_form.html
│   │   │   │── pedidos_lista.html
│   │   │   │── pedido_form.html
│   │── static/
│   │   │── css/style.css
│   │── models.py
│   │── views.py
│   │── urls.py
│   │── forms.py
```

---

## ⚙️ Instalación y ejecución

### 1. Crear entorno virtual

```bash
python -m venv .venv
source .venv/bin/activate
```

---

### 2. Instalar Django

```bash
pip install django
```

---

### 3. Crear proyecto

```bash
django-admin startproject mini_cafeteria
cd mini_cafeteria
```

---

### 4. Crear aplicación

```bash
python manage.py startapp cafeteria
```

---

### 5. Registrar app en `settings.py`

```python
INSTALLED_APPS = [
    ...
    'cafeteria',
]
```

---

### 6. Ejecutar migraciones iniciales

```bash
python manage.py makemigrations
python manage.py migrate
```

---

### 7. Ejecutar servidor

```bash
python manage.py runserver
```

Abrir en navegador:

```
http://127.0.0.1:8000/
```

---

## 🗄️ Modelos de base de datos

### 👤 Cliente

```python
class Cliente(models.Model):
    nombre = models.CharField(max_length=100)
    apellido = models.CharField(max_length=100)
    telefono = models.CharField(max_length=15)
    correo = models.EmailField(unique=True)
    fecha_registro = models.DateTimeField(auto_now_add=True)
```

---

### ☕ Producto

```python
class Producto(models.Model):
    nombre = models.CharField(max_length=100)
    descripcion = models.TextField()
    precio = models.FloatField()
    stock = models.IntegerField()
    categoria = models.CharField(max_length=50)
    disponible = models.BooleanField(default=True)
```

---

### 🧾 Pedido

```python
class Pedido(models.Model):
    cliente = models.ForeignKey(Cliente, on_delete=models.CASCADE)
    producto = models.ForeignKey(Producto, on_delete=models.CASCADE)
    cantidad = models.IntegerField()
    total = models.FloatField()
    fecha_pedido = models.DateTimeField(auto_now_add=True)
    estado = models.CharField(max_length=20)
```

---

## 🔗 Relaciones

* Un cliente puede tener muchos pedidos
* Un producto puede estar en muchos pedidos
* Pedido conecta cliente con producto

---

## 🔧 Funcionalidades

### Clientes

* Crear cliente
* Listar clientes
* Editar cliente
* Eliminar cliente

### Productos

* Crear producto
* Listar productos
* Editar producto
* Eliminar producto

### Pedidos

* Registrar pedido
* Listar pedidos
* Calcular total automáticamente
* Controlar stock

---

## 📋 Reglas de negocio

* No permitir precios menores o iguales a cero
* No permitir stock negativo
* No permitir pedidos sin stock suficiente
* Descontar stock al registrar pedido
* Validar correo electrónico
* Calcular total automáticamente

---

## 🖥️ Interfaz web

El sistema debe incluir:

* Página de inicio
* Listado de productos
* Listado de clientes
* Listado de pedidos
* Formularios de alta, edición y eliminación
* Navegación simple

---

## 📦 Entregables

* Proyecto completo en Django
* Base de datos SQLite
* Código fuente
* Capturas de funcionamiento
* Este archivo README

---

## 🧪 Actividades mínimas

* Crear modelos
* Ejecutar migraciones
* Implementar CRUD completo
* Crear vistas y rutas
* Crear plantillas HTML
* Aplicar validaciones

---

## 🧠 Recomendaciones

* Diseñar primero la base de datos
* Probar consultas antes de programar vistas
* Usar plantillas reutilizables (`base.html`)
* Validar datos en formularios
* Mantener código limpio

---

## ⭐ Extras (opcional)

* Buscador de productos
* Filtros por categoría
* Historial de pedidos
* Uso de mensajes de Django
* Mejor diseño con CSS

---

## 👨‍💻 Autor

Proyecto académico para práctica de desarrollo web con Django.

---

## 🚀 Estado del proyecto

✅ En desarrollo / práctica académica

---
