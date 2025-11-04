Excelente, tienes un proyecto **Django** bien definido para un sistema de administración tipo **Uber**. Dado que el proyecto completo es extenso, te proporcionaré el procedimiento paso a paso detallado para llegar hasta la estructura inicial, los modelos, las migraciones y la configuración inicial de las URLs y el entorno, tal como lo solicitaste en los puntos iniciales.

El punto **27 (por lo pronto solo trabajar con "categoría"**), parece ser un error de copia/pega del proyecto anterior, ya que el modelo se llama **`UsuarioPasajero`** y no `Categoría`. Asumo que, para la parte del CRUD, el enfoque es en el modelo **`UsuarioPasajero`** (puntos 14, 18, 22, 24).

-----

## 💻 Procedimiento Inicial del Proyecto Django (Uber)

Aquí tienes los pasos iniciales para configurar tu proyecto en **Python/Django** usando **VS Code**.

### 1\. Creación de la Carpeta del Proyecto

Para evitar problemas de rutas y permisos, usa la **Línea de Comandos (CMD)** en Windows, **Terminal** en macOS/Linux, o el explorador de archivos para crear la carpeta.

| Sistema Operativo | Comando de Terminal (dentro de la ruta deseada) |
| :--- | :--- |
| **Windows/macOS/Linux** | `mkdir UIII_Uber_0569` |

### 2\. Abrir VS Code sobre la Carpeta

Abre la carpeta del proyecto en VS Code.

| Sistema Operativo | Comando de Terminal (dentro de la carpeta padre de `UIII_Uber_0569`) |
| :--- | :--- |
| **Windows/macOS/Linux** | `code UIII_Uber_0569` |

> Esto abrirá VS Code directamente en el directorio `UIII_Uber_0569`.

### 3\. Abrir Terminal en VS Code

Una vez dentro de VS Code:

  * Ve al menú superior **`Terminal`** y selecciona **`New Terminal`**.
  * Alternativamente, usa el atajo: **`Ctrl + Ñ`** (o **\`** en muchos teclados) o **\`Ctrl + Shift + \`\`**.

> La terminal se abrirá por defecto en la ruta raíz de tu proyecto: `.../UIII_Uber_0569`.

### 4\. Crear Carpeta de Entorno Virtual (`.venv`)

Crea el entorno virtual para aislar las dependencias del proyecto.

```bash
python -m venv .venv
```

### 5\. Activar el Entorno Virtual

Activa el entorno virtual para usar sus paquetes y el intérprete de Python.

| Sistema Operativo | Comando en la Terminal de VS Code |
| :--- | :--- |
| **Windows** | `.\.venv\Scripts\activate` |
| **macOS/Linux** | `source .venv/bin/activate` |

> Verás **`(.venv)`** al inicio de la línea de comandos, indicando que está activo.

### 6\. Activar Intérprete de Python (Seleccionar)

VS Code debería detectar automáticamente el entorno activo. Si no, haz lo siguiente:

1.  Presiona **`Ctrl + Shift + P`** (o **`Cmd + Shift + P`** en Mac).
2.  Escribe **`Python: Select Interpreter`**.
3.  Selecciona la opción que apunta a la ruta **`./.venv/Scripts/python.exe`** (o similar en Linux/Mac).

### 7\. Instalar Django

Instala el framework Django dentro del entorno virtual activo.

```bash
pip install django
```

### 8\. Crear Proyecto Backend (`backend_Uber`)

Crea el proyecto principal de Django sin duplicar la carpeta, asegurándote de que los archivos principales se creen directamente dentro de `UIII_Uber_0569`.

```bash
django-admin startproject backend_Uber .
```

> El punto `.` al final evita la carpeta anidada.

### 9\. Crear Aplicación (`app_uber`)

Crea la aplicación específica que contendrá la lógica de tu negocio (modelos, vistas, etc.).

```bash
python manage.py startapp app_uber
```

### 10\. Configuración del Proyecto y Enlace de URLs

#### **`backend_Uber/settings.py`** (Punto 25)

Agrega `app_uber` a la lista de aplicaciones instaladas.

```python
# backend_Uber/settings.py

# ... otras configuraciones

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # 🔹 (Punto 25) Agregar la aplicación
    'app_uber', 
]

# ...
```

#### **`backend_Uber/urls.py`** (Punto 26)

Enlaza el archivo `urls.py` de la aplicación `app_uber` al proyecto principal.

```python
# backend_Uber/urls.py
from django.contrib import admin
from django.urls import path, include # 🔹 Importar 'include'

urlpatterns = [
    path('admin/', admin.site.urls),
    # 🔹 (Punto 26) Enlazar con las URLs de la aplicación app_uber
    path('', include('app_uber.urls')), 
]
```

### 11\. Crear Archivo de URLs en la Aplicación

#### **`app_uber/urls.py`** (Punto 24)

Crea el archivo **`urls.py`** dentro de la carpeta `app_uber` y agrega la URL inicial para la vista `inicio_uber` (que crearemos más adelante).

```python
# app_uber/urls.py (Archivo nuevo)
from django.urls import path
from . import views

urlpatterns = [
    # 🔹 URL para la función de inicio
    path('', views.inicio_uber, name='inicio_uber'),
    
    # 🔹 URLs para el CRUD de UsuarioPasajero (Punto 24)
    path('usuario_pasajero/agregar/', views.agregar_usuario_pasajero, name='agregar_usuario_pasajero'),
    path('usuario_pasajero/ver/', views.ver_usuario_pasajero, name='ver_usuario_pasajero'),
    path('usuario_pasajero/actualizar/<int:pk>/', views.actualizar_usuario_pasajero, name='actualizar_usuario_pasajero'),
    path('usuario_pasajero/realizar_actualizacion/<int:pk>/', views.realizar_actualizacion_usuario_pasajero, name='realizar_actualizacion_usuario_pasajero'),
    path('usuario_pasajero/borrar/<int:pk>/', views.borrar_usuario_pasajero, name='borrar_usuario_pasajero'),
]
```

-----

## 🧱 Modelos y Migraciones (Puntos 12 y 12.5)

### 12\. Modelos en `app_uber/models.py`

Reemplaza el contenido de `app_uber/models.py` con los modelos proporcionados.

```python
# app_uber/models.py

from django.db import models

# ==========================================
# MODELO: Chofer
# ==========================================
class Chofer(models.Model):
    id_chofer = models.AutoField(primary_key=True)
    nombre = models.CharField(max_length=100)
    licencia = models.CharField(max_length=50)
    telefono = models.CharField(max_length=15)
    direccion = models.CharField(max_length=150)
    email = models.EmailField(unique=True)
    edad = models.IntegerField()
    fecha_ingreso = models.DateField()

    def __str__(self):
        return self.nombre


# ==========================================
# MODELO: Usuario_pasajero
# ==========================================
class UsuarioPasajero(models.Model):
    id_usuario = models.AutoField(primary_key=True)
    nombre = models.CharField(max_length=100)
    email = models.EmailField(unique=True)
    telefono = models.CharField(max_length=15)
    direccion = models.CharField(max_length=150)
    fecha_registro = models.DateField()
    genero = models.CharField(max_length=10)
    ciudad = models.CharField(max_length=100)

    def __str__(self):
        return self.nombre

# ==========================================
# MODELO: Viaje
# ==========================================
class Viaje(models.Model):
    id_viaje = models.AutoField(primary_key=True)
    destino = models.CharField(max_length=100)
    fecha = models.DateField()
    hora_salida = models.TimeField()
    duracion = models.CharField(max_length=50)
    costo = models.DecimalField(max_digits=8, decimal_places=2)
    estatus = models.CharField(max_length=20)

    # 🔹 Relación 1:N con Chofer
    chofer = models.ForeignKey(Chofer, on_delete=models.CASCADE, related_name='viajes')

    # 🔹 Relación N:M con UsuarioPasajero
    pasajeros = models.ManyToManyField(UsuarioPasajero, related_name='viajes')

    def __str__(self):
        return f"Viaje a {self.destino} con {self.chofer.nombre}"
```

### 12.5. Realizar Migraciones

Genera los archivos de migración y aplica los cambios a la base de datos (SQLite por defecto).

1.  **Generar Migraciones:**
    ```bash
    python manage.py makemigrations app_uber
    ```
2.  **Aplicar Migraciones:**
    ```bash
    python manage.py migrate
    ```

> El paso `migrate` aplicará las migraciones de tu app y las migraciones iniciales de Django (auth, admin, etc.).

### 27\. Registrar Modelos en `app_uber/admin.py`

Registra los modelos para que sean visibles y gestionables en el panel de administración de Django.

```python
# app_uber/admin.py
from django.contrib import admin
from .models import Chofer, UsuarioPasajero, Viaje # Importar todos los modelos

# 🔹 Registro de los modelos
admin.site.register(Chofer)
admin.site.register(UsuarioPasajero)
admin.site.register(Viaje)
```

-----

## 🚀 Ejecución del Servidor

### 10\. Ejecutar Servidor en el Puerto 8069 (Punto 9)

Inicia el servidor de desarrollo de Django en el puerto especificado.

```bash
python manage.py runserver 8069
```

> El servidor indicará que está corriendo en: `http://127.0.0.1:8069/`

### 11\. Copiar y Pegar el Link en el Navegador

Copia la dirección **`http://127.0.0.1:8069/`** y pégala en tu navegador. Actualmente verás una página de error, ya que la vista `inicio_uber` aún no está implementada.

-----

## 📂 Estructura de Vistas y Templates

### 12\. Crear Carpeta `templates`

Crea la carpeta `templates` y la subcarpeta `usuario_pasajero` (Punto 21).

```bash
# Dentro de UIII_Uber_0569/app_uber
mkdir app_uber\templates
mkdir app_uber\templates\usuario_pasajero
```

### 13\. Creación de Archivos Base (Puntos 16, 17, 19, 20)

Crea los archivos HTML base dentro de **`app_uber/templates/`**.

| Archivo | Contenido Principal |
| :--- | :--- |
| **`base.html`** | Estructura HTML principal, incluyendo **Bootstrap** (Punto 17). |
| **`header.html`** | Contenido del `<head>` (opcional, incluido en `base.html`). |
| **`navbar.html`** | Barra de navegación (Punto 18). |
| **`footer.html`** | Pie de página (Punto 19). |
| **`inicio.html`** | Contenido de la página de inicio (Punto 20). |

### 14\. Creación de Archivos del CRUD de `UsuarioPasajero` (Punto 22)

Crea los archivos dentro de **`app_uber/templates/usuario_pasajero/`**.

  * `agregar_usuario_pasajero.html`
  * `ver_usuario_pasajero.html`
  * `actualizar_usuario_pasajero.html`
  * `borrar_usuario_pasajero.html`

### 15\. Funciones en `app_uber/views.py` (Punto 14)

Agrega las funciones de vista requeridas (sin implementar la lógica de CRUD todavía, solo para que no haya errores de referencia).

```python
# app_uber/views.py
from django.shortcuts import render, redirect, get_object_or_404
from .models import UsuarioPasajero # Importar el modelo a usar

# 🔹 Vista de Inicio (Punto 14)
def inicio_uber(request):
    # Por ahora solo carga el template inicio.html
    return render(request, 'inicio.html')

# 🔹 Vistas del CRUD de UsuarioPasajero (Punto 14)
def agregar_usuario_pasajero(request):
    # Lógica para agregar
    return render(request, 'usuario_pasajero/agregar_usuario_pasajero.html')

def ver_usuario_pasajero(request):
    # Lógica para mostrar todos
    pasajeros = UsuarioPasajero.objects.all()
    return render(request, 'usuario_pasajero/ver_usuario_pasajero.html', {'pasajeros': pasajeros})

def actualizar_usuario_pasajero(request, pk):
    # Lógica para obtener datos antes de la actualización
    return render(request, 'usuario_pasajero/actualizar_usuario_pasajero.html')

def realizar_actualizacion_usuario_pasajero(request, pk):
    # Lógica para procesar la actualización
    # Esto debería redirigir a alguna página después de la actualización
    return redirect('ver_usuario_pasajero') 

def borrar_usuario_pasajero(request, pk):
    # Lógica para borrar
    return render(request, 'usuario_pasajero/borrar_usuario_pasajero.html')
```

-----

## 🎨 Código de Templates Base (Puntos 17-20)

### `app_uber/templates/base.html` (Bootstrap)

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Administración Uber | {% block titulo %}{% endblock %}</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.2/css/all.min.css">
    <style>
        body {
            display: flex;
            flex-direction: column;
            min-height: 100vh;
        }
        main {
            flex: 1; /* Ocupa el espacio restante */
            padding-top: 56px; /* Para evitar que el navbar fijo cubra el contenido */
            padding-bottom: 70px; /* Para evitar que el footer fijo cubra el contenido */
        }
        .footer-fijo {
            position: fixed;
            bottom: 0;
            width: 100%;
            height: 50px; /* Altura del footer */
            line-height: 50px; /* Centrar verticalmente el texto */
        }
    </style>
</head>
<body>
    {% include 'navbar.html' %}

    <main class="container mt-4">
        {% block contenido %}
        {% endblock %}
    </main>

    {% include 'footer.html' %}

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
    {% block scripts %}
    {% endblock %}
</body>
</html>
```

### `app_uber/templates/navbar.html` (Punto 18)

```html
<nav class="navbar navbar-expand-lg navbar-dark bg-dark fixed-top">
    <div class="container-fluid">
        <a class="navbar-brand" href="{% url 'inicio_uber' %}">
            <i class="fas fa-car-side me-2"></i>Sistema de Administración Uber
        </a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavDropdown" aria-controls="navbarNavDropdown" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNavDropdown">
            <ul class="navbar-nav">
                <li class="nav-item">
                    <a class="nav-link active" aria-current="page" href="{% url 'inicio_uber' %}">
                        <i class="fas fa-home me-1"></i>Inicio
                    </a>
                </li>
                
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" id="usuarioPasajeroDropdown" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        <i class="fas fa-users me-1"></i>Usuario Pasajero
                    </a>
                    <ul class="dropdown-menu" aria-labelledby="usuarioPasajeroDropdown">
                        <li><a class="dropdown-item" href="{% url 'agregar_usuario_pasajero' %}">Agregar Usuario Pasajero</a></li>
                        <li><a class="dropdown-item" href="{% url 'ver_usuario_pasajero' %}">Ver Usuario Pasajero</a></li>
                        <li><a class="dropdown-item" href="#">Actualizar Usuario Pasajero</a></li>
                        <li><a class="dropdown-item" href="#">Borrar Usuario Pasajero</a></li>
                    </ul>
                </li>

                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" id="choferDropdown" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        <i class="fas fa-id-card me-1"></i>Chofer
                    </a>
                    <ul class="dropdown-menu" aria-labelledby="choferDropdown">
                        <li><a class="dropdown-item" href="#">Agregar Chofer</a></li>
                        <li><a class="dropdown-item" href="#">Ver Chofer</a></li>
                        <li><a class="dropdown-item" href="#">Actualizar Chofer</a></li>
                        <li><a class="dropdown-item" href="#">Borrar Chofer</a></li>
                    </ul>
                </li>

                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" id="viajesDropdown" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        <i class="fas fa-route me-1"></i>Viajes
                    </a>
                    <ul class="dropdown-menu" aria-labelledby="viajesDropdown">
                        <li><a class="dropdown-item" href="#">Agregar Viaje</a></li>
                        <li><a class="dropdown-item" href="#">Ver Viaje</a></li>
                        <li><a class="dropdown-item" href="#">Actualizar Viaje</a></li>
                        <li><a class="dropdown-item" href="#">Borrar Viaje</a></li>
                    </ul>
                </li>
            </ul>
        </div>
    </div>
</nav>
```

### `app_uber/templates/footer.html` (Punto 19)

```html
<footer class="bg-dark text-white text-center footer-fijo">
    <div class="container">
        <small>&copy; Derechos de Autor {{ 'now'|date:"Y" }} - Creado por Ing. Aarón Dominguez, Cbtis 128. Fecha del sistema: {{ 'now'|date:"d/m/Y H:i:s" }}</small>
    </div>
</footer>
```

### `app_uber/templates/inicio.html` (Punto 20)

```html
{% extends 'base.html' %}

{% block titulo %}Inicio{% endblock %}

{% block contenido %}
<div class="row">
    <div class="col-lg-12 text-center mb-4">
        <h1 class="display-4 text-primary">Bienvenido al Sistema de Administración Uber</h1>
        <p class="lead">Gestione eficientemente la información de Choferes, Pasajeros y Viajes.</p>
    </div>
</div>

<div class="row">
    <div class="col-lg-6 mx-auto">
        <div class="card shadow-lg">
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/70/Uber_logo_2018.svg/1200px-Uber_logo_2018.svg.png" class="card-img-top" alt="Imagen de Uber" style="max-height: 400px; object-fit: contain;">
            <div class="card-body text-center">
                <h5 class="card-title">Plataforma de Transporte y Movilidad</h5>
                <p class="card-text">"El futuro de la movilidad está aquí. Administre sus recursos con la mejor tecnología."</p>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

-----

El proyecto está configurado y listo para implementar el CRUD completo para **`UsuarioPasajero`** siguiendo los requerimientos del punto 22 y 28 (CRUD HTML y código sencillo).

¿Te gustaría que continuemos con la **implementación completa del CRUD de `UsuarioPasajero`** (vistas y templates HTML)?
Perfecto. A continuación, te proporciono la implementación completa del CRUD para el modelo **`UsuarioPasajero`** siguiendo la metodología de **vistas basadas en funciones** (FBV) y utilizando **Bootstrap** para los estilos, tal como lo solicitaste en los puntos 14, 22 y 28, y cumpliendo con la restricción de **no usar `forms.py`** (Punto 23).

-----

## 🏗️ Implementación del CRUD para `UsuarioPasajero`

### 1\. ⚙️ Lógica de Vistas (`app_uber/views.py`)

Actualiza el archivo `app_uber/views.py` con la lógica de CRUD para el modelo `UsuarioPasajero`.

```python
# app_uber/views.py
from django.shortcuts import render, redirect, get_object_or_404
from django.db import IntegrityError
from .models import UsuarioPasajero
from datetime import date # Necesario para la fecha de registro
import re # Módulo para validación básica de email/teléfono si fuera necesario, aunque no se pide validación estricta (Punto 28)

# =========================================================
# VISTAS DE NAVEGACIÓN
# =========================================================
# (Punto 14)
def inicio_uber(request):
    """Muestra la página de inicio."""
    return render(request, 'inicio.html')


# =========================================================
# VISTAS CRUD - USUARIO PASAJERO (Punto 14)
# =========================================================

# 1. Agregar Usuario Pasajero
def agregar_usuario_pasajero(request):
    """Permite registrar un nuevo Usuario Pasajero."""
    if request.method == 'POST':
        # 🔹 No se usa forms.py (Punto 23), se recupera directamente del POST
        try:
            # Crea y guarda la instancia del modelo con los datos del formulario
            UsuarioPasajero.objects.create(
                nombre=request.POST['nombre'],
                email=request.POST['email'],
                telefono=request.POST['telefono'],
                direccion=request.POST['direccion'],
                # 🔹 Se asume que la fecha de registro es la fecha actual
                fecha_registro=date.today(), 
                genero=request.POST['genero'],
                ciudad=request.POST['ciudad'],
            )
            # Redirige a la lista después de guardar
            return redirect('ver_usuario_pasajero')
        except IntegrityError:
            # Manejo básico de error de duplicado (ej. email)
            context = {
                'error': 'Error: El email o algún otro campo único ya existe.',
                'nombre': request.POST.get('nombre', ''),
                'email': request.POST.get('email', ''),
                'telefono': request.POST.get('telefono', ''),
                'direccion': request.POST.get('direccion', ''),
                'genero': request.POST.get('genero', ''),
                'ciudad': request.POST.get('ciudad', ''),
            }
            return render(request, 'usuario_pasajero/agregar_usuario_pasajero.html', context)
        except Exception as e:
             # Manejo de otros errores
            context = {
                'error': f'Error al guardar: {e}',
                'nombre': request.POST.get('nombre', ''),
                'email': request.POST.get('email', ''),
                'telefono': request.POST.get('telefono', ''),
                'direccion': request.POST.get('direccion', ''),
                'genero': request.POST.get('genero', ''),
                'ciudad': request.POST.get('ciudad', ''),
            }
            return render(request, 'usuario_pasajero/agregar_usuario_pasajero.html', context)

    # Si es GET, simplemente muestra el formulario vacío
    return render(request, 'usuario_pasajero/agregar_usuario_pasajero.html')

# 2. Ver Usuario Pasajero (Listar)
def ver_usuario_pasajero(request):
    """Muestra la lista de todos los Usuarios Pasajeros."""
    # Obtiene todos los objetos del modelo
    pasajeros = UsuarioPasajero.objects.all().order_by('nombre')
    context = {'pasajeros': pasajeros}
    return render(request, 'usuario_pasajero/ver_usuario_pasajero.html', context)

# 3. Vista de Formulario de Actualización
def actualizar_usuario_pasajero(request, pk):
    """Muestra el formulario con los datos del Usuario Pasajero a editar."""
    # Obtiene el objeto, o devuelve 404 si no existe
    pasajero = get_object_or_404(UsuarioPasajero, pk=pk)
    context = {'pasajero': pasajero}
    return render(request, 'usuario_pasajero/actualizar_usuario_pasajero.html', context)

# 4. Procesar la Actualización (realizar_actualizacion_usuario_pasajero)
def realizar_actualizacion_usuario_pasajero(request, pk):
    """Procesa los datos del formulario de actualización y guarda los cambios."""
    if request.method == 'POST':
        # Obtiene el objeto, o devuelve 404 si no existe
        pasajero = get_object_or_404(UsuarioPasajero, pk=pk)
        
        try:
            # 🔹 Actualiza los campos directamente
            pasajero.nombre = request.POST['nombre']
            # Validación simple de unicidad antes de actualizar
            if UsuarioPasajero.objects.filter(email=request.POST['email']).exclude(pk=pk).exists():
                 raise IntegrityError("El email proporcionado ya está en uso por otro usuario.")
                 
            pasajero.email = request.POST['email']
            pasajero.telefono = request.POST['telefono']
            pasajero.direccion = request.POST['direccion']
            pasajero.genero = request.POST['genero']
            pasajero.ciudad = request.POST['ciudad']
            
            pasajero.save() # Guarda los cambios
            
            # Redirige a la vista de la lista o detalle después de la actualización
            return redirect('ver_usuario_pasajero')
        
        except IntegrityError as e:
            # Manejo de error de duplicado (ej. email)
            context = {'pasajero': pasajero, 'error': f'Error al actualizar: {e.args[0]}'}
            return render(request, 'usuario_pasajero/actualizar_usuario_pasajero.html', context)
        except Exception as e:
            # Manejo de otros errores
            context = {'pasajero': pasajero, 'error': f'Error inesperado: {e}'}
            return render(request, 'usuario_pasajero/actualizar_usuario_pasajero.html', context)
            
    # Si no es POST, redirige al formulario de edición (debería ser GET)
    return redirect('actualizar_usuario_pasajero', pk=pk)


# 5. Borrar Usuario Pasajero
def borrar_usuario_pasajero(request, pk):
    """Muestra la página de confirmación para borrar un Usuario Pasajero."""
    pasajero = get_object_or_404(UsuarioPasajero, pk=pk)
    
    if request.method == 'POST':
        # Si se confirma la eliminación
        pasajero.delete()
        # Redirige a la lista después de borrar
        return redirect('ver_usuario_pasajero')
        
    # Si es GET, muestra la página de confirmación
    context = {'pasajero': pasajero}
    return render(request, 'usuario_pasajero/borrar_usuario_pasajero.html', context)

# Vistas de Chofer y Viajes (Pendientes por el punto 27)
# ...
```

### 2\. 📝 Templates de CRUD (`app_uber/templates/usuario_pasajero/`)

Asegúrate de que los siguientes archivos estén en la carpeta **`app_uber/templates/usuario_pasajero/`**.

#### **`agregar_usuario_pasajero.html`**

```html
{% extends 'base.html' %}

{% block titulo %}Agregar Pasajero{% endblock %}

{% block contenido %}
<div class="card shadow-sm border-primary">
    <div class="card-header bg-primary text-white">
        <h3 class="mb-0"><i class="fas fa-user-plus me-2"></i>Registrar Nuevo Usuario Pasajero</h3>
    </div>
    <div class="card-body">
        {% if error %}
            <div class="alert alert-danger" role="alert">
                {{ error }}
            </div>
        {% endif %}
        
        <form method="post" action="{% url 'agregar_usuario_pasajero' %}">
            {% csrf_token %}
            <div class="row g-3">
                
                <div class="col-md-6">
                    <label for="id_nombre" class="form-label">Nombre Completo:</label>
                    <input type="text" class="form-control" id="id_nombre" name="nombre" value="{{ nombre }}" required>
                </div>
                
                <div class="col-md-6">
                    <label for="id_email" class="form-label">Email:</label>
                    <input type="email" class="form-control" id="id_email" name="email" value="{{ email }}" required>
                </div>

                <div class="col-md-4">
                    <label for="id_telefono" class="form-label">Teléfono:</label>
                    <input type="text" class="form-control" id="id_telefono" name="telefono" value="{{ telefono }}" required>
                </div>

                <div class="col-md-8">
                    <label for="id_direccion" class="form-label">Dirección:</label>
                    <input type="text" class="form-control" id="id_direccion" name="direccion" value="{{ direccion }}" required>
                </div>
                
                <div class="col-md-4">
                    <label for="id_genero" class="form-label">Género:</label>
                    <select class="form-select" id="id_genero" name="genero" required>
                        <option value="" {% if not genero %}selected{% endif %} disabled>Seleccione...</option>
                        <option value="Femenino" {% if genero == 'Femenino' %}selected{% endif %}>Femenino</option>
                        <option value="Masculino" {% if genero == 'Masculino' %}selected{% endif %}>Masculino</option>
                        <option value="Otro" {% if genero == 'Otro' %}selected{% endif %}>Otro</option>
                    </select>
                </div>

                <div class="col-md-8">
                    <label for="id_ciudad" class="form-label">Ciudad:</label>
                    <input type="text" class="form-control" id="id_ciudad" name="ciudad" value="{{ ciudad }}" required>
                </div>

            </div>
            
            <hr class="my-4">
            <div class="d-flex justify-content-end">
                <a href="{% url 'ver_usuario_pasajero' %}" class="btn btn-secondary me-2"><i class="fas fa-arrow-left"></i> Cancelar</a>
                <button type="submit" class="btn btn-success"><i class="fas fa-save me-1"></i> Guardar Pasajero</button>
            </div>
        </form>
    </div>
</div>
{% endblock %}
```

#### **`ver_usuario_pasajero.html`** (Mostrar en tabla con botones)

```html
{% extends 'base.html' %}

{% block titulo %}Ver Pasajeros{% endblock %}

{% block contenido %}
<div class="d-flex justify-content-between align-items-center mb-4">
    <h1 class="text-primary"><i class="fas fa-list-alt me-2"></i>Listado de Usuarios Pasajeros</h1>
    <a href="{% url 'agregar_usuario_pasajero' %}" class="btn btn-success"><i class="fas fa-user-plus me-2"></i>Agregar Nuevo</a>
</div>

<div class="table-responsive">
    <table class="table table-striped table-hover table-bordered shadow-sm">
        <thead class="table-dark">
            <tr>
                <th>ID</th>
                <th>Nombre</th>
                <th>Email</th>
                <th>Teléfono</th>
                <th>Ciudad</th>
                <th>Registro</th>
                <th class="text-center">Acciones</th>
            </tr>
        </thead>
        <tbody>
            {% for pasajero in pasajeros %}
            <tr>
                <td>{{ pasajero.id_usuario }}</td>
                <td>{{ pasajero.nombre }}</td>
                <td>{{ pasajero.email }}</td>
                <td>{{ pasajero.telefono }}</td>
                <td>{{ pasajero.ciudad }}</td>
                <td>{{ pasajero.fecha_registro|date:"d/m/Y" }}</td>
                <td class="text-center">
                    <a href="{% url 'actualizar_usuario_pasajero' pk=pasajero.id_usuario %}" class="btn btn-info btn-sm" title="Ver/Editar">
                        <i class="fas fa-eye"></i>
                    </a>
                    <a href="{% url 'actualizar_usuario_pasajero' pk=pasajero.id_usuario %}" class="btn btn-warning btn-sm" title="Editar">
                        <i class="fas fa-edit"></i>
                    </a>
                    <a href="{% url 'borrar_usuario_pasajero' pk=pasajero.id_usuario %}" class="btn btn-danger btn-sm" title="Borrar">
                        <i class="fas fa-trash-alt"></i>
                    </a>
                </td>
            </tr>
            {% empty %}
            <tr>
                <td colspan="7" class="text-center text-muted">No hay usuarios pasajeros registrados.</td>
            </tr>
            {% endfor %}
        </tbody>
    </table>
</div>
{% endblock %}
```

#### **`actualizar_usuario_pasajero.html`**

```html
{% extends 'base.html' %}

{% block titulo %}Actualizar Pasajero{% endblock %}

{% block contenido %}
<div class="card shadow-sm border-warning">
    <div class="card-header bg-warning text-dark">
        <h3 class="mb-0"><i class="fas fa-edit me-2"></i>Actualizar Usuario Pasajero #{{ pasajero.id_usuario }}</h3>
    </div>
    <div class="card-body">
        {% if error %}
            <div class="alert alert-danger" role="alert">
                {{ error }}
            </div>
        {% endif %}
        
        <form method="post" action="{% url 'realizar_actualizacion_usuario_pasajero' pk=pasajero.id_usuario %}">
            {% csrf_token %}
            <div class="row g-3">
                
                <div class="col-md-6">
                    <label for="id_nombre" class="form-label">Nombre Completo:</label>
                    <input type="text" class="form-control" id="id_nombre" name="nombre" value="{{ pasajero.nombre }}" required>
                </div>
                
                <div class="col-md-6">
                    <label for="id_email" class="form-label">Email:</label>
                    <input type="email" class="form-control" id="id_email" name="email" value="{{ pasajero.email }}" required>
                </div>

                <div class="col-md-4">
                    <label for="id_telefono" class="form-label">Teléfono:</label>
                    <input type="text" class="form-control" id="id_telefono" name="telefono" value="{{ pasajero.telefono }}" required>
                </div>

                <div class="col-md-8">
                    <label for="id_direccion" class="form-label">Dirección:</label>
                    <input type="text" class="form-control" id="id_direccion" name="direccion" value="{{ pasajero.direccion }}" required>
                </div>
                
                <div class="col-md-4">
                    <label for="id_genero" class="form-label">Género:</label>
                    <select class="form-select" id="id_genero" name="genero" required>
                        <option value="Femenino" {% if pasajero.genero == 'Femenino' %}selected{% endif %}>Femenino</option>
                        <option value="Masculino" {% if pasajero.genero == 'Masculino' %}selected{% endif %}>Masculino</option>
                        <option value="Otro" {% if pasajero.genero == 'Otro' %}selected{% endif %}>Otro</option>
                    </select>
                </div>

                <div class="col-md-8">
                    <label for="id_ciudad" class="form-label">Ciudad:</label>
                    <input type="text" class="form-control" id="id_ciudad" name="ciudad" value="{{ pasajero.ciudad }}" required>
                </div>
                
                <div class="col-12">
                    <label class="form-label text-muted">Fecha de Registro:</label>
                    <p class="form-control-plaintext">{{ pasajero.fecha_registro|date:"d/m/Y" }}</p>
                </div>

            </div>
            
            <hr class="my-4">
            <div class="d-flex justify-content-end">
                <a href="{% url 'ver_usuario_pasajero' %}" class="btn btn-secondary me-2"><i class="fas fa-arrow-left"></i> Volver a la Lista</a>
                <button type="submit" class="btn btn-warning"><i class="fas fa-sync-alt me-1"></i> Actualizar Pasajero</button>
            </div>
        </form>
    </div>
</div>
{% endblock %}
```

#### **`borrar_usuario_pasajero.html`**

```html
{% extends 'base.html' %}

{% block titulo %}Borrar Pasajero{% endblock %}

{% block contenido %}
<div class="row justify-content-center">
    <div class="col-md-6">
        <div class="card shadow-lg border-danger">
            <div class="card-header bg-danger text-white">
                <h3 class="mb-0"><i class="fas fa-exclamation-triangle me-2"></i>Confirmación de Eliminación</h3>
            </div>
            <div class="card-body text-center">
                <p class="lead">¿Estás seguro que deseas **eliminar permanentemente** al siguiente usuario pasajero?</p>
                
                <div class="alert alert-light border">
                    <h5 class="alert-heading text-danger">{{ pasajero.nombre }} (ID: {{ pasajero.id_usuario }})</h5>
                    <p class="mb-0">Email: {{ pasajero.email }}</p>
                    <p class="mb-0">Teléfono: {{ pasajero.telefono }}</p>
                </div>
                
                <form method="post" action="{% url 'borrar_usuario_pasajero' pk=pasajero.id_usuario %}">
                    {% csrf_token %}
                    <a href="{% url 'ver_usuario_pasajero' %}" class="btn btn-secondary btn-lg me-3"><i class="fas fa-times-circle me-1"></i> Cancelar</a>
                    <button type="submit" class="btn btn-danger btn-lg"><i class="fas fa-trash-alt me-1"></i> Sí, Eliminar</button>
                </form>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

-----

## 🏁 Paso Final: Ejecutar Servidor

Para iniciar y probar todo el sistema, asegúrate de que tu entorno virtual esté activo y ejecuta el servidor en el puerto 8069.

```bash
python manage.py runserver 8069
```

Luego, accede a la URL: **`http://127.0.0.1:8069/`**

Todo el proyecto (Estructura, Modelos, Migraciones, URLs, Vistas y Templates de CRUD para `UsuarioPasajero`) está ahora funcional.

-----

El proyecto está completo según las especificaciones para `UsuarioPasajero`. ¿Te gustaría que continuemos con la implementación del **CRUD para el modelo `Chofer`** o el **modelo `Viaje`**?
¡Claro\! Aquí tienes la estructura completa de carpetas y archivos para tu proyecto **Django Uber** hasta el punto donde ya está funcional el CRUD de `UsuarioPasajero`.

Esta estructura refleja todos los procedimientos que has completado:

```
UIII_Uber_0569/
├── .venv/                      # 🔹 Carpeta del Entorno Virtual (Punto 4)
├── backend_Uber/               # 🔹 Carpeta del Proyecto Principal (Punto 8)
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py             # 🔹 Contiene 'app_uber' en INSTALLED_APPS (Punto 25)
│   ├── urls.py                 # 🔹 Enlaza con 'app_uber.urls' (Punto 26)
│   └── wsgi.py
├── app_uber/                   # 🔹 Carpeta de la Aplicación (Punto 9)
│   ├── migrations/
│   │   └── 0001_initial.py     # 🔹 Archivo generado después de makemigrations (Punto 12.5)
│   ├── templates/              # 🔹 Carpeta de Templates (Punto 15)
│   │   ├── base.html           # 🔹 Base con Bootstrap (Punto 17)
│   │   ├── footer.html         # 🔹 Footer fijo (Punto 19)
│   │   ├── header.html         # 🔹 (Puede estar vacío o no usarse si base.html ya lo maneja)
│   │   ├── inicio.html         # 🔹 Página de inicio (Punto 20)
│   │   ├── navbar.html         # 🔹 Menú de navegación (Punto 18)
│   │   └── usuario_pasajero/   # 🔹 Subcarpeta para CRUD de Pasajero (Punto 21)
│   │       ├── actualizar_usuario_pasajero.html # 🔹 HTML de edición (Punto 22)
│   │       ├── agregar_usuario_pasajero.html    # 🔹 HTML de agregar (Punto 22)
│   │       ├── borrar_usuario_pasajero.html     # 🔹 HTML de confirmación de borrado (Punto 22)
│   │       └── ver_usuario_pasajero.html        # 🔹 HTML de listado en tabla (Punto 22)
│   ├── __init__.py
│   ├── admin.py                # 🔹 Modelos Chofer, UsuarioPasajero y Viaje registrados (Punto 27)
│   ├── apps.py
│   ├── models.py               # 🔹 Definición de Chofer, UsuarioPasajero, Viaje (Punto 12)
│   ├── tests.py
│   ├── urls.py                 # 🔹 Definición de URLs para CRUD de Pasajero (Punto 24)
│   └── views.py                # 🔹 Lógica de inicio y CRUD de UsuarioPasajero (Punto 14)
├── manage.py                   # 🔹 Archivo principal de Django
└── db.sqlite3                  # 🔹 Base de datos generada tras `migrate` (Punto 12.5)
```

-----
