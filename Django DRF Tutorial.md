# Django/DRF Tutorial

Building the backend of a To Do app. 

> *This tutorial was based on [**Writing your First Django App**](https://docs.djangoproject.com/en/4.0/intro/tutorial01/) and **[How To Build a To-Do application Using Django and React](https://www.digitalocean.com/community/tutorials/build-a-to-do-application-using-django-and-react)***
> 

---

**Contents:**

---

# What is Django

> *Django is a high-level Python web framework that encourages rapid development and clean, pragmatic design. Built by experienced developers, it takes care of much of the hassle of web development, so you can focus on writing your app without needing to reinvent the wheel. It’s free and open source.*
> 

Django is a Python-based web application framework that comes fully loaded with modules and features that handle common web development tasks right out of the box. Some examples:

- authentication
- routing
- content administration
- templating
- ORM
- ...

The goal of Django is to allow developers to build out an application as quickly as possible by taking care of as much of the basic boilerplate stuff for you. This makes it a great tool choice for a hackathon or your CMPUT 401 projects.

This tutorial won’t cover all of these features extensively. It’s meant to provide enough information and template code to get you started. If you want to fill in gaps or learn more, there are plenty of resources online, starting with the documentation.

# Setting Up the Environment

## Python

First of all, make sure you have a version of Python installed on your machine. It’s recommended to use the latest version of Python3, but most 3.x.y versions should work. You can get the latest version of Python here: [https://www.python.org/downloads/](https://www.python.org/downloads/). 

Alternatively, you can use `pyenv` ([https://realpython.com/intro-to-pyenv/](https://realpython.com/intro-to-pyenv/)) to easily manage multiple Python versions on your machine. 

## Create a Project Directory

Create a new directory to house your project, and cd into it.

```bash
$ mkdir django-todo-backend
$ cd django-todo-backend
```

## Virtualenv

When developing a new Python project, it is best practice to create a *virtual environment*. This helps keep the dependencies needed by the project isolated and separate from your system (and other projects) dependencies.

You can install `virtualenv` with pip and check the installation.

```bash
$ pip install virtualenv
$ virtualenv --version
```

Next, we will create the virtualenv for our project. From within the directory you created in the last step, run the following command. 

```bash
$ virtualenv venv --python=python3.7
```

<aside>
⚠️ Specify whichever version of Python you want to use/have installed on your machine. If you don’t have 3.7 installed `--python=python3.7` won’t work.

</aside>

This will create a virtualenv folder in our project directory called `venv` using Python 3.7. It’s generally a standard practice to name your virtualenv `venv`, but you can name it as you like. There should now be a `venv` folder in your directory:

![Screen Shot 2022-01-08 at 11.01.36 AM.png](Django%20DRF%20Tutorial/Screen_Shot_2022-01-08_at_11.01.36_AM.png)

Now we can boot up our virtual environment. 

```bash
# for linux or macOS
source venv/bin/activate

# for windows
venv\Scripts\activate.bat
```

You should now see your environment name in parenthesis on the left side of your terminal:

 

![Screen Shot 2022-01-08 at 11.02.08 AM.png](Django%20DRF%20Tutorial/Screen_Shot_2022-01-08_at_11.02.08_AM.png)

To deactivate the environment. 

```bash
# for linux, macOS,a nd windows
(venv) $ deactivate
```

<aside>
⚠️ We can demonstrate our virtualenv at work with the `pip freeze` command, which lists all of the libraries currently installed. For example, if I’m not running my virtual environment and run the command it will list all of the packages I have installed on my system:

![Screen Shot 2022-01-08 at 11.11.24 AM.png](Django%20DRF%20Tutorial/Screen_Shot_2022-01-08_at_11.11.24_AM.png)

Now if I activate my venv and run freeze again:

![Screen Shot 2022-01-08 at 11.14.35 AM.png](Django%20DRF%20Tutorial/Screen_Shot_2022-01-08_at_11.14.35_AM.png)

As you can see, the different environments have different packages installed. If I install another package while running `venv` it will only be installed into that environment, keeping my system packages clean.

</aside>

## Install Django

You can install an official release of Django with pip. **Make sure you are in your virtual environment when installing it.**

```bash
$ python -m pip install Django
```

# Creating a Django Project

## Initialize a Project

Now that our development environment is set up, we can initialize our Django project. This will auto-generate a code base for us to start with.

```bash
(venv) $ django-admin startproject todo_backend
```

This will create a `todo_backend` directory in our project directory. We can cd into this new directory and take a look at what was created.

![Screen Shot 2022-01-08 at 11.45.01 AM.png](Django%20DRF%20Tutorial/Screen_Shot_2022-01-08_at_11.45.01_AM.png)

For more details on what each of these files are, check out the [official documentation](https://docs.djangoproject.com/en/4.0/intro/tutorial01/#creating-a-project). For now, suffice to say these are the basic settings and functions needed for a Django project. The `manage.py` is a command-line utility that manages many aspects of the Django project, as we will soon see. It’s usually best to work from the directory containing this file, so we can quickly run functions without having to specify the path to it, i.e.

```bash
(venv) $ python manage.py some_django_command
# vs.
(venv) $ python ../../path/to/manage.py some_django_command
```

## Create an App

In Django, the “project” is a collection of configurations and apps that define a particular website. An app is a web application that actually does something like a blog system, or in our case, a todo app. 

We can easily create a new app for our todo project. We will call it `api` as it will serve as the API for our final product. 

```bash
(venv) $ python manage.py startapp api
```

This will create a new `api` directory in our project. 

![Screen Shot 2022-01-09 at 9.21.55 PM.png](Django%20DRF%20Tutorial/Screen_Shot_2022-01-09_at_9.21.55_PM.png)

Next, we can include our app in our project by adding it to the list of installed apps in our project settings. Open the Django project in your preferred IDE or editor, open `todo_backend/settings.py` , and the todo app to `INSTALLED_APPS`:

```python
# Application definition

INSTALLED_APPS = [
		"api",
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

# Database Setup

By default, Django will set up the project to use a SQLite database. This is perfectly fine for development or prototype projects like a hackathon, but **it is not suitable for a production environment.** This means for your 401 projects, you will want to configure your project to use a more appropriate and scalable database, like PostgreSQL. We won’t cover this in this tutorial, but you can refer to the Django documentation for steps on how to do this. 

## Migrations

If you look at the `todo_backend/settings.py` file you will see a list of apps that were installed in our project when we first initialized it.

```python
# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

Some of these applications require a database table, so we need to initialize our database by running the *migrations*. A migration is essentially a file that defines changes to your database schema (e.g. adding a field to a table, renaming a table, etc.). Django will translate the changes to SQL statements and, run them against your database. We can run these initial migrations with the following command

```bash
(venv) $ python manage.py migrate
```

and we should see a list of migrations that were applied:

![Screen Shot 2022-01-08 at 12.30.36 PM.png](Django%20DRF%20Tutorial/Screen_Shot_2022-01-08_at_12.30.36_PM.png)

There should now be a `db.sqlite3` file in our working directory that contains the necessary tables:

![Screen Shot 2022-01-08 at 12.32.54 PM.png](Django%20DRF%20Tutorial/Screen_Shot_2022-01-08_at_12.32.54_PM.png)

## Creating a Model

Let’s create a model for a todo item. This will essentially define a new table for our database. Open `api/models.py`  and edit it so it looks like the following:

```python
from django.db import models

class Todo(models.Model):
    title = models.CharField(max_length=120)
    description = models.TextField()
    completed = models.BooleanField(default=False)
```

This defines a new model called **Todo** that has three fields: **title, description, complete.**

Since we’ve defined a new model, we need to create and run a new migration to update our database. With Django, this is easily achieved with the command

```python
(venv) $ python manage.py makemigration api
```

You should see an output like this:

![Screen Shot 2022-01-08 at 12.52.58 PM.png](Django%20DRF%20Tutorial/Screen_Shot_2022-01-08_at_12.52.58_PM.png)

<aside>
⚠️ If we look at `api/migartions/0001_initial.py` we can see how Django defines the migration:

```python
# ...
operations = [
        migrations.CreateModel(
            name='Todo',
            fields=[
                ('id', models.BigAutoField(auto_created=True, primary_key=True, serialize=False, verbose_name='ID')),
                ('title', models.CharField(max_length=120)),
                ('description', models.TextField()),
                ('completed', models.BooleanField(default=False)),
            ],
        ),
    ]
```

We can look at the generated SQL statements by running 

```bash
(venv) $ python manage.py sqlmigrate api 0001
```

which should output the following:

```sql
BEGIN;
--
-- Create model Todo
--
CREATE TABLE "api_todo" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "title" varchar(120) NOT NULL, "description" text NOT NULL, "completed" bool NOT NULL);
COMMIT;
```

</aside>

Now we can run the migration as before. 

```bash
(venv) $ python manage.py migrate
```

If we check the tables in our database again, we should see the new todo table:

![Screen Shot 2022-01-09 at 9.45.21 PM.png](Django%20DRF%20Tutorial/Screen_Shot_2022-01-09_at_9.45.21_PM.png)

# Django Admin

## Create a Superuser

Let’s take a look at the Django Administration site now. First, we need to create a new admin user. Run the following command and follow the onscreen prompts. 

```bash
(venv) $ python manage.py createsuperuser
```

Now let’s start up the development server

```bash
(venv) $ python manage.py runserver
```

This will start a development server, where we can view our site at [http://127.0.0.1:8000/](http://127.0.0.1:8000/). 

<aside>
⚠️ This server is fine for local development, **but it is not suitable for a production environment**. For your CMPUT 401 projects, you will have to use something like gunicorn and nginx to deploy the final version of your MVP.

</aside>

Open your browser to [http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/) and you should see a login form:

![Screen Shot 2022-01-08 at 3.44.23 PM.png](Django%20DRF%20Tutorial/Screen_Shot_2022-01-08_at_3.44.23_PM.png)

Enter the credentials you specified in the step above to log in. You should be greeted with the Django admin index page:

![Screen Shot 2022-01-08 at 3.45.27 PM.png](Django%20DRF%20Tutorial/Screen_Shot_2022-01-08_at_3.45.27_PM.png)

From here, site administrators can easily interact with and manage the models and project data. This is one of the great features of Django - we have a fully functional administration site, complete with an API to manage our data, in just a few terminal commands. Without the use of any frameworks, something like this would require substantial development time.

## Register a Model

Notice that our todo model isn’t accessible from the admin page. We need to register the model first. To do this, open the `api/admin.py` and add the following:

```python
from django.contrib import admin

from .models import Todo

admin.site.register(Todo)
```

Head back to the browser and refresh the page. You should now see the todo app:

![Screen Shot 2022-01-09 at 9.49.12 PM.png](Django%20DRF%20Tutorial/Screen_Shot_2022-01-09_at_9.49.12_PM.png)

Click the ‘add’ button to navigate to the creation form and create a new todo item:

![Screen Shot 2022-01-09 at 9.50.07 PM.png](Django%20DRF%20Tutorial/Screen_Shot_2022-01-09_at_9.50.07_PM.png)

After you save the new item, you should see a list of the todo items currently in the database:

![Screen Shot 2022-01-09 at 9.51.00 PM.png](Django%20DRF%20Tutorial/Screen_Shot_2022-01-09_at_9.51.00_PM.png)

Notice how the item in our list is not very descriptive - it simply tells us that it is a Todo object. Let’s fix this by adding a method to our Todo model. Open `api/models.py` again, and edit it so it looks as follows:

```python
from django.db import models

class Todo(models.Model):
    title = models.CharField(max_length=120)
    description = models.TextField()
    completed = models.BooleanField(default=False)
    
    def __str__(self):
        return self.title
```

The `__str__` method tells Django what to print for the representation of that object. You can add any type of method to a model just as you would for any other Python class (see Django docs for examples).

<aside>
⚠️ We’ve made a change to our Todo model, so do we need to create and run another migration? In this case, no. We’ve only added a method to the model that changes is metadata. We haven’t changed the fields or definition of the model at all. If we were to have changed the title field to have `max_length=60` this would require a new migration, however.

</aside>

If you refresh the browser, you should see the new representation of our todo item:

![Screen Shot 2022-01-09 at 9.52.10 PM.png](Django%20DRF%20Tutorial/Screen_Shot_2022-01-09_at_9.52.10_PM.png)

<aside>
⚠️ There are many more default Django features and concepts that aren’t covered in this tutorial but may be very helpful for either your hackathon or client projects. You’re encouraged to review the documentation and tutorials at [https://www.djangoproject.com](https://www.djangoproject.com/) for more information. As always, if you have any questions, feel free to ask them on the course StackOverflow.

</aside>

# Django Rest Framework

Now that we’ve covered some Django basics and have our project and app set up, let’s start creating our API for the front end of our application to interact with. We’re going to use `djangorestframework` -  a powerful and flexible toolkit that makes it easy to set up RESTful Web APIs for Django.

## Install djangorestframework

You can install an official release of DRF with pip. **Make sure you are in your virtual environment when installing it.** 

```bash
$ python -m pip install djangorestframework
```

We have to add DRF to our list of installed apps, same as we did with our todo app:

```python
# Application definition

INSTALLED_APPS = [
    "api",
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework' # add this!
]
```

## Create a Serializer

Our REST api endpoint is going to return JSON data to the front end, so that it can work with and display the data. To do this, we need a *model serializer,* which will convert our Python model instance into JSON. Create a new file `api/serializers.py` and add the following code:

```python
from rest_framework import serializers
from .models import Todo

class TodoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Todo
        fields = ['id', 'title', 'description', 'completed']
```

This defines that the Todo model should be serialized by converting the id, title, description, and completed fields to JSON. 

## Creating the Endpoint (View)

Let’s now create the endpoint. In Django terms, these are referred to as *Views.* Open `api/views.py` and create a `TodoView` class as follows:

```python
from django.shortcuts import render
from rest_framework import viewsets
from .serializers import TodoSerializer
from .models import Todo

class TodoView(viewsets.ModelViewSet):
    serializer_class = TodoSerializer
    queryset = Todo.objects.all()
```

That’s it! By specifying the `ModelViewSet` we are inheriting the default DRF actions that correspond to the basic CRUD operations - create, read, update, and delete - in just a few lines of code. Just like with Django, DRF has the ability to abstract lots of boilerplate and otherwise redundant coding away, allowing for speedy development. You can of course write your own views functions and extend the default behaviors as needed. For more details, review [https://www.django-rest-framework.org/api-guide/viewsets/#modelviewset](https://www.django-rest-framework.org/api-guide/viewsets/#modelviewset).

## Routing

Now that we have an endpoint defined, we need to make it routable so we can access it with an HTTP client. Since we are using the viewset class for our endpoint, DRF can automatically generate the URL configuration for our API. We just have to register it with a router class. Open `todo_backend/urls.py` and edit it as follows:

```python
from django.contrib import admin
from django.urls import include, path
from rest_framework import routers
from api import views

router = routers.DefaultRouter()
router.register(r'todo', views.TodoView, 'todo')

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include(router.urls)),
]
```

This will register our TodoView at the route `/api/todo/`.

<aside>
⚠️ If you need more control over the URLs, you can use regular class-based views and set up the URL conf manually. For examples see [https://www.django-rest-framework.org/tutorial/quickstart/#urls](https://www.django-rest-framework.org/tutorial/quickstart/#urls) and [https://docs.djangoproject.com/en/4.0/intro/tutorial01/#write-your-first-view](https://docs.djangoproject.com/en/4.0/intro/tutorial01/#write-your-first-view).

</aside>

## Testing the API

With the development server running, navigate to [http://127.0.0.1:8000/api/todo/](http://localhost:8000/api/todo/). You should see the following:

![Screen Shot 2022-01-08 at 5.37.09 PM.png](Django%20DRF%20Tutorial/Screen_Shot_2022-01-08_at_5.37.09_PM.png)

From this route we can GET a list of all todo items we currently have, and fill out the form and POST to the endpoint to create a new item:

![Screen Shot 2022-01-08 at 5.38.50 PM.png](Django%20DRF%20Tutorial/Screen_Shot_2022-01-08_at_5.38.50_PM.png)

![Screen Shot 2022-01-08 at 5.39.46 PM.png](Django%20DRF%20Tutorial/Screen_Shot_2022-01-08_at_5.39.46_PM.png)

We can also navigate to  [http://127.0.0.1:8000/api/todo/<id>/](http://localhost:8000/api/todo/) to perform operations on a specific (existing) item. For example, from  [http://localhost:8000/api/todo/1/](http://localhost:8000/api/todo/1/) we can GET that single item, DELETE it, or PUT to update it. 

![Screen Shot 2022-01-08 at 5.42.21 PM.png](Django%20DRF%20Tutorial/Screen_Shot_2022-01-08_at_5.42.21_PM.png)

As you can see, DRF comes with a playground where we can easily interact with and test the API through a friendly UI - yet another benefit of this toolset.

<aside>
⚠️ There are other tools we can use to test our API that could be helpful for this and other courses.

1. Postman
    
    Another friendly UI application that can be installed on most OS. Just specify the request method and url and send it. A nicely formatted response will be displayed
    
    ![Screen Shot 2022-01-08 at 6.01.31 PM.png](Django%20DRF%20Tutorial/Screen_Shot_2022-01-08_at_6.01.31_PM.png)
    
2. curl
    
    This is a command line http client. Similarly, just specify the URL and request method and headers. Check out the manual (`curl —manual`) for more details. 
    
    ![Screen Shot 2022-01-08 at 6.04.50 PM.png](Django%20DRF%20Tutorial/Screen_Shot_2022-01-08_at_6.04.50_PM.png)
    
3. httpi
    
    Another command line tool, but requires Python 3.6 or newer. You can install it with
    
    ```bash
    $ python -m pip install httpie
    ```
    
    or homebrew, apt, etc. You can send requests with 
    
    ```bash
    $ http http://127.0.0.1:8000/api/todo/
    ```
    
    It has an added benifit of formatting the response:
    
    ![Screen Shot 2022-01-08 at 6.10.44 PM.png](Django%20DRF%20Tutorial/Screen_Shot_2022-01-08_at_6.10.44_PM.png)
    
</aside>

Now that we have a functioning API endpoint for our app, we can move on to developing the front end.
