[Medium](https://medium.com/@chodvadiyasaurabh/creating-interactive-api-documentation-with-swagger-ui-in-django-53fa9e9898dc)
# Introduction

In the world of web development, having comprehensive and interactive API documentation is crucial for smooth integration and collaboration. Django, a popular web framework for Python, provides an easy way to achieve this through Swagger UI integration. In this blog, we’ll explore how to create Swagger UI in Django, empowering developers to interact with your API seamlessly. Let’s dive into the process of building interactive API documentation that streamlines the development process and fosters better communication.

## **Step 1: Set Up a Django Project**

Before we start integrating Swagger UI, ensure you have Django installed. Create a new Django project and an app to work with:
```bash
django-admin startproject myproject  
cd myproject  
python manage.py startapp myapp
```

## **Step 2: Install Required Packages**

To integrate Swagger UI, install the necessary packages using pip:

```bash
pip install drf-yasg
```

## **Step 3: Configure Django**

Settings In your Django project settings (settings.py), add the following configurations for DRF-YASG:
```python
INSTALLED_APPS = [  
    # Other apps...  
    'django.contrib.staticfiles',  
    'drf_yasg',  
]  
  
SWAGGER_SETTINGS = {  
    'SECURITY_DEFINITIONS': {  
        'Basic': {  
            'type': 'basic'  
        }  
    }  
}
```
## **Step 4: Create API Views and URLs**

In your Django app (myapp/views.py), create some sample API views:
```python
from rest_framework.decorators import api_view  
from rest_framework.response import Response  
  
@api_view(['GET'])  
def hello_world(request):  
    return Response({'message': 'Hello, World!'})
```

In your app’s urls.py, add the URL configuration for the API views:
```python
from django.urls import path  
from .views import hello_world  
  
urlpatterns = [  
    path('hello/', hello_world, name='hello-world'),  
]
```
## **Step 5: Generate Swagger UI Documentation**

Now, let’s generate the Swagger UI documentation for your API. In your Django project’s urls.py, include the Swagger UI URLs using DRF-YASG:
```python
from django.contrib import admin  
from django.urls import path, include  
from rest_framework import permissions  
from drf_yasg.views import get_schema_view  
from drf_yasg import openapi  
  
schema_view = get_schema_view(  
    openapi.Info(  
        title="My API",  
        default_version='v1',  
        description="My API description",  
        terms_of_service="https://www.example.com/terms/",  
        contact=openapi.Contact(email="contact@example.com"),  
        license=openapi.License(name="Awesome License"),  
    ),  
    public=True,  
    permission_classes=(permissions.AllowAny,),  
)  
  
urlpatterns = [  
    path('admin/', admin.site.urls),  
    path('api/', include('myapp.urls')),  
    path('swagger/', schema_view.with_ui('swagger', cache_timeout=0), name='schema-swagger-ui'),  
    path('redoc/', schema_view.with_ui('redoc', cache_timeout=0), name='schema-redoc'),  
]

```
## **Step 6: Run the Development Server**

With everything set up, run the Django development server:

```bash
python manage.py runserver
```

## **Step 7: Explore the Swagger UI Documentation**

Now that your development server is running, You'll see the Swagger UI documentation showcasing your API's endpoints, parameters, and response schemas. You can explore and interact with your API directly from the Swagger UI, simplifying the development and testing process.

## **Conclusion:**
Congratulations! You’ve successfully integrated Swagger UI into your Django project, creating interactive API documentation that enhances collaboration and simplifies API integration. With Swagger UI, your API becomes more accessible, and developers can effortlessly explore and understand its functionalities. As you continue to develop your Django project, keep updating the Swagger UI documentation to ensure it remains up-to-date and serves as a valuable resource for your team and clients.

Swagger UI is a powerful tool that bridges the gap between API developers and consumers, fostering a smoother development experience. By providing detailed and interactive API documentation, you set your Django project up for success and empower users to unlock its full potential. Happy coding!