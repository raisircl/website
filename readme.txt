Steps for django project
Step 1: make virtual enviorment 
    py -m venv myvenv
Step 2:  Activate Virtual enviorment
    myvenv\scripts\activate.bat 
Step 3: Install django
    pip install django
Step 4: check version of django
    py -m django --version
Step 5: create a project
    django-admin startproject mysite
step 6: enter to mysite project
    cd mysite
step 7: Run the project
    py manage.py runserver
 step 8: Create an App
    py manage.py startapp webapp
 step 9: create static folder inside webapp then create webapp folder inside static
 step 10: create templates folder inside webapp then create webapp folder inside templates
   
Note: Stop Server - CTRL + C 

Setup Bootstrap Global -

mysite/                      # Root project folder
|
├── manage.py               # Django management script
├── db.sqlite3              # Default DB (if using SQLite)
|
├── mysite/                 # Project configuration folder
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py         # Global settings
│   ├── urls.py             # Root URL configuration
│   └── wsgi.py
|
├── templates/              # Global templates folder
│   ├── base.html           # Base template with global navigation
│   └── home.html           # Global home page
|
├── static/                 # Global static files (e.g., Bootstrap)
│   ├── css/
│   ├── js/
│   └── bootstrap/          # Downloaded Bootstrap files
|
├── webapp/                 # First Django app
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── views.py
│   ├── urls.py
│   ├── templates/
│   │   └── webapp/
│   │       ├── base.html   # Extends global base, app-specific header/footer
│   │       ├── header.html
│   │       ├── footer.html
│   │       └── home.html   # Main page for webapp
│   └── static/
│       └── webapp/         # Optional app-specific static files
|
├── blogapp/                # Second Django app
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── views.py
│   ├── urls.py
│   ├── templates/
│   │   └── blogapp/
│   │       ├── base.html   # Blog-specific layout
│   │       ├── header.html
│   │       ├── footer.html
│   │       └── home.html
│   └── static/
│       └── blogapp/

In mysite/settings.py, add:
# Template config
TEMPLATES = [
    {
        ...
        'DIRS': [BASE_DIR / 'templates'],  # ✅ global templates
        ...
    },
]

# Static files config
STATICFILES_DIRS = [
    BASE_DIR / 'static',  # ✅ global static
]


templates/base.html (Global Base)

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{% block title %}MySite{% endblock %}</title>
    {% load static %}
    <link rel="stylesheet" href="{% static 'bootstrap/css/bootstrap.min.css' %}">
</head>
<body>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <a class="navbar-brand" href="/">Home</a>
        <a class="nav-link" href="/webapp/">WebApp</a>
        <a class="nav-link" href="/blogapp/">BlogApp</a>
    </nav>

    {% block content %}{% endblock %}

    <script src="{% static 'bootstrap/js/bootstrap.bundle.min.js' %}"></script>
</body>
</html>

Example: webapp/templates/webapp/home.html
{% extends "base.html" %}
{% block title %}WebApp Home{% endblock %}
{% block content %}
    {% include "webapp/header.html" %}

    <div class="container">
        <h2>Welcome to WebApp</h2>
    </div>

    {% include "webapp/footer.html" %}
{% endblock %}

mysite/urls.py
from django.contrib import admin
from django.urls import path, include
from django.views.generic import TemplateView

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', TemplateView.as_view(template_name='home.html'), name='home'),
    path('webapp/', include('webapp.urls')),
    path('blogapp/', include('blogapp.urls')),
]

