Crear una API Sencilla con Flask para Gestionar Usuarios
Este proyecto guía a los estudiantes para crear una API básica que permite registrar y listar usuarios. Incluye instrucciones detalladas, todo el código necesario y pasos para probar la API usando cURL y Postman.

Requisitos Previos
Antes de comenzar, asegúrate de tener lo siguiente:

Python 3.8 o superior instalado.
Una terminal (Linux, macOS o Windows con PowerShell o CMD).
Conocimientos básicos de Python.
Instalación de pip para gestionar paquetes de Python.
Instalación de Postman para probar la API (opcional pero recomendado).
Paso 1: Crear el Entorno de Trabajo
Crear un directorio para el proyecto:

bash
Copiar código
mkdir flask_api_usuarios
cd flask_api_usuarios
Crear un entorno virtual:

bash
Copiar código
python -m venv venv
Activar el entorno virtual:

En Linux/macOS:
bash
Copiar código
source venv/bin/activate
En Windows:
bash
Copiar código
venv\Scripts\activate
Instalar Flask y SQLAlchemy:

bash
Copiar código
pip install flask flask-sqlalchemy
Crear los archivos principales del proyecto:

bash
Copiar código
touch app.py models.py
