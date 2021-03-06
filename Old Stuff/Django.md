# Django 📟

⌚ Last Updated on: 07/10/2021

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
  
  * Want parameter variables in the URL that gets passed in? `path("<type:name>",views.FunctionName,...)` => In the `views.py` add a parameter named `name` to the function responsible for rendering this URL.

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

#### Admin Page Setup

* You have `website.com/admin` route with an admin page at the start. But you need to create a "super user" to actually use this page to manipulate various things on your website.
  * Command to create super user: `python3 manage.py createsuperuser`
    * Won't work => We need to create a database to store this user data.
    * We need to perform a database migration(allow us to apply changes to our database).
    * The first migration will create the database and add a bunch of default tables. => `python3 manage.py makemigration` (detects changes and prepares Django to update the database)
    * To apply the migrations: `python3 manage.py migrate`
    * Now if you re-run the superuser command it will run (it will allow you to enter some user data for the admin page).
* Add a new database/model to your admin view (go to app's `admin.py`) - allows you to manipulate/change/update it via the admin page:

```python
from .models import Class/Model_of_models.py

admin.site.register(Class/Model_of_models.pys)
```



#### Databases

* Django has its own ORM (object relational mapper) => allows us to access our database (whatever type/brand) in an object orientated way(no raw SQL). (Databases are represented as classes know as `models`)

  * Django has a pre-made user account model.

* In the app directory we will have a `models.py`:

  * Creating a new model/database:

  ```python
  class DatabaseName(models.Model):
      NewFieldInDatabase = models.Type(parameters)
      def __str__(self): # Not required
          return self.NewFieldInDatabase # Return a string representation of the object
  ```

  * Importing another database(one-to-many relationship - use a foreign key):

  ```python
  from django.contrib.auth.models import User
  
  # A User will have many of this:
  class ...:
      ...
      Field = models.ForeignKey(User, on_delete = models.CASCADE) # on_delete is behavior that should be intiated when the User gets deleted (in this we delete all its associated data)
  ```

  * Now we need to run migrations to create changes then apply them: 
    * `python3 manage.py makemigrations` (create change list)
    * `python3 manage.py sqlmigrate [APP NAME] [migration number generated from previous command]` (create SQL)
    * `python3 manage.py migrate` (apply changes)

* Querying the database:

  ```python
  from app.models import Class[from models.py of this app]
  
  [Model].objects.all() # Get all objects in database
  
  [Model].objects.first() # Get first object (tail/end version also exists)
  
  [Model].objects.filter(field='...') # Returns all objects that match given field value
  
  # This data can all be captured in a variable (use dot notation to access the objects attributes e.g obj.id , obj.pk)
  
  [Model].objects.get(id=...) # Get an object via its id
  
  Val = [Model](field1=...,field2=...,...) # Create an object (Also can set this to a variable)
  Val.save() # Save object to database
  
  
  [Model].[Other Model]_set.all() # Get all objects associated to another object in another model(one to many) (Other_model is lower case)
  
  [Model].[Other Model]_set.create(...fields..) # Create another object associated with the "main" object (no need to specify the foreign key field or save - done automatically) (Other_model is lower case)
  
  
  ```



#### Model Forms

* Go to your app directory and create a `forms.py`

```python
from django import forms
from .models import [Class Name]

class FormName(forms.ModelForm):
    class Meta:
        model = [MODEL NAME]
        fields = [
            'fields in MODEL'
        ]

```

* Go to your views function to add a form:

```python
import .forms import FormName
def ...():
	form = FormName(request.POST or None)
    if form.is_valid():
        form.save()
    context = {
        ...,
        "form":form
    }
```

* Inside your template:

```python
<form method="POST"> {% csrf_token %}
{{ form.as_p }}
<input type="submit" value= "Save"/>
</form>
```

#### User forms

* We can create classes and these classes can create forms for us. There is also a special user creation form(`from django.contrib.auth.forms import UserCreationForm`).

  ``` python
  def ...(): # View
      if request.method == "POST":
          form = UserCreationForm(request.POST)
          if form.is_valid():
            	form.save()
          	username = form.cleaned_data.get('username')
              messages.success(request,f'Account Created') # In django.contrib import messages [FLASH MESSAGE]
         		return redirect('blog-home') # Can add parameters here if URL has parameters
      else:
  		form = UserCreationForm()
  	return render(..,..,{'..':form})
  ```

  * In the template:

  ```html
  <form method="POST"> 
      
      {% csrf_token %}
      <fieldset >
          <legend>
              ...
          </legend>
          {{ form.as_p }}
      </fieldset>
      
      <button type="submit"> Sign Up
      </button>
      
  </form>
  ```

  * In `base.html` to add flash message support:

  ```jinja2
  {% if messages %}
  	// IN BOOTSTRAP
  	{% for messsage in messages %}
  		<div class="alertalert-{{% messaage.tag %}}"> {{% message%}}</div> 
  	{% endfor %}
  {% endif %}
  ```

  * In the app `forms.py` (Creating custom user authentication field):

  ```python
  from django import forms
  from django.contrib.auth.models import User
  from django.contrib.auth.forms import UserCreationForm
  
  class UserRegisterForm(UserCreationForm):
  	email = forms.EmailField()
  	class Meta:
  		model = User
  		fields = ['username','email','password1','password2']
  	
      # Use in place of UserCreationForm
  ```

  * crispy forms (styling Django forms):

    * Installation: `pip install django-crispy-forms`
    * Register in your django project's `settings.py`: 

    ```python
    INSTALLED_APPS = [
    'crispy_forms',
    ...
    ]
    ```

    ```python
    # Go to bottom of settings.py
    
    CRISPY_TEMPLATE_PACK = 'boostrap#' # Change the boostrap or any other CSS library.
    ```

    * Go to your login/register html page:

      ```jinja2
      // At the top
      {% extends ... %}
      {% load crispy_forms_tags %}
      
      // remove the _as_p and use {{ form|crispy }} to perform styling
      // Can style further but crispy is good enough
      ```



#### Login/Logout system

* Django provides views for logging in and logging out. `from django.contrib.auth import views as ...`

  * Add a new URLpattern(class-based views): 

    ```python
    path('login/',auth_views.LoginView.as_view(template_name="app_dir/login.html"),name="login"),
    path('logout/',auth_views.LogoutView.as_view(template_name="app_dir/logout.html"),name="logout"),
    ```

  * Still need to create templates for these views:

  ```jinja2
  {% extends ... %}
  ...
  {{ form|crispy}}
  ...
  ```

  * To change where users get redirected to when they are logging in:

  ```python
  # Go to project settings.py (bottom)
  
  LOGIN_REDIRECT_URL = '(NAME OF VIEW like blog-home)' 
  ```

  * Make a button redirect to the logout view.
  * How to respond to a logged in user(by adding a conditional):

  ```jinja2
  // In your HTML template
  {% if user.is_authenticated%}
  	...
  {% else %}
  	...
  {% endif %}
  ```

  * Adding restriction to certain routes if a user is not logged in:

  ```python
  # In view
  from django.contrib.auth.decorators import login_required
  
  @login_required
  def ...(request):
  	return render(..)
  	
  	
  # In your setttings.py (project) add
  LOGIN_URL = 'name of login view' # Redirected when a user is not authenticated
  ```

  * Check if a user is authenticated in a view:

  `if request.user.is_authenticated:`

***

### 🥽🥼 Resources Used:

* https://www.djangoproject.com/start/  (*Django Tutorials* by `Django Software Foundation`)
* https://www.youtube.com/watch?v=UmljXZIypDc (*Python Django Tutorial* by`Corey Schafer`)
* https://www.youtube.com/watch?v=6oOHlcHkX2U (*Try DJANGO Tutorial - 23 - Django Model Forms* by `CodingEntrepreneurs`)
* https://stackoverflow.com/questions/3644902/how-to-check-if-a-user-is-logged-in-how-to-properly-use-user-is-authenticated (*How to check if a user is logged in (how to properly use user.is_authenticated)?* by `[Rick](https://stackoverflow.com/users/378874/rick)`) 
* https://docs.djangoproject.com/en/3.2/topics/http/urls/ (Django URL Dispatcher Doc by `Django Software Foundation`)
* https://stackoverflow.com/questions/3209906/django-return-redirect-with-parameters (*Django return redirect() with parameters* by `[muntu](https://stackoverflow.com/users/383998/muntu)`)