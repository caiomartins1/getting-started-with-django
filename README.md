# Getting Started With Django Step By Step

## Set Up Your Development Environment3

### 1. Create a virtual environment to manage dependencies.

```bash
python3 -m venv venv
```

### 2. Next, you need to activate the virtual environment:

```bash
source venv/bin/activate
```

### 3. Now that you’ve created a virtual environment, it’s time to install Django.

```bash
pip install Django
```

Once you’ve set up the virtual environment and installed Django, you can now dive in to creating the application.

## Create a Django Project

### 1. Making sure you’re in the rp_portfolio directory, and you’ve activated your virtual environment, run the following command to create the project:

```bash
django-admin startproject personal_portfolio
```

### 2. Project Clean Up

Most of the work you do will be in that first personal_portfolio directory. To save having to cd through several directories each time you come to work on your project, it can be helpful to reorder this slightly by moving all the files up a directory.

While you’re in the _getting-started-with-django_ directory, run the following commands:

```bash
mv personal_portfolio/manage.py ./
mv personal_portfolio/personal_portfolio/* personal_portfolio
rm -r personal_portfolio/personal_portfolio/
```

### 3. Start the server

Make sure you cd into your _personal_portfolio_ directory, and run the following command:

```bash
python manage.py runserver
```

If you done everything correctly, when you access localhost:8000 you should see something like this:

![django homepage](https://files.realpython.com/media/Screenshot_2018-12-09_at_17.58.16.20be0c5d3f1e.png)

Congratulations, you’ve created a Django site!
The next step is to create apps so that you can add views and functionality to your site.

## Create a Django Application

### 1. To create the app, run the following command:

```bash
python manage.py startapp hello_world
```

### 2. Install the app in your project

Once you’ve created the app, you need to install it in your project. In _personal_portfolio/settings.py_, add the following line of code under _INSTALLED_APPS_:

<pre>
<code>INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles', 
    <strong>'hello_world',</strong>
]
</code></pre>

### 3. Create a view

Navigate to the `views.py` file in the `hello_world` directory. There’s already a line of code in there that imports render(). Add the following code:

<pre>
<code>from django.shortcuts import render
<strong>def hello_world(request):
    return render(request, 'hello_world.html', {})</strong>
</code></pre>

In this piece of code, you’ve defined a view function called hello_world(). When this function is called, it will render an HTML file called hello_world.html. That file doesn’t exist yet, but we’ll create it soon.

### 4. Create a template

Now that you’ve created the view function, you need to create the HTML template to display to the user. render() looks for HTML templates inside a directory called templates inside your app directory. Create that directory and subsequently a file named hello_world.html inside it:

```bash
mkdir hello_world/templates/
touch hello_world/templates/hello_world.html
```

Don't forget to add any HTML you want to your template.

You’ve now created a function to handle your views and templates to display to the user. The final step is to hook up your URLs so that you can visit the page you’ve just created. Your project has a module called `urls.py` in which you need to include a URL configuration for the `hello_world app`. Inside `personal_portfolio/urls.py`, add the following:

<pre>
<code>from django.contrib import admin
<strong>from django.urls import path, include</strong>

urlpatterns = [
    path('admin/', admin.site.urls),
    <strong>path('', include('hello_world.urls')),</strong>
]
</code></pre>

This looks for a module called `urls.py` inside the `hello_world application` and registers any URLs defined there. Whenever you visit the root path of your URL (localhost:8000), the `hello_world application’s` URLs will be registered. The `hello_world.urls` module doesn’t exist yet, so you’ll need to create it:

```bash
touch hello_world/urls.py
```

Inside this module, we need to import the path object as well as our app’s views module. Then we want to create a list of URL patterns that correspond to the various view functions. At the moment, we have only created one view function, so we need only create one URL:

<pre>
<code>from django.urls import path
from hello_world import views

urlpatterns = [
    path('', views.hello_world, name='hello_world'),
]</code>
</pre>

Now, when you restart the server and visit localhost:8000, you should be able to see the HTML template you created:

![Hello World Template](https://files.realpython.com/media/Screenshot_2018-12-09_at_17.57.22.f3c9ea711bd4.png)

Congratulations, again! You’ve created your first Django app and hooked it up to your project. The only problem now is that it doesn’t look very nice. In the next section, we’re going to add bootstrap styles to your project to make it prettier!

## Add Bootstrap to Your App

Create another directory called templates, this time inside personal_portfolio, and a file called base.html, inside the new directory:

```bash
mkdir personal_portfolio/templates/
touch personal_portfolio/templates/base.html
```

We create this additional templates directory to store HTML templates that will be used in every Django app in the project. As you saw previously, each Django project can consist of multiple apps that handle separated logic, and each app contains its own templates directory to store HTML templates related to the application.

This application structure works well for the back end logic, but we want our entire site to look consistent on the front end. Instead of having to import Bootstrap styles into every app, we can create a template or set of templates that are shared by all the apps. As long as Django knows to look for templates in this new, shared directory it can save a lot of repeated styles.

Inside this new file (personal_portfolio/templates/base.html), add the following lines of code:

```html
{% block page_content %} {% endblock %}
```

Now, in hello_world/templates/hello_world.html, we can extend this base template:

```<pre><code>
{% extends "base.html" %}

{% block page_content %}
  <h1>Hello, World!</h1>
{% endblock %}
```
