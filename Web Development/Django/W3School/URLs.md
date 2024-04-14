[Django URLs](https://www.w3schools.com/django/django_urls.php)

Create a file named `urls.py` in the same folder as the `views.py` file, and type this code in it:

`project/members/urls.py`:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('members/', views.members, name='members'),
]
```

The `urls.py` file you just created is specific for the `members` application. We have to do some routing in the root directory `project` as well. This may seem complicated, but for now, just follow the instructions below.

There is a file called `urls.py` on the `project` folder, open that file and add the `include` module in the `import` statement, and also add a `path()` function in the `urlpatterns[]` list, with arguments that will route users that comes in via `127.0.0.1:8000/`.

Then your file will look like this:

`project/my_tennis_club/urls.py`:

```python
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('', include('members.urls')),
    path('admin/', admin.site.urls),
]

```

If the server is not running, navigate to the `/my_tennis_club` folder and execute this command in the command prompt:

```python
python manage.py runserver
```

In the browser window, type `127.0.0.1:8000/members/` in the address bar.