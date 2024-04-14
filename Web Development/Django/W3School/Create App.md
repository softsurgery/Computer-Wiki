[Django Create App](https://www.w3schools.com/django/django_create_app.php)

# What is an App?

An app is a web application that has a specific meaning in your project, like a home page, a contact form, or a members database.

In this tutorial we will create an app that allows us to list and register members in a database.

But first, let's just create a simple Django app that displays "Hello World!".

# Create App :

I will name my app `members`.

Start by navigating to the selected location where you want to store the app and run the command below.

If the server is still running, and you are not able to write commands, press [CTRL] [BREAK], or [CTRL] [C] to stop the server and you should be back in the virtual environment.

```bash
python manage.py startapp members
```

Django creates a folder named `members` in my project, with this content:

```
project
    manage.py
    project/
    members/
        migrations/
            __init__.py
        __init__.py
        admin.py
        apps.py
        models.py
        tests.py
        views.py
```

First, take a look at the file called `views.py`.

This is where we gather the information we need to send back a proper response.