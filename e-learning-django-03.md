### 1. **Course Model**: Organizing Course Data

We will create a flexible model for courses. A course can have multiple lessons, modules, and assignments. Each course can also belong to a teacher or be assigned to a student for enrollment.

#### **Step 1: Create the Course Model**

##### **models.py**
```python
# courses/models.py
from django.db import models
from django.contrib.auth import get_user_model
from django.urls import reverse

# Course Model
class Course(models.Model):
    title = models.CharField(max_length=200)
    description = models.TextField()
    instructor = models.ForeignKey(get_user_model(), on_delete=models.CASCADE, related_name='courses')
    start_date = models.DateTimeField()
    end_date = models.DateTimeField()
    max_enrollment = models.IntegerField(default=50)  # Max students per course
    image = models.ImageField(upload_to='courses/', null=True, blank=True)
    
    def __str__(self):
        return self.title

    def get_absolute_url(self):
        return reverse('course_detail', kwargs={'pk': self.pk})

# Enrollment Model (Many-to-Many relationship between User and Course)
class Enrollment(models.Model):
    student = models.ForeignKey(get_user_model(), on_delete=models.CASCADE, related_name='enrollments')
    course = models.ForeignKey(Course, on_delete=models.CASCADE, related_name='enrollments')
    date_enrolled = models.DateTimeField(auto_now_add=True)

    class Meta:
        unique_together = ('student', 'course')
```

In this setup:
- **Course Model**: Contains all the details for a course (title, description, instructor, dates, etc.).
- **Enrollment Model**: Establishes a Many-to-Many relationship between students and courses.

Run the migration commands:
```bash
python manage.py makemigrations
python manage.py migrate
```

#### **Step 2: Creating Course Forms for Admin and Public Use**

We'll create forms for adding new courses and for enrolling students.

##### **forms.py**
```python
# courses/forms.py
from django import forms
from .models import Course, Enrollment

class CourseCreateForm(forms.ModelForm):
    class Meta:
        model = Course
        fields = ['title', 'description', 'instructor', 'start_date', 'end_date', 'max_enrollment', 'image']

class EnrollmentForm(forms.ModelForm):
    class Meta:
        model = Enrollment
        fields = []
```

#### **Step 3: Creating Views for Courses**

Let's set up views for creating, viewing, and enrolling in courses.

##### **views.py**
```python
# courses/views.py
from django.shortcuts import render, get_object_or_404, redirect
from django.contrib.auth.decorators import login_required
from .models import Course, Enrollment
from .forms import CourseCreateForm, EnrollmentForm

# Course Detail View
def course_detail(request, pk):
    course = get_object_or_404(Course, pk=pk)
    is_enrolled = course.enrollments.filter(student=request.user).exists()
    return render(request, 'courses/course_detail.html', {'course': course, 'is_enrolled': is_enrolled})

# Enroll in a Course
@login_required
def enroll_in_course(request, pk):
    course = get_object_or_404(Course, pk=pk)
    
    if course.enrollments.count() < course.max_enrollment:
        enrollment, created = Enrollment.objects.get_or_create(student=request.user, course=course)
        if created:
            return redirect('course_detail', pk=pk)
    return redirect('course_list')  # Redirect to course list if enrollment fails

# Course List View
def course_list(request):
    courses = Course.objects.all()
    return render(request, 'courses/course_list.html', {'courses': courses})

# Create New Course (Instructor Only)
@login_required
def create_course(request):
    if request.user.is_staff:  # Only allow instructors/admins
        if request.method == 'POST':
            form = CourseCreateForm(request.POST, request.FILES)
            if form.is_valid():
                form.save()
                return redirect('course_list')
        else:
            form = CourseCreateForm()
        return render(request, 'courses/course_create.html', {'form': form})
    return redirect('home')
```

#### **Step 4: Setting Up URLs**

Create URLs to link to all the above views.

##### **urls.py**
```python
# courses/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.course_list, name='course_list'),
    path('course/<int:pk>/', views.course_detail, name='course_detail'),
    path('course/<int:pk>/enroll/', views.enroll_in_course, name='enroll_in_course'),
    path('course/create/', views.create_course, name='create_course'),
]
```

### 2. **Templates** for Courses

Let's build some professional templates for course listing, course detail, and course creation.

#### **course_list.html**
```html
{% extends 'base.html' %}

{% block content %}
  <div class="container">
    <h2>Courses</h2>
    <div class="row">
      {% for course in courses %}
        <div class="col-md-4">
          <div class="card mb-4">
            <img src="{{ course.image.url }}" class="card-img-top" alt="{{ course.title }}">
            <div class="card-body">
              <h5 class="card-title">{{ course.title }}</h5>
              <p class="card-text">{{ course.description|truncatewords:20 }}</p>
              <a href="{% url 'course_detail' pk=course.pk %}" class="btn btn-primary">View Details</a>
            </div>
          </div>
        </div>
      {% endfor %}
    </div>
  </div>
{% endblock %}
```

#### **course_detail.html**
```html
{% extends 'base.html' %}

{% block content %}
  <div class="container">
    <h2>{{ course.title }}</h2>
    <p><strong>Instructor:</strong> {{ course.instructor }}</p>
    <p>{{ course.description }}</p>
    <p><strong>Start Date:</strong> {{ course.start_date }}</p>
    <p><strong>End Date:</strong> {{ course.end_date }}</p>
    
    {% if not is_enrolled %}
      <a href="{% url 'enroll_in_course' pk=course.pk %}" class="btn btn-success">Enroll Now</a>
    {% else %}
      <button class="btn btn-success" disabled>Enrolled</button>
    {% endif %}
  </div>
{% endblock %}
```

#### **course_create.html**
```html
{% extends 'base.html' %}

{% block content %}
  <div class="container">
    <h2>Create a New Course</h2>
    <form method="POST" enctype="multipart/form-data">
      {% csrf_token %}
      {{ form.as_p }}
      <button type="submit" class="btn btn-primary">Create Course</button>
    </form>
  </div>
{% endblock %}
```

### **Advanced Feature 1: Quizzes and Assignments**

This feature will allow instructors to create quizzes and assignments for students, while students can attempt them and get evaluated.

#### **Step 1: Create Models for Quiz and Assignment**

We'll need a model for quizzes and assignments that will be associated with a specific course.

##### **models.py**
```python
# courses/models.py (Add the following models for quizzes and assignments)
from django.db import models
from django.contrib.auth import get_user_model

class Quiz(models.Model):
    title = models.CharField(max_length=200)
    course = models.ForeignKey(Course, on_delete=models.CASCADE, related_name='quizzes')
    due_date = models.DateTimeField()
    
    def __str__(self):
        return self.title

class Question(models.Model):
    quiz = models.ForeignKey(Quiz, on_delete=models.CASCADE, related_name='questions')
    question_text = models.TextField()
    
    def __str__(self):
        return self.question_text

class Answer(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE, related_name='answers')
    answer_text = models.TextField()
    is_correct = models.BooleanField(default=False)
    
    def __str__(self):
        return self.answer_text

class Assignment(models.Model):
    course = models.ForeignKey(Course, on_delete=models.CASCADE, related_name='assignments')
    title = models.CharField(max_length=200)
    description = models.TextField()
    due_date = models.DateTimeField()
    
    def __str__(self):
        return self.title
```

#### **Step 2: Create Forms for Quiz and Assignment**

We'll need forms for instructors to create quizzes and assignments, and another form for students to submit their answers.

##### **forms.py**
```python
# courses/forms.py
from django import forms
from .models import Quiz, Question, Answer, Assignment

class QuizCreateForm(forms.ModelForm):
    class Meta:
        model = Quiz
        fields = ['title', 'course', 'due_date']

class QuestionCreateForm(forms.ModelForm):
    class Meta:
        model = Question
        fields = ['question_text', 'quiz']

class AnswerCreateForm(forms.ModelForm):
    class Meta:
        model = Answer
        fields = ['answer_text', 'question', 'is_correct']

class AssignmentCreateForm(forms.ModelForm):
    class Meta:
        model = Assignment
        fields = ['title', 'course', 'description', 'due_date']
```

#### **Step 3: Views for Creating and Submitting Quizzes/Assignments**

We’ll add views for creating and submitting quizzes and assignments.

##### **views.py**
```python
# courses/views.py
from django.shortcuts import render, redirect
from django.contrib.auth.decorators import login_required
from .models import Course, Quiz, Question, Answer, Assignment
from .forms import QuizCreateForm, QuestionCreateForm, AnswerCreateForm, AssignmentCreateForm

# Create Quiz
@login_required
def create_quiz(request, course_id):
    course = Course.objects.get(id=course_id)
    if request.user != course.instructor:
        return redirect('course_list')  # Redirect if user is not the instructor

    if request.method == 'POST':
        quiz_form = QuizCreateForm(request.POST)
        if quiz_form.is_valid():
            quiz = quiz_form.save(commit=False)
            quiz.course = course
            quiz.save()
            return redirect('course_detail', pk=course.id)
    else:
        quiz_form = QuizCreateForm()

    return render(request, 'courses/create_quiz.html', {'quiz_form': quiz_form, 'course': course})

# Create Question for Quiz
@login_required
def create_question(request, quiz_id):
    quiz = Quiz.objects.get(id=quiz_id)
    if request.user != quiz.course.instructor:
        return redirect('course_list')  # Redirect if user is not the instructor

    if request.method == 'POST':
        question_form = QuestionCreateForm(request.POST)
        if question_form.is_valid():
            question_form.save()
            return redirect('quiz_detail', pk=quiz.id)
    else:
        question_form = QuestionCreateForm()

    return render(request, 'courses/create_question.html', {'question_form': question_form, 'quiz': quiz})

# Create Assignment
@login_required
def create_assignment(request, course_id):
    course = Course.objects.get(id=course_id)
    if request.user != course.instructor:
        return redirect('course_list')

    if request.method == 'POST':
        assignment_form = AssignmentCreateForm(request.POST)
        if assignment_form.is_valid():
            assignment_form.save()
            return redirect('course_detail', pk=course.id)
    else:
        assignment_form = AssignmentCreateForm()

    return render(request, 'courses/create_assignment.html', {'assignment_form': assignment_form, 'course': course})
```

#### **Step 4: Create Templates for Quizzes and Assignments**

We’ll create templates to render the quiz creation form, question creation form, and assignment creation form.

##### **create_quiz.html**
```html
{% extends 'base.html' %}

{% block content %}
  <div class="container">
    <h2>Create Quiz for {{ course.title }}</h2>
    <form method="POST">
      {% csrf_token %}
      {{ quiz_form.as_p }}
      <button type="submit" class="btn btn-primary">Create Quiz</button>
    </form>
  </div>
{% endblock %}
```

##### **create_question.html**
```html
{% extends 'base.html' %}

{% block content %}
  <div class="container">
    <h2>Create Question for {{ quiz.title }}</h2>
    <form method="POST">
      {% csrf_token %}
      {{ question_form.as_p }}
      <button type="submit" class="btn btn-primary">Add Question</button>
    </form>
  </div>
{% endblock %}
```

##### **create_assignment.html**
```html
{% extends 'base.html' %}

{% block content %}
  <div class="container">
    <h2>Create Assignment for {{ course.title }}</h2>
    <form method="POST">
      {% csrf_token %}
      {{ assignment_form.as_p }}
      <button type="submit" class="btn btn-primary">Create Assignment</button>
    </form>
  </div>
{% endblock %}
```

---

### **Advanced Feature 2: Ratings and Reviews**

We'll let students rate and review the courses they take. This gives valuable feedback for instructors and helps future students make decisions.

#### **Step 1: Create Review Model**

Create a review model that links a course, a student, and a rating.

##### **models.py**
```python
# courses/models.py
from django.db import models

class Review(models.Model):
    course = models.ForeignKey(Course, on_delete=models.CASCADE, related_name='reviews')
    student = models.ForeignKey(get_user_model(), on_delete=models.CASCADE, related_name='reviews')
    rating = models.IntegerField(choices=[(1, '1 Star'), (2, '2 Stars'), (3, '3 Stars'), (4, '4 Stars'), (5, '5 Stars')])
    review_text = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return f"Review for {self.course.title} by {self.student.username}"
```

#### **Step 2: Create Review Form**

Create a form to handle course reviews.

##### **forms.py**
```python
# courses/forms.py
from django import forms
from .models import Review

class ReviewForm(forms.ModelForm):
    class Meta:
        model = Review
        fields = ['rating', 'review_text']
```

#### **Step 3: Create View for Adding Reviews**

Allow students to add reviews to the courses they are enrolled in.

##### **views.py**
```python
# courses/views.py
from .models import Review
from .forms import ReviewForm

@login_required
def add_review(request, course_id):
    course = Course.objects.get(id=course_id)
    if request.method == 'POST':
        form = ReviewForm(request.POST)
        if form.is_valid():
            review = form.save(commit=False)
            review.student = request.user
            review.course = course
            review.save()
            return redirect('course_detail', pk=course.id)
    else:
        form = ReviewForm()

    return render(request, 'courses/add_review.html', {'form': form, 'course': course})
```

#### **Step 4: Create Template for Review Form**

##### **add_review.html**
```html
{% extends 'base.html' %}

{% block content %}
  <div class="container">
    <h2>Write a Review for {{ course.title }}</h2>
    <form method="POST">
      {% csrf_token %}
      {{ form.as_p }}
      <button type="submit" class="btn btn-primary">Submit Review</button>
    </form>
  </div>
{% endblock %}
```

---

### **Advanced Feature 3: Course Certification**

Automatically generate and issue certificates to students upon completion of a course. We will integrate **report generation** using libraries such as **ReportLab** for PDF generation.

#### **Step 1: Install ReportLab**

You will need to install the `reportlab` library to create PDF certificates.

```bash
pip install reportlab
```

#### **Step 2: Create PDF Certificate Logic**

Create a function that generates a PDF certificate for a student.

##### **utils.py**
```python
from reportlab.lib.pagesizes import letter
from reportlab.pdfgen import canvas

def generate_certificate(student_name, course_name):
    file_path = f"certificates/{student_name}_{course_name}_certificate.pdf"
    c = canvas.Canvas(file_path, pagesize=letter)
    c.drawString(100, 750, f"Certificate of Completion")
    c.drawString(100, 730, f"This is to certify that")
    c.drawString(100, 710, f"{student_name}")
    c.drawString(100, 690, f"has completed the course:")
    c.drawString(100, 670, f"{course_name}")
    c.drawString(100, 650, f"Date: {datetime.date.today()}")
    c.save()

    return file_path
```

#### **Step 3: Link PDF Certificate to Course Completion**

Issue the certificate when a student completes a course (upon enrolling or after specific course completion criteria are met).

---

### **Advanced Feature 4: Video Integration**

We will integrate video lessons for the courses, allowing instructors to upload videos and students to watch them.

#### **Step 1: Modify the Course Model**

Add a field for video URL (could be a link to YouTube or a direct upload).

##### **models.py**
```python
# courses/models.py
class Course(models.Model):
    ...
    video_url = models.URLField(null=True, blank=True)  # Add video URL
```

#### **Step 2: Create a Video Upload Form**

You can also allow instructors to upload videos directly.

##### **forms.py**
```python
# courses/forms.py
class VideoUploadForm(forms.ModelForm):
    class Meta:
        model = Course
        fields = ['video_url']
```

#### **Step 3: Add Video Section in Course Detail View**

Display videos on the course detail page.

##### **course_detail.html**
```html
{% if course.video_url %}
  <h3>Watch Video</h3>
  <iframe width="560" height="315" src="{{ course.video_url }}" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
{% endif %}
```
