# django_task1

Application Programming Interface (API) is a great way to make two completely separate systems or in some cases same systems to interact with each other. API’s usually take data from backend and deliver it to the frontend.
If you are keen to make an API for yourself and are looking for tools that can help you in doing that in a easy and quick way, Django Rest Framework might a right fit for you.
Django Rest Framework is a great tool to quickly build robust and scalable APIs using Django in conjunction. It can do everything you want in an API and more. It has built in support for pagination, authentication and more.
Now, without any more delay, let’s dive in.
Installation.

In this project , i have created REST APIs based on Django with PostgreSQL database. It contains following things :-

Three user levels : 1. Super-admin, 2. Teacher, 3. Student,4.forgate password.
.I have used JWT authentication in this project. JSON Web Token is an open standard for securely transferring data within parties using a JSON object. JWT is used for stateless authentication mechanisms for users and providers, this means maintaining session is on the client-side instead of storing sessions on the server. we need to authenticate and obtain the token.

Student Login using JWT Authentication : http://127.0.0.1:8000/api-auth/login/?next=/student/

Teacher Login using JWT Authentication : http://127.0.0.1:8000/api-auth/login/?next=/teacher/
http://127.0.0.1:8000/admin/login/?next=/admin/
----------------------

Let’s start by installing the necessary Python packages. It is recommended to install these packages in a virtual environment.
pip install django
pip install djangorestframework
Setting up the Django project
After getting all the necessary packages, let’s set up a Django project.

django-admin startproject myproject
# go to the newly made project
cd myproject
# Create a superuser
python manage.py createsuperuser
# Provide username, email(optional) and password
# Run the server
python manage.py runserver
Open http://127.0.0.1:8000on your browser to check if the project is successfully set up or not.
Creating an app
Let’s create an app named webapp which will contain all the models and views required for the REST API.
django-admin startapp webapp

Add the above app to the list of installed app in settings.py
INSTALLED_APPS = [
    ...
    'webapp',
]


🏁 Checkpoint #1
 the list of installed apps in settings.py,
INSTALLED_APPS = [
    'webapp.apps.WebappConfig',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    'webapp',
]


Writing Models 

# webapp/models.py
from django.db import models

class teacher(models.Model):
    firstname = models.CharField( max_length=20 )
    lastname = models.CharField( max_length=20 )
    teacher_id = models.IntegerField(max_length=3)
    mobile = models.IntegerField(max_length=12)
class student(models.Model):
    firstname = models.CharField( max_length=20 )
    lastname = models.CharField( max_length=20 )
    student_id = models.IntegerField(max_length=3)
    mobile = models.IntegerField(max_length=12)

# Apply all migrations
python3 manage.py makemigration
python3 manage.py migrate
python3 manage.py sqlmigrate webapp 0001
python3 manage.py migrate


# weapp/admin.py

from django.contrib import admin
from .models import teacher
from .models import student

admin.site.register(teacher)
admin.site.register(student)

    
    
Next, we need to set up url routes for our views. We use router provided by the rest_framework to set up all the urls with different HTTP methods.
# webapp/views.py

from django.shortcuts import render
from django.http import HttpResponse
from django.shortcuts import get_object_or_404
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from .models import teacher
from .models import student
from .serializers import TeacherSerializer
from .serializers import StudentSerializer

class teacherlist(APIView):
    def get(self,request):
        teacher1=teacher.objects.all()
        serializer=TeacherSerializer(teacher1,many=True)
        return Response(serializer.data)
    def post(self):
        pass
class studentlist(APIView):
    def get(self,request):
        student1=student.objects.all()
        serializer=StudentSerializer(student1,many=True)
        return Response(serializer.data)
    def post(self):
        pass


 def login(request):
     return render(request,'login.html')


Now, let’s add webapp’s urls to our main urls under the /api/ path.

# myproject/urls.py


from django.contrib import admin
from django.urls import path,include

from webapp import views
from rest_framework.urlpatterns import format_suffix_patterns
from rest_framework_simplejwt import views as jwt_views



from  rest_framework_simplejwt.views import TokenObtainPairView, TokenRefreshView

from django.contrib.auth import views as auth_views


urlpatterns = [
    path('admin/', admin.site.urls),
    path('teacher/', views.teacherlist.as_view()),
    path('student/', views.studentlist.as_view()),
    path('api-auth/',include('rest_framework.urls')),
    path('api/jwt/',TokenObtainPairView.as_view()),
    path('login',views.login,name='login')

    path('api/jwt/refresh/',TokenRefreshView.as_view())

]


Now, we need to create serializers. Serializers are used to convert data from HTTP requests to objects that can be understood by Django. It is also used to validate data and define structure of response.
Here, we tell our serializer to use student,teacher as the model.
If a field name is not added here, it won’t be present in the API’s response.

# webapp/serializers.py
from rest_framework import serializers
from .models import teacher
from .models import student

class TeacherSerializer(serializers.ModelSerializer):
    class Meta:
        model =teacher
        fields = '__all__'

class StudentSerializer(serializers.ModelSerializer):
    class Meta:
        model =student
        fields = '__all__'

---------------------

# create templates folder:
password_reset_form.html
password_reset_done.html
password_reset_confirm.html
password_reset_complete.html
login.html
CSS

#url.py
   path('login',views.login,name='login')
#views.py
   def login(request):
       return render(request,'login.html')


Now visit http://127.0.0.1:8000/api/hospitals/to see the results. If you have added any data to the model, you’ll see them here else go ahead and add it now, either through admin panel or the form present below.
From now on, the full url will be omitted. Therefore, http://127.0.0.1:8000/api/student/ will be written as /student/

