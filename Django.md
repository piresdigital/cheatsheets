![Django Cheatsheets](https://github.com/piresdigital/cheatsheets/assets/django.jpg)

# Django Cheatsheet

## Starting a new project.

```bash
# Create project folder
~$  mkdir project_name
~$  cd project_name

# Create Python virtual env
~$  python3 -m venv venv

# Activate virtual env
~$  source venv/bin/activate

# If you want to deactivate virtual env
~$  deactivate

# Install django (~= same as 4.1.*)
~$  pip install django~=4.1.3

# New django project (from project_name folder)
~$  django-admin startproject mysite .

# Run project
~$ python manage.py runserver 8080

```

<br />

## Creating Apps

```bash
# Create app (from project_name folder)
~$  python manage.py startapp app_name

# This is the directory structure created.
#
#  app_name/
#    __init__.py
#    admin.py
#    apps.py
#    migrations/
#        __init__.py
#    models.py
#    tests.py
#    views.py
```

<br />

## Function Views

```python
# views.py
from django.shortcuts import render, redirect

def view_name(request):
    context = ''

    return render(request, 'app_folder/template.html', context)


# urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('page/', views.view_name, name='name'),
]


# mysite/urls.py
from django.urls import include, path

urlpatterns = [
    path('app_name/', include('app_name.urls')),
]
```

<br />

## Class Views

```python
# views.py
from django.views.generic import TemplateView

class PageView(TemplateView):
    template_name = 'template.html'

    # Optional: Change context data dict
    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        # context logic
        return context


# urls.py
from myapp.views import PageView

urlpatterns = [
    path('page/', PageView.as_view()),
]


# mysite/urls.py
from django.urls import include, path

urlpatterns = [
    path('app_name/', include('app_name.urls')),
]
```

<br />

## Django Templates

Templates are stored in app_folder/templates or in app_folder_templates/app_name.

```python
{% extends 'base.html' %}

{% block content %}
{% endblock %}

{% include 'header.html' %}

{% if condition %}
    # Display this block if condition is True
{% else %}
    # Display this block if condition isn't True
{% endif %}

{% for item in items %}
  <p>Access item {{ item }}<p>
{% endfor %}

{{ var_name }}

# Template variables formating
{{ title | lower }}
{{ blog.post | truncatwords:50 }}
{{ order.date | date:"D M Y" }}
{{ list_items | slice:":3" }}
{{ total | default:"nil" }}

#Current path (ex. posts/1/show)
{{ request.path }}

#URL by name with param
{% url 'posts.delete' id=post.id %}

#Use static in template:
{% load static %}
{% static 'css/main.css' %}
```

<br />

## Database Setup

```python
# mysite/settings.py

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        # 'ENGINE': 'django.db.backends.postgresql',
        # 'ENGINE': 'django.db.backends.mysql',
        # 'ENGINE': 'django.db.backends.oracle',

        'NAME': 'database_name',
        'USER': 'root',
        'PASSWORD': '****',
        'HOST': '127.0.0.1',
        'PORT': 0000,
        'OPTIONS': {
            'charset': 'utf8mb4'},
    }}
```

<br />

## Migration

### Make Migrations

This creates a file under app_name/migrations with the database structure.

```bash
~$  python manage.py makemigrations
```

### Migrate

This will execute the commands to create/restructure the database and tables.

```bash
~$  python manage.py migrate
```

<br />

## Creating Models

```python
# models.py

from django.db import models

class ModelName(models.Model)
    char_model = models.Charfield()
    int_model = models.IntegerField()
    string_model = models.TextField()
    email_model = models.EmailField()
    float_model = models.FloatField()
    bol_model = models.BooleanField()
    created_at_model = models.DateTimeField(auto_now_add=True)
    updated_at_model = models.DateTimeField(auto_now=True)


    # Select Field (return value, display value)
    TYPE_CHOICES = (
        ('Customer', 'Customer'),
        ('Supplier', 'Supplier'),
        ('Student', 'Student'),
    )

    type_model = models.CharField(choices=TYPE_CHOICES)

    # String representation
    def __str__(self):
        return self.char_model
```

## Activate Models

```python
# mysite/settings.py

INSTALLED_APPS = [
    # reference to the configuration class in the app_name/apps.py
    'app_name.apps.app_nameConfig',
]
```

```bash
~$ python manage.py makemigrations app_name

~$ python manage.py migrate
```
