+++
draft = false
date = 2019-06-09
title = "Django Notes"
description = "Some notes for Django"
tags = ["Web Development", "Frameworks"]
+++

* [Start the Django project (foo as the project name and bar as the app name)](#start-the-django-project-foo-as-the-project-name-and-bar-as-the-app-name)
* [The admin site](#the-admin-site)
* [The Django shell (Control-d to exit)](#the-django-shell-control-d-to-exit)
* [Making pages](#making-pages)
* [References](#references)

## Start the Django project (foo as the project name and bar as the app name)

1. Create a virtual environment (where we can install packages and isolate them from all other packages): (venv is the virtual environment module.)

    ```sh
    python3 -m venv foo
    ```

2. Activate the virtual environment: (Packages inside will be available only when the environment is active.) (use `deactivate` to deactivate.)

    ```sh
    source foo/bin/activate
    ```

3. Install Django (Note: use `python` and `pip` instead of `python3` or `pip3`) in the virtual environment.

    ```sh
    pip install django
    ```

4. Start the project. (The dot at the end creates the project with a directory structure, i.e. with `__init__.py`, `settings.py`, `urls.py`, and `wsgi.py` (web server gateway interface.))

    ```sh
    django-admin startproject foo .
    ```

5. Create the database (SQLite): (Run the command anytime we modify the database.)

    ```sh
    python manage.py migrate
    ```

6. Start and view the project:

    ```sh
    python manage.py runserver
    ```

7. A Django project is a group of individual apps that work together. Let's start an app: （creating the following in the bar folder: `__init__.py`, `admin.py`, `apps.py`, `migrations`, `models.py`, `test.py`, and `view.py`.）

    ```sh
    python manage.py startapp bar
    ```

8. Code-wise, a model is just a class. An example model:

    ```python
    from django.db import models

    # Create your models here.
    class Topic(models.Model):
        """The class that manages the topic the user is learning about."""
        text = models.CharField(max_length=200)
        date_added = models.DateTimeField(auto_now_add=True)

        def __str__(self):
            """The string representation of the model."""
            return self.text
    ```

9. Activate the models:
    * Add the apps to `INSTALLED_APPS` in `settings.py`;

        ```python
        INSTALLED_APPS = [
            # My apps
            'bar',

            # Default django apps
            'django.contrib.admin',
            'django.contrib.auth',
            'django.contrib.contenttypes',
            'django.contrib.sessions',
            'django.contrib.messages',
            'django.contrib.staticfiles',
        ]
        ```

    * Migrate:

        ```sh
        python manage.py makemigrations bar
        python manage.py migrate
        ```

## The admin site

1. Add a superuser: (Django only stores the hash of the password)

    ```sh
    python manage.py createsuperuser
    ```

2. Register the model with the admin site. Example `admin.py`:

    ```python
    from django.contrib import admin

    # Register your models here.

    from .models import Topic, Entry # . means looking for the file in the same directory as admin.py

    admin.site.register(Topic)
    admin.site.register(Entry)
    ```

## The Django shell (Control-d to exit)

1. Example:

    ```sh
    python manage.py shell
    >>> from bar.models import Topic
    >>> Topic.objects.all() # returns a queryset
    ```

## Making pages

1. Defining URLs:
    * Include the URLs in foo/urls.py:

        ```python
        from django.contrib import admin
        from django.urls import path, include

        urlpatterns = [
            path('admin/', admin.site.urls),
            path('', include('bar.urls')),
        ]
        ```

    * Create a new bar/urls.py file and add the following:

        ```python
        from django.urls import path

        from . import views

        app_name = 'bar'
        urlpatterns = [ # a list of individual pages that can be requested.
            # Homepage
            path('', views.index, name='index'), # '' means the base URL; call the index() function in views.py
        ]
        ```

2. Writing views:

    ```python
    from django.shortcuts import render

    def index(request):
        """The homepage."""
        return render(request, 'bar/index.html')
    ```

3. Writing templates:
    * Make the directory bar/templates/bar
    * Write the file index.html:

        ```html
        <p>Learning Log</p>

        <p>Learning Log helps you keep track of your learning, for any topic you're learning about.</p>
        ```

    * If we see the error: `ModuleNotFoundError: No module named 'learning_logs.urls'`, just CTRL-C and rerun the server by `python manage.py runserver`.
    * Template inheritance:
        * Make a base.html file and include it on every page: (template tag:{percent percent}; bar is the namespace defined in bar/urls.py app_name) (in reality, use **%** instead of percent! here is just for avoiding the markdown error.)

            ```html
            <p>
                <a href="{percent url 'bar:index' percent}">Foo</a>
            </p>

            {percent block content percent}{percent endblock content percent}
            ```

        * Inherit the base.html: (index.html)

            ```html
            {percent extends "bar/base.html" percent}

            {percent block content percent}
            <p>Learning Log helps you become a better learner by keeping track of the topic you're learning.</p>
            {percent endblock content percent}
            ```


## References

* [Python Crash Course, 2nd Edition: A Hands-On, Project-Based Introduction to Programming](https://www.amazon.com/Python-Crash-Course-2nd-Edition/dp/1593279280/ref=sr_1_1?keywords=python+crash+course&qid=1558808134&s=gateway&sr=8-1)