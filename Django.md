# Django 📟

⌚ Last Updated on: 07/06/2021

***

#### Starting a `virtualenv`

* Good practice to have a **virtual environment** (an isolated python environment where your library installations are separated from your main system's installations ).
  * Installation of `virtualenv`: `sudo apt-get install python3-venv`
  * Once you make a working directory with your app's code(and you are inside of it)- you can create your `virtualenv`:  `python3 -m venv [NAME OF ENV]`
  * To start the `virtualenv`: `source env/bin/activate`
  * Now you can do all the `pip3`s you want and they will be isolated to this `virtualenv` while its active.
  * Once you are done working in this `virtualenv`: `deactivate`

#### Django Setup:

* To install the library:`pip3 install Django`
* To start project (initialize Django in a directory): `django-admin startproject [NAME OF PROJECT]`  (No dashes should be in the name).
  *  Inside of the `[NAME OF PROJECT DIR]`:
    * `__intit__.py` (Tells Python that this is a Python package)
    * `settings.py` (Project settings can be configured here)
    * `urls.py` (Where we will setup the mapping from certain URLs to where we send the user in our project)
    * `wsgi.py` (WSGI: how our Python app and webserver communicate. Can configure this communication here but default setup is provided.)
  * At the same directory level of this project folder:
    * `manage.py `(Allows us to run command-line commands)
* Start project: `python3 manage.py runserver` ==> Starts a debug website on 127.0.0.1:8000 or localhost:8000
* Within a project you can have multiple apps(e.g store section of the website would be its own "app"). You can also mix and match apps in different projects.
  * Command to create a new app: `python manage.py startapp [NAME OF APP]`
    * There will be a new folder at the same level as the project directory created from `django-admin startproject [NAME OF PROJECT]`.
    * It will have:
      * `__init__.py`
      * `admin.py`
      * `apps.py`
      * `migrations/__init__.py`
      * `models.py`
      * `tests.py`
      * `views.py`
    * You will also notice a `db.sqlite3` at the same level as the django-admin project.

#### Creating a custom page (no templating)

* In the `app/views.py`:

  * You will initially have `render` imported but it is also good to import `HttpResponse`.

  * Create a function in this file: 

    ```python
    def FunctionName(request):
        return HttpResponse("<p>I rather be using Flask.</p>")
    ```

  * Create a file in the app directory `app/urls.py`:

    * There will also be a project level urls.py in the project directory.

    * In the `app/urls.py` have the following (to route to the function above):

      ````python
      from django.urls import path
      from . import views # Current views.py in app/
      
      urlpatterns = [
          path('',views.FunctionName,name="Name") # Keep Name non-generic to lower collision chances
      ]

    * In the project level `urls.py`: 

    ```python
    ...
    # Add include
    from django.urls import path,include
    
    urlpatterns = [
        ...,
        path("Name/",include("app_dir_name.urls")) # Matched url cut from given url and then rest of the url passed in to the app's urls.py
    ]
    
    ```

    * Once we add this paths the debug/default website should stop showing up.
    * If you have `""` in the path(with an include()): then nothing gets matched/cut and the whole thing can get passed in to the app. Good idea to have `/admin` at start if you intend on doing this, so it can still get matched.

  * `python3 manage.py runserver` : should start the dev. server which you can interact it and enter `localhost:8000/Name` to check that the above works.

#### Templating & App "Registration"

* Rather than having to type all the HTML out in a `HttpResponse` object. We can and should use templates instead.

* We first need a templates directory in the app directory: `app/templates/app` [add the `app` at the end as per convention since Django will be looking for templates in other places as well]

  * Create your HTML files in this directory.

* We need to add this app to our project to get templates working/searched properly + for database usage: 

  * Get class name of this app in `app/apps.py` (e.g `AppConfig`)
  * Then go to the project's `settings.py`. And add the app (`apps_dir.apps.AppConfig,`) to the `INSTALLED_APPS` list. **Add an app here whenever you make one.**

* Use `django.shorcuts import render` => To return a template in a function(still returns a HTTP response in the background):

  ```python
  return render(request, 'dir_in_templates_for_app/index.html',[CONTEXT])
  ```

  * View needs to always return an HTTP response or exception.

* The `[CONTEXT]` should be a dictionary (can have lists of other dictionaries or direct data). This passes data into the template.

* Templating engine syntax:

  * For loop:

  ``` jinja2
  {% for [val] in [CONTEXT Variable]%}
  	...
  {% endfor %}
  ```

  * Variable access:

  ```jinja2
  <p>{{ [val].attribute }}</p>
  ```

  * If statements:

    ```jinja2
    {% if [val] %} # Check var. existsence
    ...
    {% else %}
    ...
    {% endif %}
    ```

  * Template inheritance (assume `base.html` is a valid HTML file) - share repeated HTML and updating stuff is a lot more quick since you have a single update point: 

    ```jinja2
    # In base.html
    <!DOCTYPE html>
    <html>
    ...
    <body>
    {% block content[can have unique name that matches in other html file?] %} {% endblock %}
    <body>
    ...
    ```

    ```jinja2
    # In another html file
    {% extends "app/base.html"%}
    {% block content %}
    	<UNIQUE HTML>
    {% endblock content %}
    # This will be inserted in the block content tag in base.html
    ```

* Static files (like Javascript files and CSS files) need to be in their own directory.

  * Make a `static` directory in the app directory(*not* project directory).
  * Make another app directory in `static` for the app.
  * At the start of your HTML template write: `{% load static %}`
  * In the `<head>` tag area, have a `<link href={% static 'app/[static_file_name]' %}>`

* Sometimes you might have to clear your browser cache or restart the server to see your most up to date changes 

* Hard coding routes in `<a href="/..">` to link to other pages of your website is not a good idea since you could change the routes in the future.

  * Use `<a href="{% url '[Name given to view in urls.py of the app]' %}">`
  * (HTML BREAK) `<a href="#"/>` is considered a "dead" link that takes you no where.

  



***

### 🥽🥼 Resources Used:

* https://www.djangoproject.com/start/ (*Django Tutorials* by `Django Software Foundation`)
* https://www.youtube.com/watch?v=UmljXZIypDc (*Python Django Tutorial* by`Corey Schafer`)