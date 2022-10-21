# Django introduction

# About Django structure
Each Django web application consists of one project and several apps. 
Project defines projects wide settings, such as templates that can be used by all apps.
Each app defines a separate functionality, such as user management, private messaging in the case of social web apps.

# Setting up environment
You can make a directory, spin up virtual environment with ```python -m venv venv```, install ```pip-tools```, in ```requirements.in``` include one library ```Django``` and install it with ```pip-compile``` and ```pip-sync```

# Setting up django project
To create a project

```shell
django-admin startproject personal_portfolio
```

Above will create a somewhat cluttered folder structure with ```personal_portfolio``` folder nested within newly created ```personal_portfolio``` folder
The parent ```personal_portfolio``` folder will have a file called ```manage.py```. This file will be used to execute tasks like ```runserver```

The subfolder ```personal_portfolio``` contains following files
- settings.py
- urls.py
- wsgi.py 

We can simplify above cluttered folder structure, by moving ```manage.py``` up to top project folder, which is ```rp-portfolio``` in this case. And then move all contents of subfolder ```personal_portfolio``` to its parent and remove subfolder.

# Starting the server
You can go ahead and start django server

```shell
python manage.py runserver
```

# Creating django application

To create an application

```
python manage.py startapp <app name>
```

Above will create below files

- *admin.py* settings for Django admin pages
- *app.py* settings for app configuration
- *models.py* for models
- *tests.py*
- *views.py*

Once you create the app, you need to install it into your project.
You do that by adding your app into ```INSTALLED_APPS``` list in **<parent project>/settings.py**

# Views

You create views in **views.py** of the application

```python
from django.shortcuts import render

def hello(request):
    render(request, "hello.html", {})
```

Above view function will render **hello.html** located in **templates** folder under application folder

# URLs

You first define all url paths in **parent project** *urls.py*

In below examples we are telling to **include** all urls defined in **hello_world** app.

```python
from django.contrib import admin
from django.urls import path,include

urlpatterns = [
    path("/admin",admin.site.urls),
    path("",include("hello_world.urls"))
]
```

Then in *urls.py* of the application, in this case **hello_world**

```python
from django.urls import path
from hello_world import views

urlpatterns = [
    path("",views.hello,name="hello")
]
```

# Models

You define **model**s in **<app folder>/models.py**

```python
from django.db import models

class Project(models.Model):
    title = models.CharField(max_lenth=100)
    description = models.TextField()
    technology = models.CharField(max_length=100)
    image = models.FilePath(path="/img")
```

By default django uses sqlite as database storage.

To actually create database tables, we will have to generate **migrations** and then 
**migrate** them

```shell
python manage.py makemigrations <app name>
```

Then migrate

```bash
python manage.py migrate <app name>
```

Then you can add records to the database by accessing django shell

```bash
python manage.py shell
```

```python
>>>from projects.models import Project

>>>p1=Project(title="A title",description="Description", technology="tech", image="img/project1.png")
>>>p1.save()
```

