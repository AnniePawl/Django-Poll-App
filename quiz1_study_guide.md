1. **Start a new project:** `$ django-admin startproject mysite`
2. **Create a new app:** `$ python manage.py startapp appName`
3. **Create a superuser:** `$python3 manage.py createsuperuser`
4. **Explain how the server decides which page to show for a given URL:** When a user requests a specific page, Django determines the root URLconf module to use. It loads that module and looks for the variable 'urlpatterns'. Then it runs thru each pattern and stops at the one that matches the requested URl against path_info. Finally, it calls a given view! 
5. **Given a URL configuration, write a view function to correspond to a URL. book_site/urls.py** <br>
book_site/urls.py
```python
from django.urls import include, path
urlpatterns = [
    path('', include('books.urls')),
    path('admin/', admin.site.urls),
]
```
books/urls.py
```python
from . import views
app_name = 'books'
urlpatterns = [
    path('', views.home, name='home'),
    path('book/<int:book_id>/', views.detail, name='detail'),
]
```
books/view.py
```python
from .models import Book
def home(request):
    context = {
        'books': Book.objects.all()
    }
    return render(request, 'books/home.html', context)
def detail(request, book_id):
    context = {
        'book': Book.objects.get(id=book_id)
    }
    return render(request, 'books/detail.html', context)
```
6. **Given a scenario with real-world objects, write model definitions to correspond to those objects.**<br>
```python
from django.db import models

class Event(models.Model):
    name = models.CharField(max_length=100)
    date = models.DateTimeField()
    location = models.CharField(max_length=100)
    attendees = models.ManyToManyField(Attendee, through='Ticket')

def __str__(self):
    return self.name

class Attendee(models.Model):
    name = models.CharField(max_length=100)
    # Attendee could go to many events
    event = models.ManyToManyField(Event, through='Ticket')
    ticket = models.ForeignKey(Ticket, on_delete=models.CASCADE)

    def __str__(self):
    return self.name

class Ticket(models.Model):
    # Ticket has one event
    event = models.ForeignKey(Event, on_delete=models.CASCADE)
    # Ticket has one attendee
```
7. **Describe purpose of through field options:** “Through relationship” - Acts as mediating between two classes
Must request http response and return http response 
8. **Describe the purpose of model migrations and list the commands used to migrate your models.** Migrations are Djangos way of dealing with changes to models so that they reflect in your database scheme- like adding or deleting a new field. <br>
**Migration commands:** `$python manage.py makemigrations`, `$python manage.py migrate`



