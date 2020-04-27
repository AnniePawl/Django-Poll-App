## Views & URLS
- **Explain how server decides which page to show for given URL:** When a user requests a specific page, Django determines the root URLconf module to use. It loads that module and looks for the variable 'urlpatterns'. Then it runs thru each pattern and stops at the one that matches the requested URl against path_info. Finally, it calls a given view! 
- **Given URL configuration, write view funnction to correspond to a URL:**
```python
# book_site/urls.py 
from django.urls import include, path
urlpatterns = [
    path('', include('books.urls')),
    path('admin/', admin.site.urls),
]
book
# books/urls.py
from . import views
app_name = 'books'
urlpatterns = [
    path('', views.home, name='home'),
    path('book/<int:book_id>/', views.detail, name='detail'),
]
# books/view.py
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
- **Explain uses for class-based views and give examples of when class-based view would be preferred:**. In Django, views represnt the controller layer of the MVC pattern. They are required to accept an HttpRequest and return an HttpResponse. 
Django's built-in, class-based views are especialy great because they are reusable and have pre-configured functionalities. We might use one of Django's "generic views" as a starting point to save us work and time. If we have a more specific task in mind, we can always extend these classes. 


## Templates 
- **Given a URL configuration, use `reverse` function and `url` template tag to dynamically generate URLs:** We can use reverse function to "reverse-engineer" a URL from its name field and any parameters. 
```python
from django.http import HttpResponseRedirect
from django.urls import reverse

def redirect_to_year(request):
    # ...
    year = 2006
    # ...
    article_url = reverse('news-year-archive', args=[year])
    return HttpResponseRedirect(article_url)
```
- **Read and understand code using template filters:** Template filters allow us to compute or transnform any value in template itself, rather than the view. Multiple filters can be specified w/ pipes. Syntax: `{{ variable_name | filter}}`. 
```python
{% filter force_escape|lower %}
    This text will be HTML-escaped, and will appear in all lowercase.
{% endfilter %}
```
- **Organize templates within a project using template namespacing:** Sometimes using the name attribute by itself is not sufficient to classify urls. Django uses namespace attribute to allow group of urls to be identified with a unique qualifier: 
```python
from django.urls import include, path
urlpatterns = [
    path('',TemplateView.as_view(template_name='homepage.html'),name="homepage"),
    path('about/',include('coffeehouse.about.urls',namespace="about")),
]
```

## Authentication & Authorization 
- **Explain difference between Authentication and Authorization. Give examples of each:** Authentication verifies that a user is who they claim to be, and authorization determines what they have access to. Example: Bouncer at a club checks the photo on your id to verify that you and your id match(authentication). Then they check your date of birth to determine if you are old enough to get into the club (authorization). 
- **Use the `user` object to access information about logged-in user:** Django has built-in User model. Some of it's fields include: username, first_name, email, is_active, is_superuser, user_permissions. We can access logged-in user anywhere in views or templates with `request.user`. To limit access to a particular page to only logged-in users, we can use `@login_requrired` decorator for function-based views or `loginRequiredMixin` for class-based views. Example methods: `class models.User`<br>
`get_username()`, `get_full_name()`, `check_password(raw_password)`




