Great! Let's start with the **Account Management** system for your eLearning project, which will include user registration, login, logout, and user profile management. We'll follow professional practices to ensure the structure is clean, maintainable, and scalable.

### 1. **Create a New Django App: `accounts`**
This app will manage user registration, login, and profile.

#### Step 1: Create the `accounts` app
Run the following command to create the app:
```bash
python manage.py startapp accounts
```

#### Step 2: Configure the app in `settings.py`
Add `accounts` to your `INSTALLED_APPS` list:
```python
# settings.py
INSTALLED_APPS = [
    ...
    'accounts',
    ...
]
```

#### Step 3: Custom User Model (Optional)
If you need additional fields for the user, such as a profile picture or bio, you can create a custom user model. Here's an example of extending the default user model.

```python
# accounts/models.py
from django.contrib.auth.models import AbstractUser
from django.db import models

class CustomUser(AbstractUser):
    # Add custom fields here
    bio = models.TextField(blank=True, null=True)
    profile_picture = models.ImageField(upload_to='profile_pictures/', null=True, blank=True)

    def __str__(self):
        return self.username
```

**Note:** If you choose to extend the user model, you need to update the `AUTH_USER_MODEL` in `settings.py`:

```python
# settings.py
AUTH_USER_MODEL = 'accounts.CustomUser'
```

After creating the model, run migrations:
```bash
python manage.py makemigrations accounts
python manage.py migrate
```

#### Step 4: Create Forms for Registration and Login
Now, let's create forms for user registration and login.

```python
# accounts/forms.py
from django import forms
from django.contrib.auth.forms import UserCreationForm, AuthenticationForm
from .models import CustomUser

class CustomUserCreationForm(UserCreationForm):
    class Meta:
        model = CustomUser
        fields = ['username', 'email', 'bio', 'profile_picture']

class CustomAuthenticationForm(AuthenticationForm):
    class Meta:
        model = CustomUser
        fields = ['username', 'password']
```

#### Step 5: Create Views for Registration, Login, and Logout

Now let's create the views to handle registration, login, and logout.

```python
# accounts/views.py
from django.shortcuts import render, redirect
from django.contrib.auth import login, authenticate, logout
from django.contrib.auth.forms import AuthenticationForm
from django.http import HttpResponse
from .forms import CustomUserCreationForm, CustomAuthenticationForm

# User Registration View
def register(request):
    if request.method == 'POST':
        form = CustomUserCreationForm(request.POST, request.FILES)
        if form.is_valid():
            user = form.save()
            login(request, user)
            return redirect('home')  # Redirect to a success page or home
    else:
        form = CustomUserCreationForm()
    return render(request, 'accounts/register.html', {'form': form})

# User Login View
def user_login(request):
    if request.method == 'POST':
        form = CustomAuthenticationForm(data=request.POST)
        if form.is_valid():
            user = authenticate(username=form.cleaned_data['username'], password=form.cleaned_data['password'])
            if user is not None:
                login(request, user)
                return redirect('home')  # Redirect to a success page or home
            else:
                return HttpResponse("Invalid credentials")
    else:
        form = CustomAuthenticationForm()
    return render(request, 'accounts/login.html', {'form': form})

# User Logout View
def user_logout(request):
    logout(request)
    return redirect('home')
```

#### Step 6: Create URL Routing for Accounts

Create a URL routing system for account-related views.

```python
# accounts/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('register/', views.register, name='register'),
    path('login/', views.user_login, name='login'),
    path('logout/', views.user_logout, name='logout'),
]
```

#### Step 7: Include `accounts/urls.py` in Main URL Configuration

In the `eLearning/urls.py`, include the URLs for the `accounts` app.

```python
# eLearning/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('accounts/', include('accounts.urls')),  # Add this line to include the account URLs
    path('', include('yourapp.urls')),  # Other app URLs
]
```

#### Step 8: Create HTML Templates for Registration and Login

Now, let's create the templates for `register.html` and `login.html`.

##### `register.html`
```html
<!-- templates/accounts/register.html -->
{% block content %}
  <h2>Register</h2>
  <form method="POST" enctype="multipart/form-data">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Register</button>
  </form>
{% endblock %}
```

##### `login.html`
```html
<!-- templates/accounts/login.html -->
{% block content %}
  <h2>Login</h2>
  <form method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Login</button>
  </form>
{% endblock %}
```

#### Step 9: Update `home` Page to Include User Information

You can display user information after a successful login on the homepage.

```python
# eLearning/views.py
from django.shortcuts import render

def home(request):
    return render(request, 'home.html', {'user': request.user})
```

##### `home.html`

```html
<!-- templates/home.html -->
{% block content %}
  <h1>Welcome, {{ user.username }}</h1>
  <a href="{% url 'logout' %}">Logout</a>
{% endblock %}
```

### 2. **Login, Logout, and User Profile Management**

- **Login:** Users log in using their username and password. If the credentials are correct, they will be redirected to the homepage (`home`).
- **Logout:** Users can log out by clicking the logout link, which will call the `user_logout` view and redirect them to the homepage.
- **User Profile:** After login, you can display a simple profile with the `user.username` and allow users to update their profile details (such as bio and profile picture).

### 3. **Styling the Application**

You can use Bootstrap or custom CSS to style the login, registration, and home pages.

**For example, add Bootstrap to your templates:**

```html
<!-- base template (base.html) -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}My eLearning App{% endblock %}</title>
    <link href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
    {% block content %}
    {% endblock %}
</body>
</html>
```

Then, extend the `base.html` in your `register.html` and `login.html`.

```html
{% extends 'base.html' %}
{% block content %}
  <div class="container">
    <h2>Register</h2>
    <form method="POST" enctype="multipart/form-data">
      {% csrf_token %}
      {{ form.as_p }}
      <button type="submit" class="btn btn-primary">Register</button>
    </form>
  </div>
{% endblock %}
```

### 3. **User Profile Page** (View and Edit Profile)

Now, let's implement the **User Profile** page where users can view and update their bio and profile picture.

#### Step 1: Create Profile Views
We will create a view to display and update the user's profile.

##### **views.py**
```python
# accounts/views.py
from django.shortcuts import render, redirect
from django.contrib.auth.decorators import login_required
from .forms import CustomUserCreationForm, CustomAuthenticationForm
from django.contrib.auth import login, authenticate
from .models import CustomUser
from .forms import CustomUserCreationForm, CustomAuthenticationForm, ProfileUpdateForm

# Profile View
@login_required
def profile(request):
    if request.method == 'POST':
        form = ProfileUpdateForm(request.POST, request.FILES, instance=request.user)
        if form.is_valid():
            form.save()
            return redirect('profile')
    else:
        form = ProfileUpdateForm(instance=request.user)

    return render(request, 'accounts/profile.html', {'form': form})
```

#### Step 2: Create the Profile Update Form
Create a form for updating user profile details like bio and profile picture.

##### **forms.py**
```python
# accounts/forms.py
from django import forms
from .models import CustomUser

class ProfileUpdateForm(forms.ModelForm):
    class Meta:
        model = CustomUser
        fields = ['bio', 'profile_picture']
```

#### Step 3: Create the Profile Template
Now, let's create the template to display and edit the profile.

##### **profile.html**
```html
{% extends 'base.html' %}
{% block content %}
  <div class="container">
    <h2>Update Profile</h2>
    <form method="POST" enctype="multipart/form-data">
      {% csrf_token %}
      {{ form.as_p }}
      <button type="submit" class="btn btn-primary">Update Profile</button>
    </form>
  </div>
{% endblock %}
```

#### Step 4: Update URLs for Profile Page
Add the URL for the profile page in your `accounts/urls.py`.

```python
# accounts/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('register/', views.register, name='register'),
    path('login/', views.user_login, name='login'),
    path('logout/', views.user_logout, name='logout'),
    path('profile/', views.profile, name='profile'),  # Profile page URL
]
```

#### Step 5: Add Profile Access to Home Page (Optional)
You can add a link to the user profile from the home page so the user can easily access it after logging in.

##### **home.html**
```html
{% extends 'base.html' %}
{% block content %}
  <div class="container">
    <h1>Welcome, {{ user.username }}</h1>
    <a href="{% url 'profile' %}" class="btn btn-info">View Profile</a>
    <a href="{% url 'logout' %}" class="btn btn-danger">Logout</a>
  </div>
{% endblock %}
```

### 4. **Permissions and Roles (Student vs. Teacher)**

To handle different types of users like **students** and **teachers**, we'll use Django's built-in groups and permissions.

#### Step 1: Define User Roles

In `models.py`, let's add roles for students and teachers using Django groups.

##### **models.py**
```python
# accounts/models.py
from django.db import models
from django.contrib.auth.models import AbstractUser, Group

class CustomUser(AbstractUser):
    bio = models.TextField(blank=True, null=True)
    profile_picture = models.ImageField(upload_to='profile_pictures/', null=True, blank=True)
    
    def set_student_role(self):
        group = Group.objects.get(name='Student')
        self.groups.add(group)

    def set_teacher_role(self):
        group = Group.objects.get(name='Teacher')
        self.groups.add(group)

    def __str__(self):
        return self.username
```

Run migrations to create and apply these changes:
```bash
python manage.py makemigrations
python manage.py migrate
```

#### Step 2: Add Role Assignment During Registration
During registration, let's assign users to the correct group based on the role they select.

##### **register.html**
Add a select option for choosing roles:
```html
<!-- Add this in your register form -->
<div class="form-group">
  <label for="role">Role</label>
  <select class="form-control" id="role" name="role">
    <option value="student">Student</option>
    <option value="teacher">Teacher</option>
  </select>
</div>
```

##### **views.py**
Modify the registration view to assign roles.

```python
# accounts/views.py
def register(request):
    if request.method == 'POST':
        form = CustomUserCreationForm(request.POST, request.FILES)
        role = request.POST.get('role', 'student')  # Default to 'student'
        if form.is_valid():
            user = form.save()
            if role == 'student':
                user.set_student_role()
            else:
                user.set_teacher_role()
            login(request, user)
            return redirect('home')  # Redirect to home or profile page
    else:
        form = CustomUserCreationForm()
    return render(request, 'accounts/register.html', {'form': form})
```

#### Step 3: Restrict Access Based on Roles
To ensure that only users with specific roles can access certain views, we can use decorators.

##### **views.py**
Restrict views to specific roles (e.g., only teachers can access the teacher dashboard):

```python
# accounts/views.py
from django.contrib.auth.decorators import login_required, user_passes_test

def teacher_required(user):
    return user.groups.filter(name='Teacher').exists()

@login_required
@user_passes_test(teacher_required)
def teacher_dashboard(request):
    # Logic for teacher dashboard
    return render(request, 'teacher_dashboard.html')
```

#### Step 4: Create Templates for Teacher and Student Dashboards
You can create two separate dashboards for students and teachers.

**Teacher Dashboard:**

```html
<!-- teacher_dashboard.html -->
{% extends 'base.html' %}
{% block content %}
  <div class="container">
    <h2>Teacher Dashboard</h2>
    <!-- Teacher specific content here -->
  </div>
{% endblock %}
```

**Student Dashboard:**

```html
<!-- student_dashboard.html -->
{% extends 'base.html' %}
{% block content %}
  <div class="container">
    <h2>Student Dashboard</h2>
    <!-- Student specific content here -->
  </div>
{% endblock %}
```

### 5. **Logout and Sessions**

We have already added logout functionality earlier. Make sure to test the logout by clicking on the logout button and confirming that it redirects the user to the home page after logging out.

```python
# accounts/views.py
def user_logout(request):
    logout(request)
    return redirect('home')
```

### 6. **Testing**

Before going to production, ensure you write unit tests for registration, login, logout, profile update, and permission-based access.

#### Example Test: Test User Registration

```python
# accounts/tests.py
from django.test import TestCase
from django.urls import reverse
from django.contrib.auth import get_user_model

class UserTests(TestCase):
    
    def test_user_registration(self):
        data = {
            'username': 'john_doe',
            'password1': 'password123',
            'password2': 'password123',
            'email': 'john@example.com',
        }
        response = self.client.post(reverse('register'), data)
        self.assertEqual(response.status_code, 302)  # Should redirect after successful registration
        self.assertEqual(get_user_model().objects.count(), 1)  # Ensure user is created
```

#### Run the Tests:
```bash
python manage.py test
```

### 7. **Deploying to Production**

After testing locally, it's time to deploy your Django application. Here are the steps to deploy to **Heroku**:

- Install `gunicorn`:
```bash
pip install gunicorn
```

- Create a `Procfile` in the root directory:
```bash
web: gunicorn eLearning.wsgi
```

