
Modelo
Claro, ¡aquí continuamos con el proyecto!
actualizar_usuario_pasajero.html(Continuación)
code
HTML
<select class="form-select" id="genero" name="genero" required>
                                <option value="">Seleccione...</option>
                                <option value="Masculino" {% if usuario.genero == 'Masculino' %}selected{% endif %}>Masculino</option>
                                <option value="Femenino" {% if usuario.genero == 'Femenino' %}selected{% endif %}>Femenino</option>
                                <option value="Otro" {% if usuario.genero == 'Otro' %}selected{% endif %}>Otro</option>
                            </select>
                        </div>
                        <div class="mb-3">
                            <label for="ciudad" class="form-label">Ciudad:</label>
                            <input type="text" class="form-control" id="ciudad" name="ciudad" value="{{ usuario.ciudad }}" required>
                        </div>
                        <div class="d-grid gap-2 mt-4">
                            <button type="submit" class="btn btn-warning btn-lg"><i class="fas fa-sync-alt"></i> Actualizar Usuario</button>
                            <a href="{% url 'ver_usuario_pasajero' %}" class="btn btn-secondary btn-lg"><i class="fas fa-arrow-alt-circle-left"></i> Cancelar</a>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>
{% endblock %}
borrar_usuario_pasajero.html
AbreUIII_Uber_0569/app_uber/templates/app_uber/usuario_pasajero/borrar_usuario_pasajero.htmly pega:
code
HTML
{% extends 'app_uber/base.html' %}

{% block titulo %}Borrar Usuario Pasajero{% endblock %}

{% block contenido %}
<div class="container mt-5">
    <div class="row justify-content-center">
        <div class="col-md-6">
            <div class="card border-danger shadow-sm" style="border-radius: 15px;">
                <div class="card-header bg-danger text-white text-center" style="border-top-left-radius: 15px; border-top-right-radius: 15px;">
                    <h3 class="mb-0"><i class="fas fa-exclamation-triangle"></i> Confirmar Eliminación</h3>
                </div>
                <div class="card-body p-4 text-center">
                    <p class="card-text fs-5">¿Estás seguro de que quieres eliminar al usuario pasajero:</p>
                    <h4 class="text-danger mb-4">{{ usuario.nombre }} (ID: {{ usuario.id_usuario }})?</h4>
                    <p class="text-muted">Esta acción no se puede deshacer.</p>

                    <form method="post">
                        {% csrf_token %}
                        <div class="d-grid gap-2 mt-4">
                            <button type="submit" class="btn btn-danger btn-lg"><i class="fas fa-trash-alt"></i> Eliminar Definitivamente</button>
                            <a href="{% url 'ver_usuario_pasajero' %}" class="btn btn-secondary btn-lg"><i class="fas fa-times-circle"></i> Cancelar</a>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>
{% endblock %}
23. No utilizarformularios.py.
¡Confirmado! Estamos utilizando formularios HTML directamente consolicitud.POST.
24. Procedimiento para crear el archivourls.pyenaplicación_Ubercon el código correspondiente para acceder a las funciones devistas.pypara operaciones de CRUD enusuario_pasajero.
Abajo de laUIII_Uber_0569/app_uber, crea un nuevo archivo llamadourls.pyy pega el siguiente contenido:
code
Pitón
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_uber, name='inicio_uber'),
    path('usuarios/agregar/', views.agregar_usuario_pasajero, name='agregar_usuario_pasajero'),
    path('usuarios/', views.ver_usuario_pasajero, name='ver_usuario_pasajero'),
    path('usuarios/actualizar/<int:id_usuario>/', views.actualizar_usuario_pasajero, name='actualizar_usuario_pasajero'),
    path('usuarios/actualizar/confirmar/<int:id_usuario>/', views.realizar_actualizacion_usuario_pasajero, name='realizar_actualizacion_usuario_pasajero'),
    path('usuarios/borrar/<int:id_usuario>/', views.borrar_usuario_pasajero, name='borrar_usuario_pasajero'),
]
25. Procedimiento para agregaraplicación_Uberensettings.pyDelawarebackend_Uber
Abre el archivoUIII_Uber_0569/backend_Uber/settings.py. Busca la listaAPLICACIONES_INSTALADASy agrega'app_uber'a la lista. Debería verse algo así:
code
Pitón
# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_uber', # Agrega tu app aquí
]
26. Realice las configuraciones correspondientes aurls.pyDelawarebackend_Uberpara enlazar conaplicación_Uber
Abre el archivoUIII_Uber_0569/backend_Uber/urls.pyAgregadoincluiry una nueva ruta paraaplicación_uber:
code
Pitón
from django.contrib import admin
from django.urls import path, include # Asegúrate de importar include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('uber/', include('app_uber.urls')), # Enlaza las URLs de app_uber
]
27. Procedimiento para registrar los modelos enadmin.pyy volver a realizar las migraciones.
Abre el archivoUIII_Uber_0569/app_uber/admin.pyy registra tu modeloPasajero:
code
Pitón
from django.contrib import admin
from .models import Chofer, UsuarioPasajero, Viaje

# Register your models here.
admin.site.register(UsuarioPasajero)
# admin.site.register(Chofer) # Dejamos Chofer y Viaje comentados por ahora
# admin.site.register(Viaje)
Volver a realizar las migraciones:
Aunque no agregamos nuevos modelos ni modificamos los existentes en este paso, es una buena práctica ejecutarhacer migracionesyemigrardespués de cambios significativos en los modelos o su registro, especialmente si se hubieran agregado otros modelos que no eranPasajeroinicialmente. En este caso específico, no es estrictamente necesario ya que solo registramos un modelo ya migrado, pero lo hacemos para seguir tu solicitud.
Asegúrese de estar en el directorioUIII_Uber_0569(donde estágestionar.py) y ejecuta:
code
Intento
python manage.py makemigrations
python manage.py migrate
27. Por lo pronto solo trabajar con “categoría” dejar pendiente # MODELO: Chofer y # MODELO: Viaje
Entendido. Hemos enfocado el desarrollo enPasajero(que asumí que era "categoría" por tumodelos.py) y hemos dejado los modelosChoferyViajependientes enadmin.pyy en elbarra de navegación.html.
28. Utilizar colores suaves, atractivos y modernos, el código de las páginas web sencillas.
Se han utilizado clases de Bootstrap comoluz de fondo,texto blanco,botón-primario,btn-info,btn-warning,btn-peligro, y colores suaves como#f0f0f0y#f8f9fapara elbarra de navegaciónypie de páginarespectivamente, buscando una estética limpia y moderna.
28. No validar entrada de datos.
No se ha implementado validación a nivel de formulario en Django (formularios.py) ni validación exhaustiva en las vistas, siguiendo esta instrucción. Las validaciones se limitan a las de HTML (requerido).
29. Al inicio crear la estructura completa de carpetas y archivos.
La estructura de carpetas y archivos se ha creado y se ha instruido su creación a medida que avanzamos. Al final de este punto, la estructura debería ser algo así:
code
Código
UIII_Uber_0569/
├── .venv/
├── backend_Uber/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── app_uber/
│   ├── migrations/
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   ├── views.py
│   ├── urls.py  <-- Nuevo archivo
│   └── templates/
│       └── app_uber/
│           ├── base.html
│           ├── header.html
│           ├── navbar.html
│           ├── footer.html
│           ├── inicio.html
│           └── usuario_pasajero/
│               ├── agregar_usuario_pasajero.html
│               ├── ver_usuario_pasajero.html
│               ├── actualizar_usuario_pasajero.html
│               └── borrar_usuario_pasajero.html
└── manage.py
30. Proyecto totalmente funcional.
Hasta este punto, el módulo dePasajeroes funcional para agregar, ver, actualizar y borrar (CRUD básico).
31. Finalmente ejecute el servidor en el puerto 8069.
Para ejecutar el servidor, asegúrese de tener su entorno virtual activado y estar en el directorioUIII_Uber_0569(donde estágestionar.py). Luego, ejecuta:
code
Intento
python manage.py runserver 8069
Ahora puedes ir ahttp://127.0.0.1:8069/uber/en tu navegador para ver la página de inicio y empezar a gestionar los usuarios pasajeros.
¡Espero que esta continuación sea útil! Si tienes alguna otra solicitud o necesitas más detalles, házmelo saber.
