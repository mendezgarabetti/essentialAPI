
# Crear una API Sencilla con Flask para Gestionar Usuarios

Guía básica para crear una API básica que permite registrar y listar usuarios. Incluye instrucciones detalladas, todo el código necesario y pasos para probar la API usando **cURL** y **Postman**.

---

## Requisitos Previos

Antes de comenzar, asegúrate de tener lo siguiente:

1. Python 3.8 o superior instalado.
2. Una terminal (Linux, macOS o Windows con PowerShell o CMD).
3. Conocimientos básicos de Python.
4. Instalación de `pip` para gestionar paquetes de Python.
5. Instalación de Postman para probar la API (opcional pero recomendado).

---

## Paso 1: Crear el Entorno de Trabajo

1. **Crear un directorio para el proyecto**:
   ```bash
   mkdir flask_api_usuarios
   cd flask_api_usuarios
   ```

2. **Crear un entorno virtual**:
   ```bash
   python -m venv venv
   ```

3. **Activar el entorno virtual**:
   - En Linux/macOS:
     ```bash
     source venv/bin/activate
     ```
   - En Windows:
     ```bash
     venv\Scripts\activate
     ```

4. **Instalar Flask y SQLAlchemy**:
   ```bash
   pip install flask flask-sqlalchemy
   ```

5. **Crear los archivos principales del proyecto**:
   ```bash
   touch app.py models.py
   ```

---

## Paso 2: Configurar el Modelo de Base de Datos

Abre el archivo `models.py` y escribe el siguiente código para definir la estructura de la tabla `usuarios`:

```python
from flask_sqlalchemy import SQLAlchemy

# Crear instancia de SQLAlchemy
db = SQLAlchemy()

# Modelo de la tabla usuarios
class Usuario(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    nombre = db.Column(db.String(100), nullable=False)
    email = db.Column(db.String(100), unique=True, nullable=False)

    def __repr__(self):
        return f"<Usuario {self.nombre}>"
```

---

## Paso 3: Configurar la API en Flask

Abre el archivo `app.py` y escribe el siguiente código para configurar la API:

```python
from flask import Flask, request, jsonify
from models import db, Usuario

# Configuración inicial de la app
app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///usuarios.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

# Inicializar la base de datos
db.init_app(app)

# Crear las tablas
@app.before_first_request
def create_tables():
    db.create_all()

# Ruta para registrar un usuario
@app.route('/usuarios', methods=['POST'])
def registrar_usuario():
    data = request.json
    nuevo_usuario = Usuario(nombre=data['nombre'], email=data['email'])
    db.session.add(nuevo_usuario)
    db.session.commit()
    return jsonify({"message": "Usuario registrado con éxito!"}), 201

# Ruta para listar todos los usuarios
@app.route('/usuarios', methods=['GET'])
def listar_usuarios():
    usuarios = Usuario.query.all()
    resultado = [{"id": u.id, "nombre": u.nombre, "email": u.email} for u in usuarios]
    return jsonify(resultado), 200

# Ejecutar la app
if __name__ == '__main__':
    app.run(debug=True)
```

---

## Paso 4: Ejecutar la API Localmente

1. **Ejecuta la aplicación Flask**:
   ```bash
   python app.py
   ```

2. La aplicación estará disponible en [http://127.0.0.1:5000](http://127.0.0.1:5000).

---

## Paso 5: Probar la API

### **Probar con cURL**

1. **Registrar un nuevo usuario**:
   ```bash
   curl -X POST -H "Content-Type: application/json" -d '{"nombre": "Juan Perez", "email": "juan.perez@example.com"}' http://127.0.0.1:5000/usuarios
   ```

2. **Listar todos los usuarios**:
   ```bash
   curl -X GET http://127.0.0.1:5000/usuarios
   ```

### **Probar con Postman**

1. **Crear una nueva colección en Postman**.
2. **Agregar una solicitud para registrar un usuario**:
   - Método: `POST`.
   - URL: `http://127.0.0.1:5000/usuarios`.
   - Cuerpo (Body): Seleccionar `raw` y elegir formato `JSON`.  
     Ejemplo:
     ```json
     {
       "nombre": "Ana Lopez",
       "email": "ana.lopez@example.com"
     }
     ```

3. **Agregar una solicitud para listar usuarios**:
   - Método: `GET`.
   - URL: `http://127.0.0.1:5000/usuarios`.

---

## Paso 6: Extensiones Opcionales

1. **Validación de datos**: Usar Flask-WTF o Marshmallow para validar entradas.
2. **Autenticación básica**: Proteger los endpoints con tokens JWT.
3. **Despliegue en un servicio cloud**: Subir la API a un servicio gratuito como Render, Railway o Heroku.

---

## Código Completo del Proyecto

### `models.py`

```python
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()

class Usuario(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    nombre = db.Column(db.String(100), nullable=False)
    email = db.Column(db.String(100), unique=True, nullable=False)

    def __repr__(self):
        return f"<Usuario {self.nombre}>"
```

### `app.py`

```python
from flask import Flask, request, jsonify
from models import db, Usuario

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///usuarios.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db.init_app(app)

@app.before_first_request
def create_tables():
    db.create_all()

@app.route('/usuarios', methods=['POST'])
def registrar_usuario():
    data = request.json
    nuevo_usuario = Usuario(nombre=data['nombre'], email=data['email'])
    db.session.add(nuevo_usuario)
    db.session.commit()
    return jsonify({"message": "Usuario registrado con éxito!"}), 201

@app.route('/usuarios', methods=['GET'])
def listar_usuarios():
    usuarios = Usuario.query.all()
    resultado = [{"id": u.id, "nombre": u.nombre, "email": u.email} for u in usuarios]
    return jsonify(resultado), 200

if __name__ == '__main__':
    app.run(debug=True)
```

---

## Recursos Adicionales

1. [Documentación de Flask](https://flask.palletsprojects.com/)
2. [SQLAlchemy](https://docs.sqlalchemy.org/)
3. [Postman](https://www.postman.com/)

¡Disfruta aprendiendo y construyendo tu API! 🚀
